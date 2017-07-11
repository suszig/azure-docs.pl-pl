---
title: "Konfigurowanie środowiska projektowego w systemie Linux | Microsoft Docs"
description: "Zainstaluj środowisko uruchomieniowe i zestaw SDK oraz utwórz lokalny klaster projektowy w systemie Linux. Po ukończeniu tej konfiguracji wszystko będzie gotowe do tworzenia aplikacji."
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: d552c8cd-67d1-45e8-91dc-871853f44fc6
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 6/28/2017
ms.author: subramar
ms.translationtype: Human Translation
ms.sourcegitcommit: 6efa2cca46c2d8e4c00150ff964f8af02397ef99
ms.openlocfilehash: 5840932d79ef9fd99a94eb6ae4e587b0e616065e
ms.contentlocale: pl-pl
ms.lasthandoff: 07/01/2017


---
<a id="prepare-your-development-environment-on-linux" class="xliff"></a>

# Przygotowywanie środowiska projektowego w systemie Linux
> [!div class="op_single_selector"]
> * [Windows](service-fabric-get-started.md)
> * [Linux](service-fabric-get-started-linux.md)
> * [OSX](service-fabric-get-started-mac.md)
>
>  

 Aby wdrażać i uruchamiać [aplikacje usługi Azure Service Fabric](service-fabric-application-model.md) na maszynie deweloperskiej z systemem Linux, należy zainstalować środowisko uruchomieniowe i wspólny zestaw SDK. Można także zainstalować opcjonalne zestawy SDK dla platformy Java i .NET Core.

<a id="prerequisites" class="xliff"></a>

## Wymagania wstępne

<a id="supported-operating-system-versions" class="xliff"></a>

### Obsługiwane wersje systemu operacyjnego
Na potrzeby tworzenia aplikacji obsługiwane są następujące wersje systemu operacyjnego:

* Ubuntu 16.04 (`Xenial Xerus`)

<a id="update-your-apt-sources" class="xliff"></a>

## Aktualizowanie źródeł APT
Aby zainstalować zestaw SDK i skojarzony pakiet środowiska uruchomieniowego przy użyciu polecenia apt-get, należy najpierw zaktualizować źródła APT.

1. Otwórz terminal.
2. Dodaj repozytorium usługi Service Fabric do listy źródeł.

    ```bash
    sudo sh -c 'echo "deb [arch=amd64] http://apt-mo.trafficmanager.net/repos/servicefabric/ trusty main" > /etc/apt/sources.list.d/servicefabric.list'
    ```

3. Dodaj repozytorium `dotnet` do listy źródeł.

    ```bash
    sudo sh -c 'echo "deb [arch=amd64] https://apt-mo.trafficmanager.net/repos/dotnet-release/ xenial main" > /etc/apt/sources.list.d/dotnetdev.list'
    ```

4. Dodaj nowy klucz GPG do pęku kluczy APT.

    ```bash
    sudo apt-key adv --keyserver apt-mo.trafficmanager.net --recv-keys 417A0893
    sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 417A0893
    ```

5. Dodaj oficjalny klucz GPG platformy Docker do pęku kluczy APT.

    ```bash
    sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
    ```

6. Skonfiguruj repozytorium platformy Docker.

    ```bash
    sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
    ```

7. Odśwież listę pakietów na podstawie nowo dodanych repozytoriów.

    ```bash
    sudo apt-get update
    ```

<a id="install-and-set-up-the-sdk-for-containers-and-guest-executables" class="xliff"></a>

## Instalowanie i konfigurowanie zestawu SDK dla kontenerów i plików wykonywalnych gościa

Po zaktualizowaniu źródeł można zainstalować zestaw SDK.

1. Zainstaluj pakiet zestawu SDK usługi Service Fabric. Zostanie wyświetlona prośba o potwierdzenie instalacji i zaakceptowanie umowy licencyjnej.

    ```bash
    sudo apt-get install servicefabricsdkcommon
    ```

    >   [!TIP]
    >   Poniższe polecenia pozwalają zautomatyzować akceptowanie postanowień licencji dla pakietów usługi Service Fabric:
    >   ```bash
    >   echo "servicefabric servicefabric/accepted-eula-v1 select true" | debconf-set-selections
    >   echo "servicefabricsdkcommon servicefabricsdkcommon/accepted-eula-v1 select true" | debconf-set-selections
    >   ```
    
2. Uruchom skrypt instalacji zestawu SDK.

    ```bash
    sudo /opt/microsoft/sdk/servicefabric/common/sdkcommonsetup.sh
    ```

Po wykonaniu procedury instalacji wspólnego pakietu SDK powinno być możliwe tworzenie aplikacji z plikiem wykonywalnym gościa lub usług kontenerów przez uruchomienie polecenia `yo azuresfguest` lub `yo azuresfcontainer`. Może być konieczne ustawienie zmiennej środowiskowej **$NODE_PATH** na lokalizację modułów węzła. 


