---
title: "Rozwiązywanie problemów z synchronizacji plików Azure (wersja zapoznawcza) | Dokumentacja firmy Microsoft"
description: "Rozwiązywanie typowych problemów z synchronizacją plików Azure"
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
ms.openlocfilehash: 6bc5f2da2b8628671037b9257db746e73cd3afad
ms.sourcegitcommit: 76a3cbac40337ce88f41f9c21a388e21bbd9c13f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/25/2017
---
# <a name="troubleshoot-azure-file-sync-preview"></a>Rozwiązywanie problemów z synchronizacji plików Azure (wersja zapoznawcza)
Usługa Azure File Sync (wersja zapoznawcza) umożliwia scentralizowanie udziałów plików Twojej organizacji w usłudze Azure Files bez rezygnacji z elastyczności, wydajności i zgodności lokalnego serwera plików. Jest to realizowane poprzez przekształcanie systemów Windows Server w szybką pamięć podręczną udziału plików platformy Azure. Możesz użyć dowolnego dostępnego protokołu w systemie Windows Server w celu uzyskania lokalnego dostępu do danych (w tym protokołu SMB, systemu plików NFS i protokołu FTPS) i możesz mieć dowolną potrzebną Ci liczbę pamięci podręcznych na całym świecie.

W tym artykule zaprojektowano w celu ułatwienia rozwiązywania oraz usuwania problemów z wdrożeniem synchronizacji plików Azure. Kończą się niepowodzeniem w tym przewodniku przedstawiono sposób zbierania dzienników ważne z systemu w celu pomocy w głębsza analiza problemów. Jeśli nie widzisz odpowiedzi na pytanie, w tym miejscu nie niechętnie dotrzeć do nas za pośrednictwem następujących kanałów (w coraz szybciej kolejności):

