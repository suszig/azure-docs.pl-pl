# <a name="back-up-azure-unmanaged-vm-disks-with-incremental-snapshots"></a>Tworzenie kopii zapasowej Azure niezarządzane dysków maszyny Wirtualnej z migawkami przyrostowe
## <a name="overview"></a>Omówienie
Magazyn Azure oferuje możliwość migawek obiektów blob. Migawki przechwytywania stanu obiektu blob w danym momencie. W tym artykule opisano scenariusz, w którym można zachować kopie zapasowe przy użyciu migawek dysków maszyny wirtualnej. Można użyć tej metody, gdy nie chcesz używać usługi Kopia zapasowa Azure i usługą odzyskiwania i chcesz utworzyć niestandardowe strategii tworzenia kopii zapasowych dysków maszyny wirtualnej.

Dyski maszyny wirtualnej platformy Azure są przechowywane jako stronicowe obiekty BLOB w usłudze Azure Storage. Ponieważ firma Microsoft zawierająca opis strategii tworzenia kopii zapasowych dysków maszyny wirtualnej w tym artykule, zwane migawek w kontekście stronicowych obiektów blob. Aby dowiedzieć się więcej na temat migawek, zobacz [tworzenia migawki obiektu Blob](https://docs.microsoft.com/rest/api/storageservices/Creating-a-Snapshot-of-a-Blob).

## <a name="what-is-a-snapshot"></a>Co to jest migawkę?
Migawki obiektu blob jest tylko do odczytu wersji obiektu blob przechwyconych w punkcie w czasie. Po utworzeniu migawki, to można można odczytać, kopiowane, lub usunięte, ale nie został zmodyfikowany. Migawki umożliwiają tworzenie kopii zapasowej obiektu blob, pojawiającą się w chwili. Do POZOSTAŁEJ wersji 2015-04-05 masz możliwość kopiowania pełnej migawki. Z resztą wersji 2015-07-08 i powyżej, możesz również skopiować przyrostowe migawki.

## <a name="full-snapshot-copy"></a>Pełna migawka kopii
Migawki można skopiować do innego konta magazynu jako obiekt blob przechowywania kopii zapasowych podstawowego obiektu blob. Można również skopiować migawki za pośrednictwem jego podstawowego obiektu blob, czyli jak przywracanie obiektu blob do wcześniejszej wersji. Po skopiowaniu migawki z jednego konta magazynu do innego zajmuje to samo miejsce jako podstawowy stronicowych obiektów blob. W związku z tym kopiowania całego migawki z jednego konta magazynu do innego działa wolno i zużywa dużo miejsce docelowe konto magazynu.

> [!NOTE]
> Po skopiowaniu podstawowego obiektu blob do innej lokalizacji docelowej migawki obiektu blob nie są kopiowane razem z nim. Podobnie w przypadku zastąpienia podstawowego obiektu blob z kopią, migawki skojarzone z podstawowego obiektu blob nie dotyczy, a pozostać bez zmian w obszarze nazwy podstawowe obiektu blob.
> 
> 

### <a name="back-up-disks-using-snapshots"></a>Wykonywanie kopii zapasowych dysków przy użyciu migawek
Jako strategii tworzenia kopii zapasowych dysków maszyny wirtualnej, można wykonać okresowe migawki obiektu blob dysku lub strony i kopiowania do magazynu innego konta przy użyciu narzędzi, takich jak [kopiowania obiektu Blob](https://docs.microsoft.com/rest/api/storageservices/Copy-Blob) operacji lub [AzCopy](../articles/storage/common/storage-use-azcopy.md). Migawki można kopiować do obiektu blob strony docelowy o innej nazwie. Wynikowa stronicowych obiektów blob docelowym jest zapisywalny stronicowy obiekt blob i nie migawki. Później w tym artykule opisano kroki, aby korzystać z kopii zapasowych przy użyciu migawek dysków maszyny wirtualnej.

### <a name="restore-disks-using-snapshots"></a>Przywracanie dysków za pomocą migawek
Gdy nadejdzie czas, aby przywrócić stabilną wersję przechwyconych wcześniej w jednym z migawek kopii zapasowych na dysku, możesz skopiować migawki za pośrednictwem obiektu blob strony podstawowej. Po migawki jest podwyższany do strony podstawowej obiektu blob, pozostaje migawki, ale jego źródło jest zastępowany kopii, którą można zarówno Odczyt i zapis. W tym artykule opisano kroki, aby przywrócić poprzednią wersję dysku z jej migawek.

### <a name="implementing-full-snapshot-copy"></a>Implementowanie pełna migawka kopii
Można zaimplementować kopii pełna migawka, wykonując następujące polecenie,

* Najpierw Utwórz migawkę przy użyciu podstawowego obiektu blob [migawki obiektu Blob](https://docs.microsoft.com/rest/api/storageservices/Snapshot-Blob) operacji.
* Następnie skopiuj do docelowego magazynu konta za pomocą migawki [kopiowania obiektu Blob](https://docs.microsoft.com/rest/api/storageservices/Copy-Blob).
* Powtórz ten proces do obsługi kopii zapasowych z podstawowego obiektu blob.

## <a name="incremental-snapshot-copy"></a>Kopiuj przyrostowe migawki
Nową funkcją w [GetPageRanges](https://docs.microsoft.com/rest/api/storageservices/Get-Page-Ranges) interfejs API udostępnia znacznie poprawia sposobem wykonania kopii zapasowej migawki stronicowych obiektów blob lub dysków. Interfejsu API zwraca listę zmian podstawowego obiektu blob i migawki, co zmniejsza ilość miejsca używanego na konto kopii zapasowej. Interfejs API obsługuje stronicowych obiektów blob na magazyn w warstwie Premium, a także magazynu w warstwie standardowa. Przy użyciu tego interfejsu API, można budować szybszych i bardziej wydajnych rozwiązań kopii zapasowej dla maszyn wirtualnych platformy Azure. Ten interfejs API będą dostępne w używanej wersji REST 2015-07-08 i wyższych.

Przyrostowa kopia migawki umożliwia kopiowanie z jednego konta magazynu do innego różnicy między,

* Podstawowy obiektów blob i jego migawki lub
* Wszystkie migawki dwa podstawowe obiektu blob

Pod warunkiem, że są spełnione następujące warunki,

* Obiekt blob utworzono Jan-1-2016 lub nowszy.
* Obiekt blob nie zostało zastąpione [PutPage](https://docs.microsoft.com/rest/api/storageservices/Put-Page) lub [kopiowania obiektu Blob](https://docs.microsoft.com/rest/api/storageservices/Copy-Blob) między dwiema migawkami.

**Uwaga**: Ta funkcja jest dostępna dla Premium i standardowa Azure stronicowe obiekty BLOB.

Jeśli masz niestandardowe strategii tworzenia kopii zapasowej przy użyciu migawek Kopiowanie migawki z jednego konta magazynu do innego mogą być powolne i mogą zużywać dużą ilość miejsca do magazynowania. Zamiast kopiować całe migawki na konto magazynu kopii zapasowej, można napisać różnica między kolejnych migawki kopii zapasowej stronicowy obiekt blob. W ten sposób znacznie zmniejszyć czas kopiowania i miejsca do przechowywania kopii zapasowych.

### <a name="implementing-incremental-snapshot-copy"></a>Implementowanie kopiowania przyrostowe migawki
Wykonaj następujące czynności, można zaimplementować przyrostowe migawki kopii

* Utwórz migawkę przy użyciu podstawowego obiektu blob [migawki obiektu Blob](https://docs.microsoft.com/rest/api/storageservices/Snapshot-Blob).
* Skopiuj do docelowego magazynu kopii zapasowej konta przy użyciu migawki [kopiowania obiektu Blob](https://docs.microsoft.com/rest/api/storageservices/Copy-Blob). To jest kopia zapasowa stronicowych obiektów blob. Utworzenie migawki kopii zapasowej stronicowych obiektów blob i zapisze go w kopii zapasowej konta.
* Utwórz kolejną migawkę podstawowej za pomocą obiektu Blob migawki obiektu blob.
* Pobierz różnica między pierwszym i drugim migawki za pomocą podstawowego obiektu blob [GetPageRanges](https://docs.microsoft.com/rest/api/storageservices/Get-Page-Ranges). Użyj nowego parametru **prevsnapshot**, aby określić wartość DateTime w celu uzyskania różnica w stosunku do migawki. Jeśli ten parametr jest obecny, odpowiedź REST zawiera tylko stron, które zostały zmienione między migawki docelowej i poprzednią migawkę tym wyczyść strony.
* Użyj [PutPage](https://docs.microsoft.com/rest/api/storageservices/Put-Page) Aby zastosować te zmiany do tworzenia kopii zapasowej stronicowych obiektów blob.
* Na koniec Utwórz migawkę kopii zapasowej stronicowych obiektów blob i zapisze go na koncie magazynu kopii zapasowej.

W następnej sekcji zostaną przedstawione szczegółowo w sposób umożliwiający obsługę kopii zapasowych dysków przy użyciu przyrostowej migawki kopii

## <a name="scenario"></a>Scenariusz
W tej sekcji opisano scenariusz, który obejmuje niestandardowych strategii tworzenia kopii zapasowej przy użyciu migawek dysków maszyny wirtualnej.

Rozważ premium magazynu P30 dysku maszyny Wirtualnej Azure serii DS. Dysk P30 nazywany *mypremiumdisk* są przechowywane na koncie magazynu premium o nazwie *mypremiumaccount*. Konto magazynu w warstwie standardowa nazywane *mybackupstdaccount* służy do przechowywania kopii zapasowej *mypremiumdisk*. Chcemy zachować migawkę *mypremiumdisk* co 12 godzin.

Aby dowiedzieć się więcej o tworzeniu konta magazynu i dyski, zapoznaj się [kont magazynu Azure o](../articles/storage/storage-create-storage-account.md).

Aby dowiedzieć się więcej o tworzeniu kopii zapasowych maszyn wirtualnych platformy Azure, zapoznaj się [kopii zapasowych maszyny Wirtualnej Azure Planowanie](../articles/backup/backup-azure-vms-introduction.md).

## <a name="steps-to-maintain-backups-of-a-disk-using-incremental-snapshots"></a>Kroki do obsługi kopii zapasowych na dysku za pomocą przyrostowej migawki
W poniższych krokach opisano sposób migawek *mypremiumdisk* i obsługa kopii zapasowych w *mybackupstdaccount*. Kopia zapasowa jest standardowe stronicowy obiekt blob o nazwie *mybackupstdpageblob*. W obiekcie blob kopii zapasowej strony zawsze odzwierciedla jako ostatnia migawka z takim samym stanie *mypremiumdisk*.

1. Utworzenie obiektu blob strony tworzenia kopii zapasowej dla dysku magazynu premium, wykonując migawkę *mypremiumdisk* o nazwie *mypremiumdisk_ss1*.
2. Skopiuj tę migawkę do mybackupstdaccount jako stronicowy obiekt blob o nazwie *mybackupstdpageblob*.
3. Utwórz migawkę *mybackupstdpageblob* o nazwie *mybackupstdpageblob_ss1*za pomocą [migawki obiektu Blob](https://docs.microsoft.com/rest/api/storageservices/Snapshot-Blob) i zapisze go w *mybackupstdaccount*.
4. Podczas tworzenia kopii zapasowych, Utwórz kolejną migawkę z *mypremiumdisk*, powiedz *mypremiumdisk_ss2*i zapisz go w *mypremiumaccount*.
5. Pobierz zmiany przyrostowe między dwiema migawkami *mypremiumdisk_ss2* i *mypremiumdisk_ss1*za pomocą [GetPageRanges](https://docs.microsoft.com/rest/api/storageservices/Get-Page-Ranges) na *mypremiumdisk_ ss2* z **prevsnapshot** parametr wartość sygnatury czasowej *mypremiumdisk_ss1*. Zapisać te zmiany przyrostowe do tworzenia kopii zapasowej stronicowych obiektów blob *mybackupstdpageblob* w *mybackupstdaccount*. W przypadku usunięto zakresów w przyrostowe zmiany, muszą zostać wyczyszczone z kopii zapasowej stronicowych obiektów blob. Użyj [PutPage](https://docs.microsoft.com/rest/api/storageservices/Put-Page) zapisanie zmian przyrostowych kopii zapasowych stronicowych obiektów blob.
6. Utwórz migawkę kopii zapasowej stronicowych obiektów blob *mybackupstdpageblob*o nazwie *mybackupstdpageblob_ss2*. Usunąć poprzednią migawkę *mypremiumdisk_ss1* z konta magazynu premium.
7. Powtórz kroki 4 – 6 co okna kopii zapasowej. W ten sposób można zachować kopie zapasowe *mypremiumdisk* w ramach konta magazynu w warstwie standardowa.

![Tworzenie kopii zapasowej dysku przy użyciu przyrostowej migawki](../articles/virtual-machines/windows/media/incremental-snapshots/storage-incremental-snapshots-1.png)

## <a name="steps-to-restore-a-disk-from-snapshots"></a>Kroki w celu przywrócenia dysku z migawki
W poniższych krokach opisano sposób przywracania dysku premium *mypremiumdisk* do wcześniejszej migawki z konta magazynu kopii zapasowej *mybackupstdaccount*.

1. Identyfikowania punktu w czasie, który chcesz przywrócić dysku premium. Załóżmy, że jest ona migawki *mybackupstdpageblob_ss2*, który jest przechowywany na koncie magazynu kopii zapasowej *mybackupstdaccount*.
2. W mybackupstdaccount, podwyższyć poziom migawki *mybackupstdpageblob_ss2* jako nowego obiektu blob strony podstawowej kopii zapasowej *mybackupstdpageblobrestored*.
3. Utworzenie migawki tej przywróconej kopii zapasowej stronicowych obiektów blob, nazywany *mybackupstdpageblobrestored_ss1*.
4. Skopiuj przywróconej stronicowych obiektów blob *mybackupstdpageblobrestored* z *mybackupstdaccount* do *mypremiumaccount* jako nowego dysku premium *mypremiumdiskrestored*.
5. Utwórz migawkę *mypremiumdiskrestored*o nazwie *mypremiumdiskrestored_ss1* dokonywania przyszłych przyrostowych kopii zapasowych.
6. Wskaż serii DS maszyny Wirtualnej przywróconej dysku *mypremiumdiskrestored* i odłączania stary *mypremiumdisk* z maszyny Wirtualnej.
7. Rozpocznij proces kopii zapasowej opisanych w poprzedniej sekcji przywróconej dysku *mypremiumdiskrestored*za pomocą *mybackupstdpageblobrestored* jako kopii zapasowej stronicowych obiektów blob.

![Przywracanie z migawki dysku](../articles/virtual-machines/windows/media/incremental-snapshots/storage-incremental-snapshots-2.png)

## <a name="next-steps"></a>Następne kroki
Skorzystaj z poniższych linków, aby dowiedzieć się więcej na temat tworzenia migawki obiektu blob i planowania infrastruktury kopii zapasowych maszyn wirtualnych.

* [Utworzenie migawki obiektu Blob](https://docs.microsoft.com/rest/api/storageservices/Creating-a-Snapshot-of-a-Blob)
* [Zaplanuj infrastrukturę kopii zapasowej maszyny Wirtualnej](../articles/backup/backup-azure-vms-introduction.md)

