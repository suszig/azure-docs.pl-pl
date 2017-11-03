---
title: "Diagnostyka w usłudze Azure Stack"
description: "Jak zbierać pliki dziennika diagnostyki Azure stosu"
services: azure-stack
author: adshar
manager: byronr
cloud: azure-stack
ms.service: azure-stack
ms.topic: article
ms.date: 10/2/2017
ms.author: adshar
ms.openlocfilehash: 9b1fbbf63ddd8bac2c1a76bbcd5daca69e2513f2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-stack-diagnostics-tools"></a>Narzędzia diagnostyki stosu Azure

*Dotyczy: Azure stosu zintegrowanych systemów i Azure stosu Development Kit*
 
Stos Azure jest duży zbiór elementów pracy ze sobą i interakcji ze sobą. Wszystkie te składniki generowania unikatowych dzienników. Diagnozowanie problemów może być trudne zadania, szczególnie w przypadku błędów pochodzących z wielu składników stosu Azure wchodzącymi w interakcje. 

Upewnij się, że mechanizm kolekcji dziennika jest łatwe i skuteczne pomoc do narzędzi naszych diagnostyki. Na poniższym diagramie przedstawiono sposób dziennika narzędzia kolekcji w pracach Azure stosu:

![Narzędzia do zbierania dzienników](media/azure-stack-diagnostics/image01.png)
 
 
## <a name="trace-collector"></a>Moduł zbierający śledzenia
 
Moduł zbierający śledzenia jest domyślnie włączona. Stale działa w tle i zbiera wszystkie dzienniki zdarzeń śledzenia dla systemu Windows (ETW) z usługi składowe w stosie Azure i zapisuje je w udziale lokalnym wspólnej. 

Poniżej przedstawiono ważne co należy wiedzieć o moduł zbierający śledzenia:
 
* Moduł zbierający śledzenia działa w sposób ciągły z limitami rozmiaru domyślnego. Domyślny maksymalny rozmiar dozwolony dla każdego pliku (200 MB) jest **nie** odcięcia rozmiar. Sprawdź rozmiar występuje okresowo (obecnie co 2 minuty) i jest bieżący plik > = 200 MB jest zapisywany i utworzony nowy plik. Na rozmiar pliku, generowany dla sesji zdarzeń istnieje również limit (można konfigurować) 8 GB. Po osiągnięciu tego limitu najstarsze pliki są usuwane, ponieważ są tworzone nowe.
* Istnieje limit 5-dniowy okres ważności w dziennikach. Konfiguruje się ten limit. 
* Każdy składnik definiuje właściwości konfiguracji śledzenia przy użyciu pliku JSON. Pliki JSON są przechowywane w `C:\TraceCollector\Configuration`. W razie potrzeby można edytować tych plików, aby zmienić limitów wieku i rozmiaru zebranych dzienników. Zmiany w tych plikach wymagać ponownego uruchomienia *moduł zbierający śledzenia stosu Azure firmy Microsoft* usługi, aby zmiany zaczęły obowiązywać.
* Poniższy przykład jest to plik JSON konfiguracji śledzenia w operacjach FabricRingServices z XRP maszyny Wirtualnej: 

```
{
    "LogFile": 
    {
        "SessionName": "FabricRingServicesOperationsLogSession",
        "FileName": "\\\\SU1FileServer\\SU1_ManagementLibrary_1\\Diagnostics\\FabricRingServices\\Operations\\AzureStack.Common.Infrastructure.Operations.etl",
        "RollTimeStamp": "00:00:00",
        "MaxDaysOfFiles": "5",
        "MaxSizeInMB": "200",
        "TotalSizeInMB": "5120"
    },
    "EventSources":
    [
        {"Name": "Microsoft-AzureStack-Common-Infrastructure-ResourceManager" },
        {"Name": "Microsoft-OperationManager-EventSource" },
        {"Name": "Microsoft-Operation-EventSource" }
    ]
}
```

* **MaxDaysOfFiles**

    Ten parametr określa wieku plików do zachowania. Starsze pliki dziennika są usuwane.
* **Wartość argumentu MaxSizeInMB**

    Ten parametr określa próg rozmiaru dla jednego pliku. Po osiągnięciu rozmiaru tworzony jest nowy plik ETL.
* **TotalSizeInMB**

    Ten parametr określa całkowity rozmiar plików etl wygenerowane z sesji zdarzeń. Jeśli rozmiar pliku jest większa niż wartość tego parametru, starsze pliki zostaną usunięte.
  
## <a name="log-collection-tool"></a>Narzędzie do zbierania dzienników
 
Polecenia programu PowerShell `Get-AzureStackLog` może służyć do zbierania dzienników z wszystkich składników w środowisku Azure stosu. Zapisuje je w plikach zip w lokalizacji zdefiniowanej przez użytkownika. Jeśli nasz zespół pomocy technicznej potrzebuje dzienników rozwiązywania problemu, ich może poprosić o uruchamianiu tego narzędzia.