```bash
    export NODE_PATH=$NODE_PATH:$HOME/.node/lib/node_modules 
```

Jeśli używasz środowiska jako użytkownik główny, w celu ustawienia zmiennej może być konieczne użycie następującego polecenia:

```bash
    export NODE_PATH=$NODE_PATH:/root/.node/lib/node_modules 
```


> [!TIP]
> Aby nie trzeba było ustawiać zmiennej środowiskowej po każdym logowaniu, możesz dodać te polecenia do pliku ~/.bashrc.
>

<a id="setup-the-xplat-service-fabric-cli" class="xliff"></a>

## Konfigurowanie interfejsu wiersza polecenia XPlat usługi Service Fabric
[Interfejs wiersza polecenia XPlat][azure-xplat-cli-github] zawiera polecenia służące do interakcji z jednostkami usługi Service Fabric, w tym klastrami i aplikacjami. Jest on oparty na języku Node.js, więc [upewnij się, że zainstalowano środowisko Node][install-node] przed dalszym wykonywaniem następujących instrukcji:

1. Sklonuj repozytorium Github na maszynie deweloperskiej.

    ```bash
    git clone https://github.com/Azure/azure-xplat-cli.git
    ```

2. Przełącz się na sklonowane repozytorium i zainstaluj zależności interfejsu wiersza polecenia za pomocą narzędzia Node Package Manager (npm).

    ```bash
    cd azure-xplat-cli
    npm install
    ```

3. Utwórz link symboliczny z folderu `bin/azure` sklonowanego repozytorium do folderu `/usr/bin/azure`.

    ```bash
    sudo ln -s $(pwd)/bin/azure /usr/bin/azure
    ```

4. Na koniec włącz automatyczne uzupełnianie poleceń usługi Service Fabric.

    ```bash
    azure --completion >> ~/azure.completion.sh
    echo 'source ~/azure.completion.sh' >> ~/.bash_profile
    source ~/azure.completion.sh
    ```

<a id="setup-azure-cli-20" class="xliff"></a>

### Konfigurowanie interfejsu wiersza polecenia platformy Azure 2.0

Alternatywą dla interfejsu wiersza polecenia XPlat jest teraz moduł poleceń usługi Service Fabric zawarty w interfejsie wiersza polecenia platformy Azure.

Więcej informacji na temat instalowania interfejsu wiersza polecenia platformy Azure 2.0 i używania poleceń usługi Service Fabric można znaleźć w [dokumentacji wprowadzającej](service-fabric-azure-cli-2-0.md).

<a id="set-up-a-local-cluster" class="xliff"></a>

## Tworzenie klastra lokalnego
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

<a id="install-the-java-sdk-optional-if-you-wish-to-use-the-java-programming-models" class="xliff"></a>

## Instalowanie zestawu SDK Java (opcjonalnie, jeśli chcesz używać modeli programowania Java)
Zestaw Java SDK udostępnia biblioteki i szablony wymagane do kompilowania usług Service Fabric przy użyciu platformy Java.

1. Zainstaluj pakiet Java SDK.

    ```bash
    sudo apt-get install servicefabricsdkjava
    ```

2. Uruchom skrypt instalacji zestawu SDK.

    ```bash
    sudo /opt/microsoft/sdk/servicefabric/java/sdkjavasetup.sh
    ```

<a id="install-the-eclipse-neon-plugin-optional" class="xliff"></a>

## Instalowanie wtyczki środowiska Eclipse Neon (opcjonalnie)

Wtyczkę środowiska Eclipse dla usługi Service Fabric można zainstalować z poziomu środowiska **Eclipse IDE for Java Developers**. Za pomocą środowiska Eclipse można tworzyć aplikacje wykonywalne gościa i aplikacje kontenera usługi Service Fabric, a także aplikacje Java usługi Service Fabric.

> [!NOTE]
> Zestaw SDK platformy Java to warunek wstępny do korzystania z wtyczki środowiska Eclipse, nawet jeśli jest ona używana tylko na potrzeby aplikacji wykonywalnych gościa i aplikacji kontenera.
>

1. W środowisku Eclipse upewnij się, że masz zainstalowaną najnowszą wersję środowiska Eclipse **Neon** i zestawu Buildship (1.0.17 lub nowszą). Wersje zainstalowanych składników można sprawdzić, wybierając pozycję **Help > Installation Details** (Pomoc > Szczegóły instalacji). Zestaw Buildship można zaktualizować, postępując zgodnie z instrukcjami znajdującymi się [tutaj][buildship-update].
2. Aby zainstalować wtyczkę usługi Service Fabric, wybierz pozycję **Help > Install New Software** (Pomoc > Zainstaluj nowe oprogramowanie...).
3. W polu tekstowym „Work with” (Pracuj z) wprowadź adres: http://dl.microsoft.com/eclipse
4. Kliknij pozycję Add (Dodaj).

    ![Wtyczka środowiska Eclipse][sf-eclipse-plugin]

