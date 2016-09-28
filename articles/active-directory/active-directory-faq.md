<properties
    pageTitle="Często zadawane pytania dotyczące usługi Azure Active Directory | Microsoft Azure"
    description="Często zadawane pytania dotyczące usługi Azure Active Directory zawierają odpowiedzi na pytania związane z uzyskiwaniem dostępu do platformy Azure i usługi Azure Active Directory, zarządzaniem hasłami i uzyskiwaniem dostępu do aplikacji."
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/16/2016"
    ms.author="markusvi"/>


# Często zadawane pytania dotyczące usługi Azure Active Directory

Azure Active Directory jest kompleksowym rozwiązaniem Identity as a Service (IDaaS), które obejmuje wszystkie aspekty tożsamości, zarządzania dostępem i bezpieczeństwa.


Aby uzyskać więcej informacji, zobacz [Co to jest usługa Azure Active Directory?](active-directory-whatis.md).



## Uzyskiwanie dostępu do platformy Azure i usługi Azure Active Directory


**Pytanie: dlaczego otrzymuję błąd „Nie znaleziono żadnych subskrypcji” podczas próby dostępu do usługi Azure AD w klasycznym portalu Azure (https://manage.windowsazure.com)?**

**Odpowiedź:** dostęp do klasycznego portalu Azure wymaga od każdego użytkownika uprawnień subskrypcji platformy Azure. Jeśli korzystasz z płatnej licencji usługi Office 365 lub Azure AD, przejdź do strony [http://aka.ms/accessAAD](http://aka.ms/accessAAD), aby użyć jednorazowej aktywacji. W przeciwnym razie należy aktywować pełną [wersję próbną platformy Azure](https://azure.microsoft.com/pricing/free-trial/) lub płatną subskrypcję. 

Aby uzyskać więcej informacji, zobacz:

- [Jak subskrypcje platformy Azure są kojarzone z usługą Azure Active Directory](active-directory-how-subscriptions-associated-directory.md)

- [Zarządzanie katalogiem dla subskrypcji usługi Office 365 na platformie Azure](active-directory-manage-o365-subscription.md)

---

**Pytanie: jaka jest relacja między usługami Azure AD, Office 365 i platformą Azure?**

**Odpowiedź:** usługa Azure Active Directory zawiera typowe funkcje związane z tożsamością i dostępem do wszystkich usług online firmy Microsoft. Niezależnie od tego, czy używasz usługi Office 365, Microsoft Azure, Intune lub innych, już używasz usługi Azure AD, która umożliwia logowanie i zarządzanie dostępem do tych wszystkich usług. 

W rzeczywistości wszyscy użytkownicy włączeni do usług online firmy Microsoft są zdefiniowany jako konta użytkowników w co najmniej jednym wystąpieniu usługi Azure AD. Możesz włączyć te konta do bezpłatnych funkcji usługi Azure AD, np. dostępu do aplikacji w chmurze.
 
Ponadto usługi płatne Azure AD (np. Azure AD Podstawowa, Premium, EMS itp.) uzupełniają inne usługi online, np. Office 365 i Microsoft Azure, zapewniając kompleksowe rozwiązania z zakresu skalowalnego zarządzania dla przedsiębiorstw i bezpieczeństwa.


---



## Wprowadzenie do hybrydowej usługi Azure AD


**Pytanie: jak połączyć katalog lokalny z usługą Azure AD?**

**Odpowiedź:** możesz połączyć katalog lokalny z usługą Azure AD przy użyciu narzędzia **Azure AD Connect**. 

Aby uzyskać więcej informacji, zobacz [Integrowanie tożsamości lokalnych z usługą Azure Active Directory](active-directory-aadconnect.md).


---

**Pytanie: jak skonfigurować logowanie jednokrotne (SSO) między katalogiem lokalnym i aplikacjami w chmurze?**

**Odpowiedź:** wystarczy skonfigurować logowanie SSO między katalogiem lokalnym i usługą Azure AD. Jeśli tylko uzyskujesz dostęp do aplikacji w chmurze poprzez usługę Azure AD, usługa automatycznie wymusza od użytkowników odpowiednie uwierzytelnianie przy użyciu ich lokalnych poświadczeń.

Implementowanie logowania SSO z pozycji lokalnej można z łatwością przeprowadzić przy użyciu rozwiązań federacyjnych, np. ADFS lub poprzez skonfigurowanie synchronizacji skrótów haseł. Możesz z łatwością wdrożyć obie opcje, korzystając z kreatora konfiguracji programu Azure AD Connect.
  

Aby uzyskać więcej informacji, zobacz [Integrowanie tożsamości lokalnych z usługą Azure Active Directory](active-directory-aadconnect.md).
  

---

**Pytanie: czy usługa Azure Active Directory zawiera samoobsługowy portal dla użytkowników w organizacji?**

**Odpowiedź:** tak, usługa Azure Active Directory zapewnia [Panel dostępu usługi Azure AD](http://myapps.microsoft.com) do samoobsługi użytkowników i dostępu do aplikacji. Jeśli korzystasz z usługi Office 365, możesz znaleźć wiele tych samych funkcji w portalu Office 365. 

Aby uzyskać więcej informacji, zobacz [Wprowadzenie do panelu dostępu](active-directory-saas-access-panel-introduction.md). 



---

**Pytanie: czy usługa Azure AD pomaga w zarządzaniu infrastrukturą lokalną?**

**Odpowiedź:** tak, pomaga. Usługa Azure AD — wersja Premium zawiera program **Connect Health**. Program Azure AD Connect Health pomaga monitorować i zdobywać informacje o lokalnej infrastrukturze do obsługi tożsamości oraz usługach synchronizacji.  

Aby uzyskać więcej informacji, zobacz [Monitorowanie lokalnej infrastruktury do obsługi tożsamości i usług synchronizacji w chmurze](active-directory-aadconnect-health.md).  

---

## Zarządzanie hasłami

**Pytanie: czy można użyć funkcji zapisywania zwrotnego haseł usługi Azure AD bez synchronizacji haseł? (Czyli chcę używać usługi Azure AD SSPR z funkcją zapisywania zwrotnego haseł, ale nie chcę przechowywać haseł w chmurze).**

**Odpowiedź:** nie musisz synchronizować haseł usługi AD z usługą Azure AD, aby skorzystać z funkcji zapisywania zwrotnego haseł. W środowisku federacyjnym logowanie SSO usługi Azure AD opiera się na katalogu lokalnym w celu uwierzytelniania użytkownika. Ten scenariusz nie wymaga śledzenia lokalnego hasła w usłudze Azure AD.

---

**Pytanie: jak długo trwa zwrotne zapisanie hasła z powrotem w lokalnym wystąpieniu usługi AD?**

**Odpowiedź:** zapisywanie zwrotne haseł działa w czasie rzeczywistym. 

Aby uzyskać więcej informacji, zobacz [Wprowadzenie do zarządzania hasłami](active-directory-passwords-getting-started.md) 


---

**Pytane: czy mogę użyć funkcji zapisywania zwrotnego haseł wobec haseł zarządzanych przez administratora?**

**Odpowiedź:** tak, jeśli włączysz funkcję zapisywania zwrotnego haseł, operacje na hasłach wykonywane przez administratora będą zwrotnie zapisywane w środowisku lokalnym.  

Aby uzyskać więcej odpowiedzi na pytania związane z hasłami, zobacz [Często zadawane pytania dotyczące zarządzania hasłami](active-directory-passwords-faq.md).

---

## Dostęp do aplikacji


**Pytanie: gdzie mogę znaleźć listę aplikacji, które są wstępnie zintegrowane z usługą Azure AD i jej funkcjami?**

**Odpowiedź:** usługa Azure AD ma ponad 2600 wstępnie zintegrowanych aplikacji od firmy Microsoft, dostawców usług aplikacji lub partnerów. Wszystkie wstępnie zintegrowane aplikacje obsługują logowanie SSO. Logowanie SSO umożliwia użycie poświadczeń w organizacji do uzyskania dostępu do aplikacji. Niektóre aplikacje obsługują również zautomatyzowaną aprowizację i anulowanie obsługi.

Aby uzyskać pełną listę wstępnie zintegrowanych aplikacji, zobacz [Active Directory Marketplace](https://azure.microsoft.com/marketplace/active-directory/).


---

**Pytanie: co w przypadku, jeśli aplikacja, której potrzebuję, nie znajduje się w witrynie Azure AD Marketplace?**

**Odpowiedź:** przy użyciu usługi Azure AD Premium możesz dodać i skonfigurować dowolną aplikację. W zależności od możliwości aplikacji i preferencji możesz skonfigurować logowanie jednokrotne i automatyczną aprowizację.  

Aby uzyskać więcej informacji, zobacz:

- [Konfigurowanie logowania jednokrotnego do aplikacji, które nie znajdują się w galerii aplikacji Azure Active Directory](active-directory-saas-custom-apps.md)
- [Włączanie automatycznej aprowizacji użytkowników i grup z usługi Azure Active Directory do aplikacji przy użyciu SCIM](active-directory-scim-provisioning.md) 


---

**Pytanie: jak użytkownicy logują się do aplikacji przy użyciu usługi Azure Active Directory?**
 
**Odpowiedź:** usługa Azure Active Directory oferuje użytkownikom kilka możliwości wyświetlania i uzyskiwania dostępu do swoich aplikacji, np.:

- Panel dostępu usługi Azure AD

- Program uruchamiający aplikacje usługi Office 365

- Bezpośrednie logowanie do aplikacji federacyjnych

- Linki bezpośrednie do federacyjnych, opartych na hasłach lub istniejących aplikacjach

Aby uzyskać więcej informacji, zobacz [Wdrażanie zintegrowanych aplikacji usługi Azure AD dla użytkowników](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users).


---

**Pytanie: jakie są różne sposoby włączania uwierzytelniania i logowania jednokrotnego do aplikacji przez usługę Azure Active Directory?**
 
**Odpowiedź:** usługa Azure Active Directory obsługuje wiele standardowych protokołów uwierzytelniania i autoryzacji, takich jak SAML 2.0, OpenID Connect, OAuth 2.0 i WS-Federation. Usługa Azure AD obsługuje również archiwizowanie haseł i automatyczne logowanie do aplikacji, które obsługują wyłącznie uwierzytelnianie oparte na formularzach.  

Aby uzyskać więcej informacji, zobacz:

- [Scenariusze uwierzytelniania dla usługi Azure AD](active-directory-authentication-scenarios.md)

- [Protokoły uwierzytelniania usługi Active Directory](https://msdn.microsoft.com/library/azure/dn151124.aspx)

- [Jak działa logowanie jednokrotne z usługą Azure Active Directory?](active-directory-appssoaccess-whatis.md#how-does-single-sign-on-with-azure-active-directory-work)


---

**Pytanie: czy mogę dodać aplikacje uruchamiane lokalnie?**

**Odpowiedź:** serwer proxy aplikacji usługi Azure AD zapewnia prosty i bezpieczny dostęp do wybranych lokalnych aplikacji sieci Web. Te aplikacje są dostępne w ten sam sposób co aplikacje SaaS w usłudze Azure Active Directory. Nie ma potrzeby stosowania sieci VPN ani zmiany infrastruktury sieciowej.  

Aby uzyskać więcej informacji, zobacz [Jak zapewnić bezpieczny, zdalny dostęp do aplikacji lokalnych](active-directory-application-proxy-get-started.md).


--- 

**Pytanie: jak mogę pozyskać usługę MFA dla użytkowników uzyskujących dostęp do określonej aplikacji?**

**Odpowiedź:** przy użyciu dostępu warunkowego usługi Azure AD możesz przypisać unikatowe zasady dostępu dla każdej aplikacji. W zasadach możesz wymagać korzystania z usługi MFA w każdym przypadku lub wtedy, gdy użytkownicy nie są podłączeni do sieci lokalnej.  

Aby uzyskać więcej informacji, zobacz [Zabezpieczanie dostępu do usługi Office 365 i innych aplikacji podłączonych do usługi Azure Active Directory](active-directory-conditional-access.md).


---

**Pytanie: czym jest automatyczna aprowizacja użytkowników dla aplikacji SaaS?**

**Odpowiedź:** usługa Azure Active Directory umożliwia automatyzację tworzenia, obsługi i usuwania tożsamości użytkowników w wielu popularnych aplikacjach w chmurze (SaaS). 

Aby uzyskać więcej informacji, zobacz [Automate User Provisioning and Deprovisioning to SaaS Applications with Azure Active Directory](active-directory-saas-app-provisioning.md) (Automatyzacja aprowizacji i anulowania obsługi użytkowników dla aplikacji SaaS przy użyciu usługi Azure Active Directory)

---






<!--HONumber=Sep16_HO3-->


