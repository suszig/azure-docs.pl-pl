---
title: "Model aplikacji sieci szkieletowej usług Azure | Dokumentacja firmy Microsoft"
description: "Informacje o modelu i opisano aplikacji i usług w sieci szkieletowej usług."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: mani-ramaswamy
ms.assetid: 17a99380-5ed8-4ed9-b884-e9b827431b02
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: ryanwi
ms.openlocfilehash: e30482427b88eb3e58d39075c7f0734664b79aa2
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/18/2017
---
# <a name="model-an-application-in-service-fabric"></a>Model aplikacji w sieci szkieletowej usług
Ten artykuł zawiera omówienie modelu aplikacji sieci szkieletowej usług Azure i sposobu definiowania aplikacji i usług za pośrednictwem plików manifestu.

## <a name="understand-the-application-model"></a>Zrozumienie model aplikacji
Aplikacja jest kolekcja składników usług, które wykonywać niektórych funkcji lub funkcji. Usługa działa pełny i autonomiczne i można uruchomić i uruchomić niezależnie od innych usług.  Usługa składa się z kodu, konfiguracji i danych. Dla każdej usługi kod składa się z pliku wykonywalnego plików binarnych, konfiguracja obejmuje ustawienia usługi, które mogą zostać załadowane w czasie wykonywania i danych składa się z dowolnego statycznych danych do użycia przez usługę. Każdego składnika w tym modelu hierarchiczne aplikacji może być niezależnie określonej wersji, jak i uaktualnionych.

![Model aplikacji sieci szkieletowej usług][appmodel-diagram]

Typ aplikacji kategoryzacji aplikacji i składa się z pakietem typu usługi. Typ usługi jest kategoryzacji usługi. Klasyfikacja może mieć różne ustawienia i konfiguracje, ale do podstawowych funkcji jest taka sama. Wystąpień usługi są zmian konfiguracji innej usługi typu usługi.  

Klasy (lub "typów") aplikacji i usług opisanych za pomocą plików XML (manifestów aplikacji i manifestów usługi).  Manifesty są szablony, które można wdrożyć aplikacji z magazynu obrazu klastra. Definicja schematu dla pliku ServiceManifest.xml i ApplicationManifest.xml jest instalowany z zestawu SDK sieci szkieletowej usług i narzędzi do *C:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*.

Kod dla różnych aplikacji wystąpień Uruchom jako osobne procesy nawet wtedy, gdy pracujących na tym samym węźle sieci szkieletowej usług. Ponadto można zarządzać cyklem życia każdego wystąpienia aplikacji (na przykład uaktualnić) niezależnie. Na poniższym diagramie przedstawiono, jak typy aplikacji składają się z typów usług, które z kolei składają się z kodu, konfiguracji i danych. Aby uprościć na diagramie, pakietami tylko/config/danych kodu `ServiceType4` są wyświetlane, że każdy typ usługi to niektórych lub wszystkich tych typów pakietów.

![Typy aplikacji usługi Service Fabric i usługi][cluster-imagestore-apptypes]

Dwa różne pliki manifestu służą do opisywania aplikacji i usług: manifest usługi i manifest aplikacji. Manifesty są zostały szczegółowo opisane w poniższych sekcjach.

Może istnieć co najmniej jedno wystąpienie typu usługi active w klastrze. Na przykład wystąpienie usługi stanowej lub replik, osiągnąć wysoką niezawodnością poprzez replikację stanu między replikami znajdujących się w różnych węzłach w klastrze. Replikacja zasadniczo zapewnia nadmiarowość dla usługi, które mają być dostępne, nawet w przypadku awarii jednego węzła w klastrze. A [podzielona na partycje usługi](service-fabric-concepts-partitioning.md) dalsze dzieli jego stan (i wzorce dostępu do tego stanu) w węzłach w klastrze.

Na poniższym diagramie przedstawiono związek między aplikacjami i wystąpień usług, partycji i replik.

![Partycji i replik w ramach usługi][cluster-application-instances]

> [!TIP]
> Układ aplikacji można wyświetlić w klastrze za pomocą narzędzia Service Fabric Explorer dostępne w http://&lt;yourclusteraddress&gt;: 19080/Explorer. Aby uzyskać więcej informacji, zobacz [wizualizacja klastra za pomocą Eksploratora usługi sieć szkieletowa](service-fabric-visualizing-your-cluster.md).
> 
> 

