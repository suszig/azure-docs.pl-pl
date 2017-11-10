Magazynu zoptymalizowane rozmiarów maszyn wirtualnych alokację dysku wysokiej przepływności oraz we/wy i idealnie nadają się do bazy danych Big Data, SQL i NoSQL. Ten artykuł zawiera informacje o liczbie Vcpu, dysków z danymi i karty sieciowe, a także wydajność przepływności i sieć magazynu dla każdego rozmiaru w tej metodzie grupowania. 

Maszyny wirtualne serii Ls oferują do 32 procesorów wirtualnych vCPU — procesor [Intel® Xeon® z rodziny E5 v3](http://www.intel.com/content/www/us/en/processors/xeon/xeon-e5-solutions.html). Wydajność procesora CPU dla serii Ls jest taka sama jak w przypadku maszyn wirtualnych serii G/GS. Na procesor wirtualny vCPU przypada pamięć o pojemności 8 GiB.  

## <a name="ls-series"></a>Seria Ls

ACU: 180–240
 
| Rozmiar          | Procesor wirtualny | Pamięć: GiB | Magazyn tymczasowy (SSD): GiB | Maks. liczba dysków danych | Maksymalna przepływność magazynu buforowanego i tymczasowego: liczba operacji we/wy na sekundę / MB/s (rozmiar pamięci podręcznej w GiB) | Maksymalna przepływność niebuforowanych dysków: liczba operacji we/wy na sekundę / MB/s | Maksymalna liczba kart sieciowych/oczekiwana wydajność sieci (Mb/s) | 
|---------------|-----------|-------------|--------------------------|----------------|-------------------------------------------------------------|-------------------------------------------|------------------------------| 
| Standardowa_L4s  | 4    | 32   | 678   | 8              | ND / ND (0)          | 5000 / 125                               | 2 / 4000       | 
| Standardowa_L8s  | 8    | 64   | 1,388 | 16             | ND / ND (0)          | 10 000 / 250                              | 4 / 8000  | 
| Standardowa_L16s | 16   | 128  | 2,807 | 32             | ND / ND (0)          | 20 000 / 500                              | 8 / 6000–16 000 &#8224; | 
| Standardowa_L32s* | 32 | 256  | 5,630 | 64             | ND / ND (0)          | 40 000 / 1000                            | 8 / 20 000 | 
 

Maksymalna przepływność dysków możliwa dla maszyn wirtualnych serii Ls może być ograniczona przez liczbę, rozmiar i rozłożenie wszelkich dołączonych dysków. Aby uzyskać szczegółowe informacje, zobacz [Premium Storage: High-performance storage for Azure virtual machine workloads](../articles/virtual-machines/windows/premium-storage.md) (Premium Storage: magazyn o wysokiej wydajności dla obciążeń maszyn wirtualnych platformy Azure). 

* Wystąpienie jest izolowane do sprzętu dedykowanego pojedynczemu klientowi.

