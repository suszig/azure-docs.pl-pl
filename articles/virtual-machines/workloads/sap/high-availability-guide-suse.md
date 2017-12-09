---
title: "Azure maszyny wirtualnej wysoką dostępność dla programu SAP NetWeaver w systemie SUSE Linux Enterprise Server SAP aplikacji | Dokumentacja firmy Microsoft"
description: "Przewodnik wysokiej dostępności dla programu SAP NetWeaver w systemie SUSE Linux Enterprise Server dla programu SAP aplikacji"
services: virtual-machines-windows,virtual-network,storage
documentationcenter: saponazure
author: mssedusch
manager: timlt
editor: 
tags: azure-resource-manager
keywords: 
ms.assetid: 5e514964-c907-4324-b659-16dd825f6f87
ms.service: virtual-machines-windows
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 04/27/2017
ms.author: sedusch
ms.openlocfilehash: 609b811705bb6f116db055b756910450f8990528
ms.sourcegitcommit: 094061b19b0a707eace42ae47f39d7a666364d58
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/08/2017
---
# <a name="high-availability-for-sap-netweaver-on-azure-vms-on-suse-linux-enterprise-server-for-sap-applications"></a>Wysoka dostępność dla programu SAP NetWeaver na maszynach wirtualnych Azure w systemie SUSE Linux Enterprise Server dla programu SAP aplikacji

[dbms-guide]:dbms-guide.md
[deployment-guide]:deployment-guide.md
[planning-guide]:planning-guide.md

[2205917]:https://launchpad.support.sap.com/#/notes/2205917
[1944799]:https://launchpad.support.sap.com/#/notes/1944799
[1928533]:https://launchpad.support.sap.com/#/notes/1928533
[2015553]:https://launchpad.support.sap.com/#/notes/2015553
[2178632]:https://launchpad.support.sap.com/#/notes/2178632
[2191498]:https://launchpad.support.sap.com/#/notes/2191498
[2243692]:https://launchpad.support.sap.com/#/notes/2243692
[1984787]:https://launchpad.support.sap.com/#/notes/1984787
[1999351]:https://launchpad.support.sap.com/#/notes/1999351
[1410736]:https://launchpad.support.sap.com/#/notes/1410736

[sap-swcenter]:https://support.sap.com/en/my-support/software-downloads.html

[suse-hana-ha-guide]:https://www.suse.com/docrep/documents/ir8w88iwu7/suse_linux_enterprise_server_for_sap_applications_12_sp1.pdf
[suse-drbd-guide]:https://www.suse.com/documentation/sle-ha-12/singlehtml/book_sleha_techguides/book_sleha_techguides.html

[template-multisid-xscs]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-xscs-md%2Fazuredeploy.json
[template-converged]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-converged-md%2Fazuredeploy.json
[template-file-server]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-file-server-md%2Fazuredeploy.json

[sap-hana-ha]:sap-hana-high-availability.md

W tym artykule opisano sposób wdrażania maszyn wirtualnych, konfigurowanie maszyn wirtualnych, zainstalować w ramach klastra i instalowania systemu SAP NetWeaver 7.50 wysokiej dostępności.
W konfiguracji przykład polecenia instalacji itp. Liczby wystąpień ASCS 00 Wywołujących wystąpienia numer 02 oraz NWS identyfikator systemu SAP jest używany. Nazw zasobów (na przykład maszyn wirtualnych, sieci wirtualne), w tym przykładzie założono, że użyto [zbieżność szablonu] [ template-converged] systemie SAP NWS identyfikator na tworzenie zasobów.

Przeczytaj następujące uwagi SAP i dokumenty najpierw

* Uwaga SAP [1928533], który zawiera:
  * Lista rozmiary maszyn wirtualnych Azure, które są obsługiwane w przypadku wdrażania oprogramowania SAP
  * Informacje o rozmiary maszyn wirtualnych Azure ważne pojemności
  * Obsługiwane oprogramowanie SAP i kombinacji bazy danych i systemu operacyjnego (OS)
  * Wymagana wersja jądra SAP dla systemu Windows i Linux w systemie Microsoft Azure

* Uwaga SAP [2015553] wymieniono wymagania wstępne dotyczące wdrażania oprogramowania SAP SAP, obsługiwane na platformie Azure.
* Uwaga SAP [2205917] zawiera zalecane ustawienia systemu operacyjnego SUSE Linux Enterprise Server dla programu SAP aplikacji
* Uwaga SAP [1944799] ma SAP HANA wytyczne dla SUSE Linux Enterprise Server dla programu SAP aplikacji
* Uwaga SAP [2178632] zawiera szczegółowe informacje na temat wszystkie funkcje monitorowania metryki zgłoszone dla SAP na platformie Azure.
* Uwaga SAP [2191498] ma wymaganą wersję agenta hosta SAP dla systemu Linux na platformie Azure.
* Uwaga SAP [2243692] zawiera informacje o licencjonowaniu SAP w systemie Linux na platformie Azure.
* Uwaga SAP [1984787] zawiera ogólne informacje o systemie SUSE Linux Enterprise Server 12.
* Uwaga SAP [1999351] zawiera dodatkowe informacje dotyczące rozwiązywania problemów Azure rozszerzone monitorowanie rozszerzenia dla programu SAP.
* [SAP społeczności WIKI](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes) ma wszystkie wymagane informacje SAP dla systemu Linux.
* [Azure maszyn wirtualnych, planowania i wdrażania dla SAP w systemie Linux][planning-guide]
* [Maszyny wirtualne Azure wdrożenie SAP w systemie Linux (w tym artykule)][deployment-guide]
* [Azure maszyn wirtualnych systemu DBMS wdrożenie SAP w systemie Linux][dbms-guide]
* [SAP HANA SR zoptymalizowana scenariusza][suse-hana-ha-guide]  
  Przewodnik zawiera wszystkie informacje wymagane do skonfigurowania replikacji systemu SAP HANA lokalnymi. Użyj tego podręcznika, jako linii bazowej.
* [Wysoce dostępny magazyn systemu plików NFS z DRBD i rozrusznik] [ suse-drbd-guide] zawiera wszystkie wymagane informacje, aby skonfigurować serwer NFS o wysokiej dostępności. Użyj tego podręcznika, jako linii bazowej.


## <a name="overview"></a>Omówienie

Aby osiągnąć wysoką dostępność, SAP NetWeaver wymaga, aby serwer systemu plików NFS. Serwer systemu plików NFS jest skonfigurowany w osobnym klastrze i mogą być używane przez wiele systemów SAP.

![Informacje o SAP NetWeaver wysokiej dostępności](./media/high-availability-guide-suse/img_001.png)

Serwer systemu plików NFS, SAP NetWeaver ASCS SAP NetWeaver SCS, SAP NetWeaver Wywołujących i bazy danych SAP HANA używać nazwy hosta wirtualnego i wirtualnych adresów IP. Na platformie Azure usługi równoważenia obciążenia jest wymagany do użycia wirtualnego adresu IP. Poniższa lista przedstawia konfigurację usługi równoważenia obciążenia.

### <a name="nfs-server"></a>Serwer systemu plików NFS
* Konfiguracja serwera sieci Web
  * Adres IP 10.0.0.4
* Konfiguracja zaplecza
  * Połączone z interfejsów sieci podstawowej wszystkich maszyn wirtualnych, które powinny być częścią klastra systemu plików NFS
* Port sondy
  * Port 61000
* Reguły obciążenia
  * 2049 TCP 
  * 2049 UDP

### <a name="ascs"></a>(A) SCS
* Konfiguracja serwera sieci Web
  * Adres IP 10.0.0.10
* Konfiguracja zaplecza
  * Połączone z interfejsów sieci podstawowej wszystkich maszyn wirtualnych, które powinny być częścią (A) SCS/Wywołujących klastra
* Port sondy
  * Port 620**&lt;nr&gt;**
* Reguły obciążenia
  * 32**&lt;nr&gt;**  TCP
  * 36**&lt;nr&gt;**  TCP
  * 39**&lt;nr&gt;**  TCP
  * 81**&lt;nr&gt;**  TCP
  * 5**&lt;nr&gt;**13 TCP
  * 5**&lt;nr&gt;**14 TCP
  * 5**&lt;nr&gt;**16 TCP

### <a name="ers"></a>WYWOŁUJĄCYCH
* Konfiguracja serwera sieci Web
  * Adres IP 10.0.0.11
* Konfiguracja zaplecza
  * Połączone z interfejsów sieci podstawowej wszystkich maszyn wirtualnych, które powinny być częścią (A) SCS/Wywołujących klastra
* Port sondy
  * Port 621**&lt;nr&gt;**
* Reguły obciążenia
  * 33**&lt;nr&gt;**  TCP
  * 5**&lt;nr&gt;**13 TCP
  * 5**&lt;nr&gt;**14 TCP
  * 5**&lt;nr&gt;**16 TCP

### <a name="sap-hana"></a>SAP HANA
* Konfiguracja serwera sieci Web
  * Adres IP 10.0.0.12
* Konfiguracja zaplecza
  * Połączone z interfejsów sieci podstawowej wszystkich maszyn wirtualnych, które powinny być częścią klastra HANA
* Port sondy
  * Port 625**&lt;nr&gt;**
* Reguły obciążenia
  * 3**&lt;nr&gt;**15 TCP
  * 3**&lt;nr&gt;**17 TCP

## <a name="setting-up-a-highly-available-nfs-server"></a>Konfigurowanie serwera systemu plików NFS o wysokiej dostępności

### <a name="deploying-linux"></a>Wdrażanie systemu Linux

W portalu Azure Marketplace zawiera obraz systemu SUSE Linux Enterprise Server dla 12 aplikacje SAP, który służy do wdrażania nowych maszyn wirtualnych.
Umożliwia jednego z szablonów Szybki Start w serwisie github wdrażanie wszystkich wymaganych zasobów. Szablon wdraża maszyny wirtualne, usługi równoważenia obciążenia, dostępność ustawić itd. Wykonaj następujące kroki, aby wdrożyć szablon:

1. Otwórz [szablon serwera SAP pliku] [ template-file-server] w portalu Azure   
1. Wprowadź następujące parametry
   1. Prefiks zasobów  
      Wprowadź prefiks, który ma być używany. Wartość jest używana jako prefiksu dla zasobów, które zostały wdrożone.
   2. Typ systemu operacyjnego  
      Wybierz jedną z dystrybucje systemu Linux. Na przykład wybierz SLES 12.
   3. Nazwa użytkownika i hasło administratora  
      Tworzony jest nowy użytkownik, który może służyć do logowania się do komputera.
   4. Identyfikator podsieci  
      Identyfikator podsieci, do której maszyny wirtualne powinny być podłączone do. Pozostaw pole puste, jeśli chcesz utworzyć nową sieć wirtualną lub wybierz podsieć sieci VPN lub usługi Express Route wirtualnej sieci lokalnej nawiązać połączenia z maszyną wirtualną. Identyfikator zwykle wygląda /subscriptions/**&lt;identyfikator subskrypcji&gt;**/resourceGroups/**&lt;Nazwa grupy zasobów&gt;**/providers/ Microsoft.Network/virtualNetworks/**&lt;wirtualnej nazwy sieciowej&gt;**/subnets/**&lt;nazwy podsieci&gt;**

### <a name="installation"></a>Instalacja

Następujące elementy są poprzedzane prefiksem albo **[A]** — mające zastosowanie do wszystkich węzłów, **[1]** — dotyczy to tylko węzła 1 lub **[2]** — dotyczy to tylko węzeł 2.

1. **[A]**  Zaktualizować SLES

   <pre><code>
   sudo zypper update
   </code></pre>

1. **[1]**  Dostęp ssh

   <pre><code>
   sudo ssh-keygen -tdsa
   
   # Enter file in which to save the key (/root/.ssh/id_dsa): -> ENTER
   # Enter passphrase (empty for no passphrase): -> ENTER
   # Enter same passphrase again: -> ENTER
   
   # copy the public key
   sudo cat /root/.ssh/id_dsa.pub
   </code></pre>

2. **[2]**  Dostęp ssh

   <pre><code>
   sudo ssh-keygen -tdsa

   # insert the public key you copied in the last step into the authorized keys file on the second server
   sudo vi /root/.ssh/authorized_keys
   
   # Enter file in which to save the key (/root/.ssh/id_dsa): -> ENTER
   # Enter passphrase (empty for no passphrase): -> ENTER
   # Enter same passphrase again: -> ENTER
   
   # copy the public key   
   sudo cat /root/.ssh/id_dsa.pub
   </code></pre>

1. **[1]**  Dostęp ssh

   <pre><code>
   # insert the public key you copied in the last step into the authorized keys file on the first server
   sudo vi /root/.ssh/authorized_keys
   </code></pre>

1. **[A]**  Zainstalować HA rozszerzenia
   
   <pre><code>
   sudo zypper install sle-ha-release fence-agents
   </code></pre>

1. **[A]**  Instalatora rozpoznawania nazwy hosta   

   Można użyć serwera DNS lub zmodyfikować/etc/hosts na wszystkich węzłach. Ten przykład przedstawia sposób użycia pliku/etc/hosts.
   Zastąp adres IP i nazwy hosta w poniższych poleceniach

   <pre><code>
   sudo vi /etc/hosts
   </code></pre>
   
   Wstaw następujące wiersze do/etc/hosts. Zmienianie adresu IP i nazwy hosta do danego środowiska   
   
   <pre><code>
   # IP address of the load balancer frontend configuration for NFS
   <b>10.0.0.4 nws-nfs</b>
   </code></pre>

1. **[1]**  Instalacji klastra
   
   <pre><code>
   sudo ha-cluster-init
   
   # Do you want to continue anyway? [y/N] -> y
   # Network address to bind to (for example: 192.168.1.0) [10.79.227.0] -> ENTER
   # Multicast address (for example: 239.x.x.x) [239.174.218.125] -> ENTER
   # Multicast port [5405] -> ENTER
   # Do you wish to use SBD? [y/N] -> N
   # Do you wish to configure an administration IP? [y/N] -> N
   </code></pre>

1. **[2]**  Dodaj węzeł do klastra
   
   <pre><code> 
   sudo ha-cluster-join

   # WARNING: NTP is not configured to start at system boot.
   # WARNING: No watchdog device found. If SBD is used, the cluster will be unable to start without a watchdog.
   # Do you want to continue anyway? [y/N] -> y
   # IP address or hostname of existing node (for example: 192.168.1.1) [] -> IP address of node 1 for example 10.0.0.10
   # /root/.ssh/id_dsa already exists - overwrite? [y/N] N
   </code></pre>

1. **[A]**  Zmień hacluster hasło do tego samego hasła

   <pre><code> 
   sudo passwd hacluster
   </code></pre>

1. **[A]**  Skonfigurować corosync Użyj innego transportu, a następnie dodaj wstawienia. Klaster nie działa, w przeciwnym razie wartość.
   
   <pre><code> 
   sudo vi /etc/corosync/corosync.conf   
   </code></pre>

   Dodaj następującą zawartość pogrubienie do pliku.
   
   <pre><code> 
   [...]
     interface { 
        [...] 
     }
     <b>transport:      udpu</b>
   } 
   <b>nodelist {
     node {
      # IP address of <b>prod-nfs-0</b>
      ring0_addr:10.0.0.5
     }
     node {
      # IP address of <b>prod-nfs-1</b>
      ring0_addr:10.0.0.6
     } 
   }</b>
   logging {
     [...]
   </code></pre>

   Następnie uruchom ponownie usługę corosync

   <pre><code>
   sudo service corosync restart
   </code></pre>

1. **[A]**  Zainstalować składniki drbd

   <pre><code>
   sudo zypper install drbd drbd-kmp-default drbd-utils
   </code></pre>

1. **[A]**  Tworzenie partycji dla urządzenia drbd

   <pre><code>
   sudo sh -c 'echo -e "n\n\n\n\n\nw\n" | fdisk /dev/sdc'
   </code></pre>

1. **[A]**  LVM Tworzenie konfiguracji

   <pre><code>
   sudo pvcreate /dev/sdc1   
   sudo vgcreate vg_NFS /dev/sdc1
   sudo lvcreate -l 100%FREE -n <b>NWS</b> vg_NFS
   </code></pre>

1. **[A]**  Utworzyć urządzenia drbd systemu plików NFS

   <pre><code>
   sudo vi /etc/drbd.d/<b>NWS</b>_nfs.res
   </code></pre>

   Wstaw Konfiguracja nowe urządzenie drbd i zakończenia

   <pre><code>
   resource <b>NWS</b>_nfs {
      protocol     C;
      disk {
         on-io-error       pass_on;
      }
      on <b>prod-nfs-0</b> {
         address   <b>10.0.0.5</b>:7790;
         device    /dev/drbd0;
         disk      /dev/vg_NFS/NWS;
         meta-disk internal;
      }
      on <b>prod-nfs-1</b> {
         address   <b>10.0.0.6</b>:7790;
         device    /dev/drbd0;
         disk      /dev/vg_NFS/NWS;
         meta-disk internal;
      }
   }
   </code></pre>

   Utwórz urządzenie drbd i uruchom ją

   <pre><code>
   sudo drbdadm create-md <b>NWS</b>_nfs
   sudo drbdadm up <b>NWS</b>_nfs
   </code></pre>

1. **[1]**  Pominąć synchronizacji początkowej

   <pre><code>
   sudo drbdadm new-current-uuid --clear-bitmap <b>NWS</b>_nfs
   </code></pre>

1. **[1]**  Ustawić węzła podstawowego

   <pre><code>
   sudo drbdadm primary --force <b>NWS</b>_nfs
   </code></pre>

1. **[1]**  Poczekaj, aż nowych urządzeń drbd są zsynchronizowane.

   <pre><code>
   sudo cat /proc/drbd

   # version: 8.4.6 (api:1/proto:86-101)
   # GIT-hash: 833d830e0152d1e457fa7856e71e11248ccf3f70 build by abuild@sheep14, 2016-05-09 23:14:56
   # 0: cs:Connected ro:Primary/Secondary ds:UpToDate/UpToDate C r-----
   #    ns:0 nr:0 dw:0 dr:912 al:8 bm:0 lo:0 pe:0 ua:0 ap:0 ep:1 wo:f oos:0
   </code></pre>

1. **[1]**  Tworzenie systemów plików na urządzeniach drbd

   <pre><code>
   sudo mkfs.xfs /dev/drbd0
   </code></pre>


### <a name="configure-cluster-framework"></a>Konfigurowanie klastra Framework

1. **[1]**  Zmiany ustawień domyślnych

   <pre><code>
   sudo crm configure

   crm(live)configure# rsc_defaults resource-stickiness="1"

   crm(live)configure# commit
   crm(live)configure# exit
   </code></pre>

1. **[1]**  Dodaj urządzenie drbd systemu plików NFS w konfiguracji klastra

   <pre><code>
   sudo crm configure

   crm(live)configure# primitive drbd_<b>NWS</b>_nfs \
     ocf:linbit:drbd \
     params drbd_resource="<b>NWS</b>_nfs" \
     op monitor interval="15" role="Master" \
     op monitor interval="30" role="Slave"

   crm(live)configure# ms ms-drbd_<b>NWS</b>_nfs drbd_<b>NWS</b>_nfs \
     meta master-max="1" master-node-max="1" clone-max="2" \
     clone-node-max="1" notify="true" interleave="true"

   crm(live)configure# commit
   crm(live)configure# exit
   </code></pre>

1. **[1]**  Utwórz serwer systemu plików NFS

   <pre><code>
   sudo crm configure

   crm(live)configure# primitive nfsserver \
     systemd:nfs-server \
     op monitor interval="30s"

   crm(live)configure# clone cl-nfsserver nfsserver interleave="true"

   crm(live)configure# commit
   crm(live)configure# exit
   </code></pre>

1. **[1]**  Tworzenie zasobów systemu plików NFS

   <pre><code>
   sudo crm configure

   crm(live)configure# primitive fs_<b>NWS</b>_sapmnt \
     ocf:heartbeat:Filesystem \
     params device=/dev/drbd0 \
     directory=/srv/nfs/<b>NWS</b>  \
     fstype=xfs \
     op monitor interval="10s"

   crm(live)configure# group g-<b>NWS</b>_nfs fs_<b>NWS</b>_sapmnt

   crm(live)configure# order o-<b>NWS</b>_drbd_before_nfs inf: \
     ms-drbd_<b>NWS</b>_nfs:promote g-<b>NWS</b>_nfs:start
   
   crm(live)configure# colocation col-<b>NWS</b>_nfs_on_drbd inf: \
     g-<b>NWS</b>_nfs ms-drbd_<b>NWS</b>_nfs:Master

   crm(live)configure# commit
   crm(live)configure# exit
   </code></pre>

1. **[1]**  Utwórz eksportuje NFS

   <pre><code>
   sudo mkdir /srv/nfs/<b>NWS</b>/sidsys
   sudo mkdir /srv/nfs/<b>NWS</b>/sapmntsid
   sudo mkdir /srv/nfs/<b>NWS</b>/trans

   sudo crm configure

   crm(live)configure# primitive exportfs_<b>NWS</b> \
     ocf:heartbeat:exportfs \
     params directory="/srv/nfs/<b>NWS</b>" \
     options="rw,no_root_squash" \
     clientspec="*" fsid=0 \
     wait_for_leasetime_on_stop=true \
     op monitor interval="30s"

   crm(live)configure# modgroup g-<b>NWS</b>_nfs add exportfs_<b>NWS</b>

   crm(live)configure# commit
   crm(live)configure# exit
   </code></pre>

1. **[1]**  Tworzenie zasobu wirtualnego adresu IP i sondy kondycji dla wewnętrznego modułu równoważenia obciążenia

   <pre><code>
   sudo crm configure

   crm(live)configure# primitive vip_<b>NWS</b>_nfs IPaddr2 \
     params ip=<b>10.0.0.4</b> cidr_netmask=<b>24</b> \
     op monitor interval=10 timeout=20

   crm(live)configure# primitive nc_<b>NWS</b>_nfs anything \
     params binfile="/usr/bin/nc" cmdline_options="-l -k 610<b>00</b>" \
     op monitor timeout=20s interval=10 depth=0

   crm(live)configure# modgroup g-<b>NWS</b>_nfs add nc_<b>NWS</b>_nfs
   crm(live)configure# modgroup g-<b>NWS</b>_nfs add vip_<b>NWS</b>_nfs

   crm(live)configure# commit
   crm(live)configure# exit
   </code></pre>

### <a name="create-stonith-device"></a>Utwórz urządzenie STONITH

Urządzenie STONITH używa nazwy głównej usługi do autoryzacji przed Microsoft Azure. Wykonaj następujące kroki, aby utworzyć nazwy głównej usługi.

1. Przejdź do <https://portal.azure.com>
1. Otwarcie bloku usługi Azure Active Directory  
   Przejdź do właściwości i zanotuj nazwę katalogu. Jest to **Identyfikatorem dzierżawy**.
1. Kliknij przycisk rejestracji aplikacji
1. Kliknij pozycję Dodaj.
1. Wprowadź nazwę, wybierz typ aplikacji "Aplikacja/interfejs API sieci Web", wprowadź adres URL logowania (np. http://localhost) i kliknij przycisk Utwórz
1. Adres URL logowania nie jest używany i może być dowolny prawidłowy adres URL
1. Wybierz nową aplikację i kliknij na karcie Ustawienia
1. Wprowadź opis nowego klucza, wybierz pozycję "Nigdy nie wygasa" i kliknij przycisk Zapisz
1. Zanotuj wartość. Jest on używany jako **hasło** główną usługi
1. Zanotuj identyfikator aplikacji. Jest on używany jako nazwa użytkownika (**Identyfikatora** w poniższych krokach) główną usługi

Nazwy głównej usługi nie ma uprawnień do domyślnie dostęp do zasobów platformy Azure. Należy podać nazwę główną usługi uprawnień do uruchamiania i zatrzymywania (deallocate) wszystkich maszyn wirtualnych klastra.

1. Przejdź do https://portal.azure.com
1. Otwórz wszystkie bloku zasobów
1. Wybierz maszynę wirtualną
1. Kliknij przycisk kontroli dostępu (IAM)
1. Kliknij pozycję Dodaj.
1. Wybierz rolę właściciela
1. Wprowadź nazwę aplikacji, utworzoną wcześniej
1. Kliknij przycisk OK

#### <a name="1-create-the-stonith-devices"></a>**[1]**  Utworzyć urządzenia STONITH

Po można edytowane uprawnienia dla maszyn wirtualnych, możesz skonfigurować urządzenia STONITH w klastrze.

<pre><code>
sudo crm configure

# replace the bold string with your subscription ID, resource group, tenant ID, service principal ID and password

crm(live)configure# primitive rsc_st_azure_1 stonith:fence_azure_arm \
   params subscriptionId="<b>subscription ID</b>" resourceGroup="<b>resource group</b>" tenantId="<b>tenant ID</b>" login="<b>login ID</b>" passwd="<b>password</b>"

crm(live)configure# primitive rsc_st_azure_2 stonith:fence_azure_arm \
   params subscriptionId="<b>subscription ID</b>" resourceGroup="<b>resource group</b>" tenantId="<b>tenant ID</b>" login="<b>login ID</b>" passwd="<b>password</b>"

crm(live)configure# colocation col_st_azure -2000: rsc_st_azure_1:Started rsc_st_azure_2:Started

crm(live)configure# commit
crm(live)configure# exit
</code></pre>

#### <a name="1-enable-the-use-of-a-stonith-device"></a>**[1]**  Korzystanie z urządzenia STONITH

<pre><code>
sudo crm configure property stonith-enabled=true 
</code></pre>

## <a name="setting-up-ascs"></a>Konfigurowanie SCS ()

### <a name="deploying-linux"></a>Wdrażanie systemu Linux

W portalu Azure Marketplace zawiera obraz systemu SUSE Linux Enterprise Server dla 12 aplikacje SAP, który służy do wdrażania nowych maszyn wirtualnych. Obrazu z witryny marketplace zawiera agenta zasobów dla programu SAP NetWeaver.

Umożliwia jednego z szablonów Szybki Start w serwisie github wdrażanie wszystkich wymaganych zasobów. Szablon wdraża maszyny wirtualne, usługi równoważenia obciążenia, dostępność ustawić itd. Wykonaj następujące kroki, aby wdrożyć szablon:

1. Otwórz [szablon identyfikatora SID Multi ASCS/SCS] [ template-multisid-xscs] lub [zbieżność szablonu] [ template-converged] na platformie Azure portalu ASCS/SCS szablon tylko tworzy reguły równoważenia obciążenia dla programu SAP NetWeaver ASCS/SCS i wystąpień Wywołujących (tylko w systemie Linux) należy konwergentnej szablon tworzy również reguły równoważenia obciążenia dla bazy danych (na przykład Microsoft SQL Server lub SAP HANA). Jeśli planujesz zainstalowanie systemu SAP NetWeaver na podstawie i również chcesz zainstalować bazy danych na tej samej maszyny, użyj [zbieżność szablonu][template-converged].
1. Wprowadź następujące parametry
   1. Prefiks zasobów (tylko szablon identyfikatora SID Multi ASCS/SCS)  
      Wprowadź prefiks, który ma być używany. Wartość jest używana jako prefiksu dla zasobów, które zostały wdrożone.
   3. Identyfikator systemu SAP (tylko szablon konwergentnej)  
      Wprowadź identyfikator systemu SAP systemu SAP, które chcesz zainstalować. Ten identyfikator jest używany jako prefiksu dla zasobów, które zostały wdrożone.
   4. Typ stosu  
      Wybierz typ SAP NetWeaver stosu
   5. Typ systemu operacyjnego  
      Wybierz jedną z dystrybucje systemu Linux. Na przykład wybierz SLES 12 BYOS
   6. Typ bazy danych  
      Wybierz HANA
   7. Rozmiar systemu SAP  
      Ilość protokoły SAP zapewnia nowy system. Jeśli nie wiadomo jak wiele protokoły SAP wymaga systemu, skontaktuj się z partnerem technologii SAP lub Integrator systemu
   8. Dostępność systemu  
      Wybierz opcję wysokiej dostępności
   9. Nazwa użytkownika i hasło administratora  
      Tworzony jest nowy użytkownik, który może służyć do logowania się do komputera.
   10. Identyfikator podsieci  
   Identyfikator podsieci, do której maszyny wirtualne powinny być podłączone do.  Pozostaw pole puste, jeśli chcesz utworzyć nową sieć wirtualną lub wybierz tej samej podsieci, która użycia lub utworzenia jako część wdrożenia serwera systemu plików NFS. Identyfikator zwykle wygląda /subscriptions/**&lt;identyfikator subskrypcji&gt;**/resourceGroups/**&lt;Nazwa grupy zasobów&gt;**/providers/ Microsoft.Network/virtualNetworks/**&lt;wirtualnej nazwy sieciowej&gt;**/subnets/**&lt;nazwy podsieci&gt;**

### <a name="installation"></a>Instalacja

Następujące elementy są poprzedzane prefiksem albo **[A]** — mające zastosowanie do wszystkich węzłów, **[1]** — dotyczy to tylko węzła 1 lub **[2]** — dotyczy to tylko węzeł 2.

1. **[A]**  Zaktualizować SLES

   <pre><code>
   sudo zypper update
   </code></pre>

1. **[1]**  Dostęp ssh

   <pre><code>
   sudo ssh-keygen -tdsa
   
   # Enter file in which to save the key (/root/.ssh/id_dsa): -> ENTER
   # Enter passphrase (empty for no passphrase): -> ENTER
   # Enter same passphrase again: -> ENTER
   
   # copy the public key
   sudo cat /root/.ssh/id_dsa.pub
   </code></pre>

2. **[2]**  Dostęp ssh

   <pre><code>
   sudo ssh-keygen -tdsa

   # insert the public key you copied in the last step into the authorized keys file on the second server
   sudo vi /root/.ssh/authorized_keys
   
   # Enter file in which to save the key (/root/.ssh/id_dsa): -> ENTER
   # Enter passphrase (empty for no passphrase): -> ENTER
   # Enter same passphrase again: -> ENTER
   
   # copy the public key   
   sudo cat /root/.ssh/id_dsa.pub
   </code></pre>

1. **[1]**  Dostęp ssh

   <pre><code>
   # insert the public key you copied in the last step into the authorized keys file on the first server
   sudo vi /root/.ssh/authorized_keys
   </code></pre>

1. **[A]**  Zainstalować HA rozszerzenia
   
   <pre><code>
   sudo zypper install sle-ha-release fence-agents
   </code></pre>

1. **[A]**  SAP aktualizacji zasobów agentów  
   
   Poprawka dla pakietu zasobów agentów jest wymagany do użycia nowej konfiguracji, które jest opisane w tym artykule. Można sprawdzić, czy poprawka jest już zainstalowana przy użyciu następującego polecenia

   <pre><code>
   sudo grep 'parameter name="IS_ERS"' /usr/lib/ocf/resource.d/heartbeat/SAPInstance
   </code></pre>

   Dane wyjściowe powinny być podobne do

   <pre><code>
   &lt;parameter name="IS_ERS" unique="0" required="0"&gt;
   </code></pre>

   Jeśli polecenie grep nie może znaleźć parametru IS_ERS, musisz zainstalować poprawki wymienione na [SUSE stronę pobierania](https://download.suse.com/patch/finder/#bu=suse&familyId=&productId=&dateRange=&startDate=&endDate=&priority=&architecture=&keywords=resource-agents)

   <pre><code>
   # example for patch for SLES 12 SP1
   sudo zypper in -t patch SUSE-SLE-HA-12-SP1-2017-885=1
   # example for patch for SLES 12 SP2
   sudo zypper in -t patch SUSE-SLE-HA-12-SP2-2017-886=1
   </code></pre>

1. **[A]**  Instalatora rozpoznawania nazwy hosta   

   Można użyć serwera DNS lub zmodyfikować/etc/hosts na wszystkich węzłach. Ten przykład przedstawia sposób użycia pliku/etc/hosts.
   Zastąp adres IP i nazwy hosta w poniższych poleceniach

   <pre><code>
   sudo vi /etc/hosts
   </code></pre>
   
   Wstaw następujące wiersze do/etc/hosts. Zmienianie adresu IP i nazwy hosta do danego środowiska   
   
   <pre><code>
   # IP address of the load balancer frontend configuration for NFS
   <b>10.0.0.4 nws-nfs</b>
   # IP address of the load balancer frontend configuration for SAP NetWeaver ASCS/SCS
   <b>10.0.0.10 nws-ascs</b>
   # IP address of the load balancer frontend configuration for SAP NetWeaver ERS
   <b>10.0.0.11 nws-ers</b>
   # IP address of the load balancer frontend configuration for database
   <b>10.0.0.12 nws-db</b>
   </code></pre>

1. **[1]**  Instalacji klastra
   
   <pre><code>
   sudo ha-cluster-init
   
   # Do you want to continue anyway? [y/N] -> y
   # Network address to bind to (for example: 192.168.1.0) [10.79.227.0] -> ENTER
   # Multicast address (for example: 239.x.x.x) [239.174.218.125] -> ENTER
   # Multicast port [5405] -> ENTER
   # Do you wish to use SBD? [y/N] -> N
   # Do you wish to configure an administration IP? [y/N] -> N
   </code></pre>

1. **[2]**  Dodaj węzeł do klastra
   
   <pre><code> 
   sudo ha-cluster-join

   # WARNING: NTP is not configured to start at system boot.
   # WARNING: No watchdog device found. If SBD is used, the cluster will be unable to start without a watchdog.
   # Do you want to continue anyway? [y/N] -> y
   # IP address or hostname of existing node (for example: 192.168.1.1) [] -> IP address of node 1 for example 10.0.0.10
   # /root/.ssh/id_dsa already exists - overwrite? [y/N] N
   </code></pre>

1. **[A]**  Zmień hacluster hasło do tego samego hasła

   <pre><code> 
   sudo passwd hacluster
   </code></pre>

1. **[A]**  Skonfigurować corosync Użyj innego transportu, a następnie dodaj wstawienia. Klaster nie działa, w przeciwnym razie wartość.
   
   <pre><code> 
   sudo vi /etc/corosync/corosync.conf   
   </code></pre>

   Dodaj następującą zawartość pogrubienie do pliku.
   
   <pre><code> 
   [...]
     interface { 
        [...] 
     }
     <b>transport:      udpu</b>
   } 
   <b>nodelist {
     node {
      # IP address of <b>nws-cl-0</b>
      ring0_addr:     10.0.0.14
     }
     node {
      # IP address of <b>nws-cl-1</b>
      ring0_addr:     10.0.0.13
     } 
   }</b>
   logging {
     [...]
   </code></pre>

   Następnie uruchom ponownie usługę corosync

   <pre><code>
   sudo service corosync restart
   </code></pre>

1. **[A]**  Zainstalować składniki drbd

   <pre><code>
   sudo zypper install drbd drbd-kmp-default drbd-utils
   </code></pre>

1. **[A]**  Tworzenie partycji dla urządzenia drbd

   <pre><code>
   sudo sh -c 'echo -e "n\n\n\n\n\nw\n" | fdisk /dev/sdc'
   </code></pre>

1. **[A]**  LVM Tworzenie konfiguracji

   <pre><code>
   sudo pvcreate /dev/sdc1   
   sudo vgcreate vg_<b>NWS</b> /dev/sdc1
   sudo lvcreate -l 50%FREE -n <b>NWS</b>_ASCS vg_<b>NWS</b>
   sudo lvcreate -l 50%FREE -n <b>NWS</b>_ERS vg_<b>NWS</b>
   </code></pre>

1. **[A]**  Utworzyć SCS drbd urządzenia

   <pre><code>
   sudo vi /etc/drbd.d/<b>NWS</b>_ascs.res
   </code></pre>

   Wstaw Konfiguracja nowe urządzenie drbd i zakończenia

   <pre><code>
   resource <b>NWS</b>_ascs {
      protocol     C;
      disk {
         on-io-error       pass_on;
      }
      on <b>nws-cl-0</b> {
         address   <b>10.0.0.14</b>:7791;
         device    /dev/drbd0;
         disk      /dev/vg_NWS/NWS_ASCS;
         meta-disk internal;
      }
      on <b>nws-cl-1</b> {
         address   <b>10.0.0.13</b>:7791;
         device    /dev/drbd0;
         disk      /dev/vg_NWS/NWS_ASCS;
         meta-disk internal;
      }
   }
   </code></pre>

   Utwórz urządzenie drbd i uruchom ją

   <pre><code>
   sudo drbdadm create-md <b>NWS</b>_ascs
   sudo drbdadm up <b>NWS</b>_ascs
   </code></pre>

1. **[A]**  Utworzyć urządzenia drbd Wywołujących

   <pre><code>
   sudo vi /etc/drbd.d/<b>NWS</b>_ers.res
   </code></pre>

   Wstaw Konfiguracja nowe urządzenie drbd i zakończenia

   <pre><code>
   resource <b>NWS</b>_ers {
      protocol     C;
      disk {
         on-io-error       pass_on;
      }
      on <b>nws-cl-0</b> {
         address   <b>10.0.0.14</b>:7792;
         device    /dev/drbd1;
         disk      /dev/vg_NWS/NWS_ERS;
         meta-disk internal;
      }
      on <b>nws-cl-1</b> {
         address   <b>10.0.0.13</b>:7792;
         device    /dev/drbd1;
         disk      /dev/vg_NWS/NWS_ERS;
         meta-disk internal;
      }
   }
   </code></pre>

   Utwórz urządzenie drbd i uruchom ją

   <pre><code>
   sudo drbdadm create-md <b>NWS</b>_ers
   sudo drbdadm up <b>NWS</b>_ers
   </code></pre>

1. **[1]**  Pominąć synchronizacji początkowej

   <pre><code>
   sudo drbdadm new-current-uuid --clear-bitmap <b>NWS</b>_ascs
   sudo drbdadm new-current-uuid --clear-bitmap <b>NWS</b>_ers
   </code></pre>

1. **[1]**  Ustawić węzła podstawowego

   <pre><code>
   sudo drbdadm primary --force <b>NWS</b>_ascs
   sudo drbdadm primary --force <b>NWS</b>_ers
   </code></pre>

1. **[1]**  Poczekaj, aż nowych urządzeń drbd są zsynchronizowane.

   <pre><code>
   sudo cat /proc/drbd

   # version: 8.4.6 (api:1/proto:86-101)
   # GIT-hash: 833d830e0152d1e457fa7856e71e11248ccf3f70 build by abuild@sheep14, 2016-05-09 23:14:56
   # 0: cs:<b>Connected</b> ro:Primary/Secondary ds:<b>UpToDate/UpToDate</b> C r-----
   #     ns:93991268 nr:0 dw:93991268 dr:93944920 al:383 bm:0 lo:0 pe:0 ua:0 ap:0 ep:1 wo:f oos:0
   # 1: cs:<b>Connected</b> ro:Primary/Secondary ds:<b>UpToDate/UpToDate</b> C r-----
   #     ns:6047920 nr:0 dw:6047920 dr:6039112 al:34 bm:0 lo:0 pe:0 ua:0 ap:0 ep:1 wo:f oos:0
   # 2: cs:<b>Connected</b> ro:Primary/Secondary ds:<b>UpToDate/UpToDate</b> C r-----
   #     ns:5142732 nr:0 dw:5142732 dr:5133924 al:30 bm:0 lo:0 pe:0 ua:0 ap:0 ep:1 wo:f oos:0
   </code></pre>

1. **[1]**  Tworzenie systemów plików na urządzeniach drbd

   <pre><code>
   sudo mkfs.xfs /dev/drbd0
   sudo mkfs.xfs /dev/drbd1
   </code></pre>


### <a name="configure-cluster-framework"></a>Konfigurowanie klastra Framework

**[1]**  Zmiany ustawień domyślnych

   <pre><code>
   sudo crm configure

   crm(live)configure# rsc_defaults resource-stickiness="1"

   crm(live)configure# commit
   crm(live)configure# exit
   </code></pre>

## <a name="prepare-for-sap-netweaver-installation"></a>Przygotowanie do instalacji programu SAP NetWeaver

1. **[A]**  Tworzenia udostępnionych katalogów

   <pre><code>
   sudo mkdir -p /sapmnt/<b>NWS</b>
   sudo mkdir -p /usr/sap/trans
   sudo mkdir -p /usr/sap/<b>NWS</b>/SYS

   sudo chattr +i /sapmnt/<b>NWS</b>
   sudo chattr +i /usr/sap/trans
   sudo chattr +i /usr/sap/<b>NWS</b>/SYS
   </code></pre>

1. **[A]**  Skonfigurować autofs
 
   <pre><code>
   sudo vi /etc/auto.master

   # Add the following line to the file, save and exit
   +auto.master
   /- /etc/auto.direct
   </code></pre>

   Utwórz plik o

   <pre><code>
   sudo vi /etc/auto.direct

   # Add the following lines to the file, save and exit
   /sapmnt/<b>NWS</b> -nfsvers=4,nosymlink,sync <b>nws-nfs</b>:/sapmntsid
   /usr/sap/trans -nfsvers=4,nosymlink,sync <b>nws-nfs</b>:/trans
   /usr/sap/<b>NWS</b>/SYS -nfsvers=4,nosymlink,sync <b>nws-nfs</b>:/sidsys
   </code></pre>

   Uruchom ponownie autofs, aby zainstalować nowe udziały

   <pre><code>
   sudo systemctl enable autofs
   sudo service autofs restart
   </code></pre>

1. **[A]**  Skonfigurować wymiany plików
 
   <pre><code>
   sudo vi /etc/waagent.conf

   # Set the property ResourceDisk.EnableSwap to y
   # Create and use swapfile on resource disk.
   ResourceDisk.EnableSwap=<b>y</b>

   # Set the size of the SWAP file with property ResourceDisk.SwapSizeMB
   # The free space of resource disk varies by virtual machine size. Make sure that you do not set a value that is too big. You can check the SWAP space with command swapon
   # Size of the swapfile.
   ResourceDisk.SwapSizeMB=<b>2000</b>
   </code></pre>

   Uruchom ponownie agenta do aktywowania zmian

   <pre><code>
   sudo service waagent restart
   </code></pre>

### <a name="installing-sap-netweaver-ascsers"></a>Instalowanie programu SAP NetWeaver ASCS/Wywołujących

1. **[1]**  Tworzenie zasobu wirtualnego adresu IP i sondy kondycji dla wewnętrznego modułu równoważenia obciążenia

   <pre><code>
   sudo crm node standby <b>nws-cl-1</b>
   sudo crm configure

   crm(live)configure# primitive drbd_<b>NWS</b>_ASCS \
     ocf:linbit:drbd \
     params drbd_resource="<b>NWS</b>_ascs" \
     op monitor interval="15" role="Master" \
     op monitor interval="30" role="Slave"

   crm(live)configure# ms ms-drbd_<b>NWS</b>_ASCS drbd_<b>NWS</b>_ASCS \
     meta master-max="1" master-node-max="1" clone-max="2" \
     clone-node-max="1" notify="true"

   crm(live)configure# primitive fs_<b>NWS</b>_ASCS \
     ocf:heartbeat:Filesystem \
     params device=/dev/drbd0 \
     directory=/usr/sap/<b>NWS</b>/ASCS<b>00</b>  \
     fstype=xfs \
     op monitor interval="10s"

   crm(live)configure# primitive vip_<b>NWS</b>_ASCS IPaddr2 \
     params ip=<b>10.0.0.10</b> cidr_netmask=<b>24</b> \
     op monitor interval=10 timeout=20

   crm(live)configure# primitive nc_<b>NWS</b>_ASCS anything \
     params binfile="/usr/bin/nc" cmdline_options="-l -k 620<b>00</b>" \
     op monitor timeout=20s interval=10 depth=0
   
   crm(live)configure# group g-<b>NWS</b>_ASCS nc_<b>NWS</b>_ASCS vip_<b>NWS</b>_ASCS fs_<b>NWS</b>_ASCS \
      meta resource-stickiness=3000

   crm(live)configure# order o-<b>NWS</b>_drbd_before_ASCS inf: \
     ms-drbd_<b>NWS</b>_ASCS:promote g-<b>NWS</b>_ASCS:start
   
   crm(live)configure# colocation col-<b>NWS</b>_ASCS_on_drbd inf: \
     ms-drbd_<b>NWS</b>_ASCS:Master g-<b>NWS</b>_ASCS
   
   crm(live)configure# commit
   crm(live)configure# exit
   </code></pre>

   Upewnij się, że stan klastra jest prawidłowy i czy wszystkie zasoby są uruchomione. Nie jest to ważne w węźle, które zasoby są uruchomione.

   <pre><code>
   sudo crm_mon -r

   # Node nws-cl-1: standby
   # <b>Online: [ nws-cl-0 ]</b>
   # 
   # Full list of resources:
   # 
   #  Master/Slave Set: ms-drbd_NWS_ASCS [drbd_NWS_ASCS]
   #      <b>Masters: [ nws-cl-0 ]</b>
   #      Stopped: [ nws-cl-1 ]
   #  Resource Group: g-NWS_ASCS
   #      nc_NWS_ASCS        (ocf::heartbeat:anything):      <b>Started nws-cl-0</b>
   #      vip_NWS_ASCS       (ocf::heartbeat:IPaddr2):       <b>Started nws-cl-0</b>
   #      fs_NWS_ASCS        (ocf::heartbeat:Filesystem):    <b>Started nws-cl-0</b>
   </code></pre>

1. **[1]**  Zainstalować SAP NetWeaver ASCS  

   Instalowanie programu SAP NetWeaver ASCS jako katalog główny na pierwszym węźle, za pomocą wirtualnej nazwy hosta, który jest mapowany na adres IP frontonu modułu równoważenia obciążenia konfiguracji ASCS, na przykład <b>nws ascs</b>, <b>10.0.0.10</b> i wystąpienie numer, który był używany przez sondę modułu równoważenia obciążenia, na przykład <b>00</b>.

   Można użyć parametru sapinst SAPINST_REMOTE_ACCESS_USER, aby zezwolić użytkownikowi inny niż główny nawiązać sapinst.

   <pre><code>
   sudo &lt;swpm&gt;/sapinst SAPINST_REMOTE_ACCESS_USER=<b>sapadmin</b>
   </code></pre>

1. **[1]**  Tworzenie zasobu wirtualnego adresu IP i sondy kondycji dla wewnętrznego modułu równoważenia obciążenia

   <pre><code>
   sudo crm node standby <b>nws-cl-0</b>
   sudo crm node online <b>nws-cl-1</b>
   sudo crm configure

   crm(live)configure# primitive drbd_<b>NWS</b>_ERS \
     ocf:linbit:drbd \
     params drbd_resource="<b>NWS</b>_ers" \
     op monitor interval="15" role="Master" \
     op monitor interval="30" role="Slave"

   crm(live)configure# ms ms-drbd_<b>NWS</b>_ERS drbd_<b>NWS</b>_ERS \
     meta master-max="1" master-node-max="1" clone-max="2" \
     clone-node-max="1" notify="true"

   crm(live)configure# primitive fs_<b>NWS</b>_ERS \
     ocf:heartbeat:Filesystem \
     params device=/dev/drbd1 \
     directory=/usr/sap/<b>NWS</b>/ERS<b>02</b>  \
     fstype=xfs \
     op monitor interval="10s"

   crm(live)configure# primitive vip_<b>NWS</b>_ERS IPaddr2 \
     params ip=<b>10.0.0.11</b> cidr_netmask=<b>24</b> \
     op monitor interval=10 timeout=20

   crm(live)configure# primitive nc_<b>NWS</b>_ERS anything \
    params binfile="/usr/bin/nc" cmdline_options="-l -k 621<b>02</b>" \
    op monitor timeout=20s interval=10 depth=0

   crm(live)configure# group g-<b>NWS</b>_ERS nc_<b>NWS</b>_ERS vip_<b>NWS</b>_ERS fs_<b>NWS</b>_ERS

   crm(live)configure# order o-<b>NWS</b>_drbd_before_ERS inf: \
     ms-drbd_<b>NWS</b>_ERS:promote g-<b>NWS</b>_ERS:start
   
   crm(live)configure# colocation col-<b>NWS</b>_ERS_on_drbd inf: \
     ms-drbd_<b>NWS</b>_ERS:Master g-<b>NWS</b>_ERS
   
   crm(live)configure# commit
   # WARNING: Resources nc_NWS_ASCS,nc_NWS_ERS,nc_NWS_nfs violate uniqueness for parameter "binfile": "/usr/bin/nc"
   # Do you still want to commit (y/n)? y

   crm(live)configure# exit
   
   </code></pre>
 
   Upewnij się, że stan klastra jest prawidłowy i czy wszystkie zasoby są uruchomione. Nie jest to ważne w węźle, które zasoby są uruchomione.

   <pre><code>
   sudo crm_mon -r

   # Node <b>nws-cl-0: standby</b>
   # <b>Online: [ nws-cl-1 ]</b>
   # 
   # Full list of resources:
   # 
   #  Master/Slave Set: ms-drbd_NWS_ASCS [drbd_NWS_ASCS]
   #      <b>Masters: [ nws-cl-1 ]</b>
   #      Stopped: [ nws-cl-0 ]
   #  Resource Group: g-NWS_ASCS
   #      nc_NWS_ASCS        (ocf::heartbeat:anything):      <b>Started nws-cl-1</b>
   #      vip_NWS_ASCS       (ocf::heartbeat:IPaddr2):       <b>Started nws-cl-1</b>
   #      fs_NWS_ASCS        (ocf::heartbeat:Filesystem):    <b>Started nws-cl-1</b>
   #  Master/Slave Set: ms-drbd_NWS_ERS [drbd_NWS_ERS]
   #      <b>Masters: [ nws-cl-1 ]</b>
   #      Stopped: [ nws-cl-0 ]
   #  Resource Group: g-NWS_ERS
   #      nc_NWS_ERS (ocf::heartbeat:anything):      <b>Started nws-cl-1</b>
   #      vip_NWS_ERS        (ocf::heartbeat:IPaddr2):       <b>Started nws-cl-1</b>
   #      fs_NWS_ERS (ocf::heartbeat:Filesystem):    <b>Started nws-cl-1</b>
   </code></pre>

1. **[2]**  Zainstalować SAP NetWeaver Wywołujących  

   Instalowanie programu SAP NetWeaver Wywołujących jako główny na drugi węzeł, za pomocą wirtualnej nazwy hosta, który jest mapowany na adres IP frontonu modułu równoważenia obciążenia konfiguracji Wywołujących, na przykład <b>wywołujących nws</b>, <b>10.0.0.11</b> i wystąpienie numer, który był używany przez sondę modułu równoważenia obciążenia, na przykład <b>02</b>.

   Można użyć parametru sapinst SAPINST_REMOTE_ACCESS_USER, aby zezwolić użytkownikowi inny niż główny nawiązać sapinst.

   <pre><code>
   sudo &lt;swpm&gt;/sapinst SAPINST_REMOTE_ACCESS_USER=<b>sapadmin</b>
   </code></pre>

   > [!NOTE]
   > Użyj PL SP 20 SWPM 05 lub nowszej. Niższych wersjach nie należy ustawiać uprawnienia poprawnie i instalacja zakończy się niepowodzeniem.
   > 

1. **[1]**  Adapt ASCS/SCS i Wywołujących wystąpienia profilów
 
   * ASCS/SCS profilu

   <pre><code> 
   sudo vi /sapmnt/<b>NWS</b>/profile/<b>NWS</b>_<b>ASCS00</b>_<b>nws-ascs</b>

   # Change the restart command to a start command
   #Restart_Program_01 = local $(_EN) pf=$(_PF)
   Start_Program_01 = local $(_EN) pf=$(_PF)

   # Add the following lines
   service/halib = $(DIR_CT_RUN)/saphascriptco.so
   service/halib_cluster_connector = /usr/bin/sap_suse_cluster_connector

   # Add the keep alive parameter
   enque/encni/set_so_keepalive = true
   </code></pre>

   * Profil Wywołujących

   <pre><code> 
   sudo vi /sapmnt/<b>NWS</b>/profile/<b>NWS</b>_ERS<b>02</b>_<b>nws-ers</b>

   # Add the following lines
   service/halib = $(DIR_CT_RUN)/saphascriptco.so
   service/halib_cluster_connector = /usr/bin/sap_suse_cluster_connector
   </code></pre>


1. **[A]**  Skonfigurować podtrzymanie aktywności

   Komunikacja między serwerem SAP NetWeaver aplikacji i ASCS/SCS jest kierowany przez programowy moduł równoważenia obciążenia. Moduł równoważenia obciążenia rozłącza nieaktywne połączenia po upływie limitu czasu w można konfigurować. Aby zapobiec takiej sytuacji należy ustawić parametr w profilu SAP NetWeaver ASCS/SCS i zmienić ustawienia systemu Linux. Odczyt [1410736 Uwaga SAP] [ 1410736] Aby uzyskać więcej informacji.
   
   ASCS/SCS profilu parametru umieścić/encni/set_so_keepalive został już dodany w ostatnim kroku.

   <pre><code> 
   # Change the Linux system configuration
   sudo sysctl net.ipv4.tcp_keepalive_time=120
   </code></pre>

1. **[A]**  Po instalacji, skonfigurować użytkowników SAP
 
   <pre><code>
   # Add sidadm to the haclient group
   sudo usermod -aG haclient <b>nws</b>adm   
   </code></pre>

1. **[1]**  Dodawanie usług ASCS i Wywołujących SAP do pliku sapservice

   Dodaj ASCS usługi wpisu do drugiego węzła i kopiowanie wpisu usługi Wywołujących pierwszy węzeł.

   <pre><code>
   cat /usr/sap/sapservices | grep ASCS<b>00</b> | sudo ssh <b>nws-cl-1</b> "cat >>/usr/sap/sapservices"
   sudo ssh <b>nws-cl-1</b> "cat /usr/sap/sapservices" | grep ERS<b>02</b> | sudo tee -a /usr/sap/sapservices
   </code></pre>

1. **[1]**  Utworzenie zasobów klastra SAP

   <pre><code>
   sudo crm configure property maintenance-mode="true"

   sudo crm configure

   crm(live)configure# primitive rsc_sap_<b>NWS</b>_ASCS<b>00</b> SAPInstance \
    operations $id=rsc_sap_<b>NWS</b>_ASCS<b>00</b>-operations \
    op monitor interval=11 timeout=60 on_fail=restart \
    params InstanceName=<b>NWS</b>_ASCS<b>00</b>_<b>nws-ascs</b> START_PROFILE="/sapmnt/<b>NWS</b>/profile/<b>NWS</b>_ASCS<b>00</b>_<b>nws-ascs</b>" \
    AUTOMATIC_RECOVER=false \
    meta resource-stickiness=5000 failure-timeout=60 migration-threshold=1 priority=10

   crm(live)configure# primitive rsc_sap_<b>NWS</b>_ERS<b>02</b> SAPInstance \
    operations $id=rsc_sap_<b>NWS</b>_ERS<b>02</b>-operations \
    op monitor interval=11 timeout=60 on_fail=restart \
    params InstanceName=<b>NWS</b>_ERS<b>02</b>_<b>nws-ers</b> START_PROFILE="/sapmnt/<b>NWS</b>/profile/<b>NWS</b>_ERS<b>02</b>_<b>nws-ers</b>" AUTOMATIC_RECOVER=false IS_ERS=true \
    meta priority=1000

   crm(live)configure# modgroup g-<b>NWS</b>_ASCS add rsc_sap_<b>NWS</b>_ASCS<b>00</b>
   crm(live)configure# modgroup g-<b>NWS</b>_ERS add rsc_sap_<b>NWS</b>_ERS<b>02</b>

   crm(live)configure# colocation col_sap_<b>NWS</b>_no_both -5000: g-<b>NWS</b>_ERS g-<b>NWS</b>_ASCS
   crm(live)configure# location loc_sap_<b>NWS</b>_failover_to_ers rsc_sap_<b>NWS</b>_ASCS<b>00</b> rule 2000: runs_ers_<b>NWS</b> eq 1
   crm(live)configure# order ord_sap_<b>NWS</b>_first_start_ascs Optional: rsc_sap_<b>NWS</b>_ASCS<b>00</b>:start rsc_sap_<b>NWS</b>_ERS<b>02</b>:stop symmetrical=false

   crm(live)configure# commit
   crm(live)configure# exit

   sudo crm configure property maintenance-mode="false"
   sudo crm node online <b>nws-cl-0</b>
   </code></pre>

   Upewnij się, że stan klastra jest prawidłowy i czy wszystkie zasoby są uruchomione. Nie jest to ważne w węźle, które zasoby są uruchomione.

   <pre><code>
   sudo crm_mon -r

   # Online: <b>[ nws-cl-0 nws-cl-1 ]</b>
   # 
   # Full list of resources:
   # 
   #  Master/Slave Set: ms-drbd_NWS_ASCS [drbd_NWS_ASCS]
   #      <b>Masters: [ nws-cl-0 ]</b>
   #      <b>Slaves: [ nws-cl-1 ]</b>
   #  Resource Group: g-NWS_ASCS
   #      nc_NWS_ASCS        (ocf::heartbeat:anything):      <b>Started nws-cl-0</b>
   #      vip_NWS_ASCS       (ocf::heartbeat:IPaddr2):       <b>Started nws-cl-0</b>
   #      fs_NWS_ASCS        (ocf::heartbeat:Filesystem):    <b>Started nws-cl-0</b>
   #      rsc_sap_NWS_ASCS00 (ocf::heartbeat:SAPInstance):   <b>Started nws-cl-0</b>
   #  Master/Slave Set: ms-drbd_NWS_ERS [drbd_NWS_ERS]
   #      <b>Masters: [ nws-cl-1 ]</b>
   #      <b>Slaves: [ nws-cl-0 ]</b>
   #  Resource Group: g-NWS_ERS
   #      nc_NWS_ERS (ocf::heartbeat:anything):      <b>Started nws-cl-1</b>
   #      vip_NWS_ERS        (ocf::heartbeat:IPaddr2):       <b>Started nws-cl-1</b>
   #      fs_NWS_ERS (ocf::heartbeat:Filesystem):    <b>Started nws-cl-1</b>
   #      rsc_sap_NWS_ERS02  (ocf::heartbeat:SAPInstance):   <b>Started nws-cl-1</b>
   </code></pre>

### <a name="create-stonith-device"></a>Utwórz urządzenie STONITH

Urządzenie STONITH używa nazwy głównej usługi do autoryzacji przed Microsoft Azure. Wykonaj następujące kroki, aby utworzyć nazwy głównej usługi.

1. Przejdź do <https://portal.azure.com>
1. Otwarcie bloku usługi Azure Active Directory  
   Przejdź do właściwości i zanotuj nazwę katalogu. Jest to **Identyfikatorem dzierżawy**.
1. Kliknij przycisk rejestracji aplikacji
1. Kliknij pozycję Dodaj.
1. Wprowadź nazwę, wybierz typ aplikacji "Aplikacja/interfejs API sieci Web", wprowadź adres URL logowania (np. http://localhost) i kliknij przycisk Utwórz
1. Adres URL logowania nie jest używany i może być dowolny prawidłowy adres URL
1. Wybierz nową aplikację i kliknij na karcie Ustawienia
1. Wprowadź opis nowego klucza, wybierz pozycję "Nigdy nie wygasa" i kliknij przycisk Zapisz
1. Zanotuj wartość. Jest on używany jako **hasło** główną usługi
1. Zanotuj identyfikator aplikacji. Jest on używany jako nazwa użytkownika (**Identyfikatora** w poniższych krokach) główną usługi

Nazwy głównej usługi nie ma uprawnień do domyślnie dostęp do zasobów platformy Azure. Należy podać nazwę główną usługi uprawnień do uruchamiania i zatrzymywania (deallocate) wszystkich maszyn wirtualnych klastra.

1. Przejdź do https://portal.azure.com
1. Otwórz wszystkie bloku zasobów
1. Wybierz maszynę wirtualną
1. Kliknij przycisk kontroli dostępu (IAM)
1. Kliknij pozycję Dodaj.
1. Wybierz rolę właściciela
1. Wprowadź nazwę aplikacji, utworzoną wcześniej
1. Kliknij przycisk OK

#### <a name="1-create-the-stonith-devices"></a>**[1]**  Utworzyć urządzenia STONITH

Po można edytowane uprawnienia dla maszyn wirtualnych, możesz skonfigurować urządzenia STONITH w klastrze.

<pre><code>
sudo crm configure

# replace the bold string with your subscription ID, resource group, tenant ID, service principal ID and password

crm(live)configure# primitive rsc_st_azure_1 stonith:fence_azure_arm \
   params subscriptionId="<b>subscription ID</b>" resourceGroup="<b>resource group</b>" tenantId="<b>tenant ID</b>" login="<b>login ID</b>" passwd="<b>password</b>"

crm(live)configure# primitive rsc_st_azure_2 stonith:fence_azure_arm \
   params subscriptionId="<b>subscription ID</b>" resourceGroup="<b>resource group</b>" tenantId="<b>tenant ID</b>" login="<b>login ID</b>" passwd="<b>password</b>"

crm(live)configure# colocation col_st_azure -2000: rsc_st_azure_1:Started rsc_st_azure_2:Started

crm(live)configure# commit
crm(live)configure# exit
</code></pre>

#### <a name="1-enable-the-use-of-a-stonith-device"></a>**[1]**  Korzystanie z urządzenia STONITH

Korzystanie z urządzenia STONITH

<pre><code>
sudo crm configure property stonith-enabled=true 
</code></pre>

## <a name="install-database"></a>Instalowanie bazy danych

W tym przykładzie replikacji systemu SAP HANA jest zainstalowane i skonfigurowane. SAP HANA działa w tym samym klastrze SAP NetWeaver ASCS/SCS i Wywołujących. W dedykowanym klastrze, można także zainstalować SAP HANA. Aby uzyskać więcej informacji, zobacz [dostępności wysokiej programu SAP HANA na maszynach wirtualnych Azure (VM)][sap-hana-ha].

### <a name="prepare-for-sap-hana-installation"></a>Przygotowanie do instalacji SAP HANA

Ogólnie zaleca LVM dla woluminów, które przechowują dane i pliki dziennika. Do celów testowych, są również dostępne do przechowywania danych i pliku bezpośrednio na niezaszyfrowanym dysku dziennika.

1. **[A]**  LVM  
   W poniższym przykładzie założenia, że maszyny wirtualne czterech dyskach danych dołączonych, które powinny być używane do tworzenia dwa woluminy.
   
   Utwórz woluminy fizyczne dla wszystkich dysków, które chcesz użyć.
   
   <pre><code>
   sudo pvcreate /dev/sdd
   sudo pvcreate /dev/sde
   sudo pvcreate /dev/sdf
   sudo pvcreate /dev/sdg
   </code></pre>
   
   Utwórz grupę woluminu plików danych, jedna grupa woluminu dla plików dziennika i jeden do udostępnionego katalogu SAP HANA
   
   <pre><code>
   sudo vgcreate vg_hana_data /dev/sdd /dev/sde
   sudo vgcreate vg_hana_log /dev/sdf
   sudo vgcreate vg_hana_shared /dev/sdg
   </code></pre>
   
   Utwórz woluminy logiczne
   
   <pre><code>
   sudo lvcreate -l 100%FREE -n hana_data vg_hana_data
   sudo lvcreate -l 100%FREE -n hana_log vg_hana_log
   sudo lvcreate -l 100%FREE -n hana_shared vg_hana_shared
   sudo mkfs.xfs /dev/vg_hana_data/hana_data
   sudo mkfs.xfs /dev/vg_hana_log/hana_log
   sudo mkfs.xfs /dev/vg_hana_shared/hana_shared
   </code></pre>
   
   Utwórz katalogi instalacji i skopiuj identyfikator UUID wszystkie woluminy logiczne
   
   <pre><code>
   sudo mkdir -p /hana/data
   sudo mkdir -p /hana/log
   sudo mkdir -p /hana/shared
   sudo chattr +i /hana/data
   sudo chattr +i /hana/log
   sudo chattr +i /hana/shared
   # write down the ID of /dev/vg_hana_data/hana_data, /dev/vg_hana_log/hana_log and /dev/vg_hana_shared/hana_shared
   sudo blkid
   </code></pre>
   
   Tworzenie wpisów autofs trzy logiczne woluminów
   
   <pre><code>
   sudo vi /etc/auto.direct
   </code></pre>
   
   Wstaw ten wiersz, aby /etc/auto.direct vi operacji sudo
   
   <pre><code>
   /hana/data -fstype=xfs :UUID=<b>&lt;UUID of /dev/vg_hana_data/hana_data&gt;</b>
   /hana/log -fstype=xfs :UUID=<b>&lt;UUID of /dev/vg_hana_log/hana_log&gt;</b>
   /hana/shared -fstype=xfs :UUID=<b>&lt;UUID of /dev/vg_hana_shared/hana_shared&gt;</b>
   </code></pre>
   
   Zainstaluj nowe woluminy
   
   <pre><code>
   sudo service autofs restart 
   </code></pre>

1. **[A]**  Zwykłych dysków  

   Dla małych lub demonstracyjnych systemów, możesz umieścić HANA plików danych i dziennika na jednym dysku. Poniższe polecenia utworzyć partycję na /dev/sdc i sformatuj go przy xfs.
   ```bash
   sudo sh -c 'echo -e "n\n\n\n\n\nw\n" | fdisk /dev/sdd'
   sudo mkfs.xfs /dev/sdd1
   
   # write down the id of /dev/sdd1
   sudo /sbin/blkid
   sudo vi /etc/auto.direct
   ```
   
   Wstaw ten wiersz, aby /etc/auto.direct
   <pre><code>
   /hana -fstype=xfs :UUID=<b>&lt;UUID&gt;</b>
   </code></pre>
   
   Utwórz katalog docelowy i zainstalować dysk.
   
   <pre><code>
   sudo mkdir /hana
   sudo chattr +i /hana
   sudo service autofs restart
   </code></pre>

### <a name="installing-sap-hana"></a>Instalowanie programu SAP HANA

Poniższe kroki są oparte na rozdziału 4 [przewodnik SAP HANA SR wydajności zoptymalizowanych pod kątem scenariusza] [ suse-hana-ha-guide] do zainstalowania replikacji systemu SAP HANA. Przeczytaj go przed kontynuowaniem instalacji.

1. **[A]**  Uruchamiania z dysku DVD HANA hdblcm
   
   <pre><code>
   sudo hdblcm --sid=<b>HDB</b> --number=<b>03</b> --action=install --batch --password=<b>&lt;password&gt;</b> --system_user_password=<b>&lt;password for system user&gt;</b>

   sudo /hana/shared/<b>HDB</b>/hdblcm/hdblcm --action=configure_internal_network --listen_interface=internal --internal_network=<b>10.0.0/24</b> --password=<b>&lt;password for system user&gt;</b> --batch
   </code></pre>

1. **[A]**  Uaktualnij agenta hosta SAP

   Pobierz najnowsze archiwum SAP Agent hosta z [SAP Softwarecenter] [ sap-swcenter] i uruchom następujące polecenie, aby uaktualnić agenta. Zastąp ścieżki do archiwum, aby wskazywał pobranego pliku.
   <pre><code>
   sudo /usr/sap/hostctrl/exe/saphostexec -upgrade -archive <b>&lt;path to SAP Host Agent SAR&gt;</b> 
   </code></pre>

1. **[1]**  Utworzyć HANA replikacji (jako główny)  

   Uruchom następujące polecenie. Upewnij się, że Zastąp bold ciągów (HANA systemu identyfikator HDB i numer wystąpienia 03) z wartościami instalację programu SAP HANA.
   <pre><code>
   PATH="$PATH:/usr/sap/<b>HDB</b>/HDB<b>03</b>/exe"
   hdbsql -u system -i <b>03</b> 'CREATE USER <b>hdb</b>hasync PASSWORD "<b>passwd</b>"' 
   hdbsql -u system -i <b>03</b> 'GRANT DATA ADMIN TO <b>hdb</b>hasync' 
   hdbsql -u system -i <b>03</b> 'ALTER USER <b>hdb</b>hasync DISABLE PASSWORD LIFETIME' 
   </code></pre>

1. **[A]**  Tworzenie magazynu kluczy wpis (root)

   <pre><code>
   PATH="$PATH:/usr/sap/<b>HDB</b>/HDB<b>03</b>/exe"
   hdbuserstore SET <b>hdb</b>haloc localhost:3<b>03</b>15 <b>hdb</b>hasync <b>&lt;passwd&gt;</b>
   </code></pre>

1. **[1]**  Kopii zapasowej bazy danych (root)

   <pre><code>
   PATH="$PATH:/usr/sap/<b>HDB</b>/HDB<b>03</b>/exe"
   hdbsql -u system -i <b>03</b> "BACKUP DATA USING FILE ('<b>initialbackup</b>')" 
   </code></pre>

1. **[1]**  Przełącz użytkownika sapsid HANA i Utwórz lokacji głównej.

   <pre><code>
   su - <b>hdb</b>adm
   hdbnsutil -sr_enable –-name=<b>SITE1</b>
   </code></pre>

1. **[2]**  Przełącz użytkownika sapsid HANA i utworzyć lokację dodatkową.

   <pre><code>
   su - <b>hdb</b>adm
   sapcontrol -nr <b>03</b> -function StopWait 600 10
   hdbnsutil -sr_register --remoteHost=<b>nws-cl-0</b> --remoteInstance=<b>03</b> --replicationMode=sync --name=<b>SITE2</b> 
   </code></pre>

1. **[1]**  Utworzyć SAP HANA zasobów klastra

   Najpierw utwórz topologii.
   
   <pre><code>
   sudo crm configure

   # replace the bold string with your instance number and HANA system ID
   
   crm(live)configure# primitive rsc_SAPHanaTopology_<b>HDB</b>_HDB<b>03</b>   ocf:suse:SAPHanaTopology \
     operations $id="rsc_sap2_<b>HDB</b>_HDB<b>03</b>-operations" \
     op monitor interval="10" timeout="600" \
     op start interval="0" timeout="600" \
     op stop interval="0" timeout="300" \
     params SID="<b>HDB</b>" InstanceNumber="<b>03</b>"
    
   crm(live)configure# clone cln_SAPHanaTopology_<b>HDB</b>_HDB<b>03</b> rsc_SAPHanaTopology_<b>HDB</b>_HDB<b>03</b> \
     meta is-managed="true" clone-node-max="1" target-role="Started" interleave="true"

   crm(live)configure# commit
   crm(live)configure# exit
   </code></pre>
   
   Następnie należy utworzyć HANA zasobów
   
   <pre><code>
   sudo crm configure

   # replace the bold string with your instance number, HANA system ID and the frontend IP address of the Azure load balancer. 
    
   crm(live)configure# primitive rsc_SAPHana_<b>HDB</b>_HDB<b>03</b> ocf:suse:SAPHana \
     operations $id="rsc_sap_<b>HDB</b>_HDB<b>03</b>-operations" \
     op start interval="0" timeout="3600" \
     op stop interval="0" timeout="3600" \
     op promote interval="0" timeout="3600" \
     op monitor interval="60" role="Master" timeout="700" \
     op monitor interval="61" role="Slave" timeout="700" \
     params SID="<b>HDB</b>" InstanceNumber="<b>03</b>" PREFER_SITE_TAKEOVER="true" \
     DUPLICATE_PRIMARY_TIMEOUT="7200" AUTOMATED_REGISTER="false"
    
   crm(live)configure# ms msl_SAPHana_<b>HDB</b>_HDB<b>03</b> rsc_SAPHana_<b>HDB</b>_HDB<b>03</b> \
     meta is-managed="true" notify="true" clone-max="2" clone-node-max="1" \
     target-role="Started" interleave="true"
    
   crm(live)configure# primitive rsc_ip_<b>HDB</b>_HDB<b>03</b> ocf:heartbeat:IPaddr2 \ 
     meta target-role="Started" is-managed="true" \ 
     operations $id="rsc_ip_<b>HDB</b>_HDB<b>03</b>-operations" \ 
     op monitor interval="10s" timeout="20s" \ 
     params ip="<b>10.0.0.12</b>" 

   crm(live)configure# primitive rsc_nc_<b>HDB</b>_HDB<b>03</b> anything \ 
     params binfile="/usr/bin/nc" cmdline_options="-l -k 625<b>03</b>" \ 
     op monitor timeout=20s interval=10 depth=0 

   crm(live)configure# group g_ip_<b>HDB</b>_HDB<b>03</b> rsc_ip_<b>HDB</b>_HDB<b>03</b> rsc_nc_<b>HDB</b>_HDB<b>03</b>
    
   crm(live)configure# colocation col_saphana_ip_<b>HDB</b>_HDB<b>03</b> 2000: g_ip_<b>HDB</b>_HDB<b>03</b>:Started \ 
     msl_SAPHana_<b>HDB</b>_HDB<b>03</b>:Master  

   crm(live)configure# order ord_SAPHana_<b>HDB</b>_HDB<b>03</b> 2000: cln_SAPHanaTopology_<b>HDB</b>_HDB<b>03</b> \ 
     msl_SAPHana_<b>HDB</b>_HDB<b>03</b>
    
   crm(live)configure# commit
   crm(live)configure# exit
   </code></pre>

   Upewnij się, że stan klastra jest prawidłowy i czy wszystkie zasoby są uruchomione. Nie jest to ważne w węźle, które zasoby są uruchomione.

   <pre><code>
   sudo crm_mon -r

   # <b>Online: [ nws-cl-0 nws-cl-1 ]</b>
   # 
   # Full list of resources:
   # 
   #  Master/Slave Set: ms-drbd_NWS_ASCS [drbd_NWS_ASCS]
   #      <b>Masters: [ nws-cl-1 ]</b>
   #      <b>Slaves: [ nws-cl-0 ]</b>
   #  Resource Group: g-NWS_ASCS
   #      nc_NWS_ASCS        (ocf::heartbeat:anything):      <b>Started nws-cl-1</b>
   #      vip_NWS_ASCS       (ocf::heartbeat:IPaddr2):       <b>Started nws-cl-1</b>
   #      fs_NWS_ASCS        (ocf::heartbeat:Filesystem):    <b>Started nws-cl-1</b>
   #      rsc_sap_NWS_ASCS00 (ocf::heartbeat:SAPInstance):   <b>Started nws-cl-1</b>
   #  Master/Slave Set: ms-drbd_NWS_ERS [drbd_NWS_ERS]
   #      <b>Masters: [ nws-cl-0 ]</b>
   #      <b>Slaves: [ nws-cl-1 ]</b>
   #  Resource Group: g-NWS_ERS
   #      nc_NWS_ERS (ocf::heartbeat:anything):      <b>Started nws-cl-0</b>
   #      vip_NWS_ERS        (ocf::heartbeat:IPaddr2):       <b>Started nws-cl-0</b>
   #      fs_NWS_ERS (ocf::heartbeat:Filesystem):    <b>Started nws-cl-0</b>
   #      rsc_sap_NWS_ERS02  (ocf::heartbeat:SAPInstance):   <b>Started nws-cl-0</b>
   #  Clone Set: cln_SAPHanaTopology_HDB_HDB03 [rsc_SAPHanaTopology_HDB_HDB03]
   #      <b>Started: [ nws-cl-0 nws-cl-1 ]</b>
   #  Master/Slave Set: msl_SAPHana_HDB_HDB03 [rsc_SAPHana_HDB_HDB03]
   #      <b>Masters: [ nws-cl-0 ]</b>
   #      <b>Slaves: [ nws-cl-1 ]</b>
   #  Resource Group: g_ip_HDB_HDB03
   #      rsc_ip_HDB_HDB03   (ocf::heartbeat:IPaddr2):       <b>Started nws-cl-0</b>
   #      rsc_nc_HDB_HDB03   (ocf::heartbeat:anything):      <b>Started nws-cl-0</b>
   # rsc_st_azure_1  (stonith:fence_azure_arm):      <b>Started nws-cl-0</b>
   # rsc_st_azure_2  (stonith:fence_azure_arm):      <b>Started nws-cl-1</b>
   </code></pre>

1. **[1]**  Zainstalować wystąpienie bazy danych SAP NetWeaver

   Zainstaluj wystąpienie bazy danych SAP NetWeaver jako główny przy użyciu wirtualnego nazwy hosta, który jest mapowany na adres IP z konfiguracji serwera sieci Web usługi równoważenia obciążenia dla bazy danych, na przykład <b>nws-db</b> i <b>10.0.0.12</b>.

   Można użyć parametru sapinst SAPINST_REMOTE_ACCESS_USER, aby zezwolić użytkownikowi inny niż główny nawiązać sapinst.

   <pre><code>
   sudo &lt;swpm&gt;/sapinst SAPINST_REMOTE_ACCESS_USER=<b>sapadmin</b>
   </code></pre>

## <a name="sap-netweaver-application-server-installation"></a>Instalacja serwera programu SAP NetWeaver aplikacji

Wykonaj następujące kroki, aby zainstalować serwer aplikacji SAP. Poniższych kroków założono, zainstalować serwer aplikacji na serwerze innym niż serwer ASCS/SCS i HANA. W przeciwnym razie niektóre czynności (np. Konfigurowanie rozpoznawania nazwy hosta) nie są wymagane.

1. Ustawienia rozpoznawania nazwy hosta    
   Można użyć serwera DNS lub zmodyfikować/etc/hosts na wszystkich węzłach. Ten przykład przedstawia sposób użycia pliku/etc/hosts.
   Zastąp adres IP i nazwy hosta w poniższych poleceniach
   ```bash
   sudo vi /etc/hosts
   ```
   Wstaw następujące wiersze do/etc/hosts. Zmienianie adresu IP i nazwy hosta do danego środowiska    
    
   <pre><code>
   # IP address of the load balancer frontend configuration for NFS
   <b>10.0.0.4 nws-nfs</b>
   # IP address of the load balancer frontend configuration for SAP NetWeaver ASCS/SCS
   <b>10.0.0.10 nws-ascs</b>
   # IP address of the load balancer frontend configuration for SAP NetWeaver ERS
   <b>10.0.0.11 nws-ers</b>
   # IP address of the load balancer frontend configuration for database
   <b>10.0.0.12 nws-db</b>
   # IP address of the application server
   <b>10.0.0.8 nws-di-0</b>
   </code></pre>

1. Utwórz katalog sapmnt

   <pre><code>
   sudo mkdir -p /sapmnt/<b>NWS</b>
   sudo mkdir -p /usr/sap/trans

   sudo chattr +i /sapmnt/<b>NWS</b>
   sudo chattr +i /usr/sap/trans
   </code></pre>

1. Skonfiguruj autofs
 
   <pre><code>
   sudo vi /etc/auto.master

   # Add the following line to the file, save and exit
   +auto.master
   /- /etc/auto.direct
   </code></pre>

   Utwórz nowy plik o

   <pre><code>
   sudo vi /etc/auto.direct

   # Add the following lines to the file, save and exit
   /sapmnt/<b>NWS</b> -nfsvers=4,nosymlink,sync <b>nws-nfs</b>:/sapmntsid
   /usr/sap/trans -nfsvers=4,nosymlink,sync <b>nws-nfs</b>:/trans
   </code></pre>

   Uruchom ponownie autofs, aby zainstalować nowe udziały

   <pre><code>
   sudo systemctl enable autofs
   sudo service autofs restart
   </code></pre>

1. Konfigurowanie pliku wymiany
 
   <pre><code>
   sudo vi /etc/waagent.conf

   # Set the property ResourceDisk.EnableSwap to y
   # Create and use swapfile on resource disk.
   ResourceDisk.EnableSwap=<b>y</b>

   # Set the size of the SWAP file with property ResourceDisk.SwapSizeMB
   # The free space of resource disk varies by virtual machine size. Make sure that you do not set a value that is too big. You can check the SWAP space with command swapon
   # Size of the swapfile.
   ResourceDisk.SwapSizeMB=<b>2000</b>
   </code></pre>

   Uruchom ponownie agenta do aktywowania zmian

   <pre><code>
   sudo service waagent restart
   </code></pre>

1. Instalowanie programu SAP NetWeaver serwera aplikacji

   Zainstaluj serwer aplikacje SAP NetWeaver podstawowej lub dodatkowej.

   Można użyć parametru sapinst SAPINST_REMOTE_ACCESS_USER, aby zezwolić użytkownikowi inny niż główny nawiązać sapinst.

   <pre><code>
   sudo &lt;swpm&gt;/sapinst SAPINST_REMOTE_ACCESS_USER=<b>sapadmin</b>
   </code></pre>

1. Aktualizacja SAP HANA bezpiecznego magazynu

   Zaktualizuj bezpiecznego magazynu SAP HANA wskaż nazwę wirtualnego ustawień replikacji systemu SAP HANA.
   <pre><code>
   su - <b>nws</b>adm
   hdbuserstore SET DEFAULT <b>nws-db</b>:3<b>03</b>15 <b>SAPABAP1</b> <b>&lt;password of ABAP schema&gt;</b>
   </code></pre>

## <a name="next-steps"></a>Następne kroki
* [Azure maszyn wirtualnych, planowania i wdrażania dla programu SAP][planning-guide]
* [Maszyny wirtualne Azure wdrożenia SAP][deployment-guide]
* [Wdrożenia usługi Azure DBMS maszyny wirtualnej dla programu SAP][dbms-guide]
* Aby dowiedzieć się jak ustanowić wysokiej dostępności i planu odzyskiwania po awarii programu SAP HANA na platformie Azure (wystąpienia duże), zobacz [SAP HANA (duże wystąpień) wysokiej dostępności i odzyskiwania po awarii na platformie Azure](hana-overview-high-availability-disaster-recovery.md).
* Aby dowiedzieć się, jak nawiązać wysokiej dostępności i planu odzyskiwania po awarii programu SAP HANA na maszynach wirtualnych Azure, zobacz [dostępności wysokiej programu SAP HANA na maszynach wirtualnych Azure (VM)][sap-hana-ha]
