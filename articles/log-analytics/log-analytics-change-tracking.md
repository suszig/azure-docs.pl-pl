---
title: "Śledzenie zmian z Azure Log Analytics | Dokumentacja firmy Microsoft"
description: "Rozwiązanie śledzenia zmian w Log Analytics pomaga zidentyfikować oprogramowanie i zmiany usługi systemu Windows w środowisku."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: f8040d5d-3c89-4f0c-8520-751c00251cb7
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/11/2017
ms.author: banders
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 57af000e47188786a77cdb84ebb6ffb5c50eafaa
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/18/2017
---
# <a name="track-software-changes-in-your-environment-with-the-change-tracking-solution"></a>Śledzenie zmian w oprogramowaniu w Twoim środowisku z rozwiązaniem do śledzenia zmian

![Zmień symbol śledzenia](./media/log-analytics-change-tracking/change-tracking-symbol.png)

W tym artykule opisano, jak za pomocą rozwiązania śledzenia zmian w analizy dzienników można łatwo zidentyfikować zmian w środowisku. Rozwiązania śledzi zmiany do oprogramowania systemu Windows i Linux, pliki systemu Windows i kluczy rejestru usług systemu Windows i demonów systemu Linux. Identyfikowanie zmian konfiguracji może pomóc w określeniu problemy z działaniem.

Należy zainstalować rozwiązanie, aby zaktualizować typu agenta, który został zainstalowany. Zmiany do zainstalowanego oprogramowania, usług systemu Windows i demonów systemu Linux na monitorowanych serwerach są do odczytu. Następnie dane są wysyłane do usługi Analiza dzienników w chmurze do przetwarzania. Logika jest stosowany do odebranych danych i usługi w chmurze rejestruje dane. Korzystając z informacji na pulpicie nawigacyjnym śledzenia zmian, można łatwo zobaczyć zmiany wprowadzone w ramach infrastruktury serwera.

## <a name="installing-and-configuring-the-solution"></a>Instalowanie i konfigurowanie rozwiązania
Skorzystaj z poniższych informacji, aby zainstalować i skonfigurować rozwiązania.

* Musi mieć [Windows](log-analytics-windows-agents.md), [programu Operations Manager](log-analytics-om-agents.md), lub [Linux](log-analytics-linux-agents.md) agenta na każdym komputerze, na którym mają być monitorowane zmian.
* Dodaj rozwiązanie śledzenia zmian na obszar roboczy OMS z [witrynę Azure marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.ChangeTrackingOMS?tab=Overview). Można dodać rozwiązania, korzystając z informacji w [rozwiązań dodać analizy dzienników z galerii rozwiązań](log-analytics-add-solutions.md). Jest wymagana żadna dalsza konfiguracja.

### <a name="configure-linux-files-to-track"></a>Konfiguruj pliki Linux do śledzenia
Poniższe kroki umożliwiają konfigurowanie plików na komputerach z systemem Linux.

1. W portalu OMS kliknij **ustawienia** (symbol koło zębate).
2. Na **ustawienia** kliknij przycisk **danych**, a następnie kliknij przycisk **Linux pliku śledzenia**.
3. W obszarze śledzenia zmian plików systemu Linux, wpisz pełną ścieżkę, łącznie z nazwą pliku, pliku, który chcesz śledzić, a następnie kliknij przycisk **Dodaj** symbolu. Na przykład: "/etc/*.conf"
4. Kliknij pozycję **Zapisz**.  

> [!NOTE]
> Linux pliku śledzenia ma dodatkowe możliwości, w tym katalogu śledzenia, recrusion za pośrednictwem katalogów i symboli wieloznacznych śledzenia.

### <a name="configure-windows-files-to-track"></a>Konfiguruj pliki systemu Windows do śledzenia
Poniższe kroki umożliwiają konfigurowanie plików na komputerach z systemem Windows.

1. W portalu OMS kliknij **ustawienia** (symbol koło zębate).
2. Na **ustawienia** kliknij przycisk **danych**, a następnie kliknij przycisk **śledzenia plików systemu Windows**.
3. W obszarze śledzenia zmian plików systemu Windows wpisz pełną ścieżkę, łącznie z nazwą pliku, pliku, który chcesz śledzić, a następnie kliknij przycisk **Dodaj** symbolu. Na przykład: C:\Program Files (x86) \Internet Explorer\iexplore.exe lub C:\Windows\System32\drivers\etc\hosts.
4. Kliknij pozycję **Zapisz**.  
   ![Śledzenie zmian plików systemu Windows](./media/log-analytics-change-tracking/windows-file-change-tracking.png)

