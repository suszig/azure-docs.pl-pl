---
title: "Informacje o wersji 0,1 StorSimple 8000 z aktualizacją Update | Dokumentacja firmy Microsoft"
description: "Opisuje nowe funkcje i poprawki, otwórz problemy i obejścia dostępne dla października 2014 r. wersji Microsoft Azure StorSimple (aktualizacja 0,1)."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: fd35e3c3-4770-460c-999d-f72ab7053a20
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 09/21/2016
ms.author: alkohli
ms.openlocfilehash: 4dfd3973593a94adfc15a6e15d69c697e13998af
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="storsimple-8000-series-update-01-release-notes--october-2014"></a>StorSimple 8000 z aktualizacją Update 0.1 wersji — październik 2014 r.
## <a name="overview"></a>Omówienie
W poniższych informacjach o zidentyfikować krytyczne problemy otwarte dla aktualizacji serii StorSimple 8000 0,1 wydane w ramach października 2014 r. Zawierają one również listę StorSimple aktualizacje oprogramowania zawarte w tej wersji. Jest to pierwsze wydanie po wersji StorSimple 8000 serii zlecenia nastąpiła ogólnie dostępna w lipca 2014 r. i odpowiada wersji oprogramowania 6.3.9600.17312.  

Firma Microsoft zaleca skanowanie pod kątem oraz zastosować dostępne aktualizacje od razu po zainstalowaniu urządzenia. Można również włączyć Aktualizacje automatyczne, aby pobrać i zainstalować aktualizacje o wysokim priorytecie od firmy Microsoft, jak ich wydania. Aby uzyskać więcej informacji, zobacz temat jak [aktualizacja urządzenia StorSimple](storsimple-update-device.md).  

Przejrzyj informacje zawarte w informacjach o wersji przed wdrożeniem aktualizacji w rozwiązaniu StorSimple.  

> [!IMPORTANT]
> * Użyj usługi Menedżer StorSimple i nie programu Windows PowerShell dla urządzenia StorSimple, aby zainstalować aktualizacje października.  
> * Aktualizacje zwykle zajmują około 3 godziny, aby zakończyć.  
> * Wersja października StorSimple nie zawiera żadnych aktualizacji do urządzenia wirtualnego StorSimple. Można nadal stosować wszystkich dostępnych aktualizacji systemu Windows, w tym najnowsze poprawki zabezpieczeń, ale nie zobaczą zmiany w wersji dla urządzenia wirtualnego.  
> 
> 

Upewnij się, że są spełnione następujące wymagania wstępne, przed zaktualizowaniem urządzenia StorSimple.  

