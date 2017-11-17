---
title: "Wybór między przepływu, Logic Apps, funkcje i zadań Webjob | Dokumentacja firmy Microsoft"
description: "Porównywanie i kontrastu chmury integracji usług firmy Microsoft i zdecydować, które usługi, należy użyć."
services: functions,app-service\logic
documentationcenter: na
author: ggailey777
manager: wpickett
tags: 
keywords: "Microsoft przepływu, przepływ, aplikacje logiki, funkcje platformy azure, funkcje platformy azure webjobs zadań webjob, przetwarzania, dynamiczne obliczeń niekorzystającą architektura zdarzeń"
ms.assetid: e9ccf7ad-efc4-41af-b9d3-584957b1515d
ms.service: functions
ms.devlang: multiple
ms.topic: overview
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 11/03/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: 03246846484878f7155449ad11b009aeffe8a576
ms.sourcegitcommit: c7215d71e1cdeab731dd923a9b6b6643cee6eb04
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/17/2017
---
# <a name="choose-between-flow-logic-apps-functions-and-webjobs"></a>Wybieranie między przepływem, aplikacjami logiki, funkcjami a zadaniami WebJob
W tym artykule porównuje i zachowanie różni się od następujących usług w chmurze firmy Microsoft, które można rozwiązać wszystkie problemy integracji i automatyzować procesy biznesowe:

