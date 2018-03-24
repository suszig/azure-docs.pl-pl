---
title: "Menedżer stanu niezawodny sieci szkieletowej usług Azure i niezawodne kolekcji wewnętrzne | Dokumentacja firmy Microsoft"
description: "Nowości dotyczące pojęć niezawodnej kolekcji i Projekt w sieci szkieletowej usług Azure."
services: service-fabric
documentationcenter: .net
author: mcoskun
manager: timlt
editor: rajak
ms.assetid: 62857523-604b-434e-bd1c-2141ea4b00d1
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 5/1/2017
ms.author: mcoskun
ms.openlocfilehash: d607449a16e886337ab1bd96213fbb4231124353
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-service-fabric-reliable-state-manager-and-reliable-collection-internals"></a>Menedżer stanu niezawodny sieci szkieletowej usług Azure i niezawodne kolekcji wewnętrzne
Ten dokument delves wewnątrz niezawodnej Menedżer stanu i niezawodne kolekcji, aby zobaczyć, jak podstawowe składniki działają w tle.

> [!NOTE]
> Ten dokument jest w toku. Dodawanie komentarzy do tego artykułu, aby poinstruować NAS chcesz dowiedzieć się więcej o tematu.
>

##  <a name="local-persistence-model-log-and-checkpoint"></a>Model trwałości lokalnego: dziennika i punktu kontrolnego
Menedżer niezawodnej stanu i niezawodne kolekcji wykonaj modelu trwałości, który jest nazywany dziennika i punktu kontrolnego.
W tym modelu każdej zmiany stanu jest rejestrowane jako pierwsze na dysku, a następnie stosowane w pamięci.
Stan ukończenia, sam jest trwały sporadycznie () Punkt kontrolny).
Korzyścią jest to, że wystąpiły są przekształcane w sekwencyjnych zapisów tylko Dołącz na dysku dla lepszą wydajność.

Aby lepiej zrozumieć modelu dziennika oraz punkt kontrolny, najpierw Przyjrzyjmy się w scenariuszu nieskończone dysku.
Niezawodne Menedżer stanu rejestruje każdej operacji przed ich replikacji.
Rejestrowanie umożliwia niezawodnej kolekcję do zastosowania operacji tylko w pamięci.
Ponieważ dzienniki są trwałe, nawet wtedy, gdy replika nie powiedzie się i konieczne jest ponowne uruchomienie, niezawodne Menedżer stanu ma za mało informacji w jego dzienniku powtarzania wszystkie operacje, który utracił repliki.
Ponieważ dysk jest nieograniczony, rekordów dziennika nigdy nie muszą zostać usunięte i niezawodnej kolekcji musi zarządzać tylko stan w pamięci.

Teraz Przyjrzyjmy się w scenariuszu skończoną dysku.
Jak accumulate rekordów dziennika, niezawodne Menedżer stanu uruchomić mało miejsca na dysku.
Zanim tak się stanie, niezawodne Menedżer stanu musi obcinania dziennika, aby zwolnić miejsce dla nowszej rekordów.
Menedżer stanu niezawodny żądań niezawodnej kolekcje do punktu kontrolnego ich stan w pamięci na dysk.
W tym momencie w kolekcjach niezawodnej czy utrwalić stanu w pamięci.
Po niezawodnej kolekcje ukończyć ich punkty kontrolne, niezawodne menedżera stanu można obciąć dziennika, aby zwolnić miejsce na dysku.
Gdy replika musi zostać uruchomiony ponownie, niezawodne kolekcje odzyskać ich użyciu stan i niezawodne Menedżer stanu odzyskuje i odtwarza wszystkie zmiany stanu, które od czasu ostatniego punktu kontrolnego.

Dodaj inną wartość z procesu tworzenia punktów kontrolnych jest zwiększa czasy odzyskiwania w typowych scenariuszy. Dziennik zawiera wszystkie operacje, które miały miejsce od czasu ostatniego punktu kontrolnego.
Dlatego może zawierać wiele wersji elementu, takich jak wiele wartości dla danego wiersza w słowniku niezawodne.
Z drugiej strony, punkty kontrolne niezawodnej kolekcji tylko najnowszą wersję każdej wartości dla klucza.

## <a name="next-steps"></a>Następne kroki
* [Transakcje i blokad](service-fabric-reliable-services-reliable-collections-transactions-locks.md)

