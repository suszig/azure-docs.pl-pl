---
title: "Informacje o wersji 0,3 StorSimple 8000 z aktualizacją Update | Dokumentacja firmy Microsoft"
description: "Opisuje nowe funkcje i poprawki, otwórz problemy i obejścia dostępne dla luty 2015 release Microsoft Azure StorSimple (aktualizacja 0,3)."
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: b01bfd04-f9f8-45f4-ade8-95ac2b979e6a
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 04/18/2016
ms.author: v-sharos
ms.openlocfilehash: c059fd74854813615754e67547497b7ededbe4dd
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="storsimple-8000-series-update-03-release-notes---february-2015"></a>Informacje o wersji 0,3 StorSimple 8000 z aktualizacją Update — luty 2015
## <a name="overview"></a>Omówienie
W poniższych informacjach o zidentyfikować problemy krytyczne Otwórz aktualizacji serii StorSimple 8000 0,3 wydane w ramach luty 2015. Zawierają one również listę StorSimple aktualizacje oprogramowania zawarte w tej wersji. Jest to trzeci wersji po wersji StorSimple 8000 serii zlecenia nastąpiła ogólnie dostępna w lipca 2014.

Ta aktualizacja nie zmienia aktualizacji stycznia wersji oprogramowania na urządzeniu. Nadal można wersji 6.3.9600.17312. Można potwierdzić, że aktualizacja została zainstalowana przez sprawdzenie **ostatniej aktualizacji** daty. Jeśli data jest 2/10/2015 lub nowszego, a następnie aktualizacja została pomyślnie zainstalowana.  

Firma Microsoft zaleca skanowanie pod kątem oraz zastosować dostępne aktualizacje od razu po zainstalowaniu urządzenia StorSimple. Można również włączyć Aktualizacje automatyczne, aby pobrać i zainstalować aktualizacje o wysokim priorytecie od firmy Microsoft, jak ich wydania. Aby uzyskać więcej informacji, zobacz [aktualizacja urządzenia StorSimple](storsimple-update-device.md).  

Przejrzyj informacje zawarte w informacjach o wersji przed wdrożeniem aktualizacji w rozwiązaniu StorSimple.  

> [!IMPORTANT]
> * Użyj usługi Menedżer StorSimple i nie środowiska Windows PowerShell dla urządzenia StorSimple do zainstalowania aktualizacji lutego.   
> * Trwa około godzinę, aby zainstalować tę aktualizację. Jednak jeśli instalujesz aktualizacje zbiorcze proces może zająć około 3 godziny. Aby zakończyć.  
> * Wersja lutego StorSimple nie zawiera żadnych aktualizacji do urządzenia wirtualnego StorSimple. Można nadal stosować dostępne aktualizacje systemu Windows do urządzenia wirtualnego, w tym najnowsze poprawki zabezpieczeń, ale nie zobaczą zmiany w wersji dla urządzenia wirtualnego.  
> 
> 

Upewnij się, że spełniono następujące wymagania wstępne, przed zaktualizowaniem urządzenia StorSimple.  

