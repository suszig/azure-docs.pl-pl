---
title: 'Samouczek: Integracji Azure Active Directory z Replicon | Dokumentacja firmy Microsoft'
description: "Dowiedz się, jak używać Replicon usłudze Azure Active Directory w celu włączenia logowania jednokrotnego, automatyczne Inicjowanie obsługi i inne!"
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 02a62f15-917c-417c-8d80-fe685e3fd601
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/23/2017
ms.author: jeedes
ms.openlocfilehash: 2aeeceb61191962b62892b8409218684f76c6fa8
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-replicon"></a>Samouczek: Integracji Azure Active Directory z Replicon
Celem tego samouczka jest pokazanie integracji Azure i Replicon. Scenariusz opisany w tym samouczku założono, że już następujące elementy:

* Ważnej subskrypcji platformy Azure
* Dzierżawy Replicon

Ten samouczek użytkowników usługi Azure AD, zostały przypisane do Replicon będzie można funkcji logowania jednokrotnego do aplikacji w witrynie firmy Replicon (usługa zainicjował dostawcy logowania) lub przy użyciu [wprowadzenie do panelu dostępu](active-directory-saas-access-panel-introduction.md).

Scenariusz opisany w tym samouczku składa się z następujących bloków konstrukcyjnych:

1. Włączanie integracji aplikacji dla Replicon
2. Konfigurowanie rejestracji jednokrotnej (SSO)
3. Konfigurowanie Inicjowanie obsługi użytkowników
4. Przypisywanie użytkowników

![Scenariusz](./media/active-directory-saas-replicon-tutorial/IC777798.png "scenariusza")

## <a name="enable-the-application-integration-for-replicon"></a>Włącz integrację aplikacji dla Replicon
Celem tej sekcji jest przedstawiają sposób włączania integracji aplikacji dla Replicon.

**Aby włączyć integrację aplikacji dla Replicon, wykonaj następujące czynności:**

1. W klasycznym portalu Azure, w okienku nawigacji po lewej stronie kliknij **usługi Active Directory**.
   
    ![Usługi Active Directory](./media/active-directory-saas-replicon-tutorial/IC700993.png "usługi Active Directory")
2. Z **katalogu** listy, wybierz katalog, dla którego chcesz włączyć integracji katalogów.
3. Aby otworzyć widok aplikacji, w widoku katalogu, kliknij przycisk **aplikacji** w menu u góry.
   
    ![Aplikacje](./media/active-directory-saas-replicon-tutorial/IC700994.png "aplikacji")
4. Kliknij przycisk **Dodaj** w dolnej części strony.
   
    ![Dodaj aplikację](./media/active-directory-saas-replicon-tutorial/IC749321.png "Dodaj aplikację")
5. Na **co chcesz zrobić** okna dialogowego, kliknij przycisk **dodać aplikację z galerii**.
   
    ![Dodawanie aplikacji z gallerry](./media/active-directory-saas-replicon-tutorial/IC749322.png "dodać aplikację z gallerry")
6. W **pole wyszukiwania**, typ **Replicon**.
   
    ![Galerii aplikacji](./media/active-directory-saas-replicon-tutorial/IC777799.png "galerii aplikacji")
7. W okienku wyników wybierz **Replicon**, a następnie kliknij przycisk **Complete** można dodać aplikację.
   
    ![Replicon](./media/active-directory-saas-replicon-tutorial/IC777800.png "Replicon")
   
## <a name="configure-single-sign-on"></a>Konfigurowanie rejestracji jednokrotnej

Celem tej sekcji jest przedstawiają sposób umożliwić użytkownikom uwierzytelnianie na Replicon do swojego konta w usłudze Azure AD przy użyciu federacyjnego na podstawie protokołu SAML.

**Aby skonfigurować rejestrację jednokrotną, wykonaj następujące czynności:**

1. W klasycznym portalu Azure na **Replicon** strona integracji aplikacji, kliknij przycisk **skonfigurować logowanie jednokrotne** otworzyć **skonfigurować rejestrację jednokrotną** okna dialogowego.
   
    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-replicon-tutorial/IC777801.png "skonfigurować logowanie jednokrotne")
2. Na **jak chcesz użytkownikom zalogować się na Replicon** wybierz pozycję **usługi Microsoft Azure AD rejestracji jednokrotnej**, a następnie kliknij przycisk **dalej**.
   
    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-replicon-tutorial/IC777802.png "skonfigurować logowanie jednokrotne")
