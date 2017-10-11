---
title: Niepowodzenie tworzenia kopii w maszynach wirtualnych VMware z platformy Azure w klasycznym portalu | Dokumentacja firmy Microsoft
description: "Informacje o niepowodzeniu do lokacji lokalnej po przejściu w tryb failover maszyn wirtualnych VMware i serwerów fizycznych do platformy Azure."
services: site-recovery
documentationcenter: 
author: ruturaj
manager: mkjain
editor: 
ms.assetid: 7ca86e21-7a5d-45ab-8f4b-e42da90f199a
ms.service: site-recovery
ms.devlang: na
ms.tgt_pltfrm: na
ms.topic: article
ms.workload: storage-backup-recovery
ms.date: 06/05/2017
ms.author: ruturajd
ms.openlocfilehash: 82d5eb7fd13b1e9700a3e9bc2d30775e9c129749
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/03/2017
---
# <a name="fail-back-vmware-virtual-machines-and-physical-servers-to-the-on-premises-site-classic-portal"></a>Nie wstecz maszyn wirtualnych VMware lub serwerów fizycznych do lokacji lokalnej (klasyczny portal)
> [!div class="op_single_selector"]
> * [Azure Portal](site-recovery-failback-azure-to-vmware.md)
> * [Klasyczny portal Azure](site-recovery-failback-azure-to-vmware-classic.md)
> * [Klasyczny Portal Azure (starsze)](site-recovery-failback-azure-to-vmware-classic-legacy.md)
>
>

W tym artykule opisano, jak niepowodzenie wstecz maszyn wirtualnych platformy Azure z platformy Azure do lokacji lokalnej. Wykonaj instrukcje w tym artykule, kiedy zechcesz się nie powieść z powrotem z maszyn wirtualnych VMware lub systemem Windows lub Linux, serwerów fizycznych, po zostały one przejścia w tryb failover z lokalnej lokacji do Azure za pomocą tej [samouczek](site-recovery-vmware-to-azure-classic.md).

## <a name="overview"></a>Omówienie
Ten diagram przedstawia architekturę powrotu po awarii dla tego scenariusza.

Użyj tej architektury, gdy serwer przetwarzania jest lokalnie i za pomocą usługi ExpressRoute.

![](./media/site-recovery-failback-azure-to-vmware-classic/architecture.png)

Użyj tej architektury, gdy serwer przetwarzania jest na platformie Azure, sieci VPN lub połączenia ExpressRoute.

![](./media/site-recovery-failback-azure-to-vmware-classic/architecture2.png)

Aby wyświetlić pełną listę portów i powrót po awarii architechture diagram dotyczą obraz poniżej

![](./media/site-recovery-failback-azure-to-vmware-classic/Failover-Failback.png)

Oto, jak działa powrotu po awarii:

