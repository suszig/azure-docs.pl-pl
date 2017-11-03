---
title: "Administrator dzierżawy podniesienie uprawnień dostępu — usługi Azure AD | Dokumentacja firmy Microsoft"
description: "W tym temacie opisano wbudowanych ról dla kontroli dostępu opartej na rolach (RBAC)."
services: active-directory
documentationcenter: 
author: andredm7
manager: femila
editor: rqureshi
ms.assetid: b547c5a5-2da2-4372-9938-481cb962d2d6
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/09/2017
ms.author: andredm
ms.openlocfilehash: 22b62be1773c5042ecf6ee078e68a4ffdf791d53
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="elevate-access-as-a-tenant-admin-with-role-based-access-control"></a>Podniesienie uprawnień dostępu jako Administrator dzierżawy przy użyciu kontroli dostępu opartej na rolach

Oparta na rolach kontrola dostępu ułatwia administratorom dzierżawy uzyskiwania wybierającym tymczasowe w programie access, przyznają uprawnienia wyższe niż zwykle. Administrator dzierżawy może podnieść poziom siebie do roli Administrator dostępu użytkowników w razie potrzeby. Ta rola zapewnia dzierżawcy uprawnienia administratora, aby przydzielić sobie lub innych ról w zakresie "/".

Ta funkcja jest ważna, ponieważ zezwala ona na administratora dzierżawy wyświetlić wszystkie subskrypcje, które istnieją w organizacji. Umożliwia on również automatyzacji aplikacji (takich jak fakturowania i inspekcji) dostęp do wszystkich subskrypcji i podaj dokładnego widoku stanu zarządzania rozliczeń lub zasobów organizacji.  

## <a name="how-to-use-elevateaccess-for-tenant-access-with-azure-ad-admin-center"></a>Jak używać elevateAccess dostęp do dzierżawy przy użyciu Centrum administracyjnego usługi Azure AD

