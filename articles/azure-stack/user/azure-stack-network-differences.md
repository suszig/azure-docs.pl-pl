---
title: "Sieci Azure stosu: Różnice i zagadnienia dotyczące"
description: "Więcej informacji na temat różnic i zagadnienia dotyczące podczas pracy z sieci w programie Azure stosu."
services: azure-stack
keywords: 
author: ScottNapolitan
ms.author: victorh
ms.date: 9/25/2017
ms.topic: article
ms.service: azure-stack
ms.openlocfilehash: 3c72c58e63335f1cb440811e283bd742b8124161
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/08/2017
---
# <a name="considerations-for-azure-stack-networking"></a>Zagadnienia dotyczące sieci Azure stosu

*Dotyczy: Azure stosu zintegrowanych systemów i Azure stosu Development Kit*

Sieć w stosie Azure udostępnia wiele funkcji, których możesz znaleźć na platformie Azure z pewne różnice, które należy zrozumieć przed rozpoczęciem wdrażania.


Ten artykuł zawiera omówienie unikatowy zagadnienia dotyczące sieci i jej funkcji w stosie Azure. Aby uzyskać informacje ogólne różnice między stosu Azure i usługi Azure, zobacz [kluczowe zagadnienia dotyczące](azure-stack-considerations.md) tematu.


## <a name="cheat-sheet-networking-differences"></a>Ściągawka: różnice sieci

|Usługa | Funkcja | Azure (globalna) | Azure Stack |
| --- | --- | --- | --- |
| DNS | Wielodostępne DNS | Obsługiwane| Nie jest jeszcze obsługiwane|
| |Rekordów AAAA usługi DNS|Obsługiwane|Nieobsługiwane|
| |Strefy DNS na subskrypcję|100 (ustawienie domyślne)<br>Można zwiększyć na żądanie.|100|
| |Zestawy rekordów DNS dla strefy|5000 (ustawienie domyślne)<br>Można zwiększyć na żądanie.|5000|
||Serwery nazw dla delegowania strefy|Azure Podaj cztery serwery nazw dla każdej strefy użytkownika (dzierżawcy), która jest tworzona.|Stos Azure udostępnia dwa serwery nazw dla każdej strefy użytkownika (dzierżawcy), która jest tworzona.|
| Sieć wirtualna|Wirtualne sieci równorzędne|Połącz dwie sieci wirtualne w tym samym regionie za pośrednictwem sieci Azure sieci szkieletowej.|Nie jest jeszcze obsługiwane|
| |Adresy IPv6|Można przypisać adres IPv6 w ramach [Konfiguracja interfejsu sieciowego](https://docs.microsoft.com/azure/virtual-network/virtual-network-network-interface-addresses#ip-address-versions).|Obsługiwany jest tylko protokół IPv4.|
|Bramy sieci VPN|Brama sieci VPN typu punkt lokacja|Obsługiwane|Nie jest jeszcze obsługiwane|
| |Brama sieci wirtualnej do sieci wirtualnej|Obsługiwane|Nie jest jeszcze obsługiwane|
| |Jednostki SKU bramy sieci VPN|Obsługa Basic, GW1, GW2, GW3, standardowe wysokiej wydajności, niezwykle wydajny. |Obsługa podstawowa, standardowa i jednostki SKU wysokiej wydajności.|
|Moduł równoważenia obciążenia|Publiczne adresy IP protokołu IPv6|Obsługa przypisywanie IPv6 publicznego adresu IP do modułu równoważenia obciążenia.|Obsługiwany jest tylko protokół IPv4.|
|Network Watcher|Monitor sieci dzierżawcy możliwości monitorowania sieci|Obsługiwane|Nie jest jeszcze obsługiwane|
|CDN|Profile sieci dostarczania zawartości|Obsługiwane|Nie jest jeszcze obsługiwane|
|Brama aplikacji|Równoważenia obciążenia warstwy 7|Obsługiwane|Nie jest jeszcze obsługiwane|
|Traffic Manager|Kierującą ruch przychodzący do aplikacji optymalnej wydajności i niezawodności.|Obsługiwane|Nie jest jeszcze obsługiwane|
|ExpressRoute|Skonfigurowanie istnieje w przypadku szybkiego połączenia do usług chmurowych firmy Microsoft z zakładzie infrastruktury lub wspólnej lokalizacji lokalnej.|Obsługiwane|Obsługa nawiązywania stosu Azure obwodzie usługi Express Route.|

## <a name="next-steps"></a>Następne kroki

[Usługa DNS w usłudze Azure Stack](azure-stack-dns.md)
