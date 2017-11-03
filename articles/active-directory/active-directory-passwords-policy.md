---
title: 'Zasady: Azure AD SSPR | Dokumentacja firmy Microsoft'
description: "Azure AD samoobsługowego resetowania hasła opcji zasad"
services: active-directory
keywords: "Zarządzanie hasłami w usłudze Active directory, zarządzanie hasłami, usługi Azure AD samodzielnego resetowania hasła usługi"
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.reviewer: sahenry
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2017
ms.author: joflore
ms.custom: it-pro
ms.openlocfilehash: 5c33f08e54d522e0eea13a3e267f14f407fc59b6
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/31/2017
---
# <a name="password-policies-and-restrictions-in-azure-active-directory"></a>Ograniczenia w usłudze Azure Active Directory i zasad haseł

W tym artykule opisano zasady haseł i wymagania dotyczące złożoności skojarzone z kont użytkowników przechowywanych w dzierżawie usługi Azure AD.

## <a name="administrator-password-policy-differences"></a>Różnice zasad hasła administratora

Firma Microsoft wymusza silne domyślne zasady **dwóch bram** w przypadku resetowania hasła dla dowolnej roli administratora platformy Azure (np. Administrator globalny, Administrator pomocy technicznej, Administrator haseł itp.)

To powoduje wyłączenie administratorów za pomocą pytań zabezpieczających i wymusza poniżej.

Dwa gate zasady wymagające dwóch rodzajów danych uwierzytelniania (adres e-mail **i** numer telefonu), stosuje się w następujących okolicznościach

* Wszystkie role administratora platformy Azure
  * Administrator pomocy technicznej
  * Administrator pomocy technicznej usługi
  * Administrator rozliczeń
  * Obsługa Tier1 partnera
  * Obsługa Tier2 partnera
  * Administrator usługi Exchange
  * Administrator usługi Lync
  * Administrator konta użytkownika
  * Składniki zapisywania katalogu
  * Administrator globalny Administrator/firmy
  * Administrator usługi programu SharePoint
  * Administrator zgodności
  * Administrator aplikacji
  * Administrator zabezpieczeń
  * Administrator ról uprzywilejowanych
  * Administrator usługi Intune
  * Administrator usługi serwera Proxy aplikacji
  * Administrator programu CRM usługi
  * Administrator usługi Power BI
  
* upływie 30 dni w ramach próbnego **lub**
* Występuje niestandardowych domeny (contoso.com) **lub**
* Azure AD Connect synchronizuje tożsamości z katalogu lokalnego

### <a name="exceptions"></a>Wyjątki
Zasady co brama, wymagających jednego elementu danych uwierzytelniania (adres e-mail **lub** numer telefonu), stosuje się w następujących okolicznościach

* Pierwsze 30 dni korzystania z wersji próbnej **lub**
* Nie ma domen niestandardowych (*. onmicrosoft.com) **i** Azure AD Connect nie synchronizuje tożsamości


## <a name="userprincipalname-policies-that-apply-to-all-user-accounts"></a>UserPrincipalName zasad, które są stosowane do wszystkich kont użytkowników

Każdego konta użytkownika, który musi się zalogować do usługi Azure AD musi mieć wartość atrybutu unikatowego użytkownika głównej nazwy (UPN) skojarzonych z jego konta. Tabela poniżej zawiera zasady, które dotyczą zarówno lokalnego konta użytkownika usługi Active Directory synchronizowane w chmurze i kont użytkowników tylko w chmurze.

| Właściwość | Wymagania dotyczące UserPrincipalName |
| --- | --- |
| Znaki są dozwolone |<ul> <li>A-Z</li> <li>- z</li><li>0 – 9</li> <li> . - \_ ! \# ^ \~</li></ul> |
| Znaki nie są dozwolone |<ul> <li>Wszelkie "@" znak, który nie jest oddzielając nazwy użytkownika w domenie.</li> <li>Nie może zawierać znaku kropki "." bezpośrednio przed "@" — symbol</li></ul> |
| Ograniczenia długości |<ul> <li>Łączna długość nie może przekraczać 113 znaków</li><li>64 znaki, przed "@" — symbol</li><li>48 znaków po "@" — symbol</li></ul> |

