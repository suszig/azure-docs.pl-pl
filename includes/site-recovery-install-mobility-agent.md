---
title: 
description: 
services: site-recovery
documentationcenter: 
author: 
manager: 
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: 
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 
ms.author: 
translationtype: Human Translation
ms.sourcegitcommit: 4871e5959483058aa957e3373ddc5121b398e1d9
ms.openlocfilehash: 1d6d595bf788fc3fdba0bbc98887614e17c791dc


---



### <a name="install-the-mobility-service"></a>Instalowanie usługi mobilności
Aby włączyć ochronę dla maszyn wirtualnych i serwerów fizycznych, należy najpierw zainstalować usługę mobilności. Można to zrobić na kilka sposobów:

* **Wypychanie serwera przetwarzania**: Po włączeniu replikacji na maszynie wypchnij i zainstaluj składnik usługi mobilności z serwera przetwarzania. 
*Uwaga!* Instalacja wypychana nie zostanie wykonana, jeśli na maszynach jest już uruchomiona aktualna wersja składnika.
* **Wypychanie w przedsiębiorstwie**: Zainstaluj składnik automatycznie przy użyciu przetwarzania wypychania w przedsiębiorstwie takiego jak usługa WSUS, program System Center Configuration Manager lub [usługa Azure Automation i konfiguracja żądanego stanu](site-recovery-automate-mobility-service-install.md). Zanim to zrobisz, skonfiguruj najpierw serwer konfiguracji.
* **Instalacja ręczna**: Zainstaluj składnik ręcznie na każdej maszynie, którą chcesz replikować. Zanim to zrobisz, skonfiguruj najpierw serwer konfiguracji.

#### <a name="prepare-for-automatic-push-on-windows-machines"></a>Przygotowanie do wypychania automatycznego na maszynach z systemem Windows
Oto jak przygotować maszyny z systemem Windows, aby usługa mobilności mogła zostać automatycznie zainstalowana przez serwer przetwarzania.

1. Utwórz konto, za pomocą którego serwer przetwarzania będzie mógł uzyskać dostęp do maszyny. To konto powinno mieć uprawnienia administratora (lokalnego lub domeny) i będzie użyte tylko na potrzeby instalacji wypychanej.

   > [!NOTE]
   > Jeśli nie korzystasz z konta domeny, należy wyłączyć kontrolę dostępu użytkowników zdalnych na maszynie lokalnej. Aby to zrobić, w kluczu rejestru HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System dodaj wpis DWORD LocalAccountTokenFilterPolicy o wartości 1. Aby dodać wpis rejestru z poziomu interfejsu wiersza polecenia, wpisz **`REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1`**.
   >
   >
2. W Zaporze systemu Windows na maszynie, którą chcesz chronić, wybierz pozycję **Zezwalaj aplikacji lub funkcji na dostęp przez Zaporę**. Włącz **Udostępnianie plików i drukarek** oraz **Instrumentację zarządzania Windows**. W przypadku maszyn należących do domeny możesz skonfigurować ustawienia zapory przy użyciu obiektu zasad grupy.

   ![Ustawienia zapory](./media/site-recovery-vmware-to-azure/mobility1.png)
3. Dodaj utworzone konto:

   * Otwórz narzędzie **cspsconfigtool**. Jest ono dostępne jako skrót na pulpicie i znajduje się w folderze [LOKALIZACJA INSTALACJI]\home\svsystems\bin.
   * Na karcie **Zarządzaj kontami** kliknij pozycję **Dodaj konto**.
   * Dodaj utworzone konto. Gdy po dodaniu konta włączysz replikację dla danej maszyny, konieczne będzie podanie poświadczeń.

