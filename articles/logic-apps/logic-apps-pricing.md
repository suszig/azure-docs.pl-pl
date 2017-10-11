---
title: "Cennik i rozliczenia — usługi Azure Logic Apps | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak działa cennik i rozliczenia dla usługi Azure Logic Apps."
author: kevinlam1
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: f8f528f5-51c5-4006-b571-54ef74532f32
ms.service: logic-apps
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: LADocs; klam
ms.openlocfilehash: 63784c5e3af360b2f3f8cb330a9df8b27a85d859
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="logic-apps-pricing-model"></a>Model cen aplikacji logiki
Aplikacje logiki platformy Azure pozwala na skalowanie i wykonywać integracji przepływy pracy w chmurze.  Poniżej przedstawiono więcej informacji dotyczących rozliczeń i planów cenowych dla usługi Logic Apps.
## <a name="consumption-pricing"></a>Cennik zużycie
Nowo utworzona Logic Apps użyć planu zużycia. Z modelu cenowego zużycie Logic Apps płacisz tylko za można użyć.  Aplikacje logiki nie są ograniczane, korzystając z planu zużycia.
Wszystkie akcje wykonywane podczas uruchomienia wystąpienia aplikacji logiki są naliczane.
### <a name="what-are-action-executions"></a>Co to są wykonania akcji?
Każdy krok w definicji aplikacji logiki akcji, która zawiera wyzwalacze, przepływ sterowania kroki tak, czy warunków, zakresów dla każdej pętli do pętli, wywołania łączników i wywołań natywnych akcje.
Wyzwalacze są specjalne działania, które są przeznaczone do utworzenia wystąpienia nowe wystąpienie aplikacji logiki, po wystąpieniu określonego zdarzenia.  Istnieje kilka różnych zachowań wyzwalaczy, które mogą wpłynąć na sposób taryfowych aplikacji logiki.
* **Wyzwalacz sondowania** — tego wyzwalacza stale sonduje punkt końcowy, dopóki nie odbierze komunikatu, który spełnia kryteria dotyczące tworzenia wystąpienia aplikacji logiki.  Interwał sondowania można skonfigurować w wyzwalaczu w Projektancie aplikacji logiki.  Każde żądanie sondowania, nawet jeśli nie tworzy wystąpienie aplikacji logiki, traktowana jako wykonywania akcji.
* **Wyzwalacz Webhook** — oczekiwania klienta do wysyłania żądania w określonym punkcie końcowym tego wyzwalacza.  Zlicza każde żądanie wysłane do punktu końcowego elementu webhook jak wykonanie akcji. Żądanie i wyzwalacza HTTP elementu Webhook są obie wyzwalaczy elementu webhook.
* **Wyzwalacz cyklu** — tego wyzwalacza tworzy wystąpienie aplikacji logiki oparte na interwał cyklu skonfigurowane w wyzwalaczu.  Na przykład wyzwalacz cyklu można skonfigurować do uruchamiania co trzy dni lub nawet co minutę.

Wyzwalacz wykonaniami są widoczne w bloku zasobów aplikacji logiki w części historii wyzwalacza.

Wszystkie akcje, które zostały wykonane, czy zostały one powiodły się czy nie są mierzone jako wykonywania akcji.  Akcje, które zostały pominięte z powodu nie został spełniony warunek i akcje, które nie wykonać operacji, ponieważ aplikacji logiki przerwane przed ukończeniem, nie są liczone jako wykonania akcji.

Akcje wykonywane w pętli są uwzględniane na iteracji pętli.  Na przykład jednej akcji w dla każdej pętli iteracja listę 10 elementów są liczone jako liczba elementów na liście (10) pomnożona przez liczbę akcji w pętli (1) i jeden dla inicjacji pętli , które w tym przykładzie będzie (10 * 1) + 1 = 11 wykonania akcji.
Wyłączone Logic Apps nie może mieć nowych wystąpień utworzonych i w związku z tym podczas wyłączony, nie pobiera.  Należy zachować ostrożność, po wyłączeniu aplikacji logiki może zająć nieco czasu dla wystąpień w stan spoczynku przed całkowicie wyłączana.
### <a name="integration-account-usage"></a>Użycie konta integracji
Uwzględnione w podstawie zużycia użycia jest [konta integracji](logic-apps-enterprise-integration-create-integration-account.md) do badań, projektowania i testowania, co umożliwia używanie [B2B/EDI](logic-apps-enterprise-integration-b2b.md) i [przetwarzania XML](logic-apps-enterprise-integration-xml.md)funkcji Logic Apps bez ponoszenia dodatkowych kosztów. Jesteś w stanie utworzyć maksymalnie jedno konto według regionu i magazynu maksymalnie 10 umów oraz 25 mapy. Schematy, certyfikaty i partnerów mają Brak ograniczeń i możesz przekazać tyle, zgodnie z potrzebami.

Oprócz włączenia konta integracji zużycie można także tworzyć konta standardowe integracji bez tych ograniczeń i z naszego standardowego SLA aplikacji logiki. Aby uzyskać więcej informacji, zobacz [cennik platformy Azure](https://azure.microsoft.com/pricing/details/logic-apps).

## <a name="app-service-plans"></a>Plany usługi App Service
Aplikacje logiki wcześniej utworzony odwołujące się do planu usługi App Service w dalszym ciągu zachowywać się jak poprzednio. W zależności od wybranego planu są ograniczane po wyznaczonych wykonaniami codzienne przekroczona, ale są rozliczane za pomocą licznika wykonywania akcji.
Umowa EA używających planu usługi App Service w ich subskrypcję, która ma zostać jawnie skojarzony z aplikacji logiki, skorzystać ilości uwzględnione.  Na przykład jeśli masz standardowy Plan usługi App Service w subskrypcji EA i aplikacji logiki w tej samej subskrypcji następnie możesz nie są naliczane opłaty za 10 000 wykonania akcji na dzień (patrz tabela poniżej). 

Plany usługi App Service i ich codzienne wykonaniami dozwolonych akcji:
|  | Zwolnij/udostępnione/Basic | Standardowa | Premium |
| --- | --- | --- | --- |
| Wykonania akcji na dzień |200 |10 000 |50,000 |
### <a name="convert-from-app-service-plan-pricing-to-consumption"></a>Konwertowanie z planu usługi App Service cennik zużycie
Aby zmienić aplikację logiki, która ma Plan usługi aplikacji skojarzonych z nim modelu zużycia, Usuń odwołanie do planu usługi App Service w definicji aplikacji logiki.  Ta zmiana może odbywać się przy użyciu wywołania do polecenia cmdlet programu PowerShell:`Set-AzureRmLogicApp -ResourceGroupName ‘rgname’ -Name ‘wfname’ –UseConsumptionModel -Force`
## <a name="pricing"></a>Cennik
Aby uzyskać szczegółowe informacje o cenach, zobacz [cennik aplikacje logiki](https://azure.microsoft.com/pricing/details/logic-apps).

## <a name="next-steps"></a>Następne kroki
* [Omówienie usługi Logic Apps][whatis]
* [Tworzenie pierwszej aplikacji logiki][create]

[pricing]: https://azure.microsoft.com/pricing/details/logic-apps/
[whatis]: logic-apps-what-are-logic-apps.md
[create]: logic-apps-create-a-logic-app.md

