---
title: "Samouczek: Integracji Azure Active Directory z uwierzytelnianie tożsamości platformy chmury SAP | Dokumentacja firmy Microsoft"
description: "Informacje o sposobie konfigurowania rejestracji jednokrotnej między usługą Azure Active Directory i uwierzytelniania tożsamości platformy SAP w chmurze."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 1c1320d1-7ba4-4b5f-926f-4996b44d9b5e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/20/2017
ms.author: jeedes
ms.openlocfilehash: e4e7c7273acc216ae82a52b3e6dcd530a6ad1bd7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sap-cloud-platform-identity-authentication"></a>Samouczek: Integracji Azure Active Directory z uwierzytelniania tożsamości platformy SAP w chmurze

Z tego samouczka dowiesz się integrowanie uwierzytelniania tożsamości platformy SAP w chmurze z usługą Azure Active Directory (Azure AD). Uwierzytelnianie tożsamości platformy chmury SAP jest używany jako serwer proxy IdP dostępu do programu SAP aplikacji przy użyciu usługi Azure AD jako główny dostawca tożsamości.

Integrowanie uwierzytelniania tożsamości platformy SAP w chmurze z usługą Azure AD zapewnia następujące korzyści:

- Można kontrolować w usłudze Azure AD, kto ma dostęp do aplikacji SAP.
- Użytkowników, aby automatycznie pobrać zalogowane do SAP aplikacji rejestracji jednokrotnej (SSO) można włączyć) przy użyciu ich kont usługi Azure AD.
- Możesz zarządzać kont w jednej centralnej lokalizacji - portalu Azure.