* Upewnij się, że zarówno kontrolery urządzeń są uruchomione przed rozpoczęciem skanowania aktualizacji. Jeśli nie działa albo kontrolera, skanowanie zakończy się niepowodzeniem. Aby sprawdzić, czy kontrolery są w dobrej kondycji, przejdź do **stan sprzętu** w obszarze **konserwacji** strony. Jeśli istnieją składniki który **wymaga uwagi**, skontaktuj się z Microsoft Support przed kontynuowaniem.
* Upewnij się, że stałe adresy IP dla kontrolera 0 i kontrolera 1 są rutowalne i można połączyć się z Internetem, ponieważ są one używane do obsługi aktualizacji na urządzeniu. Można użyć [polecenia cmdlet Test-Connection](https://technet.microsoft.com/library/hh849808.aspx) na polecenie ping znanego adresu spoza sieci, takich jak outlook.com, aby sprawdzić, czy kontroler jest połączony z siecią zewnętrzną.
* Upewnij się, że porty 80 i 443 są dostępne na urządzeniu StorSimple dla komunikacji wychodzącej. Aby uzyskać więcej informacji, zobacz [wymagania dotyczące sieci dla urządzenia StorSimple](storsimple-system-requirements.md#networking-requirements-for-your-storsimple-device).
* Jeśli wersja oprogramowania na urządzeniu jest starsza niż 6.3.9600.17312 (aktualizacja z października 2014 r.), jeśli włączone, przed rozpoczęciem aktualizacji należy wyłączyć porty dane 2 i dane 3. Pozostawienie dane 2 i dane 3 porty włączona po zastosowaniu aktualizacji może spowodować kontrolerze urządzenia przejść do trybu odzyskiwania. Należy pamiętać, po wyłączeniu interfejsy sieciowe skojarzone woluminy zostaną przełączone do trybu offline i operacji We/Wy zostanie zakłócone na czas trwania aktualizacji.  

## <a name="whats-new-in-the-february-release"></a>Nowości w wersji lutego
Ta aktualizacja zawiera poprawkę dla fabrycznie zresetować problem, który wystąpił na urządzenia, które uaktualnione z GA wersji do wersji z października 2014 r. Aby uzyskać więcej informacji, zobacz [rozwiązać problemy w tej wersji](#issues-fixed-in-the-february-release).   

Ta aktualizacja nie zawiera nowych funkcji lub funkcji.  

## <a name="issues-fixed-in-the-february-release"></a>Problemy rozwiązane w wersji lutego
W poniższej tabeli opisano problem, który został rozwiązany w ramach tej aktualizacji.  

| Nie. | Funkcja | Problem | Dotyczy urządzenia fizycznego | Dotyczy urządzenia wirtualnego |
| --- | --- | --- | --- | --- |
| 1 |Resetowanie do ustawień fabrycznych |Spróbuj przeprowadzić resetowanie urządzenia, która pierwotnie była zainstalowana wersja Ogólnodostępna (wersja 6.3.9600.17215), ale została zaktualizowana do października do ustawień fabrycznych wersji (wersja 6.3.9600.17312). Fabryka zresetować kończy się niepowodzeniem i urządzenie staje się niestabilny. |Tak |Nie |

## <a name="known-issues-in-the-february-release"></a>Znane problemy w wersji lutego
Poniższa tabela zawiera podsumowanie znanych problemów występujących w tej wersji.

| Nie. | Funkcja | Problem | Komentarz/obejście | Dotyczy urządzenia fizycznego | Dotyczy urządzenia wirtualnego |
| --- | --- | --- | --- | --- | --- |
| 1 |Resetowanie do ustawień fabrycznych |W niektórych przypadkach po wykonaniu Resetowanie do ustawień fabrycznych urządzenia StorSimple może zostać zatrzymane i wyświetla ten komunikat: **resetowania z fabryką jest w toku (faza 8)**. Dzieje się tak, jeśli naciśnij klawisze CTRL + C, gdy polecenie cmdlet jest w toku. |Naciskaj klawisze CTRL + C po zainicjowaniu resetowania do ustawień fabrycznych. Jeśli jesteś już w tym stanie, skontaktuj się z Microsoft Support, dalsze czynności. |Tak |Nie |
| 2 |Dysku kworum |W rzadkich przypadkach jeżeli większość dysków w obudowa EBOD 8600device są odłączone spowodować, że nie ma dysku kworum, następnie puli magazynu będzie w trybie offline. Pozostanie on w trybie offline nawet wtedy, gdy dyski są podłączeni ponownie. |Należy ponowne uruchomienie urządzenia. Jeśli problem będzie się powtarzać, skontaktuj się z Microsoft Support, dalsze czynności. |Tak |Nie |
| 3 |Błędy migawkę chmury |W rzadkich przypadkach migawka w chmurze może zakończyć się niepowodzeniem z powodu błędu **osiągnięto limit kopii zapasowej maksymalnie**. Dzieje się tak, jeśli przekracza 255 klony online na jednym urządzeniu, z tym samym oryginalnego woluminu, który został usunięty. | |Tak |Tak |
| 4 |Identyfikator niepoprawne kontrolera |Po wykonaniu zastąpić kontrolera kontrolera 0 mogą wyglądać jak kontrolera 1. Podczas wymiany kontrolera podczas ładowania obrazu z węzła równorzędnego identyfikator kontrolera można wyświetlane początkowo jako identyfikator kontrolera elementu równorzędnego. W rzadkich przypadkach to zachowanie może być widoczny po ponownym uruchomieniu systemu. |Nie jest wymagana żadna akcja użytkownika. Sytuacja ta zostanie rozwiązany samoczynnie po zakończeniu zastąpienia kontrolera. |Tak |Nie |
| 5 |Wykresy monitorowania urządzeń |W usłudze Menedżer StorSimple wykresy monitorowania urządzenia nie działają w przypadku podstawowych lub w konfiguracji serwera proxy dla tego urządzenia jest włączone uwierzytelnianie NTLM. |Zmodyfikuj konfigurację serwera proxy sieci web dla urządzenia zarejestrowane przy użyciu usługi Menedżer StorSimple, tak że uwierzytelnianie ma wartość NONE. Aby to zrobić, uruchom program Windows PowerShell dla polecenia cmdlet StorSimple Set-HcsWebProxy. |Tak |Tak |
| 6 |Konta magazynu |Korzystanie z usługi Magazyn można usunąć konta magazynu jest to nieobsługiwany scenariusz. Spowoduje to doprowadzić do sytuacji, w której nie można pobrać danych użytkownika. | |Tak |Tak |
| 7 |Urządzenia trybu failover |Wiele przechodzenia kontener woluminów z tego samego urządzenia źródłowego na urządzeniach inny element docelowy nie jest obsługiwane.    Tryb failover z jednym urządzeniu martwy na wielu urządzeniach spowoduje, że kontenery woluminów na pierwszym przejścia w tryb failover urządzeń utraty własności danych. Po przełączeniu te kontenery woluminów są wyświetlane lub zachowywać się inaczej, podczas wyświetlania w klasycznym portalu Azure. | |Tak |Nie |
| 8 |Instalacja |Podczas StorSimple karty dla instalacji programu SharePoint musisz podać IP urządzenia, aby instalacja zakończyło się pomyślnie. | |Tak |Nie |
| 9 |Serwer proxy sieci Web |Jeśli konfiguracji serwera proxy sieci web ma określony protokół HTTPS, będzie mieć wpływ na komunikacji usługi urządzeń i urządzenie przejdzie w tryb offline. Obsługa pakietów również zostanie wygenerowany w procesie wykorzystywanie znaczące ilości zasobów na urządzeniu. |Upewnij się, że adres URL serwera proxy sieci web ma HTTP jako protokół określony. Więcej informacji na temat sposobu [skonfigurować serwer proxy sieci web dla danego urządzenia](storsimple-configure-web-proxy.md). |Tak |Nie |
| 10 |Serwer proxy sieci Web |Jeśli możesz skonfigurować i włączyć serwer proxy sieci web na zarejestrowanym urządzeniu, będzie konieczne ponowne uruchomienie kontrolera active na urządzeniu. | |Tak |Nie |
| 11 |Wysoka chmury opóźnienia i wysokie obciążenie We/Wy |Gdy urządzenie StorSimple napotka kombinacją chmury w bardzo duże opóźnienia (kolejność sekund) i wysokie obciążenie We/Wy, woluminy urządzenia przechodzi w stan obniżeniem i operacji We/Wy może zakończyć się niepowodzeniem z powodu błędu "urządzenie nie jest gotowe". |Konieczne będzie ręczne ponownego uruchamiania kontrolerów urządzeń lub urządzeń w tryb failover do odzyskania z tej sytuacji. |Tak |Nie |

## <a name="physical-device-updates-in-the-february-release"></a>Urządzenie fizyczne aktualizacje w lutym zlecenia
Ta aktualizacja rozwiązuje problem resetowania fabryki, który wystąpił na urządzenia, które uaktualnione z GA wersji do wersji z października 2014 r. Nie zawiera innych aktualizacji na urządzeniu StorSimple.  

## <a name="serial-attached-scsi-sas-controller-and-firmware-updates-in-the-february-release"></a>Kontroler magistrali Serial attached SCSI (SAS) i aktualizacje oprogramowania układowego w lutym zlecenia
Ta wersja nie zawiera żadnych aktualizacji do kontrolera magistrali serial attached SCSI (SAS) lub oprogramowania układowego. Aktualizacja sterownika był października 2014 wersji.  

## <a name="virtual-device-updates-in-the-february-release"></a>Urządzenie wirtualne aktualizacje w lutym zlecenia
Ta wersja nie zawiera żadnych aktualizacji dla urządzenia wirtualnego. Zastosowanie tej aktualizacji nie spowoduje zmiany wersji oprogramowania urządzenia wirtualnego.

