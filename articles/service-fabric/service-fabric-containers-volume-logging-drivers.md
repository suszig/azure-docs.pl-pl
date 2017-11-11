---
title: "Usługa Azure sieci szkieletowej z rozwiązania Docker Compose w wersji zapoznawczej | Dokumentacja firmy Microsoft"
description: "Sieć szkieletowa usług Azure akceptuje rozwiązania Docker Compose format, aby ułatwić organizowanie kontenery exsiting przy użyciu sieci szkieletowej usług. Ta obsługa jest obecnie w przeglądzie."
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: ab49c4b9-74a8-4907-b75b-8d2ee84c6d90
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: subramar
ms.openlocfilehash: 955f84e5656bbf568234cbaf69faa4dd0a741206
ms.sourcegitcommit: 6a22af82b88674cd029387f6cedf0fb9f8830afd
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/11/2017
---
# <a name="using-volume-plugins-and-logging-drivers-in-your-container"></a>Przy użyciu woluminu wtyczek i sterowników rejestrowania w kontenerze sieci
Sieć szkieletowa usług obsługuje określanie [wtyczek woluminu Docker](https://docs.docker.com/engine/extend/plugins_volume/) i [sterowniki rejestrowania Docker](https://docs.docker.com/engine/admin/logging/overview/) dla usługi kontenera.  Pozwoli to do utrwalenia danych w [plików Azure](https://azure.microsoft.com/en-us/services/storage/files/) nawet wtedy, gdy użytkownik kontenera zostanie przeniesiona lub uruchomiona ponownie na innym hoście.

Obecnie są tylko sterowniki wolumin kontenerów Linux, jak pokazano poniżej.  Jeśli używasz systemu Windows kontenerów, istnieje możliwość mapowania woluminu do plików Azure [udziału protokołu SMB3](https://blogs.msdn.microsoft.com/clustering/2017/08/10/container-storage-support-with-cluster-shared-volumes-csv-storage-spaces-direct-s2d-smb-global-mapping/) bez sterownika woluminu przy użyciu najnowszej wersji 1709 systemu Windows Server. To wymaga aktualizacji do wersji Windows Server 1709 maszyn wirtualnych w klastrze.


## <a name="install-volumelogging-driver"></a>Instalacja sterownika wolumin/rejestrowania

Jeśli sterownik wolumin/rejestrowania Docker nie jest zainstalowany na komputerze, zainstalować go ręcznie za pośrednictwem protokołu RDP/SSH-ne do komputera, za pomocą [VMSS uruchamiania skryptu](https://azure.microsoft.com/en-us/resources/templates/201-vmss-custom-script-windows/) lub przy użyciu [SetupEntryPoint](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-application-model#describe-a-service) skryptu. Wybranie jednej z metod, o których wspomniano, można napisać skrypt, aby zainstalować [sterownik woluminu Docker na platformie Azure](https://docs.docker.com/docker-for-azure/persistent-data-volumes/):


```bash
docker plugin install --alias azure --grant-all-permissions docker4x/cloudstor:17.09.0-ce-azure1  \
    CLOUD_PLATFORM=AZURE \
    AZURE_STORAGE_ACCOUNT="[MY-STORAGE-ACCOUNT-NAME]" \
    AZURE_STORAGE_ACCOUNT_KEY="[MY-STORAGE-ACCOUNT-KEY]" \
    DEBUG=1
```

## <a name="specify-the-plugin-or-driver-in-the-manifest"></a>Określ wtyczka lub sterownik w manifeście
Wtyczki są określone w manifeście aplikacji, jak pokazano w manifeście następujące:

```xml
?xml version="1.0" encoding="UTF-8"?>
<ApplicationManifest ApplicationTypeName="WinNodeJsApp" ApplicationTypeVersion="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
    <Description>Calculator Application</Description>
    <Parameters>
        <Parameter Name="ServiceInstanceCount" DefaultValue="3"></Parameter>
      <Parameter Name="MyCpuShares" DefaultValue="3"></Parameter>
    </Parameters>
    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="NodeServicePackage" ServiceManifestVersion="1.0"/>
     <Policies>
       <ContainerHostPolicies CodePackageRef="NodeService.Code" Isolation="hyperv"> 
        <PortBinding ContainerPort="8905" EndpointRef="Endpoint1"/>
        <RepositoryCredentials PasswordEncrypted="false" Password="****" AccountName="test"/>
        <LogConfig Driver="etwlogs" >
          <DriverOption Name="test" Value="vale"/>
        </LogConfig>
        <Volume Source="c:\workspace" Destination="c:\testmountlocation1" IsReadOnly="false"></Volume>
        <Volume Source="d:\myfolder" Destination="c:\testmountlocation2" IsReadOnly="true"> </Volume>
        <Volume Source="myvolume1" Destination="c:\testmountlocation2" Driver="azure" IsReadOnly="true">
           <DriverOption Name="share" Value="models"/>
        </Volume>
       </ContainerHostPolicies>
   </Policies>
    </ServiceManifestImport>
    <ServiceTemplates>
        <StatelessService ServiceTypeName="StatelessNodeService" InstanceCount="5">
            <SingletonPartition></SingletonPartition>
        </StatelessService>
    </ServiceTemplates>
</ApplicationManifest>
```

W powyższym przykładzie `Source` znacznika `Volume` odwołuje się do folderu źródłowego. Folder źródłowy może być folderem w maszynie Wirtualnej, która obsługuje kontenery lub trwałego magazynu zdalnego. `Destination` Tag to lokalizacja, w który `Source` jest mapowany w kontenerze uruchomione.  W związku z tym folderu docelowego nie może być już istniejącej lokalizacji z kontenera.

Podczas określania wtyczki woluminu, Service Fabric automatycznie tworzy woluminu przy użyciu określonych parametrów. `Source` Tag jest nazwa woluminu, a `Driver` tag określa dodatek sterownika woluminu. Opcje można określić za pomocą `DriverOption` tagów, jak pokazano w poniższy fragment kodu:

```xml
<Volume Source="myvolume1" Destination="c:\testmountlocation4" Driver="azure" IsReadOnly="true">
           <DriverOption Name="share" Value="models"/>
</Volume>
```
Jeśli sterownik dziennika Docker jest określony, jest wymagane do wdrożenia agentów (lub kontenery) do obsługi dzienników w klastrze.  `DriverOption` Tag może służyć do określ także opcje sterownika dziennika.

Zobacz następujące artykuły, aby wdrażanie kontenerów do klastra usługi sieć szkieletowa usług:


[Wdrażanie kontenera w sieci szkieletowej usług](service-fabric-deploy-container.md)

