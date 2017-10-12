---
title: "Usługa Azure Active Directory dla deweloperów | Microsoft Docs"
description: "Ten artykuł zawiera omówienie logowania się na konta służbowe Microsoft przy użyciu usługi Azure Active Directory."
services: active-directory
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: 5c872c89-ef04-4f4c-98de-bc0c7460c7c2
ms.service: active-directory
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/07/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: dce813b885d492343428428056a2e8aada27b461
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-active-directory-for-developers"></a>Usługa Azure Active Directory dla deweloperów
Azure Active Directory to usługa zarządzania tożsamościami w chmurze, która umożliwia deweloperom bezpieczne logowanie każdego użytkownika mającego konto służbowe bazujące na technologii Microsoft.  W tej dokumentacji pokazano, jak dodać obsługę usługi Azure AD do swojej aplikacji, używając standardowych w branży protokołów uwierzytelniania: OAuth i OpenID Connect.

| | |
| --- | --- |
|[Podstawowe informacje o uwierzytelnianiu](active-directory-authentication-scenarios.md) | Wprowadzenie do uwierzytelniania za pomocą usługi Azure AD |
|[Typy aplikacji](active-directory-authentication-scenarios.md#application-types-and-scenarios) | Przegląd scenariuszy uwierzytelniania obsługiwanych przez usługę Azure AD |                                
                                                                              
## <a name="get-started"></a>Rozpoczęcie pracy
Te instalatory z przewodnikiem pokazują krok po kroku, jak używać naszych bibliotek uwierzytelniania do logowania użytkowników usługi Azure Active Directory.

|  |  |  |  |
| --- | --- | --- | --- |
| <center>![Aplikacje mobilne i klasyczne](./media/active-directory-developers-guide/NativeApp_Icon.png)<br />Aplikacje mobilne i klasyczne</center> | [Omówienie](active-directory-authentication-scenarios.md#native-application-to-web-api)<br /><br />[iOS](active-directory-devquickstarts-ios.md)<br /><br />[Android](active-directory-devquickstarts-android.md) | [.NET](active-directory-devquickstarts-dotnet.md)<br /><br />[Windows](active-directory-devquickstarts-windowsstore.md)<br /><br />[Xamarin](active-directory-devquickstarts-xamarin.md) | [Cordova](active-directory-devquickstarts-cordova.md)<br /><br />[OAuth 2.0](active-directory-protocols-oauth-code.md) |
| <center>![Aplikacje sieci Web](./media/active-directory-developers-guide/Web_app.png)<br />Aplikacje sieci Web</center> | [Omówienie](active-directory-authentication-scenarios.md#web-browser-to-web-application)<br /><br />[ASP.NET](active-directory-devquickstarts-webapp-dotnet.md)<br /><br />[Java](active-directory-devquickstarts-webapp-java.md) | [NodeJS](active-directory-devquickstarts-openidconnect-nodejs.md)<br /><br />[OpenID Connect 1.0](active-directory-protocols-openid-connect-code.md) |  |
| <center>![Aplikacje jednostronicowe](./media/active-directory-developers-guide/SPA.png)<br />Aplikacje jednostronicowe</center> | [Omówienie](active-directory-authentication-scenarios.md#single-page-application-spa)<br /><br />[AngularJS](active-directory-devquickstarts-angular.md)<br /><br />[JavaScript](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi) |  |  |
| <center>![Interfejsy API sieci Web](./media/active-directory-developers-guide/Web_API.png)<br />Interfejsy API sieci Web</center> | [Omówienie](active-directory-authentication-scenarios.md#web-application-to-web-api)<br /><br />[ASP.NET](active-directory-devquickstarts-webapi-dotnet.md)<br /><br />[NodeJS](active-directory-devquickstarts-webapi-nodejs.md) | &nbsp; |
| <center>![Usługa-usługa](./media/active-directory-developers-guide/Service_App.png)<br />Usługa-usługa</center> | [Omówienie](active-directory-authentication-scenarios.md#daemon-or-server-application-to-web-api)<br /><br />[.NET](active-directory-code-samples.md#server-or-daemon-application-to-web-api)<br /><br />[Poświadczenia klienta OAuth 2.0](active-directory-protocols-oauth-service-to-service.md) |  |

## <a name="guides"></a>Przewodniki
Te artykuły zawierają informacje o tym, jak wykonywać typowe zadania w usłudze Azure Active Directory.

|                                                                           |  |
|---------------------------------------------------------------------------| --- |
|[Rejestrowanie aplikacji](active-directory-integrating-applications.md)           | Jak zarejestrować aplikację w usłudze Azure AD |
|[Aplikacje z wieloma dzierżawami](active-directory-devhowto-multi-tenant-overview.md)    | Jak zalogować dowolne konto służbowe Microsoft |
|[OAuth i OpenID Connect](active-directory-protocols-openid-connect-code.md)| Jak logować użytkowników i wywoływać interfejsy API sieci Web za pomocą naszych nowoczesnych protokołów uwierzytelniania |
|[Więcej przewodników...](active-directory-developers-guide-index.md#guides)        |     |

## <a name="reference"></a>Dokumentacja
Te artykuły zawierają szczegółowe informacje o interfejsach API, komunikatach protokołów i terminach używanych w usłudze Azure Active Directory.

|                                                                                   | |
| ----------------------------------------------------------------------------------| --- |
| [Biblioteki uwierzytelniania (ADAL)](active-directory-authentication-libraries.md)   | Omówienie bibliotek i zestawów SDK dostarczanych przez usługę Azure AD |
| [Przykłady kodu](active-directory-code-samples.md)                                  | Lista wszystkich przykładów kodu usługi Azure AD |
| [Słownik](active-directory-dev-glossary.md)                                      | Terminologia i definicje słów używanych w tej dokumentacji |
| [Więcej materiałów referencyjnych...](active-directory-developers-guide-index.md#reference)|     |

## <a name="help--support"></a>Pomoc i obsługa techniczna
To są najlepsze miejsca, aby uzyskać pomoc w programowaniu dla usługi Azure Active Directory.

|  |  
|---|
|[Tagi `azure-active-directory` i `adal` w witrynie Stack Overflow](http://stackoverflow.com/questions/tagged/azure-active-directory+or+adal)      |
|[Opinia o usłudze Azure Active Directory](https://feedback.azure.com/forums/169401-azure-active-directory/category/164757-developer-experiences)|
| [Wypróbuj usługę Microsoft Dev Chat (bezpłatnie przez ograniczony czas)](http://aka.ms/devchat) |

<br />

> [!NOTE]
> Jeśli musisz logować konta osobiste Microsoft, warto rozważyć skorzystanie z [punktu końcowego usługi Azure AD v2.0](active-directory-appmodel-v2-overview.md).  Punkt końcowy usługi Azure AD v2.0 stanowi ujednolicenie kont osobistych i kont służbowych Microsoft (z usługi Azure AD) do postaci jednego systemu uwierzytelniania.
