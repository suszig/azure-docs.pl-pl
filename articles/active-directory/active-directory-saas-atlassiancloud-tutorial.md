---
title: "Samouczek: Integracji Azure Active Directory z chmurą Atlassian | Dokumentacja firmy Microsoft"
description: "Informacje o sposobie konfigurowania rejestracji jednokrotnej między usługą Azure Active Directory i w chmurze Atlassian."
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 729b8eb6-efc4-47fb-9f34-8998ca2c9545
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/06/2017
ms.author: jeedes
ms.openlocfilehash: fff906913b637c47d394f9127983a08f96d568e4
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-atlassian-cloud"></a>Samouczek: Integracji Azure Active Directory z chmurą Atlassian

Z tego samouczka dowiesz się integrowanie Atlassian chmury w usłudze Azure Active Directory (Azure AD).

Integrowanie Atlassian chmurze z usługą Azure AD zapewnia następujące korzyści:

- Można kontrolować w usłudze Azure AD, który ma dostęp do chmury Atlassian.
- Umożliwia użytkownikom automatycznie pobrać zalogowane do chmury Atlassian (logowanie jednokrotne) z konta usługi Azure AD.
- Możesz zarządzać kont w jednej centralnej lokalizacji - portalu Azure.

Jeśli chcesz dowiedzieć się więcej informacji o integracji aplikacji SaaS w usłudze Azure AD, zobacz [co to jest dostęp do aplikacji i logowanie jednokrotne z usługą Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Wymagania wstępne

Aby skonfigurować integrację usługi Azure AD z chmurą Atlassian, potrzebne są następujące elementy:

- Subskrypcję usługi Azure AD
- Aby włączyć SAML rejestracji jednokrotnej dla chmury Atlassian produktów należy skonfigurować Identity Manager. Dowiedz się więcej o [Identity Manager]( https://www.atlassian.com/enterprise/cloud/identity-manager)

> [!NOTE]
> Aby przetestować kroki opisane w tym samouczku, zaleca się używania środowiska produkcyjnego.

Aby przetestować kroki opisane w tym samouczku, należy wykonać te zalecenia:

- Nie należy używać środowiska produkcyjnego, jeśli jest to konieczne.
- Jeśli nie masz środowisko wersji próbnej usługi Azure AD, możesz [uzyskać miesięczna wersja próbna](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Opis scenariusza
W tym samouczku można przetestować usługę Azure AD rejestracji jednokrotnej w środowisku testowym. Scenariusz opisany w tym samouczku składa się z dwóch głównych elementów:

1. Dodawanie Atlassian chmury z galerii
2. Konfigurowanie i testowanie usługi Azure AD logowanie jednokrotne

## <a name="adding-atlassian-cloud-from-the-gallery"></a>Dodawanie Atlassian chmury z galerii
Aby skonfigurować integrację usługi Azure AD Atlassian Cloud, należy dodać Atlassian chmury z galerii do listy zarządzanych aplikacji SaaS.

**Aby dodać Atlassian chmury z galerii, wykonaj następujące czynności:**

1. W  **[portalu Azure](https://portal.azure.com)**, na panelu nawigacyjnym po lewej stronie kliknij **usługi Azure Active Directory** ikony. 

    ![Przycisk usługi Azure Active Directory][1]

2. Przejdź do **aplikacje dla przedsiębiorstw**. Następnie przejdź do **wszystkie aplikacje**.

    ![Blok aplikacje przedsiębiorstwa][2]
    
3. Aby dodać nową aplikację, kliknij przycisk **nowej aplikacji** przycisk w górnej części okna dialogowego.

    ![Nowy przycisk aplikacji][3]

4. W polu wyszukiwania wpisz **chmury Atlassian**, wybierz pozycję **chmury Atlassian** z panelu wyników kliknięcie **Dodaj** przycisk, aby dodać aplikację.

    ![Chmura Atlassian na liście wyników](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfiguracja i testowanie usługi Azure AD rejestracji jednokrotnej

W tej sekcji możesz skonfigurować i test usługi Azure AD rejestracji jednokrotnej z chmurą Atlassian w oparciu o nazwie "Britta Simona" użytkownika testowego.

Dla rejestracji jednokrotnej do pracy usługi Azure AD musi wiedzieć, użytkownik odpowiednika w chmurze Atlassian jest dla użytkownika, w usłudze Azure AD. Innymi słowy link relację między użytkownikiem usługi Azure AD i danemu użytkownikowi w chmurze Atlassian musi się.

W chmurze Atlassian przypisać wartość **nazwy użytkownika** w usłudze Azure AD jako wartość **Username** do ustanawiania relacji łącza.

Aby skonfigurować i przetestować usługi Azure AD rejestracji jednokrotnej z chmurą Atlassian, należy wykonać poniższe bloki konstrukcyjne:

1. **[Konfigurowanie usługi Azure AD rejestracji jednokrotnej](#configure-azure-ad-single-sign-on)**  — aby umożliwić użytkownikom korzystać z tej funkcji.
2. **[Tworzenie użytkownika testowego usługi Azure AD](#create-an-azure-ad-test-user)**  — do przetestowania usługi Azure AD rejestracji jednokrotnej z Simona Britta.
3. **[Tworzenie użytkownika testowego chmury Atlassian](#create-an-atlassian-cloud-test-user)**  — aby odpowiednikiem Simona Britta w chmurze Atlassian, która jest połączona z usługi Azure AD reprezentację użytkownika.
4. **[Przypisz użytkownika testowego usługi Azure AD](#assign-the-azure-ad-test-user)**  — aby umożliwić Simona Britta do użycia usługi Azure AD rejestracji jednokrotnej.
5. **[Test rejestracji jednokrotnej](#test-single-sign-on)**  — Aby sprawdzić, czy konfiguracja działa.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurowanie usługi Azure AD rejestracji jednokrotnej

W tej sekcji można włączyć usługi Azure AD rejestracji jednokrotnej w portalu Azure i skonfigurować logowanie jednokrotne do aplikacji w chmurze Atlassian.

**Aby skonfigurować usługi Azure AD rejestracji jednokrotnej z chmurą Atlassian, wykonaj następujące czynności:**

1. W portalu Azure na **chmury Atlassian** strona integracji aplikacji, kliknij przycisk **logowanie jednokrotne**.

    ![Skonfigurować łącze rejestracji jednokrotnej][4]

2. Na **logowanie jednokrotne** okno dialogowe, wybierz opcję **tryb** jako **na języku SAML logowania jednokrotnego** Aby włączyć logowanie jednokrotne.
 
    ![Okno dialogowe rejestracji jednokrotnej](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_samlbase.png)

3. Na **adresy URL i domenę chmury Atlassian** sekcji, wykonaj następujące kroki, aby skonfigurować aplikację w **IDP** inicjowane tryb:

    ![Adresy URL i domenę chmury Atlassian pojedynczy informacje logowania jednokrotnego](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_url_new.png)
    
    a. W **identyfikator** tekstowym, wpisz adres URL:`https://id.atlassian.com/login`
    
    b. W **adres URL odpowiedzi** tekstowym, wpisz adres URL:`https://id.atlassian.com/login/saml/acs`

    c. W **stan przekazywania** tekstowym, wpisz adres URL, używając następującego wzorca:`https://<instancename>.atlassian.net`

4. Sprawdź **Pokaż zaawansowane ustawienia adresu URL** i wykonać następujący krok, jeśli chcesz skonfigurować aplikację w **SP** inicjowane tryb:

    ![Adresy URL i domenę chmury Atlassian pojedynczy informacje logowania jednokrotnego](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_url1.png)

    W **pole tekstowe adresu URL logowania jednokrotnego**, wpisz adres URL przy użyciu następującego wzorca:`https://<instancename>.atlassian.net`

    > [!NOTE] 
    > Wartości te nie są prawdziwe. Te wartości można uzyskać z ekranu konfiguracji SAML chmury Atlassian, który znajduje się w dalszej części tego samouczka.

5. Na **certyfikat podpisywania SAML** kliknij **Certificate(Base64)** , a następnie zapisz plik certyfikatu na tym komputerze.

    ![Łącze pobierania certyfikatu](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_certificate.png) 

6. Kliknij przycisk **zapisać** przycisku.

    ![Skonfiguruj przycisk pojedynczego logowania jednokrotnego Zapisz](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_400.png)

7. Na **konfiguracji chmury Atlassian** , kliknij przycisk **Konfigurowanie chmury Atlassian** otworzyć **Konfigurowanie logowania jednokrotnego** okna. Kopiuj **identyfikator jednostki SAML i SAML pojedynczy znak na adres URL usługi** z **sekcji krótkimi opisami.**

    ![Konfiguracja chmury Atlassian](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_configure.png) 

8. Aby uzyskać logowania jednokrotnego skonfigurowane dla danej aplikacji, należy zalogować się do portalu Atlassian przy użyciu uprawnień administratora.

9. Przejdź do **administrowania lokacją Atlassian** > **organizacji i zabezpieczenia**. Jeśli nie jest jeszcze, tworzenie i nazwę organizacji. Następnie w obszarze nawigacji po lewej stronie, kliknij przycisk **domen**.

    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_06.png)

10. Wybierz sposób, że chcesz zweryfikować domenę - **DNS** lub **HTTPS**.

    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_17.png)

11. W celu weryfikacji DNS wybierz **DNS** karcie na **domen** strony i wykonaj kroki Oto:

    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_18.png)

    a. Kliknij przycisk **kopiowania** skopiowanie wartości dla rekordu TXT.

    b. W systemie DNS należy znaleźć strona ustawień dla dodawania nowego rekordu

    c. Wybierz opcję dodawania nowego rekordu i Wklej skopiowany z wartość **domen** strony **wartość** pola. Serwery DNS może także odwołuje się do niego jako **odpowiedzi** lub **opis**.

    d. Rekordu DNS może także zawierać następujące pola:
    
    * **Typ rekordu**: wprowadź **TXT**
    * **Alias-nazwy/hosta**: pozostaw wartość domyślną (@ lub puste)
    * **Czas wygaśnięcia (TTL)**: wprowadź **86400**
    
    e.  Zapisz rekord.

12. Wróć do **domeny** Administracja organizacji, a następnie kliknij polecenie **weryfikowanie domeny** przycisku. Wpisz nazwę domeny w oknie podręcznym i kliknij **weryfikowanie domeny** przycisku.

    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_19.png)
    
    > [!NOTE]
    > Może potrwać do 72 godzin rekordu TXT zmiany zaczęły obowiązywać, aby dowiedzieć się od razu czy weryfikację domeny zakończyła się pomyślnie. Sprawdź Twojej **domen** stronie zaraz po wykonaj te kroki dla stan weryfikacji. Zostanie wyświetlony następujący ekran z zaktualizowany stan jako **ZWERYFIKOWANO**.

    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_20.png)

13. W celu weryfikacji HTTPS wybierz **HTTPS** karcie na **domen** strony i wykonać następujące kroki:

    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_21.png)

    a.  Kliknij przycisk **pobierania pliku** można pobrać pliku HTML.

    b.  Przekaż plik HTML do katalogu głównym domeny.

14. Wróć do **domen** w organizacji administracji, a następnie kliknij przycisk **weryfikowanie domeny** przycisku. Wprowadź użytkownika **nazwy domeny** w oknie podręcznym i kliknij **weryfikowanie domeny** przycisku.

    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_22.png)

15. Jeśli proces weryfikacji można zlokalizować pliku zostało przez Ciebie przekazane w katalogu głównym, stan domeny aktualizacje **zweryfikowano**.

    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_23.png)

    > [!NOTE]
    > Aby uzyskać więcej informacji można znaleźć [Atlassian w domenie weryfikacji dokumentacji](https://confluence.atlassian.com/cloud/domain-verification-873871234.html)

16. Na pasku nawigacyjnym po lewej stronie kliknij **SAML logowania jednokrotnego**. Jeśli nie jest jeszcze subskrypcji do jego Atlassian Identity Manager.

    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_11.png)

17. W **Konfiguracja SAML dodać** okno dialogowe Dodaj ustawienia dostawcy tożsamości, w następujący sposób:

    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_12.png)

    a. W **dostawcy tożsamości identyfikator jednostki** tekst Wklej wartość **identyfikator jednostki SAML** którego została skopiowana z portalu Azure.

    b. W **dostawcy tożsamości adresu URL logowania jednokrotnego** tekst Wklej wartość **SAML pojedynczy znak na adres URL usługi** którego została skopiowana z portalu Azure.

    c. Otwórz pobranego certyfikatu w programie Notatnik i skopiuj wartości bez rozpoczęcia i zakończenia wierszy i wklej go w **X509 publicznego certyfikatu** pole.
    
    d. Kliknij przycisk **Zapisz konfigurację** Aby zapisać ustawienia.
     
18. Zaktualizuj ustawienia usługi Azure AD, aby upewnić się, że Instalator prawidłowe adresy URL.
  
    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_13.png)

    a. Kopiuj **Identyfikatora tożsamości SP** z SAML ekranu i Wklej wartość w **identyfikator** pole w portalu Azure w chmurze Atlassian **domeny i adres URL** sekcji.
    
    b. Kopiuj **adres URL programu SP potwierdzenia klienta usługi** z SAML ekranu i Wklej wartość w **adres URL odpowiedzi służący** pole w portalu Azure w chmurze Atlassian **domeny i adres URL** sekcja.
    
    c. Zaloguj się na adres URL jest adresem URL dzierżawy Atlassian chmury. 
    
