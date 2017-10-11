---
title: "Rozwiązywanie problemów z Azure błędy kopii zapasowych w portalu klasycznym | Dokumentacja firmy Microsoft"
description: "Rozwiązywanie problemów z usługi Kopia zapasowa Azure i przywracanie maszyn wirtualnych platformy Azure w klasycznym portalu."
services: backup
documentationcenter: 
author: trinadhk
manager: shreeshd
editor: 
ms.assetid: 117201fb-c0cd-4be4-b47f-abd88fe914cf
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 1/23/2017
ms.author: trinadhk;markgal;
ms.openlocfilehash: 284a1b64fbb15d0aa800182c6671d447e191b76a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshoot-azure-virtual-machine-backup"></a>Rozwiązywanie problemów z kopiami zapasowymi maszyn wirtualnych platformy Azure
> [!div class="op_single_selector"]
> * [Magazyn usług odzyskiwania](backup-azure-vms-troubleshoot.md)
> * [Magazyn kopii zapasowych](backup-azure-vms-troubleshoot-classic.md)
>
>

Można rozwiązać błędów napotkanych podczas przy użyciu usługi Kopia zapasowa Azure informacje wymienione w poniższej tabeli.

## <a name="discovery"></a>Odnajdywania
| Operacja tworzenia kopii zapasowej | Szczegóły błędu | Obejście problemu |
| --- | --- | --- |
| Odnajdywania |Nie można rozpoznać nowe elementy -, a kopia zapasowa Microsoft Azure napotkał błąd wewnętrzny. Poczekaj kilka minut, a następnie spróbuj ponownie wykonać operację. |Po 15 minutach, ponów próbę wykonania procesu odnajdywania. |
| Odnajdywania |Nie można wykryć nowe elementy — kolejna operacja odnajdywania wykonana operacja jest już w toku. Poczekaj, aż do zakończenia bieżącej operacji odnajdywania. |Brak |

