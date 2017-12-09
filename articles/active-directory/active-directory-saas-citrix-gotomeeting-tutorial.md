---
title: 'Samouczek: Integracji Azure Active Directory z GoToMeeting | Dokumentacja firmy Microsoft'
description: "Informacje o sposobie konfigurowania rejestracji jednokrotnej między usługą Azure Active Directory i GoToMeeting."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: bcaf19f2-5809-4e1c-acbc-21a8d3498ccf
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/20/2017
ms.author: jeedes
ms.openlocfilehash: 44a1a7d1c3116af71e07f9579dd8b7570693b813
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/08/2017
---
# <a name="tutorial-azure-active-directory-integration-with-gotomeeting"></a>Samouczek: Integracji Azure Active Directory z GoToMeeting

Z tego samouczka dowiesz się integrowanie GoToMeeting z usługi Azure Active Directory (Azure AD).

Integracja z usługą Azure AD GoToMeeting zapewnia następujące korzyści:

- Można kontrolować w usłudze Azure AD, który ma dostęp do GoToMeeting.
- Umożliwia użytkownikom automatycznie pobrać zalogowane do GoToMeeting (logowanie jednokrotne) przy użyciu ich kont usługi Azure AD.
- Możesz zarządzać kont w jednej centralnej lokalizacji - portalu Azure.