* Po przez Ciebie nieudanych na platformie Azure, możesz powrót po awarii do lokacji sieci lokalnej w kilku etapach:
  * **Etap 1**: Wznów maszynach wirtualnych platformy Azure, aby rozpoczynały się replikację z powrotem do maszyn wirtualnych VMware uruchomiona w lokacji lokalnej. Włączanie przełączonej przenosi maszyny Wirtualnej do grupy ochrony powrotu po awarii, który został utworzony automatycznie po utworzeniu pierwotnie trybu failover grupy ochrony. Jeśli dodano grupę ochrony, trybu failover planu odzyskiwania następnie powrotu po awarii grupy ochrony została również automatycznie dodawane do planu.  Podczas ponownej ochrony można określić sposób zaplanować wykonaj powrót po awarii.
  * **Etap 2**: po maszynach wirtualnych platformy Azure jest replikowana do lokacji lokalnej, należy uruchomić za pośrednictwem błędu awarii z platformy Azure.
  * **Etap 3**: po danych nie powiodło się ponownie, aby rozpoczynały się replikację do platformy Azure Wznów lokalnych maszyn wirtualnych, których nie powiodła się wstecz do.


  [!VIDEO https://channel9.msdn.com/Blogs/Azure/Enhanced-VMware-to-Azure-Failback/player]

### <a name="failback-to-the-original-or-alternate-location"></a>Powrót po awarii do lokalizacji oryginalnej lub alternatywnej
Jeśli zostanie przełączone do trybu failover maszyny Wirtualnej VMware mogą nie do tej samej źródłowej maszyny Wirtualnej Jeśli nadal istnieje lokalnie. W tym scenariuszu tylko zmiany różnicowe nie powiedzie się ponownie. Należy pamiętać, że:

* Jeśli nie powiodła się za pośrednictwem serwerów fizycznych następnie powrotu po awarii jest zawsze nowej maszyny Wirtualnej VMware.
  * Przed powrotem niepowodzenie fizyczny komputer należy pamiętać, że
  * Fizyczny komputer chroniony zostanie wróć jako maszynę wirtualną, gdy nie powiodło się za pośrednictwem platformy Azure z powrotem do programu VMware
  * Upewnij się, że można wykryć co najmniej jeden główny cel serwera oraz konieczne hostów ESX/ESXi, do których należy powrotu po awarii.
* Jeśli nie zostanie ponownie oryginalna maszyna wirtualna wymagane są następujące:

  * Jeśli maszyna wirtualna jest zarządzany przez serwer vCenter hosta ESX docelowego elementu głównego powinien mieć dostęp do magazynu danych maszyn wirtualnych.
  * Jeśli maszyna wirtualna znajduje się na hoście ESX, ale nie jest zarządzany przez vCenter dysku twardego maszyny wirtualnej musi być w dostępny magazyn danych przez hosta na głównym serwerze Docelowym.
  * Jeśli maszyna wirtualna znajduje się na hoście ESX, a nie używa vCenter następnie należy wykonać odnajdywania hosta ESX głównym serwerze Docelowym przed możesz Włącz ponownie ochronę. Dotyczy to serwerów fizycznych wstecz powrotu po awarii zbyt.
  * Inną opcją (jeśli istnieje lokalne maszyny Wirtualnej) jest go usunąć przed wykonaniem powrotu po awarii. Następnie powrotu po awarii spowoduje to utworzenie nowej maszyny Wirtualnej na tym samym hoście jako główny serwer docelowy host ESX.
* Podczas powrotu do lokalizacji alternatywnej dane zostaną odzyskane do tego samego magazynu danych i tego samego hosta ESX, który jest używany przez lokalny główny serwer docelowy.

## <a name="prerequisites"></a>Wymagania wstępne
* Będziesz potrzebować środowisku VMware, aby zakończyć się niepowodzeniem kopię maszyn wirtualnych VMware oraz serwerach fizycznych. Niepowodzenie fizyczny serwer nie jest obsługiwana.
* Aby można było powrócić powinien utworzono sieci platformy Azure po początkowym skonfigurowaniu ochrony. Powrót po awarii wymaga połączenia sieci VPN lub usługi ExpressRoute z sieci platformy Azure, w którym znajdują się w witrynie lokalnej maszynach wirtualnych platformy Azure.
* Maszyny wirtualne mają być powrót po awarii do są zarządzane przez serwer vCenter, które będą potrzebne, aby upewnić się, masz wymagane uprawnienia dla odnajdywania maszyn wirtualnych na serwer vCenter. [Dowiedz się więcej](site-recovery-vmware-to-azure-classic.md).
* Jeśli migawki maszyny Wirtualnej przełączonej zakończy się niepowodzeniem. Możesz usunąć migawki lub dysków.
* Zanim nie zostanie ponownie należy utworzyć wiele składników:
  * **Tworzenie serwera przetwarzania na platformie Azure**. Jest to maszyny Wirtualnej platformy Azure musisz utworzyć i zachować uruchomiona podczas powrotu po awarii. Po zakończeniu powrotu po awarii można usunąć maszynę.
  * **Tworzenie głównego serwera docelowego**: główny serwer docelowy wysyła i odbiera dane powrotu po awarii. Serwer zarządzania został utworzony lokalnie ma głównego serwera docelowego instalowane domyślnie. W zależności od ilości ruchu Wstecz nie powiodło się może być konieczne do utworzenia oddzielnych głównego serwera docelowego do powrotu po awarii.
  * Jeśli chcesz utworzyć dodatkowe główny serwer docelowy z systemem Linux, należy skonfigurować maszyny Wirtualnej systemu Linux przed zainstalowaniem główny serwer docelowy, zgodnie z poniższym opisem.
* Serwer konfiguracji jest wymagana lokalnymi podczas powrotu po awarii. Podczas powrotu po awarii maszyna wirtualna musi istnieć w bazie danych serwera konfiguracji, niepowodzeniem, które powrotu po awarii nie powiódł się. Dlatego upewnij się, że wykonujesz regularnie zaplanowanej kopii zapasowej serwera. W przypadku awarii należy go przywrócić z tego samego adresu IP, aby działały powrotu po awarii.

## <a name="set-up-the-process-server-in-azure"></a>Skonfiguruj serwer przetwarzania na platformie Azure
Należy zainstalować serwer przetwarzania na platformie Azure, dzięki czemu maszyny wirtualne Azure może wysłać dane do lokalny główny serwer docelowy.

1. W portalu usługi Site Recovery > **serwery konfiguracji** wybierz, aby dodać nowy serwer procesu.

   ![](./media/site-recovery-failback-azure-to-vmware-classic/ps1.png)
2. Określ nazwę serwera procesu, a następnie wprowadź nazwę i hasło, które będzie używane do łączenia się z maszyną Wirtualną Azure jako administrator. W **serwera konfiguracji** wybierz serwer zarządzania lokalnymi i określ sieć platformy Azure, w którym powinny zostać wdrożone serwer przetwarzania. Powinno to być sieć, w którym znajdują się maszyny wirtualne platformy Azure. Określ unikatowy adres IP podsieci, wybierz i rozpocząć wdrażanie.

   ![](./media/site-recovery-failback-azure-to-vmware-classic/ps2.png)

   Zostanie wyzwolone zadanie do wdrożenia serwera przetwarzania

   ![](./media/site-recovery-failback-azure-to-vmware-classic/ps3.png)

   Po wdrożeniu serwera przetwarzania na platformie Azure możesz zalogować się przy użyciu poświadczeń, które określiłeś. Podczas pierwszego logowania w oknie dialogowym serwera proces zostanie uruchomiony. Wpisz adres IP serwera zarządzania lokalnymi i jego hasło. Pozostaw domyślne ustawienie portu 443. Można także pozostawić domyślne 9443 portu do replikacji danych, chyba że w szczególności należy zmodyfikować to ustawienie podczas konfigurowania lokalnego serwera zarządzania.

   > [!NOTE]
   > Serwer nie będzie wyświetlany w obszarze **właściwości maszyny Wirtualnej**. Tylko jest widoczny w **serwerów** kartę na serwerze zarządzania, który został zarejestrowany. Może upłynąć o 10 – 15 minutach serwera przetwarzania i pojawienie się.
   >
   >

## <a name="set-up-the-master-target-server-on-premises"></a>Konfigurowanie głównego celu serwera lokalnego
Główny serwer docelowy odbiera dane powrotu po awarii. Główny serwer docelowy jest automatycznie instalowany na serwerze zarządzania lokalnymi, ale w przypadku powrotu po awarii wstecz dużych ilości danych może być konieczne konfigurowanie dodatkowych główny serwer docelowy. Wykonaj następujące czynności w następujący sposób:

> [!NOTE]
> Jeśli chcesz zainstalować głównego serwera docelowego w systemie Linux, postępuj zgodnie z instrukcjami w następnej procedurze.
>
>

1. Jeśli instalujesz głównego serwera docelowego w systemie Windows, należy otworzyć stronę Szybki Start z maszyny Wirtualnej, na którym jest instalowany serwer główny serwer docelowy i Pobierz plik instalacyjny dla Kreatora Unified instalacja usługi Azure Site Recovery.
2. Uruchom Instalatora, a w **przed rozpoczęciem** wybierz **dodać serwery dodatkowych procesów na potrzeby skalowania wdrożenia**.
3. Ukończ pracę kreatora w taki sam sposób jak podczas możesz [Konfigurowanie serwera zarządzania](site-recovery-vmware-to-azure-classic.md). Na **szczegóły konfiguracji serwera** Określ adres IP tego głównego serwera docelowego, a hasło do maszyny Wirtualnej.

### <a name="set-up-a-linux-vm-as-the-master-target-server"></a>Skonfiguruj Maszynę wirtualną systemu Linux jako główny serwer docelowy
Aby skonfigurować serwer zarządzania systemem główny serwer docelowy jako maszyny Wirtualnej systemu Linux należy zainstalować %) 6.6 S minimalnego systemu operacyjnego pobrać identyfikatorów SCSI dla każdego dysku twardego SCSI, zainstalować niektóre dodatkowe pakiety i zastosowanie niektórych zmian niestandardowych.

