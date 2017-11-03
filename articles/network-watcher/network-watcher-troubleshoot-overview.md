---
title: "Wprowadzenie do rozwiązywania problemów w obserwatora sieciowego Azure zasobów | Dokumentacja firmy Microsoft"
description: "Ta strona zawiera omówienie funkcji rozwiązywania problemów z zasobów obserwatora sieciowego"
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: 
ms.assetid: c1145cd6-d1cf-4770-b1cc-eaf0464cc315
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/19/2017
ms.author: jdial
ms.openlocfilehash: a37c92e1aa58184ed29185742ec727c120fe593f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="introduction-to-resource-troubleshooting-in-azure-network-watcher"></a>Wprowadzenie do rozwiązywania problemów w obserwatora sieciowego Azure zasobów

Bramy sieci wirtualnej zapewniają łączność między zasobami lokalnymi i innych sieci wirtualnych w obrębie platformy Azure. Monitorowanie tych bram i ich połączeń jest bardzo istotne dla zapewnienia komunikacji nie jest uszkodzona. Obserwatora sieciowego umożliwia rozwiązywanie problemów z bramy sieci wirtualnej i połączenia. Można go wywołać za pomocą portalu, programu PowerShell, interfejsu wiersza polecenia lub interfejsu API REST. Po wywołaniu obserwatora sieciowego diagnozuje kondycji bramy sieci wirtualnej lub połączenia i zwracają odpowiednie wartości. To żądanie jest długo działającą transakcję, wyniki są zwracane po zakończeniu diagnostyki.

![portal][2]

## <a name="results"></a>Wyniki

Wstępne wyników zwróconych nadaj ogólny obraz kondycji zasobu. Lepszy informacje mogą być dostępne dla zasobów, jak pokazano w poniższej sekcji:

Poniżej znajduje się wartości zwracane z Rozwiązywanie problemów z interfejsu API:

* **wartość startTime** — ta wartość jest uruchomieniu wywołania interfejsu API Rozwiązywanie problemów.
* **wartość endTime** — ta wartość jest czas zakończenia rozwiązywania problemów.
* **Kod** — ta wartość jest zła, w przypadku awarii jednego diagnostyki.
* **wyniki** -wyników jest zbiór wyników zwróconych na połączenie lub brama sieci wirtualnej.
    * **Identyfikator** — ta wartość jest typu błędu.
    * **Podsumowanie** — ta wartość jest podsumowanie usterki.
    * **szczegółowe** -wartość zawiera szczegółowy opis błędu.
    * **recommendedActions** — ta właściwość jest kolekcją zalecane akcje.
      * **actionText** — ta wartość zawiera opis czynności do wykonania.
      * **actionUri** — ta wartość Określa identyfikator URI do dokumentacji na temat sposobu działania.
      * **actionUriText** — ta wartość jest krótki opis tekst akcji.

W poniższej tabeli przedstawiono typy inny błąd (identyfikator w obszarze wyniki z powyższej listy), które są dostępne, a jeśli błąd tworzy dzienniki.

### <a name="gateway"></a>Brama

| Typ błędu | Przyczyna | Log|
|---|---|---|
| NoFault | Gdy błąd nie została wykryta. |Tak|
| GatewayNotFound | Nie można odnaleźć bramy lub bramy nie jest obsługiwana administracyjnie. |Nie|
| PlannedMaintenance |  Wystąpienie bramy jest w trakcie konserwacji.  |Nie|
| UserDrivenUpdate | Gdy aktualizacja użytkownika jest w toku. Może to być operacji zmiany rozmiaru. | Nie |
| VipUnResponsive | Nie można osiągnąć podstawowego wystąpienia bramy. Dzieje się tak, gdy badania kondycji nie powiodło się. | Nie |
| PlatformInActive | Istnieje problem z platformą. | Nie|
| ServiceNotRunning | Usługa podstawowy nie jest uruchomiona. | Nie|
| NoConnectionsFoundForGateway | Połączenia nie istnieje w bramie. Jest to tylko ostrzeżenie.| Nie|
| ConnectionsNotConnected | Połączenia nie są połączone. Jest to tylko ostrzeżenie.| Tak|
| GatewayCPUUsageExceeded | Bieżące użycie procesora CPU bramy jest > 95%. | Tak |

### <a name="connection"></a>Połączenie

