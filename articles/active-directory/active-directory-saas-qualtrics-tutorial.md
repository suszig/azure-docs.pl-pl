---
title: 'Samouczek: Integracji Azure Active Directory z Qualtrics | Dokumentacja firmy Microsoft'
description: "Dowiedz się, jak używać Qualtrics usłudze Azure Active Directory w celu włączenia logowania jednokrotnego, automatyczne Inicjowanie obsługi i inne!"
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 4df889ab-2685-4d15-a163-1ba26567eeda
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/23/2017
ms.author: jeedes
ms.openlocfilehash: 2fcde595a40dafda7549f5bccb582b57585b314e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-qualtrics"></a>Samouczek: Integracji Azure Active Directory z Qualtrics
Celem tego samouczka jest pokazanie integracji Azure i Qualtrics.  

Scenariusz opisany w tym samouczku założono, że już następujące elementy:

* Ważnej subskrypcji platformy Azure
* Qualtrics rejestracji jednokrotnej (SSO) włączone subskrypcji

Ten samouczek użytkowników usługi Azure AD, zostały przypisane do Qualtrics będzie można funkcji logowania jednokrotnego do aplikacji w witrynie firmy Qualtrics (usługa zainicjował dostawcy logowania) lub przy użyciu [wprowadzenie do panelu dostępu](active-directory-saas-access-panel-introduction.md).

Scenariusz opisany w tym samouczku składa się z następujących bloków konstrukcyjnych:

1. Włączanie integracji aplikacji dla Qualtrics
2. Konfigurowanie rejestracji jednokrotnej (SSO)
3. Konfigurowanie Inicjowanie obsługi użytkowników
4. Przypisywanie użytkowników

![Scenariusz](./media/active-directory-saas-qualtrics-tutorial/IC789542.png "scenariusza")

## <a name="enabling-the-application-integration-for-qualtrics"></a>Włączanie integracji aplikacji dla Qualtrics
Celem tej sekcji jest przedstawiają sposób włączania integracji aplikacji dla Qualtrics.

**Aby włączyć integrację aplikacji dla Qualtrics, wykonaj następujące czynności:**

1. W klasycznym portalu Azure, w okienku nawigacji po lewej stronie kliknij **usługi Active Directory**.
   
   ![Usługi Active Directory](./media/active-directory-saas-qualtrics-tutorial/IC700993.png "usługi Active Directory")
2. Z **katalogu** listy, wybierz katalog, dla którego chcesz włączyć integracji katalogów.
3. Aby otworzyć widok aplikacji, w widoku katalogu, kliknij przycisk **aplikacji** w menu u góry.
   
   ![Aplikacje](./media/active-directory-saas-qualtrics-tutorial/IC700994.png "aplikacji")
4. Kliknij przycisk **Dodaj** w dolnej części strony.
   
   ![Dodaj aplikację](./media/active-directory-saas-qualtrics-tutorial/IC749321.png "Dodaj aplikację")
5. Na **co chcesz zrobić** okna dialogowego, kliknij przycisk **dodać aplikację z galerii**.
   
   ![Dodawanie aplikacji z gallerry](./media/active-directory-saas-qualtrics-tutorial/IC749322.png "dodać aplikację z gallerry")
6. W **pole wyszukiwania**, typ **Qualtrics**.
   
   ![Galerii aplikacji](./media/active-directory-saas-qualtrics-tutorial/IC789543.png "galerii aplikacji")
7. W okienku wyników wybierz **Qualtrics**, a następnie kliknij przycisk **Complete** można dodać aplikację.
   
   ![Qualtrics](./media/active-directory-saas-qualtrics-tutorial/IC789544.png "Qualtrics")
   
## <a name="configure-single-sign-on"></a>Konfigurowanie rejestracji jednokrotnej

Celem tej sekcji jest przedstawiają sposób umożliwić użytkownikom uwierzytelnianie na Qualtrics do swojego konta w usłudze Azure AD przy użyciu federacyjnego na podstawie protokołu SAML.

