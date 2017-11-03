---
title: "Dodawanie nowych użytkowników do usługi Azure Active Directory | Microsoft Docs"
description: "Opisano sposób dodawania nowych użytkowników w usłudze Azure Active Directory."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/22/2017
ms.author: curtand
ms.reviewer: jeffsta
ms.custom: it-pro
ms.openlocfilehash: 9b6a48220132bb8ea18ae5efca46ea2faf825806
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="quickstart-add-new-users-to-azure-active-directory"></a>Szybki Start: Dodawanie nowych użytkowników do usługi Azure Active Directory
W tym artykule opisano sposób dodawania nowych użytkowników w organizacji w usłudze Azure Active Directory (Azure AD) za pomocą portalu Azure lub przez synchronizację danych konta użytkownika systemu Windows Server AD lokalnymi. 

## <a name="add-cloud-based-users"></a>Dodaj użytkowników w chmurze
1. Zaloguj się do [Centrum administracyjnego usługi Azure Active Directory](https://aad.portal.azure.com) przy użyciu konta, które jest administratorem globalnym katalogu.
2. Wybierz **usługi Azure Active Directory** , a następnie **użytkowników i grup**.
3. Na **użytkowników i grup**, wybierz pozycję **wszyscy użytkownicy**, a następnie wybierz **nowego użytkownika**.
   ![Dodaj polecenie](./media/add-users-azure-active-directory/add-user.png)
4. Wprowadź szczegóły użytkownika, takich jak **nazwa** i **nazwy użytkownika**. Część nazwy domeny nazwa użytkownika musi być początkowej domyślnej domeny nazwa "[nazwa domeny].onmicrosoft.com" lub zweryfikowane, niefederacyjnych [niestandardowej nazwy domeny](add-custom-domain.md) takich jak "contoso.com".
5. Skopiuj lub w przeciwnym razie należy pamiętać wygenerowane hasło, dzięki czemu można udostępnić go użytkownikowi po zakończeniu tego procesu.
6. Opcjonalnie można otwierać i Wypełnij informacje w **profilu**, **grup**, lub **roli katalogu** dla użytkownika. Aby uzyskać więcej informacji dotyczących ról użytkowników i administratorów, zobacz [Przypisywanie ról administratorów w usłudze Azure AD](active-directory-assign-admin-roles.md).
7. Na **użytkownika**, wybierz pozycję **Utwórz**.
8. Bezpiecznie dystrybucji wygenerowane hasło dla nowego użytkownika, dzięki czemu użytkownicy mogą się logować.

> [!TIP]
> Możesz także zsynchronizować dane konto użytkownika z lokalnego systemu Windows Server AD. Firmy Microsoft tożsamościach span lokalnych i chmurze możliwości tworzenia tożsamością jednego użytkownika do uwierzytelniania i autoryzacji do wszystkich zasobów, niezależnie od lokalizacji. Nazywamy to tożsamość hybrydowa. [Azure AD Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect) może służyć do integracji katalogów lokalnych z usługą Azure Active Directory dla scenariuszy hybrydowych tożsamości. Dzięki temu użytkownicy mogą posługiwać się wspólną tożsamością dla usługi Office 365, platformy Azure i aplikacji SaaS zintegrowanych z usługą Azure AD. 

## <a name="delete-users-from-azure-ad"></a>Usuwanie użytkowników z usługi Azure AD
1. Zaloguj się do [Centrum administracyjnego usługi Azure Active Directory](https://aad.portal.azure.com) przy użyciu konta, które jest administratorem globalnym katalogu.
2. Wybierz **użytkowników i grup**.
3. Na **użytkowników i grup** bloku, wybierz użytkownika, aby usunąć z listy. 
4. W bloku dla wybranego użytkownika, wybierz **omówienie**, a następnie na pasku poleceń Wybierz **usunąć**.
   ![Dodaj polecenie](./media/add-users-azure-active-directory/delete-user.png)


### <a name="learn-more"></a>Dowiedz się więcej 
* [Dodaj użytkownika zewnętrznego](active-directory-users-create-external-azure-portal.md)

* [Przypisanie użytkownika do roli w usługi Azure AD](active-directory-users-assign-role-azure-portal.md)

## <a name="next-steps"></a>Następne kroki
W tym szybkiego startu kiedy znasz już sposób dodawania nowych użytkowników do usługi Azure AD Premium. 

Poniższe łącze służy do tworzenia nowego użytkownika w usłudze Azure AD z portalu Azure.

> [!div class="nextstepaction"]
> [Dodawanie użytkowników do usługi Azure AD](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/UserManagementMenuBlade/All users) 