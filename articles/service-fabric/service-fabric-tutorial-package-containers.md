---
title: "Pakiet, a następnie wdrożyć aplikację kontenery sieci szkieletowej usług | Dokumentacja firmy Microsoft"
description: "Informacje o sposobie generowania definicji aplikacji usługi Azure Service Fabric przy użyciu narzędzia Yeoman i pakietu aplikacji."
services: service-fabric
documentationcenter: 
author: suhuruli
manager: timlt
editor: suhuruli
tags: servicefabric
keywords: "Rozwiązanie docker kontenerów, Mikrousług, sieć szkieletowa usług Azure"
ms.assetid: 
ms.service: service-fabric
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/12/2017
ms.author: suhuruli
ms.custom: mvc
ms.openlocfilehash: 0631b621c01eb880393d07323cdeb815e564a2e3
ms.sourcegitcommit: f67f0bda9a7bb0b67e9706c0eb78c71ed745ed1d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/20/2017
---
# <a name="package-and-deploy-containers-as-a-service-fabric-application"></a>Pakiet i wdróż kontenery jako aplikacji sieci szkieletowej usług

W tym samouczku wchodzi w skład dwóch w serii. W tym samouczku narzędzie generowania szablonu (narzędzia Yeoman) służy do generowania definicji aplikacji sieci szkieletowej usług. Ta aplikacja może następnie służyć do wdrażanie kontenerów do sieci szkieletowej usług. Ten samouczek zawiera informacje na temat wykonywania następujących czynności: 

> [!div class="checklist"]
> * Zainstaluj narzędzia Yeoman  
> * Utwórz pakiet aplikacji za pomocą narzędzia Yeoman
> * Skonfiguruj ustawienia w pakiecie aplikacji do użycia z kontenerami
> * Kompilowanie aplikacji  
> * Wdrażanie i uruchamianie aplikacji 
> * Oczyszczanie aplikacji

## <a name="prerequisites"></a>Wymagania wstępne

- Obrazy kontenera wypychana utworzone w rejestrze kontenera Azure [część 1](service-fabric-tutorial-create-container-images.md) tego samouczka serii są używane.
- Środowisko projektowe Linux jest [Konfigurowanie](service-fabric-tutorial-create-container-images.md).

## <a name="install-yeoman"></a>Zainstaluj narzędzia Yeoman
Usługa Service fabric realizuje szkieletów narzędzi do tworzenia aplikacji z terminalu przy użyciu narzędzia Yeoman generatora szablonu. Wykonaj następujące kroki, aby zapewnić narzędzia Yeoman generatora szablonu. 

1. Na tym komputerze można zainstalować nodejs i NPM. Należy pamiętać, że systemu Mac OS x, użytkownicy będą musieli używać Menedżera pakietów oprogramowania Homebrew

    ```bash
    sudo apt-get install npm && sudo apt install nodejs-legacy
    ```
2. Zainstaluj narzędzia Yeoman generatora szablonu na komputerze z pakietu NPM 

    ```bash
    sudo npm install -g yo
    ```
3. Zainstaluj narzędzia usługi sieć szkieletowa Yeoman generator kontenera

    ```bash 
    sudo npm install -g generator-azuresfcontainer
    ```

## <a name="package-a-docker-image-container-with-yeoman"></a>Pakowanie kontenera obrazu platformy Docker przy użyciu narzędzia Yeoman

1. Aby utworzyć kontener sieci szkieletowej usług aplikacji, w katalogu "kontener — samouczek" sklonowanego repozytorium, uruchom następujące polecenie.

    ```bash
    yo azuresfcontainer
    ```
2. Nazwa aplikacji "Obiektu TestContainer" i nazwę aplikacji usługi "azurevotefront".
3. Podaj ścieżkę obrazu kontenera w ACR dla repozytorium frontonu — na przykład "test.azurecr.io/azure-vote-front:v1". 
4. Naciśnij klawisz Enter, aby pozostawić polecenia sekcji puste.
5. Określ liczbę wystąpień 1.

