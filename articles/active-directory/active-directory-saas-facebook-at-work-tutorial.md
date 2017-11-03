---
title: 'Samouczek: Integracji Azure Active Directory z miejsca pracy przez Facebook | Dokumentacja firmy Microsoft'
description: "Informacje o sposobie konfigurowania rejestracji jednokrotnej między usługą Azure Active Directory i miejsca pracy przez usługi Facebook."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 30f2ee64-95d3-44ef-b832-8a0a27e2967c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/18/2017
ms.author: jeedes
ms.openlocfilehash: 27e62a00832484667117d8718db9f2fc05e2f4e2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-workplace-by-facebook"></a>Samouczek: Integracji Azure Active Directory z miejsca pracy przez usługi Facebook

Z tego samouczka dowiesz się sposobu integracji z usługą Azure Active Directory (Azure AD) miejsca pracy przez usługi Facebook.

Integrowanie miejsca pracy przez Facebook z usługą Azure AD zapewnia następujące korzyści:

- Można kontrolować w usłudze Azure AD, który ma dostęp do miejsca pracy przez usługi Facebook.
- Można umożliwić użytkownikom automatycznie pobrać zalogowanego do miejsca pracy przez Facebook (logowanie jednokrotne) przy użyciu ich kont usługi Azure AD.
- Możesz zarządzać kont w jednej centralnej lokalizacji: portalu Azure.

