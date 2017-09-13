---
title: "Tworzenie aplikacji kontenera usługi Azure Service Fabric w systemie Linux | Dokumentacja firmy Microsoft"
description: "Utwórz swoją pierwszą aplikację kontenera systemu Linux w usłudze Azure Service Fabric.  Zbuduj obraz Docker za pomocą własnej aplikacji, wypchnij obraz do rejestru kontenerów, skompiluj i wdróż aplikację kontenera usługi Service Fabric."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
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
ms.openlocfilehash: 78306672e812745fd1902ae264c2adea196ab721
ms.contentlocale: pl-pl
ms.lasthandoff: 09/07/2017

---

# <a name="deploy-a-service-fabric-linux-container-application-on-azure"></a>Tworzenie aplikacji kontenera systemu Linux w usłudze Service Fabric na platformie Azure
Usługa Azure Service Fabric to platforma systemów rozproszonych ułatwiająca pakowanie i wdrażanie skalowalnych oraz niezawodnych mikrousług i kontenerów, a także zarządzanie nimi. 

Uruchomienie istniejącej aplikacji w kontenerze systemu Linux w klastrze usługi Service Fabric nie wymaga dokonywania żadnych zmian w aplikacji. W tym przewodniku Szybki start przedstawiono sposób wdrażania wstępnie skompilowanego obrazu kontenera platformy Docker w aplikacji usługi Service Fabric. Po zakończeniu będziesz mieć działający kontener nginx.  W tym przewodniku Szybki start opisano wdrażanie kontenera systemu Linux. Aby wdrożyć kontener systemu Windows, zapoznaj się z [tym przewodnikiem Szybki start](service-fabric-quickstart-containers.md).

![Nginx][nginx]

W tym przewodniku Szybki start zawarto informacje na temat wykonywania następujących czynności:
> [!div class="checklist"]
> * Pakowanie kontenera obrazu platformy Docker
> * Konfigurowanie komunikacji
> * Kompilowanie i tworzenie pakietu aplikacji usługi Service Fabric
> * Wdrażanie aplikacji kontenera na platformie Azure

## <a name="prerequisites"></a>Wymagania wstępne
Zainstaluj [zestaw SDK usługi Service Fabric, interfejs wiersza polecenia usługi Service Fabric i generatory szablonów narzędzia Yeoman usługi Service Fabric](service-fabric-get-started-linux.md).
  