#### <a name="install-centos-66"></a>Zainstaluj CentOS 6.6
1. Zainstaluj CentOS 6.6 minimalny system operacyjny na serwerze zarządzania maszyny Wirtualnej. Przechowuj plik ISO w stacji dysków DVD i uruchom komputer. Pomiń testowania nośnik, zaznacz US English w języku, wybierz opcję **podstawowe urządzeń magazynujących**, sprawdź, czy dysk twardy nie zawiera wszystkich ważnych danych i kliknij przycisk **tak**, odrzucenie danych. Wprowadź nazwę hosta serwera zarządzania i wybierz karty sieciowej serwera.  W **edycji systemu** okno dialogowe Wybierz ** Połącz automatycznie, ** i dodać statycznego adresu IP, sieci i ustawień DNS. Określ strefę czasową i hasła głównego do uzyskiwania dostępu do serwera zarządzania.
2. Kiedy zażądano typ instalacji, ma wybierz **tworzenie układu niestandardowego** jako partycja. Po kliknięciu **dalej** wybierz **wolne** i kliknij przycisk Utwórz. Utwórz  **/** , **/var/awarii** i **/home partycje** z **typu FS:** **ext4**. Utwórz partion wymiany jako **typu FS: wymiany**.
3. Jeśli istniejące urządzenia znajdują się, że zostanie wyświetlony komunikat ostrzegawczy. Kliknij przycisk **Format** do sformatowania dysku przy użyciu ustawień partycji. Kliknij przycisk **zapisać zmiany na dysku** można zastosować zmian partycji.
4. Wybierz **moduł ładujący rozruchu instalacji** > **dalej** zainstalować moduł ładujący rozruchu na partycji katalogu głównego.
5. Po zakończeniu instalacji kliknij przycisk **ponowny rozruch**.

