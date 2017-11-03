---
title: "Czym jest rejestracja samoobsługowa na platformie Azure? | Microsoft Docs"
description: "Omówienie subskrypcji samoobsługowej platformy Azure, jak zarządzać procesu zakładania i jak przejąć nazwy domeny DNS."
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
ms.date: 08/28/2017
ms.author: curtand
ms.reviewer: elkuzmen
ms.custom: it-pro
ms.openlocfilehash: 511088156d3546e2e0f3ac40e72bf2b8e4ae2cb9
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="what-is-self-service-signup-for-azure"></a>Czym jest rejestracja samoobsługowa na platformie Azure?
W tym temacie opisano proces subskrypcji samoobsługowej oraz sposób przejęcia nazwy domeny DNS.  

## <a name="why-use-self-service-signup"></a>Dlaczego warto używać subskrypcji samoobsługowej?
* Pobierz klientów do usług, które mają szybszy.
* Utwórz opartych na poczcie e-mail oferty usługi.
* Tworzenie przepływów opartych na poczcie e-mail signup szybko Zezwalaj użytkownikom na tworzenie tożsamości za pomocą ich aliasów e-mail pracy łatwych do zapamiętania.
* Niezarządzane katalogów platformy Azure można uwzględnić w zarządzanych katalogi później i można użyć ponownie dla innych usług.

## <a name="terms-and-definitions"></a>— Warunki
* **Portal rejestracji**: jest to metoda, za pomocą którego użytkownik tworzy konto w usłudze w chmurze i ma tożsamość automatycznie utworzone dla nich w usłudze Azure Active Directory (Azure AD), na podstawie ich domenę poczty e-mail.
* **Niezarządzane katalogu Azure**: jest to katalog, w której jest tworzony tej tożsamości. Niezarządzane katalog jest katalogiem, który nie ma administratora globalnego.
* **Zweryfikować adres e-mail użytkownika**: jest to typ konta użytkownika w usłudze Azure AD. Użytkownik, który ma tożsamość automatycznie utworzone po zarejestrowaniu się do oferty samoobsługi nazywa się zweryfikować adres e-mail użytkownika. Zweryfikować adres e-mail użytkownika jest członkiem regularne katalogu z creationmethod = EmailVerified.

## <a name="user-experience"></a>Środowisko użytkownika
Na przykład, załóżmy, że użytkownik, którego poczta e-mail jest Dan@BellowsCollege.com odbiera poufnych plików za pośrednictwem poczty e-mail. Pliki są chronione przez usługę Azure Rights Management (Azure RMS). Jednak organizacji Dan firmy, uczelni miech nie została podpisana dla usługi Azure RMS ani nie została wdrożona Active Directory RMS. W takim przypadku Dan można założyć bezpłatnej subskrypcji usługi RMS dla użytkowników indywidualnych w celu odczytywać pliki chronione.

Jeśli Dan jest pierwszy użytkownik za pomocą adresu e-mail z BellowsCollege.com w celu uzyskania tego samoobsługi oferty, niezarządzany katalogu zostanie utworzony dla BellowsCollege.com w usłudze Azure AD. Jeśli innym użytkownikom z domeny BellowsCollege.com Załóż tej oferty lub podobne oferty samoobsługi, będą miały również zweryfikować adres e-mail konta utworzone w tym samym katalogu niezarządzanego na platformie Azure.

## <a name="admin-experience"></a>Środowisko pracy administratora
Administrator, który jest właścicielem nazwy domeny DNS niezarządzane katalogu Azure może przejąć lub scalania katalogu po potwierdzania własności. W kolejnych sekcjach wyjaśniono środowisko pracy administratora bardziej szczegółowo, ale w tym miejscu znajduje się podsumowanie:

* Po przejęciu niezarządzane usługi Azure directory stają się po prostu administratora globalnego katalogu niezarządzane. Jest to czasem nazywane przejęcia wewnętrznego.
* Podczas scalania niezarządzane katalogu platformy Azure, dodać nazwę domeny DNS katalogu niezarządzanych do zarządzanych katalogu Azure i mapowanie użytkowników do zasobów zostanie utworzony tak użytkownicy mogą nadal dostępu do usług bez przeszkód. Jest to czasem nazywane przejęcia zewnętrznych.

