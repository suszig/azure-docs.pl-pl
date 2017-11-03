---
title: 'Samouczek: Integracji Azure Active Directory z TOPdesk - bezpiecznego | Dokumentacja firmy Microsoft'
description: "Dowiedz się, jak używać TOPdesk - zabezpieczyć za pomocą usługi Azure Active Directory, aby włączyć logowanie jednokrotne, automatyczne Inicjowanie obsługi i inne!."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 8e149d2d-7849-48ec-9993-31f4ade5fdb4
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/22/2017
ms.author: jeedes
ms.openlocfilehash: 28f0542dbe87bb34c83a7852db7c3a9fef055ce9
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-topdesk---secure"></a>Samouczek: Integracji Azure Active Directory z TOPdesk - bezpieczne
Celem tego samouczka jest pokazanie integracji Azure i TOPdesk - bezpieczny.  
Scenariusz opisany w tym samouczku założono, że już następujące elementy:

* Ważnej subskrypcji platformy Azure
* A TOPdesk - bezpieczne logowanie jednokrotne włączone subskrypcji

Ten samouczek użytkowników usługi Azure AD, zostały przypisane do TOPdesk - bezpiecznego będzie można funkcji logowania jednokrotnego do aplikacji w TOPdesk - firmy bezpiecznej witryny (usługi zainicjował dostawcy logowania) lub przy użyciu [wprowadzenie dostępu Panel](active-directory-saas-access-panel-introduction.md).

Scenariusz opisany w tym samouczku składa się z następujących bloków konstrukcyjnych:

1. Włączanie integracji aplikacji dla TOPdesk - bezpieczne
2. Konfigurowanie rejestracji jednokrotnej
3. Konfigurowanie Inicjowanie obsługi użytkowników
4. Przypisywanie użytkowników

![Scenariusz](./media/active-directory-saas-topdesk-secure-tutorial/IC790596.png "scenariusza")

## <a name="enabling-the-application-integration-for-topdesk---secure"></a>Włączanie integracji aplikacji dla TOPdesk - bezpieczne
Celem tej sekcji jest przedstawiają sposób włączania integracji aplikacji dla TOPdesk - bezpieczny.

### <a name="to-enable-the-application-integration-for-topdesk---secure-perform-the-following-steps"></a>W celu umożliwienia integracji aplikacji programu TOPdesk - bezpieczny, wykonaj następujące czynności:
1. W klasycznym portalu Azure, w okienku nawigacji po lewej stronie kliknij **usługi Active Directory**.
   
    ![Usługi Active Directory](./media/active-directory-saas-topdesk-secure-tutorial/IC700993.png "usługi Active Directory")

2. Z **katalogu** listy, wybierz katalog, dla którego chcesz włączyć integracji katalogów.

3. Aby otworzyć widok aplikacji, w widoku katalogu, kliknij przycisk **aplikacji** w menu u góry.
   
    ![Aplikacje](./media/active-directory-saas-topdesk-secure-tutorial/IC700994.png "aplikacji")

4. Kliknij przycisk **Dodaj** w dolnej części strony.
   
    ![Dodaj aplikację](./media/active-directory-saas-topdesk-secure-tutorial/IC749321.png "Dodaj aplikację")

5. Na **co chcesz zrobić** okna dialogowego, kliknij przycisk **dodać aplikację z galerii**.
   
    ![Dodawanie aplikacji z gallerry](./media/active-directory-saas-topdesk-secure-tutorial/IC749322.png "dodać aplikację z gallerry")

6. W **pole wyszukiwania**, typ **TOPdesk - bezpiecznego**.
   
    ![Galerii aplikacji](./media/active-directory-saas-topdesk-secure-tutorial/IC790597.png "galerii aplikacji")

7. W okienku wyników wybierz **TOPdesk - bezpiecznego**, a następnie kliknij przycisk **Complete** można dodać aplikację.
   
    ![TOPdesk - bezpiecznego](./media/active-directory-saas-topdesk-secure-tutorial/IC791933.png "TOPdesk - bezpieczne")

## <a name="configuring-single-sign-on"></a>Konfigurowanie rejestracji jednokrotnej
Celem tej sekcji jest przedstawiają sposób umożliwić użytkownikom uwierzytelnianie na TOPdesk — zabezpieczyć za pomocą swojego konta w usłudze Azure AD przy użyciu federacyjnego na podstawie protokołu SAML.  
Konfigurowanie rejestracji jednokrotnej dla TOPdesk - bezpiecznego wymaga przekazać plik ikony logo. Aby uzyskać plik ikony, skontaktuj się z zespołem pomocy technicznej TOPdesk.

