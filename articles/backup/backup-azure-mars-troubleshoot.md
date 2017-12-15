---
title: "Rozwiązywanie problemów z agenta kopii zapasowej systemu Azure (agenta MARS) | Dokumentacja firmy Microsoft"
description: "Rozwiązywanie problemów z Instalowanie i rejestrowanie agenta usługi Kopia zapasowa Azure"
services: backup
documentationcenter: 
author: saurabhsensharma
manager: shreeshd
editor: 
ms.assetid: 778c6ccf-3e57-4103-a022-367cc60c411a
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/4/2017
ms.author: saurse;markgal;
ms.openlocfilehash: 42b622e081d266da81b83c2140424f9d47008131
ms.sourcegitcommit: 0e4491b7fdd9ca4408d5f2d41be42a09164db775
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/14/2017
---
# <a name="troubleshooting-azure-backup-agent-configurationregistration-issues"></a>Rozwiązywanie problemów konfiguracji/Rejestracja w usłudze Azure Backup Agent
## <a name="recommended-steps"></a>Zalecane czynności
Zapoznaj się zalecane akcje wymienionymi poniżej, aby wyeliminować odpowiedniego błędy występujące podczas konfiguracji lub rejestracji agenta usługi Kopia zapasowa Azure.

## <a name="invalid-vault-credentials-provided-the-file-is-either-corrupted-or-does-not-have-the-latest-credentials-associated-with-recovery-service"></a>Udostępniono nieprawidłowe poświadczenia magazynu. Plik jest uszkodzony lub jest nie ma najnowszych poświadczeń skojarzonych z usługą odzyskiwania.
| Szczegóły błędu | Możliwe przyczyny | Zalecane akcje |
| ---     | ---     | ---    |
| **Błąd** </br> *Udostępniono nieprawidłowe poświadczenia magazynu. Plik jest uszkodzony lub jest nie ma najnowszych poświadczeń skojarzonych z usługą odzyskiwania. (IDENTYFIKATOR: 34513)* | <ol><li> Poświadczenia magazynu są nieprawidłowe (tzn. one zostały pobrane ponad 48 godzin przed upływem terminu rejestracji).<li>   Agent usługi Kopia zapasowa Azure nie jest w stanie pobrać plik tymczasowy w folderze Temp systemu Windows. <li>Poświadczenia magazynu znajdują się w lokalizacji sieciowej. <li>Protokołu TLS 1.0 jest wyłączona.<li> Skonfigurowany serwer proxy blokuje połączenia <br> |  <ol><li>Pobranie nowych poświadczeń magazynu<li>Przejdź do opcji internetowych > Zabezpieczenia > Internet > kliknij przycisk Poziom niestandardowy > przewiń do zobacz sekcja pobierania pliku i zaznacz opcję Włącz.<li>Może także wymagać Dodaj witrynę do [Zaufane witryny](https://docs.microsoft.com/azure/backup/backup-try-azure-backup-in-10-mins#network-and-connectivity-requirements).<li>Zmień ustawienia, aby użyć serwera proxy i podaj szczegóły dotyczące serwera proxy <li> Zgodny z komputera data i godzina (UTC)<li>Przejdź do C:/Windows/Temp i sprawdź, czy istnieją pliki ponad 60 000 lub 65 000 (z rozszerzeniem TMP) i usuń te pliki.<li>Można to sprawdzić, uruchamiając SDP pakietu na serwerze. Jeśli zostanie wyświetlony błąd, który pobiera określania pliku nie są dozwolone, można zapewnić, że rozsądnych o dużej liczby plików w katalogu Temp-C:/Windows.<li>Upewnij się, że zainstalowano .NET framework 4.6.2 <li>Jeśli protokół TLS 1.0 zostało wyłączone z powodu zgodności PCI, odwołują się do tego [Rozwiązywanie problemów z łącza](https://support.microsoft.com/help/4022913). <li>Jeśli masz oprogramowanie antywirusowe zainstalowane na serwerze, należy wyłączyć następujące pliki ze skanowania antywirusowego. <ol><li>CBengine.exe<li>CSC.exe (związane z .NET Framework. Brak CSC.exe dla poszczególnych wersji .NET zainstalowany na serwerze. Wyklucz wszystkie pliki CSC.exe powiązane do wszystkich wersji platformy .NET framework na tym serwerze). <li>Pliki tymczasowe lokalizacji pamięci podręcznej lub folderu. <br>*Domyślna lokalizacja dla pliki tymczasowe folder lub ścieżka lokalizacji pamięci podręcznej to C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch*

## <a name="the-microsoft-azure-recovery-service-agent-was-unable-to-connect-to-microsoft-azure-backup"></a>Agent usług odzyskiwania Microsoft Azure nie może nawiązać połączenia z usługą kopia zapasowa Microsoft Azure

| Szczegóły błędu | Możliwe przyczyny | Zalecane akcje |
| ---     | ---     | ---    |
| **Błąd** </br><ol><li>*Agent usług odzyskiwania Microsoft Azure nie może nawiązać połączenia z usługą kopia zapasowa Microsoft Azure. (IDENTYFIKATOR: 100050) Sprawdź ustawienia sieciowe i upewnij się, że jesteś w stanie nawiązać połączenia z Internetem*<li>*Wymagane jest uwierzytelnianie serwera proxy (407)* |Serwer proxy blokuje połączenia * |  <ol><li>Przejdź do opcji internetowych > Zabezpieczenia > Internet > kliknij przycisk Poziom niestandardowy > przewiń do zobacz sekcja pobierania pliku i zaznacz opcję Włącz.<li>Może także wymagać Dodaj witrynę do [Zaufane witryny](https://docs.microsoft.com/azure/backup/backup-try-azure-backup-in-10-mins#network-and-connectivity-requirements).<li>Zmień ustawienia, aby użyć serwera proxy i podaj szczegóły dotyczące serwera proxy <li>Jeśli masz oprogramowanie antywirusowe zainstalowane na serwerze, należy wyłączyć następujące pliki ze skanowania antywirusowego. <ol><li>CBengine.exe<li><li>CSC.exe (związane z .NET Framework. Brak CSC.exe dla poszczególnych wersji .NET zainstalowany na serwerze. Wyklucz wszystkie pliki CSC.exe powiązane do wszystkich wersji platformy .NET framework na tym serwerze). <li>Pliki tymczasowe lokalizacji pamięci podręcznej lub folderu <br>*Domyślna lokalizacja dla pliki tymczasowe folder lub ścieżka lokalizacji pamięci podręcznej to C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch*

## <a name="failed-to-set-the-encryption-key-for-secure-backups"></a>Nie można ustawić klucza szyfrowania bezpiecznego tworzenia kopii zapasowych

| Szczegóły błędu | Możliwe przyczyny | Zalecane akcje |
| ---     | ---     | ---    |      
| **Błąd** </br>*Nie można ustawić klucza szyfrowania bezpiecznego tworzenia kopii zapasowych. Bieżąca operacja nie powiodła się z powodu wewnętrznego błędu "Nieprawidłowa wejściowych błędu usługi". Spróbuj ponownie wykonać operację po pewnym czasie. Jeśli problem będzie się powtarzać, skontaktuj się z pomocą techniczną firmy Microsoft* |Serwer jest już zarejestrowany w innym magazynie.| Wyrejestrowanie z magazynu i Zarejestruj ponownie serwer.

## <a name="the-activation-did-not-complete-successfully-the-current-operation-failed-due-to-an-internal-service-error-0x1fc07"></a>Aktywacja nie została pomyślnie ukończona. Bieżąca operacja nie powiodła się z powodu wewnętrznego błędu usługi [0x1FC07]

| Szczegóły błędu | Możliwe przyczyny | Zalecane akcje |
| ---     | ---     | ---    |          
| **Błąd** </br><ol><li>*Aktywacja nie została pomyślnie ukończona. Bieżąca operacja nie powiodła się z powodu wewnętrznego błędu usługi [0x1FC07]. Spróbuj ponownie wykonać operację po pewnym czasie. Jeśli problem będzie się powtarzać, skontaktuj się z pomocą techniczną firmy Microsoft* <li>*Błąd 34506. Hasło szyfrowania zapisane na tym komputerze nie jest poprawnie skonfigurowany.* | <li> Pliki tymczasowe folder znajduje się na woluminie, który ma za mało miejsca <li> Pliki tymczasowe folderu jest niepoprawnie przeniesiony do innej lokalizacji <li> Brak pliku OnlineBackup.KEK | <li>Przenieś pliki tymczasowe folderu lub lokalizacji pamięci podręcznej na wolumin o ilości wolnego miejsca odpowiednikiem 5 – 10% całkowitego rozmiaru danych kopii zapasowej. Zapoznaj się czynności wymienionych w [w tym artykule](https://docs.microsoft.com/azure/backup/backup-azure-file-folder-backup-faq#backup) można poprawnie przenieść lokalizację pamięci podręcznej.<li> Upewnij się, że plik OnlineBackup.KEK jest obecny <br>*Domyślna lokalizacja dla pliki tymczasowe folder lub ścieżka lokalizacji pamięci podręcznej to C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch*

## <a name="need-help-contact-support"></a>Potrzebujesz pomocy? Kontakt z pomocą techniczną
Jeśli nadal potrzebujesz pomocy, [się z pomocą techniczną](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) uzyskać szybkie rozwiązanie problemu.

## <a name="next-steps"></a>Następne kroki
* Dowiedz się więcej [tworzenie kopii zapasowej serwera z systemem Windows przy użyciu agenta usługi Kopia zapasowa Azure](tutorial-backup-windows-server-to-azure.md).
* Jeśli chcesz przywrócić kopię zapasową, w tym artykule znajdziesz informacje dotyczące [przywracania plików na maszynę z systemem Windows](backup-azure-restore-windows-server.md).
