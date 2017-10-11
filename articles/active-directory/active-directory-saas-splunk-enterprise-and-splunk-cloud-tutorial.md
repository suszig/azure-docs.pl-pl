---
title: "Samouczek: Integracji usługi Azure Active Directory z Splunk przedsiębiorstwa i w chmurze Splunk | Dokumentacja firmy Microsoft"
description: "Informacje o sposobie konfigurowania rejestracji jednokrotnej między usługi Azure Active Directory i Splunk Enterprise i Splunk chmury."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: b3e2b4a9-749c-4895-813d-db46f8dfdbf8
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 3/09/2017
ms.author: jeedes
ms.openlocfilehash: b78e9b7161207a74880e912241d5e965b353d1c5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-splunk-enterprise-and-splunk-cloud"></a>Samouczek: Integracji usługi Azure Active Directory z Splunk przedsiębiorstwa i w chmurze Splunk

Z tego samouczka dowiesz integrowanie Splunk przedsiębiorstwa i w chmurze Splunk z usługi Azure Active Directory (Azure AD).

Integracja z usługą Azure AD Splunk przedsiębiorstwa i w chmurze Splunk zapewnia następujące korzyści:

- Można kontrolować w usłudze Azure AD, który ma dostęp do Splunk przedsiębiorstwa i w chmurze Splunk
- Umożliwia użytkownikom automatycznie pobrać zalogowane do Splunk przedsiębiorstwa i w chmurze Splunk rejestracji jednokrotnej (SSO) przy użyciu ich kont usługi Azure AD
- Możesz zarządzać kont w jednej centralnej lokalizacji - klasycznego portalu Azure

