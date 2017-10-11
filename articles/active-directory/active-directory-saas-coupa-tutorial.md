---
title: 'Samouczek: Integracji Azure Active Directory z Coupa | Dokumentacja firmy Microsoft'
description: "Dowiedz się, jak używać Coupa usłudze Azure Active Directory w celu włączenia logowania jednokrotnego, automatyczne Inicjowanie obsługi i inne!"
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 47f27746-9057-4b9c-991e-3abf77710f73
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/10/2017
ms.author: jeedes
ms.openlocfilehash: c952975919cfd948f1b9ea93ff2ac2641a53f923
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-coupa"></a>Samouczek: Integracji Azure Active Directory z Coupa
Celem tego samouczka jest pokazanie integracji Azure i Coupa.  
Scenariusz opisany w tym samouczku założono, że już następujące elementy:

* Ważnej subskrypcji platformy Azure
* Coupa rejestracji jednokrotnej (SSO) włączone subskrypcji

Ten samouczek użytkowników usługi Azure AD, zostały przypisane do Coupa będzie można funkcji logowania jednokrotnego do aplikacji przy użyciu [wprowadzenie do panelu dostępu](active-directory-saas-access-panel-introduction.md).

Scenariusz opisany w tym samouczku składa się z następujących bloków konstrukcyjnych:

* Włączanie integracji aplikacji dla Coupa
* Konfigurowanie rejestracji jednokrotnej
* Konfigurowanie Inicjowanie obsługi użytkowników
* Przypisywanie użytkowników

![Scenariusz](./media/active-directory-saas-coupa-tutorial/IC791897.png "scenariusza")

## <a name="enable-the-application-integration-for-coupa"></a>Włącz integrację aplikacji dla Coupa
Celem tej sekcji jest przedstawiają sposób włączania integracji aplikacji dla Coupa.

**Aby włączyć integrację aplikacji dla Coupa, wykonaj następujące czynności:**

1. W klasycznym portalu Azure, w okienku nawigacji po lewej stronie kliknij **usługi Active Directory**.
   
   ![Usługi Active Directory](./media/active-directory-saas-coupa-tutorial/IC700993.png "usługi Active Directory")
2. Z **katalogu** listy, wybierz katalog, dla którego chcesz włączyć integracji katalogów.
3. Aby otworzyć widok aplikacji, w widoku katalogu, kliknij przycisk **aplikacji** w menu u góry.
   
   ![Aplikacje](./media/active-directory-saas-coupa-tutorial/IC700994.png "aplikacji")
4. Kliknij przycisk **Dodaj** w dolnej części strony.
   
   ![Dodaj aplikację](./media/active-directory-saas-coupa-tutorial/IC749321.png "Dodaj aplikację")
5. Na **co chcesz zrobić** okna dialogowego, kliknij przycisk **dodać aplikację z galerii**.
   
   ![Dodawanie aplikacji z gallerry](./media/active-directory-saas-coupa-tutorial/IC749322.png "dodać aplikację z gallerry")
6. W **pole wyszukiwania**, typ **Coupa**.
   
   ![Galerii aplikacji](./media/active-directory-saas-coupa-tutorial/IC791898.png "galerii aplikacji")
7. W okienku wyników wybierz **Coupa**, a następnie kliknij przycisk **Complete** można dodać aplikację.
   
   ![Coupa](./media/active-directory-saas-coupa-tutorial/IC791899.png "Coupa")
   
## <a name="configure-single-sign-on"></a>Konfigurowanie rejestracji jednokrotnej

Celem tej sekcji jest przedstawiają sposób umożliwić użytkownikom uwierzytelnianie na Coupa do swojego konta w usłudze Azure AD przy użyciu federacyjnego na podstawie protokołu SAML.  

Konfigurowanie rejestracji jednokrotnej dla Coupa należy pobrać wartość odcisku palca certyfikatu. Jeśli nie masz doświadczenia z tej procedury, zobacz [jak pobrać wartość odcisku palca certyfikatu](http://youtu.be/YKQF266SAxI).

**Aby skonfigurować rejestrację jednokrotną, wykonaj następujące czynności:**

1. Zalogować się do witryny firmy Coupa jako administrator.
2. Przejdź do **Instalator \> zabezpieczeniem**.
   
   ![Opcje zabezpieczeń](./media/active-directory-saas-coupa-tutorial/IC791900.png "środki zabezpieczające.")
3. Aby pobrać pliku metadanych Coupa do komputera, kliknij przycisk **pobieranie i Importowanie metadanych SP**.
   
   ![Metadane Coupa SP](./media/active-directory-saas-coupa-tutorial/IC791901.png "Coupa SP metadanych")
4. W oknie innej przeglądarki należy zalogować się do klasycznego portalu Azure.
5. Na **Coupa** strona integracji aplikacji, kliknij przycisk **skonfigurować logowanie jednokrotne** otworzyć **skonfigurować rejestrację jednokrotną** okna dialogowego.
   
   ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-coupa-tutorial/IC791902.png "skonfigurować logowanie jednokrotne")
6. Na **jak chcesz użytkownikom zalogować się na Coupa** wybierz pozycję **usługi Microsoft Azure AD rejestracji jednokrotnej**, a następnie kliknij przycisk **dalej**.
   
   ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-coupa-tutorial/IC791903.png "skonfigurować logowanie jednokrotne")
