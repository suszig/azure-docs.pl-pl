---
title: "Samouczek: Integracji usługi Azure Active Directory z Cisco Webex | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak używać Cisco Webex z usługą Azure Active Directory w celu włączenia logowania jednokrotnego, automatyczne Inicjowanie obsługi i inne!"
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 26704ca7-13ed-4261-bf24-fd6252e2072b
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/10/2017
ms.author: jeedes
ms.openlocfilehash: b44b1a5b3e988a51db3325ec8a181651fa84e768
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-cisco-webex"></a>Samouczek: Integracji usługi Azure Active Directory z Cisco Webex
Celem tego samouczka jest pokazanie integracji Azure i Cisco Webex.  
Scenariusz opisany w tym samouczku założono, że już następujące elementy:

* Ważnej subskrypcji platformy Azure
* Dzierżawy Cisco Webex

Ten samouczek użytkowników usługi Azure AD, zostały przypisane do Cisco Webex będzie można funkcji logowania jednokrotnego do aplikacji w witrynie firmy Cisco Webex (usługa zainicjował dostawcy logowania) lub przy użyciu [wprowadzenie do panelu dostępu ](active-directory-saas-access-panel-introduction.md).

Scenariusz opisany w tym samouczku składa się z następujących bloków konstrukcyjnych:

* Włączanie integracji aplikacji dla Cisco Webex
* Konfigurowanie rejestracji jednokrotnej (SSO)
* Konfigurowanie Inicjowanie obsługi użytkowników
* Przypisywanie użytkowników

![Scenariusz](./media/active-directory-saas-cisco-webex-tutorial/IC777614.png "scenariusza")

## <a name="enable-the-application-integration-for-cisco-webex"></a>Włącz integrację aplikacji dla Cisco Webex
Celem tej sekcji jest przedstawiają sposób włączania integracji aplikacji dla Cisco Webex.

**Aby włączyć integrację aplikacji dla Cisco Webex, wykonaj następujące czynności:**

1. W klasycznym portalu Azure, w okienku nawigacji po lewej stronie kliknij **usługi Active Directory**.
   
   ![Usługi Active Directory](./media/active-directory-saas-cisco-webex-tutorial/IC700993.png "usługi Active Directory")
2. Z **katalogu** listy, wybierz katalog, dla którego chcesz włączyć integracji katalogów.
3. Aby otworzyć widok aplikacji, w widoku katalogu, kliknij przycisk **aplikacji** w menu u góry.
   
   ![Aplikacje](./media/active-directory-saas-cisco-webex-tutorial/IC700994.png "aplikacji")
4. Kliknij przycisk **Dodaj** w dolnej części strony.
   
   ![Dodaj aplikację](./media/active-directory-saas-cisco-webex-tutorial/IC749321.png "Dodaj aplikację")
5. Na **co chcesz zrobić** okna dialogowego, kliknij przycisk **dodać aplikację z galerii**.
   
   ![Dodawanie aplikacji z gallerry](./media/active-directory-saas-cisco-webex-tutorial/IC749322.png "dodać aplikację z gallerry")
6. W **pole wyszukiwania**, typ **Cisco Webex**.
   
   ![Galerii aplikacji](./media/active-directory-saas-cisco-webex-tutorial/IC777615.png "galerii aplikacji")
7. W okienku wyników wybierz **Cisco Webex**, a następnie kliknij przycisk **Complete** można dodać aplikację.
   
   ![Cisco Webex](./media/active-directory-saas-cisco-webex-tutorial/IC777616.png "Cisco Webex")
   
## <a name="configure-single-sign-on"></a>Konfigurowanie rejestracji jednokrotnej

Celem tej sekcji jest przedstawiają sposób umożliwić użytkownikom uwierzytelnianie na Cisco Webex do swojego konta w usłudze Azure AD przy użyciu federacyjnego na podstawie protokołu SAML.  