| Typ błędu | Przyczyna | Log|
|---|---|---|
| NoFault | Gdy błąd nie została wykryta. |Tak|
| GatewayNotFound | Nie można odnaleźć bramy lub bramy nie jest obsługiwana administracyjnie. |Nie|
| PlannedMaintenance | Wystąpienie bramy jest w trakcie konserwacji.  |Nie|
| UserDrivenUpdate | Gdy aktualizacja użytkownika jest w toku. Może to być operacji zmiany rozmiaru.  | Nie |
| VipUnResponsive | Nie można osiągnąć podstawowego wystąpienia bramy. Zdarza się, gdy badania kondycji nie powiodło się. | Nie |
| ConnectionEntityNotFound | Brak konfiguracji połączenia. | Nie |
| ConnectionIsMarkedDisconnected | Połączenie jest oznaczony jako "odłączony". |Nie|
| ConnectionNotConfiguredOnGateway | Podległej usłudze nie ma skonfigurowanego połączenia. | Tak |
| ConnectionMarkedStandy | Podstawowe usługi jest oznaczony jako stan wstrzymania.| Tak|
| Authentication | Niezgodność klucz wstępny. | Tak|
| PeerReachability | Brama elementu równorzędnego nie jest dostępny. | Tak|
| IkePolicyMismatch | Brama równorzędnej ma zasady IKE, które nie są obsługiwane przez platformę Azure. | Tak|
| Błąd WfpParse | Wystąpił błąd podczas analizowania dziennika platformy filtrowania systemu Windows. |Tak|

## <a name="supported-gateway-types"></a>Obsługiwane typy bramy

Na poniższej liście przedstawiono obsługę Pokazuje połączenia i bram, które są obsługiwane w rozwiązywaniu problemów obserwatora sieciowego.
|  |  |
|---------|---------|
|**Typy bramy**   |         |
|Sieć VPN      | Obsługiwane        |
|ExpressRoute | Nieobsługiwane |
|Hypernet | Nieobsługiwane|
|**Typy z siecią VPN** | |
|Na podstawie trasy | Obsługiwane|
|Na podstawie zasad | Nieobsługiwane|
|**Typy połączeń**||
|Protokół IPSec| Obsługiwane|
|VNet2Vnet| Obsługiwane|
|ExpressRoute| Nieobsługiwane|
|Hypernet| Nieobsługiwane|
|VPNClient| Nieobsługiwane|

## <a name="log-files"></a>Pliki dziennika

Pliki zasobów do rozwiązywania problemów z dziennika są przechowywane na koncie magazynu po zakończeniu rozwiązywania problemów z zasobów. Na poniższej ilustracji przedstawiono przykład zawartość wywołanie, które spowodowało wystąpienie błędu.

![plik zip][1]

> [!NOTE]
> W niektórych przypadkach tylko podzestaw pliki dzienników są zapisywane w pamięci masowej.