## <a name="describe-a-service"></a>Opis usługi
Manifest usługi deklaratywnie definiuje typ usługi i wersji. Określa usługę metadane, takie jak typ usługi, właściwości kondycji metryki równoważenia obciążenia, pliki binarne usługi i pliki konfiguracyjne.  Innymi słowy, opisuje pakiety kodu, konfiguracji i danych, które tworzą pakietu usług do obsługi co najmniej jeden typ usługi. Poniżej przedstawiono prosty przykład manifest usługi:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<ServiceManifest Name="MyServiceManifest" Version="SvcManifestVersion1" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <Description>An example service manifest</Description>
  <ServiceTypes>
    <StatelessServiceType ServiceTypeName="MyServiceType" />
  </ServiceTypes>
  <CodePackage Name="MyCode" Version="CodeVersion1">
    <SetupEntryPoint>
      <ExeHost>
        <Program>MySetup.bat</Program>
      </ExeHost>
    </SetupEntryPoint>
    <EntryPoint>
      <ExeHost>
        <Program>MyServiceHost.exe</Program>
      </ExeHost>
    </EntryPoint>
    <EnvironmentVariables>
      <EnvironmentVariable Name="MyEnvVariable" Value=""/>
      <EnvironmentVariable Name="HttpGatewayPort" Value="19080"/>
    </EnvironmentVariables>
  </CodePackage>
  <ConfigPackage Name="MyConfig" Version="ConfigVersion1" />
  <DataPackage Name="MyData" Version="DataVersion1" />
</ServiceManifest>
```

**Wersja** atrybuty są ciągami bez struktury i nie zostały przeanalizowane przez system. Wersja atrybuty są używane do wersji każdego składnika dla uaktualnienia.

**Serivcetype** deklaruje, jakie typy usługi są obsługiwane przez **CodePackages** w tym manifeście. Usługa jest uruchomione na jednym z następujących typów usługi, wszystkie pakiety kodu zadeklarowany w tym manifeście zostaną aktywowane przez uruchomienie ich punktów wejścia. Wynikowa procesów oczekuje się zarejestrować obsługiwanych typów usług w czasie wykonywania. Typ usługi został zadeklarowany na poziomie manifestu i nie poziomu pakiet kodu. Dlatego w przypadku wielu pakietów kodu ich wszystkich aktywacji zawsze, gdy system wyszukuje jeden z typów usług zadeklarowane.

**SetupEntryPoint** punkt wejścia uprzywilejowanych uruchamiane przy użyciu poświadczeń usługi sieć szkieletowa (zazwyczaj *LocalSystem* konta) przed innymi punktu wejścia. Plik wykonywalny określony przez **punktu wejścia** jest zazwyczaj długotrwałe hosta usługi. Obecność punktu wejścia instalacji oddzielnych pozwala uniknąć konieczności uruchamiania usługi hosta z wysokiego poziomu uprawnień przez dłuższy czas. Plik wykonywalny określony przez **punktu wejścia** po zakończeniu **SetupEntryPoint** kończy się pomyślnie. Jeśli kiedykolwiek proces kończy lub ulegnie awarii, proces wynikowy jest monitorowane i ponownego uruchomienia (począwszy od ponownie **SetupEntryPoint**).  

Typowe scenariusze korzystania **SetupEntryPoint** są podczas uruchomienia pliku wykonywalnego przed uruchomieniem usługi lub wykonać operację z podwyższonym poziomem uprawnień. Na przykład:

* Konfigurowanie i Inicjowanie zmiennych środowiskowych, które wymaga pliku wykonywalnego usługi. To nie jest ograniczona do tylko pliki wykonywalne napisane przez modele programowania sieci szkieletowej usług. Na przykład npm.exe musi niektóre zmienne środowiskowe skonfigurowane do wdrażania aplikacji node.js.
* Konfigurowanie kontroli dostępu, instalując certyfikaty zabezpieczeń.

Aby uzyskać więcej informacji na temat konfigurowania **SetupEntryPoint** zobacz [skonfigurować zasady dla punktu wejścia instalacji usługi](service-fabric-application-runas-security.md)

**EnvironmentVariables** zawiera listę zmiennych środowiskowych, które są ustawione dla tego pakietu kodu. Zmienne Environmentment może zostać zastąpiona w `ApplicationManifest.xml` o podanie wartości różnych wystąpień innej usługi. 

**DataPackage** deklaruje folder o nazwie **nazwa** atrybutu, zawierającego dowolne dane statyczne zużywanych przez proces w czasie wykonywania.

**Lokalizacji ConfigPackage** deklaruje folder o nazwie **nazwa** atrybut, który zawiera *Settings.xml* pliku. Plik ustawień zawiera sekcje ustawień pary klucz wartość, zdefiniowanych przez użytkownika, które proces odczytuje Wstecz w czasie wykonywania. Podczas uaktualniania, jeśli tylko **lokalizacji ConfigPackage** **wersji** została zmieniona, a następnie uruchomiony proces nie zostanie ponownie uruchomiony. Zamiast tego wywołania zwrotnego powiadamia proces, który zmieniono ustawienia konfiguracji, mogą być ładowane dynamicznie. Oto przykład *Settings.xml* pliku:

```xml
<Settings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
  <Section Name="MyConfigurationSection">
    <Parameter Name="MySettingA" Value="Example1" />
    <Parameter Name="MySettingB" Value="Example2" />
  </Section>
