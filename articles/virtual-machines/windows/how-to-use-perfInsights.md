---
title: "Jak używać PerfInsights na platformie Microsoft Azure | Dokumentacja firmy Microsoft"
description: "Uzyskuje informacje o sposobie używania PerfInsights do rozwiązywania problemów z wydajnością maszyny Wirtualnej systemu Windows."
services: virtual-machines-windows'
documentationcenter: 
author: genlin
manager: cshepard
editor: na
tags: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: troubleshooting
ms.date: 11/03/2017
ms.author: genli
ms.openlocfilehash: bb4c21456643532df040df4fcd5f4fa1a4f48d2c
ms.sourcegitcommit: 80eb8523913fc7c5f876ab9afde506f39d17b5a1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/02/2017
---
# <a name="how-to-use-perfinsights"></a>Jak używać PerfInsights 

[PerfInsights](http://aka.ms/perfinsightsdownload) jest automatyczne skrypt, który gromadzi informacje przydatne informacje diagnostyczne, działa obciążenia We/Wy obciążenia i zapewnia raport analizy, aby ułatwić rozwiązywanie problemów z wydajnością maszyny Wirtualnej systemu Windows w systemie Microsoft Azure. To może działać na maszynach wirtualnych jako skrypt autonomicznej lub bezpośrednio z portalu, instalując [rozszerzenia maszyny Wirtualnej systemu Azure wydajności diagnostyki](performance-diagnostics-vm-extension.md).

Firma Microsoft zaleca uruchomienie tego skryptu przed otwarciem biletu pomocy technicznej w firmie Microsoft problemy z wydajnością maszyny Wirtualnej.

## <a name="supported-troubleshooting-scenarios"></a>Obsługiwane scenariusze rozwiązywania problemów

PerfInsights można zbierać i analizować kilka rodzajów informacji, które są zgrupowane w scenariuszach unikatowy.

### <a name="collect-basic-configuration"></a>Zbierz dane o konfiguracji podstawowej 

W tym scenariuszu zbiera konfiguracji dysków oraz inne ważne informacje, w tym następujące elementy:

-   Dzienniki zdarzeń

-   Stan sieci dla wszystkich połączeń przychodzących i wychodzących

-   Ustawienia konfiguracji sieci i zapory

-   Listy zadań dla wszystkich aplikacji, które są aktualnie uruchomione w systemie

-   Informacje o pliku utworzonego przez msinfo32 dla maszyny wirtualnej (VM)

-   Ustawienia konfiguracji bazy danych programu Microsoft SQL Server (Jeśli maszyna wirtualna została zidentyfikowana jako serwer, na którym działa program SQL Server)

-   Liczniki niezawodności magazynu

-   Ważne poprawki systemu Windows

-   Sterowniki filtrów zainstalowany

To jest pasywny zbiór informacje, które nie powinny mieć wpływ na system. 

>[!Note]
>Ten scenariusz jest automatycznie uwzględnione w każdym z poniższych scenariuszy.

### <a name="benchmarking"></a>Przeprowadzenia testów porównawczych

W tym scenariuszu uruchamia [diskspd](https://github.com/Microsoft/diskspd) testu (IOPS i MB/s) dla wszystkich dysków dołączonych do maszyny Wirtualnej. 

> [!Note]
> W tym scenariuszu może mieć wpływ na system i nie powinny być uruchamiane na komputerze produkcyjnym. W razie potrzeby uruchom ten scenariusz, w oknie obsługi dedykowanych Aby uniknąć problemów. Większe obciążenie jest spowodowane testu śledzenia lub testu porównawczego może niekorzystnie wpłynąć na wydajność maszyny Wirtualnej.
>

### <a name="slow-vm-analysis"></a>Powolne analizy maszyny Wirtualnej 

W tym scenariuszu uruchamia [licznika wydajności](https://msdn.microsoft.com/library/windows/desktop/aa373083(v=vs.85).aspx) śledzenia przy użyciu liczniki, które są określone w pliku Generalcounters.txt. Jeśli maszyna wirtualna została zidentyfikowana jako serwer, na którym działa program SQL Server, uruchamia śledzenia licznika wydajności przy użyciu liczników, które znajdują się w pliku Sqlcounters.txt. Zawiera również dane diagnostyczne wydajności.

### <a name="slow-vm-analysis-and-benchmarking"></a>Powolne analizy maszyny Wirtualnej i testowania wydajności

W tym scenariuszu uruchamia [licznika wydajności](https://msdn.microsoft.com/library/windows/desktop/aa373083(v=vs.85).aspx) śledzenia, w którym następuje [diskspd](https://github.com/Microsoft/diskspd) testu. 

> [!Note]
> W tym scenariuszu może mieć wpływ na system i nie powinny być uruchamiane na komputerze produkcyjnym. W razie potrzeby uruchom ten scenariusz, w oknie obsługi dedykowanych Aby uniknąć problemów. Większe obciążenie jest spowodowane testu śledzenia lub testu porównawczego może niekorzystnie wpłynąć na wydajność maszyny Wirtualnej.
>

### <a name="azure-files-analysis"></a>Azure analizy plików 

W tym scenariuszu uruchamia przechwytywania licznika wydajności specjalne wraz z śledzenia ścieżek połączeń sieciowych. Przechwytywanie obejmuje wszystkie liczniki "Udziały SMB klienta". Poniżej przedstawiono niektóre kluczowe klienta udziału liczniki wydajności protokołu SMB należących do przechwytywania:

| **Typ**     | **Licznik udziałów klienta protokołu SMB** |
|--------------|-------------------------------|
| Operacje wejścia/wyjścia         | Dane żądania/s             |
|              | Liczba żądań odczytu/s             |
|              | Zapisu żądania/s            |
| Opóźnienie      | Średnia liczba żądań danych/s         |
|              | Średni czas odczytu s                 |
|              | Średni czas zapisu s                |
| Rozmiar operacji We/Wy      | Średni Bajty/danych żądania       |
|              | Średni Bajty odczytu               |
|              | Średni Bajty/zapisu              |
| Przepływność   | Dane bajty/s                |
|              | Odczytu dysku/s                |
|              | Bajty zapisane/s               |
| Długość kolejki | Średni Długość kolejki odczytu        |
|              | Średni Długość kolejki zapisu       |
|              | Średni Długość kolejki danych        |

### <a name="custom-slow-vm-analysis"></a>Niestandardowe powolne analizy maszyny Wirtualnej 

Podczas wykonywania niestandardowej powolne analizy maszyny Wirtualnej używasz wszystkie ślady (licznika wydajności, program xperf, sieci, storport) równolegle, zależności, jak wiele różnych dane śledzenia są wybrane. Po zakończeniu śledzenie narzędzie jest uruchamiane testu narzędzia diskspd, jeśli jest zaznaczone. 

> [!Note]
> W tym scenariuszu może mieć wpływ na system i nie powinny być uruchamiane na komputerze produkcyjnym. W razie potrzeby uruchom ten scenariusz, w oknie obsługi dedykowanych Aby uniknąć problemów. Większe obciążenie jest spowodowane testu śledzenia lub testu porównawczego może niekorzystnie wpłynąć na wydajność maszyny Wirtualnej.
>

## <a name="what-kind-of-information-is-collected-by-the-script"></a>Jakie informacje są zbierane przez skrypt?

Rejestruje informacje dotyczące maszyny Wirtualnej systemu Windows, dyski lub konfigurację pule magazynu, liczniki wydajności, a różne dane śledzenia są zbierane w zależności od scenariusza wydajności używanego:

|Zebrane dane                              |  |  | Scenariusze wydajności |  |  | |
|----------------------------------|----------------------------|------------------------------------|--------------------------|--------------------------------|----------------------|----------------------|
|                              | Zbierz dane o konfiguracji podstawowej | Przeprowadzenia testów porównawczych | Powolne analizy maszyny Wirtualnej | Powolne analizy maszyny Wirtualnej i testowania wydajności | Azure analizy plików | Niestandardowe powolne analizy maszyny Wirtualnej |
| Informacje z dzienników zdarzeń      | Tak                        | Tak                                | Tak                      | Tak                            | Tak                  | Tak                  |
| Informacje o systemie               | Tak                        | Tak                                | Tak                      | Tak                            | Tak                  | Tak                  |
| Mapa woluminu                       | Tak                        | Tak                                | Tak                      | Tak                            | Tak                  | Tak                  |
| Mapy dysku                         | Tak                        | Tak                                | Tak                      | Tak                            | Tak                  | Tak                  |
| Uruchomione zadania                    | Tak                        | Tak                                | Tak                      | Tak                            | Tak                  | Tak                  |
| Liczniki niezawodności magazynu     | Tak                        | Tak                                | Tak                      | Tak                            | Tak                  | Tak                  |
| Informacje o magazynu              | Tak                        | Tak                                | Tak                      | Tak                            | Tak                  | Tak                  |
| Dane wyjściowe fsutil                    | Tak                        | Tak                                | Tak                      | Tak                            | Tak                  | Tak                  |
| Informacje dotyczące sterownik filtru               | Tak                        | Tak                                | Tak                      | Tak                            | Tak                  | Tak                  |
| Dane wyjściowe polecenia netstat                   | Tak                        | Tak                                | Tak                      | Tak                            | Tak                  | Tak                  |
| Konfiguracja sieci            | Tak                        | Tak                                | Tak                      | Tak                            | Tak                  | Tak                  |
| Konfiguracja zapory           | Tak                        | Tak                                | Tak                      | Tak                            | Tak                  | Tak                  |
| Konfiguracja programu SQL Server         | Tak                        | Tak                                | Tak                      | Tak                            | Tak                  | Tak                  |
| Dane wydajności diagnostyki śledzenia * | Tak                        | Tak                                | Tak                      |                                | Tak                  | Tak                  |
| Licznik wydajności śledzenia **     |                            |                                    |                          |                                |                      | Tak                  |
| Licznik SMB śledzenia **             |                            |                                    |                          |                                | Tak                  |                      |
| SQL Server licznika śledzenia **      |                            |                                    |                          |                                |                      | Tak                  |
| Program XPerf śledzenia                      |                            |                                    |                          |                                |                      | Tak                  |
| StorPort śledzenia                   |                            |                                    |                          |                                |                      | Tak                  |
| Śledzenie sieci                    |                            |                                    |                          |                                | Tak                  | Tak                  |
| Śledzenia testu narzędzia Diskspd ***      |                            | Tak                                |                          | Tak                            |                      |                      |
|       |                            |                         |                                                   |                      |                      |

### <a name="performance-diagnostics-trace-"></a>Wydajności diagnostyki śledzenia (*)

Działa to aparat oparty na regułach w tle do zbierania danych i diagnozowanie problemów z wydajnością stałe. Obecnie obsługiwane są następujące reguły:

- Reguła HighCpuUsage: wykrywa wysokiej okresów użycia Procesora i przedstawia najwięcej użycia procesora CPU podczas tych okresów.
- Reguła HighDiskUsage: wykrywa okresów użycia dysku na dyskach fizycznych i przedstawia top dysku konsumentów użycia tych okresach.
- Reguła HighResolutionDiskMetric: przedstawia metryki opóźnienia IOPS, przepływności i we/wy na 50 milisekund dla każdego dysku fizycznego. Ułatwia szybkie identyfikowanie dysku ograniczania okresów.
- Reguła HighMemoryUsage: wykrywa okresów użycia pamięci wysokiej i przedstawia top pamięci konsumentów użycia tych okresach.

> [!NOTE] 
> Obecnie są obsługiwane wersje systemu Windows, które obejmują programu .NET Framework 3.5 lub nowszy.

### <a name="performance-counter-trace-"></a>Śledzenia licznik wydajności (*)

Zbiera następujące liczniki wydajności:

- \Process, \Processor, \Memory, \Thread, \PhysicalDisk, \LogicalDisk
- \Server\Pool stron, \Cache\Lazy zapisu opróżnienia/s, \Cache\Dirty niestronicowanej, błędy, błędy \Server\Pool stronicowanej
- Wybrane liczniki w obszarze \Network interfejsu, \IPv4\Datagrams, \IPv6\Datagrams, \TCPv4\Segments, \TCPv6\Segments, \Network karty, \WFPv4\Packets, \WFPv6\Packets, \UDPv4\Datagrams, \UDPv6\Datagrams, \TCPv4\Connection, \TCPv6\Connection, \ Sieci Winsock \Microsoft QoS Policy\Packets, \Per procesora interfejsu karty działań w sieci, podstawowego dostawcy

#### <a name="for-sql-server-instances"></a>Dla wystąpienia programu SQL Server
- Menedżer serwera: bufor \SQL, Statystyka puli \SQLServer:Resource, \SQLServer:SQL Statistics\
- \SQLServer:Locks, \SQLServer:General, statystyki
- Metody \SQLServer:Access

#### <a name="for-azure-files"></a>W przypadku plików platformy Azure
Udziały klient \SMB

### <a name="diskspd-benchmark-trace-"></a>Testu narzędzia Diskspd śledzenia (*)
Testy obciążenia We/Wy Diskspd [dysk systemu operacyjnego (zapis) oraz dyski puli (odczyt/zapis)]

## <a name="run-the-perfinsights-on-your-vm"></a>Uruchom PerfInsights na maszynie Wirtualnej

### <a name="what-do-i-have-to-know-before-i-run-the-script"></a>Co trzeba wiedzieć przed rozpoczęciem I Uruchom skrypt? 

**Wymagania dotyczące skryptu**

1.  Ten skrypt należy uruchomić na maszynie Wirtualnej, która ma problem z wydajnością. 

2.  Obsługiwane są następujące systemy operacyjne: Windows Server 2008 R2, 2012, 2012 R2, 2016; Windows 8.1 i Windows 10.

**Możliwe problemy podczas uruchamiania skryptu na maszynach wirtualnych w środowisku produkcyjnym:**

1.  Korzystając z wszystkie scenariusze Benchmarking lub scenariusza "Niestandardowe powolne analizy maszyny Wirtualnej", który jest skonfigurowany do używania narzędzia XPerf lub narzędzia DiskSpd, skrypt może niekorzystnie wpłynąć na wydajność maszyny Wirtualnej. Nie zaleca się uruchamiania tych scenariuszy w środowisku produkcyjnym bez nadzoru inżyniera CSS.

2.  Używając wszystkie scenariusze Benchmarking lub scenariusza "Niestandardowe powolne analizy maszyny Wirtualnej", który jest skonfigurowany do używania narzędzia DiskSpd, należy się upewnić, że nie inne operacje w tle zakłóca obciążenia We/Wy na dyskach przetestowane.

3.  Domyślnie skrypt używa dysku tymczasowego magazynu do zbierania danych. Jeśli śledzenie pozostaje włączona przez dłuższy czas, ilość zbieranych danych może być istotne. Może to zmniejszyć dostępność miejsca na dysku tymczasowym, w związku z tym wpływających na dowolnej aplikacji, która opiera się na tym dysku.

### <a name="how-do-i-run-perfinsights"></a>Jak uruchomić PerfInsights? 

PerfInsights można uruchomić na maszynie wirtualnej, instalując [rozszerzenia maszyny Wirtualnej systemu Azure wydajności diagnostyki](performance-diagnostics-vm-extension.md) lub uruchomić go jako skrypt autonomicznych. 

**Zainstaluj i uruchom PerfInsights z portalu Azure**

PerfInsights teraz mogą być uruchamiane przy użyciu rozszerzenia maszyny Wirtualnej o nazwie Azure wydajności diagnostyki rozszerzenia. Aby uzyskać więcej informacji, zobacz [zainstalować Azure Diagnostics wydajności rozszerzenia](performance-diagnostics-vm-extension.md#install-the-extension).  

**Uruchom skrypt PerfInsights w trybie autonomicznym**

Aby uruchomić skrypt PerfInsights, wykonaj następujące kroki:


1. Pobierz [PerfInsights.zip](http://aka.ms/perfinsightsdownload).

2. Odblokować plik PerfInsights.zip. Aby to zrobić, kliknij prawym przyciskiem myszy plik PerfInsights.zip, wybierz **właściwości**. W **ogólne** wybierz opcję **Odblokuj** , a następnie wybierz **OK**. Będzie to upewnij się, że skrypt jest uruchamiany bez dodatkowych zabezpieczeń monity.  

    ![Odblokować plik zip](media/how-to-use-perfInsights/unlock-file.png)

3.  Rozwiń skompresowany plik PerfInsights.zip do stacji tymczasowe (domyślnie zwykle dysku D). Skompresowany plik powinien zawierać następujące pliki i foldery:

    ![pliki z folderu zip](media/how-to-use-perfInsights/file-folder.png)

4.  Otwórz program Windows PowerShell jako administrator, a następnie uruchom skrypt PerfInsights.ps1.

    ```
    cd <the path of PerfInsights folder >
    Powershell.exe -ExecutionPolicy UnRestricted -NoProfile -File .\\PerfInsights.ps1
    ```

    Może być konieczne do wprowadź "y", jeśli zostanie wyświetlona prośba o potwierdzenie, że chcesz zmienić zasady wykonywania.

    W oknie dialogowym zastrzeżenie podano opcję, aby udostępnić informacje diagnostyczne Support firmy Microsoft. Również musi wyrazić zgodę na umowę licencyjną, aby kontynuować. Wybierz odpowiednie opcje, a następnie kliknij przycisk **Uruchom skrypt**.

    ![Pole zrzeczenie odpowiedzialności](media/how-to-use-perfInsights/disclaimer.png)

5.  Przedstawia liczbę przypadków, jeśli jest dostępne po uruchomieniu skryptu (jest to naszych statystyki). Następnie kliknij przycisk **OK**.
    
    ![Wprowadź identyfikator pomocy technicznej](media/how-to-use-perfInsights/enter-support-number.png)

6.  Wybierz dysk magazynu tymczasowego. Skrypt może wykryć automatycznie literę dysku. Ewentualnych problemów na tym etapie może być monit o wybranie dysku (dysk domyślny jest D). Wygenerowane dzienniki są przechowywane w tym miejscu w dzienniku\_folder kolekcji. Po wprowadzeniu lub zaakceptować literę dysku, kliknij przycisk **OK**.

    ![Wprowadź dysku](media/how-to-use-perfInsights/enter-drive.png)

7.  Wybierz scenariusza rozwiązywania problemów z podaną listą.

       ![Wybierz scenariusze pomocy technicznej](media/how-to-use-perfInsights/select-scenarios.png)

8.  Można również uruchomić PerfInsights bez interfejsu użytkownika.

    Następujące polecenie uruchamia "Analiza powoli maszyny Wirtualnej" Rozwiązywanie problemów z scenariusz bez monitowania interfejsu użytkownika lub przechwycenia danych dla 30 sekund. Monituje użytkownika o zgodę na te same zastrzeżenie i umowy licencyjnej, które są wymienione w kroku 4.

        powershell.exe -ExecutionPolicy UnRestricted -NoProfile -Command ".\\PerfInsights.ps1 -NoGui -Scenario vmslow -TracingDuration 30"

    Jeśli chcesz PerfInsights do uruchamiania w trybie dyskretnym, użyj **- AcceptDisclaimerAndShareDiagnostics** parametru. Na przykład następujące polecenie:

        powershell.exe -ExecutionPolicy UnRestricted -NoProfile -Command ".\\PerfInsights.ps1 -NoGui -Scenario vmslow -TracingDuration 30 -AcceptDisclaimerAndShareDiagnostics"

### <a name="how-do-i-troubleshoot-issues-while-running-the-script"></a>Jak rozwiązywać problemy podczas uruchamiania skryptu?

Jeśli skrypt zakończy się nieprawidłowo, możesz wyczyścić niespójna, uruchamiając skrypt razem z przełącznikiem oczyszczania, w następujący sposób:

    powershell.exe -ExecutionPolicy UnRestricted -NoProfile -Command ".\\PerfInsights.ps1 -Cleanup"

Ewentualnych problemów podczas automatycznego wykrywania tymczasowej stacji może być monit o wybranie dysku (dysk domyślny jest D).

![Wprowadź dysku](media/how-to-use-perfInsights/enter-drive.png)

Skrypt odinstalowuje narzędzia i usuwa folderów tymczasowych.

### <a name="troubleshooting-other-script-issues"></a>Rozwiązywanie problemów z innych problemów skryptu 

Ewentualnych problemów podczas wykonywania skryptu, naciśnij klawisze Ctrl + C, aby przerwać wykonywanie skryptu. Aby usunąć obiekty tymczasowe, zobacz sekcję "Oczyszczanie po zakończeniu nietypowe".

Jeśli nadal występują błędu skryptu nawet mimo kilku prób, zaleca się uruchomić skrypt w "tryb debugowania" za pomocą "-Debug" opcja parametru podczas uruchamiania.

Po awarii, skopiuj pełne dane wyjściowe z konsoli programu PowerShell i wysyłają je do agenta Microsoft Support, który jest dla instytucji ułatwić rozwiązanie problemu.

### <a name="how-do-i-run-the-script-in-custom-slow-vm-analysis-mode"></a>Jak uruchomić skrypt w trybie analizy wirtualna powolne niestandardowe?

Wybierając **niestandardowe powolne wirtualna analizy**, można włączyć kilka śladów równoległe (Użyj Shift, aby wielokrotnego wyboru):

![Wybierz scenariuszy](media/how-to-use-perfInsights/select-scenario.png)

Po wybraniu śledzenia licznika wydajności, śledzenia programu XPerf, śledzenia sieci lub scenariuszy Storport śledzenia, postępuj zgodnie z instrukcjami wyświetlanymi w oknach dialogowych, a następnie spróbuj do odtworzenia problemu spadek wydajności, po uruchomieniu śledzenia.

Poniższe okno dialogowe umożliwia uruchamianie śledzenia:

![Rozpocznij śledzenie](media/how-to-use-perfInsights/start-trace-message.png)

Aby zatrzymać śledzenia, masz o potwierdzenie polecenia w oknie dialogowym drugiego.

![Zatrzymaj śledzenie](media/how-to-use-perfInsights/stop-trace-message.png)
![Zatrzymaj śledzenie](media/how-to-use-perfInsights/ok-trace-message.png)

Po ukończeniu operacji lub dane śledzenia w D: jest utworzony nowy plik\\dziennika\_kolekcji (lub dysku tymczasowym) o nazwie **CollectedData\_RRRR MM-dd\_hh\_mm\_ss.zip.** Możesz wysłać ten plik do agenta programu obsługi dla analizy.

## <a name="review-the-diagnostics-report-created-by-perfinsights"></a>Przejrzyj raport diagnostyczny utworzone przez PerfInsights

W ramach **CollectedData\_RRRR MM-dd\_hh\_mm\_pliku ss.zip** generowany przez PerfInsights, można znaleźć raport HTML ze szczegółami ustalenia PerfInsights. Aby wyświetlić raport, rozwiń węzeł **CollectedData\_RRRR MM-dd\_hh\_mm\_ss.zip** pliku, a następnie otwórz **PerfInsights Report.html** plik.

Wybierz **ustalenia** kartę.

![Znajdź kartę](media/how-to-use-perfInsights/findingtab.png)
![wyników](media/how-to-use-perfInsights/findings.PNG)

**Uwagi**

-   Ustalenia skategoryzowane jako krytyczne znane problemy, które mogą powodować problemy z wydajnością.

-   Wyniki są sklasyfikowane jako ważne reprezentują-optymalne konfiguracje, które nie muszą powodować problemy z wydajnością.

-   Ustalenia skategoryzowane jako informacyjne są tylko szczegółowych instrukcji.

Sprawdź zalecenia i linki do wszystkich ustalenia krytyczne i ważne uzyskać bardziej szczegółowe informacje o wynikach i ich wpływu na wydajność lub najlepszych rozwiązań dotyczących konfiguracji zoptymalizowana pod kątem wydajności.

### <a name="storage-tab"></a>Karta magazynu

**Ustalenia** sekcja wyświetla różne wyniki i zalecenia dotyczące magazynu.

**DiskMap** i **VolumeMap** sekcjach opisano na podwójną perspektywy jak logicznej woluminów i dysków fizycznych są ze sobą powiązane.

W perspektywie dysk fizyczny (DiskMap) w tabeli przedstawiono wszystkie woluminy logiczne, które są uruchomione na dysku. W poniższym przykładzie PhysicalDrive2 uruchamia dwa logiczne woluminy tworzone na wielu partycjach (J i H):

![Karta dane](media/how-to-use-perfInsights/disktab.png)

W perspektywie woluminu (*VolumeMap*), w tabelach przedstawiono wszystkie dyski fizyczne w obszarze każdego woluminu logicznego. Zwróć uwagę, że dla dysków dynamicznych/RAID, mogą zostać uruchomione logicznej woluminu na wiele dysków fizycznych. W poniższym przykładzie *C:\\instalacji* jest punkt_instalacji, skonfigurowany jako *SpannedDisk* na PhysicalDisks \#2 i \#3:

![Karta woluminu](media/how-to-use-perfInsights/volumetab.png)

### <a name="sql-tab"></a>Karta SQL

Jeśli element docelowy maszyny Wirtualnej obsługuje wszystkie wystąpienia programu SQL Server, zobacz dodatkowe karty w raporcie o nazwie **SQL**:

![Karta SQL](media/how-to-use-perfInsights/sqltab.png)

Ta sekcja zawiera kartę "Ustalenia" i sub dodatkowe karty dla wszystkich wystąpień programu SQL Server obsługiwanych na maszynie Wirtualnej.

Na karcie "Ustalenia" zawiera listę wszystkich SQL dotyczące problemów z wydajnością znaleziono wraz z zaleceniami.

W poniższym przykładzie *PhysicalDrive0* (uruchomionego dysk C) jest wyświetlany, ponieważ zarówno *modeldev* i *modellog* pliki znajdują się na dysku C, a ich różne typy (takich jak plik danych i dziennika transakcji, odpowiednio):

![LogInfo](media/how-to-use-perfInsights/loginfo.png)

Karty danego wystąpienia programu SQL Server zawierają ogólne sekcja, która zawiera podstawowe informacje o wybranym wystąpieniu i dodatkowe sekcje zaawansowane informacje, w tym ustawienia, konfiguracje i opcje użytkownika.

### <a name="diagnostic-tab"></a>Karta diagnostyki
Karta diagnostyki zawiera informacje o najwyższym procesora CPU, uruchom konsumentów dysków i pamięci w polu Czas trwania PerfInsights. Można również znaleźć inne przydatne informacje, takie jak krytycznych poprawek czy system może brakować, listę zadań i ważnych zdarzeń systemu. 

## <a name="references-to-the-external-tools-used"></a>Odwołania do zewnętrznego narzędzia używane

### <a name="diskspd"></a>Narzędzia Diskspd

DISKSPD jest magazynu obciążenia generator wydajności testu narzędziem zespołów engineering systemu Windows i Windows Server i infrastrukturą serwera chmury. Aby uzyskać więcej informacji, zobacz [Diskspd](https://github.com/Microsoft/diskspd).

### <a name="xperf"></a>Program XPerf

Program XPerf jest narzędziem wiersza polecenia do przechwytywania danych śledzenia w zestawie narzędzi wydajności systemu Windows.

Aby uzyskać więcej informacji, zobacz [Toolkit wydajności systemu Windows — program Xperf](https://blogs.msdn.microsoft.com/ntdebugging/2008/04/03/windows-performance-toolkit-xperf/).

## <a name="next-steps"></a>Następne kroki

### <a name="upload-diagnostics-logs-and-reports-to-microsoft-support-for-further-review"></a>Przekazywanie dzienników diagnostyki i raporty do firmy Microsoft Support do dalszej analizy

Podczas pracy z personelu Support firmy Microsoft, mogą być wymagane do przesyłania danych wyjściowych, który jest generowany przez PerfInsights, aby ułatwić proces rozwiązywania problemów.

Agent pomocy technicznej utworzy DTM obszaru roboczego dla Ciebie, a otrzymasz wiadomość e-mail zawierającą łącze do [DTM portal (https://filetransfer.support.microsoft.com/EFTClient/Account/Login.htm) oraz identyfikator unikatowy użytkownika i hasła.

Ta wiadomość zostanie wysłana z **usługi diagnostyki automatycznego CTS** (ctsadiag@microsoft.com).

![Przykładowy komunikat](media/how-to-use-perfInsights/supportemail.png)

Aby dodatkowo zwiększyć bezpieczeństwo trzeba będzie zmienić hasło przy pierwszym użyciu.

Po zalogowaniu się do DTM znajdują się okno dialogowe, aby przekazać **CollectedData\_RRRR MM-dd\_hh\_mm\_ss.zip** pliku, który został zebrany przez PerfInsights.
