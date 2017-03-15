---
title: "Wprowadzenie do dostawcy usługi Azure Multi-Factor Authentication | Microsoft Docs"
description: "Dowiedz się, jak utworzyć dostawcę usługi Azure Multi-Factor Authentication."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: a7dd5030-7d40-4654-8fbd-88e53ddc1ef5
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 02/24/2017
ms.author: kgremban
translationtype: Human Translation
ms.sourcegitcommit: 2f03ba60d81e97c7da9a9fe61ecd419096248763
ms.openlocfilehash: b0c3c5dfd695b28b75bf44c9f835b34966413a9a
ms.lasthandoff: 03/04/2017


---
# <a name="getting-started-with-an-azure-multi-factor-auth-provider"></a>Wprowadzenie do dostawcy usługi Azure Multi-Factor Authentication
Weryfikacja dwuetapowa jest domyślnie dostępna dla administratorów globalnych, którzy zarządzają użytkownikami usług Azure Active Directory i Office 365. Jeśli jednak chcesz korzystać z [funkcji zaawansowanych](multi-factor-authentication-whats-next.md), musisz kupić pełną wersję usługi Azure Multi-Factor Authentication (MFA).

> [!NOTE]
> Funkcje oferowane w pełnej wersji usługi Azure MFA są dostępne za pośrednictwem dostawcy usługi Azure Multi-Factor Authentication. Jest on przeznaczony dla użytkowników, którzy **nie mają licencji usługi Azure MFA lub usługi Azure AD w wersji Premium ani licencji pakietu EMS**.  Usługa Azure MFA, usługa Azure AD w wersja Premium oraz pakiet EMS domyślnie zawierają pełną wersję usługi Azure MFA.  Jeśli masz te licencje, nie potrzebujesz dostawcy usługi Azure Multi-Factor Authentication.

Dostawca usługi Azure Multi-Factor Authentication jest wymagany do pobrania zestawu SDK.

> [!IMPORTANT]
> Aby pobrać zestaw SDK, utwórz dostawcę usługi Azure Multi-Factor Authentication, nawet jeśli masz licencje usług Azure MFA, AAD Premium lub EMS.  Jeśli tworzysz dostawcę usługi Azure Multi-Factor Authentication w tym celu i masz już licencje, pamiętaj o utworzeniu go przy użyciu modelu **Za włączonego użytkownika**. Następnie połącz dostawcę z katalogiem zawierającym licencje usług Azure MFA, Azure AD Premium lub EMS.  Dzięki tej konfiguracji będziesz ponosić dodatkowe opłaty jedynie w przypadku, gdy liczba unikatowych użytkowników korzystających z weryfikacji dwuetapowej przekroczy liczbę posiadanych przez Ciebie licencji.

## <a name="create-a-multi-factor-auth-provider"></a>Tworzenie dostawcy usługi Multi-Factor Authentication
Aby utworzyć dostawcę usługi Azure Multi-Factor Authentication, postępuj zgodnie z poniższymi instrukcjami.

1. Zaloguj się do [klasycznego portalu Azure](https://manage.windowsazure.com) jako administrator.
2. W obszarze po lewej stronie wybierz pozycję **Active Directory**.
3. Na górze strony usługi Active Directory wybierz pozycję **Dostawcy usługi Multi-Factor Authentication**.
   ![Tworzenie dostawcy usługi MFA](./media/multi-factor-authentication-get-started-auth-provider/authprovider1.png)
4. Na dole strony kliknij pozycję **Nowy**.
   ![Tworzenie dostawcy usługi MFA](./media/multi-factor-authentication-get-started-auth-provider/authprovider2.png)
5. W obszarze App Services wybierz pozycję **Multi-Factor Auth**
   ![Tworzenie dostawcy usługi MFA](./media/multi-factor-authentication-get-started-auth-provider/authprovider3.png)
6. Wybierz pozycję **Szybkie tworzenie**.
   ![Tworzenie dostawcy usługi MFA](./media/multi-factor-authentication-get-started-auth-provider/authprovider4.png)
7. Wypełnij poniższe pola i wybierz pozycję **Utwórz**.
   1. **Nazwa** — nazwa dostawcy usługi Multi-Factor Authentication.
   2. **Model użycia** – wybierz jedną z dwóch opcji:
      * Za uwierzytelnienie — model zakupu z opłatami za każde uwierzytelnienie. Zazwyczaj używany w scenariuszach, w których usługa Azure Multi-Factor Authentication jest używana w aplikacjach udostępnianych klientom.
      * Za włączonego użytkownika – model zakupu z opłatami za każdego włączonego użytkownika. Zazwyczaj używany w scenariuszach obejmujących korzystanie przez pracowników z aplikacji, takich jak Office 365. Wybierz tę opcję, jeśli niektórzy z Twoich użytkowników mają już licencje usługi Azure MFA.
   3. **Katalog** — dzierżawa usługi Azure Active Directory, z którą jest skojarzony dostawca usługi Multi-Factor Authentication. Należy pamiętać o następujących kwestiach:
      * Do utworzenia dostawcy usługi Multi-Factor Authentication nie jest potrzebny katalog usługi Azure AD. Jeśli zamierzasz używać tylko serwera usługi Azure Multi-Factor Authentication lub zestawu SDK, pozostaw to pole puste.
      * Aby móc korzystać z zaawansowanych funkcji, dostawca usługi Multi-Factor Auth musi być skojarzony z katalogiem usługi Azure AD.
      * Narzędzia Azure AD Connect, AAD Sync lub DirSync są wymagane tylko w przypadku synchronizowania lokalnego środowiska usługi Active Directory z katalogiem usługi Azure AD.  Jeśli używasz jedynie niesynchronizowanego katalogu usługi Azure AD, nie będą potrzebne.
        ![Tworzenie dostawcy usługi MFA](./media/multi-factor-authentication-get-started-auth-provider/authprovider5.png)
8. Po kliknięciu pozycji Utwórz zostanie utworzony dostawca usługi Multi-Factor Authentication i pojawi się komunikat: **Pomyślnie utworzono dostawcę usługi Multi-Factor Authentication**. Kliknij przycisk **OK**.
   ![Tworzenie dostawcy usługi MFA](./media/multi-factor-authentication-get-started-auth-provider/authprovider6.png)


