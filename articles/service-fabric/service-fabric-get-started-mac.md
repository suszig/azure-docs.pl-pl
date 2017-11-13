---
title: "Konfigurowanie środowiska projektowego w systemie Mac OS X do pracy z usługą Azure Service Fabric | Microsoft Docs"
description: "Zainstaluj środowisko uruchomieniowe, zestaw SDK i narzędzia oraz utwórz lokalny klaster projektowy. Po ukończeniu tej konfiguracji wszystko będzie gotowe do kompilowania aplikacji w systemie Mac OS X."
services: service-fabric
documentationcenter: java
author: sayantancs
manager: timlt
editor: 
ms.assetid: bf84458f-4b87-4de1-9844-19909e368deb
ms.service: service-fabric
ms.devlang: java
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/31/2017
ms.author: saysa
ms.openlocfilehash: cf67c377cd5c17f7b540fb23af48a333a9cddf69
ms.sourcegitcommit: 38c9176c0c967dd641d3a87d1f9ae53636cf8260
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/06/2017
---
# <a name="set-up-your-development-environment-on-mac-os-x"></a>Konfigurowanie środowiska projektowego w systemie Mac OS X
> [!div class="op_single_selector"]
> * [Windows](service-fabric-get-started.md)
> * [Linux](service-fabric-get-started-linux.md)
> * [OSX](service-fabric-get-started-mac.md)
>
>  

Przy użyciu systemu Mac OS X można kompilować aplikacje usługi Service Fabric, aby działały w klastrach Linux.

## <a name="prerequisites"></a>Wymagania wstępne
Usługa Service Fabric nie działa natywnie w systemie OS X. Aby uruchomić lokalny klaster usługi Service Fabric, udostępniamy wstępnie skonfigurowany obraz kontenera Docker. Przed rozpoczęciem potrzebne są następujące elementy:

* Co najmniej 4 GB pamięci RAM
* Najnowsza wersja platformy [Docker](https://www.docker.com/)
* Dostęp do jednopunktowego [obrazu](https://hub.docker.com/r/servicefabricoss/service-fabric-onebox/) kontenera Docker usługi Service Fabric

>[!TIP]
> * W celu zainstalowania platformy Docker na komputerze Mac możesz wykonać czynności opisane w oficjalnej [dokumentacji](https://docs.docker.com/docker-for-mac/install/#what-to-know-before-you-install). 
> * Po zakończeniu instalacji upewnij się, że została ona przeprowadzona pomyślnie, wykonując kroki opisane [tutaj](https://docs.docker.com/docker-for-mac/#check-versions-of-docker-engine-compose-and-machine).


## <a name="create-a-local-container-and-setup-service-fabric"></a>Tworzenie kontenera lokalnego i konfigurowanie usługi Service Fabric
Aby skonfigurować lokalny kontener platformy Docker i uruchomić na nim klaster usługi Service Fabric, wykonaj następujące czynności:

1. Ściągnij obraz z repozytorium platformy Docker:

    ```bash
    docker pull servicefabricoss/service-fabric-onebox
    ```

2. Uruchom jednopunktowe wystąpienie kontenera Docker usługi Service Fabric przy użyciu obrazu:

    ```bash
    docker run -itd -p 19080:19080 --name sfonebox servicefabricoss/service-fabric-onebox
    ```
    >[!TIP]
    >Określenie nazwy wystąpienia kontenera pomoże usprawnić jego obsługę. 

3. Zaloguj się do kontenera Docker w trybie interaktywnym ssh:

    ```bash
    docker exec -it sfonebox bash
    ```

4. Uruchom skrypt instalacji, który pobierze wymagane zależności, a następnie uruchom klaster w kontenerze.

    ```bash
    ./setup.sh     # Fetches and installs the dependencies required for Service Fabric to run
    ./run.sh       # Starts the local cluster
    ```

5. Po pomyślnym ukończeniu kroku 4 możesz przejść pod adres ``http://localhoist:19080`` z poziomu komputera Mac. Powinno wyświetlić się narzędzie Service Fabric Explorer.


## <a name="set-up-the-service-fabric-cli-sfctl-on-your-mac"></a>Konfigurowanie interfejsu wiersza polecenia usługi Service Fabric (sfctl) na komputerze Mac

Postępuj zgodnie z instrukcjami w temacie [Interfejs wiersza polecenia usługi Service Fabric](service-fabric-cli.md#cli-mac), aby zainstalować interfejs wiersza polecenia usługi Service Fabric (`sfctl`) na komputerze Mac.
Interfejs wiersza polecenia zawiera polecenia służące do interakcji z jednostkami usługi Service Fabric, w tym klastrami, aplikacjami i usługami.

## <a name="create-application-on-your-mac-using-yeoman"></a>Tworzenie aplikacji na komputerze Mac przy użyciu narzędzia Yeoman

Usługa Service Fabric udostępnia narzędzia do tworzenia szkieletów, które ułatwiają tworzenie aplikacji usługi Service Fabric z poziomu terminalu przy użyciu generatora szablonów narzędzia Yeoman. Wykonaj poniższe kroki, aby upewnić się, że generator szablonów Yeoman usługi Service Fabric działa na Twoim komputerze.

1. Na komputerze Mac musi być zainstalowane środowisko Node.js i narzędzie NPM. Jeśli jeszcze nie zostało to zrobione, możesz zainstalować środowisko Node.js i narzędzie NPM przy użyciu programu Homebrew, uruchamiając poniższe polecenie. Aby sprawdzić wersje środowiska Node.js i narzędzia NPM zainstalowane na komputerze Mac, możesz użyć opcji ``-v``.

    ```bash
    brew install node
    node -v
    npm -v
    ```
2. Zainstaluj generator szablonów [narzędzia Yeoman](http://yeoman.io/) na swoim komputerze z poziomu narzędzia NPM.

    ```bash
    npm install -g yo
    ```
3. Zainstaluj generator Yeoman, którego chcesz używać, zgodnie z instrukcjami podanymi w [dokumentacji](service-fabric-get-started-linux.md) wprowadzającej. Aby utworzyć aplikacje usługi Service Fabric za pomocą narzędzia Yeoman, wykonaj następujące kroki:

    ```bash
    npm install -g generator-azuresfjava       # for Service Fabric Java Applications
    npm install -g generator-azuresfguest      # for Service Fabric Guest executables
    npm install -g generator-azuresfcontainer  # for Service Fabric Container Applications
    ```
4. Aby skompilować aplikację Java usługi Service Fabric na komputerze Mac, będziesz potrzebować zestawu JDK 1.8 i narzędzia Gradle zainstalowanych na komputerze hosta. Jeśli jeszcze nie zostało to zrobione, możesz przeprowadzić instalację za pomocą programu [HomeBrew](https://brew.sh/). 

    ```bash
    brew update
    brew cask install java
    brew install gradle
    ```

## <a name="deploy-application-on-your-mac-from-terminal"></a>Wdrażanie aplikacji na komputerze Mac z poziomu terminalu

Po utworzenia i skompilowaniu aplikacji usługi Service Fabric możesz wdrożyć ją przy użyciu [interfejsu wiersza polecenia usługi Service Fabric](service-fabric-cli.md#cli-mac), wykonując następujące kroki:

1. Połącz się z klastrem usługi Service Fabric działającym wewnątrz wystąpienia kontenera na komputerze Mac.

    ```bash
    sfctl cluster select --endpoint http://localhost:19080
    ```

2. Przejdź do katalogu projektu i uruchom skrypt instalacji.

    ```bash
    cd MyProject
    bash install.sh
    ```

## <a name="set-up-net-core-20-development"></a>Konfigurowanie tworzenia aplikacji w programie .NET Core 2.0

Zainstaluj [zestaw .NET Core 2.0 SDK dla komputerów Mac](https://www.microsoft.com/net/core#macos), aby uruchomić [tworzenie aplikacji usługi Service Fabric w języku C#](service-fabric-create-your-first-linux-application-with-csharp.md). Pakiety dla aplikacji .NET Core 2.0 usługi Service Fabric są hostowane w witrynie NuGet.org, obecnie w wersji zapoznawczej.

## <a name="install-the-service-fabric-plugin-for-eclipse-neon-on-your-mac"></a>Instalowanie wtyczki usługi Service Fabric dla środowiska Eclipse Neon na komputerze Mac

Usługa Service Fabric udostępnia wtyczkę dla **środowiska IDE Eclipse Neon dla języka Java**, która upraszcza proces tworzenia, kompilowania i wdrażania usług Java. Możesz wykonać kroki instalacji wymienione w ogólnej [dokumentacji](service-fabric-get-started-eclipse.md#install-or-update-the-service-fabric-plug-in-in-eclipse-neon) dotyczącej instalowania lub aktualizowania wtyczki dla najnowszej wersji usługi Service Fabric dla środowiska Eclipse.

Pozostałe kroki wymienione w [dokumentacji środowiska Eclipse usługi Service Fabric](service-fabric-get-started-eclipse.md) dotyczące tworzenia aplikacji, dodawania usługi do aplikacji, instalowania/odinstalowywania aplikacji itp. również będzie można tutaj zastosować.

Aby wtyczka usługi Service Fabric środowiska Eclipse mogła współpracować z kontenerem Docker na komputerze Mac, należy również oprócz kroków opisanych powyżej utworzyć wystąpienie kontenera ze ścieżką współdzieloną z hostem w następujący sposób:
```bash
docker run -itd -p 19080:19080 -v /Users/sayantan/work/workspaces/mySFWorkspace:/tmp/mySFWorkspace --name sfonebox servicefabricoss/service-fabric-onebox
```
gdzie element ``/Users/sayantan/work/workspaces/mySFWorkspace`` jest w pełni kwalifikowaną ścieżką obszaru roboczego komputera Mac, a element ``/tmp/mySFWorkspace`` jest ścieżką wewnątrz kontenera, na którego będzie on zamapowany.

> [!NOTE]
>1. Jeśli nazwa/ścieżka obszaru roboczego jest inna, zaktualizuj ją odpowiednio przy użyciu powyższego polecenia ``docker run``.
>2. Jeśli uruchamiasz kontener o innej nazwie niż ``sfonebox``, zaktualizuj ją odpowiednio w pliku ``testclient.sh`` w aplikacji Java usługi Service Fabric aktora.

## <a name="next-steps"></a>Następne kroki
<!-- Links -->
* [Create and deploy your first Service Fabric Java application on Linux using Yeoman](service-fabric-create-your-first-linux-application-with-java.md) (Tworzenie i wdrażanie pierwszej aplikacji Java usługi Service Fabric w systemie Linux przy użyciu programu Yeoman)
* [Create and deploy your first Service Fabric Java application on Linux using Service Fabric Plugin for Eclipse](service-fabric-get-started-eclipse.md) (Tworzenie i wdrażanie pierwszej aplikacji Java usługi Service Fabric w systemie Linux przy użyciu wtyczki usługi Service Fabric dla środowiska Eclipse)
* [Tworzenie klastra usługi Service Fabric w witrynie Azure Portal](service-fabric-cluster-creation-via-portal.md)
* [Tworzenie klastra usługi Service Fabric przy użyciu usługi Azure Resource Manager](service-fabric-cluster-creation-via-arm.md)
* [Informacje o modelu aplikacji usługi Service Fabric](service-fabric-application-model.md)
* [Use the Service Fabric CLI to manage your applications](service-fabric-application-lifecycle-sfctl.md) (Zarządzanie aplikacjami przy użyciu interfejsu wiersza polecenia usługi Service Fabric)

<!-- Images -->
[cluster-setup-script]: ./media/service-fabric-get-started-mac/cluster-setup-mac.png
[sfx-mac]: ./media/service-fabric-get-started-mac/sfx-mac.png
[sf-eclipse-plugin-install]: ./media/service-fabric-get-started-mac/sf-eclipse-plugin-install.png
[buildship-update]: https://projects.eclipse.org/projects/tools.buildship
