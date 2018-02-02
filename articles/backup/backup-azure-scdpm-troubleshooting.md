---
title: "Rozwiązywanie problemów z programu System Center Data Protection Manager w usłudze Azure Backup | Dokumentacja firmy Microsoft"
description: "Rozwiązywanie problemów w System Center Data Protection Manager."
services: backup
documentationcenter: 
author: adigan
manager: shreeshd
editor: 
ms.assetid: 2d73c349-0fc8-4ca8-afd8-8c9029cb8524
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/24/2017
ms.author: pullabhk;markgal;adigan
ms.openlocfilehash: 2244a39217f54eb5906d05990f19fc8ca2fdeb0e
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/01/2018
---
# <a name="troubleshoot-system-center-data-protection-manager"></a>Rozwiązywanie problemów z programu System Center Data Protection Manager

Możesz znaleźć informacje o najnowszej wersji programu SC DPM [tutaj](https://docs.microsoft.com/en-us/system-center/dpm/dpm-release-notes?view=sc-dpm-2016).
Macierz obsługi ochrony można znaleźć [tutaj](https://docs.microsoft.com/en-us/system-center/dpm/dpm-protection-matrix?view=sc-dpm-2016).

## <a name="replica-is-inconsistent"></a>Replika jest niespójna

Ten błąd może się zdarzyć z różnych powodów, takich jak - zadania tworzenia repliki nie powiodła się, problemy z dziennika zmian woluminu filtru poziomu mapy bitowej błędów, komputera źródłowego został nieoczekiwanie wyłączony, przepełnienia dziennika synchronizacji lub replika jest niespójna naprawdę. Wykonaj następujące kroki, aby rozwiązać ten problem:
- Aby usunąć stan niespójna, uruchom sprawdzanie spójności ręcznie lub zaplanować codzienne sprawdzanie spójności.
- Upewnij się, że używasz najnowszej wersji MAB serwera lub programu System Center DPM
- Upewnij się, że jest włączone automatyczne sprawdzanie spójności
- Spróbuj ponownie uruchomić usługi z wiersza polecenia ("net stop dpmra" następuje polecenie "net start dpmra")
- Upewnij się, że spełniono wymagania dotyczące łączności i przepustowości sieci
- Sprawdź, czy maszyna źródłowa została nieoczekiwanie wyłączony
- Upewnij się, dysk jest uszkodzony i ma wystarczającej ilości miejsca dla replik
- Upewnij się, nie zduplikowane zadanie tworzenia kopii zapasowej są uruchomione jednocześnie

## <a name="online-recovery-point-creation-failed"></a>Niepowodzenie tworzenia punktu odzyskiwania w trybie online

Wykonaj następujące kroki, aby rozwiązać ten problem:
- Upewnij się, że używasz najnowszej wersji agenta usługi Kopia zapasowa Azure
- Spróbuj ręcznie utworzyć punkt odzyskiwania w obszarze zadanie ochrony
- Upewnij się, że Uruchom sprawdzanie spójności w źródle danych
- Upewnij się, że spełniono wymagania dotyczące łączności i przepustowości sieci
- Dane repliki są w stanie niespójnym. Utwórz punkt odzyskiwania dysku tego źródła danych
- Upewnij się, że replika jest obecny i nie brakuje
- Replika ma za mało miejsca, aby utworzyć dziennika USN

## <a name="unable-to-configure-protection"></a>Nie można skonfigurować ochrony

Ten błąd jest wyświetlany, gdy serwer programu DPM nie mógł skontaktować się z serwerem chronionym. Wykonaj następujące kroki, aby rozwiązać ten problem:
- Upewnij się, że używasz najnowszej wersji agenta usługi Kopia zapasowa Azure
- Upewnij się, że istnieje łączność (/ zaporą/serwera proxy sieci) między serwerem DPM i serwerze chronionym
- W przypadku ochrony programu SQL Server, upewnij się, że NT AUTHORITY\SYSTEM ma włączyć za pomocą właściwości logowania administratora systemu

## <a name="this-server-is-not-registered-to-the-vault-specified-by-the-vault-credential"></a>Ten serwer nie jest zarejestrowany w magazynie określonym przez poświadczenie magazynu

Ten błąd jest wyświetlany, gdy plik poświadczeń magazynu wybrane, nie należy do magazynu usług odzyskiwania skojarzone z programem System Center DPM / Azure Utwórz kopię zapasową serwera, na którym nastąpiła odzyskiwania. Wykonaj następujące kroki, aby rozwiązać ten problem:
- Pobierz plik poświadczeń magazynu usług odzyskiwania magazynu, do którego programu System Center DPM / serwer kopii zapasowej Azure jest zarejestrowany.
- Spróbuj zarejestrować serwer w magazynie przy użyciu najnowszego pliku poświadczeń magazynu pobranego.

## <a name="either-the-recoverable-data-is-not-available-or-the-selected-server-is-not-a-dpm-server"></a>Dane możliwe do odzyskania są niedostępne albo wybrany serwer nie jest serwerem DPM
Ten błąd jest wyświetlany, gdy nie ma żadnych innych programu System Center DPM / kopia zapasowa Azure serwery zarejestrowane w magazynie usług odzyskiwania lub serwerów nie zostały jeszcze przekazane metadanych lub wybrany serwer nie jest System Center DPM / serwer kopii zapasowej Azure.
- Jeśli istnieją inne programu System Center DPM / serwerów kopia zapasowa Azure zarejestrowany w magazynie usług odzyskiwania, upewnij się, że agent jest zainstalowany najnowszej kopii zapasowej Azure.
- Jeśli istnieją inne programu System Center DPM / serwerów kopia zapasowa Azure zarejestrowany w magazynie usług odzyskiwania, poczekaj na dzień po zakończeniu instalacji, aby rozpocząć proces odzyskiwania. Nocne zadanie przekazuje metadanych dla wszystkich chronionych kopii zapasowych w chmurze. Dane są dostępne do odzyskania.

## <a name="the-encryption-passphrase-provided-does-not-match-with-passphrase-associated-with-the-following-server"></a>Podane hasło szyfrowania nie jest zgodne z hasłem skojarzonym z następującym serwerem

> [!NOTE]
>Jeśli nie pamiętasz/utracie hasło szyfrowania, to nie ma możliwość odzyskania danych. Jedyną opcją jest ponownie wygenerować hasła i służy do szyfrowania danych kopii zapasowych w przyszłości.
>
>

Ten błąd jest wyświetlany, gdy hasło szyfrowania używany w procesie szyfrowania danych z programu System Center DPM / są odzyskiwane dane serwera kopii zapasowej systemu Azure jest niezgodna z podane hasło szyfrowania. Agent jest do odszyfrowania danych. Dlatego odzyskiwanie zakończy się niepowodzeniem. Wykonaj następujące kroki, aby rozwiązać ten problem:
- Podaj dokładny tego samego hasła szyfrowania skojarzone z programem System Center DPM / serwer kopii zapasowej Azure, którego dane są odzyskiwane. 
