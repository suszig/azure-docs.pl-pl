---
title: "Wprowadzenie do następnego przeskoku obserwatora sieciowego Azure | Dokumentacja firmy Microsoft"
description: "Ta strona zawiera omówienie obserwatora sieciowego następnego przeskoku możliwości"
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: 
ms.assetid: febf7bca-e0b7-41d5-838f-a5a40ebc5aac
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.openlocfilehash: bb2ca0486b3b3d27a77b70927cb3cbfbeac12c7c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="introduction-to-next-hop-in-azure-network-watcher"></a>Wprowadzenie do następnego przeskoku obserwatora sieciowego Azure

Ruch z maszyny Wirtualnej są wysyłane do lokalizacji docelowej, na podstawie tras skuteczne skojarzony z jedną kartą sieciową. Następny przeskok pobiera typ następnego przeskoku i adres IP pakietu z określonej maszyny wirtualnej i karty sieciowej. Dzięki temu do określenia, czy pakiet jest być kierowany do lokalizacji docelowej i jest holed ruchu jest czarny. Niepoprawna konfiguracja tras przez użytkownika, których ruch jest kierowany do lokalizacji lokalnej lub urządzenie wirtualne, może prowadzić do problemów z łącznością. Następny przeskok zwraca również wartość tabeli tras skojarzone z następnego przeskoku. Podczas wykonywania zapytania następnego przeskoku, jeśli trasa jest zdefiniowana jako trasy zdefiniowane przez użytkownika, zostanie zwrócony tej trasy. W przeciwnym razie wartość następnego skoku zwraca "Trasa systemowa".

![następnego przeskoku — omówienie][1]

Poniżej znajduje się lista typów następnego przeskoku, które mogą być zwrócone podczas wykonywania zapytania w następnym przeskoku.

* Internet
* VirtualAppliance
* VirtualNetworkGateway
* VnetLocal
* HyperNetGateway
* VnetPeering
* Brak

### <a name="next-steps"></a>Następne kroki

Dowiedz się, jak znaleźć problemy z połączeniem sieciowym, odwiedzając za pomocą następnego przeskoku [Sprawdź następnego przeskoku na maszynie Wirtualnej](network-watcher-check-next-hop-portal.md)

<!--Image references-->
[1]: ./media/network-watcher-next-hop-overview/figure1.png













