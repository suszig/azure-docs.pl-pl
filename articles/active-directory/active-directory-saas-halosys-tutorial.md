---
title: 'Samouczek: Integracji Azure Active Directory z Halosys | Dokumentacja firmy Microsoft'
description: "Dowiedz się, jak używać Halosys usłudze Azure Active Directory w celu włączenia logowania jednokrotnego, automatyczne Inicjowanie obsługi i inne!"
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 42a0eb7c-5cb7-44a9-b00b-b0e7df4b63e8
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/22/2017
ms.author: jeedes
ms.openlocfilehash: 18c5cd8eb4ca211f8ae2b8dd994c0e8c48625a2f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-halosys"></a>Samouczek: Integracji Azure Active Directory z Halosys

Z tego samouczka dowiesz się integrowanie Halosys z usługi Azure Active Directory (Azure AD).

Integracja z usługą Azure AD Halosys zapewnia następujące korzyści:

- Można kontrolować w usłudze Azure AD, który ma dostęp do Halosys
- Umożliwia użytkownikom automatycznie pobrać zalogowane do Halosys (logowanie jednokrotne) przy użyciu ich kont usługi Azure AD
- Możesz zarządzać kont w jednej centralnej lokalizacji - klasycznego portalu Azure

Jeśli chcesz dowiedzieć się więcej informacji o integracji aplikacji SaaS w usłudze Azure AD, zobacz [co to jest dostęp do aplikacji i logowanie jednokrotne z usługą Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Wymagania wstępne

Aby skonfigurować integrację usługi Azure AD z Halosys, potrzebne są następujące elementy:

- Subskrypcję usługi Azure AD
- Halosys jednokrotnego włączone subskrypcji


> [!NOTE] 
> Aby przetestować kroki opisane w tym samouczku, zaleca się używania środowiska produkcyjnego.


Aby przetestować kroki opisane w tym samouczku, należy wykonać te zalecenia:

- Nie należy używać środowiska produkcyjnego, chyba że jest to konieczne.
- Jeśli nie masz środowisko wersji próbnej usługi Azure AD, możesz pobrać miesięczna wersja próbna [tutaj](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Opis scenariusza
W tym samouczku można przetestować usługę Azure AD rejestracji jednokrotnej w środowisku testowym.

Scenariusz opisany w tym samouczku składa się z dwóch głównych elementów:

1. Dodawanie Halosys z galerii
2. Konfigurowanie i testowanie usługi Azure AD logowanie jednokrotne


## <a name="adding-halosys-from-the-gallery"></a>Dodawanie Halosys z galerii
Aby skonfigurować integrację usługi Azure AD Halosys, należy dodać Halosys z galerii do listy zarządzanych aplikacji SaaS.

**Aby dodać Halosys z galerii, wykonaj następujące czynności:**

1. W **klasycznego portalu Azure**, w lewym okienku nawigacji, kliknij polecenie **usługi Active Directory**.

    ![Usługa Active Directory][1]
2. Z **katalogu** listy, wybierz katalog, dla którego chcesz włączyć integracji katalogów.

3. Aby otworzyć widok aplikacji, w widoku katalogu, kliknij przycisk **aplikacji** w menu u góry.

    ![Aplikacje][2]

4. Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Aplikacje][3]

5. Na **co chcesz zrobić** okna dialogowego, kliknij przycisk **dodać aplikację z galerii**.

    ![Aplikacje][4]

6. W polu wyszukiwania wpisz **Halosys**.

    ![Tworzenie użytkownika testowego usługi Azure AD](./media/active-directory-saas-Halosys-tutorial/tutorial_Halosys_01.png)
    
7. W okienku wyników wybierz **Halosys**, a następnie kliknij przycisk **Complete** można dodać aplikację.

    ![Tworzenie użytkownika testowego usługi Azure AD](./media/active-directory-saas-Halosys-tutorial/tutorial_Halosys_011.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurowanie i testowanie usługi Azure AD logowanie jednokrotne
W tej sekcji skonfigurować i przetestować usługi Azure AD rejestracji jednokrotnej z Halosys w oparciu o nazwie "Britta Simona" użytkownika testowego.

Dla rejestracji jednokrotnej do pracy usługi Azure AD musi wiedzieć, użytkownik odpowiednika w Halosys jest dla użytkownika, w usłudze Azure AD. Innymi słowy link relację między użytkownikiem usługi Azure AD i danemu użytkownikowi w Halosys musi się.

Ta relacja łącza zostanie nawiązane, przypisując wartość **nazwy użytkownika** w usłudze Azure AD jako wartość **Username** w Halosys.

Aby skonfigurować i przetestować usługi Azure AD rejestracji jednokrotnej z Halosys, należy wykonać poniższe bloki konstrukcyjne:

1. **[Konfigurowanie usługi Azure AD rejestracji jednokrotnej](#configuring-azure-ad-single-sign-on)**  — aby umożliwić użytkownikom korzystać z tej funkcji.
2. **[Tworzenie użytkownika testowego usługi Azure AD](#creating-an-azure-ad-test-user)**  — do przetestowania usługi Azure AD rejestracji jednokrotnej z Simona Britta.
3. **[Tworzenie użytkownika testowego Halosys](#creating-a-halosys-test-user)**  — w celu zapewnienia odpowiednikiem Simona Britta Halosys połączonego z jej reprezentacji usługi Azure AD.
4. **[Przypisanie użytkownika testowego usługi Azure AD](#assigning-the-azure-ad-test-user)**  — aby umożliwić Simona Britta do użycia usługi Azure AD rejestracji jednokrotnej.
5. **[Testowanie rejestracji jednokrotnej](#testing-single-sign-on)**  — Aby sprawdzić, czy konfiguracja działa.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurowanie usługi Azure AD rejestracji jednokrotnej

W tej sekcji można włączyć usługi Azure AD rejestracji jednokrotnej w klasycznym portalu i skonfigurować logowanie jednokrotne w aplikacji Halosys.


**Aby skonfigurować usługi Azure AD rejestracji jednokrotnej z Halosys, wykonaj następujące czynności:**

1. W klasycznym portalu na **Halosys** strona integracji aplikacji, kliknij przycisk **skonfigurować logowanie jednokrotne** otworzyć **skonfigurować logowanie jednokrotne** okna dialogowego.
     
    ![Konfigurowanie rejestracji jednokrotnej][6] 

2. Na **jak chcesz użytkownikom zalogować się na Halosys** wybierz pozycję **Azure AD rejestracji jednokrotnej**, a następnie kliknij przycisk **dalej**.

    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-Halosys-tutorial/tutorial_Halosys_03.png) 

3. Na **Konfigurowanie ustawień aplikacji** okna dialogowego strony, należy wykonać następujące czynności:

    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-Halosys-tutorial/tutorial_Halosys_04.png) 

    a. W **na adres URL logowania** tekstowym, wpisz adres URL używany przez użytkowników do logowania jednokrotnego do aplikacji Halosys przy użyciu następującego wzorca: `https://<company-name>.Halosys.com/client-api/api`.

    b.In **adres URL identyfikatora** tekstowym, wpisz adres URL do następującego wzorca: `https://<company-name>.Halosys.com`.   
         
4. Na **skonfigurować logowanie jednokrotne w Halosys** kliknij przycisk **pobierania metadanych**, a następnie zapisz plik na komputerze:

    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-Halosys-tutorial/tutorial_Halosys_05.png)
   
5. Aby uzyskać logowania jednokrotnego skonfigurowane dla aplikacji, skontaktuj się z zespołem pomocy technicznej Halosys i podaj z następujących czynności:

    • Pobrany **pliku metadanych**
    
    • **Adres URL logowania jednokrotnego SAML**
    

6. W klasycznym portalu, wybierz Potwierdzenie konfiguracji rejestracji jednokrotnej, a następnie kliknij przycisk **dalej**.
    
    ![Azure AD rejestracji jednokrotnej][10]

7. Na **pojedynczy znak na potwierdzenie** kliknij przycisk **Complete**.  
 
    ![Azure AD rejestracji jednokrotnej][11]


### <a name="creating-an-azure-ad-test-user"></a>Tworzenie użytkownika testowego usługi Azure AD
W tej sekcji utworzysz użytkownika testowego w klasycznym portalu o nazwie Simona Britta.


![Tworzenie użytkowników usługi Azure AD][20]

**Aby utworzyć użytkownika testowego w usłudze Azure AD, wykonaj następujące czynności:**

1. W **klasycznego portalu Azure**, w lewym okienku nawigacji, kliknij polecenie **usługi Active Directory**.

    ![Tworzenie użytkownika testowego usługi Azure AD](./media/active-directory-saas-Halosys-tutorial/create_aaduser_09.png) 

2. Z **katalogu** listy, wybierz katalog, dla którego chcesz włączyć integracji katalogów.

3. Aby wyświetlić listę użytkowników, w menu u góry, kliknij przycisk **użytkowników**.

    ![Tworzenie użytkownika testowego usługi Azure AD](./media/active-directory-saas-Halosys-tutorial/create_aaduser_03.png) 

4. Aby otworzyć **Dodaj użytkownika** okna dialogowego na pasku narzędzi u dołu, kliknij przycisk **Dodaj użytkownika**.

    ![Tworzenie użytkownika testowego usługi Azure AD](./media/active-directory-saas-Halosys-tutorial/create_aaduser_04.png) 

5. Na **Poinformuj nas o tym użytkowniku** okna dialogowego wykonaj następujące kroki: ![Tworzenie użytkownika testowego usługi Azure AD](./media/active-directory-saas-Halosys-tutorial/create_aaduser_05.png) 

    a. Jako typ użytkownika wybierz nowego użytkownika w organizacji.

    b. W nazwie użytkownika **pole tekstowe**, typ **BrittaSimon**.

    c. Kliknij przycisk **Dalej**.

6.  Na **profilu użytkownika** okna dialogowego wykonaj następujące kroki: ![Tworzenie użytkownika testowego usługi Azure AD](./media/active-directory-saas-Halosys-tutorial/create_aaduser_06.png) 

    a. W **imię** pole tekstowe, typ **Britta**.  

    b. W **nazwisko** pole tekstowe, typ **Simona**.

    c. W **Nazwa wyświetlana** pole tekstowe, typ **Simona Britta**.

    d. W **roli** listy, wybierz **użytkownika**.

    e. Kliknij przycisk **Dalej**.

7. Na **Uzyskaj hasło tymczasowe** strony okna dialogowego, kliknij przycisk **utworzyć**.

    ![Tworzenie użytkownika testowego usługi Azure AD](./media/active-directory-saas-Halosys-tutorial/create_aaduser_07.png) 

8. Na **Uzyskaj hasło tymczasowe** okna dialogowego strony, należy wykonać następujące czynności:

    ![Tworzenie użytkownika testowego usługi Azure AD](./media/active-directory-saas-Halosys-tutorial/create_aaduser_08.png) 

    a. Zanotuj wartość **nowe hasło**.

    b. Kliknij przycisk **Complete** (Zakończ).   



### <a name="creating-a-halosys-test-user"></a>Tworzenie użytkownika testowego Halosys

W tej sekcji należy utworzyć użytkownika o nazwie Simona Britta w Halosys. Należy współpracować z zespołem pomocy technicznej Halosys Aby dodać użytkowników do platformy Halosys.


### <a name="assigning-the-azure-ad-test-user"></a>Przypisanie użytkownika testowego usługi Azure AD

W tej sekcji można włączyć Simona Britta do udostępnienia jej Halosys za pomocą usługi Azure rejestracji jednokrotnej.

![Przypisz użytkownika][200] 

**Aby przypisać Simona Britta Halosys, wykonaj następujące czynności:**

1. W klasycznym portalu, aby otworzyć widok aplikacji, w widoku katalogu, kliknij przycisk **aplikacji** w menu u góry.

    ![Przypisz użytkownika][201] 

2. Na liście aplikacji zaznacz **Halosys**.

    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-Halosys-tutorial/tutorial_Halosys_50.png) 

3. W menu u góry kliknij **użytkowników**.

    ![Przypisz użytkownika][203]

4. Na liście użytkowników wybierz **Simona Britta**.

5. Na pasku narzędzi u dołu, kliknij przycisk **przypisać**.

    ![Przypisz użytkownika][205]


### <a name="testing-single-sign-on"></a>Testowanie rejestracji jednokrotnej

W tej sekcji można przetestować konfiguracji usługi Azure AD pojedynczego logowania za pomocą panelu dostępu.

Po kliknięciu kafelka Halosys w panelu dostępu użytkownik powinien pobrać automatycznie zalogowane do aplikacji Halosys.


## <a name="additional-resources"></a>Dodatkowe zasoby

* [Lista samouczków dotyczących sposobów integracji aplikacji SaaS przy użyciu usługi Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co to jest dostęp do aplikacji i logowanie jednokrotne z usługą Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_205.png
