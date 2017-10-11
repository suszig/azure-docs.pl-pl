---
title: "Rejestrowanie aplikacji z punktem końcowym v2.0 usługi Azure AD przy użyciu portalu | Dokumentacja firmy Microsoft"
description: "Jak zarejestrować aplikację w firmie Microsoft za włączanie logowania i uzyskiwanie dostępu do usług firmy Microsoft przy użyciu punktu końcowego v2.0"
services: active-directory
documentationcenter: 
author: lnalepa
manager: mbaldwin
editor: 
ms.assetid: bb2f701f-3bc3-4759-94a5-8b9d53a8a0b6
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/01/2017
ms.author: lenalepa
ms.custom: aaddev
ms.openlocfilehash: e6202aa8665c906382666fe08a561421e50e0a8d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-register-an-app-with-the-v20-endpoint"></a>Jak zarejestrować aplikację z punktem końcowym v2.0
Do tworzenia aplikacji, która akceptuje zarówno zarządzanych kont usług, jak i usługi Azure AD logowanie, musisz najpierw zarejestrować aplikację w firmie Microsoft.  W tej chwili nie można używać żadnych istniejących aplikacji, które mogą wiązać Ciebie z usługi Azure AD lub MSA — należy utworzyć nowy.

> [!NOTE]
> Nie wszystkie usługi Azure Active Directory scenariuszy i funkcji obsługiwanych przez punktu końcowego v2.0.  Aby ustalić, czy należy używać punktu końcowego v2.0, przeczytaj o [ograniczenia v2.0](active-directory-v2-limitations.md).
> 
> 

## <a name="visit-the-microsoft-app-registration-portal"></a>Odwiedź portal rejestracji aplikacji firmy Microsoft
Przede wszystkim najpierw - przejdź do [https://apps.dev.microsoft.com/?deeplink=/appList](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList).  Jest to nowy portal rejestracji aplikacji, gdzie zarządzalnych aplikacji firmy Microsoft.

Zaloguj się przy użyciu albo osobistego lub służbowe konto Microsoft.  Jeśli nie masz albo utworzyć nowe konto osobiste. Teraz, nie potrwa długo — poczekamy tutaj.

Zrobić? Powinny teraz można patrzy listę aplikacji firmy Microsoft, który jest prawdopodobnie puste.  Ta funkcja pozwala zmienić.

Kliknij przycisk **Dodaj aplikację**i nadaj mu nazwę.  Portalu przypisze globalnie unikatowy identyfikator aplikacji, który będzie potrzebny później w kodzie aplikacji.  Jeśli aplikacja zawiera składnik po stronie serwera wymaga tokenów dostępu dla wywołania interfejsów API (wziąć pod uwagę: Office, Azure lub własnego interfejsu API sieci web), należy utworzyć **klucz tajny aplikacji** również w tym miejscu.

Następnie dodaj platformy, które będą korzystać z aplikacji.

* W przypadku aplikacji sieci web, podaj **identyfikator URI przekierowania** wysyłania komunikatów do logowania.
* Dla aplikacji mobilnych skopiuj domyślny przekierowania uri utworzony automatycznie.

Opcjonalnie można dostosować wygląd i działanie strony logowania w sekcji profilu.  Upewnij się, że **zapisać** przed kontynuowaniem.

> [!NOTE]
> Po utworzeniu aplikacji przy użyciu [https://apps.dev.microsoft.com/?deeplink=/appList](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), aplikacja zostanie zarejestrowana w domu dzierżawy konta, którego używasz do logowania się do portalu.  Oznacza to, że nie można zarejestrować aplikację w dzierżawie usługi Azure AD za pomocą osobistego konta Microsoft.  Jeśli chcesz jawnie zarejestrować aplikację w określonym dzierżawcy, zaloguj się przy użyciu konta pierwotnie utworzone w tej dzierżawie.
> 
> 

## <a name="build-a-quick-start-app"></a>Tworzenie aplikacji szybki start
Teraz, gdy masz aplikacji firmy Microsoft, można wykonać jedną z naszych samouczków szybkiego startu v2.0.  Poniżej przedstawiono kilka zaleceń:

[!INCLUDE [active-directory-v2-quickstart-table](../../../includes/active-directory-v2-quickstart-table.md)]