Aby uzyskać instrukcje dotyczące pobierania plików z kontami magazynu azure, zapoznaj się [Rozpoczynanie pracy z magazynem obiektów Blob platformy Azure przy użyciu platformy .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md). Kolejnym narzędziem, który może służyć jest Eksploratora usługi Storage. Więcej informacji na temat Eksploratora usługi Storage można znaleźć tutaj przy użyciu następującego łącza: [Eksploratora usługi Storage](http://storageexplorer.com/)

### <a name="connectionstatstxt"></a>ConnectionStats.txt

**ConnectionStats.txt** plik zawiera ogólna Statystyka połączenia, w tym Bajty przychodzące i wychodzące, stan połączenia i czasu, połączenie zostało nawiązane.

> [!NOTE]
> Jeśli wywołanie do rozwiązywania problemów z interfejsu API zwraca dobrej kondycji, jest jedynym elementem zwrócił w pliku zip **ConnectionStats.txt** pliku.

Zawartość tego pliku są podobne do poniższego przykładu:

```
Connectivity State : Connected
Remote Tunnel Endpoint :
Ingress Bytes (since last connected) : 288 B
Egress Bytes (Since last connected) : 288 B
Connected Since : 2/1/2017 8:22:06 PM
```

### <a name="cpustatstxt"></a>CPUStats.txt

**CPUStats.txt** plik zawiera użycie procesora CPU i pamięci w czasie testów.  Zawartość tego pliku jest podobny do poniższego przykładu:

```
Current CPU Usage : 0 % Current Memory Available : 641 MBs
```

### <a name="ikeerrorstxt"></a>IKEErrors.txt

**IKEErrors.txt** plik zawiera błędy IKE, które zostały odnalezione podczas monitorowania.

W poniższym przykładzie przedstawiono zawartość pliku IKEErrors.txt. Błędy mogą się różnić w zależności od tego problemu.

```
Error: Authentication failed. Check shared key. Check crypto. Check lifetimes. 
     based on log : Peer failed with Windows error 13801(ERROR_IPSEC_IKE_AUTH_FAIL)
Error: On-prem device sent invalid payload. 
     based on log : IkeFindPayloadInPacket failed with Windows error 13843(ERROR_IPSEC_IKE_INVALID_PAYLOAD)
```

### <a name="scrubbed-wfpdiagtxt"></a>Wyczyszczona wfpdiag.txt

**Scrubbed wfpdiag.txt** plik dziennika zawiera dziennik platformy filtrowania systemu Windows. Ten dziennik zawiera rejestrowanie IKE/AuthIP błędy i listy pakietów.

W poniższym przykładzie przedstawiono zawartość pliku Scrubbed wfpdiag.txt. W tym przykładzie klucza współużytkowanego połączenia jest niepoprawny, co wynika z 3 wiersza od dołu. Poniższy przykład się tylko fragment dziennika cały dziennik może być długi w zależności od tego problemu.

```
...
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|Deleted ICookie from the high priority thread pool list
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|IKE diagnostic event:
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|Event Header:
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  Timestamp: 1601-01-01T00:00:00.000Z
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  Flags: 0x00000106
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|    Local address field set
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|    Remote address field set
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|    IP version field set
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  IP version: IPv4
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  IP protocol: 0
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  Local address: 13.78.238.92
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  Remote address: 52.161.24.36
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  Local Port: 0
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  Remote Port: 0
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  Application ID:
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  User SID: <invalid>
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|Failure type: IKE/Authip Main Mode Failure
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|Type specific info:
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  Failure error code:0x000035e9
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|    IKE authentication credentials are unacceptable
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  Failure point: Remote
...
```

### <a name="wfpdiagtxtsum"></a>wfpdiag.txt.sum

**Wfpdiag.txt.sum** plik jest dziennika pokazującego buforów i przetworzonych zdarzeń.

Poniższy przykład jest zawartość pliku wfpdiag.txt.sum.
```
Files Processed:
    C:\Resources\directory\924336c47dd045d5a246c349b8ae57f2.GatewayTenantWorker.DiagnosticsStorage\2017-02-02T17-34-23\wfpdiag.etl
Total Buffers Processed 8
Total Events  Processed 2169
Total Events  Lost      0
Total Format  Errors    0
Total Formats Unknown   486
Elapsed Time            330 sec
+-----------------------------------------------------------------------------------+
|EventCount    EventName            EventType   TMF                                 |
+-----------------------------------------------------------------------------------+
|        36    ikeext               ike_addr_utils_c844  a0c064ca-d954-350a-8b2f-1a7464eef8b6|
|        12    ikeext               ike_addr_utils_c857  a0c064ca-d954-350a-8b2f-1a7464eef8b6|
|        96    ikeext               ike_addr_utils_c832  a0c064ca-d954-350a-8b2f-1a7464eef8b6|
|         6    ikeext               ike_bfe_callbacks_c133  1dc2d67f-8381-6303-e314-6c1452eeb529|
|         6    ikeext               ike_bfe_callbacks_c61  1dc2d67f-8381-6303-e314-6c1452eeb529|
|        12    ikeext               ike_sa_management_c5698  7857a320-42ee-6e90-d5d9-3f414e3ea2d3|
|         6    ikeext               ike_sa_management_c8447  7857a320-42ee-6e90-d5d9-3f414e3ea2d3|
|        12    ikeext               ike_sa_management_c494  7857a320-42ee-6e90-d5d9-3f414e3ea2d3|
|        12    ikeext               ike_sa_management_c642  7857a320-42ee-6e90-d5d9-3f414e3ea2d3|
|         6    ikeext               ike_sa_management_c3162  7857a320-42ee-6e90-d5d9-3f414e3ea2d3|
|        12    ikeext               ike_sa_management_c3307  7857a320-42ee-6e90-d5d9-3f414e3ea2d3|
```

## <a name="next-steps"></a>Następne kroki

Dowiedz się, jak diagnozować bramy sieci VPN i połączeń za pośrednictwem portalu, odwiedzając [bramy Rozwiązywanie problemów — Azure portal](network-watcher-troubleshoot-manage-portal.md).
<!--Image references-->

[1]: ./media/network-watcher-troubleshoot-overview/GatewayTenantWorkerLogs.png
[2]: ./media/network-watcher-troubleshoot-overview/portal.png
