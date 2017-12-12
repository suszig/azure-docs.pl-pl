---
title: 'Samouczek: Integracji Azure Active Directory z SuccessFactors | Dokumentacja firmy Microsoft'
description: "Dowiedz się, jak używać SuccessFactors usłudze Azure Active Directory w celu włączenia logowania jednokrotnego, automatyczne Inicjowanie obsługi i inne!"
services: active-directory
author: jeevansd
documentationcenter: na
manager: mtillman
ms.assetid: 32bd8898-c2d2-4aa7-8c46-f1f5c2aa05f1
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/21/2017
ms.author: jeedes
ms.reviewer: jeedes
ms.openlocfilehash: d206298f8c0af6c3f96740594c6ff904228ee48b
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-successfactors"></a>Samouczek: Integracji Azure Active Directory z SuccessFactors
Celem tego samouczka jest pokazanie sposobu integracji SuccessFactors z usługi Azure Active Directory (Azure AD).

Integracja z usługą Azure AD SuccessFactors zapewnia następujące korzyści:

* Można kontrolować w usłudze Azure AD, który ma dostęp do SuccessFactors
* Umożliwia użytkownikom automatycznie pobrać zalogowane do SuccessFactors (logowanie jednokrotne) przy użyciu ich kont usługi Azure AD
* Możesz zarządzać kont w jednej centralnej lokalizacji - klasycznego portalu Azure

