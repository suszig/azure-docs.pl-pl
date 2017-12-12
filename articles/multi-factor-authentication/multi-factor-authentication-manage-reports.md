---
title: "Raportów dotyczących dostępu i użycia dla usługi Azure MFA | Dokumentacja firmy Microsoft"
description: "Opisuje sposób użycia funkcji Azure Multi-Factor Authentication — raportów."
services: multi-factor-authentication
documentationcenter: 
author: MicrosoftGuyJFlo
manager: mtillman
editor: curtand
ms.assetid: 3f6b33c4-04c8-47d4-aecb-aa39a61c4189
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/03/2017
ms.author: joflore
ms.reviewer: richagi
ms.openlocfilehash: 32697eea410cb9afaa0e4347acd1a280fcf94289
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/11/2017
---
# <a name="reports-in-azure-multi-factor-authentication"></a>Raporty w uwierzytelnianie wieloskładnikowe platformy Azure

Usługa Azure Multi-Factor Authentication udostępnia kilka raportów, które mogą być używane przez Ciebie i Twojej organizacji. Te raporty są dostępne za pośrednictwem portalu zarządzania usługi Multi-Factor Authentication. W poniższej tabeli wymieniono dostępne raporty:

| Raport | Opis |
|:--- |:--- |
| Sposób użycia |Użycie raporty zawierają informacje na ogólne użycie, użytkownik Podsumowanie i szczegóły użytkownika. |
| Stan serwera |Ten raport wyświetla stan serwerów usługi Multi-Factor Authentication skojarzonych z Twoim kontem. |
| Historia zablokowanego użytkownika |Te raporty Pokaż historię żądań zablokowania lub odblokowania użytkowników. |
| Historia pominiętego użytkownika |Pokazuje historię żądań ominięcia usługi Multi-Factor Authentication dla użytkownika numeru telefonu. |
| Alert oszustwa |Wyświetla historię alertów oszustwa przesłanych w określonym zakresie daty. |
| W kolejce |Prezentuje listę raportów w kolejce do przetwarzania i ich stan. Po zakończeniu raportu zostanie podane łącze do pobrania lub wyświetlenia raportu. |

## <a name="view-reports"></a>Wyświetlanie raportów

1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com).
2. Po lewej stronie, wybierz **usługi Azure Active Directory** > **użytkowników i grup** > **wszyscy użytkownicy** > **Multi-Factor Uwierzytelnianie**.
3. W obszarze **uwierzytelnianie wieloskładnikowe**, wybierz pozycję **ustawienia usługi**. W dolnej części w obszarze **Zarządzaj zaawansowanymi ustawieniami i wyświetlanie raportów**, wybierz pozycję **przejdź do portalu**.
4. W portalu zarządzania Azure Multi-Factor Authentication należy wybrać typ raportu z **wyświetlić raport** sekcji na lewym pasku nawigacyjnym.

<center>![Chmura](./media/multi-factor-authentication-manage-reports/report.png)</center>

## <a name="powershell-reporting"></a>Raportowanie programu PowerShell

Określenie użytkowników, którzy zarejestrowany dla usługi MFA za pomocą programu PowerShell, który jest zgodny.

```Get-MsolUser -All | where {$_.StrongAuthenticationMethods -ne $null} | Select-Object -Property UserPrincipalName```

Określ użytkowników, którzy nie została zarejestrowana dla usługi MFA za pomocą programu PowerShell, który jest zgodny.

```Get-MsolUser -All | where {$_.StrongAuthenticationMethods.Count -eq 0} | Select-Object -Property UserPrincipalName```

**Dodatkowe zasoby**

* [Dla użytkowników](end-user/multi-factor-authentication-end-user.md)
* [Usługa Azure Multi-Factor Authentication w witrynie MSDN](https://msdn.microsoft.com/library/azure/dn249471.aspx)