</Settings>
```

> [!NOTE]
> Manifest usługi może zawierać wiele kodu, konfiguracji i danych. Każdy z tych może być kontrolą wersji niezależnie.
> 
> 

<!--
For more information about other features supported by service manifests, refer to the following articles:

*TODO: LoadMetrics, PlacementConstraints, ServicePlacementPolicies
*TODO: Resources
*TODO: Health properties
*TODO: Trace filters
*TODO: Configuration overrides
-->

## <a name="describe-an-application"></a>Opis aplikacji
Manifest aplikacji zawiera opis deklaratywnie, typ i wersja aplikacji. Określa usługę kompozycji metadane, takie jak nazwy stabilny oraz schemat partycji, współczynnik liczby/replikacji wystąpienia, zasady zabezpieczeń i izolacji, ograniczenia umieszczania, operacje zastępowania konfiguracji, typów usług składowych. Opisano również domen równoważenia obciążenia, w których znajduje się aplikacja.

W związku z tym manifest aplikacji zawiera opis elementów na poziomie aplikacji i odwołuje się do co najmniej jeden manifestów usługi utworzenie typu aplikacji. Poniżej przedstawiono prosty przykład manifestu aplikacji:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<ApplicationManifest
      ApplicationTypeName="MyApplicationType"
      ApplicationTypeVersion="AppManifestVersion1"
      xmlns="http://schemas.microsoft.com/2011/01/fabric"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <Description>An example application manifest</Description>
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="MyServiceManifest" ServiceManifestVersion="SvcManifestVersion1"/>
    <ConfigOverrides/>
    <EnvironmentOverrides CodePackageRef="MyCode"/>
  </ServiceManifestImport>
  <DefaultServices>
     <Service Name="MyService">
         <StatelessService ServiceTypeName="MyServiceType" InstanceCount="1">
             <SingletonPartition/>
         </StatelessService>
     </Service>
  </DefaultServices>
</ApplicationManifest>
```

Manifesty usługi, takich jak **wersji** atrybuty są ciągami bez struktury i nie są analizowane przez system. Atrybuty wersji są również używane do wersji każdego składnika dla uaktualnienia.

**ServiceManifestImport** zawiera odwołania do usługi manifestów, które tworzą ten typ aplikacji. Manifesty importowanych usługi określają, jakie typy usługi są prawidłowe w ramach tego typu aplikacji. W ramach ServiceManifestImport można zastąpić wartości konfiguracji w pliku Settings.xml i środowiska zmiennych w plikach pliku ServiceManifest.xml. 


**DefaultServices** deklaruje wystąpień usług, które są tworzone automatycznie, gdy aplikacja zostanie uruchomiony przed tego typu aplikacji. Domyślne usługi są po prostu udogodnienie i zachowywać się jak normalne usług w każdym względem po ich utworzeniu. Uaktualnienia wraz z innych usług, w przypadku aplikacji, a można również usunąć.

> [!NOTE]
> Manifest aplikacji może zawierać wiele importów manifestu usługi i usług domyślnych. Niezależnie może być wersjonowany każdego importu manifestu usługi.
> 
> 

Aby dowiedzieć się, jak należy dbać o różnych aplikacji i parametrów usługi dla poszczególnych środowisk, zobacz [Zarządzanie parametry aplikacji dla wielu środowiskach](service-fabric-manage-multiple-environment-app-configuration.md).

<!--
For more information about other features supported by application manifests, refer to the following articles:

*TODO: Application parameters
*TODO: Security, Principals, RunAs, SecurityAccessPolicy
*TODO: Service Templates
-->



## <a name="next-steps"></a>Następne kroki
[Pakiet aplikacji](service-fabric-package-apps.md) i przygotowania do wdrożenia.

[Wdrażanie i usunąć aplikacje] [ 10] opisano, jak zarządzać wystąpień aplikacji przy użyciu programu PowerShell.

[Zarządzanie parametry aplikacji dla wielu środowiskach] [ 11] opisano sposób konfigurowania parametrów i zmiennych środowiskowych dla wystąpień inną aplikację.

[Konfigurowania zasad zabezpieczeń dla aplikacji] [ 12] opisuje sposób uruchamiania usługi na podstawie zasad zabezpieczeń w celu ograniczenia dostępu.

[Hosting modeli aplikacji] [ 13] opis relacji między repliki (lub wystąpień) wdrożonej usługi i procesu hosta usługi.

<!--Image references-->
[appmodel-diagram]: ./media/service-fabric-application-model/application-model.png
[cluster-imagestore-apptypes]: ./media/service-fabric-application-model/cluster-imagestore-apptypes.png
[cluster-application-instances]: media/service-fabric-application-model/cluster-application-instances.png

<!--Link references--In actual articles, you only need a single period before the slash-->
[10]: service-fabric-deploy-remove-applications.md
[11]: service-fabric-manage-multiple-environment-app-configuration.md
[12]: service-fabric-application-runas-security.md
[13]: service-fabric-hosting-model.md
