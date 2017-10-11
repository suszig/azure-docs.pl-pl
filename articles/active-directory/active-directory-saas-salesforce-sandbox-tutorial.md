---
title: 'Samouczek: Integracji Azure Active Directory z piaskownicy Salesforce | Dokumentacja firmy Microsoft'
description: "Dowiedz się, jak użyć piaskownicy Salesforce z usługą Azure Active Directory w celu włączenia logowania jednokrotnego, automatyczne Inicjowanie obsługi i inne!."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: ee54c39e-ce20-42a4-8531-da7b5f40f57c
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/21/2017
ms.author: jeedes
ms.reviewer: jeedes
ms.openlocfilehash: 32835e79188806bb2ff319eea23b1b52ab585ab1
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-salesforce-sandbox"></a>Samouczek: Integracji Azure Active Directory z piaskownicy usług Salesforce

Celem tego samouczka jest pokazanie integracji Azure i usługi Salesforce piaskownicy.  

>[!TIP]
>Opinii, zobacz [stronę pomocy technicznej platformy Azure](http://go.microsoft.com/fwlink/?LinkId=521878). 
> 

Piaskownic zapewniają możliwość tworzenia wielu kopii organizacji w oddzielnych środowisk do różnych celów, takich jak projektowanie, testowanie i uczenie bez naruszania danych i aplikacji w organizacji produkcji Salesforce.  

Aby uzyskać więcej informacji, zobacz [piaskownicy — omówienie](https://help.salesforce.com/HTViewHelpDoc?id=create_test_instance.htm&language=en_US)

Scenariusz opisany w tym samouczku założono, że już następujące elementy:

* Ważnej subskrypcji platformy Azure
* Piaskownica w witrynie Salesforce.com

Jeśli nie masz prawidłowe piaskownicy w witrynie Salesforce.com jeszcze, musisz skontaktować się z usług Salesforce.

Scenariusz opisany w tym samouczku składa się z następujących bloków konstrukcyjnych:

1. Włączanie integracji aplikacji Salesforce piaskownica
2. Konfigurowanie rejestracji jednokrotnej (SSO)
3. Włączanie domeny
4. Konfigurowanie Inicjowanie obsługi użytkowników
5. Przypisywanie użytkowników

![Scenariusz](./media/active-directory-saas-salesforce-sandbox-tutorial/IC769571.png "scenariusza")

## <a name="enable-the-application-integration-for-salesforce-sandbox"></a>Włącz integrację aplikacji Salesforce piaskownica
Celem tej sekcji jest przedstawiają sposób włączania integracji aplikacji Salesforce piaskownica.

**Aby włączyć integrację aplikacji Salesforce piaskownica, wykonaj następujące czynności:**

1. W klasycznym portalu Azure, w okienku nawigacji po lewej stronie kliknij **usługi Active Directory**.
   
   ![Usługi Active Directory](./media/active-directory-saas-salesforce-sandbox-tutorial/IC700993.png "usługi Active Directory")
2. Z **katalogu** listy, wybierz katalog, dla którego chcesz włączyć integracji katalogów.
3. Aby otworzyć widok aplikacji, w widoku katalogu, kliknij przycisk **aplikacji** w menu u góry.
   
   ![Aplikacje](./media/active-directory-saas-salesforce-sandbox-tutorial/IC700994.png "aplikacji")
4. Aby otworzyć **galerii aplikacji**, kliknij przycisk **Dodaj aplikację**, a następnie kliknij przycisk **Dodawanie aplikacji dla mojej organizacji do używania**.
   
   ![Co chcesz zrobić? ] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC700995.png "Co chcesz zrobić?")
5. W **pole wyszukiwania**, typ **piaskownicy Salesforce**.
   
   ![Galerii aplikacji](./media/active-directory-saas-salesforce-sandbox-tutorial/IC710978.png "galerii aplikacji")
6. W okienku wyników wybierz **piaskownicy Salesforce**, a następnie kliknij przycisk **Complete** można dodać aplikację.
   
   ![Piaskownica SalesForce](./media/active-directory-saas-salesforce-sandbox-tutorial/IC746474.png "Salesforce piaskownicy")
   
## <a name="configur-single-sign-on-sso"></a>Configur rejestracji jednokrotnej (SSO)

Celem tej sekcji jest przedstawiają sposób użytkownicy mogli uwierzytelniania do usług Salesforce za pomocą swojego konta w usłudze Azure AD przy użyciu federacyjnego na podstawie protokołu SAML.

**Aby skonfigurować rejestrację jednokrotną, wykonaj następujące czynności:**

1. W klasycznym portalu Azure na **piaskownicy Salesforce** strona integracji aplikacji, kliknij przycisk **skonfigurować logowanie jednokrotne** otworzyć **skonfigurować rejestrację jednokrotną** okna dialogowego.
   
   ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-salesforce-sandbox-tutorial/IC749323.png "skonfigurować logowanie jednokrotne")