## <a name="register"></a>Zarejestruj subskrypcję
| Operacja tworzenia kopii zapasowej | Szczegóły błędu | Obejście problemu |
| --- | --- | --- |
| Zarejestruj subskrypcję |Liczba dysków danych dołączonych do maszyny wirtualnej przekracza obsługiwany limit — odłączyć niektórych dysków danych na tej maszynie wirtualnej i spróbuj ponownie wykonać operację. Kopia zapasowa Azure obsługuje maksymalnie 16 dyskach danych dołączonych do maszyny wirtualnej platformy Azure do utworzenia kopii zapasowej |Brak |
| Zarejestruj subskrypcję |Kopia zapasowa Microsoft Azure napotkał błąd wewnętrzny - Poczekaj kilka minut, a następnie spróbuj ponownie wykonać operację. Jeśli problem będzie się powtarzać, skontaktuj się z działem pomocy technicznej firmy Microsoft. |Możesz uzyskać tego błędu, ponieważ jeden z następujących nieobsługiwanej konfiguracji maszyny wirtualnej w warstwie Premium-LRS. <br> Magazyn w warstwie Premium maszyn wirtualnych utworzeniem kopii zapasowej za pomocą magazynu usług odzyskiwania. [Więcej informacji](backup-introduction-to-azure-backup.md#using-premium-storage-vms-with-azure-backup) |
| Zarejestruj subskrypcję |Rejestracja nie powiodła się limit czasu operacji instalacji agenta |Sprawdź, czy wersja systemu operacyjnego maszyny wirtualnej jest obsługiwana. |
| Zarejestruj subskrypcję |Wykonanie polecenia nie powiodło się — inna operacja jest w toku dla tego elementu. Czekaj, aż do zakończenia poprzedniej operacji |Brak |
| Zarejestruj subskrypcję |Maszyny wirtualne o wirtualnych dyskach twardych przechowywanych na magazyn w warstwie Premium nie są obsługiwane dla kopii zapasowej |Brak |
| Zarejestruj subskrypcję |Agent maszyny wirtualnej nie znajduje się na maszynie wirtualnej — Zainstaluj wymaganego wymagania wstępne, agent maszyny Wirtualnej, a następnie ponownie uruchom operację. |[Dowiedz się więcej](#vm-agent) informacji o instalacji agenta maszyny Wirtualnej i sprawdzania poprawności instalacji agenta maszyny Wirtualnej. |

## <a name="backup"></a>Tworzenie kopii zapasowych
| Operacja tworzenia kopii zapasowej | Szczegóły błędu | Obejście problemu |
| --- | --- | --- |
| Tworzenie kopii zapasowych |Nie można nawiązać komunikacji z agentem maszyny wirtualnej dla stanu migawki. Upłynął limit czasu zadania sub wirtualna migawki. — Można znaleźć w podręczniku rozwiązywania problemów na temat sposobu rozwiązania tego problemu. |Ten błąd jest zgłaszany, jeśli występuje problem z agentem maszyny Wirtualnej lub dostępu do sieci do infrastruktury platformy Azure są zablokowane w inny sposób. Dowiedz się więcej o [debugowania zapasowej maszyny Wirtualnej migawki problemów](backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout.md). <br> Jeśli agent maszyny Wirtualnej nie powoduje żadnych problemów, a następnie uruchom ponownie maszynę Wirtualną. W czasie niepoprawny stan maszyny Wirtualnej mogą powodować problemy i ponownego uruchamiania maszyny Wirtualnej resetuje to "nieprawidłowego stanu" |
| Tworzenie kopii zapasowych |Kopii zapasowej nie powiodło się z powodu wewnętrznego błędu - spróbuj ponownie wykonać operację w ciągu kilku minut. Jeśli problem będzie się powtarzać, skontaktuj się z pomocą techniczną firmy Microsoft Support |Sprawdź, czy jest to problem przejściowy podczas uzyskiwania dostępu do magazynu maszyny Wirtualnej. Sprawdź, czy [stan Azure](https://azure.microsoft.com/en-us/status/) Jeśli istnieje jakikolwiek problem w toku dotyczące obliczeń i magazynowania/sieci w regionie. Ponów operację, którego skuteczność została osłabiona problem post kopii zapasowej. |
| Tworzenie kopii zapasowych |Nie można wykonać operacji, ponieważ maszyna wirtualna już nie istnieje. |Nie można wykonać kopii zapasowej, ponieważ usunięto skonfigurowane dla kopii zapasowej maszyny Wirtualnej. Zatrzymaj dodatkowe kopie zapasowe, przechodząc do widoku elementy chronione, wybierz chronionego elementu i wybierz polecenie Zatrzymaj ochronę. Wybierając opcję danych zachować kopii zapasowej, można zachować danych. Później można wznowić ochronę dla tej maszyny wirtualnej, klikając na konfigurowanie ochrony z widoku zarejestrowane elementy |
| Tworzenie kopii zapasowych |Nie można zainstalować usług odzyskiwania Azure rozszerzenie wybranego elementu — Agent maszyny Wirtualnej jest jako warunek wstępny dla rozszerzenia usług odzyskiwania Azure. Zainstaluj agenta maszyny Wirtualnej platformy Azure i ponownie uruchom operację rejestracji |<ol> <li>Sprawdź, czy agent maszyny Wirtualnej został poprawnie zainstalowany. <li>Upewnij się, została poprawnie ustawiona flaga w konfiguracji maszyny Wirtualnej.</ol> [Dowiedz się więcej](#validating-vm-agent-installation) informacji o instalacji agenta maszyny Wirtualnej i sprawdzania poprawności instalacji agenta maszyny Wirtualnej. |
| Tworzenie kopii zapasowych |Wykonanie polecenia nie powiodło się — inna operacja jest obecnie w toku dla tego elementu. Czekaj, aż do zakończenia poprzedniej operacji, a następnie spróbuj ponownie |Uruchomione z istniejącej kopii zapasowej lub przywracania dla maszyny Wirtualnej, a nie można rozpocząć nowego zadania, istniejące zadanie jest uruchomiona. |
| Tworzenie kopii zapasowych |Instalacja rozszerzenia nie powiodło się z powodu błędu "COM + nie może komunikować się Microsoft Distributed Transaction Coordinator |Zwykle oznacza to, że usługa modelu COM + nie jest uruchomiona. Aby uzyskać pomoc dotyczącą rozwiązywania tego problemu, skontaktuj się z pomocą techniczną firmy Microsoft. |
| Tworzenie kopii zapasowych |Migawki operacja nie powiodła się z powodu błędu operacji VSS "ten dysk jest zablokowany przez szyfrowanie dysków funkcją BitLocker. Należy odblokować ten dysk z Panelu sterowania. |Wyłącz funkcję BitLocker dla wszystkich dysków na maszynie Wirtualnej i sprawdź, czy usługi VSS problem został rozwiązany |
| Tworzenie kopii zapasowych |Maszyny wirtualne o wirtualnych dyskach twardych przechowywanych na magazyn w warstwie Premium nie są obsługiwane dla kopii zapasowej |Brak |
| Tworzenie kopii zapasowych |Nie można odnaleźć maszyny wirtualnej platformy Azure. |Dzieje się tak, gdy podstawowej maszyny Wirtualnej jest usuwane, ale zasady tworzenia kopii zapasowych w dalszym ciągu wyszukiwania dla maszyny Wirtualnej wykonać kopię zapasową. Aby naprawić ten błąd: <ol><li>Utwórz ponownie maszynę wirtualną o tej samej nazwie i tej samej nazwy grupy zasobów [nazwa usługi w chmurze], <br>(LUB) <li> Wyłącz ochronę dla tej maszyny Wirtualnej tak, aby kolejnych kopii zapasowych nie zostanie uzyskać wywołane. </ol> |
| Tworzenie kopii zapasowych |Agent maszyny wirtualnej nie znajduje się na maszynie wirtualnej — Zainstaluj wymaganego wymagania wstępne, agent maszyny Wirtualnej, a następnie ponownie uruchom operację. |[Dowiedz się więcej](#vm-agent) informacji o instalacji agenta maszyny Wirtualnej i sprawdzania poprawności instalacji agenta maszyny Wirtualnej. |

## <a name="jobs"></a>Zadania
| Operacja | Szczegóły błędu | Obejście problemu |
| --- | --- | --- |
| Anulowanie zadania |Anulowanie nie jest obsługiwana dla tego typu zadania — poczekaj, aż zadanie zostało ukończone. |Brak |
| Anulowanie zadania |Zadanie nie jest w stanie można anulować — poczekaj, aż zadanie zostało ukończone. <br>LUB<br> Wybrane zadanie nie jest w stanie można anulować — Zaczekaj na ukończenie zadania. |Najprawdopodobniej zadanie jest niemal ukończone; Poczekaj, aż zadanie zostało ukończone |
| Anulowanie zadania |Nie można anulować zadania, ponieważ nie jest w toku — anulowania jest obsługiwana tylko dla zadania, które są w toku. Anuluj próba w toku zadania. |Dzieje się z powodu stanu przejściowymi. Poczekaj chwilę i ponów próbę wykonania operacji anulowania |
| Anulowanie zadania |Nie można anulować zadania - Zaczekaj na zakończenie zadania. |Brak |

## <a name="restore"></a>Przywracanie
| Operacja | Szczegóły błędu | Obejście problemu |
| --- | --- | --- |
| Przywracanie |Przywracanie nie powiodło się z powodu błędu wewnętrznego w chmurze |<ol><li>Usługi chmury, do której chcesz przywrócić jest skonfigurowany przy użyciu ustawień DNS. Możesz sprawdzić <br>$deployment = get-AzureDeployment - ServiceName "ServiceName"-miejsca "Production" Get-AzureDns - DnsSettings $deployment. DnsSettings<br>W przypadku skonfigurowany adres, oznacza to, czy ustawienia DNS są skonfigurowane.<br> <li>Usługi chmury, do której próbujesz przywrócić jest skonfigurowany z zastrzeżonego adresu IP i istniejących maszyn wirtualnych w usłudze w chmurze są w stanie zatrzymania.<br>Można sprawdzić, czy usługa w chmurze ma zastrzeżony adres IP za pomocą następujących poleceń cmdlet programu powershell:<br>$deployment = get-AzureDeployment - ServiceName "servicename"-gniazdo $ "Production" w programie dep. Nazwa zastrzeżonego adresu IP <br><li>Chcesz przywrócić maszynę wirtualną o następujące konfiguracje sieciowe specjalne w do tej samej usługi w chmurze. <br>— Maszyny wirtualne w konfiguracji usługi równoważenia obciążenia (wewnętrznych i zewnętrznych)<br>— Maszyny wirtualne z wielu zastrzeżonych adresów IP<br>— Maszyny wirtualne z wieloma kartami sieciowymi<br>Wybierz nową usługę w chmurze w interfejsie użytkownika lub zapoznaj się z [zagadnienia dotyczące przywracania](backup-azure-restore-vms.md#restoring-vms-with-special-network-configurations) dla maszyn wirtualnych z konfiguracjami sieci specjalne</ol> |
| Przywracanie |Wybranej nazwy DNS jest już zajęta — Określ inną nazwę DNS i spróbuj ponownie. |Tutaj nazwę DNS, który odwołuje się do nazwy usługi w chmurze (zazwyczaj kończąc. cloudapp.net). To musi być unikatowa. Jeśli wystąpi ten błąd, należy wybrać inną nazwę maszyny Wirtualnej podczas przywracania. <br><br> Ten błąd jest wyświetlany tylko dla użytkowników portalu Azure. Operacja przywracania za pomocą programu PowerShell zakończy się pomyślnie, ponieważ tylko przywraca dysków i nie tworzy maszynę Wirtualną. Błąd zostanie skierowany w przypadku maszyny Wirtualnej jest jawnie utworzone przez użytkownika, po operacji przywracania dysku. |
| Przywracanie |Konfiguracja określonej sieci wirtualnej jest nieprawidłowa — Określ konfiguracji innej sieci wirtualnej i spróbuj ponownie. |Brak |
| Przywracanie |Usługi w chmurze określonego używa zastrzeżonego adresu IP, który nie pasuje do konfiguracji maszyny wirtualnej są przywracane — Określ usługi inną chmurę, która nie używa zastrzeżonego adresu IP lub wybierz inny punkt odzyskiwania, aby przywrócić z. |Brak |
| Przywracanie |Usługi w chmurze osiągnięto limit liczby wejściowych punktów końcowych — spróbuj ponownie wykonać operację, określając innej usługi w chmurze lub przy użyciu istniejącego punktu końcowego. |Brak |
| Przywracanie |Konto magazynu kopii zapasowej magazynu i obiekt docelowy znajdują się w dwóch różnych regionach — upewnij się, że wybrane konto magazynu podczas operacji przywracania jest w tym samym regionie Azure jako magazynu kopii zapasowych. |Brak |
| Przywracanie |Konta magazynu określony dla operacji przywracania nie jest obsługiwana — konta magazynu tylko Basic/Standard z lokalnie nadmiarowego lub z magazynu geograficznie nadmiarowego replikacji ustawienia są obsługiwane. Wybierz konto magazynu obsługiwane |Brak |
| Przywracanie |Typ konta magazynu określony dla operacji przywracania nie jest w trybie online — upewnij się, że konta magazynu określony w operacji przywracania jest w trybie online |Taka sytuacja może wystąpić z powodu błędu przejściowego w usłudze Azure Storage lub z powodu awarii. Wybierz inne konto magazynu. |
| Przywracanie |Osiągnięto limit przydziału Grupa zasobów — Usuń niektóre grupy zasobów z portalu Azure lub skontaktuj się z pomocą techniczną platformy Azure w celu zwiększenia limitów. |Brak |
| Przywracanie |Wybrana podsieć nie istnieje — Wybierz podsieć, która istnieje |Brak |

## <a name="policy"></a>Zasady
| Operacja | Szczegóły błędu | Obejście problemu |
| --- | --- | --- |
| Tworzenie zasad |Nie można utworzyć zasad - Zmniejsz opcje przechowywania, aby kontynuować konfigurację zasad. |Brak |

## <a name="vm-agent"></a>Agent maszyny Wirtualnej
### <a name="setting-up-the-vm-agent"></a>Konfigurowanie agenta maszyny Wirtualnej
Zwykle agenta maszyny Wirtualnej jest już obecny na maszynach wirtualnych, które są tworzone z poziomu galerii Azure. Jednakże maszyn wirtualnych, które są migrowane z lokalnych centrów danych nie mieć zainstalowanego agenta maszyny Wirtualnej. Dla tych maszyn wirtualnych Agent maszyny Wirtualnej musi być jawnie zainstalowany. Przeczytaj więcej na temat [Instalowanie agenta maszyny Wirtualnej na istniejącej maszyny Wirtualnej](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx).

Dla maszyn wirtualnych systemu Windows:

* Pobierz i zainstaluj [plik MSI agenta](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). Do ukończenia procesu instalacji niezbędne są uprawnienia administratora.
* [Zaktualizuj właściwości maszyny wirtualnej](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx), aby wskazać, że agent jest zainstalowany.

Dla maszyn wirtualnych systemu Linux:

* Zainstaluj najnowsze [agenta systemu Linux](https://github.com/Azure/WALinuxAgent) z usługi github.
* [Zaktualizuj właściwości maszyny wirtualnej](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx), aby wskazać, że agent jest zainstalowany.

### <a name="updating-the-vm-agent"></a>Aktualizowanie agenta maszyny wirtualnej
Dla maszyn wirtualnych systemu Windows:

* Aktualizowanie agenta maszyny wirtualnej jest równie proste, jak ponowne zainstalowanie [plików binarnych agenta maszyny wirtualnej](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). Jednak należy się upewnić, że żadna operacja tworzenia kopii zapasowej jest uruchomiony podczas aktualizacji agenta maszyny Wirtualnej.

Dla maszyn wirtualnych systemu Linux:

* Postępuj zgodnie z instrukcjami [aktualizacja agenta maszyny Wirtualnej systemu Linux](../virtual-machines/linux/update-agent.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

### <a name="validating-vm-agent-installation"></a>Sprawdzanie poprawności instalacji agenta maszyny Wirtualnej
Jak sprawdzić, czy wersja agenta maszyny Wirtualnej na maszynach wirtualnych systemu Windows:

1. Zaloguj się do maszyny wirtualnej platformy Azure i przejdź do folderu *C:\WindowsAzure\Packages*. Powinien znajdować się w nim plik WaAppAgent.exe.
2. Kliknij plik prawym przyciskiem myszy, przejdź do opcji **Właściwości**, a następnie wybierz kartę **Szczegóły**. Wersja produktu pole powinno być 2.6.1198.718 lub nowszej
