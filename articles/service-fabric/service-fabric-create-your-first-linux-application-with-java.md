---
title: "Tworzenie pierwszej aplikacji usługi Service Fabric w systemie Linux przy użyciu platformy Java | Microsoft Docs"
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
ms.date: 10/04/2016
ms.author: seanmck
translationtype: Human Translation
ms.sourcegitcommit: 2cf98a0ef478a058c03122d3e027ef37e2404a09
ms.openlocfilehash: 8a7b100a531ea1dd5420451064fdfb1eb3f21782


---
# <a name="create-your-first-azure-service-fabric-application"></a>Tworzenie pierwszej aplikacji usługi Azure Service Fabric
> [!div class="op_single_selector"]
> * [C# — Windows](service-fabric-create-your-first-application-in-visual-studio.md)
> * [Java — Linux](service-fabric-create-your-first-linux-application-with-java.md)
> * [C# — Linux](service-fabric-create-your-first-linux-application-with-csharp.md)
> 
> 

Usługa Service Fabric udostępnia zestawy SDK do kompilowania usług w systemie Linux przy użyciu platform .NET Core i Java. W tym samouczku utworzymy aplikację dla systemu Linux i skompilujemy usługę przy użyciu języka Java.  Poniższe wideo z witryny Microsoft Virtual Academy zawiera również szczegółowe omówienie procesu tworzenia aplikacji Java w systemie Linux:  
<center><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=DOX8K86yC_206218965">  
<img src="./media/service-fabric-create-your-first-linux-application-with-java/LinuxVid.png" WIDTH="360" HEIGHT="244">  
</a></center>

> [!NOTE]
> Język Java, jako wbudowany język programowania pierwszej klasy, jest obsługiwany tylko dla podglądu w systemie Linux (planowana jest obsługa w systemie Windows). Jednak wszelkie aplikacje, a w tym aplikacje Java, mogą działać jako gościnne pliki wykonywalne lub wewnątrz kontenerów w systemie Windows lub Linux. Aby uzyskać więcej informacji, zobacz [Wdrażanie istniejącego pliku wykonywalnego w usłudze Azure Service Fabric](service-fabric-deploy-existing-app.md) i [Wdrażanie kontenerów w usłudze Service Fabric](service-fabric-deploy-container.md).
> 


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

## <a name="build-and-deploy-an-application-with-the-eclipse-neon-plugin"></a>Kompilowanie i wdrażanie aplikacji za pomocą wtyczki środowiska Eclipse Neon

Jeśli została zainstalowana [wtyczka usługi Service Fabric](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-get-started-linux#install-the-java-sdk-and-eclipse-neon-plugin-optional) dla środowiska Eclipse Neon, można używać jej do tworzenia, kompilowania i wdrażania aplikacji usługi Service Fabric skompilowanych przy użyciu języka Java.  Podczas instalowania środowiska Eclipse wybierz opcję **Eclipse IDE dla deweloperów Java**.

### <a name="create-the-application"></a>Tworzenie aplikacji

Wtyczka usługi Service Fabric jest dostępna za pośrednictwem funkcji rozszerzalności środowiska Eclipse.

1. W środowisku Eclipse wybierz pozycję **File > Other > Service Fabric** (Plik > Inne > Service Fabric). Zostanie wyświetlony zestaw opcji, w tym aktorzy i kontenery.
   
    ![Szablony usługi Service Fabric w środowisku Eclipse][sf-eclipse-templates]

2. W tym przypadku wybierz pozycję Stateless Service (Usługa bezstanowa).

3. Zostanie wyświetlony monit o potwierdzenie użycia perspektywy Service Fabric, która optymalizuje środowisko Eclipse pod kątem użycia projektów usługi Service Fabric. Wybierz pozycję „Yes” (Tak).

### <a name="deploy-the-application"></a>Wdrażanie aplikacji
Szablony usługi Service Fabric zawierają zestaw zadań narzędzia Gradle służący do kompilowania i wdrażania aplikacji, który można wywoływać za pośrednictwem środowiska Eclipse. 

1. Wybierz pozycję **Run > Run Configurations** (Uruchom > Konfiguracje uruchamiania).
2. Wybierz opcję **local** (lokalna) lub **cloud** (w chmurze). Domyślna konfiguracja to **local** (lokalna). W przypadku wdrażania w klastrze zdalnym wybierz opcję **cloud** (w chmurze).
3. Dopilnuj, aby w profilach publikowania zostały umieszczone odpowiednie informacje, edytując plik `local.json` lub `cloud.json` zgodnie z wymaganiami.
4. Kliknij pozycję **Run** (Uruchom).

Twoja aplikacja zostanie skompilowana i wdrożona w ciągu kilku minut. Możesz monitorować jej stan z poziomu narzędzia Service Fabric Explorer.

## <a name="adding-more-services-to-an-existing-application"></a>Dodawanie kolejnych usług do istniejącej aplikacji

Aby dodać kolejną usługę do aplikacji utworzonej już przy użyciu polecenia `yo`, wykonaj następujące czynności: 
1. Zmień katalog na katalog główny istniejącej aplikacji.  Na przykład wpisz polecenie `cd ~/YeomanSamples/MyApplication`, jeśli aplikacja `MyApplication` to aplikacja utworzona przez narzędzie Yeoman.
2. Uruchom polecenie `yo azuresfjava:AddService`


## <a name="next-steps"></a>Następne kroki
* [Dowiedz się więcej o usłudze Reliable Actors](service-fabric-reliable-actors-introduction.md)
* [Interakcja z klastrami usługi Service Fabric przy użyciu interfejsu wiersza polecenia platformy Azure](service-fabric-azure-cli.md)
* [Rozwiązywanie problemów z wdrażaniem](service-fabric-azure-cli.md#troubleshooting)
* Uzyskaj informacje o [opcjach pomocy technicznej usługi Service Fabric](service-fabric-support.md)

<!-- Images -->
[sf-yeoman]: ./media/service-fabric-create-your-first-linux-application-with-java/sf-yeoman.png
[sfx-primary]: ./media/service-fabric-create-your-first-linux-application-with-java/sfx-primary.png
[sf-eclipse-templates]: ./media/service-fabric-create-your-first-linux-application-with-java/sf-eclipse-templates.png



<!--HONumber=Jan17_HO1-->


