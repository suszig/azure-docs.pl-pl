---
title: "Kopia zapasowa Azure: spójnych z aplikacją kopii zapasowych maszyn wirtualnych systemu Linux | Dokumentacja firmy Microsoft"
description: "Aby zagwarantować kopii zapasowych spójnych z aplikacją na platformie Azure, dla maszyn wirtualnych systemu Linux za pomocą skryptów. Skrypty mają zastosowanie tylko do maszyn wirtualnych systemu Linux w ramach wdrożenia usługi Resource Manager; skrypty nie dotyczą maszyn wirtualnych systemu Windows lub wdrożenia Menedżera usług. Ten artykuł przedstawia kroki konfigurowania skryptów, w tym Rozwiązywanie problemów."
services: backup
documentationcenter: dev-center-name
author: anuragmehrotra
manager: shivamg
keywords: "kopii zapasowej całej aplikacji. spójnych z aplikacją kopii zapasowej maszyny Wirtualnej platformy Azure; Kopii zapasowej maszyny Wirtualnej systemu Linux. Kopia zapasowa Azure"
ms.assetid: bbb99cf2-d8c7-4b3d-8b29-eadc0fed3bef
ms.service: backup
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 4/12/2017
ms.author: anuragm;markgal
ms.openlocfilehash: 378c65bec8fd1f880ed459e76f5e4b5d85e49d2a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="application-consistent-backup-of-azure-linux-vms-preview"></a>Spójnych z aplikacją kopii zapasowej maszyn wirtualnych systemu Linux platformy Azure (wersja zapoznawcza)

Tym artykule omówiono systemu Linux skryptów przed i po wykonaniu script framework i jak może służyć do podjęcia spójnych z aplikacją kopii zapasowych maszyn wirtualnych systemu Linux platformy Azure.

> [!Note]
> Framework skryptów przed i po skryptu jest obsługiwana tylko dla maszyn wirtualnych wdrożonych przez Menedżera zasobów Azure systemu Linux. Skrypty w celu zachowania spójności aplikacji nie są obsługiwane dla maszyn wirtualnych wdrożonych programu Service Manager lub maszyn wirtualnych systemu Windows.
>

## <a name="how-the-framework-works"></a>Jak działa w ramach

Platformę udostępnia opcję umożliwiają uruchamianie niestandardowych skryptów przed i po skrypty podczas tworzenia migawki maszyny Wirtualnej. Wstępne skrypty są uruchamiane tuż przed migawki maszyny Wirtualnej, a po skrypty są uruchamiane natychmiast po wykonaniu migawki maszyny Wirtualnej. Zapewnia to elastyczność kontroli aplikacji i środowiska podczas tworzenia migawki maszyny Wirtualnej.

W tym scenariuszu jest istotne dla zapewnienia spójnych z aplikacją kopii zapasowej maszyny Wirtualnej. Wstępne skryptu można wywoływać interfejsy API natywnych aplikacji do systemu IOs w stan spoczynku i opróżnić zawartość w pamięci na dysk. Dzięki temu migawki są spójne z aplikacjami (to znaczy dostarczonego przez aplikację się po uruchomieniu maszyny Wirtualnej wykonywane po przywróceniu). Skrypt po może służyć do odblokowania systemu IOs. Robi to przy użyciu aplikacji natywnych interfejsów API, tak aby aplikację można wznowić wykonywania normalnych operacji post-VM migawki.

## <a name="steps-to-configure-pre-script-and-post-script"></a>Kroki konfigurowania skryptów przed i po skryptu

1. Zaloguj się jako użytkownik główny do maszyny Wirtualnej systemu Linux, który chcesz utworzyć kopię zapasową.

