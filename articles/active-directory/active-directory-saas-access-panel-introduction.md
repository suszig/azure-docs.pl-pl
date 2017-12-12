---
title: "Co to jest panel dostępu w usłudze Azure Active Directory? | Microsoft Docs"
description: "Dowiedz się, jak używać różnych wersji panelu dostępu (przeglądarki sieci web, aplikacji systemu Android, aplikacji iPhone i iPad) dostępu do aplikacji SaaS."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: mtillman
ms.assetid: c0252d01-7e6e-4f79-a70e-600479577dfd
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/21/2017
ms.author: markvi
ms.reviewer: asteen
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d40f67c354e29f3a58c9d0f1967e0154b5b998d0
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/11/2017
---
# <a name="what-is-the-access-panel"></a>Co to jest panel dostępu?

Panel dostępu jest portalem sieci web. Umożliwia użytkownikowi służbowy lub konto służbowe w usłudze Azure Active Directory do wyświetlania i uruchamiania aplikacji opartej na chmurze administrator usługi Azure AD udzielił im dostępu do. Można również użyć grupami samoobsługi i funkcje zarządzania aplikacjami za pomocą panelu dostępu.

Panel dostępu jest oddzielony od portalu Azure i nie może być subskrypcji platformy Azure jest.

![Panel dostępu][1]

Panel dostępu umożliwia edytowanie niektórych ustawień profilu, umożliwiając między innymi:

- Zmień hasło skojarzone z konta służbowego

- Edytuj ustawienia resetowania hasła

- Edytuj ustawienia kontaktowe i preferencje dotyczące uwierzytelniania wieloskładnikowego (dla kont, które są wymagane do używania go przez administratora)

- Wyświetl konta alternatywne szczegóły, takie jak nazwa użytkownika, poczty e-mail oraz mobile i office numery telefonów i urządzeń

- Wyświetl i uruchamiania aplikacji opartej na chmurze, które administrator usługi Azure AD udzielił im dostępu do. Aby uzyskać więcej informacji na temat panelu dostępu z punktu widzenia użytkownika Zobacz Korzystanie z panelu dostępu. 

- Własnym Zarządzanie grupami. W szczególności administrator można tworzyć i Zarządzaj grupami zabezpieczeń i żądania członkostwa w grupach zabezpieczeń w usłudze Azure AD. Aby uzyskać więcej informacji, zobacz [Samoobsługowe zarządzanie grupami użytkowników w usłudze Azure AD](active-directory-accessmanagement-self-service-group-management.md) i [Zarządzanie grupami](active-directory-manage-groups.md).




## <a name="accessing-the-access-panel"></a>Uzyskiwanie dostępu do panelu dostępu

Aby dostęp do panelu dostępu, przechodząc na stronę następujący adres URL w przeglądarce sieci web:`http://myapps.microsoft.com`

Jeśli masz niestandardowe znakowanie skonfigurowana dla strony logowania, należy załadować tego znakowania przez dołączenie domeny organizacji na końcu adresu URL:`http://myapps.microsoft.com/<your domain>.com`

W takim przypadku można użyć dowolnej nazwy aktywnych lub zweryfikowanej domeny, skonfigurowanego w portalu Azure.

![Nazwa domeny Wingtip Toys][2]  

Należy rozpowszechniać adres URL dla wszystkich użytkowników, którzy będą Zaloguj się do aplikacji, które są zintegrowane z usługą Azure AD.

## <a name="authentication"></a>Authentication

Aby uzyskać dostęp do panelu dostępu, użytkownik musi zostać uwierzytelniony przy użyciu konta służbowego w usłudze Azure AD. Użytkownik może zostać uwierzytelniony do usługi Azure AD bezpośrednio. Alternatywnie Jeśli organizacji skonfigurował Federacji przy użyciu usługi Active Directory Federation Services (AD FS) ani innych technologii, możesz mogą być uwierzytelniane przez usługę Active Directory systemu Windows Server.