Jeśli chcesz dowiedzieć się więcej informacji o integracji aplikacji SaaS w usłudze Azure AD, zobacz [co to jest dostęp do aplikacji i logowanie jednokrotne z usługą Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Wymagania wstępne

Aby skonfigurować integrację usługi Azure AD z uwierzytelnianiem tożsamość platformy chmury SAP, potrzebne są następujące elementy:

- Subskrypcję usługi Azure AD
- Uwierzytelnianie tożsamości platformy chmury SAP logowanie jednokrotne włączone subskrypcji

> [!NOTE]
> Aby przetestować kroki opisane w tym samouczku, zaleca się używania środowiska produkcyjnego.

Aby przetestować kroki opisane w tym samouczku, należy wykonać te zalecenia:

- Nie należy używać środowiska produkcyjnego, jeśli jest to konieczne.
- Jeśli nie masz środowisko wersji próbnej usługi Azure AD, możesz [uzyskać miesięczna wersja próbna](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Opis scenariusza
W tym samouczku można przetestować usługę Azure AD rejestracji jednokrotnej w środowisku testowym. Scenariusz opisany w tym samouczku składa się z dwóch głównych elementów:

1. Dodawanie uwierzytelniania tożsamości platformy SAP w chmurze z galerii
2. Konfigurowanie i testowanie usługi Azure AD logowanie jednokrotne

Przed rozpoczęciem pracy szczegółowe informacje techniczne, ważne jest zrozumienie pojęcia, które zamierzasz przyjrzeć się. Uwierzytelnianie tożsamości platformy chmury SAP i Azure Active Directory federation umożliwia Implementowanie logowania jednokrotnego w aplikacji lub usług chronione przez usługi AAD (jako IdP) przy użyciu SAP aplikacji i usług, chroniony przez tożsamość platformy w chmurze SAP Uwierzytelnianie.

Obecnie uwierzytelnianie tożsamości platformy chmury SAP działa jako dostawca tożsamości serwera Proxy, aby aplikacje SAP. Usługi Azure Active Directory z kolei pełni rolę początku dostawcy tożsamości w tej instalacji. 

Na poniższym diagramie przedstawiono to:

![Tworzenie użytkownika testowego usługi Azure AD](./media/active-directory-saas-sapcloudauth-tutorial/architecture-01.png)

W przypadku tej konfiguracji dzierżawy uwierzytelnianie tożsamości platformy chmury SAP zostanie skonfigurowany jako zaufanych aplikacji w usłudze Azure Active Directory. 

Wszystkie SAP aplikacji i usług, które chcesz chronić za pomocą w ten sposób następnie są konfigurowane w konsoli zarządzania programu SAP Cloud Platform tożsamość uwierzytelniania!

Oznacza to, że autoryzacji za udzielanie dostępu do usługi i aplikacje SAP musi odbywać się w SAP Cloud Platform tożsamość uwierzytelniania dla instalacji (w przeciwieństwie do konfigurowania autoryzacji w usłudze Azure Active Directory).

Konfigurując uwierzytelnianie tożsamości platformy chmury SAP jako aplikację za pomocą usługi Azure Active Directory Marketplace, nie trzeba zajmie się konfigurowanie wymagane oddzielne oświadczenia / potwierdzenia języka SAML i przekształcenia potrzebnych do uzyskania prawidłowej token uwierzytelniania dla aplikacji SAP.

>[!NOTE] 
>Obecnie obie strony tylko przetestowano Jednokrotną w sieci Web. Przepływy potrzebne w przypadku aplikacji do interfejsu API lub interfejsu API do interfejsu API komunikacji powinny działać, ale nie zostały one przetestowane, jeszcze. Zostaną one przetestowane w ramach kolejnych działań.
>

## <a name="adding-sap-cloud-platform-identity-authentication-from-the-gallery"></a>Dodawanie uwierzytelniania tożsamości platformy SAP w chmurze z galerii
Aby skonfigurować integrację SAP Cloud Platform tożsamość uwierzytelniania w usłudze Azure Active Directory, należy dodać uwierzytelnianie tożsamości platformy chmury SAP z galerii do listy zarządzanych aplikacji SaaS.

**Aby dodać uwierzytelnianie tożsamości platformy chmury SAP z galerii, wykonaj następujące czynności:**

1. W  **[portalu Azure](https://portal.azure.com)**, na panelu nawigacyjnym po lewej stronie kliknij **usługi Azure Active Directory** ikony. 

    ![Przycisk usługi Azure Active Directory][1]

2. Przejdź do **aplikacje dla przedsiębiorstw**. Następnie przejdź do **wszystkie aplikacje**.

    ![Blok aplikacje przedsiębiorstwa][2]
    
3. Aby dodać nową aplikację, kliknij przycisk **nowej aplikacji** przycisk w górnej części okna dialogowego.

    ![Nowy przycisk aplikacji][3]

4. W polu wyszukiwania wpisz **uwierzytelnianie tożsamości platformy chmury SAP**, wybierz pozycję **uwierzytelnianie tożsamości platformy chmury SAP** z panelu wyników następnie kliknij przycisk **Dodaj** przycisk, aby Dodaj aplikację.

    ![Uwierzytelnianie tożsamości platformy chmury SAP na liście wyników](./media/active-directory-saas-sapcloudauth-tutorial/tutorial_sapcpia_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfiguracja i testowanie usługi Azure AD rejestracji jednokrotnej

W tej sekcji skonfigurować i przetestować usługi Azure AD rejestracji jednokrotnej z SAP platformy tożsamość uwierzytelniania w chmurze w oparciu o nazwie "Britta Simona" użytkownika testowego.

Do rejestracji jednokrotnej do pracy usługi Azure AD musi ustalić użytkownika odpowiednika w uwierzytelnianie tożsamości platformy chmury SAP do użytkownika w usłudze Azure AD. Innymi słowy relację łącza między użytkownika usługi Azure AD i danemu użytkownikowi w uwierzytelnianie tożsamości platformy chmury SAP musi określone.

Uwierzytelnianie tożsamości platformy w chmurze SAP, przypisywanie wartości **nazwy użytkownika** w usłudze Azure AD jako wartość **Username** do ustanawiania relacji łącza.

Aby skonfigurować i przetestować usługi Azure AD rejestracji jednokrotnej z uwierzytelnianiem tożsamość platformy chmury SAP, należy wykonać poniższe bloki konstrukcyjne:

1. **[Konfigurowanie usługi Azure AD rejestracji jednokrotnej](#configure-azure-ad-single-sign-on)**  — aby umożliwić użytkownikom korzystać z tej funkcji.
2. **[Tworzenie użytkownika testowego usługi Azure AD](#create-an-azure-ad-test-user)**  — do przetestowania usługi Azure AD rejestracji jednokrotnej z Simona Britta.
3. **[Tworzenie użytkownika testowego uwierzytelnianie tożsamości platformy chmury SAP](#create-an-sap-cloud-platform-identity-authentication-test-user)**  — w celu zapewnienia odpowiednikiem Simona Britta SAP platformy tożsamość uwierzytelniania w chmurze połączonego z usługi Azure AD reprezentację użytkownika.
4. **[Przypisz użytkownika testowego usługi Azure AD](#assign-the-azure-ad-test-user)**  — aby umożliwić Simona Britta do użycia usługi Azure AD rejestracji jednokrotnej.
5. **[Test rejestracji jednokrotnej](#test-single-sign-on)**  — Aby sprawdzić, czy konfiguracja działa.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurowanie usługi Azure AD rejestracji jednokrotnej

W tej sekcji włączyć usługi Azure AD rejestracji jednokrotnej w portalu Azure i skonfigurować logowanie jednokrotne do aplikacji uwierzytelnianie tożsamości platformy chmury SAP.

**Aby skonfigurować usługi Azure AD rejestracji jednokrotnej uwierzytelnianie tożsamości platformy chmury SAP, wykonaj następujące czynności:**

1. W portalu Azure na **uwierzytelnianie tożsamości platformy chmury SAP** strona integracji aplikacji, kliknij przycisk **logowanie jednokrotne**.

    ![Skonfigurować łącze rejestracji jednokrotnej][4]

2. Na **logowanie jednokrotne** okno dialogowe, wybierz opcję **tryb** jako **na języku SAML logowania jednokrotnego** Aby włączyć logowanie jednokrotne.
 
    ![Okno dialogowe rejestracji jednokrotnej](./media/active-directory-saas-sapcloudauth-tutorial/tutorial_sapcpia_samlbase.png)

3. Na **domeny uwierzytelniania tożsamości SAP Cloud Platform i adres URL** sekcji, jeśli chcesz skonfigurować aplikację w **IDP** inicjowane tryb:

    ![Adresy URL i SAP Cloud Platform tożsamości uwierzytelniania domeny pojedynczy informacje logowania jednokrotnego](./media/active-directory-saas-sapcloudauth-tutorial/tutorial_sapcpia_url.png)

    W **identyfikator** tekstowym, wpisz adres URL, używając następującego wzorca:`https://<entity-id>.accounts.ondemand.com`

    > [!NOTE] 
    > Ta wartość nie jest prawdziwe. Zaktualizuj tę wartość z rzeczywistego identyfikatora. Skontaktuj się z [zespołem pomocy technicznej SAP Cloud Platform tożsamość uwierzytelniania klient](https://cloudplatform.sap.com/capabilities/security/trustcenter.html) aby zyskać tę wartość. Jeśli nie znasz tej wartości, wykonaj dokumentacji uwierzytelnianie tożsamości platformy chmury SAP [dzierżawy SAML 2.0 konfiguracji](https://help.hana.ondemand.com/cloud_identity/frameset.htm?e81a19b0067f4646982d7200a8dab3ca.html).

4. Sprawdź **Pokaż zaawansowane ustawienia adresu URL**. Jeśli chcesz skonfigurować aplikację w **SP** inicjowane tryb:

    ![Adresy URL i SAP Cloud Platform tożsamości uwierzytelniania domeny pojedynczy informacje logowania jednokrotnego](./media/active-directory-saas-sapcloudauth-tutorial/tutorial_sapcpia_url1.png)

    W **na adres URL logowania** tekstowym, wpisz adres URL, używając następującego wzorca:`https://<entity-id>.accounts.ondemand.com/admin`

    > [!NOTE] 
    > Ta wartość nie jest prawdziwe. Zaktualizuj tę wartość przy rzeczywisty adres URL logowania. Skontaktuj się z [zespołem pomocy technicznej SAP Cloud Platform tożsamość uwierzytelniania klient](https://cloudplatform.sap.com/capabilities/security/trustcenter.html) aby zyskać tę wartość.

5. Na **certyfikat podpisywania SAML** kliknij **XML metadanych** , a następnie zapisz plik metadanych na tym komputerze.

    ![Łącze pobierania certyfikatu](./media/active-directory-saas-sapcloudauth-tutorial/tutorial_sapcpia_certificate.png)

6. Aplikacja uwierzytelniania tożsamości w chmurze platformy SAP oczekuje potwierdzenia języka SAML w określonym formacie. Możesz zarządzać wartości tych atrybutów z "**atrybuty użytkownika**" sekcji na stronie integracji aplikacji. Poniższy zrzut ekranu przedstawia przykład tego. 

    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-sapcloudauth-tutorial/attribute.png)

7. W **atrybuty użytkownika** sekcji na **logowanie jednokrotne** okna dialogowego, jeśli aplikacja SAP oczekuje na przykład "imię" atrybutu. W oknie dialogowym atrybuty tokenu SAML Dodaj atrybut "imię".

    a. Kliknij przycisk **Dodaj atrybut** otworzyć **Dodawanie atrybutu** okna dialogowego.
    
    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-sapcloudauth-tutorial/tutorial_attribute_04.png)
    
    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-sapcloudauth-tutorial/tutorial_attribute_05.png)
    
    b. W **nazwa** tekstowym, wpisz nazwę atrybutu "imię".
    
    c. Z **wartość** , wybierz wartość atrybutu "user.givenname" na liście.
    
    d. Kliknij przycisk **OK**.

8. Kliknij przycisk **zapisać** przycisku.

    ![Skonfiguruj przycisk pojedynczego logowania jednokrotnego Zapisz](./media/active-directory-saas-sapcloudauth-tutorial/tutorial_general_400.png)

9. Na **konfiguracji uwierzytelniania tożsamości SAP Cloud Platform** , kliknij przycisk **skonfigurować uwierzytelnianie tożsamości platformy chmury SAP** otworzyć **Konfigurowanie logowania jednokrotnego** okno. Kopiuj **Sign-Out adres URL, identyfikator jednostki SAML i SAML pojedynczy znak na adres URL usługi** z **sekcji krótkimi opisami.**

    ![Konfiguracja uwierzytelniania tożsamości platformy chmury SAP](./media/active-directory-saas-sapcloudauth-tutorial/tutorial_sapcpia_configure.png) 

10. Aby uzyskać logowania jednokrotnego skonfigurowane dla aplikacji, przejdź do konsoli administracyjnej uwierzytelniania tożsamości SAP Cloud Platform. Adres URL ma następujący wzór: `https://<tenant-id>.accounts.ondemand.com/admin`. Następnie postępuj zgodnie z dokumentacją na uwierzytelnianie tożsamości platformy chmury SAP do [Konfigurowanie usługi Microsoft Azure AD jako firmowe dostawcy tożsamości na uwierzytelnianie tożsamości platformy chmury SAP](https://help.hana.ondemand.com/cloud_identity/frameset.htm?626b17331b4d4014b8790d3aea70b240.html). 

11. W portalu Azure kliknij **zapisać** przycisku.

12. Poniższe kroki należy kontynuować tylko, jeśli chcesz dodać do włączenia funkcji logowania jednokrotnego dla innej aplikacji SAP. Powtórz kroki opisane w sekcji "Dodawanie SAP platformy tożsamość uwierzytelniania w chmurze z galerii" Aby dodać inne wystąpienie programu SAP Cloud Platform tożsamość uwierzytelniania.

13. W portalu Azure na **uwierzytelnianie tożsamości platformy chmury SAP** strona integracji aplikacji, kliknij przycisk **połączonej logowania jednokrotnego**.

    ![Skonfiguruj połączonego logowania jednokrotnego](./media/active-directory-saas-sapcloudauth-tutorial/linked_sign_on.png)

14. Zapisz konfigurację.

>[!NOTE] 
>Nowa aplikacja będzie korzystać z logowania jednokrotnego konfiguracji poprzedniej aplikacji SAP. Upewnij się, korzystanie z tej samej firmowej dostawców tożsamości w konsoli administracyjnej programu SAP chmury platformy tożsamość uwierzytelniania.

> [!TIP]
> Teraz możesz przeczytać zwięzły wersji tych instrukcji wewnątrz [portalu Azure](https://portal.azure.com), podczas konfigurowania aplikacji!  Po dodaniu tej aplikacji z **usługi Active Directory > aplikacje dla przedsiębiorstw** po prostu kliknij **rejestracji jednokrotnej** karcie i dostęp do dokumentacji osadzonych za pomocą **konfiguracji** sekcji u dołu. Więcej o funkcji dokumentacji osadzonego w tym miejscu: [dokumentacji osadzonych usługi Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Tworzenie użytkownika testowego usługi Azure AD

Celem tej sekcji jest tworzenie użytkownika testowego w portalu Azure o nazwie Simona Britta.

   ![Tworzenie użytkownika testowego usługi Azure AD][100]

**Aby utworzyć użytkownika testowego w usłudze Azure AD, wykonaj następujące czynności:**

1. W portalu Azure, w okienku po lewej stronie kliknij **usługi Azure Active Directory** przycisku.

    ![Przycisk usługi Azure Active Directory](./media/active-directory-saas-sapcloudauth-tutorial/create_aaduser_01.png)

2. Aby wyświetlić listę użytkowników, przejdź do **użytkowników i grup**, a następnie kliknij przycisk **wszyscy użytkownicy**.

    !["Użytkownicy i grupy" i "Wszyscy użytkownicy" łącza](./media/active-directory-saas-sapcloudauth-tutorial/create_aaduser_02.png)

3. Aby otworzyć **użytkownika** okno dialogowe, kliknij przycisk **Dodaj** w górnej części **wszyscy użytkownicy** okno dialogowe.

    ![Przycisk Dodaj](./media/active-directory-saas-sapcloudauth-tutorial/create_aaduser_03.png)

4. W **użytkownika** okna dialogowego wykonaj następujące czynności:

    ![Okno dialogowe użytkownika](./media/active-directory-saas-sapcloudauth-tutorial/create_aaduser_04.png)

    a. W **nazwa** wpisz **BrittaSimon**.

    b. W **nazwy użytkownika** wpisz adres e-mail użytkownika Simona Britta.

    c. Wybierz **Pokaż hasło** pole wyboru, a następnie zanotuj wartość, która jest wyświetlana w **hasło** pole.

    d. Kliknij przycisk **Utwórz**.
 
### <a name="create-an-sap-cloud-platform-identity-authentication-test-user"></a>Tworzenie użytkownika testowego uwierzytelniania tożsamości platformy SAP w chmurze

Nie trzeba utworzyć użytkownika na uwierzytelnianie tożsamości platformy chmury SAP. Użytkownicy, którzy znajdują się w magazynie usługi Azure AD użytkownika może używać funkcji logowania jednokrotnego.

Uwierzytelnianie tożsamości platformy chmury SAP obsługuje opcję federacji tożsamości. Ta opcja umożliwia aplikacji sprawdzić, czy użytkownicy uwierzytelnieni przez dostawcę tożsamości firmy istnieją w użytkownika magazynu programu SAP platformy tożsamość uwierzytelniania w chmurze. 

W ustawieniu domyślny opcja federacji tożsamości jest wyłączona. Włączenie federacji tożsamości tylko użytkowników, którzy są importowane w SAP Cloud Platform tożsamość uwierzytelniania są w stanie uzyskać dostęp do aplikacji. 

Aby uzyskać więcej informacji o tym, jak włączyć lub wyłączyć federacji tożsamości z uwierzytelniania tożsamości platformy SAP w chmurze, zobacz włączyć Federację tożsamości z uwierzytelnianie tożsamości SAP Cloud Platform w [Konfigurowanie federacji tożsamości z Uwierzytelnianie tożsamości użytkownika magazynu SAP chmury platformy](https://help.hana.ondemand.com/cloud_identity/frameset.htm?c029bbbaefbf4350af15115396ba14e2.html).

### <a name="assign-the-azure-ad-test-user"></a>Przypisz użytkownika testowego usługi Azure AD

W tej sekcji można włączyć Simona Britta do używania Azure logowania jednokrotnego za udzielanie dostępu do uwierzytelniania tożsamości platformy SAP w chmurze.

![Przypisanie roli użytkownika][200] 

**Aby przypisać Simona Britta do uwierzytelniania tożsamości platformy SAP w chmurze, wykonaj następujące czynności:**

1. W portalu Azure Otwórz widok aplikacji, a następnie przejdź do widoku katalogu i przejdź do **aplikacje dla przedsiębiorstw** kliknięcie **wszystkie aplikacje**.

    ![Przypisz użytkownika][201] 

2. Na liście aplikacji zaznacz **uwierzytelnianie tożsamości platformy chmury SAP**.

    ![Uwierzytelnianie tożsamości platformy chmury SAP łącze na liście aplikacji](./media/active-directory-saas-sapcloudauth-tutorial/tutorial_sapcpia_app.png)  

3. W menu po lewej stronie kliknij **użytkowników i grup**.

    ![Łącze "Użytkownicy i grupy"][202]

4. Kliknij przycisk **Dodaj** przycisku. Następnie wybierz **użytkowników i grup** na **Dodaj przydziału** okna dialogowego.

    ![W okienku Dodaj przydziału][203]

5. Na **użytkowników i grup** okno dialogowe, wybierz opcję **Simona Britta** na liście Użytkownicy.

6. Kliknij przycisk **wybierz** znajdującego się na **użytkowników i grup** okna dialogowego.

7. Kliknij przycisk **przypisać** znajdującego się na **Dodaj przydziału** okna dialogowego.
    
### <a name="test-single-sign-on"></a>Test rejestracji jednokrotnej

W tej sekcji można przetestować konfiguracji usługi Azure AD pojedynczego logowania za pomocą panelu dostępu.

Po kliknięciu kafelka uwierzytelnianie tożsamości platformy chmury SAP w panelu dostępu użytkownik powinien pobrać automatycznie zalogowane do aplikacji uwierzytelnianie tożsamości platformy chmury SAP.
Aby uzyskać więcej informacji na temat panelu dostępu, zobacz [wprowadzenie do panelu dostępu](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Lista samouczków dotyczących sposobów integracji aplikacji SaaS przy użyciu usługi Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co to jest dostęp do aplikacji i logowanie jednokrotne z usługą Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-sapcloudauth-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sapcloudauth-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sapcloudauth-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sapcloudauth-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-sapcloudauth-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sapcloudauth-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sapcloudauth-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sapcloudauth-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sapcloudauth-tutorial/tutorial_general_203.png

