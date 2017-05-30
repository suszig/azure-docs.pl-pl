---
title: "Tworzenie aplikacji kontenera usługi Azure Service Fabric | Microsoft Docs"
description: "Utwórz pierwszą aplikację kontenera w usłudze Azure Service Fabric.  Zbuduj obraz Docker za pomocą swojej aplikacji, wypchnij obraz do rejestru kontenerów, zbuduj i wdróż aplikacją kontenera usługi Service Fabric."
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
ms.date: 05/08/2017
ms.author: ryanwi
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: acb68b274228aa647dc7be5d36b2b077bd213c1b
ms.contentlocale: pl-pl
ms.lasthandoff: 05/10/2017


---

# <a name="create-your-first-service-fabric-container-app"></a>Tworzenie pierwszej aplikacji kontenera usługi Service Fabric
Uruchomienie istniejącej aplikacji w kontenerze systemu Windows w klastrze usługi Service Fabric nie wymaga dokonywania żadnych zmian w aplikacji. Ten przewodnik Szybki start przeprowadzi Cię przez proces tworzenia obrazu Docker zawierającego aplikację sieci Web, wypychania nowego obrazu do usługi Azure Container Registry, tworzenia aplikacji kontenera usługi Service Fabric i wdrażania aplikacji kontenera w klastrze usługi Service Fabric.  W tym artykule przyjęto założenie, że masz podstawową wiedzą dotyczącą platformy Docker. Aby uzyskać informacje dotyczące platformy Docker, przeczytaj artykuł [Docker Overview](https://docs.docker.com/engine/understanding-docker/) (Przegląd platformy Docker).

## <a name="prerequisites"></a>Wymagania wstępne
Komputer dewelopera z następującym oprogramowaniem:
* Program Visual Studio 2015 lub Visual Studio 2017.
* [Zestaw SDK usługi Service Fabric oraz narzędzia](service-fabric-get-started.md).
*  Program Docker dla systemu Windows.  [Pobierz program Docker CE dla systemu Windows (wersja stabilna)](https://store.docker.com/editions/community/docker-ce-desktop-windows?tab=description). Po zainstalowaniu i uruchomieniu programu Docker kliknij prawym przyciskiem myszy jego ikonę na pasku zadań i wybierz pozycję **Switch to Windows containers** (Przełącz na kontenery systemu Windows). Jest to wymagane do uruchomienia obrazów Docker opartych na systemie Windows.

Klaster systemu Windows z co najmniej trzema węzłami działającymi w systemie Windows Server 2016 z kontenerami — [utwórz klaster](service-fabric-get-started-azure-cluster.md) lub [wypróbuj bezpłatnie usługę Service Fabric](http://tryazureservicefabric.westus.cloudapp.azure.com/). 

Rejestr w usłudze Azure Container Registry — [utwórz rejestr kontenera](../container-registry/container-registry-get-started-portal.md) w subskrypcji platformy Azure. 

## <a name="create-a-simple-web-app"></a>Tworzenie prostej aplikacji sieci Web
Zbierz w jednym miejscu wszystkie elementy zawartości konieczne do załadowania do obrazu Docker. Na potrzeby tego przewodnika Szybki start utwórz aplikację sieci Web „Hello World” na komputerze dewelopera.

1. Utwórz katalog, na przykład *c:\temp\helloworldapp*.
2. Utwórz podkatalog *c:\temp\helloworldapp\content*.
3. Utwórz plik *index.html* w katalogu *c:\temp\helloworldapp\content*.
4. Edytuj plik *index.html* i dodaj do niego następujący wiersz:
    ```
    <h1>Hello World!</h1>
    ```
5. Zapisz zmiany w pliku *index.html*.

## <a name="build-the-docker-image"></a>Budowanie obrazu Docker
Zbuduj obraz na podstawie [obrazu microsoft/iis](https://hub.docker.com/r/microsoft/iis/) znajdującego się w usłudze Docker Hub. Obraz microsoft/iis został utworzony na podstawie obrazu podstawowego systemu operacyjnego Windows Server Core i zawiera usługi Internet Information Services (IIS).  Uruchomienie tego obrazu w kontenerze powoduje automatyczne uruchomienie usług IIS i zainstalowanych witryn sieci Web.

Zdefiniuj obraz Docker w pliku Dockerfile. Plik Dockerfile zawiera instrukcje dotyczące budowania obrazu i ładowania aplikacji, którą chcesz uruchomić. Plik Dockerfile stanowi dane wejściowe dla polecenia ```docker build```, za pomocą którego jest tworzony obraz. 

1. Utwórz plik *Dockerfile* (bez rozszerzenia) w katalogu *c:\temp\helloworldapp* i dodaj następujące wiersze:

    ```
    # The `FROM` instruction specifies the base image. You are
    # extending the `microsoft/iis` image.
    FROM microsoft/iis

    # Create a directory to hold the web app in the container.
    RUN mkdir C:\site

    # Create a new IIS site.
    RUN powershell -NoProfile -Command \
        Import-module IISAdministration; \
        New-IISSite -Name "Site" -PhysicalPath C:\site -BindingInformation "*:8000:"

    # Opens port 8000 on the container.
    EXPOSE 8000

    # The final instruction copies the web app you created earlier into the container.
    ADD content/ /site
    ```

    W tym pliku Dockerfile nie ma polecenia ```ENTRYPOINT```. Nie jest ono potrzebne. Podczas uruchamiania systemu Windows Server z usługami IIS proces IIS jest punktem wejścia skonfigurowanym do uruchamiania w obrazie podstawowym.

    Przeczytaj artykuł [Dockerfile reference](https://docs.docker.com/engine/reference/builder/) (Informacje o pliku Dockerfile), aby uzyskać więcej informacji.

2. Uruchom polecenie ```docker build```, aby utworzyć obraz uruchamiający aplikację sieci Web. Otwórz okno programu PowerShell i przejdź do katalogu *c:\temp\helloworldapp*. Uruchom następujące polecenie:

    ```
    docker build -t helloworldapp .
    ```
    To polecenie powoduje zbudowanie nowego obrazu zgodnie z instrukcjami z pliku Dockerfile i nadanie obrazowi nazwy „helloworldapp” (dzięki zastosowaniu parametru tagowania -t). Proces budowania obrazu powoduje pobranie obrazu podstawowego z usługi Docker Hub i utworzenie nowego obrazu z aplikacją dodaną do obrazu podstawowego.  [Obraz microsft/iis](https://hub.docker.com/r/microsoft/iis/) i obraz podstawowy systemu operacyjnego mają rozmiar 10,5 GB, dlatego ich pobranie i wyodrębnienie na komputerze dewelopera zajmie trochę czasu.  Możesz w tym czasie coś zjeść lub napić się kawy.  Pobieranie zajmuje mniej czasu, jeśli wcześniej pobrano na komputer dewelopera obraz podstawowy systemu operacyjnego.

3. Po zakończeniu działania polecenia budowania uruchom polecenie `docker images`, aby wyświetlić informacje o nowym obrazie:

    ```
    docker images
    
    REPOSITORY                    TAG                 IMAGE ID            CREATED             SIZE
    helloworldapp              latest              86838648aab6        2 minutes ago       10.1 GB
    ```

## <a name="verify-the-image-runs-locally"></a>Lokalne sprawdzanie działania obrazu
Przed wypchnięciem obrazu do rejestru kontenerów należy lokalnie sprawdzić jego działanie.  

1. Uruchom kontener za pomocą polecenia ```docker run```:

    ```
    docker run -d -p 8000:8000 --name my-web-site helloworldapp
    ```

    Parametr *name* umożliwia nadanie nazwy działającemu kontenerowi (zamiast identyfikatora kontenera).

2. Po uruchomieniu kontenera znajdź jego adres IP, dzięki czemu będzie można połączyć się z działającym kontenerem za pomocą przeglądarki:
    ```
    docker inspect -f "{{ .NetworkSettings.Networks.nat.IPAddress }}" my-web-site
    ```

3. Nawiąż połączenie z działającym kontenerem.  Otwórz przeglądarkę sieci Web i wpisz adres IP zwrócony na porcie 8000, na przykład „ http://172.31.194.61:8000 ”. W przeglądarce powinien zostać wyświetlony nagłówek „Hello World!”.

4. Aby zatrzymać kontener, uruchom polecenie:

    ```
    docker stop my-web-site
    ```

5. Usuń kontener z komputera dewelopera:

    ```
    docker rm my-web-site
    ```

## <a name="push-the-image-to-the-container-registry"></a>Wypychanie obrazu do rejestru kontenerów
Po sprawdzeniu, że kontener działa na komputerze dewelopera, wypchnij obraz do rejestru w usłudze Azure Container Registry.

1. Uruchom ``docker login``, aby zalogować się do rejestru kontenera za pomocą Twoich [poświadczeń rejestru](../container-registry/container-registry-authentication.md).

    Poniższy przykład przekazuje identyfikator i hasło [nazwy głównej usługi](../active-directory/active-directory-application-objects.md) Azure Active Directory. Na przykład nazwę główną usługi można było przypisać do rejestru dla scenariusza automatyzacji.

    ```
    docker login myregistry.azurecr.io -u xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx -p myPassword
    ```

2. Następujące polecenie tworzy tag lub alias obrazu z w pełni kwalifikowaną ścieżką do Twojego rejestru. W tym przykładzie obraz jest umieszczany w przestrzeni nazw ```samples```, aby uniknąć zaśmiecania katalogu głównego rejestru.

    ```
    docker tag helloworldapp myregistry.azurecr.io/samples/helloworldapp
    ```

3.  Wypchnij obraz do rejestru kontenerów:

    ```
    docker push myregistry.azurecr.io/samples/helloworldapp
    ```

## <a name="create-and-package-the-containerized-service-in-visual-studio"></a>Tworzenie i pakowanie konteneryzowanej usługi w programie Visual Studio
Zestaw SDK usługi Service Fabric oraz narzędzia udostępniają szablon usługi ułatwiający wdrażanie kontenera w klastrze usługi Service Fabric.

1. Uruchom program Visual Studio.  Wybierz kolejno pozycje **Plik** > **Nowy** > **Projekt**.
2. Wybierz pozycję **Aplikacja usługi Service Fabric**, nadaj jej nazwę „MyFirstContainer” i kliknij przycisk **OK**.
3. Z listy **szablonów usług** wybierz pozycję **Kontener gościa**.
4. W polu **Nazwa obrazu** wprowadź „myregistry.azurecr.io/samples/helloworldapp”. Jest to obraz wypchnięty do repozytorium kontenerów. 
5. Nadaj nazwę usłudze i kliknij przycisk **OK**.
6. Jeśli konteneryzowana usługa wymaga punktu końcowego na potrzeby komunikacji, możesz teraz dodać protokół, port i typ do parametru ```Endpoint``` w pliku ServiceManifest.xml. W tym przewodniku Szybki start konteneryzowana usługa nasłuchuje na porcie 80: 

    ```xml
    <Endpoint Name="Guest1TypeEndpoint" UriScheme="http" Port="80" Protocol="http"/>
    ```
    Jeśli zostanie określony parametr ```UriScheme```, punkt końcowy kontenera zostanie automatycznie zarejestrowany w usłudze Service Fabric Naming, aby można go było odnaleźć. Pełny przykładowy plik ServiceManifest.xml znajduje się na końcu tego artykułu. 
7. Skonfiguruj mapowanie portów kontenera typu port do hosta, określając zasady ```PortBinding``` w sekcji ```ContainerHostPolicies``` pliku ApplicationManifest.xml.  W tym przewodniku Szybki start wartość portu dla parametru ```ContainerPort``` wynosi 8000 (kontener udostępnia port 8000, jak określono w pliku Dockerfile), a wartością parametru ```EndpointRef``` jest „Guest1TypeEndpoint” (punkt końcowy zdefiniowany w manifeście usługi).  Żądania przychodzące do usługi na porcie 80 są mapowane na port 8000 w kontenerze.  Jeśli kontener wymaga uwierzytelniania w prywatnym repozytorium, dodaj parametr ```RepositoryCredentials```.  Na potrzeby tego przewodnika Szybki start dodaj nazwę konta i hasło dla rejestru kontenerów myregistry.azurecr.io. 

    ```xml
    <Policies>
        <ContainerHostPolicies CodePackageRef="Code">
            <RepositoryCredentials AccountName="myregistry" Password="=P==/==/=8=/=+u4lyOB=+=nWzEeRfF=" PasswordEncrypted="false"/>
            <PortBinding ContainerPort="8000" EndpointRef="Guest1TypeEndpoint"/>
        </ContainerHostPolicies>
    </Policies>
    ```

    Pełny przykładowy plik ApplicationManifest.xml znajduje się na końcu tego artykułu.
8. Skonfiguruj punkt końcowy połączenia klastra, aby można było opublikować aplikację w klastrze.  Punkt końcowy połączenia klienta można znaleźć w bloku Przegląd klastra w witrynie [Azure Portal](https://portal.azure.com). W Eksploratorze rozwiązań otwórz plik *Cloud.xml* znajdujący się w obszarze **MyFirstContainer**->**PublishProfiles**.  Dodaj nazwę klastra i port połączenia do parametru **ClusterConnectionParameters**.  Na przykład:
    ```xml
    <ClusterConnectionParameters ConnectionEndpoint="containercluster.westus2.cloudapp.azure.com:19000" />
    ```
    
9. Zapisz wszystkie pliki i skompiluj projekt.  

10. Aby spakować aplikację, w Eksploratorze rozwiązań kliknij prawym przyciskiem myszy węzeł **MyFirstContainer** i wybierz pozycję **Pakiet**. 

## <a name="deploy-the-container-app"></a>Wdrażanie aplikacji kontenera
1. Aby opublikować aplikację, w Eksploratorze rozwiązań kliknij prawym przyciskiem myszy węzeł **MyFirstContainer** i wybierz pozycję **Publikuj**.

2. [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) to oparte na sieci Web narzędzie do sprawdzania aplikacji i węzłów oraz zarządzania nimi w klastrze usługi Service Fabric. Otwórz przeglądarkę, przejdź do adresu http://containercluster.westus2.cloudapp.azure.com:19080/Explorer/ i wykonaj czynności wdrażania aplikacji.  Aplikacja zostanie wdrożona, ale pozostanie w stanie błędu do czasu pobrania obrazu na węzły klastra (co może zająć trochę czasu w zależności od rozmiaru obrazu):  ![Błąd][1]

3. Aplikacja jest gotowa, ale w stanie ```Ready```:  ![Gotowa][2]

4. Otwórz przeglądarkę i przejdź do adresu http://containercluster.westus2.cloudapp.azure.com. W przeglądarce powinien zostać wyświetlony nagłówek „Hello World!”.

## <a name="clean-up"></a>Czyszczenie
Jeśli podczas działania klastra nadal są naliczane opłaty, rozważ [usunięcie klastra](service-fabric-get-started-azure-cluster.md#remove-the-cluster).  [Klastry innych firm](http://tryazureservicefabric.westus.cloudapp.azure.com/) są automatycznie usuwane po kilku godzinach.

Po wypchnięciu obrazu do rejestru kontenerów można usunąć lokalny obraz z komputera dewelopera:

```
docker rmi helloworldapp
docker rmi myregistry.azurecr.io/samples/helloworldapp
```

## <a name="complete-example-service-fabric-application-and-service-manifests"></a>Kompletny przykład aplikacji i manifestów usługi Service Fabric
Zamieszczono tutaj kompletne manifesty usługi i aplikacji używane w tym przewodniku Szybki start.

### <a name="servicemanifestxml"></a>ServiceManifest.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest Name="Guest1Pkg"
                 Version="1.0.0"
                 xmlns="http://schemas.microsoft.com/2011/01/fabric"
                 xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <ServiceTypes>
    <!-- This is the name of your ServiceType.
         The UseImplicitHost attribute indicates this is a guest service. -->
    <StatelessServiceType ServiceTypeName="Guest1Type" UseImplicitHost="true" />
  </ServiceTypes>

  <!-- Code package is your service executable. -->
  <CodePackage Name="Code" Version="1.0.0">
    <EntryPoint>
      <!-- Follow this link for more information about deploying Windows containers to Service Fabric: https://aka.ms/sfguestcontainers -->
      <ContainerHost>
        <ImageName>myregistry.azurecr.io/samples/helloworldapp</ImageName>
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
      <Endpoint Name="Guest1TypeEndpoint" UriScheme="http" Port="80" Protocol="http"/>
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
    <Parameter Name="Guest1_InstanceCount" DefaultValue="-1" />
  </Parameters>
  <!-- Import the ServiceManifest from the ServicePackage. The ServiceManifestName and ServiceManifestVersion 
       should match the Name and Version attributes of the ServiceManifest element defined in the 
       ServiceManifest.xml file. -->
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="Guest1Pkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides />
    <Policies>
      <ContainerHostPolicies CodePackageRef="Code">
        <RepositoryCredentials AccountName="myregistry" Password="=P==/==/=8=/=+u4lyOB=+=nWzEeRfF=" PasswordEncrypted="false"/>
        <PortBinding ContainerPort="8000" EndpointRef="Guest1TypeEndpoint"/>
      </ContainerHostPolicies>
    </Policies>
  </ServiceManifestImport>
  <DefaultServices>
    <!-- The section below creates instances of service types, when an instance of this 
         application type is created. You can also create one or more instances of service type using the 
         ServiceFabric PowerShell module.
         
         The attribute ServiceTypeName below must match the name defined in the imported ServiceManifest.xml file. -->
    <Service Name="Guest1">
      <StatelessService ServiceTypeName="Guest1Type" InstanceCount="[Guest1_InstanceCount]">
        <SingletonPartition />
      </StatelessService>
    </Service>
  </DefaultServices>
</ApplicationManifest>
```

## <a name="next-steps"></a>Następne kroki
* Dowiedz się więcej o uruchamianiu [kontenerów w usłudze Service Fabric](service-fabric-containers-overview.md).
* Uzyskaj informacje o [cyklu życia aplikacji](service-fabric-application-lifecycle.md) usługi Service Fabric.
* Sprawdź [przykłady kodu kontenera usługi Service Fabric](https://github.com/Azure-Samples/service-fabric-dotnet-containers) w witrynie GitHub.

[1]: ./media/service-fabric-get-started-containers/MyFirstContainerError.png
[2]: ./media/service-fabric-get-started-containers/MyFirstContainerReady.png

