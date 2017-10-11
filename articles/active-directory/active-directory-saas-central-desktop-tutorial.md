---
title: "Samouczek: Integracji Azure Active Directory przy użyciu centralnej pulpitu | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak włączyć logowanie jednokrotne, automatyczne Inicjowanie obsługi i inne za pomocą centralnej pulpitu z usługą Azure Active Directory!"
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: b805d485-93db-49b4-807a-18d446c7090e
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/10/2017
ms.author: jeedes
ms.openlocfilehash: fe32c1d68040ceb9d9de2ad6c4a6dc9ea93f5aef
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-central-desktop"></a>Samouczek: Integracji Azure Active Directory przy użyciu centralnej pulpitu
Celem tego samouczka jest pokazanie integracji Azure i pulpitu centralnej. Scenariusz opisany w tym samouczku założono, że już następujące elementy:

* Ważnej subskrypcji platformy Azure
* Centralna pulpitu jednokrotne włączone subskrypcji / dzierżawy centralnej pulpitu

Scenariusz opisany w tym samouczku składa się z następujących bloków konstrukcyjnych:

* Włączanie integracji aplikacji dla komputerów typu Desktop centralnej
* Konfigurowanie rejestracji jednokrotnej (SSO)
* Konfigurowanie Inicjowanie obsługi użytkowników
* Przypisywanie użytkowników

![Scenariusz](./media/active-directory-saas-central-desktop-tutorial/IC769558.png "scenariusza")

## <a name="enable-the-application-integration-for-central-desktop"></a>Włącz integrację aplikacji dla komputerów typu Desktop centralnej
Celem tej sekcji jest przedstawiają sposób włączania integracji aplikacji dla komputerów typu Desktop centralnej.

**Aby włączyć integrację aplikacji dla komputerów typu Desktop centralnej, wykonaj następujące czynności:**

1. W klasycznym portalu Azure, w okienku nawigacji po lewej stronie kliknij **usługi Active Directory**.
   
   ![Usługi Active Directory](./media/active-directory-saas-central-desktop-tutorial/IC700993.png "usługi Active Directory")
2. Z **katalogu** listy, wybierz katalog, dla którego chcesz włączyć integracji katalogów.
3. Aby otworzyć widok aplikacji, w widoku katalogu, kliknij przycisk **aplikacji** w menu u góry.
   
   ![Aplikacje](./media/active-directory-saas-central-desktop-tutorial/IC700994.png "aplikacji")
4. Kliknij przycisk **Dodaj** w dolnej części strony.
   
   ![Dodaj aplikację](./media/active-directory-saas-central-desktop-tutorial/IC749321.png "Dodaj aplikację")
5. Na **co chcesz zrobić** okna dialogowego, kliknij przycisk **dodać aplikację z galerii**.
   
   ![Dodawanie aplikacji z gallerry](./media/active-directory-saas-central-desktop-tutorial/IC749322.png "dodać aplikację z gallerry")
6. W **pole wyszukiwania**, typ **centralnej pulpitu**.
   
   ![Galerii aplikacji](./media/active-directory-saas-central-desktop-tutorial/IC769559.png "galerii aplikacji")
7. W okienku wyników wybierz **centralnej pulpitu**, a następnie kliknij przycisk **Complete** można dodać aplikację.
   
   ![Centralna pulpitu](./media/active-directory-saas-central-desktop-tutorial/IC769560.png "centralnej pulpitu")
   
## <a name="configure-single-sign-on"></a>Konfigurowanie rejestracji jednokrotnej

Celem tej sekcji jest przedstawiają sposób umożliwić użytkownikom uwierzytelnianie na pulpicie centralnej do swojego konta w usłudze Azure AD przy użyciu federacyjnego na podstawie protokołu SAML.

