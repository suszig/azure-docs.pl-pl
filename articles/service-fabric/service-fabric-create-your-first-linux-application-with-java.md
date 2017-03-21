---
title: "Tworzenie pierwszej aplikacji mikrousług platformy Azure w systemie Linux przy użyciu platformy Java | Microsoft Docs"
description: "Tworzenie i wdrażanie aplikacji usługi Service Fabric przy użyciu platformy Java"
services: service-fabric
documentationcenter: java
author: seanmck
manager: timlt
editor: 
ms.assetid: 02b51f11-5d78-4c54-bb68-8e128677783e
ms.service: service-fabric
ms.devlang: java
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/05/2017
ms.author: seanmck
translationtype: Human Translation
ms.sourcegitcommit: 24d86e17a063164c31c312685c0742ec4a5c2f1b
ms.openlocfilehash: 88b16b0b7c951ab0350649de05c00263ec76e630
ms.lasthandoff: 03/11/2017


---
# <a name="create-your-first-azure-service-fabric-application"></a>Tworzenie pierwszej aplikacji usługi Azure Service Fabric
> [!div class="op_single_selector"]
> * [C# — Windows](service-fabric-create-your-first-application-in-visual-studio.md)
> * [Java — Linux](service-fabric-create-your-first-linux-application-with-java.md)
> * [C# — Linux](service-fabric-create-your-first-linux-application-with-csharp.md)
>
>

Usługa Service Fabric udostępnia zestawy SDK do kompilowania usług w systemie Linux przy użyciu platform .NET Core i Java. W tym samouczku utworzymy aplikację dla systemu Linux i skompilujemy usługę przy użyciu języka Java.  

> [!NOTE]
> Język Java, jako wbudowany język programowania pierwszej klasy, jest obsługiwany tylko dla podglądu w systemie Linux (planowana jest obsługa w systemie Windows). Jednak wszelkie aplikacje, a w tym aplikacje Java, mogą działać jako gościnne pliki wykonywalne lub wewnątrz kontenerów w systemie Windows lub Linux. Aby uzyskać więcej informacji, zobacz [Wdrażanie istniejącego pliku wykonywalnego w usłudze Azure Service Fabric](service-fabric-deploy-existing-app.md) i [Wdrażanie kontenerów w usłudze Service Fabric](service-fabric-deploy-container.md).
>

## <a name="video-tutorial"></a>Samouczek wideo

Poniższe wideo z witryny Microsoft Virtual Academy zawiera szczegółowe omówienie procesu tworzenia aplikacji Java w systemie Linux:  
<center><a target="\_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=DOX8K86yC_206218965">  
<img src="./media/service-fabric-create-your-first-linux-application-with-java/LinuxVid.png" WIDTH="360" HEIGHT="244">  
</a></center>


## <a name="prerequisites"></a>Wymagania wstępne
Przed rozpoczęciem upewnij się, że masz [skonfigurowane środowisko programowania systemu Linux](service-fabric-get-started-linux.md). Jeśli używasz systemu Mac OS X, możesz [skonfigurować jednopunktowe środowisko systemu Linux na maszynie wirtualnej za pomocą narzędzia Vagrant](service-fabric-get-started-mac.md).

## <a name="create-the-application"></a>Tworzenie aplikacji
Aplikacja usługi Service Fabric może zawierać jedną lub więcej usług, a każda z nich pełni określoną rolę w dostarczaniu funkcjonalności aplikacji. Zestaw SDK usługi Service Fabric dla systemu Linux zawiera generator [Yeoman](http://yeoman.io/), który ułatwia tworzenie pierwszej usługi i dodawanie kolejnych w przyszłości. Utwórzmy zatem aplikację z jedną usługą za pomocą generatora Yeoman.

1. Na terminalu wpisz ``yo azuresfjava``.
2. Nadaj nazwę aplikacji.
3. Wybierz typ pierwszej usługi i nadaj jej nazwę. Na potrzeby tego samouczka wybierzmy usługę Reliable Actor Service.

   ![Generator Yeoman usługi Service Fabric dla platformy Java][sf-yeoman]

> [!NOTE]
> Aby uzyskać więcej informacji o opcjach, zobacz [Service Fabric programming model overview](service-fabric-choose-framework.md) (Omówienie modelu programowania usługi Service Fabric).
>

## <a name="build-the-application"></a>Kompilowanie aplikacji
Szablony generatora Yeoman usługi Service Fabric obejmują skrypt kompilacji dla narzędzia [Gradle](https://gradle.org/), którego można użyć do skompilowania aplikacji z poziomu terminala.

  ```bash
  cd myapp
  gradle
  ```

## <a name="deploy-the-application"></a>Wdrażanie aplikacji
Po skompilowaniu aplikację można wdrożyć w klastrze lokalnym za pomocą interfejsu wiersza polecenia platformy Azure.

1. Połącz się z lokalnym klastrem usługi Service Fabric.

    ```bash
    azure servicefabric cluster connect
    ```

2. Użyj skryptu instalacji udostępnionego w szablonie, aby skopiować pakiet aplikacji do magazynu obrazów klastra, zarejestrować typ aplikacji i utworzyć wystąpienie aplikacji.

    ```bash
    ./install.sh
    ```

3. Otwórz przeglądarkę i przejdź do narzędzia Service Fabric Explorer dostępnego pod adresem http://localhost:19080/Explorer (zastąp łańcuch localhost prywatnym adresem IP maszyny wirtualnej w przypadku używania narzędzia Vagrant w systemie Mac OS X).

4. Rozwiń węzeł Aplikacje i zwróć uwagę, że istnieje teraz wpis dla danego typu aplikacji i inny wpis dla pierwszego wystąpienia tego typu.

## <a name="start-the-test-client-and-perform-a-failover"></a>Uruchamianie klienta testowego i przechodzenie w tryb failover
Projekty aktora nie działają samodzielnie. Wymagają one wysyłania im komunikatów przez inną usługę lub klienta. Szablon aktora zawiera prosty skrypt testowy, którego można użyć do interakcji z usługą aktora.

1. Uruchom skrypt za pomocą narzędzia kontrolnego, aby wyświetlić dane wyjściowe usługi aktora.

    ```bash
    cd myactorsvcTestClient
    watch -n 1 ./testclient.sh
    ```

2. W narzędziu Service Fabric Explorer zlokalizuj węzeł, w którym znajduje się replika podstawowa usługi aktora. Na poniższym zrzucie ekranu jest to węzeł 3.

    ![Znajdowanie repliki podstawowej w narzędziu Service Fabric Explorer][sfx-primary]

3. Kliknij węzeł znaleziony w poprzednim kroku, a następnie wybierz pozycję **Dezaktywuj (uruchom ponownie)** z menu Akcje. Ta czynność spowoduje ponowne uruchomienie jednego z pięciu węzłów w klastrze lokalnym i wymuszenie przejścia w tryb failover do jednej z replik pomocniczych uruchomionych w innym węźle. Podczas wykonywania tej czynności należy zwrócić uwagę na dane wyjściowe z klienta testowego oraz to, że w liczniku będzie nadal występować przyrost niezależnie od trybu failover.

## <a name="create-and-deploy-an-application-with-the-eclipse-neon-plugin"></a>Tworzenie i wdrażanie aplikacji za pomocą wtyczki środowiska Eclipse Neon

Usługa Service Fabric udostępnia także możliwości tworzenia, kompilowania i wdrażania aplikacji Java usługi Service Fabric za pomocą środowiska Eclipse. Podczas instalowania środowiska Eclipse wybierz opcję **Eclipse IDE dla deweloperów Java**. Usługa Service Fabric aktualnie obsługuje także wtyczkę dla środowiska Eclipse **Neon**. Zapoznaj się ze szczegółową dokumentacją — [Create and deploy your first Service Fabric Java application on Linux using Service Fabric Plugin for Eclipse on Linux](service-fabric-get-started-eclipse.md) (Tworzenie i wdrażanie pierwszej aplikacji Java usługi Service Fabric w systemie Linux przy użyciu wtyczki usługi Service Fabric dla środowiska Eclipse w systemie Linux)

## <a name="adding-more-services-to-an-existing-application"></a>Dodawanie kolejnych usług do istniejącej aplikacji

### <a name="using-command-line-utility"></a>Używanie narzędzia wiersza polecenia
Aby dodać kolejną usługę do aplikacji utworzonej już przy użyciu polecenia `yo`, wykonaj następujące czynności:
1. Zmień katalog na katalog główny istniejącej aplikacji.  Na przykład wpisz polecenie `cd ~/YeomanSamples/MyApplication`, jeśli aplikacja `MyApplication` to aplikacja utworzona przez narzędzie Yeoman.
2. Uruchom polecenie `yo azuresfjava:AddService`

### <a name="using-service-fabric-eclipse-plugin-for-java-on-linux"></a>Używanie wtyczki usługi Service Fabric środowiska Eclipse w systemie Linux
Aby dodać usługę do istniejącej aplikacji utworzonej przy użyciu wtyczki usługi Service Fabric środowiska Eclipse, zapoznaj się z dokumentacją dostępną [tutaj](service-fabric-get-started-eclipse.md#add-new-service-fabric-service-to-your-service-fabric-application).

## <a name="next-steps"></a>Następne kroki
* [Create and deploy your first Service Fabric Java application on Linux using Service Fabric Plugin for Eclipse on Linux](service-fabric-get-started-eclipse.md) (Tworzenie i wdrażanie pierwszej aplikacji Java usługi Service Fabric w systemie Linux przy użyciu wtyczki usługi Service Fabric dla środowiska Eclipse w systemie Linux)
* [Dowiedz się więcej o usłudze Reliable Actors](service-fabric-reliable-actors-introduction.md)
* [Interakcja z klastrami usługi Service Fabric przy użyciu interfejsu wiersza polecenia platformy Azure](service-fabric-azure-cli.md)
* [Rozwiązywanie problemów z wdrażaniem](service-fabric-azure-cli.md#troubleshooting)
* Uzyskaj informacje o [opcjach pomocy technicznej usługi Service Fabric](service-fabric-support.md)

<!-- Images -->
[sf-yeoman]: ./media/service-fabric-create-your-first-linux-application-with-java/sf-yeoman.png
[sfx-primary]: ./media/service-fabric-create-your-first-linux-application-with-java/sfx-primary.png
[sf-eclipse-templates]: ./media/service-fabric-create-your-first-linux-application-with-java/sf-eclipse-templates.png