Aby uzyskać więcej informacji na temat oprogramowania jako usługa (SaaS) integracji aplikacji z usługą Azure AD, zobacz [co to jest dostęp do aplikacji i logowanie jednokrotne z usługą Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Wymagania wstępne

Aby skonfigurować integrację usługi Azure AD z miejsca pracy przez usługi Facebook, potrzebne są następujące elementy:

- Subskrypcję usługi Azure AD
- Dołączanie w serwisie Facebook rejestracji jednokrotnej (SSO) włączone subskrypcji

Aby przetestować kroki opisane w tym samouczku, wykonaj te zalecenia:

- Nie należy używać środowiska produkcyjnego, jeśli jest to konieczne.
- Jeśli nie masz środowisko wersji próbnej usługi Azure AD, możesz [uzyskać miesięczna wersja próbna](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Opis scenariusza
W tym samouczku można przetestować logowania jednokrotnego programu Azure AD w środowisku testowym. Scenariusz opisany w tym samouczku składa się z dwóch głównych elementów:

1. Dodaj obszar roboczy w serwisie Facebook w galerii.
2. Konfiguracja i testowanie usługi Azure AD rejestracji jednokrotnej.

## <a name="add-workplace-by-facebook-from-the-gallery"></a>Dodawanie miejsca pracy przez Facebook z galerii
Aby skonfigurować integrację usługi Azure AD miejsca pracy przez usługi Facebook, należy dodać miejsca pracy przez Facebook z galerii do listy zarządzanych aplikacji SaaS.

1. W [portalu Azure](https://portal.azure.com), w okienku po lewej stronie wybierz **usługi Azure Active Directory**. 

    ![Przycisk usługi Azure Active Directory][1]

2. Przejdź do **aplikacje dla przedsiębiorstw** > **wszystkie aplikacje**.

    ![Blok aplikacje przedsiębiorstwa][2]
    
3. Aby dodać nową aplikację, zaznacz **nowej aplikacji** górnej części okna dialogowego.

    ![Nowy przycisk aplikacji][3]

4. W polu wyszukiwania wpisz **miejsca pracy przez Facebook**i wybierz **miejsca pracy przez Facebook** z wyników. Następnie wybierz **Dodaj**, aby dodać aplikację.

    ![Miejsce pracy przez usługi Facebook na liście wyników](./media/active-directory-saas-facebook-at-work-tutorial/tutorial_workplacebyfacebook_search.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfiguracja i testowanie usługi Azure AD rejestracji jednokrotnej
W tej sekcji Konfiguracja i testowanie usługi Azure AD SSO z miejsca pracy przez Facebook, na podstawie użytkownika testowego, nazywany "Britta Simona".

Dla logowania jednokrotnego do pracy usługi Azure AD musi wiedzieć, użytkownik odpowiednika w miejscu pracy przez Facebook jest dla użytkownika, w usłudze Azure AD. Innymi słowy można ustanowić połączonego relacji między użytkownikiem usługi Azure AD i danemu użytkownikowi w miejscu pracy przez usługi Facebook.

Ustanowić tę relację, przypisując wartość **nazwy użytkownika** w usłudze Azure AD jako wartość **Username** w miejscu pracy przez usługi Facebook.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurowanie usługi Azure AD rejestracji jednokrotnej

W tej sekcji włączenia funkcji logowania jednokrotnego usługi Azure AD w portalu Azure i skonfigurować logowanie Jednokrotne w miejscu pracy aplikacji usługi Facebook.

1. W portalu Azure na **miejsca pracy przez Facebook** strona integracji aplikacji, wybierz opcję **logowanie jednokrotne**.

    ![Skonfigurować łącze rejestracji jednokrotnej][4]

2. W **logowanie jednokrotne** okno dialogowe, wybierz opcję **tryb** jako **na języku SAML logowania jednokrotnego** do włączenia funkcji logowania jednokrotnego.
 
    ![Okno dialogowe rejestracji jednokrotnej](./media/active-directory-saas-facebook-at-work-tutorial/tutorial_workplacebyfacebook_samlbase.png)

3. W **miejsca pracy przez domenę usługi Facebook i adresy URL** sekcji, wykonaj następujące czynności:

    a. W **adres URL logowania** wpisz adres URL, który korzysta z następującego wzorca:`https://<company subdomain>.facebook.com`

    b. W **identyfikator** wpisz adres URL, który korzysta z następującego wzorca:`https://www.facebook.com/company/<scim company id>`

    > [!NOTE]
    > Te wartości są tylko przykładem. Rzeczywisty adres URL logowania i identyfikator, należy zaktualizować te wartości. Skontaktuj się z [miejsca pracy przez zespół obsługi klienta usługi Facebook](https://workplace.fb.com/faq/) uzyskać te wartości. 

4. W **certyfikat podpisywania SAML** wybierz opcję **certyfikatu (Base64)**, a następnie zapisz plik certyfikatu na komputerze.

    ![Łącze pobierania certyfikatu](./media/active-directory-saas-facebook-at-work-tutorial/tutorial_workplacebyfacebook_certificate.png) 

5. Wybierz pozycję **Zapisz**.

    ![Skonfiguruj przycisk pojedynczego logowania jednokrotnego Zapisz](./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_400.png)

6. W **miejsca pracy przez konfigurację usługi Facebook** wybierz opcję **skonfigurować miejsca pracy przez Facebook** otworzyć **Konfigurowanie logowania jednokrotnego** okna. Kopiuj **Sign-Out adres URL, identyfikator jednostki SAML i SAML pojedynczy znak na adres URL usługi** z **krótkimi opisami** sekcji.

    ![Obszar roboczy w konfiguracji usługi Facebook](./media/active-directory-saas-facebook-at-work-tutorial/config.png) 

7. W oknie przeglądarki innej witryny sieci web należy zalogować się jako administrator do miejsca pracy przez witrynę firmy Facebook.
  
   > [!NOTE] 
   > W ramach procesu uwierzytelniania SAML miejsca pracy można użyć ciągów zapytania do 2,5 kilobajtów rozmiaru w celu przekazania parametrów do usługi Azure AD.

8. W **pulpitu nawigacyjnego firmy**, przejdź do **uwierzytelniania** kartę.

9. W obszarze **uwierzytelnianie SAML**, wybierz pozycję **logowania jednokrotnego tylko** z listy rozwijanej.

10. Wprowadź wartości skopiowanych z **miejsca pracy przez konfigurację usługi Facebook** części portalu Azure do odpowiednich pól:

    *   W **adres URL SAML** tekst Wklej wartość **pojedynczy znak na adres URL usługi**, które zostały skopiowane z portalu Azure.
    *   W **adres URL wystawcy SAML** tekst Wklej wartość **identyfikator jednostki SAML**, które zostały skopiowane z portalu Azure.
    *   W **SAML wylogowania przekierowania (opcjonalnie)**, Wklej wartość **Sign-Out URL**, które zostały skopiowane z portalu Azure.
    *   Otwórz z **certyfikatu algorytmem base-64** w Notatniku pobrany z portalu Azure. Skopiuj zawartość go do Schowka, a następnie wklej go do **certyfikatu SAML** pola tekstowego.

11. Może być konieczne wprowadzenie adresu URL odbiorców, odbiorca wyświetlany adres URL i adresu URL ACS (potwierdzenie konsumenta usługa), w obszarze **Konfiguracja SAML** sekcji.

12. Przewiń w dół do sekcji, a następnie wybierz **Test rejestracji Jednokrotnej**. Zostanie wyświetlone okno podręczne z strony logowania usługi Azure AD. W celu uwierzytelnienia wprowadź swoje poświadczenia, jak zwykle. Upewnij się, adres e-mail zwracana z usługi Azure AD jest taka sama jak konto w miejscu pracy, które są rejestrowane przy użyciu.

13. Jeśli test zakończył się pomyślnie, przewiń w dół strony i wybierz **zapisać**.

14. Teraz przedstawiono wszystkich osób korzystających z miejsca pracy z usługą Azure AD strony logowania dla uwierzytelniania.

Można skonfigurować SAML wylogowaniu adresu URL, który może służyć do punktu w wyrejestrowywania usługi Azure AD. Gdy to ustawienie jest włączone i skonfigurowane, użytkownik nie jest kierowany do strony wylogowania obszar roboczy. Użytkownik jest kierowana do adresu URL, który został dodany w ustawieniu przekierowania wylogowania SAML.


> [!TIP]
> Teraz możesz przeczytać zwięzły wersji tych instrukcji wewnątrz [portalu Azure](https://portal.azure.com), podczas konfigurowania aplikacji. Po dodaniu tej aplikacji z **usługi Active Directory** > **aplikacje dla przedsiębiorstw** po prostu zaznacz **rejestracji jednokrotnej** karcie i dostęp do dokumentacji osadzonych za pomocą **konfiguracji** sekcji u dołu. Więcej o funkcji dokumentacji osadzony w [usługi Azure AD osadzonych dokumentacji]( https://go.microsoft.com/fwlink/?linkid=845985).

### <a name="configure-reauthentication-frequency"></a>Konfigurowanie częstotliwości ponowne uwierzytelnianie

Można skonfigurować miejsca pracy z monitem o wyboru SAML codziennie trzech dni, co tydzień, dwa tygodnie, co miesiąc lub nigdy.

> [!NOTE] 
>Minimalna wartość wyboru SAML na aplikacje mobilne wynosi tydzień.

Możesz też wymusić SAML zresetować dla wszystkich użytkowników. Aby to zrobić, użyj **SAML wymagają uwierzytelniania wszystkich użytkowników, którzy teraz** przycisku.


### <a name="create-an-azure-ad-test-user"></a>Tworzenie użytkownika testowego usługi Azure AD
Celem tej sekcji jest tworzenie użytkownika testowego w portalu Azure o nazwie Simona Britta.

![Tworzenie użytkowników usługi Azure AD][100]

1. W **portalu Azure**, w okienku po lewej stronie wybierz **usługi Azure Active Directory**.

    ![Przycisk usługi Azure Active Directory](./media/active-directory-saas-facebook-at-work-tutorial/create_aaduser_01.png) 

2. Aby wyświetlić listę użytkowników, przejdź do **użytkowników i grup**i wybierz **wszyscy użytkownicy**.
    
    !["Użytkownicy i grupy" i "Wszyscy użytkownicy" łącza](./media/active-directory-saas-facebook-at-work-tutorial/create_aaduser_02.png) 

3. Aby otworzyć **użytkownika** okno dialogowe, wybierz opcję **Dodaj**.
 
    ![Przycisk Dodaj](./media/active-directory-saas-facebook-at-work-tutorial/create_aaduser_03.png) 

4. W **użytkownika** okno dialogowe, wykonaj następujące czynności:
 
    ![Okno dialogowe użytkownika](./media/active-directory-saas-facebook-at-work-tutorial/create_aaduser_04.png) 

    a. W **nazwa** polu tekstowym **BrittaSimon**.

    b. W **nazwy użytkownika** polu tekstowym **adres e-mail** z BrittaSimon.

    c. Wybierz **Pokaż hasło**i zapisz ją.

    d. Wybierz pozycję **Utwórz**.
 
### <a name="create-a-workplace-by-facebook-test-user"></a>Utwórz pracy przez użytkownika testowego usługi Facebook

W tej sekcji o nazwie Simona Britta tworzenia użytkownika w miejscu pracy przez usługi Facebook. Obszar roboczy w serwisie Facebook obsługę w czasie, który jest domyślnie włączona.

Brak akcji można w tej sekcji. Jeśli użytkownik nie istnieje w miejscu pracy przez usługi Facebook, nowy jest tworzona przy próbie uzyskania dostępu do miejsca pracy przez usługi Facebook.

>[!Note]
>Jeśli trzeba ręcznie utworzyć użytkownika, skontaktuj się z [miejsca pracy przez zespół obsługi klienta usługi Facebook](https://workplace.fb.com/faq/).

### <a name="assign-the-azure-ad-test-user"></a>Przypisz użytkownika testowego usługi Azure AD

W tej sekcji można włączyć Simona Britta do używania Azure logowania jednokrotnego za udzielanie dostępu do miejsca pracy przez usługi Facebook.

![Przypisz użytkownika][200] 

1. W portalu Azure Otwórz widok aplikacji. Przejdź do widoku katalogu, przejdź do **aplikacje dla przedsiębiorstw**, a następnie wybierz **wszystkie aplikacje**.

    ![Przypisz użytkownika][201] 

2. Na liście aplikacji zaznacz **miejsca pracy przez Facebook**.

    ![Miejsce pracy za pośrednictwem łącza na liście aplikacji usługi Facebook](./media/active-directory-saas-facebook-at-work-tutorial/tutorial_workplacebyfacebook_app.png) 

3. W menu po lewej stronie wybierz **użytkowników i grup**.

    ![Łącze "Użytkownicy i grupy"][202] 

4. Wybierz pozycję **Dodaj**. Następnie w **Dodaj przydziału** okienku wybierz **użytkowników i grup**.

    ![W okienku Dodaj przydziału][203]

5. W **użytkowników i grup** okno dialogowe, wybierz opcję **Simona Britta** na liście Użytkownicy.

6. W **użytkowników i grup** okno dialogowe, wybierz opcję **wybierz**.

7. W **Dodaj przydziału** okno dialogowe, wybierz opcję **przypisać**.
    
### <a name="test-single-sign-on"></a>Test rejestracji jednokrotnej

Jeśli chcesz przetestować ustawienia logowania jednokrotnego, otwórz Panel dostępu.
Aby uzyskać więcej informacji, zobacz [Wprowadzenie do panelu dostępu](active-directory-saas-access-panel-introduction.md).


## <a name="next-steps"></a>Następne kroki

* Zobacz [lista samouczków dotyczących sposobów integracji aplikacji SaaS w usłudze Azure Active Directory](active-directory-saas-tutorial-list.md).
* Odczyt [co to jest dostęp do aplikacji i logowanie jednokrotne z usługą Azure Active Directory?](active-directory-appssoaccess-whatis.md).
* Dowiedz się więcej o sposobie [skonfigurować Inicjowanie obsługi użytkowników](active-directory-saas-facebook-at-work-provisioning-tutorial.md).



<!--Image references-->

[1]: ./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_203.png

