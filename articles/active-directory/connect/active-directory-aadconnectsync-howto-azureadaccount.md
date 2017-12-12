---
title: "Synchronizacja programu Azure AD Connect: Zarządzanie konta usługi Azure AD | Dokumentacja firmy Microsoft"
description: "W tym temacie omówiono sposób przywracania konta usługi Azure AD."
services: active-directory
keywords: "AADSTS70002, AADSTS50054, jak można zresetować hasła dla konta łącznika usługi synchronizacji Azure AD Connect"
documentationcenter: 
author: andkjell
manager: mtillman
editor: 
ms.assetid: 6077043a-27f1-4304-a44b-81dc46620f24
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 096b14f8e64ac288fe6d3956658a4b738993cea9
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/11/2017
---
# <a name="azure-ad-connect-sync-how-to-manage-the-azure-ad-service-account"></a>Synchronizacja programu Azure AD Connect: Zarządzanie konta usługi Azure AD
Konto usługi używane przez łącznik usługi Azure AD powinien być bezpłatnej usługi. Jeśli potrzebujesz zresetować swoje poświadczenia, w tym temacie jest dla Ciebie. Na przykład jeśli przez pomyłkę administratora globalnego zresetować hasło konta usługi, za pomocą programu PowerShell.

## <a name="reset-the-credentials"></a>Resetowanie poświadczeń
Jeśli konto usługi zdefiniowane w programie Azure AD Connector nie może skontaktować się z usługi Azure AD z powodu problemów z uwierzytelniania, można zresetować hasła.

1. Zaloguj się do serwera synchronizacji Azure AD Connect i uruchom program PowerShell.
2. Uruchom polecenie `Add-ADSyncAADServiceAccount`.  
   ![Addadsyncaadserviceaccount polecenia cmdlet programu PowerShell](./media/active-directory-aadconnectsync-howto-azureadaccount/addadsyncaadserviceaccount.png)
3. Podaj poświadczenia administratora globalnego usługi Azure AD.

To polecenie cmdlet resetuje hasło dla konta usługi i zaktualizować go, zarówno w usłudze Azure AD, jak i w aparacie synchronizacji.

## <a name="known-issues-these-steps-can-solve"></a>Znane problemy można rozwiązać te kroki
W tej sekcji znajduje się lista błędów zgłaszanych przez klientów, które zostały usunięte przez poświadczenia, zresetuj na koncie usługi Azure AD.

- - -
Zdarzenie 6900  
Serwer napotkał nieoczekiwany błąd podczas przetwarzania powiadomienia o zmianie hasła:  
AADSTS70002: Błąd podczas sprawdzania poprawności poświadczeń. AADSTS50054: Stare hasło jest używany do uwierzytelniania.

- - -
Zdarzenie 659  
Błąd podczas pobierania konfiguracji synchronizacji zasad haseł. Microsoft.IdentityModel.Clients.ActiveDirectory.AdalServiceException:  
AADSTS70002: Błąd podczas sprawdzania poprawności poświadczeń. AADSTS50054: Stare hasło jest używany do uwierzytelniania.

## <a name="next-steps"></a>Następne kroki
**Tematy poglądowe**

* [Synchronizacja programu Azure AD Connect: zrozumienie i dostosowywanie synchronizacji](active-directory-aadconnectsync-whatis.md)
* [Integrowanie tożsamości lokalnych z usługą Azure Active Directory](active-directory-aadconnect.md)

