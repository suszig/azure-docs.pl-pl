---
title: "Samouczek: Integracji Azure Active Directory z usługi ServiceNow | Dokumentacja firmy Microsoft"
description: "Informacje o sposobie konfigurowania rejestracji jednokrotnej między usługą Azure Active Directory i usługi ServiceNow i usługi ServiceNow Express."
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: 1d8eb99e-8ce5-4ba4-8b54-5c3d9ae573f6
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: jeedes
ms.reviewer: jeedes
ms.openlocfilehash: a91fab90a94b655b93c8ae9064ea4836b80d7678
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-servicenow"></a>Samouczek: Integracji Azure Active Directory z usługi ServiceNow
Z tego samouczka dowiesz się Integrowanie usługi ServiceNow i Express usługi ServiceNow z usługą Azure Active Directory (Azure AD).

Integrowanie usługi ServiceNow i Express usługi ServiceNow z usługą Azure AD zapewnia następujące korzyści:

* Można kontrolować w usłudze Azure AD, kto ma dostęp do usługi ServiceNow oraz usługi ServiceNow Express
* Umożliwia użytkownikom automatycznie pobrać zalogowane usługi ServiceNow i usługi ServiceNow Express (logowanie jednokrotne) przy użyciu ich kont usługi Azure AD
* Możesz zarządzać kont w jednej centralnej lokalizacji - klasycznego portalu Azure

