---
title: 'Samouczek: Integracji Azure Active Directory z Wizergos oprogramowanie | Dokumentacja firmy Microsoft'
description: "Informacje o sposobie konfigurowania rejestracji jednokrotnej między usługą Azure Active Directory i Wizergos wydajności oprogramowania."
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: acc04396-13c5-4c24-ab9a-30fbc9234ebd
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/20/2017
ms.author: jeedes
ms.openlocfilehash: 73b3bc05aeb337c12acb7e47c0dbebe6d0196530
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-wizergos-productivity-software"></a>Samouczek: Integracji Azure Active Directory z Wizergos wydajności oprogramowania
Celem tego samouczka jest pokazanie sposobu integracji Wizergos wydajności oprogramowania z usługi Azure Active Directory (Azure AD).

Integrowanie Wizergos wydajności oprogramowania z usługi Azure AD zapewnia następujące korzyści:

* Można kontrolować w usłudze Azure AD, który ma dostęp do Wizergos wydajności oprogramowania
* Umożliwia użytkownikom automatycznie pobrać zalogowane do Wizergos oprogramowanie rejestracji jednokrotnej (SSO) przy użyciu ich kont usługi Azure AD
* Możesz zarządzać kont w jednej centralnej lokalizacji - klasycznego portalu Azure

