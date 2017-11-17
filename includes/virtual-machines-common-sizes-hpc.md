
Rozmiary A8–A11 i serii H są również nazywane *wystąpieniami intensywnie korzystającymi z mocy obliczeniowej*. Sprzęt, na którym działają te rozmiary maszyn wirtualnych, został zaprojektowany i zoptymalizowany pod kątem aplikacji intensywnie korzystających z mocy obliczeniowej i sieci, w tym aplikacji klastrów obliczeń o wysokiej wydajności, modelowania i symulacji. Maszyny wirtualne serii A8–A11 korzystają z procesorów Intel Xeon E5-2670 o częstotliwości 2,6 GHz, a seria H korzysta z procesorów Intel Xeon E5-2667 v3 o częstotliwości 3,2 GHz.  Ten artykuł zawiera informacje o liczbie Vcpu, dysków z danymi i karty sieciowe, a także przepustowości przepływności i sieć magazynu dla każdego rozmiaru w tej metodzie grupowania. 

Maszyny wirtualne serii H platformy Azure to następna generacja maszyn wirtualnych o wysokiej wydajności obliczeniowej, które idealnie sprawdzają się w przypadku najwyższych potrzeb obliczeniowych, na przykład w modelowaniu molekularnym i analizach obliczeniowych dynamiki płynów. Te maszyny wirtualne z 8 lub 16 procesorami wirtualnymi vCPU wykorzystują technologię procesora Intel Haswell E5-2667 V3 i wyposażone są w pamięć DDR4 oraz tymczasowy magazyn SSD. 

Seria H oferuje, obok znacznej mocy procesora CPU, różnorodne opcje dla sieci obsługujących technologię RDMA i niskie opóźnienia, korzystając z sieci InfiniBand o przepustowości FDR wraz z kilkoma konfiguracjami pamięci do obsługi obliczeń wymagających znacznego wykorzystania pamięci.



## <a name="h-series"></a>Seria H

ACU: 290–300

| Rozmiar | Procesor wirtualny | Pamięć: GiB | Magazyn tymczasowy (SSD): GiB | Maks. liczba dysków danych | Maksymalna przepływność dysków: liczba operacji we/wy na sekundę | Maksymalna liczba kart sieciowych |
| --- | --- | --- | --- | --- | --- | --- |
| Standardowa_H8 |8 |56 |1000 |32 |32 x 500 |2  |
| Standardowa_H16 |16 |112 |2000 |64 |64 x 500 |4 |
| Standardowa_H8m |8 |112 |1000 |32 |32 x 500 |2  |
| Standardowa_H16m |16 |224 |2000 |64 |64 x 500 |4  |
| Standardowa_H16r* |16 |112 |2000 |64 |64 x 500 |4  |
| Standardowa_H16mr* |16 |224 |2000 |64 |64 x 500 |4 |

*W przypadku aplikacji MPI dedykowana sieć zaplecza RDMA jest zapewniana przez sieć FDR InfiniBand, która gwarantuje ultraniskie opóźnienia i wysoką przepustowość.

<br>



## <a name="a-series---compute-intensive-instances"></a>Seria A — wystąpienia intensywnie korzystające z mocy obliczeniowej

ACU: 225

| Rozmiar | Procesor wirtualny | Pamięć: GiB | Magazyn tymczasowy (HDD): GiB | Maks. liczba dysków danych | Maksymalna przepływność dysków danych: liczba operacji we/wy na sekundę | Maksymalna liczba kart sieciowych|
| --- | --- | --- | --- | --- | --- | --- |
| Standardowa_A8* |8 |56 |382 |32 |32 x 500 |2 |
| Standardowa_A9* |16 |112 |382 |64 |64 x 500 |4 |
| Standardowa_A10 |8 |56 |382 |32 |32 x 500 |2  |
| Standardowa_A11 |16 |112 |382 |64 |64 x 500 |4 |

*W przypadku aplikacji MPI dedykowana sieć zaplecza RDMA jest zapewniana przez sieć FDR InfiniBand, która gwarantuje ultraniskie opóźnienia i wysoką przepustowość.

<br>



