---
title: "Tworzenie aplikacji kontenera dla systemu Windows w usłudze Azure Service Fabric | Microsoft Docs"
description: "Utwórz pierwszą aplikację kontenera systemu Windows w usłudze Azure Service Fabric."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: vturecek
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/05/2017
ms.author: ryanwi
ms.translationtype: HT
ms.sourcegitcommit: eeed445631885093a8e1799a8a5e1bcc69214fe6
ms.openlocfilehash: c4f8c94e23a165b22533ffd74e04c9a7310f2d22
ms.contentlocale: pl-pl
ms.lasthandoff: 09/07/2017

---

# <a name="deploy-a-service-fabric-windows-container-application-on-azure"></a>Wdrażanie aplikacji kontenera dla systemu Windows w usłudze Service Fabric na platformie Azure
Usługa Azure Service Fabric to platforma systemów rozproszonych ułatwiająca pakowanie i wdrażanie skalowalnych oraz niezawodnych mikrousług i kontenerów, a także zarządzanie nimi. 

Uruchomienie istniejącej aplikacji w kontenerze systemu Windows w klastrze usługi Service Fabric nie wymaga dokonywania żadnych zmian w aplikacji. W tym przewodniku Szybki start przedstawiono sposób wdrażania wstępnie skompilowanego obrazu kontenera platformy Docker w aplikacji usługi Service Fabric. Po zakończeniu będziesz mieć uruchomiony kontener systemu Windows Server 2016 Nano Server i usług IIS. W tym przewodniku Szybki start opisano wdrażanie kontenera dla systemu Windows. Aby wdrożyć kontener dla systemu Linux, zapoznaj się z [tym przewodnikiem Szybki start](service-fabric-quickstart-containers-linux.md).

![Domyślna strona internetowa usług IIS][iis-default]

W tym przewodniku Szybki start zawarto informacje na temat wykonywania następujących czynności:
> [!div class="checklist"]
> * Pakowanie kontenera obrazu platformy Docker
> * Konfigurowanie komunikacji
> * Kompilowanie i tworzenie pakietu aplikacji usługi Service Fabric
> * Wdrażanie aplikacji kontenera na platformie Azure

## <a name="prerequisites"></a>Wymagania wstępne
* Subskrypcja platformy Azure (można utworzyć [bezpłatne konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)).
* Komputer dewelopera z następującym oprogramowaniem:
  * Program Visual Studio 2015 lub Visual Studio 2017.
  * [Zestaw SDK usługi Service Fabric oraz narzędzia](service-fabric-get-started.md).

## <a name="package-a-docker-image-container-with-visual-studio"></a>Pakowanie kontenera obrazu platformy Docker przy użyciu programu Visual Studio
Zestaw SDK usługi Service Fabric oraz narzędzia udostępniają szablon usługi ułatwiający wdrażanie kontenera w klastrze usługi Service Fabric.

Uruchom program Visual Studio jako administrator.  Wybierz kolejno pozycje **Plik** > **Nowy** > **Projekt**.

Wybierz pozycję **Aplikacja usługi Service Fabric**, nadaj jej nazwę „MyFirstContainer” i kliknij przycisk **OK**.

Z listy **szablonów usług** wybierz pozycję **Kontener**.

