<properties
   pageTitle="Pierwsze kroki wdrażania i uaktualniania aplikacji w klastrze lokalnym | Microsoft Azure"
   description="Konfigurowanie lokalnego klastra usługi Service Fabric, wdrażanie w nim istniejącej aplikacji, a następnie uaktualnianie tej aplikacji."
   services="service-fabric"
   documentationCenter=".net"
   authors="rwike77"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="04/12/2016"
   ms.author="ryanwi"/>

# Pierwsze kroki wdrażania i aktualizowania aplikacji w klastrze lokalnym
Zestaw SDK usługi Azure Service Fabric zawiera pełne lokalne środowisko deweloperskie, którego można użyć, aby szybko rozpocząć wdrażanie aplikacji i zarządzanie nimi w klastrze lokalnym. Z tego artykułu dowiesz się, jak utworzyć klaster lokalny, wdrożyć w nim istniejącą aplikację, a następnie uaktualnić ją do nowej wersji — a wszystko to z programu Windows PowerShell.

> [AZURE.NOTE] W tym artykule przyjęto, że czynności [konfigurowania środowiska deweloperskiego](service-fabric-get-started.md) zostały już ukończone.

## Tworzenie klastra lokalnego
Klaster usługi Service Fabric stanowi zestaw zasobów sprzętowych, w którym można wdrażać aplikacje. Zazwyczaj klaster składa się z dowolnej liczby maszyn z zakresu od pięciu sztuk do wielu tysięcy. Jednak zestaw SDK usługi Service Fabric obejmuje konfigurację klastra, która może działać na jednej maszynie.

Należy zrozumieć, że klaster lokalny usługi Service Fabric nie jest emulatorem ani symulatorem. Jest na nim wykonywany ten sam kod platformy, który można znaleźć w klastrach obejmujących wiele maszyn. Jedyna różnica polega na tym, że na jednej maszynie realizuje on procesy platformowe, które standardowo są rozmieszczone na pięciu maszynach.

Zestaw SDK udostępnia dwa sposoby instalacji klastra lokalnego: skrypt programu Windows PowerShell i aplikacja Local Cluster Manager dostępna na pasku zadań. W tym samouczku używamy skryptu programu PowerShell.

> [AZURE.NOTE] Jeśli utworzono już klaster lokalny poprzez wdrożenie aplikacji z programu Visual Studio, tę sekcję można pominąć.


1. Uruchom nowe okno programu PowerShell jako administrator.

2. Uruchom skrypt instalacji klastra z folderu zestawu SDK:

    ```powershell
    & "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\ClusterSetup\DevClusterSetup.ps1"
    ```

    Instalacja klastra potrwa kilka chwil. Po zakończeniu instalacji powinny być widoczne dane wyjściowe podobne do poniższych:

    ![Dane wyjściowe instalacji klastra][cluster-setup-success]

    Teraz możesz spróbować wdrożyć aplikację na swoim klastrze.

## Wdrażanie aplikacji
Zestaw SDK usługi Service Fabric zawiera bogaty zestaw struktur i narzędzi programistycznych przeznaczonych do tworzenia aplikacji. Jeśli chcesz się dowiedzieć, jak tworzyć aplikacje w programie Visual Studio, zobacz [Tworzenie pierwszej aplikacji usługi Service Fabric w programie Visual Studio](service-fabric-create-your-first-application-in-visual-studio.md).

W tym samouczku używamy istniejącej aplikacji przykładowej (o nazwie WordCount), dzięki czemu możemy skupić się na aspektach zarządzania na platformie, takich jak wdrażanie, monitorowanie i uaktualnianie.


1. Uruchom nowe okno programu PowerShell jako administrator.

2. Zaimportuj moduł programu PowerShell zestawu SDK usługi Service Fabric.

    ```powershell
    Import-Module "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\Tools\PSModule\ServiceFabricSDK\ServiceFabricSDK.psm1"
    ```

3. Utwórz katalog do przechowywania aplikacji, którą pobierzesz i wdrożysz, na przykład C:\ServiceFabric.

    ```powershell
    mkdir c:\ServiceFabric\
    cd c:\ServiceFabric\
    ```

