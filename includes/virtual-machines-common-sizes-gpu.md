Rozmiary serii NC i NV są również nazywane wystąpieniami z włączonymi procesorami GPU. Są to wyspecjalizowane maszyny wirtualne zawierające karty procesorów GPU firmy NVIDIA, zoptymalizowane pod kątem różnych scenariuszy i przypadków użycia. Rozmiary serii NV są zoptymalizowane i zaprojektowane na potrzeby scenariuszy zdalnej wizualizacji, przesyłania strumieniowego, gier, kodowania oraz infrastruktury pulpitu wirtualnego (VDI) wykorzystujących platformy takie jak OpenGL i DirectX. Rozmiary NC są bardziej zoptymalizowane pod kątem aplikacji intensywnie korzystających z mocy obliczeniowej i sieci, takich jak aplikacje i symulacje oparte na strukturach CUDA i OpenCL. 


Wystąpienia NV korzystają z karty procesorów GPU Tesla M60 firmy NVIDIA oraz technologii NVIDIA GRID dla przyspieszonych aplikacji komputerowych i pulpitów wirtualnych, na których klienci będą mogli wizualizować swoje dane lub symulacje. Użytkownicy będą mogli wizualizować swoje przepływy pracy o dużych obciążeniach graficznych na wystąpieniach NV, aby uzyskać doskonałe możliwości grafiki oraz dodatkowo uruchamiać obciążenia o pojedynczej precyzji, takie jak kodowanie i renderowanie. Karta Tesla M60 zapewnia 4096 rdzeni CUDA w konstrukcji z dwoma procesorami GPU z maksymalnie 36 strumieniami H.264 1080p. 

Wystąpienia NC są obsługiwane przez kartę Tesla K80 firmy NVIDIA. Użytkownicy mogą teraz przetwarzać dane znacznie szybciej dzięki wykorzystaniu architektury CUDA dla aplikacji takich jak eksploracja zasobów energetycznych, symulacje zderzeń, renderowanie metodą śledzenia promieni, uczenie głębokie i wiele innych. Wyposażona w dwa procesory GPU karta Tesla K80 dostarcza 4992 rdzeni CUDA, maksymalnie 2,91 teraflopa wydajności obliczeń o podwójnej precyzji i maksymalnie 8,93 teraflopa wydajności obliczeń o pojedynczej precyzji.

## <a name="nv-instances"></a>Wystąpienia NV

| Rozmiar | Rdzenie procesora CPU | Pamięć: GiB | Lokalne dyski SSD: GiB | Procesory GPU |
| --- | --- | --- | --- | --- |
| Standardowa_NV6 |6 |56 |380 | 1 |
| Standardowa_NV12 |12 |112 |680 | 2 |
| Standardowa_NV24 |24 |224 |1440 | 4 |

1 procesor GPU = połowa karty M60.

**Obsługiwane systemy operacyjne**

* Windows Server 2016, Windows Server 2012 R2 — zobacz artykuł [N-series driver setup for Windows](../articles/virtual-machines/windows/n-series-driver-setup.md) (Konfiguracja sterowników serii N dla systemu Windows)

## <a name="nc-instances"></a>Wystąpienia NC

| Rozmiar | Rdzenie procesora CPU | Pamięć: GiB | Lokalne dyski SSD: GiB | Procesory GPU |
| --- | --- | --- | --- | --- |
| Standardowa_NC6 |6 |56 | 380 | 1 |
| Standardowa_NC12 |12 |112 | 680 | 2 |
| Standardowa_NC24 |24 |224 | 1440 | 4 |
| Standardowa_NC24r* |24 |224 | 1440 | 4 |

1 procesor GPU = połowa karty K80.

* Obsługa technologii RDMA

**Obsługiwane systemy operacyjne**

* Windows Server 2016, Windows Server 2012 R2 — zobacz artykuł [N-series driver setup for Windows](../articles/virtual-machines/windows/n-series-driver-setup.md) (Konfiguracja sterowników serii N dla systemu Windows)
* Ubuntu 16.04 LTS — zobacz artykuł [N-series driver setup for Linux](../articles/virtual-machines/linux/n-series-driver-setup.md) (Konfiguracja sterowników serii N dla systemu Linux)

<br>

