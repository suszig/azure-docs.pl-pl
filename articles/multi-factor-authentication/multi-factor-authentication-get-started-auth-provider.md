---
title: "Wprowadzenie do dostawcy usługi Azure Multi-Factor Authentication | Microsoft Docs"
description: "Dowiedz się, jak utworzyć dostawcę usługi Azure Multi-Factor Authentication."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: a7dd5030-7d40-4654-8fbd-88e53ddc1ef5
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/28/2017
ms.author: kgremban
ms.reviewer: yossib
ms.custom: it-pro
ms.openlocfilehash: ed14a5a762bab20a1ccde699504dd21f25009b52
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="getting-started-with-an-azure-multi-factor-auth-provider"></a>Wprowadzenie do dostawcy usługi Azure Multi-Factor Authentication
Weryfikacja dwuetapowa jest domyślnie dostępna dla administratorów globalnych, którzy zarządzają użytkownikami usług Azure Active Directory i Office 365. Jeśli jednak chcesz korzystać z [funkcji zaawansowanych](multi-factor-authentication-whats-next.md), musisz kupić pełną wersję usługi Azure Multi-Factor Authentication (MFA).

Funkcje oferowane w pełnej wersji usługi Azure MFA są dostępne za pośrednictwem dostawcy usługi Azure Multi-Factor Authentication. Jest on przeznaczony dla użytkowników, którzy **nie mają licencji usługi Azure MFA lub Azure AD w wersji Premium ani licencji pakietu Enterprise Mobility + Security (EMS)**.  Usługa Azure MFA, usługa Azure AD w wersja Premium oraz pakiet EMS domyślnie zawierają pełną wersję usługi Azure MFA. Jeśli masz te licencje, nie potrzebujesz dostawcy usługi Azure Multi-Factor Authentication.

Dostawca usługi Azure Multi-Factor Authentication jest wymagany do pobrania zestawu SDK.

> [!IMPORTANT]
> Aby pobrać zestaw SDK, musisz utworzyć dostawcę usługi Azure Multi-Factor Authentication, nawet jeśli masz licencje usług Azure MFA, AAD Premium lub EMS.  Jeśli tworzysz dostawcę usługi Azure Multi-Factor Authentication w tym celu i masz już licencje, pamiętaj o utworzeniu go przy użyciu modelu **Za włączonego użytkownika**. Następnie połącz dostawcę z katalogiem zawierającym licencje usług Azure MFA, Azure AD Premium lub EMS. Dzięki tej konfiguracji będziesz ponosić dodatkowe opłaty jedynie w przypadku, gdy liczba unikatowych użytkowników korzystających z weryfikacji dwuetapowej przekroczy liczbę posiadanych przez Ciebie licencji.

## <a name="what-is-an-azure-multi-factor-auth-provider"></a>Co to jest dostawca usługi Azure Multi-Factor Authentication?

Jeśli nie masz licencji na korzystanie z usługi Azure Multi-Factor Authentication, możesz utworzyć dostawcę uwierzytelniania w celu wdrożenia weryfikacji dwuetapowej użytkowników. Jeśli tworzysz aplikację niestandardową i chcesz włączyć usługę Azure MFA, utwórz dostawcę uwierzytelniania i [pobierz zestaw SDK](multi-factor-authentication-sdk.md).

Istnieją dwa typy dostawców uwierzytelniania, które różnią się sposobem naliczania opłat za korzystanie z subskrypcji platformy Azure. W modelu opartym na uwierzytelnianiu jest uwzględniana liczba uwierzytelnień w dzierżawie w ciągu miesiąca. Ta opcja jest przydatna, jeśli masz pewną liczbę użytkowników, którzy uwierzytelniają się tylko czasami, na przykład gdy aplikacja niestandardowa wymaga uwierzytelniania wieloskładnikowego. W modelu opartym na użytkownikach jest uwzględniana liczba osób w dzierżawie, które w danym miesiącu korzystają z weryfikacji dwuetapowej. Ta opcja jest przydatna, jeśli masz użytkowników z licencjami, ale musisz objąć uwierzytelnianiem wieloskładnikowym dodatkowych użytkowników, którzy nie mają licencji.

## <a name="create-a-multi-factor-auth-provider"></a>Tworzenie dostawcy usługi Multi-Factor Authentication
Aby utworzyć dostawcę usługi Azure Multi-Factor Authentication, postępuj zgodnie z poniższymi instrukcjami. Dostawców usługi Azure Multi-Factor Auth można utworzyć tylko w klasycznej witrynie Azure Portal. Jeśli nie możesz się zalogować do klasycznej witryny Azure Portal, upewnij się, że dzierżawa usługi Azure AD jest [skojarzona z subskrypcją platformy Azure](../active-directory/active-directory-how-subscriptions-associated-directory.md). 

