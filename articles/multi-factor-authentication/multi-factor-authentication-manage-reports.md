---
title: "Raportów dotyczących dostępu i użycia dla usługi Azure MFA | Dokumentacja firmy Microsoft"
description: "Opisuje sposób użycia funkcji Azure Multi-Factor Authentication — raportów."
services: multi-factor-authentication
documentationcenter: 
author: MicrosoftGuyJFlo
manager: mtillman
ms.assetid: 3f6b33c4-04c8-47d4-aecb-aa39a61c4189
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2017
ms.author: joflore
ms.reviewer: richagi
ms.openlocfilehash: 696f4ae3cb479a208e73e53a9a9a437caeabd294
ms.sourcegitcommit: 0e1c4b925c778de4924c4985504a1791b8330c71
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/06/2018
---
# <a name="reports-in-azure-multi-factor-authentication"></a>Raporty w uwierzytelnianie wieloskładnikowe platformy Azure

Usługa Azure Multi-Factor Authentication udostępnia kilka raportów, które mogą być używane przez Ciebie i Twojej organizacji, które są dostępne za pośrednictwem portalu Azure. W poniższej tabeli wymieniono dostępne raporty:

| Raport | Lokalizacja | Opis |
|:--- |:--- |:--- |
| Historia zablokowanego użytkownika | Usługi Azure AD > serwera usługi MFA > Zablokuj/Odblokuj użytkowników | Przedstawia historię żądań zablokowania lub odblokowania użytkowników. |
| Alerty użycia i oszustwa | Usługi Azure AD > logowania | Zawiera informacje dotyczące użycia ogólnej, podsumowanie dotyczące użytkownika i szczegóły użytkownika; jak również historię alertów oszustwa przesłanych określony zakres dat. |
| Użycie lokalnego składników | Usługi Azure AD > serwera usługi MFA > raport aktywności | Zawiera informacje dotyczące ogólnej użycia dla usługi MFA za pomocą rozszerzenia serwera NPS, usług AD FS, a serwer usługi MFA. |
| Historia pominiętego użytkownika | Usługi Azure AD > serwera usługi MFA > jednorazowe obejście | Zawiera historię żądań ominięcia usługi Multi-Factor Authentication dla użytkownika. |
| Stan serwera | Usługi Azure AD > serwera usługi MFA > Stan serwera | Wyświetla stan serwerów usługi Multi-Factor Authentication skojarzonych z Twoim kontem. |

## <a name="view-reports"></a>Wyświetlanie raportów 

1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com).
2. Po lewej stronie, wybierz **usługi Azure Active Directory** > **serwera usługi MFA**.
3. Wybierz raport, który chcesz wyświetlić.

   <center>![Chmura](./media/multi-factor-authentication-manage-reports/report.png)</center>

## <a name="powershell-reporting"></a>Raportowanie programu PowerShell

Określenie użytkowników, którzy zarejestrowany dla usługi MFA za pomocą programu PowerShell, który jest zgodny.

```Get-MsolUser -All | where {$_.StrongAuthenticationMethods -ne $null} | Select-Object -Property UserPrincipalName```

Określ użytkowników, którzy nie została zarejestrowana dla usługi MFA za pomocą programu PowerShell, który jest zgodny.

```Get-MsolUser -All | where {$_.StrongAuthenticationMethods.Count -eq 0} | Select-Object -Property UserPrincipalName```

## <a name="next-steps"></a>Kolejne kroki

* [Dla użytkowników](end-user/multi-factor-authentication-end-user.md)
* [Gdzie można wdrożyć](multi-factor-authentication-get-started.md)
