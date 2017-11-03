---
title: "Usługi Azure AD w wersji 2 JS SPA instrukcje konfiguracji — wprowadzenie | Dokumentacja firmy Microsoft"
description: "Jak aplikacje JavaScript SPA można wywołać interfejsu API, które wymagają tokenów dostępu przez punkt końcowy w wersji 2 usługi Azure Active Directory"
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
ms.date: 06/01/2017
ms.author: andret
ms.openlocfilehash: 3d195d0d67f8f82c9450ffd93767917698addee3
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="call-the-microsoft-graph-api-from-a-javascript-single-page-application-spa"></a>Wywołania programu Microsoft Graph API z poziomu aplikacji JavaScript pojedynczej strony (SPA)

W tym przewodniku pokazano, jak JavaScript jednej strony aplikacji JEDNOSTRONICOWEJ mógł zalogować się w osobistym, pracy i kont służbowych uzyskać token dostępu i wywołania interfejsu API programu Graph firmy Microsoft lub innych interfejsów API, które wymagają tokenów dostępu z punktu końcowego usługi Azure Active Directory w wersji 2.

### <a name="how-this-guide-works"></a>Jak działa w tym przewodniku

![Jak działa przykładowej aplikacji wygenerowane przez ten przewodnik](media/active-directory-singlepageapp-javascriptspa-introduction/javascriptspa-intro.png)

<!--start-collapse-->
### <a name="more-information"></a>Więcej informacji

Przykładowa aplikacja utworzone przez ten przewodnik umożliwia SPA JavaScript wysyłać kwerendy interfejsu API programu Microsoft Graph i interfejsu API sieci Web, który akceptuje tokeny od punktu końcowego w wersji 2 usługi Azure Active Directory. W tym scenariuszu po użytkownika logowania token dostępu jest wymagane i dodać do żądań HTTP za pośrednictwem nagłówek autoryzacji. Token nabycia i odnawiania są obsługiwane przez biblioteki uwierzytelniania firmy Microsoft (MSAL).

<!--end-collapse-->

<!--start-collapse-->
### <a name="libraries"></a>Biblioteki

W tym przewodniku korzysta z biblioteki następujące:

|Biblioteka|Opis|
|---|---|
|[msal.js](https://github.com/AzureAD/microsoft-authentication-library-for-js)|Biblioteka uwierzytelniania firmy Microsoft dla podglądu JavaScript|

> [!NOTE]
> *msal.js* celów *punktu końcowego w wersji 2 usługi Azure Active Directory* — co pozwala kont prywatnych, szkolnego i zalogować się i uzyskać tokeny. *Punktu końcowego w wersji 2 usługi Azure Active Directory* ma [pewne ograniczenia](..\active-directory-v2-limitations.md). Jeśli interesuje Cię tylko na kontach służbowych, jak i szkoły, użyj *adal.js* i *punktu końcowego V1*. Aby poznać różnice między punktami końcowymi v1 i v2 odczytu [porównania v1 v2](..\active-directory-v2-compare.md).

<!--end-collapse-->
