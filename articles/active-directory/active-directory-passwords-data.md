---
title: "Wymagania dotyczące danych usługi Azure AD SSPR | Dokumentacja firmy Microsoft"
description: "Wymagania dotyczące danych dla usługi Azure AD samoobsługi hasła resetowania i sposobu ich spełnienia"
services: active-directory
keywords: 
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
ms.openlocfilehash: 7936f47007285e3f7fa1d3220efa022a6e3881ca
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/31/2017
---
# <a name="deploy-password-reset-without-requiring-end-user-registration"></a>Wdrażanie resetowania hasła bez konieczności rejestrowania użytkownika końcowego

Wdrażanie funkcji samoobsługowego resetowania hasła (SSPR) wymaga dane uwierzytelniania obecności. Niektóre organizacje mają użytkownikom wprowadź dane uwierzytelniania samodzielnie, ale w wielu organizacjach preferowane można zsynchronizować z istniejącymi danymi w usłudze Active Directory. Prawidłowo sformatowane dane w katalogu lokalnego i skonfigurować [Azure AD Connect przy użyciu ustawień ekspresowych](./connect/active-directory-aadconnect-get-started-express.md), że dane będą dostępne dla usługi Azure AD i SSPR z bez udziału użytkownika.

Wszystkie numery telefonów musi być w formacie + CountryCode PhoneNumber przykład: + 1 4255551234 działała poprawnie.

> [!NOTE]
> Resetowanie hasła nie obsługuje rozszerzeń telefonu. Nawet w formacie 12345 4255551234 + 1 X rozszerzenia zostały usunięte przed wykonaniem połączenia.

## <a name="fields-populated"></a>Pól

Jeśli używasz ustawienia domyślne w programie Azure AD Connect zostały wprowadzone następujące mapowania.

| Lokalne usługi AD | Azure AD | Azure AD Authentication informacje kontaktowe |
| --- | --- | --- |
| TelephoneNumber | Telefon biurowy | Alternatywne telefonu |
| Telefon komórkowy | Telefon komórkowy | Numer telefonu |


## <a name="security-questions-and-answers"></a>Pytania zabezpieczające i odpowiedzi

