---
title: "Azure AD Connect: Bezproblemowe logowanie jednokrotne - GDPR zgodności | Dokumentacja firmy Microsoft"
description: "Ten artykuł dotyczy bezproblemowe logowania jednokrotnego usługi Azure Active Directory (Azure AD) i GDPR zgodności."
services: active-directory
keywords: "Co to jest usługa Azure AD Connect, GDPR, wymaganych składników dla usługi Azure AD, SSO, Single Sign-on"
documentationcenter: 
author: swkrish
manager: mtillman
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/12/2018
ms.author: billmath
ms.openlocfilehash: 0c7ed376accb1eed01106358491e925d3b8126c5
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/16/2018
---
# <a name="azure-ad-seamless-single-sign-on-gdpr-compliance"></a>Azure AD bezproblemowe logowanie jednokrotne: zgodności GDPR

## <a name="overview"></a>Przegląd

W 2018 maja, prawa Europejskiego prywatności, [rozporządzenia ogólne ochrony danych (GDPR)](http://ec.europa.eu/justice/data-protection/reform/index_en.htm), przypada zaczęły obowiązywać. GDPR nakłada nowe zasady dotyczące firmy, agencji rządowych z systemem innym niż zysków i innymi organizacjami, że oferta towarów i usług do osób w Unii Europejskiej (UE), lub że zbieranie i analizowanie danych powiązane mieszkańców Unii Europejskiej. Stosuje GDPR niezależnie od tego, w którym znajduje się. 

Usług i produktów firmy Microsoft dostępnych dzisiaj ułatwić spełniają wymagania GDPR. Dowiedz się więcej o zasady Privacy firmy Microsoft pod adresem [Centrum zaufania](https://www.microsoft.com/trustcenter).

Azure AD SSO bezproblemowe tworzy następujący typ dziennika, który może zawierać EUII:

- Pliki dziennika śledzenia usługi Azure AD Connect.

Zgodność GDPR bezproblemowe rejestracji Jednokrotnej jest osiągalna na dwa sposoby:

1.  Na żądanie wyodrębnić dane dla osoby i usuwanie danych z tej osoby z instalacji.
2.  Upewnij się, że żadne dane nie są przechowywane poza 48 godzin.

Druga opcja zdecydowanie zaleca się, ponieważ możliwe jest łatwiejsze do wdrożenia i konserwacji. Zobacz, postępując zgodnie z instrukcjami dla każdego typu dziennika:

### <a name="delete-azure-ad-connect-trace-log-files"></a>Usuń pliki dziennika śledzenia Azure AD Connect

Sprawdź zawartość **%ProgramData%\AADConnect** folder i Usuń śledzenia dziennik zawartość (**śledzenia -\*log** pliki) tego folderu w ciągu 48 godzin Instalowanie lub uaktualnianie Azure AD Connect lub modyfikowanie konfiguracji bezproblemowe logowanie Jednokrotne, jak ta akcja może utworzyć danych objętych GDPR.

>[!IMPORTANT]
>Nie należy usuwać **PersistedState.xml** pliku w tym folderze, ponieważ ten plik jest używany do zarządzania stanem poprzedniej instalacji programu Azure AD Connect i jest używany po zakończeniu instalacji uaktualnienia. Ten plik nigdy nie będzie zawierać wszystkie dane dotyczące osoby i nigdy nie powinien zostać usunięty.

Możesz przejrzeć i usunąć te pliki dziennika śledzenia przy użyciu Eksploratora Windows, lub można wykonać niezbędne czynności następujący skrypt programu PowerShell:

```
$Files = ((Get-Item -Path "$env:programdata\aadconnect\trace-*.log").VersionInfo).FileName 
 
Foreach ($file in $Files) { 
    {Remove-Item -Path $File -Force} 
}
```

Zapisz skrypt w pliku z ". PS1 "rozszerzenia. Uruchom ten skrypt, zgodnie z potrzebami.

Aby dowiedzieć się więcej o związane z usługi Azure AD Connect GDPR wymagania, zobacz [w tym artykule](active-directory-aadconnect-gdpr.md).

### <a name="note-about-domain-controller-logs"></a>Należy pamiętać o dziennikach kontrolera domeny

Jeśli włączono rejestrowanie inspekcji, ten produkt może generować dzienniki zabezpieczeń dla kontrolerów domeny. Aby dowiedzieć się więcej o konfigurowaniu zasad inspekcji, przeczytaj to [artykułu](https://technet.microsoft.com/library/dd277403.aspx).

## <a name="next-steps"></a>Kolejne kroki

- [**Rozwiązywanie problemów z** ](active-directory-aadconnect-troubleshoot-sso.md) — Dowiedz się, jak rozwiązać typowe problemy z funkcją.
- [**UserVoice** ](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) — w przypadku zgłoszenia żądania nowych funkcji.