Jeśli chcesz dowiedzieć się więcej informacji o integracji aplikacji SaaS w usłudze Azure AD, zobacz [co to jest dostęp do aplikacji i logowanie jednokrotne z usługą Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Wymagania wstępne
Aby skonfigurować integrację usługi Azure AD z usługi ServiceNow i Express usługi ServiceNow, potrzebne są następujące elementy:

* Subskrypcję usługi Azure AD
* Dla usługi ServiceNow, wystąpienie lub dzierżawy usługi ServiceNow, wersja Calgary lub nowszej
* Usługi ServiceNow Express, wystąpienie usługi ServiceNow Express, wersja Helsinkach lub nowszej
* Dzierżawca usługi ServiceNow musi mieć [wielu pojedynczy znak na dodatek dostawcy](http://wiki.servicenow.com/index.php?title=Multiple_Provider_Single_Sign-On#gsc.tab=0) włączone. Można to zrobić [przesyłanie żądania obsługi](https://hi.service-now.com). 

> [!NOTE]
> Aby przetestować kroki opisane w tym samouczku, zaleca się używania środowiska produkcyjnego.
> 
> 

Aby przetestować kroki opisane w tym samouczku, należy wykonać te zalecenia:

* Nie należy używać środowiska produkcyjnego, chyba że jest to konieczne.
* Jeśli nie masz środowisko wersji próbnej usługi Azure AD, możesz pobrać miesięczna wersja próbna [tutaj](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Opis scenariusza
W tym samouczku można przetestować usługę Azure AD rejestracji jednokrotnej w środowisku testowym. Scenariusz opisany w tym samouczku składa się z dwóch głównych elementów:

1. Dodawanie usługi ServiceNow z galerii
2. Konfigurowanie i testowanie usługi Azure AD logowanie jednokrotne dla usługi ServiceNow lub usługi ServiceNow Express

## <a name="adding-servicenow-from-the-gallery"></a>Dodawanie usługi ServiceNow z galerii
Aby skonfigurować integrację usługi ServiceNow lub Express usługi ServiceNow z usługą Azure AD, należy dodać usługi ServiceNow z galerii do listy zarządzanych aplikacji SaaS. 

**Aby dodać usługi ServiceNow z galerii, wykonaj następujące czynności:**

1. W **klasycznego portalu Azure**, w lewym okienku nawigacji, kliknij polecenie **usługi Active Directory**. 
   
    ![Usługa Active Directory][1]
2. Z **katalogu** listy, wybierz katalog, dla którego chcesz włączyć integracji katalogów.
3. Aby otworzyć widok aplikacji, w widoku katalogu, kliknij przycisk **aplikacji** w menu u góry.
   
    ![Aplikacje][2]
4. Kliknij przycisk **Dodaj** w dolnej części strony.
   
    ![Aplikacje][3]
5. Na **co chcesz zrobić** okna dialogowego, kliknij przycisk **dodać aplikację z galerii**.
   
    ![Aplikacje][4]
6. W polu wyszukiwania wpisz **ServiceNow**.
   
    ![Tworzenie użytkownika testowego usługi Azure AD](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_01.png)
7. W okienku wyników wybierz **ServiceNow**, a następnie kliknij przycisk **Complete** można dodać aplikację.
   
    ![Tworzenie użytkownika testowego usługi Azure AD](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_02.png)

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurowanie i testowanie usługi Azure AD logowanie jednokrotne
W tej sekcji możesz skonfigurować i test usługi Azure AD rejestracji jednokrotnej z usługi ServiceNow lub Express usługi ServiceNow w oparciu o nazwie "Britta Simona" użytkownika testowego.

Dla rejestracji jednokrotnej do pracy usługi Azure AD musi wiedzieć, użytkownik odpowiednika w ServiceNow jest dla użytkownika, w usłudze Azure AD. Innymi słowy łącze relację między użytkownikiem usługi Azure AD i danemu użytkownikowi w usługi ServiceNow musi się.
Ta relacja łącza zostanie nawiązane, przypisując wartość **nazwy użytkownika** w usłudze Azure AD jako wartość **Username** w usługi ServiceNow. Aby skonfigurować i przetestować usługi Azure AD rejestracji jednokrotnej z usługi ServiceNow, należy wykonać poniższe bloki konstrukcyjne:

1. **[Konfigurowanie usługi Azure AD rejestracji jednokrotnej dla usługi ServiceNow](#configuring-azure-ad-single-sign-on-for-servicenow)**  — aby umożliwić użytkownikom korzystać z tej funkcji.
2. **[Konfigurowanie usługi Azure AD logowania jednokrotnego usługi ServiceNow Express](#configuring-azure-ad-single-sign-on-for-servicenow-express)**  — aby umożliwić użytkownikom korzystać z tej funkcji.
3. **[Tworzenie użytkownika testowego usługi Azure AD](#creating-an-azure-ad-test-user)**  — do przetestowania usługi Azure AD rejestracji jednokrotnej z Simona Britta.
4. **[Tworzenie użytkownika testowego usługi ServiceNow](#creating-a-servicenow-test-user)**  — w celu zapewnienia odpowiednikiem Simona Britta ServiceNow połączonego z jej reprezentacji usługi Azure AD.
5. **[Przypisanie użytkownika testowego usługi Azure AD](#assigning-the-azure-ad-test-user)**  — aby umożliwić Simona Britta do użycia usługi Azure AD rejestracji jednokrotnej.
6. **[Testowanie rejestracji jednokrotnej](#testing-single-sign-on)**  — Aby sprawdzić, czy konfiguracja działa.

> [!NOTE]
> Jeśli chcesz skonfigurować usługi ServiceNow pominąć krok 2. Podobnie jeśli chcesz skonfigurować usługi ServiceNow Express pominąć krok 1.
> 
> 

### <a name="configuring-azure-ad-single-sign-on-for-servicenow"></a>Konfigurowanie usługi Azure AD rejestracji jednokrotnej dla usługi ServiceNow
1. W klasycznym portalu usługi Azure AD na **ServiceNow** strona integracji aplikacji, kliknij przycisk **skonfigurować logowanie jednokrotne** otworzyć **skonfigurować rejestrację jednokrotną** okna dialogowego.
   
    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-servicenow-tutorial/IC749323.png "skonfigurować logowanie jednokrotne")

2. Na **jak chcesz użytkownikom zalogować się do usługi ServiceNow** wybierz pozycję **usługi Microsoft Azure AD rejestracji jednokrotnej**, a następnie kliknij przycisk **dalej**.
   
    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-servicenow-tutorial/IC749324.png "skonfigurować logowanie jednokrotne")

3. Na **Konfigurowanie ustawień aplikacji** wykonaj następujące czynności:
   
    ![Skonfiguruj adres URL aplikacji](./media/active-directory-saas-servicenow-tutorial/IC769497.png "skonfigurować adres URL aplikacji")
   
    a. w **usługi ServiceNow na adres URL logowania** tekstowym, wpisz adres URL używany przez użytkowników do logowania jednokrotnego do aplikacji usługi ServiceNow zgodnie ze wzorcem: `https://<instance-name>.service-now.com`.
   
    b. W **identyfikator** tekstowym, wpisz adres URL używany przez użytkowników do logowania jednokrotnego do aplikacji usługi ServiceNow zgodnie ze wzorcem: `https://<instance-name>.service-now.com`.
   
    c. Kliknij przycisk **Dalej**

4. Aby program Azure AD automatycznie konfigurować usługi ServiceNow dla uwierzytelniania opartego na SAML, wprowadź Twojej nazwy wystąpienia usługi ServiceNow, nazwa użytkownika i hasło administratora w **automatycznie skonfigurować logowanie jednokrotne** tworzą, a następnie kliknij przycisk  *Skonfiguruj*. Należy pamiętać, że podana nazwa użytkownika administratora musi mieć **security_admin** rola przypisana w usługi ServiceNow tej pracy. W przeciwnym razie, aby ręcznie skonfigurować usługi ServiceNow przy użyciu usługi Azure AD jako dostawca tożsamości SAML, kliknij przycisk **ręcznie skonfigurować aplikację do logowania jednokrotnego**, następnie kliknij przycisk **dalej** i wykonaj następujące czynności .
   
    ![Skonfiguruj adres URL aplikacji](./media/active-directory-saas-servicenow-tutorial/IC7694971.png "skonfigurować adres URL aplikacji")

5. Na **skonfigurować logowanie jednokrotne w ServiceNow** kliknij przycisk **pobierania certyfikatu**, Zapisz plik certyfikatu lokalnie na komputerze.
   
    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-servicenow-tutorial/IC749325.png "skonfigurować logowanie jednokrotne")

6. Logowanie do aplikacji usługi ServiceNow jako administrator.

7. Aktywuj *integracja — wiele pojedynczego logowania Instalatora dostawcy* wtyczki dalej wykonaj czynności:
   
    a. W okienku nawigacji po lewej stronie, przejdź do **definicji systemu** sekcji, a następnie kliknij przycisk **wtyczek**.
   
    ![Skonfiguruj adres URL aplikacji](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_03.png "aktywowanie wtyczki")
   
    b. Wyszukaj *integracja — wiele pojedynczego logowania Instalatora dostawcy*.
   
    ![Skonfiguruj adres URL aplikacji](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_04.png "aktywowanie wtyczki")
   
    c. Wybierz wtyczki. Rigth kliknij i zaznacz **Aktywuj/Upgrade**.
   
    d. Kliknij przycisk **Aktywuj** przycisku.

8. W okienku nawigacji po lewej stronie kliknij **właściwości**.  
   
    ![Skonfiguruj adres URL aplikacji](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_06.png "skonfigurować adres URL aplikacji")

9. Na **wiele właściwości logowania jednokrotnego dostawcy** okna dialogowego, wykonaj następujące czynności:
   
    ![Skonfiguruj adres URL aplikacji](./media/active-directory-saas-servicenow-tutorial/IC7694981.png "skonfigurować adres URL aplikacji")
   
    a. Jako **włączyć wiele dostawcy logowania jednokrotnego**, wybierz pozycję **tak**.
   
    b. Jako **włączania rejestrowania debugowania uzyskano wiele dostawcy logowania jednokrotnego integracji**, wybierz pozycję **tak**.
   
    c. W **tabeli pole użytkownika, który...**  pole tekstowe, typ **nazwa_użytkownika**.
   
    d. Kliknij pozycję **Zapisz**.

10. W okienku nawigacji po lewej stronie kliknij **certyfikaty x509**.
    
     ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_05.png "skonfigurować logowanie jednokrotne")

11. Na **certyfikatów X.509** okna dialogowego, kliknij przycisk **nowy**.
    
     ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-servicenow-tutorial/IC7694974.png "skonfigurować logowanie jednokrotne")

12. Na **certyfikatów X.509** okna dialogowego, wykonaj następujące czynności:
    
     ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-servicenow-tutorial/IC7694975.png "skonfigurować logowanie jednokrotne")
    
     a. Kliknij przycisk **Nowy**.
    
     b. W **nazwa** tekstowym, wpisz nazwę konfiguracji (np.: **TestSAML2.0**).
    
     c. Wybierz **Active**.
    
     d. Jako **Format**, wybierz pozycję **PEM**.
    
     e. Jako **typu**, wybierz pozycję **zaufania certyfikatów magazynu**.
    
     f. Otwórz w Notatniku Twojego certyfikatu szyfrowania Base64, skopiuj zawartość go do Schowka, a następnie wklej go do **certyfikatu PEM** pola tekstowego.
    
     g. Kliknij przycisk **aktualizacji**.

13. W okienku nawigacji po lewej stronie kliknij **dostawców tożsamości**.
    
     ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_07.png "skonfigurować logowanie jednokrotne")

14. Na **dostawców tożsamości** okna dialogowego, kliknij przycisk **nowy**:
    
     ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-servicenow-tutorial/IC7694977.png "skonfigurować logowanie jednokrotne")

15. Na **dostawców tożsamości** okna dialogowego, kliknij przycisk **aktualizację1 SAML2?**:
    
     ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-servicenow-tutorial/IC7694978.png "skonfigurować logowanie jednokrotne")

