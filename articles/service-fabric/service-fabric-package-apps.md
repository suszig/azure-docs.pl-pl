---
title: Pakiet Azure Service Fabric aplikacji | Dokumentacja firmy Microsoft
description: "Jak pakiet aplikacji usługi Service Fabric, przed wdrożeniem w klastrze."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: mani-ramaswamy
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: ryanwi
ms.openlocfilehash: 486a27d7ca576c8fe1552c02eb24ece6b8bb2ba8
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/18/2017
---
# <a name="package-an-application"></a>Pakiet aplikacji
W tym artykule opisano sposób pakietu aplikacji usługi Service Fabric i przygotowania do wdrożenia.

## <a name="package-layout"></a>Układ pakietu
Manifest aplikacji, co najmniej jeden manifestów usługi i inne pliki niezbędne pakietu muszą być zorganizowane w układzie określonej dla wdrożenia do klastra usługi sieć szkieletowa usług. Przykład manifesty w tym artykule musi być podzielone na następującą strukturę katalogów:

```
PS D:\temp> tree /f .\MyApplicationType

D:\TEMP\MYAPPLICATIONTYPE
│   ApplicationManifest.xml
│
└───MyServiceManifest
    │   ServiceManifest.xml
    │
    ├───MyCode
    │       MyServiceHost.exe
    │
    ├───MyConfig
    │       Settings.xml
    │
    └───MyData
            init.dat
```

Foldery noszą nazwy odpowiadające **nazwa** atrybuty każdego odpowiadającego mu elementu. Na przykład, jeśli manifest usługi zawiera dwa pakiety kodu z nazwami **MyCodeA** i **MyCodeB**, a następnie dwa foldery z tymi samymi nazwami zawierałoby niezbędne pliki binarne dla każdego pakietu kodu.

## <a name="use-setupentrypoint"></a>Użyj elementu SetupEntryPoint
Typowe scenariusze korzystania **SetupEntryPoint** po należy uruchomić plik wykonywalny przed uruchomieniem usługi lub należy wykonać operację z podwyższonym poziomem uprawnień. Na przykład:

* Konfigurowanie i Inicjowanie zmiennych środowiskowych, które wymaga pliku wykonywalnego usługi. Nie jest ograniczona do tylko pliki wykonywalne napisane przez modele programowania sieci szkieletowej usług. Na przykład npm.exe musi niektóre zmienne środowiskowe skonfigurowane do wdrażania aplikacji node.js.
* Konfigurowanie kontroli dostępu, instalując certyfikaty zabezpieczeń.

Aby uzyskać więcej informacji na temat konfigurowania **SetupEntryPoint**, zobacz [skonfigurować zasady dla punktu wejścia instalacji usługi](service-fabric-application-runas-security.md)

<a id="Package-App"></a>
## <a name="configure"></a>Konfigurowanie
### <a name="build-a-package-by-using-visual-studio"></a>Tworzenie pakietu przy użyciu programu Visual Studio
Jeśli używasz programu Visual Studio 2015 do tworzenia aplikacji, służy polecenie pakietu do automatycznego tworzenia pakietu, który odpowiada układu opisane powyżej.

Aby utworzyć pakiet, kliknij prawym przyciskiem myszy projekt aplikacji w Eksploratorze rozwiązań i wybierz polecenie pakietu, jak pokazano poniżej:

![Pakowanie aplikacji przy użyciu programu Visual Studio][vs-package-command]

Po zakończeniu tworzenia pakietów można znaleźć lokalizacji pakietu w **dane wyjściowe** okna. Krok pakowania odbywa się automatycznie podczas wdrażania lub debugowania aplikacji w programie Visual Studio.

### <a name="build-a-package-by-command-line"></a>Tworzenie pakietu przy użyciu wiersza polecenia
Istnieje również możliwość spakować programowo przy użyciu aplikacji `msbuild.exe`. Pod maską Visual Studio jest uruchomiony, dane wyjściowe jest taka sama.

```shell
D:\Temp> msbuild HelloWorld.sfproj /t:Package
```

## <a name="test-the-package"></a>Testowanie pakietu
Struktura pakietu lokalnie za pomocą programu PowerShell można sprawdzić za pomocą [ServiceFabricApplicationPackage testu](/powershell/module/servicefabric/test-servicefabricapplicationpackage?view=azureservicefabricps) polecenia.
To polecenie sprawdza dla manifest analizowania problemów i sprawdź wszystkie odwołania. To polecenie weryfikuje tylko strukturalnych poprawność katalogów i plików w pakiecie.
Nie Sprawdź dowolnej zawartości pakietu kodu lub danych poza sprawdzanie, czy wszystkie niezbędne pliki są obecne.