2. Na **jak chcesz użytkownikom zalogować się na piaskownicy Salesforce** wybierz pozycję **usługi Microsoft Azure AD rejestracji jednokrotnej**, a następnie kliknij przycisk **dalej**.
   
   ![Piaskownica SalesForce](./media/active-directory-saas-salesforce-sandbox-tutorial/IC746479.png "Salesforce piaskownicy")
3. Na **Konfigurowanie adresu URL aplikacji** strony w **na adres URL logowania** pole tekstowe, wpisz adres URL przy użyciu następującego wzorca `http://company.my.salesforce.com`, a następnie kliknij przycisk **dalej**.
   
   ![Skonfiguruj adres URL aplikacji](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781022.png "skonfigurować adres URL aplikacji")
4. Jeśli skonfigurowano już program rejestracji jednokrotnej dla innego wystąpienia usług Salesforce piaskownicy w katalogu, a następnie należy także skonfigurować **identyfikator** mają taką samą wartość jak **Zaloguj się na adres URL**. 
 * **Identyfikator** można znaleźć pola sprawdzając **Pokaż zaawansowane ustawienia** wyboru na **Konfigurowanie adresu URL aplikacji** strony okna dialogowego.
5. Na **skonfigurować logowanie jednokrotne w piaskownicy Salesforce** kliknij przycisk **pobierania certyfikatu**, a następnie zapisz plik certyfikatu na tym komputerze.
   
   ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781023.png "skonfigurować logowanie jednokrotne")
6. W oknie przeglądarki innej witryny sieci web Zaloguj się do Twojego piaskownicy Salesforce jako administrator.
7. W menu u góry kliknij **Instalatora**.
   
   ![Instalator](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781024.png "Instalatora")
8. W okienku nawigacji po lewej stronie kliknij **kontroli bezpieczeństwa**, a następnie kliknij przycisk **ustawień rejestracji jednokrotnej**.
   
   ![Single Sign-On ustawienia](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781025.png "Single Sign-On ustawienia")
9. W sekcji Ustawienia rejestracji jednokrotnej wykonaj następujące czynności:
   
   ![Single Sign-On ustawienia](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781026.png "Single Sign-On ustawienia")  
 1.  Wybierz **SAML włączone**. 
 2.  Kliknij przycisk **Nowy**.
