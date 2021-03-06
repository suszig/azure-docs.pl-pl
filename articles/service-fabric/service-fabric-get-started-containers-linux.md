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
ms.date: 1/09/2018
ms.author: ryanwi
ms.openlocfilehash: 0e7e0f1262ee8c31bc6e71b49e9ef62129887f2c
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/24/2018
---
# <a name="create-your-first-service-fabric-container-application-on-linux"></a>Tworzenie pierwszej aplikacji kontenera usługi Service Fabric w systemie Linux
> [!div class="op_single_selector"]
> * [Windows](service-fabric-get-started-containers.md)
> * [Linux](service-fabric-get-started-containers-linux.md)

Uruchomienie istniejącej aplikacji w kontenerze systemu Linux w klastrze usługi Service Fabric nie wymaga dokonywania żadnych zmian w aplikacji. W tym artykule przedstawiono proces tworzenia obrazu Docker zawierającego aplikację sieci Web w języku Python na platformie [Flask](http://flask.pocoo.org/) oraz wdrażania go w klastrze usługi Service Fabric.  Będziesz również udostępniać aplikację skonteneryzowaną za pomocą usługi [Azure Container Registry](/azure/container-registry/).  W tym artykule przyjęto założenie, że masz podstawową wiedzą dotyczącą platformy Docker. Aby uzyskać informacje dotyczące platformy Docker, przeczytaj artykuł [Docker Overview](https://docs.docker.com/engine/understanding-docker/) (Przegląd platformy Docker).

## <a name="prerequisites"></a>Wymagania wstępne
* Komputer dewelopera z następującym oprogramowaniem:
  * [Zestaw SDK usługi Service Fabric oraz narzędzia](service-fabric-get-started-linux.md).
  * [Docker CE dla systemu Linux](https://docs.docker.com/engine/installation/#prior-releases). 
  * [Interfejs wiersza polecenia usługi Service Fabric](service-fabric-cli.md)

* Rejestr w usłudze Azure Container Registry — [utwórz rejestr kontenera](../container-registry/container-registry-get-started-portal.md) w subskrypcji platformy Azure. 

## <a name="define-the-docker-container"></a>Definiowanie kontenera platformy Docker
Zbuduj obraz na podstawie [obrazu Python](https://hub.docker.com/_/python/) znajdującego się w usłudze Docker Hub. 

Zdefiniuj kontener platformy Docker w pliku Dockerfile. Plik Dockerfile zawiera instrukcje dotyczące konfigurowania środowiska wewnątrz kontenera, ładowania aplikacji do uruchomienia i mapowania portów. Plik Dockerfile stanowi dane wejściowe dla polecenia `docker build`, za pomocą którego jest tworzony obraz. 

Utwórz pusty katalog, a następnie utwórz plik *Dockerfile* (bez rozszerzenia). Dodaj następujący kod do pliku *Dockerfile* i zapisz zmiany:

```
# Use an official Python runtime as a base image
FROM python:2.7-slim

# Set the working directory to /app
WORKDIR /app

# Copy the current directory contents into the container at /app
ADD . /app

# Install any needed packages specified in requirements.txt
RUN pip install -r requirements.txt

# Make port 80 available outside this container
EXPOSE 80

# Define environment variable
ENV NAME World

# Run app.py when the container launches
CMD ["python", "app.py"]
```

Przeczytaj artykuł [Dockerfile reference](https://docs.docker.com/engine/reference/builder/) (Informacje o pliku Dockerfile), aby uzyskać więcej informacji.

## <a name="create-a-simple-web-application"></a>Tworzenie prostej aplikacji sieci Web
Utwórz aplikację sieci Web platformy Flask, która nasłuchuje na porcie 80 i zwraca wartość „Hello World!”.  W tym samym katalogu utwórz plik *requirements.txt*.  Dodaj następujący kod i zapisz zmiany:
```
Flask
```

Utwórz również plik *app.py* i dodaj następujący kod:

```python
from flask import Flask

app = Flask(__name__)

@app.route("/")
def hello():
    
    return 'Hello World!'

if __name__ == "__main__":
    app.run(host='0.0.0.0', port=80)
```

## <a name="build-the-image"></a>Tworzenie obrazu
Uruchom polecenie `docker build`, aby utworzyć obraz uruchamiający aplikację sieci Web. Otwórz okno programu PowerShell i przejdź do katalogu *c:\temp\helloworldapp*. Uruchom następujące polecenie:

```bash
docker build -t helloworldapp .
```

To polecenie powoduje zbudowanie nowego obrazu zgodnie z instrukcjami z pliku Dockerfile i nadanie obrazowi nazwy „helloworldapp” (dzięki zastosowaniu parametru tagowania -t). Proces budowania obrazu powoduje pobranie obrazu podstawowego z usługi Docker Hub i utworzenie nowego obrazu z aplikacją dodaną do obrazu podstawowego.  

Po zakończeniu działania polecenia budowania uruchom polecenie `docker images`, aby wyświetlić informacje o nowym obrazie:

```bash
$ docker images
    
REPOSITORY                    TAG                 IMAGE ID            CREATED             SIZE
helloworldapp                 latest              86838648aab6        2 minutes ago       194 MB
```

## <a name="run-the-application-locally"></a>Uruchamianie aplikacji lokalnie
Przed wypchnięciem aplikacji skonteneryzowanej do rejestru kontenerów sprawdź, czy działa ona lokalnie.  

Uruchom aplikację, mapując port komputera 4000 na widoczny port kontenera 80:

```bash
docker run -d -p 4000:80 --name my-web-site helloworldapp
```

Parametr *name* umożliwia nadanie nazwy działającemu kontenerowi (zamiast identyfikatora kontenera).

Nawiąż połączenie z działającym kontenerem.  Otwórz przeglądarkę sieci Web i wpisz adres IP zwrócony na porcie 4000, na przykład „http://localhost:4000 ”. W przeglądarce powinien zostać wyświetlony nagłówek „Hello World!”.

![Hello World!][hello-world]

Aby zatrzymać kontener, uruchom polecenie:

```bash
docker stop my-web-site
```

Usuń kontener z komputera dewelopera:

```bash
docker rm my-web-site
```

## <a name="push-the-image-to-the-container-registry"></a>Wypychanie obrazu do rejestru kontenerów
Po sprawdzeniu, że aplikacja działa na platformie Docker, wypchnij obraz do rejestru w usłudze Azure Container Registry.

Uruchom `docker login`, aby zalogować się do rejestru kontenera za pomocą Twoich [poświadczeń rejestru](../container-registry/container-registry-authentication.md).

Poniższy przykład przekazuje identyfikator i hasło [nazwy głównej usługi](../active-directory/active-directory-application-objects.md) Azure Active Directory. Na przykład nazwę główną usługi można było przypisać do rejestru dla scenariusza automatyzacji.  Możesz również zalogować się za pomocą nazwy i hasła użytkownika rejestru.

```bash
docker login myregistry.azurecr.io -u xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx -p myPassword
```

Następujące polecenie tworzy tag lub alias obrazu z w pełni kwalifikowaną ścieżką do Twojego rejestru. W tym przykładzie obraz jest umieszczany w przestrzeni nazw `samples`, aby uniknąć zaśmiecania katalogu głównego rejestru.

```bash
docker tag helloworldapp myregistry.azurecr.io/samples/helloworldapp
```

Wypchnij obraz do rejestru kontenerów:

```bash
docker push myregistry.azurecr.io/samples/helloworldapp
```

## <a name="package-the-docker-image-with-yeoman"></a>Umieszczanie w pakiecie obrazu platformy Docker za pomocą narzędzia Yeoman
Zestaw SDK usługi Service Fabric dla systemu Linux zawiera generator [Yeoman](http://yeoman.io/), który ułatwia tworzenie własnej aplikacji i dodawanie obrazu kontenera. Wykorzystamy narzędzie Yeoman w celu utworzenia aplikacji z pojedynczym kontenerem platformy Docker o nazwie *SimpleContainerApp*.

Aby utworzyć aplikację kontenera usługi Service Fabric, otwórz okno terminalu i uruchom polecenie `yo azuresfcontainer`.  

Nazwij aplikację (np. „mycontainer”) i usługę aplikacji (np. „myservice”).

W przypadku nazwy obrazu podaj adres URL obrazu kontenera w rejestrze kontenerów (np. „myregistry.azurecr.io/samples/helloworldapp”). 

Ponieważ ten obraz ma zdefiniowany punkt wejścia obciążenia, nie musisz jawnie określić poleceń wejściowych (są to polecenia uruchamiane wewnątrz kontenera, które zapewnią działanie kontenera po uruchomieniu). 

Określ liczbę wystąpień jako „1”.

![Generator Yeoman usługi Service Fabric dla kontenerów][sf-yeoman]

## <a name="configure-port-mapping-and-container-repository-authentication"></a>Konfigurowanie mapowania portów i uwierzytelniania repozytorium kontenerów
Skonteneryzowana usługa wymaga punktu końcowego dla celów komunikacyjnych.  Teraz możesz dodać protokół, port i typ do elementu `Endpoint` w pliku ServiceManifest.xml w obszarze tagu „Zasoby”. W tym artykule skonteneryzowana usługa nasłuchuje na porcie 4000: 

```xml

<Resources>
    <Endpoints>
      <!-- This endpoint is used by the communication listener to obtain the port on which to 
           listen. Please note that if your service is partitioned, this port is shared with 
           replicas of different partitions that are placed in your code. -->
      <Endpoint Name="myServiceTypeEndpoint" UriScheme="http" Port="4000" Protocol="http"/>
    </Endpoints>
  </Resources>
 ```
 
Jeśli zostanie określony parametr `UriScheme`, punkt końcowy kontenera zostanie automatycznie zarejestrowany w usłudze Service Fabric Naming, aby można go było odnaleźć. Pełny przykładowy plik ServiceManifest.xml znajduje się na końcu tego artykułu. 

Skonfiguruj mapowanie portów kontenera typu port do hosta, określając zasady `PortBinding` w sekcji `ContainerHostPolicies` pliku ApplicationManifest.xml.  W tym artykule wartość portu dla parametru `ContainerPort` to 80 (kontener uwidacznia port 80, jak określono w pliku Dockerfile), a wartością parametru `EndpointRef` jest „myserviceTypeEndpoint” (punkt końcowy zdefiniowany w manifeście usługi).  Żądania przychodzące do usługi na porcie 4000 są mapowane na port 80 w kontenerze.  Jeśli kontener wymaga uwierzytelniania w prywatnym repozytorium, dodaj parametr `RepositoryCredentials`.  Na potrzeby tego artykułu dodaj nazwę konta i hasło dla rejestru kontenerów myregistry.azurecr.io. Upewnij się, że zasady zostały dodane w tagu „ServiceManifestImport” odpowiadającym właściwemu pakietowi usług.

```xml
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="MyServicePkg" ServiceManifestVersion="1.0.0" />
    <Policies>
        <ContainerHostPolicies CodePackageRef="Code">
        <RepositoryCredentials AccountName="myregistry" Password="=P==/==/=8=/=+u4lyOB=+=nWzEeRfF=" PasswordEncrypted="false"/>
        <PortBinding ContainerPort="80" EndpointRef="myServiceTypeEndpoint"/>
        </ContainerHostPolicies>
    </Policies>
   </ServiceManifestImport>
``` 
## <a name="configure-docker-healthcheck"></a>Konfigurowanie funkcji HEALTHCHECK platformy Docker 
Począwszy od wersji 6.1, usługa Service Fabric automatycznie integruje zdarzenia [funkcji HEALTHCHECK platformy Docker](https://docs.docker.com/engine/reference/builder/#healthcheck) z raportem o kondycji systemu. Oznacza to, że jeśli w kontenerze włączono funkcję **HEALTHCHECK**, usługa Service Fabric będzie raportować kondycję przy każdej zmianie stanu kondycji kontenera zgłoszonej przez platformę Docker. Raport kondycji **OK** pojawi się w narzędziu [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md), gdy wartość *health_status* będzie równa *healthy*, a raport **OSTRZEŻENIE** pojawi się, gdy wartość *health_status* będzie równa *unhealthy*. Instrukcja **HEALTHCHECK** wskazująca rzeczywisty test wykonywany w celu monitorowania kondycji kontenera musi występować w pliku **dockerfile** używanym podczas generowania obrazu kontenera. 

![HealthCheckHealthy][1]

![HealthCheckUnealthyApp][2]

![HealthCheckUnhealthyDsp][3]

Możesz skonfigurować zachowanie funkcji **HEALTHCHECK** dla każdego kontenera, określając opcje **HealthConfig** jako część sekcji **ContainerHostPolicies** w pliku ApplicationManifest.

```xml
<ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="ContainerServicePkg" ServiceManifestVersion="2.0.0" />
    <Policies>
      <ContainerHostPolicies CodePackageRef="Code">
        <HealthConfig IncludeDockerHealthStatusInSystemHealthReport="true" RestartContainerOnUnhealthyDockerHealthStatus="false" />
      </ContainerHostPolicies>
    </Policies>
</ServiceManifestImport>
```
Domyślnie wartość *IncludeDockerHealthStatusInSystemHealthReport* jest ustawiona na **true**, a wartość *RestartContainerOnUnhealthyDockerHealthStatus* jest ustawiona na **false**. Jeśli wartość *RestartContainerOnUnhealthyDockerHealthStatus* jest ustawiona na **true**, kontener wielokrotnie raportujący złą kondycję jest uruchamiany ponownie (potencjalnie w innych węzłach).

Aby wyłączyć integrację funkcji **HEALTHCHECK** dla całego klastra usługi Service Fabric, należy ustawić wartość [EnableDockerHealthCheckIntegration](service-fabric-cluster-fabric-settings.md) na **false**.

## <a name="build-and-package-the-service-fabric-application"></a>Kompilowanie i tworzenie pakietu aplikacji usługi Service Fabric
Szablony generatora Yeoman usługi Service Fabric obejmują skrypt kompilacji dla narzędzia [Gradle](https://gradle.org/), którego można użyć do skompilowania aplikacji z poziomu terminalu. Aby skompilować aplikację i utworzyć jej pakiet, uruchom następujące polecenie:

```bash
cd mycontainer
gradle
```

## <a name="deploy-the-application"></a>Wdrażanie aplikacji
Po skompilowaniu aplikację można wdrożyć w klastrze lokalnym za pomocą interfejsu wiersza polecenia usługi Service Fabric.

Połącz się z lokalnym klastrem usługi Service Fabric.

```bash
sfctl cluster select --endpoint http://localhost:19080
```

Użyj skryptu instalacji udostępnionego w szablonie, aby skopiować pakiet aplikacji do magazynu obrazów klastra, zarejestrować typ aplikacji i utworzyć wystąpienie aplikacji.

```bash
./install.sh
```

Otwórz przeglądarkę i przejdź do narzędzia Service Fabric Explorer dostępnego pod adresem http://localhost:19080/Explorer (zastąp łańcuch localhost prywatnym adresem IP maszyny wirtualnej w przypadku używania narzędzia Vagrant w systemie Mac OS X). Rozwiń węzeł Aplikacje i zwróć uwagę, że istnieje teraz wpis dla danego typu aplikacji i inny wpis dla pierwszego wystąpienia tego typu.

Nawiąż połączenie z działającym kontenerem.  Otwórz przeglądarkę sieci Web i wpisz adres IP zwrócony na porcie 4000, na przykład „http://localhost:4000 ”. W przeglądarce powinien zostać wyświetlony nagłówek „Hello World!”.

![Hello World!][hello-world]


## <a name="clean-up"></a>Czyszczenie
Użyj skryptu dezinstalacji udostępnionego w szablonie, aby usunąć wystąpienie aplikacji z lokalnego klastra projektowego i wyrejestrować typ aplikacji.

```bash
./uninstall.sh
```

Po wypchnięciu obrazu do rejestru kontenerów można usunąć lokalny obraz z komputera dewelopera:

```
docker rmi helloworldapp
docker rmi myregistry.azurecr.io/samples/helloworldapp
```

## <a name="complete-example-service-fabric-application-and-service-manifests"></a>Kompletny przykład aplikacji i manifestów usługi Service Fabric
W tym miejscu przedstawiono kompletne manifesty usługi i aplikacji używane w tym artykule.

### <a name="servicemanifestxml"></a>ServiceManifest.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest Name="myservicePkg"
                 Version="1.0.0"
                 xmlns="http://schemas.microsoft.com/2011/01/fabric"
                 xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <ServiceTypes>
    <!-- This is the name of your ServiceType.
         The UseImplicitHost attribute indicates this is a guest service. -->
    <StatelessServiceType ServiceTypeName="myserviceType" UseImplicitHost="true" />
  </ServiceTypes>

  <!-- Code package is your service executable. -->
  <CodePackage Name="Code" Version="1.0.0">
    <EntryPoint>
      <!-- Follow this link for more information about deploying containers 
      to Service Fabric: https://aka.ms/sfguestcontainers -->
      <ContainerHost>
        <ImageName>myregistry.azurecr.io/samples/helloworldapp</ImageName>
        <!-- Pass comma delimited commands to your container: dotnet, myproc.dll, 5" -->
        <!--Commands> dotnet, myproc.dll, 5 </Commands-->
        <Commands></Commands>
      </ContainerHost>
    </EntryPoint>
    <!-- Pass environment variables to your container: -->
    
    <EnvironmentVariables>
      <!--
      <EnvironmentVariable Name="VariableName" Value="VariableValue"/>
      -->
    </EnvironmentVariables>
    
  </CodePackage>

  <Resources>
    <Endpoints>
      <!-- This endpoint is used by the communication listener to obtain the port on which to 
           listen. Please note that if your service is partitioned, this port is shared with 
           replicas of different partitions that are placed in your code. -->
      <Endpoint Name="myServiceTypeEndpoint" UriScheme="http" Port="4000" Protocol="http"/>
    </Endpoints>
  </Resources>
</ServiceManifest>
```
### <a name="applicationmanifestxml"></a>ApplicationManifest.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest ApplicationTypeName="mycontainerType"
                     ApplicationTypeVersion="1.0.0"
                     xmlns="http://schemas.microsoft.com/2011/01/fabric"
                     xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                     xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <!-- Import the ServiceManifest from the ServicePackage. The ServiceManifestName and ServiceManifestVersion 
       should match the Name and Version attributes of the ServiceManifest element defined in the 
       ServiceManifest.xml file. -->
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="myservicePkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides />
    <Policies>
      <ContainerHostPolicies CodePackageRef="Code">
        <RepositoryCredentials AccountName="myregistry" Password="=P==/==/=8=/=+u4lyOB=+=nWzEeRfF=" PasswordEncrypted="false"/>
        <PortBinding ContainerPort="80" EndpointRef="myServiceTypeEndpoint"/>
      </ContainerHostPolicies>
    </Policies>
  </ServiceManifestImport>
  <DefaultServices>
    <!-- The section below creates instances of service types, when an instance of this 
         application type is created. You can also create one or more instances of service type using the 
         ServiceFabric PowerShell module.
         
         The attribute ServiceTypeName below must match the name defined in the imported ServiceManifest.xml file. -->
    <Service Name="myservice">
      <!-- On a local development cluster, set InstanceCount to 1.  On a multi-node production 
      cluster, set InstanceCount to -1 for the container service to run on every node in 
      the cluster.
      -->
      <StatelessService ServiceTypeName="myserviceType" InstanceCount="1">
        <SingletonPartition />
      </StatelessService>
    </Service>
  </DefaultServices>
</ApplicationManifest>
```
## <a name="adding-more-services-to-an-existing-application"></a>Dodawanie kolejnych usług do istniejącej aplikacji

Aby dodać kolejną usługę kontenera do aplikacji utworzonej już przy użyciu narzędzia Yeoman, wykonaj następujące czynności:

1. Zmień katalog na katalog główny istniejącej aplikacji.  Na przykład wpisz polecenie `cd ~/YeomanSamples/MyApplication`, jeśli aplikacja `MyApplication` to aplikacja utworzona przez narzędzie Yeoman.
2. Uruchom polecenie `yo azuresfcontainer:AddService`

<a id="manually"></a>


## <a name="configure-time-interval-before-container-is-force-terminated"></a>Konfigurowanie interwału czasu wymuszania przerwania działania kontenera

Możesz skonfigurować interwał czasu środowiska uruchomieniowego, po upływie którego kontener ma zostać usunięty po rozpoczęciu usuwania usługi (lub przenoszenia do innego węzła). Skonfigurowanie interwału czasu powoduje wysłanie polecenia `docker stop <time in seconds>` do kontenera.   Aby uzyskać więcej informacji, zobacz [docker stop](https://docs.docker.com/engine/reference/commandline/stop/). Interwał czasu oczekiwania jest określony w sekcji `Hosting`. W poniższym fragmencie manifestu klastra pokazano, jak ustawić interwał oczekiwania:


```json
{
        "name": "Hosting",
        "parameters": [
          {
                "name": "ContainerDeactivationTimeout",
                "value" : "10"
          },
          ...
        ]
}
```

Domyślny interwał to 10 sekund. Ta konfiguracja jest dynamiczna, dlatego uaktualnienie samej konfiguracji w klastrze powoduje zaktualizowanie limitu czasu. 

## <a name="configure-the-runtime-to-remove-unused-container-images"></a>Konfigurowanie środowiska uruchomieniowego w celu usunięcia nieużywanych obrazów kontenerów

Możesz skonfigurować klaster usługi Service Fabric w celu usuwania nieużywanych obrazów kontenerów z węzła. Ta konfiguracja pozwala na ponowne przechwycenie miejsca na dysku w przypadku zbyt wielu obrazów kontenerów w węźle.  Aby włączyć tę funkcję, zaktualizuj sekcję `Hosting` w manifeście klastra, jak pokazano w poniższym fragmencie kodu: 


```json
{
        "name": "Hosting",
        "parameters": [
          {
                "name": "PruneContainerImages",
                "value": "True"
          },
          {
                "name": "ContainerImagesToSkip",
                "value": "microsoft/windowsservercore|microsoft/nanoserver|microsoft/dotnet-frameworku|..."
          }
          ...
          }
        ]
} 
```

Jeśli nie chcesz usuwać pewnych obrazów, określ je przy użyciu parametru `ContainerImagesToSkip`. 

## <a name="configure-container-image-download-time"></a>Konfigurowanie czasu pobierania obrazów kontenerów

Domyślnie środowisko uruchomieniowe usługi Service Fabric przydziela 20 minut na pobieranie i wyodrębnianie obrazów kontenerów, co sprawdza się w przypadku większości obrazów kontenerów. W przypadku dużych obrazów lub wolnego połączenia sieciowego może być konieczne wydłużenie czasu oczekiwania przed przerwaniem pobierania i wyodrębniania obrazu. Można to skonfigurować przy użyciu atrybutu **ContainerImageDownloadTimeout** w sekcji **Hosting** manifestu klastra, jak pokazano w poniższym fragmencie kodu:

```json
{
"name": "Hosting",
        "parameters": [
          {
              "name": " ContainerImageDownloadTimeout ",
              "value": "1200"
          }
]
}
```


## <a name="set-container-retention-policy"></a>Ustawianie zasad przechowywania kontenerów

Aby ułatwić diagnozowanie błędów uruchamiania kontenerów, usługa Service Fabric (w wersji 6.1 lub nowszej) obsługuje przechowywanie kontenerów, w przypadku których działanie zostało przerwane lub uruchamianie nie powiodło się. Te zasady można ustawić w pliku **ApplicationManifest.xml**, jak pokazano w poniższym fragmencie kodu:

```xml
 <ContainerHostPolicies CodePackageRef="NodeService.Code" Isolation="process" ContainersRetentionCount="2"  RunInteractive="true"> 
```

Ustawienie **ContainersRetentionCount** określa liczbę kontenerów do przechowywania w przypadku wystąpienia w nich błędu. Jeśli zostanie określona wartość ujemna, będą przechowywane wszystkie kontenery z błędami. Jeśli atrybut **ContainersRetentionCount** nie zostanie określony, nie będą przechowywane żadne kontenery. Atrybut **ContainersRetentionCount** obsługuje też parametry aplikacji, dzięki czemu użytkownicy mogą określać różne wartości dla klastrów testowych i produkcyjnych. W przypadku używania tej funkcji zalecane jest stosowanie ograniczeń rozmieszczania, tak aby obiektem docelowym usługi kontenerów był określony węzeł, co zapobiega przenoszeniu usługi kontenerów do innych węzłów. Wszelkie kontenery przechowywane przy użyciu tej funkcji należy usunąć ręcznie.


## <a name="next-steps"></a>Następne kroki
* Dowiedz się więcej o uruchamianiu [kontenerów w usłudze Service Fabric](service-fabric-containers-overview.md).
* Zapoznaj się z samouczkiem [Deploy a .NET application in a container](service-fabric-host-app-in-a-container.md) (Wdrażanie aplikacji .NET w kontenerze).
* Uzyskaj informacje o [cyklu życia aplikacji](service-fabric-application-lifecycle.md) usługi Service Fabric.
* Sprawdź [przykłady kodu kontenera usługi Service Fabric](https://github.com/Azure-Samples/service-fabric-containers) w witrynie GitHub.

[hello-world]: ./media/service-fabric-get-started-containers-linux/HelloWorld.png
[sf-yeoman]: ./media/service-fabric-get-started-containers-linux/YoSF.png

[1]: ./media/service-fabric-get-started-containers/HealthCheckHealthy.png
[2]: ./media/service-fabric-get-started-containers/HealthCheckUnhealthy_App.png
[3]: ./media/service-fabric-get-started-containers/HealthCheckUnhealthy_Dsp.png