### <a name="to-configure-single-sign-on-perform-the-following-steps"></a>Aby skonfigurować rejestrację jednokrotną, wykonaj następujące czynności:
1. Zaloguj się na Twojej **TOPdesk - bezpiecznego** witryny firmy jako administrator.
2. W **TOPdesk** menu, kliknij przycisk **ustawienia**.
   
    ![Ustawienia](./media/active-directory-saas-topdesk-secure-tutorial/IC790598.png "ustawienia")

3. Kliknij przycisk **ustawienia logowania**.
   
    ![Ustawienia logowania](./media/active-directory-saas-topdesk-secure-tutorial/IC790599.png "ustawienia logowania")

4. Rozwiń węzeł **ustawienia logowania** menu, a następnie kliknij przycisk **ogólne**.
   
    ![Ogólne](./media/active-directory-saas-topdesk-secure-tutorial/IC790600.png "ogólne")

5. W **bezpiecznego** sekcji **logowania SAML** konfiguracji sekcji, wykonaj następujące czynności:
   
    ![Ustawienia techniczne](./media/active-directory-saas-topdesk-secure-tutorial/IC790855.png "ustawienia techniczne")
   
    a. Kliknij przycisk **Pobierz** można pobrać pliku metadanych publicznego, a następnie zapisz go lokalnie na komputerze.
   
    b. Otwórz plik metadanych, a następnie zlokalizuj **AssertionConsumerService** węzła.
    
    ![Usługa konsumenta potwierdzenia](./media/active-directory-saas-topdesk-secure-tutorial/IC790856.png "potwierdzenia klienta usługi")
   
    c. Kopiuj **AssertionConsumerService** wartości.  
      
    > [!NOTE]
    > Konieczne będzie wartość **Konfigurowanie adresu URL aplikacji** dalszej części tego samouczka.
    > 
    > 

6. W oknie przeglądarki innej witryny sieci web, zaloguj się do Twojego **klasycznego portalu Azure** jako administrator.

7. Na **TOPdesk - bezpiecznego** strona integracji aplikacji, kliknij przycisk **skonfigurować logowanie jednokrotne** otworzyć ** skonfigurować rejestrację jednokrotną ** okna dialogowego.
   
    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-topdesk-secure-tutorial/IC790602.png "skonfigurować logowanie jednokrotne")

8. Na **jak chcesz użytkownikom zalogować się na TOPdesk - bezpiecznego** wybierz pozycję **usługi Microsoft Azure AD rejestracji jednokrotnej**, a następnie kliknij przycisk **dalej**.
   
    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-topdesk-secure-tutorial/IC790603.png "skonfigurować logowanie jednokrotne")

9. Na **Konfigurowanie adresu URL aplikacji** wykonaj następujące czynności:
   
    ![Skonfiguruj adres URL aplikacji](./media/active-directory-saas-topdesk-secure-tutorial/IC790604.png "skonfigurować adres URL aplikacji")
   
    a. W **TOPdesk - bezpiecznego logowania na adres URL** tekstowym, wpisz adres URL używany przez użytkowników do logowania się do sieci TOPdesk - zabezpieczonej aplikacji (np.: "*https://qssolutions.topdesk.net*").
   
    b. W **TOPdesk — publiczny adres URL odpowiedzi** pole tekstowe, Wklej **TOPdesk - bezpiecznego adresu URL AssertionConsumerService** (np.: "*https://qssolutions.topdesk.net/tas/public/login/saml*")
   
    c. Kliknij przycisk **Dalej**.

10. Na **skonfigurować logowanie jednokrotne w TOPdesk - bezpiecznego** można pobrać pliku metadanych, kliknij przycisk **pobierania metadanych**, a następnie zapisz plik lokalnie na komputerze.
    
    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-topdesk-secure-tutorial/IC790605.png "skonfigurować logowanie jednokrotne")

11. Aby utworzyć plik certyfikatu, wykonaj następujące czynności:
    
    ![Certyfikat](./media/active-directory-saas-topdesk-secure-tutorial/IC790606.png "certyfikatu")
    
    a. Otwórz plik pobrany metadanych.
    b. Rozwiń węzeł **RoleDescriptor** węzła, który ma **xsi: type** z **przekazywani: ApplicationServiceType**.
    c. Skopiuj wartość **X509Certificate** węzła.
    d. Zapisz skopiowanych **X509Certificate** wartość lokalnie na komputerze w pliku.

12. W Twojej TOPdesk - zabezpieczenie witryny firmy, w **TOPdesk** menu, kliknij przycisk **ustawienia**.
    
    ![Ustawienia](./media/active-directory-saas-topdesk-secure-tutorial/IC790598.png "ustawienia")

13. Kliknij przycisk **ustawienia logowania**.
    
    ![Ustawienia logowania](./media/active-directory-saas-topdesk-secure-tutorial/IC790599.png "ustawienia logowania")

