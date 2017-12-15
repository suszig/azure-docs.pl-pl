---
title: "Zarządzanie kontami administracyjnym awaryjnego dostępu w usłudze Azure AD | Dokumentacja firmy Microsoft"
description: "Informacje dotyczące używania kont dostęp awaryjny ułatwiające organizacjom ograniczenie dostępu uprzywilejowanego w istniejącym środowisku usługi Azure Active Directory."
services: active-directory
keywords: "Nie dodawaj lub Edytuj słowa kluczowe bez konsultacji z ekspert od optymalizacji pod kątem Wyszukiwarek."
author: markwahl-msft
ms.author: billmath
ms.date: 12/13/2017
ms.topic: article-type-from-white-list
ms.service: active-directory
ms.workload: identity
ms.custom: it-pro
ms.reviewer: markwahl-msft
ms.openlocfilehash: 75ea8e445c890e7af5ab14f4fc4c53cfb1af4568
ms.sourcegitcommit: 0e4491b7fdd9ca4408d5f2d41be42a09164db775
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/14/2017
---
# <a name="managing-emergency-access-administrative-accounts-in-azure-ad"></a>Zarządzanie dostęp awaryjny konta z uprawnieniami administracyjnymi w usłudze Azure AD 

Dla większości codziennych działań nie są wymagane prawa rola administratora globalnego.  Użytkownicy powinna nie można trwale przypisana do tej roli, użytkownik przypadkowo mogą wykonywać zadanie z uprawnieniami wyższe niż wymagana. Gdy użytkownik musi działać jako administrator globalny, ich należy aktywować przypisania roli przy użyciu usługi Azure AD PIM na własne konto lub alternatywne konta administracyjnego.

Oprócz użytkowników biorąc praw dostępu administracyjnego do siebie klienci muszą przygotować się do zapewnienia, że nie są w sytuacji, gdy ich może być przypadkowo zablokowana poza administracji ich dzierżawy usługi Azure AD, ponieważ ich do podpisania w lub uaktywnić konto istniejące danego użytkownika z uprawnieniami administratora.  One ograniczanie wpływu przypadkowemu braku dostęp administracyjny do przechowywania w swojej dzierżawy, co najmniej dwa *kont dostęp awaryjny*.

Dostęp awaryjny kont pomagające organizacjom ograniczenie dostępu uprzywilejowanego w istniejącym środowisku usługi Azure Active Directory. Te konta są wysoce uprzywilejowane i nie są przypisane do konkretnych osób. Dostęp awaryjny konta są ograniczone do awaryjnego dla scenariuszy "awaryjne, gdy nie można użyć normalnego konta z uprawnieniami administracyjnymi.  Organizacje muszą zapewnić w celu kontrolowania i ograniczania się tylko do tego czasu, dla których konieczne jest użycie kont awaryjnych.

Scenariusze, w których organizacja może być konieczne użycie konta awaryjne dostępu:

 - Konta użytkowników są federacyjnych i federacyjnych jest niedostępna z powodu przerwania połączenia sieciowego lub awaria dostawcy tożsamości.  Na przykład jeśli host dostawcy tożsamości w środowisku klienta zostało zamknięte, użytkownik może nie będzie mógł zalogować się przypadku usługi Azure AD przekierowania do dostawcy tożsamości. 
 - Administratorzy są rejestrowane za pomocą usługi MFA i wszystkie ich poszczególne urządzenia są niedostępne.  Istnieje możliwość, że użytkownicy mogą nie pełną uwierzytelnianie wieloskładnikowe, aby aktywować rolę, jeśli na przykład awaria sieci komórki może uniemożliwić użytkownikom uniemożliwia odbieranie połączeń telefonicznych i odbieranie wiadomości tekstowych i że jego mechanizmu tylko że zarejestrowanym urządzeniu. 
 - Nazwisko osoby, która ma dostęp administratora globalnego organizacji w lewo.  Usługi Azure AD zapobiega ostatnie konto administratora globalnego usuwany, ale nie uniemożliwiają konto przed usunięciem lub wyłączone lokalnie, co prowadzi do sytuacji, w których organizacja jest nie można odzyskać tego konta.

## <a name="initial-configuration"></a>Wstępna konfiguracja

Utwórz dwa lub więcej kont awaryjnych dostępu.  Powinny być tylko w chmurze kont przy użyciu *. onmicrosoft.com domeny, które nie są federacyjnych lub synchronizowane ze środowiska lokalnego.  

Nie powinna być skojarzona z żadnym użytkownikiem poszczególnych w organizacji.  Organizacje należy upewnić się, że poświadczenia tego konta są przechowywane w bezpiecznej i znanych tylko do osób, które są autoryzowane do używania ich. 

Uwaga: Zwykle hasło konta dla konta dostęp awaryjny jest podzielone na dwie lub trzy części, zapisane w oddzielnych części dokumentu i przechowywane w bezpieczny, ognioodporną sejfów znajdujących się w różnych lokalizacjach bezpieczne. Upewnij się, że konta dostęp awaryjny nie są połączone z telefonów komórkowych wszelkie dostarczone przez pracowników, które są przesyłane z poszczególnych pracowników lub innych poświadczeń danego pracownika tokenów sprzętu w przypadku tego pracownika jest nieosiągalny w chwili wymagane jest poświadczenie. 

### <a name="initial-configuration-with-permanent-assignments"></a>Wstępna konfiguracja z przypisania stałego

Jedną z opcji jest można przypisać tych użytkowników jako trwałe członkowie roli administratora globalnego.  Będzie to odpowiednie dla organizacji, które nie mają subskrypcji Azure AD Premium P2.