### <a name="configure-windows-registry-keys-to-track"></a>Konfigurowanie kluczy rejestru systemu Windows do śledzenia
Poniższe kroki umożliwiają konfigurowanie klucze rejestru, aby śledzić na komputerach z systemem Windows.

1. W portalu OMS kliknij **ustawienia** (symbol koło zębate).
2. Na **ustawienia** kliknij przycisk **danych**, a następnie kliknij przycisk **śledzenia rejestru systemu Windows**.
3. W obszarze śledzenia zmian rejestru systemu Windows wpisz cały klucz, który chcesz śledzić, a następnie kliknij przycisk **Dodaj** symbolu.
4. Kliknij pozycję **Zapisz**.  
   ![Śledzenie zmian w rejestrze systemu Windows](./media/log-analytics-change-tracking/windows-registry-change-tracking.png)

### <a name="explanation-of-linux-file-collection-properties"></a>Opis właściwości kolekcji plików systemu Linux
1. **Typ**
   * **Plik** (raport metadanych pliku — rozmiar, Data modyfikacji, skrót itp.)
   * **Katalog** (katalogu metadanych raportu — rozmiar, Data modyfikacji itp.)
2. **Łącza** (Obsługa systemu Linux łącza symbolicznego odwołania do innych plików lub katalogów)
   * **Ignoruj** (Ignoruj łączy symbolicznych podczas recurions, aby nie dołączać plików/katalogów, do których odwołuje się)
   * **Postępuj zgodnie z** (wykonaj łączy symbolicznych podczas rekursji, aby obejmować plików/katalogów, do których odwołuje się)
   * **Zarządzanie** (wykonaj łączy symbolicznych i zmień traktowanie zwrócony zawartości)

   > [!NOTE]   
   > Opcja łącza "Manage" nie jest zalecane. Pobieranie zawartości pliku nie jest obsługiwana.

3. **Recurse** (Recurse za pomocą poziomów folderu i śledzić wszystkie pliki spełniające instrukcji ścieżki)
4. **Sudo** (Włącz dostęp do plików lub katalogów, które wymagają uprawnień sudo)

### <a name="limitations"></a>Ograniczenia
Rozwiązanie śledzenia zmian nie obsługuje obecnie następujące elementy:

* Folderów (katalogów) dla systemu Windows plik śledzenia
* Rekursja plików śledzenia
* Symbole wieloznaczne śledzenia plików systemu Windows
* Zmienne ścieżek
* Systemy plików sieciowych
* Zawartość pliku

Inne ograniczenia:

* **Maksymalny rozmiar pliku** kolumny i wartości są używane w bieżącej implementacji.
* W przypadku zebrania plików więcej niż 2500 w cyklu zbierania 30 minut, może wiązać ze spadkiem wydajności rozwiązania.
* Gdy ruch sieciowy jest duże, rejestruje zmiany może potrwać maksymalnie sześć godzin do wyświetlenia.
* Jeśli zmodyfikujesz konfiguracji, gdy komputer jest wyłączony, komputer opublikować zmian plików, które należały do poprzedniej konfiguracji.

## <a name="change-tracking-data-collection-details"></a>Zmień szczegóły kolekcji danych śledzenia
Śledzenie zmian zbiera dane spisu oprogramowania i metadanych usługi systemu Windows przy użyciu agentów, które zostało włączone.

W poniższej tabeli przedstawiono metody zbierania danych i inne szczegółowe informacje o jak dane są zbierane dla śledzenia zmian.

| Platformy | Bezpośrednie agenta | Agent programu Operations Manager | Agent systemu Linux | Azure Storage | Wymagane programu Operations Manager? | Danych agenta programu Operations Manager są wysyłane za pośrednictwem grupy zarządzania | Częstotliwość kolekcji |
| --- | --- | --- | --- | --- | --- | --- | --- |
| System Windows i Linux | &#8226; | &#8226; | &#8226; |  |  | &#8226; | 5 minut do 50 minut w zależności od typu zmiany. Więcej informacji można znaleźć w tabeli poniżej. |


W poniższej tabeli przedstawiono częstotliwość zbierania danych dla typów zmian.

