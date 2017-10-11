---
title: Niepowodzenie tworzenia kopii w maszynach wirtualnych VMware z platformy Azure w klasycznym portalu starszej wersji | Dokumentacja firmy Microsoft
description: "W tym artykule opisano sposób wykonaj powrót po awarii maszyny wirtualnej VMware, która jest jeszcze zreplikowane na platformie Azure za pomocą usługi Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: ruturaj
manager: mkjain
editor: 
ms.assetid: a63524bf-990c-42ee-8554-e017e6e67e68
ms.service: site-recovery
ms.devlang: na
ms.tgt_pltfrm: na
ms.topic: article
ms.workload: storage-backup-recovery
ms.date: 06/05/2017
ms.author: ruturajd@microsoft.com
ms.openlocfilehash: 3053fc622c6343898e2007b8aaafbe1fa8e6934e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="fail-back-vmware-virtual-machines-and-physical-servers-from-azure-to-vmware-with-azure-site-recovery-legacy"></a>Niepowodzenie wstecz maszyny wirtualne VMware i serwery fizyczne z platformy Azure do programu VMware z usługą Azure Site Recovery (starsze)
> [!div class="op_single_selector"]
> * [Azure Portal](site-recovery-failback-azure-to-vmware.md)
> * [Klasyczny portal Azure](site-recovery-failback-azure-to-vmware-classic.md)
> * [Klasyczny Portal Azure (starsze)](site-recovery-failback-azure-to-vmware-classic-legacy.md)
>
>

W tym artykule opisano, jak niepowodzenie wstecz maszyn wirtualnych VMware i systemem Windows lub Linux serwery fizyczne z platformy Azure do lokalnej lokacji po w przypadku replikacji z lokacji lokalnej do platformy Azure przy użyciu [usługi Azure Site Recovery?](site-recovery-overview.md).

W tym artykule opisano starą konfigurację i nie powinny być używane do nowych magazynów.

## <a name="architecture"></a>Architektura
Ten diagram przedstawia scenariusza trybu failover i powrotu po awarii. Niebieski wiersze są połączenia używane w trybie failover. Czerwony wiersze są połączenia używane podczas powrotu po awarii. Wiersze z strzałki go przez internet.

![](./media/site-recovery-failback-azure-to-vmware/vconports.png)

## <a name="before-you-start"></a>Przed rozpoczęciem
* Powinna zakończyć się niepowodzeniem na maszynach wirtualnych VMware lub serwerów fizycznych i powinna być uruchomiona na platformie Azure.
* Należy zauważyć, że można tylko w trybie wstecz maszyny wirtualne VMware i serwery fizyczne systemu Windows i Linux z platformy Azure dla maszyn wirtualnych VMware w lokalnej lokacji głównej.  Jeśli powrotu po awarii powrót komputera fizycznego, trybu failover na platformie Azure przekonwertuje go na maszynie Wirtualnej platformy Azure i powrotu po awarii do programu VMware przekonwertuje go do maszyny Wirtualnej VMware.

Poniżej przedstawiono sposób konfigurowania powrotu po awarii:

1. **Konfigurowanie składników powrotu po awarii**: należy skonfigurować serwer vContinuum lokalnie, a następnie wskaż serwer konfiguracji maszyny Wirtualnej na platformie Azure. Należy także skonfigurować serwer przetwarzania jako maszyny Wirtualnej platformy Azure do wysyłania danych do lokalnego serwera głównego celu. Należy zarejestrować serwer przetwarzania na serwerze konfiguracji obsługiwany tryb failover. Należy zainstalować na lokalny główny serwer docelowy. Jeśli potrzebujesz Windows głównego serwera docelowego jest ustawiona automatycznie po zainstalowaniu vContinuum. Jeśli potrzebujesz Linux należy skonfigurować ją ręcznie na osobnym serwerze.
2. **Włączanie ochrony i powrotu po awarii**: po skonfigurowaniu składników w vContinuum konieczne będzie włączenie ochrony dla przejścia w tryb failover maszyn wirtualnych platformy Azure. Zostanie Uruchom sprawdzanie gotowości na maszynach wirtualnych oraz uruchomienie trybu failover z platformy Azure do lokacji lokalnej. Po zakończeniu powrotu po awarii Wznów lokalnymi maszynami aby rozpoczynały się replikację do platformy Azure.

## <a name="step-1-install-vcontinuum-on-premises"></a>Krok 1: Instalowanie vContinuum lokalnej
Musisz zainstalować serwer vContinuum lokalnie, a następnie wskaż serwer konfiguracji.