Pytania zabezpieczające i odpowiedzi są bezpiecznie przechowywane w dzierżawie usługi Azure AD i są dostępne dla użytkowników za pośrednictwem jedynie [portal rejestracji SSPR](https://aka.ms/ssprsetup). Administratorzy nie widać lub zmodyfikować zawartość innym użytkownikom pytania i odpowiedzi.

### <a name="what-happens-when-a-user-registers"></a>Co się dzieje, gdy użytkownik rejestruje

Gdy użytkownik rejestruje, strony rejestracji ustawia następujące pola:

* Numer telefonu uwierzytelniania
* Uwierzytelnianie wiadomości E-mail
* Pytania zabezpieczające i odpowiedzi

Jeśli podano wartość **telefon komórkowy** lub **alternatywny adres E-mail**, użytkownicy mogą natychmiast używać tych wartości na resetowanie haseł, nawet jeśli nie zostały one zarejestrowane przez usługę. Ponadto użytkownicy zobaczyć te wartości podczas rejestrowania po raz pierwszy, a ich modyfikacji, jeśli chcesz, aby ich. Po ich pomyślnie zarejestrować, te wartości zostaną utrwalone w **numer telefonu uwierzytelniania** i **E-mail uwierzytelniania** pola odpowiednio.

## <a name="set-and-read-authentication-data-using-powershell"></a>Ustaw i odczytywanie danych uwierzytelniania przy użyciu programu PowerShell

Następujące pola można ustawić przy użyciu programu PowerShell

* Alternatywny adres E-mail
* Telefon komórkowy
* Telefon biurowy — można ustawiać tylko, jeśli nie synchronizacji z katalogiem lokalnym

### <a name="using-powershell-v1"></a>Przy użyciu programu PowerShell w wersji 1

Aby rozpocząć pracę, musisz [Pobierz i zainstaluj moduł programu PowerShell usługi Azure AD](https://msdn.microsoft.com/library/azure/jj151815.aspx#bkmk_installmodule). Gdy jest zainstalowany, można wykonać kroki, które należy wykonać, aby skonfigurować każde pole.

#### <a name="set-authentication-data-with-powershell-v1"></a>Zestaw danych uwierzytelniania za pomocą programu PowerShell w wersji 1

```
Connect-MsolService

Set-MsolUser -UserPrincipalName user@domain.com -AlternateEmailAddresses @("email@domain.com")
Set-MsolUser -UserPrincipalName user@domain.com -MobilePhone "+1 1234567890"
Set-MsolUser -UserPrincipalName user@domain.com -PhoneNumber "+1 1234567890"

Set-MsolUser -UserPrincipalName user@domain.com -AlternateEmailAddresses @("email@domain.com") -MobilePhone "+1 1234567890" -PhoneNumber "+1 1234567890"
```

#### <a name="read-authentication-data-with-powershellpowershell-v1"></a>Odczyt danych uwierzytelniania z PowerShellPowerShell V1

```
Connect-MsolService

Get-MsolUser -UserPrincipalName user@domain.com | select AlternateEmailAddresses
Get-MsolUser -UserPrincipalName user@domain.com | select MobilePhone
Get-MsolUser -UserPrincipalName user@domain.com | select PhoneNumber

Get-MsolUser | select DisplayName,UserPrincipalName,AlternateEmailAddresses,MobilePhone,PhoneNumber | Format-Table
```

#### <a name="authentication-phone-and-authentication-email-can-only-be-read-using-powershell-v1-using-the-commands-that-follow"></a>Numer telefonu uwierzytelniania i uwierzytelniania wiadomości E-mail mogą być odczytywane tylko przy użyciu programu Powershell w wersji 1 za pomocą poleceń, które należy wykonać

```
Connect-MsolService
Get-MsolUser -UserPrincipalName user@domain.com | select -Expand StrongAuthenticationUserDetails | select PhoneNumber
Get-MsolUser -UserPrincipalName user@domain.com | select -Expand StrongAuthenticationUserDetails | select Email
```

### <a name="using-powershell-v2"></a>Przy użyciu programu PowerShell w wersji 2

Aby rozpocząć pracę, musisz [pobrać i zainstalować moduł PowerShell usługi Azure AD w wersji 2](https://github.com/Azure/azure-docs-powershell-azuread/blob/master/Azure%20AD%20Cmdlets/AzureAD/index.md). Gdy jest zainstalowany, można wykonać kroki, które należy wykonać, aby skonfigurować każde pole.

Aby zainstalować szybko z obsługujących instalację modułu nowe wersje programu PowerShell, uruchom następujące polecenia, (pierwszy wiersz po prostu sprawdza, czy nazwa jest już zainstalowany):

```
Get-Module AzureADPreview
Install-Module AzureADPreview
Connect-AzureAD
```

#### <a name="set-authentication-data-with-powershell-v2"></a>Zestaw danych uwierzytelniania za pomocą programu PowerShell w wersji 2

```
Connect-AzureAD

Set-AzureADUser -ObjectId user@domain.com -OtherMails @("email@domain.com")
Set-AzureADUser -ObjectId user@domain.com -Mobile "+1 2345678901"
Set-AzureADUser -ObjectId user@domain.com -TelephoneNumber "+1 1234567890"

Set-AzureADUser -ObjectId user@domain.com -OtherMails @("emails@domain.com") -Mobile "+1 1234567890" -TelephoneNumber "+1 1234567890"
```

### <a name="read-authentication-data-with-powershell-v2"></a>Odczyt danych uwierzytelniania za pomocą programu PowerShell w wersji 2

```
Connect-AzureAD

Get-AzureADUser -ObjectID user@domain.com | select otherMails
Get-AzureADUser -ObjectID user@domain.com | select Mobile
Get-AzureADUser -ObjectID user@domain.com | select TelephoneNumber

Get-AzureADUser | select DisplayName,UserPrincipalName,otherMails,Mobile,TelephoneNumber | Format-Table
```

## <a name="next-steps"></a>Następne kroki

* [Jak wykonać pomyślne wdrożenie SSPR?](active-directory-passwords-best-practices.md)
* [Resetowanie lub zmienianie hasła](active-directory-passwords-update-your-own-password.md).
* [Rejestrowanie się w celu samodzielnego resetowania hasła](active-directory-passwords-reset-register.md).
* [Masz pytanie Licencjonowanie?](active-directory-passwords-licensing.md)
* [Jakie metody uwierzytelniania są dostępne dla użytkowników?](active-directory-passwords-how-it-works.md#authentication-methods)
* [Jakie są opcje zasad z SSPR?](active-directory-passwords-policy.md)
* [Co to jest funkcji zapisywania zwrotnego haseł i dlaczego I interesujących go?](active-directory-passwords-writeback.md)
* [Jak zgłosić w działaniu w SSPR](active-directory-passwords-reporting.md)
* [Co to są wszystkie opcje w SSPR i do czego ich znaczenie?](active-directory-passwords-how-it-works.md)
* [Myślę, że dany element jest uszkodzony. Jak rozwiązywać problemy z SSPR](active-directory-passwords-troubleshoot.md)
* [Masz pytania, na które nie objęte gdzieś else](active-directory-passwords-faq.md)
