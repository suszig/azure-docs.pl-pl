---
title: "Wysoka dostępność SAP HANA na maszynach wirtualnych platformy Azure (VM) | Dokumentacja firmy Microsoft"
description: "Ustanów wysokiej dostępności SAP HANA na maszynach wirtualnych platformy Azure (VM)."
services: virtual-machines-linux
documentationcenter: 
author: MSSedusch
manager: timlt
editor: 
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 04/25/2017
ms.author: sedusch
ms.openlocfilehash: 5f6ef18e93b8f77162b3524f31cb632e1db38f80
ms.sourcegitcommit: 094061b19b0a707eace42ae47f39d7a666364d58
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/08/2017
---
# <a name="high-availability-of-sap-hana-on-azure-virtual-machines-vms"></a>Wysoka dostępność SAP HANA na maszynach wirtualnych platformy Azure (VM)

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

[hana-ha-guide-replication]:sap-hana-high-availability.md#14c19f65-b5aa-4856-9594-b81c7e4df73d
[hana-ha-guide-shared-storage]:sap-hana-high-availability.md#498de331-fa04-490b-997c-b078de457c9d

[suse-hana-ha-guide]:https://www.suse.com/docrep/documents/ir8w88iwu7/suse_linux_enterprise_server_for_sap_applications_12_sp1.pdf
[sap-swcenter]:https://launchpad.support.sap.com/#/softwarecenter
[template-multisid-db]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-db%2Fazuredeploy.json
[template-converged]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-converged%2Fazuredeploy.json

W infrastrukturze lokalnej, możesz użyć albo HANA replikacji systemu lub magazynu udostępnionego ustanowienie wysoką dostępność dla programu SAP HANA.
Aktualnie obsługiwany jest tylko skonfigurowanie HANA replikacji systemu Azure. SAP HANA replikacji składa się z jednego węzła głównego i co najmniej jeden podrzędny węzła. Węzły podrzędne zmiany danych w węźle głównym są replikowane synchronicznie lub asynchronicznie.

W tym artykule opisano sposób wdrażania maszyn wirtualnych, konfigurowanie maszyn wirtualnych, zainstaluj platformę klaster, zainstalować i skonfigurować replikacji systemu SAP HANA.
W konfiguracji przykład instalacji poleceń itp. liczby wystąpień 03 i HANA systemu identyfikator HDB jest używany.

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
* [SAP HANA SR wydajności zoptymalizowanych pod kątem scenariusza] [ suse-hana-ha-guide] zawiera wszystkie wymagane informacje, aby skonfigurować lokalne replikacji systemu SAP HANA. Użyj tego podręcznika, jako linii bazowej.

## <a name="deploying-linux"></a>Wdrażanie systemu Linux

Agent zasobów dla SAP HANA znajduje się w systemie SUSE Linux Enterprise Server aplikacje SAP.
Portalu Azure Marketplace zawiera obraz systemu SUSE Linux Enterprise Server dla programu SAP 12 aplikacji z BYOS (Bring Your własnej subskrypcji), który służy do wdrażania nowych maszyn wirtualnych.

### <a name="manual-deployment"></a>Ręczne wdrażanie

1. Tworzenie grupy zasobów
1. Tworzenie sieci wirtualnej
1. Utwórz dwa konta magazynu
1. Tworzenie zestawu dostępności  
   Zestaw aktualizacji max domeny
1. Tworzenie modułu równoważenia obciążenia (wewnętrzny)  
   Wybierz sieć Wirtualną w kroku powyżej
1. Utwórz maszynę wirtualną 1  
   Użyj co najmniej z dodatkiem SP1 z 12 SLES4SAP, w tym przykładzie używamy SLES4SAP 12 SP1 BYOS https://portal.azure.com/#create/suse-byos.sles-for-sap-byos12-sp1 obrazu  
   SLES 12 aplikacje SAP z dodatkiem SP1 (BYOS)  
   Wybierz konto magazynu 1  
   Wybierz zestaw dostępności  
1. Tworzenie maszyny wirtualnej 2  
   Użyj co najmniej z dodatkiem SP1 z 12 SLES4SAP, w tym przykładzie używamy SLES4SAP 12 SP1 BYOS https://portal.azure.com/#create/suse-byos.sles-for-sap-byos12-sp1 obrazu  
   SLES 12 aplikacje SAP z dodatkiem SP1 (BYOS)  
   Wybierz konto magazynu 2   
   Wybierz zestaw dostępności  
