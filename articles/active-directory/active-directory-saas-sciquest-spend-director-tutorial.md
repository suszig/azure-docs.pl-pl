---
title: "Samouczek: Integracji Azure Active Directory z SciQuest spędzają na katalog | Dokumentacja firmy Microsoft"
description: "Informacje o sposobie konfigurowania rejestracji jednokrotnej między usługą Azure Active Directory i SciQuest spędzają na katalog."
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: 9fab641b-292e-4bef-91d1-8ccc4f3a0c1f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/17/2017
ms.author: jeedes
ms.openlocfilehash: 84b707668dc45e92e6151f422f1c919f638533b1
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sciquest-spend-director"></a>Samouczek: Integracji Azure Active Directory z SciQuest spędzają na katalog
Celem tego samouczka jest pokazanie sposobu integracji SciQuest spędzają na katalog w usłudze Azure Active Directory (Azure AD).  
Integracja z usługą Azure AD SciQuest spędzają na katalog zapewnia następujące korzyści: 

* Można kontrolować w usłudze Azure AD, który ma dostęp do SciQuest spędzają na katalog 
* Umożliwia użytkownikom automatycznie pobrać zalogowane SciQuest spędzają dyrektora (logowanie jednokrotne) z konta usługi Azure AD
* Możesz zarządzać kont w jednej centralnej lokalizacji - klasycznego portalu Azure

