## <a name="what-is-blob-storage"></a>Co to jest magazyn obiektów Blob?
Azure Blob Storage to usługa do przechowywania dużych ilości danych obiektów niestrukturalnych, takich jak dane tekstowe lub binarne, do których można uzyskać dostęp z dowolnego miejsca na świecie za pośrednictwem protokołu HTTP lub HTTPS. Magazyn obiektów Blob może być użyty do udostępniania danych publicznie lub do przechowywania danych aplikacji prywatnie.

Najczęstsze zastosowania usługi Blob Storage obejmują:

* Obsługiwanie obrazów i dokumentów bezpośrednio do przeglądarki.
* Przechowywanie plików do dostępu rozproszonego.
* Przesyłanie strumieniowe audio i wideo.
* Zapisywanie danych dla kopii zapasowej i przywracania, odzyskiwania po awarii i archiwizowania.
* Zapisywanie danych do analizy przez lokalną lub usługi hostowanej platformy Azure.

## <a name="blob-service-concepts"></a>Pojęcia dotyczące usługi Blob
Usługa Blob obejmuje następujące składniki:

![Diagram architektury usługi Blob](./media/storage-blob-concepts-include/blob1.png)

* **Konto magazynu:** dostęp do usługi Azure Storage odbywa się za pośrednictwem konta magazynu. Konto magazynu może być **konta magazynu ogólnego przeznaczenia** lub **kontem magazynu obiektów Blob**, które jest przeznaczone do przechowywania obiektów lub obiekty BLOB. Aby uzyskać więcej informacji, zobacz [About Azure storage accounts](../articles/storage/common/storage-create-storage-account.md) (Informacje o kontach usługi Azure Storage).
* **Kontener:** kontener zawiera grupowanie zestawu obiektów blob. Wszystkie obiekty blob muszą być w kontenerze. Konto może zawierać nieograniczoną liczbę kontenerów. Kontener może przechowywać nieograniczoną liczbę obiektów blob. Pamiętaj, że wszystkie litery w nazwie kontenera muszą być małymi literami.
* **Obiekt blob:** plik dowolnego typu o dowolnym rozmiarze. Usługa Azure Storage udostępnia trzy typy obiektów blob: blokowe obiekty BLOB, uzupełnialnych obiektów blob i stronicowe.
  
    *Blokowe obiekty blob* idealnie nadają się do przechowywania tekstu lub plików binarnych, takich dokumenty czy pliki multimedialne. Pojedynczy blokowy obiekt blob może zawierać do 50 000 bloków do 100 MB każdy, których rozmiar całkowity może nieco przekraczać 4.75 TB (100 MB X 50 000). 

    *Uzupełnialne obiekty blob* są podobne do obiektów blokowych, ponieważ składają się z bloków, ale zoptymalizowane do operacji uzupełnialnych, więc przydatne w scenariuszach logowania. Pojedynczy uzupełniany obiekt blob może zawierać do 50 000 bloków do 4 MB każdy, których rozmiar całkowity może nieco przekraczać 195 GB (4 MB X 50 000).
  
    *Stronicowe obiekty blob* mogą mieć rozmiar maksymalnie 1 TB i są bardziej efektywne w przypadku operacji częstego odczytu/zapisu. Usługa Azure Virtual Machines używa stronicowych obiektów blob jako systemu operacyjnego i dysków z danymi.
  
    Aby uzyskać szczegółowe informacje o nazewnictwie kontenerów i obiektów blob, zobacz [nazewnictwa i odwołuje się do kontenerów, obiektów blob i metadanych](/rest/api/storageservices/Naming-and-Referencing-Containers--Blobs--and-Metadata).