5. Wybierz wtyczkę usługi Service Fabric i kliknij przycisk **Next** (Dalej).
6. Postępuj zgodnie z instrukcjami instalacji i zaakceptuj umowę licencyjną użytkownika końcowego.

Jeśli wtyczka środowiska Eclipse dla usługi Service Fabric jest już zainstalowana, upewnij się, że używasz najnowszej wersji. W tym celu wybierz opcję ``Help => Installation Details`` (Pomoc — Szczegóły instalacji) i wyszukaj pozycję Service Fabric na liście zainstalowanych wtyczek. Wybierz opcję aktualizacji, jeśli jest dostępna nowsza wersja.

Aby uzyskać więcej informacji, zobacz [Wprowadzenie do korzystania z usługi Service Fabric w środowisku Eclipse](service-fabric-get-started-eclipse.md).


<a id="install-the-net-core-sdk-optional-if-you-wish-to-use-the-net-core-programming-models" class="xliff"></a>

## Instalowanie zestawu SDK platformy .NET Core (opcjonalnie, jeśli chcesz używać modeli programowania .NET Core)
Zestaw SDK platformy .NET Core udostępnia biblioteki i szablony wymagane do kompilowania usług Service Fabric przy użyciu środowiska .NET Core.

1. Zainstaluj zestaw SDK platformy .NET Core.

   ```bash
   sudo apt-get install servicefabricsdkcsharp
   ```

2. Uruchom skrypt instalacji zestawu SDK.

   ```bash
   sudo /opt/microsoft/sdk/servicefabric/csharp/sdkcsharpsetup.sh
   ```

<a id="updating-the-sdk-and-runtime" class="xliff"></a>

## Aktualizowanie zestawu SDK i środowiska uruchomieniowego

Aby zaktualizować zestaw SDK i środowisko uruchomieniowe do najnowszej wersji, uruchom następujące polecenia (usuń zaznaczenie zestawów SDK, których nie chcesz aktualizować ani instalować):

```bash
sudo apt-get update
sudo apt-get install servicefabric servicefabricsdkcommon servicefabricsdkcsharp servicefabricsdkjava
```


> [!NOTE]
> Zaktualizowanie pakietów może spowodować zatrzymanie lokalnego klastra projektowego. Po uaktualnieniu uruchom ponownie klaster lokalny, postępując zgodnie z instrukcjami opisanymi na tej stronie.

<a id="next-steps" class="xliff"></a>

## Następne kroki
* [Create and deploy your first Service Fabric Java application on Linux using Yeoman](service-fabric-create-your-first-linux-application-with-java.md) (Tworzenie i wdrażanie pierwszej aplikacji Java usługi Service Fabric w systemie Linux przy użyciu programu Yeoman)
* [Create and deploy your first Service Fabric Java application on Linux using Service Fabric Plugin for Eclipse](service-fabric-get-started-eclipse.md) (Tworzenie i wdrażanie pierwszej aplikacji Java usługi Service Fabric w systemie Linux przy użyciu wtyczki usługi Service Fabric dla środowiska Eclipse)
* [Create your first CSharp application on Linux](service-fabric-create-your-first-linux-application-with-csharp.md) (Tworzenie pierwszej aplikacji CSharp w systemie Linux)
* [Przygotowywanie środowiska projektowego w systemie OSX](service-fabric-get-started-mac.md)
* [Use the XPlat CLI to manage your Service Fabric applications](service-fabric-azure-cli.md) (Zarządzanie aplikacjami usługi Service Fabric przy użyciu interfejsu wiersza polecenia XPlat)
* [Różnice w usłudze Service Fabric w systemie Windows i Linux](service-fabric-linux-windows-differences.md)

<a id="related-articles" class="xliff"></a>

## Pokrewne artykuły:

* [Getting started with Service Fabric and Azure CLI 2.0](service-fabric-azure-cli-2-0.md) (Wprowadzenie do usługi Service Fabric i interfejsu wiersza polecenia platformy Azure 2.0)
* [Getting started with Service Fabric XPlat CLI](service-fabric-azure-cli.md) (Wprowadzenie do interfejsu wiersza polecenia XPlat usługi Service Fabric)

<!-- Links -->

[azure-xplat-cli-github]: https://github.com/Azure/azure-xplat-cli
[install-node]: https://nodejs.org/en/download/package-manager/#installing-node-js-via-package-manager
[buildship-update]: https://projects.eclipse.org/projects/tools.buildship

<!--Images -->

[sf-eclipse-plugin]: ./media/service-fabric-get-started-linux/service-fabric-eclipse-plugin.png
[sfx-linux]: ./media/service-fabric-get-started-linux/sfx-linux.png