4. [Pobierz aplikację WordCount](http://aka.ms/servicefabric-wordcountapp) do utworzonej lokalizacji.

5. Nawiąż połączenie z klastrem lokalnym:

    ```powershell
    Connect-ServiceFabricCluster localhost:19000
    ```

6. Wywołaj polecenie wdrożenia zestawu SDK, aby utworzyć nową aplikację, podając nazwę pakietu aplikacji i ścieżkę do niego.

    ```powershell  
  Publish-NewServiceFabricApplication -ApplicationPackagePath c:\ServiceFabric\WordCountV1.sfpkg -ApplicationName "fabric:/WordCount"
    ```

    Jeśli wszystko przebiegnie poprawnie, powinny pojawić się dane wyjściowe podobne do następujących:

    ![Wdrażanie aplikacji w klastrze lokalnym][deploy-app-to-local-cluster]

7. Aby zobaczyć aplikację w akcji, uruchom przeglądarkę i przejdź pod adres [http://localhost:8081/wordcount/index.html](http://localhost:8081/wordcount/index.html). Powinny zostać wyświetlone informacje podobne do następujących:

    ![Interfejs użytkownika wdrożonej aplikacji][deployed-app-ui]

    Aplikacja WordCount jest bardzo prosta. Zawiera kliencki kod JavaScript generujący losowe pięcioznakowe „słowa”, które są następnie przekazywane do aplikacji za pośrednictwem interfejsu API ASP.NET Web. Usługa stanowa śledzi liczbę zliczonych słów. Są one przydzielane do partycji na podstawie pierwszego znaku słowa.

    Aplikacja, którą wdrożyliśmy, zawiera cztery partycje. Słowa zaczynające się na litery od A do G są przechowywane w pierwszej partycji, słowa zaczynające się na litery od H do N są przechowywane w drugiej partycji i tak dalej.

## Wyświetlanie szczegółów i stanu aplikacji
Po wdrożeniu aplikacji przyjrzymy się części jej szczegółów w programie PowerShell.

1. Wygeneruj zapytanie dotyczące wszystkich aplikacji wdrożonych w klastrze:

    ```powershell
    Get-ServiceFabricApplication
    ```

    Jeśli została wdrożona tylko aplikacja WordCount, powinny pojawić się dane podobne do poniższych:

    ![Zapytanie dotyczące wszystkich wdrożonych aplikacji w powłoce PowerShell][ps-getsfapp]

2. Przejdź do następnego poziomu, generując zapytanie dotyczące zestawu usług zawartych w aplikacji WordCount.

    ```powershell
    Get-ServiceFabricService -ApplicationName 'fabric:/WordCount'
    ```

    ![Lista usług dla aplikacji w programie PowerShell][ps-getsfsvc]

    Aplikacja składa się z dwóch usług — frontonu sieci Web i usługi stanowej, która zarządza słowami.

3. Na koniec przyjrzyjmy się liście partycji dla usługi WordCountService:

    ```powershell
    Get-ServiceFabricPartition 'fabric:/WordCount/WordCountService'
    ```

    ![Widok partycji usługi w programie PowerShell][ps-getsfpartitions]

    Zestaw poleceń, z których korzystaliśmy, podobnie jak wszystkie polecenia programu PowerShell usługi Service Fabric, jest dostępny dla dowolnego klastra, z którym można nawiązać połączenie — lokalnego lub zdalnego.

    Interakcje z klastrem w sposób bardziej wizualny można realizować przy użyciu opartego na sieci Web narzędzia Service Fabric Explorer, które jest dostępne po przejściu w przeglądarce pod adres [http://localhost:19080/Explorer](http://localhost:19080/Explorer).

    ![Wyświetlanie szczegółów aplikacji w narzędziu Service Fabric Explorer][sfx-service-overview]

    > [AZURE.NOTE] Aby uzyskać więcej informacji na temat narzędzia Service Fabric Explorer, zobacz [Visualizing your cluster with Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) (Wizualizacja klastra przy użyciu narzędzia Service Fabric Explorer).

## Uaktualnianie aplikacji
Usługa Service Fabric realizuje uaktualnienia bez przestojów, ponieważ monitoruje stan aplikacji wdrażanej w klastrze. Przeprowadźmy proste uaktualnienie aplikacji WordCount.

Nowa wersja aplikacji będzie teraz zliczać tylko słowa zaczynające się od samogłosek. Podczas wdrażania tego uaktualnienia zaobserwujemy dwie zmiany w zachowaniu aplikacji. Po pierwsze szybkości narastania licznika powinna być mniejsza, ponieważ liczba zliczanych słów będzie mniejsza. Po drugie w pierwszej partycji znajdują się dwie samogłoski (A i E), a wszystkie pozostałe partycje zawierają po jednej sylabie, dlatego licznik pierwszej partycji powinien po pewnym czasie narastać szybciej niż pozostałych.

1. [Pobierz pakiet WordCount v2](http://aka.ms/servicefabric-wordcountappv2) do tej samej lokalizacji, do której pobrano pakiet w wersji 1.

2. Wróć do okna programu PowerShell i użyj w zestawie SDK polecenia uaktualniania, aby zarejestrować nową wersję w klastrze. Następnie rozpocznij uaktualnianie aplikacji fabric:/WordCount.

    ```powershell
    Publish-UpgradedServiceFabricApplication -ApplicationPackagePath C:\ServiceFabric\WordCountV2.sfpkg -ApplicationName "fabric:/WordCount" -UpgradeParameters @{"FailureAction"="Rollback"; "UpgradeReplicaSetCheckTimeout"=1; "Monitored"=$true; "Force"=$true}
    ```

    Po rozpoczęciu uaktualnienia dane wyjściowe w programie PowerShell powinny przypominać dane widoczne poniżej.

    ![Postęp uaktualniania w programie PowerShell][ps-appupgradeprogress]

3. Podczas uaktualniania łatwiejsze może być monitorowanie stanu tego procesu z narzędzia Service Fabric Explorer. Uruchom okno przeglądarki i przejdź pod adres [http://localhost:19080/Explorer](http://localhost:19080/Explorer). Rozwiń węzeł **Aplikacje** w drzewie po lewej stronie, wybierz pozycję **WordCount**, a na koniec wybierz pozycję **fabric:/WordCount**. Na karcie danych podstawowych będzie widoczny stan uaktualnienia obejmującego domeny uaktualnienia klastra.

    ![Postęp uaktualniania w narzędziu Service Fabric Explorer][sfx-upgradeprogress]

    W trakcie uaktualnienia w poszczególnych domenach wykonywane jest sprawdzanie kondycji w celu zapewnienia, że aplikacja zachowuje się prawidłowo.

4. Jeśli uruchomisz wcześniejsze zapytanie względem zestawu usług zawartych w aplikacji fabric:/WordCount, zauważysz, że wersja usługi WordCountService uległa zmianie, ale wersja usługi WordCountWebService nie zmieniła się:

    ```powershell
    Get-ServiceFabricService -ApplicationName 'fabric:/WordCount'
    ```

    ![Zapytanie dotyczące usług aplikacji po uaktualnieniu][ps-getsfsvc-postupgrade]

    Ten rezultat podkreśla sposób, w jaki usługa Service Fabric zarządza uaktualnieniami aplikacji. Działania związane z uaktualnieniami są realizowane tylko względem zestawu usług (albo pakietów kodu/konfiguracji w tych usługach), które uległy zmianie, co sprawia, że proces uaktualniania przebiega szybciej i bardziej niezawodnie.

5. Na koniec wróć do przeglądarki, aby przyjrzeć się zachowaniu nowej wersji aplikacji. Zgodnie z oczekiwaniami liczniki narastają wolniej, a ilość w pierwszej partycji jest nieznacznie większa.

    ![Widok nowej wersji aplikacji w przeglądarce][deployed-app-ui-v2]

## Następne kroki
- Po wdrożeniu i uaktualnieniu wstępnie przygotowanych aplikacji możesz [spróbować utworzyć własne aplikacje w programie Visual Studio](service-fabric-create-your-first-application-in-visual-studio.md).
- Wszystkie akcje wykonane w tym artykule w klastrze lokalnym można również wykonać w [klastrze platformy Azure](service-fabric-cluster-creation-via-portal.md).
- Uaktualnienie wykonane w tym artykule było bardzo podstawowe. Aby dowiedzieć się więcej o możliwościach i elastyczności uaktualnień w usłudze Service Fabric, zobacz [dokumentację uaktualniania](service-fabric-application-upgrade.md).

<!-- Images -->

[cluster-setup-success]: ./media/service-fabric-get-started-with-a-local-cluster/LocalClusterSetup.png
[extracted-app-package]: ./media/service-fabric-get-started-with-a-local-cluster/ExtractedAppPackage.png
[deploy-app-to-local-cluster]: ./media/service-fabric-get-started-with-a-local-cluster/DeployAppToLocalCluster.png
[deployed-app-ui]: ./media/service-fabric-get-started-with-a-local-cluster/DeployedAppUI-v1.png
[deployed-app-ui-v2]: ./media/service-fabric-get-started-with-a-local-cluster/DeployedAppUI-PostUpgrade.png
[sfx-app-instance]: ./media/service-fabric-get-started-with-a-local-cluster/SfxAppInstance.png
[sfx-two-app-instances-different-partitions]: ./media/service-fabric-get-started-with-a-local-cluster/SfxTwoAppInstances-DifferentPartitionCount.png
[ps-getsfapp]: ./media/service-fabric-get-started-with-a-local-cluster/PS-GetSFApp.png
[ps-getsfsvc]: ./media/service-fabric-get-started-with-a-local-cluster/PS-GetSFSvc.png
[ps-getsfpartitions]: ./media/service-fabric-get-started-with-a-local-cluster/PS-GetSFPartitions.png
[ps-appupgradeprogress]: ./media/service-fabric-get-started-with-a-local-cluster/PS-AppUpgradeProgress.png
[ps-getsfsvc-postupgrade]: ./media/service-fabric-get-started-with-a-local-cluster/PS-GetSFSvc-PostUpgrade.png
[sfx-upgradeprogress]: ./media/service-fabric-get-started-with-a-local-cluster/SfxUpgradeOverview.png
[sfx-service-overview]: ./media/service-fabric-get-started-with-a-local-cluster/sfx-service-overview.png



<!--HONumber=Jun16_HO2-->


