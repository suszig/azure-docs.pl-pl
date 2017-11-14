---
title: "Konfigurowanie logowania automatycznego przyspieszenie dla aplikacji za pomocą Narzędzia główne zasad odnajdywania obszaru | Dokumentacja firmy Microsoft"
description: "Wyjaśnienia dotyczące pojęcia „dzierżawa usługi Azure AD” i zarządzania nią za pomocą usługi Azure Active Directory"
services: active-directory
documentationcenter: 
author: billmath
manager: femila
ms.service: active-directory
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: it-pro
ms.date: 11/09/2017
ms.author: billmath
ms.openlocfilehash: b1f0e5da09ef1d6acd234b72878cc71d66bb551e
ms.sourcegitcommit: 659cc0ace5d3b996e7e8608cfa4991dcac3ea129
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/13/2017
---
# <a name="configure-sign-in-auto-acceleration-for-an-application-using-home-realm-discovery-hrd-policy"></a>Konfigurowanie logowania automatycznego przyspieszenie dla aplikacji za pomocą obszaru macierzystego zasad odnajdowania (obszaru macierzystego HRD)

## <a name="introduction-to-home-realm-discovery-and-auto-acceleration"></a>Wprowadzenie do odnajdowania obszaru macierzystego i automatycznie przyspieszenia
Ten dokument zawiera wprowadzenie do odnajdowania obszaru macierzystego i przyspieszenie automatycznie.

### <a name="home-realm-discovery"></a>Odnajdowanie obszaru macierzystego
Odnajdowanie obszaru macierzystego to proces, który umożliwia usłudze Azure Active Directory określić, w czasie logowania, gdy użytkownik musi uwierzytelnić.  Podczas logowania się do dzierżawy usługi Azure AD, uzyskać dostęp do zasobów lub usługi Azure AD wspólnej strony logowania, użytkownik wpisuje nazwę użytkownika (UPN).  Usługi Azure AD użyty do odnajdywania, gdy użytkownik musi się zalogować.   Użytkownik może być konieczne jest uważana za jeden z następujących uwierzytelniania:

- Dzierżawca domowej użytkownika (może to być tej samej dzierżawy zgodnie z zasobów użytkownik próbuje uzyskać dostęp do). 
- Konta Microsoft.   Użytkownik jest gościa w dzierżawie zasobów
- Inny dostawca tożsamości federacyjnych z dzierżawy usługi Azure AD.  Dostawca tożsamości lokalnych, takich jak usługi AD FS dla wystąpienia.

### <a name="auto-acceleration"></a>Akceleracja automatycznie 
Niektóre organizacje skonfigurować ich dzierżawy usługi Azure Active Directory możliwości utworzenia federacji z innego dostawców tożsamości, takie jak usługi AD FS do uwierzytelniania użytkowników.  W takich przypadkach przy logowaniu się do aplikacji użytkownik najpierw zobaczy strony logowania usługi Azure AD i po wpisywania ich nazwy UPN są następnie zostają przeniesieni do strony logowania IdP.  W sytuacjach, w których warto Administratorzy mogą chcieć skierowany bezpośrednio do strony logowania, po zalogowaniu się do określonych aplikacji, użytkowników, pomijanie stronę początkową usługi Azure Active Directory. Jest to określane jako "Logowanie automatyczne przyspieszenie".

W przypadkach, gdy dzierżawcy jest Sfederowane do innego dostawców tożsamości dla logowania Włączanie automatycznego przyspieszenie sprawia, że użytkownika logowania więcej uproszczone w przypadkach, w którym wiadomo, że wszyscy logowania mogą być uwierzytelniane za tym IdP.  Możesz skonfigurować automatyczne przyspieszenie dla poszczególnych aplikacji.

>[!NOTE]
>Jeśli skonfigurujesz aplikację do przyspieszania automatycznie gości nie można się zalogować. Biorąc użytkownika bezpośrednio do federacyjnych dostawców tożsamości dla uwierzytelniania jest jednokierunkowa ulicę, ponieważ nie istnieje sposób, aby wrócić do strony logowania usługi Azure Active Directory.  Gość użytkowników, którzy mogą muszą być kierowane do innych dzierżawców lub zewnętrznych dostawców tożsamości, takich jak konta Microsoft uwierzytelnienia, nie będzie można zalogować się do tej aplikacji, jak krok odnajdowania obszaru macierzystego zostanie pominięte.  

Istnieją dwa sposoby kontrolowania przyspieszenie automatycznie do IdP federacyjnych.  Albo:   

- Użyj wskazówkę domeny żądań uwierzytelniania dla aplikacji 
- Konfigurowanie zasad HomeRealmDiscovery, aby włączyć przyspieszenie automatycznie

