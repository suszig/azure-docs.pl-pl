---
title: "Samouczek: Integracji Azure Active Directory z E sprzedaży Menedżera Remix | Dokumentacja firmy Microsoft"
description: "Informacje o sposobie konfigurowania rejestracji jednokrotnej między usługą Azure Active Directory i E sprzedaży Menedżera Remix."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 89b5022c-0d5b-4103-9877-ddd32b6e1c02
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/21/2018
ms.author: jeedes
ms.openlocfilehash: 56c2860b6aeac9fdc66f2b1fdd1ed9126bf77d3f
ms.sourcegitcommit: d1f35f71e6b1cbeee79b06bfc3a7d0914ac57275
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/22/2018
---
# <a name="tutorial-azure-active-directory-integration-with-e-sales-manager-remix"></a>Samouczek: Integracji Azure Active Directory z Remix Menedżera sprzedaży E

Z tego samouczka dowiesz się integrowanie E sprzedaży Menedżera Remix w usłudze Azure Active Directory (Azure AD).

Integrowanie E sprzedaży Menedżera Remix z usługą Azure AD zapewnia następujące korzyści:

- Można kontrolować w usłudze Azure AD, kto ma dostęp do E sprzedaży Menedżera Remix.
- Umożliwia użytkownikom automatycznie pobrać zalogowane do E sprzedaży Menedżera Remix (logowanie jednokrotne) z konta usługi Azure AD.
- Możesz zarządzać kont w jednej centralnej lokalizacji - portalu Azure.