| **Zmień typ** | **częstotliwość** | **Jest****agenta****wysyłania różnice, jeśli znaleziono?**  |
| --- | --- | --- |
| Rejestr systemu Windows | 50 minut | Nie |
| Plik systemu Windows | 30 minut | Tak. Jeśli nie została zmieniona w ciągu 24 godzin, jest wysyłane migawki. |
| Plik systemu Linux | 15 minut | Tak. Jeśli nie została zmieniona w ciągu 24 godzin, jest wysyłane migawki. |
| Usługi systemu Windows | 30 minut | Tak, co 30 minut, jeśli znaleziono zmian. Co 24 godziny migawki są wysyłane, niezależnie od zmian. Dlatego migawki są wysyłane nawet w przypadku, gdy nie wprowadzono żadnych zmian. |
| Demonów systemu Linux | 5 minut | Tak. Jeśli nie została zmieniona w ciągu 24 godzin, jest wysyłane migawki. |
| Oprogramowanie Windows | 30 minut | Tak, co 30 minut, jeśli znaleziono zmian. Co 24 godziny migawki są wysyłane, niezależnie od zmian. Dlatego migawki są wysyłane nawet w przypadku, gdy nie wprowadzono żadnych zmian. |
| Oprogramowania w systemie Linux | 5 minut | Tak. Jeśli nie została zmieniona w ciągu 24 godzin, jest wysyłane migawki. |

### <a name="registry-key-change-tracking"></a>Śledzenie zmian klucza rejestru

Analiza dzienników wykonuje rejestru systemu Windows, monitorowanie i śledzenie dzięki rozwiązaniu do śledzenia zmian. Monitorowanie zmian w kluczach rejestru ma na celu wskazanie punkty rozszerzeń, gdzie można uaktywnić kodu innych firm i złośliwego oprogramowania. Poniższa lista zawiera domyślne klucze rejestru, które są śledzone przez rozwiązanie i dlaczego każdego śledzone.

- Klucz HKEY\_lokalnego\_MACHINE\Software\Microsoft\Windows\CurrentVersion\Group Policy\Scripts\Startup
    - Monitory skrypty uruchamiane automatycznie.
- Klucz HKEY\_lokalnego\_MACHINE\Software\Microsoft\Windows\CurrentVersion\Group Policy\Scripts\Shutdown
    - Skrypty monitorów Uruchom przy zamykaniu.
- Klucz HKEY\_lokalnego\_MACHINE\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\Run
    - Monitoruje klucze, które są ładowane przed użytkownik loguje się ich kont systemu Windows. Klucz jest używany dla 32-bitowe programów na komputerach 64-bitowych.
- Klucz HKEY\_lokalnego\_MACHINE\SOFTWARE\Microsoft\Active Setup\Installed składników
    - Monitoruje zmiany w ustawieniach aplikacji.
- Klucz HKEY\_lokalnego\_MACHINE\Software\Classes\Directory\ShellEx\ContextMenuHandlers
    - Monitory wspólnej autostart wpisów, które przyłączanie się bezpośrednio do Eksploratora Windows i zwykle uruchamiania w procesie z Explorer.exe.
- Klucz HKEY\_lokalnego\_MACHINE\Software\Classes\Directory\Shellex\CopyHookHandlers
    - Monitory wspólnej autostart wpisów, które przyłączanie się bezpośrednio do Eksploratora Windows i zwykle uruchamiania w procesie z Explorer.exe.
- Klucz HKEY\_lokalnego\_MACHINE\Software\Classes\Directory\Background\ShellEx\ContextMenuHandlers
    - Monitory wspólnej autostart wpisów, które przyłączanie się bezpośrednio do Eksploratora Windows i zwykle uruchamiania w procesie z Explorer.exe.
- Klucz HKEY\_lokalnego\_MACHINE\Software\Microsoft\Windows\CurrentVersion\Explorer\ShellIconOverlayIdentifiers
    - Monitory dla ikony nakładki obsługi rejestracji.
- Klucz HKEY\_lokalnego\_MACHINE\Software\Wow6432Node\Microsoft\Windows\CurrentVersion\Explorer\ShellIconOverlayIdentifiers
    - Monitory dla ikony nakładki rejestracji programu obsługi dla 32-bitowe programów na komputerach 64-bitowych.
- Klucz HKEY\_lokalnego\_MACHINE\Software\Microsoft\Windows\CurrentVersion\Explorer\Browser obiekty pomocnicze
    - Monitory nowych wtyczek obiektu pomocnika przeglądarki Internet Explorer. Używane do dostępu do modelu DOM (Document Object) bieżącej strony i sterowania nawigacji.