16. W oknie dialogowym właściwości aktualizację1 SAML2 wykonaj następujące czynności:
    
     ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-servicenow-tutorial/IC7694982.png "skonfigurować logowanie jednokrotne")

    a. w **nazwa** tekstowym, wpisz nazwę konfiguracji (np.: **SAML 2.0**).

    b. W **pole użytkownika** pole tekstowe, typ **e-mail** lub **nazwa_użytkownika**, w zależności od pole, które jest używane do unikatowego identyfikowania użytkowników w danym wdrożeniu usługi ServiceNow. 

    > [!NOTE] 
    > Można configue usługi Azure AD, aby emitować identyfikator użytkownika usługi Azure AD (główna nazwa użytkownika) lub adres e-mail jako unikatowy identyfikator w tokenie SAML, przechodząc do **ServiceNow > atrybuty > logowanie jednokrotne** sekcji klasyczny Portal Azure Portal i mapowanie odpowiednie pola **nameidentifier** atrybutu. Wartość przechowywana wybranego atrybutu w usłudze Azure AD (np. główna nazwa użytkownika) musi odpowiadać wartości przechowywane w usługi ServiceNow wprowadzony w polu (np. nazwa_użytkownika)

    c. W klasycznym portalu usługi Azure AD, skopiuj **identyfikator dostawcy tożsamości** wartość, a następnie wklej ją do **adres URL dostawcy tożsamości** pola tekstowego.

    d. W klasycznym portalu usługi Azure AD, skopiuj **adres URL żądania uwierzytelniania** wartość, a następnie wklej ją do **AuthnRequest dostawcy tożsamości** pola tekstowego.

    e. W klasycznym portalu usługi Azure AD, skopiuj **pojedynczy adres URL usługi Sign-Out** wartość, a następnie wklej ją do **SingleLogoutRequest dostawcy tożsamości** pola tekstowego.

    f. W **strony głównej usługi ServiceNow** tekstowym, wpisz adres URL strony głównej wystąpienia usługi ServiceNow.

    > [!NOTE] 
    > Na stronie głównej wystąpienia usługi ServiceNow jest złączeniem Twojej **adres URL dzierżawy ServieNow** i **/navpage.do** (np.:`https://fabrikam.service-now.com/navpage.do`).

    g. W **identyfikator podmiot / wystawca** pole tekstowe, wpisz adres URL dzierżawy usługi ServiceNow.

    h. W **URL odbiorców** tekstowym, wpisz adres URL dzierżawy usługi ServiceNow. 

    i. W **powiązanie protokołu IDP SingleLogoutRequest** pole tekstowe, typ **urn: oasis: nazwy: tc: SAML:2.0:bindings:HTTP-przekierowania**.

    j. W polu tekstowym zasad NameID wpisz **urn: oasis: nazwy: tc: SAML:1.1:nameid-format: nieokreślony**.

    k. Anuluj wybór **utworzyć AuthnContextClass**.

    l. W **metody AuthnContextClassRef**, typ `http://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password`. Jest to potrzebne tylko w przypadku organizacji tylko w chmurze. Jeśli używasz lokalnych usług AD FS lub usługę MFA dla uwierzytelniania, a następnie ta wartość nie należy konfigurować. 

    m. W **pochylanie zegara** pole tekstowe, typ **60**.

    n. Jako **pojedynczy znak na skrypt**, wybierz pozycję **MultiSSO_SAML2_Update1**.

    o. Jako **x509 certyfikatu**, wybierz certyfikat został utworzony w poprzednim kroku.

    p. Kliknij przycisk **przesłać**. 

