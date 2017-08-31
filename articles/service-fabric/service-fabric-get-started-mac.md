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
ms.date: 08/21/2017
ms.author: saysa
ms.translationtype: HT
ms.sourcegitcommit: cf381b43b174a104e5709ff7ce27d248a0dfdbea
ms.openlocfilehash: 8b4fc0ab9034263418cac42ced203035e0a8fcad
ms.contentlocale: pl-pl
ms.lasthandoff: 08/23/2017

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
Usługa Service Fabric nie działa natywnie w systemie OS X. Aby uruchomić lokalny klaster usługi Service Fabric, udostępniamy wstępnie skonfigurowaną maszynę wirtualną z systemem Ubuntu przy użyciu narzędzia Vagrant i VirtualBox. Przed rozpoczęciem potrzebne są następujące elementy:

* [Vagrant (wersja 1.8.4 lub nowsza)](http://www.vagrantup.com/downloads.html)
* [VirtualBox](http://www.virtualbox.org/wiki/Downloads)

>[!NOTE]
> Należy użyć wzajemnie obsługiwanych wersji programów Vagrant i VirtualBox. Program Vagrant może zachowywać się niestabilnie w przypadku nieobsługiwanej wersji programu VirtualBox.
>

## <a name="create-the-local-vm"></a>Tworzenie lokalnej maszyny wirtualnej
Aby utworzyć lokalną maszynę wirtualną zawierającą pięciowęzłowy klaster usługi Service Fabric, wykonaj następujące kroki:

1. Sklonuj repozytorium `Vagrantfile`

    ```bash
    git clone https://github.com/azure/service-fabric-linux-vagrant-onebox.git
    ```
    Ten krok spowoduje pobranie pliku `Vagrantfile` zawierającego konfigurację maszyny wirtualnej wraz z lokalizacją, z której jest pobierana maszyna wirtualna.

2. Przejdź do lokalnego klona repozytorium.

    ```bash
    cd service-fabric-linux-vagrant-onebox
    ```
3. (Opcjonalnie) Zmodyfikuj domyślne ustawienia maszyny wirtualnej.

    Domyślnie lokalna maszyna wirtualna jest skonfigurowana w następujący sposób:

   * 3 GB przydzielonej pamięci
   * Sieć hosta prywatnego skonfigurowana pod adresem IP 192.168.50.50 umożliwiająca przekazywanie ruchu z hosta komputera Mac

     Dowolne z tych ustawień można zmienić lub można dodać inną konfigurację maszyny wirtualnej w pliku `Vagrantfile`. Zobacz [dokumentację narzędzia Vagrant](http://www.vagrantup.com/docs), aby uzyskać pełną listę opcji konfiguracji.
4. Tworzenie maszyny wirtualnej

    ```bash
    vagrant up
    ```

   W trakcie wykonywania tego kroku zostanie pobrany wstępnie skonfigurowany obraz maszyny, będzie miał miejsce jego rozruch lokalny, a następnie w ramach tego obrazu zostanie przeprowadzona konfiguracja lokalnego klastra usługi Service Fabric. Powinno to potrwać kilka minut. Jeśli konfiguracja zakończy się pomyślnie, jako wynik zostanie wyświetlony komunikat, że klaster jest uruchamiany.

    ![Uruchomienie konfiguracji klastra po aprowizacji maszyny wirtualnej][cluster-setup-script]

    >[!TIP]
    > Jeśli pobieranie maszyny wirtualnej trwa długo, możesz ją pobrać przy użyciu polecenia wget lub curl albo za pośrednictwem przeglądarki, korzystając z linku podanego we właściwości **config.vm.box_url** w pliku `Vagrantfile`. Po pobraniu do środowiska lokalnego edytuj plik `Vagrantfile`, aby wskazać ścieżkę lokalną, do której pobrano obraz. Jeśli na przykład obraz pobrano do katalogu /home/users/test/azureservicefabric.tp8.box, zmień wartość **config.vm.box_url** na tę ścieżkę.
    >

5. Sprawdź, czy klaster został poprawnie skonfigurowany, przechodząc do narzędzia Service Fabric Explorer pod adresem http://192.168.50.50:19080/Explorer (przy założeniu, że pozostawiono domyślny prywatny adres IP).

    ![Narzędzie Service Fabric Explorer wyświetlane z komputera Mac hosta][sfx-mac]


## <a name="create-application-on-mac-using-yeoman"></a>Tworzenie aplikacji na komputerze Mac przy użyciu narzędzia Yeoman
Usługa Service Fabric udostępnia narzędzia do tworzenia szkieletów, które ułatwiają tworzenie aplikacji usługi Service Fabric z poziomu terminalu przy użyciu generatora szablonów narzędzia Yeoman. Wykonaj poniższe kroki, aby upewnić się, że masz generator szablonów narzędzia Yeoman szablonu usługi Service Fabric działający na Twoim komputerze.

1. Na komputerze Mac musi być zainstalowane środowisko Node.js i narzędzie NPM. Jeśli jeszcze nie zostało to zrobione, możesz zainstalować środowisko Node.js i narzędzie NPM przy użyciu programu Homebrew, uruchamiając poniższe polecenie. Aby sprawdzić wersje środowiska Node.js i narzędzia NPM zainstalowane na komputerze Mac, możesz użyć opcji ``-v``.

  ```bash
  brew install node
  node -v
  npm -v
  ```
2. Zainstaluj generator szablonów [narzędzia Yeoman](http://yeoman.io/) na swoim komputerze z poziomu narzędzia NPM

  ```bash
  npm install -g yo
  ```
3. Zainstaluj generator Yeoman, którego chcesz używać, zgodnie z instrukcjami podanymi w [dokumentacji](service-fabric-get-started-linux.md) wprowadzającej. Aby utworzyć aplikacje usługi Service Fabric za pomocą narzędzia Yeoman, wykonaj kroki -

  ```bash
  npm install -g generator-azuresfjava       # for Service Fabric Java Applications
  npm install -g generator-azuresfguest      # for Service Fabric Guest executables
  npm install -g generator-azuresfcontainer  # for Service Fabric Container Applications
  ```
4. Aby skompilować aplikację Java usługi Service Fabric na komputerze Mac, będziesz potrzebować zestawu JDK 1.8 i narzędzia Gradle zainstalowanych na komputerze.


## <a name="install-the-service-fabric-plugin-for-eclipse-neon"></a>Instalowanie wtyczki usługi Service Fabric dla środowiska Eclipse Neon

Usługa Service Fabric udostępnia wtyczkę dla **środowiska IDE Eclipse Neon dla języka Java**, która upraszcza proces tworzenia, kompilowania i wdrażania usług Java. Możesz wykonać kroki instalacji wymienione w ogólnej [dokumentacji](service-fabric-get-started-eclipse.md#install-or-update-the-service-fabric-plug-in-in-eclipse-neon) dotyczącej instalowania lub aktualizowania wtyczki usługi Service Fabric dla środowiska Eclipse.

>[!TIP]
> Automatycznie obsługujemy domyślny adres IP zgodnie z opisem w pliku ``Vagrantfile`` w pliku ``Local.json`` wygenerowanej aplikacji. Jeśli to zmienisz i wdrożysz program Vagrant z innym adresem IP, zaktualizuj odpowiedni adres IP również w pliku ``Local.json`` aplikacji.

## <a name="next-steps"></a>Następne kroki
<!-- Links -->
* [Create and deploy your first Service Fabric Java application on Linux using Yeoman](service-fabric-create-your-first-linux-application-with-java.md) (Tworzenie i wdrażanie pierwszej aplikacji Java usługi Service Fabric w systemie Linux przy użyciu programu Yeoman)
* [Create and deploy your first Service Fabric Java application on Linux using Service Fabric Plugin for Eclipse](service-fabric-get-started-eclipse.md) (Tworzenie i wdrażanie pierwszej aplikacji Java usługi Service Fabric w systemie Linux przy użyciu wtyczki usługi Service Fabric dla środowiska Eclipse)
* [Tworzenie klastra usługi Service Fabric w witrynie Azure Portal](service-fabric-cluster-creation-via-portal.md)
* [Tworzenie klastra usługi Service Fabric przy użyciu usługi Azure Resource Manager](service-fabric-cluster-creation-via-arm.md)
* [Informacje o modelu aplikacji usługi Service Fabric](service-fabric-application-model.md)

<!-- Images -->
[cluster-setup-script]: ./media/service-fabric-get-started-mac/cluster-setup-mac.png
[sfx-mac]: ./media/service-fabric-get-started-mac/sfx-mac.png
[sf-eclipse-plugin-install]: ./media/service-fabric-get-started-mac/sf-eclipse-plugin-install.png
[buildship-update]: https://projects.eclipse.org/projects/tools.buildship