```
PS D:\temp> Test-ServiceFabricApplicationPackage .\MyApplicationType
False
Test-ServiceFabricApplicationPackage : The EntryPoint MySetup.bat is not found.
FileName: C:\Users\servicefabric\AppData\Local\Temp\TestApplicationPackage_7195781181\nrri205a.e2h\MyApplicationType\MyServiceManifest\ServiceManifest.xml
```

Ten błąd wskazuje, że *MySetup.bat* pliku, do którego odwołuje się manifestu usługi **SetupEntryPoint** brakuje pakietu kodu. Po dodaniu brakujący plik przekazuje weryfikacji aplikacji:

```
PS D:\temp> tree /f .\MyApplicationType

D:\TEMP\MYAPPLICATIONTYPE
│   ApplicationManifest.xml
│
└───MyServiceManifest
    │   ServiceManifest.xml
    │
    ├───MyCode
    │       MyServiceHost.exe
    │       MySetup.bat
    │
    ├───MyConfig
    │       Settings.xml
    │
    └───MyData
            init.dat

PS D:\temp> Test-ServiceFabricApplicationPackage .\MyApplicationType
True
PS D:\temp>
```

Jeśli aplikacja ma [parametry aplikacji](service-fabric-manage-multiple-environment-app-configuration.md) zdefiniowane, można przekazać je w [ServiceFabricApplicationPackage testu](/powershell/module/servicefabric/test-servicefabricapplicationpackage?view=azureservicefabricps) do właściwego weryfikacji.

Jeśli znasz klastra, gdzie aplikacja zostanie wdrożona, zalecane jest przekazywane w `ImageStoreConnectionString` parametru. W takim przypadku pakiet również jest weryfikowany pod kątem poprzednie wersje aplikacji, które już są uruchomione w klastrze. Na przykład walidacji może wykryć, czy pakiet o takiej samej wersji, ale inną zawartość została już wdrożona.  

Gdy aplikacja jest poprawnie spakowany i pozytywnej weryfikacji, należy ocenić na podstawie rozmiaru i liczbę plików, jeśli kompresja nie jest konieczne.