10. W sekcji SAML jednego ustawienia rejestracji wykonaj następujące czynności:
    
    ![SAML pojedynczego logowania jednokrotnego ustawienia](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781027.png "SAML jednego ustawienia rejestracji")  
 1. W polu tekstowym Nazwa wpisz nazwę konfiguracji (np.: *SPSSOWAAD\_testu*). 
 2. W klasycznym portalu Azure na **skonfigurować logowanie jednokrotne w piaskownicy Salesforce** dialogu strony, kopiowania **adres URL wystawcy** wartość, a następnie wklej ją do **wystawcy** pola tekstowego.
 3. W **identyfikator jednostki** pole tekstowe, typ **https://test.salesforce.com** Jeśli jest to pierwsze wystąpienie piaskownicy Salesforce, które ma zostać dodane do katalogu. Jeśli masz już dodany wystąpienia usług Salesforce piaskownicy, następnie dla **identyfikator jednostki** wpisz **na adres URL logowania**, która powinna być w następującym formacie:`http://company.my.salesforce.com`   
 4. Kliknij przycisk **Przeglądaj** przekazywania pobranego certyfikatu.  
 5. Jako **typ tożsamości SAML**, wybierz pozycję **potwierdzenia zawiera identyfikator federacji z obiektu użytkownika**. 
 6. Jako **lokalizacji tożsamości SAML**, wybierz pozycję **jest tożsamość w elemencie NameIdentifier instrukcji podmiotu**.
 7. W klasycznym portalu Azure na **skonfigurować logowanie jednokrotne w piaskownicy Salesforce** dialogu strony, kopiowania **zdalnego adresu URL logowania** wartość, a następnie wklej ją do **adresu URL logowania do dostawcy tożsamości** pola tekstowego. 
 8. SFDC nie obsługuje SAML wylogowania.  Jako obejście, Wklej "https://login.microsoftonline.com/common/wsfederation?wa=wsignout1.0" go do **adres URL wylogowania dostawcy tożsamości** pola tekstowego.
 9. Jako **dostawcy zainicjował żądanie powiązania usługi**, wybierz pozycję **HTTP POST**. 
 10. Kliknij pozycję **Zapisz**.
11. W klasycznym portalu Azure, wybierz Potwierdzenie konfiguracji rejestracji jednokrotnej, a następnie kliknij przycisk **Complete** zamknąć **skonfigurować rejestrację jednokrotną** okna dialogowego.
    
    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781028.png "skonfigurować logowanie jednokrotne")