14. Rozwiń węzeł **ustawienia logowania** menu, a następnie kliknij przycisk **ogólne**.
    
    ![Ogólne](./media/active-directory-saas-topdesk-secure-tutorial/IC790600.png "ogólne")

15. W **publicznego** kliknij **Dodaj**.
    
    ![Dodaj](./media/active-directory-saas-topdesk-secure-tutorial/IC790607.png "Dodaj")

16. Na **Asystenta konfiguracji SAML** okna dialogowego strony, należy wykonać następujące czynności:
    
    ![Asystent konfiguracji SAML](./media/active-directory-saas-topdesk-secure-tutorial/IC790608.png "Asystenta konfiguracji SAML")
    
    a. Aby przesłać plik metadanych pobranych, w obszarze **metadanych Federacji**, kliknij przycisk **Przeglądaj**.

    b. Aby przesłać plik certyfikatu, w obszarze **certyfikatu (RSA)**, kliknij przycisk **Przeglądaj**.

    c. Aby przekazać plik logo uzyskano z zespołem pomocy technicznej TOPdesk, w obszarze **ikona Logo**, kliknij przycisk **Przeglądaj**.

    d. W **atrybutu nazwy użytkownika** pole tekstowe, typ **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.

    e. W **Nazwa wyświetlana** tekstowym, wpisz nazwę dla danej konfiguracji.

    f. Kliknij pozycję **Zapisz**.

17. W klasycznym portalu Azure, wybierz Potwierdzenie konfiguracji rejestracji jednokrotnej, a następnie kliknij przycisk **Complete** zamknąć **skonfigurować rejestrację jednokrotną** okna dialogowego.
    
    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-topdesk-secure-tutorial/IC790609.png "skonfigurować logowanie jednokrotne")

## <a name="configuring-user-provisioning"></a>Konfigurowanie Inicjowanie obsługi użytkowników
Aby włączyć użytkowników usługi Azure AD zalogować się do TOPdesk - bezpieczny, ich muszą mieć przydzielone do TOPdesk - bezpieczny.  
W przypadku TOPdesk - bezpieczny, inicjowanie obsługi to zadanie ręczne.

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>Aby skonfigurować, inicjowanie obsługi użytkowników, wykonaj następujące czynności:
1. Zaloguj się na Twojej **TOPdesk - bezpiecznego** witryny firmy jako administrator.
2. W menu u góry kliknij **TOPdesk \> nowy \> pliki obsługi \> Operator**.
   
    ![Operator](./media/active-directory-saas-topdesk-secure-tutorial/IC790610.png "— Operator")

3. Na **operatora New** okna dialogowego, wykonaj następujące czynności:
   
    ![New Operator](./media/active-directory-saas-topdesk-secure-tutorial/IC790611.png "New — Operator")
   
    a. Kliknij kartę Ogólne.
   
    b. W **nazwisko** pole tekstowe z **ogólne** wpisz nazwisko prawidłowe konto usługi Azure Active Directory, aby udostępnić.
   
    c. Wybierz **lokacji** dla konta w **lokalizacji** sekcji.
   
    d. W **nazwa logowania** pole tekstowe z **logowania TOPdesk** wpisz nazwę logowania dla użytkownika.
   
    e. Kliknij pozycję **Zapisz**.

> [!NOTE]
> Można użyć dowolnego innego TOPdesk — narzędzia do tworzenia konta użytkownika bezpiecznego lub interfejsów API dostarczonych przez TOPdesk - bezpieczny do udostępnienia konta użytkownika usługi AAD.
> 
> 

## <a name="assigning-users"></a>Przypisywanie użytkowników
Aby przetestować konfigurację, musisz przyznać użytkowników usługi Azure AD, czy chcesz zezwolić, używając przypisywania do nich dostęp aplikacji do niego.

### <a name="to-assign-users-to-topdesk---secure-perform-the-following-steps"></a>Do przypisywania użytkowników do TOPdesk - bezpieczny, wykonaj następujące czynności:
1. W klasycznym portalu Azure Utwórz konto testu.
2. Na ** TOPdesk - bezpiecznego ** strona integracji aplikacji, kliknij przycisk **przypisywać użytkowników**.
   
    ![Przypisywanie użytkowników](./media/active-directory-saas-topdesk-secure-tutorial/IC790612.png "przypisywanie użytkowników")

3. Wybierz użytkownika testowego, kliknij przycisk **przypisać**, a następnie kliknij przycisk **tak** o potwierdzenie przypisania.
   
    ![Tak](./media/active-directory-saas-topdesk-secure-tutorial/IC767830.png "tak")

Jeśli chcesz przetestować jednego ustawienia logowania jednokrotnego, otwórz Panel dostępu. Aby uzyskać więcej informacji na temat panelu dostępu, zobacz [wprowadzenie do panelu dostępu](active-directory-saas-access-panel-introduction.md).