1. Zaloguj się do [klasycznego portalu Azure](https://manage.windowsazure.com) jako administrator.
2. W obszarze po lewej stronie wybierz pozycję **Active Directory**.
3. Na górze strony usługi Active Directory wybierz pozycję **Dostawcy usługi Multi-Factor Authentication**.
   
   ![Tworzenie dostawcy usługi MFA](./media/multi-factor-authentication-get-started-auth-provider/authprovider1.png)

4. Na dole strony kliknij pozycję **Nowy**.
   
   ![Tworzenie dostawcy usługi MFA](./media/multi-factor-authentication-get-started-auth-provider/authprovider2.png)

5. W obszarze App Services wybierz pozycję **Dostawca usługi Multi-Factor Authentication**
   
   ![Tworzenie dostawcy usługi MFA](./media/multi-factor-authentication-get-started-auth-provider/authprovider3.png)

6. Wybierz pozycję **Szybkie tworzenie**.
   
   ![Tworzenie dostawcy usługi MFA](./media/multi-factor-authentication-get-started-auth-provider/authprovider4.png)

7. Wypełnij poniższe pola i wybierz pozycję **Utwórz**.
   1. **Nazwa** — nazwa dostawcy usługi Multi-Factor Authentication.
   2. **Model użycia** – wybierz jedną z dwóch opcji:
      * Za uwierzytelnienie — model zakupu z opłatami za każde uwierzytelnienie. Zazwyczaj używany w scenariuszach, w których usługa Azure Multi-Factor Authentication jest używana w aplikacjach udostępnianych klientom.
      * Za włączonego użytkownika – model zakupu z opłatami za każdego włączonego użytkownika. Zazwyczaj używany w scenariuszach obejmujących korzystanie przez pracowników z aplikacji, takich jak Office 365. Wybierz tę opcję, jeśli niektórzy z Twoich użytkowników mają już licencje usługi Azure MFA.
   3. **Katalog** — dzierżawa usługi Azure Active Directory, z którą jest skojarzony dostawca usługi Multi-Factor Authentication. Należy pamiętać o następujących kwestiach:
      * Do utworzenia dostawcy usługi Multi-Factor Authentication nie jest potrzebny katalog usługi Azure AD. Jeśli zamierzasz tylko pobrać serwer usługi Azure Multi-Factor Authentication lub zestaw SDK, pozostaw to pole puste.
      * Aby móc korzystać z zaawansowanych funkcji, dostawca usługi Multi-Factor Auth musi być skojarzony z katalogiem usługi Azure AD.
      * Z każdym katalogiem usługi Azure AD może być skojarzony tylko jeden dostawca usługi Multi-Factor Auth.  
      ![Tworzenie dostawcy usługi MFA](./media/multi-factor-authentication-get-started-auth-provider/authprovider5.png)

8. Po kliknięciu pozycji Utwórz zostanie utworzony dostawca usługi Multi-Factor Authentication i pojawi się komunikat: **Pomyślnie utworzono dostawcę usługi Multi-Factor Authentication**. Kliknij przycisk **OK**.  
   
   ![Tworzenie dostawcy usługi MFA](./media/multi-factor-authentication-get-started-auth-provider/authprovider6.png)  

## <a name="manage-your-multi-factor-auth-provider"></a>Zarządzanie dostawcą usługi Multi-Factor Auth

Po utworzeniu dostawcy usługi MFA nie możesz zmienić modelu użycia (na włączonego użytkownika lub na uwierzytelnienie). Możesz jednak usunąć dostawcę usługi MFA, a następnie utworzyć dostawcę z innym modelem użycia.

Jeśli bieżący dostawca usługi Multi-Factor Auth jest skojarzony z katalogiem usługi Azure AD (znanym również jako dzierżawa usługi Azure AD), możesz bezpiecznie usunąć dostawcę usługi MFA, a następnie utworzyć dostawcę połączonego z tą samą dzierżawą usługi Azure AD. Alternatywnie, jeśli masz zakupionych dostatecznie dużo licencji MFA, Azure AD Premium lub Enterprise Mobility + Security (EMS), aby objęły one wszystkich użytkowników włączonych do usługi MFA, możesz zupełnie usunąć dostawcę usługi MFA.

Jeśli dostawca usługi MFA nie jest połączony z dzierżawą usługi Azure AD lub łączysz nowego dostawcę usługi MFA z inną dzierżawą usługi Azure AD, ustawienia użytkownika i opcje konfiguracji nie są przenoszone. Ponadto należy ponownie aktywować istniejące serwery usługi Azure MFA przy użyciu poświadczeń aktywacji wygenerowanych za pośrednictwem nowego dostawcy usługi MFA. Ponowne aktywowanie serwerów usługi MFA w celu ich połączenia z nowym dostawcą usługi MFA nie wpływa na uwierzytelnianie za pośrednictwem połączeń telefonicznych i wiadomości SMS, ale powiadomienia w aplikacji mobilnej przestaną działać dla wszystkich użytkowników do momentu, w którym ponownie aktywują aplikację mobilną.

## <a name="next-steps"></a>Następne kroki

[Pobieranie pakietu SDK usługi Multi-Factor Authentication](multi-factor-authentication-sdk.md)

[Konfigurowanie ustawień usługi Multi-Factor Authentication](multi-factor-authentication-whats-next.md)
