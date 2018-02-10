---
title: "Warunki dostępu warunkowego w usłudze Azure Active Directory | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak przypisania są używane w dostępu warunkowego w usłudze Azure Active Directory do wyzwalania zasad."
services: active-directory
keywords: "dostęp warunkowy do aplikacji, dostęp warunkowy przy użyciu usługi Azure AD, bezpieczny dostęp do zasobów firmy, zasady dostępu warunkowego"
documentationcenter: 
author: MarkusVi
manager: mtillman
editor: 
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/08/2018
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: 22cc43a861e2a9d1d0c508da362a5f0b48d0c268
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/09/2018
---
# <a name="conditions-in-azure-active-directory-conditional-access"></a>Warunki dostępu warunkowego w usłudze Azure Active Directory 

Z [dostępu warunkowego w usłudze Azure Active Directory (Azure AD)](active-directory-conditional-access-azure-portal.md), można kontrolować sposób autoryzowanym użytkownikom dostępu aplikacji w chmurze. W zasadach dostępu warunkowego zdefiniowanego odpowiedzi ("tym") Przyczyna służącą do wyzwalania zasad ("w takim przypadku"). 

![Kontrola](./media/active-directory-conditional-access-conditions/10.png)


W kontekście dostępu warunkowego:

- "**w takim przypadku**" jest wywoływana **warunki**. 
- "**To zrobić**" jest wywoływana **dostęp do formantów**.

Kombinacja warunki z kontroli dostępu w sieci reprezentuje zasady dostępu warunkowego.

![Kontrola](./media/active-directory-conditional-access-conditions/61.png)

W tym artykule przedstawiono omówienie warunki i sposób ich użycia w zasadach dostępu warunkowego. 


## <a name="users-and-groups"></a>Użytkownicy i grupy

Warunek użytkowników i grup jest obowiązkowa w zasadach dostępu warunkowego. W zasadach, można wybrać operator **wszyscy użytkownicy** lub wybierz konkretnych użytkowników i grup.

![Kontrola](./media/active-directory-conditional-access-conditions/02.png)

Po wybraniu:

- **Wszyscy użytkownicy**, zasady są stosowane do wszystkich użytkowników z w katalogu. W tym gości.

- **Wybierz użytkowników i grupy**, można kierować określonych zestawów użytkowników, na przykład, wszyscy członkowie działu HR, podczas logowania się do aplikacji HR. 

- Grupy, może być dowolnego typu grupy w usłudze Azure AD, w tym dynamiczne lub przypisanych grup zabezpieczeń i dystrybucji.

Można również wykluczyć określonych użytkowników lub grup z zasad. Jeden przypadek użycia wspólnych są konta usług, jeśli uwierzytelnianie wieloskładnikowe wymusza zasady. 

Przeznaczonych dla określonych zestawów użytkowników jest przydatne w przypadku wdrażania nowych zasad. W nowych zasad powinien wskazywać tylko początkowego zestawu użytkowników, aby sprawdzić poprawność zasad zachowania. 



## <a name="cloud-apps"></a>Aplikacje w chmurze 

