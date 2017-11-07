---
title: "Punktu końcowego v2.0 usługi Azure Active Directory | Dokumentacja firmy Microsoft"
description: "Wprowadzenie do kompilowania aplikacji za pomocą logowanie zarówno Account firmy Microsoft, jak i usługi Azure Active Directory."
services: active-directory
documentationcenter: 
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: 2dee579f-fdf6-474b-bc2c-016c931eaa27
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/01/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: a6a7c6bdf3deaee3a3949fe409a7fab6b7664695
ms.sourcegitcommit: 295ec94e3332d3e0a8704c1b848913672f7467c8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/06/2017
---
# <a name="sign-in-microsoft-account--azure-ad-users-in-a-single-app"></a>Logowanie użytkowników usługi Azure AD w jednej aplikacji & Account firmy Microsoft
W przeszłości Deweloper aplikacji, którzy chcieli obsługuje oba osobistego konta Microsoft i działać kont z usługi Azure Active Directory było wymagane do integracji z dwóch oddzielnych systemach.  **Punktu końcowego v2.0 usługi Azure AD** wprowadziła nową wersję interfejsu API uwierzytelniania, umożliwiający Zaloguj się w obu typów kont przy użyciu jednego prostą integrację.  Aplikacje korzystające z punktem końcowym v2.0 można również korzystać z interfejsów API REST z [Microsoft Graph](https://graph.microsoft.io) za pomocą dowolnego typu konta.

## <a name="getting-started"></a>Wprowadzenie
Wybierz preferowaną platformę z poniższej listy kompilowania aplikacji za pomocą naszych typu open source bibliotek i platform.  Alternatywnie można użyć naszej dokumentacji protokołu OAuth 2.0 & OpenID Connect do wysyłania i odbierania wiadomości protokołu bezpośrednio, bez przy użyciu biblioteki uwierzytelniania.

<br />

[!INCLUDE [active-directory-v2-quickstart-table](../../../includes/active-directory-v2-quickstart-table.md)]

## <a name="whats-new"></a>Co nowego
Informacje w tym miejscu będą przydatne zrozumieć, co to jest & co nie jest możliwe z punktem końcowym v2.0.

* Dowiedz się więcej o [typy aplikacji, można tworzyć z punktem końcowym v2.0](active-directory-v2-flows.md).
* Zrozumienie [ograniczenia, ograniczenia i ograniczenia](active-directory-v2-limitations.md) z punktem końcowym v2.0.
* Wyewidencjonuj ten przegląd wideo dla punktu końcowego v2.0:

>[!VIDEO https://channel9.msdn.com/Events/Build/2017/P4031/player]

## <a name="reference"></a>Dokumentacja
Te linki przydadzą się do eksplorowania platform szczegółowo:

* [Dokumentacja protokołu v2.0](active-directory-v2-protocols.md)
* [Odwołania do tokenu w wersji 2.0](active-directory-v2-tokens.md)
* [Odwołanie do biblioteki w wersji 2.0](active-directory-v2-libraries.md)
* [Zakresy i zgody w punkcie końcowym v2.0](active-directory-v2-scopes.md)
* [Program Microsoft Graph](https://graph.microsoft.io)

> [!NOTE]
> Jeśli wymagane jest tylko do logowania do kont służbowych z usługi Azure Active Directory, należy zacząć z naszych [przewodnik dewelopera usługi Azure AD](active-directory-developers-guide.md).  Punktem końcowym v2.0 jest przeznaczony do użycia przez deweloperów, którzy jawnie konieczne logowanie w osobistego konta Microsoft.


[!INCLUDE  [Help and Support Options](../../../includes/active-directory-develop-help-support-include.md)]
