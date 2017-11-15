---
title: "Planowanie wdrożenia synchronizacji plików Azure (wersja zapoznawcza) | Dokumentacja firmy Microsoft"
description: "Dowiedz się, co należy wziąć pod uwagę podczas planowania wdrożenia usługi pliki Azure."
services: storage
documentationcenter: 
author: wmgries
manager: klaasl
editor: jgerend
ms.assetid: 297f3a14-6b3a-48b0-9da4-db5907827fb5
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/08/2017
ms.author: wgries
ms.openlocfilehash: 241b744f5c5e89f53addb4d41d732245d76ef9a3
ms.sourcegitcommit: e38120a5575ed35ebe7dccd4daf8d5673534626c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/13/2017
---
# <a name="planning-for-an-azure-file-sync-preview-deployment"></a>Planowanie wdrożenia synchronizacji plików Azure (wersja zapoznawcza)
Umożliwia synchronizacji plików Azure (wersja zapoznawcza) scentralizowanie udziałów plików w organizacji w plikach Azure, przy zachowaniu elastyczności, wydajności i zgodności serwera plików lokalnych. Synchronizacja programu Azure pliku przy użyciu systemu Windows Server do szybkiego pamięci podręcznej udziału plików na platformę Azure. Można użyć każdego protokołu, który jest dostępny w systemie Windows Server dostępu do danych lokalnie, w tym protokołu SMB, systemu plików NFS i FTPS. Może mieć dowolną liczbę pamięci podręcznych zgodnie z potrzebami na całym świecie.

W tym artykule opisano ważne kwestie dotyczące wdrażania synchronizacji plików Azure. Zaleca się również przeczytanie [planowania wdrożenia usługi pliki Azure](storage-files-planning.md). 

## <a name="azure-file-sync-terminology"></a>Terminologia synchronizacji plików platformy Azure
Przed przejściem do szczegółów planowania wdrożenia synchronizacji plików Azure, koniecznie zapoznaj się z terminologią.

### <a name="storage-sync-service"></a>Usługa synchronizacji magazynu
Usługa synchronizacji magazynu jest najwyższego poziomu zasobów platformy Azure dla synchronizacji plików Azure. Zasób usługi synchronizacji magazynu jest elementu równorzędnego zasobów konta magazynu i podobnie można wdrożyć dla grup zasobów platformy Azure. Ponieważ usługa synchronizacji magazynu można utworzyć relacji synchronizacji z wieloma kontami magazynu za pośrednictwem grupy synchronizacji wielu różnych zasobów najwyższego poziomu z zasobów konta magazynu jest wymagana. Subskrypcja może mieć wdrożono wiele zasobów magazynu usługi synchronizacji.

### <a name="sync-group"></a>Grupy synchronizacji
Grupy synchronizacji definiuje topologia synchronizacji dla grupy plików. Punkty końcowe w ramach grupy synchronizacji są zsynchronizowane ze sobą. Jeśli na przykład zawiera dwa różne zestawy plików, które mają być zarządzane za pomocą synchronizacji usługi Azure pliku, czy utworzyć dwie grupy synchronizacji i dodać do każdej grupy synchronizacji różnych punktów końcowych. Usługa synchronizacji magazynu może obsługiwać dowolną liczbę grup synchronizacji.  

### <a name="registered-server"></a>Zarejestrowanego serwera
Obiekt serwera w zarejestrowany reprezentuje relację zaufania między serwerem (lub klastra) i usługi synchronizacji magazynu. Możesz zarejestrować dowolną liczbę serwerów do wystąpienia usługi synchronizacji magazynu ma. Jednak serwera (lub klastra) można zarejestrować z tylko jedną usługę synchronizacji magazynu naraz.

