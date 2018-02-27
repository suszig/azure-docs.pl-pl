---
title: "Informacje o wersji agenta usługi Azure File Sync | Microsoft Docs"
description: "Informacje o wersji usługi Azure File Sync"
services: storage
documentationcenter: 
author: wmgries
manager: klaasl
editor: tamram
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/08/2017
ms.author: wgries
ms.openlocfilehash: 9b6dfec6465482efcbf55d0441e44a0278f44a22
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/21/2018
---
# <a name="azure-file-sync-agent-release-notes"></a>Informacje o wersji agenta usługi Azure File Sync
Usługa Azure File Sync (wersja zapoznawcza) umożliwia scentralizowanie udziałów plików Twojej organizacji w usłudze Azure Files bez rezygnacji z elastyczności, wydajności i zgodności lokalnego serwera plików. Jest to realizowane poprzez przekształcanie systemów Windows Server w szybką pamięć podręczną udziału plików platformy Azure. Możesz użyć dowolnego dostępnego protokołu w systemie Windows Server w celu uzyskania lokalnego dostępu do danych (w tym protokołu SMB, systemu plików NFS i protokołu FTPS) i możesz mieć dowolną potrzebną Ci liczbę pamięci podręcznych na całym świecie.

W tym artykule omówiono informacje o wersji dotyczące obsługiwanych wersji agenta usługi Azure File Sync.

## <a name="supported-versions"></a>Obsługiwane wersje
Następujące wersje są obsługiwane przez usługę Azure File Sync:

| Numer wersji agenta | Data wydania | Obsługiwane do |
|----------------------|--------------|------------------|
| 2.0.11.0 | 2018-02-08 | Bieżąca wersja |
| 1.1.0.0 | 2017-09-26 | 2018-07-30 |

### <a name="azure-file-sync-agent-update-policy"></a>Zasady aktualizacji agenta usługi Azure File Sync
[!INCLUDE [storage-sync-files-agent-update-policy](../../../includes/storage-sync-files-agent-update-policy.md)]

## <a name="agent-version-20110"></a>Wersja agenta 2.0.11.0
Poniższe informacje o wersji dotyczą agenta w wersji 2.0.11.0, która została wydana 9 lutego 2018 r. 

### <a name="agent-installation-and-server-configuration"></a>Instalacja agenta i konfiguracja serwera
Aby uzyskać więcej informacji na temat sposobu instalowania i konfigurowania agenta usługi Azure File Sync z systemem Windows Server, zobacz [Planning for an Azure File Sync (preview) deployment (Planowanie wdrożenia usługi Azure File Sync (wersja zapoznawcza))](storage-sync-files-planning.md) i [How to deploy Azure File Sync (preview) (Jak wdrożyć usługę Azure File Sync (wersja zapoznawcza))](storage-sync-files-deployment-guide.md).

- Musi być zainstalowany pakiet instalacyjny agenta (MSI) z podniesionymi uprawnieniami (administrator).
- Nieobsługiwane w opcjach wdrażania systemu Windows Server Core lub Nano Server.
- Obsługiwane tylko w systemie Windows Server 2016 i 2012 R2.
- Agent wymaga co najmniej 2 GB pamięci fizycznej.

