---
title: "Rozwiązywanie problemów z serwerem kopii zapasowej Azure | Dokumentacja firmy Microsoft"
description: "Rozwiązywania problemów z instalacją, rejestracji serwera kopii zapasowej Azure i tworzenia kopii zapasowej i przywracania obciążeń aplikacji"
services: backup
documentationcenter: 
author: pvrk
manager: shreeshd
editor: 
ms.assetid: 2d73c349-0fc8-4ca8-afd8-8c9029cb8524
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/24/2017
ms.author: pullabhk;markgal;
ms.openlocfilehash: 696f86f616575364bb65021260daf0c8458fc4e9
ms.sourcegitcommit: f847fcbf7f89405c1e2d327702cbd3f2399c4bc2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2017
---
# <a name="troubleshoot-azure-backup-server"></a>Rozwiązywanie problemów ze składnikiem Azure Backup Server

Można rozwiązać błędów napotkanych podczas za pomocą serwera usługi Kopia zapasowa Azure informacje wymienione w poniższej tabeli.

## <a name="invalid-vault-credentials-provided"></a>Udostępniono nieprawidłowe poświadczenia magazynu 

Postępuj zgodnie z następującymi [kroki rozwiązywania problemów] (https://docs.microsoft.com/en-us/azure/backup/backup-azure-mabs-troubleshoot#registration-and-agent-related-issues), aby rozwiązać ten problem.

## <a name="the-agent-operation-failed-because-of-a-communication-error-with-the-dpm-agent-coordinator-service-on-server"></a>Operacja agenta nie powiodła się z powodu błędu komunikacji z koordynatorem agenta programu DPM na serwerze 

Postępuj zgodnie z następującymi [kroki rozwiązywania problemów] (https://docs.microsoft.com/en-us/azure/backup/backup-azure-mabs-troubleshoot#registration-and-agent-related-issues), aby rozwiązać ten problem.

## <a name="setup-could-not-update-registry-metadata"></a>Instalator nie może zaktualizować rejestru metadanych

Postępuj zgodnie z następującymi [kroki rozwiązywania problemów] (https://docs.microsoft.com/en-us/azure/backup/backup-azure-mabs-troubleshoot#installation-issues), aby rozwiązać ten problem.


## <a name="installation-issues"></a>Problemy z instalacją

| Operacja | Szczegóły błędu | Obejście problemu |
|-----------|---------------|------------|
|Instalacja | Instalator nie może zaktualizować rejestru metadanych. Błąd tej aktualizacji może prowadzić do przez użycie wykorzystania magazynu. Aby uniknąć tego zaktualizuj wpis rejestru przycinanie system plików ReFS. | Dostosuj SYSTEM\CurrentControlSet\Control\FileSystem\RefsEnableInlineTrim klucza rejestru. Ustaw wartość Dword na 1. |
|Instalacja | Instalator nie może zaktualizować rejestru metadanych. Błąd tej aktualizacji może prowadzić do przez użycie wykorzystania magazynu. Aby uniknąć tego zaktualizuj wpis rejestru SnapOptimization woluminu. | Utwórz klucz rejestru SOFTWARE\Microsoft Manager\Configuration\VolSnapOptimization\WriteIds ochrony danych, z wartością pusty ciąg. |

## <a name="registration-and-agent-related-issues"></a>Problemy związane z rejestracji i agenta
| Operacja | Szczegóły błędu | Obejście problemu |
| --- | --- | --- |
| Rejestrowanie do magazynu | Udostępniono nieprawidłowe poświadczenia magazynu. Plik jest uszkodzony lub jest nie ma najnowszych poświadczeń skojarzonych z usługą odzyskiwania | Aby naprawić ten błąd: <ol><li> Pobierz najnowszy plik poświadczeń z magazynu i spróbuj ponownie <br>(LUB)</li> <li> Jeśli powyższe akcji nie powiodło się, spróbuj pobrać poświadczenia do innego katalogu lokalnego lub Utwórz nowy magazyn <br>(LUB)</li> <li> Spróbuj zaktualizować daty i godziny ustawienia, zgodnie z [ten blog](https://azure.microsoft.com/blog/troubleshooting-common-configuration-issues-with-azure-backup/) <br>(LUB)</li> <li> Sprawdź, czy c:\windows\temp ma więcej niż 65000 plików. Przenieś stare pliki do innej lokalizacji lub usuń elementy w folderze Temp <br>(LUB)</li> <li> Sprawdź stan certyfikatów <br> a. Otwórz "Zarządzanie certyfikatów komputera" (w Panelu sterowania) <br> b. Rozwiń węzeł "Osobiste" i jego węzła podrzędnego "Certyfikaty" <br> c.  Usuń certyfikat "Narzędzi systemu Windows Azure" <br> d. Ponów próbę rejestracji w kliencie usługi Kopia zapasowa Azure <br> (LUB) </li> <li> Sprawdź, czy wszystkie zasad grupy w miejscu </li></ol> |
| Wypychanie agentów na chronionych serwerach | Operacja agenta nie powiodła się z powodu błędu komunikacji z usługą koordynatora agentów DPM \<ServerName > | **Jeśli nie działa zalecanej akcji, które przedstawiono w produkcie**, <ol><li> Dołączając komputera z niezaufanej domeny, wykonaj [te](https://technet.microsoft.com/library/hh757801(v=sc.12).aspx) kroki <br> (LUB) </li><li> Dołączając komputer w zaufanej domenie, rozwiązywanie problemów przy użyciu kroków opisanych w [ten blog](https://blogs.technet.microsoft.com/dpm/2012/02/06/data-protection-manager-agent-network-troubleshooting/) <br>(LUB)</li><li> Spróbuj wyłączyć oprogramowanie antywirusowe podczas rozwiązywania problemów. Jeśli został rozwiązany, zmodyfikuj ustawienia antywirusowe zgodnie z sugestią podaną w [w tym artykule] (https://technet.microsoft.com/library/hh757911.aspx)</li></ol> |
| Wypychanie agentów na chronionych serwerach | Poświadczenia określone dla serwera są nieprawidłowe | **Jeśli nie działa zalecanej akcji, które przedstawiono w produkcie**, <br> spróbuj ręcznie zainstalować agenta ochrony na serwerze produkcyjnym, jak określono w [w tym artykule](https://technet.microsoft.com/library/hh758186(v=sc.12).aspx#BKMK_Manual)|
| Agent usługi Kopia zapasowa Azure nie może połączyć się z usługą kopia zapasowa Azure (identyfikator: 100050) | Azure Backup Agent nie mógł połączyć się z usługą kopia zapasowa Azure. | **Jeśli nie działa zalecanej akcji, które przedstawiono w produkcie**, <br>1. Uruchom następujące polecenia z podwyższonym poziomem uprawnień w wierszu, psexec -i -s "c:\Program Files\Internet Explorer\iexplore.exe" będzie Otwórz okno programu internet explorer. <br/> 2. Przejdź do pozycji Narzędzia -> Opcje internetowe -> połączenia -> Ustawienia sieci LAN. <br/> 3. Sprawdź ustawienia serwera proxy dla konta System. Ustaw Proxy adresu IP i portu. <br/> 4. Zamknij program Internet Explorer.|
| Nie można zainstalować agenta usługi Kopia zapasowa Azure | Instalacja usług odzyskiwania Microsoft Azure nie powiodła się. Wszystkie zmiany wprowadzone przez instalację usług odzyskiwania Microsoft Azure w systemie zostały wycofane. (IDENTYFIKATOR: 4024) | Ręcznie zainstaluj agenta platformy Azure


## <a name="configuring-protection-group"></a>Konfigurowanie grupy ochrony
| Operacja | Szczegóły błędu | Obejście problemu |
| --- | --- | --- |
| Konfigurowanie grup ochrony | Program DPM nie mógł wyliczyć składnika aplikacji na chronionym komputerze (Protected nazwa) | Kliknij przycisk "Odśwież" na ekranie konfiguracji ochrony grupy interfejsu użytkownika na odpowiednim poziomie źródła danych/składnika |
| Konfigurowanie grup ochrony | Nie można skonfigurować ochrony | Serwer chroniony w przypadku programu SQL server, sprawdź, czy uprawnienia roli sysadmin zostały przedstawione konto systemowe (NTAuthority\System) na chronionym komputerze zgodnie z [w tym artykule](https://technet.microsoft.com/library/hh757977(v=sc.12).aspx)
| Konfigurowanie grup ochrony | Brak wolnego miejsca w puli magazynu dla tej grupy ochrony | Dyski, które są dodawane do puli magazynu [partycji nie powinna zawierać](https://technet.microsoft.com/library/hh758075(v=sc.12).aspx). Usuń wszystkie woluminy istniejące na dyskach, a następnie dodaj go do puli magazynu|
| Zmiana zasad |Nie można modyfikować zasady tworzenia kopii zapasowej. Błąd: Bieżąca operacja nie powiodła się z powodu wewnętrznego błędu usługi [0x29834]. Spróbuj wykonać ponownie operację za jakiś czas. Jeśli problem będzie nadal występować, skontaktuj się z pomocą techniczną firmy Microsoft. |**Przyczyna:**<br/>Ten błąd jest dostarczany, gdy ustawienia zabezpieczeń zostały włączone, spróbuj zmniejszyć zakres przechowywania, poniżej wartości minimalnej wymienione powyżej i znajdują się na nieobsługiwaną wersję (poniżej wersja MAB 2.0.9052 i kopia zapasowa Azure serwera update 1). <br/>**Zalecana akcja:**<br/> W takim przypadku należy ustawić okres przechowywania powyżej minimalne przechowywania podanego okresu (siedem dni codziennie, czterech tygodni, przez co tydzień, trzy tygodnie, co miesiąc lub rok dla corocznej) kontynuowanie aktualizacje związane z zasadami. Opcjonalnie zaktualizuj agenta kopii zapasowej i kopii zapasowej serwera Azure wykorzystać wszystkie aktualizacje zabezpieczeń jest preferowana metoda. |

## <a name="backup"></a>Tworzenie kopii zapasowych
| Operacja | Szczegóły błędu | Obejście problemu |
| --- | --- | --- |
| Tworzenie kopii zapasowych | Replika jest niespójna | Upewnij się, że automatycznej kontroli spójności zaznacz opcję w grupie ochrony, Kreator jest włączona. Znajdź więcej szczegółów dotyczących przyczyn niespójności repliki i sugestie odpowiednich [tutaj](https://technet.microsoft.com/library/cc161593.aspx) <br> <ol><li> W przypadku kopii zapasowej stanu systemu/BMR Sprawdź, czy kopia zapasowa systemu Windows Server jest zainstalowany lub nie na serwer chroniony </li><li> Sprawdź, czy miejsce związane problemy magazynu DPM na serwerze programu DPM/MABS w puli i przydzielania magazynu, zgodnie z wymaganiami </li><li> Sprawdź stan usługi woluminów w tle kopii na serwerze chronionym. Jeśli jest w stanie wyłączenia Ustaw ręcznie i uruchom usługę na serwerze. Następnie wróć do konsoli programu DPM/MABS i Rozpocznij synchronizację z zadanie sprawdzania spójności.</li></ol>|
| Tworzenie kopii zapasowych | Wystąpił nieoczekiwany błąd, gdy zadanie było uruchomione, urządzenie nie jest gotowy | **Jeśli nie działa zalecanej akcji, które przedstawiono w produkcie**, <br> <ol><li>Ustaw obszar magazynu kopii w tle nieograniczoną liczbę elementów w grupie ochrony, a następnie uruchom sprawdzanie spójności <br></li> (LUB) <li>Spróbuj usunąć istniejącą grupę ochrony i tworzenie wielu nowych — jeden z każdego elementu w nim</li></ol> |
| Tworzenie kopii zapasowych | W przypadku wykonywania kopii zapasowej tylko stanu systemu, sprawdź, czy jest wystarczająco dużo wolnego miejsca na chronionym komputerze do przechowywania kopii zapasowej stanu systemu | <ol><li>Sprawdź, czy WSB na chronionym komputerze jest zainstalowany</li><li>Sprawdź, czy występuje na komputerze chronionym dla stanu systemu jest wystarczająca ilość miejsca: w tym celu najłatwiej przejdź do grupy ochrony, otwórz usługę WSB i kliknij opcje i wybierz odzyskiwania systemu od ZERA. Interfejs użytkownika informuje, ile miejsca jest wymagane dla tego. Lokalny -> Otwórz WSB backup -> kopii zapasowej harmonogram -> Wybierz opcję tworzenia kopii zapasowej konfiguracji -> całego serwera (rozmiar jest wyświetlana). Użyj tego rozmiaru do weryfikacji.</li></ol>
| Tworzenie kopii zapasowych | Tworzenie punktu odzyskiwania w trybie online nie powiodło się. | Jeśli komunikat o błędzie "systemu Windows Azure Backup Agent nie mógł utworzyć migawki wybranego woluminu", spróbuj zwiększenie rozmiaru repliki i odzyskiwania na woluminie punktu.
| Tworzenie kopii zapasowych | Tworzenie punktu odzyskiwania w trybie online nie powiodło się. | Komunikat o błędzie jest wyświetlany komunikat "Systemu Windows Azure Backup Agent nie może połączyć się z usługą OBEngine" Sprawdź, czy OBEngine istnieje na liście z uruchomionymi usługami na komputerze. Jeśli z usługą OBEngine nie jest uruchomiony. Użyj polecenia "net start OBEngine", aby uruchomić z usługą OBEngine.
| Tworzenie kopii zapasowych | Tworzenie punktu odzyskiwania w trybie online nie powiodło się. | Jeśli komunikat o błędzie "nie ustawiono hasła szyfrowania dla tego serwera. Skonfiguruj hasło szyfrowania"Spróbuj Konfigurowanie hasło szyfrowania. W przypadku niepowodzenia <br> <ol><li>Sprawdź, czy pliki tymczasowe lokalizacji istnieje lub nie. Lokalizacja wspomnianego rejestru HKEY_LOCAL_MACHINE\Software\Microsoft\Windows Azure Backup\Config o nazwie "ScratchLocation" powinna istnieć.</li><li> Jeśli istnieje lokalizacja pliki tymczasowe, spróbuj ponowne rejestrowanie przy użyciu starego hasła. **Gdy hasło szyfrowania można skonfigurować, najpierw zapisz go w bezpiecznej lokalizacji**</li><ol>
| Tworzenie kopii zapasowych | Niepowodzenia wykonywania kopii zapasowej BMR | Jeśli rozmiar odzyskiwania systemu od ZERA jest ogromny, ponów próbę po przeniesienie niektórych plików aplikacji na dysku systemu operacyjnego |
| Tworzenie kopii zapasowych | Ponownie ochronę maszyny Wirtualnej VMWare na nowym serwerze MABS nie są wyświetlane jako dostępne do dodania. | Właściwości VMWare są wskazany serwer MABS stare, wycofane. Aby rozwiązać ten problem: W programie VCenter (SC-VMM odpowiednik), przejdź do karty "Podsumowanie" i "Atrybuty niestandardowe".  Usuń starą nazwę serwera MABS z wartości "DPMServer".  Wróć do nowego serwera MABS i modyfikować strony  Po użyciu przycisku "Odśwież", maszyna wirtualna zostanie wyświetlone pole wyboru jako dostępny, aby dodać do ochrony. |
| Tworzenie kopii zapasowych | Błąd podczas uzyskiwania dostępu do udostępnionych plików/folderów | Spróbuj zmodyfikować ustawienia antywirusowe zgodnie z sugestią podaną [tutaj](https://technet.microsoft.com/library/hh757911.aspx)|
| Tworzenie kopii zapasowych | Zadanie tworzenia punktu odzyskiwania w trybie online dla maszyny Wirtualnej VMware nie powiedzie się. Program DPM napotkał błąd z VMware, podczas próby pobrania informacji śledzenia zmian. ErrorCode — FileFaultFault (identyfikator 33621) |  1. Resetuj ctk VMware, odpowiednich maszyn wirtualnych <br/> 2. Sprawdź, czy niezależne dysk nie jest stosowane w programie VMWare <br/> 3. Zatrzymaj ochronę odpowiednich maszyn wirtualnych i Wznów ochronę z przycisku Odśwież <br/> 4. Uruchom DW dla odpowiednich maszyn wirtualnych|


## <a name="change-passphrase"></a>Zmień hasło
| Operacja | Szczegóły błędu | Obejście problemu |
| --- | --- | --- |
| Zmień hasło |Zabezpieczenia wprowadzony numer PIN jest niepoprawny. Podaj poprawny numer PIN zabezpieczeń, aby wykonać tę operację. |**Przyczyna:**<br/> Ten błąd jest dostarczany podczas wprowadzania numeru PIN zabezpieczeń nieważny lub wygasły podczas wykonywania krytycznej operacji (jak zmienić hasło). <br/>**Zalecana akcja:**<br/> Aby ukończyć operację, wprowadź prawidłowy kod PIN zabezpieczeń. Aby uzyskać numer PIN, zaloguj się do portalu Azure i przejdź do magazynu usług odzyskiwania > Ustawienia > Właściwości > Generowanie kodu PIN zabezpieczeń. Ten numer PIN umożliwia zmianę hasła. |
| Zmień hasło |Operacja nie powiodła się. IDENTYFIKATOR: 120002 |**Przyczyna:**<br/>Ten błąd jest dostarczany podczas próby zmiany hasła, ustawienia zabezpieczeń zostały włączone, a na nieobsługiwanej wersji.<br/>**Zalecana akcja:**<br/> Aby zmienić hasło, należy zaktualizować agenta kopii zapasowej do minimalna wersja minimalna 2.0.9052 i kopia zapasowa Azure serwerowi minimalnej aktualizacji 1, wprowadź prawidłowy kod PIN zabezpieczeń. Aby uzyskać numer PIN, zaloguj się do portalu Azure i przejdź do magazynu usług odzyskiwania > Ustawienia > Właściwości > Generowanie kodu PIN zabezpieczeń. Ten numer PIN umożliwia zmianę hasła. |


## <a name="configure-email-notifications"></a>Skonfiguruj powiadomienia e-mail
| Operacja | Szczegóły błędu | Obejście problemu |
| --- | --- | --- |
| Próbujesz skonfigurować powiadomienia e-mail przy użyciu konta usługi Office 365. | Pobieranie Identyfikator błędu: 2013| **Przyczyna:**<br/> Próby użycia konta usługi Office 365 <br/> **Zalecana akcja:**<br/> 1. Pierwszą czynnością, aby upewnić się, jest to "Zezwalaj na anonimowe przekazywania na łącznika odbioru" dla serwera programu DPM Instalatora na serwerze Exchange. Oto [łącze](http://technet.microsoft.com/en-us/library/bb232021.aspx) na temat sposobu konfigurowania tego.  <br/> 2. Jeśli nie można używać wewnętrznego przekaźnika SMTP i należy skonfigurować za pomocą serwera usługi Office 365, usług IIS można skonfigurować jako przekaźnik dla tego. Konieczne będzie skonfigurowanie serwera programu DPM [możliwość przekazywania SMTP dla usługi Office 365 za pomocą usług IIS](https://technet.microsoft.com/en-us/library/aa995718(v=exchg.65).aspx) <br/><br/> Ważna Uwaga: W kroku 3 -> g -> ii, należy użyć user@domain.com format i nie domena\użytkownik <br/> Punkt programu DPM użyć lokalnego servername SMTP serwera, portu 587 i adres e-mail użytkownika, który wiadomości e-mail powinna pochodzić z. <br/> Nazwa użytkownika i hasło na stronie Instalatora programu DPM SMTP należy konto domeny w domenie, w którym znajduje się program DPM. <br/><br/> Uwaga: Jeśli zmiana adresu serwera SMTP, wprowadzić zmiany, aby nowe ustawienia, zamknij okno Ustawienia i ponownie otwórz aby upewnić się, że odzwierciedla nową wartość.  Po prostu zmieniania i testowania może nie zawsze przyjmować nowe ustawienia, testowanie w ten sposób jest najlepszym rozwiązaniem. <br/><br/> W dowolnym momencie w trakcie tego procesu można wyczyścić te ustawienia zamknięcie konsoli programu DPM, a następnie edytując następujące klucze rejestru:<br/> HKLM\SOFTWARE\Microsoft\Microsoft Data Protection Manager\Notification\ <br/> Usuń klucze SMTPPassword i SMTPUserName. <br/> Można dodać je ponownie w interfejsie użytkownika po uruchomieniu go ponownie.
