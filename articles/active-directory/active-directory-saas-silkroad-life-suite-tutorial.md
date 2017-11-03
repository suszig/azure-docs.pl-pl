---
title: "Samouczek: Azure Active Directory integracji z pakietem życia SilkRoad | Dokumentacja firmy Microsoft"
description: "Informacje o sposobie konfigurowania rejestracji jednokrotnej między usługą Azure Active Directory i SilkRoad życia pakietu."
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: 3cd92319-7964-41eb-8712-444f5c8b4d15
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 3/10/2017
ms.author: jeedes
ms.openlocfilehash: ecf4e31ecea00d003fc47ea4cebb781ca58957f7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-silkroad-life-suite"></a>Samouczek: Azure Active Directory integracji z pakietem życia SilkRoad
Celem tego samouczka jest pokazanie sposobu integracji SilkRoad Suite użytkowania z usługą Azure Active Directory (Azure AD). 

Integrowanie SilkRoad Suite użytkowania z usługą Azure AD zapewnia następujące korzyści: 

* Można kontrolować w usłudze Azure AD, który ma dostęp do zestawu życia SilkRoad 
* Umożliwia użytkownikom automatycznie pobrać zalogowane do SilkRoad życia pakiet rejestracji jednokrotnej (SSO) przy użyciu ich kont usługi Azure AD