### <a name="azure-file-sync-agent"></a>Agent synchronizacji plików Azure
Agent synchronizacji plików Azure jest pobrania pakietu, który umożliwia systemu Windows Server można synchronizować z udziałem plików na platformę Azure. Agent synchronizacji plików Azure ma trzy główne składniki: 
- **FileSyncSvc.exe**: tła usługi systemu Windows, który jest odpowiedzialny za monitorowanie zmian w punktach końcowych serwera i inicjowania sesji synchronizacji na platformie Azure.
- **StorageSync.sys**: Filtr systemu plików synchronizacji plików Azure, która jest odpowiedzialna za warstw plików do usługi pliki Azure (usług w chmurze, gdy jest włączona obsługa poziomów).
- **Polecenia cmdlet programu PowerShell do zarządzania**: polecenia cmdlet programu PowerShell, używanej do interakcji z Microsoft.StorageSync Azure dostawcy zasobów. Dane można znaleźć w następujących lokalizacjach (domyślnie):
    - C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.PowerShell.Cmdlets.dll
    - C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll

### <a name="server-endpoint"></a>Punkt końcowy serwera
Punkt końcowy serwera reprezentuje określonej lokalizacji na serwerze zarejestrowany, takie jak folder na wolumin serwera lub w katalogu głównym woluminu. Wiele punktów końcowych serwera może istnieć na tym samym woluminie, jeśli ich przestrzenie nazw nie nakładają się (na przykład F:\sync1 i F:\sync2). Zasady warstw chmury można skonfigurować osobno dla każdego punktu końcowego serwera. Jeśli dodasz lokalizacji serwera, która ma istniejącego zestawu plików jako punktu końcowego serwera do grupy synchronizacji te pliki są łączone z innymi plikami, które znajdują się już na innych punktów końcowych w grupie synchronizacji.

### <a name="cloud-endpoint"></a>Punktu końcowego w chmurze
Punkt końcowy w chmurze jest na udział plików na platformę Azure, który jest częścią grupy synchronizacji. Synchronizacje udziału plików na platformę Azure całego i udziału plików na platformę Azure może być elementem członkowskim tylko jednej punktu końcowego w chmurze. W związku z tym udziałem plików Azure może być członkiem tylko jednej grupy synchronizacji. Jeśli dodasz na udział plików na platformę Azure, który ma istniejącego zestawu plików jako punktu końcowego w chmurze do grupy synchronizacji, istniejące pliki są łączone z innymi plikami, które znajdują się już na innych punktów końcowych w grupie synchronizacji.

