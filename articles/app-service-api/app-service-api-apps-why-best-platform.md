<properties 
    pageTitle="Wprowadzenie do API Apps | Microsoft Azure" 
    description="Dowiedz się, jak usługa Azure App Service pomaga tworzyć i obsługiwać interfejsy API RESTful oraz z nich korzystać." 
    services="app-service\api" 
    documentationCenter=".net" 
    authors="tdykstra" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-api" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="05/03/2016" 
    ms.author="tdykstra"/>

# Omówienie API Apps

Aplikacje interfejsu API w usłudze Azure App Service udostępniają funkcje, które ułatwiają tworzenie i obsługę interfejsów API oraz korzystanie z nich — zarówno w chmurze, jak i lokalnie. Aplikacje interfejsu API zapewniają zabezpieczenia klasy korporacyjnej, prostą kontrolę dostępu, połączenia hybrydowe, automatyczne generowanie zestawów SDK oraz bezproblemową integrację z usługą [Logic Apps](../app-service-logic/app-service-logic-what-are-logic-apps.md).

[Usługa Azure App Service](../app-service/app-service-value-prop-what-is.md) to w pełni zarządzana platforma obsługująca scenariusze dotyczące integracji, sieci Web oraz urządzeń przenośnych. Aplikacje API Apps należą do zestawu obejmującego cztery typy aplikacji udostępniane przez [usługę Azure App Service](../app-service/app-service-value-prop-what-is.md).

![Typy aplikacji w usłudze Azure App Service](./media/app-service-api-apps-why-best-platform/appservicesuite.png)

## Dlaczego warto używać aplikacji API Apps?

Oto główne funkcje aplikacji API Apps:

- **Możliwość korzystania z istniejących interfejsów API** — aby używać aplikacji API Apps, nie trzeba zmieniać żadnego kodu istniejących interfejsów API — wystarczy go wdrożyć w aplikacji interfejsu API. Interfejs API może korzystać z dowolnego języka lub platformy obsługiwanej przez usługę App Service, na przykład ASP.NET, C#, Java, PHP, Node.js lub Python.