* [Przepływ firmy Microsoft](https://flow.microsoft.com/)
* [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/)
* [Azure Functions](https://azure.microsoft.com/services/functions/)
* [Usługa aplikacji Azure Webjob](../app-service/web-sites-create-web-jobs.md)

Wszystkie te usługi są przydatne, gdy "przyklejanie" ze sobą różnych systemów. Można wszystkie definiują dane wejściowe, akcje, warunki i danych wyjściowych. Każdy z nich można uruchomić na harmonogram lub wyzwalacza. Jednak każda usługa ma unikatowy zalety i ich porównanie nie jest kwestia "usługi, która jest najbardziej?" oprócz jednego "usługi, która najlepiej nadaje się do tej sytuacji?" Często kombinacji tych usług to najlepszy sposób, aby szybko tworzyć rozwiązania skalowalne, oferujący wszystkie funkcje integracji.

<a name="flow"></a>

## <a name="flow-vs-logic-apps"></a>Przepływ programu vs. Aplikacje logiki
Można omówimy Flow firmy Microsoft i usługi Azure Logic Apps razem ponieważ są one zarówno *pierwszej konfiguracji* usług integracji. One ułatwiają tworzenie procesów i przepływów pracy oraz integrują się z różnych aplikacji SaaS i enterprise. 

* Przepływ bazuje na Logic Apps
* Mają one z tej samej projektanta przepływów pracy
* [Łączniki](../connectors/apis-list.md) czy pracy w jednym mogą również działać w innym

Przepływ upoważnia do wykonywania prostych integracji roboczych pakietu office (na przykład get SMS ważne wiadomości e-mail) bez przechodzenia przez deweloperów lub IT. Z drugiej strony aplikacje logiki można włączyć integracji zaawansowane lub krytycznym (na przykład B2B procesów), których praktyki dotyczące opracowywania oprogramowania i zabezpieczeń przedsiębiorstw są wymagane. Jest typowa dla biznesowego przepływu pracy zwiększa się złożoność wraz z upływem czasu. W związku z tym można uruchomić z przepływem na początku, a następnie przekonwertować go do aplikacji logiki, zgodnie z potrzebami.

Poniższa tabela ułatwia określenie, czy przepływ lub Logic Apps jest najlepsze dla danego integracji.

|  | Ruch | Aplikacje logiki |
| --- | --- | --- |
| Odbiorcy |Pracownicy biura, użytkownicy biznesowi |Informatycy i deweloperzy |
| Scenariusze |Samoobsługi |Krytycznym |
| Narzędzie do projektowania |W przeglądarce i przenośnych, aplikacja, tylko interfejsu użytkownika |W przeglądarce i [programu Visual Studio](../logic-apps/logic-apps-deploy-from-vs.md), [widok Kod](../logic-apps/logic-apps-author-definitions.md) dostępne |
| DevOps |Ad-hoc, tworzenie w środowisku produkcyjnym |Źródło kontroli, testowania, obsługi i automatyzacji oraz możliwości zarządzania w [zarządzania zasobami Azure](../logic-apps/logic-apps-create-deploy-azure-resource-manager-templates.md) |
| Środowisko pracy administratora |[https://Flow.microsoft.com](https://flow.microsoft.com) |[https://Portal.Azure.com](https://portal.azure.com) |
| Zabezpieczenia |Standardowe rozwiązania: [suwerenności danych](https://wikipedia.org/wiki/Technological_Sovereignty), [szyfrowanie magazynowanych](https://wikipedia.org/wiki/Data_at_rest#Encryption) dla poufnych danych, itp. |Zapewnienie bezpieczeństwa systemu Azure: [zabezpieczeń Azure](https://www.microsoft.com/trustcenter/Security/AzureSecurity), [Centrum zabezpieczeń](https://azure.microsoft.com/services/security-center/), [dzienniki inspekcji](https://azure.microsoft.com/blog/azure-audit-logs-ux-refresh/)itd. |

<a name="function"></a>

## <a name="functions-vs-webjobs"></a>Funkcje programu vs. Zadania WebJob
Można omówiono usługi Azure Functions i zadań Webjob Azure App Service razem ponieważ są one zarówno *pierwszy kod* integracji usług i przeznaczone dla deweloperów. Pozwalają użytkownikom na uruchamianie skryptu lub fragment kodu w odpowiedzi na różnych zdarzeń, takich jak [nowego magazynu obiektów blob](functions-bindings-storage.md) lub [żądania elementu WebHook](functions-bindings-http-webhook.md). Oto ich podobieństwa: 

* Zarówno są tworzone [usłudze Azure App Service](../app-service/app-service-web-overview.md) i korzystać z funkcji takich jak [kontroli źródła](../app-service/app-service-continuous-deployment.md), [uwierzytelniania](../app-service/app-service-authentication-overview.md), i [monitorowania](../app-service/web-sites-monitor.md).
* Oba są ukierunkowane dewelopera usługi.
* Koszty pomocy technicznej standard skryptów i języków programowania.
* Mają NuGet i NPM obsługuje.

Funkcje powstał fizycznych zadań Webjob przyjmuje najlepszych rzeczy informacji o zadaniach Webjob, i zwiększa się na nich. Ulepszenia obejmują: 

* [Niekorzystającą](https://azure.microsoft.com/overview/serverless-computing/) modelu aplikacji.
* Prostsze deweloperów, testowania i uruchom kodu bezpośrednio w przeglądarce.
* Wbudowane integrację z usługami 3rd firm i Azure więcej usług, takich jak [elementów Webhook GitHub](https://developer.github.com/webhooks/creating/).
* Płatności użycia, nie trzeba płacić za [planu usługi aplikacji](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).
* Automatyczne, [dynamiczne skalowanie](functions-scale.md).
* Dla istniejących klientów usługi App Service, systemem planu usługi aplikacji nadal możliwe (wykorzystać stopniu wykorzystania zasobów).
* Integracja z usługą Logic Apps.

W poniższej tabeli przedstawiono różnice w funkcjach i zadań Webjob:

|  | Funkcje | Zadania WebJob |
| --- | --- | --- |
| Skalowanie |Skalowanie configurationless |Skalowanie planu usługi aplikacji |
| Cennik |Płatności użycia lub w ramach planu usługi aplikacji |Część planu usługi aplikacji |
| Typ uruchomienia |wyzwalane, zaplanowane (za pomocą czasomierza wyzwalacz) |wyzwalane, ciągłego, zaplanowane |
| Wyzwalacz zdarzenia |[Czasomierz](functions-bindings-timer.md), [rozwiązania Cosmos Azure DB](functions-bindings-documentdb.md), [usługi Azure Event Hubs](functions-bindings-event-hubs.md), [HTTP/WebHook (GitHub, zapas czasu)](functions-bindings-http-webhook.md), [aplikacji mobilnych w usłudze Azure App Service](functions-bindings-mobile-apps.md), [Azure Event Hubs](functions-bindings-event-hubs.md), [obiektów blob i kolejek usługi Azure Storage](functions-bindings-storage-blob.md), [tematów i kolejek usługi Azure Service Bus](functions-bindings-service-bus.md) |[Azure magazynu kolejek i obiektów blob](functions-bindings-storage-blob.md), [tematów i kolejek usługi Azure Service Bus](functions-bindings-service-bus.md) |
| Programowanie w przeglądarce |Obsługiwane |Nieobsługiwane |
| C# |Obsługiwane |Obsługiwane |
| F# |Obsługiwane |Nieobsługiwane |
| JavaScript |Obsługiwane |Obsługiwane |
| Java |Podgląd | Nieobsługiwane |
| Bash |Eksperymentalne |Obsługiwane |
| Skryptów systemu Windows (cmd, bat) |Eksperymentalne |Obsługiwane |
| PowerShell |Eksperymentalne |Obsługiwane |
| PHP |Eksperymentalne |Obsługiwane |
| Python |Eksperymentalne |Obsługiwane |
| TypeScript |Eksperymentalne |Nieobsługiwane |

Czy używać funkcji lub zadań Webjob jest ostatecznie uzależniona od wykonywanych już czynności z usługi aplikacji. Jeśli masz aplikację usługi aplikacji, dla którego chcesz uruchomić wstawki kodu, i chcesz zarządzać nimi w tym samym środowisku DevOps, użyj zadań Webjob. W następujących scenariuszach Użyj funkcji.

* Chcesz uruchomić wstawki kodu dla innych usług platformy Azure lub aplikacje firm 3rd.
* Chcesz zarządzać kodu integracji oddzielnie z aplikacji usługi aplikacji.
* Chcesz się połączyć z fragmentów kodu z aplikacji logiki. 

<a name="together"></a>

## <a name="flow-logic-apps-and-functions-together"></a>Są zgrupowane, Logic Apps i funkcje
Jak wcześniej wspomniano usługi, która najlepiej nadaje się do Ciebie w zależności od sytuacji. 

* Optymalizacja biznesowa proste można użyć przepływu.
* Jeśli danego scenariusza integracji jest zbyt zaawansowane dla przepływu lub potrzebujesz możliwości DevOps i compliances zabezpieczeń, użyj Logic Apps.
* Jeśli krok w danym scenariuszu integracji wymaga dużej niestandardowych transformacji lub wyspecjalizowany kod, napisać funkcję i wywołać funkcję jako akcja w aplikacji logiki.

Możesz wywołać aplikacji logiki w strumieniu. Można również wywołać funkcję w aplikacji logiki i aplikacji logiki w funkcji. Integracja przepływu, Logic Apps i funkcje w dalszym ciągu poprawy wraz z upływem czasu. Możesz skompilować coś w jedną usługę i używać go w innych usług. W związku z tym inwestycji, wszelkie wprowadzone w tych trzech technologii jest rozważyć.

## <a name="next-steps"></a>Następne kroki
Wprowadzenie do wszystkich usług, tworząc Twojego pierwszego przepływu, aplikację logiki, aplikacji funkcji lub zadania WebJob. Kliknij dowolny z następujących łączy:

* [Rozpoczynanie pracy z Flow firmy Microsoft](https://flow.microsoft.com/en-us/documentation/getting-started/)
* [Tworzenie aplikacji logiki](../logic-apps/logic-apps-create-a-logic-app.md)
* [Tworzenie pierwszej funkcji platformy Azure](functions-create-first-azure-function.md)
* [Wdrażanie zadań WebJob za pomocą programu Visual Studio](../app-service/websites-dotnet-deploy-webjobs.md)

Można też uzyskać więcej informacji na temat tych usług integracji z następujących łączy:

* [Korzystanie z usług Azure Functions & Azure App Service dla scenariuszy integracji przez Christopher Anderson](http://www.biztalk360.com/integrate-2016-resources/leveraging-azure-functions-azure-app-service-integration-scenarios/)
* [Uproszczona Lamanna Charlesa integracji](http://www.biztalk360.com/integrate-2016-resources/integrations-made-simple/)
* [Emisja na żywo Logic Apps](http://aka.ms/logicappslive)
* [Microsoft przepływu często zadawane pytania](https://flow.microsoft.com/documentation/frequently-asked-questions/)