#### <a name="retrieve-the-scsi-ids"></a>Pobierz identyfikatory SCSI
1. Po zakończeniu instalacji należy pobrać identyfikatory SCSI dla każdego dysku twardego SCSI w maszynie Wirtualnej. Aby zrobić to zamykania serwera zarządzania maszyny Wirtualnej, we właściwościach maszyny Wirtualnej VMware kliknij prawym przyciskiem myszy wpis maszyny Wirtualnej > **edytowanie ustawień** > **opcje**.
2. Wybierz **zaawansowane** > **ogólne elementu** i kliknij przycisk **parametry konfiguracji**. Ta opcja będzie de-active, gdy komputer jest uruchomiony. Aby ją uaktywnić komputer musi zostać zamknięta.
3. Jeśli wiersz **dysku. EnableUUID** istnieje upewnij się, że wartość jest równa **True** (z uwzględnieniem wielkości liter). Jeśli jest już anulować i przetestować polecenie SCSI w systemie operacyjnym gościa, po jego uruchomieniu.
4. Jeśli wiersz nie istniejących klawiszem **Dodaj wiersz** — i dodaj go z **True** wartość. Nie używaj podwójnych cudzysłowów.

#### <a name="install-additional-packages"></a>Instalowanie dodatkowych pakietów
Należy pobrać i zainstalować niektóre dodatkowe pakiety.

1. Upewnij się, że główny serwer docelowy jest połączony z Internetem.
2. Uruchom to polecenie, aby pobrać i zainstalować pakiety 15 z repozytorium CentOS: **# yum zainstalować — y xfsprogs perl lsscsi rsync wget kexec narzędzia**.
3. Jeśli chronisz maszyny źródłowej systemem Linux typu elementu roboczego Reiser lub XFS pliku systemu głównego lub rozruchowego urządzenia, a następnie należy pobrać i zainstalować dodatkowe pakiety w następujący sposób:

   * # <a name="cd-usrlocal"></a>/usr/local dysku CD
   * # <a name="wget-httpelrepoorglinuxelrepoel6x8664rpmskmod-reiserfs-00-1el6elrepox8664rpmhttpelrepoorglinuxelrepoel6x8664rpmskmod-reiserfs-00-1el6elrepox8664rpm"></a>wget [http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/kmod-reiserfs-0.0-1.el6.elrepo.x86_64.rpm](http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/kmod-reiserfs-0.0-1.el6.elrepo.x86_64.rpm)
   * # <a name="wget-httpelrepoorglinuxelrepoel6x8664rpmsreiserfs-utils-3621-1el6elrepox8664rpmhttpelrepoorglinuxelrepoel6x8664rpmsreiserfs-utils-3621-1el6elrepox8664rpm"></a>wget [http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/reiserfs-utils-3.6.21-1.el6.elrepo.x86_64.rpm](http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/reiserfs-utils-3.6.21-1.el6.elrepo.x86_64.rpm)
   * # <a name="rpm-ivh-kmod-reiserfs-00-1el6elrepox8664rpm-reiserfs-utils-3621-1el6elrepox8664rpm"></a>reiserfs kmod-reiserfs 0,0 1.el6.elrepo.x86_64.rpm — ivh obr. / min-witryny-3.6.21-1.el6.elrepo.x86_64.rpm
   * # <a name="wget-httpmirrorcentosorgcentos66osx8664packagesxfsprogs-311-16el6x8665rpmhttpmirrorcentosorgcentos66osx8664packagesxfsprogs-311-16el6x8665rpm"></a>wget [http://mirror.centos.org/centos/6.6/os/x86_64/Packages/xfsprogs-3.1.1-16.el6.x86_65.rpm](http://mirror.centos.org/centos/6.6/os/x86_64/Packages/xfsprogs-3.1.1-16.el6.x86_65.rpm)
   * # <a name="rpm-ivh-xfsprogs-311-16el6x8664rpm"></a>obr. / min — ivh xfsprogs-3.1.1-16.el6.x86_64.rpm

