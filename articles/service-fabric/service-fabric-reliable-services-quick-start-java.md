---
title: "Tworzenie pierwszego niezawodnej mikrousługi Azure w języku Java | Dokumentacja firmy Microsoft"
description: "Wprowadzenie do tworzenia aplikacji usługi sieć szkieletowa usług Microsoft Azure z usług bezstanowych i stanowych."
services: service-fabric
documentationcenter: java
author: suhuruli
manager: timlt
editor: 
ms.assetid: 7831886f-7ec4-4aef-95c5-b2469a5b7b5d
ms.service: service-fabric
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/02/2017
ms.author: suhuruli
ms.openlocfilehash: 59b58e9d9bdb044c81261fd19338c3f95bd409b3
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/04/2017
---
# <a name="get-started-with-reliable-services"></a>Wprowadzenie do usług Reliable Services
> [!div class="op_single_selector"]
> * [C# w systemie Windows](service-fabric-reliable-services-quick-start.md)
> * [Java w systemie Linux](service-fabric-reliable-services-quick-start-java.md)
>
>

W tym artykule opisano podstawy niezawodne usługi sieci szkieletowej usług Azure i przeprowadzi użytkownika przez proces tworzenia i wdrażania prostą aplikację niezawodnej usługi napisane w języku Java. Microsoft Virtual Academy wideo przedstawiono również sposób tworzenia bezstanowej niezawodnej usługi:<center><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=DOX8K86yC_206218965">  
<img src="./media/service-fabric-reliable-services-quick-start-java/ReliableServicesJavaVid.png" WIDTH="360" HEIGHT="244">  
</a></center>

## <a name="installation-and-setup"></a>Instalacja i Konfiguracja
Przed rozpoczęciem upewnij się, że masz środowisko projektowe sieci szkieletowej usług na komputerze.
Jeśli musisz je skonfigurować, przejdź do [wprowadzenie dla komputerów Mac](service-fabric-get-started-mac.md) lub [wprowadzenie w systemie Linux](service-fabric-get-started-linux.md).

## <a name="basic-concepts"></a>Podstawowe pojęcia
Aby rozpocząć pracę z usługami Reliable Services, tylko musisz poznać kilka podstawowych pojęć:

* **Typ usługi**: jest to implementacji usługi. Jest on zdefiniowany przez klasę pisania, rozszerzający `StatelessService` i innego kodu lub zależności używany, oraz nazwę i numer wersji.
* **Nazwane wystąpienie usługi**: Aby uruchomić usługę, tworzenia nazwanego wystąpienia danego typu usługi znacznie tak, jak utworzyć wystąpienia obiektu typu klasy. Wystąpienie usługi są w istocie wystąpieniami klasie usługi, który można zapisać obiektu.
* **Host usługi**: tworzenia wystąpień usługi o nazwie trzeba uruchamiać wewnątrz hosta. Host usługi jest właśnie procesu, których można uruchomić wystąpienia usługi.
* **Usługa rejestracji**: rejestracji, połączono wszystko. Typ usługi musi być zarejestrowana ze środowiskiem uruchomieniowym usługi sieć szkieletowa w umożliwia sieci szkieletowej usług do tworzenia wystąpień usługi hosta do uruchomienia.  

## <a name="create-a-stateless-service"></a>Tworzenie usługi bezstanowej
Rozpocznij od utworzenia aplikacji sieci szkieletowej usług. Zestaw SDK sieci szkieletowej usług dla systemu Linux zawiera narzędzia Yeoman generatora, aby zapewnić szkieletów dla aplikacji sieci szkieletowej usług za pomocą usługi bezstanowej. Uruchom, uruchamiając następujące narzędzia Yeoman polecenia:

```bash
$ yo azuresfjava
```

Postępuj zgodnie z instrukcjami, aby utworzyć **usługi bezstanowej niezawodnej**. W tym samouczku, nazwa aplikacji "HelloWorldApplication" i "HelloWorld" usługi. Wynik zawiera katalogów `HelloWorldApplication` i `HelloWorld`.

```bash
HelloWorldApplication/
├── build.gradle
├── HelloWorld
│   ├── build.gradle
│   └── src
│       └── statelessservice
│           ├── HelloWorldServiceHost.java
│           └── HelloWorldService.java
├── HelloWorldApplication
│   ├── ApplicationManifest.xml
│   └── HelloWorldPkg
│       ├── Code
│       │   ├── entryPoint.sh
│       │   └── _readme.txt
│       ├── Config
│       │   └── _readme.txt
│       ├── Data
│       │   └── _readme.txt
│       └── ServiceManifest.xml
├── install.sh
├── settings.gradle
└── uninstall.sh
```

## <a name="implement-the-service"></a>Wdrożenie usługi
Otwórz **HelloWorldApplication/HelloWorld/src/statelessservice/HelloWorldService.java**. Ta klasa definiuje typ usługi, a następnie można uruchomić dowolny kod. Interfejs API usługi zawiera dwa punkty wejścia kodu:

* Metoda punktu wejścia otwarty, nazywany `runAsync()`, w którym można rozpocząć wykonywania dowolnych zadań, w tym obciążeń obliczeniowych długotrwałe.

```java
@Override
protected CompletableFuture<?> runAsync(CancellationToken cancellationToken) {
    ...
}
```

* Punkt wejścia komunikacji, gdzie można podłączyć stosu komunikacji wyboru. Jest to, gdzie można uruchomić odbieranie żądań od użytkowników i innych usług.

```java
@Override
protected List<ServiceInstanceListener> createServiceInstanceListeners() {
    ...
}
```

W tym samouczku, możemy skupić się na `runAsync()` metoda punktu wejścia. Jest to, gdzie natychmiast można rozpocząć uruchamianie kodu.

### <a name="runasync"></a>RunAsync
Platforma wywołuje tę metodę, gdy wystąpienie usługi jest umieszczony i gotowa do wykonania. Dla usługi bezstanowej po prostu oznacza to, gdy wystąpienie usługi jest otwarty. Token anulowania został dostarczony do koordynowania, gdy wystąpienie usługi musi zostać zamknięty. W sieci szkieletowej usług ten cykl otwarcie i zamknięcie wystąpienia usługi może wystąpić wiele razy w okresie istnienia usługi jako całość. Może się to zdarzyć z różnych powodów, w tym:

* System przeniesie swoich wystąpień usługi dla równoważenia zasobów.
* Błędy występują w kodzie.
* Uaktualnianie aplikacji lub systemu.
* Sprzętu źródłowego ulegnie awarii.

Aranżacja jest zarządzana przez sieci szkieletowej usług do zachowania usługi wysokiej dostępności i nieprawidłowo zrównoważone.

`runAsync()`nie zablokować synchronicznie. Implementacji runAsync powinien zwrócić CompletableFuture umożliwia środowiska wykonawczego kontynuować. Jeśli obciążenie należy zaimplementować długotrwała zadanie, które należy wykonać wewnątrz CompletableFuture.

#### <a name="cancellation"></a>Anulowanie
Anulowanie obciążenie jest współpraca wysiłku, zorkiestrowana przez token anulowania podanego. System oczekuje na zadanie zakończenia (przez pomyślne zakończenie anulowania lub fault), zanim przesyłane w. Ważne jest, aby uwzględnić token anulowania, Zakończ pracę i zamknąć `runAsync()` tak szybko jak to możliwe, gdy system żąda anulowania. W poniższym przykładzie pokazano sposób obsługi zdarzenia anulowania:

```java
    @Override
    protected CompletableFuture<?> runAsync(CancellationToken cancellationToken) {

        // TODO: Replace the following sample code with your own logic
        // or remove this runAsync override if it's not needed in your service.

        CompletableFuture.runAsync(() -> {
          long iterations = 0;
          while(true)
          {
            cancellationToken.throwIfCancellationRequested();
            logger.log(Level.INFO, "Working-{0}", ++iterations);

            try
            {
              Thread.sleep(1000);
            }
            catch (IOException ex) {}
          }
        });
    }
```

### <a name="service-registration"></a>Rejestracja usługi
Środowisko uruchomieniowe usługi sieć szkieletowa usług musi być zarejestrowany typów usług. Typ usługi jest zdefiniowany w `ServiceManifest.xml` i klasie usługi, który implementuje `StatelessService`. Rejestracja usługi jest wykonywane w procesie główny punkt wejścia. W tym przykładzie jest proces główny punkt wejścia `HelloWorldServiceHost.java`:

```java
public static void main(String[] args) throws Exception {
    try {
        ServiceRuntime.registerStatelessServiceAsync("HelloWorldType", (context) -> new HelloWorldService(), Duration.ofSeconds(10));
        logger.log(Level.INFO, "Registered stateless service type HelloWorldType.");
        Thread.sleep(Long.MAX_VALUE);
    }
    catch (Exception ex) {
        logger.log(Level.SEVERE, "Exception in registration:", ex);
        throw ex;
    }
}
```

## <a name="run-the-application"></a>Uruchamianie aplikacji

Narzędzia Yeoman szkieletów zawiera skrypt narzędzia gradle do tworzenia aplikacji i skryptów służących do wdrażania i Usuń aplikację bash. Aby uruchomić aplikację, należy najpierw utworzyć aplikację z narzędziem gradle:

```bash
$ gradle
```

Daje to pakiet aplikacji sieci szkieletowej usług, które można wdrożyć przy użyciu interfejsu wiersza polecenia usługi sieci szkieletowej.

### <a name="deploy-with-service-fabric-cli"></a>Wdrażanie z sieci szkieletowej usług interfejsu wiersza polecenia

Skrypt install.sh zawiera niezbędne polecenia interfejsu wiersza polecenia usługi sieci szkieletowej w celu wdrożenia pakietu aplikacji. Uruchom skrypt install.sh do wdrożenia aplikacji.

```bash
$ ./install.sh
```

## <a name="next-steps"></a>Następne kroki

* [Getting started with Service Fabric CLI (Wprowadzenie do interfejsu wiersza polecenia usługi Service Fabric)](service-fabric-cli.md)