#### <a name="prepare-for-automatic-push-on-linux-servers"></a>Przygotowanie do wypychania automatycznego na serwerach z systemem Linux
1. Upewnij się, że maszyna z systemem Linux, którą chcesz chronić, jest obsługiwana, zgodnie z [wymaganiami wstępnymi dotyczącymi chronionej maszyny](#protected-machine-prerequisites). Upewnij się, że nawiązano połączenie sieciowe między maszyną z systemem Linux a serwerem przetwarzania.
2. Utwórz konto, za pomocą którego serwer przetwarzania będzie mógł uzyskać dostęp do maszyny. Konto powinno być użytkownikiem głównym na źródłowym serwerze z systemem Linux i będzie zostanie ono użyte tylko na potrzeby instalacji wypychanej.

   * Otwórz narzędzie **cspsconfigtool**. Jest ono dostępne jako skrót na pulpicie i znajduje się w folderze [LOKALIZACJA INSTALACJI]\home\svsystems\bin.
   * Na karcie **Zarządzaj kontami** kliknij pozycję **Dodaj konto**.
   * Dodaj utworzone konto. Gdy po dodaniu konta włączysz replikację dla danej maszyny, konieczne będzie podanie poświadczeń.
3. Sprawdź, czy plik /etc/hosts na źródłowym serwerze z systemem Linux zawiera wpisy mapujące lokalną nazwę hosta na adres IP skojarzony ze wszystkimi kartami sieciowymi.
4. Zainstaluj najnowsze pakiety openssh, openssh-server i openssl na maszynie, którą chcesz replikować.
5. Upewnij się, że protokół SSH jest włączony i uruchomiony na porcie 22.
6. Włącz podsystem SFTP i uwierzytelnianie hasłem w pliku sshd_config w następujący sposób:

   * Zaloguj się jako użytkownik główny.
   * W pliku /etc/ssh/sshd_config znajdź wiersz zaczynający się od ciągu **PasswordAuthentication**.
   * Usuń znaczniki komentarza i zmień wartość **no** na **yes**.
   * Znajdź wiersz zaczynający się od ciągu **Subsystem** i usuń znaczniki komentarza.

     ![Linux](./media/site-recovery-vmware-to-azure/mobility2.png)

### <a name="install-the-mobility-service-manually"></a>Ręczne instalowanie usługi mobilności
Instalatory są dostępne na serwerze konfiguracji w katalogu **C:\Program Files (x86)\Microsoft Azure Site Recovery\home\svsystems\pushinstallsvc\repository**.

| Źródłowy system operacyjny | Plik instalacyjny usługi mobilności |
| --- | --- |
| Windows Server (tylko wersja&64;-bitowa) |Microsoft-ASR_UA_9.*.0.0_Windows_* release.exe |
| CentOS 6.4, 6.5, 6.6 (tylko wersja 64-bitowa) |Microsoft-ASR_UA_9.*.0.0_RHEL6-64_*release.tar.gz |
| SUSE Linux Enterprise Server 11 SP3 (tylko wersja 64-bitowa) |Microsoft-ASR_UA_9.*.0.0_SLES11-SP3-64_*release.tar.gz |
| Oracle Enterprise Linux 6.4, 6.5 (tylko wersja 64-bitowa) |Microsoft-ASR_UA_9.*.0.0_OL6-64_*release.tar.gz |

#### <a name="install-mobility-service-on-a-windows-server"></a>Instalowanie usługi mobilności w systemie Windows Server
1. Pobierz i zainstaluj odpowiedni instalator.
2. W obszarze **Przed rozpoczęciem** wybierz pozycję **Usługa mobilności**.

    ![Usługa mobilności](./media/site-recovery-vmware-to-azure/mobility3.png)
3. W obszarze **Szczegóły serwera konfiguracji** określ adres IP serwera konfiguracji i hasło wygenerowane po uruchomieniu ujednoliconego instalatora. Możesz pobrać hasło, uruchamiając polecenie **<SiteRecoveryInstallationFolder>\home\sysystems\bin\genpassphrase.exe –v** na serwerze konfiguracji.

    ![Usługa mobilności](./media/site-recovery-vmware-to-azure/mobility6.png)
4. W obszarze **Lokalizacja instalacji** zostaw ustawienia domyślne bez zmian i kliknij przycisk **Dalej**, aby rozpocząć instalację.
5. Możesz monitorować instalację w obszarze **Postęp instalacji**. Gdy zostanie wyświetlony monit, ponownie uruchom maszynę. Po zainstalowaniu usługi zaktualizowanie stanu w portalu może potrwać około 15 minut.

#### <a name="install-mobility-service-on-a-windows-server-using-the-command-prompt"></a>Instalowanie usługi mobilności w systemie Windows Server przy użyciu wiersza polecenia
1. Skopiuj instalator do folderu lokalnego (np. C:\Temp) na serwerze, który chcesz chronić. Instalator znajduje się na serwerze konfiguracji w katalogu **[lokalizacja instalacji]\home\svsystems\pushinstallsvc\repository**. Pakiet dla systemów operacyjnych Windows ma nazwę podobną do Microsoft-ASR_UA_9.3.0.0_Windows_GA_17thAug2016_release.exe
2. Zmień nazwę tego pliku na MobilitySvcInstaller.exe
3. Uruchom następujące polecenie, aby wyodrębnić instalator MSI:

    ``C:\> cd C:\Tempww
    ``C:\Temp> MobilitySvcInstaller.exe /q /xC:\Temp\Extracted``
    ``C:\Temp> cd Extracted``
    ``C:\Temp\Extracted> UnifiedAgent.exe /Role "Agent" /CSEndpoint "adres IP serwera konfiguracji" /PassphraseFilePath <Full path to the passphrase file>``

##### <a name="full-command-line-syntax"></a>Pełna składnia wiersza polecenia
    UnifiedAgent.exe [/Role <Agent/MasterTarget>] [/InstallLocation <Installation Directory>] [/CSIP <IP address of CS to be registered with>] [/PassphraseFilePath <Passphrase file path>] [/LogFilePath <Log File Path>]<br/>

**Parametry**

* **/Role:** Obowiązkowy. Określa, czy usługa mobilności powinna zostać zainstalowana. Wartości wejściowe Agent|MasterTarget
* **/InstallLocation:** Obowiązkowy. Określa, gdzie zainstalować usługę.
* **/PassphraseFilePath:** Obowiązkowy. Hasło serwera konfiguracji.
* **/LogFilePath:** Obowiązkowy. Lokalizacja, w której powinny zostać utworzone pliki dziennika instalacji.

#### <a name="uninstall-the-mobility-service-manually"></a>Ręczne odinstalowanie usługi mobilności
Usługę mobilności można odinstalować przy użyciu funkcji Dodaj/Usuń programy w Panelu sterowania lub przy użyciu tej instrukcji wiersza polecenia: MsiExec.exe /qn /x {275197FC-14FD-4560-A5EB-38217F80CBD1}

#### <a name="install-the-mobility-service-on-a-linux-server"></a>Instalowanie usługi mobilności na serwerze z systemem Linux
1. Skopiuj odpowiednie archiwum tar (zgodnie z powyższą tabelą) na maszynę z systemem Linux, którą chcesz replikować.
2. Otwórz program powłoki i wyodrębnij spakowane archiwum tar do ścieżki lokalnej, uruchamiając polecenie: `tar -xvzf Microsoft-ASR_UA_8.5.0.0*`
3. Utwórz plik passphrase.txt w katalogu lokalnym, do którego wyodrębniono zawartość archiwum tar. Aby to zrobić, skopiuj hasło z pliku C:\ProgramData\Microsoft Azure Site Recovery\private\connection.passphrase na serwerze konfiguracji i zapisz je w pliku passphrase.txt, uruchamiając polecenie *`echo <passphrase> >passphrase.txt`* w powłoce.
4. Zainstaluj usługę mobilności, uruchamiając polecenie *`sudo ./install -t both -a host -R Agent -d /usr/local/ASR -i <IP address> -p <port> -s y -c https -P passphrase.txt`*.
5. Określ wewnętrzny adres IP serwera konfiguracji i upewnij się, że wybrano port 443. Po zainstalowaniu usługi zaktualizowanie stanu w portalu może potrwać około 15 minut.

**Instalację można także przeprowadzić z poziomu wiersza polecenia**:

Skopiuj hasło z katalogu C:\Program Files (x86)\InMage Systems\private\connection na serwerze konfiguracji i zapisz je jako plik „passphrase.txt” na serwerze konfiguracji. Następnie uruchom te polecenia. W naszym przykładzie adres IP serwera konfiguracji to 104.40.75.37, a portem HTTPS powinien być port 443:


Aby zainstalować serwer produkcyjny:

    ./install -t both -a host -R Agent -d /usr/local/ASR -i 104.40.75.37 -p 443 -s y -c https -P passphrase.txt

Aby zainstalować główny serwer docelowy:

    ./install -t both -a host -R MasterTarget -d /usr/local/ASR -i 104.40.75.37 -p 443 -s y -c https -P passphrase.txt






<!--HONumber=Feb17_HO3-->