#### <a name="apply-custom-changes"></a>Zastosuj zmiany niestandardowych
Czy następujących czynności, aby zastosować zmiany niestandardowe po wprowadzeniu wykonaj poinstalacyjne czynności i zainstalowane pakiety:

1. Skopiuj RHEL 6-64 Unified Agent binarnego do maszyny Wirtualnej. Uruchom to polecenie, aby untar dane binarne: **tar — zxvf<file name>**
2. Uruchom to polecenie, aby nadać uprawnienia: **./ApplyCustomChanges.sh chmod &#755;**
3. Uruchom skrypt: **#./ApplyCustomChanges.sh**. Skrypt należy uruchomić tylko raz. Po pomyślnym uruchomieniu skryptu, należy ponownie uruchomić serwer.

## <a name="run-the-failback"></a>Uruchom powrót po awarii
### <a name="reprotect-the-azure-vms"></a>Włącz ponownie ochronę maszyny wirtualne platformy Azure
1. W portalu usługi Site Recovery > **maszyny** maszynę Wirtualną, która jest Failover, a następnie kliknij przycisk Wybierz kartę **ponownego włączenia ochrony**.
2. W **główny serwer docelowy** i **serwera przetwarzania** wybierz lokalny główny serwer docelowy i serwera przetwarzania maszyny Wirtualnej platformy Azure.
3. Wybierz konto, które są skonfigurowane do nawiązywania połączenia z maszyną Wirtualną.
4. Wybierz wersję powrotu po awarii dla grupy ochrony. Na przykład, jeśli maszyna wirtualna jest chroniona w PG1, a następnie możesz musisz wybrać PG1_Failback.
5. Jeśli chcesz odzyskać do alternatywnej lokalizacji, wybierz dysk przechowywania i magazynu danych skonfigurowane na głównym serwerze docelowym. Gdy użytkownik powrót po awarii do lokacji lokalnej maszyn wirtualnych VMware w przypadku powrotu po awarii plan ochrony będzie używać tego samego magazynu danych jako główny serwer docelowy. Jeśli chcesz odzyskać replikę maszyny Wirtualnej platformy Azure do tej samej lokalnej maszyny Wirtualnej VM lokalnymi już musi należeć do tego samego magazynu danych jako główny serwer docelowy, a następnie. Jeśli nie ma żadnych maszyny Wirtualnej lokalnych podczas zastosowania, zostanie utworzony nowy.

   ![](./media/site-recovery-failback-azure-to-vmware-classic/failback1.png)
6. Po kliknięciu **OK** zacząć przełączonej zadania rozpocznie replikowanie maszyny Wirtualnej z platformy Azure do lokacji lokalnej. Postęp można śledzić na **zadania** kartę.

   ![](./media/site-recovery-failback-azure-to-vmware-classic/failback2.png)

### <a name="run-a-failover-to-the-on-premises-site"></a>Uruchom tryb failover do lokacji lokalnej
Po przełączonej maszyna wirtualna zostanie przeniesiona do wersji powrotu po awarii z jej grupy ochrony i jest automatycznie dodawany do planu odzyskiwania, używane w trybie failover na platformie Azure, jeśli istnieje.