* Upewnij się, że zarówno kontrolery urządzeń są uruchomione przed rozpoczęciem skanowania aktualizacji. Jeśli nie działa albo kontrolera, skanowanie zakończy się niepowodzeniem. Aby sprawdzić, czy kontrolery są w dobrej kondycji, przejdź do **stan sprzętu** w obszarze **konserwacji** strony. Jeśli istnieją składniki który **wymaga uwagi**, skontaktuj się z Microsoft Support przed kontynuowaniem.  
* Sprawdź, czy stałe adresy IP dla obu kontrolera 0 i kontrolera 1 są rutowalne i można połączyć się z Internetem, ponieważ są one używane do obsługi aktualizacji na urządzeniu. Można użyć [polecenia cmdlet Test-Connection](https://technet.microsoft.com/library/hh849808.aspx) na polecenie ping znanego adresu spoza sieci, takich jak outlook.com, aby sprawdzić, czy kontroler jest połączony z siecią zewnętrzną.  
* Upewnij się, że wymagane porty dla ruchu wychodzącego są dostępne na urządzeniu StorSimple dla komunikacji wychodzącej. Aby uzyskać więcej informacji, zobacz [wymagania dotyczące sieci dla urządzenia StorSimple](storsimple-system-requirements.md#networking-requirements-for-your-storsimple-device).  
* Jeśli wersja oprogramowania na urządzeniu jest starsza niż 6.3.9600.17312 (aktualizacja z października 2014 r.), jeśli włączone, przed rozpoczęciem aktualizacji należy wyłączyć porty dane 2 i dane 3. Jeśli opuścisz dane 2 i dane 3 porty włączona po zastosowaniu tej aktualizacji, może to spowodować kontrolerze urządzenia przejść do trybu odzyskiwania. Zauważ, że po wyłączeniu interfejsy sieciowe skojarzone woluminy zostaną przełączone do trybu offline we/wy zostanie zakłócone na czas trwania aktualizacji.  

## <a name="whats-new-in-the-october-release"></a>Nowości w wersji z października
Ta aktualizacja obejmuje następujące ulepszenia:

* Interfejs użytkownika usługi Menedżer StorSimple umożliwia teraz Zarządzanie kontrolerów urządzeń. Zarządzania, które obejmują akcje ponownego uruchomienia, zamknij lub włączyć kontrolera. Aby uzyskać więcej informacji, przejdź do [kontrolery urządzeń StorSimple zarządzanie](storsimple-manage-device-controller.md).  
* Można zaplanować alokacji przepustowości sieci WAN zgodnie z kombinacji dzień tygodnia i godzina dnia. Umożliwia to lepsze wykorzystanie przepustowości sieci WAN poza godzinami szczytu. Przepustowość różne szablony są dozwolone dla kontenerów inny wolumin. Aby uzyskać więcej informacji, przejdź do [zarządzania szablonami przepustowości StorSimple](storsimple-manage-bandwidth-templates.md).  
* Można skonfigurować powiadomienia e-mail do aktywnego powiadomienia administratorom i innym istniejących lub planowanych prawdopodobnie problemy. Aby uzyskać więcej informacji, przejdź do [Konfigurowanie ustawień alertów](storsimple-manage-alerts.md#configure-alert-settings).  

## <a name="issues-fixed-in-the-october-release"></a>Problemy rozwiązane w wersji z października
Poniższa tabela zawiera podsumowanie problemów, które zostały usunięte w ramach tej aktualizacji.  

| Nie. | Funkcja | Problem | Dotyczy urządzenia fizycznego | Dotyczy urządzenia wirtualnego |
| --- | --- | --- | --- | --- |
| 1 |Interfejsy sieciowe |W poprzedniej wersji oprogramowania zostały zamienione interfejsy sieciowe dane 2 i dane 3. Ten problem został naprawiony w ramach tej aktualizacji. Wyczyść ustawienia i wyłączyć te interfejsy sieciowe, przed zainstalowaniem aktualizacji. Po zainstalowaniu aktualizacji, należy ponownie skonfigurować te interfejsy. |Tak |Nie |
| 2 |Pakiet pomocy technicznej |W poprzednich wersjach, w przypadku uruchomienia programu Windows PowerShell **HcsSupportPackage eksportu** dzienniki polecenia cmdlet, aby pobrać kontrolera zarządzania płytą główną (BMC), operacja nie powiodła się następujące ostrzeżenie: "Operacja powiodła się na tym kontrolerze, ale nie na kontrolerze elementów równorzędnych z powodu następujących błędów. Sprawdź, czy elementu równorzędnego jest w dobrej kondycji i określa, czy bieżący węzeł mogą łączyć się dla elementu równorzędnego." Ten problem teraz zostanie usunięty. |Tak |Nie |
| 3 |Urządzenia trybu failover |W poprzednich wersjach było ryzyko niespójność danych Jeśli **odnajdywanie kopii zapasowej** zadanie nie powiodło się podczas pracy w trybie failover urządzenia. Ten problem teraz zostanie usunięty. |Tak |Nie |
| 4 |Urządzenia trybu failover |W poprzednich wersjach przejściu w tryb failover urządzeń kopie zapasowe były widoczne, ale kontenera woluminów skojarzone nie jest obecny na urządzeniu docelowym. Ten problem teraz zostanie usunięty. |Tak |Nie |
| 5 |Urządzenia trybu failover |W poprzednich wersjach wystąpił błąd w wyliczeniu kopii zapasowych w chmurze podczas operacji przywracania rejestru, który może potencjalnie pozwolić na niespójność danych, jeśli wystąpiły problemy z połączeniem z chmury. |Tak |Nie |
| 6 |Aktualizacja oprogramowania układowego |W poprzednich wersjach zadania aktualizacji oprogramowania układowego urządzenia nie powiodła się i wyświetlony błąd, który stwierdził, że polecenia cmdlet nie jest rozpoznawanym i w związku z tym nie aktualizacji. Kontroler następnie przeszedł do trybu odzyskiwania. Ten problem teraz zostanie usunięty. |Tak |Nie |
| 7 |Instalacja |Teraz Naprawiono błędy spowodowane przez urządzenia nie są obrazami poprawnie podczas instalacji. |Tak |Nie |
| 8 |Resetowanie do ustawień fabrycznych |Teraz można opcjonalnie pominąć sprawdzanie oprogramowania układowego Resetowanie do ustawień fabrycznych. Jest to zmiana z poprzednią wersją. |Tak |Nie |
| 9 |Resetowanie do ustawień fabrycznych |W poprzedniej wersji podczas uruchamiania polecenia cmdlet resetowania fabryki kontroli wersji oprogramowania układowego wprowadzono tylko dla niektórych składników sprzętowych. Po ponownym rozruchu komputera w procesie, która może spowodować niepowodzenie Resetowanie wprowadzono dodatkowe oprogramowanie układowe kontroli. Ta poprawka gwarantuje, że wszystkie oprogramowania układowego sprawdzane jest podczas fabrycznie zresetować polecenia cmdlet jest uruchamiane i przed pierwszym systemu ponowny rozruch. |Tak |Nie |
| 10 |Obracanie klucza konta magazynu |**Invoke HcsmServiceDataEncryptionKeyChange** polecenie cmdlet używane do obracania teraz klucze konta magazynu monituje użytkownika o wprowadzenie klucza szyfrowania danych usługi. Jest to zmiana z poprzedniej wersji, w którym klucz szyfrowania danych usługi został przekazany jako parametr wbudowanego. |Tak |Nie |
| 11 |Powrót po awarii w ciągu 24 godzin |Podczas odzyskiwania po awarii Oczyszczanie na urządzenia źródłowego nie nastąpiły powrotu po awarii bezpośrednio, powodując niepowodzenie. Ten problem został naprawiony w tej wersji. |Tak |Nie |

## <a name="known-issues-in-the-october-release"></a>Znane problemy w wersji z października
Poniższa tabela zawiera podsumowanie znanych problemów występujących w tej wersji.

| Nie. | Funkcja | Problem | Komentarz/obejście | Dotyczy urządzenia fizycznego | Dotyczy urządzenia wirtualnego |
| --- | --- | --- | --- | --- | --- |
| 1 |Resetowanie do ustawień fabrycznych |W niektórych przypadkach po wykonaniu Resetowanie do ustawień fabrycznych urządzenia StorSimple może zostać zatrzymane i wyświetla ten komunikat: **resetowania z fabryką jest w toku (faza 8)**. Dzieje się tak, jeśli naciśnij klawisze CTRL + C, gdy polecenie cmdlet jest w toku. |Naciskaj klawisze CTRL + C po zainicjowaniu resetowania do ustawień fabrycznych. Jeśli jesteś już w tym stanie, skontaktuj się z Microsoft Support, dalsze czynności. |Tak |Nie |
| 2 |Resetowanie do ustawień fabrycznych |Czy nie Resetowanie do ustawień fabrycznych urządzenia StorSimple, które są aktualizowane na podstawie GA do października 2014 wersji. |Ta operacja działa tylko, jeśli zainstalowano poprawkę. Skontaktuj się z Microsoft Support można pobrać tego wymagane poprawki. |Tak |Nie |
| 3 |Dysku kworum |W rzadkich przypadkach jeżeli większość dysków w obudowie EBOD urządzenia 8600 są odłączone spowodować, że nie ma dysku kworum, następnie puli magazynu będzie w trybie offline. Pozostanie on w trybie offline nawet wtedy, gdy dyski są podłączeni ponownie. |Należy ponowne uruchomienie urządzenia. Jeśli problem będzie się powtarzać, skontaktuj się z Microsoft Support, dalsze czynności. |Tak |Nie |
| 4 |Błędy migawkę chmury |W rzadkich przypadkach migawka w chmurze może zakończyć się niepowodzeniem z powodu błędu **osiągnięto limit kopii zapasowej maksymalnie**. Dzieje się tak, jeśli przekracza 255 klony online na jednym urządzeniu, z tym samym oryginalnego woluminu, który został usunięty. | |Tak |Tak |
| 5 |Identyfikator niepoprawne kontrolera |Po wykonaniu zastąpić kontrolera kontrolera 0 mogą wyglądać jak kontrolera 1. Podczas wymiany kontrolera podczas ładowania obrazu z węzła równorzędnego identyfikator kontrolera można wyświetlane początkowo jako identyfikator kontrolera elementu równorzędnego. W rzadkich przypadkach to zachowanie może być widoczny po ponownym uruchomieniu systemu. |Nie jest wymagana żadna akcja użytkownika. Sytuacja ta zostanie rozwiązany samoczynnie po zakończeniu zastąpienia kontrolera. |Tak |Nie |
| 6 |Wykresy monitorowania urządzeń |W usłudze Menedżer StorSimple wykresy monitorowania urządzenia nie działają w przypadku podstawowych lub w konfiguracji serwera proxy dla tego urządzenia jest włączone uwierzytelnianie NTLM. |Zmodyfikuj konfigurację serwera proxy sieci web dla urządzenia zarejestrowane przy użyciu usługi Menedżer StorSimple, tak że uwierzytelnianie ma wartość NONE. Aby to zrobić, uruchom program Windows PowerShell dla polecenia cmdlet StorSimple Set-HcsWebProxy. |Tak |Tak |
| 7 |Konta magazynu |Korzystanie z usługi Magazyn można usunąć konta magazynu jest to nieobsługiwany scenariusz. Spowoduje to doprowadzić do sytuacji, w której nie można pobrać danych użytkownika. | |Tak |Tak |
| 8 |Urządzenia trybu failover |Wiele przechodzenia kontener woluminów z tego samego urządzenia źródłowego na urządzeniach inny element docelowy nie jest obsługiwane. |Tryb failover z jednym urządzeniu martwy na wielu urządzeniach spowoduje, że kontenery woluminów na pierwszym przejścia w tryb failover urządzeń utraty własności danych. Po przełączeniu te kontenery woluminów są wyświetlane lub zachowywać się inaczej, podczas wyświetlania w klasycznym portalu Azure. |Tak |Nie |
| 9 |Instalacja |Podczas StorSimple karty dla instalacji programu SharePoint musisz podać IP urządzenia, aby instalacja zakończyło się pomyślnie. | |Tak |Nie |
| 10 |Serwer proxy sieci Web |Jeśli konfiguracji serwera proxy sieci web ma określony protokół HTTPS, będzie mieć wpływ na komunikacji usługi urządzeń i urządzenie przejdzie w tryb offline. Obsługa pakietów również zostanie wygenerowany w procesie wykorzystywanie znaczące ilości zasobów na urządzeniu. |Upewnij się, że adres URL serwera proxy sieci web ma HTTP jako protokół określony. Więcej informacji na temat sposobu [skonfigurować serwer proxy sieci web dla danego urządzenia](storsimple-configure-web-proxy.md). |Tak |Nie |
| 11 |Serwer proxy sieci Web |Jeśli możesz skonfigurować i włączyć serwer proxy sieci web na zarejestrowanym urządzeniu, będzie konieczne ponowne uruchomienie kontrolera active na urządzeniu. | |Tak |Nie |
| 12 |Wysoka chmury opóźnienia i wysokie obciążenie We/Wy |Gdy urządzenie StorSimple napotka kombinacją chmury w bardzo duże opóźnienia (kolejność sekund) i wysokie obciążenie We/Wy, woluminy urządzenia przechodzi w stan obniżeniem i operacji We/Wy może zakończyć się niepowodzeniem z powodu błędu "urządzenie nie jest gotowe". |Konieczne będzie ręczne ponownego uruchamiania kontrolerów urządzeń lub urządzeń w tryb failover do odzyskania z tej sytuacji. |Tak |Nie |

## <a name="physical-device-updates-in-the-october-release"></a>Urządzenie fizyczne aktualizacje w październiku zlecenia
Jeśli te aktualizacje zostały zastosowane do urządzenia fizycznego, wersji oprogramowania zmieni się na 6.3.9600.17312. Jeżeli nie określono inaczej, te informacje o wersji dotyczą wszystkich modeli urządzenia StorSimple. Aby uzyskać więcej informacji na temat tych aktualizacji, zobacz [aktualizacji oprogramowania urządzenia fizycznego października 2014 r. dla systemu Microsoft Azure StorSimple urządzenia](http://support.microsoft.com/kb/2986997).  

## <a name="serial-attached-scsi-sas-controller-and-firmware-updates-in-the-october-release"></a>Kontroler magistrali Serial attached SCSI (SAS) i aktualizacje oprogramowania układowego w październiku zlecenia
Ta wersja aktualizacje sterowników i oprogramowania układowego od urządzenia fizycznego kontrolera SAS. Aby uzyskać więcej informacji na temat aktualizacji kontrolera SAS, zobacz [aktualizacji października 2014 r. dla kontrolerów LSI SAS w Microsoft Azure StorSimple urządzenia](http://support.microsoft.com/kb/2987020).   

Ta wersja ma również zastosowanie aktualizację zbiorczą oprogramowania układowego, która rozwiązuje problemy ze zgodnością ze składnikami sprzętowymi urządzenia. Aby uzyskać więcej informacji na temat aktualizacji oprogramowania układowego, zobacz [aktualizacji oprogramowania układowego października 2014 r. dla systemu Microsoft Azure StorSimple urządzenia](http://support.microsoft.com/kb/2987015).  

## <a name="virtual-device-updates-in-the-october-release"></a>Urządzenie wirtualne aktualizacje w październiku zlecenia
Ta wersja nie zawiera żadnych aktualizacji dla urządzenia wirtualnego. Zastosowanie tej aktualizacji nie spowoduje zmiany wersji oprogramowania urządzenia wirtualnego.