1. W sekcji uwag w tym artykule.
2. [Forum usługi Azure Storage](https://social.msdn.microsoft.com/Forums/home?forum=windowsazuredata)
3. [Usługa pliki Azure UserVoice](https://feedback.azure.com/forums/217298-storage/category/180670-files) 
4. Microsoft Support: Aby utworzyć nową sprawę pomocy technicznej, przejdź do "Pomoc i obsługa techniczna" karcie w portalu Azure, a następnie kliknij przycisk "Nowy obsługują żądania".

## <a name="agent-installation-and-server-registration"></a>Agent instalacji i serwera rejestracji
<a id="agent-installation-failures"></a>**Jak rozwiązywać problemy z niepowodzeniem instalacji agenta**  
Jeśli podczas instalacji agenta synchronizacji plików Azure kończy się niepowodzeniem, uruchom następujące polecenie z wiersza polecenia z podwyższonym poziomem uprawnień, aby włączyć rejestrowanie podczas instalacji agenta:

```
StorageSyncAgent.msi /l*v Installer.log
```

Po instalacji nie powiedzie się, należy przejrzeć installer.log, aby ustalić przyczynę. 

> [!Note]  
> Instalacja agenta zakończy się niepowodzeniem, jeśli została wybrana do użycia usługi Microsoft Update i usługa Windows Update nie jest uruchomiona.

<a id="server-registration-missing"></a>**Serwer nie jest wyszczególniony w zarejestrowany serwerów w portalu Azure**  
Jeśli serwer nie jest wymieniony w obszarze zarejestrowane serwery usługi synchronizacji magazynu, wykonaj następujące czynności:
1. Zaloguj się do serwera, który chcesz zarejestrować.
2. Otwórz Eksplorator plików i przejdź do katalogu instalacyjnego agenta synchronizacji magazynu (domyślna lokalizacja to `C:\Program Files\Azure\StorageSyncAgent`). 
3. Uruchom ServerRegistration.exe i użyj kreatora, aby zarejestrować serwer z magazynu usługi synchronizacji.

<a id="server-already-registered"></a>**Rejestracja serwera wyświetli następujący komunikat po zainstalowaniu agenta synchronizacji plików Azure: "ten serwer jest już zarejestrowany"**  
![Zrzut ekranu okna dialogowego rejestrowania serwera z powodu błędu "serwer jest już zarejestrowany" wiadomości](media/storage-sync-files-troubleshoot/server-registration-1.png)

Ten komunikat jest wyświetlany, jeśli serwer został poprzednio zarejestrowany z usługą synchronizacji magazynu. Aby wyrejestrować serwera z bieżącej usługi synchronizacji magazynu i rejestrowanie przy użyciu nowej usługi synchronizacji magazynu, należy wykonać kroki, aby [Wyrejestruj serwer z funkcją synchronizacji plików Azure](storage-sync-files-server-registration.md#unregister-the-server-with-storage-sync-service).

Jeśli serwer nie jest wymieniony w obszarze zarejestrowane serwery usługi synchronizacji magazynu, uruchom następujące polecenia programu PowerShell na serwerze, który chcesz wyrejestrować:

```PowerShell
Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll"
Reset-StorageSyncServer
```

> [!Note]  
> Jeśli serwer jest częścią klastra, jest opcjonalny `Reset-StorageSyncServer -CleanClusterRegistration` parametr, który spowoduje również usunięcie rejestracji klastra.

<a id="web-site-not-trusted"></a>**Podczas rejestrowania serwera pojawia się wiele odpowiedzi "nie jest zaufane witryny sieci web", dlaczego?**  
Ten błąd występuje, ponieważ **zwiększonych zabezpieczeń programu Internet Explorer** jest włączona podczas rejestrowania serwera. Uzyskać więcej informacji na temat wyłączania prawidłowo **zwiększonych zabezpieczeń programu Internet Explorer** zasad, zobacz [przygotowanie systemów Windows Server do użytku z synchronizacji plików Azure](storage-sync-files-deployment-guide.md#prepare-windows-servers-for-use-with-azure-file-sync) i [wdrażanie plików Azure Synchronizacja (wersja zapoznawcza)](storage-sync-files-deployment-guide.md).

## <a name="sync-group-management"></a>Synchronizacji grupy zarządzania
<a id="cloud-endpoint-using-share"></a>**Utworzenie punktu końcowego w chmurze zakończy się niepowodzeniem z powodu następującego błędu: "Określonym Azure udziału plików jest już używany przez inny CloudEndpoint"**  
Ten błąd występuje, gdy udział plików Azure jest już używany przez inny punkt końcowy w chmurze. 

Jeśli otrzymujesz ten błąd i udziału plików platformy Azure nie jest obecnie używany przez punkt końcowy w chmurze, wykonaj następujące czynności, aby wyczyścić metadane synchronizacji plików Azure w udziale plików Azure:

> [!Warning]  
> Usunięcie metadanych w udziale plików platformy Azure, który jest obecnie używany przez punkt końcowy w chmurze spowoduje niepowodzenie operacji synchronizacji plików Azure. 

1. Przejdź do udziału plików Azure w portalu Azure.  
2. Kliknij prawym przyciskiem myszy udział plików Azure i wybierz **edycji metadanych**
3. SyncService kliknij prawym przyciskiem myszy i wybierz **usunąć**.

<a id="cloud-endpoint-authfailed"></a>**Utworzenie punktu końcowego w chmurze zakończy się niepowodzeniem z powodu błędu: AuthorizationFailed**  
Ten problem występuje, gdy konto użytkownika nie ma wystarczających praw tworzenia punktu końcowego w chmurze. 

Aby utworzyć punktu końcowego w chmurze, Twoje konto użytkownika musi mieć następujące uprawnienia Authorization firmy Microsoft:  
* Przeczytaj: Pobrać definicji roli
* Zapis: Utwórz lub zaktualizuj niestandardową definicję roli
* Przeczytaj: Pobrać przypisania roli
* Zapis: Utworzyć przypisanie roli

Następujące role wbudowane mają odpowiednie uprawnienia Authorization firmy Microsoft:  
* Właściciel
* Administrator dostępu użytkowników

Aby ustalić, czy rola konto użytkownika ma odpowiednie uprawnienia, wykonaj następujące czynności:  
* Kliknij przycisk **grup zasobów** w portalu Azure
* Wybierz grupę zasobów, w którym znajduje się na koncie magazynu, a następnie kliknij przycisk **(IAM) kontroli dostępu**
* Kliknij przycisk **roli** (np. właściciela, współautora) dla tego konta użytkownika.
* W **dostawcy zasobów** listy, wybierz **Authorization firmy Microsoft** 
* **Przypisanie roli** powinien mieć **odczytu** i **uprawnienia zapisu**
* **Definicja roli** powinien mieć **odczytu** i **uprawnienia zapisu**

<a id="cloud-endpoint-deleteinternalerror"></a>**Usuwanie punktu końcowego nie powiedzie się z powodu błędu: MgmtInternalError**  
Ten problem może wystąpić, jeśli konto udziału lub magazyn plików Azure zostało usunięte przed usunięciem punktu końcowego w chmurze. Ten problem zostanie rozwiązany w przyszłej aktualizacji i można go usunąć punktu końcowego w chmurze.

Aby zapobiec wystąpieniu tego problemu, należy usunąć przed usunięciem konta udziału lub magazyn plików Azure punktu końcowego w chmurze.

## <a name="sync"></a>Sync
<a id="afs-change-detection"></a>**Utworzono plik bezpośrednio w mojej udziału plików platformy Azure przy użyciu protokołu SMB lub za pośrednictwem portalu. Jak długo, aż do pliku jest synchronizowany z serwerami w grupie synchronizacji?**  
[!INCLUDE [storage-sync-files-change-detection](../../../includes/storage-sync-files-change-detection.md)]

<a id="broken-sync"></a>**Jak rozwiązywać problemy z synchronizacji nie działa na serwerze**  
Jeśli synchronizacji kończy się niepowodzeniem na serwerze, wykonaj następujące czynności:
- Sprawdź, czy punkt końcowy serwera istnieje w portalu Azure w katalogu, który chcesz synchronizować z udziałem plików Azure:
    
    ![Zrzut ekranu grupy synchronizacji z chmury i punkt końcowy serwera w portalu Azure](media/storage-sync-files-troubleshoot/sync-troubleshoot-1.png)

- Przejrzyj dzienniki zdarzeń działa i Diagnostyka za pomocą Podglądu zdarzeń, znajduje się w obszarze `Applications and Services\Microsoft\FileSync\Agent`.
- Upewnij się, że serwer ma połączenie z Internetem.
- Sprawdź synchronizacji plików Azure usługa jest uruchomiona na serwerze otwarcie przystawki MMC usług i sprawdź, czy Usługa agenta synchronizacji magazynu (FileSyncSvc) jest uruchomiona.

<a id="replica-not-ready"></a>**Synchronizacja nie powiedzie się z powodu błędu: 0x80c8300f — replika nie jest gotowa do wykonania żądanej operacji**  
Ten błąd jest oczekiwany po utworzeniu punktu końcowego w chmurze i używać udziału plików platformy Azure, która zawiera dane. Po zakończeniu wykrywania zmian w udziale plików Azure (może potrwać do 24 godzin), należy zacząć synchronizacji działa prawidłowo.

<a id="broken-sync-files"></a>**Jak rozwiązywać problemy z poszczególnych plików nie powiodło się synchronizacji**  
Jeśli pojedynczych plików kończą się niepowodzeniem do synchronizacji, wykonaj następujące czynności:
- Przejrzyj dzienniki zdarzeń działa i diagnostyki w obszarze `Applications and Services\Microsoft\FileSync\Agent` w Podglądzie zdarzeń
- Sprawdź, że nie ma żadnych otwartych dojść do pliku
    - Uwaga: Synchronizacja programu Azure pliku okresowo zacznie migawki VSS, aby zsynchronizować pliki z otwartymi dojściami

## <a name="cloud-tiering"></a>Obsługa warstw w chmurze 
<a id="files-fail-tiering"></a>**Jak rozwiązywać problemy z plików, które nie są do warstwy**  
Jeśli pliki nie warstwy do usługi pliki Azure, wykonaj następujące czynności:

- Sprawdź, czy istnieją pliki w udziale plików na platformę Azure
    - Uwaga: Plik musi synchronizowane z udziału plików na platformę Azure przed jego mogą należeć do warstwy
- Przejrzyj dzienniki zdarzeń działa i Diagnostyka, znajduje się w obszarze `Applications and Services\Microsoft\FileSync\Agent` w Podglądzie zdarzeń
- Upewnij się, że serwer ma połączenie z Internetem 
- Sprawdź, czy sterowniki filtrów synchronizacji plików Azure (StorageSync.sys & StorageSyncGuard.sys) są uruchomione.
    - Otwórz wiersz polecenia z podwyższonym poziomem uprawnień, uruchom `fltmc` i sprawdź StorageSync.sys i StorageSyncGuard.sys sterowniki filtrów systemu plików są wyświetlane.

<a id="files-fail-recall"></a>**Jak rozwiązywać problemy z plików, które nie można wycofać**  
Jeśli pliki nie można wycofać, wykonaj następujące czynności:
- Przejrzyj dzienniki zdarzeń działa i Diagnostyka, znajduje się w obszarze `Applications and Services\Microsoft\FileSync\Agent` w Podglądzie zdarzeń
- Sprawdź, czy istnieje plików w udziale plików Azure
- Upewnij się, że serwer ma połączenie z Internetem 
- Sprawdź, czy sterowniki filtrów synchronizacji plików Azure (StorageSync.sys & StorageSyncGuard.sys) są uruchomione.
    - Otwórz wiersz polecenia z podwyższonym poziomem uprawnień, uruchom `fltmc` i sprawdź StorageSync.sys i StorageSyncGuard.sys sterowniki filtrów systemu plików są wyświetlane.

<a id="files-unexpectedly-recalled"></a>**Jak rozwiązywać problemy z nieoczekiwanie odwoływanego na serwerze plików**  
Oprogramowanie antywirusowe, kopia zapasowa i inne aplikacje, które zapoznały dużej liczby plików spowoduje, że niepożądanych odwołań, chyba że przestrzegać atrybutu w trybie offline i Pomiń odczytywania zawartości tych plików. Pomijanie pliki trybu offline dla tych produktów, które obsługują tę pomaga uniknąć niepożądanych odwołań podczas wykonywania operacji, takich jak skanowania w poszukiwaniu wirusów lub zadania tworzenia kopii zapasowej.

Zapoznaj się z dostawcą oprogramowania dotyczące sposobu konfigurowania ich rozwiązanie, aby pominąć odczytywania plików trybu offline.

Dodatkowo niepożądane odwołania może się tak zdarzyć w innych sytuacjach, takich jak przeglądanie plików w Eksploratorze plików. Otwieranie folderu z plikami warstwy chmury w Eksploratorze plików na serwerze może spowodować niepożądane odwołania, bardziej Jeśli oprogramowanie antywirusowe jest włączona na serwerze.

## <a name="general-troubleshooting"></a>Rozwiązywanie ogólnych
Jeśli wystąpią problemy z synchronizacją plików Azure na serwerze, należy uruchomić, wykonując następujące czynności:
- Przejrzyj dzienniki zdarzeń diagnostycznych i działa w Podglądzie zdarzeń
    - Synchronizacja, Obsługa poziomów w chmurze i problemy odwołania są rejestrowane w dziennikach zdarzeń diagnostycznych i działa w obszarze`Applications and Services\Microsoft\FileSync\Agent`
    - Problemy dotyczące zarządzania serwerem (na przykład ustawienia konfiguracji) są rejestrowane w dziennikach zdarzeń diagnostycznych i działa w obszarze`Applications and Services\Microsoft\FileSync\Management`
- Sprawdź, czy usługa synchronizacji plików Azure działa na serwerze
    - Otwórz przystawkę MMC usługi i sprawdź, czy jest uruchomiona usługa agenta synchronizacji magazynu (FileSyncSvc)
- Sprawdź, czy sterowniki filtrów synchronizacji plików Azure (StorageSync.sys & StorageSyncGuard.sys) są uruchomione.
    - Otwórz wiersz polecenia z podwyższonym poziomem uprawnień, uruchom fltmc i sprawdź, czy są wyświetlane StorageSync.sys i StorageSyncGuard.sys sterowniki filtrów systemu plików

Jeśli problem nie zostanie rozwiązany po wykonaniu powyższych kroków, uruchom narzędzie AFSDiag, wykonując następujące czynności:
1. Utwórz katalog, który będzie używany do zapisywania danych wyjściowych AFSDiag (na przykład c:\output).
2. Otwórz okno programu PowerShell z podwyższonym poziomem uprawnień i uruchom następujące polecenia (naciśnij klawisz enter po każdym poleceniu):

    ```PowerShell
    cd "c:\Program Files\Azure\StorageSyncAgent"
    Import-Module .\afsdiag.ps1
    Debug-Afs c:\output # Note: use the path created in step 1
    ```

3. Poziom śledzenia trybu jądra synchronizacji plików Azure wprowadź wartość 1 (chyba że określony w celu utworzenia ślady na pełniejsze) i naciśnij klawisz enter.
4. Dla poziomu śledzenia trybu użytkownika synchronizacji plików Azure wprowadź wartość 1 (chyba że określony w celu utworzenia ślady na pełniejsze) i naciśnij klawisz enter.
5. Odtwórz problem, a następnie naciśnij klawisz D po zakończeniu.
6. Plik zip zawierający dzienników i pliki śledzenia będzie należeć do podanego katalogu wyjściowego.

## <a name="see-also"></a>Zobacz też
- [Usługa pliki Azure — często zadawane pytania](storage-files-faq.md)
- [Rozwiązywanie problemów plików Azure w systemie Windows](storage-troubleshoot-windows-file-connection-problems.md)
- [Rozwiązywanie problemów plików Azure w systemie Linux](storage-troubleshoot-linux-file-connection-problems.md)