W ramach tej procedury jest wymagane do utworzenia zakodowanego certyfikatu base-64. Jeśli nie masz doświadczenia z tej procedury, zobacz [sposób konwertowania binarne certyfikatu do pliku tekstowego](http://youtu.be/PlgrzUZ-Y1o)

**Aby skonfigurować rejestrację jednokrotną, wykonaj następujące czynności:**

1. W klasycznym portalu Azure na **Cisco Webex** strona integracji aplikacji, kliknij przycisk **skonfigurować logowanie jednokrotne** otworzyć **skonfigurować rejestrację jednokrotną** okna dialogowego.
   
   ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-cisco-webex-tutorial/IC777617.png "skonfigurować logowanie jednokrotne")
2. Na **jak chcesz użytkownikom zalogować się na Cisco Webex** wybierz pozycję **usługi Microsoft Azure AD rejestracji jednokrotnej**, a następnie kliknij przycisk **dalej**.
   
   ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-cisco-webex-tutorial/IC777618.png "skonfigurować logowanie jednokrotne")
3. Na **Konfigurowanie adresu URL aplikacji** , wykonaj następujące czynności, a następnie kliknij przycisk **dalej**.
   
   ![Skonfiguruj adres URL aplikacji](./media/active-directory-saas-cisco-webex-tutorial/IC777619.png "skonfigurować adres URL aplikacji")   
   1. W **rejestrują na adres URL** tekstowym, wpisz adres URL dzierżawy Cisco Webex (np.: *http://contoso.webex.com*).
   2. W **adres URL odpowiedzi Webex Cisco** pole tekstowe, typ użytkownika **adres URL AssertionConsumerService Webex Cisco** (np.: *https://company.webex.com/dispatcher/SAML2AuthService?siteurl=company* ).
4. Na **skonfigurować logowanie jednokrotne w Cisco Webex** , aby pobrać certyfikat, kliknij przycisk **pobierania certyfikatu**, a następnie zapisz plik certyfikatu na tym komputerze.
   
   ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-cisco-webex-tutorial/IC777620.png "skonfigurować logowanie jednokrotne")
5. W oknie przeglądarki innej witryny sieci web Zaloguj się do witryny firmy Cisco Webex jako administrator.
6. W menu u góry kliknij **administrowania lokacją**.
   
   ![Administrowanie lokacją](./media/active-directory-saas-cisco-webex-tutorial/IC777621.png "lokacji administracyjnej")
7. W **zarządzania witryną** kliknij **konfiguracji logowania jednokrotnego**.
   
   ![Konfiguracja rejestracji Jednokrotnej](./media/active-directory-saas-cisco-webex-tutorial/IC777622.png "konfiguracji logowania jednokrotnego")
8. W sekcji federacyjnych konfiguracji Usługa rejestracji Jednokrotnej w sieci Web wykonaj następujące czynności:
   
   ![Federacyjna usługa rejestracji Jednokrotnej w konfiguracji](./media/active-directory-saas-cisco-webex-tutorial/IC777623.png "Federacyjna usługa rejestracji Jednokrotnej w konfiguracji")  
   1. Z **protokołu federacyjnego** listy, wybierz **SAML 2.0**.
   2. Utwórz **algorytmem Base-64** pliku z pobranego certyfikatu.  
    >[!TIP]
    >Aby uzyskać więcej informacji, zobacz [sposób konwertowania binarne certyfikatu do pliku tekstowego](http://youtu.be/PlgrzUZ-Y1o)
    >  
   3. Otwórz w Notatniku certyfikatu zakodowanego base-64, a następnie skopiuj zawartość tego.
   4. Kliknij przycisk **Importowanie metadanych SAML**, a następnie wklej certyfikatu zakodowanego base-64.
   5. W klasycznym portalu Azure na **skonfigurować logowanie jednokrotne w Cisco Webex** strony okna dialogowego, kopiowania **adres URL wystawcy** wartość, a następnie wklej ją do **wystawca SAML (identyfikator IdP)** pole tekstowe.
   6. W klasycznym portalu Azure na **skonfigurować logowanie jednokrotne w Cisco Webex** strony okna dialogowego, kopiowania **zdalnego adresu URL logowania** wartość, a następnie wklej ją do **adres URL logowania usługi logowania jednokrotnego klienta** pola tekstowego.
   7. Z **NameID Format** listy, wybierz **adres E-mail**.
   8. W **AuthnContextClassRef** pole tekstowe, typ **urn: oasis: nazwy: tc: SAML:2.0:ac:classes:Password**.
   9. W klasycznym portalu Azure na **skonfigurować logowanie jednokrotne w Cisco Webex** strony okna dialogowego, kopiowania **zdalnego adresu URL wylogowania** wartość, a następnie wklej ją do **adres URL wylogowania usługi logowania jednokrotnego klienta** pola tekstowego.
   10. Kliknij przycisk **aktualizacji**.
9. W klasycznym portalu Azure na **skonfigurować logowanie jednokrotne w Cisco Webex** strony okna dialogowego, wybierz Potwierdzenie konfiguracji rejestracji jednokrotnej, a następnie kliknij **Complete**.
   
   ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-cisco-webex-tutorial/IC777624.png "skonfigurować logowanie jednokrotne")
   
## <a name="configure-user-provisioning"></a>Skonfiguruj Inicjowanie obsługi użytkowników

Aby włączyć użytkowników usługi Azure AD zalogować się do Cisco Webex, musi być przygotowana do Cisco Webex.  

* W przypadku Cisco Webex Inicjowanie obsługi to zadanie ręczne.

**Aby udostępnić konta użytkowników, wykonaj następujące czynności:**

1. Zaloguj się do Twojego **Cisco Webex** dzierżawy.
2. Przejdź do **Zarządzanie użytkownikami \> dodać użytkownika**.
   
   ![Dodawanie użytkowników](./media/active-directory-saas-cisco-webex-tutorial/IC777625.png "Dodawanie użytkowników")
3. W sekcji Dodaj użytkownika wykonaj następujące czynności:
   
   ![Dodaj użytkownika](./media/active-directory-saas-cisco-webex-tutorial/IC777626.png "Dodaj użytkownika")   
   1. Jako **typ konta**, wybierz pozycję **hosta**.
   2. Wpisz informacje istniejącego użytkownika usługi Azure AD do następujących pól tekstowych: **imię, nazwisko**, **nazwy użytkownika**, **E-mail**, **hasło**, **Potwierdź hasło**.
   3. Kliknij pozycję **Dodaj**.

>[!NOTE]
>Możesz użyć innych Cisco Webex użytkownika konta tworzenia narzędzi lub interfejsów API dostarczonych przez Cisco Webex do kont użytkowników usługi AAD. 
> 

## <a name="assign-users"></a>Przypisywanie użytkowników
Aby przetestować konfigurację, musisz przyznać użytkowników usługi Azure AD, czy chcesz zezwolić, używając przypisywania do nich dostęp aplikacji do niego.

**Do przypisywania użytkowników do Cisco Webex, wykonaj następujące czynności:**

1. W klasycznym portalu Azure Utwórz konto testu.
2. Na **Cisco Webex** strona integracji aplikacji, kliknij przycisk **przypisywać użytkowników**.
   
   ![Przypisywanie użytkowników](./media/active-directory-saas-cisco-webex-tutorial/IC777627.png "przypisywanie użytkowników")
3. Wybierz użytkownika testowego, kliknij przycisk **przypisać**, a następnie kliknij przycisk **tak** o potwierdzenie przypisania.
   
   ![Tak](./media/active-directory-saas-cisco-webex-tutorial/IC767830.png "tak")

Jeśli chcesz przetestować jednego ustawienia logowania jednokrotnego, otwórz Panel dostępu. Aby uzyskać więcej informacji na temat panelu dostępu, zobacz [wprowadzenie do panelu dostępu](active-directory-saas-access-panel-introduction.md).