1. Dodawanie dysków z danymi
1. Konfigurowanie usługi równoważenia obciążenia
    1. Utwórz pulę IP frontonu
        1. Otwórz moduł równoważenia obciążenia, wybierz puli adresów IP frontonu i kliknij przycisk Dodaj
        1. Wprowadź nazwę puli adresów IP nowego serwera sieci Web (na przykład frontonu hana)
        1. Kliknij przycisk OK
        1. Po utworzeniu puli adresów IP frontonu, zanotuj jego adresu IP
    1. Tworzenie puli wewnętrznej bazy danych
        1. Otwórz moduł równoważenia obciążenia, wybierz pul zaplecza i kliknij przycisk Dodaj
        1. Wprowadź nazwę dla nowej puli wewnętrznej bazy danych (na przykład zaplecze hana)
        1. Kliknij przycisk Dodaj maszynę wirtualną
        1. Wybierz zestaw dostępności utworzoną wcześniej
        1. Wybierz maszyny wirtualne klastra SAP HANA
        1. Kliknij przycisk OK
    1. Utworzyć sondy kondycji
        1. Otwórz moduł równoważenia obciążenia, wybierz sond kondycji i kliknij przycisk Dodaj
        1. Wprowadź nazwę nowego sondy kondycji (na przykład hp hana)
        1. Wybierz protokół, port 625 TCP**03**, interwał 5 i próg złej kondycji 2
        1. Kliknij przycisk OK
    1. Tworzenie reguł równoważenia obciążenia
        1. Otwórz moduł równoważenia obciążenia, wybierz reguły równoważenia obciążenia i kliknij przycisk Dodaj
        1. Wprowadź nazwę nowej reguły modułu równoważenia obciążenia (na przykład hana-lb-3**03**15)
        1. Wybierz adres IP frontonu, puli wewnętrznej bazy danych i kondycji sondowania utworzonego wcześniej (na przykład frontonu hana)
        1. Zachowaj protokołu TCP, wprowadź port 3**03**15
        1. Zwiększ limit czasu bezczynności do 30 minut
        1. **Upewnij się, że można włączyć pływającego adresu IP**
        1. Kliknij przycisk OK
        1. Powtórz powyższe kroki dla portu 3**03**17

### <a name="deploy-with-template"></a>Wdrażanie za pomocą szablonu
Umożliwia jednego z szablonów Szybki Start w serwisie github wdrażanie wszystkich wymaganych zasobów. Szablon wdraża maszyny wirtualne, usługi równoważenia obciążenia, dostępność ustawić itd. Wykonaj następujące kroki, aby wdrożyć szablon:

1. Otwórz [szablonu bazy danych] [ template-multisid-db] lub [zbieżność szablonu] [ template-converged] w portalu Azure, jedynie tworzy szablon bazy danych Równoważenie obciążenia zasady dla bazy danych, natomiast konwergentnej szablon tworzy także reguły równoważenia obciążenia ASCS/SCS i wystąpienia Wywołujących (tylko w systemie Linux). Jeśli planujesz zainstalowanie systemu SAP NetWeaver na podstawie i również chcesz zainstalować wystąpienie ASCS/SCS na tej samej maszyny, użyj [zbieżność szablonu][template-converged].
1. Wprowadź następujące parametry
    1. Identyfikator systemu SAP  
       Wprowadź identyfikator systemu SAP systemu SAP, które chcesz zainstalować. Identyfikator będzie służyć jako prefiks dla zasobów, które zostały wdrożone.
    1. Typ stosu (dotyczy tylko jeśli używasz szablonu konwergentnej)  
       Wybierz typ SAP NetWeaver stosu
    1. Typ systemu operacyjnego  
       Wybierz jedną z dystrybucje systemu Linux. Na przykład wybierz SLES 12 BYOS
    1. Typ bazy danych  
       Wybierz HANA
    1. Rozmiar systemu SAP  
       Ilość protokoły SAP zapewnia nowy system. Jeśli nie wiadomo jak wiele protokoły SAP wymaga systemu, należy skontaktować się z partnerem technologii SAP lub Integrator systemu
    1. Dostępność systemu  
       Wybierz opcję wysokiej dostępności
    1. Nazwa użytkownika i hasło administratora  
       Tworzony jest nowy użytkownik, który może służyć do logowania się do komputera.
    1. Nowy lub istniejący podsieci  
       Określa, czy należy utworzyć nową sieć wirtualną i podsieć lub istniejącą podsieć powinna być używana. Jeśli masz już sieć wirtualną, która jest połączona z siecią lokalną, wybierz istniejącą.
    1. Identyfikator podsieci  
    Identyfikator podsieci, do której maszyny wirtualne powinny być podłączone do. Wybierz podsieć sieci VPN lub usługi Express Route wirtualnej sieci lokalnej nawiązać połączenia z maszyną wirtualną. Identyfikator zwykle wygląda /subscriptions/`<subscription ID`> /resourceGroups/`<resource group name`> /providers/Microsoft.Network/virtualNetworks/`<virtual network name`> /subnets/`<subnet name`>

