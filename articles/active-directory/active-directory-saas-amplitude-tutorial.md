---
title: 'Samouczek: Integracji Azure Active Directory z amplitudy | Dokumentacja firmy Microsoft'
description: Informacje o sposobie konfigurowania rejestracji jednokrotnej między amplitudy i Azure Active Directory.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 496c9ffa-c833-41fa-8d17-2dc3044954d1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/16/2018
ms.author: jeedes
ms.openlocfilehash: 5bcf666774876a6def48766f93066657862536df
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/23/2018
---
# <a name="tutorial-azure-active-directory-integration-with-amplitude"></a>Samouczek: Integracji Azure Active Directory z amplitudy

Z tego samouczka dowiesz się integrowanie amplitudy w usłudze Azure Active Directory (Azure AD).

Integracja z usługą Azure AD amplitudy zapewnia następujące korzyści:

- Można kontrolować w usłudze Azure AD, kto ma dostęp do amplitudy.
- Umożliwia użytkownikom automatycznie pobrać zalogowane do amplitudy (logowanie jednokrotne) przy użyciu ich kont usługi Azure AD.
- Możesz zarządzać kont w jednej centralnej lokalizacji - portalu Azure.

Jeśli chcesz dowiedzieć się więcej informacji o integracji aplikacji SaaS w usłudze Azure AD, zobacz [co to jest dostęp do aplikacji i logowanie jednokrotne z usługą Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Wymagania wstępne

Aby skonfigurować integrację usługi Azure AD z amplitudy, potrzebne są następujące elementy:

- Subskrypcję usługi Azure AD
- Amplitudy logowanie jednokrotne włączone subskrypcji

> [!NOTE]
> Aby przetestować kroki opisane w tym samouczku, zaleca się używania środowiska produkcyjnego.

Aby przetestować kroki opisane w tym samouczku, należy wykonać te zalecenia:

- Nie należy używać środowiska produkcyjnego, jeśli jest to konieczne.
- Jeśli nie masz środowisko wersji próbnej usługi Azure AD, możesz [uzyskać miesięczna wersja próbna](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Opis scenariusza
W tym samouczku można przetestować usługę Azure AD rejestracji jednokrotnej w środowisku testowym. Scenariusz opisany w tym samouczku składa się z dwóch głównych elementów:

1. Dodawanie amplitudy z galerii
2. Konfigurowanie i testowanie usługi Azure AD logowanie jednokrotne

## <a name="adding-amplitude-from-the-gallery"></a>Dodawanie amplitudy z galerii
Aby skonfigurować integrację usługi Azure AD amplitudy, należy dodać amplitudy z galerii do listy zarządzanych aplikacji SaaS.

**Aby dodać amplitudy z galerii, wykonaj następujące czynności:**

1. W  **[portalu Azure](https://portal.azure.com)**, na panelu nawigacyjnym po lewej stronie kliknij **usługi Azure Active Directory** ikony. 

    ![Przycisk usługi Azure Active Directory][1]

2. Przejdź do **aplikacje dla przedsiębiorstw**. Następnie przejdź do **wszystkie aplikacje**.

    ![Blok aplikacje przedsiębiorstwa][2]
    
3. Aby dodać nową aplikację, kliknij przycisk **nowej aplikacji** przycisk w górnej części okna dialogowego.

    ![Nowy przycisk aplikacji][3]

4. W polu wyszukiwania wpisz **amplitudy**, wybierz pozycję **amplitudy** z panelu wyników kliknięcie **Dodaj** przycisk, aby dodać aplikację.

    ![Amplitudy na liście wyników](./media/active-directory-saas-amplitude-tutorial/tutorial_amplitude_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfiguracja i testowanie usługi Azure AD rejestracji jednokrotnej

W tej sekcji skonfigurować i przetestować usługi Azure AD rejestracji jednokrotnej z amplitudy w oparciu o nazwie "Britta Simona" użytkownika testowego.

Dla rejestracji jednokrotnej do pracy usługi Azure AD musi wiedzieć, użytkownik odpowiednika w amplitudy jest dla użytkownika, w usłudze Azure AD. Innymi słowy link relację między użytkownikiem usługi Azure AD i danemu użytkownikowi w amplitudy musi się.

Aby skonfigurować i przetestować usługi Azure AD rejestracji jednokrotnej z amplitudy, należy wykonać poniższe bloki konstrukcyjne:

1. **[Konfigurowanie usługi Azure AD rejestracji jednokrotnej](#configure-azure-ad-single-sign-on)**  — aby umożliwić użytkownikom korzystać z tej funkcji.
2. **[Tworzenie użytkownika testowego usługi Azure AD](#create-an-azure-ad-test-user)**  — do przetestowania usługi Azure AD rejestracji jednokrotnej z Simona Britta.
3. **[Tworzenie użytkownika testowego amplitudy](#create-an-amplitude-test-user)**  — w celu zapewnienia odpowiednikiem Simona Britta amplitudy połączonego z usługi Azure AD reprezentację użytkownika.
4. **[Przypisz użytkownika testowego usługi Azure AD](#assign-the-azure-ad-test-user)**  — aby umożliwić Simona Britta do użycia usługi Azure AD rejestracji jednokrotnej.
5. **[Test rejestracji jednokrotnej](#test-single-sign-on)**  — Aby sprawdzić, czy konfiguracja działa.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurowanie usługi Azure AD rejestracji jednokrotnej

W tej sekcji można włączyć usługi Azure AD rejestracji jednokrotnej w portalu Azure i skonfigurować logowanie jednokrotne w aplikacji amplitudy.

**Aby skonfigurować usługi Azure AD rejestracji jednokrotnej z amplitudy, wykonaj następujące czynności:**

1. W portalu Azure na **amplitudy** strona integracji aplikacji, kliknij przycisk **logowanie jednokrotne**.

    ![Skonfigurować łącze rejestracji jednokrotnej][4]

2. Na **logowanie jednokrotne** okno dialogowe, wybierz opcję **tryb** jako **na języku SAML logowania jednokrotnego** Aby włączyć logowanie jednokrotne.
 
    ![Okno dialogowe rejestracji jednokrotnej](./media/active-directory-saas-amplitude-tutorial/tutorial_amplitude_samlbase.png)

3. Na **amplitudy domeny i adres URL** sekcji, wykonaj następujące kroki, aby skonfigurować aplikację w **IDP** inicjowane tryb:

    ![Adresy URL i domeny amplitudy pojedynczy informacje logowania jednokrotnego](./media/active-directory-saas-amplitude-tutorial/tutorial_amplitude_url.png)

    a. W **identyfikator** tekstowym, wpisz adres URL: `https://amplitude.com/saml/sso/metadata`

    b. W **adres URL odpowiedzi** tekstowym, wpisz adres URL, używając następującego wzorca: `https://analytics.amplitude.com/saml/sso/<uniqueid>`

    > [!NOTE]
    > Wartość adresu URL odpowiedzi nie jest prawdziwe. Wartość adresu URL odpowiedzi wystąpi w dalszej części tego samouczka.

4. Sprawdź **Pokaż zaawansowane ustawienia adresu URL** i wykonać następujący krok, jeśli chcesz skonfigurować aplikację w **SP** inicjowane tryb:

    ![Adresy URL i domeny amplitudy pojedynczy informacje logowania jednokrotnego](./media/active-directory-saas-amplitude-tutorial/tutorial_amplitude_url1.png)

    W **adres URL logowania** tekstowym, wpisz adres URL: `https://analytics.amplitude.com/sso`

5. Na **certyfikat podpisywania SAML** kliknij **XML metadanych** , a następnie zapisz plik metadanych na tym komputerze.

    ![Łącze pobierania certyfikatu](./media/active-directory-saas-amplitude-tutorial/tutorial_amplitude_certificate.png) 

6. Kliknij przycisk **zapisać** przycisku.

    ![Skonfiguruj przycisk pojedynczego logowania jednokrotnego Zapisz](./media/active-directory-saas-amplitude-tutorial/tutorial_general_400.png)
    
7. Logowanie do amplitudy witryny firmy jako administrator.

8. Polecenie **planowanie Admin** na pasku nawigacyjnym po lewej stronie.

    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-amplitude-tutorial/configure1.png)

9. Wybierz **platformy Microsoft Azure Active Directory metadanych** z **integracji logowania jednokrotnego**.

    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-amplitude-tutorial/configure2.png)

10. Na **ustawić zapasowej rejestracji jednokrotnej** sekcji, wykonaj następujące czynności:

    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-amplitude-tutorial/configure3.png)

    a. Otwórz pobrany **Xml metadanych** z portalu Azure w programie Notatnik, wkleić zawartości do **Microsoft Azure Active Directory metadanych** pola tekstowego.

    b. Kopiuj **adres URL odpowiedzi (ACS)** i wklej go do **adres URL odpowiedzi** textbox sekcji amplitudy domeny i adresy URL w portalu Azure.

    c. Kliknij pozycję **Zapisz**

> [!TIP]
> Teraz możesz przeczytać zwięzły wersji tych instrukcji wewnątrz [portalu Azure](https://portal.azure.com), podczas konfigurowania aplikacji!  Po dodaniu tej aplikacji z **usługi Active Directory > aplikacje dla przedsiębiorstw** po prostu kliknij **rejestracji jednokrotnej** karcie i dostęp do dokumentacji osadzonych za pomocą **konfiguracji** sekcji u dołu. Więcej o funkcji dokumentacji osadzonego w tym miejscu: [dokumentacji osadzonych usługi Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Tworzenie użytkownika testowego usługi Azure AD

Celem tej sekcji jest tworzenie użytkownika testowego w portalu Azure o nazwie Simona Britta.

   ![Tworzenie użytkownika testowego usługi Azure AD][100]

**Aby utworzyć użytkownika testowego w usłudze Azure AD, wykonaj następujące czynności:**

1. W portalu Azure, w okienku po lewej stronie kliknij **usługi Azure Active Directory** przycisku.

    ![Przycisk usługi Azure Active Directory](./media/active-directory-saas-amplitude-tutorial/create_aaduser_01.png)

2. Aby wyświetlić listę użytkowników, przejdź do **użytkowników i grup**, a następnie kliknij przycisk **wszyscy użytkownicy**.

    !["Użytkownicy i grupy" i "Wszyscy użytkownicy" łącza](./media/active-directory-saas-amplitude-tutorial/create_aaduser_02.png)

3. Aby otworzyć **użytkownika** okno dialogowe, kliknij przycisk **Dodaj** w górnej części **wszyscy użytkownicy** okno dialogowe.

    ![Przycisk Dodaj](./media/active-directory-saas-amplitude-tutorial/create_aaduser_03.png)

4. W **użytkownika** okna dialogowego wykonaj następujące czynności:

    ![Okno dialogowe użytkownika](./media/active-directory-saas-amplitude-tutorial/create_aaduser_04.png)

    a. W **nazwa** wpisz **BrittaSimon**.

    b. W **nazwy użytkownika** wpisz adres e-mail użytkownika Simona Britta.

    c. Wybierz **Pokaż hasło** pole wyboru, a następnie zanotuj wartość, która jest wyświetlana w **hasło** pole.

    d. Kliknij przycisk **Utwórz**.
 
### <a name="create-an-amplitude-test-user"></a>Tworzenie użytkownika testowego amplitudy

Celem tej sekcji jest utworzenie użytkownika o nazwie Simona Britta w amplitudy. Amplitudy obsługę w czasie, który jest domyślnie włączone. Nie ma elementu akcji można w tej sekcji. Nowy użytkownik został utworzony podczas próby dostępu amplitudy, jeśli go jeszcze nie istnieje.
>[!Note]
>Jeśli trzeba ręcznie utworzyć użytkownika, skontaktuj się z [zespołem pomocy technicznej amplitudy](https://amplitude.zendesk.com).

### <a name="assign-the-azure-ad-test-user"></a>Przypisz użytkownika testowego usługi Azure AD

W tej sekcji musisz włączyć Simona Britta do użycia usługi Azure logowania jednokrotnego za udzielanie dostępu do amplitudy.

![Przypisanie roli użytkownika][200] 

**Aby przypisać Simona Britta do amplitudy, wykonaj następujące czynności:**

1. W portalu Azure Otwórz widok aplikacji, a następnie przejdź do widoku katalogu i przejdź do **aplikacje dla przedsiębiorstw** kliknięcie **wszystkie aplikacje**.

    ![Przypisz użytkownika][201] 

2. Na liście aplikacji zaznacz **amplitudy**.

    ![Łącze amplitudy na liście aplikacji](./media/active-directory-saas-amplitude-tutorial/tutorial_amplitude_app.png)  

3. W menu po lewej stronie kliknij **użytkowników i grup**.

    ![Łącze "Użytkownicy i grupy"][202]

4. Kliknij przycisk **Dodaj** przycisku. Następnie wybierz **użytkowników i grup** na **Dodaj przydziału** okna dialogowego.

    ![W okienku Dodaj przydziału][203]

5. Na **użytkowników i grup** okno dialogowe, wybierz opcję **Simona Britta** na liście Użytkownicy.

6. Kliknij przycisk **wybierz** znajdującego się na **użytkowników i grup** okna dialogowego.

7. Kliknij przycisk **przypisać** znajdującego się na **Dodaj przydziału** okna dialogowego.
    
### <a name="test-single-sign-on"></a>Test rejestracji jednokrotnej

W tej sekcji można przetestować konfiguracji usługi Azure AD pojedynczego logowania za pomocą panelu dostępu.

Po kliknięciu kafelka amplitudy w panelu dostępu użytkownik powinien pobrać automatycznie zalogowane do amplitudy aplikacji.
Aby uzyskać więcej informacji na temat panelu dostępu, zobacz [wprowadzenie do panelu dostępu](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Zasoby dodatkowe

* [Lista samouczków dotyczących sposobów integracji aplikacji SaaS przy użyciu usługi Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co to jest dostęp do aplikacji i logowanie jednokrotne z usługą Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-amplitude-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-amplitude-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-amplitude-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-amplitude-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-amplitude-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-amplitude-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-amplitude-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-amplitude-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-amplitude-tutorial/tutorial_general_203.png

