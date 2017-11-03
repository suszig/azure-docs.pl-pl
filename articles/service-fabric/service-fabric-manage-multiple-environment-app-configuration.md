---
title: "Zarządzanie wiele środowisk w sieci szkieletowej usług | Dokumentacja firmy Microsoft"
description: "W klastrach, w zakresie rozmiaru maszyn z jednego komputera można uruchomić aplikacji sieci szkieletowej usług. W niektórych przypadkach należy skonfigurować aplikację inaczej w przypadku tych środowisk zróżnicowane. W tym artykule opisano sposób definiowania parametry różnych aplikacji dla środowiska."
services: service-fabric
documentationcenter: .net
author: mikkelhegn
manager: timlt
editor: 
ms.assetid: f406eac9-7271-4c37-a0d3-0a2957b60537
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: mikkelhegn
ms.openlocfilehash: 671cc9b0f7b7b37fcf5b052f7e34bc98e66b2838
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="manage-application-parameters-for-multiple-environments"></a>Parametry aplikacji dla wielu środowisk zarządzania
Można utworzyć klastry z sieci szkieletowej usług Azure przy użyciu dowolnej lokalizacji z jednego do wielu tysięcy komputerów. Podczas plików binarnych aplikacji można uruchomić bez żadnych modyfikacji w tym szerokie spektrum środowisk, często chcesz skonfigurować aplikację różnie w zależności od liczby maszyn, które wdrażasz do.

Jako przykład prostego, należy wziąć pod uwagę `InstanceCount` usługi bezstanowej. Po uruchomieniu aplikacji na platformie Azure mają zwykle Ustaw ten parametr przyjmuje wartość specjalną-"1". Taka konfiguracja powoduje, że usługa działa na każdym węźle w klastrze (lub każdego węzła w typu węzła, jeśli ustawiono ograniczenia umieszczania). Jednak ta konfiguracja nie jest odpowiednia dla klastra pojedynczego komputera, ponieważ nie może mieć wiele procesów, które nasłuchują na tym samym punkcie końcowym na jednym komputerze. Zamiast tego należy zwykle ustawić `InstanceCount` "1".

## <a name="specifying-environment-specific-parameters"></a>Określanie parametrów określonego środowiska
Rozwiązanie tego problemu konfiguracji to zestaw usług domyślnych sparametryzowanych i pliki parametrów aplikacji, które wypełnić wartości tych parametrów dla danego środowiska. Parametry aplikacji i usług domyślnych są konfigurowane w manifestów aplikacji i usług. Definicja schematu dla plików ServiceManifest.xml i ApplicationManifest.xml jest instalowany z zestawu SDK sieci szkieletowej usług i narzędzi do *C:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*.

### <a name="default-services"></a>Domyślne usługi
Aplikacje usługi sieć szkieletowa składają się z kolekcji wystąpień usługi. Gdy to możliwe, należy utworzyć pustą aplikację, a następnie utwórz dynamicznie wszystkich wystąpień usługi, większość aplikacji ma zestaw podstawowe usługi, które należy zawsze utworzyć podczas tworzenia wystąpienia klasy aplikacji. Te są określane jako "domyślne usługi". Są one określone w manifeście aplikacji z symbole zastępcze-środowisko konfiguracji zawarte w nawiasach kwadratowych:

```xml
  <DefaultServices>
      <Service Name="Stateful1">
          <StatefulService
              ServiceTypeName="Stateful1Type"
              TargetReplicaSetSize="[Stateful1_TargetReplicaSetSize]"
              MinReplicaSetSize="[Stateful1_MinReplicaSetSize]">

              <UniformInt64Partition
                  PartitionCount="[Stateful1_PartitionCount]"
                  LowKey="-9223372036854775808"
                  HighKey="9223372036854775807"
              />
        </StatefulService>
    </Service>
  </DefaultServices>
```

Każdy nazwane parametry muszą być zdefiniowane w elemencie parametry dla manifest aplikacji:

```xml
    <Parameters>
        <Parameter Name="Stateful1_MinReplicaSetSize" DefaultValue="3" />
        <Parameter Name="Stateful1_PartitionCount" DefaultValue="1" />
        <Parameter Name="Stateful1_TargetReplicaSetSize" DefaultValue="3" />
    </Parameters>
```

Atrybut DefaultValue określa wartość do użycia w przypadku braku parametru bardziej specyficzne dla danego środowiska.

