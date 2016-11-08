---
title: Co zawierają obrazy szablonów usługi Azure RemoteApp? | Microsoft Docs
description: Dowiedz się więcej na temat obrazów szablonów dołączonych do usługi Azure RemoteApp.
services: remoteapp
documentationcenter: ''
author: lizap
manager: mbaldwin

ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/15/2016
ms.author: elizapo

---
# Co zawierają obrazy szablonów usługi Azure RemoteApp?
> [!IMPORTANT]
> Usługa Azure RemoteApp nie jest już obsługiwana. Szczegółowe informacje zawiera [powiadomienie](https://go.microsoft.com/fwlink/?linkid=821148).
> 
> 

Subskrypcja usługi Azure RemoteApp obejmuje trzy obrazy szablonów:

* Windows Server 2012
* Microsoft Office 365 ProPlus (wymagana jest subskrypcja usługi Office 365)
* Microsoft Office 2013 Professional Plus (tylko wersja próbna)

> [!IMPORTANT]
> Subskrypcja usługi Azure RemoteApp daje dostęp do oprogramowania w obrazach, z wyjątkiem usługi Office 365 ProPlus, która wymaga oddzielnej subskrypcji, oraz pakietu Office 2013, którego nie można używać w środowisku produkcyjnym. Oznacza to, że można udostępniać programy lub aplikacje w obrazach szablonów użytkownikom. Na przykład, jeśli utworzono kolekcję korzystającą z obrazu systemu Windows Server 2012 R2, można opublikować program System Center Endpoint Protection, aby umożliwić użytkownikom dostęp do niego za pośrednictwem usługi RemoteApp.
> 
> Aby uzyskać więcej informacji, zobacz temat [Szczegółowe informacje o licencjonowaniu usługi RemoteApp](remoteapp-licensing.md). Informacje na temat licencjonowania pakietu Office znajdują się w temacie [Korzystanie z pakietu Office przy użyciu usługi Azure RemoteApp](remoteapp-o365.md).
> 
> 

Poniżej podano szczegółowe informacje o zawartości każdego obrazu.

## Windows Server 2012 R2 (obraz podstawowy)
Ten obraz jest oparty na systemie operacyjnym Microsoft Windows Server 2012 R2 Datacenter i ma zainstalowane następujące role i funkcje spełniające wymagania obrazów szablonów usługi Azure RemoteApp:

* .NET Framework 4.5, 3.5.1, 3.5
* Środowisko pulpitu
* Usługi dotyczące pisma ręcznego i odręcznego
* Platforma Media Foundation
* Host sesji usług pulpitu zdalnego
* Windows PowerShell 4.0
* Windows PowerShell ISE
* Obsługa środowiska WoW64

W tym obrazie zainstalowano także następujące aplikacje:

* Adobe Flash Player
* Microsoft Silverlight
* Microsoft System Center 2012 Endpoint Protection
* Microsoft Windows Media Player

## Microsoft Office 365 ProPlus (wymagana jest subskrypcja)
Usługa Office 365 jest najbardziej pożądaną aplikacją, dlatego utworzyliśmy „niestandardowy” obraz umożliwiający pracę z tą usługą.

Ten obraz jest rozszerzeniem obrazu podstawowego i oprócz składników zawartych w obrazie systemu Windows Server 2012 R2 zawiera następujące zainstalowane składniki usługi Microsoft Office 365 ProPlus:

* Dostęp
* Excel
* Lync
* OneNote
* OneDrive dla Firm (zauważ, że użycie agenta synchronizacji z usługą Azure RemoteApp nie jest obsługiwane)
* Outlook
* PowerPoint
* Word
* Narzędzia sprawdzające pakietu Office

Obraz zawiera także aplikacje Visio Pro i Project Pro.

Ponadto zawiera następujące aplikacje:

* SQL Native Client
* Sterownik ODBC
* SQL Server Data Mining Client
* MasterDataServices Client
* Microsoft Publisher
* PowerQuery
* PowerMap

Pełna funkcjonalność aplikacji usługi Office 365 ProPlus jest dostępna tylko dla użytkowników, którzy posiadają plan usługi Office 365 ProPlus. Więcej szczegółowych informacji na temat planów subskrypcji usługi Office 365 zawiera artykuł [Plany usługi Office 365](http://technet.microsoft.com/library/office-365-plan-options.aspx). Nadal masz pytania? Zapoznaj się z informacjami w temacie [Usługa Office 365 + usługa RemoteApp](remoteapp-o365.md). Zobacz także nowy artykuł [Jak korzystać z subskrypcji usługi Office 365 z usługą Azure RemoteApp](remoteapp-officesubscription.md).

Należy zauważyć, że wymagane są osobne licencje usługi Office 365 ProPlus, programu Visio Pro i programu Project Pro.

## Microsoft Office 2013 Professional Plus (tylko wersja próbna)
W bezpłatnym okresie próbnym można testować usługę z użyciem obrazu pakietu Office 2013.

Ten obraz jest rozszerzeniem obrazu podstawowego i oprócz składników zawartych w obrazie systemu Windows Server 2012 R2 zawiera następujące zainstalowane składniki pakietu Microsoft Office 2013 Professional Plus:

* Access
* Excel
* Lync
* OneNote
* OneDrive dla Firm (zauważ, że użycie agenta synchronizacji z usługą Azure RemoteApp nie jest obsługiwane)
* Outlook
* PowerPoint
* Project
* Visio
* Word
* Narzędzia sprawdzające pakietu Office

> [!IMPORTANT]
> **Informacje prawne:** ten obraz nie zawiera licencji pakietu Microsoft Office i *nie może być używany w środowisku produkcyjnym*. Obraz pakietu Office 2013 Professional Plus jest przeznaczony tylko do użycia próbnego. Jeśli chcesz korzystać z aplikacji pakietu Office w usłudze Azure RemoteApp w środowisku produkcyjnym, musisz użyć obrazu usługi Office 365 ProPlus. Aby uzyskać więcej szczegółowych informacji na temat licencjonowania usługi Office, zobacz temat [Korzystanie z usługi Office 365 z usługą Azure RemoteApp](remoteapp-o365.md)
> 
> 

<!--HONumber=Sep16_HO3-->


