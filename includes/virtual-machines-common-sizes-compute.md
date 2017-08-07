<!-- F-series, Fs-series* -->

Seria F jest oparta na procesorze Intel Xeon® E5-2673 v3 (Haswell) 2,4 GHz, który dzięki technologii Intel Turbo Boost 2.0 może osiągnąć prędkość zegara do 3,1 GHz. Wydajność procesora CPU jest taka sama jak w przypadku maszyn wirtualnych serii Dv2.  Oferując niższą cenę za godzinę, seria F zapewnia najlepszy w portfolio platformy Azure stosunek ceny do wydajności, określany na podstawie liczby jednostek ACU (Azure Compute Unit) na procesor wirtualny vCPU. 

Maszyny wirtualne z serii F są doskonałym wyborem dla obciążeń wymagających szybszych procesorów CPU, ale nie potrzebujących tak dużej ilości pamięci lub magazynu tymczasowego na każdy procesor wirtualny vCPU.  Z wartości serii F skorzystają obciążenia takie jak analizy, serwery gier, serwery sieci Web i przetwarzanie wsadowe.

Seria Fs ma magazyn w warstwie Premium i wszystkie zalety serii F.

## <a name="fs-series"></a>Seria Fs*

ACU: 210–250

| Rozmiar | Procesor wirtualny | Pamięć: GiB | Magazyn tymczasowy (SSD): GiB | Maks. liczba dysków danych | Maksymalna przepływność magazynu buforowanego i tymczasowego: liczba operacji we/wy na sekundę / MB/s (rozmiar pamięci podręcznej w GiB) | Maksymalna przepływność niebuforowanych dysków: liczba operacji we/wy na sekundę / MB/s | Maksymalna liczba kart sieciowych/oczekiwana wydajność sieci (Mb/s) |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Standardowa_F1s |1 |2 |4 |2 |4000 / 32 (12) |3200 / 48 |2 / 750 |
| Standardowa_F2s |2 |4 |8 |4 |8000 / 64 (24) |6400 / 96 |2 / 1500 |
| Standardowa_F4s |4 |8 |16 |8 |16 000 / 128 (48) |12 800 / 192 |4 / 3000 |
| Standardowa_F8s |8 |16 |32 |16 |32 000 / 256 (96) |25 600 / 384 |8 / 6000 |
| Standardowa_F16s |16 |32 |64 |32 |64 000 / 512 (192) |51 200 / 768 |8 / 6000-12000 &#8224; |

MB/s = 10^6 bajtów na sekundę, GiB = 1024^3 bajtów.

* Maksymalna przepływność dysków (liczba operacji we/wy na sekundę lub MB/s) możliwa dla maszyny wirtualnej serii Fs może być ograniczona przez liczbę, rozmiar i rozkładanie dołączonych dysków.  Aby uzyskać szczegółowe informacje, zobacz [Premium Storage: High-performance storage for Azure virtual machine workloads](../articles/storage/storage-premium-storage.md) (Premium Storage: magazyn o wysokiej wydajności dla obciążeń maszyn wirtualnych platformy Azure).


<br>

## <a name="f-series"></a>Seria F

ACU: 210–250

| Rozmiar         | Procesor wirtualny | Pamięć: GiB | Magazyn tymczasowy (SSD): GiB | Maksymalna przepływność magazynu tymczasowego: operacje we/wy na sek. / odczyt MB/s / zapis MB/s | Maksymalna liczba dysków danych / przepływność: liczba operacji we/wy na sekundę | Maksymalna liczba kart sieciowych/oczekiwana wydajność sieci (Mb/s) |
|--------------|-----------|-------------|----------------|----------------------------------------------------------|-----------------------------------|------------------------------|
| Standardowa_F1  | 1         | 2           | 16             | 3000 / 46 / 23                                           | 2 / 2 x 500                         | 2 / 750                 |
| Standardowa_F2  | 2         | 4           | 32             | 6000 / 93 / 46                                           | 4 / 4 x 500                         | 2 / 1500                     |
| Standardowa_F4  | 4         | 8           | 64             | 12000 / 187 / 93                                         | 8 / 8 x 500                         | 4 / 3000                     |
| Standardowa_F8  | 8         | 16          | 128            | 24000 / 375 / 187                                        | 16 / 16 x 500                       | 8 / 6000                     |
| Standardowa_F16 | 16        | 32          | 256            | 48000 / 750 / 375                                        | 32 / 32 x 500                       | 8 / 6000–12 000 &#8224;           |


<br>


