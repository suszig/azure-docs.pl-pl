---
title: "Konfigurowanie środowiska projektowego w systemie Linux | Microsoft Docs"
description: "Zainstaluj środowisko uruchomieniowe i zestaw SDK oraz utwórz lokalny klaster projektowy w systemie Linux. Po ukończeniu tej konfiguracji wszystko będzie gotowe do tworzenia aplikacji."
services: service-fabric
documentationcenter: .net
author: seanmck
manager: timlt
editor: 
ms.assetid: d552c8cd-67d1-45e8-91dc-871853f44fc6
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/05/2017
ms.author: seanmck
translationtype: Human Translation
ms.sourcegitcommit: fc04c5f8a9cdee4b51c67b480d70678c3dca7c93
ms.openlocfilehash: 49391b604446ae1b08d04ca42c5bdcd132f8cf31


---
# <a name="prepare-your-development-environment-on-linux"></a>Przygotowywanie środowiska projektowego w systemie Linux
> [!div class="op_single_selector"]
> * [Windows](service-fabric-get-started.md)
> * [Linux](service-fabric-get-started-linux.md)
> * [OSX](service-fabric-get-started-mac.md)
>
>  

 Aby wdrażać i uruchamiać [aplikacje usługi Azure Service Fabric](service-fabric-application-model.md) na maszynie deweloperskiej z systemem Linux, należy zainstalować środowisko uruchomieniowe i wspólny zestaw SDK. Można także zainstalować opcjonalne zestawy SDK dla platformy Java i .NET Core.

## <a name="prerequisites"></a>Wymagania wstępne

### <a name="supported-operating-system-versions"></a>Obsługiwane wersje systemu operacyjnego
Na potrzeby tworzenia aplikacji obsługiwane są następujące wersje systemu operacyjnego:

* Ubuntu 16.04 („Xenial Xerus”)

## <a name="update-your-apt-sources"></a>Aktualizowanie źródeł APT
Aby zainstalować zestaw SDK i skojarzony pakiet środowiska uruchomieniowego przy użyciu polecenia apt-get, należy najpierw zaktualizować źródła APT.

1. Otwórz terminal.
2. Dodaj repozytorium usługi Service Fabric do listy źródeł.

    ```bash
    sudo sh -c 'echo "deb [arch=amd64] http://apt-mo.trafficmanager.net/repos/servicefabric/ trusty main" > /etc/apt/sources.list.d/servicefabric.list'
    ```
3. Dodaj nowy klucz GPG do pęku kluczy APT.

    ```bash
    sudo apt-key adv --keyserver apt-mo.trafficmanager.net --recv-keys 417A0893
    ```
4. Odśwież listę pakietów na podstawie nowo dodanych repozytoriów.

    ```bash
    sudo apt-get update
    ```

## <a name="install-and-set-up-the-sdk"></a>Instalowanie i konfigurowanie zestawu SDK
Po zaktualizowaniu źródeł można zainstalować zestaw SDK.

1. Zainstaluj pakiet zestawu SDK usługi Service Fabric. Zostanie wyświetlona prośba o potwierdzenie instalacji i zaakceptowanie umowy licencyjnej.

    ```bash
    sudo apt-get install servicefabricsdkcommon
    ```
2. Uruchom skrypt instalacji zestawu SDK.

    ```bash
    sudo /opt/microsoft/sdk/servicefabric/common/sdkcommonsetup.sh
    ```


## <a name="set-up-the-azure-cross-platform-cli"></a>Konfigurowanie wieloplatformowego interfejsu wiersza polecenia Azure
[Wieloplatformowy interfejs wiersza polecenia platformy Azure][azure-xplat-cli-github] zawiera polecenia służące do interakcji z jednostkami usługi Service Fabric, w tym klastrami i aplikacjami. Jest on oparty na języku Node.js, więc [upewnij się, że zainstalowano środowisko Node][install-node] przed dalszym wykonywaniem następujących instrukcji:

1. Sklonuj repozytorium Github na maszynie deweloperskiej.

    ```bash
    git clone https://github.com/Azure/azure-xplat-cli.git
    ```
2. Przełącz się na sklonowane repozytorium i zainstaluj zależności interfejsu wiersza polecenia za pomocą narzędzia Node Package Manager (npm).

    ```bash
    cd azure-xplat-cli
    npm install
    ```
3. Utwórz link symboliczny z folderu bin/azure sklonowanego repozytorium do katalogu /usr/bin/azure, aby zostało dodane do ścieżki, a polecenia były dostępne z dowolnego katalogu.

    ```bash
    sudo ln -s $(pwd)/bin/azure /usr/bin/azure
    ```
4. Na koniec włącz automatyczne uzupełnianie poleceń usługi Service Fabric.

    ```bash
    azure --completion >> ~/azure.completion.sh
    echo 'source ~/azure.completion.sh' >> ~/.bash_profile
    source ~/azure.completion.sh
    ```

> [!NOTE]
> Polecenia usługi Service Fabric nie są jeszcze dostępne w usłudze Azure CLI 2.0.

## <a name="set-up-a-local-cluster"></a>Tworzenie klastra lokalnego
Jeśli wszystko zostało zainstalowane pomyślnie, powinno być możliwe uruchomienie klastra lokalnego.

1. Uruchom skrypt instalacji klastra.

    ```bash
    sudo /opt/microsoft/sdk/servicefabric/common/clustersetup/devclustersetup.sh
    ```