1. W klasycznym portalu usługi Azure AD, wybierz Potwierdzenie konfiguracji rejestracji jednokrotnej, a następnie kliknij **dalej**. 
   
    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-servicenow-tutorial/IC7694990.png "skonfigurować logowanie jednokrotne")

2. Na **pojedynczy znak na potwierdzenie** kliknij przycisk **Complete**.
   
    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-servicenow-tutorial/IC7694991.png "skonfigurować logowanie jednokrotne")

### <a name="configuring-azure-ad-single-sign-on-for-servicenow-express"></a>Konfigurowanie usługi Azure AD jednokrotnej usługi ServiceNow Express
1. W klasycznym portalu usługi Azure AD na **ServiceNow** strona integracji aplikacji, kliknij przycisk **skonfigurować logowanie jednokrotne** otworzyć **skonfigurować rejestrację jednokrotną** okna dialogowego.
   
    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-servicenow-tutorial/IC749323.png "skonfigurować logowanie jednokrotne")

2. Na **jak chcesz użytkownikom zalogować się do usługi ServiceNow** wybierz pozycję **usługi Microsoft Azure AD rejestracji jednokrotnej**, a następnie kliknij przycisk **dalej**.
   
    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-servicenow-tutorial/IC749324.png "skonfigurować logowanie jednokrotne")

