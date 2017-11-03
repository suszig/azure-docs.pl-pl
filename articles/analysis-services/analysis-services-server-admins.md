---
title: "Zarządzanie Administratorzy serwera w usłudze Azure Analysis Services | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak zarządzać Administratorzy serwera dla serwera usług Analysis Services na platformie Azure."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: kfile
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 11/01/2017
ms.author: owend
ms.openlocfilehash: 6a42baf9fd880264e4130c009f27935a4743de21
ms.sourcegitcommit: d41d9049625a7c9fc186ef721b8df4feeb28215f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/02/2017
---
# <a name="manage-server-administrators"></a>Administratorzy serwerów zarządzania
Administratorzy serwera musi być prawidłowa nazwa użytkownika lub grupy w usłudze Azure Active Directory (Azure AD) dla dzierżawcy, w której znajduje się serwer. Można użyć **Administratorzy usług analizy** w bloku kontroli serwera w portalu Azure lub właściwości serwera w programie SSMS administratorom serwerów zarządzania. 

## <a name="to-add-server-administrators-by-using-azure-portal"></a>Aby dodać administratorom serwerów przy użyciu portalu Azure
1. W bloku formantu serwera, kliknij przycisk **Administratorzy usług analizy**.
2. W  **\<nazwa_serwera > — Administratorzy usług analizy** bloku, kliknij przycisk **Dodaj**.
3. W **dodać administratorom serwerów** bloku, wybierz kont użytkowników z usługi Azure AD lub zaprosić użytkowników zewnętrznych za pomocą adresu e-mail.

    ![Administratorzy serwera w portalu Azure](./media/analysis-services-server-admins/aas-manage-users-admins.png)

## <a name="to-add-server-administrators-by-using-ssms"></a>Aby dodać administratorom serwerów przy użyciu narzędzia SSMS
1. Kliknij prawym przyciskiem myszy serwer > **właściwości**.
2. W **właściwości serwera analiz**, kliknij przycisk **zabezpieczeń**.
3. Kliknij przycisk **Dodaj**, a następnie wprowadź adres e-mail dla użytkownika lub grupy w usługi Azure AD.
   
    ![Dodawanie administratorów serwera w programie SSMS](./media/analysis-services-server-admins/aas-manage-users-ssms.png)

## <a name="next-steps"></a>Następne kroki 
[Uwierzytelnianie i uprawnienia użytkownika](analysis-services-manage-users.md)  
[Zarządzanie ról bazy danych i użytkowników](analysis-services-database-users.md)  
[Kontrola dostępu oparta na rolach](../active-directory/role-based-access-control-what-is.md)  

