---
title: "Inicjowanie obsługi użytkowników dla aplikacji w galerii Azure AD jest lub więcej godzin z argumentami | Dokumentacja firmy Microsoft"
description: "Jak dowiedzieć się, dlaczego inicjowania obsługi administracyjnej do aplikacji może trwać dłużej niż oczekiwano"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: 183d60cbbbc8d02f7cd3cacc160453c45717ef0d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="user-provisioning-to-an-azure-ad-gallery-application-is-taking-hours-or-more"></a>Inicjowanie obsługi użytkowników dla aplikacji w galerii Azure AD jest lub więcej godzin z argumentami

Podczas włączania najpierw automatyczne Inicjowanie obsługi administracyjnej dla aplikacji, początkowej synchronizacji może potrwać od 20 minut do kilku godzin, zależnie od rozmiaru katalogu usługi Azure AD i liczbę użytkowników w zakresie obsługi. 

Kolejne synchronizacje po początkowej synchronizacji można szybciej, jak znaki wodne, przedstawiające stan obu systemów po początkowej synchronizacji, poprawa wydajności kolejne synchronizacje magazyny inicjowania obsługi usługi.

## <a name="how-to-improve-provisioning-performance"></a>Jak poprawić wydajność inicjowania obsługi administracyjnej

Jeśli synchronizacji początkowej trwa dłużej niż kilka godzin, jest jedyną operacją, której można wykonać, aby poprawić wydajność:

-   **Filtry zakresu użytkownika.** Filtry zakresu umożliwiają dostosowywanie dane inicjowania obsługi usługi wyodrębnia z usługi Azure AD przez filtrowanie na podstawie wartości atrybutu określonych użytkowników. Aby uzyskać więcej informacji na filtrami zakresów, zobacz [udostępniania aplikacji na podstawie atrybutów z filtrami zakresów](https://docs.microsoft.com/azure/active-directory/active-directory-saas-scoping-filters).

## <a name="next-steps"></a>Następne kroki
[Automatyzowanie użytkownika alokowania i anulowania alokowania do aplikacji SaaS w usłudze Azure Active Directory](active-directory-saas-app-provisioning.md)

