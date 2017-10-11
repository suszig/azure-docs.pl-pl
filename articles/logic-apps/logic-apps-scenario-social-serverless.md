---
title: "Scenariusz — Utwórz pulpit nawigacyjny szczegółowych informacji klienta z Azure Niekorzystającą | Dokumentacja firmy Microsoft"
description: "Przykład sposobu można tworzyć pulpit nawigacyjny do zarządzania opinie klientów, dane społecznościowe i inne aplikacje logiki platformy Azure i usługi Azure Functions."
keywords: 
services: logic-apps
author: jeffhollan
manager: anneta
editor: 
documentationcenter: 
ms.assetid: d565873c-6b1b-4057-9250-cf81a96180ae
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/29/2017
ms.author: jehollan
ms.openlocfilehash: 0b6e118cb13ab8185d8eeb42bec6147155967967
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-real-time-customer-insights-dashboard-with-azure-logic-apps-and-azure-functions"></a>Utwórz pulpit nawigacyjny szczegółowych informacji w czasie rzeczywistym klienta z usługi Azure Logic Apps i usługi Azure Functions

Narzędzia Niekorzystającą Azure zapewniają wydajnego szybkie tworzenie i hostowanie aplikacji w chmurze, bez konieczności pomyśleć o infrastruktury.  W tym scenariuszu zostanie utworzony pulpit nawigacyjny do wyzwolenia na opinie klientów, analizowanie opinii z uczenia maszynowego i insights Opublikuj źródła, takich jak usługi Power BI lub usługi Azure Data Lake.

## <a name="overview-of-the-scenario-and-tools-used"></a>Omówienie scenariusza i narzędzia

