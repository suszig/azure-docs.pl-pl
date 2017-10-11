---
title: "Jak zainstalować Linux głównego serwera docelowego dla trybu failover z platformy Azure do środowiska lokalnego | Dokumentacja firmy Microsoft"
description: "Przed ponownej ochrony maszyny wirtualnej systemu Linux, należy Linux głównego serwera docelowego. Dowiedz się, jak zainstalować jedną."
services: site-recovery
documentationcenter: 
author: ruturaj
manager: gauravd
editor: 
ms.assetid: 44813a48-c680-4581-a92e-cecc57cc3b1e
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 08/11/2017
ms.author: ruturajd
ms.openlocfilehash: 5341e3e56e0c366079958dd9a885f6ee3e8436cb
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/18/2017
---
# <a name="install-a-linux-master-target-server"></a>Zainstaluj serwer główny cel systemu Linux
Po przejścia w tryb failover maszyn wirtualnych, możesz w trybie wstecz maszyn wirtualnych do lokacji lokalnej. Aby wykonaj powrót po awarii, musisz Włącz ponownie ochronę maszyny wirtualnej z platformy Azure do lokacji lokalnej. W przypadku tego procesu należy lokalny główny serwer docelowy do odbierania ruchu. 

Chronione maszyny wirtualnej w przypadku maszyny wirtualnej systemu Windows, należy główny cel systemu Windows. W przypadku maszyny wirtualnej systemu Linux należy główny cel systemu Linux. Przeczytaj poniższe kroki, aby dowiedzieć się, jak utworzyć i zainstalować główny cel systemu Linux.

> [!IMPORTANT]
> Począwszy od wersji 9.10.0 główny serwer docelowy, najnowsze głównego serwera docelowego można zainstalować tylko na serwerze Ubuntu 16.04. Nowe instalacje nie są dozwolone w CentOS6.6 serwerów. Jednak możesz uaktualnić starego głównego serwerów docelowych przy użyciu 9.10.0 wersji.

## <a name="overview"></a>Omówienie
Ten artykuł zawiera instrukcje dotyczące sposobu instalowania główny cel systemu Linux.