## <a name="setting-up-linux-ha"></a>Konfigurowanie HA systemu Linux

Następujące elementy są poprzedzane prefiksem [A] - mają zastosowanie do wszystkich węzłów [1] — dotyczy to tylko węzeł 1 lub [2] — dotyczy to tylko węzeł 2.

1. [A] SLES dla SAP BYOS tylko - zarejestrować SLES, aby można było użyć repozytoria
1. [A] SLES dla SAP BYOS tylko — Dodaj moduł chmury publicznej
1. [A] Aktualizacja SLES
    ```bash
    sudo zypper update

    ```

1. [1] dostęp ssh
    ```bash
    sudo ssh-keygen -tdsa
    
    # Enter file in which to save the key (/root/.ssh/id_dsa): -> ENTER
    # Enter passphrase (empty for no passphrase): -> ENTER
    # Enter same passphrase again: -> ENTER
    
    # copy the public key
    sudo cat /root/.ssh/id_dsa.pub
    ```

2. [2] dostęp ssh
    ```bash
    sudo ssh-keygen -tdsa

    # insert the public key you copied in the last step into the authorized keys file on the second server
    sudo vi /root/.ssh/authorized_keys
    
    # Enter file in which to save the key (/root/.ssh/id_dsa): -> ENTER
    # Enter passphrase (empty for no passphrase): -> ENTER
    # Enter same passphrase again: -> ENTER
    
    # copy the public key    
    sudo cat /root/.ssh/id_dsa.pub
    ```

1. [1] dostęp ssh
    ```bash
    # insert the public key you copied in the last step into the authorized keys file on the first server
    sudo vi /root/.ssh/authorized_keys
    
    ```

1. [A] należy zainstalować rozszerzenie wysokiej dostępności
    ```bash
    sudo zypper install sle-ha-release fence-agents
    
    ```

