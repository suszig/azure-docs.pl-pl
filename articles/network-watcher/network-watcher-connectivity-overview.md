---
title: "Wprowadzenie do łączności zaewidencjonować obserwatora sieciowego Azure | Dokumentacja firmy Microsoft"
description: "Ta strona zawiera przegląd możliwości połączenia obserwator sieci"
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: 
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/11/2017
ms.author: jdial
ms.openlocfilehash: 16ceef9c923b6a933a5caf752991b466346e0ebc
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="introduction-to-connectivity-check-in-azure-network-watcher"></a>Wprowadzenie do sprawdzenia łączności z serwerem w obserwatora sieciowego Azure

Funkcja łączności obserwatora sieciowego umożliwia bezpośrednie połączenie TCP z maszyny wirtualnej z maszyną wirtualną (VM), pełną nazwę domeny (FQDN), identyfikator URI, lub adres IPv4. Scenariusze sieci są skomplikowane, są implementowane przy użyciu grup zabezpieczeń sieci, zapór, trasy zdefiniowane przez użytkownika i zasobów udostępnianych przez platformę Azure. Złożonych konfiguracji należy trudne Rozwiązywanie problemów z łącznością. Obserwatora sieciowego pozwala skrócić czas, aby znaleźć i wykrywania problemów z łącznością. Wyniki zwrócone zapewniają wgląd w informacje czy problemem z łącznością przyczyną jest to platforma lub problem z konfiguracją użytkownika. Można sprawdzić łączności z [PowerShell](network-watcher-connectivity-powershell.md), [interfejsu wiersza polecenia Azure](network-watcher-connectivity-cli.md), i [interfejsu API REST](network-watcher-connectivity-rest.md).

> [!IMPORTANT]
> Sprawdź łączność wymaga rozszerzenia maszyny wirtualnej `AzureNetworkWatcherExtension`. Instalowanie rozszerzenia na maszynie Wirtualnej systemu Windows można znaleźć [rozszerzenie maszyny wirtualnej Azure sieci obserwatorów agenta dla systemu Windows](../virtual-machines/windows/extensions-nwa.md) i dla maszyny Wirtualnej systemu Linux, odwiedź [rozszerzenie maszyny wirtualnej Azure sieci obserwatorów agenta dla systemu Linux](../virtual-machines/linux/extensions-nwa.md).

## <a name="response"></a>Odpowiedź

W poniższej tabeli przedstawiono właściwości zwracany, gdy Sprawdzanie łączności zakończył działanie.

|Właściwość  |Opis  |
|---------|---------|
|ConnectionStatus     | Stan sprawdzenia łączności z serwerem. Wyniki są **osiągalne** i **informujący**.        |
|AvgLatencyInMs     | Średni czas oczekiwania, podczas sprawdzania łączności (w milisekundach). (Tylko widoczne, gdy stan wyboru jest dostępny)        |
|MinLatencyInMs     | Minimalny czas oczekiwania, podczas sprawdzania łączności (w milisekundach). (Tylko widoczne, gdy stan wyboru jest dostępny)        |
|MaxLatencyInMs     | Maksymalny czas oczekiwania, podczas sprawdzania łączności (w milisekundach). (Tylko widoczne, gdy stan wyboru jest dostępny)        |
|ProbesSent     | Liczba sond wysyłane podczas sprawdzania. Wartość maksymalna to 100.        |
|ProbesFailed     | Liczbę sond, których nie powiodła się podczas sprawdzania. Wartość maksymalna to 100.        |
|Liczba przeskoków     | Przeskok przez ścieżkę przeskoku ze źródła do miejsca docelowego.        |
|Przeskoków []. Typ     | Typ zasobu. Możliwe wartości to **źródła**, **VirtualAppliance**, **VnetLocal**, i **Internet**.        |
|Przeskoków []. Identyfikator | Unikatowy identyfikator przeskoku.|
|Przeskoków []. Adres | Adres IP przeskoku.|
|Przeskoków []. Identyfikator zasobu | ResourceID przeskoku Jeśli przeskoków jest zasobem platformy Azure. Jeśli jest zasobu internetowego, ResourceID jest **Internet**. |
|Przeskoków []. NextHopIds | Unikatowy identyfikator następnego przeskoku podjęte.|
|Przeskoków []. Problemy | Zbiór problemów, które wystąpiły podczas sprawdzania przy tym przeskoku. Jeśli nie było żadnych problemów, wartość jest pusta.|
|Przeskoków []. Problemy z []. Punkt początkowy | W bieżącej przeskoku, którym wystąpił problem. Możliwe wartości:<br/> **Liczba przychodzących** — problem jest łącze poprzedniej przeskoku do bieżącego przeskoku<br/>**Wychodzące** — problem jest łącze bieżącego przeskoku do następnego przeskoku<br/>**Lokalne** — problem jest bieżącym przeskoku.|
|Przeskoków []. Problemy z []. Ważność | Ważność wykrytego problemu. Możliwe wartości to **błąd** i **ostrzeżenie**. |
|Przeskoków []. Problemy z []. Typ |Typ Napotkano problem. Możliwe wartości: <br/>**PROCESOR CPU**<br/>**Pamięci**<br/>**GuestFirewall**<br/>**DnsResolution**<br/>**NetworkSecurityRule**<br/>**UserDefinedRoute** |
|Przeskoków []. Problemy z []. Kontekst |Szczegóły dotyczące Napotkano problem.|
|Przeskoków []. Problemy z []. Kontekst .key] |Klucz para klucz-wartość zwracana.|
|Przeskoków []. Problemy z []. Kontekst .value] |Zwracana wartość para klucz-wartość.|

Oto przykład problemu znaleziono przeskoku.

```json
"Issues": [
    {
        "Origin": "Outbound",
        "Severity": "Error",
        "Type": "NetworkSecurityRule",
        "Context": [
            {
                "key": "RuleName",
                "value": "UserRule_Port80"
            }
        ]
    }
]
```
## <a name="fault-types"></a>Typy błędów

Sprawdź łączność zwraca typy błędów dotyczących połączenia. Poniższa tabela zawiera listę bieżące typy błędów zwracane.

|Typ  |Opis  |
|---------|---------|
|Procesor CPU     | Wysokie użycie procesora CPU.       |
|Memory (Pamięć)     | Wysokie użycie pamięci.       |
|GuestFirewall     | Ruch jest zablokowany z powodu konfiguracji zapory maszyny wirtualnej.        |
|DNSResolution     | Rozpoznawanie nazw DNS nie powiodło się dla adresu docelowego.        |
|NetworkSecurityRule    | Ruch jest blokowany przez reguły NSG (zasada jest zwracany)        |
|UserDefinedRoute|Ruch zostanie usunięte ze względu na zdefiniowane przez użytkownika lub systemu trasy. |

### <a name="next-steps"></a>Następne kroki

Dowiedz się, jak sprawdzić połączenia z zasobem, odwiedzając: [Sprawdź łączność z obserwatora sieciowego Azure](network-watcher-connectivity-powershell.md).

<!--Image references-->
[1]: ./media/network-watcher-next-hop-overview/figure1.png

