---
title: "Azure Functions — omówienie | Microsoft Docs"
description: "Informacje na temat sposobu używania usługi Azure Functions do optymalizowania obciążeń asynchronicznych w ciągu minut."
services: functions
documentationcenter: na
author: mattchenderson
manager: cfowler
editor: 
tags: 
keywords: "usługa Azure Functions, funkcje, przetwarzanie zdarzeń, elementy webhook, obliczanie dynamiczne, architektura bez serwera"
ms.assetid: 01d6ca9f-ca3f-44fa-b0b9-7ffee115acd4
ms.service: functions
ms.devlang: multiple
ms.topic: overview
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 10/03/2017
ms.author: glenga
ms.custom: H1Hack27Feb2017, mvc
ms.openlocfilehash: d60c898225b944801504f38d536262134a31e021
ms.sourcegitcommit: c7215d71e1cdeab731dd923a9b6b6643cee6eb04
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/17/2017
---
# <a name="an-introduction-to-azure-functions"></a>Wprowadzenie do usługi Azure Functions  
Azure Functions to rozwiązanie umożliwiające łatwe uruchamianie małych fragmentów kodu („funkcji”) w chmurze. Możesz napisać tylko kod rozwiązujący aktualny problem, nie martwiąc się o całą aplikację ani infrastrukturę do jej uruchomienia. Funkcje programowanie może być jeszcze wydajniejsze i można użyć język programowania wyboru, takich jak C#, F #, Node.js, Java lub PHP. Płać tylko za czas działania kodu — platforma Azure jest skalowana zgodnie z potrzebami. Środowisko Azure Functions umożliwia tworzenie [niekorzystającą](https://azure.microsoft.com/overview/serverless-computing/) aplikacji w systemie Microsoft Azure.

W tym temacie przedstawiono ogólne omówienie usługi Azure Functions. Jeśli chcesz od razu i rozpocząć korzystanie z funkcji rozpoczynać [tworzenie pierwszej funkcji platformy Azure](functions-create-first-azure-function.md). Jeśli chcesz uzyskać informacje techniczne o usłudze Functions, zobacz [dokumentację dla deweloperów](functions-reference.md).

## <a name="features"></a>Funkcje
Oto niektóre najważniejsze funkcje funkcje:

