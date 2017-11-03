---
title: "Za pomocą usługi Azure DNS dla domen prywatnej | Dokumentacja firmy Microsoft"
description: "Przegląd prywatnej DNS obsługującego usługę w systemie Microsoft Azure."
services: dns
documentationcenter: na
author: garbrad
manager: 
editor: 
ms.assetid: 
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/04/2017
ms.author: garbrad
ms.openlocfilehash: ed47a9d850995aaf9e78bfde8b6a5fd80dc36918
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="using-azure-dns-for-private-domains"></a>Za pomocą usługi Azure DNS dla domen prywatnych
System nazw domen lub DNS, jest odpowiedzialny za tłumaczenia (lub rozpoznawania) nazwę usługi na adres IP. Usługa DNS platformy Azure to Usługa hostingu domen DNS, rozpoznawania nazw przy użyciu infrastruktury Microsoft Azure.  Oprócz internetowy domen DNS usługi Azure DNS teraz obsługuje również prywatnej domen DNS jako funkcja w wersji zapoznawczej.  
 
Usługa DNS platformy Azure zapewnia niezawodne, bezpieczne usługę DNS do zarządzania i rozpoznawanie nazw domeny w sieci wirtualnej bez konieczności dodawania niestandardowego rozwiązania DNS. Strefy DNS prywatne umożliwiają używanie własne nazwy domeny niestandardowej, a nie nazwy platformy Azure obecnie dostępne.  Przy użyciu niestandardowych nazw domen pomaga dostosować architektury sieci wirtualnej w zależności do potrzeb organizacji. Udostępnia rozpoznawanie nazw dla maszyn wirtualnych w ramach sieci wirtualnej i między sieciami wirtualnymi. Ponadto można skonfigurować nazwy stref z widokiem typu horizon podziału — umożliwiając prywatnych i publicznych strefę DNS o tej samej nazwie.

![Omówienie systemu DNS](./media/private-dns-overview/scenario.png)

[!INCLUDE [private-dns-preview-notice](../../includes/private-dns-preview-notice.md)]

## <a name="benefits"></a>Korzyści

* **Eliminuje to potrzebę niestandardowych rozwiązań DNS.** Wcześniej wielu klientów utworzyć niestandardowe DNS rozwiązania do zarządzania strefy DNS w sieci wirtualnej.  Teraz można zrobić zarządzania strefy DNS przy użyciu natywnej infrastruktury platformy Azure, co spowoduje usunięcie obciążeń tworzenia i zarządzania rozwiązania niestandardowe DNS.

* **Użyj wszystkie popularne typy rekordów DNS.**  Usługa DNS platformy Azure obsługuje rekordów A, AAAA, CNAME, MX, NS, PTR, SOA, SRV i TXT.

* **Zarządzanie rekordu automatyczne nazwy hosta.** Wraz z hosting niestandardowych rekordów DNS, Azure zostanie automatycznie Obsługa rekordów nazwy hosta dla maszyn wirtualnych w określonej sieci wirtualnych.  Dzięki temu można zoptymalizować nazwy domeny, które umożliwia bez konieczności tworzenia niestandardowych rozwiązań DNS lub modyfikowania aplikacji.

* **Rozpoznawanie nazwy hosta między sieciami wirtualnymi.** W przeciwieństwie do nazw hosta dostarczone do usługi Azure prywatne stref DNS może być udostępniane między sieciami wirtualnymi.  Ta funkcja upraszcza granic sieci i usług odnajdywania scenariuszy, takich jak sieci wirtualnej komunikacji równorzędnej.

* **Znanych narzędzi i środowisko użytkownika.** Aby skrócić czas nauki, to nową ofertę używa już ustalonym narzędzi usługi Azure DNS (środowiska PowerShell, interfejsu API REST usługi Resource Manager szablonów) i pomocy technicznej zostaną dodane do portalu i interfejsu wiersza polecenia tak szybko, jak to możliwe.

* **Obsługa w systemie DNS typu horizon podziału.** Usługa DNS platformy Azure umożliwia tworzenie stref o takiej samej nazwie, która rozwiązać do różnych odpowiedzi z sieci wirtualnej i z publicznej sieci Internet.  Typowy scenariusz w DNS typu horizon podziału jest zapewnienie dedykowanych wersji usługi do użytku w Twojej sieci wirtualnej.


## <a name="pricing"></a>Cennik

Strefy DNS prywatnego będzie bezpłatne zarządzanej wersji zapoznawczej. Na anonsu ogólnej dostępności ta funkcja użyje opartej na użyciu modelu cenowego podobne do istniejącej usługi Azure DNS oferty. 


## <a name="next-steps"></a>Następne kroki

Dowiedz się, jak [utworzyć strefę DNS prywatnej](./private-dns-getstarted-powershell.md) w usłudze Azure DNS.

Więcej informacji na temat stref DNS i rekordy odwiedzając: [DNS strefy i rejestruje omówienie](dns-zones-records.md).

Poznaj inne kluczowe [możliwości sieciowe](../networking/networking-overview.md) platformy Azure.

