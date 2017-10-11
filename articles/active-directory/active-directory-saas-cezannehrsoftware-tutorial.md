---
title: 'Samouczek: Azure Active Directory integracji z oprogramowaniem Cezanne HR | Dokumentacja firmy Microsoft'
description: "Informacje o sposobie konfigurowania rejestracji jednokrotnej między oprogramowaniem Cezanne HR i Azure Active Directory."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: fb8f95bf-c3c1-4e1f-b2b3-3b67526be72d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: jeedes
ms.openlocfilehash: 623c438edfce5f98c2d32d8bb25a97d86aa77909
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/18/2017
---
# <a name="tutorial-integrate-azure-active-directory-with-cezanne-hr-software"></a>Samouczek: Integrowanie usługi Azure Active Directory z oprogramowaniem Cezanne HR

Z tego samouczka dowiesz się integrowanie Cezanne HR oprogramowania z usługi Azure Active Directory (Azure AD).

Integracja oprogramowania Cezanne HR z usługą Azure AD zapewnia następujące korzyści. Możesz:

- Kontrolowanie w usłudze Azure AD, który ma dostęp do oprogramowania Cezanne HR.
- Umożliwianie użytkownikom automatycznie logować się do oprogramowania Cezanne HR z logowaniem jednokrotnym (SSO) przy użyciu ich kont usługi Azure AD.
- Zarządzanie kont w jednej centralnej lokalizacji: portalu Azure.

