---
title: "ASP.NET w wersji 2 usługi Azure AD w sieci Web serwera pobieranie rozpoczęte — wprowadzenie | Dokumentacja firmy Microsoft"
description: "Implementowanie logowania firmy Microsoft dla rozwiązania ASP.NET z aplikacji opartych na przeglądarce sieci web tradycyjnych przy użyciu standardowego protokołu OpenID Connect"
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/09/2017
ms.author: andret
ms.custom: aaddev
ms.openlocfilehash: 8062923b6270ec6253dc231a3db4333cf4666b42
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="add-sign-in-with-microsoft-to-an-aspnet-web-app"></a>Dodaj logowanie z firmy Microsoft do aplikacji sieci web platformy ASP.NET

W tym przewodniku pokazano, jak do implementowania logowania z firmą Microsoft za pomocą rozwiązania ASP.NET MVC z tradycyjnego aplikacji sieci web opartej na przeglądarce przy użyciu protokołu OpenID Connect. 

Na końcu tego przewodnika aplikacja będzie można zaakceptować znak ins osobiste konta (w tym outlook.com, live.com i inne), a także pracy i służbowych z firmy lub organizacji, która ma być zintegrowane z usługą Azure Active Directory. 

> W tym przewodniku wymaga programu Visual Studio 2015 Update 3 lub Visual Studio 2017 r.  Nie masz?  [Bezpłatnie pobrać Visual Studio 2017 r.](https://www.visualstudio.com/downloads/)

## <a name="how-this-guide-works"></a>Jak działa w tym przewodniku

![Jak działa w tym przewodniku](media/active-directory-serversidewebapp-aspnetwebappowin-intro/aspnetbrowsergeneral.png)

W tym przewodniku opiera się na scenariusz, w których przeglądarką uzyskuje dostęp do witryny sieci web ASP.NET, żądanie użytkownika do uwierzytelniania za pośrednictwem przycisku Zaloguj. W tym scenariuszu większość pracy do renderowania strony sieci web występuje po stronie serwera.

## <a name="libraries"></a>Biblioteki

W tym przewodniku korzysta z bibliotek następujące:

|Biblioteka|Opis|
|---|---|
|[Microsoft.Owin.Security.OpenIdConnect](https://www.nuget.org/packages/Microsoft.Owin.Security.OpenIdConnect/)|Oprogramowanie pośredniczące, które włącza aplikację na potrzeby uwierzytelniania OpenIdConnect|
|[Microsoft.Owin.Security.Cookies](https://www.nuget.org/packages/Microsoft.Owin.Security.Cookies)|Oprogramowanie pośredniczące, który umożliwia aplikacjom obsługę plików cookie sesji użytkownika|
|[Microsoft.Owin.Host.SystemWeb](https://www.nuget.org/packages/Microsoft.Owin.Host.SystemWeb)|Umożliwia aplikacji OWIN do uruchomienia na serwerze IIS za pomocą potoku żądania programu ASP.NET|