> [!NOTE]
> Nie wszystkie parametry wystąpienia usługi są odpowiednie dla konfiguracji poszczególnych środowiska. W powyższym przykładzie LowKey i HighKey wartości schemat partycjonowania usługi są jawnie zdefiniowany dla wszystkich wystąpień usługi, ponieważ zakres partycji jest funkcją domeny danych, nie środowiska.
> 
> 

### <a name="per-environment-service-configuration-settings"></a>Ustawienia konfiguracji usługi na środowisko
[Model aplikacji usługi sieć szkieletowa](service-fabric-application-model.md) umożliwia usługom zawierać pakiety konfiguracji, które zawierają niestandardowe pary klucz wartość, które są do odczytu w czasie wykonywania. Wartości tych ustawień można również zróżnicowane przez środowisko, określając `ConfigOverride` w manifeście aplikacji.

Załóżmy, że zostały następujące ustawienia w pliku Config\Settings.xml `Stateful1` usługi:

```xml
  <Section Name="MyConfigSection">
     <Parameter Name="MaxQueueSize" Value="25" />
  </Section>
```
Aby zastąpić tę wartość dla określonej aplikacji środowiskową pary, Utwórz `ConfigOverride` podczas importowania manifestu usługi w manifeście aplikacji.

```xml
  <ConfigOverrides>
     <ConfigOverride Name="Config">
        <Settings>
           <Section Name="MyConfigSection">
              <Parameter Name="MaxQueueSize" Value="[Stateful1_MaxQueueSize]" />
           </Section>
        </Settings>
     </ConfigOverride>
  </ConfigOverrides>
```
Ten parametr może zostać następnie skonfigurowane przez środowisko zgodnie z powyższym. Można to zrobić, deklarowanie go w sekcji parametrów w manifeście aplikacji i określając wartości określonego środowiska w pliki parametrów aplikacji.

> [!NOTE]
> W przypadku ustawienia konfiguracji usługi, istnieją trzy miejsca, w którym można ustawić wartość klucza: pakiet konfiguracji usługi, manifest aplikacji i pliku parametrów aplikacji. Zawsze wybierze sieci szkieletowej usług z pliku parametrów aplikacji pierwszego (Jeśli określono), następnie manifest aplikacji, a na końcu pakiet konfiguracji.
> 
> 

### <a name="setting-and-using-environment-variables"></a>Ustawianie i za pomocą zmiennych środowiskowych 
Można określić i ustaw zmienne środowiskowe w pliku ServiceManifest.xml i następnie zastąpić je w pliku ApplicationManifest.xml dla poszczególnych wystąpień.
W poniższym przykładzie przedstawiono dwie zmienne środowiskowe, jeden zestaw wartości, a druga zostanie zastąpiona. Można ustawić wartości zmiennych środowiskowych w taki sam sposób, jak te były używane dla zastąpień konfiguracji, mogą używać parametrów aplikacji.

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
Aby zastąpić zmienne środowiskowe w pliku ApplicationManifest.xml, odwoływania się do pakietu kodu w ServiceManifest z `EnvironmentOverrides` elementu.

```xml
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="FrontEndServicePkg" ServiceManifestVersion="1.0.0" />
    <EnvironmentOverrides CodePackageRef="MyCode">
      <EnvironmentVariable Name="MyEnvVariable" Value="mydata"/>
    </EnvironmentOverrides>
  </ServiceManifestImport>
 ``` 
 Po utworzeniu wystąpienia usługi o nazwie można uzyskać dostępu do zmiennych środowiskowych, z kodu. np. W C# możesz wykonać następujące czynności

```csharp
    string EnvVariable = Environment.GetEnvironmentVariable("MyEnvVariable");
```

### <a name="service-fabric-environment-variables"></a>Zmienne środowiskowe sieci szkieletowej usług
Sieć szkieletowa usług są wbudowane zmienne środowiskowe ustawione dla każdego wystąpienia usługi. Pełną listę zmiennych środowiskowych poniżej, gdzie pogrubioną czcionką w podane w są te, które będą używane w usłudze innych używane przez środowisko uruchomieniowe usługi sieć szkieletowa. 

* Fabric_ApplicationHostId
* Fabric_ApplicationHostType
* Fabric_ApplicationId
* **Fabric_ApplicationName**
* Fabric_CodePackageInstanceId
* **Fabric_CodePackageName**
* **TypeEndpoint Fabric_Endpoint_ [YourServiceName]**
* **Fabric_Folder_App_Log**
* **Fabric_Folder_App_Temp**
* **Fabric_Folder_App_Work**
* **Fabric_Folder_Application**
* Fabric_NodeId
* **Fabric_NodeIPOrFQDN**
* **Fabric_NodeName**
* Fabric_RuntimeConnectionAddress
* Fabric_ServicePackageInstanceId
* Fabric_ServicePackageName
* Fabric_ServicePackageVersionInstance
* FabricPackageFileName