3. Na **Konfigurowanie adresu URL aplikacji** wykonaj następujące czynności:
   
    ![Skonfiguruj adres URL aplikacji](./media/active-directory-saas-replicon-tutorial/IC777803.png "skonfigurować adres URL aplikacji")
  1. W **Replicon na adres URL logowania** tekstowym, wpisz adres URL dzierżawy Replicon (np.: *https://na2.replicon.com/company/saml2/sp-sso/post*).
  2. W **adres URL odpowiedzi Replicon** tekstowym, wpisz Replicon Twojego **AssertionConsumerService** adres URL (np.: *https://global.replicon.com/! / saml2/firmy/logowania jednokrotnego/post*).  
      
     >[!NOTE]
     >Należy uzyskać adres URL z metadanych Replicon w: **https://global.replicon.com/! /saml2/\<YourCompanyKey\>**.
     > 
     > 
 
  3. Kliknij przycisk **Dalej**.

4. Na **skonfigurować logowanie jednokrotne w Replicon** , aby pobrać metadane, kliknij przycisk **pobierania metadanych**, a następnie Zapisz metadane na komputerze.
   
    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-replicon-tutorial/IC777804.png "skonfigurować logowanie jednokrotne")
5. W oknie przeglądarki innej witryny sieci web Zaloguj się do witryny firmy Replicon jako administrator.

6. Aby skonfigurować SAML 2.0, wykonaj następujące czynności:
   
    ![Włącz uwierzytelnianie SAML](./media/active-directory-saas-replicon-tutorial/IC777805.png "uwierzytelnianie Włącz SAML")
  
  1. Aby wyświetlić **EnableSAML Authentication2** okna dialogowego, dołącz następujące na adres URL po klucz firmy: **/services/SecurityService1.svc/help/test/EnableSAMLAuthentication2**  
    * Poniżej przedstawiono schematu pełny adres URL:  
   **https://Na2.replicon.com/\<YourCompanyKey\>/services/SecurityService1.svc/help/test/EnableSAMLAuthentication2**
   2. Kliknij przycisk  **+**  rozszerzenia **v20Configuration** sekcji.
   3. Kliknij przycisk  **+**  rozszerzenia **metaDataConfiguration** sekcji.
   4. Kliknij przycisk **wybierz plik**, aby wybrać plik XML metadanych dostawcy tożsamości, a następnie kliknij przycisk **przesyłania**.

7. W klasycznym portalu Azure, wybierz Potwierdzenie konfiguracji rejestracji jednokrotnej, a następnie kliknij przycisk **Complete** zamknąć **skonfigurować rejestrację jednokrotną** okna dialogowego.
   
    ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-replicon-tutorial/IC778418.png "skonfigurować logowanie jednokrotne")
   
## <a name="configure-user-provisioning"></a>Skonfiguruj Inicjowanie obsługi użytkowników

Aby włączyć użytkowników usługi Azure AD zalogować się do Replicon, musi być przygotowana do Replicon.  

W przypadku Replicon Inicjowanie obsługi to zadanie ręczne.

**Aby skonfigurować, inicjowanie obsługi użytkowników, wykonaj następujące czynności:**

1. W oknie przeglądarki sieci web Zaloguj się do witryny firmy Replicon jako administrator.
2. Przejdź do **administracji \> użytkowników**.
   
    ![Użytkownicy](./media/active-directory-saas-replicon-tutorial/IC777806.png "użytkowników")
3. Kliknij przycisk **+ Dodaj użytkownika**.
   
    ![Dodaj użytkownika](./media/active-directory-saas-replicon-tutorial/IC777807.png "Dodaj użytkownika")
4. W **profilu użytkownika** sekcji, wykonaj następujące czynności:
   
    ![Profil użytkownika](./media/active-directory-saas-replicon-tutorial/IC777808.png "profilu użytkownika")
   
  1. W **nazwa logowania** pole tekstowe, adres e-mail użytkownika usługi Azure AD, aby udostępnić typu usługi Azure AD.
  2. Jako **typ uwierzytelniania**, wybierz pozycję **logowania jednokrotnego**.
  3. W **działu** tekstowym, wpisz dział tego użytkownika.
  4. Jako **typu pracownika**, wybierz pozycję **administratora**.
  5. Kliknij przycisk **Zapisz profil użytkownika**.

>[!NOTE]
>Możesz użyć innych Replicon użytkownika konta tworzenia narzędzi lub interfejsów API dostarczonych przez Replicon do kont użytkowników usługi AAD.
> 
> 

## <a name="assign-users"></a>Przypisywanie użytkowników
Aby przetestować konfigurację, musisz przyznać użytkowników usługi Azure AD, czy chcesz zezwolić, używając przypisywania do nich dostęp aplikacji do niego.

**Do przypisywania użytkowników do Replicon, wykonaj następujące czynności:**

1. W klasycznym portalu Azure Utwórz konto testu.

2. Na **Replicon** strona integracji aplikacji, kliknij przycisk **przypisywać użytkowników**.
   
    ![Przypisywanie użytkowników](./media/active-directory-saas-replicon-tutorial/IC777809.png "przypisywanie użytkowników")

3. Wybierz użytkownika testowego, kliknij przycisk **przypisać**, a następnie kliknij przycisk **tak** o potwierdzenie przypisania.
   
    ![Tak](./media/active-directory-saas-replicon-tutorial/IC767830.png "tak")

Jeśli chcesz przetestować jednego ustawienia logowania jednokrotnego, otwórz Panel dostępu. Aby uzyskać więcej informacji na temat panelu dostępu, zobacz [wprowadzenie do panelu dostępu](active-directory-saas-access-panel-introduction.md).

