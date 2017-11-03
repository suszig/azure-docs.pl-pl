---
title: "Azure AD w wersji 2 dla systemu iOS Getting Started — wprowadzenie | Dokumentacja firmy Microsoft"
description: "Jak aplikacje systemu iOS (Swift) można wywołać interfejsu API, które wymagają tokenów dostępu przez punkt końcowy w wersji 2 usługi Azure Active Directory"
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/09/2017
ms.author: andret
ms.openlocfilehash: 948693c8501ecc46a1508e5ea085846d0910783e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="call-the-microsoft-graph-api-from-an-ios-app"></a>Wywołanie interfejsu API programu Microsoft Graph z aplikacji systemu iOS

W tym przewodniku pokazano, jak aplikacji natywnej dla systemu iOS (Swift) można uzyskać token dostępu i wywołania interfejsu API programu Graph firmy Microsoft lub innych interfejsów API, które wymagają tokenów dostępu z punktu końcowego w wersji 2 usługi Azure Active Directory.

Na końcu tego przewodnika aplikacja będzie mogła wywołać interfejs API chronionych przy użyciu konta osobiste (w tym outlook.com, live.com i inne) oraz pracy i służbowych z firmy lub organizacji, która ma usługę Azure Active Directory.

> ### <a name="pre-requisites"></a>Wymagania wstępne
> - XCode 8.x jest wymagane dla tego przewodnika. Możesz pobrać XCode [w tym miejscu](https://geo.itunes.apple.com/us/app/xcode/id497799835?mt=12 "adresu URL pobierania w środowisku XCode").
> - [Carthage](https://github.com/Carthage/Carthage) dla pakietu administracyjnego

### <a name="how-this-guide-works"></a>Jak działa w tym przewodniku

![Jak działa w tym przewodniku](media/active-directory-mobileanddesktopapp-ios-introduction/iosintro.png)

Przykładowa aplikacja utworzone przez ten przewodnik umożliwia aplikacji systemu iOS do zapytania interfejsu API programu Microsoft Graph i interfejsu API sieci Web, który akceptuje tokeny od punktu końcowego w wersji 2 usługi Azure Active Directory. W tym scenariuszu token zostanie dodany do żądań HTTP za pośrednictwem nagłówek autoryzacji. Token nabycia i odnawiania jest obsługiwany przez bibliotekę uwierzytelniania firmy Microsoft (MSAL).


### <a name="handling-token-acquisition-for-accessing-protected-web-apis"></a>Obsługa tokenów nabycia do uzyskiwania dostępu do chronionego interfejsów API sieci Web

Po użytkownik jest uwierzytelniany, przykładowa aplikacja odbiera token, który może służyć do badania interfejsu API programu Microsoft Graph i interfejsu API sieci Web zabezpieczonych przez usługi Microsoft Azure Active Directory w wersji 2.

Interfejsy API, takich jak Microsoft Graph wymagają tokenu dostępu, aby umożliwić uzyskiwanie dostępu do określonych zasobów — na przykład do odczytu profilu użytkownika, kalendarz dostęp użytkownika lub Wyślij wiadomość e-mail. Aplikacja może wysłać żądanie tokenu dostępu przy użyciu MSAL za pośrednictwem interfejsu API zakresów. Ten token dostępu jest dodawane do nagłówka HTTP autoryzacji dla każdego wywołania wykonane w stosunku do chronionego zasobu.

MSAL zarządza buforowanie i odświeżanie tokenów dostępu, więc nie trzeba aplikacji.


### <a name="nuget-packages"></a>Pakiety NuGet

W tym przewodniku używa następujących pakietów NuGet:

|Biblioteka|Opis|
|---|---|
|[MSAL.framework](https://github.com/AzureAD/microsoft-authentication-library-for-objc)|Podgląd biblioteki uwierzytelniania firmy Microsoft dla systemu iOS|