* **Wybór języka** — funkcje zapisu przy użyciu wybór języka C#, F # lub języka Javascript. Zobacz [obsługiwanych języków](supported-languages.md) innych opcji.
* **Model cenowy płatności za użycie** — płać tylko za czas działania kodu. Zapoznaj się z opcjami planu hostingowego zużycia w [sekcji cennika](#pricing).  
* **Korzystaj z własnych zależności** — środowisko Functions obsługuje rozwiązania NuGet i NPM, dzięki czemu można używać ulubionych bibliotek.  
* **Zintegrowane zabezpieczenia** — ochrona funkcji wyzwalanych przez protokół HTTP za pośrednictwem dostawców uwierzytelniania OAuth, takich jak Azure Active Directory, Facebook, Google, Twitter i konto Microsoft.  
* **Uproszczona integracja** — łatwe korzystanie z usług platformy Azure i ofert oprogramowania jako usługi (SaaS). Kilka przykładów można znaleźć w [sekcji dotyczącej integracji](#integrations).  
* **Elastyczne programowanie** — kodowanie funkcji bezpośrednio w portalu lub konfigurowanie ciągłej integracji i wdrażanie kodu za pomocą [GitHub](../app-service/scripts/app-service-cli-continuous-deployment-github.md), [Visual Studio Team Services](../app-service/scripts/app-service-cli-continuous-deployment-vsts.md)i inne [obsługiwanych narzędzi deweloperskich](../app-service/app-service-deploy-local-git.md).  
* **Open source** — środowisko uruchomieniowe Functions typu open source jest [dostępne w serwisie GitHub](https://github.com/azure/azure-webjobs-sdk-script).  

## <a name="what-can-i-do-with-functions"></a>Co można zrobić w środowisku Functions?
Functions to doskonałe rozwiązanie do przetwarzania danych, integrowania systemów, pracy z Internetu —-rzeczy (IoT) oraz tworzenia prostych interfejsów API i mikrousług. Usługa Functions może pomóc w wykonywaniu takich zadań, jak przetwarzanie zamówień lub obrazów, zarządzanie plikami lub wszelkie zadania, które mają być uruchamiane zgodnie z harmonogramem. 

Środowisko usługi Functions zawiera szablony ułatwiające rozpoczęcie pracy z kluczowymi scenariuszami, między innymi:

* **HTTPTrigger** — wyzwalanie wykonywania kodu za pomocą żądania HTTP. Na przykład zobacz [tworzenie pierwszej funkcji](functions-create-first-azure-function.md).
* **TimerTrigger** — wykonywanie oczyszczania lub innych zadań wsadowych zgodnie ze wstępnie zdefiniowanym harmonogramem. Na przykład zobacz [Utwórz funkcję wyzwalany przez czasomierz](functions-create-scheduled-function.md).
* **Element webhook GitHub** — odpowiadanie na zdarzenia występujące w repozytoriach usługi GitHub. Na przykład zobacz [Utwórz funkcję wyzwalane przez GitHub webhook](functions-create-github-webhook-triggered-function.md).
* **Ogólny element webhook** — przetwarzanie żądań HTTP elementu webhook z dowolnej usługi obsługującej takie elementy. Na przykład zobacz [Utwórz funkcję wyzwalane przez ogólny element webhook](functions-create-generic-webhook-triggered-function.md).
* **CosmosDBTrigger** -DB rozwiązania Cosmos Azure procesu dokumentów, gdy są one dodane lub zaktualizowane w kolekcji w bazie danych NoSQL. Na przykład zobacz [Utwórz funkcję wyzwalane przez bazy danych Azure rozwiązania Cosmos](functions-create-cosmos-db-triggered-function.md).
* **BlobTrigger** — przetwarzanie obiektów blob usługi Azure Storage dodawanych do kontenerów. Tej funkcji można użyć do zmiany rozmiaru obrazu. Aby uzyskać więcej informacji, zobacz [obiektu Blob magazynu powiązania](functions-bindings-storage-blob.md).
* **QueueTrigger** — odpowiadanie na komunikaty przychodzące w kolejce usługi Azure Storage. Na przykład zobacz [tworzy wyzwalane przez magazynu kolejek Azure funkcję](functions-create-storage-queue-triggered-function.md).
* **EventHubTrigger** — odpowiadanie na zdarzenia dostarczone do usługi Azure Event Hub. Opcja szczególnie przydatna podczas instrumentacji aplikacji, przetwarzania środowiska użytkownika lub przepływu pracy oraz pracy ze scenariuszami Internetu rzeczy (IoT). Aby uzyskać więcej informacji, zobacz [powiązania usługi Event Hubs](functions-bindings-event-hubs.md).
* **ServiceBusQueueTrigger** — łączenie kodu z innymi usługami platformy Azure lub usługami lokalnymi przez nasłuchiwanie w kolejkach komunikatów. Aby uzyskać więcej informacji, zobacz [powiązania usługi Service Bus](functions-bindings-service-bus.md).
* **ServiceBusTopicTrigger** — łączenie kodu z innymi usługami platformy Azure lub usługami lokalnymi przez subskrybowanie tematów. Aby uzyskać więcej informacji, zobacz [powiązania usługi Service Bus](functions-bindings-service-bus.md).

Środowisko usługi Azure Functions obsługuje *wyzwalacze*, czyli metody uruchamiania wykonywania kodu, i *powiązania*, czyli metody upraszczania kodowania danych wejściowych i wyjściowych. Szczegółowy opis wyzwalaczy i powiązań oferowanych w środowisku Azure Functions można znaleźć w [odpowiedniej dokumentacji usługi Azure Functions dla deweloperów](functions-triggers-bindings.md).

## <a name="integrations"></a>Integracje
Usługę Azure Functions można integrować z różnymi usługami platformy Azure i firm zewnętrznych. Te usługi umożliwiają wyzwalanie funkcji i uruchamianie wykonywania. Mogą również służyć jako dane wejściowe i wyjściowe dla kodu. Azure Functions obsługiwane są poniższe integracje usług:

* Azure Cosmos DB
* Azure Event Hubs 
* Azure Event Grid
* Azure Mobile Apps (tabele)
* Azure Notification Hubs
* Azure Service Bus (kolejki i tematy)
* Azure Storage (obiekty blob, kolejki i tabele) 
* GitHub (elementy webhook)
* Lokalne (za pomocą usługi Service Bus)
* Twilio (wiadomości SMS)

## <a name="pricing"></a>Ile kosztuje usługa Azure Functions?
Usługa Azure Functions oferuje dwa rodzaje planów cenowych. Wybierz ten, który najlepiej zaspokaja Twoje potrzeby: 

* **Plan zużycia** — po uruchomieniu funkcji platforma Azure udostępnia wszystkie niezbędne zasoby obliczeniowe. Nie musisz martwić się o zarządzanie zasobami i płacisz tylko za czas działania kodu. 
* **Plan usługi App Service** — uruchamianie funkcji tak samo jak aplikacji sieci Web, mobilnych i interfejsu API. Jeśli już używasz usługi App Service dla innych aplikacji, możesz uruchamiać swoje funkcje w ramach tego samego planu bez dodatkowych kosztów. 

Aby uzyskać więcej informacji o planach obsługi, zobacz [usługi Azure Functions hosting porównania planu](functions-scale.md). Szczegółowe informacje są dostępne na [stronie cennika usługi Functions](https://azure.microsoft.com/pricing/details/functions/).

## <a name="next-steps"></a>Następne kroki
* [Tworzenie pierwszej funkcji platformy Azure](functions-create-first-azure-function.md)  
  Od razu utwórz swoją pierwszą funkcję przy użyciu opcji szybkiego startu usługi Azure Functions. 
* [Dokumentacja usługi Azure Functions dla deweloperów](functions-reference.md)  
  Dalsze informacje techniczne na temat środowiska uruchomieniowego usługi Azure Functions, a także dokumentacja dotycząca kodowania funkcji oraz definiowania wyzwalaczy i powiązań.
* [Testowanie usługi Azure Functions](functions-test-a-function.md)  
  Opis różnych narzędzi i technik testowania funkcji.
* [Jak skalować usługę Azure Functions](functions-scale.md)  
  Omówienie planów usług dostępnych w środowisku Azure Functions, w tym planu hostingowego zużycia, oraz sposobu wybierania właściwego planu. 
* [Dowiedz się więcej o usłudze Azure App Service](../app-service/app-service-web-overview.md)  
  Azure wykorzystuje funkcje usługi Azure App Service dla podstawowych funkcji, takich jak wdrożenia, zmienne środowiskowe i Diagnostyka. 

