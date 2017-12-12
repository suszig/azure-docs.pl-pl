---
title: Azure Active Directory na podstawie certyfikatu uwierzytelniania w systemie Android | Dokumentacja firmy Microsoft
description: "Więcej informacji na temat obsługiwanych scenariuszy i wymagania dotyczące konfigurowania uwierzytelniania opartego na certyfikatach w rozwiązaniach urządzeń z systemem Android"
services: active-directory
author: MarkusVi
documentationcenter: na
manager: mtillman
ms.assetid: c6ad7640-8172-4541-9255-770f39ecce0e
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 10/13/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: d3e2ed44777a5963c8bd88f6479c4c58bebdb940
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/11/2017
---
# <a name="azure-active-directory-certificate-based-authentication-on-android"></a>Azure Active Directory na podstawie certyfikatu uwierzytelniania w systemie Android


Uwierzytelnianie oparte na certyfikatach (CBA) pozwala na uwierzytelniony przez usługę Azure Active Directory przy użyciu certyfikatu klienta na urządzeniu z systemem Windows, Android lub iOS podczas łączenia Twoje konto programu Exchange online:

* Aplikacje mobilne pakietu Office, takich jak Microsoft Outlook i Microsoft Word   
* Klienci programu Exchange ActiveSync (EAS)

Konfigurowanie tej funkcji eliminuje potrzebę wprowadzić kombinacja nazwy użytkownika i hasła do niektórych poczty i aplikacje Microsoft Office na urządzeniu przenośnym.

W tym temacie przedstawiono wymagania i obsługiwane scenariusze związane z konfigurowaniem CBA na urządzeniu z systemem iOS(Android) dla użytkowników dzierżaw Office 365 Enterprise, Business, edukacji, instytucji rządowych Stanów Zjednoczonych, Chin i planów Niemczech.



Ta funkcja jest dostępna w wersji zapoznawczej w planach Office 365 instytucji rządowych Stanów Zjednoczonych obrony i federalne.


## <a name="microsoft-mobile-applications-support"></a>Pomoc techniczna firmy Microsoft aplikacji dla urządzeń przenośnych
| Aplikacje | Pomoc techniczna |
| --- | --- |
| Azure Information Protection aplikacji |![Zaznacz][1] |
| Portal firmy usługi Intune |![Zaznacz][1] |
| Microsoft Teams |![Zaznacz][1] |
| OneNote |![Zaznacz][1] |
| OneDrive |![Zaznacz][1] |
| Outlook |![Zaznacz][1] |
| Power BI |![Zaznacz][1] |
| Skype dla firm |![Zaznacz][1] |
| Word / Excel / PowerPoint |![Zaznacz][1] |
| Yammer |![Zaznacz][1] |


### <a name="implementation-requirements"></a>Wymagania dotyczące implementacji

Wersja systemu operacyjnego urządzenia musi być Android 5.0 (interfejs typu lizak) lub nowszym.

Serwer federacyjny musi być skonfigurowany.  

Dla usługi Azure Active Directory odwołać certyfikat klienta tokenu usług AD FS musi mieć następujące oświadczeń:  

* `http://schemas.microsoft.com/ws/2008/06/identity/claims/<serialnumber>`  
  (Numer seryjny certyfikatu klienta)
* `http://schemas.microsoft.com/2012/12/certificatecontext/field/<issuer>`  
  (String wystawcy certyfikatu klienta)

Usługa Azure Active Directory dodaje te oświadczenia do tokenu odświeżania, jeśli są dostępne w tokenu usług AD FS (lub inne tokenu SAML). Gdy token odświeżania musi być weryfikowane, te informacje służy do sprawdzania odwołania.

Najlepszym rozwiązaniem należy zaktualizować strony błędów usług AD FS z instrukcjami dotyczącymi sposobu uzyskania certyfikatu użytkownika.  
Aby uzyskać więcej informacji, zobacz [dostosowywanie stron AD FS logowania](https://technet.microsoft.com/library/dn280950.aspx).  

Wyślij niektóre aplikacje pakietu Office (z włączoną nowoczesnego uwierzytelniania) "*= monit logowania*" do usługi Azure AD w żądaniu. Domyślnie program Azure AD tłumaczy to w żądaniu, aby usługi AD FS do "*wauth = usernamepassworduri*" (zapyta usług AD FS do uwierzytelniania U/P) i "*wfresh = 0*" (zapyta usług AD FS, aby zignorować stan logowania jednokrotnego i wykonać świeże uwierzytelnianie). Aby włączyć uwierzytelnianie oparte na certyfikatach dla tych aplikacji, należy zmodyfikować domyślne zachowanie usługi Azure AD. Ustaw wartość "*PromptLoginBehavior*"w ustawieniach domeny federacyjnej do"*wyłączone*".
Można użyć [MSOLDomainFederationSettings](/powershell/module/msonline/set-msoldomainfederationsettings?view=azureadps-1.0) polecenia cmdlet do wykonania tego zadania:

`Set-MSOLDomainFederationSettings -domainname <domain> -PromptLoginBehavior Disabled`



## <a name="exchange-activesync-clients-support"></a>Obsługa klientów programu Exchange ActiveSync
Niektóre aplikacje programu Exchange ActiveSync w systemie Android 5.0 (interfejs typu lizak) lub nowszym są obsługiwane. Aby ustalić, czy aplikację poczty e-mail obsługuje tę funkcję, skontaktuj się z deweloperem aplikacji.


## <a name="next-steps"></a>Następne kroki

Jeśli chcesz skonfigurować uwierzytelnianie oparte na certyfikatach w danym środowisku, zobacz [wprowadzenie do uwierzytelniania opartego na certyfikatach w systemie Android](active-directory-certificate-based-authentication-get-started.md) instrukcje.

<!--Image references-->
[1]: ./media/active-directory-certificate-based-authentication-android/ic195031.png
