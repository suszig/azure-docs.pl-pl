---
title: "Samouczek: Integracji Azure Active Directory z usług Salesforce | Dokumentacja firmy Microsoft"
description: "Informacje o sposobie konfigurowania rejestracji jednokrotnej między usługą Azure Active Directory i Salesforce."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: d2d7d420-dc91-41b8-a6b3-59579e043b35
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: 639e40ca7e406a1726033e9f5c5363c289087589
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-salesforce"></a>Samouczek: Integracji Azure Active Directory z usług Salesforce

Z tego samouczka dowiesz się Integrowanie usługi Salesforce z usługą Azure Active Directory (Azure AD).

Integrowanie usługi Salesforce z usługą Azure AD zapewnia następujące korzyści:

- Można kontrolować w usłudze Azure AD, który ma dostęp do usług Salesforce
- Umożliwia użytkownikom automatycznie pobrać zalogowane do usług Salesforce (logowanie jednokrotne) przy użyciu ich kont usługi Azure AD
- Możesz zarządzać kont w jednej centralnej lokalizacji - portalu Azure

Jeśli chcesz dowiedzieć się więcej informacji o integracji aplikacji SaaS w usłudze Azure AD, zobacz [co to jest dostęp do aplikacji i logowanie jednokrotne z usługą Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Wymagania wstępne

Aby skonfigurować integrację usługi Azure AD z usług Salesforce, potrzebne są następujące elementy:

- Subskrypcję usługi Azure AD
- Salesforce jednokrotnego włączone subskrypcji

> [!NOTE]
> Aby przetestować kroki opisane w tym samouczku, zaleca się używania środowiska produkcyjnego.

Aby przetestować kroki opisane w tym samouczku, należy wykonać te zalecenia:

- Nie należy używać środowiska produkcyjnego, jeśli jest to konieczne.
- Jeśli nie masz środowisko wersji próbnej usługi Azure AD, możesz pobrać miesięczna wersja próbna [tutaj](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Opis scenariusza
W tym samouczku można przetestować usługę Azure AD rejestracji jednokrotnej w środowisku testowym. Scenariusz opisany w tym samouczku składa się z dwóch głównych elementów:

1. Dodawanie usługi Salesforce z galerii
2. Konfigurowanie i testowanie usługi Azure AD logowanie jednokrotne

## <a name="adding-salesforce-from-the-gallery"></a>Dodawanie usługi Salesforce z galerii
Aby skonfigurować integrację usługi Salesforce z usługą Azure AD, należy dodać usługi Salesforce z galerii do listy zarządzanych aplikacji SaaS.

**Aby dodać usługi Salesforce z galerii, wykonaj następujące czynności:**

1. W  **[portalu Azure](https://portal.azure.com)**, na panelu nawigacyjnym po lewej stronie kliknij **usługi Azure Active Directory** ikony. 

    ![Usługa Active Directory][1]

2. Przejdź do **aplikacje dla przedsiębiorstw**. Następnie przejdź do **wszystkie aplikacje**.

    ![Aplikacje][2]
    
3. Kliknij przycisk **nowej aplikacji** przycisk w górnej części okna dialogowego.

    ![Aplikacje][3]

4. W polu wyszukiwania wpisz **Salesforce**.

    ![Tworzenie użytkownika testowego usługi Azure AD](./media/active-directory-saas-salesforce-tutorial/tutorial_salesforce_search.png)

5. W panelu wyników wybierz **Salesforce**, a następnie kliknij przycisk **Dodaj** przycisk, aby dodać aplikację.

    ![Tworzenie użytkownika testowego usługi Azure AD](./media/active-directory-saas-salesforce-tutorial/tutorial_salesforce_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurowanie i testowanie usługi Azure AD logowanie jednokrotne
W tej sekcji możesz skonfigurować i przetestować usługi Azure AD rejestracji jednokrotnej z Salesforce na podstawie użytkownika testowego, nazywany "Britta Simona".

Dla rejestracji jednokrotnej do pracy usługi Azure AD musi wiedzieć, użytkownik odpowiednika w usłudze Salesforce jest dla użytkownika, w usłudze Azure AD. Innymi słowy relację łącza między użytkownika usługi Azure AD i powiązane użytkownika w usłudze Salesforce musi określone.

Ta relacja łącza zostanie nawiązane, przypisując wartość **nazwy użytkownika** w usłudze Azure AD jako wartość **Username** w usłudze Salesforce.

Aby skonfigurować i przetestować usługi Azure AD rejestracji jednokrotnej z usług Salesforce, należy wykonać poniższe bloki konstrukcyjne:

1. **[Konfigurowanie usługi Azure AD rejestracji jednokrotnej](#configuring-azure-ad-single-sign-on)**  — aby umożliwić użytkownikom korzystać z tej funkcji.
2. **[Tworzenie użytkownika testowego usługi Azure AD](#creating-an-azure-ad-test-user)**  — do przetestowania usługi Azure AD rejestracji jednokrotnej z Simona Britta.
3. **[Tworzenie użytkownika testowego Salesforce](#creating-a-salesforce-test-user)**  — w celu zapewnienia odpowiednikiem Simona Britta Salesforce połączonego z usługi Azure AD reprezentację użytkownika.
4. **[Przypisanie użytkownika testowego usługi Azure AD](#assigning-the-azure-ad-test-user)**  — aby umożliwić Simona Britta do użycia usługi Azure AD rejestracji jednokrotnej.
5. **[Testowanie rejestracji jednokrotnej](#testing-single-sign-on)**  — Aby sprawdzić, czy konfiguracja działa.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurowanie usługi Azure AD rejestracji jednokrotnej

W tej sekcji można włączyć usługi Azure AD rejestracji jednokrotnej w portalu Azure i skonfigurować logowanie jednokrotne w aplikacji Salesforce.

**Aby skonfigurować usługi Azure AD rejestracji jednokrotnej z usług Salesforce, wykonaj następujące czynności:**

1. W portalu Azure na **Salesforce** strona integracji aplikacji, kliknij przycisk **logowanie jednokrotne**.

    ![Konfigurowanie rejestracji jednokrotnej][4]

2. Na **logowanie jednokrotne** okno dialogowe, wybierz opcję **tryb** jako **na języku SAML logowania jednokrotnego** Aby włączyć logowanie jednokrotne.
 
    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-salesforce-tutorial/tutorial_salesforce_samlbase.png)

3. Na **domeny Salesforce i adres URL** sekcji, wykonaj następujące czynności:

    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-salesforce-tutorial/tutorial_salesforce_url.png)

    W **adres URL logowania** tekstowym, wpisz wartość, przy użyciu następującego wzorca: 
   * Konto przedsiębiorstwa:`https://<subdomain>.my.salesforce.com`
   * Konta dewelopera:`https://<subdomain>-dev-ed.my.salesforce.com`

    > [!NOTE] 
    > Wartości te nie są rzeczywistych. Adres URL logowania rzeczywiste, należy zaktualizować te wartości. Skontaktuj się z [zespołem pomocy technicznej klienta usług Salesforce](https://help.salesforce.com/support) uzyskać te wartości. 
 
4. Na **certyfikat podpisywania SAML** kliknij **certyfikatu** , a następnie zapisz plik certyfikatu na tym komputerze.

    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-salesforce-tutorial/tutorial_salesforce_certificate.png) 

5. Kliknij przycisk **zapisać** przycisku.

    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-salesforce-tutorial/tutorial_general_400.png)

6. Na **konfiguracji Salesforce** , kliknij przycisk **skonfigurować Salesforce** otworzyć **Konfigurowanie logowania jednokrotnego** okna. Kopiuj **identyfikator jednostki SAML i SAML pojedynczy znak na adres URL usługi** z **sekcji krótkimi opisami.** 

    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-salesforce-tutorial/tutorial_salesforce_configure.png) 
<CS>
7.  Otwórz nową kartę w przeglądarce i zaloguj się do konta administratora usługi Salesforce.

8.  W obszarze **administratora** okienka nawigacji kliknij **kontroli bezpieczeństwa** do rozwiń sekcję pokrewne. Następnie kliknij przycisk **ustawień rejestracji jednokrotnej**.

    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-salesforce-tutorial/sf-admin-sso.png)

9.  Na **ustawień rejestracji jednokrotnej** kliknij przycisk **Edytuj** przycisku.
    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-salesforce-tutorial/sf-admin-sso-edit.png)

      > [!NOTE]
      > Jeśli nie można włączyć logowanie jednokrotne ustawień konta usług Salesforce, konieczne może być skontaktuj się z [zespołem pomocy technicznej klienta usług Salesforce](https://help.salesforce.com/support). 

10. Wybierz **włączone SAML**, a następnie kliknij przycisk **zapisać**.

      ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-salesforce-tutorial/sf-enable-saml.png)
11. Aby skonfigurować SAML pojedynczego logowania jednokrotnego ustawienia, kliknij przycisk **nowy**.

    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-salesforce-tutorial/sf-admin-sso-new.png)

12. Na **SAML pojedynczego logowania jednokrotnego ustawienie Edytuj** strony, sprawdź następujące konfiguracje:

    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-salesforce-tutorial/sf-saml-config.png)

    a. Aby uzyskać **nazwa** wpisz przyjazną nazwę dla tej konfiguracji. Wartość dla **nazwa** automatycznie wypełnić **Nazwa interfejsu API** pola tekstowego.

    b. Wklej **identyfikator jednostki SMAL** wartości do **wystawcy** w Salesforce.

    c. W **textbox identyfikator jednostki**, wpisz nazwę domeny witryny Salesforce przy użyciu następującego wzorca:
      
      * Konto przedsiębiorstwa:`https://<subdomain>.my.salesforce.com`
      * Konta dewelopera:`https://<subdomain>-dev-ed.my.salesforce.com`
      
    d. Kliknij przycisk **Przeglądaj** lub **wybierz plik** otworzyć **wybierz plik do przekazania** okno dialogowe, wybierz certyfikat usług Salesforce, a następnie kliknij przycisk **Otwórz** można przekazać certyfikatu.

    e. Dla **typ tożsamości SAML**, wybierz pozycję **potwierdzenia zawiera użytkownika witryny salesforce.com**.

    f. Dla **lokalizacji tożsamości SAML**, wybierz pozycję **jest tożsamość w elemencie NameIdentifier instrukcji podmiotu**

    g. Wklej **pojedynczy znak na adres URL usługi** do **adresu URL logowania do dostawcy tożsamości** w Salesforce.
    
    h. Dla **dostawcy zainicjował żądanie powiązania usługi**, wybierz pozycję **przekierowywanie HTTP**.
    
    i. Na koniec kliknij **zapisać** dotyczyć SAML pojedynczego logowania jednokrotnego ustawień.

13. W lewym okienku nawigacji w aplikacji Salesforce, kliknij polecenie **Zarządzanie domenami** rozwiń sekcję powiązane, a następnie kliknij przycisk **Moje domeny**.

    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-salesforce-tutorial/sf-my-domain.png)