1. [A] układ dysku instalacji
    1. LVM  
    Ogólnie zaleca LVM dla woluminów, które przechowują dane i pliki dziennika. W poniższym przykładzie założenia, że maszyny wirtualne czterech dyskach danych dołączonych, które powinny być używane do tworzenia dwa woluminy.
        * Utwórz woluminy fizyczne dla wszystkich dysków, które chcesz użyć.
    <pre><code>
    sudo pvcreate /dev/sdc
    sudo pvcreate /dev/sdd
    sudo pvcreate /dev/sde
    sudo pvcreate /dev/sdf
    </code></pre>
        * Utwórz grupę woluminu plików danych, jedna grupa woluminu dla plików dziennika i jeden do udostępnionego katalogu SAP HANA
    <pre><code>
    sudo vgcreate vg_hana_data /dev/sdc /dev/sdd
    sudo vgcreate vg_hana_log /dev/sde
    sudo vgcreate vg_hana_shared /dev/sdf
    </code></pre>
        * Utwórz woluminy logiczne
    <pre><code>
    sudo lvcreate -l 100%FREE -n hana_data vg_hana_data
    sudo lvcreate -l 100%FREE -n hana_log vg_hana_log
    sudo lvcreate -l 100%FREE -n hana_shared vg_hana_shared
    sudo mkfs.xfs /dev/vg_hana_data/hana_data
    sudo mkfs.xfs /dev/vg_hana_log/hana_log
    sudo mkfs.xfs /dev/vg_hana_shared/hana_shared
    </code></pre>
        * Utwórz katalogi instalacji i skopiuj identyfikator UUID wszystkie woluminy logiczne
    <pre><code>
    sudo mkdir -p /hana/data
    sudo mkdir -p /hana/log
    sudo mkdir -p /hana/shared
    # write down the ID of /dev/vg_hana_data/hana_data, /dev/vg_hana_log/hana_log and /dev/vg_hana_shared/hana_shared
    sudo blkid
    </code></pre>
        * Tworzenie wpisów fstab trzy logiczne woluminów
    <pre><code>
    sudo vi /etc/fstab
    </code></pre>
    Wstaw ten wiersz, aby /etc/fstab
    <pre><code>
    /dev/disk/by-uuid/<b>&lt;UUID of /dev/vg_hana_data/hana_data&gt;</b> /hana/data xfs  defaults,nofail  0  2
    /dev/disk/by-uuid/<b>&lt;UUID of /dev/vg_hana_log/hana_log&gt;</b> /hana/log xfs  defaults,nofail  0  2
    /dev/disk/by-uuid/<b>&lt;UUID of /dev/vg_hana_shared/hana_shared&gt;</b> /hana/shared xfs  defaults,nofail  0  2
    </code></pre>
        * Zainstaluj nowe woluminy
    <pre><code>
    sudo mount -a
    </code></pre>
    1. Zwykły dysków  
       Dla małych lub demonstracyjnych systemów, możesz umieścić HANA plików danych i dziennika na jednym dysku. Poniższe polecenia utworzyć partycję na /dev/sdc i sformatuj go przy xfs.
    ```bash
    sudo fdisk /dev/sdc
    sudo mkfs.xfs /dev/sdc1
    
    # <a name="write-down-the-id-of-devsdc1"></a>Zanotuj identyfikator /dev/sdc1
    sudo/sbin/blkid sudo vi/etc/fstab
    ```

    Insert this line to /etc/fstab
    <pre><code>
    /dev/disk/by-uuid/<b>&lt;UUID&gt;</b> /hana xfs  defaults,nofail  0  2
    </code></pre>

    Create the target directory and mount the disk.

    ```bash
    sudo mkdir /hana
    sudo mount -a
    ```

1. [A] ustawień rozpoznawania nazwy hosta dla wszystkich hostów  
    Można użyć serwera DNS lub zmodyfikować/etc/hosts na wszystkich węzłach. Ten przykład przedstawia sposób użycia pliku/etc/hosts.
   Zastąp adres IP i nazwy hosta w poniższych poleceniach
    ```bash
    sudo vi /etc/hosts
    ```
    Wstaw następujące wiersze do/etc/hosts. Zmienianie adresu IP i nazwy hosta do danego środowiska    
    
    <pre><code>
    <b>&lt;IP address of host 1&gt; &lt;hostname of host 1&gt;</b>
    <b>&lt;IP address of host 2&gt; &lt;hostname of host 2&gt;</b>
    </code></pre>

1. [1] instalacji klastra
    ```bash
    sudo ha-cluster-init
    
    # Do you want to continue anyway? [y/N] -> y
    # Network address to bind to (e.g.: 192.168.1.0) [10.79.227.0] -> ENTER
    # Multicast address (e.g.: 239.x.x.x) [239.174.218.125] -> ENTER
    # Multicast port [5405] -> ENTER
    # Do you wish to use SBD? [y/N] -> N
    # Do you wish to configure an administration IP? [y/N] -> N
    ```
        
1. [2] dodać węzeł do klastra
    ```bash
    sudo ha-cluster-join
        
    # WARNING: NTP is not configured to start at system boot.
    # WARNING: No watchdog device found. If SBD is used, the cluster will be unable to start without a watchdog.
    # Do you want to continue anyway? [y/N] -> y
    # IP address or hostname of existing node (e.g.: 192.168.1.1) [] -> IP address of node 1 e.g. 10.0.0.5
    # /root/.ssh/id_dsa already exists - overwrite? [y/N] N
    ```

1. [A] Zmień hacluster hasło do tego samego hasła
    ```bash
    sudo passwd hacluster
    
    ```

