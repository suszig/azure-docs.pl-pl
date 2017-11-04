---
title: "Usługi Azure AD w wersji 2 dla systemu Android pobieranie rozpoczęte — wprowadzenie | Dokumentacja firmy Microsoft"
description: "Jak uzyskać token dostępu i wywołania interfejsu API programu Microsoft Graph lub interfejsów API, które wymagają tokenów dostępu z punktu końcowego w wersji 2 usługi Azure Active Directory aplikacji systemu Android"
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
ms.openlocfilehash: d933781713456f73aa76db557fdf35672dfb2a29
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="call-the-microsoft-graph-api-from-an-android-app"></a>Wywoływanie Microsoft Graph API z aplikacji systemu Android

W tym przewodniku pokazano, jak natywnych aplikacji systemu Android mogą pobrać token dostępu i wywołania interfejsu API programu Graph firmy Microsoft lub innych interfejsów API, które wymagają tokenów dostępu z punktu końcowego w wersji 2 usługi Azure Active Directory.

Na końcu tego przewodnika aplikacja będzie mogła wywołać interfejs API chronionych przy użyciu konta osobiste (w tym outlook.com, live.com i inne) oraz pracy i służbowych z firmy lub organizacji, która ma usługę Azure Active Directory.  

### <a name="how-this-sample-works"></a>Jak działa w tym przykładzie
![Jak działa w tym przykładzie](media/active-directory-mobileanddesktopapp-android-intro/android-intro.png)

Przykładowe utworzone przez ten przewodnik jest oparty na scenariusz, w których aplikacja systemu Android służy do interfejsu API sieci Web, który akceptuje tokeny od usługi Azure Active Directory v2 punktu końcowego — w takim przypadku interfejsu API programu Microsoft Graph. W tym scenariuszu token zostanie dodany do żądań HTTP za pośrednictwem nagłówek autoryzacji. Token nabycia i odnawiania jest obsługiwany przez bibliotekę uwierzytelniania firmy Microsoft (MSAL).

### <a name="pre-requisites"></a>Wymagania wstępne
* Ten przewodnik instalacji koncentruje się na Android Studio, ale dopuszczalne jest również inne środowiska programowania aplikacji dla systemu Android. 
* Zestaw SDK systemu android 21 lub nowszy jest wymagany (zalecane 25 zestawu SDK).
* Google Chrome lub przeglądarki sieci web przy użyciu karty niestandardowy jest wymagany dla tej wersji programu Microsoft biblioteki uwierzytelniania (MSAL) dla systemu Android.

> Uwaga: Google Chrome nie jest uwzględniony w Visual Studio Emulator for Android. Firma Microsoft zaleca do przetestowania tego kodu na Emulator z interfejsu API 25 lub obraz z interfejsu API 21 lub nowszego z Google Chrome zainstalował.


### <a name="how-to-handle-token-acquisition-to-access-a-protected-web-api"></a>Sposób obsługi nabycia tokenu można uzyskać dostępu do chronionego interfejsu API sieci Web

Po użytkownik jest uwierzytelniany, przykładowa aplikacja odbiera token, który może służyć do badania interfejsu API programu Microsoft Graph i interfejsu API sieci Web zabezpieczonych przez usługi Microsoft Azure Active Directory w wersji 2.

Interfejsy API, takich jak Microsoft Graph wymagają tokenu dostępu, aby umożliwić uzyskiwanie dostępu do określonych zasobów — na przykład do odczytu profilu użytkownika, kalendarz dostęp użytkownika lub Wyślij wiadomość e-mail. Aplikacja może żądać tokenu dostępu przy użyciu MSAL dostępu do tych zasobów, określając zakresy interfejsu API. Ten token dostępu jest dodawane do nagłówka HTTP autoryzacji dla każdego wywołania wykonane w stosunku do chronionego zasobu. 

MSAL zarządza buforowanie i odświeżanie tokenów dostępu, więc nie trzeba aplikacji.

### <a name="libraries"></a>Biblioteki

W tym przewodniku korzysta z bibliotek następujące:

|Biblioteka|Opis|
|---|---|
|[COM.microsoft.Identity.Client](http://javadoc.io/doc/com.microsoft.identity.client/msal)|Biblioteka uwierzytelniania firmy Microsoft (MSAL)|
