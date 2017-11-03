---
title: "Dostęp do aplikacji Sklep internetowy i delegowane zarządzanie za pomocą usługi Azure Active Directory | Dokumentacja firmy Microsoft"
description: "W tym artykule opisano sposób włączania dostępu do aplikacji Sklep internetowy i delegowane zarządzanie za pomocą usługi Azure Active Directory."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 448a7fe8-a162-475e-9ba2-2e3ab59302bc
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/06/2017
ms.author: curtand
ms.reviewer: asmalser
ms.custom: oldportal;it-pro;
ms.openlocfilehash: 39c62461c9659b0cb4422de88686283ba462c53b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="self-service-application-access-and-delegated-management-with-azure-active-directory"></a>Dostęp do aplikacji Sklep internetowy i delegowane zarządzanie za pomocą usługi Azure Active Directory
Włączenie funkcji samoobsługi dla użytkowników końcowych jest typowym scenariuszem dla organizacji IT. Wielu użytkowników, wiele aplikacji i osoby, która jest best-informed podejmowanie decyzji o przyznanie dostępu nie może być administratora katalogu. Często osoba najlepszych decyzji o tym, kto ma dostęp do aplikacji jest realizacji zespołu lub innych administratora delegowanego. Jednak jest użytkownik, który korzysta z aplikacji, a użytkownik wie, muszą mieć możliwość wykonania zadania.

