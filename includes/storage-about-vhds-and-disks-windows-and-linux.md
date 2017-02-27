### <a name="disk-persistence"></a>Trwałość dysku 

W poniższej tabeli przedstawiono różne stany dla różnych dysków w maszynach wirtualnych platformy Azure i informacje o tym, czy dane w poszczególnych stanach są utrwalane, czy nie.

| Dysk | Uruchamianie | Zatrzymywanie/<br>cofanie przydziału | Wstrzymaj | Ponowny<br>rozruch | Zamykanie/<br>wyłączanie | Usuwanie | Niepowodzenie | Zmiana rozmiaru | 
| ---- | ----- | ---- | ---- | ---- | ----  | ------ | ------- | ------ | 
| Dysk systemu operacyjnego | Tak | Tak  | Tak | Tak | Tak  | Nie | Nie  | Tak | 
| Pamięć RAM  | Tak | Tak | Tak | Tak | Nie   | Nie | Nie | Nie | 
| Lokalny dysk tymczasowy | Tak | Nie | Tak | Nie | Nie  | Nie | Nie | Nie | 
| Dołączony dysk danych | Tak | Tak | Tak | Tak | Tak  | Tak | Tak | Tak | 

## <a name="about-vhds"></a>Informacje o wirtualnych dyskach twardych

Wirtualne dyski twarde używane na platformie Azure to pliki vhd przechowywane jako stronicowe obiekty blob na koncie magazynu w warstwie Standardowa lub Premium na platformie Azure. Aby uzyskać informacje na temat stronicowych obiektów blob, zobacz [Understanding block blobs and page blobs](/rest/api/storageservices/fileservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs/) (Omówienie blokowych i stronicowych obiektów blob). Aby uzyskać szczegółowe informacje na temat magazynu w warstwie Premium, zobacz [High-performance premium storage and Azure VMs](../articles/storage/storage-premium-storage.md) (Magazyn w warstwie Premium o wysokiej wydajności i maszyny wirtualne platformy Azure).

