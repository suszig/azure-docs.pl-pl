---
title: "Raportów dotyczących dostępu i użycia dla usługi Azure MFA | Dokumentacja firmy Microsoft"
description: "Opisuje sposób użycia funkcji Azure Multi-Factor Authentication — raportów."
services: multi-factor-authentication
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
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
ms.openlocfilehash: a0ac1711b6bfb8f461cd775ed1f3409925643615
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/15/2017
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

1. Zaloguj się do [klasycznej witryny Azure Portal](https://manage.windowsazure.com).
2. W obszarze po lewej stronie wybierz pozycję Active Directory.
3. Wykonaj jedną z następujących dwóch opcji, w zależności od tego, czy używać dostawcy usługi MFA:
   * **Opcja 1**: kliknij kartę dostawców uwierzytelniania wieloskładnikowego. Wybierz dostawcę uwierzytelniania MFA i kliknij przycisk **Zarządzaj** znajdujący się u dołu.
   * **Opcja 2**: Wybierz swój katalog, a następnie przejdź do **Konfiguruj** kartę. W sekcji uwierzytelniania wieloskładnikowego wybierz pozycję **Zarządzaj ustawieniami usługi**. Na dole strony Ustawienia usługi MFA, kliknij polecenie Przejdź do portalu łącza.
4. W portalu zarządzania Azure Multi-Factor Authentication należy wybrać typ raportu z **wyświetlić raport** sekcji na lewym pasku nawigacyjnym.

<center>![Chmura](./media/multi-factor-authentication-manage-reports/report.png)</center>

## <a name="powershell-reporting"></a>Raportowanie programu PowerShell

Określenie użytkowników, którzy zarejestrowany dla usługi MFA za pomocą programu Powershell, który jest zgodny.

```Get-MsolUser -All | where {$_.StrongAuthenticationMethods -ne $null} | Select-Object -Property UserPrincipalName```

Określ użytkowników, którzy nie została zarejestrowana dla usługi MFA za pomocą programu Powershell, który jest zgodny.

```Get-MsolUser -All | where {$_.StrongAuthenticationMethods.Count -eq 0} | Select-Object -Property UserPrincipalName```

**Dodatkowe zasoby**

* [Dla użytkowników](end-user/multi-factor-authentication-end-user.md)
* [Usługa Azure Multi-Factor Authentication w witrynie MSDN](https://msdn.microsoft.com/library/azure/dn249471.aspx)
