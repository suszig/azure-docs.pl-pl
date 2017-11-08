---
title: "Informacje o wersji 0,2 StorSimple 8000 z aktualizacją Update | Dokumentacja firmy Microsoft"
description: "Opisuje nowe funkcje i poprawki, otwórz problemy i obejścia dostępne dla stycznia 2015 release Microsoft Azure StorSimple (aktualizacja 0,2)."
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: d9684ae3-b38f-4678-9d70-e5dbc6b03350
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 11/03/2017
ms.author: v-sharos
ms.openlocfilehash: f2b147f32f10208b8daa391095a7d0094a0c2c09
ms.sourcegitcommit: 0930aabc3ede63240f60c2c61baa88ac6576c508
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/07/2017
---
# <a name="storsimple-8000-series-update-02-release-notes---january-2015"></a>Informacje o wersji 0,2 StorSimple 8000 z aktualizacją Update - stycznia 2015
> [!NOTE]
> Klasyczny portal dla urządzenia StorSimple jest przestarzały. Menedżerowie urządzenia StorSimple zostanie automatycznie przełączona do nowego portalu Azure, zgodnie z harmonogramem wycofywanie. Otrzymasz wiadomość e-mail i powiadomienie portalu dla tego przeniesienia. Ten dokument również zostaną wkrótce wycofane. Wszelkie pytania dotyczące przeniesienie, zobacz [— często zadawane pytania: przejść do portalu Azure](storsimple-8000-move-azure-portal-faq.md).

## <a name="overview"></a>Omówienie
W poniższych informacjach o zidentyfikować problemy krytyczne Otwórz wydania stycznia 2015 Microsoft Azure StorSimple. Zawierają one również listę StorSimple aktualizacje oprogramowania zawarte w tej wersji. To wydanie drugie po wersji StorSimple 8000 serii zlecenia nastąpiła ogólnie dostępna w lipca 2014.

Ta aktualizacja nie powoduje zmiany wersji oprogramowania na urządzeniu fizycznym z października aktualizacji. Nadal można wersji 6.3.9600.17312. Obraz używany przez urządzenie wirtualne obraz został zmieniony w tej wersji. W związku z tym wszystkie nowe urządzenia wirtualne utworzone po 2015-1-20 będzie wyświetlany w wersji co 6.3.9600.17361.  

Przejrzyj następujące informacje zawarte w informacjach o wersji dla aktualizacji stycznia 2015 r.

> [!IMPORTANT]
> * Ta aktualizacja nie jest dostępna za pośrednictwem usługi Windows Update i nie można zainstalować podobnie jak inne aktualizacje. Urządzenie nie otrzymają tej aktualizacji, nawet jeśli te aktualizacje zostały zastosowane przy użyciu klasycznego portalu Azure. Ta aktualizacja będzie dotyczyć tylko urządzeń wirtualnych utworzonych po 20 stycznia 2015 r. 
> * Wersja stycznia StorSimple nie zawiera żadnych aktualizacji do urządzenia fizycznego StorSimple. Można nadal stosować dostępne aktualizacje systemu Windows do urządzenia wirtualnego, w tym najnowsze poprawki zabezpieczeń, ale nie zobaczą zmiany w wersji dla urządzenia fizycznego StorSimple.
> 
> 