## <a name="what-gets-created-in-azure-active-directory"></a>Pobiera co utworzone w usłudze Azure Active Directory?
#### <a name="directory"></a>Katalog
* Katalog usługi Azure Active Directory dla domeny utworzeniu jednego katalogu dla domeny.
* Katalog usługi Azure AD nie ma żadnych administratora globalnego.

#### <a name="users"></a>Użytkownicy
* Dla każdego użytkownika, która zarejestruje się tworzony jest obiekt użytkownika w katalogu usługi Azure AD.
* Każdy obiekt użytkownika jest oznaczony jako zewnętrzny.
* Każdy użytkownik otrzyma dostęp do usługi one podpisane w usłudze.

### <a name="how-do-i-claim-a-self-service-azure-ad-directory-for-a-domain-i-own"></a>Jak oświadczenia samoobsługi usługi Azure AD katalogu dla domeny własnym?
Samoobsługowe usługi Azure AD mogą rezerwować katalogu, wykonując weryfikacji domeny. Domeny weryfikacji okaże się, że jesteś właścicielem domeny przez utworzenie rekordów DNS.

Istnieją dwa sposoby przejęciem DNS katalogu usługi Azure AD:

* wewnętrzny przejęcia (Admin odnajduje niezarządzane katalogu platformy Azure i chce włączyć do zarządzanego katalogu)
* zewnętrzne przejęcia (Administrator próbuje dodać nową domenę do ich zarządzanych katalogu platformy Azure)

Może Cię zainteresować sprawdzanie poprawności właścicielem domeny, ponieważ przejęcia niezarządzane katalogu po użytkownik wykona subskrypcji samoobsługowej lub może być Dodawanie nowej domeny do istniejącego katalogu zarządzanych. Na przykład masz domenę o nazwie contoso.com i chcesz dodać nową domenę o nazwie contoso.co.uk lub contoso.uk.

## <a name="what-is-domain-takeover"></a>Co to jest przejęcia domeny?
W tej sekcji opisano sposób sprawdzić, czy jesteś właścicielem domeny

### <a name="what-is-domain-validation-and-why-is-it-used"></a>Co to jest weryfikacji domeny i dlaczego służy?
W celu wykonywania operacji w katalogu, wymaga usługi Azure AD zweryfikowanie prawa własności do domeny DNS.  Sprawdzanie poprawności domeny umożliwia oświadczeń katalogu, a następnie Podwyższ poziom samoobsługi katalogu z katalogiem zarządzanych lub scalanie katalogu samoobsługi w istniejącym katalogiem zarządzanych.

## <a name="examples-of-domain-validation"></a>Przykłady weryfikacji domeny
Istnieją dwa sposoby przejęciem DNS katalogu:

* Przejmowanie wewnętrzne (na przykład administrator odnajduje samoobsługi katalogu niezarządzane i chce włączyć do katalogu zarządzane)
* przejęcia zewnętrznych (na przykład administrator próbuje dodać nową domenę do katalogu zarządzane)

### <a name="internal-takeover---promote-a-self-service-unmanaged-directory-to-be-a-managed-directory"></a>Wewnętrzny przejęcia - wspierania samoobsługi, niezarządzany katalogu zarządzanych katalogiem
Podczas wykonywania wewnętrznego przejęcia katalogu zostaje przekształcona z katalogu niezarządzanych do zarządzanych katalogu. Należy wykonać sprawdzanie nazwy domeny DNS, gdzie utworzyć rekord MX lub rekord TXT w strefie DNS. Ta akcja:

* Sprawdza, czy jesteś właścicielem domeny
* Sprawia, że katalog zarządzane
* Sprawia, że użytkownik Administrator globalny katalogu

Załóżmy, że administrator IT z uczelni miech wykrywa, że użytkownicy z szkoły utworzyli konto samoobsługi oferty. Zarejestrowany właściciel DNS nazw BellowsCollege.com, IT administrator może zweryfikować prawo własności nazwy DNS na platformie Azure i wykonaj za pośrednictwem katalogu niezarządzane. Katalog staje się zarządzany katalog, a IT administrator jest przypisany do roli administratora globalnego katalogu BellowsCollege.com.

### <a name="external-takeover---merge-a-self-service-directory-into-an-existing-managed-directory"></a>Zewnętrzne przejęcia - scalić samoobsługi katalogu do istniejącego katalogu zarządzanych
W zewnętrznej przejęcia masz już zarządzany katalog i chcesz dołączyć do tego katalogu zarządzanych przez wszystkich użytkowników i grup z katalogu niezarządzane, a nie własnych dwa oddzielne katalogów.

