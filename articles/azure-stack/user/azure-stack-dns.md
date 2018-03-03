---
title: DNS w stosie Azure | Dokumentacja firmy Microsoft
description: "Usługa DNS w usłudze Azure Stack"
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
ms.assetid: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/28/2018
ms.author: mabrigg
ms.openlocfilehash: 394abe5295af4ed99e48d50b5886ac93af87e875
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/02/2018
---
# <a name="dns-in-azure-stack"></a>Usługa DNS w usłudze Azure Stack

*Dotyczy: Azure stosu zintegrowanych systemów i Azure stosu Development Kit*

Stos Azure oferuje następujące funkcje DNS:
* Obsługa rozpoznawania nazwy hosta DNS
* Tworzenie i zarządzanie strefy DNS i rekordów przy użyciu interfejsu API

## <a name="support-for-dns-hostname-resolution"></a>Obsługa rozpoznawania nazwy hosta DNS
Można określić etykiety nazwy domeny DNS dla publicznego zasób adresu IP, który tworzy mapowanie dla *domainnamelabel.location*. cloudapp.azurestack.external publicznego adresu IP w stosie Azure zarządzanych serwerów DNS.  

Na przykład, jeśli utworzono zasób publicznego adresu IP z **contoso** jako etykieta nazwy domeny, w lokalizacji lokalnej stosu Azure, w pełni kwalifikowaną nazwę (FQDN) **contoso.local.cloudapp.azurestack.external**rozpoznawany jako publiczny adres IP zasobu. Można utworzyć domeny niestandardowej rekord CNAME, wskazujące publiczny adres IP w stosie Azure, można użyć tej nazwy FQDN.

> [!IMPORTANT]
> Każda etykieta nazwy domeny utworzony muszą być unikatowe w lokalizacji stosu Azure.

Jeśli utworzysz publiczny adres IP za pomocą portalu wygląda następująco:

![Utwórz publiczny adres IP](media/azure-stack-whats-new-dns/image01.png)

Ta konfiguracja jest użyteczna, jeśli chcesz skojarzyć z równoważeniem obciążenia zasobem publicznego adresu IP. Na przykład może mieć modułu równoważenia obciążenia przetwarzania żądań z aplikacji sieci web. Za obciążenia równoważenia jest witryną sieci web, znajduje się na co najmniej jednej maszyny wirtualnej. Teraz można dostęp do witryny sieci web równoważeniem obciążenia, nazwy DNS, a nie adres IP.

## <a name="create-and-manage-dns-zones-and-records-using-api"></a>Tworzenie i zarządzanie strefy DNS i rekordów przy użyciu interfejsu API
Można tworzyć i zarządzać strefy DNS i rekordy w stosie Azure.  

Stos Azure oferuje usługi DNS, takich jak Azure w, za pomocą interfejsów API, które są zgodne z interfejsami API usługi Azure DNS.  Hosting domeny w usłudze Azure DNS stosu, można zarządzać rekordami DNS z tymi samymi poświadczeniami, interfejsy API, narzędzia, rozliczeń i pomocy technicznej jako innymi usługami Azure. 

Oczywistym ze względu na infrastruktury usługi Azure DNS stosu jest mniejszych niż platformy Azure. W związku z tym zakresu, skalowalność i wydajność będzie zależeć od skali wdrożenia stosu Azure i środowisko, w których jest wdrożona.  Tak np. wydajności, dostępności, globalne dystrybucji i wysokiej dostępności (HA) może się różnić wdrożenia wdrożenia.

## <a name="comparison-with-azure-dns"></a>Porównanie z usługi Azure DNS
DNS w stosie Azure jest podobny do DNS na platformie Azure z dwóch głównych wyjątkami:
* **Nie obsługuje rekordów AAAA**

    Stos Azure nie obsługuje rekordów AAAA, ponieważ stos Azure nie obsługuje adresów IPv6.  Jest to Najważniejsza różnica między DNS na platformie Azure i stosu Azure.
* **Nie jest wieloma dzierżawcami**

    W przeciwieństwie do usługi Azure usługa DNS w stosie Azure nie jest wielodostępnej. Dlatego każdego dzierżawcy nie można utworzyć tej samej strefie DNS. Tylko pierwszy subskrypcji, która podejmuje próbę utworzenia strefie zakończy się pomyślnie, a kolejne żądania zakończyć się niepowodzeniem.  Jest to znany problem i Najważniejsza różnica między Azure i usługi Azure DNS stosu. Ten problem zostanie rozwiązany w przyszłej wersji.