> [!CAUTION]
> Te pliki dziennika może zawierać dane osobowe (dane osobowe). To brać pod uwagę przed publicznie post wszystkie pliki dziennika.
 
Poniżej przedstawiono kilka przykładowych typów dziennika, które są zbierane:
*   **Dzienniki wdrożenia usługi Azure stosu**
*   **Dzienniki zdarzeń systemu Windows**
*   **Dzienniki Panther**

   Aby rozwiązać problemy z tworzeniem maszyny Wirtualnej:
*   **Dzienniki klastra**
*   **Dzienniki diagnostyczne magazynu**
*   **Dzienniki zdarzeń systemu Windows**

Te pliki są zbierane przez moduł zbierający śledzenia i przechowywane w udziale, skąd `Get-AzureStackLog` pobiera je.
 
**Aby uruchomić Get AzureStackLog w systemie Azure stosu Development Kit (ASDK)**
1.  Zaloguj się jako AzureStack\AzureStackAdmin na hoście.
2.  Otwórz okno programu PowerShell jako administrator.
3.  Uruchom polecenie `Get-AzureStackLog`.  

    **Przykłady**

    - Zbieraj wszystkie dzienniki dla wszystkich ról:

        `Get-AzureStackLog -OutputPath C:\AzureStackLogs`

    - Zbierz dzienniki ról maszyn wirtualnych i BareMetal:

        `Get-AzureStackLog -OutputPath C:\AzureStackLogs -FilterByRole VirtualMachines,BareMetal`

    - Zbierz dzienniki ról maszyn wirtualnych i BareMetal, oraz datę filtrowania dla plików dziennika przez ostatnie 8 godzin:

        `Get-AzureStackLog -OutputPath C:\AzureStackLogs -FilterByRole VirtualMachines,BareMetal -FromDate (Get-Date).AddHours(-8)`

    - Zbierz dzienniki ról maszyn wirtualnych i BareMetal, oraz datę filtrowania dla plików dziennika w okresie między 8 godz i 2 godz. temu:

      `Get-AzureStackLog -OutputPath C:\AzureStackLogs -FilterByRole VirtualMachines,BareMetal -FromDate (Get-Date).AddHours(-8) -ToDate (Get-Date).AddHours(-2)`

**Do uruchomienia Get-AzureStackLog na stosie Azure zintegrowany system:**

Aby uruchomić narzędzie do zbierania dzienników na zintegrowany system, należy mieć dostęp do uprzywilejowanych punktu końcowego (program ten). Poniżej przedstawiono przykładowy skrypt można uruchomić przy użyciu program ten zbierania dzienników na zintegrowany system:

```
$ip = "<IP OF THE PEP VM>" # You can also use the machine name instead of IP here.
 
$pwd= ConvertTo-SecureString "<CLOUD ADMIN PASSWORD>" -AsPlainText -Force
$cred = New-Object System.Management.Automation.PSCredential ("<DOMAIN NAME>\CloudAdmin", $pwd)
 
$shareCred = Get-Credential
 
$s = New-PSSession -ComputerName $ip -ConfigurationName PrivilegedEndpoint -Credential $cred

$fromDate = (Get-Date).AddHours(-8)
$toDate = (Get-Date).AddHours(-2)  #provide the time that includes the period for your issue
 
Invoke-Command -Session $s {    Get-AzureStackLog -OutputPath "\\<HLH MACHINE ADDREESS>\c$\logs" -OutputSharePath "<EXTERNAL SHARE ADDRESS>" -OutputShareCredential $using:shareCred  -FilterByRole Storage -FromDate $using:fromDate -ToDate $using:toDate}

if($s)
{
    Remove-PSSession $s
}
```

- Zbieranie dzienników z program ten, określić `OutputPath` parametr do lokalizacji na komputerze HLH. Również upewnić się, że lokalizacja jest zaszyfrowany.
- Parametry `OutputSharePath` i `OutputShareCredential` są opcjonalne i są używane podczas przekazywania dzienników do zewnętrznego folderu udostępnionego. Użyj tych parametrów *dodatkowo* do `OutputPath`. Jeśli `OutputPath` nie zostanie określony, narzędzie do zbierania dzienników używa dysku systemowego maszyny wirtualnej program ten magazyn. Może to spowodować skryptu, aby zakończyć się niepowodzeniem, ponieważ miejsce na dysku jest ograniczone.
- Jak pokazano w poprzednim przykładzie `FromDate` i `ToDate` parametry może służyć do zbierania dzienników dla danego okresu. To są dostępne w przydatne w scenariuszach, takich jak zbieranie dzienników po zastosowaniu pakietu aktualizacji na zintegrowany system.

