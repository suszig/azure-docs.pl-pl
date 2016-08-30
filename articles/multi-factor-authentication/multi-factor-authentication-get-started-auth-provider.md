<properties 
    pageTitle="Wprowadzenie do dostawcy usługi Microsoft Azure Multi-Factor Authentication" 
    description="Dowiedz się, jak utworzyć dostawcę usługi Azure Multi-Factor Authentication." 
    services="multi-factor-authentication" 
    documentationCenter="" 
    authors="billmath" 
    manager="stevenpo" 
    editor="curtand"/>

<tags 
    ms.service="multi-factor-authentication" 
    ms.workload="identity" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="05/12/2016" 
    ms.author="billmath"/>



# Wprowadzenie do dostawcy usługi Azure Multi-Factor Authentication
Uwierzytelnianie wieloskładnikowe jest domyślnie dostępne dla administratorów globalnych, którzy zarządzają użytkownikami usług Azure Active Directory i Office 365. Jednak jeśli chcesz korzystać z [funkcji zaawansowanych](multi-factor-authentication-whats-next.md), musisz kupić pełną wersję usługi Azure MFA. 

> [AZURE.NOTE]  Funkcje oferowane w pełnej wersji usługi Azure MFA są dostępne za pośrednictwem dostawcy usługi Azure Multi-Factor Authentication. Jest on przeznaczony dla użytkowników, którzy **nie mają licencji usługi Azure MFA lub usługi Azure AD w wersji Premium ani licencji pakietu EMS**.  Usługa Azure MFA, usługa Azure AD w wersja Premium oraz pakiet EMS domyślnie zawierają pełną wersję usługi Azure MFA.  Jeśli masz te licencje, nie potrzebujesz dostawcy usługi Azure Multi-Factor Authentication. 
 
Aby utworzyć dostawcę usługi Azure Multi-Factor Authentication, postępuj zgodnie z poniższymi instrukcjami.

## Aby utworzyć dostawcę usługi Multi-Factor Authentication
--------------------------------------------------------------------------------

1. Zaloguj się do **klasycznego portalu Azure** jako administrator.
2. W obszarze po lewej stronie wybierz pozycję **Active Directory**.
3. Na górze strony usługi Active Directory wybierz pozycję **Dostawcy usługi Multi-Factor Authentication**.
![Tworzenie dostawcy usługi MFA](./media/multi-factor-authentication-get-started-auth-provider/authprovider1.png)
4. Na dole strony kliknij pozycję **Nowy**.
![Tworzenie dostawcy usługi MFA](./media/multi-factor-authentication-get-started-auth-provider/authprovider2.png)
5. W obszarze **Usługi aplikacji** wybierz pozycję **Dostawca usługi Multi-Factor Auth**
![Tworzenie dostawcy usługi MFA](./media/multi-factor-authentication-get-started-auth-provider/authprovider3.png)
6. Wybierz pozycję **Szybkie tworzenie**.
![Tworzenie dostawcy usługi MFA](./media/multi-factor-authentication-get-started-auth-provider/authprovider4.png)
5. Wypełnij poniższe pola i wybierz pozycję **Utwórz**.
    1. **Nazwa** — nazwa dostawcy usługi Multi-Factor Authentication.
    2. **Model użycia** — model użycia dostawcy usługi Multi-Factor Authentication.
        - Za uwierzytelnienie — model zakupu z opłatami za każde uwierzytelnienie. Zazwyczaj używany w scenariuszach, w których usługa Azure Multi-Factor Authentication jest używana w aplikacjach udostępnianych klientom.
        - Za włączonego użytkownika – model zakupu z opłatami za każdego włączonego użytkownika. Zazwyczaj używany w scenariuszach obejmujących korzystanie przez pracowników z aplikacji, takich jak Office 365.
    2. **Katalog** — dzierżawa usługi Azure Active Directory, z którą jest skojarzony dostawca usługi Multi-Factor Authentication. Należy pamiętać o następujących kwestiach:
        - Do utworzenia dostawcy usługi Multi-Factor Authentication nie jest potrzebny katalog usługi Azure AD.  Jeśli zamierzasz używać tylko serwera usługi Azure Multi-Factor Authentication lub zestawu SDK, po prostu pozostaw to pole puste.
        - Aby móc korzystać z zaawansowanych funkcji, dostawca usługi Multi-Factor Auth musi być skojarzony z katalogiem usługi Azure AD.
        - Narzędzia Azure AD Connect, AAD Sync lub DirSync są wymagane tylko w przypadku synchronizowania lokalnego środowiska usługi Active Directory z katalogiem usługi Azure AD.  Jeśli używasz jedynie niesynchronizowanego katalogu usługi Azure AD, nie będą potrzebne.![Tworzenie dostawcy usługi MFA](./media/multi-factor-authentication-get-started-auth-provider/authprovider5.png)    
5. Po kliknięciu pozycji Utwórz zostanie utworzony dostawca usługi Multi-Factor Authentication i pojawi się komunikat: **Pomyślnie utworzono dostawcę usługi Multi-Factor Authentication**. Kliknij przycisk **OK**.
![Tworzenie dostawcy usługi MFA](./media/multi-factor-authentication-get-started-auth-provider/authprovider6.png)    


<!--HONumber=jun16_HO2-->