### <a name="interoperability"></a>Współdziałanie
- Oprogramowanie antywirusowe, kopia zapasowa i inne aplikacje, które uzyskują dostęp do plików warstwowych, mogą spowodować niepożądane odwołania, chyba że przestrzegają one atrybutu offline i pomijają odczytywanie zawartości tych plików. Aby uzyskać więcej informacji, zobacz [Troubleshoot Azure File Sync (preview) (Rozwiązywanie problemów z usługą Azure File Sync (wersja zapoznawcza))](storage-sync-files-troubleshoot.md)
- Nowością w tej aktualizacji jest obsługa usługi DFS-R.  Aby uzyskać więcej informacji, zobacz [przewodnik planowania](storage-sync-files-planning.md#distributed-file-system-dfs).
- Nie używaj osłon plików Menedżera zasobów serwera plików (lub innych): osłony plików mogą powodować nieskończone błędy synchronizacji, jeśli pliki są zablokowane z powodu osłony plików.
- Duplikowanie zarejestrowanego serwera (w tym klonowanie maszyny wirtualnej) może prowadzić do nieoczekiwanych wyników (w szczególności synchronizacja może nigdy nie być zbieżna).
- Deduplikacja danych i obsługa warstw w chmurze nie są obsługiwane w tym samym woluminie.
 
### <a name="sync-limitations"></a>Ograniczenia synchronizacji
Następujące elementy nie są synchronizowane, ale reszta systemu będzie nadal normalnie działać:
- Ścieżki dłuższe niż 2048 znaków
- Część DACL deskryptora zabezpieczeń, jeśli jest większa niż 2K (jest to problem tylko wtedy, gdy masz więcej niż około 40 wpisów kontroli dostępu dla pojedynczego elementu)
- Część SACL deskryptora zabezpieczeń (używana do przeprowadzania inspekcji)
- Atrybuty rozszerzone
- Alternatywne strumienie danych
- Punkty ponownej analizy
- Twarde linki
- Kompresja (jeśli jest ustawiona w pliku serwera) nie zostanie zachowana, gdy zmiany z innych punktów końcowych zostaną zsynchronizowane z tym plikiem
- Każdy plik zaszyfrowany za pomocą systemu szyfrowania plików (lub innego szyfrowania trybu użytkownika), który uniemożliwia naszej usłudze odczytywanie danych 
    
    > [!Note]  
    > Usługa Azure File Sync zawsze szyfruje przesyłane dane i dane mogą być szyfrowane podczas przechowywania na platformie Azure.
 
### <a name="server-endpoints"></a>Punkty końcowe serwera
- Punkt końcowy serwera można tworzyć tylko na woluminie NTFS. Systemy plików ReFS, FAT, FAT32 i inne nie są obecnie obsługiwane przez usługę Azure File Sync.
- Punkt końcowy serwera nie może znajdować się na woluminie systemowym (na przykład C:\MyFolder nie jest akceptowalną ścieżką, chyba że C:\MyFolder jest punktem instalacji).
- Klaster pracy awaryjnej jest obsługiwany tylko przy użyciu dysków klastrowanych, a nie przy użyciu udostępnionych woluminów klastra (CSV).
- Punkt końcowy serwera nie może być zagnieżdżony, ale może współistnieć na tym samym woluminie równolegle z innym.
- Usuwanie dużej liczby katalogów z serwera jednocześnie (ponad 10 000) może powodować błędy synchronizacji — usuwaj katalogi w partiach poniżej 10 000 i przed usunięciem następnej partii upewnij się, że operacje usuwania zostały pomyślnie zsynchronizowane.
- W tej wersji dodano obsługę katalogu głównego synchronizacji będącego katalogiem głównym woluminu.
- Nie należy przechowywać pliku stronicowania systemu operacyjnego lub aplikacji w ramach punktu końcowego serwera.
- Zmiany w tej wersji: dodano nowe zdarzenia do śledzenia całkowitego czasu wykonania dla obsługi warstw w chmurze (EventID: 9016); postęp przekazywania synchronizacji (EventID: 9302); lista plików, które nie zostały zsynchronizowane (EventID: 9900).
- Zmiany w tej wersji: wydajność szybkiej synchronizacji przestrzeni nazw odzyskiwania po awarii została znacznie zwiększona.
 
### <a name="cloud-tiering"></a>Obsługa warstw w chmurze
- Zmiany względem poprzedniej wersji: nowe pliki będą umieszczane w warstwie w ciągu 1 godziny na podstawie ustawienia zasad obsługi poziomów warstw (wcześniej: 32 godziny) — udostępniamy polecenie cmdlet programu PowerShell służące do umieszczania w warstwie na żądanie, dzięki możliwa będzie bardziej wydajna obsługa warstw bez konieczności oczekiwania na proces w tle.
- Jeśli warstwa pliku zostanie skopiowana za pomocą rozszerzenia Robocopy do innej lokalizacji, wynikowy plik nie będzie obsługiwany w warstwie, ale atrybut offline może zostać ustawiony, ponieważ rozszerzenie Robocopy niepoprawnie dołącza ten atrybut podczas operacji kopiowania.
- Podczas przeglądania właściwości plików z klienta SMB atrybut offline może wyglądać na ustawiony niepoprawnie z powodu buforowania plików metadanych przez SMB.
- Zmiany względem poprzedniej wersji: pliki są teraz pobierane jako pliki warstwowe na innych serwerach, pod warunkiem, że są nowe lub były wcześniej plikami warstwowym.

## <a name="agent-version-1100"></a>Wersja agenta 1.1.0.0
Poniższe informacje o wersji dotyczą agenta w wersji 1.1.0.0, która została wydana 9 września 2017 r. Ta wersja oznacza wydanie początkowej wersji zapoznawczej usługi Azure File Sync.

### <a name="agent-installation-and-server-configuration"></a>Instalacja agenta i konfiguracja serwera
Aby uzyskać więcej informacji na temat sposobu instalowania i konfigurowania agenta usługi Azure File Sync z systemem Windows Server, zobacz [Planning for an Azure File Sync (preview) deployment (Planowanie wdrożenia usługi Azure File Sync (wersja zapoznawcza))](storage-sync-files-planning.md) i [How to deploy Azure File Sync (preview) (Jak wdrożyć usługę Azure File Sync (wersja zapoznawcza))](storage-sync-files-deployment-guide.md).

- Musi być zainstalowany pakiet instalacyjny agenta (MSI) z podniesionymi uprawnieniami (administrator).
- Nieobsługiwane w opcjach wdrażania systemu Windows Server Core lub Nano Server.
- Obsługiwane tylko w systemie Windows Server 2016 i 2012 R2.
- Agent wymaga co najmniej 2 GB pamięci fizycznej.

### <a name="interoperability"></a>Współdziałanie
- Oprogramowanie antywirusowe, kopia zapasowa i inne aplikacje, które uzyskują dostęp do plików warstwowych, mogą spowodować niepożądane odwołania, chyba że przestrzegają one atrybutu offline i pomijają odczytywanie zawartości tych plików. Aby uzyskać więcej informacji, zobacz [Troubleshoot Azure File Sync (preview) (Rozwiązywanie problemów z usługą Azure File Sync (wersja zapoznawcza))](storage-sync-files-troubleshoot.md)
- Nie używaj osłon plików Menedżera zasobów serwera plików (lub innych): osłony plików mogą powodować nieskończone błędy synchronizacji, jeśli pliki są zablokowane z powodu osłony plików.
- Duplikowanie zarejestrowanego serwera (w tym klonowanie maszyny wirtualnej) może prowadzić do nieoczekiwanych wyników (w szczególności synchronizacja może nigdy nie być zbieżna).
- Deduplikacja danych i obsługa warstw w chmurze nie są obsługiwane w tym samym woluminie.
 
### <a name="sync-limitations"></a>Ograniczenia synchronizacji
Następujące elementy nie są synchronizowane, ale reszta systemu będzie nadal normalnie działać:
- Ścieżki dłuższe niż 2048 znaków
- Część DACL deskryptora zabezpieczeń, jeśli jest większa niż 2K (jest to problem tylko wtedy, gdy masz więcej niż około 40 wpisów kontroli dostępu dla pojedynczego elementu)
- Część SACL deskryptora zabezpieczeń (używana do przeprowadzania inspekcji)
- Atrybuty rozszerzone
- Alternatywne strumienie danych
- Punkty ponownej analizy
- Twarde linki
- Kompresja (jeśli jest ustawiona w pliku serwera) nie zostanie zachowana, gdy zmiany z innych punktów końcowych zostaną zsynchronizowane z tym plikiem
- Każdy plik zaszyfrowany za pomocą systemu szyfrowania plików (lub innego szyfrowania trybu użytkownika), który uniemożliwia naszej usłudze odczytywanie danych 
    
    > [!Note]  
    > Usługa Azure File Sync zawsze szyfruje przesyłane dane i dane mogą być szyfrowane podczas przechowywania na platformie Azure.
 
### <a name="server-endpoints"></a>Punkty końcowe serwera
- Punkt końcowy serwera można tworzyć tylko na woluminie NTFS. Systemy plików ReFS, FAT, FAT32 i inne nie są obecnie obsługiwane przez usługę Azure File Sync.
- Punkt końcowy serwera nie może znajdować się na woluminie systemowym (na przykład C:\MyFolder nie jest akceptowalną ścieżką, chyba że C:\MyFolder jest punktem instalacji).
- Klaster pracy awaryjnej jest obsługiwany tylko przy użyciu dysków klastrowanych, a nie przy użyciu udostępnionych woluminów klastra (CSV).
- Punkt końcowy serwera nie może być zagnieżdżony, ale może współistnieć na tym samym woluminie równolegle z innym.
- Usuwanie dużej liczby katalogów z serwera jednocześnie (ponad 10 000) może powodować błędy synchronizacji — usuwaj katalogi w partiach poniżej 10 000 i przed usunięciem następnej partii upewnij się, że operacje usuwania zostały pomyślnie zsynchronizowane.
- Nieobsługiwane w katalogu głównym woluminu.
- Nie należy przechowywać pliku stronicowania systemu operacyjnego lub aplikacji w ramach punktu końcowego serwera.
 
### <a name="cloud-tiering"></a>Obsługa warstw w chmurze
- W celu zapewnienia, że pliki można poprawnie odwołać, system nie może automatycznie obsługiwać warstw nowych lub zmienionych plików przez maksymalnie 32 godziny, łącznie z obsługą warstw po raz pierwszy po skonfigurowaniu nowego punktu końcowego serwera — udostępniamy polecenie cmdlet programu PowerShell do obsługi warstw na żądanie, więc możesz ocenić obsługę warstw wydajniej bez oczekiwania na proces w tle.
- Jeśli warstwa pliku zostanie skopiowana za pomocą rozszerzenia Robocopy do innej lokalizacji, wynikowy plik nie będzie obsługiwany w warstwie, ale atrybut offline może zostać ustawiony, ponieważ rozszerzenie Robocopy niepoprawnie dołącza ten atrybut podczas operacji kopiowania.
- Podczas przeglądania właściwości plików z klienta SMB atrybut offline może wyglądać na ustawiony niepoprawnie z powodu buforowania plików metadanych przez SMB.