Jako administrator katalogu zarządzanych Dodawanie domeny, a tej domeny stanie się mieć katalog usługi niezarządzane skojarzonych z nim.

Na przykład załóżmy, że jesteś administratorem IT i już zarządzane katalogu dla domeny Contoso.com, nazwy domeny nie jest zarejestrowana dla Twojej organizacji. Użytkownik stwierdzi, że użytkownicy z organizacji wykonali samoobsługi logowania się oferty przy użyciu nazwy domeny poczty e-mail user@contoso.co.uk, która jest inną nazwę domeny, która własnością Twojej organizacji. Te użytkownicy mają obecnie konta w katalogu contoso.co.uk niezarządzane.

Nie chcesz zarządzać dwa oddzielne katalogi, tak scalania katalogiem niezarządzane contoso.co.uk z istniejącym katalogiem IT zarządzane dla domeny contoso.com.

Zewnętrzne przejęcia rozpoczyna ten sam proces weryfikacji DNS jako wewnętrzne przejęcia.  Czym różnicę: użytkowników i usług są mapowane ponownie do katalogu IT zarządzane.

#### <a name="whats-the-impact-of-performing-an-external-takeover"></a>Jaki jest wpływ wykonania zewnętrznego przejęcia?
Z zewnętrznego przejęcia mapowanie użytkowników do zasobów jest tworzone użytkownicy mogą nadal uzyskiwać dostęp do usług bez przeszkód. Wiele aplikacji, w tym usługi RMS dla użytkowników indywidualnych, również obsługiwać mapowanie użytkowników do zasobów, a użytkownicy mogą nadal uzyskiwać dostęp do tych usług bez zmian. Aplikacja nie obsługuje skutecznie mapowanie użytkowników do zasobów, zewnętrznych przejęcia mogą jawnie zablokowane aby uniemożliwić użytkownikom niską środowisko.

#### <a name="directory-takeover-support-by-service"></a>Obsługa przejęcia Directory przez usługę
Przejęcia obsługuje obecnie następujące usługi:

* USŁUG RMS

Przejęcia wkrótce będzie obsługiwana następujące usługi:

* Usługa PowerBI

Następujące nie i wymagają admin dodatkowe działania w celu migracji danych użytkownika po przejęciu zewnętrznych.

* SharePoint/OneDrive

## <a name="how-to-perform-a-dns-domain-name-takeover"></a>Jak przeprowadzić przejęciem nazwy domeny DNS
Masz kilka opcji dotyczących sposobu sprawdzania poprawności domeny (i wykonaj przejęcia, w razie potrzeby):

1. Portal zarządzania systemu Azure

   Przejęciem jest wyzwalane, wykonując dodawania domeny.  Jeśli katalog już istnieje w domenie, będziesz mieć możliwość przejęcia zewnętrznych.

   Zaloguj się do portalu Azure przy użyciu poświadczeń.  Przejdź do istniejącego katalogu, a następnie do **Dodaj domenę**.