7. Na **Konfigurowanie adresu URL aplikacji** wykonaj następujące czynności:
   
   ![Skonfiguruj adres URL aplikacji](./media/active-directory-saas-coupa-tutorial/IC791904.png "skonfigurować adres URL aplikacji")   
   1. W **na adres URL logowania** pole tekstowe, wprowadź adres URL używany przez użytkowników do logowania do aplikacji Coupa (np.: "*http://company.Coupa.com*").
   2. Otwórz pobrany plik metadanych Coupa, a następnie skopiuj **AssertionConsumerService indeksu lub adres URL**.
   3. W **adres URL odpowiedzi Coupa** pole tekstowe, Wklej **AssertionConsumerService indeksu lub adres URL** wartość.
   4. Kliknij przycisk **Dalej**.
8. Na **skonfigurować logowanie jednokrotne w Coupa** można pobrać pliku metadanych, kliknij przycisk **pobierania metadanych**, a następnie zapisz plik lokalnie na komputerze.
   
   ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-coupa-tutorial/IC791905.png "skonfigurować logowanie jednokrotne")
9. W witrynie firmy Coupa, przejdź do **Instalator \> zabezpieczeniem**.
   
   ![Opcje zabezpieczeń](./media/active-directory-saas-coupa-tutorial/IC791900.png "środki zabezpieczające.")
10. W **Zaloguj się za pomocą poświadczeń Coupa** sekcji, wykonaj następujące czynności:  

   ![Zaloguj się za pomocą poświadczeń Coupa](./media/active-directory-saas-coupa-tutorial/IC791906.png "Zaloguj się za pomocą poświadczeń Coupa") 
   1. Wybierz **zalogowanie się przy użyciu SAML**.
   2. Kliknij przycisk **Przeglądaj** przekazać pobranego pliku metadanych usługi Azure Active.
   3. Kliknij pozycję **Zapisz**.
11. W klasycznym portalu Azure, wybierz Potwierdzenie konfiguracji rejestracji jednokrotnej, a następnie kliknij przycisk **Complete** zamknąć **skonfigurować rejestrację jednokrotną** okna dialogowego.
    
   ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-coupa-tutorial/IC791907.png "skonfigurować logowanie jednokrotne")
    
## <a name="configure-user-provisioning"></a>Skonfiguruj Inicjowanie obsługi użytkowników

Aby włączyć użytkowników usługi Azure AD zalogować się do Coupa, musi być przygotowana do Coupa.  

* W przypadku Coupa Inicjowanie obsługi to zadanie ręczne.

**Aby skonfigurować, inicjowanie obsługi użytkowników, wykonaj następujące czynności:**

1. Zaloguj się do Twojego **Coupa** witryny firmy jako administrator.
2. W menu u góry kliknij **Instalator**, a następnie kliknij przycisk **użytkowników**.
   
   ![Użytkownicy](./media/active-directory-saas-coupa-tutorial/IC791908.png "użytkowników")
3. Kliknij przycisk **Utwórz**.
   
   ![Tworzenie użytkowników](./media/active-directory-saas-coupa-tutorial/IC791909.png "tworzenie użytkowników")
4. W **Utwórz użytkownika** sekcji, wykonaj następujące czynności:
   
   ![Szczegóły użytkownika](./media/active-directory-saas-coupa-tutorial/IC791910.png "szczegóły użytkownika")
   
   1. Typ **logowania**, **imię**, **nazwisko**, **identyfikator rejestracji jednokrotnej**, **E-mail** atrybutów prawidłowe konto usługi Azure Active Directory, które chcesz udostępnić do powiązanych pól tekstowych.
   2. Kliknij przycisk **Utwórz**.   
   >[!NOTE]
   >Właściciel konta usługi Azure Active Directory otrzyma wiadomość e-mail z łączem do potwierdzenia konta, zanim staje się aktywny. 
   > 

>[!NOTE]
>Możesz użyć innych Coupa użytkownika konta tworzenia narzędzi lub interfejsów API dostarczonych przez Coupa do kont użytkowników usługi AAD. 
> 

## <a name="assign-users"></a>Przypisywanie użytkowników
Aby przetestować konfigurację, musisz przyznać użytkowników usługi Azure AD, czy chcesz zezwolić, używając przypisywania do nich dostęp aplikacji do niego.

**Do przypisywania użytkowników do Coupa, wykonaj następujące czynności:**

1. W klasycznym portalu Azure Utwórz konto testu.
2. Na ** Coupa ** strona integracji aplikacji, kliknij przycisk **przypisywać użytkowników**.
   
   ![Przypisywanie użytkowników](./media/active-directory-saas-coupa-tutorial/IC791911.png "przypisywanie użytkowników")
3. Wybierz użytkownika testowego, kliknij przycisk **przypisać**, a następnie kliknij przycisk **tak** o potwierdzenie przypisania.
   
   ![Tak](./media/active-directory-saas-coupa-tutorial/IC767830.png "tak")

Jeśli chcesz przetestować jednego ustawienia logowania jednokrotnego, otwórz Panel dostępu. Aby uzyskać więcej informacji na temat panelu dostępu, zobacz [wprowadzenie do panelu dostępu](active-directory-saas-access-panel-introduction.md).