Aby zaimplementować to rozwiązanie, firma Microsoft będzie korzystać dwie najważniejsze składniki niekorzystającą aplikacji na platformie Azure: [usługi Azure Functions](https://azure.microsoft.com/services/functions/) i [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/).

Logic Apps jest aparatem niekorzystającą przepływu pracy w chmurze.  Udostępnia aranżacji między składnikami niekorzystającą, a także łączy się przez ponad 100 usług i interfejsów API.  W tym scenariuszu utworzymy aplikację logiki, aby wyzwolić na opinie klientów.  Łączniki, które mogą pomóc w reakcji na opinie klientów między innymi Outlook.com, usługi Office 365 małp ankiety, Twitter i żądania HTTP [za pomocą formularza sieci web](https://blogs.msdn.microsoft.com/logicapps/2017/01/30/calling-a-logic-app-from-an-html-form/).  Poniżej przepływu pracy firma Microsoft będzie monitorował hasztagiem w serwisie Twitter.

Funkcje umożliwiać niekorzystającą obliczeniowych w chmurze.  W tym scenariuszu użyjemy usługi Azure Functions do flagi tweetów z klientów w oparciu o szereg wstępnie zdefiniowanych słów kluczowych.

Całe rozwiązanie może być [kompilacji w programie Visual Studio](logic-apps-deploy-from-vs.md) i [wdrożenia w ramach szablonu zasobów](logic-apps-create-deploy-template.md).  Istnieje również przewodnik wideo scenariusza [witrynie Channel 9](http://aka.ms/logicappsdemo).

## <a name="build-the-logic-app-to-trigger-on-customer-data"></a>Tworzenie aplikacji logiki, aby wyzwolić na dane klienta

Po [tworzenie aplikacji logiki](logic-apps-create-a-logic-app.md) w programie Visual Studio lub w portalu Azure:

1. Dodaj wyzwalacz **na nowych Tweetów** z serwisem Twitter
2. Skonfiguruj wyzwalacza słuchać tweetów — słowo kluczowe lub hasztagiem.

   > [!NOTE]
   > Właściwość cyklu w wyzwalaczu określają częstotliwość aplikacji logiki sprawdza dostępność nowych elementów na podstawie sondowania wyzwalacze

   ![Przykład wyzwalacza Twitter][1]

Ta aplikacja będzie teraz Wyzwól na wszystkich nowych tweetów.  Następnie możemy pobrania tych danych tweet i dowiedzieć się więcej o wskaźniki nastrojów klientów wyrażone.  W tym używamy [kognitywnych usługę Azure](https://azure.microsoft.com/services/cognitive-services/) wykryć wskaźniki nastrojów klientów tekstu.

1. Kliknij przycisk **nowy krok**
1. Wybierz lub Wyszukaj **Analiza tekstu** łącznika
1. Wybierz **wykrywa wskaźniki nastrojów klientów** operacji
1. Po wyświetleniu monitu podaj prawidłowy klucz kognitywnych usług dla usługi Analiza tekstu
1. Dodaj **tekst Tweet** jako tekst do analizy.

Teraz, gdy mamy tweet dane i informacje na tweet wiele innych łączników mogą być istotne:
* Power BI — Dodawanie wierszy do przesyłania strumieniowego zestawu danych: Wyświetl tweetów w czasie rzeczywistym na pulpicie nawigacyjnym usługi Power BI.
* Azure Data Lake — Dołącz plik: Dodaj do zestawu danych usługi Azure Data Lake do uwzględnienia w zadania usługi analiza danych klienta.
* SQL — Dodawanie wierszy: przechowywanie danych w bazie danych do nowszej pobierania.
* Slack — wysyłanie wiadomości: Alert kanału slack na negatywnej opinii, która wymaga działania.

Funkcję platformy Azure można również wykonać więcej niestandardowego obliczenia na podstawie danych.

## <a name="enriching-the-data-with-an-azure-function"></a>Wzbogacenie danych przy użyciu funkcji platformy Azure

Zanim można utworzyć funkcji, musimy mieć aplikacji funkcji w naszym subskrypcji platformy Azure.  Szczegółowe informacje na temat tworzenia funkcji platformy Azure w portalu można [można znaleźć tutaj](../azure-functions/functions-create-first-azure-function-azure-portal.md)

Dla funkcję, która ma być wywoływana bezpośrednio z aplikacji logiki, musi mieć HTTP wyzwolenia powiązania.  Firma Microsoft zaleca używanie **HttpTrigger** szablonu.

W tym scenariuszu treść żądania funkcji platformy Azure będzie tweet tekstu.  Kod funkcji wystarczy zdefiniować logiki Jeśli tekst tweet zawiera słowo kluczowe lub frazę.  Ta funkcja może znajdować się proste lub złożone, zgodnie z potrzebami dla tego scenariusza.

Na końcu funkcję po prostu zwraca odpowiedź do aplikacji logiki z niektórych danych.  Może to być wartość logiczną proste (np. `containsKeyword`), lub obiekt złożony.

![Skonfigurowany krok funkcji platformy Azure][2]

> [!TIP]
> Podczas uzyskiwania dostępu do złożonych odpowiedzi przez funkcję aplikacji logiki, użyj akcji przeanalizować JSON.

Po zapisaniu funkcji, można dodać do aplikacji logiki utworzone powyżej.  W aplikacji logiki:

1. Kliknij, aby dodać **nowy krok**
1. Wybierz **usługi Azure Functions** łącznika
1. Zaznacz, aby wybrać istniejące funkcję i przejdź do funkcji utworzone
1. Wysyłanie w **Tweetować tekst** dla **treść żądania**

## <a name="running-and-monitoring-the-solution"></a>Uruchamiania i monitorowania rozwiązania

Jedną z zalet tworzenia niekorzystającą orchestrations w aplikacjach logiki jest sformatowany debugowania i możliwości monitorowania.  Wszelkie Uruchom (bieżącą lub historyczny) mogą być wyświetlane z poziomu programu Visual Studio, portalu Azure lub za pośrednictwem interfejsu API REST i zestawy SDK.

Jednym ze sposobów najłatwiejsza do testowania aplikacji logiki przy użyciu **Uruchom** przycisk w projektancie.  Kliknięcie przycisku **Uruchom** będzie sondować wyzwalacza co 5 sekund, dopóki nie zostanie wykryte zdarzenie i nadaj widoku aktywnego w trakcie wykonywania.

Poprzednie historii wykonywania można wyświetlić w bloku omówienie w portalu Azure lub za pomocą programu Visual Studio Cloud Explorer.

## <a name="creating-a-deployment-template-for-automated-deployments"></a>Tworzenie szablonu wdrożenia dla zautomatyzowanych wdrożeń

Po rozwiązaniu został opracowany, można przechwycić i wdrażane za pomocą szablonu wdrożenia usługi Azure do dowolnego regionu Azure na świecie.  Jest to przydatne w przypadku obu parametrów modyfikujących dla różnych wersji tego przepływu pracy, ale również do integracji to rozwiązanie w potoku kompilacji i wydania.  Szczegóły dotyczące tworzenia szablonu wdrożenia można znaleźć [w tym artykule](logic-apps-create-deploy-template.md).

Środowisko Azure Functions można również włączona Szablon wdrożenia — tak całego rozwiązania z wszystkie zależności mogą być zarządzane jako jednego szablonu.  Przykład wdrożenia szablonu funkcji można znaleźć w [repozytorium szablonów Szybki Start Azure](https://github.com/Azure/azure-quickstart-templates/tree/master/101-function-app-create-dynamic).

## <a name="next-steps"></a>Następne kroki

* [Zobacz inne przykłady i scenariusze dla usługi Azure Logic Apps](logic-apps-examples-and-scenarios.md)
* [Obejrzyj wideo wskazówki na temat tworzenia tego rozwiązania end-to-end](http://aka.ms/logicappsdemo)
* [Informacje o sposobie obsługi i przechwytywanie wyjątków w aplikacji logiki](logic-apps-exception-handling.md)

<!-- Image References -->
[1]: ./media/logic-apps-scenario-social-serverless/twitter.png
[2]: ./media/logic-apps-scenario-social-serverless/function.png