2. Office 365

   Możesz użyć opcji w [Zarządzanie domenami](https://support.office.com/article/Navigate-to-the-Office-365-Manage-domains-page-026af1f2-0e6d-4f2d-9b33-fd147420fac2/) strony w usłudze Office 365, aby pracować z domen i rekordów DNS. Zobacz [Sprawdź domenę, w usłudze Office 365](https://support.office.com/article/Verify-your-domain-in-Office-365-6383f56d-3d09-4dcb-9b41-b5f5a5efd611/).
3. Windows PowerShell

   Poniższe kroki są wymagane do przeprowadzenia weryfikacji przy użyciu programu Windows PowerShell.

   | Krok | Polecenia cmdlet do użycia |
   | --- | --- |
   | Utwórz obiekt poświadczeń |Get-Credential |
   | Łączenie z usługą Azure AD |Connect-MsolService |
   | Pobierz listę domen |Get-MsolDomain |
   | Utwórz żądanie |Get-MsolDomainVerificationDns |
   | Utwórz rekord DNS |To zrobić na serwerze DNS |
   | Sprawdź żądania |Confirm-MsolEmailVerifiedDomain |

Na przykład:

1. Połączenie z usługą Azure AD za pomocą poświadczeń użytych do odpowiedzi z ofertą samoobsługi:

        import-module MSOnline
        $msolcred = get-credential
        connect-msolservice -credential $msolcred
2. Pobierz listę domen:

    Get-MsolDomain
3. Następnie uruchom polecenie cmdlet Get-MsolDomainVerificationDns, aby utworzyć żądanie:

    Get-MsolDomainVerificationDns — DomainName *your_domain_name* — tryb DnsTxtRecord

    Na przykład:

    Get-MsolDomainVerificationDns — DomainName contoso.com — tryb DnsTxtRecord
4. Skopiuj wartość (żądanie), która jest zwracana z tego polecenia.

    Na przykład:

    MS = 32DD01B82C05D27151EA9AE93C5890787F0E65D9
5. W sieci publicznej przestrzeni nazw DNS należy utworzyć rekord txt DNS, zawierający wartość skopiowanego w poprzednim kroku.

    Nazwa dla tego rekordu jest nazwa domeny nadrzędnej, więc jeśli utworzysz ten rekord zasobu za pomocą roli DNS z systemem Windows Server, pozostaw Wklej puste, a tylko nazwy rekordu wartość w polu tekstowym
6. Uruchom polecenie cmdlet Potwierdź MsolDomain można zweryfikować żądania:

    Potwierdź MsolEmailVerifiedDomain - DomainName *your_domain_name*

    Na przykład:

    Potwierdź MsolEmailVerifiedDomain - DomainName contoso.com

Żądanie powiodło się zwraca do wiersza polecenia bez błędów.

## <a name="how-do-i-control-self-service-settings"></a>Jak kontrolować ustawienia samoobsługowego?
Administratorzy mają dwa formanty samoobsługi dzisiaj. Kontrolować:

* Określa, czy użytkownicy mogą dołączać katalogu za pośrednictwem poczty e-mail.
* Określa, czy użytkownicy mogą licencji się dla aplikacji i usług.

### <a name="how-can-i-control-these-capabilities"></a>Metody kontrolowania tych funkcji
Administrator można skonfigurować te funkcje przy użyciu tych parametrów polecenia cmdlet Set-MsolCompanySettings usługi Azure AD:

* **AllowEmailVerifiedUsers** Określa, czy użytkownik może utworzyć czy Dołącz katalog niezarządzane. Jeśli ten parametr jest ustawiony na $false, użytkownicy nie zweryfikować poczty e-mail może dołączać katalogu.
* **AllowAdHocSubscriptions** kontroluje zdolność użytkownikom wykonywanie samoobsługowego logowania się. Jeśli ten parametr jest ustawiony na $false, użytkownicy nie mogą wykonywać subskrypcji samoobsługowej.

### <a name="how-do-the-controls-work-together"></a>Jak formanty działają razem?
Tych parametrów można w połączeniu do definiowania dokładniejszej kontroli nad samoobsługi logowania się. Na przykład następujące polecenie umożliwi użytkownikom wykonywanie samoobsługowego, ale tylko wtedy, jeśli ci użytkownicy mają już konto w usłudze Azure AD (innymi słowy, użytkownicy potrzebują zweryfikować adres e-mail konta do utworzenia nie można wykonać samoobsługi logowania się):

    Set-MsolCompanySettings -AllowEmailVerifiedUsers $false -AllowAdHocSubscriptions $true

Poniższy schemat wyjaśniono wszystkich różnych kombinacji tych parametrów i wynikowy warunki katalogu i samoobsługi logowania się.

![][1]

Aby uzyskać dodatkowe informacje i przykłady dotyczące używania tych parametrów, zobacz [Set-MsolCompanySettings](/powershell/module/msonline/set-msolcompanysettings?view=azureadps-1.0).

## <a name="see-also"></a>Zobacz też
* [Instalowanie i konfigurowanie programu Azure PowerShell](/powershell/azure/overview)
* [Azure PowerShell](/powershell/azure/overview)
* [Dokumentacja poleceń cmdlet platformy Azure](/powershell/azure/get-started-azureps)
* [Set-MsolCompanySettings](/powershell/module/msonline/set-msolcompanysettings?view=azureadps-1.0)

<!--Image references-->
[1]: ./media/active-directory-self-service-signup/SelfServiceSignUpControls.png
