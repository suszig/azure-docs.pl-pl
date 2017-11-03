---
title: "Przypisanie użytkownika lub grupę do aplikacji przedsiębiorstwa w usłudze Azure Active Directory | Dokumentacja firmy Microsoft"
description: "Jak wybrać aplikację przedsiębiorstwa przypisać użytkownika lub grupę do niego w usłudze Azure Active Directory"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 5817ad48-d916-492b-a8d0-2ade8c50a224
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/28/2017
ms.author: curtand
ms.reviewer: asteen
ms.openlocfilehash: 8e61044f261033a473241e2de152026bf49c4c70
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="assign-a-user-or-group-to-an-enterprise-app-in-azure-active-directory"></a>Przypisanie użytkownika lub grupę do aplikacji przedsiębiorstwa w usłudze Azure Active Directory
To proste przypisać użytkownika lub grupę do aplikacji w sieci przedsiębiorstwa w usłudze Azure Active Directory (Azure AD). Musi mieć odpowiednie uprawnienia do zarządzania aplikacjami przedsiębiorstwa, a musi być administratorem globalnym katalogu.

## <a name="how-do-i-assign-user-access-to-an-enterprise-app"></a>Jak przypisać dostępu użytkownika do aplikacji przedsiębiorstwa?
1. Zaloguj się do [portalu Azure](https://portal.azure.com) przy użyciu konta, które jest administratorem globalnym katalogu.
2. Wybierz **więcej usług**wprowadź Azure Active Directory w polu tekstowym, a następnie wybierz **Enter**.
3. Na **usługi Azure Active Directory - *directoryname***  bloku (to znaczy usługi Azure AD bloku katalogu zarządzasz), wybierz **aplikacje dla przedsiębiorstw**.

    ![Otwieranie aplikacji przedsiębiorstwa](./media/active-directory-coreapps-assign-user-azure-portal/open-enterprise-apps.png)
4. Na **aplikacje dla przedsiębiorstw** bloku, wybierz opcję **wszystkie aplikacje**. Zobaczysz listę aplikacji, którymi można zarządzać.
5. Na **aplikacje przedsiębiorstwa — wszystkie aplikacje** bloku, wybierz aplikację.
6. Na ***appname*** bloku (to znaczy bloku o nazwie wybranej aplikacji w tytule), wybierz **użytkownicy i grupy**.

    ![Polecenie wszystkie aplikacje](./media/active-directory-coreapps-assign-user-azure-portal/select-app-users.png)
7. Na ***appname*** **— przypisanie do grupy & użytkownika** bloku, wybierz opcję **Dodaj** polecenia.
8. Na **Dodaj przydziału** bloku, wybierz opcję **użytkowników i grup**.

    ![Przypisanie użytkownika lub grupę do aplikacji](./media/active-directory-coreapps-assign-user-azure-portal/assign-users.png)
9. Na **użytkowników i grup** bloku, wybierz jeden lub więcej użytkowników lub grup z listy, a następnie wybierz **wybierz** przycisk w dolnej części bloku.
10. Na **Dodaj przydziału** bloku, wybierz opcję **roli**. Następnie na **wybierz rolę** bloku, wybierz rolę do zastosowania do wybranych użytkowników lub grup, a następnie wybierz **OK** przycisk w dolnej części bloku.
11. Na **Dodaj przydziału** bloku, wybierz opcję **przypisać** przycisk w dolnej części bloku. Przypisanych użytkowników lub grup, będzie mieć uprawnienia określone przez wybraną rolę dla tej aplikacji przedsiębiorstwa.

## <a name="next-steps"></a>Następne kroki
* [Wyświetl wszystkie moje grupy](active-directory-groups-view-azure-portal.md)
* [Usuń przypisanie użytkownika lub grupy z aplikacjami](active-directory-coreapps-remove-assignment-azure-portal.md)
* [Wyłącz logowania użytkowników dla aplikacji przedsiębiorstwa](active-directory-coreapps-disable-app-azure-portal.md)
* [Zmiana nazwy lub logo aplikacji przedsiębiorstwa](active-directory-coreapps-change-app-logo-user-azure-portal.md)