Platforma Azure obsługuje stały format VHD dysków. W przypadku stałego formatu dysk logiczny jest zapisywany w pliku w sposób liniowy w taki sposób, że przesunięcie X na dysku jest zapisywane w obiekcie blob z przesunięciem X. Niewielka stopka na końcu obiektu blob opisuje właściwości wirtualnego dysku twardego. Często zdarza się, że stały format prowadzi do marnotrawienia miejsca, ponieważ większość dysków zawiera duże nieużywane zakresy. Platforma Azure jednak przechowuje pliki vhd w formacie rozrzedzonym, więc jednocześnie uzyskuje się zalety dysków stałych i dynamicznych. Aby uzyskać więcej szczegółów, zobacz [Getting started with virtual hard disks](https://technet.microsoft.com/library/dd979539.aspx) (Pierwsze kroki z wirtualnymi dyskami twardymi).

Wszystkie pliki vhd na platformie Azure, których chcesz użyć jako źródła do utworzenia dysków lub obrazów, są tylko do odczytu. Gdy tworzysz dysk lub obraz, platforma Azure tworzy kopie plików vhd. Te kopie mogą być tylko do odczytu lub do odczytu i zapisu, zależnie od tego, jak korzystasz z wirtualnego dysku twardego.

Gdy tworzysz maszynę wirtualną na podstawie obrazu, platforma Azure tworzy dysk dla maszyny wirtualnej, który jest kopią źródłowego pliku vhd. Platforma Azure umieszcza dzierżawę na każdym pliku vhd używanym do utworzenia obrazu, dysku systemu operacyjnego lub dysku danych, aby chronić ten plik przed przypadkowym usunięciem.

Zanim będzie możliwe usunięcie źródłowego pliku vhd, trzeba będzie usunąć tę dzierżawę, usuwając dysk lub obraz. Aby usunąć plik vhd, który jest używany przez maszynę wirtualną jako dysk systemu operacyjnego, można jednocześnie usunąć maszynę wirtualną, dysk systemu operacyjnego i źródłowy plik vhd, usuwając maszynę wirtualną i wszystkie skojarzone dyski. Usunięcie pliku vhd będącego źródłem dysku danych wymaga jednak wykonania kilku kroków w ustalonej kolejności. Najpierw należy odłączyć dysk od maszyny wirtualnej, później usunąć dysk, a następnie usunąć plik vhd.

> [!WARNING]
> Jeśli usuniesz źródłowy plik vhd z magazynu albo usuniesz swoje konto magazynu, firma Microsoft nie będzie mogła odzyskać usuniętych danych.
> 

## <a name="types-of-disks"></a>Typy dysków 

Do wyboru podczas tworzenia dysków są dwie warstwy wydajności — magazyny Standard Storage i Premium Storage. Ponadto istnieją dwa typy dysków — niezarządzane i zarządzane — mogące występować w obu warstwach wydajności.  

### <a name="standard-storage"></a>Standard Storage 

Magazyn Standard Storage bazuje na dyskach twardych (HDD) i stanowi ekonomiczne, chociaż wciąż wydajne rozwiązanie. Magazyn Standard Storage może być replikowany lokalnie w jednym centrum danych albo być objęty nadmiarowością geograficzną z głównym i dodatkowym centrum danych. Aby uzyskać więcej informacji na temat replikowania magazynu, zobacz [Replikacja usługi Azure Storage](../articles/storage/storage-redundancy.md). 

Aby uzyskać więcej informacji na temat korzystania z magazynu Standard Storage z dyskami maszyn wirtualnych, zobacz [Standard Storage and Disks](../articles/storage/storage-standard-storage.md) (Magazyn Standard Storage i dyski).

### <a name="premium-storage"></a>Premium Storage 

Magazyn Premium Storage bazuje na dyskach półprzewodnikowych (SSD) i oferuje dyski o wysokiej wydajności i małych opóźnieniach dla maszyn wirtualnych, na których działają obciążenia intensywnie korzystające z operacji wejścia/wyjścia. Magazynu Premium Storage można używać z maszynami wirtualnymi platformy Azure serii DS, DSv2, GS i FS. Aby uzyskać więcej informacji, zobacz [Premium Storage](../articles/storage/storage-premium-storage.md).

### <a name="unmanaged-disks"></a>Dyski niezarządzane

Dyski niezarządzane to tradycyjny typ dysków używany przez maszyny wirtualne. Za ich pomocą tworzy się własne konto magazynu, które wskazuje się podczas tworzenia dysku. Musisz upewnić się, że nie umieszczasz zbyt wielu dysków na tym samym koncie magazynu, ponieważ możesz przekroczyć [cele skalowalności](../articles/storage/storage-scalability-targets.md) konta magazynu (np.&20; 000 IOPS), przez co wydajność maszyn wirtualnych będzie ograniczona. Gdy korzystasz z dysków niezarządzanych, musisz opracować sposób zmaksymalizowania użycia jednego lub większej liczby kont magazynu w celu uzyskania najlepszej wydajności maszyn wirtualnych.

### <a name="managed-disks"></a>Dyski zarządzane 

W przypadku dysków zarządzanych tworzenie konta magazynu i zarządzanie nim odbywa się automatycznie w tle i nie trzeba martwić się o limity skalowalności konta magazynu. Wystarczy określić rozmiar dysku i warstwę wydajności (Standardowa/Premium), a platforma Azure sama utworzy dysk i będzie nim zarządzać. O używanym magazynie nie trzeba myśleć nawet przy dodawaniu dysków albo skalowaniu maszyny wirtualnej w górę i w dół. 

Można też zarządzać własnymi obrazami niestandardowymi na jednym koncie magazynu na region platformy Azure i używać ich do tworzenia setek maszyn wirtualnych w tej samej subskrypcji. Aby uzyskać więcej informacji na temat dysków zarządzanych, zobacz [Omówienie usługi Managed Disks](../articles/storage/storage-managed-disks-overview.md).

Aby czerpać korzyści z wielu funkcji dysków zarządzanych, zalecamy używanie usługi Azure Managed Disks dla nowych maszyn wirtualnych i przekonwertowanie wcześniej używanych dysków niezarządzanych na dyski zarządzane.

### <a name="disk-comparison"></a>Porównanie dysków

Poniższa tabela zawiera porównanie warstw Premium i Standardowa dla dysków niezarządzanych i zarządzanych, które może być pomocne przy wybieraniu rozwiązania do zastosowania.

|    | Dysk platformy Azure w warstwie Premium | Dysk platformy Azure w warstwie Standardowa |
|--- | ------------------ | ------------------- |
| Typ dysku | Dyski półprzewodnikowe (SSD) | Dyski twarde (HDD)  |
| Omówienie  | Pamięć dyskowa o wysokiej wydajności i małych opóźnieniach, bazująca na dyskach SSD, przeznaczona dla maszyn wirtualnych uruchamiających obciążenia intensywnie korzystające z operacji wejścia/wyjścia lub hostujących środowisko produkcyjne o znaczeniu krytycznym | Ekonomiczna pamięć dyskowa oparta na dyskach HDD, przeznaczona dla scenariuszy z maszynami wirtualnymi do programowania i testowania |
| Scenariusz  | Obciążenia produkcyjne i wrażliwe na wydajność | Programowanie i testowanie, zastosowania niekrytyczne <br>Rzadki dostęp |
| Rozmiar dysku | P10: 128 GB<br>P20: 512 GB<br>P30: 1024 GB | Dyski niezarządzane: 1 GB–1 TB <br><br>Dyski zarządzane:<br> S4: 32 GB <br>S6: 64 GB <br>S10: 128 GB <br>S20: 512 GB <br>S30: 1024 GB |
| Maksymalna przepływność na dysk | 200 MB/s | 60 MB/s |
| Maksymalna liczba operacji wejścia/wyjścia na dysk | 5000 IOPS | 500 IOPS |

<!--HONumber=Feb17_HO3-->


