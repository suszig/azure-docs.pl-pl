<properties 
    pageTitle="Wprowadzenie do usługi Azure Multi-Factor Authentication i usług Active Directory Federation Services" 
    description="Ta strona dotyczy usługi Azure Multi-Factor Authentication i zawiera informacje umożliwiające rozpoczęcie korzystania z usługi Azure MFA i usług AD FS." 
    services="multi-factor-authentication" 
    documentationCenter="" 
    authors="billmath" 
    manager="stevenpo" 
    editor="curtland"/>

<tags 
    ms.service="multi-factor-authentication" 
    ms.workload="identity" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" ms.topic="get-started-article" 
    ms.date="08/04/2016" 
    ms.author="billmath"/>

# Wprowadzenie do usługi Azure Multi-Factor Authentication i usług Active Directory Federation Services



<center>![Chmura](./media/multi-factor-authentication-get-started-adfs/adfs.png)</center>

W przypadku organizacji, które sfederowały lokalną usługę Active Directory z usługą Azure Active Directory za pomocą usług AD FS, dostępne są dwie opcje używania usługi Azure Multi-Factor Authentication.

- Zabezpieczenie zasobów w chmurze za pomocą usługi Azure Multi-Factor Authentication lub usług Active Directory Federation Services 
- Zabezpieczenie zasobów w chmurze i zasobów lokalnych przy użyciu serwera usługi Azure Multi-Factor Authentication 

Poniższa tabela zawiera podsumowanie obsługi uwierzytelniania w przypadku zabezpieczania zasobów za pomocą usługi Azure Multi-Factor Authentication i usług AD FS

|Obsługa uwierzytelniania — aplikacje korzystające z przeglądarki|Obsługa uwierzytelniania — aplikacje niekorzystające z przeglądarki
:------------- | :------------- | :------------- |
Zabezpieczanie zasobów usługi Azure AD za pomocą usługi Azure Multi-Factor Authentication |<li>Pierwszy etap uwierzytelniania odbywa się lokalnie przy użyciu usług AD FS.</li> <li>Drugi etap polega na użyciu metody telefonicznej, która obejmuje uwierzytelnianie w chmurze.</li>|Użytkownicy końcowi mogą używać haseł aplikacji w celu zalogowania.
Zabezpieczanie zasobów usługi Azure AD za pomocą usług Active Directory Federation Services |<li>Pierwszy etap uwierzytelniania odbywa się lokalnie przy użyciu usług AD FS.</li><li>Drugi etap odbywa się lokalnie i polega na uznaniu oświadczenia.</li>|Użytkownicy końcowi mogą używać haseł aplikacji w celu zalogowania.

Zastrzeżenia dotyczące haseł aplikacji używanych przez użytkowników federacyjnych: 

- Hasło aplikacji jest weryfikowane przy użyciu uwierzytelniania w chmurze, co pozwala pominąć federacje. Federacja jest aktywnie używana tylko podczas konfigurowania hasła aplikacji.
- Ustawienia lokalnej kontroli dostępu klienta nie są uznawane przez hasło aplikacji.
- Nie będzie można lokalnie rejestrować uwierzytelniania hasła aplikacji.
- Wyłączenie/usunięcie konta przy użyciu narzędzia DirSync może potrwać do 3 godzin, powodując opóźnienie wyłączenia/usunięcia hasła aplikacji w tożsamości w chmurze.

Aby uzyskać informacje dotyczące konfigurowania usługi Azure Multi-Factor Authentication lub serwera usługi Azure Multi-Factor Authentication z usługami AD FS, zobacz poniższe artykuły:

- [Zabezpieczanie zasobów w chmurze przy użyciu usługi Azure Multi-Factor Authentication i usług AD FS](multi-factor-authentication-get-started-adfs-cloud.md)
- [Zabezpieczanie zasobów w chmurze i lokalnych przy użyciu serwera usługi Azure Multi-Factor Authentication i usług AD FS systemu Windows Server 2012 R2](multi-factor-authentication-get-started-adfs-w2k12.md)
- [Zabezpieczanie zasobów w chmurze i zasobów lokalnych przy użyciu serwera usługi Azure Multi-Factor Authentication i usług AD FS 2.0](multi-factor-authentication-get-started-adfs-adfs2.md)







 



<!--HONumber=sep16_HO1-->


