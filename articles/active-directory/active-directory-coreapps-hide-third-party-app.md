---
title: "Ukryj aplikacji innych producentów, od środowiska użytkownika w usłudze Azure Active Directory | Dokumentacja firmy Microsoft"
description: "Jak ukryć aplikacji innych producentów, od środowiska użytkownika w usłudze Azure Active Directory"
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/11/2017
ms.author: billmath
ms.reviewer: asteen
ms.custom: it-pro
ms.openlocfilehash: 28b6885e7e74a44f4d00f4a804a93e74f6eec685
ms.sourcegitcommit: ccb84f6b1d445d88b9870041c84cebd64fbdbc72
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/14/2017
---
# <a name="hide-a-third-party-application-from-users-experience-in-azure-active-directory"></a>Ukryj aplikacji innych producentów, od środowiska użytkownika w usłudze Azure Active Directory

Jeśli masz aplikacji innych firm (aplikacji opublikowanych przez inne niż firmy Microsoft) nie chcesz widoczne na paneli dostępu użytkowników lub przyciski Uruchom usługi Office 365, ma opcji, aby ukryć ten Kafelek aplikacji. Ukrywanie użytkownicy aplikacji nadal mieć uprawnienia do aplikacji lecz nie będzie mógł przeglądać je na ich przyciski Uruchom aplikację. Musi mieć odpowiednie uprawnienia do zarządzania aplikacjami przedsiębiorstwa, a musi być administratorem globalnym katalogu.

## <a name="hiding-a-users-access-panel"></a>Ukrywanie panelu dostępu użytkownika
Wykonaj następujące kroki, aby ukryć panel dostępu użytkownika i przyciski Uruchom aplikację usługi Office 365

### <a name="how-do-i-hide-an-app-from-users-access-panel-and-o365-app-launchers"></a>Jak ukryć z aplikacji z poziomu panelu dostępu użytkownika i przyciski Uruchom aplikację usługi Office 365?

1.  Zaloguj się do [portalu Azure](https://portal.azure.com) przy użyciu konta, które jest administratorem globalnym katalogu.
2.  Wybierz **więcej usług**, wprowadź **usługi Azure Active Directory** w polu tekstowym, a następnie wybierz **Enter**.
3.  Na **usługi Azure Active Directory - *directoryname***  ekranu (to znaczy usługi Azure AD ekranu dla katalogu są używane do zarządzania), wybierz **aplikacje dla przedsiębiorstw**.
![Aplikacje przedsiębiorstwa](media/active-directory-coreapps-hide-third-party-app/app1.png)
4.  Na **aplikacje dla przedsiębiorstw** ekranu wybierz **wszystkie aplikacje**. Możesz wyświetlić listę aplikacji, którymi można zarządzać.
5.  Na **aplikacje przedsiębiorstwa — wszystkie aplikacje** ekranu, wybierz aplikację.</br>
![Aplikacje przedsiębiorstwa](media/active-directory-coreapps-hide-third-party-app/app2.png)
6.  Na ***appname*** ekranu (czyli ekranu o nazwie wybranej aplikacji w tytule), wybierz polecenie Właściwości.
7.  Na  ***appname* -właściwości** ekranu wybierz **tak** dla **widoczny dla użytkowników?**.
![Aplikacje przedsiębiorstwa](media/active-directory-coreapps-hide-third-party-app/app3.png)
8.  Wybierz **zapisać** polecenia.

## <a name="next-steps"></a>Następne kroki
* [Zobacz wszystkie moje grupy](active-directory-groups-view-azure-portal.md)
* [Przypisanie użytkownika lub grupę do aplikacji w przedsiębiorstwie](active-directory-coreapps-assign-user-azure-portal.md)
* [Usuń przypisanie użytkownika lub grupy z aplikacjami](active-directory-coreapps-remove-assignment-azure-portal.md)
* [Zmiana nazwy lub logo aplikacji przedsiębiorstwa](active-directory-coreapps-change-app-logo-user-azure-portal.md)