## <a name="password-policies-that-apply-only-to-cloud-user-accounts"></a>Zasady haseł, które są stosowane tylko do kont użytkowników w chmurze

W poniższej tabeli opisano ustawienia zasad dostępne hasła, które można zastosować do kont użytkowników, które są tworzone i zarządzane w usłudze Azure AD.

| Właściwość | Wymagania |
| --- | --- |
| Znaki są dozwolone |<ul><li>A-Z</li><li>- z</li><li>0 – 9</li> <li>@ # $ % ^ & * - _ ! + = [ ] { } &#124; \ : ‘ , . ? / ` ~ “ ( ) ;</li></ul> |
| Znaki nie są dozwolone |<ul><li>Znaki Unicode</li><li>Spacje</li><li> **Silne hasła tylko**: nie może zawierać znaku kropki "." bezpośrednio przed "@" — symbol</li></ul> |
| Ograniczenia haseł |<ul><li>minimalnej 8 znaków i maksymalnie 16 znaków.</li><li>**Silne hasła tylko**: wymaga 3 z 4 z następujących czynności:<ul><li>Małe litery</li><li>Wielkie litery</li><li>Cyfry (0 – 9)</li><li>Symbole (zobacz powyżej ograniczeń hasła)</li></ul></li></ul> |
| Okres wygasania haseł |<ul><li>Wartość domyślna: **90** dni </li><li>Wartość jest można skonfigurować za pomocą polecenia cmdlet Set-MsolPasswordPolicy z usługi Azure Active Directory modułu dla środowiska Windows PowerShell.</li></ul> |
| Powiadomienie o wygaśnięciu hasła |<ul><li>Wartość domyślna: **14** dni (do wygaśnięcia hasła)</li><li>Wartość jest można skonfigurować za pomocą polecenia cmdlet Set-MsolPasswordPolicy.</li></ul> |
| Wygaśnięcia hasła |<ul><li>Wartość domyślna: **false** dni (wskazuje, że wygaśnięcie hasła jest włączona) </li><li>Wartość można skonfigurować dla poszczególnych kont użytkowników przy użyciu polecenia cmdlet Set-MsolUser. </li></ul> |
| Hasło **zmienić** historii |Hasło ostatnio **nie** można użyć ponownie po **zmiana** hasła. |
| Hasło **zresetować** historii | Hasło ostatnio **może** można użyć ponownie po **Resetowanie** zapomniane hasło. |
| Blokada konta |Po 10 nieudanych próbach zalogowania (nieprawidłowe hasło) użytkownik będzie zablokowany przez jedną minutę. Dodatkowo niepoprawne prób zalogowania blokady użytkownika na zwiększenie wartości czasu trwania. |

## <a name="set-password-expiration-policies-in-azure-active-directory"></a>Ustawianie zasad wygasania haseł w usłudze Azure Active Directory

Administrator globalny dla usługi w chmurze firmy Microsoft umożliwia Microsoft Azure Active Directory modułu dla środowiska Windows PowerShell Ustaw hasła użytkowników nie wygaśnie. Użyciu polecenia cmdlet programu Windows PowerShell można również usunąć nigdy nie wygasa konfiguracji lub aby wyświetlić użytkowników, których hasła są konfigurowane nie wygaśnie. Niniejsze wytyczne mają zastosowanie do innych dostawców, takich jak Microsoft Intune i Office 365, które również polegać na Microsoft Azure Active Directory dla tożsamości i usługi katalogowe.

> [!NOTE]
> Można skonfigurować tylko haseł dla kont użytkowników, które nie są synchronizowane przez synchronizacji katalogów nie wygaśnie. Aby uzyskać więcej informacji na temat synchronizacji katalogów zobacz[AD z usługą Azure AD Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect).
>
>

## <a name="set-or-check-password-policies-using-powershell"></a>Ustawianie lub Sprawdzanie zasad haseł przy użyciu programu PowerShell

