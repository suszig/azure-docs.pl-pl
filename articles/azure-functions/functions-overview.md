---
title: Azure Functions — omówienie | Microsoft Docs
description: Informacje na temat sposobu używania usługi Azure Functions do optymalizowania obciążeń asynchronicznych w ciągu minut.
services: functions
documentationcenter: na
author: mattchenderson
manager: erikre
editor: ''
tags: ''
keywords: usługa Azure Functions, funkcje, przetwarzanie zdarzeń, elementy webhook, obliczanie dynamiczne, architektura bez serwera

ms.service: functions
ms.devlang: multiple
ms.topic: get-started-article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 08/29/2016
ms.author: cfowler;mahender;glenga

---
# Azure Functions — omówienie
Azure Functions to rozwiązanie umożliwiające łatwe uruchamianie małych fragmentów kodu („funkcji”) w chmurze. Możesz napisać tylko kod rozwiązujący aktualny problem, nie martwiąc się o całą aplikację ani infrastrukturę do jej uruchomienia. Dzięki temu programowanie może być jeszcze wydajniejsze i można korzystać z wybranego języka programowania, takiego jak C#, F#, Node.js, Python lub PHP. Płać tylko za czas działania kodu — platforma Azure jest skalowana zgodnie z potrzebami.

W tym temacie przedstawiono ogólne omówienie usługi Azure Functions. Jeśli chcesz od razu rozpocząć korzystanie z usługi Azure Functions, skorzystaj z artykułu [Tworzenie pierwszej funkcji platformy Azure](functions-create-first-azure-function.md). Jeśli chcesz uzyskać informacje techniczne o usłudze Functions, zobacz [dokumentację dla deweloperów](functions-reference.md).

## Funkcje
Oto główne funkcje usługi Azure Functions:

