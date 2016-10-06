<properties 
    pageTitle="Usługa Azure Multi-Factor Authentication — wprowadzenie"
    description="Wybierz odpowiednie rozwiązanie zabezpieczające oparte na uwierzytelnianiu wieloskładnikowym, uwzględniając lokalizację użytkowników i zasoby, które chcesz zabezpieczyć.  Następnie wybierz chmurę lub serwer usługi MFA albo usługi AD FS."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtland"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/15/2016"
    ms.author="kgremban"/>


#Wybieranie odpowiedniego rozwiązania zabezpieczającego opartego na uwierzytelnianiu wieloskładnikowym

Istnieje kilka typów usługi Azure Multi-Factor Authentication, dlatego trzeba ustalić kilka rzeczy, aby wybrać właściwą wersję tej usługi.  Należy uwzględnić następujące kwestie:

-   [Co chcesz zabezpieczyć](#what-am-i-trying-to-secure)
-   [Gdzie znajdują się użytkownicy](#where-are-the-users-located)

Wskazówki zawarte w poniższych sekcjach ułatwią znalezienie odpowiedzi na te pytania.

## Co chcesz zabezpieczyć?

Aby wybrać odpowiednie rozwiązanie oparte na uwierzytelnianiu wieloskładnikowym, najpierw należy określić, co ma zostać objęte ochroną przy użyciu drugiej metody uwierzytelniania.  Czy jest to aplikacja na platformie Azure?  A może system dostępu zdalnego?  Po określeniu elementów, które mają zostać zabezpieczone, spróbujemy odpowiedzieć na pytanie o miejsce włączenia uwierzytelniania wieloskładnikowego.  


Co chcesz zabezpieczyć| Usługa Multi-Factor Authentication w chmurze|Serwer Multi-Factor Authentication
------------- | :-------------: | :-------------: |
Aplikacje firmy Microsoft|* |* |
Aplikacje SaaS w galerii aplikacji|* |* |
Aplikacje usług IIS opublikowane za pośrednictwem serwera proxy aplikacji usługi Azure AD|* |* |
Aplikacje usług IIS, które nie zostały opublikowane za pośrednictwem serwera proxy aplikacji usługi Azure AD | |* |
Dostęp zdalny, np. sieć VPN lub brama usług pulpitu zdalnego| |* |



## Gdzie znajdują się użytkownicy

Następnie w zależności od tego, gdzie znajdują się użytkownicy, można określić, jakie rozwiązanie będzie odpowiednie — uwierzytelnianie wieloskładnikowe w chmurze czy uwierzytelnianie lokalne przy użyciu serwera MFA.



Lokalizacja użytkowników| Rozwiązanie
------------- | :------------- |
Usługa Azure Active Directory| Usługa Multi-Factor Authentication w chmurze|
Usługa Azure AD i lokalna usługa AD przy użyciu federacji z usługami AD FS| Dostępne opcje obejmują zarówno usługę MFA w chmurze, jak i serwer MFA
Usługa Azure AD i lokalna usługa AD używana z narzędziem DirSync, Azure AD Sync, Azure AD Connect — brak synchronizacji haseł|Dostępne opcje obejmują zarówno usługę MFA w chmurze, jak i serwer MFA
Usługa Azure AD i lokalna usługa AD używana z narzędziem DirSync, Azure AD Sync, Azure AD Connect — z synchronizacją haseł|Usługa Multi-Factor Authentication w chmurze
Lokalna usługa Active Directory|Serwer Multi-Factor Authentication

Poniższa tabela zawiera zestawienie funkcji dostępnych w przypadku użycia usługi Multi-Factor Authentication w chmurze i serwera Multi-Factor Authentication.

 | Usługa Multi-Factor Authentication w chmurze | Serwer Multi-Factor Authentication
------------- | :-------------: | :-------------: |
Powiadomienie w aplikacji mobilnej jako drugi składnik | ● | ● |
Kod weryfikacyjny w aplikacji mobilnej jako drugi składnik | ● | ●
Połączenie telefoniczne jako drugi składnik | ● | ●
Jednokierunkowa wiadomość SMS jako drugi składnik | ● | ●
Dwukierunkowa wiadomość SMS jako drugi składnik |  | ●
Tokeny sprzętowe jako drugi składnik |  | ●
Hasła aplikacji w przypadku klientów, którzy nie obsługują usługi MFA | ● |  
Kontrola administracyjna nad metodami uwierzytelniania | ● | ●
Tryb numeru PIN |  | ●
Alert dotyczący wykrycia oszustwa | ● | ●
Raporty usługi MFA | ● | ●
Jednorazowe obejście |  | ●
Niestandardowe powitania dla połączeń telefonicznych | ● | ●
Możliwość dostosowania identyfikacji numeru dla połączeń telefonicznych | ● | ●
Zaufane adresy IP | ● | ●
Pamiętanie uwierzytelniania MFA w przypadku zaufanych urządzeń  | ● |  
Dostęp warunkowy | ● | ●
Pamięć podręczna |  | ●

Gdy już udało się ustalić, czy będzie używane uwierzytelnianie wieloskładnikowe w chmurze czy lokalny serwer MFA, można rozpocząć konfigurowanie i używanie usługi Azure Multi-Factor Authentication.   **Wybierz ikonę, która odpowiada Twojemu scenariuszowi.**

<center>




[![Chmura](./media/multi-factor-authentication-get-started/cloud2.png)](multi-factor-authentication-get-started-cloud.md) &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[![Weryfikacja](./media/multi-factor-authentication-get-started/server2.png)](multi-factor-authentication-get-started-server.md) &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
</center>



<!--HONumber=Sep16_HO4-->