2. Pobierz **VMSnapshotScriptPluginConfig.json** z [GitHub](https://github.com/MicrosoftAzureBackup/VMSnapshotPluginConfig), a następnie skopiować go do **/etc/azure** folderu na wszystkich maszynach wirtualnych, które użytkownik chce utworzyć kopię zapasową. Utwórz **/etc/azure** katalogu, jeśli nie istnieje już.

3. Skopiuj skrypt przed i po skryptu aplikacji na wszystkich maszynach wirtualnych, które zamierzasz wykonać kopię zapasową. Skrypty można skopiować do dowolnej lokalizacji na maszynie Wirtualnej. Należy upewnić się, że pełna ścieżka plików skryptów w **VMSnapshotScriptPluginConfig.json** pliku.

4. Upewnij się, następujących uprawnień do tych plików:

   - **VMSnapshotScriptPluginConfig.json**: uprawnienie "600." Na przykład tylko użytkownika "root" powinien mieć uprawnienia "Odczyt" i "write" do tego pliku, a żaden użytkownik nie ma uprawnienia "execute".

   - **Plik skryptu przed**: uprawnienie "700."  Na przykład tylko "root" użytkownik powinien mieć "do odczytu", "write" i "execute" uprawnień do tego pliku.
  
   - **Skrypt po** uprawnienia "700." Na przykład tylko "root" użytkownik powinien mieć "do odczytu", "write" i "execute" uprawnień do tego pliku.

   > [!Important]
   > Platformę umożliwia użytkownikom dużo energii. Jest to bezpieczne i że tylko użytkownika "root" ma dostęp do krytycznych plików JSON i skrypt.
   > Jeśli nie są spełnione wymagania poprzedniej, skrypt nie działa. Powoduje to kopia zapasowa spójna awaria/systemu plików.
   >

5. Skonfiguruj **VMSnapshotScriptPluginConfig.json** zgodnie z opisem w tym miejscu:
    - **pluginName**: pozostaw to pole jest lub skryptów mogą nie działać zgodnie z oczekiwaniami.

    - **preScriptLocation**: Podaj pełną ścieżkę skryptu przed na maszynie Wirtualnej, która ma wejść do wykonania kopii zapasowej.

    - **postScriptLocation**: Podaj pełną ścieżkę skryptu po na maszynie Wirtualnej, która ma wejść do wykonania kopii zapasowej.

    - **preScriptParams**: Podaj parametry opcjonalne, które muszą zostać przekazane do skryptu wstępne. Wszystkie parametry powinna być w cudzysłowie, a powinien być oddzielonych przecinkami, jeśli istnieje wiele parametrów.

    - **postScriptParams**: Podaj parametry opcjonalne, które muszą być przekazywane do skryptu po. Wszystkie parametry powinna być w cudzysłowie, a powinien być oddzielonych przecinkami, jeśli istnieje wiele parametrów.

    - **preScriptNoOfRetries**: Ustaw liczbę godzin przed skryptu należy wykonać ponownie razie błędu przed zakończeniem. Zero oznacza, że tylko jedna próba i nie ponownych prób w przypadku awarii.

    - **postScriptNoOfRetries**: Ustaw liczbę razy skryptu po powinno być ponowione w razie błędu przed zakończeniem. Zero oznacza, że tylko jedna próba i nie ponownych prób w przypadku awarii.
    
    - **LimitCzasuWSekundach**: Określ poszczególnych limitów czasu dla skryptów przed i po skryptu.

    - **continueBackupOnFailure**: Ustaw tę wartość na **true** Jeśli chcesz, kopia zapasowa Azure może wrócić do kopia zapasowa spójna spójne/awarii systemu plików, jeśli skrypt przed lub po wykonaniu skryptu kończy się niepowodzeniem. To ustawienie **false** niepowodzenia tworzenia kopii zapasowej w przypadku błędu skryptu (z wyjątkiem przypadków, gdy masz jednego dysku maszyny Wirtualnej, która przechodzi do usługi Kopia zapasowa spójna w razie awarii niezależnie od tego ustawienia).

    - **fsFreezeEnabled**: Określ, czy Linux fsfreeze powinna być wywoływana podczas tworzenia migawki maszyny Wirtualnej w celu zapewnienia spójności systemu plików. Zaleca się pozostawienie to ustawienie, ustaw **true** chyba, że aplikacja ma zależności wyłączenie fsfreeze.

6. W ramach skryptu jest teraz skonfigurowany. Jeśli kopia zapasowa maszyny Wirtualnej jest już skonfigurowana, następnej kopii zapasowej wywołuje skrypty i wyzwala spójnych z aplikacją kopii zapasowej. Jeśli nie skonfigurowano kopii zapasowej maszyny Wirtualnej, skonfiguruj ją za pomocą [kopii zapasowych maszyn wirtualnych platformy Azure do magazynów usług odzyskiwania.](https://docs.microsoft.com/azure/backup/backup-azure-vms-first-look-arm)

## <a name="troubleshooting"></a>Rozwiązywanie problemów

Upewnij się, należy dodać odpowiednią rejestrowania podczas zapisywania w skrypcie przed i po skryptu i dziennikach skryptu Rozwiąż wszelkie problemy skryptu. Jeśli nadal występują problemy z uruchamianiem skryptów, zapoznaj się z poniższą tabelą, aby uzyskać więcej informacji.

| Błąd | Komunikat o błędzie | Zalecane działanie |
| ------------------------ | -------------- | ------------------ |
| Wstępnie ScriptExecutionFailed |Wstępne skryptu zwrócił błąd, aby kopia zapasowa może nie być spójnych z aplikacją.   | Sprawdź w dziennikach błędu skrypt, aby rozwiązać ten problem.|  
|   Post ScriptExecutionFailed |    Skrypt po zwrócił błąd, który może mieć wpływ na stan aplikacji. |    Sprawdź w dziennikach błędu skrypt, aby rozwiązać ten problem i sprawdź stan aplikacji. |
| Wstępnie ScriptNotFound |  Nie znaleziono wstępne skryptu w lokalizacji określonej w **VMSnapshotScriptPluginConfig.json** pliku konfiguracji. |   Upewnij się, że to wstępne skryptu znajduje się w ścieżce określonej w pliku konfiguracji, aby zapewnić spójne z aplikacjami tworzenia kopii zapasowych.|
| Post ScriptNotFound | Po skryptu nie można znaleźć w lokalizacji określonej w **VMSnapshotScriptPluginConfig.json** pliku konfiguracji. |   Upewnij się, że to po skryptu znajduje się w ścieżce określonej w pliku konfiguracji, aby zapewnić spójne z aplikacjami tworzenia kopii zapasowych.|
| IncorrectPluginhostFile | **Pluginhost** pliku, który jest dostarczany z rozszerzeniem VmSnapshotLinux, jest uszkodzony, więc nie można uruchomić skryptu przed i po skrypt i tworzenia kopii zapasowej nie będzie spójnych z aplikacją. | Odinstaluj **VmSnapshotLinux** rozszerzenia, a zostaną automatycznie zainstalowane ponownie z następnej kopii zapasowej, aby rozwiązać ten problem. |
| IncorrectJSONConfigFile | **VMSnapshotScriptPluginConfig.json** plik jest nieprawidłowy, dlatego przed skryptu i nie można uruchomić skrypt po i kopii zapasowej nie będzie spójnych z aplikacją. | Pobierz kopię z [GitHub](https://github.com/MicrosoftAzureBackup/VMSnapshotPluginConfig) i skonfiguruj go ponownie. |
| InsufficientPermissionforPre skryptu | Uruchamianie skryptów, użytkownika "root" powinien być właściciel pliku i ten plik powinien mieć uprawnienia "700" (to znaczy powinny mieć tylko "właściciela" "do odczytu", "write" i "uprawnienia do wykonywania"). | Upewnij się, użytkownika "root" jest "właścicielem" pliku skryptu i że tylko "właściciela" ma "uprawnienia do odczytu", "write" i "wykonywanie". |
| InsufficientPermissionforPost skryptu | Uruchamianie skryptów, użytkownika głównego powinny być właściciel pliku i ten plik powinien mieć uprawnienia "700" (to znaczy powinny mieć tylko "właściciela" "do odczytu", "write" i "uprawnienia do wykonywania"). | Upewnij się, użytkownika "root" jest "właścicielem" pliku skryptu i że tylko "właściciela" ma "uprawnienia do odczytu", "write" i "wykonywanie". |
| Wstępnie ScriptTimeout | Spójnych z aplikacją kopii zapasowej skryptu przed upłynął limit czasu wykonywania. | Sprawdź skrypt i zwiększyć limit czasu w **VMSnapshotScriptPluginConfig.json** pliku znajdującego się na **/etc/azure**. |
| Post ScriptTimeout | Upłynął limit czasu wykonywania skryptu po kopii zapasowej zapewniających spójność aplikacji. | Sprawdź skrypt i zwiększyć limit czasu w **VMSnapshotScriptPluginConfig.json** pliku znajdującego się na **/etc/azure**. |

## <a name="next-steps"></a>Następne kroki
[Konfigurowanie kopii zapasowej maszyny Wirtualnej w magazynie usług odzyskiwania](https://docs.microsoft.com/azure/backup/backup-azure-arm-vms)
