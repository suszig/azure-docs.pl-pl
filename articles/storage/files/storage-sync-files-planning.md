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
ms.openlocfilehash: d626f71aa21cea562ef6c9554c05e6de027e7f4d
ms.sourcegitcommit: 76a3cbac40337ce88f41f9c21a388e21bbd9c13f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/25/2017
---
# <a name="planning-for-an-azure-file-sync-preview-deployment"></a>Planowanie wdrożenia synchronizacji plików Azure (wersja zapoznawcza)
Usługa Azure File Sync (wersja zapoznawcza) umożliwia scentralizowanie udziałów plików Twojej organizacji w usłudze Azure Files bez rezygnacji z elastyczności, wydajności i zgodności lokalnego serwera plików. Jest to realizowane poprzez przekształcanie systemów Windows Server w szybką pamięć podręczną udziału plików platformy Azure. Możesz użyć dowolnego dostępnego protokołu w systemie Windows Server w celu uzyskania lokalnego dostępu do danych (w tym protokołu SMB, systemu plików NFS i protokołu FTPS) i możesz mieć dowolną potrzebną Ci liczbę pamięci podręcznych na całym świecie.

W tym przewodniku opisano, co należy wziąć pod uwagę podczas wdrażania synchronizacji plików Azure. Zaleca się przeczytanie [planowania wdrożenia usługi pliki Azure](storage-files-planning.md) przewodnik testowania. 

## <a name="understanding-azure-file-sync-terminology"></a>Opis terminologii synchronizacji plików Azure
Przed przejściem do szczegółów synchronizacji plików Azure, koniecznie zapoznaj się z terminologią.

### <a name="storage-sync-service"></a>Usługa synchronizacji magazynu
Usługa synchronizacji magazynu jest najwyższego poziomu zasobem platformy Azure, reprezentujący synchronizacji plików Azure. Zasób usługi synchronizacji magazynu jest elementu równorzędnego zasobów konta magazynu i similarily można wdrożyć do grup zasobów platformy Azure. Ponieważ usługa synchronizacji magazynu można utworzyć relacji synchronizacji z wieloma kontami magazynu za pośrednictwem grupy synchronizacji wielu różnych zasobów najwyższego poziomu z zasobów konta magazynu jest wymagana. Subskrypcja może mieć wdrożono wiele zasobów magazynu usługi synchronizacji.

### <a name="sync-group"></a>Grupy synchronizacji
Grupy synchronizacji definiuje topologia synchronizacji dla grupy plików. Punkty końcowe w ramach grupy synchronizacji zostaną zachowane w synchronizacji ze sobą. Jeśli na przykład masz dwa różne zestawy plików, które mają być zarządzane za pomocą AFS, czy utworzyć dwie grupy synchronizacji i dodać różnych punktów końcowych do każdego. Usługa synchronizacji magazynu może obsługiwać dowolną liczbę grup synchronizacji.  

### <a name="registered-server"></a>Zarejestrowanego serwera
Obiekt serwera w zarejestrowany reprezentuje relację zaufania między serwerem (lub klastra) i usługi synchronizacji magazynu. Możesz zarejestrować dowolną liczbę serwerów do wystąpienia usługi synchronizacji magazynu chcesz. Jednak serwera (lub klastra) mogą być rejestrowane tylko z jedną usługę synchronizacji magazynu w danym momencie.

### <a name="azure-file-sync-agent"></a>Agent synchronizacji plików Azure
Agent synchronizacji plików Azure jest pobrania pakietu, co pozwoli na systemu Windows Server do synchronizacji przy użyciu pliku Azure udziału. Agent synchronizacji plików Azure obejmuje trzy główne składniki: 
- **FileSyncSvc.exe**: tła usługi systemu Windows jest odpowiedzialny za monitorowanie zmian w punktach końcowych serwera i zainicjowaniu sesji synchronizacji na platformie Azure.
- **StorageSync.sys**: filtrowanie synchronizacji plików Azure systemu plików, odpowiedzialny warstw plików do usługi pliki Azure (usług w chmurze, gdy jest włączona obsługa poziomów).
- **Polecenia cmdlet programu PowerShell do zarządzania**: polecenia cmdlet programu PowerShell na potrzeby interakcji z Microsoft.StorageSync dostawcy zasobów usługi Azure. (Domyślnie) te można znaleźć w następujących lokalizacjach:
    - C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.PowerShell.Cmdlets.dll
    - C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll

### <a name="server-endpoint"></a>Punkt końcowy serwera
Punkt końcowy serwera reprezentuje określonej lokalizacji na serwerze zarejestrowany, takie jak folder na wolumin serwera lub w katalogu głównym woluminu. Wiele punktów końcowych serwera może istnieć na tym samym woluminie, jeśli ich przestrzenie nazw nakładania (np. F:\sync1 i F:\sync2). Zasady warstw chmury można skonfigurować osobno dla każdego punktu końcowego serwera. Po dodaniu do grupy synchronizacji na serwerze z istniejącego zestawu plików jako punktu końcowego serwera, te pliki zostaną scalone z innymi plikami już dla innych punktów końcowych w grupie synchronizacji.