W [Centrum administracyjnego usługi Azure Active Directory](https://aad.portal.azure.com) można wywołać tej funkcji w **właściwości**.
Funkcja jest nazywana **Administrator globalny może zarządzać subskrypcjami Azure**. Wrażenie to, że jest to właściwość globalny usługi Azure Active Directory, jednak działa na podstawie użytkownika dla obecnie zalogowanego użytkownika. Jeśli użytkownik ma uprawnienia administratora globalnego w usłudze Azure Active Directory, można wywołać funkcji elevateAccess dla użytkownika, który jest aktualnie zalogowany do Centrum administracyjnego usługi Azure Active Directory.

Wybieranie **tak** , a następnie **zapisać**: to **przypisuje** **Administrator dostępu użytkowników** roli w katalogu głównym "/" (główny zakres) dla użytkownika z której użytkownik jest aktualnie zalogowany do portalu.

Wybieranie **nr** , a następnie **zapisać**: to **usuwa** **Administrator dostępu użytkowników** roli w katalogu głównym "/" (główny zakres) dla użytkownika z której użytkownik jest aktualnie zalogowany do portalu.

![Globaladmin Centrum administracyjne — właściwości — w usłudze Azure AD można zarządzać subskrypcją Azure — zrzut ekranu](./media/role-based-access-control-tenant-admin-access/aad-azure-portal-global-admin-can-manage-azure-subscriptions.png)

## <a name="how-to-use-elevateaccess-to-give-tenant-access-with-the-rest-api"></a>Jak używać elevateAccess umożliwiają dostęp do dzierżawy przy użyciu interfejsu API REST

Podstawowy proces działa z następujących kroków:

1. Za pomocą usługi REST, wywołaj *elevateAccess*, która udziela roli Administrator dostępu użytkowników na "/" zakres.

    ```
    POST https://management.azure.com/providers/Microsoft.Authorization/elevateAccess?api-version=2016-07-01
    ```

2. Utwórz [przypisania roli](/rest/api/authorization/roleassignments) można przypisać żadnej roli w dowolnym zakresie. Poniższy przykład przedstawia właściwości przypisywanie roli czytnika w "/" zakres:

    ```
    { "properties":{
    "roleDefinitionId": "providers/Microsoft.Authorization/roleDefinitions/acdd72a7338548efbd42f606fba81ae7",
    "principalId": "cbc5e050-d7cd-4310-813b-4870be8ef5bb",
    "scope": "/"
    },
    "id": "providers/Microsoft.Authorization/roleAssignments/64736CA0-56D7-4A94-A551-973C2FE7888B",
    "type": "Microsoft.Authorization/roleAssignments",
    "name": "64736CA0-56D7-4A94-A551-973C2FE7888B"
    }
    ```

3. Podczas administratorem dostępu użytkownika użytkownik może również usunąć przypisania roli w "/" zakres.

4. Odwołaj Twoje uprawnienia administratora dostępu użytkownika, dopóki nie są potrzebne, ponownie.


## <a name="how-to-undo-the-elevateaccess-action-with-the-rest-api"></a>Jak cofnąć elevateAccess przy użyciu interfejsu API REST

Podczas wywoływania *elevateAccess* Tworzenie przypisania roli dla siebie, tak aby można było odwołać uprawnienia te należy usunąć przypisanie.

1.  Wywołanie [GET roleDefinitions](/rest/api/authorization/roledefinitions#RoleDefinitions_Get) gdzie roleName = Administrator dostępu użytkowników można określić nazwy roli Administrator dostępu użytkowników identyfikator GUID. Odpowiedź powinna wyglądać następująco:

    ```
    {"value":[{"properties":{
    "roleName":"User Access Administrator",
    "type":"BuiltInRole",
    "description":"Lets you manage user access to Azure resources.",
    "assignableScopes":["/"],
    "permissions":[{"actions":["*/read","Microsoft.Authorization/*","Microsoft.Support/*"],"notActions":[]}],
    "createdOn":"0001-01-01T08:00:00.0000000Z",
    "updatedOn":"2016-05-31T23:14:04.6964687Z",
    "createdBy":null,
    "updatedBy":null},
    "id":"/providers/Microsoft.Authorization/roleDefinitions/18d7d88d-d35e-4fb5-a5c3-7773c20a72d9",
    "type":"Microsoft.Authorization/roleDefinitions",
    "name":"18d7d88d-d35e-4fb5-a5c3-7773c20a72d9"}],
    "nextLink":null}
    ```

    Zapisz identyfikator GUID z *nazwa* parametru, w tym przypadku **18d7d88d-d35e-4fb5-a5c3-7773c20a72d9**.

2. Wywołanie [GET roleAssignments](/rest/api/authorization/roleassignments#RoleAssignments_Get) gdzie principalId = własne ObjectId. Ta lista zawiera wszystkie przypisaniami w dzierżawie. Wyszukaj co gdzie jest zakresem "/" i RoleDefinitionId kończy nazwę roli GUID można znaleźć w kroku 1. Przypisanie roli powinien wyglądać następująco:

    ```
    {"value":[{"properties":{
    "roleDefinitionId":"/providers/Microsoft.Authorization/roleDefinitions/18d7d88d-d35e-4fb5-a5c3-7773c20a72d9",
    "principalId":"{objectID}",
    "scope":"/",
    "createdOn":"2016-08-17T19:21:16.3422480Z",
    "updatedOn":"2016-08-17T19:21:16.3422480Z",
    "createdBy":"93ce6722-3638-4222-b582-78b75c5c6d65",
    "updatedBy":"93ce6722-3638-4222-b582-78b75c5c6d65"},
    "id":"/providers/Microsoft.Authorization/roleAssignments/e7dd75bc-06f6-4e71-9014-ee96a929d099",
    "type":"Microsoft.Authorization/roleAssignments",
    "name":"e7dd75bc-06f6-4e71-9014-ee96a929d099"}],
    "nextLink":null}
    ```

    Ponownie, Zapisz identyfikator GUID z *nazwa* parametru, w tym przypadku **e7dd75bc-06f6-4e71-9014-ee96a929d099**.

3. Na koniec wywołania [DELETE roleAssignments](/rest/api/authorization/roleassignments#RoleAssignments_DeleteById) gdzie roleAssignmentId = nazwa identyfikatora GUID można znaleźć w kroku 2.

## <a name="next-steps"></a>Następne kroki

- Dowiedz się więcej o [zarządzania opartego na rolach kontrola dostępu przy użyciu REST](role-based-access-control-manage-access-rest.md)

- [Zarządzanie przypisaniami dostępu](role-based-access-control-manage-assignments.md) w portalu Azure
