---
title: "Azure Active Directory dostępu warunkowego techniczne | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak za pomocą kontroli dostępu warunkowego w usłudze Azure Active Directory. Określ warunki uwierzytelniania użytkowników i kontrolowanie dostępu do aplikacji. Gdy określone warunki są spełnione, użytkownicy są uwierzytelnieni i uzyskuje dostęp do aplikacji."
services: active-directory.
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: 56a5bade-7dcc-4dcf-8092-a7d4bf5df3c1
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/01/2017
ms.author: markvi
ms.reviewer: spunukol
ms.openlocfilehash: 8cde8e10b6789e930ea1deda2f4aa0d4a5e7ea97
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/03/2017
---
# <a name="azure-active-directory-conditional-access-technical-reference"></a>Azure Active Directory dostępu warunkowego techniczne

Można użyć [dostępu warunkowego w usłudze Azure Active Directory (Azure AD)](active-directory-conditional-access-azure-portal.md) do dopasowania, w jaki sposób autoryzowani użytkownicy mają dostęp do zasobów.  

Ten temat zawiera informacje o pomocy technicznej dla następujące opcje konfiguracji zasad dostępu warunkowego: 

- Przydziały aplikacji w chmurze

- Warunek platformy urządzenia 

- Stan aplikacji klienta

- Wymagania aplikacji zatwierdzonych klienta



## <a name="cloud-apps-assignments"></a>Przydziały aplikacji w chmurze

