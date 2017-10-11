---
title: "Diagnozowanie błędów - Azure Logic Apps | Dokumentacja firmy Microsoft"
description: "Typowe sposoby zrozumieć, w którym aplikacje logiki kończą się niepowodzeniem"
services: logic-apps
documentationcenter: .net,nodejs,java
author: jeffhollan
manager: anneta
editor: 
ms.assetid: a6727ebd-39bd-4298-9e68-2ae98738576e
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 10/18/2016
ms.author: LADocs; jehollan
ms.openlocfilehash: 814e6f93088cdd96b0a663d2a7494b5a11470d99
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="diagnose-logic-app-failures"></a>Diagnozowanie błędów aplikacji logiki
Występują problemy lub awarie z aplikacji logiki, istnieje kilka metod pomaga lepiej zrozumieć, w których pochodzą z błędami.  

## <a name="azure-portal-tools"></a>Narzędzia do portalu Azure
Azure portal udostępnia wiele narzędzi do diagnozowania każdej aplikacji logiki w każdym kroku.

### <a name="trigger-history"></a>Historia wyzwalacza

Każda aplikacja logiki ma przynajmniej jeden wyzwalacz. Jeśli okaże się, że aplikacje nie są wyzwalania, przyjrzeć historii wyzwalacza, aby uzyskać więcej informacji. Jest dostępna Historia wyzwalacza w głównym bloku app'ss logiki.

![Lokalizowanie historii wyzwalacza][1]

Historia wyzwalacza zawiera listę wszystkich prób wyzwalacza wprowadzone przez aplikację logiki. Możesz kliknąć każdej próbie wyzwalacza informacji szczegółowych, w szczególności, wszystkie dane wejściowe lub danych wyjściowych wygenerowanych przy próbie wyzwalacza. Jeśli okaże się wyzwalaczy nie powiodło się, zaznacz próba wyzwalacza i wybierz polecenie **dane wyjściowe** łącze, aby wyświetlić wygenerowany komunikaty o błędach, na przykład dla poświadczeń FTP, które nie są prawidłowe.

Różne stany, które można napotkać są:

* **Pominięto**. Punkt końcowy został sondowane do sprawdzenia danych i Odebrano odpowiedź, że dane nie zostały dostępne.
* **Pomyślnie**. Wyzwalacz odebrał odpowiedź, aby dane były dostępne. Ten stan może wynikać z ręcznego wyzwalacza, wyzwalacz cyklu lub wyzwalacza sondowania. Ten stan jest zazwyczaj towarzyszy **Fired** stanu, ale może nie być, jeśli masz warunku lub polecenia SplitOn w widoku kodu, który nie został spełniony.
* **Nie powiodło się**. Wystąpił błąd.

#### <a name="start-a-trigger-manually"></a>Ręcznie uruchomić wyzwalacz

Sprawdź, czy wyzwalacz dostępne natychmiast bez oczekiwania na następne wystąpienie aplikacji logiki, kliknij przycisk **wybierz wyzwalacz** w głównym bloku, aby wymusić sprawdzenie. Na przykład kliknięcie tego łącza z wyzwalaczem skrzynki powoduje przepływu pracy natychmiast wykonać sondowanie skrzynki dla nowych plików.

### <a name="run-history"></a>Historia uruchomień

Każdy wypalane wyzwalacz powoduje do uruchomienia. Można uzyskać dostępu do informacji o uruchamianiu w głównym bloku zawierający wiele szczegółowe informacje, które mogą pomóc Ci zrozumieć, co się stało podczas przepływu pracy.

![Lokalizowanie Historia uruchomień][2]

Uruchom zawiera jedną z następujących stanów:

* **Pomyślnie**. Wszystkie akcje zakończyło się pomyślnie. Jeśli wystąpił błąd, aby awaria była obsługiwana przez akcję, który wystąpił w dalszej części przepływu pracy. Oznacza to błąd była obsługiwana przez akcję, która została ustawiona do uruchomienia po akcji nie powiodło się.
* **Nie powiodło się**. Co najmniej jedną akcję wystąpił błąd, który nie został obsłużony przez akcję później w przepływie pracy.
* **Anulowane**. Przepływ pracy był uruchomiony, ale Odebrano żądanie anulowania.
* **Uruchomiona**. Przepływ pracy jest uruchomiony. Ten stan może wystąpić dla przepływów pracy z ograniczeniem przepustowości lub z powodu bieżącego planu cenowego. Aby uzyskać więcej informacji, zobacz [limity akcji na stronie cen](https://azure.microsoft.com/pricing/details/app-service/plans/). Konfigurowanie diagnostyki (wykresy, które są wyświetlane w obszarze Historia uruchomień) również może zawierają informacje dotyczące zdarzeń ograniczania zachodzące.

Jeśli przeglądasz Historia uruchomień mogą przejść więcej szczegółów.  

#### <a name="trigger-outputs"></a>Dane wyjściowe wyzwalacza

Dane wyjściowe wyzwalacza Pokaż dane, które nadeszły z wyzwalacza. Te dane wyjściowe może pomóc w określeniu, czy wszystkie właściwości zwracane zgodnie z oczekiwaniami.

> [!NOTE]
> Jeśli widzisz zawartość, która nie rozumie, Dowiedz się, jak Azure Logic Apps [obsługuje różne typy zawartości](../logic-apps/logic-apps-content-type.md).
> 

![Przykładowe dane wyjściowe wyzwalacza][3]

#### <a name="action-inputs-and-outputs"></a>Akcja wejścia i wyjścia

Aby przejść do szczegółów w wejściach i wyjściach, które otrzymały akcji. Dane te są przydatne, aby zrozumieć wielkość i kształt dane wyjściowe, a także do znajdowania komunikaty o błędach, które mogą zostać wygenerowane.

![Akcja wejścia i wyjścia][4]

## <a name="debug-workflow-runtime"></a>Debugowanie przepływu pracy

Wraz z monitorowania wejść, wyjść i wyzwalaczy Uruchom, niektóre kroki można dodać do przepływu pracy, które pomagają z debugowaniem. 
[RequestBin](http://requestb.in) jest zaawansowanym narzędziem, które można dodać krokiem w przepływie pracy. Przy użyciu RequestBin, można skonfigurować inspektora żądania HTTP do określenia dokładnej rozmiar, kształt i format żądania HTTP. Można tworzyć RequestBin i wklej adres URL w aplikacji logiki akcji HTTP POST z zawartość treści, która ma zostać przetestowana, na przykład, wyrażenie lub inny krok danych wyjściowych. Po uruchomieniu aplikacji logiki możesz odświeżyć Twojej RequestBin, aby zobaczyć, jak żądania został utworzony podczas generowania z aparatu Logic Apps.

<!-- image references -->
[1]: ./media/logic-apps-diagnosing-failures/triggerhistory.png
[2]: ./media/logic-apps-diagnosing-failures/runhistory.png
[3]: ./media/logic-apps-diagnosing-failures/triggeroutputslink.png
[4]: ./media/logic-apps-diagnosing-failures/actionoutputs.png