Jeśli masz subskrypcji Azure lub usługi Office 365 i był używany portalu Azure lub aplikacji pakietu Office 365, widać na liście aplikacji bez logowanie się ponownie. Jeśli nie są uwierzytelniane zostanie wyświetlony monit logowania przy użyciu nazwy użytkownika i hasło dla swojego konta w usłudze Azure AD. Jeśli organizacji skonfigurował federacyjnych, wpisując nazwę użytkownika jest wystarczająca.

Gdy użytkownik jest uwierzytelniony, mogą współdziałać z aplikacjami, które administrator ma zintegrowane z katalogiem. Aby dowiedzieć się, jak integrować aplikacje z usługą Azure AD, zobacz [co to jest dostęp do aplikacji i logowanie jednokrotne z usługą Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="web-browser-requirements"></a>Wymagania dotyczące przeglądarki sieci Web

Co najmniej panelu dostępu wymaga przeglądarki obsługującej JavaScript i włączył CSS. Aby użytkownik mógł zalogowani do aplikacji za pomocą opartego na hasłach rejestracji jednokrotnej (SSO) rozszerzenie panelu dostępu musi być zainstalowany w przeglądarce. Rozszerzenie jest pobierany automatycznie po wybraniu aplikacji, która jest skonfigurowana do opartego na hasłach logowania jednokrotnego.

Rozszerzenie panelu dostępu jest obecnie dostępny dla programu Internet Explorer 8 lub nowszy, krawędzi i Chrome, Firefox przeglądarek.

## <a name="mobile-app-support"></a>Obsługa aplikacji mobilnej

Zespół usługi Azure Active Directory publikuje mojej aplikacji mobilnej aplikacji. Po zainstalowaniu aplikacji można logowania się do aplikacji opartych na hasło logowania jednokrotnego w systemach iOS i urządzeniach z systemem Android.

> [!NOTE]
> Można logowania się do aplikacji, które obsługują federacji z usługą Azure AD (w tym Salesforce, usługi Google Apps, Dropbox, pole, Concur, produktu Workday, usługi Office 365 i więcej niż 70 innych użytkowników) w praktycznie dowolnej przeglądarki sieci web, na dowolnym urządzeniu, bez konieczności wtyczki lub przenośnych aplikacji. Wszystkie inne [dostęp do środowiska panelu](https://myapps.microsoft.com/) również nie wymagają mojej aplikacji mobilnej aplikacji do użycia na urządzeniu przenośnym.
>
>

### <a name="my-apps-for-android"></a>Moje aplikacje dla systemu Android

Moje aplikacje dla systemu Android jest obsługiwana na dowolnym urządzeniu z systemem Android, którym jest uruchomiona wersja systemu Android 4.1 lub nowszej.  
Jest on dostępny w [sklepu Google Play](https://play.google.com/store/apps/details?id=com.microsoft.myapps).

![Moje aplikacje dla systemu Android][3]   

### <a name="my-apps-for-iphone-and-ipad"></a>Moje aplikacje dla urządzenia iPhone i iPad

Moje aplikacje dla systemu iOS jest obsługiwana na wszystkie urządzenia iPhone lub iPad z systemem iOS w wersji 7 lub nowszy.  
Jest on dostępny w [sklepu Apple App Store](https://itunes.apple.com/us/app/my-apps-azure-active-directory/id824048653?mt=8).

![Moje aplikacje dla systemu iOS][4]    



## <a name="managed-browser-for-my-apps"></a>Zarządzane przeglądarki pod kątem Moje aplikacje

Moje aplikacje są również zintegrowane w programie Intune Managed Browser. Intune Managed Browser dla urządzeń iOS i Android odgrywa kluczową rolę w zapewnieniu, że dane na urządzeniach przenośnych jest bezpieczne. Umożliwia ona bezpiecznie przeglądać i przejdź do strony sieci web, które mogą zawierać informacje o firmie i zapewnia bezpieczne przeglądanie sieci web.  
Możesz znaleźć szybki dostęp do mojej aplikacji na stronie głównej Managed Browser oraz w zakładki, umożliwiając mniejszej liczby kliknięć do dowolnej aplikacji, którego chcesz uzyskać dostęp.

Jest on dostępny w [sklepu Apple App Store](https://itunes.apple.com/us/app/microsoft-intune-managed-browser/id943264951?mt=8) i [sklepu Google Play](https://play.google.com/store/apps/details?id=com.microsoft.intune.mam.managedbrowser&hl=en).

![Przeglądarka Mananged Moje aplikacje][5]    





## <a name="tips-for-testing-the-user-experience"></a>Porady dotyczące testowania czynności użytkownika

Jeśli jesteś administratorem systemu Azure i użytkownik jest zalogowany do portalu Azure przy użyciu konta w katalogu, użytkownik zostanie automatycznie zarejestrowany do panelu dostępu jako swoim aktualnym kontem. W takim przypadku widoczne wszystkie aplikacje, które zostały przypisane do Ciebie.

**Aby przetestować jako *różnych* konta użytkownika:**

1. Kliknij menu użytkownika w prawym górnym rogu portalu Azure lub panel dostępu, a następnie wybierz **Wyloguj**. 
2. Przejdź do [panelu dostępu](http://myapps.microsoft.com).
3. Na stronie logowania wpisz nazwę użytkownika i hasło dla konta w katalogu, który ma zostać przetestowana.


## <a name="starting-applications"></a>Uruchamianie aplikacji

Kilka typów aplikacji może występować w panelu dostępu.

### <a name="office-365-applications"></a>Aplikacje pakietu Office 365

Jeśli organizacja korzysta z aplikacji usługi Office 365, jest udzielana dla nich aplikacje pakietu Office 365 pojawiają się na Twoje panelu dostępu.

Po kliknięciu kafelka aplikacji dla aplikacji usługi Office 365 są przekierowywane do aplikacji i zostanie automatycznie zalogowany.

### <a name="microsoft-and-third-party-applications-configured-with-federation-based-sso"></a>Aplikacje firmy Microsoft i innych firm skonfigurowaną na podstawie federacyjnego logowania jednokrotnego

Administrator może dodać aplikacje w sekcji usługi Active Directory w portalu Azure w trybie logowania jednokrotnego ustawioną **Azure AD rejestracji jednokrotnej**. Te aplikacje może zobaczyć tylko, jeśli administrator ma jawnie przyznane uprawnienia dostępu do aplikacji.

Po kliknięciu kafelka dla jednej z tych aplikacji, są przekierowywane i zostanie automatycznie zalogowany do aplikacji.

### <a name="password-based-sso-without-identity-provisioning"></a>Na podstawie hasła logowania jednokrotnego bez obsługi tożsamości

Administrator może dodać aplikacje w sekcji usługi Active Directory w portalu Azure w trybie logowania jednokrotnego ustawioną **opartego na hasłach rejestracji jednokrotnej**. Wszyscy użytkownicy w katalogu widzą wszystkie aplikacje, które zostały skonfigurowane w tym trybie.

Po raz pierwszy, kliknięciu kafelka dla jednej z tych aplikacji, zostanie wyświetlony monit zainstalować wtyczkę logowania jednokrotnego hasła dla programu Internet Explorer lub przeglądarki Chrome. Instalacja może wymagać ponownego uruchomienia przeglądarki sieci web. Po powrocie do panelu dostępu i ponownie kliknij Kafelek aplikacji, monit o nazwę użytkownika i hasło dla aplikacji. Po wprowadzeniu nazwy użytkownika i hasła, te poświadczenia są bezpiecznie przechowywane i połączony z kontem w usłudze Azure AD.

Przy następnym kliknięciu kafelka aplikacji, użytkownik zostanie automatycznie zarejestrowany aplikacji.  
Nie trzeba ponownie wprowadź swoje poświadczenia i/lub Zainstaluj wtyczkę logowania jednokrotnego hasła.

Jeśli poświadczenia zostały zmienione w aplikacji docelowej innych firm, należy również zaktualizować swoje poświadczenia, które są przechowywane w usłudze Azure AD. 

**Aby zaktualizować poświadczenia:**

1. Wybierz ikonę na kafelku aplikacji.
2. Wybierz **zaktualizować poświadczenia** ponownie wprowadzić nazwę użytkownika i hasło dla aplikacji.


### <a name="password-based-sso-with-identity-provisioning"></a>Oparte na hasłach rejestracji Jednokrotnej z inicjowaniem obsługi administracyjnej tożsamości

Administrator może dodawać aplikacje w **usługi Active Directory** części portalu Azure w trybie logowania jednokrotnego ustawioną **opartego na hasłach rejestracji jednokrotnej**, oraz inicjowanie obsługi tożsamości.

Po raz pierwszy, kliknij Kafelek aplikacji, dla jednego z tych aplikacji, monit o zainstalowanie **logowania jednokrotnego hasła dodatek dla programu Internet Explorer lub przeglądarki Chrome**. Instalacja może wymagać ponownego uruchomienia przeglądarki sieci web.  
Po powrocie do panelu dostępu i ponownie kliknij Kafelek aplikacji, użytkownik zostanie automatycznie zarejestrowany aplikacji.

Niektóre aplikacje mogą wymagać można zmienić hasło przy pierwszym logowaniu. Jeśli poświadczenia zostały zmienione w aplikacji docelowej innych firm, należy również zaktualizować poświadczenia, które są przechowywane w usłudze Azure AD. 

**Aby zaktualizować poświadczenia:**

1. Wybierz ikonę na kafelku aplikacji.
2. Wybierz **zaktualizować poświadczenia** ponownie wprowadzić nazwę użytkownika i hasło dla aplikacji.


### <a name="application-with-existing-sso-solutions"></a>Aplikacja z istniejącymi rozwiązaniami do logowania jednokrotnego

Aby skonfigurować logowanie Jednokrotne dla aplikacji, Azure portal udostępnia trzecia opcja o nazwie **istniejących rejestracji jednokrotnej**. Ta opcja umożliwia administratorem, aby utworzyć łącze do aplikacji i umieść ją na panel dostępu dla wybranych użytkowników.

Na przykład jeśli aplikacja jest skonfigurowana do uwierzytelniania użytkowników za pomocą usług AD FS 2.0, administrator może użyć **istniejących rejestracji jednokrotnej** opcję, aby utworzyć łącze do niego w panelu dostępu. Gdy uzyskujesz dostęp do łącza, są uwierzytelniane za pośrednictwem usług AD FS 2.0 lub niezależnie od istniejącej rejestracji Jednokrotnej rozwiązania przez aplikację.


## <a name="next-steps"></a>Następne kroki

- Aby wyświetlić listę wszystkich tematów, które są powiązane z zarządzaniem aplikacjami, zobacz [indeks artykułów dotyczących zarządzania aplikacjami w usłudze Azure Active Directory](active-directory-apps-index.md).
 
- Aby uzyskać informacje o integracji aplikacji SaaS w usłudze Azure AD, zobacz [lista samouczków dotyczących sposobów integracji aplikacji SaaS](active-directory-saas-tutorial-list.md).
 
- Aby dowiedzieć się więcej o zarządzaniu aplikacjami za pomocą usługi Azure AD, zobacz [wprowadzenie do uzyskania dostępu do pojedynczego aplikacji logowania jednokrotnego i zarządzanie nimi w usłudze Azure Active Directory](active-directory-appssoaccess-whatis.md).
 
- Aby dowiedzieć się więcej na temat Inicjowanie obsługi użytkowników, zobacz [zautomatyzować użytkownika alokowania i anulowania alokowania do aplikacji SaaS](active-directory-saas-app-provisioning.md).

<!--Image references-->
[1]: ./media/active-directory-saas-access-panel-introduction/01.png
[2]: ./media/active-directory-saas-access-panel-introduction/02.png
[3]: ./media/active-directory-saas-access-panel-introduction/03.png
[4]: ./media/active-directory-saas-access-panel-introduction/04.png
[5]: ./media/active-directory-saas-access-panel-introduction/05.png
