Magazynu zoptymalizowane rozmiarów maszyn wirtualnych alokację dysku wysokiej przepływności oraz we/wy i idealnie nadają się do bazy danych Big Data, SQL i NoSQL. Ten artykuł zawiera informacje o liczbie Vcpu, dysków z danymi i karty sieciowe, a także przepustowości przepływności i sieć magazynu dla każdego rozmiaru w tej metodzie grupowania. 

Maszyny wirtualne serii Ls oferują do 32 procesorów wirtualnych vCPU — procesor [Intel® Xeon® z rodziny E5 v3](http://www.intel.com/content/www/us/en/processors/xeon/xeon-e5-solutions.html). Wydajność procesora CPU dla serii Ls jest taka sama jak w przypadku maszyn wirtualnych serii G/GS. Na procesor wirtualny vCPU przypada pamięć o pojemności 8 GiB.  

## <a name="ls-series"></a>Seria Ls

ACU: 180–240
 
| Rozmiar          | Procesor wirtualny | Pamięć: GiB | Magazyn tymczasowy (SSD): GiB | Maks. liczba dysków danych | Maksymalna przepustowość magazynu tymczasowego: IOPS / MB/s | Maksymalna przepływność niebuforowanych dysków: liczba operacji we/wy na sekundę / MB/s | Maksymalna liczba kart sieciowych / oczekiwano przepustowości sieci (MB/s) | 
|---------------|-----------|-------------|--------------------------|----------------|-------------------------------------------------------------|-------------------------------------------|------------------------------| 
| Standardowa_L4s   | 4    | 32   | 678   | 16    | 20,000 / 200   | 10 000 / 250        | 2 / 4,000  | 
| Standardowa_L8s   | 8    | 64   | 1,388 | 32   | 40,000 / 400   | 20 000 / 500       | 4 / 8,000  | 
| Standardowa_L16s  | 16   | 128  | 2,807 | 64   | 80,000 / 800   | 40 000 / 1000       | 8 / 6,000 - 16,000 &#8224; | 
| Standardowa_L32s* | 32   | 256  | 5,630 | 64   | 160,000 / 1,600   | 80 000 / 2000     | 8 / 20,000 | 
 

Maksymalna przepływność dysków możliwa dla maszyn wirtualnych serii Ls może być ograniczona przez liczbę, rozmiar i rozłożenie wszelkich dołączonych dysków. Aby uzyskać szczegółowe informacje, zobacz [Premium Storage: High-performance storage for Azure virtual machine workloads](../articles/virtual-machines/windows/premium-storage.md) (Premium Storage: magazyn o wysokiej wydajności dla obciążeń maszyn wirtualnych platformy Azure). 

* Wystąpienie jest izolowane do sprzętu dedykowanego pojedynczemu klientowi.

