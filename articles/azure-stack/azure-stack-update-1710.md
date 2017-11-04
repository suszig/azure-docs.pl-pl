---
title: Aktualizacja Azure stosu 1710 (kompilacja 20171020.1) | Dokumentacja firmy Microsoft
description: "Więcej informacji na temat nowości w aktualizacji 1710 stosu Azure zintegrowanych systemów, znane problemy i pobierania aktualizacji."
services: azure-stack
documentationcenter: 
author: twooley
manager: byronr
editor: 
ms.assetid: 135314fd-7add-4c8c-b02a-b03de93ee196
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/02/2017
ms.author: twooley
ms.openlocfilehash: a66c7dddab1a2246dd0a9fa3c7533063140d2f01
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/03/2017
---
# <a name="azure-stack-1710-update-build-201710201"></a>Aktualizacja Azure stosu 1710 (kompilacja 20171020.1)

*Dotyczy: Azure stosu zintegrowane systemy*

W tym artykule opisano ulepszenia i poprawki w tym pakiecie aktualizacji znane problemy dotyczące tej wersji i pobierania aktualizacji. Znane problemy są podzielone na znane problemy bezpośrednio związane z procesem aktualizacji i znane problemy związane z kompilacją (po instalacji).

> [!IMPORTANT]
> Ten pakiet aktualizacji jest tylko dla stosu Azure zintegrowanych systemów. Nie dotyczą tego pakietu aktualizacji Azure stosu Development Kit.

## <a name="improvements-and-fixes"></a>Ulepszeń i poprawek

Ta aktualizacja obejmuje następujące ulepszenia jakości i poprawki dla stosu Azure.
 
### <a name="windows-server-2016-improvements-and-fixes"></a>Ulepszenia w systemie Windows Server 2016 i poprawki

