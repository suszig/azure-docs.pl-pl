Agent synchronizacji plików Azure została zaktualizowana, regularne dodawanie nowych funkcji, a także do rozwiązywania problemów. Firma Microsoft zaleca się skonfigurowanie usługi Microsoft Update do pobierania aktualizacji dla agenta synchronizacji plików Azure, ponieważ są one dostępne.

#### <a name="major-vs-minor-agent-versions"></a>Główne, a wersje pomocnicze agentów
* Wersje agentów głównych często zawierają nowe funkcje i mieć coraz jako pierwsza część numeru wersji. Na przykład: *2.\*.\**
* Wersje agentów pomocnicze są również nazywane "poprawek" i są wydawane częściej niż wersje główne. Zawierają one często poprawki i ulepszenia mniejsze, ale żadne nowe funkcje. Na przykład:  *\*.3.\**

#### <a name="upgrade-paths"></a>Ścieżki uaktualniania
Istnieją trzy zatwierdzone i przetestowane sposoby instalacji aktualizacji agenta synchronizacji plików Azure. Zaktualizuj te pracy ścieżki dla wersji główne i pomocnicze.
1. **(Preferowane) Konfigurowanie usługi Microsoft Update do automatycznego pobierania i instalowania aktualizacji agenta.**  
    Zawsze zalecamy biorąc każdej aktualizacji synchronizacji plików Azure, aby upewnić się, że masz dostęp do najnowszych poprawek dla agenta programu server. Microsoft Update powoduje, że ten proces jest bezproblemowe, przez automatyczne pobieranie i instalowanie aktualizacji dla Ciebie.
2. **Poprawka istniejącego agenta synchronizacji plików Azure za pomocą plik poprawki usługi Microsoft Update lub pliku wykonywalnego MSP. Można pobrać najnowszy pakiet aktualizacji synchronizacji plików Azure z [katalogu rozszerzenia Microsoft Update](https://www.catalog.update.microsoft.com/Search.aspx?q=Azure%20File%20Sync).**  
    Uruchamianie pliku wykonywalnego MSP uaktualni instalacji synchronizacji plików Azure z tej samej metody automatycznie przez usługę Microsoft Update w poprzednim ścieżki uaktualnienia. Stosowanie poprawki usługi Microsoft Update przeprowadzi uaktualnienie w miejscu instalacji synchronizacji plików Azure.
3. **Pobieranie najnowszej synchronizacji plików Azure Instalatora agenta z [Microsoft Download Center](https://go.microsoft.com/fwlink/?linkid=858257). Pobieranie Instalatora jest pakiet Microsoft Installer lub pliku wykonywalnego msi.**  
    Aby uaktualnić istniejącą instalację agenta synchronizacji plików Azure, odinstalować starszej wersji, a następnie zainstaluj najnowszą wersję z pobranego Instalatora. Rejestracja serwera, grupy synchronizacji i innych ustawień są obsługiwane przez Instalatora synchronizacji plików Azure.

#### <a name="agent-lifecycle-and-change-management-guarantees"></a>Gwarantuje agenta cykl życia i zarządzanie zmianami
Synchronizacja programu Azure plików jest usługą w chmurze, co umożliwia stale wprowadzenie nowych funkcji i funkcjonalności. Oznacza to, że określonej wersji agenta Azure synchronizacji plików może być obsługiwana tylko przez ograniczony czas. W celu ułatwienia wdrożenia, poniższe reguły, aby zagwarantować, należy mieć wystarczająco dużo czasu i powiadomienie, aby pomieścić agenta aktualizacje/uaktualnienia w procesie zarządzania zmiany:

- Wersje agentów główne są obsługiwane dla co najmniej sześciu miesięcy od daty początkowej wersji.
- Firma Microsoft gwarantuje, że co najmniej trzech miesięcy od pomocy technicznej dla wersji głównej agentów nakładają się. 
- Są wyświetlane ostrzeżenia dla zarejestrowanych serwerów przy użyciu agenta wygasłe wkrótce do się co najmniej trzech miesięcy przed jego wygaśnięciem. Aby sprawdzić, czy zarejestrowanego serwera używa starszej wersji agenta w sekcji zarejestrowane serwery usługi synchronizacji magazynu.
- Okres istnienia wersja pomocnicza agenta jest powiązany z skojarzone wersji głównej. Na przykład po zwolnieniu agenta w wersji 3.0, agenta w wersji 2. \* zostanie wszystkie ustawiona wygaśnie razem.

> [!Note]
> Zainstalowana jest wersja agenta z ostrzeżeniem wygaśnięcia zostanie wyświetlone ostrzeżenie o, ale powiodło się. Próby zainstalowania lub połączyć się z wersją agenta wygasłe nie jest obsługiwana i zostanie zablokowane.