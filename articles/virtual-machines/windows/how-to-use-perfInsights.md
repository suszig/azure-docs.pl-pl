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
ms.openlocfilehash: f15875610e2035c6f4c10c36e19c02f3e045b3ea
ms.sourcegitcommit: 3fca41d1c978d4b9165666bb2a9a1fe2a13aabb6
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/15/2017
---
# <a name="how-to-use-perfinsights"></a>Jak używać PerfInsights 

[PerfInsights](http://aka.ms/perfinsightsdownload) jest zautomatyzowanego skryptu, który zbiera przydatnych informacji diagnostycznych. On również działa obciążenia We/Wy obciążenia i zapewnia raport analizy, aby ułatwić rozwiązywanie problemów z wydajnością maszyny wirtualnej systemu Windows na platformie Azure. To może działać na maszynach wirtualnych jako skrypt autonomicznej lub bezpośrednio z portalu, instalując [rozszerzenia maszyny Wirtualnej systemu Azure wydajności diagnostyki](performance-diagnostics-vm-extension.md).

Jeśli występują problemy z wydajnością maszyn wirtualnych przed skontaktowaniem się z pomocy technicznej, uruchom ten skrypt.

## <a name="supported-troubleshooting-scenarios"></a>Obsługiwane scenariusze rozwiązywania problemów

PerfInsights można zbierać i analizować kilka rodzajów informacji. W poniższych częściach omówiono typowe scenariusze.

### <a name="collect-basic-configuration"></a>Zbierz dane o konfiguracji podstawowej 

W tym scenariuszu zbiera konfiguracji dysku i inne ważne informacje, w tym:

-   Dzienniki zdarzeń

-   Stan sieci dla wszystkich połączeń przychodzących i wychodzących

-   Ustawienia konfiguracji sieci i zapory

-   Listy zadań dla wszystkich aplikacji, które są aktualnie uruchomione w systemie

-   Informacje o pliku utworzonego przez msinfo32 dla maszyny wirtualnej

-   Ustawienia konfiguracji bazy danych programu Microsoft SQL Server (Jeśli maszyna wirtualna została zidentyfikowana jako serwer, na którym działa program SQL Server)

-   Liczniki niezawodności magazynu

-   Ważne poprawki systemu Windows

-   Sterowniki filtrów zainstalowany

To jest pasywny zbiór informacje, które nie powinny mieć wpływ na system. 

>[!Note]
>Ten scenariusz jest automatycznie uwzględnione w każdym z poniższych scenariuszy.

### <a name="benchmarking"></a>Przeprowadzenia testów porównawczych

W tym scenariuszu uruchamia [Diskspd](https://github.com/Microsoft/diskspd) testu (IOPS i MB/s) dla wszystkich dysków dołączonych do maszyny Wirtualnej. 

> [!Note]
> W tym scenariuszu może mieć wpływ na system i nie powinny być uruchamiane na komputerze produkcyjnym. W razie potrzeby uruchom ten scenariusz, w oknie obsługi dedykowanych Aby uniknąć problemów. Większe obciążenie jest spowodowane testu śledzenia lub testu porównawczego może niekorzystnie wpłynąć na wydajność maszyny Wirtualnej.
>

### <a name="slow-vm-analysis"></a>Powolne analizy maszyny Wirtualnej 

W tym scenariuszu uruchamia [licznika wydajności](https://msdn.microsoft.com/library/windows/desktop/aa373083(v=vs.85).aspx) śledzenia przy użyciu liczniki, które są określone w pliku Generalcounters.txt. Jeśli maszyna wirtualna została zidentyfikowana jako serwer, na którym działa program SQL Server, uruchamia śledzenia licznika wydajności. Robi to przy użyciu liczniki, które znajdują się w pliku Sqlcounters.txt, a także zawiera dane dotyczące wydajności diagnostyki.

### <a name="slow-vm-analysis-and-benchmarking"></a>Powolne analizy maszyny Wirtualnej i testowania wydajności

W tym scenariuszu uruchamia [licznika wydajności](https://msdn.microsoft.com/library/windows/desktop/aa373083(v=vs.85).aspx) śledzenia, w którym następuje [Diskspd](https://github.com/Microsoft/diskspd) testu. 

> [!Note]
> W tym scenariuszu może mieć wpływ na system i nie powinny być uruchamiane na komputerze produkcyjnym. W razie potrzeby uruchom ten scenariusz, w oknie obsługi dedykowanych Aby uniknąć problemów. Większe obciążenie jest spowodowane testu śledzenia lub testu porównawczego może niekorzystnie wpłynąć na wydajność maszyny Wirtualnej.
>

### <a name="azure-files-analysis"></a>Azure analizy plików 

W tym scenariuszu uruchamia przechwytywania licznika wydajności specjalne wraz z śledzenia ścieżek połączeń sieciowych. Przechwytywania zawiera wszystkie bloku komunikatów serwera (SMB) udziałów liczników klienta. Poniżej przedstawiono niektóre kluczowe klienta udziału liczniki wydajności protokołu SMB należących do przechwytywania:

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

Po uruchomieniu niestandardowych powolne analizy maszyny Wirtualnej, należy wybrać dane śledzenia do uruchomienia równoległego. Można uruchomić je wszystkie (licznika wydajności, program Xperf, sieci i StorPort) Jeśli chcesz. Po zakończeniu śledzenie narzędzie jest uruchamiane testu narzędzia Diskspd, jeśli jest zaznaczone. 

> [!Note]
> W tym scenariuszu może mieć wpływ na system i nie powinny być uruchamiane na komputerze produkcyjnym. W razie potrzeby uruchom ten scenariusz, w oknie obsługi dedykowanych Aby uniknąć problemów. Większe obciążenie jest spowodowane testu śledzenia lub testu porównawczego może niekorzystnie wpłynąć na wydajność maszyny Wirtualnej.
>

## <a name="what-kind-of-information-is-collected-by-the-script"></a>Jakie informacje są zbierane przez skrypt?

Rejestruje informacje dotyczące maszyny Wirtualnej systemu Windows, dyski lub konfigurację pule magazynu, liczniki wydajności, a różne dane śledzenia są zbierane. To zależy od używany w scenariuszu wydajności. Poniższa tabela zawiera szczegółowe informacje:

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
| Śledzenia licznika wydajności **     |                            |                                    |                          |                                |                      | Tak                  |
| SMB licznika śledzenia **             |                            |                                    |                          |                                | Tak                  |                      |
| SQL Server licznika śledzenia **      |                            |                                    |                          |                                |                      | Tak                  |
| Program XPerf śledzenia                      |                            |                                    |                          |                                |                      | Tak                  |
| StorPort śledzenia                   |                            |                                    |                          |                                |                      | Tak                  |
| Śledzenie sieci                    |                            |                                    |                          |                                | Tak                  | Tak                  |
| Śledzenia testu narzędzia Diskspd ***      |                            | Tak                                |                          | Tak                            |                      |                      |
|       |                            |                         |                                                   |                      |                      |

### <a name="performance-diagnostics-trace-"></a>Diagnostyka wydajności śledzenia (*)

Działa to aparat oparty na regułach w tle do zbierania danych i diagnozowanie problemów z wydajnością stałe. Obecnie obsługiwane są następujące reguły:

- Reguła HighCpuUsage: wykrywa wysokiej okresów użycia Procesora i przedstawia najwięcej użycia procesora CPU podczas tych okresów.
- Reguła HighDiskUsage: wykrywa okresów użycia dysku na dyskach fizycznych i przedstawia top dysku konsumentów użycia tych okresach.
- Reguła HighResolutionDiskMetric: przedstawia metryki opóźnienia IOPS, przepustowości i operacji We/Wy na 50 milisekund dla każdego dysku fizycznego. Ułatwia szybkie identyfikowanie dysku ograniczania okresów.
- Reguła HighMemoryUsage: wykrywa okresów użycia pamięci wysokiej i przedstawia top pamięci konsumentów użycia tych okresach.

> [!NOTE] 
> Obecnie są obsługiwane wersje systemu Windows, które obejmują programu .NET Framework 3.5 lub nowszy.

### <a name="performance-counter-trace-"></a>Śledzenia licznik wydajności (*)

Zbiera następujące liczniki wydajności:

- \Process, \Processor \Memory, \Thread, \PhysicalDisk i \LogicalDisk
- \Server\Pool stron, \Cache\Lazy zapisu opróżnienia/s, \Cache\Dirty niestronicowanej, awarie i błędy \Server\Pool stronicowanej
- Wybrane liczniki w obszarze \Network interfejsu, \IPv4\Datagrams, \IPv6\Datagrams, \TCPv4\Segments, \TCPv6\Segments, \Network karty, \WFPv4\Packets, \WFPv6\Packets, \UDPv4\Datagrams, \UDPv6\Datagrams, \TCPv4\Connection, \TCPv6\Connection, \ QoS Policy\Packets, \Per procesora sieci interfejsu karty działania i Winsock \Microsoft podstawowego dostawcy usługi sieciowej

#### <a name="for-sql-server-instances"></a>Dla wystąpienia programu SQL Server
- Menedżera serwera: buforów \SQL Statystyka puli \SQLServer:Resource i \SQLServer:SQL Statistics\
- \SQLServer:Locks, \SQLServer:General, statystyki
- Metody \SQLServer:Access

#### <a name="for-azure-files"></a>W przypadku plików platformy Azure
Udziały klient \SMB

### <a name="diskspd-benchmark-trace-"></a>Narzędzia Diskspd testu porównawczego śledzenia (*)
Testy obciążenia We/Wy narzędzia Diskspd (dysku systemu operacyjnego [zapisu] i dysków puli [odczytu/zapisu])

## <a name="run-the-perfinsights-script-on-your-vm"></a>Uruchom skrypt PerfInsights na maszynie Wirtualnej

### <a name="what-do-i-have-to-know-before-i-run-the-script"></a>Co trzeba wiedzieć przed rozpoczęciem I Uruchom skrypt? 

#### <a name="script-requirements"></a>Wymagania dotyczące skryptu

-  Ten skrypt należy uruchomić na maszynie Wirtualnej, która ma problem z wydajnością. 

-  Obsługiwane są następujące systemy operacyjne: Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2 i Windows Server 2016; Windows 8.1 i Windows 10.

#### <a name="possible-problems-when-you-run-the-script-on-production-vms"></a>Możliwe problemy podczas uruchamiania skryptu na maszynach wirtualnych w środowisku produkcyjnym

-  Scenariusze najlepszymi lub scenariusza "Niestandardowe powolne analizy maszyny Wirtualnej", który jest skonfigurowany do używania narzędzia Xperf lub narzędzia Diskspd skrypt może niekorzystnie wpłynąć na wydajność maszyny Wirtualnej. Te scenariusze nie należy uruchamiać w środowisku produkcyjnym.

-  Scenariusze najlepszymi lub scenariusza "Niestandardowe powolne analizy maszyny Wirtualnej", który jest skonfigurowany do używania narzędzia Diskspd upewnij się, że nie inne operacje w tle zakłóca obciążenia We/Wy.

-  Domyślnie skrypt używa dysku tymczasowego magazynu do zbierania danych. Jeśli śledzenie pozostaje włączona przez dłuższy czas, ilość zbieranych danych może być istotne. Można zmniejszyć dostępność miejsca na dysku tymczasowym, a w związku z tym może mieć wpływ na dowolnej aplikacji, która opiera się na tym dysku.

### <a name="how-do-i-run-perfinsights"></a>Jak uruchomić PerfInsights? 

PerfInsights można uruchomić na maszynie wirtualnej, instalując [rozszerzenia maszyny Wirtualnej systemu Azure wydajności diagnostyki](performance-diagnostics-vm-extension.md). Można również uruchomić jako skrypt autonomicznych. 

**Zainstaluj i uruchom PerfInsights z portalu Azure**

Aby uzyskać więcej informacji na temat tej opcji, zobacz [zainstalować rozszerzenia maszyny Wirtualnej diagnostyki wydajności Azure](performance-diagnostics-vm-extension.md#install-the-extension).  

**Uruchom skrypt PerfInsights w trybie autonomicznym**

Aby uruchomić skrypt PerfInsights, wykonaj następujące kroki:


1. Pobierz [PerfInsights.zip](http://aka.ms/perfinsightsdownload).

2. Odblokować plik PerfInsights.zip. Aby to zrobić, kliknij prawym przyciskiem myszy plik PerfInsights.zip, a następnie wybierz **właściwości**. W **ogólne** wybierz opcję **Odblokuj**, a następnie wybierz **OK**. Dzięki temu, że skrypt jest uruchamiany bez monituje dodatkowych zabezpieczeń.  

    ![Zrzut ekranu PerfInsights właściwości, z wyróżnionym Odblokuj](media/how-to-use-perfInsights/unlock-file.png)

3.  Rozwiń skompresowany plik PerfInsights.zip do stacji tymczasowe (domyślnie jest to zazwyczaj na dysku D). Skompresowany plik powinien zawierać następujące pliki i foldery:

    ![Zrzut ekranu przedstawiający pliki z folderu zip](media/how-to-use-perfInsights/file-folder.png)

4.  Otwórz program Windows PowerShell jako administrator, a następnie uruchom skrypt PerfInsights.ps1.

    ```
    cd <the path of PerfInsights folder >
    Powershell.exe -ExecutionPolicy UnRestricted -NoProfile -File .\\PerfInsights.ps1
    ```

    Może być konieczne wprowadzenie "y", aby potwierdzić, że chcesz zmienić zasady wykonywania.

    W **zauważyć i zgody** okno dialogowe, użytkownik może udostępniać informacje diagnostyczne Support firmy Microsoft. Również musi wyrazić zgodę na umowę licencyjną, aby kontynuować. Wybierz odpowiednie opcje, a następnie wybierz **Uruchom skrypt**.

    ![Zrzut ekranu ogłoszenia i zgody okno dialogowe](media/how-to-use-perfInsights/disclaimer.png)

5.  Przedstawia liczbę przypadków, jeśli jest dostępny, po uruchomieniu skryptu. Następnie wybierz **OK**.
    
    ![Zrzut ekranu przedstawiający okno dialogowe Identyfikatora pomocy technicznej](media/how-to-use-perfInsights/enter-support-number.png)

6.  Wybierz dysk magazynu tymczasowego. Skrypt może wykryć automatycznie literę dysku. Ewentualnych problemów na tym etapie może być monit o wybranie dysku (dysk domyślny jest D). Wygenerowane dzienniki są przechowywane w tym miejscu, w dzienniku\_folder kolekcji. Po wprowadzeniu lub zaakceptować literę dysku, wybierz **OK**.

    ![Zrzut ekranu przedstawiający okno dialogowe tymczasowej stacji](media/how-to-use-perfInsights/enter-drive.png)

7.  Wybierz scenariusza rozwiązywania problemów z podaną listą.

       ![Zrzut ekranu przedstawiający rozwiązywanie problemów z listy scenariuszy](media/how-to-use-perfInsights/select-scenarios.png)

Można również uruchomić PerfInsights bez interfejsu użytkownika. Następujące polecenie uruchamia "Analiza powoli maszyny Wirtualnej" Rozwiązywanie problemów z scenariusz bez interfejsu użytkownika. Monituje użytkownika o zgodę na te same zastrzeżenie i umowy licencyjnej, które są wymienione w kroku 4.

        powershell.exe -ExecutionPolicy UnRestricted -NoProfile -Command ".\\PerfInsights.ps1 -NoGui -Scenario vmslow -TracingDuration 30"

Jeśli chcesz PerfInsights do uruchamiania w trybie dyskretnym, użyj **- AcceptDisclaimerAndShareDiagnostics** parametru. Na przykład następujące polecenie:

        powershell.exe -ExecutionPolicy UnRestricted -NoProfile -Command ".\\PerfInsights.ps1 -NoGui -Scenario vmslow -TracingDuration 30 -AcceptDisclaimerAndShareDiagnostics"

### <a name="how-do-i-troubleshoot-issues-while-running-the-script"></a>Jak rozwiązywać problemy podczas uruchamiania skryptu?

Jeśli skrypt zakończy się nieprawidłowo, można powrócić do stanu spójności, uruchamiając skrypt wraz z przełącznikiem oczyszczania w następujący sposób:

    powershell.exe -ExecutionPolicy UnRestricted -NoProfile -Command ".\\PerfInsights.ps1 -Cleanup"

Ewentualnych problemów podczas automatycznego wykrywania tymczasowej stacji może być monit o wybranie dysku (dysk domyślny jest D).

![Zrzut ekranu przedstawiający okno dialogowe tymczasowej stacji](media/how-to-use-perfInsights/enter-drive.png)

Skrypt odinstalowuje narzędzia i usuwa folderów tymczasowych.

### <a name="troubleshoot-other-script-issues"></a>Rozwiązywanie problemów z innych problemów skryptu 

Ewentualnych problemów podczas wykonywania skryptu, naciśnij klawisze Ctrl + C, aby przerwać skryptu. Jeśli nadal występują błędu skryptu nawet mimo kilku prób, uruchom skrypt w trybie debugowania za pomocą "-Debug" opcja parametru podczas uruchamiania.

Po awarii, skopiuj pełne dane wyjściowe z konsoli programu PowerShell i wysyłają je do agenta Microsoft Support, który jest dla instytucji ułatwić rozwiązanie problemu.

### <a name="how-do-i-run-the-script-in-custom-slow-vm-analysis-mode"></a>Jak uruchomić skrypt w trybie "Niestandardowe powolne analizy maszyny Wirtualnej"?

Wybierając **niestandardowe powolne wirtualna analizy**, można włączyć śledzenie kilku równoległe (Aby zaznaczyć wiele śladów, użyj klawisza Shift):

![Zrzut ekranu przedstawiający listę scenariuszy](media/how-to-use-perfInsights/select-scenario.png)

Po wybraniu śledzenia licznika wydajności śledzenia programu Xperf, śledzenia sieci lub scenariuszy Storport śledzenia, postępuj zgodnie z instrukcjami w kolejnych oknach dialogowych. Spróbuj do odtworzenia problemu spadek wydajności, po uruchomieniu śledzenia.

Można uruchomić śledzenia za pomocą okna dialogowego następujące:

![Zrzut ekranu z Start licznika śledzenia wydajności — okno dialogowe](media/how-to-use-perfInsights/start-trace-message.png)

Aby zatrzymać śledzenia, masz o potwierdzenie polecenia w oknie dialogowym drugiego.

![Zrzut ekranu z zatrzymywanie licznika śledzenia wydajności — okno dialogowe](media/how-to-use-perfInsights/stop-trace-message.png)
![zrzut ekranu z zatrzymanie wszystkich śledzi — okno dialogowe](media/how-to-use-perfInsights/ok-trace-message.png)

Po ukończeniu operacji lub śladów nowy plik zostanie wyświetlony w D:\\dziennika\_kolekcji (lub tymczasowej stacji). Nazwa pliku jest **CollectedData\_RRRR MM-dd\_hh\_mm\_ss.zip.** Możesz wysłać ten plik do agenta programu obsługi dla analizy.

## <a name="review-the-diagnostics-report"></a>Przejrzyj raport diagnostyczny

W ramach **CollectedData\_RRRR MM-dd\_hh\_mm\_ss.zip** plików, można znaleźć raport HTML ze szczegółami ustalenia PerfInsights. Aby wyświetlić raport, rozwiń węzeł **CollectedData\_RRRR MM-dd\_hh\_mm\_ss.zip** pliku, a następnie otwórz **PerfInsights Report.html** plik.

Wybierz **ustalenia** kartę.

![Zrzut ekranu przedstawiający raport PerfInsights](media/how-to-use-perfInsights/findingtab.png)
![zrzut ekranu przedstawiający raport PerfInsights](media/how-to-use-perfInsights/findings.PNG)

> [!NOTE] 
> Ustalenia skategoryzowane jako krytyczne znane problemy, które mogą powodować problemy z wydajnością. Wyniki są sklasyfikowane jako ważne reprezentują-optymalne konfiguracje, które nie muszą powodować problemy z wydajnością. Ustalenia skategoryzowane jako informacyjne są tylko szczegółowych instrukcji.

Przejrzyj zalecenia i linki do wszystkich krytycznych i ważne dane. Informacje o ich wpływu na wydajność i dotyczących najlepszych rozwiązań dotyczących konfiguracji zoptymalizowana pod kątem wydajności.

### <a name="storage-tab"></a>Karta magazynu

**Ustalenia** sekcja wyświetla różne wyniki i zalecenia dotyczące magazynu.

**Mapy dysku** i **mapy woluminu** sekcjach opisano sposób logicznej woluminów i fizyczne dyski są powiązane ze sobą.

W perspektywie dysk fizyczny (mapy dysku) w tabeli przedstawiono wszystkie woluminy logiczne, które są uruchomione na dysku. W poniższym przykładzie **PhysicalDrive2** uruchamia dwa logiczne woluminy tworzone na wielu partycjach (J i H):

![Zrzut ekranu przedstawiający kartę dysku](media/how-to-use-perfInsights/disktab.png)

W perspektywie woluminu (wolumin Map) w tabelach przedstawiono wszystkie dyski fizyczne w obszarze każdego woluminu logicznego. Zwróć uwagę, że dla dysków dynamicznych/RAID, wolumin logiczny może działać na wielu dyskach fizycznych. W poniższym przykładzie *C:\\instalacji* punkt instalacji jest skonfigurowany jako *SpannedDisk* na dyskach fizycznych, 2 i 3:

![Zrzut ekranu przedstawiający kartę woluminu](media/how-to-use-perfInsights/volumetab.png)

### <a name="sql-tab"></a>Karta SQL

Jeśli element docelowy maszyny Wirtualnej obsługuje wszystkie wystąpienia programu SQL Server, zobacz dodatkowe karty w raporcie o nazwie **SQL**:

![Zrzut ekranu SQL kartę](media/how-to-use-perfInsights/sqltab.png)

Ta sekcja zawiera **ustalenia** kartę i dodatkowe karty dla wszystkich wystąpień programu SQL Server obsługiwanych na maszynie Wirtualnej.

**Ustalenia** karta zawiera listę wszystkich SQL dotyczące problemów z wydajnością znaleziono wraz z zaleceniami.

W poniższym przykładzie **PhysicalDrive0** (uruchomionego dysk C) jest wyświetlany. Jest to spowodowane zarówno **modeldev** i **modellog** pliki znajdują się na dysku C i są różnych typów (takie jak dziennik transakcji i plików danych, odpowiednio).

![Zrzut ekranu przedstawiający informacje dziennika](media/how-to-use-perfInsights/loginfo.png)

Karty dla określonego wystąpienia programu SQL Server zawierają ogólne sekcja, która zawiera podstawowe informacje o wybranym wystąpieniu. Karty również zawierać dodatkowe sekcje zaawansowane informacje, w tym ustawienia, konfiguracje i opcje użytkownika.

### <a name="diagnostic-tab"></a>Karta diagnostyki
**Diagnostycznych** karta zawiera informacje o najwięcej Procesora, dysku i pamięci na komputerze w czasie trwania działanie PerfInsights. Można również znaleźć informacje o poprawkach krytyczne czy system może być brak, listę zadań i zdarzenia systemowe. 

## <a name="references-to-the-external-tools-used"></a>Odwołania do zewnętrznego narzędzia używane

### <a name="diskspd"></a>Narzędzia Diskspd

Diskspd jest magazynu obciążenia generator i wydajności testu narzędzie firmy Microsoft. Aby uzyskać więcej informacji, zobacz [Diskspd](https://github.com/Microsoft/diskspd).

### <a name="xperf"></a>Program XPerf

Program XPerf jest narzędziem wiersza polecenia do przechwytywania danych śledzenia z zestawu narzędzi wydajności systemu Windows. Aby uzyskać więcej informacji, zobacz [Toolkit wydajności systemu Windows — program Xperf](https://blogs.msdn.microsoft.com/ntdebugging/2008/04/03/windows-performance-toolkit-xperf/).

## <a name="next-steps"></a>Następne kroki

Możesz przekazać dzienników diagnostyki i raporty do firmy Microsoft Support do dalszej analizy. Obsługa może zażądać, aby przesyłać dane wyjściowe, który jest generowany przez PerfInsights, aby ułatwić proces rozwiązywania problemów.

Poniższy zrzut ekranu przedstawia podobne do może zostać wyświetlony komunikat:

![Zrzut ekranu przedstawiający przykładowy komunikat z Support firmy Microsoft](media/how-to-use-perfInsights/supportemail.png)

Postępuj zgodnie z instrukcjami w komunikacie dostępu do obszaru roboczego transferu plików. Aby dodatkowo zwiększyć bezpieczeństwo należy zmienić hasło przy pierwszym użyciu.

Po zalogowaniu, znajdują się okno dialogowe, aby przekazać **CollectedData\_RRRR MM-dd\_hh\_mm\_ss.zip** pliku, który został zebrany przez PerfInsights.
