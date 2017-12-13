---
title: 'Samouczek: Integracji Azure Active Directory z Cisco Webex | Dokumentacja firmy Microsoft'
description: "Informacje o sposobie konfigurowania rejestracji jednokrotnej między usługą Azure Active Directory i Cisco Webex."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 26704ca7-13ed-4261-bf24-fd6252e2072b
ms.service: active-directory7
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/08/2017
ms.author: jeedes
ms.openlocfilehash: 835b7c139fc466719489c6fd1986dcbf16de549f
ms.sourcegitcommit: aaba209b9cea87cb983e6f498e7a820616a77471
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/12/2017
---
# <a name="tutorial-azure-active-directory-integration-with-cisco-webex"></a>Samouczek: Integracji Azure Active Directory z Cisco Webex

W tym samouczku Dowiedz się jak zintegrować Cisco Webex w usłudze Azure Active Directory (Azure AD).

Integracja z usługą Azure AD Cisco Webex zapewnia następujące korzyści:

- Można kontrolować w usłudze Azure AD, który ma dostęp do Cisco Webex.
- Umożliwia użytkownikom automatycznie pobrać zalogowane do Webex Cisco (logowanie jednokrotne) z konta usługi Azure AD.
- Możesz zarządzać kont w jednej centralnej lokalizacji - portalu Azure.

