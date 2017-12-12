---
title: "Samouczek: Azure Active Directory integracji z menedżerem środowisko Adobe | Dokumentacja firmy Microsoft"
description: "Informacje o sposobie konfigurowania rejestracji jednokrotnej między Azure Active Directory i Adobe środowisko Manager."
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 88a95bb5-c17c-474f-bb92-1f80f5344b5a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2017
ms.author: jeedes
ms.openlocfilehash: 4a4fccc4210fd6cf0ddbe99089c84a1fd38d5b09
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-adobe-experience-manager"></a>Samouczek: Azure Active Directory integracji z menedżerem środowisko Adobe

Z tego samouczka dowiesz się integrowanie Adobe środowisko Manager z usługą Azure Active Directory (Azure AD).

Integrowanie programu Adobe środowisko Manager z usługą Azure AD zapewnia następujące korzyści:

- Można kontrolować w usłudze Azure AD, kto ma dostęp do programu Adobe środowisko Manager.
- Umożliwia użytkownikom automatycznie pobrać zalogowane Adobe obsługi Menedżera (logowanie jednokrotne) z konta usługi Azure AD.
- Możesz zarządzać kont w jednej centralnej lokalizacji - portalu Azure.

Jeśli chcesz dowiedzieć się więcej informacji o integracji aplikacji SaaS w usłudze Azure AD, zobacz [co to jest dostęp do aplikacji i logowanie jednokrotne z usługą Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Wymagania wstępne

Aby skonfigurować integrację usługi Azure AD z menedżerem środowisko Adobe, potrzebne są następujące elementy:

- Subskrypcję usługi Azure AD
- Menedżera środowisko Adobe logowanie jednokrotne włączone subskrypcji

> [!NOTE]
> Aby przetestować kroki opisane w tym samouczku, zaleca się używania środowiska produkcyjnego.

Aby przetestować kroki opisane w tym samouczku, należy wykonać te zalecenia:

- Nie należy używać środowiska produkcyjnego, jeśli jest to konieczne.
- Jeśli nie masz środowisko wersji próbnej usługi Azure AD, możesz [uzyskać miesięczna wersja próbna](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Opis scenariusza
W tym samouczku można przetestować usługę Azure AD rejestracji jednokrotnej w środowisku testowym. Scenariusz opisany w tym samouczku składa się z dwóch głównych elementów:

1. Dodawanie menedżera środowisko Adobe z galerii
2. Konfigurowanie i testowanie usługi Azure AD logowanie jednokrotne

## <a name="adding-adobe-experience-manager-from-the-gallery"></a>Dodawanie menedżera środowisko Adobe z galerii
Aby skonfigurować integrację programu Adobe środowisko Manager z usługą Azure AD, należy dodać Menedżera środowisko Adobe z galerii do listy zarządzanych aplikacji SaaS.

**Aby dodać Menedżera środowisko Adobe z galerii, wykonaj następujące czynności:**

1. W  **[portalu Azure](https://portal.azure.com)**, na panelu nawigacyjnym po lewej stronie kliknij **usługi Azure Active Directory** ikony. 

    ![Przycisk usługi Azure Active Directory][1]

2. Przejdź do **aplikacje dla przedsiębiorstw**. Następnie przejdź do **wszystkie aplikacje**.

    ![Blok aplikacje przedsiębiorstwa][2]
    
3. Aby dodać nową aplikację, kliknij przycisk **nowej aplikacji** przycisk w górnej części okna dialogowego.

    ![Nowy przycisk aplikacji][3]

4. W polu wyszukiwania wpisz **Menedżera środowisko Adobe**, wybierz pozycję **Menedżera środowisko Adobe** z panelu wyników kliknięcie **Dodaj** przycisk, aby dodać aplikację.

    ![Menedżer obsługi Adobe na liście wyników](./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfiguracja i testowanie usługi Azure AD rejestracji jednokrotnej

W tej sekcji skonfigurować i przetestować usługi Azure AD rejestracji jednokrotnej z Adobe środowisko Manager na podstawie użytkownika testowego o nazwie "Britta Simona".

Dla rejestracji jednokrotnej do pracy usługi Azure AD musi wiedzieć, użytkownik odpowiednika w Menedżerze środowisko Adobe jest dla użytkownika, w usłudze Azure AD. Innymi słowy relację łącza między użytkownika usługi Azure AD i danemu użytkownikowi w Menedżerze środowisko Adobe musi określone.

W Menedżerze środowisko Adobe przypisać wartość **nazwy użytkownika** w usłudze Azure AD jako wartość **Username** do ustanawiania relacji łącza.

Aby skonfigurować i przetestować usługi Azure AD rejestracji jednokrotnej z menedżerem środowisko Adobe, należy wykonać poniższe bloki konstrukcyjne:

1. **[Konfigurowanie usługi Azure AD rejestracji jednokrotnej](#configure-azure-ad-single-sign-on)**  — aby umożliwić użytkownikom korzystać z tej funkcji.
2. **[Tworzenie użytkownika testowego usługi Azure AD](#create-an-azure-ad-test-user)**  — do przetestowania usługi Azure AD rejestracji jednokrotnej z Simona Britta.
3. **[Tworzenie użytkownika testowego Menedżera środowisko Adobe](#create-an-adobe-experience-manager-test-user)**  — aby odpowiednikiem Simona Britta w Adobe środowisko Manager połączonej z usługi Azure AD reprezentację użytkownika.
4. **[Przypisz użytkownika testowego usługi Azure AD](#assign-the-azure-ad-test-user)**  — aby umożliwić Simona Britta do użycia usługi Azure AD rejestracji jednokrotnej.
5. **[Test rejestracji jednokrotnej](#test-single-sign-on)**  — Aby sprawdzić, czy konfiguracja działa.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurowanie usługi Azure AD rejestracji jednokrotnej

W tej sekcji można włączyć usługi Azure AD rejestracji jednokrotnej w portalu Azure i skonfigurować logowanie jednokrotne w aplikacji Adobe obsługi Menedżera.

**Aby skonfigurować usługi Azure AD rejestracji jednokrotnej z menedżerem środowisko Adobe, wykonaj następujące czynności:**

1. W portalu Azure na **Menedżera środowisko Adobe** strona integracji aplikacji, kliknij przycisk **logowanie jednokrotne**.

    ![Skonfigurować łącze rejestracji jednokrotnej][4]

2. Na **logowanie jednokrotne** okno dialogowe, wybierz opcję **tryb** jako **na języku SAML logowania jednokrotnego** Aby włączyć logowanie jednokrotne.
 
    ![Okno dialogowe rejestracji jednokrotnej](./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_samlbase.png)

3. Na **adresy URL i domeny Menedżera środowisko Adobe** sekcji, wykonaj następujące kroki, aby skonfigurować aplikację w **IdP** tryb:

    ![Adresy URL i domeny Menedżera środowisko Adobe pojedynczy informacje logowania jednokrotnego](./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_url1.png)

    a. W **identyfikator** tekstowym, wpisz unikatową wartość, który zdefiniowano na tym serwerze AEM. 

    b. W **adres URL odpowiedzi** tekstowym, wpisz adres URL, używając następującego wzorca:`https://<AEM Server Url>/saml_login`

    > [!NOTE] 
    > Wartości te nie są prawdziwe. Rzeczywisty identyfikator i adres URL odpowiedzi, należy zaktualizować te wartości. Skontaktuj się z [zespołem pomocy technicznej Menedżera środowisko Adobe](https://helpx.adobe.com/support/experience-manager.html) uzyskać te wartości.
 
4. Sprawdź, Pokaż zaawansowane ustawienia adresu URL i wykonać następujący krok, jeśli chcesz skonfigurować aplikację w **SP** inicjowane tryb:

    ![Adresy URL i domeny Menedżera środowisko Adobe pojedynczy informacje logowania jednokrotnego](./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_spconfigure.png)

    W **na adres URL logowania** tekstowym, wpisz adres URL serwera Adobe środowisko Manager. 

5. Na **certyfikat podpisywania SAML** kliknij **certyfikatu (Base64)** , a następnie zapisz plik certyfikatu na tym komputerze.

    ![Łącze pobierania certyfikatu](./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_certificate.png) 

6. W sekcji konfiguracji Menedżera środowisko Adobe kliknij Manager środowisko Adobe skonfigurować, aby otworzyć Konfigurowanie logowania jednokrotnego okna. Kopiowania **SAML logowania jednokrotnego usługi adres URL**, **identyfikator jednostki SAML** i **Sign-Out identyfikator** z sekcji krótkimi opisami.

    ![Link w sekcji konfiguracji](./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_configure.png) 

7. Kliknij przycisk **zapisać** przycisku.

    ![Skonfiguruj przycisk pojedynczego logowania jednokrotnego Zapisz](./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_general_400.png)

8. Otwórz **Menedżera środowisko Adobe** portalu administratora w innym oknie przeglądarki.

9. Wybierz **ustawienia** -> **zabezpieczeń** -> **użytkowników**.

    ![Skonfiguruj przycisk pojedynczego logowania jednokrotnego Zapisz](./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_user.png)

10. Wybierz **administratora** lub innych odpowiednich użytkowników.

    ![Skonfiguruj przycisk pojedynczego logowania jednokrotnego Zapisz](./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_admin6.png)

11. Wybierz **ustawienia konta** -> **Tworzenie lub zarządzać TrustStore**.

    ![Skonfiguruj przycisk pojedynczego logowania jednokrotnego Zapisz](./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_managetrust.png)

12. Kliknij przycisk **wybierz plik certyfikatu** z **dodać certyfikat z pliku CER** przycisku. Wyszukaj i wybierz plik certyfikatu, który został pobrany z portalu Azure.

    ![Skonfiguruj przycisk pojedynczego logowania jednokrotnego Zapisz](./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_user2.png)

13. Certyfikat jest dodawany do TrustStore. Należy pamiętać, alias certyfikatu.

    ![Skonfiguruj przycisk pojedynczego logowania jednokrotnego Zapisz](./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_admin7.png)

14. Na **użytkowników** wybierz pozycję **usługi uwierzytelniania**.

    ![Skonfiguruj przycisk pojedynczego logowania jednokrotnego Zapisz](./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_admin8.png)

15. Wybierz **ustawienia konta** -> **KeyStore Utwórz/Zarządzaj**. Tworzenie magazynu kluczy, podając hasło.

    ![Skonfiguruj przycisk pojedynczego logowania jednokrotnego Zapisz](./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_admin9.png)

16. Wróć do administratora ekranu i wybierz pozycję **ustawienia** -> **operacji** -> **konsoli sieci Web**.

    ![Skonfiguruj przycisk pojedynczego logowania jednokrotnego Zapisz](./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_admin1.png)

17. Spowoduje to otwarcie strony konfiguracji.

    ![Skonfiguruj przycisk pojedynczego logowania jednokrotnego Zapisz](./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_admin2.png)

18. Znajdź **Adobe granitu SAML 2.0 uwierzytelniania obsługi** i wybierz polecenie **Dodaj** ikony.

    ![Skonfiguruj przycisk pojedynczego logowania jednokrotnego Zapisz](./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_admin3.png)

19. Wykonaj następujące czynności na tej stronie.

    ![Skonfiguruj przycisk pojedynczego logowania jednokrotnego Zapisz](./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_admin4.png)

    a. W **ścieżki** pole tekstowe, wprowadź  **/** .

    b. W **IDP URL** pole tekstowe, wprowadź wartość **SAML logowania jednokrotnego usługi adres URL**, które zostały skopiowane z portalu Azure.

    c. W **Alias certyfikatu IDP** pole tekstowe, wprowadź wartość **Alias certyfikatu**, dodanego w TrustStore.

    d. W **zabezpieczeń podany identyfikator jednostki** pole tekstowe, wprowadź wartość unikatową **identyfikator jednostki SAML**, które zostały skonfigurowane w portalu Azure.

    e. W **adres URL usługi klienta potwierdzenia** pole tekstowe, wprowadź wartość **adres URL odpowiedzi służący**, które zostały skonfigurowane w portalu Azure.

    f. W **magazynu haseł klucza** pole tekstowe, wprowadź **hasło**, który wybrano w magazynie kluczy.

    g. W **identyfikator atrybutu użytkownika** pole tekstowe, wprowadź **Identyfikatora nazwy** lub innych identyfikator użytkownika, który ma zastosowanie w Twoim przypadku.

    h. Wybierz **CRX automatycznie Utwórz użytkowników.**

    i. W **adresu URL wylogowania** pole tekstowe, wprowadź wartość unikatową **Sign-Out URL** mające z portalu Azure.

    j. Kliknij przycisk **Zapisz**

> [!TIP]
> Teraz możesz przeczytać zwięzły wersji tych instrukcji wewnątrz [portalu Azure](https://portal.azure.com), podczas konfigurowania aplikacji!  Po dodaniu tej aplikacji z **usługi Active Directory > aplikacje dla przedsiębiorstw** po prostu kliknij **rejestracji jednokrotnej** karcie i dostęp do dokumentacji osadzonych za pomocą **konfiguracji** sekcji u dołu. Więcej o funkcji dokumentacji osadzonego w tym miejscu: [dokumentacji osadzonych usługi Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Tworzenie użytkownika testowego usługi Azure AD

Celem tej sekcji jest tworzenie użytkownika testowego w portalu Azure o nazwie Simona Britta.

   ![Tworzenie użytkownika testowego usługi Azure AD][100]

**Aby utworzyć użytkownika testowego w usłudze Azure AD, wykonaj następujące czynności:**

1. W portalu Azure, w okienku po lewej stronie kliknij **usługi Azure Active Directory** przycisku.

    ![Przycisk usługi Azure Active Directory](./media/active-directory-saas-adobeexperiencemanager-tutorial/create_aaduser_01.png)

2. Aby wyświetlić listę użytkowników, przejdź do **użytkowników i grup**, a następnie kliknij przycisk **wszyscy użytkownicy**.

    !["Użytkownicy i grupy" i "Wszyscy użytkownicy" łącza](./media/active-directory-saas-adobeexperiencemanager-tutorial/create_aaduser_02.png)

3. Aby otworzyć **użytkownika** okno dialogowe, kliknij przycisk **Dodaj** w górnej części **wszyscy użytkownicy** okno dialogowe.

    ![Przycisk Dodaj](./media/active-directory-saas-adobeexperiencemanager-tutorial/create_aaduser_03.png)

4. W **użytkownika** okna dialogowego wykonaj następujące czynności:

    ![Okno dialogowe użytkownika](./media/active-directory-saas-adobeexperiencemanager-tutorial/create_aaduser_04.png)

    a. W **nazwa** wpisz **BrittaSimon**.

    b. W **nazwy użytkownika** wpisz adres e-mail użytkownika Simona Britta.

    c. Wybierz **Pokaż hasło** pole wyboru, a następnie zanotuj wartość, która jest wyświetlana w **hasło** pole.

    d. Kliknij przycisk **Utwórz**.
  
### <a name="create-an-adobe-experience-manager-test-user"></a>Tworzenie użytkownika testowego Adobe środowisko Manager

W tej sekcji należy utworzyć użytkownika o nazwie Simona Britta w Menedżerze środowisko Adobe. Jeśli wybrano **użytkowników CRX automatycznie Utwórz** opcję użytkowników zostanie utworzony automatycznie po pomyślnym uwierzytelnieniu. 

Jeśli chcesz ręcznie utworzyć użytkowników, współpracy z [zespołem pomocy technicznej Menedżera środowisko Adobe](https://helpx.adobe.com/support/experience-manager.html) Aby dodać użytkowników na platformie Adobe obsługi Menedżera. 

### <a name="assign-the-azure-ad-test-user"></a>Przypisz użytkownika testowego usługi Azure AD

W tej sekcji można włączyć Simona Britta do używania Azure logowania jednokrotnego za udzielanie dostępu do Menedżera środowisko Adobe.

![Przypisanie roli użytkownika][200] 

**Aby przypisać do Menedżera środowisko Adobe Simona Britta, wykonaj następujące czynności:**

1. W portalu Azure Otwórz widok aplikacji, a następnie przejdź do widoku katalogu i przejdź do **aplikacje dla przedsiębiorstw** kliknięcie **wszystkie aplikacje**.

    ![Przypisz użytkownika][201] 

2. Na liście aplikacji zaznacz **Menedżera środowisko Adobe**.

    ![Łączy Menedżera środowisko Adobe na liście aplikacji](./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_app.png)  

3. W menu po lewej stronie kliknij **użytkowników i grup**.

    ![Łącze "Użytkownicy i grupy"][202]

4. Kliknij przycisk **Dodaj** przycisku. Następnie wybierz **użytkowników i grup** na **Dodaj przydziału** okna dialogowego.

    ![W okienku Dodaj przydziału][203]

5. Na **użytkowników i grup** okno dialogowe, wybierz opcję **Simona Britta** na liście Użytkownicy.

6. Kliknij przycisk **wybierz** znajdującego się na **użytkowników i grup** okna dialogowego.

7. Kliknij przycisk **przypisać** znajdującego się na **Dodaj przydziału** okna dialogowego.
    
### <a name="test-single-sign-on"></a>Test rejestracji jednokrotnej

W tej sekcji można przetestować konfiguracji usługi Azure AD pojedynczego logowania za pomocą panelu dostępu.

Po kliknięciu kafelka Adobe środowisko Manager w panelu dostępu należy należy pobrać automatycznie zalogowane do aplikacji Adobe obsługi Menedżera.
Aby uzyskać więcej informacji na temat panelu dostępu, zobacz [wprowadzenie do panelu dostępu](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Lista samouczków dotyczących sposobów integracji aplikacji SaaS przy użyciu usługi Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co to jest dostęp do aplikacji i logowanie jednokrotne z usługą Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_general_203.png

