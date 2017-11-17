
## <a name="size-table-definitions"></a>Definicje tabel rozmiaru

- Pojemność magazynu jest podawana w jednostkach GiB (1024^3 bajtów). Podczas porównywania dysków mierzonych w GB (1000^3 bajtów) z dyskami mierzonymi w GiB (1024^3 bajtów) należy pamiętać, że pojemność podawana w GiB może wydawać się mniejsza. Na przykład 1023 GiB = 1098,4 GB.
- Przepływność dysku mierzona jest jako liczba operacji wejścia/wyjścia na sekundę i MB/s, gdzie 1 MB/s = 10^6 bajtów/s.
- Dyski danych mogą działać w trybie buforowanym lub niebuforowanym. Dla pracy dysku danych w trybie buforowanym tryb pamięci podręcznej hosta jest ustawiony na wartość **ReadOnly** lub **ReadWrite**.  Dla pracy dysku danych bez buforowania tryb pamięci podręcznej hosta jest ustawiony na wartość **None**.
-   Jeśli chcesz uzyskać najlepszą wydajność dla maszyn wirtualnych, należy ograniczyć liczbę dysków z danymi na 2 dyski na vCPU.
- **Oczekiwano przepustowość sieci** jest agregowana maksymalną [przepustowości zaalokowany typu maszyny Wirtualnej](../articles/virtual-network/virtual-machine-network-throughput.md) dla wszystkich kart dla wszystkich miejsc docelowych. Górne limity nie są gwarantowane, ale mają stanowić wskazówkę do wybierania właściwego typu maszyny wirtualnej dla planowanej aplikacji. Rzeczywista wydajność sieci będzie zależeć od wielu czynników, takich jak przeciążenie sieci, obciążenie aplikacji oraz ustawienia sieci. Aby uzyskać informacje na temat optymalizowania przepływności sieci, zobacz [Optimizing network throughput for Windows and Linux (Optymalizowanie przepływności sieci dla systemów Windows i Linux)](../articles/virtual-network/virtual-network-optimize-network-bandwidth.md). Aby uzyskać oczekiwaną wydajność sieci w systemie Linux lub Windows, konieczne może być wybranie konkretnej wersji maszyny wirtualnej lub jej zoptymalizowanie. Aby uzyskać więcej informacji, zobacz [How to reliably test for virtual machine throughput (Jak wiarygodnie przetestować przepływność maszyny wirtualnej)](../articles/virtual-network/virtual-network-bandwidth-testing.md).

- &#8224; W nadchodzącym wydaniu wydajność 16 procesów wirtualnych vCPU będzie stale osiągać górny limit.


