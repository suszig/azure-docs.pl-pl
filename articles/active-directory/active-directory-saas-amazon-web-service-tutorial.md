---
title: "Samouczek: Integracji Azure Active Directory z usług sieci Web firmy Amazon (AWS) | Dokumentacja firmy Microsoft"
description: "Informacje o sposobie konfigurowania rejestracji jednokrotnej między usługą Azure Active Directory i usług sieci Web firmy Amazon (AWS)."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 7561c20b-2325-4d97-887f-693aa383c7be
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.reviewer: jeedes
ms.openlocfilehash: 0fb9c8f428368271b548e3f174726fa01ea910c5
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-amazon-web-services-aws"></a>Samouczek: Integracji Azure Active Directory z usług sieci Web firmy Amazon (AWS)

Z tego samouczka dowiesz się integrowanie Amazon Web Services (AWS) z usługi Azure Active Directory (Azure AD).

Integrowanie usługi Amazon Web Services (AWS) z usługą Azure AD zapewnia następujące korzyści:

- Można kontrolować w usłudze Azure AD, który ma dostęp do usług sieci Web firmy Amazon (AWS)
- Umożliwia użytkownikom automatycznie pobrać zalogowane do Amazon Web Services (AWS) (logowanie jednokrotne) z konta usługi Azure AD
- Możesz zarządzać kont w jednej centralnej lokalizacji - portalu Azure

Jeśli chcesz dowiedzieć się więcej informacji o integracji aplikacji SaaS w usłudze Azure AD, zobacz [co to jest dostęp do aplikacji i logowanie jednokrotne z usługą Azure Active Directory](active-directory-appssoaccess-whatis.md).

<!--## Overview

To enable single sign-on with Amazon Web Services (AWS), it must be configured to use Azure Active Directory as an identity provider. This guide provides information and tips on how to perform this configuration in Amazon Web Services (AWS).