3. Na **Konfigurowanie ustawień aplikacji** wykonaj następujące czynności:
   
    ![Skonfiguruj adres URL aplikacji](./media/active-directory-saas-servicenow-tutorial/IC769497.png "skonfigurować adres URL aplikacji")
   
    a. w **usługi ServiceNow na adres URL logowania** tekstowym, wpisz adres URL używany przez użytkowników do logowania jednokrotnego do aplikacji usługi ServiceNow zgodnie ze wzorcem: `https://<instance-name>.service-now.com`.
   
    b. W **adres URL wystawcy** tekstowym, wpisz adres URL używany przez użytkowników do logowania jednokrotnego do aplikacji usługi ServiceNow zgodnie ze wzorcem `https://<instance-name>.service-now.com`.
   
    c. Kliknij przycisk **Dalej**

4. Kliknij przycisk **ręcznie skonfigurować aplikację do logowania jednokrotnego**, następnie kliknij przycisk **dalej** i wykonaj następujące kroki.
   
    ![Skonfiguruj adres URL aplikacji](./media/active-directory-saas-servicenow-tutorial/IC7694971.png "skonfigurować adres URL aplikacji")

5. Na **skonfigurować logowanie jednokrotne w ServiceNow** kliknij przycisk **pobierania certyfikatu**, Zapisz plik certyfikatu lokalnie na komputerze, a następnie kliknij przycisk **dalej**.
   
    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-servicenow-tutorial/IC749325.png "skonfigurować logowanie jednokrotne")

6. Logowanie do aplikacji usługi ServiceNow Express jako administrator.

7. W okienku nawigacji po lewej stronie kliknij **rejestracji jednokrotnej**.  
   
    ![Skonfiguruj adres URL aplikacji](./media/active-directory-saas-servicenow-tutorial/ic7694980ex.png "skonfigurować adres URL aplikacji")

8. Na **rejestracji jednokrotnej** okna dialogowego, kliknij ikonę konfiguracji w prawym górnym rogu i ustaw następujące właściwości:
   
    ![Skonfiguruj adres URL aplikacji](./media/active-directory-saas-servicenow-tutorial/ic7694981ex.png "skonfigurować adres URL aplikacji")
   
    a. Przełącz **włączyć wiele dostawcy logowania jednokrotnego** z prawej strony.
   
    b. Przełącz **debugowania Włącz rejestrowanie dla wielu dostawcy logowania jednokrotnego integracji** z prawej strony.
   
    c. W **tabeli pole użytkownika, który...**  pole tekstowe, typ **nazwa_użytkownika**.
9. Na **rejestracji jednokrotnej** okna dialogowego, kliknij przycisk **Dodaj nowy certyfikat**.
   
    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-servicenow-tutorial/ic7694973ex.png "skonfigurować logowanie jednokrotne")
