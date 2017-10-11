---
title: "Utwórz moduł Azure IoT krawędzi z językiem Java | Dokumentacja firmy Microsoft"
description: "W tym samouczku ilustrację jak napisać cz modułu konwertera danych przy użyciu najnowszych pakietów Azure IoT krawędzi Maven."
services: iot-hub
author: junyi
manager: timlt
ms.service: iot-hub
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 06/28/2017
ms.author: junyi
ms.openlocfilehash: 0c430272225d79737baec2be15ed7c93991cdeac
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/03/2017
---
# <a name="create-an-azure-iot-edge-module-with-java"></a>Utwórz moduł Azure IoT krawędzi z językiem Java

W tym samouczku ilustrację, jak jeden może zbudować modułu Azure IoT Edge w języku Java.

W tym samouczku opisano konfiguracji środowiska i jak napisać [cz](https://en.wikipedia.org/wiki/Bluetooth_Low_Energy) modułu konwertera danych przy użyciu najnowszych pakietów Azure IoT krawędzi Maven.

## <a name="prerequisites"></a>Wymagania wstępne

W tej sekcji zostanie skonfigurowany środowiska do tworzenia modułu IoT krawędzi. Dotyczy ona obu *64-bitowym systemie Windows* i *64-bitowego systemu Linux (8 Ubuntu/Debian)* systemów operacyjnych.

Następujące oprogramowanie jest wymagane:

* [Klient Git](https://git-scm.com/downloads).
* [**x64** JDK](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).
* [Maven](https://maven.apache.org/install.html).

Otwórz okno terminala wiersza polecenia i klonowania repozytorium na następujący:

1. `git clone https://github.com/Azure-Samples/iot-edge-samples.git`.
2. `cd iot-edge-samples/java/simulated_ble`

## <a name="overall-architecture"></a>Ogólna architektura

Platforma Azure IoT krawędzi silnie przyjmuje [architektura Neumanna Von](https://en.wikipedia.org/wiki/Von_Neumann_architecture). Co oznacza, że całej architektury Azure IoT krawędzi system, który przetwarza dane wejściowe i generuje dane wyjściowe; i każdego pojedynczego modułu jest również niewielki rozmiar podsystemu wejścia/wyjścia. W tym samouczku wprowadzeniu dwóch następujących modułów:

1. Moduł, który odbiera symulowanego [cz](https://en.wikipedia.org/wiki/Bluetooth_Low_Energy) sygnału i konwertuje ją na sformatowany [JSON](https://en.wikipedia.org/wiki/JSON) wiadomości.
2. Moduł, który wyświetla odebranej [JSON](https://en.wikipedia.org/wiki/JSON) wiadomości.

Poniższa ilustracja przedstawia typowego przepływu end-to-end danych dla tego projektu:

![Przepływ danych między modułami trzy](media/iot-hub-iot-edge-create-module/dataflow.png "dane wejściowe: symulowane cz modułu; Procesor: Konwertera modułu; Dane wyjściowe: Moduł drukarki")

## <a name="understanding-the-code"></a>Opis kodu

### <a name="maven-project-structure"></a>Struktura projekt maven

Ponieważ pakiety krawędzi IoT Azure są oparte na Maven, należy utworzyć typowe struktury projekt Maven, która zawiera `pom.xml` pliku.

Dziedziczy POM `com.microsoft.azure.gateway.gateway-module-base` pakiet, który deklaruje wszystkie zależności wymagane przez projekt moduł, który zawiera pliki binarne środowiska wykonawczego, ścieżka pliku konfiguracji bramy i sposób wykonywania. To możemy zaoszczędzić wiele czasu i wyeliminować potrzebę zapisać i ponownie setki wierszy kodu wielokrotnie.

Należy zaktualizować plik pom.xml przez zadeklarowanie wymagane zależności/dodatków plug-in i nazwę pliku konfiguracji, które mają być używane przez naszych modułu, jak pokazano w poniższy fragment kodu.

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <!-- Inherit from parent -->
  <parent>
    <groupId>com.microsoft.azure.gateway</groupId>
    <artifactId>gateway-module-base</artifactId>
    <version>1.0.1</version>
  </parent>
  
  <groupId>com.microsoft.azure.gateway</groupId>
  <artifactId>ble-converter</artifactId>
  <version>1.0</version>
  <packaging>jar</packaging>

  <!-- Set the filename of the Azure IoT Edge configuration located
       under ./src/main/resources/gateway/ which is used in parent -->
  <properties>
    <gw.config.fileName>gw-config.json</gw.config.fileName>
  </properties>

  <!-- Re-declare dependencies used in parent -->
  <dependencies>
    <dependency>
      <groupId>com.microsoft.azure.gateway</groupId>
      <artifactId>gateway-java-binding</artifactId>
    </dependency>
    <dependency>
      <groupId>${dependency.runtime.group}</groupId>
      <artifactId>${dependency.runtime.name}</artifactId>
    </dependency>
  </dependencies>

  <!-- Re-declare plugins used in parent -->
  <build>
    <plugins>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-dependency-plugin</artifactId>
      </plugin>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-resources-plugin</artifactId>
      </plugin>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-shade-plugin</artifactId>
      </plugin>
      <plugin>
        <groupId>org.codehaus.mojo</groupId>
        <artifactId>exec-maven-plugin</artifactId>
      </plugin>
    </plugins>
  </build>
</project>
```

### <a name="basic-understanding-of-an-azure-iot-edge-module"></a>Podstawową wiedzę na temat modułu krawędzi IoT Azure

Moduł Azure IoT krawędzi można traktować jako procesor danych, którego zadaniem jest: wejściowych go przetworzyć i generowania danych wyjściowych.

Dane wejściowe mogą być danych z urządzenia (takie jak czujnik ruchu), wiadomości z innymi modułami lub jakikolwiek inny (na przykład liczba okresowo generowane przez czasomierz).

Wynik jest podobny do danych wejściowych, może ją wywołać zachowanie sprzętu (na przykład migający LED), wiadomości do innych modułów lub jakikolwiek inny (na przykład drukowanie do konsoli).

Moduły komunikują się ze sobą przy użyciu `com.microsoft.azure.gateway.messaging.Message` klasy. **Zawartości** z `Message` jest tablica bajtów, które jest w stanie reprezentujący każdego typu danych, które chcesz. **Właściwości** dostępne są również `Message` i są po prostu mapowania ciąg ciąg. Można traktować **właściwości** jako nagłówków żądań HTTP lub metadane pliku.

Aby opracować modułu Azure IoT krawędzi w języku Java, należy utworzyć nowe klasy modułu, która dziedziczy `com.microsoft.azure.gateway.core.GatewayModule` i wdrożenie wymaganych metody abstrakcyjne `receive()` i `destroy()`. W tym momencie można też do zaimplementowania opcjonalny `start()` lub `create()` również metody. Poniższy fragment kodu pokazano, jak rozpocząć tworzenie modułu Azure IoT krawędzi.

```java
import com.microsoft.azure.gateway.core.Broker;
import com.microsoft.azure.gateway.core.GatewayModule;
import com.microsoft.azure.gateway.messaging.Message;

public class MyEdgeModule extends GatewayModule {
  public MyEdgeModule(long address, Broker broker, String configuration) {
    /* Let the GatewayModule do the dirty work of initialization. It's also
       a good time to parse your own configuration defined in Azure IoT Edge
       configuration file (typically ./src/main/resources/gateway/gw-config.json) */
    super(address, broker, configuration);
  }

  @Override
  public void start() {
    /* Acquire the resources you need. If you don't
       need any resources, you may omit this method. */
  }

  @Override
  public void destroy() {
    /* It's time to release all resources. This method is required. */
  }

  @Override
  public void receive(Message message) {
    /* Logic to process the input message. This method is required. */
    // ...
    /* Use publish() method to do the output. You are
       allowed to publish your new Message instance. */
    this.publish(message);
  }
}
```

### <a name="converter-module"></a>Moduł konwertera

| Dane wejściowe                    | Procesor                              | Dane wyjściowe                 | Plik źródłowy            |
| ------------------------ | -------------------------------------- | ---------------------- | ---------------------- |
| Komunikat danych temperatury | Analizowanie i utworzyć nowy komunikat JSON | Komunikat JSON struktury | `ConverterModule.java` |

Jest to typowe moduł Azure IoT krawędzi. Akceptuje temperatury komunikaty z innymi modułami (sprzętowy moduł, lub w tym przypadku naszej symulowane modułu cz); a następnie normalizowane komunikatu temperatury w strukturze komunikat JSON (w tym dołączanie identyfikator komunikatu ustawienie właściwości czy musimy wyzwolenia alertu temperatury i tak dalej).

```java
@Override
public void receive(Message message) {
  try {
    JSONObject messageFromBle = new JSONObject(new String(message.getContent()));
    double temperature = messageFromBle.getDouble("temperature");
    Map<String, String> inputProperties = message.getProperties();

    HashMap<String, String> properties = new HashMap<>();
    properties.put("source", inputProperties.get("source"));
    properties.put("macAddress", inputProperties.get("macAddress"));
    properties.put("temperatureAlert", temperature > 30 ? "true" : "false");

    String content = String.format(
        "{ \"deviceId\": \"Intel NUC Gateway\", \"messageId\": %d, \"temperature\": %f }",
        ++this.messageCount, temperature);

    this.publish(new Message(content.getBytes(), properties));
  } catch (Exception ex) {
    ex.printStackTrace();
  }
}
```

### <a name="printer-module"></a>Moduł drukarki

| Dane wejściowe                          | Procesor | Dane wyjściowe                     | Plik źródłowy          |
| ------------------------------ | --------- | -------------------------- | -------------------- |
| Dowolny komunikat z innymi modułami | Nie dotyczy       | Zaloguj się do konsoli wiadomości | `PrinterModule.java` |

Jest to proste i nie wymaga objaśnień, moduł, który generuje odebrane wiadomości do okna terminala.

```java
@Override
public void receive(Message message) {
  System.out.println(message.toString());
}
```

### <a name="azure-iot-edge-configuration"></a>Konfiguracja programu Azure IoT krawędzi

Ostatnim krokiem przed uruchomieniem modułów jest skonfigurowanie krawędzi IoT Azure oraz do zestawiania połączeń między modułami.

Najpierw należy zadeklarować naszych ładujący Java (od momentu krawędzi IoT Azure obsługuje modułów ładujących różnych języków), którego można odwoływać się jego `name` w sekcjach później.

```json
"loaders": [{
  "type": "java",
  "name": "java",
  "configuration": {
    "jvm.options": {
      "library.path": "./"
    }
  }
}]
```

Po zadeklarowaniu możemy naszych ładowarki, potrzebujemy będzie zadeklarować również naszych modułów. Podobnie jak deklarowanie ładowarki, ich mogą się też odwoływać linie ich `name` atrybutu. Przy deklarowaniu modułu, należy określić moduł ładujący on powinien używać (który powinien być zdefiniowanego przed) i punktu wejścia (powinna być nazwa klasy znormalizowane naszych modułu) dla każdego modułu. `simulated_device` Modułu jest uwzględniony w pakiecie Azure IoT krawędzi core czasu wykonywania modułu macierzystego. Zawsze należy uwzględnić `args` w formacie JSON plików, nawet jeśli jest ona `null`.

```json
"modules": [
  {
    "name": "simulated_device",
    "loader": {
      "name": "native",
      "entrypoint": {
        "module.path": "simulated_device"
      }
    },
    "args": {
      "macAddress": "01:02:03:03:02:01",
      "messagePeriod": 500
    }
  },
  {
    "name": "converter",
    "loader": {
      "name": "java",
      "entrypoint": {
        "class.name": "com/microsoft/azure/gateway/ConverterModule",
        "class.path": "./ble-converter-1.0-with-deps.jar"
      }
    },
    "args": null
  },
  {
    "name": "print",
    "loader": {
      "name": "java",
      "entrypoint": {
        "class.name": "com/microsoft/azure/gateway/PrinterModule",
        "class.path": "./ble-converter-1.0-with-deps.jar"
      }
    },
    "args": null
  }
]
```

Po zakończeniu konfiguracji nawiązanie połączenia. Każde połączenie jest wyrażona w `source` i `sink`. Powinna zarówno odwołujących się wstępnie zdefiniowanych modułów. Komunikat dane wyjściowe z `source` modułu zostaną przekazane w danych wejściowych `sink` modułu.

```json
"links": [
  {
    "source": "simulated_device",
    "sink": "converter"
  },
  {
    "source": "converter",
    "sink": "print"
  }
]
```

## <a name="running-the-modules"></a>Uruchomione moduły

Użyj `mvn package` do tworzenia wszystko do `target/` folderu. `mvn clean package`jest także zalecana dla czystego kompilacji.

Użyj `mvn exec:exec` do uruchomienia krawędzi IoT Azure i powinno uwzględniać dane temperatury i wszystkie właściwości są podane w konsoli według stałej stawki.

Jeśli chcesz zakończyć tę aplikację, naciśnij klawisz `<Enter>` klucza.

> [!IMPORTANT]
> Użyj klawiszy Ctrl + C, aby przerwać aplikacji bramy krawędzi IoT jest niezalecane. Ponieważ może to spowodować przerwanie wyjątkowo procesu.

