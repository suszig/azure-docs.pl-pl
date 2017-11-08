---
title: StorSimple 8000 wersji release informacje o wersji | Dokumentacja firmy Microsoft
description: "Opisuje nowe funkcje, otwórz problemy i obejścia dostępne dla lipiec 2014 Microsoft Azure StorSimple wersji."
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: 12f1796e-37c3-42b4-b997-a84fc1950c20
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 11/03/2017
ms.author: v-sharos
ms.openlocfilehash: e6e087ad61ee54fef3552895c00a478fa51e2072
ms.sourcegitcommit: 0930aabc3ede63240f60c2c61baa88ac6576c508
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/07/2017
---
# <a name="storsimple-8000-series-release-version-release-notes---july-2014"></a>Informacje o wersji Wersja Release serii StorSimple 8000 — lipiec 2014

> [!NOTE]
> Klasyczny portal dla urządzenia StorSimple jest przestarzały. Menedżerowie urządzenia StorSimple zostanie automatycznie przełączona do nowego portalu Azure, zgodnie z harmonogramem wycofywanie. Otrzymasz wiadomość e-mail i powiadomienie portalu dla tego przeniesienia. Ten dokument również zostaną wkrótce wycofane. Wszelkie pytania dotyczące przeniesienie, zobacz [— często zadawane pytania: przejść do portalu Azure](storsimple-8000-move-azure-portal-faq.md).

## <a name="overview"></a>Omówienie

Informacje o wersji wykonaj zidentyfikować krytyczne problemy otwarte serii StorSimple 8000 wersji ogólnodostępnej (GA) lipca 2014 Microsoft Azure StorSimple. Ta wersja odpowiada wersji oprogramowania 6.3.9600.17215.

Jeżeli nie określono inaczej, te informacje o wersji dotyczą wszystkich modeli urządzenia StorSimple. Informacje o wersji są stale aktualizowane; wykrywane krytyczne problemy wymagające obejścia zostaną dodane. Przed wdrożeniem rozwiązania Microsoft Azure StorSimple, należy wziąć pod uwagę następujące informacje.

## <a name="known-issues-in-this-release"></a>Znane problemy w tej wersji

Poniższa tabela zawiera podsumowanie znanych problemów występujących w tej wersji.  

| Nie. | Funkcja | Problem | Komentarz/obejście | Dotyczy urządzenia fizycznego | Dotyczy urządzenia wirtualnego |
| --- | --- | --- | --- | --- | --- |
| 1 |Resetowanie do ustawień fabrycznych |W niektórych przypadkach po wykonaniu Resetowanie do ustawień fabrycznych urządzenia StorSimple może zostać zatrzymane i wyświetla ten komunikat: **resetowania z fabryką jest w toku (faza 8)**. Dzieje się tak, jeśli naciśnij klawisze CTRL + C, gdy polecenie cmdlet jest w toku. |Naciskaj klawisze CTRL + C po zainicjowaniu resetowania do ustawień fabrycznych. Jeśli jesteś już w tym stanie, skontaktuj się z Microsoft Support następnych kroków. |Tak |Nie |
| 2 |Dysku kworum |W rzadkich przypadkach jeśli większość dysków w obudowie EBOD urządzenia 8600 rozłączenia spowodować, że nie ma dysku kworum, następnie puli magazynu przejdzie do trybu offline. Pozostanie on w trybie offline nawet wtedy, gdy dyski są podłączeni ponownie. |Należy ponowne uruchomienie urządzenia. Jeśli problem będzie się powtarzać, skontaktuj się z Microsoft Support, dalsze czynności. |Tak |Nie |
| 3 |Błędy migawkę chmury |W rzadkich przypadkach migawka w chmurze może zakończyć się niepowodzeniem z powodu błędu **osiągnięto limit kopii zapasowej maksymalnie**. Dzieje się tak, jeśli przekracza 255 klony online na jednym urządzeniu, z tym samym oryginalnego woluminu, który został usunięty. | |Tak |Tak |
| 4 |Identyfikator niepoprawne kontrolera |Po wykonaniu zastąpić kontrolera kontrolera 0 mogą wyglądać jak kontrolera 1. Podczas wymiany kontrolera podczas ładowania obrazu z węzła równorzędnego identyfikator kontrolera można wyświetlane początkowo jako identyfikator kontrolera elementu równorzędnego. W rzadkich przypadkach to zachowanie może być widoczny po ponownym uruchomieniu systemu. |Nie jest wymagana żadna akcja użytkownika. Sytuacja ta zostanie rozwiązany samoczynnie po zakończeniu zastąpienia kontrolera. |Tak |Nie |
| 5 |Wykresy monitorowania urządzeń |W usłudze Menedżer StorSimple wykresy monitorowania urządzenia nie działają w przypadku podstawowych lub w konfiguracji serwera proxy dla tego urządzenia jest włączone uwierzytelnianie NTLM. |Zmodyfikuj konfigurację serwera proxy sieci web dla urządzenia zarejestrowane przy użyciu usługi Menedżer StorSimple, tak że uwierzytelnianie ma wartość NONE. Aby to zrobić, uruchom program Windows PowerShell polecenia cmdlet StorSimple Set-HcsWebProxy. |Tak |Tak |
| 6 |Konta magazynu |Korzystanie z usługi Magazyn można usunąć konta magazynu jest to nieobsługiwany scenariusz. Spowoduje to doprowadzić do sytuacji, w której nie można pobrać danych użytkownika. | |Tak |Tak |
| 7 |Powrót po awarii |Powrót po awarii w ciągu 24 godzin odzyskiwania awaryjnego (DR) nie jest obsługiwana. | |Tak |Nie |
| 8 |Urządzenia trybu failover |Wiele przechodzenia kontener woluminów z tego samego urządzenia źródłowego na urządzeniach inny element docelowy nie jest obsługiwane. Tryb failover z jednym urządzeniu martwy na wielu urządzeniach spowoduje, że kontenery woluminów na pierwszym przejścia w tryb failover urządzeń utraty własności danych. Po przełączeniu te kontenery woluminów są wyświetlane lub zachowywać się inaczej, podczas wyświetlania w klasycznym portalu Azure. | |Tak |Nie |
| 9 |Instalacja |Podczas StorSimple karty dla instalacji programu SharePoint musisz podać IP urządzenia, dla instalacji zakończyło się pomyślnie. | |Tak |Nie |
| 10 |Interfejsy sieciowe |Interfejsy sieciowe dane 2 i dane 3 zostały zamienione w oprogramowaniu. |Skontaktuj się z Microsoft Support, jeśli konieczne będzie skonfigurowanie tych interfejsów. |Tak |Nie |