Belows kod przedstawia sposób wyświetlania zmiennych środowiskowych sieci szkieletowej usług
 ```csharp
    foreach (DictionaryEntry de in Environment.GetEnvironmentVariables())
    {
        if (de.Key.ToString().StartsWith("Fabric"))
        {
            Console.WriteLine(" Environment variable {0} = {1}", de.Key, de.Value);
        }
    }
```
Poniżej podano przykłady zmiennych środowiskowych dla typu aplikacji o nazwie `GuestExe.Application` z typem usługi o nazwie `FrontEndService` uruchomienia na komputerze deweloperskim lokalnego.

* **Fabric_ApplicationName = fabric:/GuestExe.Application**
* **Fabric_CodePackageName = kod**
* **Fabric_Endpoint_FrontEndServiceTypeEndpoint = 80**
* **Fabric_NodeIPOrFQDN = localhost**
* **Fabric_NodeName = to węzeł _Node_2**

### <a name="application-parameter-files"></a>Pliki parametrów aplikacji
Projekt aplikacji platformy Service Fabric może zawierać co najmniej jeden plik parametrów aplikacji. Każde z nich definiuje określone wartości parametrów, które są zdefiniowane w manifeście aplikacji:

```xml
    <!-- ApplicationParameters\Local.xml -->

    <Application Name="fabric:/Application1" xmlns="http://schemas.microsoft.com/2011/01/fabric">
        <Parameters>
            <Parameter Name ="Stateful1_MinReplicaSetSize" Value="3" />
            <Parameter Name="Stateful1_PartitionCount" Value="1" />
            <Parameter Name="Stateful1_TargetReplicaSetSize" Value="3" />
        </Parameters>
    </Application>
```
Domyślnie nowa aplikacja obejmuje trzy pliki parametrów aplikacji, o nazwie Local.1Node.xml, Local.5Node.xml i Cloud.xml:

![Pliki parametrów aplikacji w Eksploratorze rozwiązań][app-parameters-solution-explorer]

Aby utworzyć plik parametrów, po prostu skopiuj i Wklej istniejący i nadaj mu nazwę nowej.

## <a name="identifying-environment-specific-parameters-during-deployment"></a>Identyfikowanie określonego środowiska parametrów podczas wdrażania
W czasie wdrażania musisz wybrać plik odpowiedni parametr, aby zastosować z aplikacją. Można to zrobić za pomocą okna dialogowego publikowanie w programie Visual Studio lub za pomocą programu PowerShell.

### <a name="deploy-from-visual-studio"></a>Wdrażanie w programie Visual Studio
Można wybrać z listy dostępnych parametrów plików podczas publikowania aplikacji w programie Visual Studio.

![Wybierz plik parametrów w oknie dialogowym publikowania][publishdialog]

### <a name="deploy-from-powershell"></a>Wdrażanie z programu PowerShell
`Deploy-FabricApplication.ps1` Skrypt programu PowerShell zawarte w szablonie projektu aplikacji akceptuje profil publikowania, jako parametr i PublishProfile zawiera odwołanie do pliku parametrów aplikacji.

  ```PowerShell
    ./Deploy-FabricApplication -ApplicationPackagePath <app_package_path> -PublishProfileFile <publishprofile_path>
  ```

## <a name="next-steps"></a>Następne kroki
Aby dowiedzieć się więcej na temat niektórych podstawowych pojęciach, które zostały omówione w tym temacie, zobacz [omówienie techniczne sieci szkieletowej usług](service-fabric-technical-overview.md). Aby uzyskać informacje o inne funkcje zarządzania aplikacjami, które są dostępne w programie Visual Studio, zobacz [Zarządzaj aplikacjami sieci szkieletowej usług w programie Visual Studio](service-fabric-manage-application-in-visual-studio.md).

<!-- Image references -->

[publishdialog]: ./media/service-fabric-manage-multiple-environment-app-configuration/publish-dialog-choose-app-config.png
[app-parameters-solution-explorer]:./media/service-fabric-manage-multiple-environment-app-configuration/app-parameters-in-solution-explorer.png