> [!IMPORTANT]
> Firma Microsoft zaleca zarządzanie usługą Azure AD przy użyciu [centrum administracyjnego usługi Azure AD](https://aad.portal.azure.com) w witrynie Azure Portal zamiast korzystania z klasycznej witryny Azure Portal przywołanej w niniejszym artykule. 

Dostęp do aplikacji Sklep internetowy jest funkcją [Azure Active Directory Premium](https://azure.microsoft.com/trial/get-started-active-directory/) P1 i P2 licencjonowania, która pozwala administratorom katalogu:

* Zezwalaj użytkownikom na żądanie dostępu do aplikacji za pomocą kafelka "Pobierz więcej aplikacji" w [Panel dostępu usługi Azure AD](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users)
* Zestaw, do których aplikacji użytkownicy mogą żądać dostępu do
* Ustaw czy zatwierdzenia jest wymagana dla użytkowników można było samodzielnie przypisać dostęp do aplikacji
* Zestaw, który należy zatwierdzać żądania i zarządzanie dostępem dla każdej aplikacji

Obecnie ta funkcja jest obsługiwana dla wszystkich wstępnie zintegrowanych i niestandardowych aplikacji, które obsługują jednym federacyjnym lub opartego na hasłach logowania jednokrotnego [galerii aplikacji usługi Azure Active Directory](https://azure.microsoft.com/marketplace/active-directory/all/), w tym aplikacji, takich jak Salesforce, Dropbox, Google Apps i inne.
W tym artykule opisano sposób:

* Konfigurowanie dostępu do aplikacji Sklep internetowy dla użytkowników końcowych, łącznie z konfigurowaniem zatwierdzeń opcjonalnych przepływu pracy 
* Delegowanie zarządzania dostępem do określonych aplikacji do najbardziej odpowiednich osób w organizacji i włączyć je do zatwierdzenia żądania dostępu, bezpośrednio przypisać dostęp do wybranych użytkowników za pomocą panelu dostępu do usługi Azure AD lub (opcjonalnie) ustawiona poświadczenia dostępu do aplikacji, gdy opartego na hasłach rejestracji jednokrotnej jest skonfigurowany

## <a name="configuring-self-service-application-access"></a>Konfigurowanie dostępu do aplikacji Sklep internetowy
Aby włączyć dostęp do aplikacji Sklep internetowy i skonfigurować aplikacje, które można dodać lub żądane przez użytkowników końcowych, wykonaj te instrukcje.

1. Zaloguj się do [klasycznego portalu Azure](https://manage.windowsazure.com/).

2.   W obszarze **usługi Active Directory** sekcji, wybierz swój katalog, a następnie wybierz **aplikacji** kartę. 

3. Wybierz **Dodaj** przycisk, a następnie użyj opcji Galerii, aby wybrać i dodać aplikację.

4. Po dodaniu aplikacji można uzyskać na stronie Szybki Start aplikacji. Kliknij przycisk **skonfigurować logowanie jednokrotne**, wybierz żądany pojedynczego logowania jednokrotnego tryb i zapisać konfigurację. 

5. Następnie wybierz pozycję **Konfiguruj** kartę. Aby użytkownicy mogli poprosić o dostęp do tej aplikacji z poziomu panelu dostępu do usługi Azure AD, należy ustawić **Zezwalaj na dostęp do aplikacji Sklep internetowy** do **tak**.
  
  ![][1]

6. Opcjonalnie skonfigurować zatwierdzania dla żądań dostępu, należy ustawić **wymagają zatwierdzenia przed udzieleniem im dostępu** do **tak**. Następnie można wybrać jedną lub więcej osób zatwierdzających za pomocą **osób zatwierdzających** przycisku.

  Osoba zatwierdzająca może być dowolny użytkownik w organizacji przy użyciu konta usługi Azure AD i mogą być odpowiedzialne za konto inicjowania obsługi administracyjnej, licencjonowanie lub innych proces biznesowy organizacja wymaga przed udzieleniem im dostępu do aplikacji. Osoba zatwierdzająca nawet może być właścicielem grupy jednego lub więcej udostępnionego konta grupy, a można przypisać użytkowników do jednej z następujących grup w celu udzielenia dostępu za pomocą konta współużytkowanego. 

  Jeśli nie zatwierdzenia jest wymagana, następnie użytkownicy natychmiast uzyskują aplikacji dodany do ich panel dostępu usługi Azure AD. Jeśli aplikacja została ustawiona dla [użytkownika automatycznego inicjowania obsługi administracyjnej](active-directory-saas-app-provisioning.md), lub nie został skonfigurowany ["zarządzane przez użytkownika" hasło logowania jednokrotnego tryb](active-directory-appssoaccess-whatis.md#password-based-single-sign-on), użytkownik ma już konto i hasło użytkownika.

7. Jeśli aplikacja została skonfigurowana do użycia na podstawie hasła logowania jednokrotnego, a następnie opcję umożliwiające osoba zatwierdzająca w celu ustawienia poświadczeń logowania jednokrotnego w imieniu każdy użytkownik jest również dostępna. Aby uzyskać więcej informacji, zobacz sekcję dotyczącą [delegowane zarządzanie dostępem](#delegated-application-access-management).

8. Na koniec **grupy użytkowników Self-Assigned** zawiera nazwę grupy, który jest używany do przechowywania użytkowników, którym przyznano, lub dostępem do aplikacji. Osoba zatwierdzająca dostępu staje się właścicielem tej grupy. Jeśli nazwa grupy pokazano nie istnieje, jest tworzona automatycznie. Opcjonalnie może mieć ustawionej nazwy grupy nazwę istniejącej grupy.

9. Aby zapisać konfigurację, kliknij przycisk **zapisać** w dolnej części ekranu. Teraz użytkownicy mogą Aby zażądać dostępu do tej aplikacji w panelu dostępu.

10. Aby wypróbować środowisko użytkownika końcowego, zaloguj się do panelu dostępu usługi Azure AD w organizacji pod adresem https://myapps.microsoft.com, najlepiej przy użyciu innego konta, która nie jest osobą zatwierdzającą aplikacji. 

11. W obszarze **aplikacji** , kliknij pozycję **Pobierz więcej aplikacje** kafelka. Ten Kafelek Wyświetla wszystkie aplikacje, które zostały włączone samoobsługi dostępu do aplikacji w katalogu, z możliwością wyszukiwania i filtrowania według kategorii aplikacji po lewej stronie galerii. 

12. Kliknięcie aplikacji rozpoczyna proces żądania. Jeśli nie jest wymagany żaden proces zatwierdzania, a następnie aplikacja zostanie natychmiast dodany w obszarze **aplikacji** kartę po krótkim potwierdzenia. Jeśli zatwierdzenia jest wymagana, czym pojawi się okno dialogowe tego wskazująca i zostanie wysłana wiadomość e-mail do osób zatwierdzających. Użytkownik musi zalogować się do panelu dostępu z systemem innym niż zatwierdzającą aby zobaczyć ten proces żądania.

13. Wiadomości e-mail powoduje, że osoba zatwierdzająca zalogowania się do panelu dostępu do usługi Azure AD i zatwierdzić żądanie. Gdy żądanie zostanie zatwierdzone i wszelkie specjalne procesy, należy zdefiniować mogły zostać wykonane przez osoby zatwierdzającej, użytkownik zobaczy aplikacji są wyświetlane w obszarze ich **aplikacji** kartę, której można zalogować do niego.

## <a name="delegated-application-access-management"></a>Zarządzanie dostępem aplikacji delegowanego
Osoba zatwierdzająca dostępu do aplikacji może być dowolny użytkownik w organizacji, który jest najbardziej odpowiednią osobą do zatwierdzenia lub odmowy dostępu do aplikacji w. Ten użytkownik może być odpowiedzialny za inicjowanie obsługi konta licencjonowania, lub inne proces biznesowy organizacja wymaga przed udzieleniem im dostępu do aplikacji.

Podczas konfigurowania dostępu aplikacji samoobsługi opisane powyżej, wszelkie przypisane aplikacji, zobacz osób zatwierdzających dodatkowe **Zarządzanie aplikacjami** kafelka w panelu dostępu do usługi Azure AD zawiera ich aplikacji, które mają dostęp administratora do. Kliknięcie aplikacji powoduje wyświetlenie ekranu z kilku opcji.

![][2]

### <a name="approve-requests"></a>Zatwierdzanie żądań
**Zatwierdzanie żądań** kafelka umożliwia osób zatwierdzających wyświetlić wszystkie oczekujące zatwierdzenia specyficzne dla danej aplikacji i przekierowuje kartę zatwierdzeń, gdzie potwierdzenia lub odmowy żądania. Zawsze, gdy żądanie zostanie utworzony nakazuje co należy zrobić osoba zatwierdzająca Ponadto otrzymuje zautomatyzowane wiadomości e-mail.

### <a name="add-users"></a>Dodawanie użytkowników
**Dodaj użytkowników** kafelka umożliwia osób zatwierdzających można bezpośrednio udzielić wybranym użytkownikom dostęp do aplikacji. Po kliknięciu tego kafelka, osoba zatwierdzająca widzi, okno dialogowe umożliwia je do widoku i Wyszukaj użytkowników w ich katalogu. Dodawanie wyników użytkownika w aplikacji jest pokazywany w tych użytkowników usługi Azure AD dostęp paneli lub usługi Office 365. Jeśli dowolny użytkownik ręcznego procesu udostępniania jest wymagana w aplikacji, zanim użytkownik będzie logować się, osoba zatwierdzająca należy wykonać ten proces przed udzieleniem im dostępu.  

### <a name="manage-users"></a>Zarządzanie użytkownikami
**Zarządzanie użytkownikami** kafelka umożliwia osób zatwierdzających bezpośrednio zaktualizować lub usunąć, którzy użytkownicy mają dostęp do aplikacji. 

### <a name="configure-password-sso-credentials-if-applicable"></a>Skonfiguruj poświadczenia logowania jednokrotnego hasło (jeśli dotyczy)
**Konfiguruj** kafelka jest wyświetlana tylko, jeśli aplikacja została skonfigurowana przez administratora IT, aby użyć opartego na hasłach rejestracji jednokrotnej i administrator przyznane osoby zatwierdzającej możliwość określenia hasła poświadczeń logowania jednokrotnego, jak opisano wcześniej. W przypadku wybrania, osoba zatwierdzająca zobaczy kilka opcji jak poświadczenia są propagowane do przypisanych użytkowników:

![][3]

* **Użytkownicy, zaloguj się przy użyciu swoje hasła** — w tym trybie przypisanych użytkowników wiedzieć, co ich nazwami użytkowników i hasła są przeznaczone dla aplikacji są proszeni o wprowadzenie je na ich pierwsze logowanie do aplikacji. Scenariusz odpowiada hasła Usługa rejestracji Jednokrotnej w przypadku których [zarządzania przez użytkowników poświadczeń](active-directory-appssoaccess-whatis.md#password-based-single-sign-on).
* **Użytkownicy są automatycznie logowani przy użyciu osobnych kont, którymi zarządza** — w tym trybie przypisanych użytkowników nie muszą wprowadzać lub znasz poświadczeń specyficzny dla aplikacji, podczas logowania do aplikacji. Zamiast tego, osoba zatwierdzająca Ustawia poświadczenia dla każdego użytkownika, po przypisaniu dostępu przy użyciu **Dodaj użytkownika** kafelka. Gdy użytkownik kliknie w aplikacji w panelu dostępu i usługi Office 365, zostaną automatycznie wylogowani przy użyciu poświadczeń, ustawione przez osoby zatwierdzającej. Scenariusz odpowiada hasła Usługa rejestracji Jednokrotnej w przypadku których [administratorom zarządzanie poświadczeniami](active-directory-appssoaccess-whatis.md#password-based-single-sign-on).
* **Użytkownicy zostaną automatycznie wylogowani za pomocą jednego konta, które mogę zarządzać** -szczególnych przypadkach, przypadkiem jest odpowiednie do użycia w przypadku wszystkich użytkowników przypisanych muszą otrzymać dostęp przy użyciu jednego, udostępnionego konta. Najbardziej typowe przypadek użycia dla tej funkcji jest z aplikacjami mediów społecznościowych, w którym organizacja ma konto pojedynczy "firmy" i wielu użytkowników należy udostępnić aktualizacje dla tego konta. Scenariusz również odpowiada hasła Usługa rejestracji Jednokrotnej w przypadku których [administratorom zarządzanie poświadczeniami](active-directory-appssoaccess-whatis.md#password-based-single-sign-on). Jednak po wybraniu tej opcji, osoba zatwierdzająca pojawi się monit o wprowadzenie nazwy użytkownika i hasła dla jednego, udostępnionego konta. Po ukończeniu wszystkich użytkowników przypisanych jest zalogowany przy użyciu tego konta, po kliknięciu przycisku w aplikacji w ich panele dostępu do usługi Azure AD lub usługi Office 365.

## <a name="additional-resources"></a>Dodatkowe zasoby
* [Indeks artykułów dotyczących zarządzania aplikacjami w usłudze Azure Active Directory](active-directory-apps-index.md)

<!--Image references-->
[1]: ./media/active-directory-self-service-application-access/ssaa_admin.PNG
[2]: ./media/active-directory-self-service-application-access/ssaa_ap_manage_app.PNG
[3]: ./media/active-directory-self-service-application-access/ssaa_ap_manage_app_config.PNG
