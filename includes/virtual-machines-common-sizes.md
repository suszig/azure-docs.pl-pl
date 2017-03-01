


Na platformie Azure do wyboru jest wiele standardowych rozmiarów maszyn wirtualnych. Uwagi dotyczące niektórych z tych rozmiarów:

* Maszyny wirtualne serii D są zaprojektowane do uruchamiania aplikacji wymagających większej mocy obliczeniowej i wydajności dysków tymczasowych. Maszyny wirtualne serii D zapewniają szybsze procesory, większą ilość pamięci na rdzeń i dyski półprzewodnikowe (SSD) dla dysków tymczasowych. Szczegółowe informacje zawiera ogłoszenie [New D-Series Virtual Machine Sizes](https://azure.microsoft.com/blog/2014/09/22/new-d-series-virtual-machine-sizes/) (Nowe rozmiary maszyn wirtualnych serii D) w blogu platformy Azure.
* Seria Dv2, kontynuacja oryginalnej serii D, jest wyposażona w procesor CPU o większych możliwościach. Procesor CPU serii Dv2 jest o około 35% szybszy niż procesor CPU serii D. Seria Dv2 jest oparta na procesorze najnowszej generacji Intel Xeon® E5-2673 v3 (Haswell) z zegarem 2,4 GHz, który dzięki technologii Intel Turbo Boost 2.0 może osiągnąć częstotliwość 3,1 GHz. Konfiguracje pamięci i dysków serii Dv2 są takie same jak w przypadku serii D.
* Seria F jest oparta na procesorze Intel Xeon® E5-2673 v3 (Haswell) 2,4 GHz, który dzięki technologii Intel Turbo Boost 2.0 może osiągnąć prędkość zegara do 3,1 GHz. Wydajność procesora CPU jest taka sama jak w przypadku maszyn wirtualnych serii Dv2.  Oferując niższą cenę za godzinę, seria F zapewnia najlepszy w portfolio platformy Azure stosunek ceny do wydajności, określany na podstawie liczby jednostek ACU (Azure Compute Unit) na rdzeń. 
  
    Seria F wprowadza również nowy standard nazewnictwa rozmiarów maszyn wirtualnych dla platformy Azure. W przypadku tej serii oraz rozmiarów maszyn wirtualnych wydawanych w przyszłości liczba następująca po literze nazwy rodziny będzie odpowiadała liczbie rdzeni procesora CPU. Dodatkowe funkcje, takie jak optymalizacja pod kątem usługi Premium Storage, będą wskazywane przez litery następujące po wartości oznaczającej liczbę rdzeni procesora CPU. Ten format nazewnictwa będzie używany na potrzeby rozmiarów maszyn wirtualnych wydawanych w przyszłości, ale nie spowoduje wstecznej zmiany nazw żadnych dotychczas istniejących rozmiarów maszyn wirtualnych.
* Maszyny wirtualne z serii G oferują największą ilość pamięci i są uruchamiane na hostach z procesorami z rodziny Intel Xeon E5 V3.
* Maszyny wirtualne z serii DS, DSv2, Fs i GS mogą korzystać z usługi Premium Storage, która zapewnia magazyn o wysokiej wydajności i niskich opóźnieniach dla obciążeń intensywnie korzystających z operacji we/wy. Te maszyny wirtualne używają dysków półprzewodnikowych (SSD) do hostowania dysków maszyny wirtualnej, a także udostępniają pamięć podręczną na lokalnym dysku SSD. Usługa Premium Storage jest dostępna w określonych regionach. Aby uzyskać szczegółowe informacje, zobacz [Premium Storage: High-performance storage for Azure virtual machine workloads](../articles/storage/storage-premium-storage.md) (Premium Storage: magazyn o wysokiej wydajności dla obciążeń maszyn wirtualnych platformy Azure).
*   Maszyny wirtualne z serii A i Av2 można wdrażać na różnych typach sprzętu i procesorach. Rozmiar jest ograniczany w zależności od sprzętu, aby zapewnić spójną wydajność procesora dla uruchomionego wystąpienia niezależnie od sprzętu, na którym jest ono wdrożone. Aby określić sprzęt fizyczny, na którym jest wdrażany dany rozmiar, utwórz zapytanie o sprzęt wirtualny z poziomu maszyny wirtualnej.
* Rozmiar A0 jest nadmiernie subskrybowany na sprzęcie fizycznym. Tylko w przypadku tego konkretnego rozmiaru inne wdrożenia klienta mogą mieć wpływ na wydajność uruchomionego obciążenia. Wydajność względna jest przedstawiona poniżej jako oczekiwana linia bazowa, podlegająca przybliżonej zmienności w granicach 15 procent.

Rozmiar maszyny wirtualnej ma wpływ na ceny. Rozmiar wpływa również na wydajność przetwarzania oraz pojemność pamięci i magazynu maszyny wirtualnej. Koszty magazynowania są obliczane osobno na podstawie wykorzystanych stron na koncie magazynu. Aby uzyskać szczegółowe informacje, zobacz stronę [Virtual Machines — szczegóły cennika](https://azure.microsoft.com/pricing/details/virtual-machines/) i [Cennik usługi Azure Storage](https://azure.microsoft.com/pricing/details/storage/). 

W podjęciu decyzji o rozmiarze mogą pomóc następujące informacje:

* Rozmiary A8–A11 i serii H są również nazywane *wystąpieniami intensywnie korzystającymi z mocy obliczeniowej*. Sprzęt, na którym działają te rozmiary maszyn wirtualnych, został zaprojektowany i zoptymalizowany pod kątem aplikacji intensywnie korzystających z mocy obliczeniowej i sieci, w tym aplikacji klastrów obliczeń o wysokiej wydajności, modelowania i symulacji. Maszyny wirtualne serii A8–A11 korzystają z procesorów Intel Xeon E5-2670 o częstotliwości 2,6 GHz, a seria H korzysta z procesorów Intel Xeon E5-2667 v3 o częstotliwości 3,2 GHz. Szczegółowe informacje i uwagi dotyczące korzystania z tych rozmiarów zawiera artykuł [About the H-series and compute-intensive A-series VMs](../articles/virtual-machines/virtual-machines-windows-a8-a9-a10-a11-specs.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (Informacje o maszynach wirtualnych serii H i intensywnie korzystających z obliczeń maszynach wirtualnych serii A). 
* Serie Dv2, D, G i ich odpowiedniki DS/GS są idealne dla aplikacji wymagających szybszych procesorów CPU, lepszej wydajności dysków lokalnych lub większych ilości pamięci.  Oferują one kombinację opcji o dużych możliwościach dla wielu aplikacji klasy korporacyjnej.
* Maszyny wirtualne z serii F są doskonałym wyborem dla obciążeń wymagających szybszych procesorów CPU, ale nie potrzebujących tak dużej ilości pamięci lub lokalnego dysku SSD na każdy rdzeń procesora CPU.  Z wartości serii F skorzystają obciążenia takie jak analizy, serwery gier, serwery sieci Web i przetwarzanie wsadowe.
* Niektóre hosty fizyczne w centrach danych platformy Azure mogą nie obsługiwać większych rozmiarów maszyn wirtualnych, takich jak A5–A11. W rezultacie podczas zmiany rozmiaru istniejącej maszyny wirtualnej na nowy rozmiar, tworzenia nowej maszyny wirtualnej w sieci wirtualnej utworzonej przed 16 kwietnia 2013 lub dodawania nowej maszyny wirtualnej do istniejącej usługi w chmurze może zostać wyświetlony komunikat o błędzie **Nie można skonfigurować maszyny wirtualnej <machine name>** lub **Nie można utworzyć maszyny wirtualnej <machine name>**. Zobacz temat [Błąd: „Nie można skonfigurować maszyny wirtualnej”](https://social.msdn.microsoft.com/Forums/9693f56c-fcd3-4d42-850e-5e3b56c7d6be/error-failed-to-configure-virtual-machine-with-a5-a6-or-a7-vm-size?forum=WAVirtualMachinesforWindows) na forum pomocy technicznej, aby uzyskać informacje o obejściach dla poszczególnych scenariuszy wdrażania.  
* Subskrypcja może również ograniczać liczbę rdzeni, które można wdrożyć w rodzinach o określonym rozmiarze. Aby zwiększyć limit przydziału, skontaktuj się z pomocą techniczną platformy Azure.

## <a name="performance-considerations"></a>Zagadnienia dotyczące wydajności
Aby umożliwić porównywanie wydajności obliczeniowej (procesora CPU) jednostek SKU platformy Azure, stworzyliśmy koncepcję jednostki obliczeniowej platformy Azure (Azure Compute Unit, ACU). Ten parametr pomoże łatwo zidentyfikować jednostkę SKU, która najprawdopodobniej spełni określone potrzeby związane z wydajnością.  Jednostka ACU jest obecnie standaryzowana na małej maszynie wirtualnej (Standardowa_A1) jako równa 100, a wszystkie pozostałe jednostki SKU reprezentują w przybliżeniu, o ile szybciej dana jednostka SKU może uruchomić standardowy test porównawczy. 

> [!IMPORTANT]
> Wartość ACU jest tylko wskazówką.  Wyniki dla konkretnego obciążenia mogą się różnić. 
> 
> 

<br>

| Rodzina SKU | ACU/rdzeń |
| --- | --- |
| [A0](#a-series) |50 |
| [A1–A4](#a-series) |100 |
| [A5–A7](#a-series) |100 |
| [A1_v2–A8_v2](#av2-series) |100 |
| [A2m_v2–A8m_v2](#av2-series) |100 |
| [A8–A11](#a-series) |225* |
| [D1–D14](#d-series) |160 |
| [D1_v2–D15_v2](#dv2-series) |210 - 250* |
| [DS1–DS14](#ds-series) |160 |
| [DS1_v2–DS15_v2](#dsv2-series) |210-250* |
| [F1–F16](#f-series) |210-250* |
| [F1s–F16s](#fs-series) |210-250* |
| [G1–G5](#g-series) |180 - 240* |
| [GS1–GS5](#gs-series) |180 - 240* |
| [H](#h-series) |290 - 300* |
| [L4s–L32s](#l-series) |180 - 240* |

Jednostki ACU oznaczone gwiazdką (*) wykorzystują technologię Intel® Turbo w celu zwiększenia częstotliwości zegara procesora CPU i zapewniania większej wydajności.  Skala zwiększenia wydajności może się różnić w zależności od rozmiaru maszyny wirtualnej, obciążenia i innych obciążeń uruchomionych na tym samym hoście.

## <a name="size-tables"></a>Tabele rozmiarów
W poniższych tabelach przedstawiono rozmiary maszyn wirtualnych i możliwości, jakie oferują.

* Pojemność magazynu jest podawana w jednostkach GiB (1024^3 bajtów). Podczas porównywania dysków mierzonych w GB (1000^3 bajtów) z dyskami mierzonymi w GiB (1024^3 bajtów) należy pamiętać, że pojemność podawana w GiB może wydawać się mniejsza. Na przykład 1023 GiB = 1098,4 GB.
* Przepływność dysku mierzona jest jako liczba operacji wejścia/wyjścia na sekundę i MB/s, gdzie 1 MB/s = 10^6 bajtów/s.
* Dyski danych mogą działać w trybie buforowanym lub niebuforowanym.  Dla pracy dysku danych w trybie buforowanym tryb pamięci podręcznej hosta jest ustawiony na wartość **ReadOnly** lub **ReadWrite**.  Dla pracy dysku danych bez buforowania tryb pamięci podręcznej hosta jest ustawiony na wartość **None**.
* Maksymalna przepustowość sieci jest maksymalną zagregowaną przepustowością przydzieloną i przypisaną dla typu maszyny wirtualnej. Maksymalna przepustowość stanowi wskazówkę umożliwiającą wybranie odpowiedniego typu maszyny wirtualnej, który zapewni dostępność odpowiedniej pojemności sieci. Wraz ze wzrostem maksymalnej przepustowości (niska, średnia, wysoka, bardzo wysoka) zwiększa się również przepływność. Rzeczywista wydajność sieci będzie zależeć od wielu czynników, takich jak obciążenia sieciowe i obciążenia aplikacji oraz ustawienia sieciowe aplikacji.

## <a name="a-series"></a>Seria A
| Rozmiar | Rdzenie procesora CPU | Pamięć: GiB | Lokalny dysk twardy: GiB | Maks. liczba dysków danych | Maksymalna przepływność dysków danych: liczba operacji we/wy na sekundę | Maksymalna liczba kart sieciowych / przepustowość sieci |
| --- | --- | --- | --- | --- | --- | --- |
| Standardowa_A0 |1 |0,768 |20 |1 |1 x&500; |1 / niska |
| Standardowa_A1 |1 |1,75 |70 |2 |2 x&500; |1 / średnia |
| Standardowa_A2 |2 |3,5 |135 |4 |4 x&500; |1 / średnia |
| Standardowa_A3 |4 |7 |285 |8 |8 x&500; |2 / wysoka |
| Standardowa_A4 |8 |14 |605 |16 |16 x&500; |4 / wysoka |
| Standardowa_A5 |2 |14 |135 |4 |4 x&500; |1 / średnia |
| Standardowa_A6 |4 |28 |285 |8 |8 x&500; |2 / wysoka |
| Standardowa_A7 |8 |56 |605 |16 |16 x&500; |4 / wysoka |

<br>

## <a name="a-series---compute-intensive-instances"></a>Seria A — wystąpienia intensywnie korzystające z mocy obliczeniowej
Informacje i uwagi dotyczące korzystania z tych rozmiarów zawiera artykuł [About the H-series and compute-intensive A-series VMs](../articles/virtual-machines/virtual-machines-windows-a8-a9-a10-a11-specs.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (Informacje o maszynach wirtualnych serii H i intensywnie korzystających z obliczeń maszynach wirtualnych serii A).

| Rozmiar | Rdzenie procesora CPU | Pamięć: GiB | Lokalny dysk twardy: GiB | Maks. liczba dysków danych | Maksymalna przepływność dysków danych: liczba operacji we/wy na sekundę | Maksymalna liczba kart sieciowych / przepustowość sieci |
| --- | --- | --- | --- | --- | --- | --- |
| Standardowa_A8* |8 |56 |382 |16 |16 x&500; |2 / wysoka |
| Standardowa_A9* |16 |112 |382 |16 |16 x&500; |4 / bardzo wysoka |
| Standardowa_A10 |8 |56 |382 |16 |16 x&500; |2 / wysoka |
| Standardowa_A11 |16 |112 |382 |16 |16 x&500; |4 / bardzo wysoka |

* Obsługa technologii RDMA

<br>

## <a name="av2-series"></a>Seria Av2

| Rozmiar        | Rdzenie procesora CPU | Pamięć: GiB | Lokalne dyski SSD: GiB | Maks. liczba dysków danych | Maksymalna przepływność dysków danych: liczba operacji we/wy na sekundę | Maksymalna liczba kart sieciowych / przepustowość sieci |
|-------------|-----------|--------------|-----------------------|----------------|--------------------|-----------------------|
| Standardowa_A1_v2 | 1         | 2            | 10                   | 2              | 2 x&500;              | 1 / średnia              |
| Standardowa_A2_v2 | 2         | 4            | 20                   | 4              | 4 x&500;              | 2 / średnia              |
| Standardowa_A4_v2 | 4         | 8            | 40                   | 8              | 8 x&500;              | 4 / wysoka                  |
| Standardowa_A8_v2 | 8         | 16           | 80                   | 16             | 16 x&500;             | 8 / wysoka                  |
| Standardowa_A2m_v2 | 2        | 16           | 20                   | 4              | 4 x&500;              | 2 / średnia              |
| Standardowa_A4m_v2 | 4        | 32           | 40                   | 8              | 8 x&500;              | 4 / wysoka                  |
| Standardowa_A8m_v2 | 8        | 64           | 80                   | 16             | 16 x&500;             | 8 / wysoka                  |


## <a name="d-series"></a>Seria D
| Rozmiar | Rdzenie procesora CPU | Pamięć: GiB | Lokalne dyski SSD: GiB | Maks. liczba dysków danych | Maksymalna przepływność dysków danych: liczba operacji we/wy na sekundę | Maksymalna liczba kart sieciowych / przepustowość sieci |
| --- | --- | --- | --- | --- | --- | --- |
| Standardowa_D1 |1 |3,5 |50 |2 |2 x&500; |1 / średnia |
| Standardowa_D2 |2 |7 |100 |4 |4 x&500; |2 / wysoka |
| Standardowa_D3 |4 |14 |200 |8 |8 x&500; |4 / wysoka |
| Standardowa_D4 |8 |28 |400 |16 |16 x&500; |8 / wysoka |
| Standardowa_D11 |2 |14 |100 |4 |4 x&500; |2 / wysoka |
| Standardowa_D12 |4 |28 |200 |8 |8 x&500; |4 / wysoka |
| Standardowa_D13 |8 |56 |400 |16 |16 x&500; |8 / wysoka |
| Standardowa_D14 |16 |112 |800 |32 |32 x&500; |8 / bardzo wysoka |

<br>

## <a name="dv2-series"></a>Seria Dv2
| Rozmiar | Rdzenie procesora CPU | Pamięć: GiB | Lokalne dyski SSD: GiB | Maks. liczba dysków danych | Maksymalna przepływność dysków danych: liczba operacji we/wy na sekundę | Maksymalna liczba kart sieciowych / przepustowość sieci |
| --- | --- | --- | --- | --- | --- | --- |
| Standardowa_D1_v2 |1 |3,5 |50 |2 |2 x&500; |1 / średnia |
| Standardowa_D2_v2 |2 |7 |100 |4 |4 x&500; |2 / wysoka |
| Standardowa_D3_v2 |4 |14 |200 |8 |8 x&500; |4 / wysoka |
| Standardowa_D4_v2 |8 |28 |400 |16 |16 x&500; |8 / wysoka |
| Standardowa_D5_v2 |16 |56 |800 |32 |32 x&500; |8 / ekstremalnie wysoka |
| Standardowa_D11_v2 |2 |14 |100 |4 |4 x&500; |2 / wysoka |
| Standardowa_D12_v2 |4 |28 |200 |8 |8 x&500; |4 / wysoka |
| Standardowa_D13_v2 |8 |56 |400 |16 |16 x&500; |8 / wysoka |
| Standardowa_D14_v2 |16 |112 |800 |32 |32 x&500; |8 / ekstremalnie wysoka |
| Standardowa_D15_v2** |20 |140 |1000 |40 |40 x&500; |8 / ekstremalnie wysoka* |

* W niektórych regionach jest dostępna przyspieszona sieć dla rozmiaru Standardowa_D15_v2. Aby uzyskać więcej informacji o użyciu i dostępności, zobacz artykuły [Accelerated Networking is in Preview](https://azure.microsoft.com/updates/accelerated-networking-in-preview/) (Przyspieszona sieć jest dostępna w wersji zapoznawczej) i [Accelerated Networking for a virtual machine](../articles/virtual-network/virtual-network-accelerated-networking-powershell.md) (Przyspieszona sieć dla maszyny wirtualnej).

** Wystąpienie jest izolowane do sprzętu dedykowanego pojedynczemu klientowi.

<br>

## <a name="ds-series"></a>Seria DS*
| Rozmiar | Rdzenie procesora CPU | Pamięć: GiB | Lokalne dyski SSD: GiB | Maks. liczba dysków danych | Maksymalna przepływność buforowanych dysków: liczba operacji we/wy na sekundę / MB/s (rozmiar pamięci podręcznej w GiB) | Maksymalna przepływność niebuforowanych dysków: liczba operacji we/wy na sekundę / MB/s | Maksymalna liczba kart sieciowych / przepustowość sieci |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Standardowa_DS1 |1 |3,5 |7 |2 |4000 / 32 (43) |3200 / 32 |1 / średnia |
| Standardowa_DS2 |2 |7 |14 |4 |8000 / 64 (86) |6400 / 64 |2 / wysoka |
| Standardowa_DS3 |4 |14 |28 |8 |16 000 / 128 (172) |12 800 / 128 |4 / wysoka |
| Standardowa_DS4 |8 |28 |56 |16 |32 000 / 256 (344) |25 600 / 256 |8 / wysoka |
| Standardowa_DS11 |2 |14 |28 |4 |8000 / 64 (72) |6400 / 64 |2 / wysoka |
| Standardowa_DS12 |4 |28 |56 |8 |16 000 / 128 (144) |12 800 / 128 |4 / wysoka |
| Standardowa_DS13 |8 |56 |112 |16 |32 000 / 256 (288) |25 600 / 256 |8 / wysoka |
| Standardowa_DS14 |16 |112 |224 |32 |64 000 / 512 (576) |51 200 / 512 |8 / bardzo wysoka |

MB/s = 10^6 bajtów na sekundę, GiB = 1024^3 bajtów.

* Maksymalna przepływność dysków (liczba operacji we/wy na sekundę lub MB/s) możliwa dla maszyny wirtualnej serii DS może być ograniczona przez liczbę, rozmiar i rozkładanie dołączonych dysków.  Aby uzyskać szczegółowe informacje, zobacz [Premium Storage: High-performance storage for Azure virtual machine workloads](../articles/storage/storage-premium-storage.md) (Premium Storage: magazyn o wysokiej wydajności dla obciążeń maszyn wirtualnych platformy Azure).

<br>

## <a name="dsv2-series"></a>Seria DSv2*
| Rozmiar | Rdzenie procesora CPU | Pamięć: GiB | Lokalne dyski SSD: GiB | Maks. liczba dysków danych | Maksymalna przepływność buforowanych dysków: liczba operacji we/wy na sekundę / MB/s (rozmiar pamięci podręcznej w GiB) | Maksymalna przepływność niebuforowanych dysków: liczba operacji we/wy na sekundę / MB/s | Maksymalna liczba kart sieciowych / przepustowość sieci |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Standardowa_DS1_v2 |1 |3,5 |7 |2 |4000 / 32 (43) |3200 / 48 |1 / średnia |
| Standardowa_DS2_v2 |2 |7 |14 |4 |8000 / 64 (86) |6400 / 96 |2 / wysoka |
| Standardowa_DS3_v2 |4 |14 |28 |8 |16 000 / 128 (172) |12 800 / 192 |4 / wysoka |
| Standardowa_DS4_v2 |8 |28 |56 |16 |32 000 / 256 (344) |25 600 / 384 |8 / wysoka |
| Standardowa_DS5_v2 |16 |56 |112 |32 |64 000 / 512 (688) |51 200 / 768 |8 / ekstremalnie wysoka |
| Standardowa_DS11_v2 |2 |14 |28 |4 |8000 / 64 (72) |6400 / 96 |2 / wysoka |
| Standardowa_DS12_v2 |4 |28 |56 |8 |16 000 / 128 (144) |12 800 / 192 |4 / wysoka |
| Standardowa_DS13_v2 |8 |56 |112 |16 |32 000 / 256 (288) |25 600 / 384 |8 / wysoka |
| Standardowa_DS14_v2 |16 |112 |224 |32 |64 000 / 512 (576) |51 200 / 768 |8 / ekstremalnie wysoka |
| Standardowa_DS15_v2*** |20 |140 |280 |40 |80 000 / 640 (720) |64 000 / 960 |8 / ekstremalnie wysoka** |

MB/s = 10^6 bajtów na sekundę, GiB = 1024^3 bajtów.

* Maksymalna przepływność dysków (liczba operacji we/wy na sekundę lub MB/s) możliwa dla maszyny wirtualnej serii DSv2 może być ograniczona przez liczbę, rozmiar i rozkładanie dołączonych dysków.  Aby uzyskać szczegółowe informacje, zobacz [Premium Storage: High-performance storage for Azure virtual machine workloads](../articles/storage/storage-premium-storage.md) (Premium Storage: magazyn o wysokiej wydajności dla obciążeń maszyn wirtualnych platformy Azure).

** W niektórych regionach jest dostępna przyspieszona sieć dla rozmiaru Standardowa_DS15_v2. Aby uzyskać więcej informacji o użyciu i dostępności, zobacz artykuły [Accelerated Networking is in Preview](https://azure.microsoft.com/updates/accelerated-networking-in-preview/) (Przyspieszona sieć jest dostępna w wersji zapoznawczej) i [Accelerated Networking for a virtual machine](../articles/virtual-network/virtual-network-accelerated-networking-powershell.md) (Przyspieszona sieć dla maszyny wirtualnej).

*** Wystąpienie jest izolowane do sprzętu dedykowanego pojedynczemu klientowi.
<br>

## <a name="f-series"></a>Seria F
| Rozmiar | Rdzenie procesora CPU | Pamięć: GiB | Lokalne dyski SSD: GiB | Maks. liczba dysków danych | Maksymalna przepływność dysków: liczba operacji we/wy na sekundę | Maksymalna liczba kart sieciowych / przepustowość sieci |
| --- | --- | --- | --- | --- | --- | --- |
| Standardowa_F1 |1 |2 |16 |2 |2 x&500; |1 / średnia |
| Standardowa_F2 |2 |4 |32 |4 |4 x&500; |2 / wysoka |
| Standardowa_F4 |4 |8 |64 |8 |8 x&500; |4 / wysoka |
| Standardowa_F8 |8 |16 |128 |16 |16 x&500; |8 / wysoka |
| Standardowa_F16 |16 |32 |256 |32 |32 x&500; |8 / ekstremalnie wysoka |

<br>

## <a name="fs-series"></a>Seria Fs*
| Rozmiar | Rdzenie procesora CPU | Pamięć: GiB | Lokalne dyski SSD: GiB | Maks. liczba dysków danych | Maksymalna przepływność buforowanych dysków: liczba operacji we/wy na sekundę / MB/s (rozmiar pamięci podręcznej w GiB) | Maksymalna przepływność niebuforowanych dysków: liczba operacji we/wy na sekundę / MB/s | Maksymalna liczba kart sieciowych / przepustowość sieci |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Standardowa_F1s |1 |2 |4 |2 |4000 / 32 (12) |3200 / 48 |1 / średnia |
| Standardowa_F2s |2 |4 |8 |4 |8000 / 64 (24) |6400 / 96 |2 / wysoka |
| Standardowa_F4s |4 |8 |16 |8 |16 000 / 128 (48) |12 800 / 192 |4 / wysoka |
| Standardowa_F8s |8 |16 |32 |16 |32 000 / 256 (96) |25 600 / 384 |8 / wysoka |
| Standardowa_F16s |16 |32 |64 |32 |64 000 / 512 (192) |51 200 / 768 |8 / ekstremalnie wysoka |

MB/s = 10^6 bajtów na sekundę, GiB = 1024^3 bajtów.

* Maksymalna przepływność dysków (liczba operacji we/wy na sekundę lub MB/s) możliwa dla maszyny wirtualnej serii Fs może być ograniczona przez liczbę, rozmiar i rozkładanie dołączonych dysków.  Aby uzyskać szczegółowe informacje, zobacz [Premium Storage: High-performance storage for Azure virtual machine workloads](../articles/storage/storage-premium-storage.md) (Premium Storage: magazyn o wysokiej wydajności dla obciążeń maszyn wirtualnych platformy Azure).

<br>

## <a name="g-series"></a>Seria G
| Rozmiar | Rdzenie procesora CPU | Pamięć: GiB | Lokalne dyski SSD: GiB | Maks. liczba dysków danych | Maksymalna przepływność dysków: liczba operacji we/wy na sekundę | Maksymalna liczba kart sieciowych / przepustowość sieci |
| --- | --- | --- | --- | --- | --- | --- |
| Standardowa_G1 |2 |28 |384 |4 |4 x 500 |1 / wysoka |
| Standardowa_G2 |4 |56 |768 |8 |8 x 500 |2 / wysoka |
| Standardowa_G3 |8 |112 |1536 |16 |16 x 500 |4 / bardzo wysoka |
| Standardowa_G4 |16 |224 |3072 |32 |32 x 500 |8 / ekstremalnie wysoka |
| Standardowa_G5* |32 |448 |6144 |64 |64 x 500 |8 / ekstremalnie wysoka |

* Wystąpienie jest izolowane do sprzętu dedykowanego pojedynczemu klientowi.
<br>

## <a name="gs-series"></a>Seria GS*
| Rozmiar | Rdzenie procesora CPU | Pamięć: GiB | Lokalne dyski SSD: GiB | Maks. liczba dysków danych | Maksymalna przepływność buforowanych dysków: liczba operacji we/wy na sekundę / MB/s (rozmiar pamięci podręcznej w GiB) | Maksymalna przepływność niebuforowanych dysków: liczba operacji we/wy na sekundę / MB/s | Maksymalna liczba kart sieciowych / przepustowość sieci |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Standardowa_GS1 |2 |28 |56 |4 |10 000 / 100 (264) |5000 / 125 |1 / wysoka |
| Standardowa_GS2 |4 |56 |112 |8 |20 000 / 200 (528) |10 000 / 250 |2 / wysoka |
| Standardowa_GS3 |8 |112 |224 |16 |40 000 / 400 (1056) |20 000 / 500 |4 / bardzo wysoka |
| Standardowa_GS4 |16 |224 |448 |32 |80 000 / 800 (2112) |40 000 / 1000 |8 / ekstremalnie wysoka |
| Standardowa_GS5** |32 |448 |896 |64 |160 000 / 1600 (4224) |80 000 / 2000 |8 / ekstremalnie wysoka |

MB/s = 10^6 bajtów na sekundę, GiB = 1024^3 bajtów.

* Maksymalna przepływność dysków (liczba operacji we/wy na sekundę lub MB/s) możliwa dla maszyny wirtualnej serii GS może być ograniczona przez liczbę, rozmiar i rozkładanie dołączonych dysków. 

** Wystąpienie jest izolowane do sprzętu dedykowanego pojedynczemu klientowi.
<br>

## <a name="h-series"></a>Seria H
Maszyny wirtualne serii H platformy Azure to następna generacja maszyn wirtualnych o wysokiej wydajności obliczeniowej, które idealnie sprawdzają się w przypadku najwyższych potrzeb obliczeniowych, na przykład w modelowaniu molekularnym i analizach obliczeniowych dynamiki płynów. Te maszyny wirtualne z 8 lub 16 rdzeniami wykorzystują technologię procesora Intel Haswell E5-2667 V3 i wyposażone są w pamięć DDR4 oraz lokalny magazyn SSD. 

Seria H oferuje, obok znacznej mocy procesora CPU, różnorodne opcje dla sieci obsługujących technologię RDMA i niskie opóźnienia, korzystając z sieci InfiniBand o przepustowości FDR wraz z kilkoma konfiguracjami pamięci do obsługi obliczeń wymagających znacznego wykorzystania pamięci.

Informacje i uwagi dotyczące korzystania z tych rozmiarów zawiera artykuł [About the H-series and compute-intensive A-series VMs](../articles/virtual-machines/virtual-machines-windows-a8-a9-a10-a11-specs.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (Informacje o maszynach wirtualnych serii H i intensywnie korzystających z obliczeń maszynach wirtualnych serii A).

| Rozmiar | Rdzenie procesora CPU | Pamięć: GiB | Lokalne dyski SSD: GiB | Maks. liczba dysków danych | Maksymalna przepływność dysków: liczba operacji we/wy na sekundę | Maksymalna liczba kart sieciowych / przepustowość sieci |
| --- | --- | --- | --- | --- | --- | --- |
| Standardowa_H8 |8 |56 |1000 |16 |16 x 500 |2 / wysoka |
| Standardowa_H16 |16 |112 |2000 |32 |32 x 500 |4 / bardzo wysoka |
| Standardowa_H8m |8 |112 |1000 |16 |16 x 500 |2 / wysoka |
| Standardowa_H16m |16 |224 |2000 |32 |32 x 500 |4 / bardzo wysoka |
| Standardowa_H16r* |16 |112 |2000 |32 |32 x 500 |4 / bardzo wysoka |
| Standardowa_H16mr* |16 |224 |2000 |32 |32 x 500 |4 / bardzo wysoka |

* Obsługa technologii RDMA

<br>


## <a name="ls-series"></a>Seria Ls 

Seria Ls jest zoptymalizowana pod kątem obciążeń wymagających magazynu lokalnego o małym opóźnieniu, takich jak bazy danych NoSQL (np. Cassandra, MongoDB, Cloudera i Redis). Maszyny wirtualne serii Ls oferują do 32 rdzeni procesora CPU — procesor [Intel® Xeon® z rodziny E5 v3](http://www.intel.com/content/www/us/en/processors/xeon/xeon-e5-solutions.html). Wydajność procesora CPU jest taka sama jak w przypadku maszyn wirtualnych serii G/GS. Na rdzeń procesora przypada pamięć o pojemności 8 GiB.  

 
| Rozmiar          | Rdzenie procesora CPU | Pamięć: GiB | Lokalne dyski SSD: GiB | Maks. liczba dysków danych | Maksymalna przepływność buforowanych dysków: liczba operacji we/wy na sekundę / MB/s (rozmiar pamięci podręcznej w GiB) | Maksymalna przepływność niebuforowanych dysków: liczba operacji we/wy na sekundę / MB/s | Maksymalna liczba kart sieciowych / przepustowość sieci | 
|---------------|-----------|-------------|--------------------------|----------------|-------------------------------------------------------------|-------------------------------------------|------------------------------| 
| Standardowa_L4s  | 4    | 32   | 678   | 8              | ND / ND (0)          | 5000 / 125                               | 2 / wysoka       | 
| Standardowa_L8s  | 8    | 64   | 1,388 | 16             | ND / ND (0)          | 10 000 / 250                              | 4 / bardzo wysoka  | 
| Standardowa_L16s | 16   | 128  | 2,807 | 32             | ND / ND (0)          | 20 000 / 500                              | 8 / ekstremalnie wysoka | 
| Standardowa_L32s | 32   | 256  | 5,630 | 64             | ND / ND (0)          | 40 000 / 1000                            | 8 / ekstremalnie wysoka | 
 
MB/s = 10^6 bajtów na sekundę, GiB = 1024^3 bajtów. 



## <a name="n-series"></a>Seria N
Rozmiary serii NC i NV są również nazywane wystąpieniami z włączonymi procesorami GPU. Są to wyspecjalizowane maszyny wirtualne zawierające karty procesorów GPU firmy NVIDIA, zoptymalizowane pod kątem różnych scenariuszy i przypadków użycia. Rozmiary serii NV są zoptymalizowane i zaprojektowane na potrzeby scenariuszy zdalnej wizualizacji, przesyłania strumieniowego, gier, kodowania oraz infrastruktury pulpitu wirtualnego (VDI) wykorzystujących platformy takie jak OpenGL i DirectX. Rozmiary NC są bardziej zoptymalizowane pod kątem aplikacji intensywnie korzystających z mocy obliczeniowej i sieci, takich jak aplikacje i symulacje oparte na strukturach CUDA i OpenCL. 


### <a name="nv-instances"></a>Wystąpienia NV
Wystąpienia NV korzystają z karty procesorów GPU Tesla M60 firmy NVIDIA oraz technologii NVIDIA GRID dla przyspieszonych aplikacji komputerowych i pulpitów wirtualnych, na których klienci będą mogli wizualizować swoje dane lub symulacje. Użytkownicy będą mogli wizualizować swoje przepływy pracy o dużych obciążeniach graficznych na wystąpieniach NV, aby uzyskać doskonałe możliwości grafiki oraz dodatkowo uruchamiać obciążenia o pojedynczej precyzji, takie jak kodowanie i renderowanie. Karta Tesla M60 zapewnia 4096 rdzeni CUDA w konstrukcji z dwoma procesorami GPU z maksymalnie 36 strumieniami H.264 1080p. 


| Rozmiar | Rdzenie procesora CPU | Pamięć: GiB | Lokalne dyski SSD: GiB | Procesory GPU |
| --- | --- | --- | --- | --- |
| Standardowa_NV6 |6 |56 |380 | 1 |
| Standardowa_NV12 |12 |112 |680 | 2 |
| Standardowa_NV24 |24 |224 |1440 | 4 |

1 procesor GPU = połowa karty M60.

**Obsługiwane systemy operacyjne**

* Windows Server 2016, Windows Server 2012 R2 — zobacz artykuł [N-series driver setup for Windows](../articles/virtual-machines/virtual-machines-windows-n-series-driver-setup.md) (Konfiguracja sterowników serii N dla systemu Windows)

### <a name="nc-instances"></a>Wystąpienia NC
Wystąpienia NC są obsługiwane przez kartę Tesla K80 firmy NVIDIA. Użytkownicy mogą teraz przetwarzać dane znacznie szybciej dzięki wykorzystaniu architektury CUDA dla aplikacji takich jak eksploracja zasobów energetycznych, symulacje zderzeń, renderowanie metodą śledzenia promieni, uczenie głębokie i wiele innych. Wyposażona w dwa procesory GPU karta Tesla K80 dostarcza 4992 rdzeni CUDA, maksymalnie 2,91 teraflopa wydajności obliczeń o podwójnej precyzji i maksymalnie 8,93 teraflopa wydajności obliczeń o pojedynczej precyzji.

| Rozmiar | Rdzenie procesora CPU | Pamięć: GiB | Lokalne dyski SSD: GiB | Procesory GPU |
| --- | --- | --- | --- | --- |
| Standardowa_NC6 |6 |56 | 380 | 1 |
| Standardowa_NC12 |12 |112 | 680 | 2 |
| Standardowa_NC24 |24 |224 | 1440 | 4 |
| Standardowa_NC24r* |24 |224 | 1440 | 4 |

1 procesor GPU = połowa karty K80.

* Obsługa technologii RDMA

**Obsługiwane systemy operacyjne**

* Windows Server 2016, Windows Server 2012 R2 — zobacz artykuł [N-series driver setup for Windows](../articles/virtual-machines/virtual-machines-windows-n-series-driver-setup.md) (Konfiguracja sterowników serii N dla systemu Windows)
* Ubuntu 16.04 LTS — zobacz artykuł [N-series driver setup for Linux](../articles/virtual-machines/virtual-machines-linux-n-series-driver-setup.md) (Konfiguracja sterowników serii N dla systemu Linux)

<br>

## <a name="notes-standard-a0---a4-using-cli-and-powershell"></a>Uwagi: Standard_A0–A4 w przypadku używania interfejsu wiersza polecenia i programu PowerShell
W klasycznym modelu wdrażania niektóre nazwy rozmiarów maszyny wirtualnej są nieco inne w interfejsie wiersza polecenia i programie PowerShell:

* Standardowa_A0 = Bardzo mała 
* Standardowa_A1 = Mała
* Standardowa_A2 = Średnia
* Standardowa_A3 = Duża
* Standardowa_A4 = Bardzo duża

## <a name="next-steps"></a>Następne kroki
* Dowiedz się więcej na temat [limitów, przydziałów i ograniczeń usługi i subskrypcji platformy Azure](../articles/azure-subscription-service-limits.md).
* Dowiedz się więcej [o maszynach wirtualnych serii H i intensywnie korzystających z obliczeń maszynach wirtualnych serii A](../articles/virtual-machines/virtual-machines-windows-a8-a9-a10-a11-specs.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) dla obciążeń takich jak obliczenia o wysokiej wydajności (HPC).



<!--HONumber=Feb17_HO3-->


