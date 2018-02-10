---
title: "Samouczek: Integracji Azure Active Directory z usług sieci Web firmy Amazon (AWS) | Dokumentacja firmy Microsoft"
description: "Informacje o sposobie konfigurowania rejestracji jednokrotnej między usługą Azure Active Directory i usług sieci Web firmy Amazon (AWS)."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 7561c20b-2325-4d97-887f-693aa383c7be
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 1/16/2017
ms.author: jeedes
ms.openlocfilehash: 0ff14365323d66a101e5847d7959045c3f20dea2
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/09/2018
---
# <a name="tutorial-azure-active-directory-integration-with-amazon-web-services-aws"></a>Samouczek: Integracji Azure Active Directory z usług sieci Web firmy Amazon (AWS)

Z tego samouczka dowiesz się integrowanie Amazon Web Services (AWS) z usługi Azure Active Directory (Azure AD).

Integrowanie usługi Amazon Web Services (AWS) z usługą Azure AD zapewnia następujące korzyści:

- Można kontrolować w usłudze Azure AD, który ma dostęp do usług sieci Web firmy Amazon (AWS).
- Można umożliwić użytkownikom automatycznie pobrać zalogowany do usługi sieci Web firmy Amazon (AWS) przy użyciu ich kont usługi Azure AD.
- Możesz zarządzać kont w jednej centralnej lokalizacji--portalu Azure.