## <a name="domain-hints"></a>Wskazówki dotyczące domeny 
Wskazówki domeny są dyrektywy częścią żądania uwierzytelniania z aplikacji.  Może służyć do przyspieszenia użytkownika do ich federacyjnych IdP strony logowania lub może być używane przez aplikacji wielodostępne w celu przyspieszenia użytkownika bezpośrednio do marką usługi Azure AD strony logowania dla swojej dzierżawy.  Na przykład largeapp.com aplikacji może umożliwić klientom dostęp do aplikacji w niestandardowych contoso.largeapp.com adresu URL i może zawierać wskazówkę domeny Contoso.com w żądaniu uwierzytelnienia. Składnia wskazówka domeny różni się w zależności od używanego protokołu i zazwyczaj są konfigurowane w aplikacji.

**WS-Federation**: whr=contoso.com w ciągu zapytania

**SAML**: albo SAML uwierzytelniania żądanie zawierające wskazówki domeny lub whr=contoso.com ciąg zapytania

**Otwórz ID Connect**: domain_hint=contoso.com ciąg zapytania 

Jeśli dzierżawa jest Sfederowane przy użyciu tej domeny wskazówkę dotyczącą domeny jest uwzględniona w żądania uwierzytelniania z aplikacji, usługi Azure AD próbuje przekierować logowania do IdP skonfigurowane dla tej domeny.  Wskazówka domeny nie odwołuje się do zweryfikowanej domeny federacyjnej, zostanie zignorowany, a odnajdowania obszaru macierzystego normalne jest wywoływany.