**Aby skonfigurować rejestrację jednokrotną, wykonaj następujące czynności:**

1. W klasycznym portalu Azure na **Qualtrics** strona integracji aplikacji, kliknij przycisk **skonfigurować logowanie jednokrotne** otworzyć **skonfigurować rejestrację jednokrotną** okna dialogowego.
   
   ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-qualtrics-tutorial/IC789545.png "skonfigurować logowanie jednokrotne")
2. Na **jak chcesz użytkownikom zalogować się na Qualtrics** wybierz pozycję **usługi Microsoft Azure AD rejestracji jednokrotnej**, a następnie kliknij przycisk **dalej**.
   
   ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-qualtrics-tutorial/IC789546.png "skonfigurować logowanie jednokrotne")
3. Na **Konfigurowanie adresu URL aplikacji** strony w **Qualtrics na adres URL logowania** pole tekstowe, wpisz adres URL (np.: "*https://ssotest2ut1.qualtrics.com*"), a następnie kliknij przycisk **dalej**.
   
   ![Skonfiguruj adres URL aplikacji](./media/active-directory-saas-qualtrics-tutorial/IC789547.png "skonfigurować adres URL aplikacji")
4. Na **skonfigurować logowanie jednokrotne w Qualtrics** kliknij przycisk **pobierania metadanych**, a następnie zapisz plik metadanych na tym komputerze.
   
   ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-qualtrics-tutorial/IC789548.png "skonfigurować logowanie jednokrotne")
5. Wyślij plik metadanych do Qualtrics zespołu pomocy technicznej.
   
   >[!NOTE]
   >Konfiguracji logowania jednokrotnego musi zostać wykonana przez zespół pomocy technicznej Qualtrics. Otrzymasz powiadomienie, natychmiast po zakończeniu konfiguracji.
   > 
   > 
6. W klasycznym portalu Azure, wybierz Potwierdzenie konfiguracji rejestracji jednokrotnej, a następnie kliknij przycisk **Complete** zamknąć **skonfigurować rejestrację jednokrotną** okna dialogowego.
   
   ![Konfigurowanie rejestracji jednokrotnej](./media/active-directory-saas-qualtrics-tutorial/IC789549.png "skonfigurować logowanie jednokrotne")
   
## <a name="configure-user-provisioning"></a>Skonfiguruj Inicjowanie obsługi użytkowników

Nie ma elementu akcji do skonfigurowania inicjowania obsługi administracyjnej Qualtrics użytkownika. Gdy przypisany użytkownik próbuje zalogować się przy użyciu panelu dostępu Qualtrics, Qualtrics sprawdza, czy użytkownik istnieje.  

Jeśli nie jest Brak konta użytkownika dostępny jeszcze, są tworzone przez Qualtrics.

## <a name="assign-users"></a>Przypisywanie użytkowników
Aby przetestować konfigurację, musisz przyznać użytkowników usługi Azure AD, czy chcesz zezwolić, używając przypisywania do nich dostęp aplikacji do niego.

**Do przypisywania użytkowników do Qualtrics, wykonaj następujące czynności:**

1. W klasycznym portalu Azure Utwórz konto testu.
2. Na **Qualtrics** strona integracji aplikacji, kliknij przycisk **przypisywać użytkowników**.
   
   ![Przypisywanie użytkowników](./media/active-directory-saas-qualtrics-tutorial/IC789550.png "przypisywanie użytkowników")
3. Wybierz użytkownika testowego, kliknij przycisk **przypisać**, a następnie kliknij przycisk **tak** o potwierdzenie przypisania.
   
   ![Tak](./media/active-directory-saas-qualtrics-tutorial/IC767830.png "tak")

Jeśli chcesz przetestować ustawienia logowania jednokrotnego, otwórz Panel dostępu. Aby uzyskać więcej informacji na temat panelu dostępu, zobacz [wprowadzenie do panelu dostępu](active-directory-saas-access-panel-introduction.md).