Opublikuj komentarze lub pytania na końcu tego artykułu lub na [Forum usług odzyskiwania Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="prerequisites"></a>Wymagania wstępne

* Aby wybrać host, na którym chcesz wdrożyć główny cel, ustal, jeśli powrót po awarii będzie istniejących na lokalnej maszynie wirtualnej lub nowej maszyny wirtualnej. 
    * Dla istniejącej maszyny wirtualnej host główny cel powinien mieć dostęp do magazynów danych maszyny wirtualnej.
    * Jeśli nie istnieje na lokalnej maszynie wirtualnej, maszynę wirtualną w przypadku powrotu po awarii jest tworzony na tym samym hoście jako głównego celu. Można wybrać dowolnego hosta ESXi, aby zainstalować główny cel.
* Główny cel powinna być w sieci, który może komunikować się z serwerem przetwarzania i serwer konfiguracji.
* Wersja głównego elementu docelowego musi być późniejsza niż w przypadku wersji serwera przetwarzania i serwer konfiguracji. Na przykład jeśli wersja serwera konfiguracji jest 9.4, wersja głównego celu może być 9.4 lub 9.3, ale nie 9.5.
* Główny cel może być tylko maszyny wirtualne VMware, a nie serwera fizycznego.

## <a name="create-the-master-target-according-to-the-sizing-guidelines"></a>Tworzenie głównego celu zgodnie z wytycznymi zmiany rozmiaru

Tworzenie głównego celu zgodnie z wytycznymi następujące zmiany rozmiaru:
- **Pamięć RAM**: 6 GB lub więcej
- **Rozmiar dysku systemu operacyjnego**: 100 GB lub więcej (zainstalować CentOS6.6)
- **Dodatkowy dysk rozmiar dysku przechowywania**: 1 TB
- **Rdzenie Procesora**: rdzenie 4 lub więcej

Obsługiwane są następujące jądra Ubuntu obsługiwane.


|Seria jądra  |Obsługuje maksymalnie  |
|---------|---------|
|4.4      |4.4.0-81-Generic         |
|4.8      |4.8.0-56-Generic         |
|4.10     |4.10.0-24-Generic        |


## <a name="deploy-the-master-target-server"></a>Wdrażanie główny serwer docelowy

### <a name="install-ubuntu-16042-minimal"></a>Zainstaluj Ubuntu 16.04.2 minimalnego

Wziąć pod uwagę następujące kroki, aby zainstalować 64-bitowym systemie operacyjnym Ubuntu 16.04.2.

**Krok 1:** przejdź do [pobrać link](https://www.ubuntu.com/download/server/thank-you?version=16.04.2&architecture=amd64) i wybierz najbardziej duplikatów z których pobierania obrazu ISO Ubuntu 16.04.2 minimalnego 64-bitowych.

Zachowaj ISO Ubuntu 16.04.2 minimalnego 64-bitowe w stacji dysków DVD i uruchomić system.

**Krok 2:** wybierz **angielski** jako preferowany język, a następnie wybierz **Enter**.

![Wybierz język](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image1.png)

**Krok 3:** wybierz **zainstalować Ubuntu Server**, a następnie wybierz **Enter**.

![Wybierz opcję instalacji Ubuntu Server](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image2.png)

**Krok 4:** wybierz **angielski** jako preferowany język, a następnie wybierz **Enter**.

![Wybierz preferowany język angielski](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image3.png)

**Krok 5:** wybierz odpowiednią opcję z **strefy czasowej** listy Opcje, a następnie wybierz **Enter**.

![Wybierz odpowiednią strefę czasową](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image4.png)

**Krok 6:** wybierz **nr** (opcja domyślna), a następnie wybierz **Enter**.


![Konfigurowanie klawiatury](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image5.png)

**Krok 7:** wybierz **języka angielskiego (US)** jako kraju pochodzenia dla klawiatury, a następnie wybierz **Enter**.

![Wybierz kraj pochodzenia stany USA](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image6.png)

**Krok 8:** wybierz **języka angielskiego (US)** jako układu klawiatury, a następnie wybierz **Enter**.

![Wybierz US English jako układu klawiatury](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image7.png)

**Krok 9:** wprowadź nazwę hosta serwera w **Hostname** , a następnie wybierz **Kontynuuj**.

![Wprowadź nazwę hosta serwera](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image8.png)

**Krok 10.** Aby utworzyć konto użytkownika, wprowadź nazwę użytkownika, a następnie wybierz **Kontynuuj**.

![Tworzenie konta użytkownika](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image9.png)

**Krok 11.** wprowadź hasło dla nowego konta użytkownika, a następnie wybierz **Kontynuuj**.

![Wprowadź hasło](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image10.png)

**Krok 12.** Potwierdź hasło dla nowego użytkownika, a następnie wybierz **Kontynuuj**.

![Potwierdzenie hasła](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image11.png)

**Krok 13:** wybierz **nr** (opcja domyślna), a następnie wybierz **Enter**.

![Konfigurowanie użytkowników i haseł](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image12.png)

**Krok 14:** strefy czasowej, która jest wyświetlana jest poprawny, wybierz opcję **tak** (opcja domyślna), a następnie wybierz **Enter**.

Aby ponownie skonfigurować strefę czasową, wybierz **nr**.

![Skonfiguruj zegar](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image13.png)

**Krok 15.** partycjonowania opcje metody, zaznacz **z przewodnikiem - Użyj cały dysk**, a następnie wybierz **Enter**.

![Wybierz opcję partycjonowania — metoda](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image14.png)

**Krok 16:** wybierz odpowiedni dysk z **wybierz dysk do partycji** opcje, a następnie wybierz **Enter**.


![Wybierz dysk](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image15.png)

**Kroku 17:** wybierz **tak** Aby zapisać zmiany na dysku, a następnie wybierz **Enter**.

![Zapisać zmiany na dysku](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image16.png)

**Krok 18:** wybierz opcję domyślną, zaznacz **Kontynuuj**, a następnie wybierz **Enter**.

![Wybierz opcję domyślną](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image17.png)

**Krok 19:** wybierz opcję odpowiednią dla uaktualnienia w systemie zarządzania, a następnie wybierz **Enter**.

![Wybierz sposób zarządzania uaktualnień](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image18.png)

> [!WARNING]
> Azure Site Recovery główny serwer docelowy wymaga bardzo określonej wersji Ubuntu, dlatego należy upewnić się, że jądra, które aktualizacje są wyłączone dla maszyny wirtualnej. Jeśli są one włączone regularne uaktualnienia spowodować główny serwer docelowy do nieprawidłowego działania. Upewnij się, że wybrano **nie aktualizacje automatyczne** opcji.


**Krok 20:** wybierz opcje domyślne. Jeśli chcesz openSSH dla połączenia SSH, wybierz opcję **OpenSSH serwera** opcji, a następnie wybierz **Kontynuuj**.

![Wybierz oprogramowanie](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image19.png)

**Krok 21:** wybierz **tak**, a następnie wybierz **Enter**.

![Moduł ładujący rozruchu CHODNIKÓW instalacji](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image20.png)

**Krok 22:** wybierz odpowiednie urządzenie do instalacji moduł ładujący rozruchu (najlepiej **/dev/sda**), a następnie wybierz **Enter**.

![Wybierz urządzenie na potrzeby instalacji moduł ładujący rozruchu](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image21.png)

**Krok 23:** wybierz **Kontynuuj**, a następnie wybierz **Enter** do zakończenia instalacji.

![Zakończenie instalacji](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image22.png)

Po zakończeniu instalacji, zaloguj się do maszyny Wirtualnej z nowymi poświadczeniami użytkownika. (Zobacz **kroku 10** Aby uzyskać więcej informacji.)

Wykonaj kroki opisane na poniższym zrzucie ekranu, aby ustawić hasła użytkownika ROOT. Następnie zaloguj się jako użytkownik ROOT.

![Ustaw główny hasło użytkownika](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image23.png)


### <a name="prepare-the-machine-for-configuration-as-a-master-target-server"></a>Przygotowuje komputer do konfiguracji jako główny serwer docelowy
Następnie przygotowuje komputer do konfiguracji jako główny serwer docelowy.

Aby uzyskać identyfikator dla każdego dysku twardego SCSI maszyny wirtualnej systemu Linux, Włącz **dysku. EnableUUID = TRUE** parametru.

Aby włączyć ten parametr, należy wykonać następujące czynności:

1. Zamknij maszynę wirtualną.

2. Kliknij prawym przyciskiem myszy wpis dla maszyny wirtualnej w okienku po lewej stronie, a następnie wybierz **Edytuj ustawienia**.

3. Wybierz **opcje** kartę.

4. W okienku po lewej stronie wybierz **zaawansowane** > **ogólne**, a następnie wybierz **parametry konfiguracji** przycisk w prawej dolnej części ekranu.

    ![Karta Opcje](./media/site-recovery-how-to-install-linux-master-target/media/image20.png)

    **Parametry konfiguracji** opcja jest niedostępna, gdy komputer jest uruchomiony. Aby uaktywnić na tej karcie, zamknij maszynę wirtualną.

5. Zobacz, czy wiersz z **dysku. EnableUUID** już istnieje.

    - Jeśli wartość istnieje i ma ustawioną wartość **False**, zmień wartość na **True**. (Wartości nie jest rozróżniana).

    - Jeśli wartość istnieje i ma ustawioną wartość **True**, wybierz pozycję **anulować**.

    - Jeśli wartość nie istnieje, wybierz **Dodaj wiersz**.

    - W kolumnie Nazwa Dodaj **dysku. EnableUUID**, a następnie ustaw wartość **TRUE**.

    ![Sprawdzanie Określa, czy na dysku. EnableUUID już istnieje.](./media/site-recovery-how-to-install-linux-master-target/media/image21.png)

#### <a name="disable-kernel-upgrades"></a>Wyłącz uaktualnienia jądra

Azure Site Recovery główny serwer docelowy wymaga bardzo określonych wersji systemu Ubuntu, upewnij się, że uaktualnienia jądra są wyłączone dla maszyny wirtualnej.

Jeśli uaktualnienia jądra są włączone, regularne uaktualnienia spowodować główny serwer docelowy do nieprawidłowego działania.

#### <a name="download-and-install-additional-packages"></a>Pobieranie i instalowanie dodatkowych pakietów

> [!NOTE]
> Upewnij się, że masz połączenie z Internetem na pobieranie i instalowanie dodatkowych pakietów. Jeśli nie masz połączenie z Internetem, musisz ręcznie te pakiety RPM Znajdź i zainstaluj je.

```
apt-get install -y multipath-tools lsscsi python-pyasn1 lvm2 kpartx
```

### <a name="get-the-installer-for-setup"></a>Pobierz Instalator dla Instalatora

Jeśli urządzenie docelowe głównej ma połączenie z Internetem, można użyć następujących kroków pobrać Instalatora. W przeciwnym razie należy skopiować Instalator z serwera przetwarzania, a następnie zainstaluj go.

#### <a name="download-the-master-target-installation-packages"></a>Pobierz pakiety instalacyjne główny serwer docelowy

[Pobrać najnowsze oprogramowanie instalacji główny cel Linux](https://aka.ms/latestlinuxmobsvc).

Aby go pobrać przy użyciu systemu Linux, wpisz:

```
wget https://aka.ms/latestlinuxmobsvc -O latestlinuxmobsvc.tar.gz
```

Upewnij się, można pobrać i Rozpakuj Instalatora w katalogu macierzystym. Jeśli plików do **/usr/Local**, instalacja zakończy się niepowodzeniem.


#### <a name="access-the-installer-from-the-process-server"></a>Dostęp do Instalatora z serwera przetwarzania

1. Na serwerze przetwarzania, przejdź do **\Microsoft Azure Site Recovery\home\svsystems\pushinstallsvc\repository C:\Program Files (x86)**.

2. Skopiuj plik Instalatora wymagane z serwera przetwarzania i zapisz go jako **latestlinuxmobsvc.tar.gz** w katalogu macierzystym.


### <a name="apply-custom-configuration-changes"></a>Zastosuj zmiany konfiguracji niestandardowej

Aby zastosować zmiany konfiguracji niestandardowej, należy użyć następujące kroki:


1. Uruchom następujące polecenie, aby untar dane binarne.
    ```
    tar -zxvf latestlinuxmobsvc.tar.gz
    ```
    ![Zrzut ekranu przedstawiający polecenia do uruchomienia](./media/site-recovery-how-to-install-linux-master-target/image16.png)

2. Uruchom następujące polecenie, aby udzielić zezwolenia.
    ```
    chmod 755 ./ApplyCustomChanges.sh
    ```

3. Uruchom następujące polecenie, aby uruchomić skrypt.
    ```
    ./ApplyCustomChanges.sh
    ```
> [!NOTE]
> Uruchom skrypt tylko raz na serwerze. Zamknij serwer. Następnie uruchom ponownie serwer po dodaniu dysku, zgodnie z opisem w następnej sekcji.

### <a name="add-a-retention-disk-to-the-linux-master-target-virtual-machine"></a>Dodaj dysk przechowywania do głównej docelowej maszyny wirtualnej systemu Linux

Wykonaj następujące kroki, aby utworzyć dysk przechowywania:

1. Dołącz nowy dysk 1 TB do głównej docelowej maszyny wirtualnej systemu Linux, a następnie uruchomienie maszyny.

2. Użyj **wielościeżkowe -ll** polecenie, aby dowiedzieć się wielościeżkowe identyfikator dysku przechowywania.

    ```
    multipath -ll
    ```
    ![Wielościeżkowe identyfikator dysku przechowywania](./media/site-recovery-how-to-install-linux-master-target/media/image22.png)

3. Sformatuj dysk, a następnie utworzyć systemu plików na nowym dysku.

    ```
    mkfs.ext4 /dev/mapper/<Retention disk's multipath id>
    ```
    ![Tworzenie system plików na dysku](./media/site-recovery-how-to-install-linux-master-target/media/image23.png)

4. Po utworzeniu systemu plików, należy zainstalować dysk przechowywania.
    ```
    mkdir /mnt/retention
    mount /dev/mapper/<Retention disk's multipath id> /mnt/retention
    ```
    ![Instalowanie dysku przechowywania](./media/site-recovery-how-to-install-linux-master-target/media/image24.png)

5. Utwórz **fstab** wejścia, należy zainstalować dysk przechowywania, przy każdym uruchomieniu systemu.
    ```
    vi /etc/fstab
    ```
    Wybierz **Wstaw** aby rozpocząć edytowanie pliku. Utwórz nowy wiersz, a następnie wstaw następujący tekst. Edytuj identyfikator wielościeżkowe dysku, na podstawie Identyfikatora wielościeżkowe wyróżnione z poprzednie polecenie.

     **/dev/mapowania/ <Retention disks multipath id> /mnt/rw ext4 przechowywania 0 0**

    Wybierz **Esc**, a następnie wpisz **: wq** (zapisać i zamknąć) aby zamknąć okno edytora.

### <a name="install-the-master-target"></a>Zainstaluj główny cel

> [!IMPORTANT]
> Wersja główny serwer docelowy musi być późniejsza niż w przypadku wersji serwera przetwarzania i serwer konfiguracji. Jeśli ten warunek nie jest spełniony, ponownej ochrony zakończy się pomyślnie, ale replikacja nie powiedzie się.


> [!NOTE]
> Przed zainstalowaniem głównego serwera docelowego sprawdź, czy **/etc/hosts** pliku na maszynie wirtualnej zawiera wpisy, które mapują lokalną nazwą hosta do adresów IP, które są skojarzone z wszystkich kart sieciowych.

1. Kopiuj hasło z **Recovery\private\connection.passphrase witryny Azure C:\ProgramData\Microsoft** na serwerze konfiguracji. Następnie zapisz go jako **passphrase.txt** w tym samym katalogu lokalnego, uruchamiając następujące polecenie:

    ```
    echo <passphrase> >passphrase.txt
    ```
    Przykład: 
    
    ```
    echo itUx70I47uxDuUVY >passphrase.txt
    ```

2. Należy pamiętać, adres IP serwera konfiguracji. Należy go w następnym kroku.

3. Uruchom następujące polecenie, aby zainstalować główny serwer docelowy i Zarejestruj serwer z serwerem konfiguracji.

    ```
    ./install -q -d /usr/local/ASR -r MT -v VmWare
    /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i <ConfigurationServer IP Address> -P passphrase.txt
    ```

    Przykład: 
    
    ```
    /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i 104.40.75.37 -P passphrase.txt
    ```

    Poczekaj na zakończenie skryptu. Czy główny cel rejestruje pomyślnie, główny cel są dostępne w **infrastruktura usługi Site Recovery** strony portalu.


#### <a name="install-the-master-target-by-using-interactive-installation"></a>Zainstaluj główny cel, używając instalacja interakcyjna

1. Uruchom następujące polecenie, aby zainstalować główny cel. W roli agenta, wybierz opcję **docelowego elementu głównego**.

    ```
    ./install
    ```

2. Wybierz domyślną lokalizację instalacji, a następnie wybierz **Enter** aby kontynuować.

    ![Wybieranie domyślnej lokalizacji instalacji główny serwer docelowy](./media/site-recovery-how-to-install-linux-master-target/image17.png)

Po zakończeniu instalacji należy zarejestrować serwer konfiguracji przy użyciu wiersza polecenia.

1. Zanotuj adres IP serwera konfiguracji. Należy go w następnym kroku.

2. Uruchom następujące polecenie, aby zainstalować główny serwer docelowy i Zarejestruj serwer z serwerem konfiguracji.

    ```
    ./install -q -d /usr/local/ASR -r MT -v VmWare
    /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i <ConfigurationServer IP Address> -P passphrase.txt
    ```
    Przykład: 

    ```
    /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i 104.40.75.37 -P passphrase.txt
    ```

   Poczekaj na zakończenie skryptu. Czy główny cel został pomyślnie zarejestrowany, główny docelowy są dostępne w **infrastruktura usługi Site Recovery** strony portalu.


### <a name="upgrade-the-master-target"></a>Uaktualnij główny cel

Uruchom Instalatora. Automatycznie wykrywa, że agent jest zainstalowany w głównym celu. Aby przeprowadzić uaktualnienie, wybierz **Y**.  Po zakończeniu instalacji sprawdź wersję programu głównego celu zainstalowane za pomocą następującego polecenia.

    ```
    cat /usr/local/.vx_version
    ```

Można stwierdzić, że **wersji** pola zwraca numer wersji głównego celu.

### <a name="install-vmware-tools-on-the-master-target-server"></a>Zainstaluj narzędzia VMware na głównym serwerze docelowym

Musisz zainstalować narzędzia VMware w głównym celu tak, aby go odnaleźć w magazynie danych. Jeśli narzędzia nie są zainstalowane, ekranu ponownej ochrony nie ma na liście w magazynie danych. Po zakończeniu instalacji narzędzi VMware należy ponownie uruchomić.

## <a name="next-steps"></a>Następne kroki
Po instalacji i rejestracji główny cel zawiera finsihed, można wyświetlić głównego celu, które są wyświetlane na **docelowego elementu głównego** sekcji **infrastruktura usługi Site Recovery**, w obszarze Przegląd serwera konfiguracji.

Teraz można przystąpić do [przełączonej](site-recovery-how-to-reprotect.md), a następnie powrót po awarii.

## <a name="common-issues"></a>Typowe problemy

* Upewnij się, że nie należy włączać narzędzia Storage vMotion na wszystkie składniki zarządzania, takie jak głównego celu. Jeśli po pomyślnej ponownej ochrony jest przenoszony główny cel, nie można odłączyć dysków maszyny wirtualnej (VMDKs). W takim przypadku powrotu po awarii nie powiedzie się.

* Główny cel nie powinna mieć żadnych migawek na maszynie wirtualnej. W przypadku migawki powrotu po awarii nie powiedzie się.

* Z powodu niektórych niestandardowych konfiguracji karty Sieciowej na komputerach niektórych klientów interfejs sieciowy jest wyłączona podczas uruchamiania, a nie można zainicjować agenta głównego celu. Upewnij się, że następujące właściwości są ustawione poprawnie. Sprawdź tych właściwości w sieci Ethernet karty /etc/sysconfig/network-scripts/ifcfg pliku-eth *.
    * BOOTPROTO = dhcp
    * ONBOOT = tak