Zobacz to [wpis w blogu](https://cloudblogs.microsoft.com/enterprisemobility/2015/02/11/using-azure-ad-to-land-users-on-their-custom-login-page-from-within-your-app/) uzyskać więcej informacji na temat automatycznego przyspieszenie przy użyciu wskazówek domeny obsługiwane przez usługę Azure Active Directory.

>[!NOTE]
>Jeśli wskazówkę dotyczącą domeny jest uwzględniona w żądanie uwierzytelnienia jego obecność zastępuje dowolne zasady Wyświetlają ustawionej dla aplikacji.

## <a name="home-realm-discovery-hrd-policy"></a>Zasady odnajdowania (obszaru macierzystego HRD) obszaru macierzystego
Niektóre aplikacje nie umożliwiają konfigurowanie, które wysyłają żądania uwierzytelnienia, a w tych przypadkach nie jest możliwe przy użyciu wskazówek domeny kontrolować przyspieszenie automatycznie.   Przyspieszenie automatycznie można skonfigurować za pomocą zasad, aby uzyskać takie samo zachowanie.  

### <a name="setting-hrd-policy"></a>Ustawienie zasad HRD
Istnieją trzy kroki w celu ustawienia logowania automatycznego przyspieszenie aplikacji


1. Tworzenie zasad odnajdowanie obszaru macierzystego dla automatycznego przyspieszenia
2. Lokalizowanie zasady usług, do którego ma zostać dołączony zasad
3. Dołączanie zasady zasady usługi.  Zasady zostały utworzone w dzierżawie, ale nie ma żadnego efektu, dopóki nie są one dołączone do jednostki.  Odnajdowanie obszaru macierzystego zasad można dołączyć do nazwy głównej usługi i tylko jedne zasady HRD mogą być aktywne w danej jednostce w dowolnym momencie.  

Microsoft Azure Active Directory interfejsu API programu Graph bezpośrednio lub Azure Active Directory polecenia cmdlet programu PowerShell można użyć do skonfigurowania przyspieszenie automatycznie za pomocą zasad HRD

Interfejsu API programu Graph manipuluje zasad jest opisane [tutaj](https://msdn.microsoft.com/library/azure/ad/graph/api/policy-operations).

Oto przykład definicji zasad HRD:
    
 ```
   {  
    "HomeRealmDiscoveryPolicy":
    {  
    "AccelerateToFederatedDomain":true,
    "PreferredDomain":"federated.example.edu"
    }
   }
```

Typ zasad jest "HomeRealmDiscoveryPolicy".

Jeśli **AccelerateToFederatedDomain** ma wartość false, a następnie zasada nie ma znaczenia **PreferredDomain** powinny wskazywać, w celu przyspieszenia do domeny i można je pominąć w przypadku, gdy dzierżawa ma jeden i tylko jeden federacyjnych domeny.  Jeśli zostanie pominięty i ma więcej niż jeden zweryfikować domeny federacyjnej zasad nie ma znaczenia.

Jeśli **PreferredDomain** jest określona, musi on być zgodny domeny zweryfikowane, federacyjnych dla dzierżawcy i wszystkich użytkowników w aplikacji musi być w stanie się zalogować w tej domenie.

### <a name="priority-and-evaluation-of-hrd-policies"></a>Priorytet i oceny zasad HRD
Odnajdowanie obszaru macierzystego zasad można tworzyć i następnie przypisane do określonych organizacji i nazwy główne usług. Oznacza to, czy jest możliwe w dla wielu zasady do zastosowania dla określonej aplikacji. Zasady HRD, które obowiązuje obowiązują następujące reguły:


- Jeśli wskazówkę dotyczącą domeny jest obecny w żądaniu uwierzytelnienia, dowolne zasady HRD jest ignorowany i zachowanie określoną przez warunek domeny jest używany.
- W przeciwnym razie jeśli zasady jawnie jest przypisany do nazwy głównej usługi, jest wymuszone. 
- Jeśli żadne zasady nie zostały wyraźnie przypisane do nazwy głównej usługi bez wskazówki domeny nie istnieje, jest wymuszana zasady jawnie przypisane do organizacji nadrzędnej nazwy głównej usługi. 
- Jeśli nie istnieje żadne wskazówka domeny, a żadne zasady nie został przypisany do nazwy głównej usługi lub organizacji, domyślne zachowanie HRD jest używany.

## <a name="tutorial-for-setting-sign-in-auto-acceleration-on-an-application-using-hrd-policy-with-azure-active-directory-powershell-cmdlets"></a>Samouczek dotyczący ustawienia logowania automatycznego przyspieszenie aplikacji przy użyciu zasad HRD za pomocą poleceń cmdlet środowiska PowerShell usługi Azure Active Directory
Omówimy kilka scenariuszy, w tym:


- Konfigurowanie automatycznego przyspieszenie dla aplikacji dla dzierżawcy z jedną domeną federacyjnych
- Konfigurowanie automatycznego przyspieszenie aplikacji wiele domen, które są sprawdzane dzierżawy
- Wyświetlanie listy aplikacji, dla których skonfigurowano zasady

### <a name="prerequisites"></a>Wymagania wstępne
Poniższe przykłady służy do tworzenia, aktualizacji, łączenie i usuwania zasad na nazwy główne usług aplikacji w usłudze Azure AD.

1.  Aby rozpocząć, Pobierz najnowszą Podgląd polecenia Cmdlet programu PowerShell usługi Azure AD. 
2.  Po utworzeniu poleceń cmdlet programu PowerShell usługi Azure AD, uruchom polecenie Connect, aby zalogować się do usługi Azure AD przy użyciu konta administratora.

    ``` powershell
    Connect-AzureAD -Confirm
    ```
3.  Uruchom następujące polecenie, aby wyświetlić wszystkie zasady w organizacji.

    ``` powershell
    Get-AzureADPolicy
    ```

Jeśli nic nie zostanie zwrócone, nie masz żadnych zasad utworzony w dzierżawie

### <a name="example-setting-auto-acceleration-for-an-application"></a>Przykład: Ustawienie automatycznego przyspieszenie dla aplikacji 
W tym przykładzie utworzysz zasady, które automatycznie przyspiesza użytkowników do ekranu logowania usługi AD FS podczas rejestrowania się w aplikacji.  Można to zrobić bez nich konieczności najpierw wprowadź nazwę użytkownika na stronie logowania w usłudze Azure AD. 

#### <a name="step-1-create-an-hrd-policy"></a>Krok 1: Tworzenie zasad HRD
``` powershell
New-AzureADPoly -Definition @("{`"HomeRealmDiscoveryPolicy`":{`"AccelerateToFederatedDomain`":true}}") -DisplayName BasicAutoAccelerationPolicy -Type HomeRealmDiscoveryPolicy
```

Jeśli masz jednej domeny federacyjnej, który uwierzytelnia użytkowników dla aplikacji, wystarczy utworzyć HRD zasad.  
Aby zobaczyć nowe zasady i uzyskać jego ObjectID, uruchom następujące polecenie.

``` powershell
Get-AzureADPolicy
```


Po utworzeniu zasad HRD, aby włączyć przyspieszenie automatycznie umożliwi jej przypisanie do wielu zasad z usługi aplikacji.

#### <a name="step-2-locate-the-service-principal-to-assign-the-policy-to"></a>Krok 2: Znajdź nazwy głównej usługi, aby przypisać zasady.  
Należy ObjectId nazwy główne usług, które chcesz przypisać zasady. Istnieje kilka sposobów, aby znaleźć identyfikator obiektu nazwy główne usług.    

Mogą korzystać z portalu, możesz zbadać [Microsoft Graph](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity) lub przejdź do naszego [narzędzie Explorer wykres](https://graphexplorer.cloudapp.net/) logowanie na koncie usługi Azure AD, aby wyświetlić nazwy główne usług wszystkich organizacji, a ponieważ używasz programu PowerShell, można użyć polecenia cmdlet get-AzureADServicePrincipal Aby wyświetlić listę zasad usługi i ich identyfikatorów.

#### <a name="step-3-assign-the-policy-to-your-service-principal"></a>Krok 3: Przypisz zasady do Twojej nazwy głównej usługi  
Po utworzeniu ObjectId nazwy głównej usługi, aby skonfigurować automatyczne przyspieszenie dla aplikacji, uruchom następujące polecenie, aby skojarzyć zasady odnajdowanie obszaru macierzystego, który utworzonego w kroku 1 z nazwy głównej usługi, które zlokalizowanego w kroku 2.

``` powershell
Add-AzureADServicePrincipalPolicy -Id <ObjectID of the Service Principal> -RefObjectId <ObjectId of the Policy>
```

To polecenie można powtórzyć dla każdej nazwy głównej usługi chcesz dodać zasady.

#### <a name="step-4-check-which-application-service-principals-your-auto-acceleration-policy-is-assigned-to"></a>Krok 4: Sprawdź, które zasady automatycznego przyspieszenie jest przypisany do nazwy główne usług aplikacji
Aby sprawdzić, aplikacji, które mają automatycznie przyspieszenie skonfigurowanych zasad, użyj polecenia cmdlet Get-AzureADPolicyAppliedObject i przekaż go objectId zasad, które chcesz sprawdzić.

``` powershell
Get-AzureADPolicyAppliedObject -ObjectId <ObjectId of the Policy>
```
#### <a name="step-5-youre-done"></a>Krok 5: Gotowe!
Spróbuj aplikacji, aby sprawdzić, czy działa nowych zasad.

### <a name="example-listing-the-applications-for-which-an-auto-acceleration-policy-is-configured"></a>Przykład: Wyświetlanie listy aplikacji, dla których skonfigurowano zasadę automatycznego przyspieszenia

#### <a name="step-1-list-all-policies-created-in-your-organization"></a>Krok 1: Wyświetl listę wszystkich zasad utworzonych w organizacji 

``` powershell
Get-AzureADPolicy
```

Uwaga **obiektu o identyfikatorze** chcesz przydziałów listy zasad.

#### <a name="step-2-list-the-service-principals-the-policy-is-assigned-to"></a>Krok 2: Lista nazwy główne usług, które zasady jest przypisany do.  

``` powershell
Get-AzureADPolicyAppliedObject -ObjectId <ObjectId of the Policy>
```

### <a name="example-removing-an-auto-acceleration-policy-for-an-application"></a>Przykład: Usuwania zasad automatycznego przyspieszenie dla aplikacji
#### <a name="step-1-use-the-previous-example-to-get-the-objectid-of-the-policy-and-that-of-the-application-service-principal-you-wish-to-remove-it-from"></a>Krok 1: Umożliwia pobrania ObjectId zasad i które nazwy głównej usługi aplikacji, które chcesz usunąć go z poprzedniego przykładu
#### <a name="step-2-remove-the-policy-assignment-from-the-application-service-principal"></a>Krok 2: Usuń przypisanie zasad z nazwy głównej usługi aplikacji.  

``` powershell
Remove-AzureADApplicationPolicy -ObjectId <ObjectId of the Service Principal>  -PolicyId <ObjectId of the policy>
```

#### <a name="step-3-check-removal-by-listing-the-service-principals-the-policy-is-assigned-to"></a>Krok 3: Usunięcie wyboru poprzez wyszczególnienie nazwy główne usług zasad jest przypisany do 

``` powershell
Get-AzureADPolicyAppliedObject -ObjectId <ObjectId of the Policy>
```
## <a name="next-steps"></a>Następne kroki
- Aby uzyskać więcej informacji dotyczących działania uwierzytelniania w usłudze Azure AD, zobacz [scenariusze uwierzytelniania dla usługi Azure AD](develop/active-directory-authentication-scenarios.md).
- Aby uzyskać więcej informacji na temat użytkownika logowania jednokrotnego zobacz [dostęp do aplikacji i logowanie jednokrotne z usługą Azure Active Directory](active-directory-enterprise-apps-manage-sso.md)
- Odwiedź stronę [przewodnik dewelopera usługi Active Directory](develop/active-directory-developers-guide.md) omówienie związane z programowaniem całą jego zawartość.