Jeśli chcesz dowiedzieć się więcej informacji o integracji aplikacji SaaS w usłudze Azure AD, zobacz [co to jest dostęp do aplikacji i logowanie jednokrotne z usługą Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Wymagania wstępne
Aby skonfigurować integrację usługi Azure AD z oprogramowaniem wydajność Wizergos, potrzebne są następujące elementy:

* Subskrypcję usługi Azure AD
* Subskrypcja Wizergos wydajności oprogramowania logowanie Jednokrotne włączone

>[!NOTE]
>Aby przetestować kroki opisane w tym samouczku, zaleca się używania środowiska produkcyjnego. 
> 

Aby przetestować kroki opisane w tym samouczku, należy wykonać te zalecenia:

* Nie należy używać środowiska produkcyjnego, chyba że jest to konieczne.
* Jeśli nie masz środowisko wersji próbnej usługi Azure AD, możesz pobrać [miesięczna wersja próbna](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Opis scenariusza
Celem tego samouczka jest umożliwienie test rejestracji Jednokrotnej programu Azure AD w środowisku testowym.

Scenariusz opisany w tym samouczku składa się z dwóch głównych elementów:

1. Dodawanie Wizergos oprogramowanie z galerii
2. Konfigurowanie i testowania logowania jednokrotnego programu Azure AD

## <a name="adding-wizergos-productivity-software-from-the-gallery"></a>Dodawanie Wizergos oprogramowanie z galerii
Aby skonfigurować integrację usługi Azure AD Wizergos oprogramowanie, należy dodać Wizergos oprogramowanie z poziomu galerii do listy zarządzanych aplikacji SaaS.

**Aby dodać Wizergos oprogramowanie z poziomu galerii, wykonaj następujące czynności:**

1. W **klasycznego portalu Azure**, w lewym okienku nawigacji, kliknij polecenie **usługi Active Directory**. 
   
    ![Usługa Active Directory][1]
2. Z **katalogu** listy, wybierz katalog, dla którego chcesz włączyć integracji katalogów.
3. Aby otworzyć widok aplikacji, w widoku katalogu, kliknij przycisk **aplikacji** w menu u góry.
   
    ![Aplikacje][2]
4. Kliknij przycisk **Dodaj** w dolnej części strony.
   
    ![Aplikacje][3]
5. Na **co chcesz zrobić** okna dialogowego, kliknij przycisk **dodać aplikację z galerii**.
   
    ![Aplikacje][4]
6. W polu wyszukiwania wpisz **Wizergos oprogramowanie**.
   
    ![Tworzenie użytkownika testowego usługi Azure AD](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_01.png)
7. W panelu wyników wybierz **Wizergos oprogramowanie**, a następnie kliknij przycisk **Complete** można dodać aplikację.
   
    ![Wybieranie aplikacji w galerii](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_001.png)

## <a name="configure-and-test-azure-ad-sso"></a>Konfiguracja i testowanie logowania jednokrotnego programu Azure AD
Jest celem tej sekcji opisano, jak skonfigurować i przetestować Azure AD SSO z Wizergos wydajności oprogramowania w oparciu o nazwie "Britta Simona" użytkownika testowego.

Dla logowania jednokrotnego do pracy usługi Azure AD musi wiedzieć, co to jest odpowiednikiem użytkownika w Wizergos oprogramowanie do użytkownika w usłudze Azure AD. Innymi słowy musi można ustanowić łącze relację między użytkownikiem usługi Azure AD i danemu użytkownikowi Wizergos wydajności oprogramowania.

Ta relacja łącza zostanie nawiązane, przypisując wartość **nazwy użytkownika** w usłudze Azure AD jako wartość **Username** Wizergos wydajności oprogramowania.

Do konfigurowania i testowania usługi Azure AD rejestracji jednokrotnej z BynWizergos Softwareder wydajności, należy wykonać poniższe bloki konstrukcyjne:

1. **[Konfigurowanie usługi Azure AD rejestracji jednokrotnej](#configuring-azure-ad-single-single-sign-on)**  — aby umożliwić użytkownikom korzystać z tej funkcji.
2. **[Tworzenie użytkownika testowego usługi Azure AD](#creating-an-azure-ad-test-user)**  — do przetestowania usługi Azure AD rejestracji jednokrotnej z Simona Britta.
3. **[Tworzenie użytkownika testowego oprogramowanie Wizergos](#creating-a-wizergos-productivity-software-test-user)**  — w celu zapewnienia odpowiednikiem Simona Britta Wizergos wydajności oprogramowania, które jest połączone z jej reprezentacji usługi Azure AD.
4. **[Przypisanie użytkownika testowego usługi Azure AD](#assigning-the-azure-ad-test-user)**  — aby umożliwić Simona Britta do użycia usługi Azure AD rejestracji jednokrotnej.
5. **[Testowanie rejestracji jednokrotnej](#testing-single-sign-on)**  — Aby sprawdzić, czy konfiguracja działa.

### <a name="configuring-azure-ad-sso"></a>Konfigurowanie usługi Azure AD logowania jednokrotnego.
W tej sekcji można włączyć usługi Azure AD rejestracji jednokrotnej w klasycznym portalu i skonfigurować logowanie jednokrotne w aplikacji Wizergos wydajności oprogramowania.

**Aby skonfigurować usługi Azure AD rejestracji jednokrotnej z Wizergos oprogramowanie, wykonaj następujące czynności:**

1. W klasycznym portalu na **Wizergos oprogramowanie** strona integracji aplikacji, kliknij przycisk **skonfigurować logowanie jednokrotne** otworzyć **skonfigurować logowanie jednokrotne** okna dialogowego.
   
    ![Konfigurowanie rejestracji jednokrotnej][6] 
2. Na **jak chcesz użytkownikom zalogować się na oprogramowanie Wizergos** wybierz pozycję **Azure AD rejestracji jednokrotnej**, a następnie kliknij przycisk **dalej**:
   
    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_03.png)
3. Na **Konfigurowanie ustawień aplikacji** strony okna dialogowego, kliknij przycisk **dalej**:
   
    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_04.png)
4. Na **skonfigurować logowanie jednokrotne w oprogramowanie Wizergos** kliknij przycisk **pobierania certyfikatu**, a następnie zapisz plik na komputerze:
   
    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_05.png)
5. W oknie przeglądarki innej witryny sieci web logowanie do dzierżawy Wizergos wydajności oprogramowania jako administrator.
6. Wybierz z hamburger menu **Admin**.
   
    ![Konfigurowanie jednej logowania w aplikacji po stronie](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_000.png)
7. Na stronie Administracja w menu po lewej stronie wybierz **uwierzytelniania** i wybierz polecenie **usługi Azure AD**.
   
    ![Konfigurowanie jednej logowania w aplikacji po stronie](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_002.png)
8. Wykonaj następujące czynności na **uwierzytelniania** sekcji.
   
    ![Konfigurowanie jednej logowania w aplikacji po stronie](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_003.png)
  1. Kliknij przycisk **przekazać** przycisk, aby przekazać certyfikat pobrany z usługi Azure AD. 
  2. W **adres URL wystawcy** pole tekstowe umieścić wartość elementu **adres URL wystawcy** z Kreatora konfiguracji aplikacji usługi Azure AD.
  3. W **URL rejestracji jednokrotnej** pole tekstowe umieścić wartość elementu **pojedynczy znak na adres URL usługi** z Kreatora konfiguracji aplikacji usługi Azure AD.
  4. W **pojedynczego adresu URL Sign-Out** pole tekstowe umieścić wartość elementu **pojedynczy adres URL usługi Sign-out** z Kreatora konfiguracji aplikacji usługi Azure AD.
  5. Kliknij przycisk **zapisać** przycisku.
9. W klasycznym portalu, wybierz Potwierdzenie konfiguracji rejestracji jednokrotnej, a następnie kliknij przycisk **dalej**.
   
    ![Azure AD rejestracji jednokrotnej][10]
10. Na **pojedynczy znak na potwierdzenie** kliknij przycisk **Complete**.  
    
    ![Azure AD rejestracji jednokrotnej][11]

### <a name="create-an-azure-ad-test-user"></a>Tworzenie użytkownika testowego usługi Azure AD
Celem tej sekcji jest tworzenie użytkownika testowego w klasycznym portalu o nazwie Simona Britta.

![Tworzenie użytkowników usługi Azure AD][20]

**Aby utworzyć użytkownika testowego w usłudze Azure AD, wykonaj następujące czynności:**

1. W **klasycznego portalu Azure**, w lewym okienku nawigacji, kliknij polecenie **usługi Active Directory**.
   
    ![Tworzenie użytkownika testowego usługi Azure AD](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_09.png)
2. Z **katalogu** listy, wybierz katalog, dla którego chcesz włączyć integracji katalogów.
3. Aby wyświetlić listę użytkowników, w menu u góry, kliknij przycisk **użytkowników**.
   
    ![Tworzenie użytkownika testowego usługi Azure AD](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_03.png)
4. Aby otworzyć **Dodaj użytkownika** okna dialogowego na pasku narzędzi u dołu, kliknij przycisk **Dodaj użytkownika**.
   
    ![Tworzenie użytkownika testowego usługi Azure AD](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_04.png)
5. Na **Poinformuj nas o tym użytkowniku** okna dialogowego strony, należy wykonać następujące czynności:
   
    ![Tworzenie użytkownika testowego usługi Azure AD](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_05.png) 
  1. Jako typ użytkownika wybierz nowego użytkownika w organizacji.
  2. W nazwie użytkownika **pole tekstowe**, typ **BrittaSimon**.
  3. Kliknij przycisk **Dalej**.
6. Na **profilu użytkownika** okna dialogowego strony, należy wykonać następujące czynności:
   
   ![Tworzenie użytkownika testowego usługi Azure AD](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_06.png)
  1. W **imię** pole tekstowe, typ **Britta**.  
  2. W **nazwisko** pole tekstowe, typ **Simona**.
  3. W **Nazwa wyświetlana** pole tekstowe, typ **Simona Britta**.
  4. W **roli** listy, wybierz **użytkownika**.
  5. Kliknij przycisk **Dalej**.
7. Na **Uzyskaj hasło tymczasowe** strony okna dialogowego, kliknij przycisk **utworzyć**.
   
    ![Tworzenie użytkownika testowego usługi Azure AD](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_07.png)
8. Na **Uzyskaj hasło tymczasowe** okna dialogowego strony, należy wykonać następujące czynności:
   
    ![Tworzenie użytkownika testowego usługi Azure AD](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_08.png)
  1. Zanotuj wartość **nowe hasło**.
  2. Kliknij przycisk **Complete** (Zakończ).   

### <a name="create-a-wizergos-productivity-software-test-user"></a>Tworzenie użytkownika testowego Wizergos wydajności oprogramowania
W tej sekcji należy utworzyć użytkownika o nazwie Simona Britta Wizergos wydajności oprogramowania. Proszę współpracować z zespołem pomocy technicznej Wizergos wydajności oprogramowania za pomocą [ support@wizergos.com ](emailTo:support@wizergos.com) Aby dodać użytkowników na platformie Wizergos wydajności oprogramowania.

### <a name="assign-the-azure-ad-test-user"></a>Przypisz użytkownika testowego usługi Azure AD
Celem tej sekcji jest włączenie Simona Britta się za pomocą logowania jednokrotnego Azure przez udostępnienie jej do Wizergos wydajności oprogramowania.

  ![Przypisz użytkownika][200]

**Aby przypisać Simona Britta Wizergos oprogramowanie, wykonaj następujące czynności:**

1. W klasycznym portalu, aby otworzyć widok aplikacji, w widoku katalogu, kliknij przycisk **aplikacji** w menu u góry.
   
    ![Przypisz użytkownika][201]
2. Na liście aplikacji zaznacz **Wizergos oprogramowanie**.
   
    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_50.png)
3. W menu u góry kliknij **użytkowników**.
   
    ![Przypisz użytkownika][203]
4. Na liście użytkowników wybierz **Simona Britta**.
5. Na pasku narzędzi u dołu, kliknij przycisk **przypisać**.
   
    ![Przypisz użytkownika][205]

### <a name="test-single-sign-on"></a>Test rejestracji jednokrotnej
Celem tej sekcji służy do testowania konfigurację usługi Azure AD z logowania jednokrotnego za pomocą panelu dostępu.

Po kliknięciu kafelka Wizergos oprogramowanie w panelu dostępu użytkownik powinien pobrać automatycznie zalogowane do aplikacji Wizergos wydajności oprogramowania.

## <a name="additional-resources"></a>Dodatkowe zasoby
* [Lista samouczków dotyczących sposobów integracji aplikacji SaaS przy użyciu usługi Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co to jest dostęp do aplikacji i logowanie jednokrotne z usługą Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_205.png
