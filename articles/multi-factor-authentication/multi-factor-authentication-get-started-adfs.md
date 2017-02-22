---
title: "Weryfikacja dwuetapowa i usługi AD FS — Azure MFA | Microsoft Docs"
description: "Ta strona dotyczy usługi Azure Multi-Factor Authentication i zawiera informacje umożliwiające rozpoczęcie korzystania z usługi Azure MFA i usług AD FS."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: 44fbba68-6cf9-46c1-a9df-736580b68ae3
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/23/2017
ms.author: kgremban
translationtype: Human Translation
ms.sourcegitcommit: 00ee90b6d264c1aee4b488cd1c2a5447809a1e3c
ms.openlocfilehash: 0981be75b9caffb29b10c70ce528209984986ce2


---
# <a name="getting-started-with-azure-multi-factor-authentication-and-active-directory-federation-services"></a>Wprowadzenie do usługi Azure Multi-Factor Authentication i usług Active Directory Federation Services
<center>![Chmura](./media/multi-factor-authentication-get-started-adfs/adfs.png)</center>

W przypadku organizacji, które sfederowały lokalną usługę Active Directory z usługą Azure Active Directory za pomocą usług AD FS, dostępne są dwie opcje używania usługi Azure Multi-Factor Authentication.

* Zabezpieczenie zasobów w chmurze za pomocą usługi Azure Multi-Factor Authentication lub usług Active Directory Federation Services
* Zabezpieczenie zasobów w chmurze i zasobów lokalnych przy użyciu serwera usługi Azure Multi-Factor Authentication

Poniższa tabela zawiera podsumowanie obsługi weryfikacji w przypadku zabezpieczania zasobów za pomocą usługi Azure Multi-Factor Authentication i usług AD FS

| Proces weryfikacji — aplikacje korzystające z przeglądarki | Proces weryfikacji — aplikacje niekorzystające z przeglądarki |
|:--- |:--- |:--- |
| Zabezpieczanie zasobów usługi Azure AD za pomocą usługi Azure Multi-Factor Authentication |<li>Pierwszy etap weryfikacji odbywa się lokalnie przy użyciu usług AD FS.</li> <li>Drugi etap polega na użyciu metody telefonicznej, która obejmuje uwierzytelnianie w chmurze.</li> |
| Zabezpieczanie zasobów usługi Azure AD za pomocą usług Active Directory Federation Services |<li>Pierwszy etap weryfikacji odbywa się lokalnie przy użyciu usług AD FS.</li><li>Drugi etap odbywa się lokalnie i polega na uznaniu oświadczenia.</li> |

Zastrzeżenia dotyczące haseł aplikacji używanych przez użytkowników federacyjnych:

* Hasła aplikacji są weryfikowane przy użyciu uwierzytelniania w chmurze, co pozwala pominąć federację. Federacja jest aktywnie używana tylko podczas konfigurowania hasła aplikacji.
* Ustawienia lokalnej kontroli dostępu klienta nie są uznawane przez hasła aplikacji.
* Nie będzie można lokalnie rejestrować uwierzytelniania haseł aplikacji.
* Wyłączenie/usunięcie konta przy użyciu narzędzia do synchronizacji katalogów może potrwać do&3; godzin, powodując opóźnienie wyłączenia/usunięcia haseł aplikacji w tożsamości w chmurze.

## <a name="next-steps"></a>Następne kroki
Aby uzyskać informacje dotyczące konfigurowania usługi Azure Multi-Factor Authentication lub serwera usługi Azure Multi-Factor Authentication z usługami AD FS, zobacz poniższe artykuły:

* [Zabezpieczanie zasobów w chmurze przy użyciu usługi Azure Multi-Factor Authentication i usług AD FS](multi-factor-authentication-get-started-adfs-cloud.md)
* [Zabezpieczanie zasobów w chmurze i lokalnych przy użyciu serwera usługi Azure Multi-Factor Authentication i usług AD FS systemu Windows Server 2012 R2](multi-factor-authentication-get-started-adfs-w2k12.md)
* [Zabezpieczanie zasobów w chmurze i zasobów lokalnych przy użyciu serwera usługi Azure Multi-Factor Authentication i usług AD FS 2.0](multi-factor-authentication-get-started-adfs-adfs2.md)



<!--HONumber=Jan17_HO4-->


