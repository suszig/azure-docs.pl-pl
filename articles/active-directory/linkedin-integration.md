---
title: "Konfigurowanie integracji LinkedIn w usłudze Azure AD | Dokumentacja firmy Microsoft"
description: "Wyjaśniono, jak włączyć lub wyłączyć integrację LinkedIn dla aplikacji firmy Microsoft w usłudze Azure Active Directory."
services: active-directory
author: jeffgilb
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: jeffgilb
ms.reviewer: jsnow
ms.custom: it-pro
ms.openlocfilehash: 48b26bfcce67ce915c404a0ab2ac0f399c3b821d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="enabling-linkedin-integration-in-azure-active-directory"></a>Włączanie integracji LinkedIn w usłudze Azure Active Directory
Włączanie integracji LinkedIn umożliwia użytkownikom dostęp do danych zarówno publicznych LinkedIn i, jeśli się na ich osobistych sieci LinkedIn z aplikacji firmy Microsoft. Każdy użytkownik może wybrać niezależnie nawiązać konta służbowego do swojego konta LinkedIn.

### <a name="linkedin-integration-from-your-end-users-perspective"></a>Integracja LinkedIn z perspektywy użytkowników końcowych
Gdy użytkownicy w organizacji połączenie ich kont LinkedIn konta służbowego, są w stanie identyfikację osoby, z których korzystają użytkownicy i spoza organizacji. Połączenie dwóch kont umożliwia połączenia LinkedIn i dane profilu do użycia w aplikacjach firmy Microsoft w organizacji użytkownika, ale one zawsze można zrezygnować usuwając uprawnienie LinkedIn udostępnianie tych danych. Integracja używa również informacje o profilu publicznie dostępne, a użytkownicy mogą zdecydować się na współużytkowanie ich publiczne informacje o profilu i sieci z aplikacji firmy Microsoft za pośrednictwem ustawień prywatności LinkedIn.

>[!NOTE]
> Włączanie integracji LinkedIn w usłudze Azure AD umożliwia aplikacji i usług, aby uzyskać dostęp do pewnych informacji użytkownikom końcowym. Odczyt [zasady zachowania poufności informacji firmy Microsoft](https://privacy.microsoft.com/privacystatement/) dowiedzieć się więcej o zagadnienia dotyczące ochrony prywatności podczas włączania integracji LinkedIn w usłudze Azure AD. 

## <a name="enable-linkedin-integration"></a>Włącz integrację LinkedIn
LinkedIn integracji dla przedsiębiorstw, które jest domyślnie włączona w usłudze Azure AD. Tak gdy ta funkcja jest udostępniana dla swojej dzierżawy, wszyscy użytkownicy w organizacji widoczne funkcje LinkedIn i profile. Włączanie integracji LinkedIn umożliwia użytkownikom w organizacji do używania funkcji LinkedIn w ramach usług firmy Microsoft, takich jak wyświetlanie profile podczas odbierania wiadomości e-mail w programie Outlook. Wyłączenie tej funkcji uniemożliwia dostęp do funkcji LinkedIn i zatrzymuje połączenia konta użytkownika i udostępnianie między LinkedIn i organizacji za pomocą usługi Microsoft danych.

> [!IMPORTANT]
> Ta funkcja nie jest dostępna dla lokalnych przejdź, suwerenne i dzierżawcy dla instytucji rządowych. Ponadto aktualizacje usługi Azure AD, takich jak LinkedIn funkcje integracji, nie są wdrażane do wszystkich dzierżawców Azure w tym samym czasie. Nie można włączyć LinkedIn integracji z usługą Azure AD, dopóki ta funkcja została przeniesiona z dzierżawą platformy Azure.

1. Zaloguj się do [Centrum administracyjnego usługi Azure Active Directory](https://aad.portal.azure.com/) przy użyciu konta, które jest administratorem globalnym katalogu.
2. Wybierz **użytkowników i grup**.
3. Na **użytkowników i grup** bloku, wybierz opcję **ustawienia użytkownika**.
4. W obszarze **integracji LinkedIn**, wybierz opcję Tak lub nie, aby włączyć lub wyłączyć integrację LinkedIn.
   ![Włączanie integracji LinkedIn](./media/linkedin-integration/LinkedIn-integration.PNG)

### <a name="learn-more"></a>Dowiedz się więcej 
* [LinkedIn informacji i funkcji w aplikacjach firmy Microsoft](https://go.microsoft.com/fwlink/?linkid=850740)

* [Centrum pomocy LinkedIn](https://www.linkedin.com/help/linkedin)

## <a name="next-steps"></a>Następne kroki
Poniższe łącze służy do włączania lub wyłączania LinkedIn integracji z usługą Azure AD z portalu Azure:

[Konfigurowanie integracji LinkedIn](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/UserManagementMenuBlade/UserSettings) 