Jeśli chcesz dowiedzieć się więcej informacji o integracji aplikacji SaaS w usłudze Azure AD, zobacz [co to jest dostęp do aplikacji i logowanie jednokrotne z usługą Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Wymagania wstępne

Aby skonfigurować integrację usługi Azure AD z Cisco Webex, potrzebne są następujące elementy:

- Subskrypcję usługi Azure AD
- Cisco Webex jednokrotnego włączone subskrypcji

> [!NOTE]
> Aby przetestować kroki opisane w tym samouczku, zaleca się używania środowiska produkcyjnego.

Aby przetestować kroki opisane w tym samouczku, należy wykonać te zalecenia:

- Nie należy używać środowiska produkcyjnego, jeśli jest to konieczne.
- Jeśli nie masz środowisko wersji próbnej usługi Azure AD, możesz [uzyskać miesięczna wersja próbna](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Opis scenariusza
W tym samouczku można przetestować usługę Azure AD rejestracji jednokrotnej w środowisku testowym. Scenariusz opisany w tym samouczku składa się z dwóch głównych elementów:

1. Dodawanie Cisco Webex z galerii
2. Konfigurowanie i testowanie usługi Azure AD logowanie jednokrotne

## <a name="adding-cisco-webex-from-the-gallery"></a>Dodawanie Cisco Webex z galerii
Aby skonfigurować integrację usługi Azure AD Cisco Webex, należy dodać Cisco Webex z galerii do listy zarządzanych aplikacji SaaS.

**Aby dodać Cisco Webex z galerii, wykonaj następujące czynności:**

1. W  **[portalu Azure](https://portal.azure.com)**, na panelu nawigacyjnym po lewej stronie kliknij **usługi Azure Active Directory** ikony. 

    ![Przycisk usługi Azure Active Directory][1]

2. Przejdź do **aplikacje dla przedsiębiorstw**. Następnie przejdź do **wszystkie aplikacje**.

    ![Blok aplikacje przedsiębiorstwa][2]
    
3. Aby dodać nową aplikację, kliknij przycisk **nowej aplikacji** przycisk w górnej części okna dialogowego.

    ![Nowy przycisk aplikacji][3]

4. W polu wyszukiwania wpisz **Cisco Webex**, wybierz pozycję **Cisco Webex** z panelu wyników kliknięcie **Dodaj** przycisk, aby dodać aplikację.

    ![Cisco Webex na liście wyników](./media/active-directory-saas-cisco-webex-tutorial/tutorial_ciscowebex_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfiguracja i testowanie usługi Azure AD rejestracji jednokrotnej

W tej sekcji skonfigurować i przetestować usługi Azure AD rejestracji jednokrotnej z Webex Cisco w oparciu o nazwie "Britta Simona" użytkownika testowego.

Dla rejestracji jednokrotnej do pracy usługi Azure AD musi wiedzieć, użytkownik odpowiednika w Cisco Webex jest dla użytkownika, w usłudze Azure AD. Innymi słowy musi można ustanowić łącze relację między użytkownikiem usługi Azure AD i danemu użytkownikowi w Cisco Webex.

Cisco Webex przypisywanie wartości **nazwy użytkownika** w usłudze Azure AD jako wartość **Username** do ustanawiania relacji łącza.

Aby skonfigurować i przetestować usługi Azure AD rejestracji jednokrotnej z Cisco Webex, należy wykonać poniższe bloki konstrukcyjne:

1. **[Konfigurowanie usługi Azure AD rejestracji jednokrotnej](#configure-azure-ad-single-sign-on)**  — aby umożliwić użytkownikom korzystać z tej funkcji.
2. **[Tworzenie użytkownika testowego usługi Azure AD](#create-an-azure-ad-test-user)**  — do przetestowania usługi Azure AD rejestracji jednokrotnej z Simona Britta.
3. **[Tworzenie użytkownika testowego Cisco Webex](#create-a-cisco-webex-test-user)**  — w celu zapewnienia odpowiednikiem Simona Britta Webex Cisco, połączonej z usługi Azure AD reprezentację użytkownika.
4. **[Przypisz użytkownika testowego usługi Azure AD](#assign-the-azure-ad-test-user)**  — aby umożliwić Simona Britta do użycia usługi Azure AD rejestracji jednokrotnej.
5. **[Test rejestracji jednokrotnej](#test-single-sign-on)**  — Aby sprawdzić, czy konfiguracja działa.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurowanie usługi Azure AD rejestracji jednokrotnej

W tej sekcji można włączyć usługi Azure AD rejestracji jednokrotnej w portalu Azure i skonfigurować logowanie jednokrotne w aplikacji Cisco Webex.

**Aby skonfigurować usługi Azure AD rejestracji jednokrotnej z Cisco Webex, wykonaj następujące czynności:**

1. W portalu Azure na **Cisco Webex** strona integracji aplikacji, kliknij przycisk **logowanie jednokrotne**.

    ![Skonfigurować łącze rejestracji jednokrotnej][4]

2. Na **logowanie jednokrotne** okno dialogowe, wybierz opcję **tryb** jako **na języku SAML logowania jednokrotnego** Aby włączyć logowanie jednokrotne.
 
    ![Okno dialogowe rejestracji jednokrotnej](./media/active-directory-saas-cisco-webex-tutorial/tutorial_ciscowebex_samlbase.png)

3. Na **Cisco Webex domeny i adres URL** sekcji, wykonaj następujące czynności:

    ![Adresy URL i domeny Webex Cisco pojedynczy informacje logowania jednokrotnego](./media/active-directory-saas-cisco-webex-tutorial/tutorial_ciscowebex_url.png)

    a. W **adres URL logowania** tekstowym, wpisz adres URL, używając następującego wzorca:`https://<subdomain>.webex.com`

    b. W **identyfikator** tekstowym, wpisz adres URL:`http://www.webex.com`

    c. W **adres URL odpowiedzi** tekstowym, wpisz adres URL, używając następującego wzorca:`https://company.webex.com/dispatcher/SAML2AuthService?siteurl=company`
     
    > [!NOTE] 
    > Wartości te nie są prawdziwe. Rzeczywisty adres URL odpowiedzi i adres URL logowania, należy zaktualizować te wartości. Skontaktuj się z [zespołem pomocy technicznej klienta Webex Cisco](https://www.webex.co.in/support/support-overview.html) uzyskać te wartości. 

5. Na **certyfikat podpisywania SAML** kliknij **XML metadanych** , a następnie zapisz plik metadanych na tym komputerze.

    ![Łącze pobierania certyfikatu](./media/active-directory-saas-cisco-webex-tutorial/tutorial_ciscowebex_certificate.png) 

6. Kliknij przycisk **zapisać** przycisku.

    ![Skonfiguruj przycisk pojedynczego logowania jednokrotnego Zapisz](./media/active-directory-saas-cisco-webex-tutorial/tutorial_general_400.png)
    
6. Na **Cisco Webex konfiguracji** kliknij **skonfigurować Webex Cisco** otworzyć **Konfigurowanie logowania jednokrotnego** okna. Kopiuj **Sign-Out adres URL, identyfikator jednostki SAML i SAML pojedynczy znak na adres URL usługi** z **sekcji krótkimi opisami.**

    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-cisco-webex-tutorial/tutorial_ciscowebex_configure.png) 

7. W oknie przeglądarki innej witryny sieci web należy zalogować się jako administrator do witryny firmy Cisco Webex.

8. W menu u góry kliknij **administrowania lokacją**.

    ![Administrowanie lokacją](./media/active-directory-saas-cisco-webex-tutorial/ic777621.png "lokacji administracyjnej")

9. W **zarządzania witryną** kliknij **konfiguracji logowania jednokrotnego**.
   
    ![Konfiguracja rejestracji Jednokrotnej](./media/active-directory-saas-cisco-webex-tutorial/ic777622.png "konfiguracji logowania jednokrotnego")

10. W **federacyjnych konfiguracji Usługa rejestracji Jednokrotnej w sieci Web** sekcji, wykonaj następujące czynności:
   
    ![Federacyjna usługa rejestracji Jednokrotnej w konfiguracji](./media/active-directory-saas-cisco-webex-tutorial/ic777623.png "Federacyjna usługa rejestracji Jednokrotnej w konfiguracji")  

    a. Z **protokołu federacyjnego** listy, wybierz **SAML 2.0**.

    b. Jako **profilu rejestracji Jednokrotnej**, wybierz pozycję **SP Intiated**.

    c. Otwórz w Notatniku pobranego certyfikatu, a następnie skopiuj zawartość tego.

    d. Kliknij przycisk **Importowanie metadanych SAML**, a następnie wkleić skopiowanej zawartości certyfikatu.

    e. W **wystawca SAML (identyfikator IdP)** pole tekstowe, Wklej wartość **identyfikator jednostki SAML** którego została skopiowana z portalu Azure.

    f. W **adres URL dla klientów logowania jednokrotnego usługi logowania** pole tekstowe, Wklej **SAML pojedynczy znak na adres URL usługi** którego została skopiowana z portalu Azure.

    g. Z **NameID Format** listy, wybierz **adres E-mail**.

    h. W **AuthnContextClassRef** pole tekstowe, typ **urn: oasis: nazwy: tc: SAML:2.0:ac:classes:Password**.

    i. W **adres URL wylogowania usługi logowania jednokrotnego dla klientów** pole tekstowe, Wklej **Sign-Out URL** którego została skopiowana z portalu Azure.
   
    j. Kliknij przycisk **aktualizacji**.

> [!TIP]
> Teraz możesz przeczytać zwięzły wersji tych instrukcji wewnątrz [portalu Azure](https://portal.azure.com), podczas konfigurowania aplikacji!  Po dodaniu tej aplikacji z **usługi Active Directory > aplikacje dla przedsiębiorstw** po prostu kliknij **rejestracji jednokrotnej** karcie i dostęp do dokumentacji osadzonych za pomocą **konfiguracji** sekcji u dołu. Więcej o funkcji dokumentacji osadzonego w tym miejscu: [dokumentacji osadzonych usługi Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Tworzenie użytkownika testowego usługi Azure AD

Celem tej sekcji jest tworzenie użytkownika testowego w portalu Azure o nazwie Simona Britta.

   ![Tworzenie użytkownika testowego usługi Azure AD][100]

**Aby utworzyć użytkownika testowego w usłudze Azure AD, wykonaj następujące czynności:**

1. W portalu Azure, w okienku po lewej stronie kliknij **usługi Azure Active Directory** przycisku.

    ![Przycisk usługi Azure Active Directory](./media/active-directory-saas-cisco-webex-tutorial/create_aaduser_01.png)

2. Aby wyświetlić listę użytkowników, przejdź do **użytkowników i grup**, a następnie kliknij przycisk **wszyscy użytkownicy**.

    !["Użytkownicy i grupy" i "Wszyscy użytkownicy" łącza](./media/active-directory-saas-cisco-webex-tutorial/create_aaduser_02.png)

3. Aby otworzyć **użytkownika** okno dialogowe, kliknij przycisk **Dodaj** w górnej części **wszyscy użytkownicy** okno dialogowe.

    ![Przycisk Dodaj](./media/active-directory-saas-cisco-webex-tutorial/create_aaduser_03.png)

4. W **użytkownika** okna dialogowego wykonaj następujące czynności:

    ![Okno dialogowe użytkownika](./media/active-directory-saas-cisco-webex-tutorial/create_aaduser_04.png)

    a. W **nazwa** wpisz **BrittaSimon**.

    b. W **nazwy użytkownika** wpisz adres e-mail użytkownika Simona Britta.

    c. Wybierz **Pokaż hasło** pole wyboru, a następnie zanotuj wartość, która jest wyświetlana w **hasło** pole.

    d. Kliknij przycisk **Utwórz**.
 
### <a name="create-a-cisco-webex-test-user"></a>Tworzenie użytkownika testowego Cisco Webex

Aby umożliwić użytkownikom usługi Azure AD zalogować się do Cisco Webex, muszą mieć przydzielone do Cisco Webex. W przypadku Cisco Webex Inicjowanie obsługi to zadanie ręczne.

**Aby udostępnić konta użytkownika, wykonaj następujące czynności:**

1. Zaloguj się do Twojego **Cisco Webex** dzierżawy.

2. Przejdź do **Zarządzanie użytkownikami \> dodać użytkownika**.
   
    ![Dodawanie użytkowników](./media/active-directory-saas-cisco-webex-tutorial/ic777625.png "Dodawanie użytkowników")

3. W sekcji Dodaj użytkownika wykonaj następujące czynności:
   
    ![Dodaj użytkownika](./media/active-directory-saas-cisco-webex-tutorial/ic777626.png "Dodaj użytkownika")   

    a. Jako **typ konta**, wybierz pozycję **hosta**.

    b. W **imię** pole tekstowe, nazwę użytkownika, takich jak Britta typu pierwszy.

    c. W **nazwisko** tekstowym, wpisz nazwisko użytkownika, takich jak Simona.

    d. W **Username** tekstowym, wpisz adres e-mail użytkownika, takich jak Brittasimon@contoso.com.

    e. W **E-mail** tekstowym, wpisz adres e-mail użytkownika, takich jak Brittasimon@contoso.com.

    f. W **hasło** tekstowym, wpisz hasło użytkownika.

    g. W **Potwierdź** pole tekstowe hasła, wprowadź ponownie hasło użytkownika.

    h. Kliknij pozycję **Dodaj**.

>[!NOTE]
>Możesz użyć innych Cisco Webex użytkownika konta tworzenia narzędzi lub interfejsów API dostarczonych przez Cisco Webex do kont użytkowników usługi AAD. 

### <a name="assign-the-azure-ad-test-user"></a>Przypisz użytkownika testowego usługi Azure AD

W tej sekcji można włączyć Simona Britta do używania Azure logowania jednokrotnego za udzielanie dostępu Cisco Webex.

![Przypisanie roli użytkownika][200] 

**Aby przypisać Simona Britta Cisco Webex, wykonaj następujące czynności:**

1. W portalu Azure Otwórz widok aplikacji, a następnie przejdź do widoku katalogu i przejdź do **aplikacje dla przedsiębiorstw** kliknięcie **wszystkie aplikacje**.

    ![Przypisz użytkownika][201] 

2. Na liście aplikacji zaznacz **Cisco Webex**.

    ![Łącze Cisco Webex na liście aplikacji](./media/active-directory-saas-cisco-webex-tutorial/tutorial_ciscowebex_app.png)  

3. W menu po lewej stronie kliknij **użytkowników i grup**.

    ![Łącze "Użytkownicy i grupy"][202]

4. Kliknij przycisk **Dodaj** przycisku. Następnie wybierz **użytkowników i grup** na **Dodaj przydziału** okna dialogowego.

    ![W okienku Dodaj przydziału][203]

5. Na **użytkowników i grup** okno dialogowe, wybierz opcję **Simona Britta** na liście Użytkownicy.

6. Kliknij przycisk **wybierz** znajdującego się na **użytkowników i grup** okna dialogowego.

7. Kliknij przycisk **przypisać** znajdującego się na **Dodaj przydziału** okna dialogowego.
    
### <a name="test-single-sign-on"></a>Test rejestracji jednokrotnej

W tej sekcji można przetestować konfiguracji usługi Azure AD pojedynczego logowania za pomocą panelu dostępu.

Po kliknięciu kafelka Cisco Webex w panelu dostępu użytkownik powinien pobrać automatycznie zalogowane do aplikacji Cisco Webex.
Aby uzyskać więcej informacji na temat panelu dostępu, zobacz [wprowadzenie do panelu dostępu](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Lista samouczków dotyczących sposobów integracji aplikacji SaaS przy użyciu usługi Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co to jest dostęp do aplikacji i logowanie jednokrotne z usługą Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-cisco-webex-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cisco-webex-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cisco-webex-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cisco-webex-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-cisco-webex-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-cisco-webex-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-cisco-webex-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-cisco-webex-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-cisco-webex-tutorial/tutorial_general_203.png