14. Przewiń w dół do **konfiguracji uwierzytelniania** sekcji, a następnie kliknij polecenie **Edytuj** przycisku.

    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-salesforce-tutorial/sf-edit-auth-config.png)

15. W **usługi uwierzytelniania** sekcji, wybierz przyjazną nazwę konfiguracji logowania jednokrotnego SAML, a następnie kliknij przycisk **zapisać**.

    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-salesforce-tutorial/sf-auth-config.png)

    > [!NOTE]
    > Jeśli zostanie wybrana więcej niż jedna usługa uwierzytelniania, użytkownicy są monit o wybranie usługi uwierzytelniania, która ich, takich jak się zalogować podczas inicjowania rejestracji jednokrotnej w środowisku usług Salesforce. Jeśli nie chcesz, aby mieć miejsce, a następnie wykonaj następujące czynności **pozostawienie jej niezaznaczonej innych usług uwierzytelniania**.
<CE>    
> [!TIP]
> Teraz możesz przeczytać zwięzły wersji tych instrukcji wewnątrz [portalu Azure](https://portal.azure.com), podczas konfigurowania aplikacji!  Po dodaniu tej aplikacji z **usługi Active Directory > aplikacje dla przedsiębiorstw** po prostu kliknij **rejestracji jednokrotnej** karcie i dostęp do dokumentacji osadzonych za pomocą **konfiguracji** sekcji u dołu. Więcej o funkcji dokumentacji osadzonego w tym miejscu: [dokumentacji osadzonych usługi Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)


### <a name="creating-an-azure-ad-test-user"></a>Tworzenie użytkownika testowego usługi Azure AD
Celem tej sekcji jest tworzenie użytkownika testowego w portalu Azure o nazwie Simona Britta.

![Tworzenie użytkowników usługi Azure AD][100]

**Aby utworzyć użytkownika testowego w usłudze Azure AD, wykonaj następujące czynności:**

1. W lewym okienku nawigacji w **portalu Azure**, kliknij przycisk **usługi Azure Active Directory** ikony.

    ![Tworzenie użytkownika testowego usługi Azure AD](./media/active-directory-saas-salesforce-tutorial/create_aaduser_01.png) 

2. Aby wyświetlić listę użytkowników, przejdź do **użytkowników i grup** i kliknij przycisk **wszyscy użytkownicy**.
    
    ![Tworzenie użytkownika testowego usługi Azure AD](./media/active-directory-saas-salesforce-tutorial/create_aaduser_02.png) 

3. W górnej części okna dialogowego, kliknij przycisk **Dodaj** otworzyć **użytkownika** okna dialogowego.
 
    ![Tworzenie użytkownika testowego usługi Azure AD](./media/active-directory-saas-salesforce-tutorial/create_aaduser_03.png) 

4. Na **użytkownika** okna dialogowego strony, należy wykonać następujące czynności:
 
    ![Tworzenie użytkownika testowego usługi Azure AD](./media/active-directory-saas-salesforce-tutorial/create_aaduser_04.png) 

    a. W **nazwa** pole tekstowe, typ **BrittaSimon**.

    b. W **nazwy użytkownika** pole tekstowe, typ **adres e-mail** z BrittaSimon.

    c. Wybierz **Pokaż hasło** i zanotuj wartość **hasło**.

    d. Kliknij przycisk **Utwórz**.
 
### <a name="creating-a-salesforce-test-user"></a>Tworzenie użytkownika testowego usług Salesforce

W tej sekcji użytkownika o nazwie Simona Britta jest tworzony w Salesforce. SalesForce obsługę w czasie, który jest domyślnie włączona.
Nie ma elementu akcji można w tej sekcji. Jeśli użytkownik nie istnieje w usłudze Salesforce, nowy jest tworzony przy próbie uzyskania dostępu do usług Salesforce.

### <a name="assigning-the-azure-ad-test-user"></a>Przypisanie użytkownika testowego usługi Azure AD

W tej sekcji można włączyć Simona Britta do używania Azure logowania jednokrotnego za udzielanie dostępu do usług Salesforce.

![Przypisz użytkownika][200] 

**Aby przypisać Simona Britta Salesforce, wykonaj następujące czynności:**

1. W portalu Azure Otwórz widok aplikacji, a następnie przejdź do widoku katalogu i przejdź do **aplikacje dla przedsiębiorstw** kliknięcie **wszystkie aplikacje**.

    ![Przypisz użytkownika][201] 

2. Na liście aplikacji zaznacz **Salesforce**.

    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-salesforce-tutorial/tutorial_salesforce_app.png) 