1. [A] skonfiguruj corosync Użyj innego transportu, a następnie dodaj wstawienia. Klaster nie będą działać inaczej.
    ```bash
    sudo vi /etc/corosync/corosync.conf    
    
    ```

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
        ring0_addr:     < ip address of node 1 >
      }
      node {
        ring0_addr:     < ip address of node 2 > 
      } 
    }</b>
    logging {
      [...]
    </code></pre>

    Następnie uruchom ponownie usługę corosync

    ```bash
    sudo service corosync restart
    
    ```

1. [A] zainstalować pakiety HANA HA  
    ```bash
    sudo zypper install SAPHanaSR
    
    ```

## <a name="installing-sap-hana"></a>Instalowanie programu SAP HANA

Wykonaj rozdziału 4 [przewodnik SAP HANA SR wydajności zoptymalizowanych pod kątem scenariusza] [ suse-hana-ha-guide] do zainstalowania replikacji systemu SAP HANA.

1. [A] uruchamiania z dysku DVD HANA hdblcm
    * Wybierz opcję instalacji -> 1
    * 1 -> Wybierz dodatkowe składniki do instalacji
    * Wprowadź ścieżkę instalacji [/ hana/udostępnione]: -> ENTER
    * Wprowadź nazwę hosta lokalnego [.]: -> ENTER
    * Czy chcesz dodać dodatkowe hosty systemu? (t/n) [n]: -> ENTER
    * Wprowadź identyfikator systemu SAP HANA:<SID of HANA e.g. HDB>
    * Wprowadź liczby wystąpień [00]:   
  Liczby wystąpień HANA. Użyj 03, jeśli używany szablon Azure i stosować w powyższym przykładzie
    * Wybierz tryb bazy danych / wprowadź indeks [1]: -> ENTER
    * Użycie systemu wybierz / Wprowadź indeks [4]:  
  Wybierz system użycia
    * Wprowadź lokalizację woluminów danych [/ hana/data/HDB]: -> ENTER
    * Wprowadź lokalizację woluminy dziennika [/ HDB-hana/dziennika]: -> ENTER
    * Ogranicz maksymalną wielkość pamięci alokacji? [n]: -> ENTER
    * Wprowadź nazwę hosta certyfikat dla hosta "..." [...]: -> ENTER
    * Wprowadź nazwę użytkownika agenta hosta SAP (sapadm) hasło:
    * Upewnij się, SAP hosta Agent użytkownika (sapadm) hasło:
    * Wprowadź administratora systemu (hdbadm) hasło:
    * Upewnij się, Administrator systemu (hdbadm) hasło:
    * Wprowadź katalog macierzysty administratora systemu [/ usr/sap/HDB/głównej]: -> ENTER
    * Wprowadź powłoka logowania administratora systemu [/ bin/sh]: -> ENTER
    * Wprowadź identyfikator użytkownika administratora [1001]: -> ENTER
    * Wprowadź identyfikator użytkownika grupy (sapsys) [79]: -> ENTER
    * Wprowadź hasło użytkownika (SYSTEM) bazy danych:
    * Potwierdź hasło użytkownika (SYSTEM) bazy danych:
    * Uruchom ponownie system po ponownym uruchomieniu komputera? [n]: -> ENTER
    * Czy chcesz kontynuować? (t/n):  
  Sprawdź poprawność podsumowania, a następnie wprowadź t, aby kontynuować
1. [A] Agent hosta uaktualnienia SAP  
  Pobierz najnowsze archiwum SAP Agent hosta z [SAP Softwarecenter] [ sap-swcenter] i uruchom następujące polecenie, aby uaktualnić agenta. Zastąp ścieżki do archiwum, aby wskazywał pobranego pliku.
    ```bash
    sudo /usr/sap/hostctrl/exe/saphostexec -upgrade -archive <path to SAP Host Agent SAR>
    ```

1. [1] utworzyć HANA replikacji (jako główny)  
    Uruchom następujące polecenie. Upewnij się, że Zastąp bold ciągów (HANA systemu identyfikator HDB i numer wystąpienia 03) z wartościami instalację programu SAP HANA.
    <pre><code>
    PATH="$PATH:/usr/sap/<b>HDB</b>/HDB<b>03</b>/exe"
    hdbsql -u system -i <b>03</b> 'CREATE USER <b>hdb</b>hasync PASSWORD "<b>passwd</b>"' 
    hdbsql -u system -i <b>03</b> 'GRANT DATA ADMIN TO <b>hdb</b>hasync' 
    hdbsql -u system -i <b>03</b> 'ALTER USER <b>hdb</b>hasync DISABLE PASSWORD LIFETIME' 
    </code></pre>

1. [A] utworzenia wpisu magazynu kluczy (jako główny)
    <pre><code>
    PATH="$PATH:/usr/sap/<b>HDB</b>/HDB<b>03</b>/exe"
    hdbuserstore SET <b>hdb</b>haloc localhost:3<b>03</b>15 <b>hdb</b>hasync <b>passwd</b>
    </code></pre>
1. [1] kopii zapasowej bazy danych (jako główny)
    <pre><code>
    PATH="$PATH:/usr/sap/<b>HDB</b>/HDB<b>03</b>/exe"
    hdbsql -u system -i <b>03</b> "BACKUP DATA USING FILE ('<b>initialbackup</b>')" 
    </code></pre>
1. [1] Przełącz użytkownika sapsid (na przykład hdbadm) i utworzyć lokacji głównej.
    <pre><code>
    su - <b>hdb</b>adm
    hdbnsutil -sr_enable –-name=<b>SITE1</b>
    </code></pre>
1. [2] Przełącz użytkownika sapsid (na przykład hdbadm) i utworzyć lokację dodatkową.
    <pre><code>
    su - <b>hdb</b>adm
    sapcontrol -nr <b>03</b> -function StopWait 600 10
    hdbnsutil -sr_register --remoteHost=<b>saphanavm1</b> --remoteInstance=<b>03</b> --replicationMode=sync --name=<b>SITE2</b> 
    </code></pre>

## <a name="configure-cluster-framework"></a>Konfigurowanie klastra Framework

Zmiany ustawień domyślnych

<pre>
sudo vi crm-defaults.txt
# enter the following to crm-defaults.txt
<code>
property $id="cib-bootstrap-options" \
  no-quorum-policy="ignore" \
  stonith-enabled="true" \
  stonith-action="reboot" \
  stonith-timeout="150s"
rsc_defaults $id="rsc-options" \
  resource-stickiness="1000" \
  migration-threshold="5000"
op_defaults $id="op-options" \
  timeout="600"
</code>

# <a name="now-we-load-the-file-to-the-cluster"></a>Teraz możemy załadować pliku do klastra
sudo crm Konfigurowanie aktualizacji obciążenia crm-defaults.txt
</pre>

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

Po można edytowane uprawnienia dla maszyn wirtualnych, możesz skonfigurować urządzenia STONITH w klastrze.

<pre>
sudo vi crm-fencing.txt
# enter the following to crm-fencing.txt
# replace the bold string with your subscription ID, resource group, tenant ID, service principal ID and password
<code>
primitive rsc_st_azure_1 stonith:fence_azure_arm \
    params subscriptionId="<b>subscription ID</b>" resourceGroup="<b>resource group</b>" tenantId="<b>tenant ID</b>" login="<b>login ID</b>" passwd="<b>password</b>"

primitive rsc_st_azure_2 stonith:fence_azure_arm \
    params subscriptionId="<b>subscription ID</b>" resourceGroup="<b>resource group</b>" tenantId="<b>tenant ID</b>" login="<b>login ID</b>" passwd="<b>password</b>"

colocation col_st_azure -2000: rsc_st_azure_1:Started rsc_st_azure_2:Started
</code>

# <a name="now-we-load-the-file-to-the-cluster"></a>Teraz możemy załadować pliku do klastra
sudo crm Konfigurowanie aktualizacji obciążenia crm-fencing.txt
</pre>

### <a name="create-sap-hana-resources"></a>Utwórz zasoby SAP HANA

<pre>
sudo vi crm-saphanatop.txt
# enter the following to crm-saphana.txt
# replace the bold string with your instance number and HANA system ID
<code>
primitive rsc_SAPHanaTopology_<b>HDB</b>_HDB<b>03</b> ocf:suse:SAPHanaTopology \
    operations $id="rsc_sap2_<b>HDB</b>_HDB<b>03</b>-operations" \
    op monitor interval="10" timeout="600" \
    op start interval="0" timeout="600" \
    op stop interval="0" timeout="300" \
    params SID="<b>HDB</b>" InstanceNumber="<b>03</b>"

clone cln_SAPHanaTopology_<b>HDB</b>_HDB<b>03</b> rsc_SAPHanaTopology_<b>HDB</b>_HDB<b>03</b> \
    meta is-managed="true" clone-node-max="1" target-role="Started" interleave="true"
</code>

# <a name="now-we-load-the-file-to-the-cluster"></a>Teraz możemy załadować pliku do klastra
sudo crm Konfigurowanie aktualizacji obciążenia crm-saphanatop.txt
</pre>

<pre>
sudo vi crm-saphana.txt
# enter the following to crm-saphana.txt
# replace the bold string with your instance number, HANA system ID and the frontend IP address of the Azure load balancer. 
<code>
primitive rsc_SAPHana_<b>HDB</b>_HDB<b>03</b> ocf:suse:SAPHana \
    operations $id="rsc_sap_<b>HDB</b>_HDB<b>03</b>-operations" \
    op start interval="0" timeout="3600" \
    op stop interval="0" timeout="3600" \
    op promote interval="0" timeout="3600" \
    op monitor interval="60" role="Master" timeout="700" \
    op monitor interval="61" role="Slave" timeout="700" \
    params SID="<b>HDB</b>" InstanceNumber="<b>03</b>" PREFER_SITE_TAKEOVER="true" \
    DUPLICATE_PRIMARY_TIMEOUT="7200" AUTOMATED_REGISTER="false"

ms msl_SAPHana_<b>HDB</b>_HDB<b>03</b> rsc_SAPHana_<b>HDB</b>_HDB<b>03</b> \
    meta is-managed="true" notify="true" clone-max="2" clone-node-max="1" \
    target-role="Started" interleave="true"

primitive rsc_ip_<b>HDB</b>_HDB<b>03</b> ocf:heartbeat:IPaddr2 \ 
    meta target-role="Started" is-managed="true" \ 
    operations $id="rsc_ip_<b>HDB</b>_HDB<b>03</b>-operations" \ 
    op monitor interval="10s" timeout="20s" \ 
    params ip="<b>10.0.0.21</b>" 
primitive rsc_nc_<b>HDB</b>_HDB<b>03</b> anything \ 
    params binfile="/usr/bin/nc" cmdline_options="-l -k 625<b>03</b>" \ 
    op monitor timeout=20s interval=10 depth=0 
group g_ip_<b>HDB</b>_HDB<b>03</b> rsc_ip_<b>HDB</b>_HDB<b>03</b> rsc_nc_<b>HDB</b>_HDB<b>03</b>
 
colocation col_saphana_ip_<b>HDB</b>_HDB<b>03</b> 2000: g_ip_<b>HDB</b>_HDB<b>03</b>:Started \ 
    msl_SAPHana_<b>HDB</b>_HDB<b>03</b>:Master  
order ord_SAPHana_<b>HDB</b>_HDB<b>03</b> 2000: cln_SAPHanaTopology_<b>HDB</b>_HDB<b>03</b> \ 
    msl_SAPHana_<b>HDB</b>_HDB<b>03</b>
</code>

# <a name="now-we-load-the-file-to-the-cluster"></a>Teraz możemy załadować pliku do klastra
sudo crm Konfigurowanie aktualizacji obciążenia crm-saphana.txt
</pre>

### <a name="test-cluster-setup"></a>Konfiguracja klastra testu
Rozdział opisano, jak można przetestować konfigurację. Każdy test założono, że są głównymi i wzorzec SAP HANA działa na saphanavm1 maszyny wirtualnej.

#### <a name="fencing-test"></a>Test ogrodzenia

Można przetestować instalacji agenta ogrodzenia wyłączając interfejs sieciowy na saphanavm1 węzła.

<pre><code>
sudo ifdown eth0
</code></pre>

Teraz Pobierz ponownie uruchomiona lub zatrzymana w zależności od konfiguracji klastra maszyny wirtualnej.
Jeśli ustawisz akcji stonith wyłączone, maszyna wirtualna zostanie zatrzymana i zasoby są migrowane do uruchomionej maszyny wirtualnej.

Po ponownym uruchomieniu maszyny wirtualnej zasobów SAP HANA nie zostanie uruchomiony jako dodatkowej po ustawieniu AUTOMATED_REGISTER = "false". W takim przypadku należy skonfigurować wystąpienie HANA jako dodatkowej, wykonując następujące polecenie:

<pre><code>
su - <b>hdb</b>adm

# Stop the HANA instance just in case it is running
sapcontrol -nr <b>03</b> -function StopWait 600 10
hdbnsutil -sr_register --remoteHost=<b>saphanavm2</b> --remoteInstance=<b>03</b> --replicationMode=sync --name=<b>SITE1</b>

# switch back to root and cleanup the failed state
exit
crm resource cleanup msl_SAPHana_<b>HDB</b>_HDB<b>03</b> <b>saphanavm1</b>
</code></pre>

#### <a name="testing-a-manual-failover"></a>Testowania ręcznego przełączania trybu failover

Ręcznego przełączania trybu failover można przetestować, zatrzymując usługę rozrusznik na saphanavm1 węzła.
<pre><code>
service pacemaker stop
</code></pre>

Po przejściu w tryb failover należy ponownie uruchomić usługę. Zasób SAP HANA saphanavm1 nie zostanie uruchomiony jako dodatkowej po ustawieniu AUTOMATED_REGISTER = "false". W takim przypadku należy skonfigurować wystąpienie HANA jako dodatkowej, wykonując następujące polecenie:

<pre><code>
service pacemaker start
su - <b>hdb</b>adm

# Stop the HANA instance just in case it is running
sapcontrol -nr <b>03</b> -function StopWait 600 10
hdbnsutil -sr_register --remoteHost=<b>saphanavm2</b> --remoteInstance=<b>03</b> --replicationMode=sync --name=<b>SITE1</b> 


# switch back to root and cleanup the failed state
exit
crm resource cleanup msl_SAPHana_<b>HDB</b>_HDB<b>03</b> <b>saphanavm1</b>
</code></pre>

#### <a name="testing-a-migration"></a>Testowanie migracji

Węzeł główny SAP HANA można migrować, wykonując następujące polecenie
<pre><code>
crm resource migrate msl_SAPHana_<b>HDB</b>_HDB<b>03</b> <b>saphanavm2</b>
crm resource migrate g_ip_<b>HDB</b>_HDB<b>03</b> <b>saphanavm2</b>
</code></pre>

To należy zmigrować węzła głównego SAP HANA i grupy, która zawiera wirtualny adres IP saphanavm2.
Zasób SAP HANA saphanavm1 nie zostanie uruchomiony jako dodatkowej po ustawieniu AUTOMATED_REGISTER = "false". W takim przypadku należy skonfigurować wystąpienie HANA jako dodatkowej, wykonując następujące polecenie:

<pre><code>
su - <b>hdb</b>adm

# Stop the HANA instance just in case it is running
sapcontrol -nr <b>03</b> -function StopWait 600 10
hdbnsutil -sr_register --remoteHost=<b>saphanavm2</b> --remoteInstance=<b>03</b> --replicationMode=sync --name=<b>SITE1</b> 
</code></pre>

Migracja tworzy ograniczenia lokalizacji, które muszą zostać usunięte ponownie.

<pre><code>
crm configure edited

# delete location constraints that are named like the following contraint. You should have two constraints, one for the SAP HANA resource and one for the IP address group.
location cli-prefer-g_ip_<b>HDB</b>_HDB<b>03</b> g_ip_<b>HDB</b>_HDB<b>03</b> role=Started inf: <b>saphanavm2</b>
</code></pre>

Należy wyczyścić stanu zasobu węzła pomocniczego

<pre><code>
# switch back to root and cleanup the failed state
exit
crm resource cleanup msl_SAPHana_<b>HDB</b>_HDB<b>03</b> <b>saphanavm1</b>
</code></pre>

## <a name="next-steps"></a>Następne kroki
* [Azure maszyn wirtualnych, planowania i wdrażania dla programu SAP][planning-guide]
* [Maszyny wirtualne Azure wdrożenia SAP][deployment-guide]
* [Wdrożenia usługi Azure DBMS maszyny wirtualnej dla programu SAP][dbms-guide]
* Aby dowiedzieć się jak ustanowić wysokiej dostępności i planu odzyskiwania po awarii programu SAP HANA na platformie Azure (wystąpienia duże), zobacz [SAP HANA (duże wystąpień) wysokiej dostępności i odzyskiwania po awarii na platformie Azure](hana-overview-high-availability-disaster-recovery.md). 
