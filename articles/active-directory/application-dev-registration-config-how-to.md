---
title: "Jak wybrać uprawnienia dla danego interfejsu API | Dokumentacja firmy Microsoft"
description: "Jak znaleźć punkty końcowe uwierzytelniania dla aplikacji niestandardowej podczas tworzenia lub rejestrowania za pomocą usługi Azure AD."
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
ms.openlocfilehash: 6966cf145375bf3d830d476564c428502ae40fd4
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-select-permissions-for-a-given-api"></a>Jak wybrać uprawnienia dla danego interfejsu API

Punkty końcowe uwierzytelniania można znaleźć aplikacji w [portalu Azure](https://portal.azure.com).

-   Przejdź do witryny [Azure Portal](https://portal.azure.com).

-   W okienku nawigacji po lewej stronie kliknij **usługi Azure Active Directory**.

-   Kliknij przycisk **rejestracji aplikacji** i wybierz polecenie **punkty końcowe**.

-   To otwarcie **punkty końcowe** strony, tak aby wyświetlić listę wszystkich punktów końcowych uwierzytelniania dla dzierżawy.

-   Użyj punktu końcowego, które są specyficzne dla protokołu uwierzytelniania, którego używasz, w połączeniu z Identyfikatorem aplikacji sformułować uwierzytelnianie żądań specyficzne dla aplikacji.

## <a name="next-steps"></a>Następne kroki
[Przewodnik dewelopera usługi Azure Active Directory](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-developers-guide#authentication-and-authorization-protocols)