- **Łatwość użycia** — dzięki zintegrowanej obsłudze [metadanych interfejsu API programu Swagger](http://swagger.io/) interfejsy API są łatwo dostępne dla różnych klientów.  Za pomocą różnych języków, takich jak C#, Java i Javascript, można automatycznie generować kod klienta dla interfejsów API. [Mechanizm CORS](app-service-api-cors-consume-javascript.md) można łatwo konfigurować bez zmieniania kodu. Aby uzyskać więcej informacji, zobacz artykuły [App Service API Apps metadata for API discovery and code generation](app-service-api-metadata.md) (Metadane API Apps w usłudze App Service służące do odnajdywania interfejsów API i generowania kodu) oraz [Korzystanie z aplikacji interfejsu API z poziomu języka JavaScript przy użyciu mechanizmu CORS](app-service-api-cors-consume-javascript.md). 

- **Prosta kontrola dostępu** — ochrona aplikacji interfejsu API przed nieuwierzytelnionym dostępem jest możliwa bez wprowadzania zmian w kodzie. Wbudowane usługi uwierzytelniania umożliwiają zabezpieczenie interfejsów API przed dostępem innych usług lub klientów reprezentujących użytkowników. Obsługiwani dostawcy tożsamości obejmują usługę Azure Active Directory, konta Google i Microsoft oraz serwisy Facebook i Twitter. Klienci mogą używać biblioteki ADAL (Active Directory Authentication Library) lub zestawu Mobile Apps SDK. Aby uzyskać więcej informacji, zobacz [Authentication and authorization for API Apps in Azure App Service](app-service-api-authentication.md) (Uwierzytelnianie i autoryzacja API Apps w usłudze Azure App Service).

- **Integracja z programem Visual Studio** — dedykowane narzędzia w programie Visual Studio usprawniają procesy tworzenia, wdrażania, używania i debugowania Aplikacji interfejsu API oraz zarządzania nimi. Aby uzyskać więcej informacji, zobacz [Announcing the Azure SDK 2.8.1 for .NET](/blog/announcing-azure-sdk-2-8-1-for-net/) (Prezentacja zestawu Azure SDK 2.8.1 dla platformy .NET).

- **Integracja z usługą Logic Apps** — tworzone Aplikacje interfejsu API mogą być używane przez [aplikacje Logic Apps w usłudze App Service](../app-service-logic/app-service-logic-what-are-logic-apps.md).  Aby uzyskać więcej informacji, zobacz artykuły [Using your custom API hosted on App Service with Logic apps](../app-service-logic/app-service-logic-custom-hosted-api.md) (Używanie niestandardowego interfejsu API hostowanego przez usługę App Service razem z aplikacjami Logic Apps) oraz [New schema version 2015-08-01-preview](../app-service-logic/app-service-logic-schema-2015-08-01.md) (Nowa wersja schematu 2015-08-01-preview).

Ponadto aplikacja interfejsu API może korzystać z funkcji udostępnianych przez usługi [Web Apps](../app-service-web/app-service-web-overview.md) i [Mobile Apps](../app-service-mobile/app-service-mobile-value-prop.md). Występuje również sytuacja odwrotna: aplikacja sieci Web lub aplikacja mobilna używana do hostowania interfejsu API może korzystać z funkcji API Apps, takich jak metadane programu Swagger, w celu generowania kodu klienta oraz z obsługi mechanizmu CORS zapewniającego międzydomenowy dostęp z przeglądarki. Jedyne różnice między tymi trzema typami aplikacji (interfejsu API, sieci Web i mobilnych) dotyczą ich nazw oraz ikon wyświetlanych w portalu Azure.

## Jaka jest różnica między usługą API Apps a usługą Azure API Management?

API Apps i [Azure API Management](../api-management/api-management-key-concepts.md) to usługi, które się uzupełniają:

* Usługa API Management służy do zarządzania interfejsami API. Dodanie frontonu usługi API Management do interfejsu API pozwala na przykład monitorować i ograniczać użycie, manipulować danymi wejściowymi i wyjściowymi czy integrować kilka interfejsów API w jednym punkcie końcowym. Zarządzane Interfejsy API mogą być hostowane w dowolnym miejscu.
* Usługa API Apps umożliwia hostowanie interfejsów API. Ta usługa udostępnia funkcje ułatwiające tworzenie interfejsów API i korzystanie z nich, ale nie pozwala na monitorowanie i ograniczanie przepływności, manipulowanie danymi ani integrowanie interfejsów API tak jak usługa API Management. Jeśli funkcje usługi API Management nie są potrzebne, można hostować interfejsy API w usłudze API Apps.

Na poniższym schemacie przedstawiono użycie usługi API Management w celu korzystania z interfejsów API hostowanych w usłudze API Apps i innych miejscach.

![Usługi Azure API Management i API Apps](./media/app-service-api-apps-why-best-platform/apia-apim.png)

Niektóre funkcje usług API Management i API Apps działają podobnie,  na przykład umożliwiają automatyzację obsługi mechanizmu CORS. W przypadku korzystania z obu usług jednocześnie do obsługi mechanizmu CORS należy używać usługi API Management, ponieważ działa ona jako fronton dla usługi API Apps. 

## Wprowadzenie

Dostępne są samouczki dla różnych platform, które ułatwiają rozpoczęcie korzystania z usługi API Apps przez wdrożenie przykładowego kodu w wybranej aplikacji:

* [ASP.NET](app-service-api-dotnet-get-started.md) 
* [Node.js](app-service-api-nodejs-api-app.md) 
* [Java](app-service-api-java-api-app.md) 

Aby zadać pytanie dotyczące aplikacji interfejsu API, utwórz wątek na [forum usługi API Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureAPIApps). 



<!--HONumber=Jun16_HO2-->