Aplikacji w chmurze jest witryn sieci web lub usługi. W tym witryn sieci web chronionych przez serwer Proxy aplikacji Azure. Aby uzyskać szczegółowy opis aplikacji w chmurze obsługiwane, zobacz [przypisania aplikacji w chmurze](active-directory-conditional-access-technical-reference.md#cloud-apps-assignments).    

Warunek aplikacji w chmurze jest obowiązkowa w zasadach dostępu warunkowego. W zasadach, można wybrać operator **wszystkich aplikacji w chmurze** lub wybranie określonych aplikacji.

![Kontrola](./media/active-directory-conditional-access-conditions/03.png)

Możesz wybrać:

- **Wszystkie aplikacje w chmurze** do linii bazowej zasady do zastosowania w całej organizacji. Przypadek użycia wspólnych dla tego zaznaczenia jest zasadę, która wymaga usługi Multi-Factor authentication po wykryciu zagrożenia logowania dla dowolnej aplikacji w chmurze.

- Aplikacje w chmurze poszczególnych do określonych usług docelowych przez zasady. Na przykład można wymagać od użytkowników [zgodnego urządzenia](active-directory-conditional-access-policy-connected-applications.md) można uzyskać dostępu do usługi SharePoint Online. Ta zasada dotyczy również inne usługi przy uzyskiwaniu dostępu do zawartości programu SharePoint, na przykład Teams firmy Microsoft. 

Można też wykluczyć określone aplikacje z zasad; Jednak te aplikacje będą nadal może ulec zasady stosowane do uzyskiwania dostępu do usługi. 



## <a name="sign-in-risk"></a>Ryzyko związane z logowaniem

Ryzyko logowania jest wskaźnik prawdopodobieństwo (wysoki, średni lub niski) próba logowania nie została wykonana przez wiarygodnego właściciela konta użytkownika. Usługi Azure AD oblicza poziom ryzyka logowania podczas logowania użytkownika. Poziom ryzyka obliczeniowej logowania można użyć jako warunek w zasadach dostępu warunkowego. 

![Warunki](./media/active-directory-conditional-access-conditions/22.png)

Aby użyć tego warunku, musisz mieć [Azure Active Directory Identity Protection](active-directory-identityprotection.md) włączone.
 
Typowe przypadki użycia tego warunku są zasadami, które:

- Blokowanie użytkowników rejestrowania wysokiego ryzyka, aby uniemożliwić użytkownikom potencjalnie prawnie niedozwolonych dostęp do aplikacji w chmurze. 
- Wymagaj uwierzytelniania wieloskładnikowego podczas dla użytkowników z średnie ryzyko logowania. Wymuszając uwierzytelnianie wieloskładnikowe, możesz podać dodatkowe pewność, że logowanie jest wykonywane przez wiarygodnego właściciela konta.

Aby uzyskać więcej informacji, zobacz [Ryzykowne logowania](active-directory-identityprotection.md#risky-sign-ins).  

## <a name="device-platforms"></a>Platformy urządzeń

Platformy urządzeń charakteryzuje się systemu operacyjnego, który działa na urządzeniu. Usługi Azure AD identyfikuje platformy, korzystając z informacji podanych przez urządzenie, takie jak agent użytkownika. Ponieważ te informacje są niezweryfikowane, jest zalecane wszystkich platform zasad zastosowanych do nich, blokuje dostęp, wymagające zgodności z zasadami usługi Intune lub wymagających się, że urządzenie jest przyłączony do domeny. Wartość domyślna to zastosować zasady do wszystkich platform urządzeń. 


![Warunki](./media/active-directory-conditional-access-conditions/02.png)

Aby uzyskać pełną listę obsługiwanych platform urządzeń, zobacz [warunku platformy urządzenia](active-directory-conditional-access-technical-reference.md#device-platform-condition).


Typowy przypadek użycia dla tego warunku jest zasada, która ogranicza dostęp do aplikacji w chmurze do [zaufane urządzenia](active-directory-conditional-access-policy-connected-applications.md#trusted-devices). Aby uzyskać więcej scenariuszy, w tym warunku platformy urządzeń, zobacz [dostępu warunkowego na podstawie aplikacji usługi Azure Active Directory](active-directory-conditional-access-mam.md).


## <a name="locations"></a>Lokalizacje

Lokalizacje istnieje możliwość zdefiniowania warunków, które są oparte na której zainicjowano próby połączenia z. 
     
![Warunki](./media/active-directory-conditional-access-conditions/03.png)

Typowe przypadki użycia tego warunku są zasadami, które:

- Wymusić uwierzytelnianie wieloskładnikowe dla użytkowników uzyskujących dostęp do usługi, gdy znajdują się poza siecią firmową.  

- Zablokuj dostęp użytkowników uzyskujących dostęp do usługi z określonego krajach lub regionach. 

Aby uzyskać więcej informacji, zobacz [lokalizacji warunki dostępu warunkowego w usłudze Azure Active Directory](active-directory-conditional-access-locations.md).


## <a name="client-apps"></a>Aplikacje klienckie

Warunek aplikacji klienta umożliwia stosowanie zasad do różnych typów aplikacji, takich jak:

- Witryny sieci Web i usług
- Aplikacje mobilne i aplikacje komputerowe. 

![Warunki](./media/active-directory-conditional-access-conditions/04.png)

Aplikacja zostanie sklasyfikowany jako:

- Witryny sieci web lub usługi korzysta z protokołów Usługa rejestracji Jednokrotnej w sieci web, SAML, WS-Fed lub OpenID Connect poufne klienta.

- A aplikację mobilną lub aplikację, gdy jest używana dla natywnego klienta aplikacji mobilnej, OpenID Connect.

Aby uzyskać pełną listę aplikacji klienckich, których można używać w zasadach dostępu warunkowego, zobacz [informacje techniczne dotyczące usługi Azure Active Directory dostępu warunkowego](active-directory-conditional-access-technical-reference.md#client-apps-condition).

Typowe przypadki użycia tego warunku są zasadami, które:

- Wymagaj [zgodnego urządzenia](active-directory-conditional-access-policy-connected-applications.md) dla aplikacji mobilnych i klasycznych, które pobierać dużych ilości danych na urządzeniu, umożliwiając dostęp z dowolnego urządzenia za pomocą przeglądarki.

- Zablokuj dostęp z aplikacji sieci web, ale zezwalaj na dostęp z aplikacji mobilnych i klasycznych.

Oprócz przy użyciu protokołów nowoczesnego uwierzytelniania i logowania jednokrotnego w sieci web, ten warunek można zastosować do aplikacji poczty, które używają programu Exchange ActiveSync, takich jak aplikacje natywnego programu pocztowego w większości smartfonów. Obecnie aplikacji klienta przy użyciu starszej wersji protokołów muszą być zabezpieczone za pomocą usług AD FS.

 Aby uzyskać więcej informacji, zobacz:

- [Konfigurowanie programu SharePoint Online i Exchange Online dla usługi Azure Active Directory dostępu warunkowego](active-directory-conditional-access-no-modern-authentication.md)
 
- [Azure Active Directory na podstawie aplikacji dostępu warunkowego](active-directory-conditional-access-mam.md) 








## <a name="next-steps"></a>Kolejne kroki

- Jeśli chcesz wiedzieć, jak skonfigurować zasady dostępu warunkowego, zobacz [wprowadzenie dostępu warunkowego w usłudze Azure Active Directory](active-directory-conditional-access-azure-portal-get-started.md).

- Jeśli wszystko jest gotowe do skonfigurowania zasad dostępu warunkowego dla danego środowiska, zobacz [najlepszych rozwiązań dotyczących dostępu warunkowego w usłudze Azure Active Directory](active-directory-conditional-access-best-practices.md). 