## <a name="package-a-docker-image-container-with-yeoman"></a>Pakowanie kontenera obrazu platformy Docker przy użyciu narzędzia Yeoman
Zestaw SDK usługi Service Fabric dla systemu Linux zawiera generator [Yeoman](http://yeoman.io/), który ułatwia tworzenie własnej aplikacji i dodawanie obrazu kontenera. 

Aby utworzyć aplikację kontenera usługi Service Fabric, otwórz okno terminalu i uruchom polecenie `yo azuresfcontainer`.  

Nazwij aplikację „MyFirstContainer” i nazwij usługę aplikacji „MyContainerService”.

Podaj nazwę obrazu kontenera „nginx:latest” ([obraz kontenera nginx](https://hub.docker.com/r/_/nginx/) w usłudze Docker Hub). 

Ten obraz ma zdefiniowany punkt wejścia obciążenia, więc należy jawnie określić polecenia wejściowe. 

Określ liczbę wystąpień jako „1”.

![Generator Yeoman usługi Service Fabric dla kontenerów][sf-yeoman]

## <a name="configure-communication-and-container-port-to-host-port-mapping"></a>Konfigurowanie komunikacji i mapowania portów kontenera typu „port do hosta”
Skonfiguruj punkt końcowy HTTP, aby klienci mogli komunikować się z usługą.  Otwórz plik *./MyFirstContainer/MyContainerServicePkg/ServiceManifest.xml* i zadeklaruj zasób punktu końcowego w elemencie **ServiceManifest**.  Dodaj protokół, port i nazwę. W tym przewodniku Szybki start usługa nasłuchuje na porcie 80: 

```xml
<Resources>
  <Endpoints>
    <!-- This endpoint is used by the communication listener to obtain the port on which to 
           listen. Please note that if your service is partitioned, this port is shared with 
           replicas of different partitions that are placed in your code. -->
    <Endpoint Name="myserviceTypeEndpoint" UriScheme="http" Port="80" Protocol="http"/>
  </Endpoints>
</Resources>

```
Jeśli zostanie określony parametr `UriScheme`, punkt końcowy kontenera zostanie automatycznie zarejestrowany w usłudze Service Fabric Naming, aby można go było odnaleźć. Pełny przykładowy plik ServiceManifest.xml znajduje się na końcu tego artykułu. 

Zamapuj port kontenera na element `Endpoint` usługi przy użyciu zasad `PortBinding` w elemencie `ContainerHostPolicies` w pliku ApplicationManifest.xml.  W tym przewodniku Szybki start wartość parametru `ContainerPort` to 80 (kontener uwidacznia port 80), a wartością parametru `EndpointRef` jest „myserviceTypeEndpoint” (punkt końcowy wcześniej zdefiniowany w manifeście usługi).  Żądania przychodzące do usługi na porcie 80 są mapowane na port 80 w kontenerze.  

```xml
<Policies>
  <ContainerHostPolicies CodePackageRef="Code">
    <PortBinding ContainerPort="80" EndpointRef="myserviceTypeEndpoint"/>
  </ContainerHostPolicies>
</Policies>
```

## <a name="build-and-package-the-service-fabric-application"></a>Kompilowanie i tworzenie pakietu aplikacji usługi Service Fabric
Szablony generatora Yeoman usługi Service Fabric obejmują skrypt kompilacji dla narzędzia [Gradle](https://gradle.org/), którego można użyć do skompilowania aplikacji z poziomu terminalu. Zapisz wszystkie zmiany.  Aby skompilować aplikację i utworzyć jej pakiet, uruchom następujące polecenie:

```bash
cd MyFirstContainer
gradle
```
## <a name="create-a-cluster"></a>Tworzenie klastra
Aby wdrożyć aplikację w klastrze na platformie Azure, możesz utworzyć własny klaster lub użyć klastra testowego.

Klastry testowe to bezpłatne, ograniczone czasowo klastry usługi Service Fabric hostowane na platformie Azure i uruchamiane przez zespół usługi Service Fabric, w których każdy może wdrażać aplikacje i poznawać platformę. Aby uzyskać dostęp do klastra testowego, [postępuj zgodnie z instrukcjami](http://aka.ms/tryservicefabric).  

Aby uzyskać informacje na temat tworzenia własnego klastra, zobacz [Tworzenie pierwszego klastra usługi Service Fabric na platformie Azure](service-fabric-get-started-azure-cluster.md).

Zanotuj punkt końcowy połączenia, który zostanie użyty w następnym kroku.

## <a name="deploy-the-application-to-azure"></a>Wdrażanie aplikacji na platformie Azure
Po skompilowaniu aplikację można wdrożyć w klastrze platformy Azure za pomocą interfejsu wiersza polecenia usługi Service Fabric.

Połącz się z klastrem usługi Service Fabric na platformie Azure.

```bash
sfctl cluster select --endpoint http://lnxt10vkfz6.westus.cloudapp.azure.com:19080
```

Użyj skryptu instalacji udostępnionego w szablonie, aby skopiować pakiet aplikacji do magazynu obrazów klastra, zarejestrować typ aplikacji i utworzyć wystąpienie aplikacji.

```bash
./install.sh
```

Otwórz przeglądarkę i przejdź do narzędzia Service Fabric Explorer pod adresem http://lnxt10vkfz6.westus.cloudapp.azure.com:19080/Explorer. Rozwiń węzeł Aplikacje i zwróć uwagę, że istnieje teraz wpis dla danego typu aplikacji i inny wpis dla pierwszego wystąpienia tego typu.

![Service Fabric Explorer][sfx]

Nawiąż połączenie z działającym kontenerem.  Otwórz przeglądarkę internetową i wpisz adres IP zwrócony na porcie 80, na przykład „lnxt10vkfz6.westus.cloudapp.azure.com:80 ”. W przeglądarce powinna pojawić się strona powitalna serwera nginx.

![Nginx][nginx]

## <a name="clean-up"></a>Czyszczenie
Użyj skryptu dezinstalacji udostępnionego w szablonie, aby usunąć wystąpienie aplikacji z klastra i wyrejestrować typ aplikacji.

```bash
./uninstall.sh
```

## <a name="complete-example-service-fabric-application-and-service-manifests"></a>Kompletny przykład aplikacji i manifestów usługi Service Fabric
Zamieszczono tutaj kompletne manifesty usługi i aplikacji używane w tym przewodniku Szybki start.

### <a name="servicemanifestxml"></a>ServiceManifest.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest Name="MyContainerServicePkg" Version="1.0.0"
                 xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" >

   <ServiceTypes>
      <StatelessServiceType ServiceTypeName="MyContainerServiceType" UseImplicitHost="true">
   </StatelessServiceType>
   </ServiceTypes>
   
   <CodePackage Name="code" Version="1.0.0">
      <EntryPoint>
         <ContainerHost>
            <ImageName>nginx:latest</ImageName>
            <Commands></Commands>
         </ContainerHost>
      </EntryPoint>
      <EnvironmentVariables> 
      </EnvironmentVariables> 
   </CodePackage>
<Resources>
    <Endpoints>
      <!-- This endpoint is used by the communication listener to obtain the port on which to 
           listen. Please note that if your service is partitioned, this port is shared with 
           replicas of different partitions that are placed in your code. -->
      <Endpoint Name="myserviceTypeEndpoint" UriScheme="http" Port="80" Protocol="http"/>
    </Endpoints>
  </Resources>
 </ServiceManifest>

```
### <a name="applicationmanifestxml"></a>ApplicationManifest.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest  ApplicationTypeName="MyFirstContainerType" ApplicationTypeVersion="1.0.0"
                      xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
   
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="MyContainerServicePkg" ServiceManifestVersion="1.0.0" />
   <Policies>
      <ContainerHostPolicies CodePackageRef="Code">
        <PortBinding ContainerPort="80" EndpointRef="myserviceTypeEndpoint"/>
      </ContainerHostPolicies>
    </Policies>
</ServiceManifestImport>
   
   <DefaultServices>
      <Service Name="MyContainerService">
        <!-- On a local development cluster, set InstanceCount to 1.  On a multi-node production 
        cluster, set InstanceCount to -1 for the container service to run on every node in 
        the cluster.
        -->
        <StatelessService ServiceTypeName="MyContainerServiceType" InstanceCount="1">
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

[sfx]: ./media/service-fabric-quickstart-containers-linux/SFX.png
[nginx]: ./media/service-fabric-quickstart-containers-linux/nginx.png
[sf-yeoman]: ./media/service-fabric-quickstart-containers-linux/YoSF.png