10. Na **certyfikatów X.509** okna dialogowego, wykonaj następujące czynności:
    
    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-servicenow-tutorial/IC7694975.png "skonfigurować logowanie jednokrotne")
    
    a. W **nazwa** tekstowym, wpisz nazwę konfiguracji (np.: **TestSAML2.0**).
    
    b. Wybierz **Active**.
    
    c. Jako **Format**, wybierz pozycję **PEM**.
    
    d. Jako **typu**, wybierz pozycję **zaufania certyfikatów magazynu**.
    
    e. Utwórz plik kodowany w standardzie Base64 z pobranego certyfikatu.
    
    > [!NOTE]
    > Aby uzyskać więcej informacji, zobacz [sposób konwertowania binarne certyfikatu do pliku tekstowego](http://youtu.be/PlgrzUZ-Y1o).
    > 
    > 
    
    f. Otwórz w Notatniku Twojego certyfikatu szyfrowania Base64, skopiuj zawartość go do Schowka, a następnie wklej go do **certyfikatu PEM** pola tekstowego.
    
    g. Kliknij przycisk **aktualizacji**.
11. Na **rejestracji jednokrotnej** okna dialogowego, kliknij przycisk **Dodaj nowe IdP**.
    
    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-servicenow-tutorial/ic7694976ex.png "skonfigurować logowanie jednokrotne")
12. Na **Dodaj nowego dostawcę tożsamości** okna dialogowego, w obszarze **Konfigurowanie dostawcy tożsamości**, wykonaj następujące czynności:
    
    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-servicenow-tutorial/ic7694982ex.png "skonfigurować logowanie jednokrotne")

    a. w **nazwa** tekstowym, wpisz nazwę konfiguracji (np.: **SAML 2.0**).

    b. W klasycznym portalu usługi Azure AD, skopiuj **identyfikator dostawcy tożsamości** wartość, a następnie wklej ją do **adres URL dostawcy tożsamości** pola tekstowego.

    c. W klasycznym portalu usługi Azure AD, skopiuj **adres URL żądania uwierzytelniania** wartość, a następnie wklej ją do **AuthnRequest dostawcy tożsamości** pola tekstowego.

    d. W klasycznym portalu usługi Azure AD, skopiuj **pojedynczy adres URL usługi Sign-Out** wartość, a następnie wklej ją do **SingleLogoutRequest dostawcy tożsamości** pola tekstowego.

    e. Jako **certyfikat dostawcy tożsamości**, wybierz certyfikat został utworzony w poprzednim kroku.


1. Kliknij przycisk **Zaawansowane ustawienia**, a następnie w obszarze **dodatkowe właściwości dostawcy tożsamości**, wykonaj następujące czynności:
   
    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-servicenow-tutorial/ic7694983ex.png "skonfigurować logowanie jednokrotne")
   
    a. W **powiązanie protokołu IDP SingleLogoutRequest** pole tekstowe, typ **urn: oasis: nazwy: tc: SAML:2.0:bindings:HTTP-przekierowania**.
   
    b. W **zasad NameID** pole tekstowe, typ **urn: oasis: nazwy: tc: SAML:1.1:nameid-format: nieokreślony**.    
   
    c. W **metody AuthnContextClassRef**, typ **http://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password**.
   
    d. Anuluj wybór **utworzyć AuthnContextClass**.

2. W obszarze **dodatkowe właściwości dostawcy usług**, wykonaj następujące czynności:
   
    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-servicenow-tutorial/ic7694984ex.png "skonfigurować logowanie jednokrotne")
   
    a. W **strony głównej usługi ServiceNow** tekstowym, wpisz adres URL strony głównej wystąpienia usługi ServiceNow.
   
    > [!NOTE]
    > Na stronie głównej wystąpienia usługi ServiceNow jest złączeniem Twojej **adres URL dzierżawy ServieNow** i **/navpage.do** (np.: `https://fabrikam.service-now.com/navpage.do`).
    > 
    > 
   
    b. W **identyfikator podmiot / wystawca** pole tekstowe, wpisz adres URL dzierżawy usługi ServiceNow.
   
    c. W **identyfikatora URI odbiorców** tekstowym, wpisz adres URL dzierżawy usługi ServiceNow. 
   
    d. W **pochylanie zegara** pole tekstowe, typ **60**.
   
    e. W **pole użytkownika** pole tekstowe, typ **e-mail** lub **nazwa_użytkownika**, w zależności od pole, które jest używane do unikatowego identyfikowania użytkowników w danym wdrożeniu usługi ServiceNow.
   
    > [!NOTE]
    > Można configue usługi Azure AD, aby emitować identyfikator użytkownika usługi Azure AD (główna nazwa użytkownika) lub adres e-mail jako unikatowy identyfikator w tokenie SAML, przechodząc do **ServiceNow > atrybuty > logowanie jednokrotne** sekcji klasyczny Portal Azure Portal i mapowanie odpowiednie pola **nameidentifier** atrybutu. Wartość przechowywana wybranego atrybutu w usłudze Azure AD (np. główna nazwa użytkownika) musi odpowiadać wartości przechowywane w usługi ServiceNow wprowadzony w polu (np. nazwa_użytkownika)
    > 
    > 
   
    f. Kliknij pozycję **Zapisz**. 