1. W **plany odzyskiwania** wybierz plan odzyskiwania zawierający grupę powrotu po awarii, a następnie kliknij przycisk **pracy awaryjnej** > **nieplanowany tryb Failover**.
2. W **potwierdzić trybu Failover** Sprawdź kierunek pracy awaryjnej (na platformie Azure), a następnie wybierz punkt odzyskiwania, którego chcesz użyć do pracy awaryjnej (Najnowsza wersja). Jeśli włączono **wielu maszyn wirtualnych** podczas konfigurowania właściwości replikacji można odzyskać najnowszą wersję aplikacji lub punktu odzyskiwania na poziomie awarii. Kliknij znacznik wyboru, aby uruchomić tryb failover.
3. W trybie failover Usługa Site Recovery zostanie zamknięty maszynach wirtualnych platformy Azure. Po sprawdzeniu, że powrót po awarii została ukończona, zgodnie z oczekiwaniami, należy można można sprawdzić czy zgodnie z oczekiwaniami została zamknięta maszynach wirtualnych platformy Azure.

### <a name="reprotect-the--on-premises-site"></a>Włącz ponownie ochronę lokacji lokalnej
Po zakończeniu powrotu po awarii dane będą do lokacji lokalnej, ale nie będzie chroniony. Aby rozpocząć replikację do platformy Azure ponownie wykonaj następujące czynności:

1. W portalu usługi Site Recovery > **maszyny** wybierz kartę maszyn wirtualnych, które nie powiodło się tworzenie kopii i kliknij przycisk **ponownego włączenia ochrony**.
2. Po upewnieniu się, że replikacja Azure działa zgodnie z oczekiwaniami na platformie Azure maszynach wirtualnych platformy Azure (obecnie nieuruchomiony) można usunąć, które nie zostały ponownie.

### <a name="common-issues-in-failback"></a>Typowe problemy w przypadku powrotu po awarii
1. Jeśli odnajdywanie vCenter użytkownika tylko do odczytu i ochrony maszyn wirtualnych, próba powiedzie się i tryb failover działa prawidłowo. Podczas ponownej ochrony kończy się niepowodzeniem, ponieważ nie można odnaleźć datastores. Jako objawem nie będą widzieć datastores wymienione podczas ponownie włączając ochronę. Aby rozwiązać ten problem, można zaktualizować poświadczeń vCenter z odpowiedniego konta, które ma uprawnienia i spróbuj ponownie wykonać zadanie. [Dowiedz się więcej](site-recovery-vmware-to-azure-classic.md)
2. Po awarii maszyny Wirtualnej systemu Linux i uruchom go lokalnego, zobaczysz, że pakiet Menedżera sieci został odinstalowany z komputera. Jest to spowodowane po odzyskaniu maszyna wirtualna na platformie Azure, pakiet Menedżera sieci jest usuwany.
3. Gdy maszyna wirtualna skonfigurowano adres statyczny adres IP i przeszła w tryb failover na platformie Azure, adres IP są uzyskiwane za pośrednictwem protokołu DHCP. Podczas przejścia w tryb failover z powrotem lokalnego, maszyny Wirtualnej w dalszym ciągu używać protokołu DHCP w celu pobrania adresu IP. Konieczne będzie ręczne logowania do komputera i adres IP z powrotem do adresów statycznych w razie potrzeby zestawu.
4. Jeśli używasz bezpłatna wersja ESXi 5.5 lub bezpłatna wersja funkcji Hypervisor vSphere 6 powiedzie się pracy awaryjnej, ale nie powiedzie się powrotu po awarii. Będzie ned uaktualnienie do licencji ewaluacyjnej, albo aby umożliwić powrót po awarii.

## <a name="failing-back-with-expressroute"></a>Wystąpił błąd wstecz z usługi ExpressRoute
Wróć za pośrednictwem połączenia sieci VPN lub usługa Azure ExpressRoute może zakończyć się niepowodzeniem. Jeśli chcesz użyć usługi ExpressRoute Uwaga następujące:

* ExpressRoute powinny zostać skonfigurowane w sieci wirtualnej platformy Azure, którego źródłem błędów maszyn za pośrednictwem, w których maszynach wirtualnych platformy Azure znajdują się po pracy awaryjnej.
* Dane są replikowane do konta magazynu platformy Azure na publiczny punkt końcowy. Należy skonfigurować publicznej komunikacji równorzędnej w usługi ExpressRoute z centrum danych docelowych replikacji usługi Site Recovery można używać usługi ExpressRoute.