Jeśli chcesz dowiedzieć się więcej informacji o integracji aplikacji SaaS w usłudze Azure AD, zobacz [co to jest dostęp do aplikacji i logowanie jednokrotne z usługą Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Wymagania wstępne

Aby skonfigurować integrację usługi Azure AD z Remix Menedżera sprzedaży E, potrzebne są następujące elementy:

- Subskrypcję usługi Azure AD
- E sprzedaży Menedżera Remix logowanie jednokrotne włączone subskrypcji

> [!NOTE]
> Aby przetestować kroki opisane w tym samouczku, zaleca się używania środowiska produkcyjnego.

Aby przetestować kroki opisane w tym samouczku, należy wykonać te zalecenia:

- Nie należy używać środowiska produkcyjnego, jeśli jest to konieczne.
- Jeśli nie masz środowisko wersji próbnej usługi Azure AD, możesz [uzyskać miesięczna wersja próbna](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Opis scenariusza
W tym samouczku można przetestować usługę Azure AD rejestracji jednokrotnej w środowisku testowym. Scenariusz opisany w tym samouczku składa się z dwóch głównych elementów:

1. Dodawanie E sprzedaży Menedżera Remix z galerii
2. Konfigurowanie i testowanie usługi Azure AD logowanie jednokrotne

## <a name="adding-e-sales-manager-remix-from-the-gallery"></a>Dodawanie E sprzedaży Menedżera Remix z galerii
Aby skonfigurować integrację E sprzedaży Menedżera Remix do usługi Azure AD, należy dodać E sprzedaży Menedżera Remix z galerii do listy zarządzanych aplikacji SaaS.

**Aby dodać E sprzedaży Menedżera Remix z galerii, wykonaj następujące czynności:**

1. W  **[portalu Azure](https://portal.azure.com)**, na panelu nawigacyjnym po lewej stronie kliknij **usługi Azure Active Directory** ikony. 

    ![Przycisk usługi Azure Active Directory][1]

2. Przejdź do **aplikacje dla przedsiębiorstw**. Następnie przejdź do **wszystkie aplikacje**.

    ![Blok aplikacje przedsiębiorstwa][2]
    
3. Aby dodać nową aplikację, kliknij przycisk **nowej aplikacji** przycisk w górnej części okna dialogowego.

    ![Nowy przycisk aplikacji][3]

4. W polu wyszukiwania wpisz **E sprzedaży Menedżera Remix**, wybierz pozycję **E sprzedaży Menedżera Remix** z panelu wyników kliknięcie **Dodaj** przycisk, aby dodać aplikację.

    ![E sprzedaży Menedżera Remix na liście wyników](./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_esalesmanagerremix_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfiguracja i testowanie usługi Azure AD rejestracji jednokrotnej

W tej sekcji skonfigurować i przetestować usługi Azure AD rejestracji jednokrotnej z E sprzedaży Menedżera Remix w oparciu o nazwie "Britta Simona" użytkownika testowego.

Dla rejestracji jednokrotnej do pracy usługi Azure AD musi wiedzieć, użytkownik odpowiednika w E sprzedaży Menedżera Remix jest dla użytkownika, w usłudze Azure AD. Innymi słowy relację łącza między użytkownika usługi Azure AD i danemu użytkownikowi w E sprzedaży Menedżera Remix musi określone.

Aby skonfigurować i przetestować usługi Azure AD rejestracji jednokrotnej z Remix Menedżera sprzedaży E, należy wykonać poniższe bloki konstrukcyjne:

1. **[Konfigurowanie usługi Azure AD rejestracji jednokrotnej](#configure-azure-ad-single-sign-on)**  — aby umożliwić użytkownikom korzystać z tej funkcji.
2. **[Tworzenie użytkownika testowego usługi Azure AD](#create-an-azure-ad-test-user)**  — do przetestowania usługi Azure AD rejestracji jednokrotnej z Simona Britta.
3. **[Tworzenie użytkownika testowego E sprzedaży Menedżera Remix](#create-an-e-sales-manager-remix-test-user)**  — w celu zapewnienia odpowiednikiem Simona Britta E sprzedaży Menedżera Remix połączonego z usługi Azure AD reprezentację użytkownika.
4. **[Przypisz użytkownika testowego usługi Azure AD](#assign-the-azure-ad-test-user)**  — aby umożliwić Simona Britta do użycia usługi Azure AD rejestracji jednokrotnej.
5. **[Test rejestracji jednokrotnej](#test-single-sign-on)**  — Aby sprawdzić, czy konfiguracja działa.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurowanie usługi Azure AD rejestracji jednokrotnej

W tej sekcji można włączyć usługi Azure AD rejestracji jednokrotnej w portalu Azure i skonfigurować logowanie jednokrotne w E sprzedaży Menedżera Remix aplikacji.

**Aby skonfigurować usługi Azure AD rejestracji jednokrotnej z Remix Menedżera sprzedaży E, wykonaj następujące czynności:**

1. W portalu Azure na **E sprzedaży Menedżera Remix** strona integracji aplikacji, kliknij przycisk **logowanie jednokrotne**.

    ![Skonfigurować łącze rejestracji jednokrotnej][4]

2. Na **logowanie jednokrotne** okno dialogowe, wybierz opcję **tryb** jako **na języku SAML logowania jednokrotnego** Aby włączyć logowanie jednokrotne.
 
    ![Okno dialogowe rejestracji jednokrotnej](./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_esalesmanagerremix_samlbase.png)

3. Na **E sprzedaży Menedżera Remix domeny i adres URL** sekcji, wykonaj następujące czynności:

    ![Adresy URL i E sprzedaży Menedżera Remix domeny pojedynczy informacje logowania jednokrotnego](./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_esalesmanagerremix_url.png)

    a. W **adres URL logowania** tekstowym, wpisz adres URL, używając następującego wzorca: `https://<Server-Based-URL>/<sub-domain>/esales-pc`

    b. W **identyfikator** tekstowym, wpisz adres URL, używając następującego wzorca: `https://<Server-Based-URL>/<sub-domain>/`

    c. Kopiuj **identyfikator** wartość w Notatniku. Wartość identyfikatora zostanie użyta w dalszej części tego samouczka.
    
    > [!NOTE] 
    > Wartości te nie są prawdziwe. Rzeczywisty adres URL logowania i identyfikator, należy zaktualizować te wartości. Skontaktuj się z [zespołem pomocy technicznej klienta Remix E sprzedaży Menedżera](mailto:esupport@softbrain.co.jp) uzyskać te wartości.

4. Na **certyfikat podpisywania SAML** kliknij **Certificate(Base64)** , a następnie zapisz plik certyfikatu na tym komputerze.

    ![Łącze pobierania certyfikatu](./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_esalesmanagerremix_certificate.png) 

5. Wybierz **widoku i edytować wszystkie atrybuty użytkowników** i wybierz polecenie **emailaddress** atrybutu.
    
    ![Konfiguracja Remix kierownik E](./media/active-directory-saas-esalesmanagerremix-tutorial/configure1.png)

6. Kopiuj **Namespace** i **nazwa** wartość w polu tekstowym. Generowanie wartości we wzorcu następujące - `<Namespace>/<Name>`. Ta wartość zostanie użyta w dalszej części tego samouczka.

    ![Konfiguracja Remix kierownik E](./media/active-directory-saas-esalesmanagerremix-tutorial/configure2.png)

7. Na **E sprzedaży Menedżera konfiguracji Remix** , kliknij przycisk **skonfigurować Remix Menedżera sprzedaży E** otworzyć **Konfigurowanie logowania jednokrotnego** okna. Kopiuj **Sign-Out adresu URL i SAML pojedynczy znak na adres URL usługi** z **sekcji krótkimi opisami.**

    ![Konfiguracja Remix kierownik E](./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_esalesmanagerremix_configure.png) 

8. Kliknij przycisk **zapisać** przycisku.

    ![Skonfiguruj przycisk pojedynczego logowania jednokrotnego Zapisz](./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_general_400.png)

9. Zaloguj się do aplikacji E sprzedaży Menedżera Remix jako administrator.

10. Wybierz **do Menu administratora** w menu u góry po prawej.

    ![Konfiguracja Remix kierownik E](./media/active-directory-saas-esalesmanagerremix-tutorial/configure4.png)

11. Wybierz **ustawienia systemu**>**współpracy z systemu zewnętrznego**

    ![Konfiguracja Remix kierownik E](./media/active-directory-saas-esalesmanagerremix-tutorial/configure5.png)
    
12. Wybierz **SAML**

    ![Konfiguracja Remix kierownik E](./media/active-directory-saas-esalesmanagerremix-tutorial/configure6.png)

13. W **ustawienia uwierzytelniania SAML** sekcji, wykonaj następujące czynności:

    ![Konfiguracja Remix kierownik E](./media/active-directory-saas-esalesmanagerremix-tutorial/configure3.png)
    
    a. Wybierz **wersji**
    
    b. Wybierz **e-mail** z listy rozwijanej we współpracy element sekcji.

    c. W polu tekstowym elementu współpracy Wklej **wartości oświadczenia** skopiowanego z Azure tj portalu `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`

    d. W **wystawcy (identyfikator jednostki)** pole tekstowe, Wklej **identyfikator** wartość, która została skopiowana z portalu Azure z **E sprzedaży Menedżera Remix domeny i adres URL** sekcji.

    e. Aby przekazać z pobranego **certyfikatu** z portalu Azure, wybierz **pliku wybór**.

    f. W **adres URL logowania dostawcy identyfikator** pole tekstowe, Wklej **SAML pojedynczy znak na adres URL usługi** skopiowanego z portalu Azure.

    g. W **adres URL wylogowania dostawcy tożsamości** pole tekstowe, Wklej **Sign-Out URL** wartość, która została skopiowana z portalu Azure.

    h. Kliknij przycisk **ustawienie ukończone**

> [!TIP]
> Teraz możesz przeczytać zwięzły wersji tych instrukcji wewnątrz [portalu Azure](https://portal.azure.com), podczas konfigurowania aplikacji!  Po dodaniu tej aplikacji z **usługi Active Directory > aplikacje dla przedsiębiorstw** po prostu kliknij **rejestracji jednokrotnej** karcie i dostęp do dokumentacji osadzonych za pomocą **konfiguracji** sekcji u dołu. Więcej o funkcji dokumentacji osadzonego w tym miejscu: [dokumentacji osadzonych usługi Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Tworzenie użytkownika testowego usługi Azure AD

Celem tej sekcji jest tworzenie użytkownika testowego w portalu Azure o nazwie Simona Britta.

   ![Tworzenie użytkownika testowego usługi Azure AD][100]

**Aby utworzyć użytkownika testowego w usłudze Azure AD, wykonaj następujące czynności:**

1. W portalu Azure, w okienku po lewej stronie kliknij **usługi Azure Active Directory** przycisku.

    ![Przycisk usługi Azure Active Directory](./media/active-directory-saas-esalesmanagerremix-tutorial/create_aaduser_01.png)

2. Aby wyświetlić listę użytkowników, przejdź do **użytkowników i grup**, a następnie kliknij przycisk **wszyscy użytkownicy**.

    !["Użytkownicy i grupy" i "Wszyscy użytkownicy" łącza](./media/active-directory-saas-esalesmanagerremix-tutorial/create_aaduser_02.png)

3. Aby otworzyć **użytkownika** okno dialogowe, kliknij przycisk **Dodaj** w górnej części **wszyscy użytkownicy** okno dialogowe.

    ![Przycisk Dodaj](./media/active-directory-saas-esalesmanagerremix-tutorial/create_aaduser_03.png)

4. W **użytkownika** okna dialogowego wykonaj następujące czynności:

    ![Okno dialogowe użytkownika](./media/active-directory-saas-esalesmanagerremix-tutorial/create_aaduser_04.png)

    a. W **nazwa** wpisz **BrittaSimon**.

    b. W **nazwy użytkownika** wpisz adres e-mail użytkownika Simona Britta.

    c. Wybierz **Pokaż hasło** pole wyboru, a następnie zanotuj wartość, która jest wyświetlana w **hasło** pole.

    d. Kliknij przycisk **Utwórz**.
 
### <a name="create-an-e-sales-manager-remix-test-user"></a>Tworzenie użytkownika testowego Remix Menedżera sprzedaży E

1. Zaloguj się do aplikacji E sprzedaży Menedżera Remix jako administrator.

2. Wybierz **do Menu administratora** w menu u góry po prawej.

    ![Konfiguracja Remix kierownik E](./media/active-directory-saas-esalesmanagerremix-tutorial/configure4.png)

3. Wybierz **ustawień firmy**>**konserwacji działów i pracownicy** i wybierz **pracowników w zarejestrowany**.

    ![Użytkownik](./media/active-directory-saas-esalesmanagerremix-tutorial/user1.png)

4. W **nowej rejestracji pracownika** sekcji, wykonaj następujące czynności:
    
    ![Użytkownik](./media/active-directory-saas-esalesmanagerremix-tutorial/user2.png)

    a. W **nazwa pracownika** tekstowym, wpisz nazwę użytkownika, takich jak Britta.

    b. Wypełnienie wszystkich odpowiednich wymaganych pól informacji użytkownika.
    
    c. Jeśli włączysz SAML, administrator nie będzie mógł zalogować się na ekranie logowania, więc przyznać administratora uprawnień logowania dla użytkownika, wybierając **identyfikator logowania administratora**

    d. Kliknij przycisk **rejestracji**

5. W przyszłości, jeśli chcesz zalogować się jako administrator, zalogować się użytkownik, który został udzielony uprawnienia administratora i kliknij przycisk **do Menu administratora** w menu u góry po prawej.

    ![Użytkownik](./media/active-directory-saas-esalesmanagerremix-tutorial/configure4.png)

### <a name="assign-the-azure-ad-test-user"></a>Przypisz użytkownika testowego usługi Azure AD

W tej sekcji można włączyć Simona Britta do używania Azure logowania jednokrotnego za udzielanie dostępu do E sprzedaży Menedżera Remix.

![Przypisanie roli użytkownika][200] 

**Aby przypisać Simona Britta Remix Menedżera sprzedaży E, wykonaj następujące czynności:**

1. W portalu Azure Otwórz widok aplikacji, a następnie przejdź do widoku katalogu i przejdź do **aplikacje dla przedsiębiorstw** kliknięcie **wszystkie aplikacje**.

    ![Przypisz użytkownika][201] 

2. Na liście aplikacji zaznacz **E sprzedaży Menedżera Remix**.

    ![E sprzedaży Menedżera Remix łącza na liście aplikacji](./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_esalesmanagerremix_app.png)  

3. W menu po lewej stronie kliknij **użytkowników i grup**.

    ![Łącze "Użytkownicy i grupy"][202]

4. Kliknij przycisk **Dodaj** przycisku. Następnie wybierz **użytkowników i grup** na **Dodaj przydziału** okna dialogowego.

    ![W okienku Dodaj przydziału][203]

5. Na **użytkowników i grup** okno dialogowe, wybierz opcję **Simona Britta** na liście Użytkownicy.

6. Kliknij przycisk **wybierz** znajdującego się na **użytkowników i grup** okna dialogowego.

7. Kliknij przycisk **przypisać** znajdującego się na **Dodaj przydziału** okna dialogowego.
    
### <a name="test-single-sign-on"></a>Test rejestracji jednokrotnej

W tej sekcji można przetestować konfiguracji usługi Azure AD pojedynczego logowania za pomocą panelu dostępu.

Po kliknięciu kafelka E sprzedaży Menedżera Remix w panelu dostępu należy należy pobrać automatycznie zalogowane E sprzedaży Menedżera Remix aplikacji.
Aby uzyskać więcej informacji na temat panelu dostępu, zobacz [wprowadzenie do panelu dostępu](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Zasoby dodatkowe

* [Lista samouczków dotyczących sposobów integracji aplikacji SaaS przy użyciu usługi Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co to jest dostęp do aplikacji i logowanie jednokrotne z usługą Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_general_203.png

