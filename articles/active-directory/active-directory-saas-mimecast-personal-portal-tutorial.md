---
title: 'Samouczek: Azure Active Directory integracji z portalu osobiste Mimecast | Dokumentacja firmy Microsoft'
description: "Informacje o sposobie konfigurowania rejestracji jednokrotnej między Mimecast osobistych portalu i usługi Azure Active Directory."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 345b22be-d87e-45a4-b4c0-70a67eaf9bfd
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: bf46da35a55608d7e4656c9dd3ad9d5f2253e225
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-mimecast-personal-portal"></a>Samouczek: Integracji Azure Active Directory z Mimecast osobistych portalu

Z tego samouczka dowiesz integrowanie Mimecast Portal osobiste z usługą Azure Active Directory (Azure AD).

Integrowanie portalu osobiste Mimecast z usługą Azure AD zapewnia następujące korzyści:

- Można kontrolować w usłudze Azure AD, który ma dostęp do portalu osobiste Mimecast
- Umożliwia użytkownikom automatycznie pobrać zalogowane Mimecast portalu osobiste (logowanie jednokrotne) z konta usługi Azure AD
- Możesz zarządzać kont w jednej centralnej lokalizacji - portalu Azure

Jeśli chcesz dowiedzieć się więcej informacji o integracji aplikacji SaaS w usłudze Azure AD, zobacz [co to jest dostęp do aplikacji i logowanie jednokrotne z usługą Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Wymagania wstępne

Aby skonfigurować integrację usługi Azure AD z portalem osobiste Mimecast, potrzebne są następujące zasoby:

- Subskrypcję usługi Azure AD
- Mimecast osobistych portalu rejestracji jednokrotnej włączone subskrypcji

> [!NOTE]
> Aby przetestować kroki opisane w tym samouczku, zaleca się używania środowiska produkcyjnego.

Aby przetestować kroki opisane w tym samouczku, należy wykonać te zalecenia:

- Nie należy używać środowiska produkcyjnego, jeśli jest to konieczne.
- Jeśli nie masz środowisko wersji próbnej usługi Azure AD, możesz pobrać miesięczna wersja próbna [tutaj](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Opis scenariusza
W tym samouczku można przetestować usługę Azure AD rejestracji jednokrotnej w środowisku testowym. Scenariusz opisany w tym samouczku składa się z dwóch głównych elementów:

1. Dodawanie portalu osobiste Mimecast z galerii
2. Konfigurowanie i testowanie usługi Azure AD logowanie jednokrotne

## <a name="adding-mimecast-personal-portal-from-the-gallery"></a>Dodawanie portalu osobiste Mimecast z galerii
Aby skonfigurować integrację usługi Azure AD Mimecast osobistych portalu, należy dodać Portal osobiste Mimecast z galerii do listy zarządzanych aplikacji SaaS.

**Aby dodać Mimecast osobistych Portal z poziomu galerii, wykonaj następujące czynności:**

1. W  **[portalu Azure](https://portal.azure.com)**, na panelu nawigacyjnym po lewej stronie kliknij **usługi Azure Active Directory** ikony. 

    ![Usługa Active Directory][1]

2. Przejdź do **aplikacje dla przedsiębiorstw**. Następnie przejdź do **wszystkie aplikacje**.

    ![Aplikacje][2]
    
3. Aby dodać nową aplikację, kliknij przycisk **nowej aplikacji** przycisk w górnej części okna dialogowego.

    ![Aplikacje][3]

4. W polu wyszukiwania wpisz **Portal osobiste Mimecast**.

    ![Tworzenie użytkownika testowego usługi Azure AD](./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_mimecastpersonalportal_search.png)

5. W panelu wyników wybierz **Portal osobiste Mimecast**, a następnie kliknij przycisk **Dodaj** przycisk, aby dodać aplikację.

    ![Tworzenie użytkownika testowego usługi Azure AD](./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_mimecastpersonalportal_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurowanie i testowanie usługi Azure AD logowanie jednokrotne
W tej sekcji skonfigurować i przetestować usługi Azure AD rejestracji jednokrotnej z portalem osobiste Mimecast w oparciu o nazwie "Britta Simona" użytkownika testowego.

Dla rejestracji jednokrotnej do pracy usługi Azure AD musi wiedzieć, użytkownik odpowiednika w portalu osobiste Mimecast jest dla użytkownika, w usłudze Azure AD. Innymi słowy relację łącza między użytkownika usługi Azure AD i powiązane użytkownika w portalu osobiste Mimecast musi określone.

W portalu osobiste Mimecast, należy przypisać wartość **nazwy użytkownika** w usłudze Azure AD jako wartość **Username** do ustanawiania relacji łącza.

Aby skonfigurować i przetestować usługi Azure AD rejestracji jednokrotnej z portalem osobiste Mimecast, należy wykonać poniższe bloki konstrukcyjne:

1. **[Konfigurowanie usługi Azure AD rejestracji jednokrotnej](#configuring-azure-ad-single-sign-on)**  — aby umożliwić użytkownikom korzystać z tej funkcji.
2. **[Tworzenie użytkownika testowego usługi Azure AD](#creating-an-azure-ad-test-user)**  — do przetestowania usługi Azure AD rejestracji jednokrotnej z Simona Britta.
3. **[Tworzenie użytkownika testowego portalu osobiste Mimecast](#creating-a-mimecast-personal-portal-test-user)**  — w celu zapewnienia odpowiednikiem Simona Britta Mimecast Portal osobistych, który jest połączony z usługi Azure AD reprezentację użytkownika.
4. **[Przypisanie użytkownika testowego usługi Azure AD](#assigning-the-azure-ad-test-user)**  — aby umożliwić Simona Britta do użycia usługi Azure AD rejestracji jednokrotnej.
5. **[Testowanie rejestracji jednokrotnej](#testing-single-sign-on)**  — Aby sprawdzić, czy konfiguracja działa.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurowanie usługi Azure AD rejestracji jednokrotnej

W tej sekcji można włączyć usługi Azure AD rejestracji jednokrotnej w portalu Azure i skonfigurować logowanie jednokrotne w aplikacji Portal osobiste Mimecast.

**Aby skonfigurować usługi Azure AD rejestracji jednokrotnej z Mimecast osobistych portalu, wykonaj następujące czynności:**

1. W portalu Azure na **Portal osobiste Mimecast** strona integracji aplikacji, kliknij przycisk **logowanie jednokrotne**.

    ![Konfigurowanie rejestracji jednokrotnej][4]

2. Na **logowanie jednokrotne** okno dialogowe, wybierz opcję **tryb** jako **na języku SAML logowania jednokrotnego** Aby włączyć logowanie jednokrotne.
 
    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_mimecastpersonalportal_samlbase.png)

3. Na **Mimecast osobistych portalu domeny i adres URL** sekcji, wykonaj następujące czynności:

    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_mimecastpersonalportal_url.png)

    a. W **adres URL logowania** tekstowym, wpisz adres URL, używając następującego wzorca: 
    | |     
    | ----------------------------------------|
    | `https://webmail-uk.mimecast.com`|
    | `https://webmail-us.mimecast.com`|
    | |
   
    b. W **identyfikator** tekstowym, wpisz adres URL, używając następującego wzorca:

    | |     
    | --- |
    | `https://webmail-us.mimecast.com/sso/<companyname>`|
    | `https://webmail-uk.mimecast.com/sso/<companyname>`|    
    | `https://webmail-za.mimecast.com/sso/<companyname>`|
    | `https://webmail.mimecast-offshore.com/sso/<companyname>`|
    ||                                                 
    
    > [!NOTE] 
    > Wartości te nie są prawdziwe. Rzeczywisty adres URL logowania i identyfikator, należy zaktualizować te wartości. Skontaktuj się z [zespołem pomocy technicznej osobiste klienta portalu Mimecast](https://www.mimecast.com/customer-success/technical-support/) uzyskać te wartości. 
 


4. Na **certyfikat podpisywania SAML** kliknij **Certificate(Base64)** , a następnie zapisz plik certyfikatu na tym komputerze.

    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_mimecastpersonalportal_certificate.png) 

5. Kliknij przycisk **zapisać** przycisku.

    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_general_400.png)

6. Na **Mimecast osobistych konfiguracji portalu** , kliknij przycisk **skonfigurować Portal osobiste Mimecast** otworzyć **Konfigurowanie logowania jednokrotnego** okna. Kopiuj **Sign-Out adres URL, identyfikator jednostki SAML i SAML pojedynczy znak na adres URL usługi** z **sekcji krótkimi opisami.**

    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_mimecastpersonalportal_configure.png) 

7. W oknie przeglądarki innej witryny sieci web Zaloguj się do portalu osobiste Mimecast jako administrator.

8. Przejdź do **usług \> aplikacji**.
   
    ![Aplikacje](./media/active-directory-saas-mimecast-personal-portal-tutorial/ic794998.png "aplikacji")

9. Kliknij przycisk **profile uwierzytelniania**.
   
    ![Profile uwierzytelniania](./media/active-directory-saas-mimecast-personal-portal-tutorial/ic794999.png "profile uwierzytelniania")

10. Kliknij przycisk **nowy profil uwierzytelniania**.
   
    ![Nowy profil uwierzytelniania](./media/active-directory-saas-mimecast-personal-portal-tutorial/ic795000.png "nowy profil uwierzytelniania")

11. W **profilu uwierzytelniania** sekcji, wykonaj następujące czynności:
   
    ![Profil uwierzytelniania](./media/active-directory-saas-mimecast-personal-portal-tutorial/ic795001.png "profilu uwierzytelniania")
   
    a. W **opis** tekstowym, wpisz nazwę dla danej konfiguracji.
   
    b. Wybierz **wymusić uwierzytelnianie SAML portalu osobiste Mimecast**.
   
    c. Jako **dostawcy**, wybierz pozycję **usługi Azure Active Directory**.
   
    d. W **adres URL wystawcy** pole tekstowe, Wklej wartość **identyfikator jednostki SAML** którego została skopiowana z portalu Azure.
   
    e. W **adres URL logowania** pole tekstowe, Wklej wartość **SAML pojedynczy znak na adres URL usługi** którego została skopiowana z portalu Azure.
   
    f. W **adresu URL wylogowania** pole tekstowe, Wklej wartość **Sign-Out URL** którego została skopiowana z portalu Azure.

    g. Otwórz z **base-64** zakodowanego certyfikatu w programie Notatnik pobrany z portalu Azure, skopiuj zawartość go do Schowka, a następnie wklej go do **certyfikat dostawcy tożsamości (metadanymi)** pola tekstowego.

    h. Wybierz **Zezwalaj funkcji logowania jednokrotnego w**.
   
    i. Kliknij pozycję **Zapisz**.

> [!TIP]
> Teraz możesz przeczytać zwięzły wersji tych instrukcji wewnątrz [portalu Azure](https://portal.azure.com), podczas konfigurowania aplikacji!  Po dodaniu tej aplikacji z **usługi Active Directory > aplikacje dla przedsiębiorstw** po prostu kliknij **rejestracji jednokrotnej** karcie i dostęp do dokumentacji osadzonych za pomocą **konfiguracji** sekcji u dołu. Więcej o funkcji dokumentacji osadzonego w tym miejscu: [dokumentacji osadzonych usługi Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Tworzenie użytkownika testowego usługi Azure AD
Celem tej sekcji jest tworzenie użytkownika testowego w portalu Azure o nazwie Simona Britta.

![Tworzenie użytkowników usługi Azure AD][100]

**Aby utworzyć użytkownika testowego w usłudze Azure AD, wykonaj następujące czynności:**

1. W **portalu Azure**, w lewym okienku nawigacji, kliknij polecenie **usługi Azure Active Directory** ikony.

    ![Tworzenie użytkownika testowego usługi Azure AD](./media/active-directory-saas-mimecast-personal-portal-tutorial/create_aaduser_01.png) 

2. Aby wyświetlić listę użytkowników, przejdź do **użytkowników i grup** i kliknij przycisk **wszyscy użytkownicy**.
    
    ![Tworzenie użytkownika testowego usługi Azure AD](./media/active-directory-saas-mimecast-personal-portal-tutorial/create_aaduser_02.png) 

3. Aby otworzyć **użytkownika** okna dialogowego, kliknij przycisk **Dodaj** górnej części okna dialogowego.
 
    ![Tworzenie użytkownika testowego usługi Azure AD](./media/active-directory-saas-mimecast-personal-portal-tutorial/create_aaduser_03.png) 

4. Na **użytkownika** okna dialogowego strony, należy wykonać następujące czynności:
 
    ![Tworzenie użytkownika testowego usługi Azure AD](./media/active-directory-saas-mimecast-personal-portal-tutorial/create_aaduser_04.png) 

    a. W **nazwa** pole tekstowe, typ **BrittaSimon**.

    b. W **nazwy użytkownika** pole tekstowe, typ **adres e-mail** z BrittaSimon.

    c. Wybierz **Pokaż hasło** i zanotuj wartość **hasło**.

    d. Kliknij przycisk **Utwórz**.
 
### <a name="creating-a-mimecast-personal-portal-test-user"></a>Tworzenie użytkownika testowego Mimecast osobistych portalu

Aby umożliwić użytkownikom zalogować się do portalu osobiste Mimecast usługi Azure AD, muszą mieć przydzielone do Mimecast osobistych portalu. W przypadku osobistych portalu Mimecast Inicjowanie obsługi to zadanie ręczne.

Należy zarejestrować domeny, przed przystąpieniem do tworzenia użytkowników.

**Aby skonfigurować, inicjowanie obsługi użytkowników, wykonaj następujące czynności:**

1. Zaloguj się na Twojej **Portal osobiste Mimecast** jako administrator.

2. Przejdź do **katalogów \> wewnętrzny**.
   
    ![Katalogi](./media/active-directory-saas-mimecast-personal-portal-tutorial/ic795003.png "katalogów")

3. Kliknij przycisk **zarejestrować nową domenę**.
   
    ![Zarejestrować nową domenę](./media/active-directory-saas-mimecast-personal-portal-tutorial/ic795004.png "zarejestrować nową domenę")

4. Po utworzeniu nowej domeny, kliknij przycisk **nowy adres**.
   
    ![Nowy adres](./media/active-directory-saas-mimecast-personal-portal-tutorial/ic795005.png "nowy adres")

5. W oknie dialogowym Nowy adres, wykonaj następujące kroki prawidłowy Azure AD konta chcesz udostępnić:
   
    ![Zapisz](./media/active-directory-saas-mimecast-personal-portal-tutorial/ic795006.png "Zapisz")
   
    a. W **adres E-mail** pole tekstowe, typ **adres E-mail** użytkownika jako  **BrittaSimon@contoso.com** .
    
    b. W **globalną nazwę** pole tekstowe, typ **username** jako **BrittaSimon**.

    c. W **hasło**, i **Potwierdź hasło** pól tekstowych, typ **hasło** użytkownika.
   
    b. Kliknij pozycję **Zapisz**.

>[!NOTE]
>Inne narzędzia do tworzenia konta użytkownika portalu osobiste Mimecast lub interfejsów API dostarczonych przez Portal osobiste Mimecast służy do obsługi administracyjnej kont użytkowników usługi Azure AD. 

### <a name="assigning-the-azure-ad-test-user"></a>Przypisanie użytkownika testowego usługi Azure AD

W tej sekcji można włączyć Simona Britta do używania Azure logowania jednokrotnego za udzielanie dostępu do portalu osobiste Mimecast.

![Przypisz użytkownika][200] 

**Aby przypisać Simona Britta Mimecast osobistych portalu, wykonaj następujące czynności:**

1. W portalu Azure Otwórz widok aplikacji, a następnie przejdź do widoku katalogu i przejdź do **aplikacje dla przedsiębiorstw** kliknięcie **wszystkie aplikacje**.

    ![Przypisz użytkownika][201] 

2. Na liście aplikacji zaznacz **Portal osobiste Mimecast**.

    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_mimecastpersonalportal_app.png) 

3. W menu po lewej stronie kliknij **użytkowników i grup**.

    ![Przypisz użytkownika][202] 

4. Kliknij przycisk **Dodaj** przycisku. Następnie wybierz **użytkowników i grup** na **Dodaj przydziału** okna dialogowego.

    ![Przypisz użytkownika][203]

5. Na **użytkowników i grup** okno dialogowe, wybierz opcję **Simona Britta** na liście Użytkownicy.

6. Kliknij przycisk **wybierz** znajdującego się na **użytkowników i grup** okna dialogowego.

7. Kliknij przycisk **przypisać** znajdującego się na **Dodaj przydziału** okna dialogowego.
    
### <a name="testing-single-sign-on"></a>Testowanie rejestracji jednokrotnej
W tej sekcji można przetestować konfiguracji usługi Azure AD pojedynczego logowania za pomocą panelu dostępu.

Po kliknięciu kafelka Mimecast Portal osobistych w panelu dostępu użytkownik powinien pobrać automatycznie zalogowane do aplikacji Portal osobiste Mimecast. Aby uzyskać więcej informacji na temat panelu dostępu, zobacz [wprowadzenie do panelu dostępu](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Lista samouczków dotyczących sposobów integracji aplikacji SaaS przy użyciu usługi Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co to jest dostęp do aplikacji i logowanie jednokrotne z usługą Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_general_203.png

