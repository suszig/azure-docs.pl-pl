---
title: "Co nowego? Informacje o wersji dla usługi Azure Active Directory | Dokumentacja firmy Microsoft"
description: "Dowiedz się, co jest nowego w usłudze Azure Active Directory (Azure AD) w tym informacje o najnowszej wersji, znane problemy, poprawki, przestarzałe funkcje i nadchodzących zmianach."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
featureFlags: clicktale
ms.assetid: 06a149f7-4aa1-4fb9-a8ec-ac2633b031fb
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/06/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 766ab10c853eb56a1203686d21d0e131c44a41b5
ms.sourcegitcommit: adf6a4c89364394931c1d29e4057a50799c90fc0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/09/2017
---
# <a name="whats-new-in-azure-active-directory"></a>Nowości w usłudze Azure Active Directory




> Stosowanie nowości w usłudze Azure Active Directory to subskrypcja [źródła danych](https://docs.microsoft.com/api/search/rss?search=%22what%27s%20new%20in%20azure%20active%20directory%3F%22&locale=en-us) w ulubionej czytnik danych RSS.



Firma Microsoft umożliwiają zwiększenie usługi Azure Active Directory w sposób ciągły. Aby umożliwić bądź na bieżąco o najnowszych zmianach, w tym temacie przedstawiono informacje o:

-   Najnowsze wersje 
-   Znane problemy 
-   Poprawki błędów 
-   Funkcje uznane za przestarzałe 
-   Plany zmiany 

Należy ponownie tę stronę regularnie, ponieważ trwa jego aktualizacja co miesiąc.

## <a name="november-2017"></a>2017 listopada

**Typ:** przestarzałe funkcje  
**Kategoria usług:** ACS  
**Możliwości produktu:** usługi kontroli dostępu 

<a name="acs-retirement"></a>

Microsoft Azure Active Directory kontroli dostępu (znanej także jako usługa kontroli dostępu lub ACS) została wycofana w 2018 opóźnione.  Więcej informacji, w tym szczegółowy harmonogram & wskazówki wysokiego poziomu migracji zostanie podana w następnych kilku tygodni. Tymczasem zostaw komentarze na tej stronie z pytania dotyczące usług ACS i członkiem naszego zespołu będą dotrzeć do pomocy odpowiedzi.

---


## <a name="october-2017"></a>2017 października

**Typ:** Planowanie zmian  
**Kategoria usług:** raportowania  
**Możliwości produktu:** zarządzania cyklem życia tożsamości  


**Wycofano raportów usługi Azure AD (wersja beta) interfejsów API w obszarze `https://graph.windows.net/<tenant-name>/reports/` węzła**

Azure portal udostępnia:

- Nową konsolę administracyjną usługi Azure Active Directory 
- Nowe interfejsy API dla działania i zabezpieczeń raportów
 
Z powodu nowych funkcji, raport interfejsów API w obszarze **/reports** punktu końcowego zostaną wycofane na 10 grudnia 2017 r. 

---

**Typ:** stałej   
**Kategoria usług:** Moje aplikacje  
**Możliwości produktu:** logowania jednokrotnego  


Azure Active Directory obsługuje wykrywanie pola automatycznego logowania dla aplikacji, które renderują pola HTML nazwy użytkownika i hasła.  Te kroki są udokumentowane w artykule [automatycznie Przechwytywanie pola logowania dla aplikacji](application-config-sso-problem-configure-password-sso-non-gallery.md#how-to-manually-capture-sign-in-fields-for-an-application). Tej funkcji można znaleźć, dodając *Non-galerii* aplikację na **aplikacje dla przedsiębiorstw** strony [portalu Azure](http://aad.portal.azure.com). Ponadto można skonfigurować **rejestracji jednokrotnej** tryb tej nowej aplikacji do **opartego na hasłach rejestracji jednokrotnej**, wprowadzając adres URL sieci web, a następnie zapisanie strony.
 
Ze względu na problem dotyczący usługi ta funkcja została tymczasowo wyłączona w danym okresie czasu. Problem został rozwiązany i wykrywanie automatyczne logowanie pole jest dostępne ponownie.

---

**Typ:** nowej funkcji  
**Kategoria usług:** MFA  
**Możliwości produktu:** tożsamości bezpieczeństwa i ochrony  


Na świecie, które firma Microsoft live w uwierzytelnianie wieloskładnikowe (MFA) jest integralną część ochrony Twojej organizacji. Zespół tożsamości w firmie Microsoft ewoluuje uwierzytelnianie wieloskładnikowe, aby więcej adaptacyjną poświadczeń i doświadczenia płynniejszą. Jestem dzisiaj przyjemnością ogłaszamy dwie ważne czynności, w tym podróży: 

- Integracji wyników wieloskładnikowego żądania bezpośrednio do usługi Azure AD logowania raportu, tym programowy dostęp do usługi MFA wyników

- Lepsza integracja konfiguracji usługi MFA w podstawowej konfiguracji usługi Azure AD doświadczenie w portalu Azure

Z publicznej wersji zapoznawczej MFA zarządzania i raportowania są integralną częścią podstawowe środowisko konfiguracji usługi Azure AD, co umożliwia zarządzanie funkcje portalu zarządzania usługi MFA w ramach obsługi usługi Azure AD.

Aby uzyskać więcej informacji, zobacz [informacje dotyczące uwierzytelniania wieloskładnikowego raportowanie w portalu Azure](active-directory-reporting-activity-sign-ins-mfa.md) 


---
**Typ:** nowej funkcji  
**Kategoria usług:** warunki użytkowania  
**Możliwości produktu:** ładu  


**Warunki użytkowania w usłudze Azure AD** zapewnia prosty sposób prezentować informacje dla użytkowników końcowych. Daje to pewność, że użytkownicy będą widzieć zastrzeżenia istotne dla prawne lub wymaganiami dotyczącymi zgodności.

Warunki użytkowania usługi Azure AD można użyć w następujących scenariuszach:

- Ogólne warunki użytkowania obowiązujące wszystkich użytkowników w organizacji. 

- Określone warunki użytkowania na podstawie atrybutów użytkownika (np. lekarzy pielęgniarek vs lub krajowych vs międzynarodowych pracowników, programach grup dynamicznych). 

- Określone warunki użytkowania do uzyskiwania dostępu do aplikacji wpływ wysokiej biznesowych, takie jak Salesforce.

Aby uzyskać więcej informacji, zobacz [Azure Active Directory warunki użytkowania](active-directory-tou.md).


---
**Typ:** nowej funkcji  
**Kategoria usług:** PIM  
**Możliwości produktu:** Privileged Identity Management  


Z usługi Azure Active Directory uprzywilejowanych tożsamości zarządzania (PIM), mogą teraz zarządzać, kontrolę i monitorowanie dostępu do zasobów Azure (wersja zapoznawcza) w ramach danej organizacji. W tym subskrypcji, grupy zasobów i nawet maszyn wirtualnych. Wszystkie zasoby w portalu Azure, korzystających z funkcji Azure roli na podstawie kontroli dostępu (RBAC) korzystać ze wszystkich niezwykłych zabezpieczeń i możliwości zarządzania cyklem życia Azure AD PIM musi oferować i nowe, fantastyczne funkcje planujemy doprowadzić do Wkrótce role w AD platformy Azure.

Aby uzyskać więcej informacji, zobacz [PIM zasobów Azure](privileged-identity-management/azure-pim-resource-rbac.md).


---
**Typ:** nowej funkcji  
**Kategoria usług:** dostęp do przeglądu  
**Możliwości produktu:** ładu  


Przeglądy dostęp (wersja zapoznawcza) umożliwiają organizacjom efektywne zarządzanie członkostwami grup, a dostęp do aplikacji przedsiębiorstwa: 

- Można recertify dostępu użytkownika gościa przy użyciu dostępu przeglądy ich dostęp do aplikacji i członkostwa grup. Szczegółowych danych dostarczonych przez przeglądy dostępu Włącz recenzentów wydajnie zdecydować, czy gości powinny mieć stały dostęp.

- Przeprowadzając przeglądy dostępu, można zatwierdzać dostęp do aplikacji i członkostwo w grupach pracowników.

Kontrole z przeglądem dostępu można łączyć w programy odpowiednie dla danej organizacji, aby śledzić przeglądy dla aplikacji dotyczących zgodności lub aplikacji narażonych na ryzyko.

Aby uzyskać więcej informacji, zobacz [sprawdza dostęp do usługi Azure AD](active-directory-azure-ad-controls-access-reviews-overview.md).


---
**Typ:** nowej funkcji  
**Kategoria usług:** Moje aplikacje  
**Możliwości produktu:** logowania jednokrotnego  


**Możliwość Ukryj aplikacji innych firm z Moje aplikacje i uruchamiania usługi Office 365**

Teraz można lepiej zarządzać aplikacjami na Twoje portale użytkownika za pomocą nowego **Ukryj aplikacji** właściwości. Pomaga to z przypadkami, w którym kafelków aplikacji są wyświetlane dla usług zaplecza lub zduplikowane Kafelki i końcowa przeładowania Przyciski Uruchom aplikacji użytkownika. Przełącznik znajduje się w sekcji właściwości aplikacji innych producentów i etykietą **widoczny dla użytkownika?**. Można również ukryć aplikacji programowo przy użyciu programu PowerShell. 

Aby uzyskać więcej informacji, zobacz [Ukryj aplikacji innych producentów, od środowiska użytkownika w usłudze Azure Active Directory](active-directory-coreapps-hide-third-party-app.md). 


**Co to jest dostępny?**

 W ramach przejścia do nowej konsoli administracyjnej wprowadzono 2 nowych interfejsów API do pobierania dostępnych dzienników działania usługi Azure AD. Nowy zestaw interfejsów API zawierają bardziej zaawansowane funkcje filtrowania i sortowania funkcje oprócz bardziej zaawansowane funkcje inspekcji i rejestrowania działania. Obecnie jest możliwy wcześniej dostępne raporty dotyczące zabezpieczeń danych za pośrednictwem zdarzenia ryzyka Identity Protection interfejsu API programu Microsoft Graph.


## <a name="september-2017"></a>2017 września

**Typ:** funkcji zmienione  
**Kategoria usług:** programu Microsoft Identity Manager  
**Możliwości produktu:** zarządzania cyklem życia tożsamości  


Pakiet zbiorczy poprawek (kompilacja 4.4.1642.0) jest dostępny od 25 września 2017 Microsoft Identity Manager (MIM) 2016 2016 z dodatkiem Service Pack 1 (SP1). Ten pakiet zbiorczy:

- Rozwiązuje problemy i dodaje ulepszenia
- Jest aktualizacją zbiorczą, który zastępuje wszystkie aktualizacje MIM 2016 z dodatkiem SP1 do kompilacji 4.4.1459.0 dla programu Microsoft Identity Manager 2016. 
- Wymagane jest posiadanie **4.4.1302.0 kompilacji programu Microsoft Identity Manager 2016.** 

Aby uzyskać więcej informacji, zobacz [pakiet zbiorczy poprawek (kompilacja 4.4.1642.0) jest dostępna dla programu Microsoft Identity Manager 2016 z dodatkiem SP1](https://support.microsoft.com/en-us/help/4021562). 

---