## <a name="whats-new-in-the-january-release"></a>Nowości w wersji stycznia
Ta aktualizacja zawiera poprawkę związanych z woluminami przechodzi do trybu offline na urządzeniu wirtualnym. (Zobacz [rozwiązać problemy w tej wersji](#issues-fixed-in-the-january-release).)  

Aktualizacja nie zawiera nowych funkcji lub funkcji.  

## <a name="issues-fixed-in-the-january-release"></a>Problemy rozwiązane w wersji stycznia
W poniższej tabeli opisano problem, który został rozwiązany w ramach tej aktualizacji.

| Nie. | Funkcja | Problem | Dotyczy urządzenia fizycznego | Dotyczy urządzenia wirtualnego |
| --- | --- | --- | --- | --- |
| 1 |Woluminy przechodzi do trybu offline |Opóźnienia wysokiej chmury będą się powtarzać kilka minut, woluminy urządzenia wirtualnego StorSimple Przejdź w trybie offline na hostach. Ta poprawka zwiększa próg opóźnienia chmury, a tym samym minimalizując w sytuacjach, powodujących woluminów przejścia w tryb offline na hostach. |Nie |Tak |

## <a name="known-issues-in-the-january-release"></a>Znane problemy w wersji stycznia
Poniższa tabela zawiera podsumowanie znanych problemów występujących w tej wersji.

| Nie. | Funkcja | Problem | Komentarz/obejście | Dotyczy urządzenia fizycznego | Dotyczy urządzenia wirtualnego |
| --- | --- | --- | --- | --- | --- |
| 1 |Resetowanie do ustawień fabrycznych |W niektórych przypadkach po wykonaniu Resetowanie do ustawień fabrycznych urządzenia StorSimple może zostać zatrzymane i wyświetla ten komunikat: **resetowania z fabryką jest w toku (faza 8).** Dzieje się tak, jeśli naciśnij klawisze CTRL + C, gdy polecenie cmdlet jest w toku. |Naciskaj klawisze CTRL + C po zainicjowaniu resetowania do ustawień fabrycznych. Jeśli jesteś już w tym stanie, skontaktuj się z Microsoft Support, dalsze czynności. |Tak |Nie |
| 2 |Dysku kworum |W rzadkich przypadkach jeżeli większość dysków w obudowie EBOD urządzenia 8600 są odłączone spowodować, że nie ma dysku kworum, następnie puli magazynu będzie w trybie offline. Pozostanie on w trybie offline nawet wtedy, gdy dyski są podłączeni ponownie. |Należy ponowne uruchomienie urządzenia. Jeśli problem będzie się powtarzać, skontaktuj się z Microsoft Support, dalsze czynności. |Tak |Nie |
| 3 |Błędy migawkę chmury |W rzadkich przypadkach migawka w chmurze może zakończyć się niepowodzeniem z powodu błędu **osiągnięto limit kopii zapasowej maksymalnie**. Dzieje się tak, jeśli przekracza 255 klony online na jednym urządzeniu, z tym samym oryginalnego woluminu, który został usunięty. | |Tak |Tak |
| 4 |Identyfikator niepoprawne kontrolera |Po wykonaniu zastąpić kontrolera kontrolera 0 mogą wyglądać jak kontrolera 1. Podczas wymiany kontrolera podczas ładowania obrazu z węzła równorzędnego identyfikator kontrolera można wyświetlane początkowo jako identyfikator kontrolera elementu równorzędnego.  W rzadkich przypadkach to zachowanie może być widoczny po ponownym uruchomieniu systemu. |Nie jest wymagana żadna akcja użytkownika. Sytuacja ta zostanie rozwiązany samoczynnie po zakończeniu zastąpienia kontrolera. |Tak |Nie |
| 5 |Wykresy monitorowania urządzeń |W usłudze Menedżer StorSimple wykresy monitorowania urządzenia nie działają w przypadku podstawowych lub w konfiguracji serwera proxy dla tego urządzenia jest włączone uwierzytelnianie NTLM. |Zmodyfikuj konfigurację serwera proxy sieci web dla urządzenia zarejestrowane przy użyciu usługi Menedżer StorSimple, tak że uwierzytelnianie ma wartość NONE. Aby to zrobić, uruchom program Windows PowerShell dla polecenia cmdlet StorSimple Set-HcsWebProxy. |Tak |Tak |
| 6 |Konta magazynu |Korzystanie z usługi Magazyn można usunąć konta magazynu jest to nieobsługiwany scenariusz. Spowoduje to doprowadzić do sytuacji, w której nie można pobrać danych użytkownika. | |Tak |Tak |
| 7 |Urządzenia trybu failover |Wiele przechodzenia kontener woluminów z tego samego urządzenia źródłowego na urządzeniach inny element docelowy nie jest obsługiwane. |Tryb failover z jednym urządzeniu martwy na wielu urządzeniach spowoduje, że kontenery woluminów na pierwszym przejścia w tryb failover urządzeń utraty własności danych. Po przełączeniu te kontenery woluminów są wyświetlane lub zachowywać się inaczej, podczas wyświetlania w klasycznym portalu Azure. |Tak |Nie |
| 8 |Instalacja |Podczas StorSimple karty dla instalacji programu SharePoint musisz podać IP urządzenia, aby instalacja zakończyło się pomyślnie. | |Tak |Nie |
| 9 |Serwer proxy sieci Web |Jeśli konfiguracji serwera proxy sieci web ma określony protokół HTTPS, będzie mieć wpływ na komunikacji usługi urządzeń i urządzenie przejdzie w tryb offline. Obsługa pakietów również zostanie wygenerowany w procesie wykorzystywanie znaczące ilości zasobów na urządzeniu. |Upewnij się, że adres URL serwera proxy sieci web ma HTTP jako protokół określony. Wyświetl więcej informacji na temat sposobu [skonfigurować serwer proxy sieci web dla danego urządzenia](storsimple-configure-web-proxy.md). |Tak |Nie |
| 10 |Serwer proxy sieci Web |Jeśli możesz skonfigurować i włączyć serwer proxy sieci web na zarejestrowanym urządzeniu, będzie konieczne ponowne uruchomienie kontrolera active na urządzeniu. | |Tak |Nie |
| 11 |Wysoka chmury opóźnienia i wysokie obciążenie We/Wy |Gdy urządzenie StorSimple napotka kombinacją chmury w bardzo duże opóźnienia (kolejność sekund) i wysokie obciążenie We/Wy, woluminy urządzenia przechodzi w stan obniżeniem i operacji We/Wy może zakończyć się niepowodzeniem z powodu błędu "urządzenie nie jest gotowe". |Konieczne będzie ręczne ponownego uruchamiania kontrolerów urządzeń lub urządzeń w tryb failover do odzyskania z tej sytuacji. |Tak |Nie |

## <a name="physical-device-updates-in-the-january-release"></a>Urządzenie fizyczne aktualizacje w styczniu zlecenia
Ta aktualizacja zawiera inne zmiany w urządzeniu StorSimple.

## <a name="serial-attached-scsi-sas-controller-and-firmware-updates-in-the-january-release"></a>Kontroler magistrali Serial attached SCSI (SAS) i aktualizacje oprogramowania układowego w styczniu zlecenia
Ta wersja nie zawiera żadnych aktualizacji do kontrolera magistrali serial attached SCSI (SAS) lub oprogramowania układowego. Aktualizacja sterownika był października 2014 wersji. 

## <a name="virtual-device-updates-in-the-january-release"></a>Urządzenie wirtualne aktualizacje w styczniu zlecenia
Ta wersja zawiera zaktualizowanego obrazu dla urządzenia wirtualnego. Urządzenia wirtualne utworzone po 20 stycznia 2015 wyświetli wersji oprogramowania jako 6.3.9600.17361.

