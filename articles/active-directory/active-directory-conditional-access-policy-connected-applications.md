---
title: "Konfigurowanie zasad dostępu warunkowego opartego na urządzenia usługi Azure Active Directory | Dokumentacja firmy Microsoft"
description: "Informacje o sposobie konfigurowania zasad dostępu warunkowego opartego na urządzenia usługi Azure Active Directory."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: a27862a6-d513-43ba-97c1-1c0d400bf243
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/04/2017
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: a1aa2d4b4cc3acafb1f255d95e76abd80fb40596
ms.sourcegitcommit: 7136d06474dd20bb8ef6a821c8d7e31edf3a2820
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/05/2017
---
# <a name="configure-azure-active-directory-device-based-conditional-access-policies"></a>Konfigurowanie zasad dostępu warunkowego opartego na urządzenia usługi Azure Active Directory

Z [dostępu warunkowego w usłudze Azure Active Directory (Azure AD)](active-directory-conditional-access-azure-portal.md), można dostosować sposób autoryzowani użytkownicy mają dostęp do zasobów. Na przykład ograniczyć dostęp do niektórych zasobów do zaufanych urządzeń. Zasady dostępu warunkowego, która wymaga zaufanego urządzenia jest nazywana zasad dostępu warunkowego opartego na urządzeniu.

Ten temat zawiera informacje dotyczące sposobu konfigurowania zasad dostępu warunkowego opartego na urządzeniu dla Azure AD połączonych aplikacji. 


## <a name="before-you-begin"></a>Przed rozpoczęciem

Ties dostępu warunkowego opartego na urządzeniu **dostępu warunkowego dla usługi Azure AD** i **zarządzania urządzeniami usługi Azure AD razem**. Jeśli nie masz doświadczenia z jednym z tych obszarów jeszcze, najpierw należy przeczytać następujące tematy:

- **[Dostęp warunkowy w usłudze Azure Active Directory](active-directory-conditional-access-azure-portal.md)**  — ten temat zawiera omówienie pojęć dotyczących dostępu warunkowego i terminologię pokrewne.

- **[Wprowadzenie do zarządzania urządzeniami w usłudze Azure Active Directory](device-management-introduction.md)**  — ten temat zawiera omówienie różnych opcji, należy podłączyć urządzenia z usługą Azure AD. 


## <a name="trusted-devices"></a>Zaufanych urządzeń

W świecie pierwszy mobile, najpierw chmury Azure Active Directory umożliwia logowanie jednokrotne do urządzeń, aplikacji i usług z dowolnego miejsca. Dla niektórych zasobów w danym środowisku, udzielanie dostępu do odpowiednich użytkowników może nie być wystarczająca. Oprócz odpowiednich użytkowników może również wymagać zaufanego urządzenia ma być używany do uzyskania dostępu do zasobu. W danym środowisku, można zdefiniować zaufanego urządzenia oparte na następujących składników:

- [Platform urządzeń](active-directory-conditional-access-azure-portal.md#device-platforms) na urządzeniu
- Określa, czy urządzenie jest zgodne
- Określa, czy urządzenie jest przyłączony do domeny 

[Platform urządzeń](active-directory-conditional-access-azure-portal.md#device-platforms) charakteryzuje się systemu operacyjnego, który działa na urządzeniu. W zasadach dostępu warunkowego opartego na urządzeniu można ograniczyć dostęp do niektórych zasobów do konkretnych platform sprzętowych.



W zasadach dostępu warunkowego opartego na urządzeniu można wymagać zaufanych urządzeń może być oznaczony jako zgodne.

![Aplikacje w chmurze](./media/active-directory-conditional-access-policy-connected-applications/24.png)

Urządzenia może być oznaczony jako zgodne w katalogu przez:

- Intune 
- Urządzeniami przenośnymi innej firmy zarządzanych systemu, która zarządza urządzeniami z systemem Windows 10 za pomocą integracji z usługą Azure AD 
 
  

Tylko urządzenia, które są połączone z usługą Azure AD może być oznaczony jako zgodne. Podłącz urządzenie do usługi Azure Active Directory, masz następujące opcje: 

- Azure AD w zarejestrowany
- Azure AD dołączony
- Hybrydowe przyłączonych do usługi Azure AD

    ![Aplikacje w chmurze](./media/active-directory-conditional-access-policy-connected-applications/26.png)

Jeśli masz nakłady zasobów lokalnej usługi Active Directory (AD), można rozważyć urządzeń, które nie są podłączone do usługi Azure AD, ale przyłączony do usługi AD jako zaufane.

![Aplikacje w chmurze](./media/active-directory-conditional-access-policy-connected-applications/25.png)


## <a name="next-steps"></a>Następne kroki

Przed skonfigurowaniem zasad dostępu warunkowego opartego na urządzeniu w danym środowisku, należy podjąć przyjrzeć się [najlepszych rozwiązań dotyczących dostępu warunkowego w usłudze Azure Active Directory](active-directory-conditional-access-best-practices.md).

