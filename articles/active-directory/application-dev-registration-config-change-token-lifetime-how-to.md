---
title: "Jak zmienić okres istnienia tokenu domyślnie rozwinięte niestandardowych aplikacji | Dokumentacja firmy Microsoft"
description: "Jak aktualizować zasady okres istnienia tokenu aplikacji, które tworzysz usługi Azure AD"
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
ms.openlocfilehash: a28eacd820ed28a6470992ce86b060e886c00bcb
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-change-the-token-lifetime-defaults-for-a-custom-developed-application"></a>Jak zmienić domyślne okres istnienia tokenu dla aplikacji utworzonych niestandardowych

Azure AD Premium umożliwia deweloperom aplikacji i administratorów dzierżawy, aby skonfigurować okres istnienia tokenów wystawionych dla klientów z systemem innym niż poufne. Okres istnienia tokenu zasady są skonfigurowane na poziomie dzierżawy lub zasobów, do której uzyskuje dostęp.

 * Aby skonfigurować zasady okres istnienia tokenu, należy pobrać [modułu Azure AD PowerShell](https://www.powershellgallery.com/packages/AzureADPreview).

 * Uruchom **Connect AzureAD-Potwierdź** polecenia.

 * Oto przykład zasad, która ustawia token odświeżania pojedynczy czynnik maksymalny wiek. Tworzenie zasad:```New-AzureADPolicy -Definition @('{"TokenLifetimePolicy":{"Version":1, "MaxAgeSingleFactor":"until-revoked"}}') -DisplayName "OrganizationDefaultPolicyScenario" -IsOrganizationDefault $true -Type "TokenLifetimePolicy"```

 * Wyewidencjonowanie [okres istnienia tokenu Konfigurowanie](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-configurable-token-lifetimes) dokumentu, aby dowiedzieć się, jak utworzyć inne niestandardowe.

## <a name="next-steps"></a>Następne kroki
[Konfigurowanie okres istnienia tokenu](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-configurable-token-lifetimes)<br>

[Odwołanie do tokenu usługi Azure AD](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-token-and-claims)