> [!Important]  
> Synchronizacja programu Azure pliku nie obsługuje bezpośrednio wprowadzania zmian do udziału plików na platformę Azure. Jednak wszelkie zmiany wprowadzone w udziale plików na platformę Azure najpierw konieczne ich odnalezienie przez zadanie wykrywania zmian synchronizacji plików Azure. Zadanie wykrywania zmian jest inicjowane dla punktu końcowego w chmurze tylko raz na 24 godziny. Aby uzyskać więcej informacji, zobacz [plików Azure — często zadawane pytania](storage-files-faq.md#afs-change-detection).

### <a name="cloud-tiering"></a>Obsługa warstw w chmurze 
Chmura warstwy jest opcjonalna funkcja synchronizacji plików Azure, w którym rzadko używane lub uzyskał dostęp do plików może należeć do warstwy do usługi pliki Azure. Gdy plik jest warstwowa, filtr systemu plików Azure plik synchronizacji (StorageSync.sys) zastępuje plik lokalnie wskaźnika ani punktu ponownej analizy. Punkt ponownej analizy reprezentuje adres URL do pliku w plikach Azure. Warstwowych plik ma ustawiony w systemie plików NTFS, identyfikacji plików warstwowych aplikacji innych firm, atrybut "offline". Po otwarciu pliku warstwowych synchronizacji plików Azure bezproblemowo odwołania do danych plików z plików Azure bez konieczności wiedzieć, że plik nie jest przechowywany lokalnie na komputerze użytkownika. Ta funkcja jest nazywana zarządzania magazynu hierarchicznych (HSM).

## <a name="azure-file-sync-interoperability"></a>Współdziałanie synchronizacji plików platformy Azure 
W tej sekcji omówiono synchronizacji plików Azure współdziałanie z funkcji systemu Windows Server oraz ról i rozwiązania innych firm.

### <a name="supported-versions-of-windows-server"></a>Obsługiwane wersje systemu Windows Server
Obecnie są obsługiwane wersje systemu Windows Server przez synchronizacji plików Azure:

| Wersja | Obsługiwane wersje produktu | Opcje wdrożenia obsługiwany |
|---------|----------------|------------------------------|
| Windows Server 2016 | Datacenter i Standard | Pełna (serwer z interfejsem użytkownika) |
| Windows Server 2012 R2 | Datacenter i Standard | Pełna (serwer z interfejsem użytkownika) |

Przyszłych wersji systemu Windows Server zostanie dodana po ich wydaniu. Wcześniejsze wersje systemu Windows mogą być dodane oparte na opinie użytkowników.

> [!Important]  
> Zaleca się pozostawienie wszystkich serwerów używanych z synchronizacji plików Azure na bieżąco z najnowszymi aktualizacjami z usługi Windows Update. 

### <a name="file-system-features"></a>Funkcje systemu plików
| Funkcja | Obsługuje stanu | Uwagi |
|---------|----------------|-------|
| Listy kontroli dostępu (ACL) | W pełni obsługiwane | Listy ACL systemu Windows są zachowywane przez synchronizacji plików Azure i są wymuszane przez system Windows Server na serwerze punktów końcowych. Listy ACL systemu Windows nie są (jeszcze) obsługiwany przez pliki Azure, jeśli pliki są udostępniane bezpośrednio w chmurze. |
| Twarde linki | Pominięto | |
| Łącza symbolicznego | Pominięto | |
| Punkty instalacji | Częściowo obsługiwanej | Punkty instalacji może być katalogiem głównym serwera punktu końcowego, ale są one pomijane, jeśli są one w przestrzeni nazw punktu końcowego serwera. |
| Skrzyżowania | Pominięto | |
| Punkty ponownej analizy | Pominięto | |
| Kompresja NTFS | W pełni obsługiwane | |
| Plików rozrzedzonych | W pełni obsługiwane | Synchronizacja plików rozrzedzonych (nie są blokowane), ale synchronizacji w chmurze jako całego pliku. Zawartość pliku zmiany w chmurze (lub na innym serwerze), plik nie jest już rozrzedzony, gdy zmiana zostały pobrane. |
| Alternatywne strumienie danych (AD) | Zachowane, ale nie zsynchronizowano | |

> [!Note]  
> Obsługiwane są tylko woluminy NTFS.

### <a name="failover-clustering"></a>Klaster trybu failover
Windows Server Failover Clustering jest obsługiwany przez synchronizacji plików Azure w opcji wdrażania "Serwer plików do użytku ogólnego". Klaster trybu failover nie jest obsługiwana w "Serwer plików skalowalny w poziomie dla danych aplikacji" (SOFS) lub na udostępnionych woluminów klastra (CSV).

> [!Note]  
> Na każdym węźle w klastrze pracy awaryjnej dla synchronizacji działał poprawnie, musi być zainstalowany agent synchronizacji plików Azure.

### <a name="data-deduplication"></a>Funkcja deduplikacji danych
Dla woluminów, które nie mają chmury, dodając funkcje warstw włączone synchronizacji plików Azure obsługuje deduplikacji danych serwera systemu Windows jest włączona na tym woluminie. Współdziałanie między synchronizacji plików Azure z chmurą, dodając funkcje warstw włączone i deduplikacji danych nie jest obecnie obsługiwana.

### <a name="antivirus-solutions"></a>Programy antywirusowe
Ponieważ oprogramowanie antywirusowe polega na skanowanie plików do znanego złośliwego kodu, antywirusowe może spowodować odwołania warstwowych plików. Ponieważ pliki warstwowych ma ustawiony atrybut "offline", firma Microsoft zaleca konsultacji z dostawcą oprogramowania, aby dowiedzieć się, jak skonfigurować ich rozwiązanie, aby pominąć odczytywania plików trybu offline. 

Znane są następujące rozwiązania do obsługi pomijanie plików trybu offline:

- [Symantec Endpoint Protection](https://support.symantec.com/en_US/article.tech173752.html)
- [Zabezpieczenia punktu końcowego McAfee](https://kc.mcafee.com/resources/sites/MCAFEE/content/live/PRODUCT_DOCUMENTATION/26000/PD26799/en_US/ens_1050_help_0-00_en-us.pdf) (zobacz "Skanuj tylko najważniejsze" na stronie 90 PDF)
- [Oprogramowanie antywirusowe Kaspersky](https://support.kaspersky.com/4684)
- [Sophos programu Endpoint Protection](https://community.sophos.com/kb/en-us/40102)
- [TrendMicro OfficeScan](https://success.trendmicro.com/solution/1114377-preventing-performance-or-backup-and-restore-issues-when-using-commvault-software-with-osce-11-0#collapseTwo) 

### <a name="backup-solutions"></a>Rozwiązania kopii zapasowej
Na przykład oprogramowania antywirusowego rozwiązań tworzenia kopii zapasowych może spowodować odwołania warstwowych plików. Firma Microsoft zaleca, aby utworzyć kopię zapasową udziału plików na platformę Azure zamiast lokalnie instalowanym produktem kopii zapasowej za pomocą rozwiązania kopii zapasowych w chmurze.

### <a name="encryption-solutions"></a>Rozwiązania
Obsługa szyfrowania rozwiązań zależy od sposobu implementacji. Synchronizacja programu Azure pliku jest znany do pracy z:

- Szyfrowanie funkcją BitLocker
- Azure Rights Management Services (Azure RMS) (i starszych Active Directory RMS)

Synchronizacja programu Azure pliku wiadomo, że nie będą działać z:

- NTFS System szyfrowania plików (EFS)

Ogólnie rzecz biorąc synchronizacji plików Azure powinien obsługiwać współdziałanie z rozwiązaniami szyfrowania, które znajdują się poniżej systemu plików, takich jak funkcja BitLocker oraz z rozwiązania, które zostały wdrożone w formacie pliku, na przykład funkcji BitLocker. Nie współdziałania wprowadzono rozwiązań, które znajdują się powyżej systemu plików (takich jak system szyfrowania plików NTFS).

### <a name="other-hierarchical-storage-management-hsm-solutions"></a>Innych rozwiązań do zarządzania magazynu hierarchicznych (HSM)
Inne rozwiązania sprzętowego modułu zabezpieczeń powinien być używany z synchronizacji plików Azure.

## <a name="region-availability"></a>Dostępność w danym regionie
Synchronizacja programu Azure plik jest dostępny tylko w następujących regionach w wersji zapoznawczej:

| Region | Lokalizacja centrum danych |
|--------|---------------------|
| Zachodnie stany USA | Polski, USA |
| Europa Zachodnia | Holandia |
| Azja Południowo-Wschodnia | Singapur |
| Australia Wschodnia | Nowe Pomorze, Australia |

W wersji zapoznawczej firma Microsoft obsługuje synchronizację tylko z udziału plików na platformę Azure, który znajduje się w tym samym regionie co magazyn usługi synchronizacji.

## <a name="azure-file-sync-agent-update-policy"></a>Zasady aktualizacji agenta usługi Azure File Sync
[!INCLUDE [storage-sync-files-agent-update-policy](../../../includes/storage-sync-files-agent-update-policy.md)]

## <a name="next-steps"></a>Następne kroki
* [Planowanie wdrożenia usługi pliki Azure](storage-files-planning.md)
* [Wdrażanie plików platformy Azure](storage-files-deployment-guide.md)
* [Wdrażanie synchronizacji plików na platformę Azure](storage-sync-files-deployment-guide.md)