Jeśli chcesz dowiedzieć się więcej informacji o integracji aplikacji SaaS w usłudze Azure AD, zobacz [co to jest dostęp do aplikacji i logowanie jednokrotne z usługą Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Wymagania wstępne

Aby skonfigurować integrację usługi Azure AD z Splunk przedsiębiorstwa i w chmurze Splunk, potrzebne są następujące elementy:

- Subskrypcję usługi Azure AD
- Splunk Enterprise lub logowania jednokrotnego chmury Splunk włączone subskrypcji


>[!NOTE]
>Aby przetestować kroki opisane w tym samouczku, zaleca się używania środowiska produkcyjnego.
>

Aby przetestować kroki opisane w tym samouczku, należy wykonać te zalecenia:

- Nie należy używać środowiska produkcyjnego, chyba że jest to konieczne.
- Jeśli nie masz środowisko wersji próbnej usługi Azure AD, możesz pobrać [miesięczna wersja próbna](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Opis scenariusza
W tym samouczku można przetestować usługę Azure AD rejestracji jednokrotnej w środowisku testowym.

Scenariusz opisany w tym samouczku składa się z dwóch głównych elementów:

1. Dodawanie Splunk przedsiębiorstwa i w chmurze Splunk z galerii
2. Konfigurowanie i testowania logowania jednokrotnego programu Azure AD


## <a name="add-splunk-enterprise-and-splunk-cloud-from-the-gallery"></a>Dodaj Splunk przedsiębiorstwa i w chmurze Splunk z galerii
Aby skonfigurować integrację Splunk przedsiębiorstwa i Splunk chmury do usługi Azure AD, należy dodać Splunk przedsiębiorstwa i w chmurze Splunk z galerii do listy zarządzanych aplikacji SaaS.

**Aby dodać Splunk przedsiębiorstwa i w chmurze Splunk z galerii, wykonaj następujące czynności:**

1. W **klasycznego portalu Azure**, w lewym okienku nawigacji, kliknij polecenie **usługi Active Directory**.

    ![Usługa Active Directory][1]

2. Z **katalogu** listy, wybierz katalog, dla którego chcesz włączyć integracji katalogów.

3. Aby otworzyć widok aplikacji, w widoku katalogu, kliknij przycisk **aplikacji** w menu u góry.

    ![Aplikacje][2]

4. Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Aplikacje][3]

5. Na **co chcesz zrobić** okna dialogowego, kliknij przycisk **dodać aplikację z galerii**.

    ![Aplikacje][4]

6. W polu wyszukiwania wpisz **Splunk przedsiębiorstwa lub w chmurze Splunk**.

    ![Tworzenie użytkownika testowego usługi Azure AD](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_splunk_01.png)

7. W okienku wyników wybierz **Splunk przedsiębiorstwa i w chmurze Splunk**, a następnie kliknij przycisk **Complete** można dodać aplikację.

    ![Tworzenie użytkownika testowego usługi Azure AD](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_splunk_02.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfiguracja i testowanie usługi Azure AD rejestracji jednokrotnej
W tej sekcji możesz skonfigurować i przetestować usługi Azure AD rejestracji jednokrotnej z Splunk przedsiębiorstwa i Splunk chmury oparte na użytkownika testowego o nazwie "Britta Simona".

Dla rejestracji jednokrotnej do pracy usługi Azure AD musi wiedzieć, użytkownik odpowiednika w Splunk przedsiębiorstwa i w chmurze Splunk jest dla użytkownika, w usłudze Azure AD. Innymi słowy link relację między użytkownikiem usługi Azure AD i danemu użytkownikowi w organizacji Splunk i w chmurze Splunk musi się.

Ta relacja łącza zostanie nawiązane, przypisując wartość **nazwy użytkownika** w usłudze Azure AD jako wartość **nazwy użytkownika** Splunk przedsiębiorstwa i Splunk chmury.

Aby skonfigurować i przetestować usługi Azure AD rejestracji jednokrotnej z Splunk przedsiębiorstwa i w chmurze Splunk, należy wykonać poniższe bloki konstrukcyjne:

1. **[Konfigurowanie usługi Azure AD rejestracji jednokrotnej](#configuring-azure-ad-single-sign-on)**  — aby umożliwić użytkownikom korzystać z tej funkcji.
2. **[Tworzenie użytkownika testowego usługi Azure AD](#creating-an-azure-ad-test-user)**  — do przetestowania usługi Azure AD rejestracji jednokrotnej z Simona Britta.
3. **[Tworzenie użytkownika testowego Splunk przedsiębiorstwa i w chmurze Splunk](#creating-a-splunk-enterprise-and-splunk-cloud-test-user)**  — mają odpowiednika Simona Britta w przedsiębiorstwie Splunk i Splunk chmurze, która jest połączona z jej reprezentacji usługi Azure AD.
4. **[Przypisanie użytkownika testowego usługi Azure AD](#assigning-the-azure-ad-test-user)**  — aby umożliwić Simona Britta do użycia usługi Azure AD rejestracji jednokrotnej.
5. **[Testowanie rejestracji jednokrotnej](#testing-single-sign-on)**  — Aby sprawdzić, czy konfiguracja działa.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurowanie usługi Azure AD rejestracji jednokrotnej

W tej sekcji włączenia funkcji logowania jednokrotnego usługi Azure AD w klasycznym portalu i konfigurowanie logowania jednokrotnego w aplikacji przedsiębiorstwa Splunk i Splunk chmury.


**Aby skonfigurować usługi Azure AD rejestracji jednokrotnej z Splunk przedsiębiorstwa i Splunk chmury, wykonaj następujące czynności:**

1. W klasycznym portalu na **Splunk przedsiębiorstwa i w chmurze Splunk** strona integracji aplikacji, kliknij przycisk **skonfigurować logowanie jednokrotne** otworzyć **skonfigurować logowanie jednokrotne**okna dialogowego.
     
    ![Konfigurowanie rejestracji jednokrotnej][6] 

2. Na **jak chcesz użytkownikom logowanie się Splunk przedsiębiorstwa i w chmurze Splunk** wybierz pozycję **Azure AD rejestracji jednokrotnej**, a następnie kliknij przycisk **dalej**.

    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_splunk_03.png) 

3. Na **Konfigurowanie ustawień aplikacji** okna dialogowego strony, należy wykonać następujące czynności:

    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_splunk_04.png) 
  1. W **na adres URL logowania** tekstowym, wpisz adres URL używany przez użytkowników do logowania jednokrotnego do aplikacji przedsiębiorstwa Splunk i w chmurze Splunk przy użyciu następującego wzorca:`https://<splunkserverUrl>/en-US/app/launcher/home`
  2. W **identyfikator** tekstowym, wpisz adres URL serwera Splunk.
  3. W **adres URL odpowiedzi** tekstowym, wpisz adres URL z następującego wzorca:`https://<splunkserver>/saml/acs`
  4. Kliknij przycisk **Dalej**.
 
4. Na **skonfigurować logowanie jednokrotne w Splunk przedsiębiorstwa i w chmurze Splunk** wykonaj następujące czynności:

    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_splunk_05.png)
  1. Kliknij przycisk **pobierania metadanych**, a następnie zapisz plik na komputerze.
  2. Kliknij przycisk **Dalej**.

5. Aby uzyskać logowania jednokrotnego skonfigurowane dla aplikacji, skontaktuj się z Splunk przedsiębiorstwa i Splunk chmury z pomocą techniczną i podaj z następującymi:

    * Pobrany **federaton metadanych**
6. W klasycznym portalu, wybierz Potwierdzenie konfiguracji rejestracji jednokrotnej, a następnie kliknij przycisk **dalej**.
    
    ![Azure AD rejestracji jednokrotnej][10]

7. Na **pojedynczy znak na potwierdzenie** kliknij przycisk **Complete**.  
 
    ![Azure AD rejestracji jednokrotnej][11]

### <a name="create-an-azure-ad-test-user"></a>Tworzenie użytkownika testowego usługi Azure AD
W tej sekcji utworzysz użytkownika testowego w klasycznym portalu o nazwie Simona Britta.

![Tworzenie użytkowników usługi Azure AD][20]

**Aby utworzyć użytkownika testowego w usłudze Azure AD, wykonaj następujące czynności:**

1. W **klasycznego portalu Azure**, w lewym okienku nawigacji, kliknij polecenie **usługi Active Directory**.

    ![Tworzenie użytkownika testowego usługi Azure AD](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/create_aaduser_09.png) 

2. Z **katalogu** listy, wybierz katalog, dla którego chcesz włączyć integracji katalogów.

3. Aby wyświetlić listę użytkowników, w menu u góry, kliknij przycisk **użytkowników**.

    ![Tworzenie użytkownika testowego usługi Azure AD](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/create_aaduser_03.png) 

4. Aby otworzyć **Dodaj użytkownika** okna dialogowego na pasku narzędzi u dołu, kliknij przycisk **Dodaj użytkownika**.

    ![Tworzenie użytkownika testowego usługi Azure AD](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/create_aaduser_04.png) 

5. Na **Poinformuj nas o tym użytkowniku** okna dialogowego strony, należy wykonać następujące czynności:

    ![Tworzenie użytkownika testowego usługi Azure AD](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/create_aaduser_05.png) 
  1. Jako typ użytkownika wybierz nowego użytkownika w organizacji.
  2. W nazwie użytkownika **pole tekstowe**, typ **BrittaSimon**.
  3. Kliknij przycisk **Dalej**.

6.  Na **profilu użytkownika** okna dialogowego strony, należy wykonać następujące czynności:
  
    ![Tworzenie użytkownika testowego usługi Azure AD](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/create_aaduser_06.png) 
  1. W **imię** pole tekstowe, typ **Britta**.  
  2. W **nazwisko** pole tekstowe, typ **Simona**.
  3. W **Nazwa wyświetlana** pole tekstowe, typ **Simona Britta**.
  4. W **roli** listy, wybierz **użytkownika**.
  5. Kliknij przycisk **Dalej**.

7. Na **Uzyskaj hasło tymczasowe** strony okna dialogowego, kliknij przycisk **utworzyć**.

    ![Tworzenie użytkownika testowego usługi Azure AD](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/create_aaduser_07.png) 

8. Na **Uzyskaj hasło tymczasowe** okna dialogowego strony, należy wykonać następujące czynności:

    ![Tworzenie użytkownika testowego usługi Azure AD](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/create_aaduser_08.png) 
  1. Zanotuj wartość **nowe hasło**.
  2. Kliknij przycisk **Complete** (Zakończ).   

### <a name="create-a-splunk-enterprise-and-splunk-cloud-test-user"></a>Tworzenie Splunk przedsiębiorstwa i chmury Splunk użytkownika testowego

W tej sekcji utworzysz użytkownika o nazwie Simona Britta w przedsiębiorstwie Splunk i Splunk chmury. Proszę współpracować z Splunk Enterprise i chmury Splunk zespołem pomocy technicznej, aby dodać użytkowników na platformie Splunk przedsiębiorstwa i w chmurze Splunk.


### <a name="assign-the-azure-ad-test-user"></a>Przypisz użytkownika testowego usługi Azure AD

W tej sekcji można włączyć Simona Britta umożliwia udostępnienie jej Splunk przedsiębiorstwa i w chmurze Splunk SSOy Azure.

![Przypisz użytkownika][200] 

**Aby przypisać Simona Britta do Splunk przedsiębiorstwa i Splunk chmury, wykonaj następujące czynności:**

1. W klasycznym portalu, aby otworzyć widok aplikacji, w widoku katalogu, kliknij przycisk **aplikacji** w menu u góry.

    ![Przypisz użytkownika][201] 

2. Na liście aplikacji zaznacz **Splunk przedsiębiorstwa i w chmurze Splunk**.

    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_splunk_50.png) 

3. W menu u góry kliknij **użytkowników**.

    ![Przypisz użytkownika][203]

4. Na liście użytkowników wybierz **Simona Britta**.

5. Na pasku narzędzi u dołu, kliknij przycisk **przypisać**.

    ![Przypisz użytkownika][205]

### <a name="test-single-sign-on"></a>Test rejestracji jednokrotnej

W tej sekcji można przetestować programu Azure AD SSOonfiguration, za pomocą panelu dostępu.

Po kliknięciu Splunk Enterprise i chmury Splunk kafelka w panelu dostępu, możesz należy pobrać automatycznie zalogowane Splunk przedsiębiorstwa i w chmurze Splunk aplikacji.


## <a name="additional-resources"></a>Dodatkowe zasoby

* [Lista samouczków dotyczących sposobów integracji aplikacji SaaS przy użyciu usługi Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co to jest dostęp do aplikacji i logowanie jednokrotne z usługą Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_205.png