>[!Note]: 
>This embedded guide is brand new in the new Azure portal, and we’d love to hear your thoughts. Use the Feedback ? button at the top of the portal to provide feedback. The older guide for using the [Azure classic portal](https://manage.windowsazure.com) to configure this application can be found [here](https://github.com/Azure/AzureAD-App-Docs/blob/master/articles/en-us/_/sso_configure.md).-->


## <a name="prerequisites"></a>Wymagania wstępne

Aby skonfigurować integrację usługi Azure AD z Amazon Web Services (AWS), potrzebne są następujące elementy:

- Subskrypcję usługi Azure AD
- Amazon Web Services (AWS) jednokrotnego włączone subskrypcji

> [!NOTE]
> Aby przetestować kroki opisane w tym samouczku, zaleca się używania środowiska produkcyjnego.

Aby przetestować kroki opisane w tym samouczku, należy wykonać te zalecenia:

- Nie należy używać środowiska produkcyjnego, chyba że jest to konieczne.
- Jeśli nie masz środowisko wersji próbnej usługi Azure AD, możesz pobrać miesięczna wersja próbna [tutaj](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Opis scenariusza
W tym samouczku można przetestować usługę Azure AD rejestracji jednokrotnej w środowisku testowym. Scenariusz opisany w tym samouczku składa się z dwóch głównych elementów:

1. Dodawanie Amazon Web Services (AWS) z galerii
2. Konfigurowanie i testowanie usługi Azure AD logowanie jednokrotne

## <a name="adding-amazon-web-services-aws-from-the-gallery"></a>Dodawanie Amazon Web Services (AWS) z galerii
Aby skonfigurować integrację z usługami sieci Web Amazon (AWS) do usługi Azure AD, należy dodać Amazon Web Services (AWS) z galerii do listy zarządzanych aplikacji SaaS.

**Aby dodać Amazon Web Services (AWS) z galerii, wykonaj następujące czynności:**

1. W  **[Azure Portal](https://portal.azure.com)**, na panelu nawigacyjnym po lewej stronie kliknij **usługi Azure Active Directory** ikony. 

    ![Usługa Active Directory][1]

2. Przejdź do **aplikacje dla przedsiębiorstw**. Następnie przejdź do **wszystkie aplikacje**.

    ![Aplikacje][2]
    
3. Kliknij przycisk **Dodaj** przycisk w górnej części okna dialogowego.

    ![Aplikacje][3]

4. W polu wyszukiwania wpisz **Amazon Web Services (AWS)**.

    ![Tworzenie użytkownika testowego usługi Azure AD](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_search.png)

5. W panelu wyników wybierz **Amazon Web Services (AWS)**, a następnie kliknij przycisk **Dodaj** przycisk, aby dodać aplikację.

    ![Tworzenie użytkownika testowego usługi Azure AD](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurowanie i testowanie usługi Azure AD logowanie jednokrotne
W tej sekcji skonfigurować i przetestować usługi Azure AD rejestracji jednokrotnej z Amazon Web usługi (AWS) w oparciu o nazwie "Britta Simona" użytkownika testowego.

Dla rejestracji jednokrotnej do pracy usługi Azure AD musi wiedzieć, użytkownik odpowiednika w Amazon Web Services (AWS) jest dla użytkownika, w usłudze Azure AD. Innymi słowy musi można ustanowić łącze relację między użytkownikiem usługi Azure AD i danemu użytkownikowi w Amazon Web Services (AWS).

Ta relacja łącza zostanie nawiązane, przypisując wartość **nazwy użytkownika** w usłudze Azure AD jako wartość **Username** w Amazon Web Services (AWS).

Aby skonfigurować i przetestować usługi Azure AD rejestracji jednokrotnej z Amazon Web Services (AWS), należy wykonać poniższe bloki konstrukcyjne:

1. **[Konfigurowanie usługi Azure AD rejestracji jednokrotnej](#configuring-azure-ad-single-sign-on)**  — aby umożliwić użytkownikom korzystać z tej funkcji.
2. **[Tworzenie użytkownika testowego usługi Azure AD](#creating-an-azure-ad-test-user)**  — do przetestowania usługi Azure AD rejestracji jednokrotnej z Simona Britta.
3. **[Tworzenie użytkownika testowego usług Amazon Web Services](#creating-an-amazon-web-services-test-user)**  — aby mieć odpowiednikiem Simona Britta w Amazon usług sieci Web (AWS) połączonej z jej reprezentacji usługi Azure AD.
4. **[Przypisanie użytkownika testowego usługi Azure AD](#assigning-the-azure-ad-test-user)**  — aby umożliwić Simona Britta do użycia usługi Azure AD rejestracji jednokrotnej.
5. **[Testowanie rejestracji jednokrotnej](#testing-single-sign-on)**  — Aby sprawdzić, czy konfiguracja działa.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurowanie usługi Azure AD rejestracji jednokrotnej

W tej sekcji można włączyć usługi Azure AD rejestracji jednokrotnej w portalu Azure i skonfigurować logowanie jednokrotne w aplikacji usługi sieci Web firmy Amazon (AWS).

**Aby skonfigurować usługi Azure AD rejestracji jednokrotnej z Amazon Web Services (AWS), wykonaj następujące czynności:**

1. W portalu Azure na **Amazon Web Services (AWS)** strona integracji aplikacji, kliknij przycisk **logowanie jednokrotne**.

    ![Konfigurowanie rejestracji jednokrotnej][4]

2. Na **logowanie jednokrotne** okna dialogowego, jako **tryb** wybierz **na języku SAML logowania jednokrotnego** Aby włączyć logowanie jednokrotne.
 
    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_samlbase.png)

3. Na **Amazon Web Services (AWS) domeny i adres URL** sekcji, użytkownik nie trzeba wykonywać żadnych czynności, jak aplikacja już jest wstępna Integracja z usługą Azure.

    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_url.png)

4. Na **certyfikat podpisywania SAML** kliknij **XML metadanych** , a następnie zapisz plik XML na tym komputerze.
    
    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_certificate.png)

5. Aplikacji Amazon Web Services (AWS) oczekuje potwierdzenia języka SAML w określonym formacie. Skonfiguruj następujące oświadczeń dla tej aplikacji. Możesz zarządzać wartości tych atrybutów z "**atrybuty użytkownika**" sekcji na stronie integracji aplikacji. Poniższy zrzut ekranu przedstawia przykład tego.

    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_attribute.png)

6. W **atrybuty użytkownika** sekcji na **logowanie jednokrotne** okna dialogowego, skonfiguruj atrybut tokenu SAML, jak pokazano na ilustracji powyżej i wykonaj następujące czynności:
    
    | Nazwa atrybutu  | Wartość atrybutu | przestrzeń nazw |
    | --------------- | --------------- | --------------- |
    | RoleSessionName | User.userPrincipalName | https://awS.amazon.com/SAML/Attributes |
    | Rola            | User.assignedroles |  https://awS.amazon.com/SAML/Attributes |
    
    >[!TIP]
    >Należy skonfigurować Inicjowanie obsługi użytkowników w usłudze Azure AD można pobrać za pomocą konsoli usług AWS wszystkich ról. Przeczytaj poniższe kroki inicjowania obsługi administracyjnej.

    a. Kliknij przycisk **Dodaj atrybut** otworzyć **Dodawanie atrybutu** okna dialogowego.

    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_attribute_04.png)

    b. W **nazwa** tekstowym, wpisz nazwę atrybut wyświetlany dla danego wiersza.

    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_attribute_05.png)

    c. Z **wartość** listy, wpisz wartość atrybutu wyświetlany dla danego wiersza. Dodaj wartość Namespace podane powyżej.
    
    d. Kliknij przycisk **OK**.

7. Kliknij przycisk **zapisać** przycisk, aby zapisać ustawienia na platformie Azure.

    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_400.png)

8. W oknie innej przeglądarki logowania do witryny firmy Amazon Web Services (AWS) jako administrator.

9. Kliknij przycisk **konsoli głównej**.
   
    ![Konfigurowanie rejestracji jednokrotnej][11]

10. Kliknij przycisk **IAM** z **zabezpieczeń, tożsamości i zgodności** usługi.
   
    ![Konfigurowanie rejestracji jednokrotnej][12]

11. Kliknij przycisk **dostawców tożsamości**, a następnie kliknij przycisk **Tworzenie dostawcy**.
   
    ![Konfigurowanie rejestracji jednokrotnej][13]

12. Na **Konfigurowanie dostawcy** okna dialogowego strony, należy wykonać następujące czynności:
   
    ![Konfigurowanie rejestracji jednokrotnej][14]
 
    a. Jako **typ dostawcy**, wybierz pozycję **SAML**.

    b. W **Nazwa dostawcy** tekstowym, wpisz nazwę dostawcy (np.: *drewna*).

    c. Aby przekazać plik metadanych pobranych, kliknij przycisk **wybierz plik**.

    d. Kliknij przycisk **następny krok**.

13. Na **Sprawdź informacje o dostawcy** strony okna dialogowego, kliknij przycisk **Utwórz**. 
    
    ![Konfigurowanie rejestracji jednokrotnej][15]

14. Kliknij przycisk **ról**, a następnie kliknij przycisk **Utwórz nową rolę**. 
    
    ![Konfigurowanie rejestracji jednokrotnej][16]

15. Na **Ustaw nazwę roli** okna dialogowego, wykonaj następujące czynności: 
    
    ![Konfigurowanie rejestracji jednokrotnej][17] 

    a. W **nazwy roli** tekstowym, wpisz nazwę roli (np.: *TestUser*). 

    b. Kliknij przycisk **następny krok**.

16. Na **wybierz typ roli** okna dialogowego, wykonaj następujące czynności: 
    
    ![Konfigurowanie rejestracji jednokrotnej][18] 

    a. Wybierz **roli dostęp dostawcy tożsamości**. 

    b. W **Grant sieci Web rejestracji jednokrotnej (WebSSO) dostępu do dostawcy SAML** kliknij **wybierz**.

17. Na **ustanowić zaufanie** okna dialogowego, wykonaj następujące czynności:  
    
    ![Konfigurowanie rejestracji jednokrotnej][19] 

    a. Jako dostawca SAML, należy wybrać dostawcę SAML utworzonym wcześniej (np.: *drewna*)
  
    b. Kliknij przycisk **następny krok**.

18. Na **Sprawdź zaufania roli** okna dialogowego, kliknij przycisk **następnego kroku**.
    
    ![Konfigurowanie rejestracji jednokrotnej][32]

19. Na **Dołącz zasady** okna dialogowego, kliknij przycisk **następnego kroku**.
    
    ![Konfigurowanie rejestracji jednokrotnej][33]

20. Na **przeglądu** okna dialogowego, wykonaj następujące czynności:
    
    ![Konfigurowanie rejestracji jednokrotnej][34]
 
    a. Kliknij przycisk **utworzyć rolę**.

    b. Tworzenie ról tyle zgodnie z potrzebami i zamapowania ich na dostawcy tożsamości.

21. Teraz skonfigurować przypisywania do pobierania z usług AWS wszystkie role użytkowników

    a. W konsoli usług AWS logowanie przy użyciu konta głównego

    b. W prawym górnym rogu kliknij swoją nazwę, a następnie kliknij przycisk **moje poświadczenia zabezpieczeń** opcji. Spowoduje to otwarcie ekranu jako komunikat ostrzegawczy. Kliknij przycisk **poświadczenia zabezpieczeń** przycisk, aby przekazać ekranu.
        
       ![Konfigurowanie rejestracji jednokrotnej][36]

       ![Konfigurowanie rejestracji jednokrotnej][37]

    c. W sekcji klucze dostępu kliknij **Utwórz nowy klucz dostępu** przycisku. Spowoduje to wygenerowanie Identyfikatora klucza dostępu i wartość tokenu.
    
       ![Konfigurowanie rejestracji jednokrotnej][38]

    d. Skopiuj obie te wartości i go również pobrać, tak aby nie zostaną utracone.

    e. W portalu Azure na stronie integracji aplikacji Amazon Web Services (AWS) kliknij **inicjowania obsługi administracyjnej**.
        
       ![Konfigurowanie rejestracji jednokrotnej][35]

    f. Ustaw tryb inicjowania obsługi administracyjnej **automatyczne**
        
       ![Konfigurowanie rejestracji jednokrotnej][39]

    g. Teraz w **clientsecret** i **klucz tajny tokenu** Wklej odpowiednie wartości, które zostały skopiowane z konsoli usług AWS.
    
    h. Możesz kliknąć **Testuj połączenie** przycisk, aby przetestować połączenie. Gdy zostanie pomyślnie przesłany można uruchomić łącznik inicjowania obsługi administracyjnej.
       
       ![Konfigurowanie rejestracji jednokrotnej][40]

    i. Teraz włączyć stan inicjowania obsługi administracyjnej **na**. Spowoduje to uruchomienie pobierania ról z aplikacji.

       ![Konfigurowanie rejestracji jednokrotnej][41]

    > [!NOTE]
    > Azure działa Usługa AD inicjowania obsługi administracyjnej co po pewnym czasie, aby zsynchronizować role z usług AWS. Powinna zostać wyświetlona dostawcy tożsamości dołączony ról usług AWS z usługą Azure AD i można ich użyć podczas przypisywania aplikacji do użytkowników lub grup.

<!--### Next steps

To ensure users can sign-in to Amazon Web Services (AWS) after it has been configured to use Azure Active Directory, review the following tasks and topics:

- User accounts must be pre-provisioned into Amazon Web Services (AWS) prior to sign-in. To set this up, see Provisioning.
 
- Users must be assigned access to Amazon Web Services (AWS) in Azure AD to sign-in. To assign users, see Users.
 
- To configure access polices for Amazon Web Services (AWS) users, see Access Policies.
 
- For additional information on deploying single sign-on to users, see [this article](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#deploying-azure-ad-integrated-applications-to-users).-->


### <a name="creating-an-azure-ad-test-user"></a>Tworzenie użytkownika testowego usługi Azure AD
Celem tej sekcji jest tworzenie użytkownika testowego w portalu Azure o nazwie Simona Britta.

![Tworzenie użytkowników usługi Azure AD][100]

**Aby utworzyć użytkownika testowego w usłudze Azure AD, wykonaj następujące czynności:**

1. W **portalu Azure**, w lewym okienku nawigacji, kliknij polecenie **usługi Azure Active Directory** ikony.

    ![Tworzenie użytkownika testowego usługi Azure AD](./media/active-directory-saas-amazon-web-service-tutorial/create_aaduser_01.png) 

2. Przejdź do **użytkowników i grup** i kliknij przycisk **wszyscy użytkownicy** do wyświetlenia na liście Użytkownicy.
    
    ![Tworzenie użytkownika testowego usługi Azure AD](./media/active-directory-saas-amazon-web-service-tutorial/create_aaduser_02.png) 

3. W górnej części okna dialogowego kliknij **Dodaj** otworzyć **użytkownika** okna dialogowego.
 
    ![Tworzenie użytkownika testowego usługi Azure AD](./media/active-directory-saas-amazon-web-service-tutorial/create_aaduser_03.png) 

4. Na **użytkownika** okna dialogowego strony, należy wykonać następujące czynności:
 
    ![Tworzenie użytkownika testowego usługi Azure AD](./media/active-directory-saas-amazon-web-service-tutorial/create_aaduser_04.png) 

    a. W **nazwa** pole tekstowe, typ **BrittaSimon**.

    b. W **nazwy użytkownika** pole tekstowe, typ **adres e-mail** z BrittaSimon.

    c. Wybierz **Pokaż hasło** i zanotuj wartość **hasło**.

    d. Kliknij przycisk **Utwórz**.
 
### <a name="creating-an-amazon-web-services-test-user"></a>Tworzenie użytkownika testowego usług Amazon Web Services

Aby włączyć użytkowników usługi Azure AD zalogować się do usługi sieci Web firmy Amazon (AWS), są udostępniane do usług sieci Web firmy Amazon (AWS). W przypadku Amazon Web Services (usług AWS), inicjowanie obsługi to zadanie ręczne.

**Aby udostępnić konta użytkownika, wykonaj następujące czynności:**

1. Zaloguj się do Twojego **Amazon Web Services (AWS)** witryny firmy jako administrator.

2. Kliknij przycisk **konsoli głównej** ikony. 
   
    ![Konfigurowanie rejestracji jednokrotnej][11]

3. Tożsamość i zarządzanie dostępem. 
   
    ![Konfigurowanie rejestracji jednokrotnej][28]

4. Na pulpicie nawigacyjnym kliknij **użytkowników**, a następnie kliknij przycisk **Utwórz nowych użytkowników**. 
   
    ![Konfigurowanie rejestracji jednokrotnej][29]

5. W oknie dialogowym Tworzenie użytkownika wykonaj następujące czynności: 
   
    ![Konfigurowanie rejestracji jednokrotnej][30]   
    
    a. W **wprowadź nazwy użytkowników** pól tekstowych, wpisz nazwę użytkownika Simona Brita (userprincipalname) w usłudze Azure AD.

    b. Kliknij przycisk **utworzyć.**
        
### <a name="assigning-the-azure-ad-test-user"></a>Przypisanie użytkownika testowego usługi Azure AD

W tej sekcji można włączyć Simona Britta do użycia usługi Azure logowania jednokrotnego za udostępnienie jej do usługi sieci Web firmy Amazon (AWS).

![Przypisz użytkownika][200] 

**Aby przypisać Simona Britta do Amazon Web Services (AWS), wykonaj następujące czynności:**

1. W portalu Azure Otwórz widok aplikacji, a następnie przejdź do widoku katalogu i przejdź do **aplikacje dla przedsiębiorstw** kliknięcie **wszystkie aplikacje**.

    ![Przypisz użytkownika][201] 

2. Na liście aplikacji zaznacz **Amazon Web Services (AWS)**.

    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_app.png) 

3. W menu po lewej stronie kliknij **użytkowników i grup**.

    ![Przypisz użytkownika][202] 

4. Kliknij przycisk **Dodaj** przycisku. Następnie wybierz **użytkowników i grup** na **Dodaj przydziału** okna dialogowego.

    ![Przypisz użytkownika][203]

5. Na **użytkowników i grup** okno dialogowe, wybierz opcję **Simona Britta** na liście Użytkownicy.

6. Kliknij przycisk **wybierz** znajdującego się na **użytkowników i grup** okna dialogowego.

7. Na **wybierz rolę** , a następnie wybierz odpowiednią rolę dla użytkownika. Te role są wyświetlane z nazwą roli i nazwa dostawcy tożsamości. Dzięki temu można łatwo zidentyfikować role z usług AWS.

8. Kliknij przycisk **przypisać** znajdującego się na **Dodaj przydziału** okna dialogowego.
    
### <a name="testing-single-sign-on"></a>Testowanie rejestracji jednokrotnej

W tej sekcji można przetestować konfiguracji usługi Azure AD pojedynczego logowania za pomocą panelu dostępu.

Po kliknięciu kafelka Amazon Web Services (AWS) w panelu dostępu użytkownik powinien pobrać automatycznie zalogowane do aplikacji usługi sieci Web firmy Amazon (AWS). 

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Lista samouczków dotyczących sposobów integracji aplikacji SaaS przy użyciu usługi Azure Active Directory](active-directory-saas-tutorial-list.md)
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
[20]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950351.png
[21]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_80.png
[22]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950352.png
[23]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_81.png
[24]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950353.png
[25]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_15.png

[28]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950321.png
[29]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795037.png
[30]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795038.png
[32]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950251.png
[33]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950252.png
[34]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950253.png
[35]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning.png
[36]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_securitycredentials.png
[37]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_securitycredentials_continue.png
[38]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_createnewaccesskey.png
[39]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning_automatic.png
[40]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning_testconnection.png
[41]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning_on.png
