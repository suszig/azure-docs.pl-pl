---
title: 'Samouczek: Integracji Azure Active Directory z piaskownicy Salesforce | Dokumentacja firmy Microsoft'
description: "Informacje o sposobie konfigurowania rejestracji jednokrotnej między usługą Azure Active Directory i usług Salesforce piaskownicy."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: ee54c39e-ce20-42a4-8531-da7b5f40f57c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: 90e08b9cf2feb93de4877bec9734352949896dca
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-salesforce-sandbox"></a>Samouczek: Integracji Azure Active Directory z piaskownicy usług Salesforce

Z tego samouczka dowiesz się integrowanie piaskownicy usługi Salesforce z usługą Azure Active Directory (Azure AD).

Integrowanie piaskownicy usługi Salesforce z usługą Azure AD zapewnia następujące korzyści:

- Można kontrolować w usłudze Azure AD, który ma dostęp do usług Salesforce piaskownicy
- Umożliwia użytkownikom automatycznie pobrać podpisany w przypadku piaskownicy Salesforce (logowanie jednokrotne) z konta usługi Azure AD
- Możesz zarządzać kont w jednej centralnej lokalizacji - portalu Azure

Jeśli chcesz dowiedzieć się więcej informacji o integracji aplikacji SaaS w usłudze Azure AD, zobacz [co to jest dostęp do aplikacji i logowanie jednokrotne z usługą Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Wymagania wstępne

Aby skonfigurować integrację usługi Azure AD z piaskownicy Salesforce, potrzebne są następujące elementy:

- Subskrypcję usługi Azure AD
- Piaskownica Salesforce jednokrotnego włączone subskrypcji

> [!NOTE]
> Aby przetestować kroki opisane w tym samouczku, zaleca się używania środowiska produkcyjnego.

Aby przetestować kroki opisane w tym samouczku, należy wykonać te zalecenia:

- Nie należy używać środowiska produkcyjnego, jeśli jest to konieczne.
- Jeśli nie masz środowisko wersji próbnej usługi Azure AD, możesz pobrać miesięczna wersja próbna [tutaj](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Opis scenariusza
W tym samouczku można przetestować usługę Azure AD rejestracji jednokrotnej w środowisku testowym. Scenariusz opisany w tym samouczku składa się z dwóch głównych elementów:

1. Dodawanie piaskownicy usługi Salesforce z galerii
2. Konfigurowanie i testowanie usługi Azure AD logowanie jednokrotne

## <a name="adding-salesforce-sandbox-from-the-gallery"></a>Dodawanie piaskownicy usługi Salesforce z galerii
Aby skonfigurować integrację piaskownicy usługi Salesforce z usługą Azure AD, należy dodać piaskownicy usługi Salesforce z galerii do listy zarządzanych aplikacji SaaS.

**Aby dodać piaskownicy usługi Salesforce z galerii, wykonaj następujące czynności:**

1. W  **[portalu Azure](https://portal.azure.com)**, na panelu nawigacyjnym po lewej stronie kliknij **usługi Azure Active Directory** ikony. 

    ![Usługa Active Directory][1]

2. Przejdź do **aplikacje dla przedsiębiorstw**. Następnie przejdź do **wszystkie aplikacje**.

    ![Aplikacje][2]
    
3. Kliknij przycisk **nowej aplikacji** przycisk w górnej części okna dialogowego.

    ![Aplikacje][3]

4. W polu wyszukiwania wpisz **piaskownicy Salesforce**.

    ![Tworzenie użytkownika testowego usługi Azure AD](./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_salesforcesandbox_search.png)

5. W panelu wyników wybierz **piaskownicy Salesforce**, a następnie kliknij przycisk **Dodaj** przycisk, aby dodać aplikację.

    ![Tworzenie użytkownika testowego usługi Azure AD](./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_salesforcesandbox_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurowanie i testowanie usługi Azure AD logowanie jednokrotne
W tej sekcji możesz skonfigurować i test usługi Azure AD rejestracji jednokrotnej z piaskownicy Salesforce oparte na użytkownika testowego o nazwie "Britta Simona".

Dla rejestracji jednokrotnej do pracy usługi Azure AD musi wiedzieć, użytkownik odpowiednika w piaskownicy Salesforce jest dla użytkownika, w usłudze Azure AD. Innymi słowy relację łącza między użytkownika usługi Azure AD i danemu użytkownikowi w piaskownicy Salesforce musi określone.

Ta relacja łącza zostanie nawiązane, przypisując wartość **nazwy użytkownika** w usłudze Azure AD jako wartość **Username** w piaskownicy Salesforce.

Do konfigurowania i testowania usługi Azure AD rejestracji jednokrotnej z piaskownicy Salesforce, należy wykonać poniższe bloki konstrukcyjne:

1. **[Konfigurowanie usługi Azure AD rejestracji jednokrotnej](#configuring-azure-ad-single-sign-on)**  — aby umożliwić użytkownikom korzystać z tej funkcji.
2. **[Tworzenie użytkownika testowego usługi Azure AD](#creating-an-azure-ad-test-user)**  — do przetestowania usługi Azure AD rejestracji jednokrotnej z Simona Britta.
3. **[Tworzenie użytkownika testowego piaskownicy Salesforce](#creating-a-salesforce-sandbox-test-user)**  — w celu zapewnienia odpowiednikiem Simona Britta piaskownicy Salesforce, połączonej z usługi Azure AD reprezentację użytkownika.
4. **[Przypisanie użytkownika testowego usługi Azure AD](#assigning-the-azure-ad-test-user)**  — aby umożliwić Simona Britta do użycia usługi Azure AD rejestracji jednokrotnej.
5. **[Testowanie rejestracji jednokrotnej](#testing-single-sign-on)**  — Aby sprawdzić, czy konfiguracja działa.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurowanie usługi Azure AD rejestracji jednokrotnej

W tej sekcji można włączyć usługi Azure AD rejestracji jednokrotnej w portalu Azure i skonfigurować logowanie jednokrotne do aplikacji Salesforce piaskownicy.

**Aby skonfigurować usługi Azure AD rejestracji jednokrotnej z piaskownicy Salesforce, wykonaj następujące czynności:**

1. W portalu Azure na **piaskownicy Salesforce** strona integracji aplikacji, kliknij przycisk **logowanie jednokrotne**.

    ![Konfigurowanie rejestracji jednokrotnej][4]

2. Na **logowanie jednokrotne** okno dialogowe, wybierz opcję **tryb** jako **na języku SAML logowania jednokrotnego** Aby włączyć logowanie jednokrotne.
 
    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_salesforcesandbox_samlbase.png)

3. Na **Salesforce piaskownicy domeny i adres URL** sekcji, wykonaj następujące czynności:

    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_salesforcesandbox_url.png)

     W **adres URL logowania** tekstowym, wpisz wartość, przy użyciu następującego wzorca:`https://<subdomain>.my.salesforce.com`

    > [!NOTE] 
    > Ta wartość nie jest rzeczywistym. Zaktualizuj tę wartość przy rzeczywisty adres URL logowania. Skontaktuj się z [zespołem pomocy technicznej klienta piaskownicy Salesforce](https://help.salesforce.com/support) aby zyskać tę wartość.


4. Jeśli skonfigurowano już program rejestracji jednokrotnej dla innego wystąpienia usług Salesforce piaskownicy w katalogu, a następnie należy także skonfigurować **identyfikator** mają taką samą wartość jak **Zaloguj się na adres URL**. 
    
    >[!Note]
    >**Identyfikator** można znaleźć pola sprawdzając **Pokaż zaawansowane ustawienia** wyboru na **Konfigurowanie adresu URL aplikacji** strony okna dialogowego 


5. Na **certyfikat podpisywania SAML** kliknij **certyfikatu** , a następnie zapisz plik certyfikatu na tym komputerze.

    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_salesforcesandbox_certificate.png) 

6. Kliknij przycisk **zapisać** przycisku.

    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_400.png)

7. Na **konfiguracji piaskownicy Salesforce** , kliknij przycisk **skonfigurować piaskownicy Salesforce** otworzyć **Konfigurowanie logowania jednokrotnego** okna. Kopiuj **identyfikator jednostki SAML i SAML pojedynczy znak na adres URL usługi** z **sekcji krótkimi opisami.**

    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_salesforcesandbox_configure.png) 
<CS>
8. Otwórz nową kartę w przeglądarce i zaloguj się do konta administratora usługi Salesforce piaskownicy.

9. W menu u góry kliknij **Instalatora**.

    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-salesforcesandbox-tutorial/IC781024.png)
10. W okienku nawigacji po lewej stronie kliknij **kontroli bezpieczeństwa**, a następnie kliknij przycisk **ustawień rejestracji jednokrotnej**.

    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-salesforcesandbox-tutorial/IC781025.png)
11. W sekcji Ustawienia rejestracji jednokrotnej, wykonaj następujące kroki: ![skonfigurować logowanie jednokrotne](./media/active-directory-saas-salesforcesandbox-tutorial/IC781026.png)
     
     a.  Wybierz **SAML włączone**. 

     b.  Kliknij przycisk **Nowy**.

12. W sekcji SAML jednego ustawienia rejestracji wykonaj następujące czynności:

    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-salesforcesandbox-tutorial/IC781027.png)

    a.In pole tekstowe Nazwa wpisz nazwę konfiguracji (np.: *SPSSOWAAD\_testu*). 

    b. Wklej **identyfikator jednostki SMAL** wartości do **wystawcy** pola tekstowego.

    c. W **identyfikator jednostki** pole tekstowe, typ **https://test.salesforce.com** Jeśli pierwsze wystąpienie piaskownicy Salesforce, które ma zostać dodane do katalogu. Jeśli masz już dodany wystąpienia usług Salesforce piaskownicy, następnie dla **identyfikator jednostki** wpisz **na adres URL logowania**, która powinna być w następującym formacie:`http://company.my.salesforce.com`  
 
    d. Kliknij przycisk **Przeglądaj** przekazywania pobranego certyfikatu.  

    e. Jako **typ tożsamości SAML**, wybierz pozycję **potwierdzenia zawiera identyfikator federacji z obiektu użytkownika**.
 
    f. Jako **lokalizacji tożsamości SAML**, wybierz pozycję **jest tożsamość w elemencie NameIdentifier instrukcji podmiotu**.

    g. Wklej **pojedynczy znak na adres URL usługi** do **adresu URL logowania do dostawcy tożsamości** pola tekstowego. 

    h. SFDC nie obsługuje SAML wylogowania.  Jako obejście, Wklej "https://login.microsoftonline.com/common/wsfederation?wa=wsignout1.0" go do **adres URL wylogowania dostawcy tożsamości** pola tekstowego.

    i. Jako **dostawcy zainicjował żądanie powiązania usługi**, wybierz pozycję **HTTP POST**. 

    j. Kliknij pozycję **Zapisz**.

### <a name="enable-your-domain"></a>Włącz domeny
W tej sekcji założono, że już utworzono domeny.  Aby uzyskać więcej informacji, zobacz [Definiowanie Twojej nazwy domeny](https://help.salesforce.com/HTViewHelpDoc?id=domain_name_define.htm&language=en_US).

**Aby włączyć domeny, wykonaj następujące czynności:**

1. W okienku nawigacji po lewej stronie kliknij **Zarządzanie domenami**, a następnie kliknij przycisk **Moje domeny.**
   
     ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-salesforcesandbox-tutorial/IC781029.png)
   
   >[!NOTE]
   >Upewnij się, że poprawnie skonfigurowano domenę. 

2. W **ustawienia strony logowania** kliknij **Edytuj**, następnie jako **usługi uwierzytelniania**, wybierz nazwę SAML pojedynczy znak na ustawienie z poprzedniej sekcji, a na koniec kliknij przycisk **zapisać**.
   
   ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-salesforcesandbox-tutorial/IC781030.png)

Jak masz domenę skonfigurowane, użytkownicy należy używać URL domeny na potrzeby logowania do usługi Salesforce piaskownicy.  

Aby uzyskać wartość adresu URL, kliknij profil rejestracji Jednokrotnej, utworzony w poprzedniej sekcji.    

> [!TIP]
> Teraz możesz przeczytać zwięzły wersji tych instrukcji wewnątrz [portalu Azure](https://portal.azure.com), podczas konfigurowania aplikacji!  Po dodaniu tej aplikacji z **usługi Active Directory > aplikacje dla przedsiębiorstw** po prostu kliknij **rejestracji jednokrotnej** karcie i dostęp do dokumentacji osadzonych za pomocą **konfiguracji** sekcji u dołu. Więcej o funkcji dokumentacji osadzonego w tym miejscu: [dokumentacji osadzonych usługi Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)


### <a name="creating-an-azure-ad-test-user"></a>Tworzenie użytkownika testowego usługi Azure AD
Celem tej sekcji jest tworzenie użytkownika testowego w portalu Azure o nazwie Simona Britta.

![Tworzenie użytkowników usługi Azure AD][100]

**Aby utworzyć użytkownika testowego w usłudze Azure AD, wykonaj następujące czynności:**

1. W **portalu Azure**, w lewym okienku nawigacji, kliknij polecenie **usługi Azure Active Directory** ikony.

    ![Tworzenie użytkownika testowego usługi Azure AD](./media/active-directory-saas-salesforcesandbox-tutorial/create_aaduser_01.png) 

2. Aby wyświetlić listę użytkowników, przejdź do **użytkowników i grup** i kliknij przycisk **wszyscy użytkownicy**.
    
    ![Tworzenie użytkownika testowego usługi Azure AD](./media/active-directory-saas-salesforcesandbox-tutorial/create_aaduser_02.png) 

3. W górnej części okna dialogowego, kliknij przycisk **Dodaj** otworzyć **użytkownika** okna dialogowego.
 
    ![Tworzenie użytkownika testowego usługi Azure AD](./media/active-directory-saas-salesforcesandbox-tutorial/create_aaduser_03.png) 

4. Na **użytkownika** okna dialogowego strony, należy wykonać następujące czynności:
 
    ![Tworzenie użytkownika testowego usługi Azure AD](./media/active-directory-saas-salesforcesandbox-tutorial/create_aaduser_04.png) 

    a. W **nazwa** pole tekstowe, typ **BrittaSimon**.

    b. W **nazwy użytkownika** pole tekstowe, typ **adres e-mail** z BrittaSimon.

    c. Wybierz **Pokaż hasło** i zanotuj wartość **hasło**.

    d. Kliknij przycisk **Utwórz**.
 
### <a name="creating-a-salesforce-sandbox-test-user"></a>Tworzenie użytkownika testowego piaskownicy usług Salesforce

W tej sekcji użytkownika o nazwie Simona Britta jest tworzony w piaskownicy Salesforce. Piaskownica SalesForce obsługę w czasie, który jest domyślnie włączona.
Nie ma elementu akcji można w tej sekcji. Jeśli użytkownik nie istnieje w piaskownicy Salesforce, nowy jest tworzony przy próbie uzyskania dostępu do piaskownicy Salesforce.

### <a name="assigning-the-azure-ad-test-user"></a>Przypisanie użytkownika testowego usługi Azure AD

W tej sekcji można włączyć Simona Britta do używania Azure logowania jednokrotnego za udzielanie dostępu do piaskownicy Salesforce.

![Przypisz użytkownika][200] 

**Aby przypisać Simona Britta piaskownicy Salesforce, wykonaj następujące czynności:**

1. W portalu Azure Otwórz widok aplikacji, a następnie przejdź do widoku katalogu i przejdź do **aplikacje dla przedsiębiorstw** kliknięcie **wszystkie aplikacje**.

    ![Przypisz użytkownika][201] 

2. Na liście aplikacji zaznacz **piaskownicy Salesforce**.

    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_salesforcesandbox_app.png) 

3. W menu po lewej stronie kliknij **użytkowników i grup**.

    ![Przypisz użytkownika][202] 

4. Kliknij przycisk **Dodaj** przycisku. Następnie wybierz **użytkowników i grup** na **Dodaj przydziału** okna dialogowego.

    ![Przypisz użytkownika][203]

5. Na **użytkowników i grup** okno dialogowe, wybierz opcję **Simona Britta** na liście Użytkownicy.

6. Kliknij przycisk **wybierz** znajdującego się na **użytkowników i grup** okna dialogowego.

7. Kliknij przycisk **przypisać** znajdującego się na **Dodaj przydziału** okna dialogowego.
    
### <a name="testing-single-sign-on"></a>Testowanie rejestracji jednokrotnej

Jeśli chcesz przetestować ustawienia logowania jednokrotnego, otwórz Panel dostępu. Aby uzyskać więcej informacji na temat panelu dostępu, zobacz [wprowadzenie do panelu dostępu](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Lista samouczków dotyczących sposobów integracji aplikacji SaaS przy użyciu usługi Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co to jest dostęp do aplikacji i logowanie jednokrotne z usługą Azure Active Directory?](active-directory-appssoaccess-whatis.md)
* [Skonfiguruj Inicjowanie obsługi użytkowników](active-directory-saas-salesforce-sandbox-provisioning-tutorial.md)



<!--Image references-->

[1]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_203.png

