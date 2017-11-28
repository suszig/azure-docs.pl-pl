---
title: "Rozwiązywanie problemów z migracji Azure | Dokumentacja firmy Microsoft"
description: "Zawiera omówienie znanych problemów dotyczących usługi Azure migracji i rozwiązywania problemów wskazówki dotyczące typowych błędów."
services: migrate
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 40faffa3f-1f44-4a72-94bc-457222ed7ac8
ms.service: migrate
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 11/22/2017
ms.author: raynew
ms.openlocfilehash: 8c7c79a23ee09a7de35252d7819d1f0e5b1d98c5
ms.sourcegitcommit: f847fcbf7f89405c1e2d327702cbd3f2399c4bc2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2017
---
# <a name="troubleshoot-azure-migrate"></a>Rozwiązywanie problemów z Azure migracji

## <a name="troubleshoot-common-errors"></a>Rozwiąż typowe błędy

[Azure migracji](migrate-overview.md) ocenia obciążeń lokalnych do migracji do usługi Azure. W tym artykule umożliwiają rozwiązywanie problemów podczas wdrażania i przy użyciu migracji Azure.


**Moduł zbierający nie może połączyć się projektu za pomocą Identyfikatora projektu i klucza I skopiowany z portalu.**

Upewnij się, zostały skopiowane i wklejone odpowiednie informacje. Aby rozwiązać problemy, należy zainstalować Microsoft Monitoring Agent (MMA) w następujący sposób:

