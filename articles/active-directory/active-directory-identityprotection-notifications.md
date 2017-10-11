---
title: Azure Active Directory Identity Protection powiadomienia | Dokumentacja firmy Microsoft
description: "Dowiedz się, jak powiadomienia obsługują działaniach dochodzenia."
services: active-directory
keywords: "ochronę tożsamości usługi Azure active directory, usługa cloud app discovery, zarządzanie aplikacjami, zabezpieczeń, ryzyka, poziom ryzyka, luki w zabezpieczeniach, zasady zabezpieczeń"
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 65ca79b9-4da1-4d5b-bebd-eda776cc32c7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 079d16bbf75cd2b3b94269d684e1ae1a0e6aa967
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/03/2017
---
# <a name="azure-active-directory-identity-protection-notifications"></a>Azure Active Directory Identity Protection powiadomienia
Azure AD Identity Protection wysyła dwa rodzaje automatycznych powiadomień e-mail w celu zarządzania ryzykiem użytkownika i zdarzenia ryzyka:

* Użytkownik naruszony alertów e-mail
* Wiadomość e-mail z podsumowaniem co tydzień

## <a name="user-compromised-alert-email"></a>Użytkownik naruszony alertów e-mail
Alert ze złamanymi zabezpieczeniami poczty e-mail użytkownika jest generowany, gdy usługi Azure AD Identity Protection zidentyfikuje konto, jako naruszenia zabezpieczeń. Wiadomości e-mail zawiera łącze do użytkowników oflagowane ryzyka raportu na pulpicie nawigacyjnym Identity Protection. Zaleca się natychmiast Zbadaj powiadomienia, którego bezpieczeństwo zostało naruszone kont.

## <a name="weekly-digest-email"></a>Wiadomość e-mail z podsumowaniem co tydzień
Co tydzień wiadomość e-mail z podsumowaniem zawiera podsumowanie nowych zdarzeń ryzyka.<br>
Obejmuje on:

* Narażeni użytkownicy
* Podejrzane działania
* Wykryto luk w zabezpieczeniach
* Linki do powiązanych raportów w Identity Protection

<br>
![Korygowanie](./media/active-directory-identityprotection-notifications/400.png "korygowania")
<br>

Możesz przełączyć wysyłania co tydzień wiadomość e-mail z podsumowaniem.
<br><br>
![Ryzyko użytkownika](./media/active-directory-identityprotection-notifications/62.png "zagrożeń użytkownika")
<br>

**Aby otworzyć okno dialogowe elementami konfiguracji**:

1. Na **Azure AD Identity Protection** bloku, kliknij przycisk **ustawienia**.
   <br><br>
   ![Zasady użytkownika ryzyka](./media/active-directory-identityprotection-notifications/401.png "zasad ryzyka użytkownika")
   <br>
2. W **ogólne** kliknij **powiadomienia**.
   <br><br>
   ![Zasady użytkownika ryzyka](./media/active-directory-identityprotection-notifications/405.png "zasad ryzyka użytkownika")
   <br>

## <a name="see-also"></a>Zobacz też
* [Ochronę tożsamości usługi Azure Active Directory](active-directory-identityprotection.md)