Aby dowiedzieć się więcej na temat oprogramowania jako usługa (SaaS) integracji aplikacji z usługą Azure AD, zobacz [co to jest dostęp do aplikacji i logowania jednokrotnego w usłudze Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Wymagania wstępne

Aby skonfigurować integrację usługi Azure AD z oprogramowaniem Cezanne HR, potrzebne są następujące elementy:

- Subskrypcję usługi Azure AD
- Logowanie Jednokrotne włączone subskrypcji oprogramowania Cezanne HR

> [!NOTE]
> Do testowania czynności w tym samouczku, firma Microsoft zaleca, nie używaj do środowiska produkcyjnego.

Aby przetestować kroki opisane w tym samouczku, wykonaj te zalecenia:

- Nie należy używać środowiska produkcyjnego, jeśli jest to konieczne.
- Jeśli nie masz środowisko wersji próbnej usługi Azure AD, możesz [uzyskać miesięczna wersja próbna](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Opis scenariusza
W tym samouczku można przetestować logowania jednokrotnego programu Azure AD w środowisku testowym. 

Scenariusz opisany w tym samouczku składa się z dwóch głównych elementów:

* Dodawanie oprogramowania Cezanne HR z galerii
* Konfigurowanie i testowania logowania jednokrotnego programu Azure AD

## <a name="add-cezanne-hr-software-from-the-gallery"></a>Dodawanie oprogramowania Cezanne HR z galerii
Aby skonfigurować integrację usługi Azure AD Cezanne HR oprogramowania, należy dodać Cezanne HR oprogramowania z galerii do listy zarządzanych aplikacji SaaS.

Aby dodać Cezanne HR oprogramowanie z poziomu galerii, wykonaj następujące czynności:

1. W  **[portalu Azure](https://portal.azure.com)**, w okienku po lewej stronie wybierz **usługi Azure Active Directory** przycisku. 

    ![Przycisk "Azure Active Directory"][1]

2. Wybierz **aplikacje dla przedsiębiorstw** > **wszystkie aplikacje**.

    ![Łącze "Wszystkie aplikacje"][2]
    
3. Aby dodać nową aplikację, w górnej części **wszystkie aplikacje** okno dialogowe, wybierz opcję **nowej aplikacji**.

    !["Nowa aplikacja" przycisku][3]

4. W polu wyszukiwania wpisz **Cezanne HR oprogramowania**.

    ![Pole wyszukiwania](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_search.png)

5. Na liście wyników wybierz **oprogramowania HR Cezanne** , a następnie wybierz **Dodaj** przycisk, aby dodać aplikację.

    ![Lista wyników](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfiguracja i testowanie usługi Azure AD rejestracji jednokrotnej
W tej sekcji możesz skonfigurować i przetestować logowania jednokrotnego programu Azure AD z oprogramowaniem Cezanne HR oparte na koncie użytkownika testu o nazwie "Britta Simona".

Aby logowania jednokrotnego do pracy usługi Azure AD musi znać odpowiednikiem oprogramowania Cezanne HR użytkownika usługi Azure AD. Innymi słowy musisz ustanowić łącze relację między użytkownikiem usługi Azure AD i danemu użytkownikowi w oprogramowaniu Cezanne HR.

Ustanowienie relacji łącze przypisywania oprogramowania Cezanne HR **nazwy użytkownika** wartość, jak usługi Azure AD **Username** wartość.

Aby skonfigurować i test rejestracji Jednokrotnej programu Azure AD przy użyciu oprogramowania Cezanne HR, wykonaj poniższe bloki konstrukcyjne.

### <a name="configure-azure-ad-sso"></a>Konfigurowanie usługi Azure AD logowania jednokrotnego.

W tej sekcji możesz włączenia funkcji logowania jednokrotnego usługi Azure AD w portalu Azure i konfigurowanie logowania jednokrotnego do aplikacji oprogramowania Cezanne HR w następujący sposób:

1. W portalu Azure na **oprogramowania HR Cezanne** strona integracji aplikacji, wybierz opcję **logowanie jednokrotne**.

    ![Polecenie "Logowanie jednokrotne"][4]

2. Aby włączyć logowanie Jednokrotne, w **logowanie jednokrotne** okno dialogowe, wybierz opcję **tryb** jako **na języku SAML logowania jednokrotnego**.
 
    ![Pole "Tryb"](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_samlbase.png)

3. W obszarze **Cezanne HR oprogramowania domeny i adres URL**, wykonaj następujące czynności:

    ![W sekcji "Cezanne HR oprogramowania domeny i adresy URL"](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_url.png)

    a. W **adres URL logowania** wpisz adres URL, który ma następującą składnię:`https://w3.cezanneondemand.com/cezannehr/-/<tenant id>`

    b. W **adres URL odpowiedzi** wpisz adres URL, który ma następującą składnię:`https://w3.cezanneondemand.com:443/<tenantid>`    
     
    > [!NOTE] 
    > Poprzednie wartości nie są prawdziwe. Zaktualizuj je z odpowiedzi rzeczywisty adres URL i adresu URL logowania jednokrotnego. Aby uzyskać wartości, skontaktuj się z [Cezanne HR oprogramowanie klienta z pomocą techniczną](mailto:info@cezannehr.com).

4. W obszarze **SAML certyfikat podpisywania**, wybierz pozycję **certyfikatu (Base64)**, a następnie zapisz plik certyfikatu na komputerze.

    ![W sekcji "Certyfikat podpisywania SAML"](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_certificate.png) 

5. Wybierz pozycję **Zapisz**.

    ![Przycisk "Zapisz"](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_400.png)
    
6. W obszarze **konfiguracji oprogramowania HR Cezanne**, wybierz pozycję **Konfigurowanie oprogramowania HR Cezanne** otworzyć **Konfigurowanie logowania jednokrotnego** okna. Kopiuj **SAML identyfikator jednostki** i **SAML logowania jednokrotnego usługi pojedynczej** adresu URL z **krótki przewodnik** sekcji.

    ![W sekcji "Konfiguracja oprogramowania HR Cezanne"](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_configure.png) 

7. W oknie przeglądarki innej witryny sieci web Zaloguj się do dzierżawy oprogramowania Cezanne HR jako administrator.

8. W okienku po lewej stronie wybierz **konfiguracji systemu**. Wybierz **ustawienia zabezpieczeń** > **pojedynczy konfiguracji logowania jednokrotnego**.

    ![Łącze "Pojedynczego logowania jednokrotnego konfiguracji"](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_000.png)

9. W **Zezwalaj użytkownikom na logowanie przy użyciu następujących usług pojedynczego logowania jednokrotnego (SSO)** okienku wybierz **SAML 2.0** pole wyboru, a następnie wybierz **Advanced Configuration** opcji.

    ![Opcje logowania jednokrotnego usług](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_001.png)

10. Wybierz **Dodawanie nowych**.

    ![Przycisk "Dodaj nowy"](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_002.png)

11. W obszarze **dostawców tożsamości SAML 2.0**, wykonaj następujące czynności:

    ![W sekcji "Dostawców tożsamości SAML 2.0"](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_003.png)
    
    a. W **Nazwa wyświetlana** wprowadź nazwę dostawcy tożsamości.

    b. W **identyfikator** Wklej **identyfikator jednostki SAML** skopiowany z portalu Azure. 

    c. W **powiązanie SAML** pola listy, wybierz opcję **POST**.

    d. W **punktu końcowego usługi tokenu zabezpieczającego** Wklej **SAML logowania jednokrotnego usługi pojedynczej** adres URL, który został skopiowany z portalu Azure. 
    
    e. W **nazwa atrybutu ID użytkownika** wprowadź `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.
    
    f. Aby przekazać certyfikat pobrany z usługi Azure AD, wybierz **przekazać** przycisku.
    
    g. Kliknij przycisk **OK**. 

12. Wybierz pozycję **Zapisz**.

    ![Przycisk "Zapisz"](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_004.png)

> [!TIP]
> Po skonfigurowaniu aplikacji może odczytywać zwięzły wersji poprzednich instrukcji w [portalu Azure](https://portal.azure.com). Po dodaniu aplikacji z **usługi Active Directory** > **aplikacje dla przedsiębiorstw** zaznacz **logowanie jednokrotne** kartę. Dostęp do dokumentacji osadzonych z **konfiguracji** sekcji. 

Aby dowiedzieć się więcej o funkcji osadzonych dokumentacji, zobacz [usługi Azure AD osadzonych dokumentacji]( https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="create-an-azure-ad-test-user"></a>Tworzenie użytkownika testowego usługi Azure AD
W tej sekcji utworzysz użytkownika testowego Simona Britta w portalu Azure.

![Simona Britta użytkownika testowego][100]

Aby utworzyć użytkownika testowego w usłudze Azure AD, wykonaj następujące czynności:

1. W **portalu Azure**, w okienku po lewej stronie wybierz **usługi Azure Active Directory** przycisku.

    ![Przycisk "Azure Active Directory"](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_01.png) 

2. Aby wyświetlić listę użytkowników, wybierz **użytkowników i grup** > **wszyscy użytkownicy**.
    
    ![Łącze "Wszyscy użytkownicy"](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_02.png) 
    
    **Wszyscy użytkownicy** zostanie otwarte okno dialogowe.

3. Aby otworzyć **użytkownika** okno dialogowe, wybierz opcję **Dodaj**.
 
    ![Przycisk "Dodaj"](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_03.png) 

4. W **użytkownika** okno dialogowe, wykonaj następujące czynności:
 
    ![Okno dialogowe "Użytkownika"](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_04.png) 

    a. W **nazwa** wpisz **BrittaSimon**.

    b. W **nazwy użytkownika** wpisz użytkownika Britta Simona **adres e-mail**.

    c. Wybierz **Pokaż hasło** pole wyboru, a następnie zanotuj wartości, który został wygenerowany w **hasło** pole.

    d. Wybierz pozycję **Utwórz**.
 
### <a name="create-a-cezanne-hr-software-test-user"></a>Tworzenie użytkownika testowego Cezanne HR oprogramowania

Aby umożliwić użytkownikom usługi Azure AD do logowania się na Cezanne HR oprogramowania, muszą mieć przydzielone do Cezanne HR oprogramowania. W przypadku oprogramowania Cezanne HR Inicjowanie obsługi to zadanie ręczne.

Ustanowienie konta użytkownika, wykonując następujące czynności:

1.  Zaloguj się do witryny Cezanne HR oprogramowania firmy jako administrator.

2.  W okienku po lewej stronie wybierz **konfiguracji systemu** > **Zarządzanie użytkownikami** > **Dodaj nowego użytkownika**.

    ![Łącze "Dodaj nowego użytkownika"](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_005.png "nowego użytkownika")

3.  W obszarze **dane osobowe**, wykonaj następujące czynności:

    ![W sekcji "Dane osobowe"](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_006.png "nowego użytkownika")
    
    a. Ustaw **wewnętrznego użytkownika** jako **OFF**.
    
    b. W **imię** wpisz imię użytkownika, na przykład **Britta**.  
 
    c. W **nazwisko** wpisz nazwisko użytkownika, na przykład **Simona**.
    
    d. W **E-mail** wpisz adres e-mail użytkownika, na przykład Brittasimon@contoso.com.

4.  W obszarze **informacje o koncie**, wykonaj następujące czynności:

    ![Sekcja "Informacje o koncie"](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_007.png "nowego użytkownika")
    
    a. W **Username** wpisz adres e-mail użytkownika, na przykład Brittasimon@contoso.com.
    
    b. W **hasło** wpisz hasło użytkownika.
    
    c. W **roli zabezpieczeń** wybierz opcję **HR Professional**.
    
    d. Kliknij przycisk **OK**.

5. Na **logowanie jednokrotne** karcie **SAML 2.0 identyfikatory** zaznacz **Dodaj nowy**.

    ![Przycisk "Dodaj nowy"](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_008.png "użytkownika")

6. W **dostawcy tożsamości** pola listy, wybierz dostawcy tożsamości. W **identyfikator użytkownika** wprowadź adres e-mail konta Simona testu użytkownika Britta.

    ![Pola "Dostawcy tożsamości" i "Identyfikator użytkownika"](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_009.png "użytkownika")
    
7. Wybierz pozycję **Zapisz**.

    ![Przycisk "Zapisz"](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_010.png "użytkownika")

### <a name="assign-the-azure-ad-test-user"></a>Przypisz użytkownika testowego usługi Azure AD

W tej sekcji można włączyć użytkownika testowego Simona Britta do udzielania dostępu do oprogramowania Cezanne HR za pomocą logowania jednokrotnego usługi Azure.

![Dostęp użytkownika testowego][200] 

1. W portalu Azure Otwórz widok aplikacji, a następnie przejdź do widoku katalogu. Wybierz **aplikacje dla przedsiębiorstw** > **wszystkie aplikacje**.

    ![Łącze "Wszystkie aplikacje"][201] 

2. Na liście aplikacji zaznacz **Cezanne HR oprogramowania**.

    ![Na liście "Aplikacji"](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_app.png) 

3. W menu po lewej stronie wybierz **użytkowników i grup**.

    ![Przypisz użytkownika][202] 

4. Wybierz pozycję **Dodaj**. Następnie w **Dodaj przydziału** okno dialogowe, wybierz opcję **użytkowników i grup**.

    ![Łącze "Użytkownicy i grupy"][203]

5. W **użytkowników i grup** okna dialogowego, **użytkowników** listy, wybierz **Simona Britta**.

6. W **użytkowników i grup** okno dialogowe, wybierz opcję **wybierz**.

7. W **Dodaj przydziału** okno dialogowe, wybierz opcję **przypisać**.
    
### <a name="test-sso"></a>Test rejestracji Jednokrotnej

W tej sekcji możesz przetestować konfigurację programu Azure AD z logowania jednokrotnego za pomocą panelu dostępu.

Po wybraniu kafelka oprogramowania Cezanne HR w panelu dostępu logowania automatycznie do aplikacji oprogramowania Cezanne HR.

## <a name="next-steps"></a>Następne kroki

* [Lista samouczków dotyczących sposobów integracji aplikacji SaaS w usłudze Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co to jest dostęp do aplikacji i logowania jednokrotnego w usłudze Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_203.png