1. W module zbierającym maszyny Wirtualnej, należy pobrać [MMA](https://go.microsoft.com/fwlink/?LinkId=828603).
2. Aby rozpocząć instalację, kliknij dwukrotnie pobrany plik.
3. W Instalatorze na **powitalnej** kliknij przycisk **dalej**. Na **postanowień licencyjnych** kliknij przycisk **zgadzam się** do akceptowania licencji.
4. W **Folder docelowy**, Zachowaj lub zmienić domyślny folder instalacji > **dalej**.
5. W **opcje instalacji agenta**, wybierz pozycję **Analiza dzienników Azure (OMS)** > **dalej**.
6. Kliknij przycisk **Dodaj** Aby dodać nowy obszar roboczy analizy dzienników. Wklej identyfikator projektu i klucz skopiowane. Następnie kliknij przycisk **Next** (Dalej).
7. Sprawdź, czy agent może połączyć się z projektem. Jeśli nie, sprawdź ustawienia. Jeśli agent może połączyć, ale nie przez moduł zbierający, skontaktuj się z pomocą techniczną.


**Błąd 802: błąd datę i godzinę synchronizacji.**

Zegar serwera może być typu "out synchronizacji" przy użyciu bieżącego czasu, przez ponad pięć minut. Zmiana czasu zegara w module zbierającym maszyny Wirtualnej, aby dopasować bieżący czas, w następujący sposób:

1. Otwórz wiersz polecenia administratora na maszynie Wirtualnej.
2. Aby sprawdzić strefę czasową, uruchom w32tm /tz.
3. Aby zsynchronizować czas, należy uruchomić w32tm/resync.

**Mój klucz projektów ma "==" symbole w końcowej. Są one zakodowane, znaki alfanumeryczne przez moduł zbierający. Jest to oczekiwane?**

Tak, każdy klucz Projekt kończy się wyrazem "==". Moduł zbierający szyfruje klucz projektu przed jego przetworzeniem.

**Przedstawia dane wydajności dotyczące dysków i sieci karty jako zera**

Może to wystąpić, jeśli poziom ustawienie statystyk na serwer vCenter jest ustawiony na mniej niż trzech. Na poziomie 3 lub nowszym vCenter przechowuje historii wydajności maszyny Wirtualnej dla zasobów obliczeniowych, magazynu i sieci. Dla trzech poniżej poziomu vCenter nie przechowuje magazynu i sieci danych, ale tylko dane Procesora i pamięci. W tym scenariuszu wydajności pokazuje dane jako zero w migracji Azure i migracji Azure udostępnia zalecenia rozmiaru dysków i sieci na podstawie metadanych zebrane z komputerów lokalnych.

Aby włączyć zbieranie danych wydajności dysku i sieci, należy zmienić poziom ustawienia statystyki do trzech. Następnie należy poczekać co najmniej jeden dzień w celu odnajdywania środowisku i jego oceny. 

**Agenci zostali zainstalowani i używane do tworzenia grup wizualizacji zależności. Teraz po pracy w trybie failover maszyny Pokaż akcji "Zainstaluj agentów" zamiast "Wyświetl zależności"**
* Post planowane lub nieplanowane przełączenie awaryjne, lokalne maszyny są wyłączone i maszyny równoważne są uruchomione na platformie Azure. Te maszyny uzyskać inny adres MAC. Mogą one uzyskać inny adres IP w oparciu Określa, czy użytkownik wybrał opcję zachowania lokalny adres IP, czy nie. Jeżeli różnią się adresy IP i MAC, Azure migracji nie wiąże lokalnymi maszynami z żadnych danych zależności mapy usług i pyta użytkownika, aby zainstalować agentów zamiast wyświetlanie zależności.
* Opublikuj testowania trybu failover maszyny lokalnej pozostają włączone zgodnie z oczekiwaniami. Odpowiednik maszyny przejścia na platformie Azure uzyskać inny adres MAC oraz może uzyskać inny adres IP. Chyba że użytkownik blokuje ruch wychodzący OMS z urządzeń, Azure migracji nie Skojarz lokalnymi maszynami z żadnych danych zależności mapy usług i pyta użytkownika, aby zainstalować agentów zamiast wyświetlanie zależności.


## <a name="troubleshoot-readiness-issues"></a>Rozwiązywanie problemów gotowości

**Problem** | **Poprawka**
--- | ---
Nieobsługiwany typ rozruchu | Zmień na systemu BIOS, przed rozpoczęciem migracji.
Liczba dysków przekracza limit | Usuń nieużywane dyski na komputerze przed migracją.
Rozmiar dysku przekracza limit | Zmniejszanie dysków do mniej niż 4 TB przed migracją. 
Niedostępne dysku w określonej lokalizacji | Upewnij się, że dysk jest w lokalizacji docelowej, przed przeprowadzeniem migracji.
Niedostępne nadmiarowości określonego dysku | Dysk należy używać typu magazynu nadmiarowość zdefiniowanego w ustawieniach oceny (LRS domyślnie).
Nie można określić przydatności dysk z powodu błędu wewnętrznego | Spróbuj utworzyć nowy oceny grupy. 
Maszyna wirtualna z wymagane rdzeni i ilości pamięci nie można odnaleźć | Azure nie drobne odpowiedniego typu maszyny Wirtualnej. Ograniczenia pamięci i liczby rdzeni na lokalnej maszynie, przed przeprowadzeniem migracji. 
Co najmniej jeden dysk nieodpowiednie. | Upewnij się, że lokalne dyski są 4 TB lub w obszarze przed rozpoczęciem migracji.
Jeden lub więcej kart sieciowych nie nadaje się. | Usuń nieużywane sieciowe z maszyny przed migracją.
Nie można określić przydatności maszyny Wirtualnej z powodu błędu wewnętrznego. | Spróbuj utworzyć nowy oceny grupy. 
Nie można określić przydatności do co najmniej jeden dysk z powodu błędu wewnętrznego. | Spróbuj utworzyć nowy oceny grupy.
Nie można określić przydatności do co najmniej jednej karty sieciowej z powodu błędu wewnętrznego. | Spróbuj utworzyć nowy oceny grupy.
Nie można odnaleźć wydajności wymagane magazynu maszyny Wirtualnej. | Wydajność magazynu (IOPS/przepływność) wymagane dla komputera przekracza obsługi maszyny Wirtualnej platformy Azure. Zmniejsz wymagania dotyczące magazynu dla maszyny przed migracją.
Nie można odnaleźć wydajności wymagana sieć maszyny Wirtualnej. | Wydajność sieci (We/Wy) wymagane dla komputera przekracza obsługi maszyny Wirtualnej platformy Azure. Zmniejsz wymagania sieciowe dla komputera. 
Maszyna wirtualna nie można odnaleźć określonej warstwie cenowej. | Sprawdź ustawienia warstwy cenowej. 
Maszyna wirtualna nie można odnaleźć w określonej lokalizacji. | Użyj inną lokalizację docelową przed migracją.
Problemy dotyczące obsługi systemu operacyjnego Linux | Upewnij się, że jest uruchomiona 64-bitowym z tych obsługiwane [systemów operacyjnych](../virtual-machines/linux/endorsed-distros.md).
Problemy dotyczące obsługi systemu operacyjnego Windows | Upewnij się, że używasz obsługiwanego systemu operacyjnego. [Dowiedz się więcej](concepts-assessment-calculation.md#azure-suitability-analysis)
Nieznany system operacyjny. | Sprawdź, czy system operacyjny określony w programie vCenter jest poprawny, a następnie powtórzyć ten proces odnajdowania.
Wymagana jest subskrypcja programu Visual Studio. | Klienckie systemy operacyjne Windows są obsługiwane tylko dla subskrypcji Visual Studio (MSDN).


## <a name="collect-logs"></a>Zbieranie dzienników

**Jak zebrać dzienniki w module zbierającym maszyny Wirtualnej?**

Rejestrowanie jest domyślnie włączone. Dzienniki znajdują się w następujący sposób:

- C:\Profiler\ProfilerEngineDB.sqlite
- C:\Profiler\Service.log
- C:\Profiler\WebApp.log

Aby zbierać zdarzenia śledzenia systemu Windows, wykonaj następujące czynności:

1. W module zbierającym maszyny Wirtualnej Otwórz okno poleceń programu PowerShell.
2. Uruchom **Get EventLog - Nazwa_dziennika aplikacji | export-csv eventlog.csv**.

**Jak zebrać dzienniki ruchu sieciowego portalu?**

1. Otwórz przeglądarkę i przejdź i zaloguj się za [do portalu](https://portal.azure.com).
2. Naciśnij klawisz F12, aby uruchomić narzędzia deweloperskie. Jeśli to konieczne, wyczyść ustawienie **Wyczyść wpisy w nawigacji**.
3. Kliknij przycisk **sieci** , a następnie uruchomić Przechwytywanie ruchu sieciowego:
 - W przeglądarce Chrome, wybierz **dziennika Preserve**. Rejestrowanie powinna być uruchamiana automatycznie. Czerwone koło wskazuje, że ruch jest przechwytywania. Jeśli nie zostanie wyświetlone, kliknij przycisk czarny okręgu do uruchomienia
 - W programie Edge/IE rejestrowania powinna być uruchamiana automatycznie. W przeciwnym razie kliknij przycisk Odtwórz zielony.
4. Spróbuj odtwarzania błędu.
5. Po zostały napotkał błąd podczas rejestrowania, Zatrzymaj rejestrowanie i zapisać kopię zarejestrowanej czynności:
 - W przeglądarce Chrome, kliknij prawym przyciskiem myszy, a następnie kliknij przycisk **Zapisz jako HAR z zawartością**. To zips i eksportuje dzienniki w postaci pliku .har.
 - W programie Edge/IE, kliknij przycisk **eksportu przechwycone ruchu** ikony. To zips i eksportuje dziennika.
6. Przejdź do **konsoli** kartę, aby sprawdzić wszelkie ostrzeżenia lub błędy. Aby zapisać dziennik konsoli:
 - W przeglądarce Chrome kliknij prawym przyciskiem myszy w dzienniku konsoli. Wybierz **Zapisz jako**, aby wyeksportować i zip dziennika.
 - W programie Edge/IE, kliknij prawym przyciskiem myszy na błędy i wybierz **skopiuj wszystkie**. 
7. Zamknij narzędzia dla deweloperów.
 



