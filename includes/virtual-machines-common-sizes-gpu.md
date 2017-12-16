
Procesor GPU zoptymalizowanych pod kątem maszyny Wirtualnej są dostępne z jednego lub wielu procesorów graficznych NVIDIA specjalne maszyn wirtualnych. Rozmiary są przeznaczone dla obciążeń obliczeniowych, dużą ilością grafiki i wizualizacji. Ten artykuł zawiera informacje o liczbę i typ procesorów graficznych, Vcpu, dyski danych i karty sieciowe, jak również przepustowości przepływności i sieć magazynu dla każdego rozmiaru w tej metodzie grupowania. 

* **NC, NCv2 i ND** rozmiary są optymalizowane dla aplikacji obliczeniowych i obciążenie sieci i algorytmy, w tym aplikacji opartych na CUDA i OpenCL oraz symulacje, AI i uczenie się bezpośrednich. 
* **Trwałym** rozmiary są zoptymalizowane i przeznaczony dla zdalnego wizualizacji, przesyłania strumieniowego, gier, kodowanie i scenariuszy VDI przy użyciu platform, takich jak OpenGL i DirectX.  


## <a name="nc-instances"></a>Wystąpienia NC

Wystąpienia NC są obsługiwane przez [K80 tesla — NVIDIA](http://images.nvidia.com/content/pdf/kepler/Tesla-K80-BoardSpec-07317-001-v05.pdf) karty. Użytkownicy mogą wykonywać za pomocą danych szybsze dzięki wykorzystaniu CUDA dla aplikacji eksploracji energii, symulacji awarii, ray śledzonych renderowania, głębokie uczenie i inne. Konfiguracja NC24r zapewnia małe opóźnienia wysokiej przepustowości sieciowej zoptymalizowane pod kątem silnie sprzężonego równoległego przetwarzania obciążeń.


| Rozmiar | Procesor wirtualny | Pamięć: GiB | Magazyn tymczasowy (SSD): GiB | Procesory GPU | Maksymalna liczba dysków danych |
| --- | --- | --- | --- | --- | --- |
| Standardowa_NC6 |6 |56 | 380 | 1 | 24 |
| Standardowa_NC12 |12 |112 | 680 | 2 | 48 |
| Standardowa_NC24 |24 |224 | 1440 | 4 | 64 |
| Standardowa_NC24r* |24 |224 | 1440 | 4 | 64 |

1 procesor GPU = połowa karty K80.

* Obsługa technologii RDMA

## <a name="ncv2-instances"></a>NCv2 wystąpień

NCv2 wystąpienia są generacji maszyn serii NC, obsługiwane przez [P100 tesla — NVIDIA](http://images.nvidia.com/content/tesla/pdf/nvidia-tesla-p100-datasheet.pdf) GPU. Te jednostki GPU zapewniają więcej niż 2 x obliczeniowej bieżącej serii NC. Klienci mogą wykorzystać te GPU zaktualizowane dla tradycyjnych obciążeń HPC, takich jak modelowania zbiornika, DNS sekwencjonowania białka analizy, Symulacje Monte Carlo i inne. Podobnie jak serii NC serii NCv2 oferuje konfiguracji z niskim opóźnieniem, wysokiej przepustowości sieciowej zoptymalizowane pod kątem silnie sprzężonego równoległego przetwarzania obciążeń.

> [!IMPORTANT]
> Dla tej rodziny rozmiar przydziału vCPU (podstawowe) w subskrypcji jest początkowo równa 0 w każdym regionie. [Zażądać zwiększenia limitu przydziału vCPU](../articles/azure-supportability/resource-manager-core-quotas-request.md) tej rodziny w [dostępny region](https://azure.microsoft.com/regions/services/).
>

| Rozmiar | Procesor wirtualny | Pamięć: GiB | Magazyn tymczasowy (SSD): GiB | Procesory GPU | Maksymalna liczba dysków danych |
| --- | --- | --- | --- | --- | --- |
| Standard_NC6_v2 |6 |112 | 336 | 1 | 12 |
| Standard_NC12_v2 |12 |224 | 672 | 2 | 24 |
| Standard_NC24_v2 |24 |448 | 1344 | 4 | 32 |
| Standard_NC24r_v2 * |24 |1448 | 1344 | 4 | 32 |

Procesor GPU 1 = co karta P100.

* Obsługa technologii RDMA

## <a name="nd-instances"></a>ND wystąpień

Maszyny wirtualne ND serii są nowe uzupełnienie rodziny procesora GPU, przeznaczone dla obciążeń AI i uczenie się bezpośrednich. Oferują one doskonałą wydajność dla szkolenia i wnioskowania. ND wystąpienia są obsługiwane przez [P40 tesla — NVIDIA](http://images.nvidia.com/content/pdf/tesla/184427-Tesla-P40-Datasheet-NV-Final-Letter-Web.pdf) GPU. Te wystąpienia zapewnić doskonałą wydajność dla pojedynczej precyzji operacje zmiennoprzecinkowe, AI obciążeń przy użyciu kognitywnych zestaw narzędzi firmy Microsoft, TensorFlow Caffe i innych platform. Seria ND oferuje także dużo pojemniejszą pamięć procesora GPU (24 GB) mieszczącą znacznie większe modele sieci neuronowych. Jak NC serii serii ND oferuje konfiguracji dodatkowej małym opóźnieniu i dużej przepustowości sieci przez funkcję RDMA i łączność InfiniBand, więc można uruchamiać zadania na dużą skalę szkolenia obejmujących wiele procesorów graficznych.

> [!IMPORTANT]
> Dla tej rodziny rozmiar przydziału vCPU (podstawowe) na region w ramach subskrypcji początkowo jest równa 0. [Zażądać zwiększenia limitu przydziału vCPU](../articles/azure-supportability/resource-manager-core-quotas-request.md) tej rodziny w [dostępny region](https://azure.microsoft.com/regions/services/).
>

| Rozmiar | Procesor wirtualny | Pamięć: GiB | Magazyn tymczasowy (SSD): GiB | Procesory GPU | Maksymalna liczba dysków danych |
| --- | --- | --- | --- | --- | --- |
| Standard_ND6 |6 |112 | 336 | 1 | 12 |
| Standard_ND12 |12 |224 | 672 | 2 | 24 |
| Standard_ND24 |24 |448 | 1344 | 4 | 32 |
| Standard_ND24r * |24 |1448 | 1344 | 4 | 32 |

Procesor GPU 1 = co karta P40.

* Obsługa technologii RDMA

## <a name="nv-instances"></a>Wystąpienia NV



Wystąpień wirtualizacją sieci są obsługiwane przez [M60 tesla — NVIDIA ](http://images.nvidia.com/content/tesla/pdf/188417-Tesla-M60-DS-A4-fnl-Web.pdf) GPU i siatki NVIDIA technologii pulpitu przyspieszony aplikacji i pulpitów wirtualnych gdzie klienci mogą się wizualizacji ich danych lub symulacji. Użytkownicy będą mogli wizualizacji w wystąpieniach wirtualizacją sieci, aby uzyskać możliwość wyższego poziomu grafiki i dodatkowo uruchamiania obciążeń pojedynczej precyzji, takie jak kodowanie i renderowania znacznym przepływy pracy ich grafiki. 

| Rozmiar | Procesor wirtualny | Pamięć: GiB | Magazyn tymczasowy (SSD): GiB | Procesory GPU | Maksymalna liczba dysków danych |
| --- | --- | --- | --- | --- | --- |
| Standardowa_NV6 |6 |56 |380 | 1 | 24 |
| Standardowa_NV12 |12 |112 |680 | 2 | 48 |
| Standardowa_NV24 |24 |224 |1440 | 4 | 64 |

1 procesor GPU = połowa karty M60.


