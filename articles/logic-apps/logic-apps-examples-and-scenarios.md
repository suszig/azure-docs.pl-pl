---
title: "Przykłady & typowych scenariuszy - Azure Logic Apps | Dokumentacja firmy Microsoft"
description: "Dowiedz się więcej o aplikacjach logiki z przykładami, scenariusze samouczki i wskazówki"
services: logic-apps
author: jeffhollan
manager: anneta
editor: 
documentationcenter: 
ms.assetid: e06311bc-29eb-49df-9273-1f05bbb2395c
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: logic-apps
ms.date: 09/13/2017
ms.author: LADocs; jehollan
ms.openlocfilehash: b88d0c1ccb7a729c95299bcdc3cba5fd73fcdeac
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/19/2018
---
# <a name="common-scenarios-examples-tutorials-and-walkthroughs-for-azure-logic-apps"></a>Typowe scenariusze, przykłady samouczki i wskazówki dla usługi Azure Logic Apps

[Aplikacje logiki platformy Azure](../logic-apps/logic-apps-overview.md) pomaga organizować i integrowanie różnych usług zapewniając [łączniki gotowe do użycia 100 +](../connectors/apis-list.md), począwszy od lokalnego programu SQL Server lub SAP do kognitywnych usług firmy Microsoft. Usługa Logic Apps jest "niekorzystającą", tak aby nie trzeba martwić się o skali lub wystąpienia. Wystarczy, że jest zdefiniowanie przepływu pracy z wyzwalacza i akcji wykonywanych przez przepływ pracy. Platforma podstawowa obsługuje skali, dostępności i wydajności. Logic Apps jest szczególnie przydatne w przypadku przypadki użycia i scenariusze, w których należy do zapewnienia koordynacji wielu czynności w wielu systemach.

Aby dowiedzieć się więcej o wielu wzorców i możliwości który [Azure Logic Apps](../logic-apps/logic-apps-overview.md) obsługuje, poniżej przedstawiono typowe przykłady i scenariusze.

## <a name="popular-starting-points-for-logic-app-workflows"></a>Popularne punktami startowymi dla przepływów pracy aplikacji logiki