- Aktualizacje systemu Windows Server 2016: 10 października 2017 — KB4041691 (kompilacja 14393.1770 systemu operacyjnego. Zobacz [https://support.microsoft.com/help/4041691](https://support.microsoft.com/help/4041691) Aby uzyskać więcej informacji.

### <a name="additional-quality-improvements-and-fixes"></a>Dodatkową jakością ulepszeń i poprawek

- Dodane polecenia cmdlet programu PowerShell uprzywilejowanych punktu końcowego, ułatwiające rozwiązywanie problemów z serwerem i zaktualizować protokołu NTP (Network Time).
- Dodano obsługę aktualizacja modułów punktu końcowego tylko tyle administracyjnej (JEA) uprzywilejowany punktu końcowego i polecenia cmdlet listy dozwolonych adresów IP. 
- Błędy stałym język lokalny uprzywilejowanych punktu końcowego.
- Dodano możliwość obracania poświadczeń bramy.
- Usunięte CBLocalAdmin konta administratora lokalnego. 
- Stałe zawartość szablonu alertów pulsu aby upewnij się, że Puls alerty pracy prawidłowo po aktualizacji.
- Rozwiązany przez dostawcę zasobów sieci szkieletowej postępowania w przypadku przekroczenia limitu czasu podczas operacji FRU. 
- Dodano możliwość deweloperom chmury profile usługi Azure Resource Manager interfejsu API Azure stosu.
- Wyłączyć usługę Windows Update na maszynie wirtualnej wdrożenia (Menedżer DVM). 
- Usunięte zasilania węzła lub wyłącza akcje w interfejsie użytkownika.
- Różne inne wydajności i stabilności poprawki. 
 
## <a name="known-issues-with-the-update-process"></a>Znane problemy związane z procesem aktualizacji

Ta sekcja zawiera znane problemy, które mogą wystąpić podczas instalacji aktualizacji 1710.

> [!IMPORTANT]
> W przypadku niepowodzenia aktualizacji później podczas Wznów aktualizację, należy użyć `Resume-AzureStackUpdate` polecenia cmdlet z uprzywilejowanego punktu końcowego. Wznawia aktualizacji za pomocą portalu administratora. (Jest to znany problem w tej wersji). Aby uzyskać więcej informacji, zobacz [monitorowania aktualizacji w stosie Azure przy użyciu punktu końcowego uprzywilejowanych](azure-stack-monitor-update.md).

| Objaw  | Przyczyna  | Rozwiązanie |
|---------|---------|---------|
|Podczas wykonywania aktualizacji podczas wykonywania kroku "Magazynu hostów ponownie uruchomić węzeł magazynu" planu działania aktualizacji może wystąpić błąd podobny do następującego błędu.<br><br>**{"name": "Uruchom ponownie magazynu hostów", "opis": "Uruchom ponownie magazynu hostów.", "errorMessage": "typu"Restart"roli"BareMetal"uruchamiany komputera wyjątek: \n\nThe HostName-05 zostanie pominięta. Nie można pobrać jej LastBootUpTime za pomocą usługi WMI z powodu następującego błędu: serwer RPC jest niedostępny. (Wyjątek od HRESULT: 0x800706BA). \nat ponownego uruchomienia komputera hosta** | Przyczyną tego problemu jest potencjalnych uszkodzony sterownik istnieje w niektórych konfiguracjach. | 1. Zaloguj się do interfejsu sieci web uzyskiwania informacji na temat kontrolera zarządzania płytą główną i uruchom ponownie hosta, który określono w komunikacie o błędzie.<br><br>2. Wznów aktualizacji przy użyciu uprzywilejowanych punktu końcowego. |
| Podczas wykonywania aktualizacji proces aktualizacji jest dostępna dla zatrzymania i nie wprowadzać postępu po wykonaniu kroku "krok: uruchamiania kroku 2.4 - instalacji aktualizacji" planu działania aktualizacji.<br><br>W tym kroku następuje szereg procesami kopiowania plików .nupkg do wewnętrznej infrastruktury udziałów plików. Na przykład:<br><br>**Kopiowanie plików 1 z content\PerfCollector\VirtualMachines do \VirtualMachineName-ERCS03\C$\TraceCollectorUpdate\PerfCounterConfiguration**  | Przyczyną problemu jest pliki dziennika zapełnia się dyski na maszynie wirtualnej infrastruktury i problemu w systemu Windows Server skalowalnego w poziomie serwerem plików (SOFS) które zostaną dostarczone w kolejnych aktualizacji. | Skontaktuj się z Microsoft dział obsługi klienta i pomocy technicznej (CSS), aby uzyskać pomoc. | 
| Podczas wykonywania aktualizacji podczas wykonywania kroku może wystąpić błąd podobny do następującego "krok: uruchamiania kroku 2.13.2 — aktualizacja *VM_Name*" planu działania aktualizacji. (Nazwa maszyny wirtualnej może się różnić.)<br><br>**ActionPlanInstanceWarning NZ/MachineName: WarningMessage:Task: wywołanie interfejsu "LiveUpdate" roli "Cloud\Fabric\WAS" nie powiodło się:<br>typu "LiveUpdate" rola 'WAS' zgłosił wyjątek:<br>wystąpił błąd podczas przechowywania Inicjowanie: Wystąpił błąd podczas próby wywołania interfejsu API usługi Microsoft Storage: {"Komunikat": "Upłynął limit czasu podczas komunikacji z sieci szkieletowej usług. Typ wyjątku: TimeoutException. Komunikat o wyjątku: Upłynął limit czasu operacji. "}**  | Przyczyną problemu jest limit czasu operacji We/Wy w systemie Windows Server, który zostanie rozwiązany w kolejnych aktualizacji. | Aby uzyskać pomoc, skontaktuj się z Microsoft CSS.
| Podczas wykonywania aktualizacji podobny do poniższego może wystąpić błąd podczas wykonywania kroku "krok maszyn wirtualnych 21 ponowne uruchomienie programu SQL server".<br><br>**Typ "LiveUpdateRestart" roli "VirtualMachines" zgłosił wyjątek:<br>VerboseMessage: kwerendy [VirtualMachines:LiveUpdateRestart] dla maszyny Wirtualnej MachineName-Sql01. - 10/13/2017 5:23:50: 00 VerboseMessage: [maszyn wirtualnych: LiveUpdateRestart] maszyny Wirtualnej jest oznaczony jako dużej dostępności. -VerboseMessage 5:23:50: 00 10/13/2017: [VirtualMachines:LiveUpdateRestart] w MS. Internal.ServerClusters.ExceptionHelp.Build w MS. Internal.ServerClusters.ClusterResource.BeginTakeOffline (wartość logiczna force) w Microsoft.FailoverClusters.PowerShell.StopClusterResourceCommand.BeginTimedOperation() w Microsoft.FailoverClusters.PowerShell.TimedCmdlet.WrappedProcessRecord() na Microsoft.FailoverClusters.PowerShell.FCCmdlet.ProcessRecord() - 10/13/2017 WarningMessage:Task 5:23:50: 00: wywołanie interfejsu "LiveUpdateRestart" roli " Cloud\Fabric\VirtualMachines nie powiodło się:** | Ten problem może wystąpić, jeśli maszyna wirtualna nie może uruchomić ponownie. | Aby uzyskać pomoc, skontaktuj się z Microsoft CSS.
| Podczas wykonywania aktualizacji może wystąpić błąd podobny do następującego:<br><br>**2017-10-22T01:37:37.5369944Z typu "Zamknij", roli 'SQL' zgłosił wyjątek: wstrzymywanie klientem węzła "s45r1004 Sql01" Stop-SQL, C:\ProgramData\SF\ErcsClusterNode2\Fabric\work\Applications\EnterpriseCloudEngineApplicationType_ wystąpił błąd App1\EnterpriseCloudEngineServicePkg.code.1.0.597.18\CloudDeployment\Roles\SQL\SQL.psm1: zamknięcie 542at, C:\ProgramData\SF\ErcsClusterNode2\Fabric\work\Applications\EnterpriseCloudEngineApplicationType_App1\ wiersz EnterpriseCloudEngineServicePkg.Code.1.0.597.18\CloudDeployment\Classes\SQL\SQL.psm1: wiersz 50at <ScriptBlock>, <No file>: wiersz 18at <ScriptBlock>, <No file>: wiersz 16** | Ten problem może wystąpić, jeśli nie można umieścić maszyny wirtualnej w stanie wstrzymanym, aby opróżnić dziennik ról. | Aby uzyskać pomoc, skontaktuj się z Microsoft CSS.
| Po wykonaniu aktualizacji może wystąpić jeden z następujących błędów:<br><br>**Typ "Weryfikuj" roli "Usług AD FS" zgłosił wyjątek: Weryfikacja dla roli usług AD FS/grafu nie powiodła się z powodu błędu: błąd podczas sprawdzania punktu końcowego sondowanie usług AD FS *endpoint_URI*: wyjątek podczas wywoływania "GetResponse" z argumentów "0": "serwera zdalnego zwrócony błąd: (503) serwer jest niedostępny. "na Invoke ADFSGraphValidation**<br><br>**Typ "Weryfikuj" roli "Usług AD FS" zgłosił wyjątek: Walidacja dla roli usług AD FS/grafu nie powiodła się z powodu błędu: błąd podczas pobierania właściwości usługi AD FS: nie można połączyć z NET.TCP://localhost: 1500/zasad. Próba nawiązania połączenia trwała 00:00:02.0498923 przedział czasu. Kod błędu TCP 10061: połączenie nie może ustanowione, ponieważ na docelowym komputerze aktywnie go odmawia 127.0.0.1:1500. w ADFSGraphValidation wywołania** | Plan akcji update nie może sprawdzić poprawności kondycji usługi Active Directory Federation Services (AD FS). | Aby uzyskać pomoc, skontaktuj się z Microsoft CSS.

## <a name="known-issues-post-installation"></a>Znane problemy (po instalacji)

Ta sekcja zawiera po instalacji, znane problemy związane z 20171020.1 kompilacji.

### <a name="portal"></a>Portal

- Może nie być możliwe do wyświetlania zasobów obliczeniowych i magazynu w portalu administratora. Oznacza to, że wystąpił błąd podczas instalowania aktualizacji i aktualizacji niepoprawnie Zgłoszono następujący powiodło się. Jeśli ten problem występuje, skontaktuj się z CSS firmy Microsoft, aby uzyskać pomoc.
- Może pojawić się pustego pulpitu nawigacyjnego w portalu. Aby odzyskać pulpitu nawigacyjnego, wybierz koło zębate ikonę w prawym górnym rogu portalu, a następnie wybierz **przywrócić ustawienia domyślne**.
- Użytkownicy można przeglądać pełnego portalu marketplace bez subskrypcji i widoczne elementy administracyjne, takie jak plany i oferty. Te elementy są niefunkcjonalne dla użytkowników.
- **Przenieś** przycisku jest wyłączona podczas przeglądania właściwości grupy zasobów. To zachowanie jest oczekiwane. Przeniesienie grup zasobów między subskrypcjami nie jest obecnie obsługiwane.
- Nie jest możliwe wyświetlić uprawnienia do subskrypcji przy użyciu portali stosu Azure. Jako rozwiązanie alternatywne można sprawdzić uprawnień za pomocą programu PowerShell.
-  Na każdy przepływ pracy, gdzie wybierz subskrypcji, grupy zasobów lub lokalizacji w listy rozwijanej może wystąpić jeden lub więcej z następujących problemów:

   - Może pojawić się pusty wiersz na początku listy. Nadal można wybrać elementu zgodnie z oczekiwaniami.
   - Jeśli na liście elementów na liście rozwijanej jest krótki, nie można przeglądać nazwy elementu.
   - Jeśli masz wiele subskrypcji użytkownika listy rozwijanej grupy zasobów może być pusta. 

   Aby obejść problemy z ostatnich dwóch, możesz wpisać nazwę subskrypcji lub grupy zasobów (jeśli ją znasz), lub można użyć programu PowerShell.
  
### <a name="backup"></a>Tworzenie kopii zapasowych

- Nie należy włączać infrastruktura kopii zapasowej na **infrastruktura kopii zapasowej** bloku.

### <a name="health-and-monitoring"></a>Monitorowania kondycji i

- Jeśli ponowne uruchomienie wystąpienia roli infrastruktury, może zostać wyświetlony komunikat informujący, że ponowne uruchomienie nie powiodło się. Jednak ponownego uruchamiania faktycznie zakończyło się pomyślnie.

### <a name="services"></a>Usługi

**Marketplace**
- Podczas dodawania elementów do stosu Azure marketplace przy użyciu **Dodaj z platformy Azure** opcji, nie wszystkie elementy mogą być widoczne do pobrania.
- Brak nie doświadczenie marketplace, aby utworzyć zestawy skalowania maszyny wirtualnej. Można utworzyć skali ustawić za pomocą szablonu.
- Dzierżawca musi zarejestrować dostawcy zasobów magazynu, przed utworzeniem ich pierwszej funkcji platformy Azure w ramach subskrypcji.
- Usunięcie użytkownika powoduje subskrypcje zasoby oddzielone. Jako obejście najpierw usuń zasoby użytkownika lub grupy zasobów całej, a następnie usuń subskrypcji użytkownika. 

**SQL/MySQL**
- Może potrwać do godziny dzierżawców można utworzyć bazy danych w nowym SQL lub MySQL jednostki SKU. 
- Tworzenie elementów bezpośrednio na SQL i MySQL serwerów, które nie są wykonywane przez dostawcę zasobów hosta nie jest obsługiwana i może spowodować niezgodne stanu.

**Obliczanie**
- Użytkownicy skorzystać z opcji, aby utworzyć maszynę wirtualną z magazynu geograficznie nadmiarowego. Ta konfiguracja powoduje niepowodzenie tworzenia maszyny wirtualnej.
- Można skonfigurować dostępność maszyn wirtualnych, ustaw tylko z jednej domeny błędów i jedną domenę aktualizacji.
 
**Sieć**
- Nie można utworzyć modułu równoważenia obciążenia z publicznym adresem IP, za pomocą portalu. Jako rozwiązanie alternatywne można utworzyć modułu równoważenia obciążenia za pomocą programu PowerShell.
- Podczas tworzenia modułu równoważenia obciążenia sieciowego, należy utworzyć regułę translatora adresów sieciowych adres. Jeśli nie, otrzymasz wystąpił błąd podczas próby dodania reguły NAT po utworzeniu usługi równoważenia obciążenia.
- Nie można usunąć skojarzenie publicznego adresu IP z maszyną wirtualną (VM), po utworzeniu maszyny Wirtualnej i skojarzonych z tym adresem IP. Usuwanie skojarzeń pojawi się do pracy, ale poprzednio przypisanych publiczny adres IP pozostają skojarzone z oryginalna maszyna wirtualna. Dzieje się tak nawet w przypadku ponownego przypisywania adresów IP do nowej maszyny Wirtualnej (nazywane *wymiany wirtualnych adresów IP*). Wszystkie przyszłe próbuje nawiązać połączenie za pośrednictwem tego wyniku adresów IP w przypadku połączenia pierwotnie skojarzonego VM, a nie nowy. Obecnie tylko musi używać nowego publiczne adresy IP do tworzenia nowej maszyny Wirtualnej.
 
### <a name="field-replaceable-unit-fru-procedures"></a>Pole procedury zamiennych (FRU)

- Podczas przebiegu aktualizacji nie są poprawianie obrazów w trybie offline. Jeśli chcesz zamienić węzła jednostki skalowania, współpracować z dostawcą sprzętu przez producenta OEM w celu upewnij się, że węzeł zastąpionego zawiera najnowsze poziomu poprawki.

## <a name="download-the-update"></a>Pobierz aktualizację

Możesz pobrać pakiet aktualizacji 1710 [tutaj](https://aka.ms/azurestackupdatedownload).

## <a name="next-steps"></a>Następne kroki

- Omówienie zarządzania aktualizacjami w stosie Azure, zobacz [zarządzania aktualizacjami w omówieniu stosu Azure](azure-stack-updates.md).
- Aby uzyskać informacje dotyczące stosowania aktualizacji, zobacz [stosowanie aktualizacji w stosie Azure](azure-stack-apply-updates.md).