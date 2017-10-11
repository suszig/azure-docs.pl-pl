---
title: "Usługa Azure Active Directory B2C: Zagrożenia zarządzania | Dokumentacja firmy Microsoft"
description: "Więcej informacji na temat techniki wykrywanie i zapobieganie atakom typu \"odmowa usługi\" i prób złamania hasła w usłudze Azure Active Directory B2C."
services: active-directory-b2c
documentationcenter: 
author: vigunase
manager: Ajith Alexander
editor: 
ms.assetid: 6df79878-65cb-4dfc-98bb-2b328055bc2e
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/27/2016
ms.author: 
ms.openlocfilehash: 9472cb01eb713e297053727b1a314293574bb657
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="azure-active-directory-b2c-threat-management"></a>Usługa Azure Active Directory B2C: Zarządzanie zagrożeniami

Zarządzanie zagrożeniami obejmuje planowanie dotyczące ochrony przed atakami systemu i sieci. Ataki typu "odmowa usługi" może spowodować zasobów niedostępny uprawnionych użytkowników. Złamania hasła prowadzić do nieautoryzowanego dostępu do zasobów. Usługa Azure Active Directory B2C (Azure AD B2C) ma wbudowane funkcje, które mogą pomóc chronić dane przed zagrożeniami tych na wiele sposobów.

## <a name="denial-of-service-attacks"></a>Ataki typu "odmowa usługi"

Azure AD B2C używa wykrywanie i zapobieganie technik, takich jak pliki cookie SYN i limitów szybkości i połączenia w celu ochrony zasobów przed atakami typu "odmowa usługi".

## <a name="password-attacks"></a>Złamania hasła

Usługa Azure AD B2C ma również techniki środki zaradcze w przypadku złamania hasła. Środki zaradcze obejmuje ataków siłowych hasła i słownikowymi hasła. Hasła, które są ustawiane przez użytkowników muszą być rozsądnych złożonych. Za pomocą różnych sygnałów, usługi Azure AD B2C analizuje integralności żądań. Usługa Azure AD B2C jest przeznaczona do inteligentnie rozróżnianie uprawnionych użytkowników przed hakerami i zakłócanych. Usługi Azure AD B2C udostępnia zaawansowane strategii blokady konta oparte na wprowadzone w prawdopodobieństwo ataku hasła.

Aby uzyskać więcej informacji, odwiedź stronę [Microsoft Trust Center](https://www.microsoft.com/trustcenter/security/threatmanagement).
