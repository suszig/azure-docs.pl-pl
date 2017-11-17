---
title: "Samouczek: Integracji Azure Active Directory z usługi Zendesk | Dokumentacja firmy Microsoft"
description: "Informacje o sposobie konfigurowania rejestracji jednokrotnej między usługą Azure Active Directory i usługi Zendesk."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 9d7c91e5-78f5-4016-862f-0f3242b00680
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: jeedes
ms.openlocfilehash: ebf07218a6b356d71af51383ac85933ec63b543b
ms.sourcegitcommit: c7215d71e1cdeab731dd923a9b6b6643cee6eb04
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/17/2017
---
# <a name="tutorial-azure-active-directory-integration-with-zendesk"></a>Samouczek: Integracji Azure Active Directory z usługi Zendesk

Z tego samouczka dowiesz się Integrowanie usługi Zendesk w usłudze Azure Active Directory (Azure AD).

Integrowanie usługi Zendesk z usługą Azure AD zapewnia następujące korzyści:

- Można kontrolować w usłudze Azure AD, który ma dostęp do usługi Zendesk
- Umożliwia użytkownikom automatycznie pobrać zalogowane do Zendesk (logowanie jednokrotne) przy użyciu ich kont usługi Azure AD
- Możesz zarządzać kont w jednej centralnej lokalizacji - portalu Azure

