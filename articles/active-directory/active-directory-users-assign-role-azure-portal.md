---
title: "Przypisz użytkownika do ról administratora w usłudze Azure Active Directory | Dokumentacja firmy Microsoft"
description: "Wyjaśniono, jak zmienić administratora informacje o użytkowniku w usłudze Azure Active Directory"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: a1ca1a53-50d8-4bf0-ae8f-73fa1253e2d9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: curtand
ms.reviewer: jeffsta
ms.openlocfilehash: bfadf133154488f9827cfbeaa98ddb0eb84b52f6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="assign-a-user-to-administrator-roles-in-azure-active-directory"></a>Przypisz użytkownika do ról administratora w usłudze Azure Active Directory
W tym artykule opisano sposób przypisywania roli administracyjnej dla użytkownika w usłudze Azure Active Directory (Azure AD). Aby uzyskać informacje dotyczące dodawania nowych użytkowników w organizacji, zobacz [Dodawanie nowych użytkowników do usługi Azure Active Directory](active-directory-users-create-azure-portal.md). Dodani użytkownicy domyślnie nie mają uprawnień administratora, ale możesz przypisać im role w dowolnym momencie.

## <a name="assign-a-role-to-a-user"></a>Przypisywanie użytkownikowi roli
1. Zaloguj się do [portalu Azure](https://portal.azure.com) przy użyciu konta, które jest administratorem globalnym katalogu.
2. Wybierz **więcej usług**, wprowadź **użytkowników i grup** w polu tekstowym, a następnie wybierz **Enter**.

   ![Otwieranie Zarządzanie użytkownikami](./media/active-directory-users-assign-role-azure-portal/create-users-user-management.png)
3. Na **użytkowników i grup** bloku, wybierz opcję **wszyscy użytkownicy**.

   ![Otwieranie bloku wszystkich użytkowników](./media/active-directory-users-assign-role-azure-portal/create-users-open-users-blade.png)
4. Na **użytkowników i grup — wszyscy użytkownicy** bloku, wybierz użytkownika z listy.
5. W bloku dla wybranego użytkownika, wybierz **roli katalogu**, a następnie przypisz użytkownika do roli **roli katalogu** listy. Aby uzyskać więcej informacji dotyczących ról użytkowników i administratorów, zobacz [Przypisywanie ról administratorów w usłudze Azure AD](active-directory-assign-admin-roles.md).

      ![Przypisanie użytkownika do roli](./media/active-directory-users-assign-role-azure-portal/create-users-assign-role.png)
6. Wybierz pozycję **Zapisz**.

## <a name="next-steps"></a>Następne kroki
* [Dodaj użytkownika](active-directory-users-create-azure-portal.md)
* [Resetuj hasło użytkownika w portalu Azure](active-directory-users-reset-password-azure-portal.md)
* [Zmień informacje dotyczące pracy użytkownika](active-directory-users-work-info-azure-portal.md)
* [Zarządzanie profilami użytkowników](active-directory-users-profile-azure-portal.md)
* [Usunięcie użytkownika w usługi Azure AD](active-directory-users-delete-user-azure-portal.md)
