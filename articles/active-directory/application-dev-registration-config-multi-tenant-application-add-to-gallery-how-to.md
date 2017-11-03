---
title: "Jak dodać aplikację wielodostępne do galerii aplikacji Azure AD | Dokumentacja firmy Microsoft"
description: "Wyjaśniono, jak można wyświetlić listę własnej rozwinięte wielodostępnych aplikacji w galerii aplikacji usługi Azure AD"
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
ms.openlocfilehash: 208f0d40bd7a8e8f35f16e1fcb09c305d833dbb2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-add-a-multi-tenant-application-to-the-azure-ad-application-gallery"></a>Jak dodać aplikację wielodostępne do galerii aplikacji Azure AD

## <a name="what-is-the-azure-ad-application-gallery"></a>Co to jest galerii aplikacji usługi Azure AD?

Galerii aplikacji usługi Azure AD jest doskonałym sposobem na aplikacji przed miliony klientów usługi Azure Active Directory rozszerzenie wpływ i osiągnąć aplikacji w witrynie marketplace. Kroki wyjaśniają, jak można wyświetlić listę aplikacji w galerii aplikacji usługi Azure AD.

## <a name="if-your-application-supports-saml-or-openidconnect"></a>Jeśli aplikacja obsługuje SAML lub OpenIDConnect
Jeśli masz aplikację wielodostępne, który chcesz wyświetlić w galerii aplikacji usługi Azure AD, najpierw należy się upewnić, czy aplikacja obsługuje jeden z następujących pojedynczego logowania jednokrotnego technologii:

1. **OpenID Connect** -bezpośrednia Integracja z usługą Azure AD przy użyciu protokołu OpenID Connect do uwierzytelniania i Azure AD zgody interfejsu API dla konfiguracji. Jeśli aplikacja nie obsługuje SAML integracji jest uruchamiana, to tryb zaleca się.
2. **SAML** — aplikacja już ma możliwość konfigurowania przy użyciu protokołu SAML dostawcy tożsamości innych firm.

Jeżeli aplikacja obsługuje jeden z tych pojedynczego trybów logowania jednokrotnego i chcesz wyświetlanie wielodostępnych aplikacji w galerii aplikacji usługi Azure AD, należy wykonać kroki opisane w dokumencie poniżej. Aby szybko rozpocząć Wyślij wiadomość e-mail do  **waadpartners@microsoft.com** .

## <a name="if-your-application-does-not-support-saml-or-openidconnect"></a>Jeśli aplikacja nie obsługuje SAML lub OpenIDConnect
Nawet wtedy, gdy aplikacja nie obsługuje jeden z trybów, firma Microsoft może nadal zintegrować ją z galerii przy użyciu technologii nasze hasło logowania jednokrotnego. Jeśli chcesz zapoznać się z tej opcji, możesz wysłać wiadomość e-mail na adres  **waadpartners@microsoft.com** .

## <a name="next-steps"></a>Następne kroki
[Jak wyświetlić listę aplikacji w galerii aplikacji usługi Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing)