Poniżej pokazano dane wejściowe i wyjściowe uruchamiania polecenia yo:

```bash
? Name your application TestContainer
? Name of the application service: azurevotefront
? Input the Image Name: <acrName>.azurecr.io/azure-vote-front:v1
? Commands: 
? Number of instances of guest container application: 1
   create TestContainer/TestContainer/ApplicationManifest.xml
   create TestContainer/TestContainer/azurevotefrontPkg/ServiceManifest.xml
   create TestContainer/TestContainer/azurevotefrontPkg/config/Settings.xml
   create TestContainer/TestContainer/azurevotefrontPkg/code/Dummy.txt
   create TestContainer/install.sh
   create TestContainer/uninstall.sh
```

Aby dodać kolejną usługę kontenera do aplikacji utworzonej już przy użyciu narzędzia Yeoman, wykonaj następujące czynności:

1. Zmień katalog na **obiektu TestContainer** katalogu
2. Uruchom polecenie `yo azuresfcontainer:AddService` 
3. Nazwa usługi azurevoteback
4. Podaj ścieżkę do obrazu kontenera w ACR dla repozytorium wewnętrznej bazy danych — na przykład "test.azurecr.io/azure-vote-back:v1"
5. Naciśnij klawisz Enter, aby pozostawić polecenia sekcji pusty
6. Określ liczbę wystąpień jako „1”.

Wpisy dodanie tej usługi, używane są podane:

```bash
? Name of the application service: azurevoteback
? Input the Image Name: <acrName>.azurecr.io/azure-vote-back:v1
? Commands: 
? Number of instances of guest container application: 1
   create TestContainer/azurevotebackPkg/ServiceManifest.xml
   create TestContainer/azurevotebackPkg/config/Settings.xml
   create TestContainer/azurevotebackPkg/code/Dummy.txt
```

W pozostałej części tego samouczka, firma Microsoft pracuje **obiektu TestContainer** katalogu.

## <a name="configure-the-application-manifest-with-credentials-for-azure-container-registry"></a>Skonfiguruj manifest aplikacji przy użyciu poświadczeń dla rejestru kontenera platformy Azure
Dla sieci szkieletowej usług do pobierania kontenera obrazów z rejestru kontenera platformy Azure, należy podać poświadczenia w **ApplicationManifest.xml**. 