3. W klasycznym portalu usługi Azure AD, wybierz Potwierdzenie konfiguracji rejestracji jednokrotnej, a następnie kliknij **dalej**. 
   
    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-servicenow-tutorial/IC7694990.png "skonfigurować logowanie jednokrotne")

4. Na **pojedynczy znak na potwierdzenie** kliknij przycisk **Complete**.
   
    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-servicenow-tutorial/IC7694991.png "skonfigurować logowanie jednokrotne")

## <a name="configuring-user-provisioning"></a>Konfigurowanie Inicjowanie obsługi użytkowników
Celem tej sekcji jest przedstawiają sposób włączania kont użytkowników usługi Active Directory do usługi ServiceNow Inicjowanie obsługi użytkowników.

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>Aby skonfigurować, inicjowanie obsługi użytkowników, wykonaj następujące czynności:
1. W klasycznym portalu Azure Management na **ServiceNow** strona integracji aplikacji, kliknij przycisk **skonfigurować Inicjowanie obsługi użytkowników**. 
   
    ![Inicjowanie obsługi użytkowników](./media/active-directory-saas-servicenow-tutorial/IC769498.png "Inicjowanie obsługi użytkowników")

2. Na **wprowadź swoje poświadczenia usługi ServiceNow, aby umożliwić inicjowanie obsługi użytkowników automatyczne** Podaj następujące ustawienia konfiguracji:
   
     a. W **nazwy wystąpienia usługi ServiceNow** tekstowym, wpisz nazwę wystąpienia usługi ServiceNow.
   
     b. W **nazwa użytkownika administratora usługi ServiceNow** tekstowym, wpisz nazwę konta administratora usługi ServiceNow.
   
     c. W **hasło administratora usługi ServiceNow** tekstowym, wpisz hasło dla tego konta.
   
     d. Kliknij przycisk **zweryfikować** Aby zweryfikować konfigurację.
   
     e. Kliknij przycisk **dalej** przycisk, aby otworzyć **następne kroki** strony.
   
     f. Jeśli chcesz udostępnić wszystkim użytkownikom na tę aplikację, wybierz opcję "**automatycznie obsługiwać wszystkie konta użytkowników w katalogu do tej aplikacji**". 
   
    ![Następne kroki](./media/active-directory-saas-servicenow-tutorial/IC698804.png "następne kroki")
   
     g. Na **następne kroki** kliknij przycisk **Complete** Aby zapisać konfigurację.

### <a name="creating-an-azure-ad-test-user"></a>Tworzenie użytkownika testowego usługi Azure AD
W tej sekcji utworzysz użytkownika testowego w klasycznym portalu o nazwie Simona Britta.

![Tworzenie użytkowników usługi Azure AD][20]

**Aby utworzyć użytkownika testowego w usłudze Azure AD, wykonaj następujące czynności:**

1. W **klasycznego portalu Azure**, w lewym okienku nawigacji, kliknij polecenie **usługi Active Directory**.
   
    ![Tworzenie użytkownika testowego usługi Azure AD](./media/active-directory-saas-servicenow-tutorial/create_aaduser_09.png) 

2. Z **katalogu** listy, wybierz katalog, dla którego chcesz włączyć integracji katalogów.

3. Aby wyświetlić listę użytkowników, w menu u góry, kliknij przycisk **użytkowników**.
   
    ![Tworzenie użytkownika testowego usługi Azure AD](./media/active-directory-saas-servicenow-tutorial/create_aaduser_03.png) 

4. Aby otworzyć **Dodaj użytkownika** okna dialogowego na pasku narzędzi u dołu, kliknij przycisk **Dodaj użytkownika**.
   
    ![Tworzenie użytkownika testowego usługi Azure AD](./media/active-directory-saas-servicenow-tutorial/create_aaduser_04.png) 

