---
title: "Uaktualnij do serwera Proxy aplikacji usługi Azure AD | Dokumentacja firmy Microsoft"
description: "Wybierz, jakie rozwiązanie do serwera proxy jest najlepsze w przypadku uaktualniania z programu Microsoft Forefront lub Unified bramy dostępu."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: 6c9f70493155de6989b26fd4e8bcf1dff01c835c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="compare-remote-access-solutions"></a>Porównanie rozwiązań dostępu zdalnego

Azure Active Directory serwera Proxy aplikacji jest jednym z dwóch rozwiązań dostępu zdalnego, które firma Microsoft oferuje. Druga to Proxy aplikacji sieci Web, wersji lokalnej. Te dwa rozwiązania Zastąp starsze produkty, które oferowane przez firmy Microsoft: Microsoft Forefront Threat Management bramy (TMG) i Unified Access Gateway (UAG). Użyj w tym artykule, aby zrozumieć, jak te rozwiązania cztery porównać ze sobą. Dla osób, które nadal przy użyciu przestarzałe rozwiązań TMG lub UAG należy użyć w tym artykule do planowania migracji do jednego serwera proxy aplikacji. 


## <a name="feature-comparison"></a>Porównanie funkcji

Użyj tej tabeli, aby zrozumieć, jak Threat Management Gateway (TMG), Unified Access Gateway (UAG), serwer Proxy aplikacji sieci Web (WAP) i serwera Proxy aplikacji usługi Azure AD (AP) porównać ze sobą.

| Funkcja | TMG | UAG | WAP | Interfejs API |
| ------- | --- | --- | --- | --- |
| Uwierzytelnianie certyfikatu | Tak | Tak | - | - |
| Selektywnie publikować aplikacje przeglądarki | Tak | Tak | Tak | Tak |
| Wstępnego uwierzytelniania i pojedynczego logowania jednokrotnego | Tak | Tak | Tak | Tak | 
| Warstwy 2/3 zapory | Tak | Tak | - | - |
| Funkcje serwera proxy do przodu | Tak | - | - | - |
| Funkcje sieci VPN | Tak | Tak | - | - |
| Obsługa protokołu sformatowanego | - | Tak | Tak, jeśli uruchomiony za pośrednictwem protokołu HTTP | Tak, jeśli uruchomiony za pośrednictwem protokołu HTTP lub za pośrednictwem bramy usług pulpitu zdalnego |
| Służy jako serwer proxy usług AD FS | - | Tak | Tak | - |
| Jednego portalu dostępu do aplikacji | - | Tak | - | Tak |
| Tłumaczenie łącze treści odpowiedzi | Tak | Tak | - | Tak | 
| Uwierzytelnianie za pomocą nagłówków | - | Tak | - | Tak, z PingAccess | 
| Zabezpieczenia w skali chmury | - | - | - | Tak | 
| Dostęp warunkowy | - | Tak | - | Tak |
| Brak składników w strefą zdemilitaryzowaną (DMZ) | - | - | - | Tak |
| Nie połączenia przychodzące | - | - | - | Tak |

W przypadku większości scenariuszy zalecamy aplikacji usługi Azure AD jako nowoczesnego rozwiązania. Serwer Proxy aplikacji sieci Web jest tylko preferowane w scenariuszach wymagających serwera proxy usług AD FS i nie można używać niestandardowych domen w usłudze Azure Active Directory. 

Serwer Proxy aplikacji usługi Azure AD oferują unikatowych zalet w porównaniu do podobnych produktów, w tym:

- Rozszerzenie usługi Azure AD z lokalnymi zasobami
   - Skali chmury zabezpieczenia i ochrona
   - Funkcje, takie jak dostęp warunkowy i uwierzytelniania wieloskładnikowego można łatwo włączyć
- Nie componenet w strefą zdemilitaryzowaną
- Nie połączenia przychodzące wymagane
- Panel dostępu jednego użytkownika można przejść do dla wszystkich aplikacji, włącznie z usługi Office 365, Azure AD zintegrowanych aplikacji SaaS i lokalnej sieci web aplikacji. 


## <a name="next-steps"></a>Następne kroki

- [Użyj aplikacji usługi Azure AD, aby zapewnić bezpieczny zdalny dostęp do aplikacji lokalnych](active-directory-application-proxy-get-started.md)
- [Przejście z programu Forefront TMG i UAG do serwera Proxy aplikacji](https://blogs.technet.microsoft.com/isablog/2015/06/30/modernizing-microsoft-application-access-with-web-application-proxy-and-azure-active-directory-application-proxy/).