Po skonfigurowaniu zasad dostępu warunkowego należy [Wybierz aplikacje w chmurze, korzystających z zasadami](active-directory-conditional-access-azure-portal.md#who). 

![Wybierz zasady aplikacji w chmurze](./media/active-directory-conditional-access-technical-reference/09.png)


### <a name="microsoft-cloud-applications"></a>Aplikacje w chmurze firmy Microsoft

Zasady dostępu warunkowego można przypisać do następujących aplikacji w chmurze firmy Microsoft:

- Usługa Azure Information Protection - [Dowiedz się więcej](https://docs.microsoft.com/information-protection/get-started/faqs#i-see-azure-information-protection-is-listed-as-an-available-cloud-app-for-conditional-accesshow-does-this-work)
- Azure RemoteApp

- Microsoft Dynamics 365

- Yammer pakietu Microsoft Office 365

- Microsoft Office 365 usługi Exchange Online

- Microsoft Office 365 SharePoint Online (w tym usługi OneDrive dla firm)

- Microsoft Power BI 

- Microsoft Visual Studio Team Services

- Microsoft Teams


### <a name="other-applications"></a>Inne aplikacje 

Oprócz aplikacji firmy Microsoft w chmurze można przypisać zasady dostępu warunkowego do następujących typów aplikacji w chmurze:

- Azure AD połączonych aplikacji

- Wstępnie zintegrowanych federacyjnych aplikacja oprogramowania jako usługa (SaaS)

- Aplikacje używające hasło rejestracji jednokrotnej (SSO)

- -Aplikacji biznesowych

- Aplikacje, które używają serwera Proxy aplikacji usługi Azure AD


## <a name="device-platform-condition"></a>Warunek platformy urządzenia

W zasadach dostępu warunkowego można skonfigurować warunek platformy urządzenia powiązać zasady systemu operacyjnego na komputerze klienckim.

![Powiązanie zasady dostępu do systemu operacyjnego klienta](./media/active-directory-conditional-access-technical-reference/41.png)

Dostęp warunkowy usługi Azure AD obsługuje następujące platformy urządzeń:

- Android

- iOS

- Windows Phone

- Windows

- System macOS (wersja zapoznawcza)



## <a name="client-apps-condition"></a>Stan aplikacji klienta 

Po skonfigurowaniu zasad dostępu warunkowego można [Wybierz aplikacje klienckie](active-directory-conditional-access-azure-portal.md#client-apps) warunku aplikacji klienta. Ustaw klienta warunku aplikacji, aby przydzielić lub blokowanie dostępu, gdy podejmowana jest próba dostępu z następujących typów aplikacji klienta:

- Przeglądarka
- Aplikacje mobilne i aplikacje komputerowe

![Kontrolowanie dostępu do aplikacji klienta](./media/active-directory-conditional-access-technical-reference/03.png)

### <a name="supported-browsers"></a>Obsługiwane przeglądarki 

Kontrola dostępu w przeglądarce przy użyciu **przeglądarki** opcji w zasadach dostępu warunkowego. Dostęp tylko wtedy, gdy podejmowana próba dostępu przez obsługiwanej przeglądarki. Próba została zablokowana, gdy podejmowana jest próba dostępu przez nieobsługiwanej przeglądarki.

![Kontrolowanie dostępu do obsługiwanych przeglądarek](./media/active-directory-conditional-access-technical-reference/05.png)

W zasadach dostępu warunkowego obsługiwane są poniższe przeglądarki: 


| System operacyjny                     | Przeglądarki                    | Pomoc techniczna     |
| :--                    | :--                         | :-:         |
| Windows 10             | Program Internet Explorer, krawędzi     | ![Zaznacz][1] |
| Windows 10             | Chrome                      | ![Zaznacz][1] |
| Windows 8 / 8.1        | Program Internet Explorer, Chrome   | ![Zaznacz][1] |
| Windows 7              | Program Internet Explorer, Chrome   | ![Zaznacz][1] |
| iOS                    | Safari, Intune Managed Browser                      | ![Zaznacz][1] |
| Android                | Chrome, Intune Managed Browser                      | ![Zaznacz][1] |
| Windows Phone          | Program Internet Explorer, krawędzi     | ![Zaznacz][1] |
| Windows Server 2016    | Program Internet Explorer, krawędzi     | ![Zaznacz][1] |
| Windows Server 2016    | Chrome                      | Wkrótce |
| Windows Server 2012 R2 | Program Internet Explorer, Chrome   | ![Zaznacz][1] |
| Windows Server 2008 R2 | Program Internet Explorer, Chrome   | ![Zaznacz][1] |
| macOS                  | Safari                      | ![Zaznacz][1] |
| macOS                  | Chrome                      | Wkrótce |

> [!NOTE]
> Obsługa Chrome, należy użyć systemu Windows 10 twórców aktualizację (wersja 1703) lub nowszym.<br>
> Można zainstalować [to rozszerzenie](https://chrome.google.com/webstore/detail/windows-10-accounts/ppnbnpeolgkicgegkbkbjmhlideopiji).

### <a name="supported-mobile-applications-and-desktop-clients"></a>Obsługiwane klienci pulpitów i aplikacji mobilnych

Kontrolować dostęp do aplikacji i klienta za pomocą **aplikacji mobilnych i klasycznych klientów** opcji w zasadach dostępu warunkowego. Dostęp tylko w przypadku, gdy próba dostępu nawiązuje obsługiwanych aplikacji mobilnej lub klienta. Gdy podejmowana jest próba dostępu przez klienta lub nieobsługiwany aplikacji nie jest zablokowany.

![Kontrolowanie dostępu do obsługiwanych aplikacji mobilnych lub klienci usług pulpitu](./media/active-directory-conditional-access-technical-reference/06.png)

Następujące aplikacje mobilne i będących klientami pulpitu obsługują dostępu warunkowego dla usługi Office 365 i innych aplikacji usługi Azure AD, połączony:


| Aplikacje klienta| Usługa docelowa| Platforma |
| --- | --- | --- |
| Usługa Azure RemoteApp| Usługa Azure RemoteApp| Windows 10, Windows 8.1, Windows 7, iOS, Android i Mac OS X|
| Aplikacji programu Dynamics CRM| Dynamics CRM| Windows 10, Windows 8.1, Windows 7, iOS i Android|
| Osoby-mail/kalendarza aplikacji Outlook 2016 Outlook 2013 (z nowoczesnego uwierzytelniania)| Office 365 Exchange Online| Windows 10|
| Zasady MFA i lokalizację dla aplikacji. Urządzenia, na podstawie zasad nie są obsługiwane.| Moje aplikacje usługi aplikacji| Android i iOS|
| Kontroluje usługi zespoły firmy Microsoft — do wszystkich usług, które obsługują Teams firmy Microsoft i wszystkie jego aplikacjach klienckich - pulpitu systemu Windows, iOS, Android, WP i klienta sieci web| Microsoft Teams| Windows 10, Windows 8.1, Windows 7, iOS i Android|
| Aplikacje pakietu Office 2016, Office 2013 (z nowoczesnego uwierzytelniania), usługi OneDrive Synchronizowanie klienta (zobacz [uwagi](https://support.office.com/en-US/article/Azure-Active-Directory-conditional-access-with-the-OneDrive-sync-client-on-Windows-028d73d7-4b86-4ee0-8fb7-9a209434b04e))| Usługi Office 365 SharePoint Online| Windows 8.1, Windows 7|
| Aplikacje pakietu Office 2016, Office uniwersalnych aplikacji, Office 2013 (z nowoczesnego uwierzytelniania), klient synchronizacji usługi OneDrive (zobacz [uwagi](https://support.office.com/en-US/article/Azure-Active-Directory-conditional-access-with-the-OneDrive-sync-client-on-Windows-028d73d7-4b86-4ee0-8fb7-9a209434b04e)), obsługa grupy Office jest planowane w przyszłości, obsługi aplikacji programu SharePoint jest planowane w przyszłości| Usługi Office 365 SharePoint Online| Windows 10|
| Pakiet Office 2016 dla macOS (Word, Excel, PowerPoint, tylko w programie OneNote). OneDrive dla firm Obsługa planowanych w przyszłości| Usługi Office 365 SharePoint Online| Mac OS X|
| Aplikacje mobilne pakietu Office| Usługi Office 365 SharePoint Online| iOS i Android|
| Aplikacja usługi Yammer pakietu Office| Yammer usługi Office 365| Windows 10, iOS, Android|
| Outlook 2016 (Urząd macOS)| Office 365 Exchange Online| Mac OS X|
| Outlook 2016, Outlook 2013 (z nowoczesnego uwierzytelniania), usługi Skype dla firm (z nowoczesnego uwierzytelniania)| Office 365 Exchange Online| Windows 8.1, Windows 7|
| Outlook aplikacji mobilnej| Office 365 Exchange Online| iOS|
| Aplikacja usługi Power BI. Aplikacja usługi Power BI dla systemu Android nie obsługuje obecnie dostępu warunkowego opartego na urządzeniu.| Usługa Power BI| Windows 10, Windows 8.1, Windows 7 i iOS|
| Skype dla firm| Office 365 Exchange Online| Android i IOS|
| Visual Studio Team Services aplikacji| Visual Studio Team Services| Windows 10, Windows 8.1, Windows 7, iOS i Android|



## <a name="approved-client-app-requirement"></a>Wymagania aplikacji zatwierdzonych klienta 

Kontrolowanie połączeń klienta przy użyciu **wymaga aplikacji klienta zatwierdzonych** opcji w zasadach dostępu warunkowego. Dostęp tylko wtedy, gdy próba połączenia zostało utworzone przez aplikację klienta zatwierdzone.

![Kontroli dostępu dla aplikacji klienckich zatwierdzone](./media/active-directory-conditional-access-technical-reference/21.png)

Następujące aplikacje klienckie mogą być używane z wymaganiami aplikacji zatwierdzonych klienta:

- Microsoft Excel

- Microsoft OneDrive

- Program Microsoft Outlook

- Microsoft OneNote

- Program Microsoft PowerPoint

- Microsoft SharePoint

- Microsoft Skype dla firm

- Microsoft Teams

- Program Microsoft Visio

- Program Microsoft Word


**Uwagi**

- Aplikacje klienta zatwierdzonych obsługuje funkcja zarządzania aplikacjami mobilnymi usługi Intune.

- **Wymaga aplikacji klienta zatwierdzonych** wymaganie:

    - Obsługuje tylko systemy iOS i Android dla [warunku platformy urządzenia](#device-platforms-condition).

    - Nie obsługuje **przeglądarki** opcja dla [warunku aplikacje klienckie](#supported-browsers).
    
    - Zastępuje **aplikacji mobilnych i klasycznych klientów** opcja dla [warunku aplikacje klienckie](#supported-mobile-apps-and-desktop-clients) po wybraniu tej opcji.


## <a name="next-steps"></a>Następne kroki

- Omówienie dostępu warunkowego, zobacz [dostępu warunkowego w usłudze Azure Active Directory](active-directory-conditional-access-azure-portal.md).
- Jeśli wszystko jest gotowe do skonfigurowania zasad dostępu warunkowego w środowisku, zobacz [zalecane wskazówki dotyczące dostępu warunkowego w usłudze Azure Active Directory](active-directory-conditional-access-best-practices.md).



<!--Image references-->
[1]: ./media/active-directory-conditional-access-technical-reference/01.png


