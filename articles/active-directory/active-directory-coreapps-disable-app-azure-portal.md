---
title: "Wyłącz logowania użytkowników dla aplikacji przedsiębiorstwa w usłudze Azure Active Directory | Dokumentacja firmy Microsoft"
description: "Jak wyłączyć aplikację przedsiębiorstwa, dzięki czemu użytkownicy nie mogą Zaloguj się do niego w usłudze Azure Active Directory"
services: active-directory
documentationcenter: 
author: curtand
manager: mtillman
editor: 
ms.assetid: a27562f9-18dc-42e8-9fee-5419566f8fd7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/28/2017
ms.author: curtand
ms.reviewer: asteen
ms.custom: it-pro
ms.openlocfilehash: d31aa3b1399bab2b1dcfdb2eb6ece393610c769f
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/11/2017
---
# <a name="disable-user-sign-ins-for-an-enterprise-app-in-azure-active-directory"></a>Wyłącz logowania użytkowników dla aplikacji przedsiębiorstwa w usłudze Azure Active Directory
To proste wyłączyć aplikację przedsiębiorstwa, dzięki czemu użytkownicy nie mogą Zaloguj się do niego w usłudze Azure Active Directory (Azure AD). Musi mieć odpowiednie uprawnienia do zarządzania aplikacjami przedsiębiorstwa, a musi być administratorem globalnym katalogu.

## <a name="how-do-i-disable-user-sign-ins"></a>Jak wyłączyć logowania użytkowników?
1. Zaloguj się do [portalu Azure](https://portal.azure.com) przy użyciu konta, które jest administratorem globalnym katalogu.
2. Wybierz **więcej usług**, wprowadź **usługi Azure Active Directory** w polu tekstowym, a następnie wybierz **Enter**.
3. Na **usługi Azure Active Directory** -  ***directoryname*** bloku (to znaczy usługi Azure AD bloku katalogu zarządzasz), wybierz **aplikacje dla przedsiębiorstw**.

    ![Otwieranie aplikacji przedsiębiorstwa](./media/active-directory-coreapps-disable-app-azure-portal/open-enterprise-apps.png)
4. Na **aplikacje dla przedsiębiorstw** bloku, wybierz opcję **wszystkie aplikacje**. Możesz wyświetlić listę aplikacji, którymi można zarządzać.
5. Na **aplikacje przedsiębiorstwa — wszystkie aplikacje** bloku, wybierz aplikację.
6. Na ***appname*** bloku (to znaczy bloku o nazwie wybranej aplikacji w tytule), wybierz **właściwości**.

    ![Polecenie wszystkie aplikacje](./media/active-directory-coreapps-disable-app-azure-portal/select-app.png)
7. Na ***appname*** - **właściwości** bloku, wybierz opcję **nr** dla **dostępny dla użytkowników do logowania?**.
8. Wybierz **zapisać** polecenia.

## <a name="next-steps"></a>Następne kroki
* [Zobacz wszystkie moje grupy](active-directory-groups-view-azure-portal.md)
* [Przypisanie użytkownika lub grupę do aplikacji w przedsiębiorstwie](active-directory-coreapps-assign-user-azure-portal.md)
* [Usuń przypisanie użytkownika lub grupy z aplikacjami](active-directory-coreapps-remove-assignment-azure-portal.md)
* [Zmiana nazwy lub logo aplikacji przedsiębiorstwa](active-directory-coreapps-change-app-logo-user-azure-portal.md)