3. W menu po lewej stronie kliknij **użytkowników i grup**.

    ![Przypisz użytkownika][202] 

4. Kliknij przycisk **Dodaj** przycisku. Następnie wybierz **użytkowników i grup** na **Dodaj przydziału** okna dialogowego.

    ![Przypisz użytkownika][203]

5. Na **użytkowników i grup** okno dialogowe, wybierz opcję **Simona Britta** na liście Użytkownicy.

6. Kliknij przycisk **wybierz** znajdującego się na **użytkowników i grup** okna dialogowego.

7. Kliknij przycisk **przypisać** znajdującego się na **Dodaj przydziału** okna dialogowego.
    
### <a name="testing-single-sign-on"></a>Testowanie rejestracji jednokrotnej

Aby przetestować jednego ustawienia logowania jednokrotnego, otwórz Panel dostępu w [https://myapps.microsoft.com](https://myapps.microsoft.com/), następnie zaloguj się do konta testowego i kliknij przycisk **Salesforce**.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Lista samouczków dotyczących sposobów integracji aplikacji SaaS przy użyciu usługi Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co to jest dostęp do aplikacji i logowanie jednokrotne z usługą Azure Active Directory?](active-directory-appssoaccess-whatis.md)
* [Skonfiguruj Inicjowanie obsługi użytkowników](active-directory-saas-salesforce-provisioning-tutorial.md)

<!--Image references-->

[1]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_203.png