Aby rozpocząć pracę, musisz [Pobierz i zainstaluj moduł programu PowerShell usługi Azure AD](https://docs.microsoft.com/powershell/module/Azuread/?view=azureadps-2.0). Gdy jest zainstalowany, może wykonaj poniższe kroki, aby skonfigurować każde pole.

### <a name="how-to-check-expiration-policy-for-a-password"></a>Jak sprawdzić zasady wygasania hasła
1. Podłącz do środowiska Windows PowerShell przy użyciu poświadczeń administratora firmy.
2. Wykonaj jedną z następujących poleceń:

   * Aby zobaczyć, czy ustawiono pojedynczego użytkownika, hasło nigdy nie wygasa, uruchom następujące polecenie cmdlet, za pomocą głównej nazwy użytkownika (UPN) (na przykład aprilr@contoso.onmicrosoft.com) lub identyfikator użytkownika, aby sprawdzić użytkownika:`Get-MSOLUser -UserPrincipalName <user ID> | Select PasswordNeverExpires`
   * Aby wyświetlić ustawienia "Hasło nigdy nie wygasa" dla wszystkich użytkowników, uruchom następujące polecenie cmdlet:`Get-MSOLUser | Select UserPrincipalName, PasswordNeverExpires`

### <a name="set-a-password-to-expire"></a>Ustaw hasło wygaśnie

1. Podłącz do środowiska Windows PowerShell przy użyciu poświadczeń administratora firmy.
2. Wykonaj jedną z następujących poleceń:

   * Aby ustawić hasło jednego użytkownika tak, aby hasło wygaśnie, uruchom następujące polecenie cmdlet za pomocą głównej nazwy użytkownika (UPN) lub identyfikator użytkownika, użytkownik:`Set-MsolUser -UserPrincipalName <user ID> -PasswordNeverExpires $false`
   * Aby ustawić hasła wszystkich użytkowników w organizacji, co umożliwi wygasną, użyj następującego polecenia cmdlet:`Get-MSOLUser | Set-MsolUser -PasswordNeverExpires $false`

### <a name="set-a-password-to-never-expire"></a>Ustaw hasło nigdy nie wygasa

1. Podłącz do środowiska Windows PowerShell przy użyciu poświadczeń administratora firmy.
2. Wykonaj jedną z następujących poleceń:

   * Aby ustawić hasło nigdy nie wygasa dla wielu użytkowników, uruchom następujące polecenie cmdlet za pomocą głównej nazwy użytkownika (UPN) lub identyfikator użytkownika, użytkownik:`Set-MsolUser -UserPrincipalName <user ID> -PasswordNeverExpires $true`
   * Aby ustawić hasła wszystkich użytkowników w organizacji nigdy nie wygasa, uruchom następujące polecenie cmdlet:`Get-MSOLUser | Set-MsolUser -PasswordNeverExpires $true`

## <a name="next-steps"></a>Następne kroki

Poniższe linki dają dostęp do dodatkowych informacji dotyczących resetowania haseł za pomocą usługi Azure AD

* [Jak wykonać pomyślne wdrożenie SSPR?](active-directory-passwords-best-practices.md)
* [Resetowanie lub zmienianie hasła](active-directory-passwords-update-your-own-password.md).
* [Rejestrowanie się w celu samodzielnego resetowania hasła](active-directory-passwords-reset-register.md).
* [Masz pytanie Licencjonowanie?](active-directory-passwords-licensing.md)
* [Jakie dane są używane przez SSPR i jakie dane powinny można wypełnić dla użytkowników?](active-directory-passwords-data.md)
* [Jakie metody uwierzytelniania są dostępne dla użytkowników?](active-directory-passwords-how-it-works.md#authentication-methods)
* [Co to jest funkcji zapisywania zwrotnego haseł i dlaczego I interesujących go?](active-directory-passwords-writeback.md)
* [Jak zgłosić w działaniu w SSPR](active-directory-passwords-reporting.md)
* [Co to są wszystkie opcje w SSPR i do czego ich znaczenie?](active-directory-passwords-how-it-works.md)
* [Myślę, że dany element jest uszkodzony. Jak rozwiązywać problemy z SSPR](active-directory-passwords-troubleshoot.md)
* [Masz pytania, na które nie objęte gdzieś else](active-directory-passwords-faq.md)