19. W portalu Azure kliknij **zapisać** przycisku.

    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_400.png)

> [!TIP]
> Teraz możesz przeczytać zwięzły wersji tych instrukcji wewnątrz [portalu Azure](https://portal.azure.com), podczas konfigurowania aplikacji!  Po dodaniu tej aplikacji z **usługi Active Directory > aplikacje dla przedsiębiorstw** po prostu kliknij **rejestracji jednokrotnej** karcie i dostęp do dokumentacji osadzonych za pomocą **konfiguracji** sekcji u dołu. Więcej o funkcji dokumentacji osadzonego w tym miejscu: [dokumentacji osadzonych usługi Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Tworzenie użytkownika testowego usługi Azure AD

Celem tej sekcji jest tworzenie użytkownika testowego w portalu Azure o nazwie Simona Britta.

   ![Tworzenie użytkownika testowego usługi Azure AD][100]

**Aby utworzyć użytkownika testowego w usłudze Azure AD, wykonaj następujące czynności:**

1. W portalu Azure, w okienku po lewej stronie kliknij **usługi Azure Active Directory** przycisku.

    ![Przycisk usługi Azure Active Directory](./media/active-directory-saas-atlassian-cloud-tutorial/create_aaduser_01.png)

2. Aby wyświetlić listę użytkowników, przejdź do **użytkowników i grup**, a następnie kliknij przycisk **wszyscy użytkownicy**.

    !["Użytkownicy i grupy" i "Wszyscy użytkownicy" łącza](./media/active-directory-saas-atlassian-cloud-tutorial/create_aaduser_02.png)

3. Aby otworzyć **użytkownika** okno dialogowe, kliknij przycisk **Dodaj** w górnej części **wszyscy użytkownicy** okno dialogowe.

    ![Przycisk Dodaj](./media/active-directory-saas-atlassian-cloud-tutorial/create_aaduser_03.png)

4. W **użytkownika** okna dialogowego wykonaj następujące czynności:

    ![Okno dialogowe użytkownika](./media/active-directory-saas-atlassian-cloud-tutorial/create_aaduser_04.png)

    a. W **nazwa** wpisz **BrittaSimon**.

    b. W **nazwy użytkownika** wpisz adres e-mail użytkownika Simona Britta.

    c. Wybierz **Pokaż hasło** pole wyboru, a następnie zanotuj wartość, która jest wyświetlana w **hasło** pole.

    d. Kliknij przycisk **Utwórz**.
  
### <a name="create-an-atlassian-cloud-test-user"></a>Tworzenie użytkownika testowego Atlassian chmury

Aby umożliwić użytkownikom usługi Azure AD do logowania się w chmurze Atlassian, muszą mieć przydzielone do Atlassian chmury. W przypadku Atlassian chmury Inicjowanie obsługi to zadanie ręczne.

**Aby udostępnić konta użytkownika, wykonaj następujące czynności:**

1. W sekcji Administracja witryny portalu Atlassian kliknij **użytkowników** przycisku

    ![Utwórz użytkownika Atlassian chmury](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_14.png) 

2. Kliknij przycisk **użytkownika zaproszenia** przycisk, aby utworzyć użytkownika w chmurze Atlassian.

    ![Utwórz użytkownika Atlassian chmury](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_15.png) 

3. Wprowadź nazwę danego użytkownika **adres E-mail** i przypisz dostęp do aplikacji. 

    ![Utwórz użytkownika Atlassian chmury](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_16.png)
 
4. Kliknij przycisk **zaprosić użytkowników** przycisku będzie wysyłać wiadomości e-mail z zaproszeniem dla użytkownika i po zaakceptowaniu zaproszenia użytkownik będzie aktywny w systemie. 

>[!NOTE] 
>Zbiorcze użytkowników można również tworzyć przez kliknięcie **Tworzenie zbiorczego** przycisk w sekcji użytkownicy.

### <a name="assign-the-azure-ad-test-user"></a>Przypisz użytkownika testowego usługi Azure AD

W tej sekcji musisz włączyć Simona Britta do użycia usługi Azure logowania jednokrotnego za udzielanie dostępu do chmury Atlassian.

![Przypisanie roli użytkownika][200] 

**Aby przypisać Simona Britta Atlassian chmury, wykonaj następujące czynności:**

1. W portalu Azure Otwórz widok aplikacji, a następnie przejdź do widoku katalogu i przejdź do **aplikacje dla przedsiębiorstw** kliknięcie **wszystkie aplikacje**.

    ![Przypisz użytkownika][201] 

2. Na liście aplikacji zaznacz **chmury Atlassian**.

    ![Łącze Atlassian chmury na liście aplikacji](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_app.png)  

3. W menu po lewej stronie kliknij **użytkowników i grup**.

    ![Łącze "Użytkownicy i grupy"][202]

4. Kliknij przycisk **Dodaj** przycisku. Następnie wybierz **użytkowników i grup** na **Dodaj przydziału** okna dialogowego.

    ![W okienku Dodaj przydziału][203]

5. Na **użytkowników i grup** okno dialogowe, wybierz opcję **Simona Britta** na liście Użytkownicy.

6. Kliknij przycisk **wybierz** znajdującego się na **użytkowników i grup** okna dialogowego.

7. Kliknij przycisk **przypisać** znajdującego się na **Dodaj przydziału** okna dialogowego.
    
### <a name="test-single-sign-on"></a>Test rejestracji jednokrotnej

W tej sekcji można przetestować konfiguracji usługi Azure AD pojedynczego logowania za pomocą panelu dostępu.

Po kliknięciu kafelka Atlassian chmury w panelu dostępu użytkownik powinien pobrać automatycznie zalogowane do aplikacji w chmurze Atlassian.
Aby uzyskać więcej informacji na temat panelu dostępu, zobacz [wprowadzenie do panelu dostępu](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Lista samouczków dotyczących sposobów integracji aplikacji SaaS przy użyciu usługi Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co to jest dostęp do aplikacji i logowanie jednokrotne z usługą Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_203.png