- Klucz HKEY\_lokalnego\_MACHINE\Software\Wow6432Node\Microsoft\Windows\CurrentVersion\Explorer\Browser obiekty pomocnicze
    - Monitory nowych wtyczek obiektu pomocnika przeglądarki Internet Explorer. Używane do dostępu do modelu DOM (Document Object) bieżącej strony i sterowanie nawigacją dla 32-bitowe programów na komputerach 64-bitowych.
- Klucz HKEY\_lokalnego\_MACHINE\Software\Microsoft\Internet Explorer\Extensions
    - Monitory dla nowych rozszerzeń programu Internet Explorer, takie jak narzędzie niestandardowe menu i niestandardowe przyciski paska narzędzi.
- Klucz HKEY\_lokalnego\_MACHINE\Software\Wow6432Node\Microsoft\Internet Explorer\Extensions
    - Monitory dla nowych rozszerzeń programu Internet Explorer, takie jak narzędzie niestandardowe menu i przycisków paska narzędzi niestandardowych dla 32-bitowe programów na komputerach 64-bitowych.
- Klucz HKEY\_lokalnego\_MACHINE\Software\Microsoft\Windows NT\CurrentVersion\Drivers32
    - Monitoruje 32-bitowe sterowniki skojarzone z wavemapper, wave1 i wave2 msacm.imaadpcm, .msadpcm, .msgsm610 i vidc. Podobnie jak w sekcji [sterowników] w systemie. Pliku INI.
- Klucz HKEY\_lokalnego\_MACHINE\Software\Wow6432Node\Microsoft\Windows NT\CurrentVersion\Drivers32
    - Monitory 32-bitowe sterowniki są skojarzone z wavemapper, wave1 i wave2 msacm.imaadpcm, .msadpcm, .msgsm610 i vidc dla 32-bitowe programów na komputerach 64-bitowych. Podobnie jak w sekcji [sterowników] w systemie. Pliku INI.
- Klucz HKEY\_lokalnego\_MACHINE\System\CurrentControlSet\Control\Session Manager\KnownDlls
    - Monitoruje listę znanych lub często używane systemowej biblioteki dll; Ten system uniemożliwia osób wykorzystania uprawnienia katalogu aplikacji słabe przez usunięcie koń trojański wersji systemowej biblioteki dll.
- Klucz HKEY\_lokalnego\_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon\Notify
    - Monitoruje listy pakietów mogli otrzymywać powiadomienia o zdarzeniach usługi Winlogon, logowania interakcyjnego modelu pomocy technicznej dla systemu operacyjnego Windows.


## <a name="use-change-tracking"></a>Użyj śledzenie zmian
Po zainstalowaniu rozwiązania przy użyciu można wyświetlić podsumowanie zmian monitorowanych serwerów **śledzenia zmian** Kafelek na **omówienie** strony w OMS.

![Obraz śledzenia zmian kafelka](./media/log-analytics-change-tracking/change-tracking-tile.png)

Można wyświetlić zmiany do Twojej infrastruktury, a następnie przechodzenie do szczegółów w ramach następujących kategorii:

* Zmiany według typu konfiguracji oprogramowania i usług systemu Windows
* Zmiany oprogramowania do aplikacji i aktualizacji dla poszczególnych serwerów
* Całkowita liczba zmiany oprogramowania dla każdej aplikacji
* Pakietów systemu Linux
* Zmiany usług systemu Windows dla poszczególnych serwerów
* Zmiany demonów systemu Linux

![Obraz śledzenia zmian pulpitu nawigacyjnego](./media/log-analytics-change-tracking/change-tracking-dash01.png)

![Obraz śledzenia zmian pulpitu nawigacyjnego](./media/log-analytics-change-tracking/change-tracking-dash02.png)

### <a name="to-view-changes-for-any-change-type"></a>Aby wyświetlić zmiany w każdej zmianę typu
1. Na **omówienie** kliknij przycisk **śledzenia zmian** kafelka.
2. Na **zmienić śledzenia** pulpitu nawigacyjnego, sprawdź informacje w jednym z bloków typ zmiany, a następnie kliknij przycisk jedna, aby wyświetlić szczegółowe informacje o nim w **wyszukiwania dziennika** strony.
3. Na wszystkich stronach wyszukiwania dziennika można wyświetlić wyniki według czasu, szczegółowe wyniki i historię dziennik wyszukiwania. Można również filtrować według aspekty, aby zawęzić wyniki.

## <a name="next-steps"></a>Następne kroki
* Użyj [Zaloguj wyszukiwania analizy dzienników](log-analytics-log-searches.md) aby zobaczyć zmianę szczegółowe dane śledzenia.