Ponadto istnieją pewne niewielkie różnice w jak usługi Azure DNS stosu implementuje tagi, metadane elementy etag i limity.

Następujące informacje w szczególności dotyczy usługi Azure DNS stosu i różni się nieco w usłudze Azure DNS. Aby dowiedzieć się więcej o usłudze Azure DNS, zobacz [DNS strefy i rejestruje](../../dns/dns-zones-records.md) w witrynie Microsoft Azure w dokumentacji.

### <a name="tags-metadata-and-etags"></a>Tagi, metadane i elementy etag

**Tagi**

Usługa Azure DNS stosu obsługuje przy użyciu usługi Azure Resource Manager tagów zasobów strefy DNS. Nie obsługuje tagi zestawów rekordów DNS, mimo że jako alternatywę "metadanych" jest obsługiwana na zestawów rekordów DNS, jak wyjaśniono dalej.

**Metadata**

Alternatywą dla tagów zestawu rekordów DNS stosu Azure obsługuje dodawanie adnotacji do zestawów rekordów przy użyciu "metadanych". Podobnie jak tagi, metadanych umożliwia kojarzenie pary nazwa wartość z każdego zestawu rekordów. Na przykład może to być przydatne do rejestrowania przeznaczenia każdego zestawu rekordów. W przeciwieństwie do tagów metadanych nie może służyć do zapewnienia filtrowany widok rachunku Azure i nie można określić w zasadach usługi Azure Resource Manager.

**Elementy etag**

Załóżmy, że dwie osoby lub dwoma procesami spróbuj zmodyfikować rekord DNS, w tym samym czasie. Który wins? I będzie wygrał użytkownik wiadomo, że zostały one zastąpione zmiany utworzone przez innego użytkownika?

Usługa Azure DNS stosu korzysta elementy etag bezpiecznie obsłużyć równoczesnych zmian do tego samego zasobu. Elementy etag są niezależne od usługi Azure Resource Manager "Tagi". Każdy zasób DNS (strefy lub zestawu rekordów) ma element Etag skojarzone z nim. Zawsze, gdy zasób jest pobierana, również są pobierane jego Etag. Podczas aktualizowania zasobu, można przesłać tag Etag dzięki usłudze Azure DNS stosu można sprawdzić, czy tag Etag na dopasowań serwera. Ponieważ każda aktualizacja do zasobu powoduje Etag ponownie generowane, element etag wskazuje, że nastąpiła zmiana współbieżnych. Elementy etag można również podczas tworzenia nowego zasobu upewnij się, że zasób nie istnieje.

Domyślnie program PowerShell DNS stosu Azure korzysta elementy etag blokować równoczesnych zmian do stref i zestawy rekordów. Opcjonalny *-zastąpić* przełącznika mogą być używane do pomijania sprawdzania Etag, w tym przypadku wszystkie równoczesnych zmian zostaną zastąpione.

Na poziomie interfejsu API REST Azure stosu DNS zostały określone elementy etag korzystanie z nagłówków HTTP. W poniższej tabeli znajduje się ich zachowanie:

| Nagłówek | Zachowanie|
|--------|---------|
| None   | Umieść zawsze powiedzie się (nie są sprawdzane Etag)|
| IF-match| Umieść powiedzie się tylko, jeśli zasób istnieje i element Etag odpowiada|
| IF-match *| Umieść powiedzie się tylko, jeśli istnieje zasób|
| IF-none-match *| Umieść powiedzie się tylko, jeśli zasób nie istnieje.|

### <a name="limits"></a>Limity

Następujące domyślne limity stosowane podczas korzystania z usługi Azure DNS stosu:

| Zasób| Limit domyślny|
|---------|--------------|
| Strefy na subskrypcję| 100|
| Zestawy rekordów na strefy| 5000|
| Rejestruje dla danego zestawu rekordów| 20|

## <a name="next-steps"></a>Kolejne kroki
[Introducing Azure stosu międzynarodowych nazw domen.](azure-stack-understanding-dns.md)
