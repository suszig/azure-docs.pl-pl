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
ms.date: 11/11/2017
ms.author: LADocs; klam
ms.openlocfilehash: 49a66c826a98f7160b97b516e6fd541795ae3b0e
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/19/2018
---
# <a name="logic-apps-pricing-model"></a>Model cen aplikacji logiki
Można utworzyć i uruchomić zautomatyzowanych skalowalnej integracji przepływów pracy w chmurze za pomocą usługi Azure Logic Apps. Poniżej przedstawiono szczegółowe informacje o rozliczeniach i cenach dla usługi Logic Apps.
## <a name="consumption-pricing-model"></a>Użycie modelu cenowego
Dzięki aplikacjom logiki nowo utworzony płaci się tylko w przypadku należy użyć. Nowe aplikacje logiki używają zużycie planu cennik modelu, co oznacza, że wszystkie wykonaniami wykonywane przez wystąpienie aplikacji logiki są naliczane.
### <a name="what-are-action-executions"></a>Co to są wykonania akcji?
Każdy krok w definicji aplikacji logiki jest akcji, która zawiera wyzwalacze, kroki przepływu sterowania wywołania akcje wbudowane i wywołania łączników.
### <a name="triggers"></a>Wyzwalacze
Wyzwalacze są specjalne działania, które Utwórz wystąpienie aplikacji logiki w przypadku określonego zdarzenia. Wyzwalacze ma kilka różne zachowania, które mają wpływ na sposób taryfowych aplikacji logiki.
* **Wyzwalacz sondowania** — tego wyzwalacza stale sprawdza punkt końcowy, aż otrzyma komunikat który spełnia kryteria tworzenia wystąpienia aplikacji logiki, aby uruchomić przepływ pracy. Można skonfigurować interwał sondowania w wyzwalaczu poprzez projektanta aplikacji logiki. Każde żądanie sondowania jest traktowana jako wykonywania nawet wtedy, gdy jest tworzone żadne wystąpienie aplikacji logiki.
* **Wyzwalacz Webhook** — tego wyzwalacza czeka na klienta wysłać żądania do określonego punktu końcowego. Zlicza każde żądanie wysłane do punktu końcowego elementu webhook jak wykonanie akcji. Na przykład wyzwalacz żądania i HTTP elementu Webhook są obie wyzwalaczy elementu webhook.
* **Wyzwalacz cyklu** — tego wyzwalacza tworzy wystąpienie aplikacji logiki oparte na interwał cyklu, skonfigurowanego w wyzwalaczu. Na przykład można ustawić górę wyzwalacz cyklu co trzy dni lub bardziej złożonych zgodnie z harmonogramem.

Wyzwalacz wykonaniami można znaleźć w okienku przeglądu aplikację logiki w sekcji historii wyzwalacza.

### <a name="actions"></a>Akcje
Akcje wbudowane, na przykład akcje, które wywołują HTTP, funkcje platformy Azure lub interfejsu API zarządzania, a także kroki przepływu sterowania, są mierzone jako akcje macierzystego i ich odpowiednich typów. Akcje, które wywołują [łączniki](https://docs.microsoft.com/connectors) ma typ "ApiConnection". Łączniki są sklasyfikowane jako standard lub enterprise łączników i są mierzone w odpowiednich [cennik][pricing].
Wszystkie akcje pomyślnie i niepomyślnie wykonywania są zliczane i mierzone jako wykonania akcji. Jednak akcje, które są pomijane, ze względu na warunki unmet lub akcje, które nie działają, ponieważ aplikacji logiki przerwane przed ukończeniem, nie są liczone jak wykonania akcji. Aplikacje logiki wyłączone nie można utworzyć wystąpienia nowych wystąpień, więc ich nie są naliczane opłaty, gdy są one wyłączone.

> [!NOTE]
> Po wyłączeniu aplikacji logiki jego uruchomione wystąpienia może zająć nieco czasu przed całkowicie zatrzymać.

Akcje, które są uruchamiane wewnątrz pętli są liczone każdy cykl w pętli. Na przykład jednej akcji w pętli "for each", który przetwarza listę 10 elementu jest liczony mnożąc liczbę elementów listy (10) przez liczbę akcji w pętli (1) plus jeden uruchamiania pętli. Tak, na przykład obliczenie jest (10 * 1) + 1, co prowadzi do 11 wykonania akcji.

### <a name="integration-account-usage"></a>Użycie konta integracji
Na podstawie zużycia użycia obejmuje [konta integracji](logic-apps-enterprise-integration-create-integration-account.md) gdzie można eksplorować, opracowanie i przetestowanie [B2B/EDI](logic-apps-enterprise-integration-b2b.md) i [przetwarzania XML](logic-apps-enterprise-integration-xml.md) funkcji Logic Apps bez dodatkowych kosztów. Może mieć jedno z tych kont integracji według regionu i magazynu maksymalnie 10 umów oraz 25 mapy. Możesz mieć i przekaż nieograniczone partnerów, schematy i certyfikaty.

Logic Apps oferuje również konta integracji basic i standard z obsługiwanych SLA aplikacji logiki. Możesz użyć kont podstawową integrację, gdy użytkownik ma być używany tylko obsługa komunikatów lub działa jako partner małych firm, która ma relację handlowym partnera z większą jednostki biznesowe. Konta standardowe integracji obsługuje bardziej złożonych relacji B2B i zwiększyć liczbę obiektów, którymi można zarządzać. Aby uzyskać więcej informacji, zobacz [cennik platformy Azure](https://azure.microsoft.com/pricing/details/logic-apps).

## <a name="pricing"></a>Cennik
Aby uzyskać więcej informacji, zobacz [cennik aplikacje logiki](https://azure.microsoft.com/pricing/details/logic-apps).

## <a name="next-steps"></a>Kolejne kroki
* [Omówienie usługi Logic Apps][whatis]
* [Tworzenie pierwszej aplikacji logiki][create]

[pricing]: https://azure.microsoft.com/pricing/details/logic-apps/
[whatis]: logic-apps-overview.md
[create]: quickstart-create-first-logic-app-workflow.md