### <a name="cloud-endpoint"></a>Punktu końcowego w chmurze
Punkt końcowy w chmurze jest udział plików Azure, który jest częścią grupy synchronizacji. Cały synchronizacje udziału plików platformy Azure i udziału plików platformy Azure można tylko członek jednego punktu końcowego w chmurze, a w związku z tym jednej grupy synchronizacji. Po dodaniu do grupy synchronizacji udziału plików platformy Azure z istniejącego zestawu plików jako punktu końcowego w chmurze, te pliki zostaną scalone z innymi plikami już dla innych punktów końcowych w grupie synchronizacji.

> [!Important]  
> Azure obsługuje synchronizacji plików wprowadzania zmian w pliku Azure udziału bezpośrednio, jednak należy zauważyć, że zmiany wprowadzone w udziale plików Azure najpierw mają zostać wykryte przez zadanie wykrywania zmian synchronizacji plików Azure, czyli tylko initatiated dla punktu końcowego w chmurze, co 24 godziny. Zobacz [plików Azure — często zadawane pytania](storage-files-faq.md#afs-change-detection) Aby uzyskać więcej informacji.

### <a name="cloud-tiering"></a>Obsługa warstw w chmurze 
Chmura warstwy jest opcjonalna funkcja synchronizacji plików Azure, które umożliwia rzadko używane lub dostęp do plików do należeć do warstwy do usługi pliki Azure. Plik jest warstwowa, system plików synchronizacji plików Azure filtrowanie plików lokalnie za pomocą wskaźnika (StorageSync.sys) zastępuje lub punkty ponownej analizy, reprezentujący adres URL do pliku w plikach Azure. Warstwowych plik ma ustawiony w systemie plików NTFS, identyfikacji plików warstwowych aplikacji innych firm, atrybut "offline". Po otwarciu pliku warstwowych synchronizacji plików Azure bezproblemowo odwołuje się, że plik danych z plików Azure bez dokładnej znajomości pliku użytkownika nie jest przechowywany lokalnie na komputerze. Ta funkcja jest nazywana zarządzania magazynu hierarchicznych (HSM).

## <a name="azure-file-sync-interoperability"></a>Współdziałanie synchronizacji plików na platformę Azure 
W tej sekcji omówiono synchronizacji plików Azure współdziałanie z funkcji systemu Windows Server oraz ról i rozwiązania innych firm.

### <a name="supported-versions-of-windows-server"></a>Obsługiwane wersje systemu Windows Server
Obecnie są obsługiwane wersje systemu Windows Server przez synchronizacji plików Azure:

| Wersja | Obsługiwane wersje produktu | Opcje wdrożenia obsługiwany |
|---------|----------------|------------------------------|
| Windows Server 2016 | Datacenter i Standard | Pełna (serwer z interfejsem użytkownika) |
| Windows Server 2012 R2 | Datacenter i Standard | Pełna (serwer z interfejsem użytkownika) |

Przyszłych wersji systemu Windows Server zostanie dodana jako ich wydania i starszych wersji systemu Windows mogą być dodawane oparte na opinie użytkowników.

> [!Important]  
> Zaleca się pozostawienie wszystkich serwerów systemu Windows używana z opcją synchronizacji plików Azure najnowszych aktualizacji z witryny Windows Update. 

### <a name="file-system-features"></a>Funkcje systemu plików
| Funkcja | Obsługuje stanu | Uwagi |
|---------|----------------|-------|
| Listy kontroli dostępu (ACL) | W pełni obsługiwane | Listy ACL systemu Windows są zachowywane przez synchronizacji plików Azure i są wymuszane przez system Windows Server na serwerze punktów końcowych. Listy ACL systemu Windows nie są (jeszcze) obsługiwany przez pliki Azure, jeśli pliki są udostępniane bezpośrednio w chmurze. |
| Twarde łącza | Pominięto | |
| Łącza symbolicznego | Pominięto | |
| Punkty instalacji | Częściowo obsługiwanej | Punkty instalacji może być katalogiem głównym serwera punktu końcowego, ale zostanie pominięta, jeśli zawarte w przestrzeni nazw serwera punktu końcowego. |
| Skrzyżowania | Pominięto | |
| Punkty ponownej analizy | Pominięto | |
| Kompresja NTFS | W pełni obsługiwane | |
| Plików rozrzedzonych | W pełni obsługiwane | Plików rozrzedzonych zostaną zsynchronizowane (nie blokowane), ale zostaną zsynchronizowane do chmury w postaci pełnego pliku. Zawartość pliku zmiany w chmurze (lub na innym serwerze), plik nie będzie już rozrzedzony, gdy zmiany zostały pobrane |
| Alternatywne strumienie danych (AD) | Zachowane, ale nie zsynchronizowano | |

> [!Note]  
> Obsługiwane są tylko woluminy NTFS.

### <a name="failover-clustering"></a>Klaster trybu failover
Windows Server Failover Clustering jest obsługiwany przez synchronizacji plików Azure w opcji wdrażania "Serwer plików do użytku ogólnego". Klaster trybu failover nie jest obsługiwana w "Serwer plików skalowalny w poziomie dla danych aplikacji" (SOFS) lub na woluminy udostępnione klastra (CSV).

> [!Note]  
> Na każdym węźle w klastrze pracy awaryjnej dla synchronizacji działało poprawnie, musi być zainstalowany agent synchronizacji plików Azure.

### <a name="windows-server-data-deduplication"></a>Deduplikacja danych w systemie Windows Server
Dla woluminów bez chmury, dodając funkcje warstw włączone synchronizacji plików Azure obsługuje deduplikacji danych włączana w woluminie. Współdziałanie między synchronizacji plików Azure z chmurą, dodając funkcje warstw włączone i deduplikacji danych w tej chwili nie jest obsługiwana.

### <a name="anti-virus-solutions"></a>Oprogramowanie antywirusowe rozwiązania
Ponieważ oprogramowanie antywirusowe polega na skanowanie plików do znanego złośliwego kodu, oprogramowanie antywirusowe może powodować odwołania warstwowych plików. Ponieważ warstwowych pliki mają ustawiony atrybut "offline", firma Microsoft zaleca konsultacji z dostawcą oprogramowania dotyczące sposobu konfigurowania ich rozwiązanie, aby pominąć odczytywania plików trybu offline. 

Znane są następujące rozwiązania do obsługi pomijanie plików trybu offline:

- [Symantec Endpoint Protection](https://support.symantec.com/en_US/article.tech173752.html)
- [Zabezpieczenia punktu końcowego McAfee](https://kc.mcafee.com/resources/sites/MCAFEE/content/live/PRODUCT_DOCUMENTATION/26000/PD26799/en_US/ens_1050_help_0-00_en-us.pdf) (zobacz sekcja "Skanuj tylko najważniejsze" na stronie 90)
- [Oprogramowanie antywirusowe Kaspersky](https://support.kaspersky.com/4684)
- [Sophos programu Endpoint Protection](https://community.sophos.com/kb/en-us/40102)
- [TrendMicro OfficeScan](https://success.trendmicro.com/solution/1114377-preventing-performance-or-backup-and-restore-issues-when-using-commvault-software-with-osce-11-0#collapseTwo) 

### <a name="backup-solutions"></a>Rozwiązania kopii zapasowej
Jak oprogramowanie antywirusowe rozwiązania w zakresie tworzenia kopii zapasowych może spowodować odwołania warstwowych plików. Zaleca się przy użyciu kopii zapasowej rozwiązania chmury do usługi Kopia zapasowa udziału plików platformy Azure, a nie za pomocą lokalnie instalowanym produktem kopii zapasowej.

### <a name="encryption-solutions"></a>Rozwiązania
Obsługa szyfrowania rozwiązań zależy od sposobu implementacji. Synchronizacja programu Azure pliku jest znany do pracy z:

- Szyfrowanie funkcją BitLocker
- Usługa Azure RMS (i starszych Active Directory RMS)

Synchronizacja programu Azure pliku wiadomo, że nie będą działać z:

- NTFS System szyfrowania plików (EFS)

Ogólnie rzecz biorąc synchronizacji plików Azure powinno być możliwe współdziałanie z rozwiązaniami szyfrowania, które znajdują się poniżej systemu plików, takich jak funkcja BitLocker i rozwiązań w formacie pliku, na przykład funkcja BitLocker, ale dokonano nie specjalnych interop współdziałanie z rozwiązaniami, które znajdują się powyżej systemu plików (takich jak szyfrowane systemu plików NTFS).

### <a name="other-hierarchical-storage-management-hsm-solutions"></a>Innych rozwiązań do zarządzania magazynu hierarchicznych (HSM)
Żadne rozwiązanie Zarządzanie magazynami hierarchicznych powinien być używany z synchronizacji plików Azure.

## <a name="region-availability"></a>Dostępność w danym regionie
Synchronizacja programu Azure pliku jest dostępna tylko w następujących regionach w wersji zapoznawczej:

| Region | Lokalizacja centrum danych |
|--------|---------------------|
| Zachodnie stany USA | Polski, USA |
| Europa Zachodnia | Holandia |
| Azja Południowo-Wschodnia | Singapur |
| Australia Wschodnia | Nowe Pomorze, Australia |

W wersji zapoznawczej obsługiwany jest tylko synchronizacji z udziału plików platformy Azure, w tym samym regionie co magazyn usługi synchronizacji.

## <a name="azure-file-sync-agent-update-policy"></a>Zasady aktualizacji agenta usługi Azure File Sync
[!INCLUDE [storage-sync-files-agent-update-policy](../../../includes/storage-sync-files-agent-update-policy.md)]

## <a name="next-steps"></a>Następne kroki
* [Planowanie wdrożenia usługi pliki Azure](storage-files-planning.md)
* [Wdrażanie plików platformy Azure](storage-files-deployment-guide.md)
* [Wdrażanie synchronizacji plików na platformę Azure](storage-sync-files-deployment-guide.md)
