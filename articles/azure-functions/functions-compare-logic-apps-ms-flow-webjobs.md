---
title: "Wybieranie między usługami Flow, Logic Apps, Functions i WebJobs | Microsoft Docs"
description: "Porównaj i zestaw ze sobą usługi integracji z chmurą firmy Microsoft, a następnie zdecyduj, których usług chcesz używać."
services: functions,app-service\logic
documentationcenter: na
author: ggailey777
manager: wpickett
tags: 
keywords: microsoft flow, flow, logic apps, azure functions, functions, azure webjobs, webjobs, event processing, dynamic compute, serverless architecture
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
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/17/2017
---
# <a name="choose-between-flow-logic-apps-functions-and-webjobs"></a>Wybieranie między przepływem, aplikacjami logiki, funkcjami a zadaniami WebJob
W tym artykule porównano i zestawiono ze sobą następujące usługi w chmurze firmy Microsoft, które mogą rozwiązać problemy z integracją i zautomatyzować procesy biznesowe:

* [Microsoft Flow](https://flow.microsoft.com/)
* [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/)
* [Azure Functions](https://azure.microsoft.com/services/functions/)
* [Azure App Service WebJobs](../app-service/web-sites-create-web-jobs.md)

Wszystkie te usługi są przydatne podczas łączenia ze sobą różnych systemów. Mogą one definiować dane wejściowe, akcje, warunki i dane wyjściowe. Każdą z nich można uruchomić zgodnie z harmonogramem lub za pomocą wyzwalacza. Jednak każda usługa ma unikatowe zalety, dlatego ich porównanie nie daje odpowiedzi na pytanie „Która usługa jest najlepsza?”, tylko na pytanie „Która usługa najlepiej pasuje do mojej sytuacji?”. Często najlepszym sposobem, aby szybko utworzyć skalowalne, wyposażone we wszystkie funkcje rozwiązanie z zakresu integracji, jest kombinacja tych usług.

<a name="flow"></a>

## <a name="flow-vs-logic-apps"></a>Flow a Logic Apps
Usługi Microsoft Flow i Azure Logic Apps możemy omówić razem, ponieważ są one usługami integracji, w których *pierwszeństwo ma konfiguracja*. Ułatwiają one tworzenie procesów i przepływów pracy oraz integrują się z różnymi aplikacjami SaaS i aplikacjami dla przedsiębiorstw. 

* Usługa Flow bazuje na usłudze Logic Apps
* Mają one tego samego projektanta przepływów pracy
* [Łączniki](../connectors/apis-list.md), które działają w jednej usłudze, mogą również działać w drugiej

Usługa Flow umożliwia każdemu pracownikowi biurowemu wykonywanie prostych integracji (na przykład odbieranie wiadomości SMS dotyczących ważnych wiadomości e-mail) bez konieczności zwracania się do deweloperów lub działu IT. Z drugiej strony usługa Logic Apps umożliwia korzystanie z zaawansowanych lub krytycznych dla działalności firmy integracji (na przykład procesów B2B), w których są wymagane operacje deweloperskie i praktyki dotyczące zabezpieczeń na poziomie przedsiębiorstw. Typowe dla biznesowego przepływu pracy jest zwiększanie się stopnia skomplikowania wraz z upływem czasu. W związku z tym możesz zacząć od przepływu, a następnie przekonwertować go na aplikację logiki odpowiednio do potrzeb.

Poniższa tabela ułatwia określenie, czy dla danej integracji najlepsza jest usługa Flow, czy Logic Apps.

|  | Flow | Logic Apps |
| --- | --- | --- |
| Grupy odbiorców |Pracownicy biurowi, użytkownicy biznesowi |Informatycy, deweloperzy |
| Scenariusze |Samoobsługa |Operacje krytyczne dla działalności firmy |
| Narzędzie do projektowania |Aplikacja w przeglądarce i aplikacja mobilna, tylko interfejs użytkownika |Aplikacja w przeglądarce i program [Visual Studio](../logic-apps/logic-apps-deploy-from-vs.md), dostępny [widok kodu](../logic-apps/logic-apps-author-definitions.md) |
| DevOps |Ad hoc, opracowywanie w środowisku produkcyjnym |Kontrola kodu źródłowego, testowanie, obsługa, automatyzacja oraz możliwości zarządzania w usłudze [Azure Resource Management](../logic-apps/logic-apps-create-deploy-azure-resource-manager-templates.md) |
| Doświadczenie administratora |[https://flow.microsoft.com](https://flow.microsoft.com) |[https://portal.azure.com](https://portal.azure.com) |
| Bezpieczeństwo |Standardowe rozwiązania: [niezależność danych](https://wikipedia.org/wiki/Technological_Sovereignty), [szyfrowanie magazynowanych](https://wikipedia.org/wiki/Data_at_rest#Encryption) poufnych danych itd. |Zapewnienie bezpieczeństwa platformy Azure: [zabezpieczenia platformy Azure](https://www.microsoft.com/trustcenter/Security/AzureSecurity), [Security Center](https://azure.microsoft.com/services/security-center/), [dzienniki inspekcji](https://azure.microsoft.com/blog/azure-audit-logs-ux-refresh/) itd. |

<a name="function"></a>

## <a name="functions-vs-webjobs"></a>Functions a WebJobs
Usługi Azure Functions i Azure App Service WebJobs możemy omówić razem, ponieważ są one usługami integracji, w których *pierwszeństwo ma kod*, i są przeznaczone dla deweloperów. Umożliwiają one użytkownikom uruchamianie skryptów lub fragmentów kodu w odpowiedzi na różne zdarzenia, takie jak [nowe obiekty blob magazynu](functions-bindings-storage.md) czy [żądanie elementu webhook](functions-bindings-http-webhook.md). Oto ich podobieństwa: 

* Obie są wbudowane w usłudze [Azure App Service](../app-service/app-service-web-overview.md) i korzystają z funkcji, takich jak [kontrola kodu źródłowego](../app-service/app-service-continuous-deployment.md), [uwierzytelnianie](../app-service/app-service-authentication-overview.md) i [monitorowanie](../app-service/web-sites-monitor.md).
* Obie są usługami ukierunkowanymi na dewelopera.
* Obie obsługują standardowe języki skryptowe i języki programowania.
* Obie obsługują menedżery NuGet i NPM.

Usługa Functions to naturalna ewolucja usługi WebJobs, w której przejęto i udoskonalono najlepsze elementy usługi WebJobs. Wprowadzone ulepszenia to m.in.: 

* Model aplikacji [bezserwerowych](https://azure.microsoft.com/overview/serverless-computing/).
* Prostsze opracowywanie, testowanie i uruchamianie kodu bezpośrednio w przeglądarce.
* Wbudowana integracja z innymi usługami platformy Azure oraz usługami innych firm, takimi jak [elementy webhook GitHub](https://developer.github.com/webhooks/creating/).
* Płatność za użycie — nie trzeba płacić za [plan usługi App Service](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).
* Automatyczne, [dynamiczne skalowanie](functions-scale.md).
* Możliwość korzystania z planu usługi App Service dla istniejących klientów usługi App Service (w celu użycia niewystarczająco wykorzystywanych zasobów).
* Integracja z usługą Logic Apps.

W poniższej tabeli zestawiono różnice między usługami Functions i WebJobs:

|  | Functions | WebJobs |
| --- | --- | --- |
| Skalowanie |Skalowanie bez konfiguracji |Skalowanie z planem usługi App Service |
| Cennik |Płatność za użycie lub w ramach planu usługi App Service |W ramach planu usługi App Service |
| Sposób uruchamiania |Wyzwalane, zaplanowane (za pomocą wyzwalacza w postaci czasomierza) |Wyzwalane, ciągłe, zaplanowane |
| Zdarzenia wyzwalacza |[Czasomierz](functions-bindings-timer.md), [Azure Cosmos DB](functions-bindings-documentdb.md), [Azure Event Hubs](functions-bindings-event-hubs.md), [HTTP/WebHook (GitHub, Slack)](functions-bindings-http-webhook.md), [Azure App Service Mobile Apps](functions-bindings-mobile-apps.md), [Azure Event Hubs](functions-bindings-event-hubs.md), [kolejki i obiekty blob usługi Azure Storage](functions-bindings-storage-blob.md), [kolejki i tematy usługi Azure Service Bus](functions-bindings-service-bus.md) |[Kolejki i obiekty blob usługi Azure Storage](functions-bindings-storage-blob.md), [kolejki i tematy usługi Azure Service Bus](functions-bindings-service-bus.md) |
| Opracowywanie w przeglądarce |Obsługiwane |Nieobsługiwane |
| C# |Obsługiwane |Obsługiwane |
| F# |Obsługiwane |Nieobsługiwane |
| JavaScript |Obsługiwane |Obsługiwane |
| Java |Wersja zapoznawcza | Nieobsługiwane |
| Bash |Eksperymentalne |Obsługiwane |
| Skrypty systemu Windows (cmd, bat) |Eksperymentalne |Obsługiwane |
| PowerShell |Eksperymentalne |Obsługiwane |
| PHP |Eksperymentalne |Obsługiwane |
| Python |Eksperymentalne |Obsługiwane |
| TypeScript |Eksperymentalne |Nieobsługiwane |

Wybór między usługą Functions i WebJobs ostatecznie zależy od aktualnie wykonywanych czynności w usłudze App Service. Jeśli masz aplikację usługi App Service, dla której chcesz uruchamiać fragmenty kodu, i chcesz zarządzać nimi razem w tym samym środowisku DevOps, użyj usługi WebJobs. W poniższych scenariuszach użyj usługi Functions.

* Chcesz uruchamiać fragmenty kodu dla innych usług platformy Azure lub aplikacji innych firm.
* Chcesz zarządzać kodem integracji niezależnie od aplikacji usługi App Service.
* Chcesz wywoływać fragmenty kodu z poziomu aplikacji logiki. 

<a name="together"></a>

## <a name="flow-logic-apps-and-functions-together"></a>Usługi Flow, Logic Apps i Functions razem
Jak wspomniano wcześniej, to, która usługa najlepiej nadaje się do Twoich celów, zależy od sytuacji. 

* W przypadku prostej optymalizacji biznesowej użyj usługi Flow.
* Jeśli scenariusz integracji jest zbyt zaawansowany dla usługi Flow lub potrzebujesz funkcji środowiska DevOps i zgodności z zabezpieczeniami, użyj usługi Logic Apps.
* Jeśli jakiś krok w scenariuszu integracji wymaga bardzo niestandardowej transformacji lub wyspecjalizowanego kodu, napisz funkcję i wywołaj ją jako akcję w aplikacji logiki.

Aplikację logiki możesz wywołać w przepływie. Możesz również wywołać funkcję w aplikacji logiki i aplikację logiki w funkcji. Integracja usług Flow, Logic Apps i Functions jest cały czas ulepszana. Możesz utworzyć jakiś element w jednej usłudze i używać go w innych usługach. W związku z tym każda inwestycja w te trzy technologie jest cenna.

## <a name="next-steps"></a>Następne kroki
Rozpocznij pracę z każdą z tych usług, tworząc pierwszy przepływ, aplikację logiki, aplikację funkcji lub zadanie WebJob. Kliknij dowolny z następujących linków:

* [Rozpoczynanie pracy z platformą Microsoft Flow](https://flow.microsoft.com/en-us/documentation/getting-started/)
* [Tworzenie aplikacji logiki](../logic-apps/logic-apps-create-a-logic-app.md)
* [Tworzenie pierwszej funkcji platformy Azure](functions-create-first-azure-function.md)
* [Wdrażanie zadań WebJob za pomocą programu Visual Studio](../app-service/websites-dotnet-deploy-webjobs.md)

Możesz również uzyskać więcej informacji na temat tych usług integracji, korzystając z następujących linków:

* [Korzystanie z usług Azure Functions i Azure App Service w scenariuszach integracji (autor: Christopher Anderson)](http://www.biztalk360.com/integrate-2016-resources/leveraging-azure-functions-azure-app-service-integration-scenarios/)
* [Uproszczone integracje (autor: Charles Lamanna)](http://www.biztalk360.com/integrate-2016-resources/integrations-made-simple/)
* [Logic Apps — emisja internetowa na żywo](http://aka.ms/logicappslive)
* [Microsoft Flow — często zadawane pytania](https://flow.microsoft.com/documentation/frequently-asked-questions/)