* **Wybór języka** — tworzenie funkcji przy użyciu języka C#, F#, Node.js, Python, PHP, batch, bash, Java lub użycie dowolnych plików wykonywalnych.
* **Model cenowy płatności za użycie** — płać tylko za czas działania kodu. Zobacz opis opcji dynamicznego planu usługi App Service w poniższej [sekcji dotyczącej cen](#pricing).  
* **Korzystaj z własnych zależności** — środowisko Functions obsługuje rozwiązania NuGet i NPM, dzięki czemu można używać ulubionych bibliotek.  
* **Zintegrowane zabezpieczenia** — ochrona funkcji wyzwalanych przez protokół HTTP za pośrednictwem dostawców uwierzytelniania OAuth, takich jak Azure Active Directory, Facebook, Google, Twitter i konto Microsoft.  
* **Uproszczona integracja** — łatwe korzystanie z usług platformy Azure i ofert oprogramowania jako usługi (SaaS). Kilka przykładów można znaleźć w poniższej [sekcji dotyczącej integracji](#integrations).  
* **Elastyczne programowanie** — kodowanie funkcji bezpośrednio w portalu lub konfigurowanie ciągłej integracji i wdrażanie kodu za pomocą usług GitHub, Visual Studio Team Services i innych [obsługiwanych narzędzi deweloperskich](../app-service-web/web-sites-deploy.md#deploy-using-an-ide).  
* **Open source** — środowisko uruchomieniowe Functions typu open source jest [dostępne w serwisie GitHub](https://github.com/azure/azure-webjobs-sdk-script).  

## Co można zrobić w środowisku Functions?
Środowisko Azure Functions to doskonałe rozwiązanie do przetwarzania danych, integrowania systemów, pracy z Internetem rzeczy oraz tworzenia prostych interfejsów API i mikrousług. Usługa Functions może pomóc w wykonywaniu takich zadań jak przetwarzanie zamówień lub obrazów, zarządzanie plikami, zadania długoterminowe do uruchamiania w wątku w tle lub wszelkie zadania, które mają być uruchamiane zgodnie z harmonogramem. 

Środowisko usługi Functions zawiera szablony ułatwiające rozpoczęcie pracy z kluczowymi scenariuszami, między innymi:

* **BlobTrigger** — przetwarzanie obiektów blob usługi Azure Storage dodawanych do kontenerów. Tej opcji można użyć do zmiany rozmiaru obrazu.
* **EventHubTrigger** — odpowiadanie na zdarzenia dostarczone do usługi Azure Event Hub. Opcja szczególnie przydatna podczas instrumentacji aplikacji, przetwarzania środowiska użytkownika lub przepływu pracy oraz pracy ze scenariuszami Internetu rzeczy (IoT).
* **Ogólny element webhook** — przetwarzanie żądań HTTP elementu webhook z dowolnej usługi obsługującej takie elementy.
* **Element webhook GitHub** — odpowiadanie na zdarzenia występujące w repozytoriach usługi GitHub. Przykład można znaleźć w temacie [Create a webhook or API function](functions-create-a-web-hook-or-api-function.md) (Tworzenie funkcji interfejsu API lub elementu webhook).
* **HTTPTrigger** — wyzwalanie wykonywania kodu za pomocą żądania HTTP.
* **QueueTrigger** — odpowiadanie na komunikaty przychodzące w kolejce usługi Azure Storage. Przykład można znaleźć w temacie [Create an Azure Function which binds to an Azure service](functions-create-an-azure-connected-function.md) (Tworzenie funkcji platformy Azure powiązanej z usługą platformy Azure).
* **ServiceBusQueueTrigger** — łączenie kodu z innymi usługami platformy Azure lub usługami lokalnymi przez nasłuchiwanie w kolejkach komunikatów. 
* **ServiceBusTopicTrigger** — łączenie kodu z innymi usługami platformy Azure lub usługami lokalnymi przez subskrybowanie tematów. 
* **TimerTrigger** — wykonywanie oczyszczania lub innych zadań wsadowych zgodnie ze wstępnie zdefiniowanym harmonogramem. Przykład można znaleźć w temacie [Tworzenie funkcji przetwarzania zdarzeń](functions-create-an-event-processing-function.md).

Środowisko usługi Azure Functions obsługuje *wyzwalacze*, czyli metody uruchamiania wykonywania kodu, i *powiązania*, czyli metody upraszczania kodowania danych wejściowych i wyjściowych. Szczegółowy opis wyzwalaczy i powiązań oferowanych w środowisku Azure Functions można znaleźć w [odpowiedniej dokumentacji usługi Azure Functions dla deweloperów](functions-triggers-bindings.md).

## <a name="integrations"></a>Integracje
Usługę Azure Functions można integrować z różnymi usługami platformy Azure i firm zewnętrznych. Umożliwiają one wyzwalanie funkcji i uruchamianie wykonywania. Mogą również służyć jako dane wejściowe i wyjściowe dla kodu. W środowisku usługi Azure Functions obsługiwane są poniższe integracje usług. 

* Azure DocumentDB
* Azure Event Hubs 
* Azure Mobile Apps (tabele)
* Azure Notification Hubs
* Azure Service Bus (kolejki i tematy)
* Azure Storage (obiekty blob, kolejki i tabele) 
* GitHub (elementy webhook)
* Lokalne (za pomocą usługi Service Bus)

## <a name="pricing"></a>Ile kosztuje usługa Azure Functions?
Usługa Azure Functions oferuje dwa rodzaje planów cenowych. Wybierz ten, który najlepiej zaspokaja Twoje potrzeby: 

* **Dynamiczny plan hostingu** — po uruchomieniu funkcji platforma Azure udostępnia wszystkie niezbędne zasoby obliczeniowe. Nie musisz martwić się o zarządzanie zasobami i płacisz tylko za czas działania kodu. Szczegółowe informacje są dostępne na [stronie cennika usługi Functions](/pricing/details/functions). 
* **Plan usługi App Service** — uruchamianie funkcji tak samo jak aplikacji sieci Web, mobilnych i interfejsu API. Jeśli już używasz usługi App Service dla innych aplikacji, możesz uruchamiać swoje funkcje w ramach tego samego planu bez dodatkowych kosztów. Szczegółowe informacje są dostępne na [stronie cennika usługi App Service](/pricing/details/app-service/).

Aby uzyskać więcej informacji na temat skalowania funkcji, zobacz artykuł [Jak skalować usługę Azure Functions](functions-scale.md).

## Następne kroki
* [Tworzenie pierwszej funkcji platformy Azure](functions-create-first-azure-function.md)  
  Od razu utwórz swoją pierwszą funkcję przy użyciu opcji szybkiego startu usługi Azure Functions. 
* [Dokumentacja usługi Azure Functions dla deweloperów](functions-reference.md)  
  Dalsze informacje techniczne na temat środowiska uruchomieniowego usługi Azure Functions, a także dokumentacja dotycząca kodowania funkcji oraz definiowania wyzwalaczy i powiązań.
* [Testowanie usługi Azure Functions](functions-test-a-function.md)  
  Opis różnych narzędzi i technik testowania funkcji.
* [Jak skalować usługę Azure Functions](functions-scale.md)  
  Omówienie planów usług dostępnych w środowisku Azure Functions, w tym dynamicznego planu usług, oraz sposobu wybierania właściwego planu. 
* [Dowiedz się więcej o usłudze Azure App Service](../app-service/app-service-value-prop-what-is.md)  
  Środowisko Azure Functions używa platformy Azure App Service na potrzeby funkcji podstawowych, takich jak wdrożenia, zmienne środowiskowe i diagnostyka. 

<!--HONumber=Sep16_HO3-->


