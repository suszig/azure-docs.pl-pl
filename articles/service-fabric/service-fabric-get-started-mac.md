---
title: "Konfigurowanie środowiska projektowego w systemie Mac OS X | Microsoft Docs"
description: "Zainstaluj środowisko uruchomieniowe, zestaw SDK i narzędzia oraz utwórz lokalny klaster projektowy. Po ukończeniu tej konfiguracji wszystko będzie gotowe do kompilowania aplikacji w systemie Mac OS X."
services: service-fabric
documentationcenter: .net
author: seanmck
manager: timlt
editor: 
ms.assetid: bf84458f-4b87-4de1-9844-19909e368deb
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 12/27/2016
ms.author: seanmck
translationtype: Human Translation
ms.sourcegitcommit: d5a04e34a3f46097b3ad1b04ac6b2b845fc946af
ms.openlocfilehash: beb869fd5c1fcc19101b16149244187c37a6d6a3


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

## <a name="create-the-local-vm"></a>Tworzenie lokalnej maszyny wirtualnej
Aby utworzyć lokalną maszynę wirtualną zawierającą pięciowęzłowy klaster usługi Service Fabric, wykonaj następujące kroki:

1. Sklonuj repozytorium **Vagrantfile**.
   
    ```bash
    git clone https://github.com/azure/service-fabric-linux-vagrant-onebox.git
    ```
2. Przejdź do lokalnego klona repozytorium.

    ```bash
    cd service-fabric-linux-vagrant-onebox
    ```
3. (Opcjonalnie) Zmodyfikuj domyślne ustawienia maszyny wirtualnej.

    Domyślnie lokalna maszyna wirtualna jest skonfigurowana w następujący sposób:

   * 3 GB przydzielonej pamięci
   * Sieć hosta prywatnego skonfigurowana pod adresem IP 192.168.50.50 umożliwiająca przekazywanie ruchu z hosta komputera Mac

     Można zmienić dowolne z tych ustawień lub wprowadzić inną konfigurację maszyny wirtualnej w repozytorium Vagrantfile. Zobacz [dokumentację narzędzia Vagrant](http://www.vagrantup.com/docs), aby uzyskać pełną listę opcji konfiguracji.
4. Tworzenie maszyny wirtualnej

    ```bash
    vagrant up
    ```
  
   W trakcie wykonywania tego kroku zostanie pobrany wstępnie skonfigurowany obraz maszyny, będzie miał miejsce jego rozruch lokalny, a następnie w ramach tego obrazu zostanie przeprowadzona konfiguracja lokalnego klastra usługi Service Fabric. Powinno to potrwać kilka minut. Jeśli konfiguracja zakończy się pomyślnie, jako wynik zostanie wyświetlony komunikat, że klaster jest uruchamiany.
   

    ![Uruchomienie konfiguracji klastra po aprowizacji maszyny wirtualnej][cluster-setup-script]
5. Sprawdź, czy klaster został poprawnie skonfigurowany, przechodząc do narzędzia Service Fabric Explorer pod adresem http://192.168.50.50:19080/Explorer (przy założeniu, że pozostawiono domyślny prywatny adres IP).

    ![Narzędzie Service Fabric Explorer wyświetlane z komputera Mac hosta][sfx-mac]

## <a name="install-the-service-fabric-plugin-for-eclipse-neon-optional"></a>Instalowanie wtyczki usługi Service Fabric dla środowiska Eclipse Neon (opcjonalnie)
Usługa Service Fabric udostępnia wtyczkę dla środowiska IDE Eclipse Neon, która upraszcza proces kompilowania i wdrażania usług Java.

1. W środowisku Eclipse upewnij się, że zainstalowany został zestaw Buildship w wersji 1.0.17 lub nowszej. Wersje zainstalowanych składników można sprawdzić, wybierając pozycję **Help > Installation Details** (Pomoc > Szczegóły instalacji). Zestaw Buildship można zaktualizować, postępując zgodnie z instrukcjami znajdującymi się [tutaj][buildship-update].
2. Aby zainstalować wtyczkę usługi Service Fabric, wybierz pozycję **Help > Install New Software** (Pomoc > Zainstaluj nowe oprogramowanie...).
3. W polu tekstowym „Work with” (Pracuj z) wprowadź http://dl.windowsazure.com/eclipse/servicefabric.
4. Kliknij pozycję Add (Dodaj).

    ![Wtyczka środowiska Eclipse Neon dla usługi Service Fabric][sf-eclipse-plugin-install]
5. Wybierz wtyczkę usługi Service Fabric i kliknij przycisk Next (Dalej).
6. Postępuj zgodnie z instrukcjami instalacji i zaakceptuj umowę licencyjną użytkownika końcowego.

## <a name="next-steps"></a>Następne kroki
<!-- Links -->

* [Tworzenie pierwszej aplikacji usługi Service Fabric dla systemu Linux](service-fabric-create-your-first-linux-application-with-java.md)
* [Tworzenie klastra usługi Service Fabric w witrynie Azure Portal](service-fabric-cluster-creation-via-portal.md)
* [Tworzenie klastra usługi Service Fabric przy użyciu usługi Azure Resource Manager](service-fabric-cluster-creation-via-arm.md)
* [Informacje o modelu aplikacji usługi Service Fabric](service-fabric-application-model.md)

<!-- Images -->
[cluster-setup-script]: ./media/service-fabric-get-started-mac/cluster-setup-mac.png
[sfx-mac]: ./media/service-fabric-get-started-mac/sfx-mac.png
[sf-eclipse-plugin-install]: ./media/service-fabric-get-started-mac/sf-eclipse-plugin-install.png
[buildship-update]: https://projects.eclipse.org/projects/tools.buildship



<!--HONumber=Jan17_HO1-->