Jeśli chcesz dowiedzieć się więcej informacji o integracji aplikacji SaaS w usłudze Azure AD, zobacz [co to jest dostęp do aplikacji i logowanie jednokrotne z usługą Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Wymagania wstępne

Aby skonfigurować integrację usługi Azure AD z usługi Zendesk, potrzebne są następujące elementy:

- Subskrypcję usługi Azure AD
- Zendesk logowanie jednokrotne włączone subskrypcji


> [!NOTE]
> Aby przetestować kroki opisane w tym samouczku, zaleca się używania środowiska produkcyjnego.


Aby przetestować kroki opisane w tym samouczku, należy wykonać te zalecenia:

- Nie należy używać środowiska produkcyjnego, jeśli jest to konieczne.
- Jeśli nie masz środowisko wersji próbnej usługi Azure AD, możesz pobrać miesięczna wersja próbna [tutaj](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Opis scenariusza
W tym samouczku można przetestować usługę Azure AD rejestracji jednokrotnej w środowisku testowym. Scenariusz opisany w tym samouczku składa się z dwóch głównych elementów:

1. Dodawanie Zendesk z galerii
2. Konfigurowanie i testowanie usługi Azure AD logowanie jednokrotne


## <a name="adding-zendesk-from-the-gallery"></a>Dodawanie Zendesk z galerii
Aby skonfigurować integrację usługi Zendesk w usłudze Azure Active Directory, należy dodać Zendesk z galerii do listy zarządzanych aplikacji SaaS.

**Aby dodać Zendesk z galerii, wykonaj następujące czynności:**

1. W  **[Azure Portal](https://portal.azure.com)**, na panelu nawigacyjnym po lewej stronie kliknij **usługi Azure Active Directory** ikony. 

    ![Active Directory][1]

2. Przejdź do **aplikacje dla przedsiębiorstw**. Następnie przejdź do **wszystkie aplikacje**.

    ![aplikacji][2]
    
3. Kliknij przycisk **nowej aplikacji** przycisk w górnej części okna dialogowego.

    ![aplikacji][3]

4. W polu wyszukiwania wpisz **Zendesk**.

    ![Tworzenie użytkownika testowego usługi Azure AD](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_search.png)

5. W panelu wyników wybierz **Zendesk**, a następnie kliknij przycisk **Dodaj** przycisk, aby dodać aplikację.

    ![Tworzenie użytkownika testowego usługi Azure AD](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurowanie i testowanie usługi Azure AD logowanie jednokrotne
W tej sekcji skonfigurować i przetestować usługi Azure AD rejestracji jednokrotnej z usługi Zendesk w oparciu o nazwie "Britta Simona" użytkownika testowego.

Dla rejestracji jednokrotnej do pracy usługi Azure AD musi wiedzieć, użytkownik odpowiednika w Zendesk jest dla użytkownika, w usłudze Azure AD. Innymi słowy link relację między użytkownikiem usługi Azure AD i danemu użytkownikowi w Zendesk musi się.

Ta relacja łącza zostanie nawiązane, przypisując wartość **nazwy użytkownika** w usłudze Azure AD jako wartość **Username** w Zendesk.

Aby skonfigurować i przetestować usługi Azure AD rejestracji jednokrotnej z usługi Zendesk, należy wykonać poniższe bloki konstrukcyjne:

1. **[Konfigurowanie usługi Azure AD rejestracji jednokrotnej](#configuring-azure-ad-single-sign-on)**  — aby umożliwić użytkownikom korzystać z tej funkcji.
2. **[Tworzenie użytkownika testowego usługi Azure AD](#creating-an-azure-ad-test-user)**  — do przetestowania usługi Azure AD rejestracji jednokrotnej z Simona Britta.
3. **[Tworzenie użytkownika testowego Zendesk](#creating-a-zendesk-test-user)**  — w celu zapewnienia odpowiednikiem Simona Britta Zendesk połączonego z usługi Azure AD reprezentację użytkownika.
4. **[Przypisanie użytkownika testowego usługi Azure AD](#assigning-the-azure-ad-test-user)**  — aby umożliwić Simona Britta do użycia usługi Azure AD rejestracji jednokrotnej.
5. **[Testowanie rejestracji jednokrotnej](#testing-single-sign-on)**  — Aby sprawdzić, czy konfiguracja działa.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurowanie usługi Azure AD rejestracji jednokrotnej

W tej sekcji można włączyć usługi Azure AD rejestracji jednokrotnej w portalu Azure i skonfigurować logowanie jednokrotne w aplikacji Zendesk.

**Aby skonfigurować usługi Azure AD rejestracji jednokrotnej z usługi Zendesk, wykonaj następujące czynności:**

1. W portalu Azure na **Zendesk** strona integracji aplikacji, kliknij przycisk **logowanie jednokrotne**.

    ![Konfigurowanie rejestracji jednokrotnej][4]

2. Na **logowanie jednokrotne** okno dialogowe, wybierz opcję **tryb** jako **na języku SAML logowania jednokrotnego** Aby włączyć logowanie jednokrotne.
 
    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_samlbase.png)

3. Na **Zendesk domeny i adres URL** sekcji, wykonaj następujące czynności:

    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_url.png)

    a. W **adres URL logowania** tekstowym, wpisz wartość, przy użyciu następującego wzorca:`https://<subdomain>.zendesk.com`

    b. W **identyfikator** tekstowym, wpisz wartość, przy użyciu następującego wzorca:`<subdomain>.zendesk.com`

    > [!NOTE] 
    > Wartości te nie są prawdziwe. Rzeczywisty adres URL logowania i adres URL identyfikatora, należy zaktualizować te wartości. Skontaktuj się z [zespołem pomocy technicznej usługi Zendesk](https://support.zendesk.com/hc/articles/203663676-Using-SAML-for-single-sign-on-Professional-and-Enterprise) uzyskać te wartości. 

4. Zendesk oczekuje potwierdzenia języka SAML w określonym formacie. Nie obowiązkowych atrybutów SAML, ale Opcjonalnie można dodać atrybutu z **atrybuty użytkownika** sekcji, wykonując następujące czynności: 

     ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_attributes1.png)

    a. Kliknij przycisk **wyświetlanie i edytowanie wszystkimi innymi atrybutami** pole wyboru.
     
    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_attributes2.png)
   
    b. Kliknij przycisk **Dodawanie atrybutu** otworzyć **Dodaj atrybut** okna dialogowego.
    
    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-zendesk-tutorial/tutorial_attribute_05.png)

    c. W **nazwa** tekstowym, wpisz nazwę atrybutu (na przykład **emailaddress**).
    
    d. Z **wartość** , wybierz wartość atrybutu na liście (jako **user.mail**).
    
    e. Kliknij przycisk **Ok**
 
    > [!NOTE] 
    > Atrybuty rozszerzenia umożliwia dodawanie atrybutów, które nie znajdują się w usłudze Azure AD domyślnie. Kliknij przycisk [atrybutów użytkowników, które można ustawić w SAML](https://support.zendesk.com/hc/en-us/articles/203663676-Using-SAML-for-single-sign-on-Professional-and-Enterprise-) Aby uzyskać pełną listę SAML atrybuty, które **Zendesk** akceptuje. 

5. Na **certyfikat podpisywania SAML** sekcji, skopiuj **odcisk PALCA** wartości certyfikatu.

    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_certificate.png) 

6. Na **konfiguracji Zendesk** , kliknij przycisk **skonfigurować Zendesk** otworzyć **Konfigurowanie logowania jednokrotnego** okna. Kopiuj **Sign-Out adresu URL i SAML pojedynczy znak na adres URL usługi** z **sekcji krótkimi opisami.**

    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_configure.png) 

7. W oknie przeglądarki innej witryny sieci web Zaloguj się do witryny firmy Zendesk jako administrator.

8. Kliknij przycisk **Admin**.

9. W okienku nawigacji po lewej stronie kliknij **ustawienia**, a następnie kliknij przycisk **zabezpieczeń**.

10. Na **zabezpieczeń** wykonaj następujące czynności: 
   
     ![Zabezpieczenia](./media/active-directory-saas-zendesk-tutorial/ic773089.png "zabezpieczeń")

    ![Logowanie jednokrotne](./media/active-directory-saas-zendesk-tutorial/ic773090.png "logowanie jednokrotne")

     a. Kliknij przycisk **Admin & agentów** kartę.

     b. Wybierz **logowanie jednokrotne (SSO) oraz SAML**, a następnie wybierz **SAML**.

     c. W **adres URL logowania jednokrotnego SAML** pole tekstowe, Wklej wartość **SAML pojedynczy znak na adres URL usługi** którego została skopiowana z portalu Azure. 

     d. W **zdalnego adresu URL wylogowania** pole tekstowe, Wklej wartość **Sign-Out URL** którego została skopiowana z portalu Azure.
        
     e. W **odcisk palca certyfikatu** pole tekstowe, Wklej **odcisk palca** wartość certyfikatów, które zostały skopiowane z portalu Azure.
     
     f. Kliknij pozycję **Zapisz**.

### <a name="creating-an-azure-ad-test-user"></a>Tworzenie użytkownika testowego usługi Azure AD
Celem tej sekcji jest tworzenie użytkownika testowego w portalu Azure o nazwie Simona Britta.

![Tworzenie użytkowników usługi Azure AD][100]

**Aby utworzyć użytkownika testowego w usłudze Azure AD, wykonaj następujące czynności:**

1. W **portalu Azure**, w lewym okienku nawigacji, kliknij polecenie **usługi Azure Active Directory** ikony.

    ![Tworzenie użytkownika testowego usługi Azure AD](./media/active-directory-saas-zendesk-tutorial/create_aaduser_01.png) 

2. Aby wyświetlić listę użytkowników, przejdź do **użytkowników i grup** i kliknij przycisk **wszyscy użytkownicy**.
    
    ![Tworzenie użytkownika testowego usługi Azure AD](./media/active-directory-saas-zendesk-tutorial/create_aaduser_02.png) 

3. W górnej części okna dialogowego, kliknij przycisk **Dodaj** otworzyć **użytkownika** okna dialogowego.
 
    ![Tworzenie użytkownika testowego usługi Azure AD](./media/active-directory-saas-zendesk-tutorial/create_aaduser_03.png) 

4. Na **użytkownika** okna dialogowego strony, należy wykonać następujące czynności:
 
    ![Tworzenie użytkownika testowego usługi Azure AD](./media/active-directory-saas-zendesk-tutorial/create_aaduser_04.png) 

    a. W **nazwa** pole tekstowe, typ **BrittaSimon**.

    b. W **nazwy użytkownika** pole tekstowe, typ **adres e-mail** z BrittaSimon.

    c. Wybierz **Pokaż hasło** i zanotuj wartość **hasło**.

    d. Kliknij przycisk **Utwórz**. 

### <a name="creating-a-zendesk-test-user"></a>Tworzenie użytkownika testowego Zendesk

Aby umożliwić użytkownikom zalogować się do usługi Azure AD **Zendesk**, muszą mieć przydzielone do **Zendesk**.  
W zależności od roli przypisanej w aplikacji jest oczekiwane zachowanie:

 1. **Użytkownik końcowy** konta są automatycznie konfigurowani podczas logowania.
 2. **Agent** i **Admin** należy ręcznie udostępniane w kont **Zendesk** przed zarejestrowaniem się.
 
**Aby udostępnić konta użytkownika, wykonaj następujące czynności:**

1. Zaloguj się do Twojego **Zendesk** dzierżawy.

2. Wybierz **listy odbiorców** kartę.

3. Wybierz **użytkownika** , a następnie kliknij pozycję **Dodaj**.
   
    ![Dodaj użytkownika](./media/active-directory-saas-zendesk-tutorial/ic773632.png "Dodaj użytkownika")
4. Wpisz adres e-mail istniejącego konta usługi Azure AD do udostępniania, a następnie kliknij przycisk **zapisać**.
   
    ![Nowy użytkownik](./media/active-directory-saas-zendesk-tutorial/ic773633.png "nowego użytkownika")

> [!NOTE]
> Możesz użyć innych Zendesk użytkownika konta tworzenia narzędzi lub interfejsów API dostarczonych przez Zendesk do kont użytkowników usługi AAD.


### <a name="assigning-the-azure-ad-test-user"></a>Przypisanie użytkownika testowego usługi Azure AD

W tej sekcji musisz włączyć Simona Britta do używania Azure logowania jednokrotnego za udzielanie dostępu do usługi Zendesk.

![Przypisz użytkownika][200] 

**Aby przypisać Simona Britta Zendesk, wykonaj następujące czynności:**

1. W portalu Azure Otwórz widok aplikacji, a następnie przejdź do widoku katalogu i przejdź do **aplikacje dla przedsiębiorstw** kliknięcie **wszystkie aplikacje**.

    ![Przypisz użytkownika][201] 

2. Na liście aplikacji zaznacz **Zendesk**.

    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_app.png) 

3. W menu po lewej stronie kliknij **użytkowników i grup**.

    ![Przypisz użytkownika][202] 

4. Kliknij przycisk **Dodaj** przycisku. Następnie wybierz **użytkowników i grup** na **Dodaj przydziału** okna dialogowego.

    ![Przypisz użytkownika][203]

5. Na **użytkowników i grup** okno dialogowe, wybierz opcję **Simona Britta** na liście Użytkownicy.

6. Kliknij przycisk **wybierz** znajdującego się na **użytkowników i grup** okna dialogowego.

7. Kliknij przycisk **przypisać** znajdującego się na **Dodaj przydziału** okna dialogowego.
    
### <a name="testing-single-sign-on"></a>Testowanie rejestracji jednokrotnej

W tej sekcji można przetestować konfiguracji usługi Azure AD pojedynczego logowania za pomocą panelu dostępu.

Po kliknięciu kafelka Zendesk w panelu dostępu użytkownik powinien pobrać automatycznie zalogowane do aplikacji usługi Zendesk.
Aby uzyskać więcej informacji na temat panelu dostępu, zobacz [wprowadzenie do panelu dostępu](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Lista samouczków dotyczących sposobów integracji aplikacji SaaS przy użyciu usługi Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co to jest dostęp do aplikacji i logowanie jednokrotne z usługą Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_203.png