Jeśli chcesz dowiedzieć się więcej informacji o integracji aplikacji SaaS w usłudze Azure AD, zobacz [co to jest dostęp do aplikacji i logowanie jednokrotne z usługą Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Wymagania wstępne
Aby skonfigurować integrację usługi Azure AD z SciQuest spędzają na katalog, potrzebne są następujące elementy:

* Subskrypcję usługi Azure AD
* Dyrektor spędzają SciQuest jednokrotnego włączone subskrypcji

> [!NOTE]
> Aby przetestować kroki opisane w tym samouczku, zaleca się używania środowiska produkcyjnego.
> 
> 

Aby przetestować kroki opisane w tym samouczku, należy wykonać te zalecenia:

* Nie należy używać środowiska produkcyjnego, chyba że jest to konieczne.
* Jeśli nie masz środowisko wersji próbnej usługi Azure AD, możesz pobrać miesięczna wersja próbna [tutaj](https://azure.microsoft.com/pricing/free-trial/). 

## <a name="scenario-description"></a>Opis scenariusza
Celem tego samouczka jest umożliwienie umożliwia testowanie usługi Azure AD rejestracji jednokrotnej w środowisku testowym.  
Scenariusz opisany w tym samouczku składa się z dwóch głównych elementów:

1. Dodawanie SciQuest spędzają na katalog z galerii 
2. Konfigurowanie i testowanie usługi Azure AD logowanie jednokrotne

## <a name="adding-sciquest-spend-director-from-the-gallery"></a>Dodawanie SciQuest spędzają na katalog z galerii
Aby skonfigurować integrację usługi Azure AD SciQuest spędzają na katalog, należy dodać SciQuest spędzają na katalog z galerii do listy zarządzanych aplikacji SaaS.

**Aby dodać SciQuest spędzają na katalog z galerii, wykonaj następujące czynności:**

1. W **klasycznego portalu Azure**, w lewym okienku nawigacji, kliknij polecenie **usługi Active Directory**. 
   
    ![Usługa Active Directory][1]

2. Z **katalogu** listy, wybierz katalog, dla którego chcesz włączyć integracji katalogów.

3. Aby otworzyć widok aplikacji, w widoku katalogu, kliknij przycisk **aplikacji** w menu u góry.
   
    ![Aplikacje][2]

4. Kliknij przycisk **Dodaj** w dolnej części strony.
   
    ![Aplikacje][3]

5. Na **co chcesz zrobić** okna dialogowego, kliknij przycisk **dodać aplikację z galerii**.
   
    ![Aplikacje][4]

6. W polu wyszukiwania wpisz **sciQuest spędzają na katalog**.
   
    ![Aplikacje][5]

7. W okienku wyników wybierz **SciQuest spędzają na katalog**, a następnie kliknij przycisk **Complete** można dodać aplikację.
   
    ![Aplikacje][6]

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurowanie i testowanie usługi Azure AD logowanie jednokrotne
Jest celem tej sekcji opisano, jak skonfigurować i przetestować usługi Azure AD rejestracji jednokrotnej z SciQuest Dyrektor spędzają na podstawie użytkownika testowego, nazywany "Britta Simona".

Dla rejestracji jednokrotnej do pracy usługi Azure AD musi wiedzieć, co to jest odpowiednikiem użytkownika w SciQuest spędzają na katalog do użytkownika w usłudze Azure AD. Innymi słowy musi można ustanowić łącze relację między użytkownikiem usługi Azure AD i danemu użytkownikowi w SciQuest spędzają na katalog.  
Ta relacja łącza zostanie nawiązane, przypisując wartość **nazwy użytkownika** w usłudze Azure AD jako wartość **Username** w SciQuest spędzają na katalog.

Aby skonfigurować i przetestować usługi Azure AD rejestracji jednokrotnej z SciQuest spędzają na katalog, należy wykonać poniższe bloki konstrukcyjne:

1. **[Konfigurowanie usługi Azure AD pojedynczego rejestracji jednokrotnej](#configuring-azure-ad-single-single-sign-on)**  — aby umożliwić użytkownikom korzystać z tej funkcji.
2. **[Tworzenie użytkownika testowego usługi Azure AD](#creating-an-azure-ad-test-user)**  — do przetestowania usługi Azure AD rejestracji jednokrotnej z Simona Britta.
3. **[Tworzenie użytkownika testowego SciQuest spędzają na katalog](#creating-a-halogen-software-test-user)**  — w celu zapewnienia odpowiednikiem Simona Britta spędzają na katalog SciQuest, połączonej z jej reprezentacji usługi Azure AD.
4. **[Przypisanie użytkownika testowego usługi Azure AD](#assigning-the-azure-ad-test-user)**  — aby umożliwić Simona Britta do użycia usługi Azure AD rejestracji jednokrotnej.
5. **[Testowanie rejestracji jednokrotnej](#testing-single-sign-on)**  — Aby sprawdzić, czy konfiguracja działa.

### <a name="configuring-azure-ad-single-single-sign-on"></a>Konfigurowanie usługi Azure AD pojedynczej rejestracji jednokrotnej
Celem tej sekcji jest można włączyć usługi Azure AD rejestracji jednokrotnej w klasycznym portalu Azure i skonfigurować logowanie jednokrotne w SciQuest spędzają na katalog aplikacji.

**Aby skonfigurować usługi Azure AD rejestracji jednokrotnej z SciQuest spędzają na katalog, wykonaj następujące czynności:**

1. W klasycznym portalu Azure na **SciQuest spędzają na katalog** strona integracji aplikacji, kliknij przycisk **skonfigurować logowanie jednokrotne** otworzyć **skonfigurować logowanie jednokrotne**  okno dialogowe.
   
    ![Konfigurowanie rejestracji jednokrotnej][8]

2. Na **jak chcesz użytkownikom zalogować się na SciQuest spędzają na katalog** wybierz pozycję **Azure AD rejestracji jednokrotnej**, a następnie kliknij przycisk **dalej**.
   
    ![Azure AD rejestracji jednokrotnej][9]

3. Na **Konfigurowanie ustawień aplikacji** okna dialogowego strony, należy wykonać następujące czynności: 
   
    ![Konfiguruj ustawienia aplikacji][10]
   
     a. W **na adres URL logowania** tekstowym, wpisz adres URL używany przez użytkowników do logowania się SciQuest spędzają na katalog aplikacji przy użyciu następującego wzorca: *https://.* SciQuest.com/.**
   
     b. W **adres URL odpowiedzi** pole tekstowe, należy wpisać taką samą wartość wpisana do **na adres URL logowania** pola tekstowego. 
   
     c. Kliknij przycisk **Dalej**.

4. Na **skonfigurować logowanie jednokrotne w SciQuest spędzają na katalog** kliknij przycisk **pobierania metadanych**, a następnie zapisz plik metadanych lokalnie na komputerze.
   
    ![Co to jest program Azure AD Connect][11]

5. Skontaktuj się z pomocą SciQuest obsługuje Aby włączyć tę metodę uwierzytelniania przy użyciu metadanych pobranych powyżej.

6. W klasycznym portalu Azure, wybierz Potwierdzenie konfiguracji rejestracji jednokrotnej, a następnie kliknij przycisk **Complete** zamknąć **skonfigurować rejestrację jednokrotną** okna dialogowego. 
   
    ![Co to jest program Azure AD Connect][15]

7. Na **pojedynczy znak na potwierdzenie** kliknij przycisk **Complete**.  

### <a name="creating-an-azure-ad-test-user"></a>Tworzenie użytkownika testowego usługi Azure AD
Celem tej sekcji jest tworzenie użytkownika testowego w klasycznym portalu Azure o nazwie Simona Britta.

**Aby utworzyć użytkownika testowego w usłudze Azure AD, wykonaj następujące czynności:**

1. W **klasycznego portalu Azure**, w lewym okienku nawigacji, kliknij polecenie **usługi Active Directory**.
   
    ![Co to jest program Azure AD Connect][100] 

2. Z **katalogu** listy, wybierz katalog, dla którego chcesz włączyć integracji katalogów.

3. Aby wyświetlić listę użytkowników, w menu u góry, kliknij przycisk **użytkowników**.
   
    ![Co to jest program Azure AD Connect][101] 

4. Aby otworzyć **Dodaj użytkownika** okna dialogowego na pasku narzędzi u dołu, kliknij przycisk **Dodaj użytkownika**. 
   
    ![Co to jest program Azure AD Connect][102] 

5. Na **Poinformuj nas o tym użytkowniku** okna dialogowego strony, należy wykonać następujące czynności:
   
    ![Co to jest program Azure AD Connect][103] 
   
    a. Jako **typ użytkownika**, wybierz pozycję **nowy użytkownik w organizacji**.
   
    b. W nazwie użytkownika **pole tekstowe**, typ **BrittaSimon**.
   
    c. Kliknij przycisk **Dalej**.

6. Na **profilu użytkownika** okna dialogowego strony, należy wykonać następujące czynności: 
   
    ![Co to jest program Azure AD Connect][104] 
   
    a. W **imię** pole tekstowe, typ **Britta**.  
   
    b. W **nazwisko** txtbox, typ **Simona**.
   
    c. W **Nazwa wyświetlana** pole tekstowe, typ **Simona Britta**.
   
    d. W **roli** listy, wybierz **użytkownika**.
   
    e. Kliknij przycisk **Dalej**.

7. Na **Uzyskaj hasło tymczasowe** strony okna dialogowego, kliknij przycisk **utworzyć**.
   
    ![Co to jest program Azure AD Connect][105]  

8. Na **Uzyskaj hasło tymczasowe** okna dialogowego strony, należy wykonać następujące czynności:
   
    ![Co to jest program Azure AD Connect][106]   
   
    a. Zanotuj wartość **nowe hasło**.
   
    b. Kliknij przycisk **Complete** (Zakończ).   

### <a name="creating-a-sciquest-spend-director-test-user"></a>Tworzenie użytkownika testowego SciQuest spędzają na katalog
Celem tej sekcji jest utworzenie użytkownika o nazwie Simona Britta w SciQuest spędzają na katalog.

Należy skontaktować się z zespołem pomocy technicznej SciQuest spędzają na katalog i dostarczać szczegółowe informacje o Twoim koncie testu, aby został utworzony.

Alternatywnie można też skorzystać w czasie inicjowania obsługi administracyjnej, pojedynczego logowania jednokrotnego funkcja, która jest obsługiwana przez SciQuest spędzają na katalog.  
Po włączeniu w czasie inicjowania obsługi użytkowników są tworzone automatycznie przez SciQuest spędzają na katalog podczas jednego próba logowania jednokrotnego, jeśli nie istnieją. Ta funkcja eliminuje potrzebę ręcznie utworzyć użytkowników odpowiednikiem rejestracji jednokrotnej.

Można uzyskać w czasie inicjowania obsługi administracyjnej włączone, musisz skontaktować się z korzystania z zespołem pomocy technicznej SciQuest spędzają na katalog.

### <a name="assigning-the-azure-ad-test-user"></a>Przypisanie użytkownika testowego usługi Azure AD
Celem tej sekcji jest włączenie Simona Britta do używania Azure logowania jednokrotnego za udzielanie dostępu jej SciQuest spędzają na katalog.

![Co to jest program Azure AD Connect][200]

**Aby przypisać Simona Britta SciQuest spędzają na katalog, wykonaj następujące czynności:**

1. W klasycznym portalu Azure, aby otworzyć widok aplikacji, w widoku katalogu, kliknij polecenie **aplikacji** w menu u góry.
   
    ![Co to jest program Azure AD Connect][201]

2. Na liście aplikacji zaznacz **SciQuest spędzają na katalog**.
   
    ![Co to jest program Azure AD Connect][202]

3. W menu u góry kliknij **użytkowników**.
   
    ![Co to jest program Azure AD Connect][203]

4. Na liście użytkowników wybierz **Simona Britta**.
   
    ![Co to jest program Azure AD Connect][204]

5. Na pasku narzędzi u dołu, kliknij przycisk **przypisać**.
   
    ![Co to jest program Azure AD Connect][205]

### <a name="testing-single-sign-on"></a>Testowanie rejestracji jednokrotnej
Celem tej sekcji służy do testowania konfiguracji usługi Azure AD pojedynczego logowania za pomocą panelu dostępu.  
Po kliknięciu kafelka SciQuest spędzają na katalog, w panelu dostępu użytkownik powinien pobrać automatycznie zalogowane SciQuest spędzają na katalog aplikacji.

## <a name="additional-resources"></a>Dodatkowe zasoby
* [Lista samouczków dotyczących sposobów integracji aplikacji SaaS przy użyciu usługi Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co to jest dostęp do aplikacji i logowanie jednokrotne z usługą Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->
[1]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_sciquest_spend_director_01.png
[6]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_sciquest_spend_director_05.png
[8]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_06.png
[9]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_07.png
[10]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_08.png
[11]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_sciquest_spend_director_03.png
[15]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_sciquest_spend_director_04.png

[100]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_09.png 
[101]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_10.png 
[102]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_11.png 
[103]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_12.png 
[104]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_13.png 
[105]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_14.png 
[106]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_15.png 
[200]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_16.png 
[201]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_17.png 
[202]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_sciquest_spend_director_06.png
[203]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_18.png
[204]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_19.png
[205]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_20.png

