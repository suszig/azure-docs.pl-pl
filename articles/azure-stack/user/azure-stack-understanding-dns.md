---
title: Opis systemu DNS w stosie Azure | Dokumentacja firmy Microsoft
description: "Opis DNS funkcje i możliwości w stosie Azure"
services: azure-stack
documentationcenter: 
author: ScottNapolitan
manager: darmour
editor: 
ms.assetid: 60f5ac85-be19-49ac-a7c1-f290d682b5de
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 9/25/2017
ms.author: scottnap
ms.openlocfilehash: 8c023eda179ace41a082bf4a4fadc281c14db7ba
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="introducing-idns-for-azure-stack"></a>Introducing Azure stosu międzynarodowych nazw domen.

*Dotyczy: Azure stosu zintegrowanych systemów i Azure stosu Development Kit*

międzynarodowych nazw domen jest funkcją w stosie Azure, która pozwala na rozpoznawanie nazw DNS zewnętrznych (na przykład http://www.bing.com).
Można też zarejestrować nazwy wewnętrznej sieci wirtualnej. W ten sposób można rozwiązać maszyn wirtualnych w tej samej sieci wirtualnej przez nazwę, a nie adres IP bez konieczności podawania niestandardowych wpisów serwera DNS.

Jest to element, który zawsze było dostępne w usłudze Azure, ale jest dostępny w systemie Windows Server 2016 i stosu Azure zbyt.

## <a name="what-does-idns-do"></a>Do czego służy międzynarodowych nazw domen.
Międzynarodowych nazw domen w stosie Azure możesz uzyskać następujące funkcje bez konieczności określania niestandardowych wpisów serwera DNS.

* Udostępnione usługi rozpoznawania nazw DNS dla obciążeń dzierżawców.
* Autorytatywny usługi DNS dla rozpoznawania nazw i rejestrację DNS w sieci wirtualnej dzierżawcy.
* Usługa DNS cykliczne do rozpoznawania nazw internetowych dzierżawy maszyn wirtualnych. Dzierżawcy nie trzeba określić niestandardowe wpisy DNS do rozpoznawania nazw internetowych (na przykład www.bing.com).

Można nadal Ustaw własne DNS i korzystać z niestandardowych serwerów DNS. Ale teraz, jeśli właśnie chcesz mieć możliwość rozpoznania nazwy DNS w Internecie i można nawiązać połączenia z innych maszyn wirtualnych w tej samej sieci wirtualnej, nie trzeba niczego Określ i po prostu będzie działać.

## <a name="what-does-idns-not-do"></a>Do czego międzynarodowych nazw domen nie?
Jakie międzynarodowych nazw domen nie pozwala przeprowadzić jest tworzenie rekordu DNS dla nazwy, która może zostać rozwiązany z spoza sieci wirtualnej.

Na platformie Azure masz możliwość określenia etykieta nazwy DNS, który może być skojarzony z publicznym adresem IP. Możesz wybrać etykietę (prefiks), ale Azure wybiera sufiks, opartym na regionie, w którym można utworzyć publiczny adres IP.

![Etykieta nazwy DNS zrzut ekranu](media/azure-stack-understanding-dns-in-tp2/image3.png)

Na ilustracji powyżej Azure utworzy rekord "A" w systemie DNS dla etykieta nazwy DNS w strefie **westus.cloudapp.azure.com**. Prefiksu i sufiksu razem tworzą pełni kwalifikowanej domeny nazwę (FQDN), który można rozwiązać ten problem z dowolnego miejsca w publicznej sieci Internet.

Stos Azure obsługuje tylko międzynarodowych nazw domen rejestracji nazw wewnętrznych, więc nie można wykonać następujące czynności.

* Utwórz rekord DNS w istniejącej hostowanej strefy DNS (na przykład local.azurestack.external).
* Tworzenie strefy DNS (np. Contoso.com).
* Utwórz rekord w obszarze własne niestandardowe strefy DNS.
* Obsługuje zakupów nazw domen.