## <a name="compress-a-package"></a>Kompresuj pakietu
Gdy pakiet jest duży lub ma wiele plików, można skompresować je szybciej wdrożenia. Kompresja zmniejsza liczbę plików oraz rozmiar pakietu.
Dla pakietu aplikacji skompresowany [przekazywanie pakietu aplikacji](service-fabric-deploy-remove-applications.md#upload-the-application-package) może trwać dłużej, w porównaniu do przekazywania nieskompresowanych pakietu (Jeśli specjalnie czasu kompresji jest brana pod uwagę), ale [rejestrowanie](service-fabric-deploy-remove-applications.md#register-the-application-package) i [wyrejestrowanie typ aplikacji](service-fabric-deploy-remove-applications.md#unregister-an-application-type) są szybsze dla pakietu aplikacji skompresowane.

Mechanizm wdrażania jest takie samo skompresowanym i nieskompresowanym pakietów. Jeśli pakiet jest skompresowany, są przechowywane w związku z magazynu obrazu klastra i jest dekompresowane w węźle przed uruchomieniem aplikacji.
Kompresja zastępuje prawidłowy pakiet usługi sieć szkieletowa skompresowaną wersję. Folder muszą zezwalać na uprawnienia do zapisu. Systemem kompresji już skompresowanym pakietem daje żadnych zmian.

Pakiet można skompresować za pomocą polecenia programu Powershell [ServiceFabricApplicationPackage kopiowania](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) z `CompressPackage` przełącznika. Można zdekompresować pakietu z takimi samymi polecenia przy użyciu `UncompressPackage` przełącznika.

Następujące polecenie kompresuje pakiet, bez kopiowania go do magazynu obrazów. Możesz skopiować skompresowany pakiet do co najmniej jeden klaster sieci szkieletowej usług, zgodnie z potrzebami, za pomocą [ServiceFabricApplicationPackage kopiowania](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) bez `SkipCopy` flagi.
Pakiet zawiera obecnie pliki zip `code`, `config`, i `data` pakietów. Manifest aplikacji i manifestów usługi są nie zip, ponieważ są potrzebne do wielu operacji wewnętrznych (np. pakietu udostępniania, aplikacja nazwa i wersja wyodrębniania typu dla niektórych operacji sprawdzania poprawności).
Pakowanie manifesty spowodowałoby, te operacje nieefektywne.

```
PS D:\temp> tree /f .\MyApplicationType

D:\TEMP\MYAPPLICATIONTYPE
│   ApplicationManifest.xml
│
└───MyServiceManifest
    │   ServiceManifest.xml
    │
    ├───MyCode
    │       MyServiceHost.exe
    │       MySetup.bat
    │
    ├───MyConfig
    │       Settings.xml
    │
    └───MyData
            init.dat
PS D:\temp> Copy-ServiceFabricApplicationPackage -ApplicationPackagePath .\MyApplicationType -CompressPackage -SkipCopy

PS D:\temp> tree /f .\MyApplicationType

D:\TEMP\MYAPPLICATIONTYPE
│   ApplicationManifest.xml
│
└───MyServiceManifest
       ServiceManifest.xml
       MyCode.zip
       MyConfig.zip
       MyData.zip

```

Alternatywnie można kompresować i skopiować pakiet z [ServiceFabricApplicationPackage kopiowania](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) w jednym kroku.
Jeśli pakiet jest duży, podaj wystarczająco duża limitu czasu należy przewidzieć czas kompresję pakietu i przekazywania do klastra.
```
PS D:\temp> Copy-ServiceFabricApplicationPackage -ApplicationPackagePath .\MyApplicationType -ApplicationPackagePathInImageStore MyApplicationType -ImageStoreConnectionString fabric:ImageStore -CompressPackage -TimeoutSec 5400
```

Wewnętrznie Service Fabric oblicza sumy kontrolne dla pakietów aplikacji w celu weryfikacji. Korzystając z kompresji, sum kontrolnych są obliczane w wersjach zip każdego pakietu.
W przypadku kopiowania nieskompresowanych wersji pakietu aplikacji, i chcesz użyć kompresji dla tego samego pakietu, należy zmienić wersje `code`, `config`, i `data` pakietów, aby uniknąć błąd sumy kontrolnej. Jeśli pakiety nie uległy zmianie, a zmiana wersji, można użyć [różnic inicjowania obsługi administracyjnej](service-fabric-application-upgrade-advanced.md). Po wybraniu tej opcji nie dołączaj niezmienione pakietu zamiast niej odwołania z manifestu usługi.

Podobnie jeśli chcesz użyć pakietu nieskompresowanych przekazać skompresowaną wersję pakietu, musisz zaktualizować wersje, aby uniknąć błąd sumy kontrolnej.

Pakiet jest teraz spakowane poprawnie zweryfikowane i skompresowane (w razie potrzeby), aby była gotowa do [wdrożenia](service-fabric-deploy-remove-applications.md) do co najmniej jeden klaster sieci szkieletowej usług.

### <a name="compress-packages-when-deploying-using-visual-studio"></a>Kompresuj pakiety, w przypadku wdrażania przy użyciu programu Visual Studio
Możesz wydać Visual Studio, aby kompresować pakietów podczas wdrażania, dodając `CopyPackageParameters` elementu profil publikowania i ustaw `CompressPackage` atrybutu `true`.

``` xml
    <PublishProfile xmlns="http://schemas.microsoft.com/2015/05/fabrictools">
        <ClusterConnectionParameters ConnectionEndpoint="mycluster.westus.cloudapp.azure.com" />
        <ApplicationParameterFile Path="..\ApplicationParameters\Cloud.xml" />
        <CopyPackageParameters CompressPackage="true"/>
    </PublishProfile>
```

## <a name="next-steps"></a>Następne kroki
[Wdrażanie i usunąć aplikacje] [ 10] opisano, jak zarządzać wystąpień aplikacji przy użyciu programu PowerShell

[Zarządzanie parametry aplikacji dla wielu środowiskach] [ 11] opisano sposób konfigurowania parametrów i zmiennych środowiskowych dla wystąpień inną aplikację.

[Konfigurowania zasad zabezpieczeń dla aplikacji] [ 12] opisuje sposób uruchamiania usługi na podstawie zasad zabezpieczeń w celu ograniczenia dostępu.

<!--Image references-->
[vs-package-command]: ./media/service-fabric-package-apps/vs-package-command.png

<!--Link references--In actual articles, you only need a single period before the slash-->
[10]: service-fabric-deploy-remove-applications.md
[11]: service-fabric-manage-multiple-environment-app-configuration.md
[12]: service-fabric-application-runas-security.md