**Parametr kwestie dotyczące zarówno ASDK, jak i zintegrowanych systemów:**

- Jeśli `FromDate` i `ToDate` parametry nie są określone, dzienniki są domyślnie zbierane przez ostatnie cztery godziny.
- Można użyć `TimeOutInMinutes` parametr, aby ustawić limit czasu zbierania dzienników. Domyślnie jest ustawiona na 150 (2,5 godzin).

- Obecnie można używać `FilterByRole` parametru do kolekcji filtrów dziennika przez następujące role:

   |   |   |   |
   | - | - | - |
   | `ACSMigrationService`     | `ACSMonitoringService`   | `ACSSettingsService` |
   | `ACS`                     | `ACSFabric`              | `ACSFrontEnd`        |
   | `ACSTableMaster`          | `ACSTableServer`         | `ACSWac`             |
   | `ADFS`                    | `ASAppGateway`           | `BareMetal`          |
   | `BRP`                     | `CA`                     | `CPI`                |
   | `CRP`                     | `DeploymentMachine`      | `DHCP`               |
   |`Domain`                   | `ECE`                    | `ECESeedRing`        |        
   | `FabricRing`              | `FabricRingServices`     | `FRP`                |
   |` Gateway`                 | `HealthMonitoring`       | `HRP`                |               
   | `IBC`                     | `InfraServiceController` | `KeyVaultAdminResourceProvider`|
   | `KeyVaultControlPlane`    | `KeyVaultDataPlane`      | `NC`                 |            
   | `NonPrivilegedAppGateway` | `NRP`                    | `SeedRing`           |
   | `SeedRingServices`        | `SLB`                    | `SQL`                |     
   | `SRP`                     | `Storage`                | `StorageController`  |
   | `URP`                     | `UsageBridge`            | `VirtualMachines`    |  
   | `WAS`                     | `WASPUBLIC`              | `WDS`                |


Kilka dodatkowych rzeczy do uwzględnienia:

* Polecenie wymaga pewnego czasu do uruchomienia w oparciu o role, które są zbierane w dziennikach. Czynników także czas trwania zbierania dzienników i liczby węzłów w środowisku Azure stosu.
* Po zakończeniu zbierania dzienników, sprawdź nowy folder utworzony w `-OutputPath` określony w poleceniu parametru.
* Każda rola ma jej dzienników wewnątrz zip poszczególnych plików. W zależności od rozmiaru zebranych dzienniki rola może być jej dzienników podzielony na wiele plików zip. Dla roli Jeśli chcesz, aby rozpakować w celu pojedynczy folder plików dziennika, użycie narzędzia, które można rozpakować zbiorczo (na przykład 7zip). Zaznacz wszystkie pliki zip dla roli, a następnie wybierz **wyodrębnić tutaj**. To unzips wszystkich plików dziennika dla tej roli w jednym folderze scalone.
* Plik o nazwie `Get-AzureStackLog_Output.log` również jest tworzony w folderze, który zawiera pliki dziennika zip. Ten plik znajduje się w dzienniku danych wyjściowych polecenia, które mogą służyć do rozwiązywania problemów podczas zbierania dzienników.
* Do sprawdzania, czy określony błąd, dzienniki mogą być wymagane z więcej niż jednego składnika.
    -   W dziennikach zdarzeń dla wszystkich maszyn wirtualnych infrastruktury i systemu są gromadzone w *VirtualMachines* roli.
    -   System i dzienniki zdarzeń, dla wszystkich hostów są gromadzone w *BareMetal* roli.
    -   Dzienniki zdarzeń klastra trybu failover i funkcja Hyper-V są gromadzone w *magazynu* roli.
    -   Usługi ACS dzienniki są gromadzone w *magazynu* i *ACS* ról.

> [!NOTE]
> Limity rozmiaru i wiek są wymuszane na dzienniki zebrane, ponieważ jest, aby zapewnić jego efektywne wykorzystanie przestrzeni dyskowej, aby upewnić się, że nie pobierają rozpływową z dziennikami. Podczas diagnozowania problemu czasami potrzebny, dzienniki, które już nie istnieje z powodu tych ograniczeń. W związku z tym jest **zdecydowanie zalecane** czy odciążania dzienników do obszaru magazynu zewnętrznego (konto magazynu na platformie Azure, urządzenie magazynu lokalnego dodatkowe itp.) co 8 do 12 godzin i przechowywać je tam 1 – 3 miesiące, w zależności od sieci wymagania. Ponadto upewnij się, że ta lokalizacja magazynu jest zaszyfrowany.

## <a name="next-steps"></a>Następne kroki
[Rozwiązywanie problemów z Microsoft Azure stosu](azure-stack-troubleshooting.md)
