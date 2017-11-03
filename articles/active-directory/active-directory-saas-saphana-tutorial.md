---
title: 'Samouczek: Integracji Azure Active Directory z SAP HANA | Dokumentacja firmy Microsoft'
description: "Informacje o sposobie konfigurowania rejestracji jednokrotnej między usługą Azure Active Directory i SAP HANA."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: cef4a146-f4b0-4e94-82de-f5227a4b462c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jeedes
ms.openlocfilehash: a7e73f6ee763d1005ad85935cf2d8f6b24ecf116
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sap-hana"></a>Samouczek: Integracji Azure Active Directory z SAP HANA

Z tego samouczka dowiesz się sposobu integracji z usługą Azure Active Directory (Azure AD) SAP HANA.

Integrowanie SAP HANA z usługą Azure AD zapewnia następujące korzyści:

- Można kontrolować w usłudze Azure AD, który ma dostęp do programu SAP HANA
- Umożliwia użytkownikom automatycznie pobrać zalogowane do SAP HANA (logowanie jednokrotne) z konta usługi Azure AD
- Możesz zarządzać kont w jednej centralnej lokalizacji - portalu Azure

Jeśli chcesz dowiedzieć się więcej informacji o integracji aplikacji SaaS w usłudze Azure AD, zobacz [co to jest dostęp do aplikacji i logowanie jednokrotne z usługą Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Wymagania wstępne

Aby skonfigurować integrację usługi Azure AD z SAP HANA, potrzebne są następujące elementy:

- Subskrypcję usługi Azure AD
- SAP HANA logowanie jednokrotne włączone subskrypcji
- HANA uruchomione wystąpienie na publicznych IaaS, lokalnymi, maszynach wirtualnych platformy Azure lub SAP dużych wystąpień na platformie Azure
- Interfejs sieci Web administracji XSA jak również zainstalowana w wystąpieniu HANA Studio HANA.

> [!NOTE]
> Aby przetestować kroki opisane w tym samouczku, zaleca się używania środowiska produkcyjnego programu SAP HANA. Przetestowanie integracji w rozwoju lub środowisko przejściowe aplikacji, a następnie użyj środowiska produkcyjnego.

Aby przetestować kroki opisane w tym samouczku, należy wykonać te zalecenia:

- Nie należy używać środowiska produkcyjnego, jeśli jest to konieczne.
- Jeśli nie masz środowisko wersji próbnej usługi Azure AD, możesz [uzyskać miesięczna wersja próbna](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Opis scenariusza
W tym samouczku można przetestować usługę Azure AD rejestracji jednokrotnej w środowisku testowym. Scenariusz opisany w tym samouczku składa się z dwóch głównych elementów:

1. Dodawanie SAP HANA z galerii
2. Konfigurowanie i testowanie usługi Azure AD logowanie jednokrotne

## <a name="adding-sap-hana-from-the-gallery"></a>Dodawanie SAP HANA z galerii
Aby skonfigurować integrację usługi Azure AD SAP HANA, należy dodać SAP HANA z galerii do listy zarządzanych aplikacji SaaS.

**Aby dodać SAP HANA z galerii, wykonaj następujące czynności:**

1. W  **[portalu Azure](https://portal.azure.com)**, na panelu nawigacyjnym po lewej stronie kliknij **usługi Azure Active Directory** ikony. 

    ![Przycisk usługi Azure Active Directory][1]

2. Przejdź do **aplikacje dla przedsiębiorstw**. Następnie przejdź do **wszystkie aplikacje**.

    ![Blok aplikacje przedsiębiorstwa][2]
    
3. Aby dodać nową aplikację, kliknij przycisk **nowej aplikacji** przycisk w górnej części okna dialogowego.

    ![Nowy przycisk aplikacji][3]

4. W polu wyszukiwania wpisz **SAP HANA**, wybierz pozycję **SAP HANA** z panelu wyników kliknięcie **Dodaj** przycisk, aby dodać aplikację. 

    ![Nowa aplikacja](./media/active-directory-saas-saphana-tutorial/tutorial_saphana_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurowanie i testowanie usługi Azure AD logowanie jednokrotne
W tej sekcji możesz skonfigurować i przetestować usługi Azure AD rejestracji jednokrotnej z SAP HANA oparte na koncie użytkownika testu o nazwie "Britta Simona".

Dla rejestracji jednokrotnej do pracy usługi Azure AD musi wiedzieć, użytkownik odpowiednika w SAP HANA jest dla użytkownika, w usłudze Azure AD. Innymi słowy musi można ustanowić łącze relację między użytkownikiem usługi Azure AD i danemu użytkownikowi w SAP HANA.

SAP HANA przypisywanie wartości **nazwy użytkownika** w usłudze Azure AD jako wartość **Username** do ustanawiania relacji łącza.

Aby skonfigurować i przetestować usługi Azure AD rejestracji jednokrotnej z SAP HANA, należy wykonać poniższe bloki konstrukcyjne:

1. **[Konfigurowanie usługi Azure AD rejestracji jednokrotnej](#configuring-azure-ad-single-sign-on)**  — aby umożliwić użytkownikom korzystać z tej funkcji.
2. **[Tworzenie użytkownika testowego usługi Azure AD](#creating-an-azure-ad-test-user)**  — do przetestowania usługi Azure AD rejestracji jednokrotnej z Simona Britta.
3. **[Tworzenie użytkownika testowego SAP HANA](#creating-a-sap-hana-test-user)**  — w celu zapewnienia odpowiednikiem Simona Britta SAP HANA, połączonej z usługi Azure AD reprezentację użytkownika.
4. **[Przypisanie użytkownika testowego usługi Azure AD](#assigning-the-azure-ad-test-user)**  — aby umożliwić Simona Britta do użycia usługi Azure AD rejestracji jednokrotnej.
5. **[Testowanie rejestracji jednokrotnej](#testing-single-sign-on)**  — Aby sprawdzić, czy konfiguracja działa.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurowanie usługi Azure AD rejestracji jednokrotnej

W tej sekcji można włączyć usługi Azure AD rejestracji jednokrotnej w portalu Azure i skonfigurować logowanie jednokrotne do aplikacji SAP HANA.

**Aby skonfigurować usługi Azure AD rejestracji jednokrotnej z SAP HANA, wykonaj następujące czynności:**

1. W portalu Azure na **SAP HANA** strona integracji aplikacji, kliknij przycisk **logowanie jednokrotne**.

    ![Konfigurowanie rejestracji jednokrotnej][4]

2. Na **logowanie jednokrotne** okno dialogowe, wybierz opcję **tryb** jako **na języku SAML logowania jednokrotnego** Aby włączyć logowanie jednokrotne.
 
    ![Okno dialogowe rejestracji jednokrotnej](./media/active-directory-saas-saphana-tutorial/tutorial_saphana_samlbase.png)

3. Na **adresy URL i SAP HANA domeny** sekcji, wykonaj następujące czynności:

    ![Domena i adres URL z jednym informacje logowania jednokrotnego](./media/active-directory-saas-saphana-tutorial/tutorial_saphana_url.png)

    a. W **identyfikator** pole tekstowe, typu co:`HA100` 

    b. W **adres URL odpowiedzi** tekstowym, wpisz adres URL, używając następującego wzorca:`https://<Customer-SAP-instance-url>/sap/hana/xs/saml/login.xscfunc`

    > [!NOTE] 
    > Wartości te nie są prawdziwe. Rzeczywisty identyfikator i adres URL odpowiedzi, należy zaktualizować te wartości. Skontaktuj się z [zespołem pomocy technicznej SAP HANA klienta](https://cloudplatform.sap.com/contact.html) uzyskać te wartości. 

4. Na **certyfikat podpisywania SAML** kliknij **XML metadanych** , a następnie zapisz plik metadanych na tym komputerze.

    ![Łącze pobierania certyfikatu](./media/active-directory-saas-saphana-tutorial/tutorial_saphana_certificate.png) 

    >[!Note]
    >Jeśli certyfikat nie jest aktywne następnie uaktywnić go, klikając pole wyboru "Uaktywnij nowy certyfikat" w usłudze Azure AD. 

5. Aplikacja SAP HANA oczekuje potwierdzenia języka SAML w określonym formacie. Poniższy zrzut ekranu przedstawia przykład tego. W tym miejscu możemy zamapowaniu **identyfikator użytkownika** z **ExtractMailPrefix()** funkcji **user.mail**. Dzięki temu wartość prefiksu adres e-mail użytkownika, który jest unikatowy identyfikator użytkownika. To są wysyłane do aplikacji SAP HANA w każdym pomyślnym odpowiedzi.

    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-saphana-tutorial/attribute.png)

6. W **atrybuty użytkownika** sekcji na **logowanie jednokrotne** okna dialogowego:

    a. W **identyfikator użytkownika** listy rozwijanej wybierz **ExtractMailPrefix**.
    
    b. W **poczty** listy rozwijanej wybierz **user.mail**.

7. Kliknij przycisk **zapisać** przycisku.

    ![Skonfiguruj przycisk pojedynczego logowania jednokrotnego Zapisz](./media/active-directory-saas-saphana-tutorial/tutorial_general_400.png)
    
8. Do konfigurowania rejestracji jednokrotnej na **SAP HANA** strona, zaloguj się do sieci **konsoli sieci Web XSA HANA** przechodząc do odpowiednich końcowego HTTPS.

    > [!Note]
    > W domyślnej konfiguracji adres URL przekierowuje żądanie do strony logowania, co wymaga poświadczeń uwierzytelnionego użytkownika bazy danych SAP HANA do ukończenia procesu logowania. Użytkownik, który loguje się musi mieć uprawnienia wymagane do wykonywania zadań administracyjnych SAML.

9. W interfejsie sieci Web XSA, przejdź do **dostawca tożsamości SAML** i z tego miejsca, kliknij przycisk **"+"** — przycisk w dolnej części ekranu, aby wyświetlić okienko dodać informacje dostawcy tożsamości i wykonaj następujące czynności kroki:

    ![Dodaj dostawcę tożsamości](./media/active-directory-saas-saphana-tutorial/sap1.png)

    a. W **dodać informacje dostawcy tożsamości** okienku Wklej zawartość XML metadanych, który został pobrany z portalu Azure do **metadanych** pola tekstowego.

    ![Dodaj ustawienia dostawcy tożsamości](./media/active-directory-saas-saphana-tutorial/sap2.png)

    b. Jeśli zawartość dokumentu XML są prawidłowe, proces analizowania wyodrębnia informacje wymagane do wstawienia do **podmiotu, identyfikator jednostki i wystawcy** pola w obszarze ekranu ogólne danych i pola Adres URL na ekranie docelowego obszar, na przykład  **podstawowego adresu URL i adresu URL SingleSignOn (*)**.

    ![Dodaj ustawienia dostawcy tożsamości](./media/active-directory-saas-saphana-tutorial/sap3.png)

    c. W polu Nazwa obszaru danych ogólne ekranu wprowadź nazwę dla nowego logowania jednokrotnego SAML dostawcy tożsamości.

    > [!Note]
    > Nazwa SAML IDP jest wymagana i musi być unikatowa. wygląda na to, na liście dostępnych IDPs SAML, która jest wyświetlana, wtedy, gdy metoda uwierzytelniania SAP HANA XS do użycia przez aplikacje, na przykład w obszarze uwierzytelnianie ekranu narzędzia do administrowania artefaktu XS SAML.

10. Zapisz szczegóły nowego dostawcy tożsamości SAML. Wybierz **zapisać** zapisać szczegóły dostawca tożsamości SAML i dodać nowe IDP SAML do listy znanych IDPs SAML.

    ![Przyciskiem Zapisz](./media/active-directory-saas-saphana-tutorial/sap4.png)

11. W Studio HANA w ramach właściwości systemu **konfiguracji** karcie, po prostu filtrowania ustawień przez **saml** i dostosować **assertion_timeout** z **10 s**  do **120 s**.

    ![Ustawienie assertion_timeout](./media/active-directory-saas-saphana-tutorial/sap7.png)

> [!TIP]
> Teraz możesz przeczytać zwięzły wersji tych instrukcji wewnątrz [portalu Azure](https://portal.azure.com), podczas konfigurowania aplikacji!  Po dodaniu tej aplikacji z **usługi Active Directory > aplikacje dla przedsiębiorstw** po prostu kliknij **rejestracji jednokrotnej** karcie i dostęp do dokumentacji osadzonych za pomocą **konfiguracji** sekcji u dołu. Więcej o funkcji dokumentacji osadzonego w tym miejscu: [dokumentacji osadzonych usługi Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Tworzenie użytkownika testowego usługi Azure AD
Celem tej sekcji jest tworzenie użytkownika testowego w portalu Azure o nazwie Simona Britta.

![Tworzenie użytkowników usługi Azure AD][100]

**Aby utworzyć użytkownika testowego w usłudze Azure AD, wykonaj następujące czynności:**

1. W **portalu Azure**, w lewym okienku nawigacji, kliknij polecenie **usługi Azure Active Directory** ikony.

    ![Przycisk usługi Azure Active Directory](./media/active-directory-saas-saphana-tutorial/create_aaduser_01.png) 

2. Aby wyświetlić listę użytkowników, przejdź do **użytkowników i grup** i kliknij przycisk **wszyscy użytkownicy**.
    
    !["Użytkownicy i grupy" i "Wszyscy użytkownicy" łącza](./media/active-directory-saas-saphana-tutorial/create_aaduser_02.png) 

3. Aby otworzyć **użytkownika** okna dialogowego, kliknij przycisk **Dodaj** górnej części okna dialogowego.
 
    ![Przycisk Dodaj](./media/active-directory-saas-saphana-tutorial/create_aaduser_03.png) 

4. Na **użytkownika** okna dialogowego strony, należy wykonać następujące czynności:
 
    ![Okno dialogowe użytkownika](./media/active-directory-saas-saphana-tutorial/create_aaduser_04.png) 

    a. W **nazwa** pole tekstowe, typ **BrittaSimon**.

    b. W **nazwy użytkownika** pole tekstowe, typ **adres e-mail** z BrittaSimon.

    c. Wybierz **Pokaż hasło** i zanotuj wartość **hasło**.

    d. Kliknij przycisk **Utwórz**.
 
### <a name="creating-a-sap-hana-test-user"></a>Tworzenie użytkownika testowego SAP HANA

Aby umożliwić użytkownikom usługi Azure AD zalogować się do programu SAP HANA, muszą mieć przydzielone do programu SAP HANA.
SAP HANA obsługę w czasie, który jest domyślnie włączone.

Jeśli trzeba ręcznie utworzyć użytkownika, wykonaj następujące czynności:

>[!Note]
>Uwierzytelnianie zewnętrzne używane przez użytkownika, można zmienić.
Użytkownicy zewnętrzni są uwierzytelniani, za pomocą zewnętrznego systemu, na przykład system protokołu Kerberos. Aby uzyskać szczegółowe informacje o tożsamości zewnętrznych, skontaktuj się z [administrator domeny](https://cloudplatform.sap.com/contact.html).

1. Otwórz [SAP HANA Studio](https://help.sap.com/viewer/a2a49126a5c546a9864aae22c05c3d0e/2.0.01/en-us) jako administrator i Włącz użytkownika bazy danych dla logowania jednokrotnego SAML.

    ![Utwórz użytkownika](./media/active-directory-saas-saphana-tutorial/sap5.png)

2. Znaczników niewidoczne pola wyboru po lewej **SAML** i kliknij link Konfiguruj.

3. Kliknij przycisk **Dodaj** dodany SAML IDP i kliknij przycisk **OK** wybranie odpowiednich IDP SAML.

4. Dodaj **tożsamości zewnętrznych** (np. W tym miejscu BrittaSimon) lub wybierz **"Wszystkie"** i kliknij przycisk **OK**.

    >[!Note]
    >Jeśli nie zaznaczono pole "Wszystkie", a następnie nazwę użytkownika w HANA musi dokładnie odpowiadać nazwie użytkownika w głównej nazwy użytkownika przed sufiksem domeny (np. BrittaSimon@contoso.com staje się BrittaSimon w HANA).

5. Do celów testowych, przypisać wszystkie **"XS"** ról do użytkownika.

    ![Przypisywanie ról](./media/active-directory-saas-saphana-tutorial/sap6.png)

    > [!TIP]
    > Należy podać te uprawnienia, które są odpowiednie dla Twojej przypadki użycia tylko.

6. Zapisz użytkownika.

### <a name="assigning-the-azure-ad-test-user"></a>Przypisanie użytkownika testowego usługi Azure AD

W tej sekcji można włączyć Simona Britta do używania Azure logowania jednokrotnego za udzielanie dostępu do programu SAP HANA.

![Przypisanie roli użytkownika][200] 

**Aby przypisać Simona Britta SAP HANA, wykonaj następujące czynności:**

1. W portalu Azure Otwórz widok aplikacji, a następnie przejdź do widoku katalogu i przejdź do **aplikacje dla przedsiębiorstw** kliknięcie **wszystkie aplikacje**.

    ![Przypisz użytkownika][201] 

2. Na liście aplikacji zaznacz **SAP HANA**.

    ![Przypisz użytkownika](./media/active-directory-saas-saphana-tutorial/tutorial_saphana_app.png) 

3. W menu po lewej stronie kliknij **użytkowników i grup**.

    ![Łącze "Użytkownicy i grupy"][202] 

4. Kliknij przycisk **Dodaj** przycisku. Następnie wybierz **użytkowników i grup** na **Dodaj przydziału** okna dialogowego.

    ![W okienku Dodaj przydziału][203]

5. Na **użytkowników i grup** okno dialogowe, wybierz opcję **Simona Britta** na liście Użytkownicy.

6. Kliknij przycisk **wybierz** znajdującego się na **użytkowników i grup** okna dialogowego.

7. Kliknij przycisk **przypisać** znajdującego się na **Dodaj przydziału** okna dialogowego.
    
### <a name="testing-single-sign-on"></a>Testowanie rejestracji jednokrotnej

W tej sekcji można przetestować konfiguracji usługi Azure AD pojedynczego logowania za pomocą panelu dostępu.

Po kliknięciu kafelka SAP HANA w panelu dostępu użytkownik powinien pobrać automatycznie zalogowane do aplikacji SAP HANA.
Aby uzyskać więcej informacji na temat panelu dostępu, zobacz [wprowadzenie do panelu dostępu](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Lista samouczków dotyczących sposobów integracji aplikacji SaaS przy użyciu usługi Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co to jest dostęp do aplikacji i logowanie jednokrotne z usługą Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-saphana-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-saphana-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-saphana-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-saphana-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-saphana-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-saphana-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-saphana-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-saphana-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-saphana-tutorial/tutorial_general_203.png