W polu **Nazwa obrazu** wprowadź wartość „nanoserver/iis” ([podstawowy obraz serwera Nano Server systemu Windows Server 2016 i usług IIS](https://hub.docker.com/r/nanoserver/iis/)). 

Nazwij usługę „MyContainerService”, a następnie kliknij przycisk **OK**.

## <a name="configure-communication-and-container-port-to-host-port-mapping"></a>Konfigurowanie komunikacji i mapowania portów kontenera typu „port do hosta”
Usługa wymaga punktu końcowego na potrzeby komunikacji.  Teraz możesz dodać protokół, port i typ do znacznika `Endpoint` w pliku ServiceManifest.xml. W tym przewodniku Szybki start konteneryzowana usługa nasłuchuje na porcie 80: 

```xml
<Endpoint Name="MyContainerServiceTypeEndpoint" UriScheme="http" Port="80" Protocol="http"/>
```
Jeśli zostanie określony parametr `UriScheme`, punkt końcowy kontenera zostanie automatycznie zarejestrowany w usłudze Service Fabric Naming, aby można go było odnaleźć. Pełny przykładowy plik ServiceManifest.xml znajduje się na końcu tego artykułu. 

Skonfiguruj mapowanie portów kontenera typu port do hosta, określając zasady `PortBinding` w sekcji `ContainerHostPolicies` pliku ApplicationManifest.xml.  W tym przewodniku Szybki start wartością parametru `ContainerPort` jest 80, a wartością parametru `EndpointRef` jest „MyContainerServiceTypeEndpoint” (punkt końcowy zdefiniowany w manifeście usługi).  Żądania przychodzące do usługi na porcie 80 są mapowane na port 80 w kontenerze.  

```xml
<ServiceManifestImport>
...
  <Policies>
    <ContainerHostPolicies CodePackageRef="Code">
      <PortBinding ContainerPort="80" EndpointRef="MyContainerServiceTypeEndpoint"/>
    </ContainerHostPolicies>
  </Policies>
</ServiceManifestImport>
```

Pełny przykładowy plik ApplicationManifest.xml znajduje się na końcu tego artykułu.

## <a name="create-a-cluster"></a>Tworzenie klastra
Aby wdrożyć aplikację w klastrze na platformie Azure, możesz utworzyć własny klaster lub użyć klastra testowego.

Klastry testowe to bezpłatne, ograniczone czasowo klastry usługi Service Fabric hostowane na platformie Azure i uruchamiane przez zespół usługi Service Fabric, w których każdy może wdrażać aplikacje i poznawać platformę. Aby uzyskać dostęp do klastra testowego, [postępuj zgodnie z instrukcjami](http://aka.ms/tryservicefabric).  

Aby uzyskać informacje na temat tworzenia własnego klastra, zobacz [Tworzenie pierwszego klastra usługi Service Fabric na platformie Azure](service-fabric-get-started-azure-cluster.md).

Zanotuj punkt końcowy połączenia, który zostanie użyty w następnym kroku.  

## <a name="deploy-the-application-to-azure-using-visual-studio"></a>Wdrażanie aplikacji na platformie Azure przy użyciu programu Visual Studio
Kiedy aplikacja jest gotowa, można wdrożyć ją w klastrze bezpośrednio z programu Visual Studio.

Kliknij prawym przyciskiem myszy pozycję **MyFirstContainer** w Eksploratorze rozwiązań i wybierz polecenie **Publikuj**. Zostanie wyświetlone okno dialogowe Publikowanie.

![Okno dialogowe Publikowanie](./media/service-fabric-quickstart-dotnet/publish-app.png)

Wpisz punkt końcowy połączenia klastra w polu **Punkt końcowy połączenia**, a następnie kliknij przycisk **Publikuj**. Podczas rejestracji dla klastra testowego punkt końcowy połączenia jest udostępniany w przeglądarce — na przykład `winh1x87d1d.westus.cloudapp.azure.com:19000`.

Otwórz przeglądarkę i przejdź pod adres http://winh1x87d1d.westus.cloudapp.azure.com:80. Powinna zostać wyświetlona domyślna strona internetowa usługi IIS: ![domyślna strona internetowa usług IIS][iis-default]

## <a name="complete-example-service-fabric-application-and-service-manifests"></a>Kompletny przykład aplikacji i manifestów usługi Service Fabric
Zamieszczono tutaj kompletne manifesty usługi i aplikacji używane w tym przewodniku Szybki start.

### <a name="servicemanifestxml"></a>ServiceManifest.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest Name="MyContainerServicePkg"
                 Version="1.0.0"
                 xmlns="http://schemas.microsoft.com/2011/01/fabric"
                 xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <ServiceTypes>
    <!-- This is the name of your ServiceType.
         The UseImplicitHost attribute indicates this is a guest service. -->
    <StatelessServiceType ServiceTypeName="MyContainerServiceType" UseImplicitHost="true" />
  </ServiceTypes>

  <!-- Code package is your service executable. -->
  <CodePackage Name="Code" Version="1.0.0">
    <EntryPoint>
      <!-- Follow this link for more information about deploying Windows containers to Service Fabric: https://aka.ms/sfguestcontainers -->
      <ContainerHost>
        <ImageName>nanoserver/iis</ImageName>
      </ContainerHost>
    </EntryPoint>
    <!-- Pass environment variables to your container: -->
    <!--
    <EnvironmentVariables>
      <EnvironmentVariable Name="VariableName" Value="VariableValue"/>
    </EnvironmentVariables>
    -->
  </CodePackage>

  <!-- Config package is the contents of the Config directoy under PackageRoot that contains an 
       independently-updateable and versioned set of custom configuration settings for your service. -->
  <ConfigPackage Name="Config" Version="1.0.0" />

  <Resources>
    <Endpoints>
      <!-- This endpoint is used by the communication listener to obtain the port on which to 
           listen. Please note that if your service is partitioned, this port is shared with 
           replicas of different partitions that are placed in your code. -->
      <Endpoint Name="MyContainerServiceTypeEndpoint" UriScheme="http" Port="80" Protocol="http"/>
    </Endpoints>
  </Resources>
</ServiceManifest>
```
### <a name="applicationmanifestxml"></a>ApplicationManifest.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest ApplicationTypeName="MyFirstContainerType"
                     ApplicationTypeVersion="1.0.0"
                     xmlns="http://schemas.microsoft.com/2011/01/fabric"
                     xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                     xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <Parameters>
    <Parameter Name="MyContainerService_InstanceCount" DefaultValue="-1" />
  </Parameters>
  <!-- Import the ServiceManifest from the ServicePackage. The ServiceManifestName and ServiceManifestVersion 
       should match the Name and Version attributes of the ServiceManifest element defined in the 
       ServiceManifest.xml file. -->
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="MyContainerServicePkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides />
    <Policies>
      <ContainerHostPolicies CodePackageRef="Code">
        <PortBinding ContainerPort="80" EndpointRef="MyContainerServiceTypeEndpoint"/>
      </ContainerHostPolicies>
    </Policies>
  </ServiceManifestImport>
  <DefaultServices>
    <!-- The section below creates instances of service types, when an instance of this 
         application type is created. You can also create one or more instances of service type using the 
         ServiceFabric PowerShell module.
         
         The attribute ServiceTypeName below must match the name defined in the imported ServiceManifest.xml file. -->
    <Service Name="MyContainerService" ServicePackageActivationMode="ExclusiveProcess">
      <StatelessService ServiceTypeName="MyContainerServiceType" InstanceCount="[MyContainerService_InstanceCount]">
        <SingletonPartition />
      </StatelessService>
    </Service>
  </DefaultServices>
</ApplicationManifest>
```

## <a name="next-steps"></a>Następne kroki
W tym przewodniku Szybki start zawarto informacje na temat wykonywania następujących czynności:
> [!div class="checklist"]
> * Pakowanie kontenera obrazu platformy Docker
> * Konfigurowanie komunikacji
> * Kompilowanie i tworzenie pakietu aplikacji usługi Service Fabric
> * Wdrażanie aplikacji kontenera na platformie Azure

* Dowiedz się więcej o uruchamianiu [kontenerów w usłudze Service Fabric](service-fabric-containers-overview.md).
* Zapoznaj się z samouczkiem [Deploy a .NET application in a container](service-fabric-host-app-in-a-container.md) (Wdrażanie aplikacji .NET w kontenerze).
* Uzyskaj informacje o [cyklu życia aplikacji](service-fabric-application-lifecycle.md) usługi Service Fabric.
* Sprawdź [przykłady kodu kontenera usługi Service Fabric](https://github.com/Azure-Samples/service-fabric-dotnet-containers) w witrynie GitHub.

[iis-default]: ./media/service-fabric-quickstart-containers/iis-default.png
[publish-dialog]: ./media/service-fabric-quickstart-containers/publish-dialog.png

