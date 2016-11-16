---
title: "Serwer a chmura usługi Azure MFA | Microsoft Docs"
description: "Wybierz odpowiednie rozwiązanie zabezpieczające oparte na uwierzytelnianiu wieloskładnikowym, uwzględniając lokalizację użytkowników i zasoby, które chcesz zabezpieczyć.  Następnie wybierz chmurę lub serwer usługi MFA albo usługi AD FS."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: ec2270ea-13d7-4ebc-8a00-fa75ce6c746d
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/14/2016
ms.author: kgremban
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 72347099d980f2ca73f39f984787197e1f87e45a


---
# <a name="choose-the-azure-multifactor-authentication-solution-for-you"></a>Wybieranie odpowiedniego rozwiązania usługi Azure Multi-Factor Authentication
Istnieje kilka typów usługi Azure Multi-Factor Authentication (MFA), dlatego należy udzielić odpowiedzi na kilka pytań, aby wybrać właściwą wersję tej usługi.  Oto te pytania:

* [Co chcesz zabezpieczyć?](#what-am-i-trying-to-secure)
* [Gdzie znajdują się użytkownicy?](#where-are-the-users-located)
* [Jakich funkcji potrzebujesz?](#what-featured-do-i-need)

Wskazówki zawarte w poniższych sekcjach ułatwią znalezienie odpowiedzi na te pytania.

## <a name="what-am-i-trying-to-secure"></a>Co chcesz zabezpieczyć?
Aby wybrać odpowiednie rozwiązanie weryfikacji dwuetapowej, najpierw należy określić, co ma zostać objęte ochroną przy użyciu drugiej metody uwierzytelniania.  Czy jest to aplikacja na platformie Azure?  Czy może system z dostępem zdalnym?  Po określeniu elementów, które mają zostać zabezpieczone, będzie można znaleźć odpowiedź na pytanie o miejsce włączenia usługi Multi-Factor Authentication.  

| Co chcesz zabezpieczyć | Usługa Multi-Factor Authentication w chmurze | Serwer Multi-Factor Authentication |
| --- |:---:|:---:|
| Aplikacje firmy Microsoft |● |● |
| Aplikacje SaaS w galerii aplikacji |● |● |
| Aplikacje usług IIS opublikowane za pośrednictwem serwera proxy aplikacji usługi Azure AD |● |● |
| Aplikacje usług IIS, które nie zostały opublikowane za pośrednictwem serwera proxy aplikacji usługi Azure AD | |● |
| Dostęp zdalny, np. sieć VPN lub brama usług pulpitu zdalnego | |● |

## <a name="where-are-the-users-located"></a>Gdzie znajdują się użytkownicy
Następnie w zależności od tego, gdzie znajdują się użytkownicy, można określić, jakie rozwiązanie będzie odpowiednie — uwierzytelnianie w chmurze czy uwierzytelnianie lokalne przy użyciu serwera MFA.

| Lokalizacja użytkowników | Usługa Multi-Factor Authentication w chmurze | Serwer Multi-Factor Authentication |
| --- |:---:|:---:|
| Usługa Azure Active Directory |● | |
| Usługa Azure AD i lokalna usługa AD przy użyciu federacji z usługami AD FS |● |● |
| Usługa Azure AD i lokalna usługa AD używana z narzędziem DirSync, Azure AD Sync, Azure AD Connect — brak synchronizacji haseł |● |● |
| Usługa Azure AD i lokalna usługa AD używana z narzędziem DirSync, Azure AD Sync, Azure AD Connect — z synchronizacją haseł |● | |
| Lokalna usługa Active Directory | |● |

## <a name="what-features-do-i-need"></a>Jakich funkcji potrzebujesz?
Poniższa tabela zawiera zestawienie funkcji dostępnych w przypadku użycia usługi Multi-Factor Authentication w chmurze i serwera Multi-Factor Authentication.

| Usługa Multi-Factor Authentication w chmurze | Serwer Multi-Factor Authentication |
| --- |:---:|:---:|
| Powiadomienie w aplikacji mobilnej jako drugi składnik |● |
| Kod weryfikacyjny w aplikacji mobilnej jako drugi składnik |● |
| Połączenie telefoniczne jako drugi składnik |● |
| Jednokierunkowa wiadomość SMS jako drugi składnik |● |
| Dwukierunkowa wiadomość SMS jako drugi składnik | |
| Tokeny sprzętowe jako drugi składnik | |
| Hasła aplikacji w przypadku klientów, którzy nie obsługują usługi MFA |● |
| Kontrola administracyjna nad metodami uwierzytelniania |● |
| Tryb numeru PIN | |
| Alert dotyczący wykrycia oszustwa |● |
| Raporty usługi MFA |● |
| Jednorazowe obejście | |
| Niestandardowe powitania dla połączeń telefonicznych |● |
| Możliwość dostosowania identyfikacji numeru dla połączeń telefonicznych |● |
| Zaufane adresy IP |● |
| Pamiętanie uwierzytelniania MFA w przypadku zaufanych urządzeń |● |
| Dostęp warunkowy |● |
| Pamięć podręczna | |

Gdy już udało się ustalić, czy będzie używane uwierzytelnianie wieloskładnikowe w chmurze czy lokalny serwer MFA, można rozpocząć konfigurowanie i używanie usługi Azure Multi-Factor Authentication. **Wybierz ikonę, która odpowiada Twojemu scenariuszowi.**

<center>




[![Chmura](./media/multi-factor-authentication-get-started/cloud2.png)](multi-factor-authentication-get-started-cloud.md)  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[![Weryfikacja](./media/multi-factor-authentication-get-started/server2.png)](multi-factor-authentication-get-started-server.md) &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
</center>



<!--HONumber=Nov16_HO2-->


