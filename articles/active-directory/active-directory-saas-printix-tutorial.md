---
title: 'Samouczek: Integracji Azure Active Directory z Printix | Dokumentacja firmy Microsoft'
description: "Informacje o sposobie konfigurowania rejestracji jednokrotnej między usługą Azure Active Directory i Printix."
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 4aea7320-b2d5-49e0-9b63-aeaff0f6fe66
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: jeedes
ms.openlocfilehash: 3b04388d3d91d9fb1ac38988071ebb5ab00366ee
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-printix"></a>Samouczek: Integracji Azure Active Directory z Printix

Z tego samouczka dowiesz się integrowanie Printix z usługi Azure Active Directory (Azure AD).

Integracja z usługą Azure AD Printix zapewnia następujące korzyści:

- Można kontrolować w usłudze Azure AD, który ma dostęp do Printix
- Umożliwia użytkownikom automatycznie pobrać zalogowane do Printix (logowanie jednokrotne) przy użyciu ich kont usługi Azure AD
- Możesz zarządzać kont w jednej centralnej lokalizacji - portalu Azure

Jeśli chcesz dowiedzieć się więcej informacji o integracji aplikacji SaaS w usłudze Azure AD, zobacz [co to jest dostęp do aplikacji i logowanie jednokrotne z usługą Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Wymagania wstępne

Aby skonfigurować integrację usługi Azure AD z Printix, potrzebne są następujące elementy:

- Subskrypcję usługi Azure AD
- Printix logowanie jednokrotne włączone subskrypcji

> [!NOTE]
> Aby przetestować kroki opisane w tym samouczku, zaleca się używania środowiska produkcyjnego.

Aby przetestować kroki opisane w tym samouczku, należy wykonać te zalecenia:

- Nie należy używać środowiska produkcyjnego, jeśli jest to konieczne.
- Jeśli nie masz środowisko wersji próbnej usługi Azure AD, możesz pobrać miesięczna wersja próbna [tutaj](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Opis scenariusza
W tym samouczku można przetestować usługę Azure AD rejestracji jednokrotnej w środowisku testowym. Scenariusz opisany w tym samouczku składa się z dwóch głównych elementów:

1. Dodawanie Printix z galerii
2. Konfigurowanie i testowanie usługi Azure AD logowanie jednokrotne

## <a name="adding-printix-from-the-gallery"></a>Dodawanie Printix z galerii
Aby skonfigurować integrację usługi Azure AD Printix, należy dodać Printix z galerii do listy zarządzanych aplikacji SaaS.

**Aby dodać Printix z galerii, wykonaj następujące czynności:**

1. W  **[portalu Azure](https://portal.azure.com)**, na panelu nawigacyjnym po lewej stronie kliknij **usługi Azure Active Directory** ikony. 

    ![Usługa Active Directory][1]

2. Przejdź do **aplikacje dla przedsiębiorstw**. Następnie przejdź do **wszystkie aplikacje**.

    ![Aplikacje][2]
    
3. Aby dodać nową aplikację, kliknij przycisk **nowej aplikacji** przycisk w górnej części okna dialogowego.

    ![Aplikacje][3]

4. W polu wyszukiwania wpisz **Printix**.

    ![Tworzenie użytkownika testowego usługi Azure AD](./media/active-directory-saas-printix-tutorial/tutorial_printix_search.png)

5. W panelu wyników wybierz **Printix**, a następnie kliknij przycisk **Dodaj** przycisk, aby dodać aplikację.

    ![Tworzenie użytkownika testowego usługi Azure AD](./media/active-directory-saas-printix-tutorial/tutorial_printix_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurowanie i testowanie usługi Azure AD logowanie jednokrotne
W tej sekcji skonfigurować i przetestować usługi Azure AD rejestracji jednokrotnej z Printix w oparciu o nazwie "Britta Simona" użytkownika testowego.

Dla rejestracji jednokrotnej do pracy usługi Azure AD musi wiedzieć, użytkownik odpowiednika w Printix jest dla użytkownika, w usłudze Azure AD. Innymi słowy link relację między użytkownikiem usługi Azure AD i danemu użytkownikowi w Printix musi się.

W Printix, należy przypisać wartość **nazwy użytkownika** w usłudze Azure AD jako wartość **Username** do ustanawiania relacji łącza.

Aby skonfigurować i przetestować usługi Azure AD rejestracji jednokrotnej z Printix, należy wykonać poniższe bloki konstrukcyjne:

1. **[Konfigurowanie usługi Azure AD rejestracji jednokrotnej](#configuring-azure-ad-single-sign-on)**  — aby umożliwić użytkownikom korzystać z tej funkcji.
2. **[Tworzenie użytkownika testowego usługi Azure AD](#creating-an-azure-ad-test-user)**  — do przetestowania usługi Azure AD rejestracji jednokrotnej z Simona Britta.
3. **[Tworzenie użytkownika testowego Printix](#creating-a-printix-test-user)**  — w celu zapewnienia odpowiednikiem Simona Britta Printix połączonego z usługi Azure AD reprezentację użytkownika.
4. **[Przypisanie użytkownika testowego usługi Azure AD](#assigning-the-azure-ad-test-user)**  — aby umożliwić Simona Britta do użycia usługi Azure AD rejestracji jednokrotnej.
5. **[Testowanie rejestracji jednokrotnej](#testing-single-sign-on)**  — Aby sprawdzić, czy konfiguracja działa.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurowanie usługi Azure AD rejestracji jednokrotnej

W tej sekcji można włączyć usługi Azure AD rejestracji jednokrotnej w portalu Azure i skonfigurować logowanie jednokrotne w aplikacji Printix.

**Aby skonfigurować usługi Azure AD rejestracji jednokrotnej z Printix, wykonaj następujące czynności:**

1. W portalu Azure na **Printix** strona integracji aplikacji, kliknij przycisk **logowanie jednokrotne**.

    ![Konfigurowanie rejestracji jednokrotnej][4]

2. Na **logowanie jednokrotne** okno dialogowe, wybierz opcję **tryb** jako **na języku SAML logowania jednokrotnego** Aby włączyć logowanie jednokrotne.
 
    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-printix-tutorial/tutorial_printix_samlbase.png)

3. Na **Printix domeny i adres URL** sekcji, wykonaj następujące czynności:

    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-printix-tutorial/tutorial_printix_url.png)

    W **adres URL logowania** tekstowym, wpisz adres URL, używając następującego wzorca:`https://<subdomain>.printix.net`

    > [!NOTE] 
    > Wartość nie jest prawdziwe. Zaktualizuj tę wartość z adresem URL logowania rzeczywistych. Skontaktuj się z [zespołem pomocy technicznej klienta Printix](mailto:support@printix.net) można uzyskać wartość. 
 
4. Na **certyfikat podpisywania SAML** kliknij **XML metadanych** , a następnie zapisz plik metadanych na tym komputerze.

    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-printix-tutorial/tutorial_printix_certificate.png) 

5. Kliknij przycisk **zapisać** przycisku.

    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-printix-tutorial/tutorial_general_400.png)

6. Logowanie do dzierżawy Printix jako administrator.

7. W menu u góry kliknij ikonę w prawym górnym rogu i wybierz pozycję "**uwierzytelniania**".
   
    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-printix-tutorial/tutorial_printix_06.png)

8. Na **Instalator** wybierz opcję **uwierzytelniania Włącz Azure/Office 365**
   
    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-printix-tutorial/tutorial_printix_07.png)

9. Na **Azure** karcie, wprowadź adres URL metadanych Federacji pole tekstowe z "**dokument metadanych usług federacyjnych**". 

    Dołącz plik xml metadanych, który został pobrany z usługi Azure AD do [zespołem pomocy technicznej Printix](mailto:support@printix.net). Następnie załaduj plik xml i wprowadzić adres URL metadanych federacji.
   
    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-printix-tutorial/tutorial_printix_08.png)
   
10. Kliknij przycisk "**Test**"przycisk i kliknij przycisk"**OK**" przycisku, jeśli test zakończyło się pomyślnie.
   
     Strona usługi Azure active directory zostaną wyświetlone po kliknięciu **test** przycisku. "Powiodło się uruchomienie testu" poniżej oznacza, że po wprowadzeniu poświadczeń konta Azure testu, który będzie punktu obecności komunikat "Ustawienia sprawdzona". Następnie kliknij przycisk **OK** przycisku.
   
    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-printix-tutorial/tutorial_printix_09.png)

11. Kliknij przycisk **zapisać** znajdującego się na "**uwierzytelniania**" strony.


> [!TIP]
> Teraz możesz przeczytać zwięzły wersji tych instrukcji wewnątrz [portalu Azure](https://portal.azure.com), podczas konfigurowania aplikacji!  Po dodaniu tej aplikacji z **usługi Active Directory > aplikacje dla przedsiębiorstw** po prostu kliknij **rejestracji jednokrotnej** karcie i dostęp do dokumentacji osadzonych za pomocą **konfiguracji** sekcji u dołu. Więcej o funkcji dokumentacji osadzonego w tym miejscu: [dokumentacji osadzonych usługi Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Tworzenie użytkownika testowego usługi Azure AD
Celem tej sekcji jest tworzenie użytkownika testowego w portalu Azure o nazwie Simona Britta.

![Tworzenie użytkowników usługi Azure AD][100]

**Aby utworzyć użytkownika testowego w usłudze Azure AD, wykonaj następujące czynności:**

1. W **portalu Azure**, w lewym okienku nawigacji, kliknij polecenie **usługi Azure Active Directory** ikony.

    ![Tworzenie użytkownika testowego usługi Azure AD](./media/active-directory-saas-printix-tutorial/create_aaduser_01.png) 

2. Aby wyświetlić listę użytkowników, przejdź do **użytkowników i grup** i kliknij przycisk **wszyscy użytkownicy**.
    
    ![Tworzenie użytkownika testowego usługi Azure AD](./media/active-directory-saas-printix-tutorial/create_aaduser_02.png) 

3. Aby otworzyć **użytkownika** okna dialogowego, kliknij przycisk **Dodaj** górnej części okna dialogowego.
 
    ![Tworzenie użytkownika testowego usługi Azure AD](./media/active-directory-saas-printix-tutorial/create_aaduser_03.png) 

4. Na **użytkownika** okna dialogowego strony, należy wykonać następujące czynności:
 
    ![Tworzenie użytkownika testowego usługi Azure AD](./media/active-directory-saas-printix-tutorial/create_aaduser_04.png) 

    a. W **nazwa** pole tekstowe, typ **BrittaSimon**.

    b. W **nazwy użytkownika** pole tekstowe, typ **adres e-mail** z BrittaSimon.

    c. Wybierz **Pokaż hasło** i zanotuj wartość **hasło**.

    d. Kliknij przycisk **Utwórz**.
 
### <a name="creating-a-printix-test-user"></a>Tworzenie użytkownika testowego Printix

Celem tej sekcji jest utworzenie użytkownika o nazwie Simona Britta w Printix. Printix obsługę w czasie, który jest domyślnie włączone.

Nie ma elementu akcji można w tej sekcji. Nowy użytkownik został utworzony podczas próby dostępu Printix, jeśli go jeszcze nie istnieje. 

> [!NOTE]
> Jeśli trzeba ręcznie utworzyć użytkownika, należy skontaktować się [zespołem pomocy technicznej Printix](mailto:support@printix.net).
> 

### <a name="assigning-the-azure-ad-test-user"></a>Przypisanie użytkownika testowego usługi Azure AD

W tej sekcji można włączyć Simona Britta do używania Azure logowania jednokrotnego za udzielanie dostępu Printix.

![Przypisz użytkownika][200] 

**Aby przypisać Simona Britta Printix, wykonaj następujące czynności:**

1. W portalu Azure Otwórz widok aplikacji, a następnie przejdź do widoku katalogu i przejdź do **aplikacje dla przedsiębiorstw** kliknięcie **wszystkie aplikacje**.

    ![Przypisz użytkownika][201] 

2. Na liście aplikacji zaznacz **Printix**.

    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-printix-tutorial/tutorial_printix_app.png) 

3. W menu po lewej stronie kliknij **użytkowników i grup**.

    ![Przypisz użytkownika][202] 

4. Kliknij przycisk **Dodaj** przycisku. Następnie wybierz **użytkowników i grup** na **Dodaj przydziału** okna dialogowego.

    ![Przypisz użytkownika][203]

5. Na **użytkowników i grup** okno dialogowe, wybierz opcję **Simona Britta** na liście Użytkownicy.

6. Kliknij przycisk **wybierz** znajdującego się na **użytkowników i grup** okna dialogowego.

7. Kliknij przycisk **przypisać** znajdującego się na **Dodaj przydziału** okna dialogowego.
    
### <a name="testing-single-sign-on"></a>Testowanie rejestracji jednokrotnej

W tej sekcji można przetestować konfiguracji usługi Azure AD pojedynczego logowania za pomocą panelu dostępu.

Po kliknięciu kafelka Printix w panelu dostępu użytkownik powinien pobrać automatycznie zalogowane do aplikacji Printix.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Lista samouczków dotyczących sposobów integracji aplikacji SaaS przy użyciu usługi Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co to jest dostęp do aplikacji i logowanie jednokrotne z usługą Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-printix-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-printix-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-printix-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-printix-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-printix-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-printix-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-printix-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-printix-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-printix-tutorial/tutorial_general_203.png

