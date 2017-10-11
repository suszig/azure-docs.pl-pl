---
title: 'Samouczek: Integracji Azure Active Directory z @Task| Dokumentacja firmy Microsoft'
description: "Informacje o sposobie konfigurowania rejestracji jednokrotnej między usługą Azure Active Directory i @Task."
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: aab8bd2f-f9dd-42da-a18e-d707865687d7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/22/2017
ms.author: jeedes
ms.openlocfilehash: ebb19ca6cbaf04106fbce937d95651e709854cfd
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-task"></a>Samouczek: Integracji Azure Active Directory z@Task
Celem tego samouczka jest pokazano, jak zintegrować @Task z usługą Azure Active Directory (Azure AD).  
Integrowanie @Task z usługą Azure AD zapewnia następujące korzyści: 

* Można kontrolować w usłudze Azure AD, kto ma dostęp@Task
* Można umożliwić użytkownikom automatycznie pobrać zalogowane do @Task (logowanie jednokrotne) przy użyciu ich kont usługi Azure AD
* Możesz zarządzać kont w jednej centralnej lokalizacji - klasycznego portalu Azure

Jeśli chcesz dowiedzieć się więcej informacji o integracji aplikacji SaaS w usłudze Azure AD, zobacz [co to jest dostęp do aplikacji i logowanie jednokrotne z usługą Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Wymagania wstępne
Aby skonfigurować integrację usługi Azure AD z @Task, potrzebne są następujące zasoby:

* Subskrypcję usługi Azure AD
* @Task Jednokrotnego włączone subskrypcji

> [!NOTE]
> Aby przetestować kroki opisane w tym samouczku, zaleca się używania środowiska produkcyjnego.
> 
> 

Aby przetestować kroki opisane w tym samouczku, należy wykonać te zalecenia:

* Nie należy używać środowiska produkcyjnego, chyba że jest to konieczne.
* Jeśli nie masz środowisko wersji próbnej usługi Azure AD, możesz pobrać miesięczna wersja próbna [tutaj](https://azure.microsoft.com/pricing/free-trial/). 

## <a name="scenario-description"></a>Opis scenariusza
Celem tego samouczka jest umożliwienie umożliwia testowanie usługi Azure AD rejestracji jednokrotnej w środowisku testowym.  
Scenariusz opisany w tym samouczku składa się z trzech głównych bloków konstrukcyjnych:

1. Dodawanie @Task z galerii 
2. Konfigurowanie i testowanie usługi Azure AD logowanie jednokrotne

## <a name="adding-task-from-the-gallery"></a>Dodawanie @Task z galerii
Aby skonfigurować integrację @Task do usługi Azure AD, należy dodać @Task z galerii do listy zarządzanych aplikacji SaaS.

**Aby dodać @Task z galerii, wykonaj następujące czynności:**

1. W **klasycznego portalu Azure**, w lewym okienku nawigacji, kliknij polecenie **usługi Active Directory**. 
   
    ![Usługa Active Directory][1] 
2. Z **katalogu** listy, wybierz katalog, dla którego chcesz włączyć integracji katalogów.
3. Aby otworzyć widok aplikacji, w widoku katalogu, kliknij przycisk **aplikacji** w menu u góry.
   
    ![Aplikacje][2] 
4. Kliknij przycisk **Dodaj** w dolnej części strony.
   
    ![Aplikacje][3] 
5. Na **co chcesz zrobić** okna dialogowego, kliknij przycisk **dodać aplikację z galerii**.
   
    ![Aplikacje][4] 
6. W polu wyszukiwania wpisz  **@Task** .
   
    ![Aplikacje][5] 
7. W okienku wyników wybierz  **@Task** , a następnie kliknij przycisk **Complete** można dodać aplikację.
   
    ![Aplikacje][30] 

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurowanie i testowanie usługi Azure AD logowanie jednokrotne
Celem tej sekcji jest opisano, jak skonfigurować i przetestować usługi Azure AD rejestracji jednokrotnej z @Task w oparciu o nazwie "Britta Simona" użytkownika testowego.

Dla rejestracji jednokrotnej do pracy, usługi Azure AD musi wiedzieć, jaki użytkownik odpowiednika w @Task użytkownika w usłudze Azure AD jest. Innymi słowy, relacja linku między użytkownika usługi Azure AD i danemu użytkownikowi w @Task należy ustanowić.   
Ta relacja łącza zostanie nawiązane, przypisując wartość **nazwy użytkownika** w usłudze Azure AD jako wartość **Username** w @Task.

Aby skonfigurować i przetestować usługi Azure AD rejestracji jednokrotnej z @Task, należy wykonać poniższe bloki konstrukcyjne:

1. **[Konfigurowanie usługi Azure AD rejestracji jednokrotnej](#configuring-azure-ad-single-single-sign-on)**  — aby umożliwić użytkownikom korzystać z tej funkcji.
2. **[Tworzenie użytkownika testowego usługi Azure AD](#creating-an-azure-ad-test-user)**  — do przetestowania usługi Azure AD rejestracji jednokrotnej z Simona Britta.
3. **[Tworzenie @Tasktest użytkownika](#creating-a-halogen-software-test-user)**  — aby odpowiednikiem Simona Britta w @Taskthat jest połączona z jej reprezentacji usługi Azure AD.
4. **[Przypisanie użytkownika testowego usługi Azure AD](#assigning-the-azure-ad-test-user)**  — aby umożliwić Simona Britta do użycia usługi Azure AD rejestracji jednokrotnej.
5. **[Testowanie rejestracji jednokrotnej](#testing-single-sign-on)**  — Aby sprawdzić, czy konfiguracja działa.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurowanie usługi Azure AD rejestracji jednokrotnej
Celem tej sekcji jest do włączenia usługi Azure AD rejestracji jednokrotnej w klasycznym portalu Azure i konfigurowania rejestracji jednokrotnej w sieci @Task aplikacji.

**Aby skonfigurować usługi Azure AD rejestracji jednokrotnej z @Task, wykonaj następujące czynności:**

1. W klasycznym portalu Azure na  **@Task**  strona integracji aplikacji, kliknij przycisk **skonfigurować logowanie jednokrotne** otworzyć **skonfigurować logowanie jednokrotne** okna dialogowego.
   
    ![Konfigurowanie rejestracji jednokrotnej][6] 
2. Na **jak chcesz użytkownikom logowanie się @Task**  wybierz pozycję **Azure AD rejestracji jednokrotnej**, a następnie kliknij przycisk **dalej**.
   
    ![Azure AD rejestracji jednokrotnej][7] 
3. Na **Konfigurowanie ustawień aplikacji** okna dialogowego strony, należy wykonać następujące czynności:
   
    ![Konfiguruj ustawienia aplikacji][8] 
   
     a. W **na adres URL logowania** tekstowym, wpisz adres URL używany przez użytkowników do logowania jednokrotnego w sieci @Task aplikacji (np.:*https://<Tenant name>.attask ondemand.com*).
   
     b. Kliknij przycisk **Dalej**.
4. Na **skonfigurować logowanie jednokrotne w @Task**  kliknij przycisk **pobierania metadanych**, Zapisz plik metadanych lokalnie na komputerze, a następnie kliknij przycisk **dalej**.
   
    ![Co to jest program Azure AD Connect][9] 
5. Logowanie do Twojej @Task witryny firmy jako administrator.
6. Przejdź do **Rejestracja jednokrotna w konfiguracji**.
7. Na **rejestracji jednokrotnej** okna dialogowego, wykonaj następujące czynności
   
    ![Konfigurowanie rejestracji jednokrotnej][23]
   
    a. Jako **typu**, wybierz pozycję **SAML 2.0**.
   
    b. Wybierz **usługi identyfikator dostawcy**.
   
    c. W klasycznym portalu Azure, skopiuj **zdalnego adresu URL logowania**, a następnie wklej go do **adresu URL logowania do portalu** pola tekstowego.
   
    d. W klasycznym portalu Azure, skopiuj **pojedynczy adres URL usługi Sign-Out**, a następnie wklej go do **Sign-Out adres URL** pola tekstowego.
   
    e. W klasycznym portalu Azure, skopiuj **Zmień adres URL hasła**, a następnie wklej go do **Zmień adres URL hasła** pola tekstowego.
   
    f. Kliknij pozycję **Zapisz**.
8. W klasycznym portalu Azure, wybierz Potwierdzenie konfiguracji rejestracji jednokrotnej, a następnie kliknij przycisk **dalej**. 
   
    ![Co to jest program Azure AD Connect][10]
9. Na **pojedynczy znak na potwierdzenie** kliknij przycisk **Complete**.  
   
    ![Co to jest program Azure AD Connect][11]

### <a name="creating-an-azure-ad-test-user"></a>Tworzenie użytkownika testowego usługi Azure AD
Celem tej sekcji jest tworzenie użytkownika testowego w klasycznym portalu Azure o nazwie Simona Britta.  

![Tworzenie użytkowników usługi Azure AD][20]

**Aby utworzyć użytkownika testowego w usłudze Azure AD, wykonaj następujące czynności:**

1. W **klasycznego portalu Azure**, w lewym okienku nawigacji, kliknij polecenie **usługi Active Directory**.
   
    ![Tworzenie użytkownika testowego usługi Azure AD](./media/active-directory-saas-attask-tutorial/create_aaduser_02.png) 
2. Z **katalogu** listy, wybierz katalog, dla którego chcesz włączyć integracji katalogów.
3. Aby wyświetlić listę użytkowników, w menu u góry, kliknij przycisk **użytkowników**.
   
    ![Tworzenie użytkownika testowego usługi Azure AD](./media/active-directory-saas-attask-tutorial/create_aaduser_03.png) 
4. Aby otworzyć **Dodaj użytkownika** okna dialogowego na pasku narzędzi u dołu, kliknij przycisk **Dodaj użytkownika**. 
   
    ![Tworzenie użytkownika testowego usługi Azure AD](./media/active-directory-saas-attask-tutorial/create_aaduser_04.png) 
5. Na **Poinformuj nas o tym użytkowniku** okna dialogowego strony, należy wykonać następujące czynności: 
   
    ![Tworzenie użytkownika testowego usługi Azure AD](./media/active-directory-saas-attask-tutorial/create_aaduser_05.png) 
   
    a. Jako typ użytkownika wybierz nowego użytkownika w organizacji.
   
    b. W nazwie użytkownika **pole tekstowe**, typ **BrittaSimon**.
   
    c. Kliknij przycisk **Dalej**.
6. Na **profilu użytkownika** okna dialogowego strony, należy wykonać następujące czynności: 
   
    ![Tworzenie użytkownika testowego usługi Azure AD](./media/active-directory-saas-attask-tutorial/create_aaduser_06.png) 
   
    a. W **imię** pole tekstowe, typ **Britta**.  
   
    b. W **nazwisko** pole tekstowe, typ **Simona**.
   
    c. W **Nazwa wyświetlana** pole tekstowe, typ **Simona Britta**.
   
    d. W **roli** listy, wybierz **użytkownika**.

    e. Kliknij przycisk **Dalej**.

7. Na **Uzyskaj hasło tymczasowe** strony okna dialogowego, kliknij przycisk **utworzyć**.
   
    ![Tworzenie użytkownika testowego usługi Azure AD](./media/active-directory-saas-attask-tutorial/create_aaduser_07.png) 
8. Na **Uzyskaj hasło tymczasowe** okna dialogowego strony, należy wykonać następujące czynności:
   
    ![Tworzenie użytkownika testowego usługi Azure AD](./media/active-directory-saas-attask-tutorial/create_aaduser_08.png) 
   
    a. Zanotuj wartość **nowe hasło**.
   
    b. Kliknij przycisk **Complete** (Zakończ).   

### <a name="creating-an-task-test-user"></a>Tworzenie @Task użytkownika testowego
Celem tej sekcji jest utworzenie użytkownika o nazwie Simona Britta w @Task.

**Aby utworzyć użytkownika o nazwie Simona Britta w @Task, wykonaj następujące czynności:**

1. Zaloguj się na Twojej @Task witryny firmy jako administrator.
2. W menu u góry kliknij **osób**.
3. Kliknij przycisk **nowej osoby**. 
4. W oknie dialogowym nowej osoby wykonaj następujące czynności:
   
    ![Utwórz @Task użytkownika testowego][21] 
   
    a. W **imię** tekstowym, wpisz "Britta".
   
    b. W **nazwisko** tekstowym, wpisz "Simona".
   
    c. W **adres E-mail** tekstowym, wpisz adres e-mail Simona Britta w usłudze Azure Active Directory.
   
    d. Kliknij przycisk **Dodaj osobę**.

### <a name="assigning-the-azure-ad-test-user"></a>Przypisanie użytkownika testowego usługi Azure AD
Celem tej sekcji jest włączenie Simona Britta do używania Azure logowania jednokrotnego za udostępnienie jej @Task.

![Przypisz użytkownika][200] 

**Aby przypisać Simona Britta do @Task, wykonaj następujące czynności:**

1. W klasycznym portalu Azure, aby otworzyć widok aplikacji, w widoku katalogu, kliknij polecenie **aplikacji** w menu u góry.
   
    ![Przypisz użytkownika][201] 
2. Na liście aplikacji zaznacz  **@Task** .
   
    ![Przypisz użytkownika][202] 
3. W menu u góry kliknij **użytkowników**.
   
    ![Przypisz użytkownika][203] 
4. Na liście użytkowników wybierz **Simona Britta**.
5. Na pasku narzędzi u dołu, kliknij przycisk **przypisać**.
   
    ![Przypisz użytkownika][205]

### <a name="testing-single-sign-on"></a>Testowanie rejestracji jednokrotnej
Celem tej sekcji służy do testowania konfiguracji usługi Azure AD pojedynczego logowania za pomocą panelu dostępu.  
Po kliknięciu @Task kafelka w panelu dostępu, należy pobrać automatycznie zalogowane do Twojej @Task aplikacji.

## <a name="additional-resources"></a>Dodatkowe zasoby
* [Lista samouczków dotyczących sposobów integracji aplikacji SaaS przy użyciu usługi Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co to jest dostęp do aplikacji i logowanie jednokrotne z usługą Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-attask-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-attask-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-attask-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-attask-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_01.png
[30]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_02.png


[6]: ./media/active-directory-saas-attask-tutorial/tutorial_general_05.png
[7]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_03.png
[8]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_04.png
[9]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_05.png
[10]: ./media/active-directory-saas-attask-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-attask-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-attask-tutorial/tutorial_general_100.png
[21]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_08.png


[23]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_06.png

[200]: ./media/active-directory-saas-attask-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-attask-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_09.png
[203]: ./media/active-directory-saas-attask-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-attask-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-attask-tutorial/tutorial_general_205.png






