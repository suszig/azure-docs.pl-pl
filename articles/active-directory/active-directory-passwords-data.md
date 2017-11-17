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
ms.openlocfilehash: ed82200bf81702bbe35a371e7d86676c2c27d8f4
ms.sourcegitcommit: c7215d71e1cdeab731dd923a9b6b6643cee6eb04
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/17/2017
---
# <a name="deploy-password-reset-without-requiring-end-user-registration"></a>Wdrażanie resetowania hasła bez konieczności rejestrowania użytkownika końcowego

Do wdrożenia usługi Azure Active Directory (Azure AD) samoobsługowego resetowania hasła (SSPR), dane uwierzytelniania musi być obecny. Niektóre organizacje mają wprowadź dane uwierzytelniania się użytkowników. Jednak w wielu organizacjach preferowane można zsynchronizować z danymi, która już istnieje w usłudze Active Directory. Zsynchronizowane dane będą dostępne dla usługi Azure AD i SSPR bez konieczności interakcji z użytkownikiem, jeśli użytkownik:
   * Poprawnie formatowania danych w katalogu lokalnym.
   * Skonfiguruj [Azure AD Connect przy użyciu ustawień ekspresowych](./connect/active-directory-aadconnect-get-started-express.md).

Aby zapewnić prawidłowe działanie, numerów telefonów musi być w formacie *+ CountryCode PhoneNumber*, na przykład 4255551234 + 1.

> [!NOTE]
> Resetowanie hasła nie obsługuje rozszerzeń telefonu. Nawet w formacie 12345 4255551234 + 1 X rozszerzenia zostały usunięte przed wykonaniem połączenia.

## <a name="fields-populated"></a>Pól

Jeśli używasz ustawienia domyślne w programie Azure AD Connect zostały wprowadzone następujące mapowania:

| Lokalna usługa Active Directory | Azure AD | Azure AD authentication informacje o kontaktach |
| --- | --- | --- |
| TelephoneNumber | Telefon biurowy | Alternatywne telefonu |
| Telefon komórkowy | Telefon komórkowy | Telefon |


## <a name="security-questions-and-answers"></a>Pytania zabezpieczające i odpowiedzi