W ramach tej procedury jest wymagane do przekazania do dzierżawy centralnej pulpitu zakodowanego certyfikatu base-64.  
Jeśli nie masz doświadczenia z tej procedury, zobacz [sposób konwertowania binarne certyfikatu do pliku tekstowego](http://youtu.be/PlgrzUZ-Y1o).

**Aby skonfigurować rejestrację jednokrotną, wykonaj następujące czynności:**

1. W klasycznym portalu Azure na **centralnej pulpitu** strona integracji aplikacji, kliknij przycisk **skonfigurować logowanie jednokrotne** otworzyć ** skonfigurować rejestrację jednokrotną ** okna dialogowego.
   
   ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-central-desktop-tutorial/IC749323.png "skonfigurować logowanie jednokrotne")
2. Na **jak chcesz użytkownikom zalogować się na pulpicie centralnej** wybierz pozycję **usługi Microsoft Azure AD rejestracji jednokrotnej**, a następnie kliknij przycisk **dalej**.
   
   ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-central-desktop-tutorial/IC777628.png "skonfigurować logowanie jednokrotne")
3. Na **Konfigurowanie adresu URL aplikacji** , wykonaj następujące czynności, a następnie kliknij przycisk **dalej**: 
   
   1. W **centralnej pulpitu adres URL logowania** tekstowym, wpisz adres URL dzierżawy centralnej pulpitu (np.: *http://contoso.centraldesktop.com*).
   2. W polu tekstowym adres URL odpowiedzi Central pulpitu, wpisz adres URL centralnej AssertionConsumerService pulpitu (np.: https://contoso.centraldesktop.com/saml2-assertion.php).
   
   >[!NOTE]
   >Można pobrać wartości z centralnego pulpitu metadanych (np.: *http://contoso.centraldesktop.com*).
   >  
   
   ![Skonfiguruj adres URL aplikacji](./media/active-directory-saas-central-desktop-tutorial/IC769561.png "skonfigurować adres URL aplikacji")
4. Na **skonfigurować logowanie jednokrotne w centralnej pulpitu** , aby pobrać certyfikat, kliknij przycisk **pobierania certyfikatu**, a następnie zapisz plik certyfikatu na komputerze.
   
  ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-central-desktop-tutorial/IC769562.png "skonfigurować logowanie jednokrotne")
5. Zaloguj się do Twojego **centralnej pulpitu** dzierżawy.
6. Przejdź do **ustawienia**, kliknij przycisk **zaawansowane**, a następnie kliknij przycisk **rejestracji jednokrotnej**.
   
  ![Instalator — zaawansowane](./media/active-directory-saas-central-desktop-tutorial/IC769563.png "Instalator — zaawansowane")
7. Na **pojedynczy znak na ustawienia** wykonaj następujące czynności:
   
  ![Rejestracja jednokrotna ustawień](./media/active-directory-saas-central-desktop-tutorial/IC769564.png "Rejestracja jednokrotna ustawień")
   
  1. Wybierz **Włącz SAML v2 logowania jednokrotnego**.
  2. W klasycznym portalu Azure na **skonfigurować logowanie jednokrotne w centralnej pulpitu** strony, skopiuj **adres URL wystawcy** wartość, a następnie wklej ją do **adres URL logowania jednokrotnego** pola tekstowego.
  3. W klasycznym portalu Azure na **skonfigurować logowanie jednokrotne w centralnej pulpitu** strony, skopiuj **zdalnego adresu URL logowania** wartość, a następnie wklej ją do **adres URL logowania jednokrotnego logowania** pole tekstowe .
  4. W klasycznym portalu Azure na **skonfigurować logowanie jednokrotne w centralnej pulpitu** strony, skopiuj **pojedynczy adres URL usługi Sign-Out** wartość, a następnie wklej ją do **adresu URL wylogowania logowania jednokrotnego**pola tekstowego.
8. W **metody weryfikacji podpisu wiadomości** sekcji, wykonaj następujące czynności:
   
   ![Metoda weryfikacji podpisu wiadomości](./media/active-directory-saas-central-desktop-tutorial/IC769565.png "komunikatu metodę weryfikacji podpisu")
   
  1. Wybierz **certyfikatu**.
  2. Z **certyfikatu logowania jednokrotnego** listy, wybierz **RSH SHA256**.
  3. Utwórz plik tekstowy z pobranego certyfikatu, skopiuj zawartość pliku tekstowego, a następnie wklej go do **certyfikatu logowania jednokrotnego** pola.  
     >[!TIP]
     >Aby uzyskać więcej informacji, zobacz [sposób konwertowania binarne certyfikatu do pliku tekstowego](http://youtu.be/PlgrzUZ-Y1o)
      >  
   4. Wybierz **wyświetlone łącze do strony logowania SAMLv2**.
9. Kliknij przycisk **aktualizacji**.
10. W klasycznym portalu Azure, wybierz Potwierdzenie konfiguracji rejestracji jednokrotnej, a następnie kliknij przycisk **Complete** zamknąć **skonfigurować rejestrację jednokrotną** okna dialogowego.
    
    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-central-desktop-tutorial/IC769566.png "skonfigurować logowanie jednokrotne")
    
## <a name="configure-user-provisioning"></a>Skonfiguruj Inicjowanie obsługi użytkowników

Dla użytkowników usługi AAD można było zarejestrować muszą mieć przydzielone do aplikacji centralnej pulpitu. W tej sekcji opisano sposób tworzenia kont użytkowników usługi AAD w centralnej pulpitu.

**Do obsługi administracyjnej kont użytkowników do centralnej pulpitu:**
1. Zaloguj się do dzierżawy centralnej pulpitu.
2. Przejdź do **osób \> wewnętrzne elementy członkowskie**.
3. Kliknij przycisk **Dodaj wewnętrzne elementy członkowskie**.
   
  ![Osoby](./media/active-directory-saas-central-desktop-tutorial/IC781051.png "osób")
4. W **wiadomości E-mail adres z nowym elementom członkowskim** tekstowym, wpisz konto usługi AAD do udostępniania, a następnie kliknij przycisk **dalej**.
   
  ![Adresy nowe elementy członkowskie e-mail](./media/active-directory-saas-central-desktop-tutorial/IC781052.png "E-mail adresów nowych elementów członkowskich")
5. Kliknij przycisk **Dodaj wewnętrzne elementy członkowskie**.
   
  ![Dodawanie wewnętrznego elementu członkowskiego](./media/active-directory-saas-central-desktop-tutorial/IC781053.png "dodać wewnętrznego elementu członkowskiego")
   
   >[!NOTE]
   >Użytkownicy, dodane otrzymają wiadomość e-mail zawierającą łącze potwierdzenia, które muszą kliknij, aby aktywować konto.
   > 

>[!NOTE]
>Możesz użyć innych centralnej pulpitu użytkownika konta tworzenia narzędzi lub interfejsów API dostarczonych przez centralną pulpitu do kont użytkowników usługi AAD
>  

## <a name="assign-users"></a>Przypisywanie użytkowników
Aby przetestować konfigurację, musisz przyznać użytkowników usługi Azure AD, czy chcesz zezwolić, używając przypisywania do nich dostęp aplikacji do niego.

**Do przypisywania użytkowników do centralnej pulpitu, wykonaj następujące czynności:**

1. W klasycznym portalu Azure Utwórz konto testu.
2. Na **centralnej pulpitu** strona integracji aplikacji, kliknij przycisk **przypisywać użytkowników**.
   
   ![Przypisywanie użytkowników](./media/active-directory-saas-central-desktop-tutorial/IC769567.png "przypisywanie użytkowników")
3. Wybierz użytkownika testowego, kliknij przycisk **przypisać**, a następnie kliknij przycisk **tak** o potwierdzenie przypisania.
   
   ![Tak](./media/active-directory-saas-central-desktop-tutorial/IC767830.png "tak")

Jeśli chcesz przetestować jednego ustawienia logowania jednokrotnego, otwórz Panel dostępu. Aby uzyskać więcej informacji na temat panelu dostępu, zobacz [wprowadzenie do panelu dostępu](active-directory-saas-access-panel-introduction.md).