Każda aplikacja logiki rozpoczyna się od [ *wyzwalacza*](../logic-apps/logic-apps-overview.md#logic-app-concepts)i tylko jeden wyzwalacz, która uruchamia przepływ pracy aplikacji logiki i przekazuje w żadnych danych w ramach tego wyzwalacza. Kilka łączników udostępnia wyzwalacze, które są dostępne w następujących typów:

* *Wyzwalacze sondowania*: regularnie sprawdza punkt końcowy usługi dla nowych danych. Jeśli istnieje nowych danych, wyzwalacz tworzy i uruchamia nowe wystąpienie przepływu pracy z danymi wejściowymi.

* *Wyzwalacze wypychania*: nasłuchuje danych na punkt końcowy usługi i czeka, aż określone zdarzenie. Po wystąpieniu zdarzenia wyzwalacz uruchamia natychmiast, tworzenie i uruchamianie nowego wystąpienia przepływu pracy, który wykorzystuje wszystkie dostępne dane jako dane wejściowe.

Oto kilka przykładów popularnych wyzwalacz:

* Sondowania: 

  * [**Harmonogram - cyklu** wyzwalacza](../connectors/connectors-native-recurrence.md) umożliwia określenie daty rozpoczęcia i czas oraz cykl dla uruchamiania aplikacji logiki. 
  Można na przykład wybierz dni tygodnia oraz godzin służącą do wyzwalania aplikacji logiki.

  * Wyzwalacz "Po odebraniu wiadomości e-mail" umożliwia aplikację logiki, sprawdź, czy nowy adres e-mail z każdego dostawcy poczty, która jest obsługiwana przez logikę aplikacji, na przykład [Office 365 Outlook](../connectors/connectors-create-api-office365-outlook.md), [Gmail](https://docs.microsoft.com/connectors/gmail/), [ Outlook.com](https://docs.microsoft.com/connectors/outlook/)i tak dalej.

  * [ **HTTP** wyzwalacza](../connectors/connectors-native-http.md) umożliwia aplikację logiki, czy punkt końcowy usługi określonego przez komunikację za pośrednictwem protokołu HTTP.
  
* Wypchnij:

  * [ **Żądanie / odpowiedź — żądanie** wyzwalacza](../connectors/connectors-native-reqres.md) umożliwia aplikacji logiki odbierania żądań HTTP i odpowiedzi w czasie rzeczywistym na zdarzenia w inny sposób.

  * [ **HTTP elementu Webhook** wyzwalacza](../connectors/connectors-native-webhook.md) subskrybuje punkt końcowy usługi w zarejestrowani *wywołania zwrotnego adresu URL* przy użyciu tej usługi. 
  W ten sposób usługa tylko powiadamiać wyzwalacza w przypadku określonego zdarzenia, aby wyzwalacz nie musi sondować usługi.

Po otrzymaniu powiadomienia o nowych danych lub zdarzenie, wyzwalacz generowane, tworzy nowe wystąpienie przepływu pracy aplikacji logiki i uruchamia działania w przepływie pracy. Są dostępne wszystkie dane z wyzwalacza w całym przepływie pracy. Na przykład wyzwalacza "na nowy tweet" jest przekazywana zawartości tweet do aplikacji logiki, uruchom. 

## <a name="respond-to-triggers-and-extend-actions"></a>Odpowiadanie na wyzwalacze i akcje rozszerzenia

Dla systemów i usług, które mogą nie mieć opublikowane łączników można rozszerzać aplikacji logiki.

* [Tworzenie niestandardowych wyzwalaczy lub akcji](../logic-apps/logic-apps-create-api-app.md)
* [Konfigurowanie akcji długotrwałe dla przebiegów przepływu pracy](../logic-apps/logic-apps-create-api-app.md)
* [Odpowiadanie na zdarzenia zewnętrzne i działania z elementów webhook](../logic-apps/logic-apps-create-api-app.md)
* [Wywołaj wyzwalacz, lub zagnieżdżania przepływy pracy synchronicznej odpowiedzi na żądania HTTP](../logic-apps/logic-apps-http-endpoint.md)
* [Samouczek: Zasilane AI społecznościowych pulpit nawigacyjny kompilacji w minutach, z Logic Apps i usługi Power BI](http://aka.ms/logicappsdemo)
* [Samouczek: Odpowiadanie na elementów webhook SMS usługi Twilio i wysyłania odpowiedzi tekstu](https://channel9.msdn.com/Blogs/Windows-Azure/Azure-Logic-Apps-Walkthrough-Webhook-Functions-and-an-SMS-Bot)

## <a name="control-flow-error-handling-and-logging-capabilities"></a>Przepływ sterowania, obsługa błędów oraz możliwości rejestrowania zdarzeń

Aplikacje logiki to rozbudowane możliwości przepływu sterowania zaawansowanych, takich jak warunki, przełączniki, pętli i zakresów. Aby zapewnić odporność rozwiązania, można też wdrożyć błąd i obsługa wyjątków w przepływów pracy. Dla powiadomień i dzienników diagnostycznych dla przepływu pracy stan uruchomienia Azure Logic Apps oferuje również monitorowanie i alerty.

* [Proces elementów w kolekcji z pętli i partie w aplikacjach logiki i tablic](../logic-apps/logic-apps-loops-and-scopes.md)
* [Wykonaj różne akcje w instrukcjach przełącznika](../logic-apps/logic-apps-switch-case.md)
* [Błąd tworzenie i obsługa wyjątków w przepływie pracy](../logic-apps/logic-apps-exception-handling.md)
* [Przypadek użycia: jak opieki zdrowotnej firma korzysta z obsługi dla przepływów pracy HL7 FHIR wyjątków aplikacji logiki](../logic-apps/logic-apps-scenario-error-and-exception-handling.md)
* [Włącz monitorowanie, rejestrowanie i alerty dla istniejących aplikacji logiki](../logic-apps/logic-apps-monitor-your-logic-apps.md)
* [Włącz monitorowanie i rejestrowanie danych diagnostycznych podczas tworzenia aplikacji logiki](../logic-apps/logic-apps-monitor-your-logic-apps-oms.md)

## <a name="deploy-and-manage-logic-apps"></a>Wdrażanie i zarządzanie nimi aplikacje logiki

Pełni mogą tworzyć i wdrażać aplikacje logiki z programu Visual Studio, Visual Studio Team Services lub innymi kontroli źródła i narzędzi kompilacji automatycznych. Do obsługi wdrożenia dla przepływów pracy i zależne połączeń w szablonie zasobów, aplikacje logiki Użyj szablony wdrażania zasobów platformy Azure. Narzędzia programu Visual Studio automatycznie generować tych szablonów, które można sprawdzić kontroli źródła dla wersji.

* [Tworzenie i wdrażanie aplikacji logiki w programie Visual Studio](../logic-apps/logic-apps-deploy-from-vs.md)
* [Włącz monitorowanie, rejestrowanie i alerty dla istniejących aplikacji logiki](../logic-apps/logic-apps-monitor-your-logic-apps.md)
* [Tworzenie szablonu automatycznego wdrażania](../logic-apps/logic-apps-create-deploy-template.md)

## <a name="content-types-conversions-and-transformations-within-a-run"></a>Typy zawartości, konwersje i przekształcenia w Uruchom

Dostęp, konwersji i przekształcanie wiele typów zawartości za pomocą wielu funkcji w usłudze Azure Logic Apps [język definicji przepływu pracy](http://aka.ms/logicappsdocs). Na przykład można konwertować między ciągu, JSON i XML z `@json()` i `@xml()` wyrażeń przepływu pracy. Aparat Logic Apps zachowuje typy zawartości do obsługi transferu zawartości w sposób bezstratny między usługami.

* [Sposób działania wyrażeń przepływu pracy w aplikacji logiki](../logic-apps/logic-apps-author-definitions.md)
* [Obsługi typów zawartości bez JSON](../logic-apps/logic-apps-content-type.md), takiej jak `application/xml`, `application/octet-stream`, i`multipart/formdata`
* [Odwołanie: Język definicji przepływu pracy aplikacje logiki platformy Azure](http://aka.ms/logicappsdocs)

## <a name="other-integrations-and-capabilities"></a>Inne integracji i możliwości

Usługa Logic apps oferuje także integrację z wielu usług, takich jak usługi Azure Functions, zarządzanie interfejsami API Azure usługi aplikacji Azure i niestandardowych końcowych HTTP, na przykład REST i SOAP.

* [Tworzenie w czasie rzeczywistym społecznościowych pulpitu nawigacyjnego z Niekorzystającą Azure](../logic-apps/logic-apps-scenario-social-serverless.md)
* [Wywołanie usługi Azure Functions w aplikacjach logic apps](../logic-apps/logic-apps-azure-functions.md)
* [Scenariusz: Aplikacje logiki wyzwalacza w środowisku Azure Functions](../logic-apps/logic-apps-scenario-function-sb-trigger.md)
* [Blog: Wywołanie punktów końcowych SOAP z aplikacji logiki](https://blogs.msdn.microsoft.com/logicapps/2016/04/07/using-soap-services-with-logic-apps/)

## <a name="end-to-end-scenarios"></a>Kompleksowe scenariusze

* [Oficjalny dokument: Enterprise end-to-end wielkość zarządzanie integracją z usługami Azure, takich jak aplikacje logiki](https://aka.ms/enterprise-integration-e2e-case-management-utilities-logic-apps)

## <a name="next-steps"></a>Kolejne kroki

* [Tworzenie definicji przepływu pracy przy użyciu języka definicji przepływu pracy](../logic-apps/logic-apps-author-definitions.md)
* [Obsługa błędów i wyjątków w aplikacji logiki](../logic-apps/logic-apps-exception-handling.md)
* [Przesyłanie komentarzy, pytania, opinie i sugestie dla jak możemy ulepszyć aplikacje logiki platformy Azure](https://feedback.azure.com/forums/287593-logic-apps)