2. Otwórz przeglądarkę internetową i przejdź na adres http://localhost:19080/Explorer. Jeśli klaster został uruchomiony, powinien być widoczny pulpit nawigacyjny narzędzia Service Fabric Explorer.

    ![Narzędzie Service Fabric Explorer w systemie Linux][sfx-linux]

W tym momencie możliwe jest wdrożenie wstępnie skompilowanych pakietów aplikacji usługi Service Fabric lub nowych pakietów opartych na kontenerach gościa lub plikach wykonywalnych gościa. Aby tworzyć nowe usługi przy użyciu zestawów SDK platformy Java lub .NET Core, wykonaj opcjonalne kroki instalacji opisane w kolejnych sekcjach.


> [!NOTE]
> Klastry autonomiczne nie są obsługiwane w systemie Linux — w wersji zapoznawczej są obsługiwane tylko pojedyncze maszyny i klastry obejmujące wiele maszyn platformy Azure z systemem Linux.
>
>

## <a name="install-the-java-sdk-and-eclipse-neon-plugin-optional"></a>Instalowanie zestawu Java SDK i wtyczki środowiska Eclipse Neon (opcjonalnie)
Zestaw Java SDK udostępnia biblioteki i szablony wymagane do kompilowania usług Service Fabric przy użyciu platformy Java.

1. Zainstaluj pakiet Java SDK.

    ```bash
    sudo apt-get install servicefabricsdkjava
    ```
2. Uruchom skrypt instalacji zestawu SDK.

    ```bash
    sudo /opt/microsoft/sdk/servicefabric/java/sdkjavasetup.sh
    ```

Wtyczkę środowiska Eclipse dla usługi Service Fabric można zainstalować z poziomu środowiska IDE Eclipse Neon.

1. W środowisku Eclipse upewnij się, że zainstalowany został zestaw Buildship w wersji 1.0.17 lub nowszej. Wersje zainstalowanych składników można sprawdzić, wybierając pozycję **Help > Installation Details** (Pomoc > Szczegóły instalacji). Zestaw Buildship można zaktualizować, postępując zgodnie z instrukcjami znajdującymi się [tutaj][buildship-update].
2. Aby zainstalować wtyczkę usługi Service Fabric, wybierz pozycję **Help > Install New Software** (Pomoc > Zainstaluj nowe oprogramowanie...).
3. W polu tekstowym „Work with” (Pracuj z) wprowadź http://dl.windowsazure.com/eclipse/servicefabric
4. Kliknij pozycję Add (Dodaj).

    ![Wtyczka środowiska Eclipse][sf-eclipse-plugin]
5. Wybierz wtyczkę usługi Service Fabric i kliknij przycisk Next (Dalej).
6. Postępuj zgodnie z instrukcjami instalacji i zaakceptuj umowę licencyjną użytkownika końcowego.

## <a name="install-the-net-core-sdk-optional"></a>Instalowanie zestawu SDK platformy .NET Core (opcjonalnie)
Zestaw SDK platformy .NET Core udostępnia biblioteki i szablony wymagane do kompilowania usług Service Fabric przy użyciu wieloplatformowego środowiska .NET Core.

1. Zainstaluj zestaw SDK platformy .NET Core.

   ```bash
   sudo apt-get install servicefabricsdkcsharp
   ```

2. Uruchom skrypt instalacji zestawu SDK.

   ```bash
   sudo /opt/microsoft/sdk/servicefabric/csharp/sdkcsharpsetup.sh
   ```

## <a name="updating-the-sdk-and-runtime"></a>Aktualizowanie zestawu SDK i środowiska uruchomieniowego

Aby zaktualizować zestaw SDK i środowisko uruchomieniowe do najnowszej wersji, wykonaj następujące czynności (usuń z listy zestawy SDK, których nie chcesz aktualizować ani instalować):

   ```bash
   sudo apt-get update
   sudo apt-get install servicefabric, servicefabricsdkcommon, servicefabricsdkcsharp, servicefabricsdkjava
   ```

Aby zaktualizować interfejs wiersza polecenia, przejdź do katalogu, w którym sklonowano interfejs wiersza polecenia, i uruchom polecenie `git pull` w celu rozpoczęcia aktualizacji.

## <a name="next-steps"></a>Następne kroki
* [Tworzenie pierwszej aplikacji Java w systemie Linux](service-fabric-create-your-first-linux-application-with-java.md)
* [Create your first CSharp application on Linux](service-fabric-create-your-first-linux-application-with-csharp.md) (Tworzenie pierwszej aplikacji CSharp w systemie Linux)
* [Przygotowywanie środowiska projektowego w systemie OSX](service-fabric-get-started-mac.md)
* [Use the Azure CLI to manage your Service Fabric applications](service-fabric-azure-cli.md) (Używanie interfejsu wiersza polecenia platformy Azure do zarządzania aplikacjami usługi Service Fabric)

<!-- Links -->

[azure-xplat-cli-github]: https://github.com/Azure/azure-xplat-cli
[install-node]: https://nodejs.org/en/download/package-manager/#installing-node-js-via-package-manager
[buildship-update]: https://projects.eclipse.org/projects/tools.buildship

<!--Images -->

[sf-eclipse-plugin]: ./media/service-fabric-get-started-linux/service-fabric-eclipse-plugin.png
[sfx-linux]: ./media/service-fabric-get-started-linux/sfx-linux.png



<!--HONumber=Feb17_HO3-->