## <a name="enable-your-domain"></a>Włącz domeny
W tej sekcji założono, że już utworzono domeny.  Aby uzyskać więcej informacji, zobacz [Definiowanie Twojej nazwy domeny](https://help.salesforce.com/HTViewHelpDoc?id=domain_name_define.htm&language=en_US).

**Aby włączyć domeny, wykonaj następujące czynności:**

1. W okienku nawigacji po lewej stronie kliknij **Zarządzanie domenami**, a następnie kliknij przycisk **Moje domeny.**
   
   ![Mojej domeny](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781029.png "mojej domeny")
   
   >[!NOTE]
   >Upewnij się, że poprawnie skonfigurowano domenę. 
   > 
2. W **ustawienia strony logowania** kliknij **Edytuj**, następnie jako **usługi uwierzytelniania**, wybierz nazwę SAML pojedynczy znak na ustawienie z poprzedniej sekcji, a na koniec kliknij przycisk **zapisać**.
   
   ![Mojej domeny](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781030.png "mojej domeny")

Jak masz domenę skonfigurowane, użytkownicy należy używać URL domeny na potrzeby logowania do usługi Salesforce piaskownicy.  

Aby uzyskać wartość adresu URL, kliknij profil rejestracji Jednokrotnej, utworzony w poprzedniej sekcji.

## <a name="configure-user-provisioning"></a>Skonfiguruj Inicjowanie obsługi użytkowników
Celem tej sekcji jest przedstawiają sposób włączania kont użytkowników usługi Active Directory do usługi Salesforce piaskownicy Inicjowanie obsługi użytkowników.

**Aby skonfigurować, inicjowanie obsługi użytkowników, wykonaj następujące czynności:**

1. W portalu usług Salesforce w górnym pasku nawigacyjnym wybierz nazwę, aby rozwinąć menu programu użytkownika:
   
   ![Ustawienia](./media/active-directory-saas-salesforce-sandbox-tutorial/IC698773.png "Moje ustawienia")
2. Menu użytkownika wybierz **Moje ustawienia** otworzyć Twojego **Moje ustawienia** strony.
3. W okienku po lewej stronie kliknij **osobistych** rozwiń sekcję osobistych, a następnie kliknij przycisk **zresetować moje tokenu zabezpieczeń**:
   
   ![Ustawienia](./media/active-directory-saas-salesforce-sandbox-tutorial/IC698774.png "Moje ustawienia")
4. Na **zresetować moje tokenu zabezpieczeń** kliknij przycisk **zresetować tokenu zabezpieczeń** Aby zażądać wiadomości e-mail, który zawiera token zabezpieczeń witryny Salesforce.com.
   
   ![Nowy Token](./media/active-directory-saas-salesforce-sandbox-tutorial/IC698776.png "nowy Token")
5. Sprawdź skrzynki odbiorczej poczty e-mail, wiadomości e-mail z witryny Salesforce.com z "**potwierdzenia zabezpieczeń salesforce.com.com**" jako podmiotu.
6. Przejrzyj tę wiadomość e-mail, a następnie skopiuj wartość tokenu zabezpieczeń.
7. W klasycznym portalu Azure na **salesforce piaskownicy** strona integracji aplikacji, kliknij przycisk **skonfigurować Inicjowanie obsługi użytkowników** otworzyć **skonfigurować Inicjowanie obsługi użytkowników** okna dialogowego.
   
   ![Skonfiguruj Inicjowanie obsługi użytkowników](./media/active-directory-saas-salesforce-sandbox-tutorial/IC769573.png "skonfigurować Inicjowanie obsługi użytkowników")
8. Na **wprowadź swoje poświadczenia piaskownicy Salesforce umożliwia inicjowanie obsługi użytkowników automatyczne** Podaj następujące ustawienia konfiguracji:
   
   ![Piaskownica SalesForce](./media/active-directory-saas-salesforce-sandbox-tutorial/IC746476.png "Salesforce piaskownicy")   
 1. W **nazwa użytkownika administratora piaskownicy Salesforce** tekstowym, wpisz nazwę, która ma konto Salesforce piaskownicy **administratorem** profilu w witrynie Salesforce.com przypisane.
 2. W **hasło administratora piaskownicy Salesforce** tekstowym, wpisz hasło dla tego konta.
 3. W **tokenu zabezpieczeń użytkownika** pole tekstowe, Wklej wartość tokenu zabezpieczeń.
 4. Kliknij przycisk **weryfikacji** Aby zweryfikować konfigurację.
 5. Kliknij przycisk **dalej** przycisk, aby otworzyć **potwierdzenie** strony.
9. Na **potwierdzenie** kliknij przycisk **Complete** Aby zapisać konfigurację.
   
## <a name="assigning-users"></a>Przypisywanie użytkowników

Aby przetestować konfigurację, musisz przyznać użytkowników usługi Azure AD, czy chcesz zezwolić, używając przypisywania do nich dostęp aplikacji do niego.

**Do przypisywania użytkowników do usługi Salesforce piaskownicy, wykonaj następujące czynności:**

1. W klasycznym portalu Azure Utwórz konto testu.
2. Na ** piaskownicy Salesforce ** strona integracji aplikacji, kliknij przycisk **przypisywać użytkowników**.
   
   ![Przypisywanie użytkowników](./media/active-directory-saas-salesforce-sandbox-tutorial/IC769574.png "przypisywanie użytkowników")
3. Wybierz użytkownika testowego, kliknij przycisk **przypisać**, a następnie kliknij przycisk **tak** o potwierdzenie przypisania.
   
   ![Tak](./media/active-directory-saas-salesforce-sandbox-tutorial/IC767830.png "tak")

Należy teraz Odczekaj 10 minut i sprawdź, czy konto zostało zsynchronizowane z Salesforce piaskownicy.

Jeśli chcesz przetestować ustawienia logowania jednokrotnego, otwórz Panel dostępu. Aby uzyskać więcej informacji na temat panelu dostępu, zobacz [wprowadzenie do panelu dostępu](https://msdn.microsoft.com/library/dn308586).