Aby uzyskać więcej informacji o integracji aplikacji SaaS w usłudze Azure AD, zobacz [co to jest dostęp do aplikacji i logowanie jednokrotne z usługą Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Wymagania wstępne

Aby skonfigurować integrację usługi Azure AD z Amazon Web Services (AWS), potrzebne są następujące elementy:

- Subskrypcję usługi Azure AD
- Amazon Web Services (AWS) logowanie jednokrotne włączone subskrypcji

> [!NOTE]
> Nie zaleca się używanie środowiska produkcyjnego do testowania czynności w tym samouczku.

Aby przetestować kroki opisane w tym samouczku, wykonaj te zalecenia:

- Nie należy używać środowiska produkcyjnego, jeśli jest to konieczne.
- Jeśli nie masz środowisko wersji próbnej usługi Azure AD, możesz [Pobierz bezpłatną wersję próbną miesięcznego](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Opis scenariusza
W tym samouczku można przetestować usługę Azure AD rejestracji jednokrotnej w środowisku testowym. Scenariusz opisany w tym samouczku składa się z dwóch głównych elementów:

1. Dodawanie Amazon Web Services (AWS) z galerii
2. Konfigurowanie i testowanie usługi Azure AD logowanie jednokrotne

## <a name="add-amazon-web-services-aws-from-the-gallery"></a>Dodaj Amazon Web Services (AWS) z galerii
Aby skonfigurować integrację z usługami sieci Web Amazon (AWS) do usługi Azure AD, należy dodać Amazon Web Services (AWS) z galerii do listy zarządzanych aplikacji SaaS.

**Aby dodać Amazon Web Services (AWS) z galerii, należy wykonać następujące czynności:**

1. W  **[portalu Azure](https://portal.azure.com)**, w lewym okienku nawigacji, wybierz **usługi Azure Active Directory** ikony. 

    ![Przycisk usługi Azure Active Directory][1]

2. Przejdź do **aplikacje dla przedsiębiorstw**. Następnie przejdź do **wszystkie aplikacje**.

    ![Blok aplikacje przedsiębiorstwa][2]
    
3. Aby dodać nową aplikację, zaznacz **nowej aplikacji** przycisk w górnej części okna dialogowego.

    ![Nowy przycisk aplikacji][3]

4. W polu wyszukiwania wpisz **Amazon Web Services (AWS)**. Wybierz **Amazon Web Services (AWS)** z panelu wyniki, a następnie wybierz **Dodaj** przycisk, aby dodać aplikację.

    ![Amazon Web Services (AWS) na liście wyników](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices(aws)_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfiguracja i testowanie usługi Azure AD rejestracji jednokrotnej

W tej sekcji możesz skonfigurować i przetestować usługi Azure AD rejestracji jednokrotnej z Amazon Web usługi (AWS) na podstawie użytkownika testowego, nazywany "Britta Simona".

Dla rejestracji jednokrotnej do pracy usługi Azure AD musi wiedzieć, będący odpowiednikiem użytkownika w portalu Amazon usług sieci Web (AWS) do użytkownika w usłudze Azure AD. Innymi słowy należy ustanowić powiązanie użytkownika usługi Azure AD i powiązanych użytkowników w portalu Amazon usług sieci Web (AWS).

Aby ustanowić łącze w Amazon Web Services (AWS), należy podać wartość **Username** tę samą wartość jak **nazwy użytkownika** w usłudze Azure AD. 

Aby skonfigurować i przetestować usługi Azure AD rejestracji jednokrotnej z Amazon Web Services (AWS), wykonaj poniższe bloki konstrukcyjne:

1. [Konfigurowanie usługi Azure AD rejestracji jednokrotnej](#configure-azure-ad-single-sign-on) umożliwienie użytkownikom korzystać z tej funkcji.
2. [Tworzenie użytkownika testowego usługi Azure AD](#create-an-azure-ad-test-user) do testowania usługi Azure AD rejestracji jednokrotnej z Simona Britta.
3. [Tworzenie użytkownika testowego Amazon Web Services (AWS)](#create-an-amazon-web-services-aws-test-user) mają odpowiednikiem Simona Britta w Amazon usług sieci Web (AWS) połączonej z usługi Azure AD reprezentację użytkownika.
4. [Przypisz użytkownika testowego usługi Azure AD](#assign-the-azure-ad-test-user) umożliwiające Simona Britta do użycia usługi Azure AD rejestracji jednokrotnej.
5. [Test rejestracji jednokrotnej](#test-single-sign-on) Aby sprawdzić, czy konfiguracja działa.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurowanie usługi Azure AD rejestracji jednokrotnej

W tej sekcji można włączyć usługi Azure AD rejestracji jednokrotnej w portalu Azure i skonfigurować logowanie jednokrotne w aplikacji usługi sieci Web firmy Amazon (AWS).

**Aby skonfigurować usługi Azure AD rejestracji jednokrotnej z Amazon Web Services (AWS), należy wykonać następujące czynności:**

1. W portalu Azure na **Amazon Web Services (AWS)** strona integracji aplikacji, wybierz opcję **logowanie jednokrotne**.

    ![Skonfigurować łącze rejestracji jednokrotnej][4]

2. Aby włączyć logowanie jednokrotne, w **logowanie jednokrotne** okna dialogowego, **tryb** listy rozwijanej wybierz **na języku SAML logowania jednokrotnego**.
 
    ![Okno dialogowe rejestracji jednokrotnej](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices(aws)_samlbase.png)

3. W **Amazon Web Services (AWS) domeny i adres URL** sekcji, użytkownik nie ma żadnych czynności, ponieważ aplikacja już jest wstępna Integracja z usługą Azure.

    ![Domena Amazon Web Services (AWS) i adresy URL pojedynczy informacje logowania jednokrotnego](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices(aws)_url.png)

4. Aplikacja oprogramowania Amazon Web Services (AWS) oczekuje potwierdzenia języka SAML w określonym formacie. Skonfiguruj następujące oświadczeń dla tej aplikacji. Możesz zarządzać wartości tych atrybutów z **atrybuty użytkownika** na stronie integracji aplikacji. Poniższy zrzut ekranu przedstawia przykład:

    ![Konfigurowanie atrybutów rejestracji jednokrotnej](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices(aws)_attribute.png)  

5. W **atrybuty użytkownika** sekcji **logowanie jednokrotne** skonfiguruj atrybut tokenu SAML, jak pokazano na poprzedniej ilustracji i wykonaj następujące czynności:
    
    | Nazwa atrybutu  | Wartość atrybutu | Przestrzeń nazw |
    | --------------- | --------------- | --------------- |
    | RoleSessionName | user.userprincipalname | https://aws.amazon.com/SAML/Attributes |
    | Rola            | User.assignedroles |  https://aws.amazon.com/SAML/Attributes |
    
    >[!TIP]
    >Konfigurowanie udostępniania użytkownika w usłudze Azure AD można pobrać wszystkich ról z poziomu konsoli usługi Amazon Web Services (AWS). Zapoznaj się z następujących kroków inicjowania obsługi administracyjnej.

    a. Aby otworzyć **Dodawanie atrybutu** okno dialogowe, wybierz opcję **Dodaj atrybut**.

    ![Konfigurowanie atrybutów rejestracji jednokrotnej](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_attribute_04.png)

    ![Konfigurowanie atrybutów rejestracji jednokrotnej](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_attribute_05.png)

    b. W **nazwa** wpisz nazwę atrybutu, który jest wyświetlany dla danego wiersza.

    c. Z **wartość** listy, wpisz wartość atrybutu, który jest wyświetlany dla danego wiersza.

    d. W **Namespace** wpisz wartość przestrzeni nazw, który jest wyświetlany dla danego wiersza.
    
    d. Wybierz **Ok**.

6. W **certyfikat podpisywania SAML** zaznacz **XML metadanych**. Następnie zapisz plik metadanych na tym komputerze.

    ![Łącze pobierania certyfikatu](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices(aws)_certificate.png) 

7. Wybierz pozycję **Zapisz**.

    ![Konfigurowanie rejestracji jednokrotnej przycisk zapisywania](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_400.png)

8. W oknie innej przeglądarki należy zalogować się jako administrator do witryny firmy Amazon Web Services (AWS).

9. Wybierz **konsoli głównej**.
   
    ![Skonfiguruj główną rejestracji jednokrotnej][11]

10. Wybierz **Zarządzanie tożsamościami i dostępem**. 
   
    ![Konfigurowanie tożsamości rejestracji jednokrotnej][12]

11. Wybierz **dostawców tożsamości**. Następnie wybierz **Tworzenie dostawcy**. 
   
    ![Konfigurowanie dostawcy rejestracji jednokrotnej][13]

12. W **Konfigurowanie dostawcy** okna dialogowego należy wykonać następujące czynności: 
   
    ![Skonfiguruj jeden znak n okna dialogowego][14]
 
    a. Aby uzyskać **typ dostawcy**, wybierz pozycję **SAML**.

    b. W **Nazwa dostawcy** wpisz nazwę dostawcy (na przykład: *drewna*).

    c. Aby przekazać z pobranego **pliku metadanych** z portalu Azure wybierz **wybierz plik**.

    d. Wybierz **następny krok**.

13. W **Sprawdź informacje o dostawcy** okno dialogowe, wybierz opcję **Utwórz**. 
    
    ![Skonfiguruj weryfikacji rejestracji jednokrotnej][15]

14. Wybierz **ról**. Następnie wybierz **Utwórz nową rolę**. 
    
    ![Konfigurowanie ról rejestracji jednokrotnej][16]

15. W **Ustaw nazwę roli** okna dialogowego należy wykonać następujące czynności: 
    
    ![Konfigurowanie jednej nazwy logowania jednokrotnego][17] 

    a. W **nazwy roli** wpisz nazwę roli (na przykład *TestUser*). 

    b. Wybierz **następny krok**.

16. W **wybierz typ roli** okna dialogowego należy wykonać następujące czynności: 
    
    ![Skonfiguruj typ roli rejestracji jednokrotnej][18] 

    a. Wybierz **roli dostęp dostawcy tożsamości**. 

    b. W **Grant sieci Web rejestracji jednokrotnej (WebSSO) dostępu do dostawcy SAML** kliknij **wybierz**.

17. W **ustanowić zaufanie** okna dialogowego należy wykonać następujące czynności:  
    
    ![Skonfigurować zaufanie rejestracji jednokrotnej][19] 

    a. Wybierz dostawcę SAML wcześniej utworzony (na przykład: *drewna*). 
  
    b. Wybierz **następny krok**.

18. W **Sprawdź zaufania roli** okno dialogowe, wybierz opcję **następnego kroku**. 
    
    ![Skonfiguruj relację zaufania roli rejestracji jednokrotnej][32]

19. W **Dołącz zasady** okno dialogowe, wybierz opcję **następnego kroku**.  
    
    ![Konfigurowanie zasad rejestracji jednokrotnej][33]

20. W **przeglądu** okna dialogowego należy wykonać następujące czynności:   
    
    ![Skonfiguruj przeglądu rejestracji jednokrotnej][34] 

    a. Wybierz **utworzyć rolę**.

    b. Utwórz tyle ról w razie potrzeby, a następnie mapować je do dostawcy tożsamości.

21. Użyj poświadczeń konta usługi Amazon Web Services (AWS) używany do pobierania ról z konta Amazon Web Services (AWS) w Inicjowanie obsługi użytkowników usługi Azure AD. Aby uruchomić to zadanie, otwórz konsolę usługi Amazon Web Services (AWS) macierzystego.

22. Wybierz **usług** > **zabezpieczeń, tożsamości i zgodności** > **IAM**.

    ![Pobieranie role z konta Amazon Web Services (AWS)](./media/active-directory-saas-amazon-web-service-tutorial/fetchingrole1.png)

23. W sekcji IAM wybierz **zasady** kartę.

    ![Pobieranie role z konta Amazon Web Services (AWS)](./media/active-directory-saas-amazon-web-service-tutorial/fetchingrole2.png)

24. Aby utworzyć nowe zasady, wybierz **Tworzenie zasad**.

    ![Tworzenie nowych zasad](./media/active-directory-saas-amazon-web-service-tutorial/fetchingrole3.png)
 
25. Aby utworzyć własne zasady, aby pobrać wszystkie role z konta Amazon Web Services (AWS), wykonaj następujące czynności:

    ![Tworzenie nowych zasad](./media/active-directory-saas-amazon-web-service-tutorial/policy1.png)

    a. W **Tworzenie zasad** zaznacz **JSON** kartę.

    b. W dokumencie zasady Dodaj następujący kod JSON:
    
    ```
    
    {

    "Version": "2012-10-17",

    "Statement": [

    {

    "Effect": "Allow",
        
    "Action": [
        
    "iam: ListRoles"
        
    ],

    "Resource": "*"

    }

    ]

    }
    
    ```

    c. Aby sprawdzić zasady, wybierz **przycisk przeglądu zasad**.

    ![Definiowanie nowych zasad](./media/active-directory-saas-amazon-web-service-tutorial/policy5.png)

26. Zdefiniuj **nowe zasady** , wykonując następujące czynności:

    ![Definiowanie nowych zasad](./media/active-directory-saas-amazon-web-service-tutorial/policy2.png)

    a. Podaj **Nazwa zasady** jako **AzureAD_SSOUserRole_Policy**.

    b. Możesz podać następujące **opis** zasad: **ta zasada umożliwia pobranie role z kont usług AWS**.
    
    c. Wybierz **Utwórz zasady** przycisku.
        
27. Aby utworzyć nowe konto użytkownika w usłudze IAM Amazon Web Services (AWS), należy wykonać następujące czynności:

    a. Wybierz **użytkowników** w konsoli IAM Amazon Web Services (AWS).

    ![Definiowanie nowych zasad](./media/active-directory-saas-amazon-web-service-tutorial/policy3.png)
    
    b.To utworzenie nowego użytkownika, wybierz opcję **Dodaj użytkownika** przycisku.

    ![Dodawanie użytkownika](./media/active-directory-saas-amazon-web-service-tutorial/policy4.png)

    c. W **Dodaj użytkownika** sekcji, wykonaj następujące czynności:
    
    ![Dodawanie użytkownika](./media/active-directory-saas-amazon-web-service-tutorial/adduser1.png)
    
    * Wprowadź **AzureADRoleManager** w polu Nazwa użytkownika.
    
    * Wybierz typ dostępu **dostęp programistyczny** opcji. Dzięki temu użytkownik możesz wywoływać interfejsy API i pobrać role z konta Amazon Web Services (AWS).
    
    * Wybierz **dalej uprawnienia** przycisk w prawym dolnym rogu.

28. Utwórz nowe zasady dla tego użytkownika, wykonując następujące czynności:

    ![Dodawanie użytkownika](./media/active-directory-saas-amazon-web-service-tutorial/adduser2.png)
    
    a. Wybierz **dołączyć istniejących zasad bezpośrednio** przycisku.

    b. Wyszukaj zasady nowo utworzony w sekcji Filtr **AzureAD_SSOUserRole_Policy**.
    
    c. Wybierz **zasad**. Następnie wybierz **dalej: Przejrzyj** przycisku.

29. Przejrzyj zasady dla użytkownika, wykonując następujące czynności:

    ![Dodawanie użytkownika](./media/active-directory-saas-amazon-web-service-tutorial/adduser3.png)
    
    a. Sprawdź nazwę użytkownika, typ dostępu i zasad, które są mapowane do użytkownika.
    
    b. Aby utworzyć użytkownika, wybierz **tworzenia użytkownika** przycisk w prawym dolnym rogu, aby utworzyć użytkownika.

30. Pobieranie poświadczeń użytkownika, wykonując następujące czynności:

    ![Dodawanie użytkownika](./media/active-directory-saas-amazon-web-service-tutorial/adduser4.png)
    
    a. Skopiuj użytkownika **dostępu Identyfikatora klucza** i **klucza tajnego dostępu**.
    
    b. Wprowadź te poświadczenia do użytkownika usługi Azure AD inicjowania obsługi sekcji można pobrać ról z poziomu konsoli usługi Amazon Web Services (AWS).
    
    c. Wybierz **Zamknij** przycisk w prawym dolnym rogu.

31. Przejdź do **Inicjowanie obsługi użytkowników** części aplikacji Amazon Web Services (AWS) w portalu zarządzania usługi Azure AD.

    ![Dodawanie użytkownika](./media/active-directory-saas-amazon-web-service-tutorial/provisioning.png)

32. Wprowadź **klucz dostępu** i **klucz tajny** w **klucz tajny klienta** i **klucz tajny tokenu** odpowiednio do pola.

    ![Dodawanie użytkownika](./media/active-directory-saas-amazon-web-service-tutorial/provisioning1.png)
    
    a. Wprowadź klucz dostępu użytkownika Amazon Web Services (AWS) w **clientsecret** pola.
    
    b. Wprowadź hasło użytkownika usługi Amazon Web Services (AWS) w **klucz tajny tokenu** pola.
    
    c. Wybierz **Testuj połączenie** przycisku. Należy mógł pomyślnie przetestować tego połączenia.

    d. Zapisz ustawienia, wybierając **zapisać** na górze.
 
33. Upewnij się, że włączenie stan inicjowania obsługi administracyjnej **na** w **ustawienia**. Aby to zrobić, wybierając **na**, a następnie wybierając **zapisać** na górze.

    ![Dodawanie użytkownika](./media/active-directory-saas-amazon-web-service-tutorial/provisioning2.png)

> [!TIP]
> Teraz możesz przeczytać zwięzły wersji tych instrukcji wewnątrz [portalu Azure](https://portal.azure.com) podczas konfigurowania aplikacji. Po dodaniu tej aplikacji z **usługi Active Directory > aplikacje dla przedsiębiorstw** zaznacz **rejestracji jednokrotnej** kartę. Dostęp do dokumentacji osadzonych za pomocą **konfiguracji** sekcji u dołu. Możesz przeczytać dodatkowe informacje o funkcji dokumentacji osadzony w [usługi Azure AD osadzonych dokumentacji]( https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="create-an-azure-ad-test-user"></a>Tworzenie użytkownika testowego usługi Azure AD

Celem tej sekcji jest tworzenie użytkownika testowego w portalu Azure o nazwie Simona Britta.

   ![Tworzenie użytkownika testowego usługi Azure AD][100]

**Aby utworzyć użytkownika testowego w usłudze Azure AD, należy wykonać następujące czynności:**

1. W portalu Azure, w lewym okienku wybierz **usługi Azure Active Directory** przycisku.

    ![Przycisk usługi Azure Active Directory](./media/active-directory-saas-amazon-web-service-tutorial/create_aaduser_01.png)

2. Aby wyświetlić listę użytkowników, przejdź do **użytkowników i grup**. Następnie wybierz **wszyscy użytkownicy**.

    !["Użytkownicy i grupy" i "Wszyscy użytkownicy" łącza](./media/active-directory-saas-amazon-web-service-tutorial/create_aaduser_02.png)

3. Aby otworzyć **użytkownika** okno dialogowe, wybierz opcję **Dodaj** w górnej części **wszyscy użytkownicy** okno dialogowe.

    ![Przycisk Dodaj](./media/active-directory-saas-amazon-web-service-tutorial/create_aaduser_03.png)

4. W **użytkownika** okna dialogowego należy wykonać następujące czynności:

    ![Okno dialogowe użytkownika](./media/active-directory-saas-amazon-web-service-tutorial/create_aaduser_04.png)

    a. W **nazwa** wpisz **BrittaSimon**.

    b. W **nazwy użytkownika** wpisz adres e-mail użytkownika Simona Britta.

    c. Wybierz **Pokaż hasło** pole wyboru, a następnie zanotuj wartość, która jest wyświetlana w **hasło** pole.

    d. Wybierz pozycję **Utwórz**.
 
### <a name="create-an-amazon-web-services-aws-test-user"></a>Tworzenie użytkownika testowego Amazon Web Services (AWS)

Celem tej sekcji jest utworzenie użytkownika o nazwie Simona Britta w Amazon Web Services (AWS). Amazon Web Services (AWS) nie musi użytkownika mogą być tworzone w swoim systemie single-sign-on, więc nie trzeba wykonywać żadnych czynności w tym miejscu.

### <a name="assign-the-azure-ad-test-user"></a>Przypisz użytkownika testowego usługi Azure AD

W tej sekcji można włączyć Simona Britta do przyznania im dostępu do usług sieci Web firmy Amazon (AWS) za pomocą usługi Azure rejestracji jednokrotnej.

![Przypisanie roli użytkownika][200] 

**Aby przypisać Simona Britta do Amazon Web Services (AWS), należy wykonać następujące czynności:**

1. W portalu Azure Otwórz widok aplikacji. Następnie przejdź do widoku katalogu i wybierz **aplikacje dla przedsiębiorstw**. Następnie wybierz pozycję **wszystkie aplikacje**.

    ![Przypisz użytkownika][201] 

2. Na liście aplikacji zaznacz **usług Amazon Web Services (usługi Amazon Web Services (AWS)**.

    ![Łącze Amazon Web Services (AWS) na liście aplikacji](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices(aws)_app.png)  

3. W menu po lewej stronie wybierz **użytkowników i grup**.

    ![Łącze "Użytkownicy i grupy"][202]

4. Wybierz **Dodaj** przycisku. Następnie w **Dodaj przydziału** okno dialogowe, wybierz opcję **użytkowników i grup**.

    ![W okienku Dodaj przydziału][203]

5. W **użytkowników i grup** okno dialogowe, wybierz opcję **Simona Britta** na liście Użytkownicy.

6. W **użytkowników i grup** okno dialogowe, kliknij przycisk **wybierz** przycisku. 

7. W **Dodaj przydziału** okno dialogowe, wybierz opcję **przypisać** przycisku.
    
### <a name="test-single-sign-on"></a>Test rejestracji jednokrotnej

W tej sekcji można przetestować konfiguracji usługi Azure AD pojedynczego logowania za pomocą panelu dostępu.

Po wybraniu kafelka Amazon Web Services (AWS) w panelu dostępu należy należy pobrać automatycznie zalogowany do aplikacji usługi sieci Web firmy Amazon (AWS). Aby uzyskać więcej informacji na temat panelu dostępu, zobacz [wprowadzenie do panelu dostępu](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Zasoby dodatkowe

* [Lista samouczków dotyczących sposobów integracji aplikacji SaaS w usłudze Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co to jest dostęp do aplikacji i logowanie jednokrotne z usługą Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_203.png
[11]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795031.png
[12]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795032.png
[13]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795033.png
[14]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795034.png
[15]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795035.png
[16]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795022.png
[17]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795023.png
[18]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795024.png
[19]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795025.png
[32]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950251.png
[33]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950252.png
[35]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning.png
[34]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950253.png
[36]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_securitycredentials.png
[37]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_securitycredentials_continue.png
[38]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_createnewaccesskey.png
[39]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning_automatic.png
[40]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning_testconnection.png
[41]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning_on.png

