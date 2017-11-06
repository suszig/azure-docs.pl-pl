---
title: "Samoobsługi lub wirusa rejestracji w usłudze Azure Active Directory | Dokumentacja firmy Microsoft"
description: "Użyj subskrypcji samoobsługowej w dzierżawie usługi Azure Active Directory (Azure AD)"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: b9f01876-29d1-4ab8-8b74-04d43d532f4b
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/03/2017
ms.author: curtand
ms.reviewer: elkuzmen
ms.custom: it-pro
ms.openlocfilehash: 762627a1d743922d680d90d1065ae80fbd3f4420
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/04/2017
---
# <a name="what-is-self-service-signup-for-azure-active-directory"></a>Co to jest subskrypcji samoobsługowej usługi Azure Active Directory?
W tym artykule opisano subskrypcji samoobsługowej oraz do jego obsługi w usłudze Azure Active Directory (Azure AD). Jeśli chcesz przejąć nazwy domeny z usługi Azure AD niezarządzanej dzierżawy, zobacz [przejąć niezarządzane katalogu jako administrator](domains-admin-takeover.md).

## <a name="why-use-self-service-signup"></a>Dlaczego warto używać subskrypcji samoobsługowej?
* Uzyskanie dostępu do usług, które mają szybszy klientów
* Tworzenie opartych na poczcie e-mail oferty usługi
* Tworzenie przepływów signup opartych na poczcie e-mail, umożliwiające szybkie tworzenie tożsamości za pomocą ich aliasów e-mail łatwych do zapamiętania pracy
* Niezależne-eksploatacyjnych utworzonych katalog usługi Azure AD mogą być uwzględniane w zarządzanych katalogu, który może służyć do innych usług

## <a name="terms-and-definitions"></a>— Warunki
* **Subskrypcji samoobsługowej**: jest to metoda, za pomocą którego użytkownik tworzy konto w usłudze w chmurze i ma tożsamość automatycznie utworzone dla nich w usłudze Azure AD na podstawie ich domenę poczty e-mail.
* **Katalog usługi Azure AD niezarządzanych**: jest to katalog, w której jest tworzony tej tożsamości. Niezarządzane katalog jest katalogiem, który nie ma administratora globalnego.
* **Zweryfikować adres e-mail użytkownika**: jest to typ konta użytkownika w usłudze Azure AD. Użytkownik, który ma tożsamość automatycznie utworzone po zarejestrowaniu się do oferty samoobsługi nazywa się zweryfikować adres e-mail użytkownika. Zweryfikować adres e-mail użytkownika jest członkiem regularne katalogu z creationmethod = EmailVerified.

## <a name="how-do-i-control-self-service-settings"></a>Jak kontrolować ustawienia samoobsługowego?
Administratorzy mają dwa formanty samoobsługi dzisiaj. Kontrolować, czy:

* Użytkownicy mogą dołączać katalogu za pośrednictwem poczty e-mail.
* Użytkownicy mogą się licencji dla aplikacji i usług.

### <a name="how-can-i-control-these-capabilities"></a>Metody kontrolowania tych funkcji
Administrator można skonfigurować te funkcje przy użyciu następujących parametrów polecenia cmdlet Set-MsolCompanySettings usługi Azure AD:

* **AllowEmailVerifiedUsers** Określa, czy użytkownik może utworzyć czy Dołącz katalog niezarządzane. Jeśli ten parametr jest ustawiony na $false, użytkownicy nie zweryfikować poczty e-mail może dołączać katalogu.
* **AllowAdHocSubscriptions** kontroluje zdolność użytkownikom wykonywanie subskrypcji samoobsługowej. Jeśli ten parametr jest ustawiony na $false, użytkownicy nie mogą wykonywać subskrypcji samoobsługowej.

### <a name="how-do-the-controls-work-together"></a>Jak formanty działają razem?
Tych parametrów można w połączeniu do definiowania dokładniejszej kontroli nad subskrypcji samoobsługowej. Na przykład następujące polecenie umożliwi użytkownikom wykonywanie subskrypcji samoobsługowej, ale tylko wtedy, jeśli ci użytkownicy mają już konto w usłudze Azure AD (innymi słowy, użytkownicy potrzebują zweryfikować adres e-mail konta należy utworzyć najpierw nie można wykonać subskrypcji samoobsługowej):

````
    Set-MsolCompanySettings -AllowEmailVerifiedUsers $false -AllowAdHocSubscriptions $true
````
Poniższy schemat wyjaśniono różnych kombinacji tych parametrów i wynikowy warunki katalogu i subskrypcji samoobsługowej.

![][1]

Aby uzyskać dodatkowe informacje i przykłady dotyczące używania tych parametrów, zobacz [Set-MsolCompanySettings](/powershell/module/msonline/set-msolcompanysettings?view=azureadps-1.0).

## <a name="next-steps"></a>Następne kroki
* [Dodawanie niestandardowej nazwy domeny do usługi Azure AD](add-custom-domain.md)
* [Instalowanie i konfigurowanie programu Azure PowerShell](/powershell/azure/overview)
* [Azure PowerShell](/powershell/azure/overview)
* [Dokumentacja poleceń cmdlet platformy Azure](/powershell/azure/get-started-azureps)
* [Set-MsolCompanySettings](/powershell/module/msonline/set-msolcompanysettings?view=azureadps-1.0)

<!--Image references-->
[1]: ./media/active-directory-self-service-signup/SelfServiceSignUpControls.png