5. Na **Poinformuj nas o tym użytkowniku** okna dialogowego strony, należy wykonać następujące czynności:
   
    ![Tworzenie użytkownika testowego usługi Azure AD](./media/active-directory-saas-servicenow-tutorial/create_aaduser_05.png) 
   
    a. Jako typ użytkownika wybierz nowego użytkownika w organizacji.
   
    b. W nazwie użytkownika **pole tekstowe**, typ **BrittaSimon**.
   
    c. Kliknij przycisk **Dalej**.

6. Na **profilu użytkownika** okna dialogowego strony, należy wykonać następujące czynności:
   
   ![Tworzenie użytkownika testowego usługi Azure AD](./media/active-directory-saas-servicenow-tutorial/create_aaduser_06.png) 
   
   a. W **imię** pole tekstowe, typ **Britta**.  
   
   b. W **nazwisko** pole tekstowe, typ **Simona**.
   
   c. W **Nazwa wyświetlana** pole tekstowe, typ **Simona Britta**.
   
   d. W **roli** listy, wybierz **użytkownika**.
   
   e. Kliknij przycisk **Dalej**.

7. Na **Uzyskaj hasło tymczasowe** strony okna dialogowego, kliknij przycisk **utworzyć**.
   
    ![Tworzenie użytkownika testowego usługi Azure AD](./media/active-directory-saas-servicenow-tutorial/create_aaduser_07.png) 

8. Na **Uzyskaj hasło tymczasowe** okna dialogowego strony, należy wykonać następujące czynności:
   
    ![Tworzenie użytkownika testowego usługi Azure AD](./media/active-directory-saas-servicenow-tutorial/create_aaduser_08.png) 
   
    a. Zanotuj wartość **nowe hasło**.
   
    b. Kliknij przycisk **Complete** (Zakończ).   

### <a name="creating-a-servicenow-test-user"></a>Tworzenie użytkownika testowego usługi ServiceNow
W tej sekcji należy utworzyć użytkownika o nazwie Simona Britta w usługi ServiceNow. W tej sekcji należy utworzyć użytkownika o nazwie Simona Britta w usługi ServiceNow. Jeśli nie wiadomo, jak dodać użytkownika w ramach konta usługi ServiceNow lub usługi ServiceNow Express, skontaktuj się z zespołem pomocy technicznej usługi ServiceNow.

### <a name="assigning-the-azure-ad-test-user"></a>Przypisanie użytkownika testowego usługi Azure AD
W tej sekcji można włączyć Simona Britta na udostępnienie jej do usługi ServiceNow za pomocą usługi Azure rejestracji jednokrotnej.

![Przypisz użytkownika][200] 

**Aby przypisać Simona Britta usługi ServiceNow, wykonaj następujące czynności:**

1. W klasycznym portalu, aby otworzyć widok aplikacji, w widoku katalogu, kliknij przycisk **aplikacji** w menu u góry.
   
    ![Przypisz użytkownika][201] 

2. Na liście aplikacji zaznacz **ServiceNow**.
   
    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_10.png) 

3. W menu u góry kliknij **użytkowników**.
   
    ![Przypisz użytkownika][203] 

4. Na liście wszystkich użytkowników, wybierz **Simona Britta**.

5. Na pasku narzędzi u dołu, kliknij przycisk **przypisać**.
   
    ![Przypisz użytkownika][205]

### <a name="testing-single-sign-on"></a>Testowanie rejestracji jednokrotnej
Celem tej sekcji służy do testowania konfiguracji usługi Azure AD pojedynczego logowania za pomocą panelu dostępu.

Po kliknięciu kafelka usługi ServiceNow w panelu dostępu użytkownik powinien pobrać automatycznie zalogowane do aplikacji usługi ServiceNow.

## <a name="additional-resources"></a>Dodatkowe zasoby
* [Lista samouczków dotyczących sposobów integracji aplikacji SaaS przy użyciu usługi Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co to jest dostęp do aplikacji i logowanie jednokrotne z usługą Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_04.png


[5]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_05.png
[6]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_06.png
[7]:  ./media/active-directory-saas-servicenow-tutorial/tutorial_general_050.png
[10]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_060.png
[11]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_070.png
[20]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_205.png
