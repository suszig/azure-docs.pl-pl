---
title: "Centrum IoT Azure bezpośredniego metody (Java) | Dokumentacja firmy Microsoft"
description: "Jak używać bezpośrednich metod Centrum IoT Azure. Przy użyciu urządzenia Azure IoT SDK dla języka Java aplikacji symulowane urządzenie, która zawiera metoda bezpośrednia i usługę Azure IoT SDK dla języka Java, aby zaimplementować aplikację usługi, która wywołuje metodę bezpośredniego."
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: ab035b8e-bff8-4e12-9536-f31d6b6fe425
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2017
ms.author: dobett
ms.openlocfilehash: 4fb759ecd7767c126bc22165494652039ba1caa4
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="use-direct-methods-java"></a>Użyj metody bezpośredniego (Java)

[!INCLUDE [iot-hub-selector-c2d-methods](../../includes/iot-hub-selector-c2d-methods.md)]

W tym samouczku można utworzyć dwie aplikacje konsoli Java:

* **wywołania bezpośrednie — metody**, aplikacji wewnętrznych Java, która wywołuje metodę w aplikacji symulowane urządzenie i wyświetla odpowiedzi.
* **Symulowane urządzenie**, aplikacji Java, która symuluje urządzenia nawiązywanie połączeń z Centrum IoT z tożsamości urządzenia można utworzyć. Ta aplikacja odpowiada bezpośrednio wywołać z poziomu wewnętrznej.

> [!NOTE]
> Aby uzyskać informacji na temat zestawów SDK, które służy do tworzenia aplikacji do uruchamiania na urządzeniach i z zaplecza rozwiązania, zobacz [Azure IoT SDK][lnk-hub-sdks].

Do ukończenia tego samouczka niezbędne są następujące elementy:

* Java SE 8. <br/> W artykule [Prepare your development environment][lnk-dev-setup] (Przygotowanie środowiska projektowego) opisano, jak zainstalować środowisko Java na potrzeby tego samouczka w systemie Windows lub Linux.
* Maven 3.  <br/> W artykule [Prepare your development environment][lnk-dev-setup] (Przygotowanie środowiska projektowego) opisano, jak zainstalować środowisko [Maven][lnk-maven] na potrzeby tego samouczka w systemie Windows lub Linux.
* [Środowisko node.js w wersji 0.10.0 lub nowszym](http://nodejs.org).

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="create-a-simulated-device-app"></a>Tworzenie aplikacji symulowanego urządzenia

W tej sekcji utworzysz aplikację konsoli języka Java odpowiadający do metody wywoływane przez rozwiązania zakończenia.

1. Utwórz pusty folder o nazwie iot-java bezpośrednio method.

1. W folderze iot-java bezpośrednio method, Utwórz projekt Maven o nazwie **symulowane urządzenie** przy użyciu następującego polecenia z wiersza polecenia. Polecenie jest pojedynczą, długie polecenie:

    `mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=simulated-device -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false`

1. W wierszu polecenia przejdź do folderu simulated-device.

1. Za pomocą edytora tekstów otwórz plik pom.xml w folderze simulated-device i dodaj następujące zależności do węzła **zależności**. Ta zależność umożliwia za pomocą pakietu klienta w przypadku urządzenia iot w aplikacji do komunikowania się z Centrum IoT:

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-device-client</artifactId>
      <version>1.3.32</version>
    </dependency>
    ```

    > [!NOTE]
    > Możesz sprawdzić dostępność najnowszej wersji pakietu **iot-device-client** za pomocą [funkcji wyszukiwania narzędzia Maven][lnk-maven-device-search].

1. Dodaj następujące **kompilacji** węzła po **zależności** węzła. Ta konfiguracja powoduje, że Maven na potrzeby tworzenia aplikacji Java 1.8:

    ```xml
    <build>
      <plugins>
        <plugin>
          <groupId>org.apache.maven.plugins</groupId>
          <artifactId>maven-compiler-plugin</artifactId>
          <version>3.3</version>
          <configuration>
            <source>1.8</source>
            <target>1.8</target>
          </configuration>
        </plugin>
      </plugins>
    </build>
    ```

1. Zapisz i zamknij plik pom.xml.

1. Za pomocą edytora tekstów otwórz plik simulated-device\src\main\java\com\mycompany\app\App.java.

1. Dodaj do pliku następujące instrukcje **importowania**:

    ```java
    import com.microsoft.azure.sdk.iot.device.*;
    import com.microsoft.azure.sdk.iot.device.DeviceTwin.*;

    import java.io.IOException;
    import java.net.URISyntaxException;
    import java.util.Scanner;
    ```

1. Dodaj następujące zmienne na poziomie klasy do klasy **App**. Zastępowanie `{youriothubname}` z nazwę Centrum IoT i `{yourdevicekey}` kluczem urządzenia wartość, zostanie wygenerowany w *tworzenie tożsamości urządzenia* sekcji:

    ```java
    private static String connString = "HostName={youriothubname}.azure-devices.net;DeviceId=myDeviceID;SharedAccessKey={yourdevicekey}";
    private static IotHubClientProtocol protocol = IotHubClientProtocol.MQTT;
    private static String deviceId = "myDeviceId";
    private static final int METHOD_SUCCESS = 200;
    private static final int METHOD_NOT_DEFINED = 404;
    ```

    Ta przykładowa aplikacja używa zmiennej **protocol** podczas tworzenia wystąpienia obiektu **DeviceClient**. 

1. Aby przywrócić Centrum IoT kod stanu, Dodaj następujące zagnieżdżona klasa na **aplikacji** klasy:

    ```java
    protected static class DirectMethodStatusCallback implements IotHubEventCallback
    {
      public void execute(IotHubStatusCode status, Object context)
      {
        System.out.println("IoT Hub responded to device method operation with status " + status.name());
      }
    }
    ```

1. Do obsługi wywołań metody bezpośrednio z zaplecza rozwiązania, należy dodać następujące zagnieżdżona klasa na **aplikacji** klasy:

    ```java
    protected static class DirectMethodCallback implements com.microsoft.azure.sdk.iot.device.DeviceTwin.DeviceMethodCallback
    {
      @Override
      public DeviceMethodData call(String methodName, Object methodData, Object context)
      {
        DeviceMethodData deviceMethodData;
        switch (methodName)
        {
          case "writeLine" :
          {
            int status = METHOD_SUCCESS;
            System.out.println(new String((byte[])methodData));
            deviceMethodData = new DeviceMethodData(status, "Executed direct method " + methodName);
            break;
          }
          default:
          {
            int status = METHOD_NOT_DEFINED;
            deviceMethodData = new DeviceMethodData(status, "Not defined direct method " + methodName);
          }
        }
        return deviceMethodData;
      }
    }
    ```

1. Aby utworzyć **DeviceClient** i nasłuchiwania bezpośrednie wywołania metod, Dodaj **głównego** metodę **aplikacji** klasy:

    ```java
    public static void main(String[] args)
      throws IOException, URISyntaxException
    {
      System.out.println("Starting device sample...");

      DeviceClient client = new DeviceClient(connString, protocol);

      try
      {
        client.open();
        client.subscribeToDeviceMethod(new DirectMethodCallback(), null, new DirectMethodStatusCallback(), null);
        System.out.println("Subscribed to direct methods. Waiting...");
      }
      catch (Exception e)
      {
        System.out.println("On exception, shutting down \n" + " Cause: " + e.getCause() + " \n" +  e.getMessage());
        client.close();
        System.out.println("Shutting down...");
      }

      System.out.println("Press any key to exit...");
      Scanner scanner = new Scanner(System.in);
      scanner.nextLine();
      scanner.close();
      client.close();
      System.out.println("Shutting down...");
    }
    ```

1. Zapisz i zamknij plik simulated-device\src\main\java\com\mycompany\app\App.java

1. Tworzenie **symulowane urządzenie** aplikacji i poprawić błędy. Wierszu polecenia przejdź do folderu symulowane urządzenie, a następnie uruchom następujące polecenie:

    `mvn clean package -DskipTests`

## <a name="call-a-direct-method-on-a-device"></a>Wywołanie metody bezpośrednio na urządzeniu

W tej sekcji utworzysz aplikację konsoli języka Java, która wywołuje metodę bezpośredniego, a następnie wyświetla odpowiedzi. Łączy się Centrum IoT można wywołać metody bezpośrednio z tej aplikacji konsoli.

1. W folderze iot-java bezpośrednio method, Utwórz projekt Maven o nazwie **invoke bezpośrednio method** za pomocą następującego polecenia z wiersza polecenia. Polecenie jest pojedynczą, długie polecenie:

    `mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=invoke-direct-method -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false`

1. Wierszu polecenia przejdź do folderu metody invoke bezpośrednio.

1. Za pomocą edytora tekstu, otwórz plik pom.xml w folderze metody invoke bezpośredniego i dodaj następujące zależności do **zależności** węzła. Ta zależność umożliwia za pomocą pakietu iot usługi klienta w aplikacji do komunikowania się z Centrum IoT:

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-service-client</artifactId>
      <version>1.7.23</version>
      <type>jar</type>
    </dependency>
    ```

    > [!NOTE]
    > Możesz sprawdzić dostępność najnowszej wersji pakietu **iot-service-client** za pomocą [funkcji wyszukiwania narzędzia Maven][lnk-maven-service-search].

1. Dodaj następujące **kompilacji** węzła po **zależności** węzła. Ta konfiguracja powoduje, że Maven na potrzeby tworzenia aplikacji Java 1.8:

    ```xml
    <build>
      <plugins>
        <plugin>
          <groupId>org.apache.maven.plugins</groupId>
          <artifactId>maven-compiler-plugin</artifactId>
          <version>3.3</version>
          <configuration>
            <source>1.8</source>
            <target>1.8</target>
          </configuration>
        </plugin>
      </plugins>
    </build>
    ```

1. Zapisz i zamknij plik pom.xml.

1. Za pomocą edytora tekstu, otwórz plik invoke-direct-method\src\main\java\com\mycompany\app\App.java.

1. Dodaj do pliku następujące instrukcje **importowania**:

    ```java
    import com.microsoft.azure.sdk.iot.service.devicetwin.DeviceMethod;
    import com.microsoft.azure.sdk.iot.service.devicetwin.MethodResult;
    import com.microsoft.azure.sdk.iot.service.exceptions.IotHubException;

    import java.io.IOException;
    import java.util.concurrent.TimeUnit;
    ```

1. Dodaj następujące zmienne na poziomie klasy do klasy **App**. Zastąp `{youriothubconnectionstring}` z wymienionych w ciągu połączenia Centrum IoT *tworzenia Centrum IoT* sekcji:

    ```java
    public static final String iotHubConnectionString = "{youriothubconnectionstring}";
    public static final String deviceId = "myDeviceId";

    public static final String methodName = "writeLine";
    public static final Long responseTimeout = TimeUnit.SECONDS.toSeconds(30);
    public static final Long connectTimeout = TimeUnit.SECONDS.toSeconds(5);
    public static final String payload = "a line to be written";
    ```

1. Aby wywołać metodę na symulowane urządzenie, Dodaj następujący kod do **głównego** metody:

    ```java
    System.out.println("Starting sample...");
    DeviceMethod methodClient = DeviceMethod.createFromConnectionString(iotHubConnectionString);

    try
    {
        System.out.println("Invoke direct method");
        MethodResult result = methodClient.invoke(deviceId, methodName, responseTimeout, connectTimeout, payload);

        if(result == null)
        {
            throw new IOException("Direct method invoke returns null");
        }
        System.out.println("Status=" + result.getStatus());
        System.out.println("Payload=" + result.getPayload());
    }
    catch (IotHubException e)
    {
        System.out.println(e.getMessage());
    }

    System.out.println("Shutting down sample...");
    ```

1. Zapisz i zamknij plik invoke-direct-method\src\main\java\com\mycompany\app\App.java

1. Tworzenie **invoke bezpośrednio method** aplikacji i poprawić błędy. Wiersz polecenia przejdź do folderu metody invoke bezpośrednio i uruchom następujące polecenie:

    `mvn clean package -DskipTests`

## <a name="run-the-apps"></a>Uruchamianie aplikacji

Teraz można przystąpić do uruchomienia aplikacji konsoli.

1. W wierszu polecenia w folderze symulowane urządzenie uruchom następujące polecenie, aby rozpocząć nasłuchiwania dla wywołań metod z Centrum IoT:

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![Centrum IoT Java symulowane urządzenie aplikacji do nasłuchiwania dla wywołań metod bezpośredniego][8]

1. W wierszu polecenia w folderze metody invoke bezpośrednio uruchom następujące polecenie, aby wywołać metodę na symulowane urządzenie z Centrum IoT:

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![Centrum IoT Java usługi aplikacji, aby wywołać metodę bezpośredniego][7]

1. Symulowane urządzenie odpowiada na wywołanie metody bezpośrednie:

    ![Centrum IoT Java symulowane urządzenie aplikacji odpowiada na wywołanie bezpośrednie — metoda][9]

## <a name="next-steps"></a>Następne kroki

W tym samouczku opisano konfigurowanie nowego centrum IoT Hub w witrynie Azure Portal, a następnie tworzenie tożsamości urządzenia w rejestrze tożsamości centrum. Tej tożsamości urządzenia są używane do włączenia aplikacji symulowane urządzenie zareagować na metody wywoływane przez chmurę. Utworzono aplikację, która wywołuje metody na urządzeniu i wyświetla odpowiedzi z urządzenia.

Aby zapoznać się z innymi scenariuszami IoT, zobacz [Planowanie zadań na wielu urządzeniach][lnk-devguide-jobs].

Aby dowiedzieć się, jak rozszerzyć IoT, Twoje rozwiązanie i harmonogram metoda wywołuje na wielu urządzeniach, zobacz [emisji zadania i harmonogramu] [ lnk-tutorial-jobs] samouczka.

<!-- Images. -->
[7]: ./media/iot-hub-java-java-direct-methods/invoke-method.png
[8]: ./media/iot-hub-java-java-direct-methods/device-listen.png
[9]: ./media/iot-hub-java-java-direct-methods/device-respond.png

<!-- Links -->
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-java/blob/master/doc/java-devbox-setup.md
[lnk-maven]: https://maven.apache.org/what-is-maven.html
[lnk-hub-sdks]: iot-hub-devguide-sdks.md

[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-tutorial-jobs]: iot-hub-node-node-schedule-jobs.md
[lnk-maven-service-search]: http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-service-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22
[lnk-maven-device-search]: http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-device-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22