Pytania zabezpieczające i odpowiedzi są bezpiecznie przechowywane w dzierżawie usługi Azure AD i są dostępne dla użytkowników za pośrednictwem jedynie [portal rejestracji SSPR](https://aka.ms/ssprsetup). Administratorzy nie widać lub zmodyfikować zawartość innym użytkownikom pytania i odpowiedzi.

### <a name="what-happens-when-a-user-registers"></a>Co się dzieje, gdy użytkownik rejestruje

Gdy użytkownik rejestruje, strony rejestracji ustawia następujące pola:

* **Numer telefonu uwierzytelniania**
* **Uwierzytelnianie wiadomości E-mail**
* **Pytania zabezpieczające i odpowiedzi**

Jeśli podano wartość **telefon komórkowy** lub **alternatywny adres e-mail**, użytkownicy mogą natychmiast używać tych wartości na resetowanie haseł, nawet jeśli nie zostały one zarejestrowane przez usługę. Ponadto użytkownicy będą widzieć te wartości podczas ich Zarejestruj po raz pierwszy, a ich ich modyfikować, jeśli chcą. Po zarejestrowaniu się pomyślnie, te wartości zostaną utrwalone w **numer telefonu uwierzytelniania** i **E-mail uwierzytelniania** pola odpowiednio.

## <a name="set-and-read-the-authentication-data-through-powershell"></a>Ustaw i odczytywanie danych uwierzytelniania za pomocą programu PowerShell

Za pomocą programu PowerShell można ustawić następujące pola:

* **Alternatywny adres e-mail**
* **Telefon komórkowy**
* **Telefon biurowy**: może zostać ustawiona tylko wtedy, gdy nie synchronizacja z katalogu lokalnego

### <a name="use-powershell-version-1"></a>Za pomocą programu PowerShell w wersji 1

Aby rozpocząć pracę, musisz [Pobierz i zainstaluj moduł programu PowerShell usługi Azure AD](https://msdn.microsoft.com/library/azure/jj151815.aspx#bkmk_installmodule). Po jest zainstalowany, należy użyć kroki, które należy wykonać, aby skonfigurować każde pole.

#### <a name="set-the-authentication-data-with-powershell-version-1"></a>Ustawia dane uwierzytelniania przy użyciu programu PowerShell w wersji 1

```
Connect-MsolService

Set-MsolUser -UserPrincipalName user@domain.com -AlternateEmailAddresses @("email@domain.com")
Set-MsolUser -UserPrincipalName user@domain.com -MobilePhone "+1 1234567890"
Set-MsolUser -UserPrincipalName user@domain.com -PhoneNumber "+1 1234567890"

Set-MsolUser -UserPrincipalName user@domain.com -AlternateEmailAddresses @("email@domain.com") -MobilePhone "+1 1234567890" -PhoneNumber "+1 1234567890"
```

#### <a name="read-the-authentication-data-with-powershell-version-1"></a>Odczytanie danych uwierzytelniania przy użyciu programu PowerShell w wersji 1

```
Connect-MsolService

Get-MsolUser -UserPrincipalName user@domain.com | select AlternateEmailAddresses
Get-MsolUser -UserPrincipalName user@domain.com | select MobilePhone
Get-MsolUser -UserPrincipalName user@domain.com | select PhoneNumber

Get-MsolUser | select DisplayName,UserPrincipalName,AlternateEmailAddresses,MobilePhone,PhoneNumber | Format-Table
```

#### <a name="read-the-authentication-phone-and-authentication-email-options"></a>Opcje uwierzytelniania telefonu i poczty E-mail uwierzytelniania do odczytu

Odczytać **numer telefonu uwierzytelniania** i **E-mail uwierzytelniania** podczas korzystania z programu PowerShell w wersji 1, użyj następujących poleceń:

```
Connect-MsolService
Get-MsolUser -UserPrincipalName user@domain.com | select -Expand StrongAuthenticationUserDetails | select PhoneNumber
Get-MsolUser -UserPrincipalName user@domain.com | select -Expand StrongAuthenticationUserDetails | select Email
```

### <a name="use-powershell-version-2"></a>Za pomocą programu PowerShell w wersji 2

Aby rozpocząć pracę, musisz [Pobierz i zainstaluj moduł programu PowerShell w wersji 2 usługi Azure AD](https://github.com/Azure/azure-docs-powershell-azuread/blob/master/Azure%20AD%20Cmdlets/AzureAD/index.md). Po jest zainstalowany, należy użyć kroki, które należy wykonać, aby skonfigurować każde pole.

Aby szybko zainstalować z obsługujących instalację modułu nowe wersje programu PowerShell, uruchom następujące polecenia. (Pierwszy wiersz sprawdza, czy moduł jest już zainstalowany.)

```
Get-Module AzureADPreview
Install-Module AzureADPreview
Connect-AzureAD
```

#### <a name="set-the-authentication-data-with-powershell-version-2"></a>Ustawia dane uwierzytelniania przy użyciu programu PowerShell w wersji 2

```
Connect-AzureAD

Set-AzureADUser -ObjectId user@domain.com -OtherMails @("email@domain.com")
Set-AzureADUser -ObjectId user@domain.com -Mobile "+1 2345678901"
Set-AzureADUser -ObjectId user@domain.com -TelephoneNumber "+1 1234567890"

Set-AzureADUser -ObjectId user@domain.com -OtherMails @("emails@domain.com") -Mobile "+1 1234567890" -TelephoneNumber "+1 1234567890"
```

#### <a name="read-the-authentication-data-with-powershell-version-2"></a>Odczytanie danych uwierzytelniania przy użyciu programu PowerShell w wersji 2

```
Connect-AzureAD

Get-AzureADUser -ObjectID user@domain.com | select otherMails
Get-AzureADUser -ObjectID user@domain.com | select Mobile
Get-AzureADUser -ObjectID user@domain.com | select TelephoneNumber

Get-AzureADUser | select DisplayName,UserPrincipalName,otherMails,Mobile,TelephoneNumber | Format-Table
```

## <a name="next-steps"></a>Następne kroki

* [Jak wykonać pomyślne wdrożenie funkcji samoobsługowego resetowania haseł?](active-directory-passwords-best-practices.md)
* [Resetowanie lub zmienianie hasła](active-directory-passwords-update-your-own-password.md)
* [Rejestrowanie na potrzeby samoobsługowego resetowania haseł](active-directory-passwords-reset-register.md)
* [Masz pytanie licencjonowania?](active-directory-passwords-licensing.md)
* [Jakie metody uwierzytelniania są dostępne dla użytkowników?](active-directory-passwords-how-it-works.md#authentication-methods)
* [Jakie są opcje zasad dla funkcji samoobsługowego resetowania haseł?](active-directory-passwords-policy.md)
* [Co to jest funkcja zapisywania zwrotnego haseł i dlaczego jest ona tak ważna?](active-directory-passwords-writeback.md)
* [Jak zgłosić działanie funkcji samoobsługowego resetowania haseł?](active-directory-passwords-reporting.md)
* [Jakie są dostępne opcje funkcji samoobsługowego resetowania haseł i do czego one służą?](active-directory-passwords-how-it-works.md)
* [Myślę, że coś działa niewłaściwie. Jak rozwiązywać problemy z funkcją samoobsługowego resetowania haseł?](active-directory-passwords-troubleshoot.md)
* [Mam pytanie, na które nie mogę znaleźć odpowiedzi](active-directory-passwords-faq.md)