Jeśli chcesz dowiedzieć się więcej informacji o integracji aplikacji SaaS w usłudze Azure AD, zobacz [co to jest dostęp do aplikacji i logowanie jednokrotne z usługą Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Wymagania wstępne
Aby skonfigurować integrację usługi Azure AD z SuccessFactors, potrzebne są następujące elementy:

* Ważnej subskrypcji platformy Azure
* Dzierżawcy w SuccessFactors

> [!NOTE]
> Aby przetestować kroki opisane w tym samouczku, zaleca się używania środowiska produkcyjnego.
> 
> 

Aby przetestować kroki opisane w tym samouczku, należy wykonać te zalecenia:

* Nie należy używać środowiska produkcyjnego, chyba że jest to konieczne.
* Jeśli nie masz środowisko wersji próbnej usługi Azure AD, możesz pobrać miesięczna wersja próbna [tutaj](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Opis scenariusza
Celem tego samouczka jest umożliwienie umożliwia testowanie usługi Azure AD rejestracji jednokrotnej w środowisku testowym.

Scenariusz opisany w tym samouczku składa się z dwóch głównych elementów:

1. Dodawanie SuccessFactors z galerii
2. Konfigurowanie i testowanie usługi Azure AD logowanie jednokrotne

## <a name="adding-successfactors-from-the-gallery"></a>Dodawanie SuccessFactors z galerii
Aby skonfigurować integrację usługi Azure AD SuccessFactors, należy dodać SuccessFactors z galerii do listy zarządzanych aplikacji SaaS.

**Aby dodać SuccessFactors z galerii, wykonaj następujące czynności:**

1. W klasycznym portalu Azure, na panelu nawigacyjnym po lewej stronie kliknij **usługi Active Directory**.
   
    ![Konfigurowanie rejestracji jednokrotnej][1]
2. Z **katalogu** listy, wybierz katalog, dla którego chcesz włączyć integracji katalogów.
3. Aby otworzyć widok aplikacji, w widoku katalogu, kliknij przycisk **aplikacji** w menu u góry.
   
    ![Konfigurowanie rejestracji jednokrotnej][2]
4. Kliknij przycisk **Dodaj** w dolnej części strony.
   
    ![Aplikacje][3]
5. Na **co chcesz zrobić** okna dialogowego, kliknij przycisk **dodać aplikację z galerii**.
   
    ![Konfigurowanie rejestracji jednokrotnej][4]
6. W **pole wyszukiwania**, typ **SuccessFactors**.
   
    ![Konfigurowanie rejestracji jednokrotnej][5]
7. W panelu wyników wybierz **SuccessFactors**, a następnie kliknij przycisk **Complete** można dodać aplikację.
   
    ![Konfigurowanie rejestracji jednokrotnej][6]

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurowanie i testowanie usługi Azure AD logowanie jednokrotne
Jest celem tej sekcji opisano, jak skonfigurować i przetestować usługi Azure AD rejestracji jednokrotnej z SuccessFactors w oparciu o nazwie "Britta Simona" użytkownika testowego.

Dla rejestracji jednokrotnej do pracy usługi Azure AD musi wiedzieć, co to jest odpowiednikiem użytkownikowi w SuccessFactors użytkownika w usłudze Azure AD. Innymi słowy link relację między użytkownikiem usługi Azure AD i danemu użytkownikowi w SuccessFactors musi się.

Ta relacja łącza zostanie nawiązane, przypisując wartość **nazwy użytkownika** w usłudze Azure AD jako wartość **Username** w SuccessFactors.

Aby skonfigurować i przetestować usługi Azure AD rejestracji jednokrotnej z SuccessFactors, należy wykonać poniższe bloki konstrukcyjne:

1. **[Konfigurowanie usługi Azure AD rejestracji jednokrotnej](#configuring-azure-ad-single-single-sign-on)**  — aby umożliwić użytkownikom korzystać z tej funkcji.
2. **[Tworzenie użytkownika testowego usługi Azure AD](#creating-an-azure-ad-test-user)**  — do przetestowania usługi Azure AD rejestracji jednokrotnej z Simona Britta.
3. **[Tworzenie użytkownika testowego SuccessFactors](#creating-a-successfactors-test-user)**  — w celu zapewnienia odpowiednikiem Simona Britta SuccessFactors połączonego z jej reprezentacji usługi Azure AD.
4. **[Przypisanie użytkownika testowego usługi Azure AD](#assigning-the-azure-ad-test-user)**  — aby umożliwić Simona Britta do użycia usługi Azure AD rejestracji jednokrotnej.
5. **[Testowanie rejestracji jednokrotnej](#testing-single-sign-on)**  — Aby sprawdzić, czy konfiguracja działa.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurowanie usługi Azure AD rejestracji jednokrotnej
W tej sekcji można włączyć usługi Azure AD rejestracji jednokrotnej w klasycznym portalu i skonfigurować logowanie jednokrotne w aplikacji SuccessFactors.

**Aby skonfigurować usługi Azure AD rejestracji jednokrotnej z SuccessFactors, wykonaj następujące czynności:**

1. W klasycznym portalu Azure na **SuccessFactors** strona integracji aplikacji, kliknij przycisk **skonfigurować logowanie jednokrotne** otworzyć **skonfigurować rejestrację jednokrotną** okna dialogowego.
   
    ![Konfigurowanie rejestracji jednokrotnej][7]
2. Na **jak chcesz użytkownikom zalogować się na SuccessFactors** wybierz pozycję **usługi Microsoft Azure AD rejestracji jednokrotnej**, a następnie kliknij przycisk **dalej**.
   
    ![Konfigurowanie rejestracji jednokrotnej][8]
3. Na **Konfigurowanie adresu URL aplikacji** , wykonaj następujące czynności, a następnie kliknij przycisk **dalej**.
   
    ![Konfigurowanie rejestracji jednokrotnej][9]
   
    a. W **na adres URL logowania** tekstowym, wpisz adres URL przy użyciu jednej z następujących wzorców: 
   
    |  |
    | --- |
    | `https://<company name>.successfactors.com/<company name>` |
    | `https://<company name>.sapsf.com/<company name>` |
    | `https://<company name>.successfactors.eu/<company name>` |
    | `https://<company name>.sapsf.eu` |
   
    b. W **adres URL odpowiedzi** tekstowym, wpisz adres URL przy użyciu jednej z następujących wzorców: 
   
    |  |
    | --- |
    | `https://<company name>.successfactors.com/<company name>` |
    | `https://<company name>.sapsf.com/<company name>` |
    | `https://<company name>.successfactors.eu/<company name>` |
    | `https://<company name>.sapsf.eu` |
    | `https://<company name>.sapsf.eu/<company name>` |
   
    c. Kliknij przycisk **Dalej**. 

    > [!NOTE]
    > Należy pamiętać, że nie są one rzeczywiste wartości. Należy zaktualizować te wartości rzeczywistych na adres URL logowania i adres URL odpowiedzi. Aby uzyskać te wartości, skontaktuj się z [SuccessFactors obsługuje zespołu](https://www.successfactors.com/en_us/support.html).

1. Na **skonfigurować logowanie jednokrotne w SuccessFactors** kliknij przycisk **pobierania certyfikatu**, a następnie zapisz plik certyfikatu lokalnie na komputerze.
   
    ![Konfigurowanie rejestracji jednokrotnej][10]

2. W oknie przeglądarki innej witryny sieci web, zaloguj się do Twojego **portalu administracyjnego SuccessFactors** jako administrator.

3. Odwiedź stronę **zabezpieczeń aplikacji** i macierzysty **pojedynczy znak w funkcji**. 

4. Umieść wszystkie wartości w **zresetować tokenu** i kliknij przycisk **zapisać tokenu** do włączenia funkcji logowania jednokrotnego SAML.
   
    ![Konfigurowanie rejestracji jednokrotnej po stronie aplikacji][11]

    > [!NOTE] 
    > Ta wartość jest używana tylko jako przełącznika wł. / wył. Po zapisaniu wartości logowania jednokrotnego SAML jest włączone. Po zapisaniu pustego logowania jednokrotnego SAML jest wyłączona.

1. Macierzysty poniżej zrzut ekranu i wykonaj następujące czynności.
   
    ![Konfigurowanie rejestracji jednokrotnej po stronie aplikacji][12]
   
    a. Wybierz **logowania jednokrotnego SAML v2** przycisk radiowy
   
    b. Ustaw Name(e.g. SAml issuer + company name) strona zostanie SAML.
   
    c. W **wystawcy SAML** pole tekstowe umieścić wartość elementu **adres URL wystawcy** z Kreatora konfiguracji aplikacji usługi Azure AD.
   
    d. Wybierz **odpowiedzi (odbiorcy wygenerowanych/IdP/AP)** jako **wymagają podpisu obowiązkowe**.
   
    e. Wybierz **włączone** jako **Włącz flagę SAML**.
   
    f. Wybierz **nr** jako **podpisu żądania logowania (rz wygenerowanych/SP/RP)**.
   
    g. Wybierz **profilu przeglądarki lub używanego po** jako **profilu SAML**.
   
    h. Wybierz **nr** jako **wymusić nieprawidłowy okres certyfikatu**.
   
    i. Skopiuj zawartość pliku pobranego certyfikatu, a następnie wklej go do **certyfikatu weryfikacji SAML** pola tekstowego.

    > [!NOTE] 
    > Zawartość certyfikatu musi mieć rozpocząć certyfikatu i na końcu tagi certyfikatu.

1. Przejdź do SAML V2, a następnie wykonaj następujące czynności:
   
    ![Konfigurowanie rejestracji jednokrotnej po stronie aplikacji][13]
   
    a. Wybierz **tak** jako **inicjowane SP wylogowania globalnego obsługują**.
   
    b. W **globalne wylogowania adres URL usługi (LogoutRequest docelowy)** pole tekstowe umieścić wartość elementu **zdalnego adresu URL wylogowania** z Kreatora konfiguracji aplikacji usługi Azure AD.
   
    c. Wybierz **nr** jako **wymagają sp należy szyfrować wszystkie element NameID**.
   
    d. Wybierz **nieokreślony** jako **NameID Format**.
   
    e. Wybierz **tak** jako **Włącz sp zainicjował logowania (AuthnRequest)**.
   
    f. W **żądanie wysłania jako wystawca firmie** pole tekstowe umieścić wartość elementu **zdalnego adresu URL logowania** z Kreatora konfiguracji aplikacji usługi Azure AD.
2. Wykonaj następujące kroki, aby wprowadzić nazwy logowania użytkowników bez uwzględniania wielkości liter,.
   
    a. Odwiedź stronę **ustawienia firmy**(w dolnej).
   
    b. Zaznacz pole wyboru obok **włączyć Username Non-uwzględniana wielkość liter**.
   
    c.Click **zapisać**.
   
    ![Konfigurowanie rejestracji jednokrotnej][29]

    > [!NOTE] 
    > Jeśli spróbujesz je włączyć, system sprawdza, czy spowoduje utworzenie zduplikowanej nazwy logowania SAML. Na przykład, jeśli klient ma nazw użytkowników Użytkownik1 i Użytkownik1. Zdjęciu uwzględniana wielkość liter powoduje, że te duplikaty. System otrzymasz komunikat o błędzie i nie włączy tę funkcję. Klienta należy zmienić jedną z nazw użytkowników, tak faktycznie Pisownia jest inny. 

1. W klasycznym portalu Azure, wybierz Potwierdzenie konfiguracji rejestracji jednokrotnej, a następnie kliknij przycisk **Complete** zamknąć **skonfigurować rejestrację jednokrotną** okna dialogowego.
   
    ![Aplikacje][14]
2. Na **pojedynczy znak na potwierdzenie** kliknij przycisk **Complete**.
   
    ![Aplikacje][15]

### <a name="creating-an-azure-ad-test-user"></a>Tworzenie użytkownika testowego usługi Azure AD
Celem tej sekcji jest tworzenie użytkownika testowego w klasycznym portalu o nazwie Simona Britta.

![Tworzenie użytkowników usługi Azure AD][16]

**Aby utworzyć użytkownika testowego w usłudze Azure AD, wykonaj następujące czynności:**

1. W **klasycznego portalu Azure**, w lewym okienku nawigacji, kliknij polecenie **usługi Active Directory**.
   
    ![Tworzenie użytkownika testowego usługi Azure AD][17]
2. Z **katalogu** listy, wybierz katalog, dla którego chcesz włączyć integracji katalogów.
3. Aby wyświetlić listę użytkowników, w menu u góry, kliknij przycisk **użytkowników**.
   
    ![Tworzenie użytkownika testowego usługi Azure AD][18]
4. Aby otworzyć **Dodaj użytkownika** okna dialogowego na pasku narzędzi u dołu, kliknij przycisk **Dodaj użytkownika**.
   
    ![Tworzenie użytkownika testowego usługi Azure AD][19]
5. Na **Poinformuj nas o tym użytkowniku** okna dialogowego strony, należy wykonać następujące czynności:
   
    ![Tworzenie użytkownika testowego usługi Azure AD][20]
   
    a. Jako typ użytkownika wybierz nowego użytkownika w organizacji.
   
    b. W nazwie użytkownika **pole tekstowe**, typ **BrittaSimon**.
   
    c. Kliknij przycisk **Dalej**.
6. Na **profilu użytkownika** okna dialogowego strony, należy wykonać następujące czynności:
   
    ![Tworzenie użytkownika testowego usługi Azure AD][21]
   
    a. W **imię** pole tekstowe, typ **Britta**.  
   
    b. W **nazwisko** pole tekstowe, typ **Simona**.
   
    c. W **Nazwa wyświetlana** pole tekstowe, typ **Simona Britta**.
   
    d. W **roli** listy, wybierz **użytkownika**.
   
    e. Kliknij przycisk **Dalej**.
7. Na **Uzyskaj hasło tymczasowe** strony okna dialogowego, kliknij przycisk **utworzyć**.
   
    ![Tworzenie użytkownika testowego usługi Azure AD][22]
8. Na **Uzyskaj hasło tymczasowe** okna dialogowego strony, należy wykonać następujące czynności:
   
    ![Tworzenie użytkownika testowego usługi Azure AD][23]
   
    a. Zanotuj wartość **nowe hasło**.
   
    b. Kliknij przycisk **Complete** (Zakończ).  

### <a name="creating-a-successfactors-test-user"></a>Tworzenie użytkownika testowego SuccessFactors
Aby włączyć użytkowników usługi Azure AD zalogować się do SuccessFactors, musi być przygotowana do SuccessFactors.  
W przypadku SuccessFactors Inicjowanie obsługi to zadanie ręczne.

Aby uzyskać użytkownicy utworzeni w SuccessFactors, należy skontaktować się [SuccessFactors obsługuje zespołu](https://www.successfactors.com/en_us/support.html).

### <a name="assigning-the-azure-ad-test-user"></a>Przypisanie użytkownika testowego usługi Azure AD
Celem tej sekcji jest włączenie Simona Britta do udostępnienia jej SuccessFactors za pomocą usługi Azure rejestracji jednokrotnej.

![Przypisz użytkownika][24]

**Aby przypisać Simona Britta SuccessFactors, wykonaj następujące czynności:**

1. W klasycznym portalu, aby otworzyć widok aplikacji, w widoku katalogu, kliknij przycisk **aplikacji** w menu u góry.
   
    ![Przypisz użytkownika][25]
2. Na liście aplikacji zaznacz **SuccessFactors**.
   
    ![Konfigurowanie rejestracji jednokrotnej][26]
3. W menu u góry kliknij **użytkowników**.
   
    ![Przypisz użytkownika][27]
4. Na liście użytkowników wybierz **Simona Britta**.
5. Na pasku narzędzi u dołu, kliknij przycisk **przypisać**.
   
    ![Przypisz użytkownika][28]

### <a name="testing-single-sign-on"></a>Testowanie rejestracji jednokrotnej
Celem tej sekcji służy do testowania konfiguracji usługi Azure AD pojedynczego logowania za pomocą panelu dostępu.

Po kliknięciu kafelka SuccessFactors w panelu dostępu użytkownik powinien pobrać automatycznie zalogowane do aplikacji SuccessFactors.

## <a name="additional-resources"></a>Dodatkowe zasoby
* [Lista samouczków dotyczących sposobów integracji aplikacji SaaS przy użyciu usługi Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co to jest dostęp do aplikacji i logowanie jednokrotne z usługą Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[0]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_00.png
[1]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_01.png
[6]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_02.png
[7]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_03.png
[8]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_04.png
[9]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_05.png
[10]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_06.png

[11]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_07.png
[12]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_08.png
[13]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_09.png
[14]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_05.png
[15]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_06.png

[16]: ./media/active-directory-saas-successfactors-tutorial/create_aaduser_00.png
[17]: ./media/active-directory-saas-successfactors-tutorial/create_aaduser_01.png
[18]: ./media/active-directory-saas-successfactors-tutorial/create_aaduser_02.png
[19]: ./media/active-directory-saas-successfactors-tutorial/create_aaduser_03.png
[20]: ./media/active-directory-saas-successfactors-tutorial/create_aaduser_04.png
[21]: ./media/active-directory-saas-successfactors-tutorial/create_aaduser_05.png
[22]: ./media/active-directory-saas-successfactors-tutorial/create_aaduser_06.png
[23]: ./media/active-directory-saas-successfactors-tutorial/create_aaduser_07.png

[24]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_07.png
[25]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_08.png
[26]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_11.png
[27]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_09.png
[28]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_10.png
[29]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_10.png
