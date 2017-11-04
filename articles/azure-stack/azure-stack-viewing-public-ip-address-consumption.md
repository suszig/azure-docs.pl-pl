---
title: "Wyświetl publicznych zużycia adresów IP w stosie Azure | Dokumentacja firmy Microsoft"
description: "Administratorzy mogą wyświetlać zużycie publicznych adresów IP w regionie"
services: azure-stack
documentationcenter: 
author: ScottNapolitan
manager: darmour
editor: 
ms.assetid: 0f77be49-eafe-4886-8c58-a17061e8120f
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 9/25/2017
ms.author: scottnap
ms.openlocfilehash: 7651565eebf6272f307a4ce4790ca19b41bfa826
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="view-public-ip-address-consumption-in-azure-stack"></a>Wyświetl publicznych zużycia adresów IP w stosie Azure

*Dotyczy: Azure stosu zintegrowanych systemów i Azure stosu Development Kit*

Administrator chmury można wyświetlić liczbę publiczne adresy IP przydzielone do dzierżawców liczba publicznych adresów IP, które są nadal dostępne do alokacji i odsetek publiczne adresy IP, które zostały przydzielone w tej lokalizacji.

**Publicznego adresu IP do puli użycia** kafelka zawiera całkowitą liczbę publicznych adresów IP, które zostały wykorzystane przez wszystkie publiczne pule adresów IP w sieci szkieletowej, czy były używane dla dzierżawcy wystąpień maszyn wirtualnych IaaS, infrastruktury sieci szkieletowej usługi lub zasoby publicznych adresów IP, które jawnie zostały utworzone przez dzierżawców.

Celem tego kafelka jest umożliwiają administratorom Azure stosu w pewnym sensie ogólna liczba publicznych adresów IP, które zostały wykorzystane w tej lokalizacji. Dzięki temu administratorzy ustalić, czy działają na niski w przypadku tego zasobu.

Na **dostawców zasobów**, **sieci** bloku **publicznego adresu IP, adresy** elementu menu pod **zasoby dzierżawcy** wyświetla tylko te publiczne Adresy IP, które zostały *jawnie utworzonego przez dzierżawców*. Działa w taki sposób, liczba **używane** publicznego adresu IP, adresy na **publicznego adresu IP do puli użycia** kafelka zawsze różni się od (większe niż) numer na **publiczne adresy IP** kafelka w obszarze **dzierżawy zasobów**.

## <a name="view-the-public-ip-address-usage-information"></a>Widok publiczne informacje o użyciu adresu IP
Aby wyświetlić całkowita liczba publicznych adresów IP, które zostały wykorzystane w regionie:

1. W portalu administratora stosu Azure kliknij **więcej usług**w obszarze **zasobów administracyjnych**, kliknij przycisk **dostawców zasobów**.
2. Z listy **dostawców zasobów**, wybierz pozycję **sieci**.
3. **Sieci** wyświetla bloku **publicznego adresu IP do puli użycia** kafelka w **omówienie** sekcji.

![Blok dostawcy zasobów sieciowych](media/azure-stack-viewing-public-ip-address-consumption/image01.png)

Należy pamiętać, że **używane** liczba reprezentuje liczbę publicznych adresów IP z wszystkich publicznych pule adresów IP w tej lokalizacji są przypisane. **Wolne** pul, które nie zostały przypisane i są nadal dostępne adresów liczba reprezentuje liczba publicznych adresów IP adresy z wszystkich publicznego adresu IP. **Wykorzystania** numer reprezentuje liczbę używane lub przypisać adresy jako procent całkowitej liczby publicznych adresów IP w wszystkich publicznych pule adresów IP w tej lokalizacji.

## <a name="view-the-public-ip-addresses-that-were-created-by-tenant-subscriptions"></a>Wyświetl publiczne adresy IP, które zostały utworzone przez subskrypcji dzierżawcy
Aby wyświetlić listę publicznych adresów IP, które jawnie zostały utworzone przez subskrypcji dzierżawcy w określonym regionie, kliknij przycisk **publicznego adresu IP, adresy** w obszarze **zasoby dzierżawcy**.

![Publiczne adresy IP dzierżawy](media/azure-stack-viewing-public-ip-address-consumption/image02.png)

Można zauważyć, że niektóre publiczne adresy IP, które dynamicznie przydzielane znajdują się na liście, ale nie ma adresu jeszcze skojarzonych z nimi. Jest to spowodowane zasobu adresu ma zostały utworzone w dostawcy zasobów sieciowych, ale nie w kontrolerze sieci.

Kontroler sieci nie przypisuje adres dla tego zasobu, do momentu rzeczywistości jest on powiązany z interfejsem, do karty interfejsu sieciowego (NIC), usługi równoważenia obciążenia lub bramy sieci wirtualnej. Podczas publiczny adres IP jest powiązany z interfejsem, adres IP do niego przydziela Kontroler sieci i wyświetlone w **adres** pola.

## <a name="view-the-public-ip-address-information-summary-table"></a>Widok publiczny tabelę adresów IP informacji podsumowania
Istnieje wiele różnych w których publiczne adresy IP są przypisywane przypadków, które określają, czy adres jest wyświetlany w jedną listę lub innym.

| **Publiczny przypadku przypisanie adresów IP** | **Zostanie wyświetlony w Podsumowanie użycia** | **Zostanie wyświetlony na liście adresów IP publiczny dzierżawcy** |
| --- | --- | --- |
| Dynamiczne publiczny adres IP nie zostały jeszcze przypisane do karty Sieciowej lub Usługa równoważenia obciążenia (tymczasowe) |Nie |Tak |
| Dynamiczne publiczny adres IP przypisane do karty Sieciowej lub Usługa równoważenia obciążenia. |Tak |Tak |
| Statycznego publicznego adresu IP przypisanego do dzierżawcy karty Sieciowej lub Usługa równoważenia obciążenia. |Tak |Tak |
| Statycznego publicznego adresu IP przypisane do punktu końcowego usługi infrastruktury sieci szkieletowej. |Tak |Nie |
| Publiczny adres IP niejawnie tworzonych dla wystąpień maszyn wirtualnych IaaS i używać dla ruchu wychodzącego translatora adresów Sieciowych w sieci wirtualnej. Są one tworzone w tle, gdy dzierżawcy tworzy wystąpienie maszyny Wirtualnej, dzięki czemu maszyny wirtualne można wysyłać informacje do Internetu. |Tak |Nie |

## <a name="next-steps"></a>Następne kroki
[Zarządzanie kontami magazynu Azure stosu](azure-stack-manage-storage-accounts.md)