---
title: "Wprowadzenie do usługi API Apps | Microsoft Docs"
description: "Dowiedz się, jak usługa Azure App Service pomaga tworzyć i obsługiwać interfejsy API RESTful oraz z nich korzystać."
services: app-service\api
documentationcenter: .net
author: alexkarcher-msft
manager: erikre
editor: 
ms.assetid: 60049a16-8159-47aa-a34b-110be0d8dab6
ms.service: app-service-api
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/23/2016
ms.author: alkarche
translationtype: Human Translation
ms.sourcegitcommit: 66fc8f7e1da55dbe6bb1dd8b8d6a535c498c1cf7
ms.openlocfilehash: 62b1541414543fc947886374424dadee09759323
ms.lasthandoff: 01/20/2017


---
# <a name="api-apps-overview"></a>Omówienie API Apps
Aplikacje interfejsu API w usłudze Azure App Service udostępniają funkcje, które ułatwiają tworzenie i obsługę interfejsów API oraz korzystanie z nich — zarówno w chmurze, jak i lokalnie. Aplikacje interfejsu API zapewniają zabezpieczenia klasy korporacyjnej, prostą kontrolę dostępu, połączenia hybrydowe, automatyczne generowanie zestawów SDK oraz bezproblemową integrację z usługą [Logic Apps](../logic-apps/logic-apps-what-are-logic-apps.md).

[Usługa Azure App Service](../app-service/app-service-value-prop-what-is.md) to w pełni zarządzana platforma obsługująca scenariusze dotyczące integracji, sieci Web oraz urządzeń przenośnych. Funkcja API Apps należy do zestawu obejmującego cztery typy aplikacji udostępniane przez [usługę Azure App Service](../app-service/app-service-value-prop-what-is.md).

![Typy aplikacji w usłudze Azure App Service](./media/app-service-api-apps-why-best-platform/appservicesuite.png)

## <a name="why-use-api-apps"></a>Dlaczego warto używać aplikacji API Apps?
Oto główne funkcje aplikacji API Apps:

* **Możliwość korzystania z istniejących interfejsów API** — aby używać aplikacji API Apps, nie trzeba zmieniać żadnego kodu istniejących interfejsów API — wystarczy go wdrożyć w aplikacji interfejsu API. Interfejs API może korzystać z dowolnego języka lub platformy obsługiwanej przez usługę App Service, na przykład ASP.NET, C#, Java, PHP, Node.js lub Python.
* **Łatwość użycia** — dzięki zintegrowanej obsłudze [metadanych interfejsu API programu Swagger](http://swagger.io/) interfejsy API są łatwo dostępne dla różnych klientów.  Za pomocą różnych języków, takich jak C#, Java i Javascript, można automatycznie generować kod klienta dla interfejsów API. [Mechanizm CORS](app-service-api-cors-consume-javascript.md) można łatwo konfigurować bez zmieniania kodu. Aby uzyskać więcej informacji, zobacz artykuły [App Service API Apps metadata for API discovery and code generation](app-service-api-metadata.md) (Metadane funkcji App Service API Apps służące do odnajdywania interfejsów API i generowania kodu) oraz [Korzystanie z aplikacji interfejsu API z poziomu języka JavaScript przy użyciu mechanizmu CORS](app-service-api-cors-consume-javascript.md). 
* **Prosta kontrola dostępu** — ochrona aplikacji interfejsu API przed nieuwierzytelnionym dostępem jest możliwa bez wprowadzania zmian w kodzie. Wbudowane usługi uwierzytelniania umożliwiają zabezpieczenie interfejsów API przed dostępem innych usług lub klientów reprezentujących użytkowników. Obsługiwani dostawcy tożsamości obejmują usługę Azure Active Directory, konta Google i Microsoft oraz serwisy Facebook i Twitter. Klienci mogą używać biblioteki ADAL (Active Directory Authentication Library) lub zestawu Mobile Apps SDK. Aby uzyskać więcej informacji, zobacz [Authentication and authorization for API Apps in Azure App Service](app-service-api-authentication.md) (Uwierzytelnianie i autoryzacja aplikacji interfejsu API w usłudze Azure App Service).
* **Integracja z programem Visual Studio** — dedykowane narzędzia w programie Visual Studio usprawniają procesy tworzenia, wdrażania, używania i debugowania Aplikacji interfejsu API oraz zarządzania nimi. Aby uzyskać więcej informacji, zobacz [Announcing the Azure SDK 2.8.1 for .NET](https://azure.microsoft.com/blog/announcing-azure-sdk-2-8-1-for-net/) (Prezentacja zestawu Azure SDK 2.8.1 dla platformy .NET).
* **Integracja z funkcją Logic Apps** — tworzone aplikacje interfejsu API mogą być używane przez funkcję [App Service Logic Apps](../logic-apps/logic-apps-what-are-logic-apps.md).  Aby uzyskać więcej informacji, zobacz artykuły [Using your custom API hosted on App Service with Logic apps](../logic-apps/logic-apps-custom-hosted-api.md) (Używanie niestandardowego interfejsu API hostowanego przez usługę App Service razem z aplikacjami Logic Apps) oraz [New schema version 2015-08-01-preview](../logic-apps/logic-apps-schema-2015-08-01.md) (Nowa wersja schematu 2015-08-01-preview).

Ponadto aplikacja interfejsu API może korzystać z funkcji udostępnianych przez usługi [Web Apps](../app-service-web/app-service-web-overview.md) i [Mobile Apps](../app-service-mobile/app-service-mobile-value-prop.md). Występuje również sytuacja odwrotna: aplikacja sieci Web lub aplikacja mobilna używana do hostowania interfejsu API może korzystać z funkcji API Apps, takich jak metadane programu Swagger, w celu generowania kodu klienta oraz z obsługi mechanizmu CORS zapewniającego międzydomenowy dostęp z przeglądarki. Jedyne różnice między tymi trzema typami aplikacji (interfejsu API, sieci Web i mobilnych) dotyczą ich nazw oraz ikon wyświetlanych w witrynie Azure Portal.

## <a name="whats-the-difference-between-api-apps-and-azure-api-management"></a>Jaka jest różnica między usługą API Apps a usługą Azure API Management?
API Apps i [Azure API Management](../api-management/api-management-key-concepts.md) to usługi, które się uzupełniają:

* Usługa API Management służy do zarządzania interfejsami API. Dodanie frontonu usługi API Management do interfejsu API pozwala na przykład monitorować i ograniczać użycie, manipulować danymi wejściowymi i wyjściowymi czy integrować kilka interfejsów API w jednym punkcie końcowym. Zarządzane Interfejsy API mogą być hostowane w dowolnym miejscu.
* Usługa API Apps umożliwia hostowanie interfejsów API. Ta usługa udostępnia funkcje ułatwiające tworzenie interfejsów API i korzystanie z nich, ale nie pozwala na monitorowanie i ograniczanie przepływności, manipulowanie danymi ani integrowanie interfejsów API tak jak usługa API Management. Jeśli funkcje usługi API Management nie są potrzebne, można hostować interfejsy API w usłudze API Apps.

Na poniższym schemacie przedstawiono użycie usługi API Management w celu korzystania z interfejsów API hostowanych w usłudze API Apps i innych miejscach.

![Usługi Azure API Management i API Apps](./media/app-service-api-apps-why-best-platform/apia-apim.png)

Niektóre funkcje usług API Management i API Apps działają podobnie,  na przykład umożliwiają automatyzację obsługi mechanizmu CORS. W przypadku korzystania z obu usług jednocześnie do obsługi mechanizmu CORS należy używać usługi API Management, ponieważ działa ona jako fronton dla usługi API Apps. 

## <a name="getting-started"></a>Wprowadzenie
Dostępne są samouczki dla różnych platform, które ułatwiają rozpoczęcie korzystania z usługi API Apps przez wdrożenie przykładowego kodu w wybranej aplikacji:

* [ASP.NET](app-service-api-dotnet-get-started.md) 
* [Node.js](app-service-api-nodejs-api-app.md) 
* [Java](app-service-api-java-api-app.md) 

Aby zadać pytanie dotyczące aplikacji interfejsu API, utwórz wątek na [forum usługi API Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureAPIApps). 


