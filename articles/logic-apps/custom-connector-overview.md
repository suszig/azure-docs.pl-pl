---
title: "Przegląd łączników niestandardowych — usługi Azure Logic Apps | Dokumentacja firmy Microsoft"
description: "Informacje o tworzeniu niestandardowych łączników programu obsługi i rozszerzanie scenariuszy integracji"
author: ecfan
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 
ms.service: logic-apps
ms.workload: logic-apps
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/22/2017
ms.author: LADocs; estfan
ms.openlocfilehash: 0515b21bc7c7cc737e14fda016606bbea1aab476
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="custom-connectors-overview"></a>Omówienie łączników niestandardowych

Bez pisania żadnego kodu, można tworzyć przepływy pracy lub aplikacji za pomocą [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/), [Microsoft Flow](https://flow.microsoft.com), lub [Microsoft PowerApps](https://powerapps.microsoft.com). Aby pomóc w zintegrowaniu procesów danych i business, oferować te usługi [100 + łączniki](../connectors/apis-list.md), a nie tylko dla usług firmy Microsoft i produktów, takich jak Azure i SQL Server, ale innych usług, takich jak GitHub, usługi Salesforce, Twitter i . 

Czasami jednak można wywoływać interfejsy API, usług i systemów, które nie są dostępne jako łączniki wbudowane. Do obsługi bardziej dopasowane scenariuszy dla firm i potrzeby użytkowników, można tworzyć *łączników niestandardowych* własne wyzwalacze i akcje.
Niestandardowe łączniki rozwiń integracji, reach, odnajdywania i użyj usługi lub produktu, co może pomóc zwiększyć i przyspieszyć wdrożenia klienta.

Na przykład ten diagram przedstawia interakcje między interfejs API sieci Web niestandardowego łącznika Logic Apps utworzone dla tego interfejsu API oraz aplikacji logiki, która współdziała z tego interfejsu API za pomocą łącznika niestandardowych:

![Omówienie pojęć dla interfejsu API sieci Web, łącznik niestandardowy i aplikacji logiki](./media/custom-connector-overview/custom-connector-conceptual.png)

W tym omówieniu przedstawiono ogólne zadania wysokiego poziomu do tworzenia, zabezpieczanie, rejestrowanie i przy użyciu oraz opcjonalnie udostępnianie lub poświadczania łączników:

![Kroki tworzenia łącznika niestandardowego](./media/custom-connector-overview/authoring-steps.png)

## <a name="prerequisites"></a>Wymagania wstępne

Aby utworzyć łącznik programu od początku do końca, potrzebne są następujące elementy:

* Subskrypcja platformy Azure. Jeśli nie masz subskrypcji, można uruchomić z poziomu [bezpłatne konto platformy Azure](https://azure.microsoft.com/free/). W przeciwnym razie zarejestrować się w celu [subskrypcji płatność za rzeczywiste użycie](https://azure.microsoft.com/pricing/purchase-options/).

* Interfejs API RESTful niektórych typu dostępu uwierzytelnionego. Aby uzyskać więcej informacji, zobacz [Tworzenie niestandardowych łączniki z interfejsów API sieci Web](../logic-apps/custom-connector-build-web-api-app-tutorial.md). Wzorców, używanych do wyzwalacze i akcje z łącznika, zobacz [Tworzenie niestandardowych interfejsów API, który można wywołać z przepływów pracy aplikacji logiki](../logic-apps/logic-apps-create-api-app.md).

* Dowolny element w tym miejscu:

  * [Pliku OpenAPI 2.0](https://github.com/OAI/OpenAPI-Specification/blob/master/versions/2.0.md), wcześniej znana jako struktury Swagger
  * Adres URL do definicji OpenAPI
  * A [kolekcji Postman](../logic-apps/custom-connector-api-postman-collection.md) do interfejsu API 

  Jeśli nie masz żadnego z tych elementów, udostępniamy wskazówki dla Ciebie.

* Opcjonalnie: Obrazu do użycia jako ikona łącznik niestandardowy

## <a name="1-build-your-restful-api"></a>1. Tworzenie interfejsu API RESTful

Z technicznego punktu widzenia łącznika jest otokę interfejsu API REST, który jest oparty na OpenAPI (dawniej Swagger) specyfikacji i umożliwia usługi podstawowej, zwróć się do aplikacji logiki, przepływ lub rozwiązaniu PowerApps. Dlatego najpierw należy pełni funkcjonalnej interfejsu API przed utworzeniem łącznik niestandardowy. 

Do interfejsu API, można użyć dowolnego języka i platformy. Dla technologii firmy Microsoft firma Microsoft zaleca jedną z tych platform:

* [Azure Functions](https://azure.microsoft.com/services/functions/)
* [Azure Web Apps](https://azure.microsoft.com/services/app-service/web/)
* [Aplikacje interfejsu API platformy Azure](https://azure.microsoft.com/services/app-service/api/)

Na przykład, w tym samouczku przedstawiono [sposób tworzenia łącznika niestandardowego składnika Web API](../logic-apps/custom-connector-build-web-api-app-tutorial.md). Wzorców, używanych do wyzwalacze i akcje z łącznika, zobacz [Tworzenie niestandardowych interfejsów API, który można wywołać z przepływów pracy aplikacji logiki](../logic-apps/logic-apps-create-api-app.md).

## <a name="2-secure-your-api"></a>2. Zabezpieczanie interfejsu API

Można użyć tych standardów uwierzytelniania dla łączników i interfejsów API:

   * [OAuth 2.0](https://oauth.net/2/), takie jak [usługi Azure Active Directory](https://azure.microsoft.com/develop/identity/) lub określonych usług, takich jak Dropbox, GitHub i SalesForce
   * Ogólny OAuth 2.0
   * [Uwierzytelnianie podstawowe](https://swagger.io/docs/specification/authentication/basic-authentication/)
   * [Klucz interfejsu API](https://swagger.io/docs/specification/authentication/api-keys/)

Można skonfigurować uwierzytelniania usługi Azure Active Directory (Azure AD) do interfejsu API w portalu Azure, dzięki czemu nie trzeba zaimplementować uwierzytelnianie za pomocą kodu. Lub może wymagać i wymusić uwierzytelnianie za pomocą kodu Twój interfejs API. 

Aby uzyskać więcej informacji wykonaj odpowiednie samouczki:

* [Logic Apps: Łącznik niestandardowy bezpieczny](../logic-apps/custom-connector-azure-active-directory-authentication.md)
* [Przepływu: Łącznik niestandardowy bezpieczny](https://ms.flow.microsoft.com/documentation/customapi-azure-resource-manager-tutorial/)
* [Rozwiązania PowerApps: Łącznik niestandardowy bezpieczny](https://powerapps.microsoft.com/tutorials/customapi-azure-resource-manager-tutorial/)

## <a name="3-describe-your-api"></a>3. Opis interfejsu API 

Przy założeniu, że Twój interfejs API zawiera pewien typ uwierzytelniony dostęp, musisz opisano interfejsu i operacji Twój interfejs API, dzięki czemu aplikacje logiki, przepływ lub rozwiązaniu PowerApps może komunikować się z interfejsem API.
Użyj jednej z tych definicji standardowego w branży:

* [Pliku OpenAPI 2.0](https://swagger.io/) można uruchomić budynku z istniejącym plikiem OpenAPI.

  Jeśli jesteś nowym użytkownikiem OpenAPI, odwiedź stronę [wprowadzenie do korzystania z programu Swagger](http://swagger.io/getting-started/) w witrynie swagger.io.
  Aby uzyskać przykładowy plik OpenAPI, zobacz [dokumentacji interfejsu API z analizy tekstu](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/export?DocumentFormat=Swagger&ApiName=Azure). 

* Kolekcja Postman, który automatycznie generuje plik OpenAPI dla Ciebie. Gdy plik OpenAPI nie został wcześniej i nie chcesz utworzyć, można nadal łatwo utworzyć niestandardowy łącznik przy użyciu kolekcji Postman.

  Jeśli jesteś nowym użytkownikiem Postman, [zainstalowania aplikacji Postman](https://www.getpostman.com/apps) z ich lokacji. Aby uzyskać więcej informacji, zobacz [opisano łączników niestandardowych z Postman](../logic-apps/custom-connector-api-postman-collection.md).

> [!IMPORTANT]
> Rozmiar pliku musi być mniejszy niż 1 MB.

W tle Logic Apps, przepływu i rozwiązanie PowerApps ostatecznie Użyj OpenAPI, analizuje kolekcji Postman i tłumaczy kolekcji do pliku definicji OpenAPI. Mimo że kolekcje OpenAPI 2.0 i Postman używają różnych formatach, są niezależne od języka, czytelną dokumentów, które opisują Twój interfejs API operacji i parametry. Możesz wygenerować tych dokumentów na podstawie różnych narzędzi, na podstawie języka i platformy użyć interfejsu API. Po zarejestrowaniu Twojego łącznika, można także utworzyć plik OpenAPI.

Na przykład można utworzyć pliku OpenAPI lub zbiór Postman z dowolnego punktu końcowego interfejsu API REST, w tym:

* Publicznie dostępnych łączników, na przykład [serwis Spotify](https://developer.spotify.com/), [Slack](https://api.slack.com/), [Rackspace](http://docs.rackspace.com/)i tak dalej

* Interfejs API, który został utworzony i wdrożony dowolnego dostawcy hostingu w chmurze, takich jak Azure, Heroku, Google Cloud i

* Niestandardowy interfejs API — biznesowych, która została wdrożona w sieci, ale tylko wtedy, gdy ten interfejs API jest narażony na publiczny internet

## <a name="4-register-your-connector"></a>4. Zarejestruj użytkownika łącznika

Proces rejestracji pomaga Logic Apps przepływu, lub rozwiązaniu PowerApps zrozumienia charakterystyki Twój interfejs API, łącznie z opisem, uwierzytelniania i operacje, takie jak parametry i wyniki dla każdej operacji. Podczas uruchamiania procesu rejestracji, musisz podać plik OpenAPI lub kolekcji Postman automatycznie wypełni pola metadanych Kreatora rejestracji. W dowolnym momencie można edytować wartości tych pól.

![Opis interfejsu API](./media/custom-connector-overview/choose-api-definition-file.png)

Aby zarejestrować Twoje łącznik, wykonaj odpowiednią samouczek:

* [Logic Apps: Zarejestrować Twojego łącznika](../logic-apps/logic-apps-custom-connector-register.md)
* [Przepływ: Zarejestrować Twojego łącznika](https://ms.flow.microsoft.com/documentation/register-custom-api/#register-your-custom-connector)
* [Rozwiązanie PowerApps: Zarejestrować Twojego łącznika](https://powerapps.microsoft.com/tutorials/register-custom-api/#register-your-custom-connector)

## <a name="5-use-your-connector-in-a-logic-app-flow-or-app"></a>5. Korzystania z łącznika w aplikacji logiki, przepływu lub aplikacji 

Korzystając z łącznika w taki sam sposób, że używasz łączników zarządzanych przez program Microsoft. Na przykład w przepływie pracy aplikacji logiki, Dodaj łącznik niestandardowy, możesz utworzyć połączenie do interfejsu API i wywołaj wszystkie operacje zawiera interfejs API w taki sam sposób jak wywołanie operacji łączników zarządzany przez firmę Microsoft.

## <a name="6-share-your-connector"></a>6. Udostępnianie Twojego łącznika 

Łącznik programu można udostępniać użytkownikom w organizacji w taki sam sposób, że udostępnianie zasobów aplikacji logiki, przepływ lub rozwiązaniu PowerApps. Mimo że udostępnianie jest opcjonalny, konieczne może być scenariuszy, w którym chcesz udostępnić innym użytkownikom łączników.

Zarejestrowane, ale Niepoświadczone łączników niestandardowych działają podobnie łączniki zarządzany przez firmę Microsoft, ale są widoczne i dostępne *tylko* autora łącznika i użytkowników, którzy mają tej samej dzierżawy usługi Azure Active Directory i subskrypcji platformy Azure w przypadku aplikacji logiki w regionie, w których te aplikacje są wdrożone. Następnym krokiem opisano, jak można udostępniać Twojego łącznika użytkowników zewnętrznych poza te granice, na przykład użytkownikom Logic Apps, przepływu i rozwiązaniu PowerApps.

> [!IMPORTANT]
> Jeśli łącznik zostanie udostępniona, inne rozpoczęcie zależą od tego łącznika. 
> ***Usunięcie łącznika programu powoduje usunięcie wszystkich połączeń z tego łącznika.***

* [Logic Apps: Łącznik programu udostępnić](../logic-apps/logic-apps-custom-connector-register.md)
* [Przepływ: Udostępnianie Twojego łącznika](https://ms.flow.microsoft.com/documentation/register-custom-api/#share-your-custom-connector)
* [Rozwiązanie PowerApps: Udostępnianie Twojego łącznika](https://powerapps.microsoft.com/tutorials/register-custom-api/#share-your-custom-connector)

## <a name="7-certify-your-connector"></a>7. Certyfikowanie Twojego łącznika

Aby zgłosić udostępniać Twojego łącznika wszystkich użytkowników w aplikacji logiki, przepływu i rozwiązanie PowerApps, opcjonalnie z łącznika do certyfikacji firmy Microsoft. Ten proces zapoznaje się z łącznika, sprawdza zgodność techniczne i zawartości i weryfikuje funkcji Logic Apps, przepływu i rozwiązanie PowerApps, zanim Microsoft można opublikować Twojego łącznika. Dowiedz się [temat dotyczący przesyłania Twojego łącznika do certyfikacji Microsoft](../logic-apps/custom-connector-submit-certification.md).

## <a name="get-support"></a>Uzyskiwanie pomocy technicznej

* Obsługę dołączania i rozwoju poczty e-mail [ condevhelp@microsoft.com ](mailto:condevhelp@microsoft.com). Firma Microsoft aktywnie monitoruje tego konta na problemów i pytania deweloperów i kieruje je do odpowiednich zespołu. 

* Często zadawane pytania, zobacz [łącznika niestandardowego — często zadawane pytania](../logic-apps/custom-connector-faq.md)

## <a name="next-steps"></a>Następne kroki

* [Tworzenie niestandardowego łącznika z interfejsu API sieci Web](../logic-apps/custom-connector-build-web-api-app-tutorial.md)
* [Konfigurowanie uwierzytelniania dla łączników niestandardowych](../logic-apps/custom-connector-azure-active-directory-authentication.md)
* [Opisz łączników niestandardowych z kolekcjami Postman](../logic-apps/custom-connector-api-postman-collection.md)
* [Przedstawia łączników niestandardowych do certyfikacji firmy Microsoft](../logic-apps/custom-connector-submit-certification.md)