Zaloguj się do swojego wystąpienia ACR. Użyj [logowania acr az](/cli/azure/acr#az_acr_login) polecenia do wykonania operacji. Podaj unikatową nazwę podane w rejestrze kontenera podczas jej tworzenia.

```bash
az acr login --name <acrName>
```

Polecenie zwraca **logowanie zakończyło się pomyślnie** komunikat po ukończeniu.

Następnie uruchom następujące polecenie, aby pobrać hasło rejestru kontenera. To hasło jest używane przez sieci szkieletowej usług uwierzytelniania za pomocą ACR ściągania obrazy kontenera.

```bash
az acr credential show -n <acrName> --query passwords[0].value
```

W **ApplicationManifest.xml**, dodać fragment kodu w obszarze **ServiceManifestImport** elementu dla każdego z nich. Wstaw z **acrName** dla **AccountName** pola i hasło zwrócone przez poprzednie polecenie służy do **hasło** pola. Pełne **ApplicationManifest.xml** znajduje się na końcu niniejszego dokumentu. 

```xml
<Policies>
  <ContainerHostPolicies CodePackageRef="Code">
    <RepositoryCredentials AccountName="<acrName>" Password="<password>" PasswordEncrypted="false"/>
  </ContainerHostPolicies>
</Policies>
```
## <a name="configure-communication-and-container-port-to-host-port-mapping"></a>Konfigurowanie komunikacji i mapowania portów kontenera typu „port do hosta”

### <a name="configure-communication-port"></a>Konfigurowanie portów komunikacji

Skonfiguruj punkt końcowy HTTP, aby klienci mogli komunikować się z usługą.  Otwórz *./TestContainer/azurevotefrontPkg/ServiceManifest.xml* plików ani deklarować zasobu punktu końcowego w **ServiceManifest** elementu.  Dodaj protokół, port i nazwę. W tym samouczku usługa nasłuchuje na porcie 80. 
  
```xml
<Resources>
  <Endpoints>
    <!-- This endpoint is used by the communication listener to obtain the port on which to 
            listen. Please note that if your service is partitioned, this port is shared with 
            replicas of different partitions that are placed in your code. -->
    <Endpoint Name="azurevotefrontTypeEndpoint" UriScheme="http" Port="80" Protocol="http"/>
  </Endpoints>
</Resources>

```
  
Podobnie zmodyfikuj Manifest usługi dla usługi wewnętrznej bazy danych. W tym samouczku domyślną redis 6379 jest obsługiwana.
```xml
<Resources>
  <Endpoints>
    <!-- This endpoint is used by the communication listener to obtain the port on which to 
            listen. Please note that if your service is partitioned, this port is shared with 
            replicas of different partitions that are placed in your code. -->
    <Endpoint Name="azurevotebackTypeEndpoint" UriScheme="http" Port="6379" Protocol="http"/>
  </Endpoints>
</Resources>
```
Zapewnianie **UriScheme**automatycznie rejestruje punkt końcowy kontenera z usługą nazewnictwa sieci szkieletowej usług w celu odnajdywania. Pełna ServiceManifest.xml przykładowy plik usługi wewnętrznej bazy danych znajduje się na końcu tego artykułu jako przykład. 

### <a name="map-container-ports-to-a-service"></a>Porty kontenera mapy do usługi
    
Aby udostępnić kontenery w klastrze, również należy utworzyć powiązanie portu w ApplicationManifest.xml. **PortBinding** odwołania zasad **punkty końcowe** zdefiniowanego w **ServiceManifest.xml** plików. Przychodzące żądania z tymi punktami końcowymi uzyskać mapowane na porty kontenera, które są otwarte i ograniczone tutaj. W **ApplicationManifest.xml** pliku, Dodaj następujący kod, aby powiązać porty 80 i 6379 do punktów końcowych. Pełne **ApplicationManifest.xml** znajduje się na końcu tego dokumentu. 
  
```xml
<ContainerHostPolicies CodePackageRef="Code">
    <PortBinding ContainerPort="80" EndpointRef="azurevotefrontTypeEndpoint"/>
</ContainerHostPolicies>
```

```xml
<ContainerHostPolicies CodePackageRef="Code">
    <PortBinding ContainerPort="6379" EndpointRef="azurevotebackTypeEndpoint"/>
</ContainerHostPolicies>
```

### <a name="add-a-dns-name-to-the-backend-service"></a>Dodaj nazwę DNS do usługi zaplecza
  
Dla sieci szkieletowej usług można przypisać tę nazwę DNS do usługi zaplecza, nazwa musi być określone w **ApplicationManifest.xml**. Dodaj **ServiceDnsName** atrybutu **usługi** element, jak pokazano: 
  
```xml
<Service Name="azurevoteback" ServiceDnsName="redisbackend.testapp">
  <StatelessService ServiceTypeName="azurevotebackType" InstanceCount="1">
    <SingletonPartition/>
  </StatelessService>
</Service>
```

Usługa frontonu odczytuje wartość zmiennej środowiskowej znać nazwę DNS wystąpienia pamięci podręcznej Redis. Zmienna środowiskowa jest zdefiniowana w plik Dockerfile, jak pokazano:
  
```Dockerfile
ENV REDIS redisbackend.testapp
```
  
Skrypt języka python, który renderuje zastosowań front end nazwa DNS tego rozwiązania i podłącz do magazynu wewnętrznej bazy danych redis, jak pokazano:

```python
# Get DNS Name
redis_server = os.environ['REDIS'] 

# Connect to the Redis store
r = redis.StrictRedis(host=redis_server, port=6379, db=0)
```

W tym momencie w samouczku szablonu dla usługi pakietu aplikacji jest dostępny do wdrożenia do klastra. Kolejne kroki samouczka ta aplikacja jest wdrażana i został uruchomiony w klastrze usługi sieć szkieletowa usług.

## <a name="create-a-service-fabric-cluster"></a>Tworzenie klastra sieci szkieletowej usług
Aby wdrożyć aplikację w klastrze na platformie Azure, użyj klastra własnego lub klastra testowego.

Klastry testowe to bezpłatne, działające przez ograniczony czas klastry usługi Service Fabric hostowane na platformie Azure. Jest obsługiwana przez zespół usługi sieć szkieletowa, gdzie każda osoba, która wdrażania aplikacji i Dowiedz się więcej o platformie. Aby uzyskać dostęp do klastra testowego, [postępuj zgodnie z instrukcjami](http://aka.ms/tryservicefabric). 

Aby uzyskać informacje o tworzeniu własnego klastra, zobacz [tworzenia klastra usługi sieć szkieletowa usług Azure](service-fabric-tutorial-create-vnet-and-linux-cluster.md).

## <a name="build-and-deploy-the-application-to-the-cluster"></a>Tworzenie i wdrażanie aplikacji do klastra
Możesz wdrożyć aplikację w klastrze platformy Azure przy użyciu interfejsu wiersza polecenia usługi sieci szkieletowej. Jeśli na tym komputerze nie zainstalowano usługi sieci szkieletowej interfejsu wiersza polecenia, wykonaj instrukcje [tutaj](service-fabric-get-started-linux.md#set-up-the-service-fabric-cli) go zainstalować. 

Połącz się z klastrem usługi Service Fabric na platformie Azure.

```bash
sfctl cluster select --endpoint http://lin4hjim3l4.westus.cloudapp.azure.com:19080
```

Użyj skryptu instalacji w **obiektu TestContainer** katalogu, aby skopiować pakiet aplikacji do magazynu obrazu klastra, Rejestracja typu aplikacji i Utwórz wystąpienie aplikacji.

```bash
./install.sh
```

Otwórz przeglądarkę i przejdź do Eksploratora usługi sieć szkieletowa w http://lin4hjim3l4.westus.cloudapp.azure.com:19080/Explorer. Rozwiń węzeł aplikacji i należy pamiętać, że wpis dla danego typu aplikacji i drugi dla tego wystąpienia.

![Service Fabric Explorer][sfx]

Aby można było nawiązać działającej aplikacji, otwórz przeglądarkę sieci web i przejdź do adresu url klastra — na przykład http://lin0823ryf2he.cloudapp.azure.com:80. Powinna zostać wyświetlona aplikacja głosowania w interfejsie użytkownika sieci web.

![votingapp][votingapp]

## <a name="clean-up"></a>Czyszczenie
Użyj skryptu dezinstalacji udostępnionego w szablonie, aby usunąć wystąpienie aplikacji z klastra i wyrejestrować typ aplikacji. To polecenie wymaga pewnego czasu, aby wyczyścić wystąpienie i nie można uruchomić polecenia "install.sh" natychmiast po ten skrypt. 

```bash
./uninstall.sh
```

## <a name="examples-of-completed-manifests"></a>Przykłady manifestów ukończone

### <a name="applicationmanifestxml"></a>ApplicationManifest.xml
```xml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<ApplicationManifest ApplicationTypeName="TestContainerType" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="azurevotefrontPkg" ServiceManifestVersion="1.0.0"/>
    <Policies> 
    <ContainerHostPolicies CodePackageRef="Code">
        <RepositoryCredentials AccountName="myaccountname" Password="<password>" PasswordEncrypted="false"/>
        <PortBinding ContainerPort="80" EndpointRef="azurevotefrontTypeEndpoint"/>
    </ContainerHostPolicies>
        </Policies>
  </ServiceManifestImport>
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="azurevotebackPkg" ServiceManifestVersion="1.0.0"/>
      <Policies> 
        <ContainerHostPolicies CodePackageRef="Code">
          <RepositoryCredentials AccountName="myaccountname" Password="<password>" PasswordEncrypted="false"/>
          <PortBinding ContainerPort="6379" EndpointRef="azurevotebackTypeEndpoint"/>
        </ContainerHostPolicies>
      </Policies>
  </ServiceManifestImport>
  <DefaultServices>
    <Service Name="azurevotefront">
      <StatelessService ServiceTypeName="azurevotefrontType" InstanceCount="1">
        <SingletonPartition/>
      </StatelessService>
    </Service>
    <Service Name="azurevoteback" ServiceDnsName="redisbackend.testapp">
      <StatelessService ServiceTypeName="azurevotebackType" InstanceCount="1">
        <SingletonPartition/>
      </StatelessService>
    </Service>
  </DefaultServices>
</ApplicationManifest>
```

### <a name="front-end-servicemanifestxml"></a>ServiceManifest.xml frontonu 
```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest Name="azurevotefrontPkg" Version="1.0.0"
                 xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" >

   <ServiceTypes>
      <StatelessServiceType ServiceTypeName="azurevotefrontType" UseImplicitHost="true">
   </StatelessServiceType>
   </ServiceTypes>
   
   <CodePackage Name="code" Version="1.0.0">
      <EntryPoint>
         <ContainerHost>
            <ImageName>my.azurecr.io/azure-vote-front:v1</ImageName>
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
      <Endpoint Name="azurevotefrontTypeEndpoint" UriScheme="http" Port="8080" Protocol="http"/>
    </Endpoints>
  </Resources>

 </ServiceManifest>
```

### <a name="redis-servicemanifestxml"></a>Redis ServiceManifest.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest Name="azurevotebackPkg" Version="1.0.0"
                 xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" >

   <ServiceTypes>
      <StatelessServiceType ServiceTypeName="azurevotebackType" UseImplicitHost="true">
   </StatelessServiceType>
   </ServiceTypes>
   
   <CodePackage Name="code" Version="1.0.0">
      <EntryPoint>
         <ContainerHost>
            <ImageName>my.azurecr.io/azure-vote-back:v1</ImageName>
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
      <Endpoint Name="azurevotebackTypeEndpoint" UriScheme="http" Port="6379" Protocol="http"/>
    </Endpoints>
  </Resources>
 </ServiceManifest>
```
## <a name="next-steps"></a>Następne kroki

W tym samouczku wielu kontenerów zostały umieszczone w aplikacji usługi Service Fabric, przy użyciu narzędzia Yeoman. Ta aplikacja została następnie wdrożone i uruchomić w klastrze usługi sieć szkieletowa. Wykonano następujące czynności:

> [!div class="checklist"]
> * Zainstaluj narzędzia Yeoman  
> * Utwórz pakiet aplikacji za pomocą narzędzia Yeoman
> * Skonfiguruj ustawienia w pakiecie aplikacji do użycia z kontenerami
> * Kompilowanie aplikacji  
> * Wdrażanie i uruchamianie aplikacji 
> * Oczyszczanie aplikacji

Przejdź do następnego samouczek informacje na temat trybu failover i skalowanie aplikacji w sieci szkieletowej usług.

> [!div class="nextstepaction"]
> [Dowiedz się więcej o pracy awaryjnej i skalowanie aplikacji](service-fabric-tutorial-containers-failover.md)

[votingapp]: ./media/service-fabric-tutorial-deploy-run-containers/votingapp.png
[sfx]: ./media/service-fabric-tutorial-deploy-run-containers/containerspackagetutorialsfx.png