Jeśli chcesz dowiedzieć się więcej informacji o integracji aplikacji SaaS w usłudze Azure AD, zobacz [co to jest dostęp do aplikacji i logowanie jednokrotne z usługą Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Wymagania wstępne
Aby skonfigurować integrację usługi Azure AD z pakietem życia SilkRoad, potrzebne są następujące elementy:

* Subskrypcję usługi Azure AD
* Subskrypcja SilkRoad życia Suite logowanie Jednokrotne włączone

>[!NOTE]
>Aby przetestować kroki opisane w tym samouczku, zaleca się używania środowiska produkcyjnego. 
> 

Aby przetestować kroki opisane w tym samouczku, należy wykonać te zalecenia:

* Nie należy używać środowiska produkcyjnego, chyba że jest to konieczne.
* Jeśli nie masz środowisko wersji próbnej usługi Azure AD, możesz pobrać [miesięczna wersja próbna](https://azure.microsoft.com/pricing/free-trial/). 

## <a name="scenario-description"></a>Opis scenariusza
Celem tego samouczka jest umożliwienie test rejestracji Jednokrotnej programu Azure AD w środowisku testowym.

Scenariusz opisany w tym samouczku składa się z dwóch głównych elementów:

1. Dodawanie pakietu życia SilkRoad z galerii 
2. Konfigurowanie i testowania logowania jednokrotnego programu Azure AD

## <a name="add-silkroad-life-suite-from-the-gallery"></a>Dodawanie zestawu życia SilkRoad z galerii
Aby skonfigurować integrację usługi Azure AD SilkRoad życia pakietu, należy dodać pakiet życia SilkRoad z galerii do listy zarządzanych aplikacji SaaS.

**Aby dodać pakiet życia SilkRoad z galerii, wykonaj następujące czynności:**

1. W **klasycznego portalu Azure**, w lewym okienku nawigacji, kliknij polecenie **usługi Active Directory**. 
   
    ![Usługa Active Directory][1]

2. Z **katalogu** listy, wybierz katalog, dla którego chcesz włączyć integracji katalogów.

3. Aby otworzyć widok aplikacji, w widoku katalogu, kliknij przycisk **aplikacji** w menu u góry.
   
    ![Aplikacje][2]

4. Kliknij przycisk **Dodaj** w dolnej części strony.
   
    ![Aplikacje][3]

5. Na **co chcesz zrobić** okna dialogowego, kliknij przycisk **dodać aplikację z galerii**.
   
    ![Aplikacje][4]

6. W polu wyszukiwania wpisz **Suite życia SilkRoad**.
   
    ![Aplikacje][5]

7. W okienku wyników wybierz **Suite życia SilkRoad**, a następnie kliknij przycisk **Complete** można dodać aplikację.
   
    ![Aplikacje][50]

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfiguracja i testowanie usługi Azure AD rejestracji jednokrotnej
Jest celem tej sekcji opisano, jak skonfigurować i przetestować Azure AD SSO z pakietem życia SilkRoad w oparciu o nazwie "Britta Simona" użytkownika testowego.

Dla logowania jednokrotnego do pracy usługi Azure AD musi wiedzieć, co to jest odpowiednikiem użytkownikowi pakietu życia SilkRoad użytkownika w usłudze Azure AD. Innymi słowy relację łącza między użytkownika usługi Azure AD i danemu użytkownikowi pakietu życia SilkRoad musi określone.

Ta relacja łącza zostanie nawiązane, przypisując wartość **nazwy użytkownika** w usłudze Azure AD jako wartość **Username** SilkRoad życia pakietu.

Aby skonfigurować i przetestować usługi Azure AD rejestracji jednokrotnej z pakietem życia SilkRoad, należy wykonać poniższe bloki konstrukcyjne:

1. **[Konfigurowanie usługi Azure AD rejestracji jednokrotnej](#configuring-azure-ad-single-single-sign-on)**  — aby umożliwić użytkownikom korzystać z tej funkcji.
2. **[Tworzenie użytkownika testowego usługi Azure AD](#creating-an-azure-ad-test-user)**  — do przetestowania usługi Azure AD rejestracji jednokrotnej z Simona Britta.
3. **[Tworzenie użytkownika testowego zestawu życia SilkRoad](#creating-a-silkroad-life-suite-test-user)**  — w celu zapewnienia odpowiednikiem Simona Britta SilkRoad życia pakiet, który jest połączony z jej reprezentacji usługi Azure AD.
4. **[Przypisanie użytkownika testowego usługi Azure AD](#assigning-the-azure-ad-test-user)**  — aby umożliwić Simona Britta do użycia usługi Azure AD rejestracji jednokrotnej.
5. **[Testowanie rejestracji jednokrotnej](#testing-single-sign-on)**  — Aby sprawdzić, czy konfiguracja działa.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurowanie usługi Azure AD rejestracji jednokrotnej
Celem tej sekcji jest do włączenia funkcji logowania jednokrotnego usługi Azure AD w klasycznym portalu Azure i konfigurowania rejestracji Jednokrotnej w SilkRoad życia pakietu aplikacji.

**Aby skonfigurować usługi Azure AD rejestracji jednokrotnej z pakietem życia SilkRoad, wykonaj następujące czynności:**

1. Logowanie do witryny firmy SilkRoad jako administrator. 

  >[!NOTE] 
  > Aby uzyskać dostęp do aplikacji uwierzytelniania Suite życia SilkRoad Konfigurowanie Federacji przy użyciu usługi Microsoft Azure AD, skontaktuj się z SilkRoad pomocy technicznej lub Twoim przedstawicielem SilkRoad usług.
  > 

2. Przejdź do **usługodawcy**, a następnie kliknij przycisk **szczegóły federacyjnego**. 
   
    ![Azure AD rejestracji jednokrotnej][10] 

3. Kliknij przycisk **pobierania metadanych Federacji**, a następnie zapisz plik metadanych na tym komputerze.
   
    ![Azure AD rejestracji jednokrotnej][11] 

4. W klasycznym portalu Azure na **Suite życia SilkRoad** strona integracji aplikacji, kliknij przycisk **skonfigurować logowanie jednokrotne** otworzyć **skonfigurować logowanie jednokrotne** okna dialogowego.
   
    ![Konfigurowanie rejestracji jednokrotnej][6] 

5. Na **jak chcesz użytkownikom zalogować się do zestawu życia SilkRoad** wybierz pozycję **Azure AD rejestracji jednokrotnej**, a następnie kliknij przycisk **dalej**.
   
    ![Azure AD rejestracji jednokrotnej][7] 

6. Na **Konfigurowanie ustawień aplikacji** okna dialogowego strony, należy wykonać następujące czynności:
   
    ![Azure AD rejestracji jednokrotnej][8]   
 1. W **na adres URL logowania** tekstowym, wpisz adres URL używany przez użytkowników do logowania do witryny SilkRoad życia pakietu (np.: *https://defcompanytest-test-redcarpet.silkroad-eng.com/Authentication/*).  
 2. Otwórz pobrany **Silkroad** pliku metadanych. 
 3. Zlokalizuj **AssertionConsumerService** tag, a następnie skopiuj **lokalizacji** atrybutu.         
   
    ![Azure AD rejestracji jednokrotnej][21] 
 4. Wklej wartości do **adres URL odpowiedzi** pola tekstowego.  
 5. Kliknij przycisk **Dalej**.

6. Na **skonfigurować logowanie jednokrotne w SilkRoad życia Suite** wykonaj następujące czynności:
   
    ![Azure AD rejestracji jednokrotnej][9]  
 1. Kliknij przycisk Pobierz certyfikat, a następnie zapisz plik na komputerze.  
 2. Kliknij przycisk **Dalej**.

7. W Twojej **SilkRoad** aplikacji, kliknij przycisk **źródeł uwierzytelniania**.
   
    ![Azure AD rejestracji jednokrotnej][12] 

8. Kliknij przycisk **Dodawanie uwierzytelniania źródła**. 
   
    ![Azure AD rejestracji jednokrotnej][13] 

9. W **Dodaj źródło uwierzytelniania** sekcji, wykonaj następujące czynności: 
   
    ![Azure AD rejestracji jednokrotnej][14]  
 1. W obszarze **opcja 2 — plik metadanych**, kliknij przycisk **Przeglądaj** można przekazać pliku pobranego metadanych.  
 2. Kliknij przycisk **dostawcy tożsamości utworzyć przy użyciu danych pliku**.

10. W **źródeł uwierzytelniania** kliknij **Edytuj**. 
    
     ![Azure AD rejestracji jednokrotnej][15] 

11. Na **Edytuj źródło uwierzytelniania** okna dialogowego, wykonaj następujące czynności: 
    
     ![Azure AD rejestracji jednokrotnej][16] 
 1. Jako **włączone**, wybierz pozycję **tak**.   
 2. W **opis IdP** tekstowym, wpisz opis dla danej konfiguracji (np.: *logowania jednokrotnego programu Azure AD*).  
 3. W **nazwa IdP** tekstowym, wpisz nazwę, która jest specyficzna dla konfiguracji (np.: *Azure SP*).  
 4. Kliknij pozycję **Zapisz**.

12. Wyłącz wszystkie źródła uwierzytelniania. 
    
     ![Azure AD rejestracji jednokrotnej][17]

13. W klasycznym portalu Azure na **pojedynczy znak na potwierdzenie** kliknij przycisk **dalej**.  
    
     ![Azure AD rejestracji jednokrotnej][18]

14. Na **pojedynczy znak na potwierdzenie** kliknij przycisk **Complete**.
    
     ![Azure AD rejestracji jednokrotnej][19]

### <a name="create-an-azure-ad-test-user"></a>Tworzenie użytkownika testowego usługi Azure AD
Celem tej sekcji jest tworzenie użytkownika testowego w klasycznym portalu Azure o nazwie Simona Britta.

![Tworzenie użytkowników usługi Azure AD][20]

**Aby utworzyć użytkownika testowego w usłudze Azure AD, wykonaj następujące czynności:**

1. W **klasycznego portalu Azure**, w lewym okienku nawigacji, kliknij polecenie **usługi Active Directory**.
   
    ![Tworzenie użytkownika testowego usługi Azure AD](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_09.png)  

2. Z **katalogu** listy, wybierz katalog, dla którego chcesz włączyć integracji katalogów.

3. Aby wyświetlić listę użytkowników, w menu u góry, kliknij przycisk **użytkowników**.
   
    ![Tworzenie użytkownika testowego usługi Azure AD](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_03.png) 

4. Aby otworzyć **Dodaj użytkownika** okna dialogowego na pasku narzędzi u dołu, kliknij przycisk **Dodaj użytkownika**. 
   
    ![Tworzenie użytkownika testowego usługi Azure AD](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_04.png) 

5. Na **Poinformuj nas o tym użytkowniku** okna dialogowego strony, należy wykonać następujące czynności: 
   
    ![Tworzenie użytkownika testowego usługi Azure AD](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_05.png)  
 1. Jako typ użytkownika wybierz nowego użytkownika w organizacji.  
 2. W nazwie użytkownika **pole tekstowe**, typ **BrittaSimon**. 
 3. Kliknij przycisk **Dalej**.

6. Na **profilu użytkownika** okna dialogowego strony, należy wykonać następujące czynności: 
   
    ![Tworzenie użytkownika testowego usługi Azure AD](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_06.png)  
 1. W **imię** pole tekstowe, typ **Britta**.    
 2. W **nazwisko** pole tekstowe, typ **Simona**. 
 3. W **Nazwa wyświetlana** pole tekstowe, typ **Simona Britta**. 
 4. W **roli** listy, wybierz **użytkownika**.
 5. Kliknij przycisk **Dalej**.

7. Na **Uzyskaj hasło tymczasowe** strony okna dialogowego, kliknij przycisk **utworzyć**.
   
    ![Tworzenie użytkownika testowego usługi Azure AD](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_07.png) 

8. Na **Uzyskaj hasło tymczasowe** okna dialogowego strony, należy wykonać następujące czynności:
   
    ![Tworzenie użytkownika testowego usługi Azure AD](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_08.png)  
 1. Zanotuj wartość **nowe hasło**. 
 2. Kliknij przycisk **Complete** (Zakończ).   

### <a name="create-a-silkroad-life-suite-test-user"></a>Tworzenie użytkownika testowego SilkRoad życia pakietu
Celem tej sekcji jest utworzenie użytkownika o nazwie Simona Britta SilkRoad życia pakietu. Britta firmy musi mieć identyfikator logowania jednokrotnego (czasami określane jako *AuthParam*), które odpowiadają na Britta **emailaddress** w usłudze Azure AD.

**Aby utworzyć użytkownika o nazwie Simona Britta SilkRoad życia pakietu, wykonaj następujące czynności:**

- Skontaktuj się z zespołem pomocy technicznej SilkRoad życia pakietu można utworzyć użytkownika, takiej jak **identyfikator logowania jednokrotnego** atrybutu taką samą wartość jak **emailaddress** z Simona Britta w usłudze Azure AD.

### <a name="assign-the-azure-ad-test-user"></a>Przypisz użytkownika testowego usługi Azure AD
Celem tej sekcji jest umożliwienie Simona Britta się za pomocą logowania jednokrotnego Azure przez udostępnienie jej SilkRoad życia pakietu.

![Przypisz użytkownika][200] 

**Aby przypisać Simona Britta SilkRoad życia pakietu, wykonaj następujące czynności:**

1. W klasycznym portalu Azure, aby otworzyć widok aplikacji, w widoku katalogu, kliknij polecenie **aplikacji** w menu u góry.
   
    ![Przypisz użytkownika][201] 

2. Na liście aplikacji zaznacz **Suite życia SilkRoad**.
   
    ![Przypisz użytkownika][202] 

3. W menu u góry kliknij **użytkowników**.
   
    ![Przypisz użytkownika][203] 

4. Na liście użytkowników wybierz **Simona Britta**.

5. Na pasku narzędzi u dołu, kliknij przycisk **przypisać**.
   
    ![Przypisz użytkownika][205]

### <a name="test-single-sign-on"></a>Test rejestracji jednokrotnej
Celem tej sekcji służy do testowania konfigurację usługi Azure AD z logowania jednokrotnego za pomocą panelu dostępu.  

Po kliknięciu kafelka SilkRoad życia pakietu w panelu dostępu użytkownik powinien pobrać automatycznie zalogowane SilkRoad życia pakietu aplikacji.

## <a name="additional-resources"></a>Dodatkowe zasoby
* [Lista samouczków dotyczących sposobów integracji aplikacji SaaS przy użyciu usługi Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co to jest dostęp do aplikacji i logowanie jednokrotne z usługą Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_01.png
[50]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_02.png

[6]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_05.png
[7]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_03.png
[8]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_04.png
[9]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_05.png
[10]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_06.png
[11]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_07.png
[12]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_08.png
[13]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_09.png
[14]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_10.png
[15]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_11.png
[16]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_12.png
[17]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_13.png
[18]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_06.png
[19]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_07.png


[20]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_100.png
[21]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_15.png


[200]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_14.png
[203]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_205.png