Usługi Azure AD zaleca wymagane że uwierzytelnianie wieloskładnikowe (MFA) dla wszystkich poszczególnych użytkowników, w tym administratorów i innych użytkowników, którzy będzie miało wpływ istotne, jeśli ich konto zostało naruszone (np. finansowych funkcjonariusze). Pozwala to ograniczyć ryzyko ataku z powodu którego bezpieczeństwo zostało naruszone hasła. Jednak jeśli organizacja nie ma udostępnionych urządzeń, następnie uwierzytelnianie wieloskładnikowe może nie można dla tych kont awaryjnych dostępu.  Jeśli konfigurujesz zasady dostępu warunkowego, aby wymagać [rejestracji usługi MFA dla każdego administratora](https://docs.microsoft.com/en-us/azure/multi-factor-authentication/multi-factor-authentication-get-started-user-states) dla usługi Azure AD, a drugą podłączoną aplikacji SaaS, może być konieczne skonfigurowanie zasad wykluczenia do wykluczenia z tego konta dostęp awaryjny wymaganie.

### <a name="initial-configuration-with-approvals"></a>Wstępna konfiguracja z zatwierdzeniami

Innym rozwiązaniem jest skonfigurowanie tych użytkowników jako kwalifikujące się i osób zatwierdzających, aby aktywować rolę administratora globalnego.  Organizacja ma subskrypcji usługi Azure AD Premium P2, a także użyć opcji MFA odpowiedni w przypadku udostępnienia między wiele osób i środowisko sieciowe jest wymagany.  Jest to spowodowane aktywacji rolę administratora globalnego wymaga od użytkownika wcześniej wykonano uwierzytelnianie wieloskładnikowe.  Dowiedz się więcej o [jak zażądać usługi MFA w usłudze Azure AD Privileged Identity Management](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-privileged-identity-management-how-to-require-mfa).

Przy użyciu usługi MFA skojarzone z urządzeń osobistych nie są zalecane dla kont awaryjnych dostępu, ponieważ w rzeczywistych awaryjnego jeden osoba musi mieć dostęp do urządzenia zarejestrowane przez usługę MFA może nie być ten, który posiada urządzenia osobistego.  Należy również rozważyć zagrożeń, jeśli z powodu nieprzewidzianego okoliczności telefonu komórkowego lub innych sieci są niedostępne podczas klęski żywiołowej awaryjnego.  Dlatego jest ważne upewnić się, że wszystkie zarejestrowane urządzenia są przechowywane w znane, bezpiecznej lokalizacji, która zawiera wiele sposób komunikowania się z usługą Azure AD.

## <a name="ongoing-monitoring"></a>Ciągłe monitorowanie

Monitor [usługi Azure AD, zaloguj się i dzienniki inspekcji](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-reporting-activity-sign-ins) żadnych logowania i inspekcji działania z kontami awaryjnego dostępu.  Zwykle tych kont nie powinny być logowanie i powinien nie być wprowadzania zmian, więc korzystanie z nich jest prawdopodobnie nietypowych i wymagają dochodzenia zabezpieczeń.

## <a name="account-check-validation-must-occur-at-regular-intervals"></a>Sprawdzanie poprawności wyboru konta należy w regularnych odstępach czasu

Co najmniej wykonaj następujące czynności:
 - Co 90 dni
 - Jeśli wprowadzono zmiany w personel działu informatycznego — zadanie zmienić, wyjścia lub nowego zatrudnienia
 - Jeśli zmieniono subskrypcji usługi Azure AD w organizacji

Aby upewnić się, pracownicy są uczone w sposób używania kont awaryjnych dostępu:

1.  Sprawdź, czy pamiętać, że konto działania wyboru bieżących pracowników monitorowania zabezpieczeń.
2.  Zweryfikuj kont użytkowników w chmurze może Zaloguj się i aktywować ich ról oraz czy użytkownicy potrzebują do wykonania tych kroków w sytuacji awaryjnej są uczone procesu.
3.  Upewnij się, że ich nie zostały zarejestrowane dla MFA lub SSPR urządzenia lub dane osobowe dowolnego użytkownika.  
4. Jeśli konta są zarejestrowane w usłudze MFA na urządzeniu, do użycia podczas uaktywniania roli, upewnij się, że urządzenie jest dostępny dla wszystkich administratorów, którzy może być konieczne użycie go w sytuacji awaryjnej.  Sprawdź również, że urządzenie zostało zarejestrowane przez co najmniej dwa mechanizmy, które nie mają wspólnego trybu awaryjnego (np. urządzenie może skomunikować się z Internetem, zarówno za pośrednictwem sieci bezprzewodowej dla obiekt, a także za pośrednictwem sieci dostawcy komórki).
5.  Zaktualizuj poświadczenia.

## <a name="next-steps"></a>Następne kroki
- [Dodaj użytkowników w chmurze](add-users-azure-active-directory.md) i [przypisać nowego użytkownika do roli administratora globalnego](active-directory-users-assign-role-azure-portal.md)
- [Załóż konto usługi Azure Active Directory Premium](active-directory-get-started-premium.md), jeśli nie jest jeszcze
- [Wymaga usługi Azure MFA dla poszczególnych użytkownicy przypisani jako administratorzy](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-user-states)
- [Skonfiguruj stosowanie dodatkowych zabezpieczeń dla administratorów globalnych w usłudze Office 365](https://support.office.com/article/Protect-your-Office-365-global-administrator-accounts-6b4ded77-ac8d-42ed-8606-c014fd947560), jeśli używasz usługi Office 365
- [Wykonanie przeglądu dostępu administratorów globalnych](active-directory-privileged-identity-management-how-to-start-security-review.md) i [przejście istniejących administratorów globalnych do bardziej szczegółowych ról administratora](active-directory-assign-admin-roles-azure-portal.md)