1. [Pobierz vContinuum](http://go.microsoft.com/fwlink/?linkid=526305).
2. Następnie pobierz [aktualizacji vContinuum](http://go.microsoft.com/fwlink/?LinkID=533813) wersji.
3. Zainstaluj najnowszą wersję vContinuum. Na **powitalnej** kliknij **dalej**.
    ![](./media/site-recovery-failback-azure-to-vmware/image2.png)
4. Na pierwszej stronie kreatora określ adres IP serwera CX i port serwera CX. Wybierz **używania protokołu HTTPS**.

   ![](./media/site-recovery-failback-azure-to-vmware/image3.png)
5. Znajdowanie adresu IP serwera konfiguracji na **pulpitu nawigacyjnego** kartę Serwer konfiguracji maszyny Wirtualnej na platformie Azure.
   ![](./media/site-recovery-failback-azure-to-vmware/image4.png)
6. Znaleziono publicznych portu HTTPS serwera konfiguracji na **punkty końcowe** kartę Serwer konfiguracji maszyny Wirtualnej na platformie Azure.

   ![](./media/site-recovery-failback-azure-to-vmware/image5.png)
7. Na **szczegóły hasło CS** strony Określ hasło zanotowaną w dół po zarejestrowaniu serwera konfiguracji. Jeśli nie pamiętasz go Sprawdź w **C:\\Program Files (x86)\\systemów InMage\\prywatnej\\connection.passphrase** na serwerze konfiguracji maszyny Wirtualnej.

    ![](./media/site-recovery-failback-azure-to-vmware/image6.png)
8. W **wybierz lokalizację docelową** strony Określ, gdzie chcesz zainstalować serwer vContinuum i kliknij przycisk **zainstalować**.

   ![](./media/site-recovery-failback-azure-to-vmware/image7.png)
9. Po zakończeniu instalacji będzie można uruchomić vContinuum.
    ![](./media/site-recovery-failback-azure-to-vmware/image8.png)

## <a name="step-2-install-a-process-server-in-azure"></a>Krok 2: Instalowanie serwera przetwarzania na platformie Azure
Należy zainstalować serwer przetwarzania na platformie Azure, dzięki czemu maszyny wirtualne na platformie Azure może wysyłać dane do lokalny główny serwer docelowy.

1. Na **serwery konfiguracji** na platformie Azure wybierz, aby dodać nowego serwera przetwarzania.

   ![](./media/site-recovery-failback-azure-to-vmware/image9.png)
2. Określ serwer przetwarzania nazwy oraz nazwę i hasło, aby połączyć z maszyną wirtualną jako administrator. Wybierz serwer konfiguracji, do którego chcesz zarejestrować serwer przetwarzania. Powinno to być tym samym serwerze, używanego do ochrony i zakończyć się niepowodzeniem na maszynach wirtualnych.
3. Określ sieć platformy Azure, w którym powinny zostać wdrożone serwer przetwarzania. Należy go z tej samej sieci co serwer konfiguracji. Określ unikatowy podsieć wybrany adres IP i rozpocząć wdrażanie.

   ![](./media/site-recovery-failback-azure-to-vmware/image10.png)
4. Zadanie jest wyzwalane, aby wdrożyć serwer przetwarzania.

   ![](./media/site-recovery-failback-azure-to-vmware/image11.png)
5. Po wdrożeniu serwera przetwarzania na platformie Azure możesz zalogować się do serwera przy użyciu poświadczeń, które określiłeś. Zarejestruj serwer przetwarzania w taki sam sposób, w zarejestrowany lokalnego serwera przetwarzania.

   ![](./media/site-recovery-failback-azure-to-vmware/image12.png)

> [!NOTE]
> Serwery zarejestrowane podczas powrotu po awarii nie będzie wyświetlany w obszarze właściwości maszyny Wirtualnej w usłudze Site Recovery. Tylko są one widoczne w obszarze **serwerów** kartę Serwer konfiguracji, do którego zostanie zarejestrowany. Może potrwać około 10 – 15 minut, aż do ich z serwera przetwarzania zostanie wyświetlony na karcie.
>
>

## <a name="step-3-install-a-master-target-server-on-premises"></a>Krok 3: Zainstaluj główny serwer docelowy lokalnych
W zależności od systemu operacyjnego źródłowej maszyny wirtualnej należy zainstalować z systemem Linux lub Windows głównego serwera docelowego lokalnie.

### <a name="deploy-a-windows-master-target-server"></a>Wdrażanie systemu Windows server główny serwer docelowy
Główny cel systemu Windows jest już powiązany z Instalatora vContinuum. Po zainstalowaniu vContinuum serwer główny jest również wdrożone na tym samym komputerze i zarejestrowana na serwerze konfiguracji.

1. Aby rozpocząć wdrażanie, Utwórz pustą komputera lokalnego na hoście ESX, na który chcesz odzyskać maszyn wirtualnych z platformy Azure.
2. Upewnij się, że istnieje co najmniej dwóch dysków dołączonych do maszyny Wirtualnej — co najmniej jedna jest używana dla systemu operacyjnego i innych służy do przechowywania dysków.
3. Instalowanie systemu operacyjnego.
4. Zainstaluj vContinuum na serwerze. Na tym kończy również instalacji główny serwer docelowy.

### <a name="deploy-a-linux-master-target-server"></a>Wdrażanie systemu Linux głównego serwera docelowego
1. Aby rozpocząć wdrażanie, Utwórz pustą komputera lokalnego na hoście ESX, na który chcesz odzyskać maszyn wirtualnych z platformy Azure.
2. Upewnij się, że istnieje co najmniej dwóch dysków dołączonych do maszyny Wirtualnej — co najmniej jedna jest używana dla systemu operacyjnego i innych służy do przechowywania dysków.
3. Instalowanie systemu operacyjnego Linux. Główny cel systemu Linux nie należy używać LVM katalogu głównego lub przechowywania miejsca do magazynowania. Linux główny serwer docelowy jest skonfigurowany w celu uniknięcia LVM partycje/dysków odnajdywania domyślnie.
4. Partycje, które możesz utworzyć:

   ![](./media/site-recovery-failback-azure-to-vmware/image13.png)
5. Przeprowadzać poniżej post kroki instalacji przed rozpoczęciem instalacji głównego celu.

#### <a name="post-os-installation-steps"></a>Opublikuj kroki instalacji systemu operacyjnego
Aby uzyskać identyfikator SCSI dla każdego dysku twardego SCSI maszyny wirtualnej systemu Linux, włącz parametr "dysk. EnableUUID = TRUE "w następujący sposób:

1. Zamknij maszynę wirtualną.
2. Kliknij prawym przyciskiem myszy wpis maszyny Wirtualnej w lewym panelu > **Edytuj ustawienia**.
3. Kliknij przycisk **opcje** kartę. Wybierz **zaawansowane\>ogólne elementu** > **parametry konfiguracji**. **Parametry konfiguracji** opcja jest dostępna tylko, gdy komputer zostanie zamknięta.

    ![](./media/site-recovery-failback-azure-to-vmware/image14.png)
4. Sprawdza, czy wiersz z **dysku. EnableUUID** istnieje. Jeśli tak i ma ustawioną wartość **False** ustawić ją na **True** (bez rozróżniania wielkości liter). Jeśli istnieje i ma ustawioną wartość true, kliknij przycisk **anulować** i testowania polecenie SCSI w systemie operacyjnym gościa, po uruchomieniu w górę. Jeśli nie istnieje kliknij **Dodaj wiersz**.
5. Dodaj dysk. EnableUUID w **nazwa** kolumny. Ustaw dla niego wartość true. Nie dodawaj powyższych wartości wraz ze podwójnych cudzysłowów.

    ![](./media/site-recovery-failback-azure-to-vmware/image15.png)

#### <a name="download-and-install-the-additional-packages"></a>Pobieranie i instalowanie dodatkowych pakietów
Uwaga: Upewnij się, że system ma połączenie z Internetem, przed pobraniem i instalowanie dodatkowych pakietów.

\#Zainstaluj YUM -y xfsprogs perl lsscsi rsync wget kexec — narzędzia

To polecenie pobiera te pakiety 15 z repozytorium CentOS 6.6 i instaluje je:

BC-1.06.95-1.el6.x86\_64. obr. / min

busybox-1.15.1-20.el6.x86\_64. obr. / min

elfutils-libs-0.158-3.2.el6.x86\_64. obr. / min

kexec-tools-2.0.0-280.el6.x86\_64. obr. / min

lsscsi 0,23 2.el6.x86\_64. obr. / min

LZO-2.03-3.1.el6\_5.1.x86\_64. obr. / min

Perl-5.10.1-136.el6\_6.1.x86\_64. obr. / min

Perl-Module-Plug-3.90-136.el6\_6.1.x86\_64. obr. / min

Perl-Pod-specjalne-1.04-136.el6\_6.1.x86\_64. obr. / min

Perl-Pod — prosty-3.13-136.el6\_6.1.x86\_64. obr. / min

Perl-libs-5.10.1-136.el6\_6.1.x86\_64. obr. / min

Perl — wersja 0,77 136.el6\_6.1.x86\_64. obr. / min

rsync-3.0.6-12.el6.x86\_64. obr. / min

szałowe 1.el6.x86 1.1.0\_64. obr. / min

wget-1.12-5.el6\_6.1.x86\_64. obr. / min

Jeśli maszyna źródłowa używa filesystem Reiser lub XFS głównego lub rozruchowego urządzenia, następnie następujące pakiety powinny zostać pobrana i zainstalowana w głównym celu Linux przed ochrony.

\#/usr/local dysku CD

\#wget <http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/kmod-reiserfs-0.0-1.el6.elrepo.x86_64.rpm>

\#wget <http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/reiserfs-utils-3.6.21-1.el6.elrepo.x86_64.rpm>

\#-ivh obr. / min kmod-reiserfs 0,0 1.el6.elrepo.x86\_64. obr. / min reiserfs witryny-3.6.21-1.el6.elrepo.x86\_64. obr. / min

\#wget <http://mirror.centos.org/centos/6.6/os/x86_64/Packages/xfsprogs-3.1.1-16.el6.x86_64.rpm>

\#-ivh obr. / min xfsprogs-3.1.1-16.el6.x86\_64. obr. / min

#### <a name="apply-custom-configuration-changes"></a>Zastosuj zmiany konfiguracji niestandardowej
Przed zastosowaniem tych zmian upewnij się, że udało Ci się ukończyć poprzedniej sekcji, a następnie wykonaj następujące kroki:

1. Skopiuj RHEL 6-64 Unified Agent binarnego do nowo utworzonej systemu operacyjnego.
2. Uruchom to polecenie, aby untar dane binarne: **tar - zxvf \<nazwy pliku\>**
3. Uruchom to polecenie, aby nadać uprawnienia: \# **./ApplyCustomChanges.sh chmod 755**
4. Uruchom skrypt:  **\# ./ApplyCustomChanges.sh**. Uruchom skrypt tylko raz na serwerze. Uruchom ponownie serwer po uruchomieniu skryptu.

### <a name="install-the-linux-server"></a>Zainstaluj serwer z systemem Linux
1. [Pobierz](http://go.microsoft.com/fwlink/?LinkID=529757) plik instalacyjny.
2. Skopiuj plik do docelowego elementu głównego systemu Linux maszyny wirtualnej przy użyciu narzędzia klienta sftp wybranych przez użytkownika. Alternatywnie logowania się do maszyny wirtualnej systemu Linux główny serwer docelowy i użyj wget, aby pobrać pakiet instalacyjny z podanego łącza
3. Dziennika na użyciu maszyny wirtualnej serwera głównego celu Linux ssh klienta
4. Jeśli jest podłączony do sieci platformy Azure, na którym wdrożono Linux główny serwer docelowy przez połączenie sieci VPN, należy użyć wewnętrznego adresu IP serwera, który znajduje się w maszynie wirtualnej **pulpitu nawigacyjnego** karcie i portu 22 nawiązać połączenia z głównego celu Linux serwera przy użyciu Secure Shell.
5. Jeśli łączysz się Linux głównego serwera docelowego za pośrednictwem publicznego użycia połączenia internetowego wzorca Linux target publiczny wirtualny adres IP serwera (z maszyn wirtualnych **pulpitu nawigacyjnego** kartę) i publiczny punkt końcowy, utworzone dla ssh do logowania do servder systemu Linux.
6. Wyodrębnij pliki z archiwum tar Instalatora serwera głównego celu formacie gzip Linux uruchamiając: *"tar — xvzf Microsoft ASR\_UA\_8.2.0.0\_RHEL6 64\"* z katalogu, który zawiera plik Instalatora.

    ![](./media/site-recovery-failback-azure-to-vmware/image16.png)
7. Po wyodrębnieniu Instalator plików do innego katalogu przejdź do katalogu, do którego zawartość tar archiwum zostały wyodrębnione. Z tego ścieżką Uruchom "sudo. / install.sh".

    ![](./media/site-recovery-failback-azure-to-vmware/image17.png)
8. Po wyświetleniu monitu wybierz wybierz podstawową rolą **2 (docelowego elementu głównego)**. Pozostaw inne opcje interaktywnych instalacji na wartości domyślne.
9. Poczekaj, aż w celu kontynuowania instalacji i konfiguracji hosta interfejsu są wyświetlane. Narzędzie konfiguracji hosta dla systemu Linux docelowego głównego serwera to narzędzie wiersza polecenia. Rozmiar nie zmienia ssh okna narzędzia klienta. Użyj klawiszy strzałek, aby wybrać **Global** opcja a naciśnij klawisz ENTER na klawiaturze.

    ![](./media/site-recovery-failback-azure-to-vmware/image18.png)

    ![](./media/site-recovery-failback-azure-to-vmware/image19.png)
10. W polu **IP** wprowadź adres IP wewnętrznego serwera konfiguracji (można znaleźć w **pulpitu nawigacyjnego** kartę Serwer konfiguracji maszyny Wirtualnej) i naciśnij klawisz ENTER. W **portu** wprowadź **22** i naciśnij klawisz ENTER.
11. Pozostaw **użycie protokołu HTTPS** jako **tak**, i naciśnij klawisz ENTER.
12. Wprowadź hasło, który został wygenerowany na serwerze konfiguracji. Jeśli używasz programu PUTTY klienta na komputerze z systemem Windows do ssh do maszyny wirtualnej systemu Linux można Shift + Insert Wklej zawartość Schowka. Kopiuj hasło do schowka lokalnego przy użyciu klawiszy Ctrl-C, a następnie wklej go za pomocą Shift + Insert. Naciśnij klawisz ENTER.
13. Użyj klawisza Strzałka w prawo, aby przejść Zamknij, a następnie naciśnij klawisz ENTER. Poczekaj na ukończenie instalacji.

    ![](./media/site-recovery-failback-azure-to-vmware/image20.png)

Jeśli z jakiegoś powodu nie można zarejestrować Linux główny serwer docelowy do konfiguracji serwera należy więc ponownie, uruchamiając narzędzie konfiguracji hosta z /usr/local/ASR/Vx/bin/hostconfigcli (najpierw należy ustawić uprawnienia dostępu do tego katalogu, uruchamiając chmod jako superużytkownika).

#### <a name="validate-master-target-registration"></a>Sprawdzanie poprawności rejestracji główny serwer docelowy
Można sprawdzić, czy główny serwer docelowy został pomyślnie zarejestrowany do konfiguracji serwera w magazynie usługi Azure Site Recovery > **serwera konfiguracji** > **szczegóły serwera**.

> [!NOTE]
> Po zarejestrowaniu główny serwer docelowy, jeśli wystąpią błędy konfiguracji maszyny wirtualnej została usunięta z platformy Azure lub punkty końcowe nie są poprawnie skonfigurowane, to ponieważ mimo że konfiguracja główny serwer docelowy zostanie wykryta przez Azure dndpoints po wdrożeniu na platformie Azure główny cel, nie jest wartość true dla lokalnego główny cel serwera lokalnego. Nie będzie to mieć wpływ na powrót po awarii, a te błędy można zignorować.
>
>

## <a name="step-4-protect-the-virtual-machines-to-the-on-premises-site"></a>Krok 4: Ochrona maszyn wirtualnych do lokacji lokalnej
Należy chronić maszyny wirtualne do lokacji lokalnej, zanim nie zostanie ponownie.

### <a name="before-you-begin"></a>Przed rozpoczęciem
Gdy maszyna wirtualna przeszła w tryb failover na platformie Azure, dodaje bardzo tymczasowego dysku dla pliku stronicowania. Jest to dodatkowych dysków, które nie są zwykle wymagane przez użytkownika w trybie Failover maszyny Wirtualnej, ponieważ mogą już mieć dysku przeznaczonego do pliku stronicowania. Przed rozpoczęciem odwrotnej ochrony maszyn wirtualnych należy upewnij się, że ten dysk do trybu offline, aby nie pobrać chronione. Wykonaj następujące czynności w następujący sposób:

1. Otwórz przystawkę Zarządzanie komputerem, a następnie wybierz Zarządzanie magazynem, który zawiera dyski w tryb online i dołączony do maszyny.
2. Wybierz tymczasowego dysk dołączony do maszyny i wybierz, aby przenieść ją do trybu offline.

### <a name="protect-the-vms"></a>Ochrona maszyn wirtualnych
1. W portalu Azure Sprawdź stan maszyny wirtualnej, aby upewnić się, że nie są one za pośrednictwem.

    ![](./media/site-recovery-failback-azure-to-vmware/image21.png)
2. Uruchom vContinuum na tym komputerze.

    ![](./media/site-recovery-failback-azure-to-vmware/image8.png)
3. Kliknij przycisk **ochrony nowego** i wybierz typ operacji systemu
4. W nowym oknie, która jest otwierana wybierz **typ systemu operacyjnego** > **Pobierz szczegóły** dla maszyn wirtualnych, aby wykonaj powrót po awarii. W **szczegóły serwera podstawowego**, identyfikowanie i wybierz maszyny wirtualne, które chcesz chronić. Maszyny wirtualne są wymienione pod nazwą hosta vCenter, które znajdowały się na przed trybu failover.
5. Wybierz maszynę wirtualną, aby chronić (i jego już przeszła w tryb failover na platformie Azure) okno podręczne zapewnia dwa wpisy dla maszyny wirtualnej. Jest to spowodowane serwer konfiguracji wykrywa dwóch wystąpień maszyn wirtualnych, w zarejestrowany do niego. Należy usunąć wpis dla lokalnej maszyny Wirtualnej tak, aby chronić poprawne maszyny Wirtualnej. Aby zidentyfikować tutaj poprawny wpis maszyny Wirtualnej platformy Azure, można zalogować się do maszyny Wirtualnej platformy Azure i przejdź do \Microsoft Azure Site Recovery\Application Data\etc C:\Program Files (x86). W drscout.conf pliku określają identyfikator hosta. W oknie dialogowym vContinuum Zachowaj wpisu, dla którego znaleziono identyfikator hosta na maszynie Wirtualnej. Usuń wszystkie inne pozycje. Aby wybrać poprawny maszyny Wirtualnej mogą odwoływać się do adresu IP. IP address zakresu lokalnej będzie lokalnej maszyny Wirtualnej.

   ![](./media/site-recovery-failback-azure-to-vmware/image22.png)

   ![](./media/site-recovery-failback-azure-to-vmware/image23.png)
6. Na serwerze vCenter Zatrzymaj maszynę wirtualną. Można również usunąć maszyn wirtualnych lokalną.
7. Następnie należy określić lokalny serwer MT, do którego chcesz chronić maszyn wirtualnych. W tym celu należy połączyć się serwer vCenter, do którego chcesz wykonaj powrót po awarii.

    ![](./media/site-recovery-failback-azure-to-vmware/image24.png)
8. Wybierz główny serwer docelowy oparty na hoście, do której chcesz odzyskać maszynę Wirtualną.

    ![](./media/site-recovery-failback-azure-to-vmware/image24.png)
9. Podaj opcję replikacji dla poszczególnych maszyn wirtualnych. W tym celu musisz wybrać magazyn danych po stronie odzyskiwania, do której zostanie odzyskana maszyn wirtualnych. W tabeli przedstawiono różne opcje, które należy podać dla każdej maszyny Wirtualnej.

    ![](./media/site-recovery-failback-azure-to-vmware/image25.png)

   | **Opcja** | **Opcja zalecana wartość** |
   | --- | --- |
   |  Adres IP serwera przetwarzania |Wybierz serwer przetwarzania wdrożona na platformie Azure |
   |  Rozmiar przechowywania w MB | |
   |  Wartość przechowywania |1 |
   |  Dni i godzin |Dni |
   |  Interwał spójności |1 |
   |  Wybierz docelowy magazyn danych |Magazyn danych dostępnych w lokacji odzyskiwania. Magazyn danych, należy za mało miejsca i być dostępne dla hosta ESX, na którym chcesz przywrócić maszynę wirtualną. |
10. Skonfiguruj właściwości, które uzyska maszyny wirtualnej po przejściu w tryb failover lokacji lokalnej. W poniższej tabeli przedstawiono podsumowanie właściwości.

    ![](./media/site-recovery-failback-azure-to-vmware/image26.png)

    | **Właściwość** | **Szczegóły** |
    | --- | --- |
    | Konfiguracja sieci |Dla każdej karty sieciowej wykryte, zaznacz go i kliknij **zmiany** Aby skonfigurować adres IP powrotu po awarii dla maszyny wirtualnej. |
    | Konfiguracja sprzętu |Określ procesora CPU i pamięci dla maszyny Wirtualnej. Ustawienia można zastosować do wszystkich maszyn wirtualnych chcesz chronić. Aby zidentyfikować poprawne wartości dla procesora CPU i pamięci, można odwoływać się do rozmiaru roli maszyny wirtualne IAAS i liczba rdzeni i ilości pamięci przypisana. |
    | Nazwa wyświetlana |Po powrotu po awarii do środowiska lokalnego jak wyświetlone w spisie vCenter można zmienić nazwy maszyny wirtualnej. Domyślna nazwa jest nazwą hosta komputera maszyny wirtualnej. Aby zidentyfikować nazwę maszyny Wirtualnej, może się odwoływać do listy maszyn wirtualnych w grupie ochrony. |
    | Konfigurację NAT |Opisano szczegółowo niżej |

    ![](./media/site-recovery-failback-azure-to-vmware/image27.png)

#### <a name="configure-nat-settings"></a>Skonfiguruj ustawienia translatora adresów Sieciowych
1. Aby włączyć ochronę maszyn wirtualnych, dwa kanały komunikacyjne, można ustanowić. Pierwszy kanał jest między maszyną wirtualną a serwerem przetwarzania. Ten kanał zbiera dane z maszyny Wirtualnej i wysyła je do serwera przetwarzania, który następnie wysyła dane do głównego serwera docelowego. Jeśli serwer przetwarzania i maszyny wirtualnej, która ma być chroniony znajdują się na tej samej sieci wirtualnej platformy Azure nie muszą używać ustawień translatora adresów Sieciowych. W przeciwnym razie Określ ustawienia translatora adresów Sieciowych. Wyświetl publiczny adres IP serwera przetwarzania na platformie Azure.

    ![](./media/site-recovery-failback-azure-to-vmware/image28.png)
2. Drugi kanał jest między serwerem przetwarzania i główny serwer docelowy. Możliwość użycia NAT lub nie zależy od tego, czy połączenie sieci VPN na podstawie lub komunikacji za pośrednictwem Internetu. Nie zaznaczaj translatora adresów sieciowych, jeśli używasz sieci VPN, ale tylko wtedy, gdy używasz połączenia internetowego.

    ![](./media/site-recovery-failback-azure-to-vmware/image29.png)

    ![](./media/site-recovery-failback-azure-to-vmware/image30.png)
3. Jeśli nie zostały usunięte lokalnych maszyn wirtualnych określone i magazynu danych, który kończy się niepowodzeniem do nadal zawiera starego VMDK następnie konieczne będzie upewnij się, że powrót po awarii, pobiera utworzone w nowe miejsce dla maszyny Wirtualnej. Aby zrobić to wybrać **zaawansowane** ustawienia i określ Folder alternatywny, aby przywrócić w **ustawienia nazwy folderu**. Pozostaw innych opcji za pomocą ustawień domyślnych. Zastosuj ustawienia nazwy folderu do wszystkich serwerów.

    ![](./media/site-recovery-failback-azure-to-vmware/image31.png)
4. Uruchom sprawdzanie gotowości do zapewnienia, że maszyny wirtualne są gotowe do ochrony do lokalnej.

    ![](./media/site-recovery-failback-azure-to-vmware/image32.png)
5. Poczekaj na jej zakończenie. Jeśli pomyślnie znajduje się na wszystkich maszynach wirtualnych można określić nazwę planu ochrony. Następnie kliknij przycisk **Chroń** do rozpoczęcia.

    ![](./media/site-recovery-failback-azure-to-vmware/image33.png)
6. Możesz monitorować postęp w vContinuum.

    ![](./media/site-recovery-failback-azure-to-vmware/image34.png)
7. Po wykonaniu kroku **aktywowanie planu ochrony** zakończeniu można monitorować ochronę maszyny Wirtualnej w portalu usługi Site Recovery.

    ![](./media/site-recovery-failback-azure-to-vmware/image35.png)
8. Stan jest dokładne monitorowanie postępu i kliknięcie na maszynie Wirtualnej jest widoczny.

    ![](./media/site-recovery-failback-azure-to-vmware/image36.png)

## <a name="prepare-the-failback-plan"></a>Przygotowanie planu powrotu po awarii
Aby przygotować planu powrotu po awarii, za pomocą vContinuum, dzięki czemu aplikacji może się nie powiodło się do lokacji lokalnej w dowolnym momencie. Te plany odzyskiwania są bardzo podobne do planów odzyskiwania w usłudze Site Recovery.

1. Uruchom vContinuum i wybierz **Zarządzanie planami** > **odzyskać.** Widać listy planów, które były używane do pracy awaryjnej maszyn wirtualnych. Możesz użyć tego samego planu, aby odzyskać.

   ![](./media/site-recovery-failback-azure-to-vmware/image37.png)
2. Wybierz plan ochrony i wszystkich maszyn wirtualnych do odzyskania w nim. Po wybraniu każdej maszyny Wirtualnej można wyświetlić więcej szczegółów, w tym dysk maszyny Wirtualnej źródłowego i docelowego serwera ESX. Kliknij przycisk **dalej** rozpocząć pracę Kreatora odzyskiwania i wybierz maszyny wirtualne, w której chcesz odzyskać.

    ![](./media/site-recovery-failback-azure-to-vmware/image38.png)
3. Będzie można odzyskać oparte na wiele opcji, jednak zalecane jest użycie **najnowsze Tag** i wybierz **Zastosuj dla wszystkich maszyn wirtualnych** aby upewnić się, że będą używane najnowsze dane z maszyny wirtualnej.
4. Uruchom **sprawdzanie gotowości.** Spowoduje to zaewidencjonowanie Jeśli prawo parametry są skonfigurowane do włączenia odzyskiwania maszyny Wirtualnej. Kliknij przycisk **dalej** Jeśli wszystkie testy zostały wykonane pomyślnie. Jeśli nie sprawdza w dzienniku i napraw błędy.

    ![](./media/site-recovery-failback-azure-to-vmware/image39.png)
5. W **konfiguracji maszyny Wirtualnej** upewnij się, że ustawienia odzyskiwania są poprawnie ustawione. Jeśli potrzebujesz, można zmienić ustawienia maszyny Wirtualnej.

   ![](./media/site-recovery-failback-azure-to-vmware/image40.png)
6. Przejrzyj listę maszyn wirtualnych, które zostaną odzyskane i określ kolejność odzyskiwania. Należy pamiętać, że maszyny wirtualne są wyświetlane przy użyciu nazwy hosta komputera. Może być trudne do mapowania nazwa hosta komputera na maszynie wirtualnej.
   Aby mapować nazwy, przejdź do maszyny wirtualnej **pulpitu nawigacyjnego** Azure i wyboru nazwa hosta maszyny Wirtualnej.

    ![](./media/site-recovery-failback-azure-to-vmware/image41.png)
7. Określ nazwę planu, a następnie wybierz **później odzyskać**. Zaleca się odzyskać później, ponieważ początkowa ochrony mogą być niekompletne.
8. Polecenie **odzyskać** zapisać plan lub wyzwalanie odzyskiwania, jeśli wybrano odzyskiwanie teraz, a nie później. Można sprawdzić stan odzyskiwania, aby zobaczyć, czy plan został zapisanego.

   ![](./media/site-recovery-failback-azure-to-vmware/image42.png)

   ![](./media/site-recovery-failback-azure-to-vmware/image43.png)

## <a name="recover-virtual-machines"></a>Odzyskiwanie maszyn wirtualnych
Po utworzeniu planu, można odzyskać maszyny wirtualnej. Przed Sprawdź, czy maszyny wirtualne zostały ukończone synchronizacji. Jeśli stan replikacji przedstawiono OK ochrony zostało zakończone i próg RPO zostały spełnione. Można sprawdzić kondycji we właściwościach maszyny Wirtualnej.

![](./media/site-recovery-failback-azure-to-vmware/image44.png)

Wyłącz maszyny wirtualne platformy Azure, przed rozpoczęciem odzyskiwania. Dzięki temu istnieje nie podziału informacji i że użytkownicy będą uzyskiwać dostęp tylko do jednej kopii aplikacji.

1. Uruchom plan zapisane. Wybierz vContinuum **Monitor** planów. Ta lista zawiera wszystkie plany, które zostały uruchomione.

   ![](./media/site-recovery-failback-azure-to-vmware/image45.png)
2. Wybierz plan w **odzyskiwania** i kliknij przycisk **Start**. Można monitorować odzyskiwania. Po włączono maszyn wirtualnych na nawiązanie połączenia ich w programie vCenter.

   ![](./media/site-recovery-failback-azure-to-vmware/image46.png)

## <a name="protect-to-azure-again-after-failback"></a>Ochronę na platformie Azure ponownie po powrotu po awarii
Po zakończeniu powrotu po awarii prawdopodobnie należy ponownie ochronę maszyny wirtualnej. Wykonaj następujące czynności w następujący sposób:

1. Sprawdź, czy działają maszyny wirtualne lokalną i że aplikacje są dostępne.
2. W portalu usługi Azure Site Recovery wybrać maszyny wirtualne i usuń je. Wybierz, aby wyłączyć ochronę maszyn wirtualnych. Dzięki temu, że maszyn wirtualnych są już chronione.
3. W programie Azure usunąć nieudane na maszynach wirtualnych platformy Azure
4. Usuń stare maszyny wirtualnej na vSpehere. Są to maszyn wirtualnych, które wcześniej nie się za pośrednictwem usługi Azure.
5. W portalu usługi Site Recovery ochrony maszyn wirtualnych, które ostatnio przejścia w tryb failover. Po są one chronione można dodać je do planu odzyskiwania.

## <a name="next-steps"></a>Następne kroki
* [Przeczytaj informacje o](site-recovery-vmware-to-azure-classic.md) replikację maszyn wirtualnych VMware i serwerów fizycznych do platformy Azure przy użyciu wdrożenia rozszerzonego.