Jeśli chcesz dowiedzieć się więcej informacji o integracji aplikacji SaaS w usłudze Azure AD, zobacz [co to jest dostęp do aplikacji i logowanie jednokrotne z usługą Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Wymagania wstępne

Aby skonfigurować integrację usługi Azure AD z GoToMeeting, potrzebne są następujące elementy:

- Subskrypcję usługi Azure AD
- GoToMeeting logowanie jednokrotne włączone subskrypcji

> [!NOTE]
> Aby przetestować kroki opisane w tym samouczku, zaleca się używania środowiska produkcyjnego.

Aby przetestować kroki opisane w tym samouczku, należy wykonać te zalecenia:

- Nie należy używać środowiska produkcyjnego, jeśli jest to konieczne.
- Jeśli nie masz środowisko wersji próbnej usługi Azure AD, możesz [uzyskać miesięczna wersja próbna](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Opis scenariusza
W tym samouczku można przetestować usługę Azure AD rejestracji jednokrotnej w środowisku testowym. Scenariusz opisany w tym samouczku składa się z dwóch głównych elementów:

1. Dodawanie GoToMeeting z galerii
2. Konfigurowanie i testowanie usługi Azure AD logowanie jednokrotne

## <a name="adding-gotomeeting-from-the-gallery"></a>Dodawanie GoToMeeting z galerii
Aby skonfigurować integrację usługi Azure AD GoToMeeting, należy dodać GoToMeeting z galerii do listy zarządzanych aplikacji SaaS.

**Aby dodać GoToMeeting z galerii, wykonaj następujące czynności:**

1. W  **[portalu Azure](https://portal.azure.com)**, na panelu nawigacyjnym po lewej stronie kliknij **usługi Azure Active Directory** ikony. 

    ![Przycisk usługi Azure Active Directory][1]

2. Przejdź do **aplikacje dla przedsiębiorstw**. Następnie przejdź do **wszystkie aplikacje**.

    ![Blok aplikacje przedsiębiorstwa][2]
    
3. Aby dodać nową aplikację, kliknij przycisk **nowej aplikacji** przycisk w górnej części okna dialogowego.

    ![Nowy przycisk aplikacji][3]

4. W polu wyszukiwania wpisz **GoToMeeting**, wybierz pozycję **GoToMeeting** z panelu wyników kliknięcie **Dodaj** przycisk, aby dodać aplikację.

    ![GoToMeeting na liście wyników](./media/active-directory-saas-gotomeeting-tutorial/tutorial_gotomeeting_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfiguracja i testowanie usługi Azure AD rejestracji jednokrotnej

W tej sekcji skonfigurować i przetestować usługi Azure AD rejestracji jednokrotnej z GoToMeeting w oparciu o nazwie "Britta Simona" użytkownika testowego.

Dla rejestracji jednokrotnej do pracy usługi Azure AD musi wiedzieć, użytkownik odpowiednika w GoToMeeting jest dla użytkownika, w usłudze Azure AD. Innymi słowy link relację między użytkownikiem usługi Azure AD i danemu użytkownikowi w GoToMeeting musi się.

W GoToMeeting, należy przypisać wartość **nazwy użytkownika** w usłudze Azure AD jako wartość **Username** do ustanawiania relacji łącza.

Aby skonfigurować i przetestować usługi Azure AD rejestracji jednokrotnej z GoToMeeting, należy wykonać poniższe bloki konstrukcyjne:

1. **[Konfigurowanie usługi Azure AD rejestracji jednokrotnej](#configure-azure-ad-single-sign-on)**  — aby umożliwić użytkownikom korzystać z tej funkcji.
2. **[Tworzenie użytkownika testowego usługi Azure AD](#create-an-azure-ad-test-user)**  — do przetestowania usługi Azure AD rejestracji jednokrotnej z Simona Britta.
3. **[Tworzenie użytkownika testowego GoToMeeting](#create-a-gotomeeting-test-user)**  — w celu zapewnienia odpowiednikiem Simona Britta GoToMeeting połączonego z usługi Azure AD reprezentację użytkownika.
4. **[Przypisz użytkownika testowego usługi Azure AD](#assign-the-azure-ad-test-user)**  — aby umożliwić Simona Britta do użycia usługi Azure AD rejestracji jednokrotnej.
5. **[Test rejestracji jednokrotnej](#test-single-sign-on)**  — Aby sprawdzić, czy konfiguracja działa.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurowanie usługi Azure AD rejestracji jednokrotnej

W tej sekcji można włączyć usługi Azure AD rejestracji jednokrotnej w portalu Azure i skonfigurować logowanie jednokrotne w aplikacji GoToMeeting.

**Aby skonfigurować usługi Azure AD rejestracji jednokrotnej z GoToMeeting, wykonaj następujące czynności:**

1. W portalu Azure na **GoToMeeting** strona integracji aplikacji, kliknij przycisk **logowanie jednokrotne**.

    ![Skonfigurować łącze rejestracji jednokrotnej][4]

2. Na **logowanie jednokrotne** okno dialogowe, wybierz opcję **tryb** jako **na języku SAML logowania jednokrotnego** Aby włączyć logowanie jednokrotne.
 
    ![Okno dialogowe rejestracji jednokrotnej](./media/active-directory-saas-gotomeeting-tutorial/tutorial_gotomeeting_samlbase.png)

3. Na **GoToMeeting domeny i adres URL** kliknij **Pokaż zaawansowane ustawienia adresu URL** i wykonywanie następujących czynności -

    ![Adresy URL i domeny GoToMeeting pojedynczy informacje logowania jednokrotnego](./media/active-directory-saas-gotomeeting-tutorial/tutorial_gotomeeting_ssourl.png)

    a. W **identyfikator** tekstowym, wpisz adres URL:`https://authentication.logmeininc.com/saml/sp`

    b. W **adres URL odpowiedzi** tekstowym, wpisz adres URL:`https://authentication.logmeininc.com/saml/acs`

    c. W **stan przekazywania** tekstowym, wpisz jedno z następujących adresów URL:

    **Dla GoToMeeting**:`https://global.gotomeeting.com`
    
    **Dla GoToTraining**:`https://global.gototraining.com`

    **Dla GoToWebinar**:`https://global.gotowebinar.com`

    **Dla GoToAssist**:`https://app.gotoassist.com`


4. Na **certyfikat podpisywania SAML** kliknij **XML metadanych** , a następnie zapisz plik metadanych na tym komputerze.

    ![Łącze pobierania certyfikatu](./media/active-directory-saas-gotomeeting-tutorial/tutorial_gotomeeting_certificate.png) 

5. Kliknij przycisk **zapisać** przycisku.

    ![Skonfiguruj przycisk pojedynczego logowania jednokrotnego Zapisz](./media/active-directory-saas-gotomeeting-tutorial/tutorial_general_400.png)

6. Aby wygenerować **metadanych** adres url, wykonaj następujące czynności:

    a. Kliknij przycisk **rejestracji aplikacji**.
    
    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-gotomeeting-tutorial/tutorial_gotomeeting_appregistrations.png)
   
    b. Kliknij przycisk **punkty końcowe** otworzyć **punkty końcowe** okno dialogowe.  
    
    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-gotomeeting-tutorial/tutorial_gotomeeting_endpointicon.png)

    c. Kliknij przycisk Kopiuj, aby skopiować **dokument METADANYCH usług FEDERACYJNYCH** adresu url i wklej go do Notatnika.
    
    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-gotomeeting-tutorial/tutorial_gotomeeting_endpoint.png)
     
    d. Teraz przejdź do strony właściwości **GoToMeeting** i skopiuj **identyfikator aplikacji** przy użyciu **kopiowania** przycisk i wklej go do Notatnika.
 
    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-gotomeeting-tutorial/tutorial_gotomeeting_appid.png)

    e. Generowanie **adres URL metadanych** przy użyciu następującego wzorca:`<FEDERATION METADATA DOCUMENT url>?appid=<application id>`   

7. Na **konfiguracji GoToMeeting** , kliknij przycisk **skonfigurować GoToMeeting** otworzyć **Konfigurowanie logowania jednokrotnego** okna. Kopiuj **Sign-Out adres URL, identyfikator jednostki SAML i SAML pojedynczy znak na adres URL usługi** z **sekcji krótkimi opisami.**

    ![Konfiguracja GoToMeeting](./media/active-directory-saas-gotomeeting-tutorial/tutorial_gotomeeting_configure.png) 

8. W oknie innej przeglądarki, zaloguj się do Twojego [GoToMeeting organizacji Center](https://account.citrixonline.com/organization/administration/)

9. W obszarze **dostawcy tożsamości** kartę, można skonfigurować ustawienia usługi Azure albo przez podanie wygenerowany **adres URL metadanych** lub pobrany **pliku metadanych** lub **Ręcznego**.

10. Aby uzyskać **adres URL metadanych** należy wykonać następujące czynności:

    ![Konfiguracja GoToMeeting](./media/active-directory-saas-gotomeeting-tutorial/config1.png)

    a. W **jak chcesz skonfigurować Twoje IDP SAML?**, wybierz pozycję **automatyczne** z listy rozwijanej.

    b. Wklej **adres URL metadanych**, które zostały wygenerowane w poprzednich krokach w **adres URL metadanych** pola tekstowego.

    c. Kliknij pozycję **Zapisz**.

11. Aby uzyskać **pliku metadanych** należy wykonać następujące czynności:

    ![Konfiguracja GoToMeeting](./media/active-directory-saas-gotomeeting-tutorial/config2.png)

    a. W **jak chcesz skonfigurować Twoje IDP SAML?**, wybierz pozycję **SAML przekazać plik metadanych** z listy rozwijanej.

    b. Aby przekazać plik metadanych pobranych, kliknij przycisk **przekazywania pliku metadanych**.

    c. Kliknij pozycję **Zapisz**.

12. Aby uzyskać **ręcznego** należy wykonać następujące czynności:

    ![Konfiguracja GoToMeeting](./media/active-directory-saas-gotomeeting-tutorial/config3.png)

    a.  W **adres URL logowania strony** pole tekstowe, Wklej wartość **SAML pojedynczy znak na adres URL usługi** którego została skopiowana z portalu Azure.

    b.  W **adres URL strony wylogowania** pole tekstowe, Wklej wartość **Sign-Out URL** którego została skopiowana z portalu Azure.

    c.  W **identyfikator jednostki dostawcy tożsamości** pole tekstowe, Wklej wartość **identyfikator jednostki SAML** którego została skopiowana z portalu Azure.

    d. Wyodrębnij certyfikacie x 509 z pliku metadanych pobranych i przekaż certyfikat tego klikając **przekazywania certyfikatu**.

    e. Kliknij pozycję **Zapisz**.

> [!TIP]
> Teraz możesz przeczytać zwięzły wersji tych instrukcji wewnątrz [portalu Azure](https://portal.azure.com), podczas konfigurowania aplikacji!  Po dodaniu tej aplikacji z **usługi Active Directory > aplikacje dla przedsiębiorstw** po prostu kliknij **rejestracji jednokrotnej** karcie i dostęp do dokumentacji osadzonych za pomocą **konfiguracji** sekcji u dołu. Więcej o funkcji dokumentacji osadzonego w tym miejscu: [dokumentacji osadzonych usługi Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Tworzenie użytkownika testowego usługi Azure AD

Celem tej sekcji jest tworzenie użytkownika testowego w portalu Azure o nazwie Simona Britta.

   ![Tworzenie użytkownika testowego usługi Azure AD][100]

**Aby utworzyć użytkownika testowego w usłudze Azure AD, wykonaj następujące czynności:**

1. W portalu Azure, w okienku po lewej stronie kliknij **usługi Azure Active Directory** przycisku.

    ![Przycisk usługi Azure Active Directory](./media/active-directory-saas-gotomeeting-tutorial/create_aaduser_01.png)

2. Aby wyświetlić listę użytkowników, przejdź do **użytkowników i grup**, a następnie kliknij przycisk **wszyscy użytkownicy**.

    !["Użytkownicy i grupy" i "Wszyscy użytkownicy" łącza](./media/active-directory-saas-gotomeeting-tutorial/create_aaduser_02.png)

3. Aby otworzyć **użytkownika** okno dialogowe, kliknij przycisk **Dodaj** w górnej części **wszyscy użytkownicy** okno dialogowe.

    ![Przycisk Dodaj](./media/active-directory-saas-gotomeeting-tutorial/create_aaduser_03.png)

4. W **użytkownika** okna dialogowego wykonaj następujące czynności:

    ![Okno dialogowe użytkownika](./media/active-directory-saas-gotomeeting-tutorial/create_aaduser_04.png)

    a. W **nazwa** wpisz **BrittaSimon**.

    b. W **nazwy użytkownika** wpisz adres e-mail użytkownika Simona Britta.

    c. Wybierz **Pokaż hasło** pole wyboru, a następnie zanotuj wartość, która jest wyświetlana w **hasło** pole.

    d. Kliknij przycisk **Utwórz**.
 
### <a name="create-a-gotomeeting-test-user"></a>Tworzenie użytkownika testowego GoToMeeting

W tej sekcji użytkownika o nazwie Simona Britta jest tworzony w GoToMeeting. GoToMeeting obsługę w czasie, który jest domyślnie włączona.

Nie ma elementu akcji można w tej sekcji. Jeśli użytkownik nie istnieje w GoToMeeting, nowy jest tworzony podczas próby uzyskania dostępu GoToMeeting.

> [!NOTE]
> Jeśli trzeba ręcznie utworzyć użytkownika, skontaktuj się z [zespołem pomocy technicznej GoToMeeting](https://support.logmeininc.com/gotomeeting).

### <a name="assign-the-azure-ad-test-user"></a>Przypisz użytkownika testowego usługi Azure AD

W tej sekcji można włączyć Simona Britta do używania Azure logowania jednokrotnego za udzielanie dostępu GoToMeeting.

![Przypisanie roli użytkownika][200] 

**Aby przypisać Simona Britta GoToMeeting, wykonaj następujące czynności:**

1. W portalu Azure Otwórz widok aplikacji, a następnie przejdź do widoku katalogu i przejdź do **aplikacje dla przedsiębiorstw** kliknięcie **wszystkie aplikacje**.

    ![Przypisz użytkownika][201] 

2. Na liście aplikacji zaznacz **GoToMeeting**.

    ![Łącze GoToMeeting na liście aplikacji](./media/active-directory-saas-gotomeeting-tutorial/tutorial_gotomeeting_app.png)  

3. W menu po lewej stronie kliknij **użytkowników i grup**.

    ![Łącze "Użytkownicy i grupy"][202]

4. Kliknij przycisk **Dodaj** przycisku. Następnie wybierz **użytkowników i grup** na **Dodaj przydziału** okna dialogowego.

    ![W okienku Dodaj przydziału][203]

5. Na **użytkowników i grup** okno dialogowe, wybierz opcję **Simona Britta** na liście Użytkownicy.

6. Kliknij przycisk **wybierz** znajdującego się na **użytkowników i grup** okna dialogowego.

7. Kliknij przycisk **przypisać** znajdującego się na **Dodaj przydziału** okna dialogowego.
    
### <a name="test-single-sign-on"></a>Test rejestracji jednokrotnej

W tej sekcji można przetestować konfiguracji usługi Azure AD pojedynczego logowania za pomocą panelu dostępu.

Po kliknięciu kafelka GoToMeeting w panelu dostępu użytkownik powinien pobrać automatycznie zalogowane do aplikacji GoToMeeting.
Aby uzyskać więcej informacji na temat panelu dostępu, zobacz [wprowadzenie do panelu dostępu](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Lista samouczków dotyczących sposobów integracji aplikacji SaaS przy użyciu usługi Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co to jest dostęp do aplikacji i logowanie jednokrotne z usługą Azure Active Directory?](active-directory-appssoaccess-whatis.md)
* [Skonfiguruj Inicjowanie obsługi użytkowników](https://docs.microsoft.com/azure/active-directory/active-directory-saas-citrixgotomeeting-provisioning-tutorial)


<!--Image references-->

[1]: ./media/active-directory-saas-gotomeeting-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-gotomeeting-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-gotomeeting-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-gotomeeting-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-gotomeeting-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-gotomeeting-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-gotomeeting-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-gotomeeting-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-gotomeeting-tutorial/tutorial_general_203.png

