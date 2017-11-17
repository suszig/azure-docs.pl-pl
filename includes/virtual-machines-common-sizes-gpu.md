
Procesor GPU zoptymalizowanych pod kątem maszyn wirtualnych są przeznaczone do duże renderowania grafiki i wideo edytowania specjalne maszyn wirtualnych. Dostępne z jednego lub wielu procesorów graficznych. Ten artykuł zawiera informacje o liczbie Vcpu, dyski danych i karty sieciowe, a także przepustowości przepływności i sieć magazynu dla każdego rozmiaru w tej metodzie grupowania. 


Rozmiary serii NC i NV są również nazywane wystąpieniami z włączonymi procesorami GPU. Są one rozmiarów specjalne maszyn wirtualnych, które obejmują firmy NVIDIA w karty GPU, zoptymalizowane pod kątem różnych scenariuszy i przypadki użycia. Rozmiary serii NV są zoptymalizowane i zaprojektowane na potrzeby scenariuszy zdalnej wizualizacji, przesyłania strumieniowego, gier, kodowania oraz infrastruktury pulpitu wirtualnego (VDI) wykorzystujących platformy takie jak OpenGL i DirectX. Rozmiary NC są bardziej zoptymalizowane pod kątem aplikacji intensywnie korzystających z mocy obliczeniowej i sieci, takich jak aplikacje i symulacje oparte na strukturach CUDA i OpenCL. 


Wystąpienia NV korzystają z karty procesorów GPU Tesla M60 firmy NVIDIA oraz technologii NVIDIA GRID dla przyspieszonych aplikacji komputerowych i pulpitów wirtualnych, na których klienci mogą wizualizować swoje dane lub symulacje. Użytkownicy będą mogli wizualizacji w wystąpieniach wirtualizacją sieci, aby uzyskać możliwość wyższego poziomu grafiki i dodatkowo uruchamiania obciążeń pojedynczej precyzji, takie jak kodowanie i renderowania znacznym przepływy pracy ich grafiki. Karta Tesla M60 zapewnia 4096 rdzeni CUDA w konstrukcji z dwoma procesorami GPU z maksymalnie 36 strumieniami H.264 1080p. 

Wystąpienia NC są obsługiwane przez kartę Tesla K80 firmy NVIDIA. Użytkownicy mogą teraz przetwarzać dane znacznie szybciej dzięki wykorzystaniu architektury CUDA dla aplikacji takich jak eksploracja zasobów energetycznych, symulacje zderzeń, renderowanie metodą śledzenia promieni, uczenie głębokie i wiele innych. Wyposażona w dwa procesory GPU karta Tesla K80 dostarcza 4992 rdzeni CUDA, maksymalnie 2,91 teraflopa wydajności obliczeń o podwójnej precyzji i maksymalnie 8,93 teraflopa wydajności obliczeń o pojedynczej precyzji.

## <a name="nv-instances"></a>Wystąpienia NV

| Rozmiar | Procesor wirtualny | Pamięć: GiB | Magazyn tymczasowy (SSD): GiB | Procesory GPU | Maksymalna liczba dysków danych |
| --- | --- | --- | --- | --- | --- |
| Standardowa_NV6 |6 |56 |380 | 1 | 24 |
| Standardowa_NV12 |12 |112 |680 | 2 | 48 |
| Standardowa_NV24 |24 |224 |1440 | 4 | 64 |

1 procesor GPU = połowa karty M60.

## <a name="nc-instances"></a>Wystąpienia NC

| Rozmiar | Procesor wirtualny | Pamięć: GiB | Magazyn tymczasowy (SSD): GiB | Procesory GPU | Maksymalna liczba dysków danych |
| --- | --- | --- | --- | --- | --- |
| Standardowa_NC6 |6 |56 | 380 | 1 | 24 |
| Standardowa_NC12 |12 |112 | 680 | 2 | 48 |
| Standardowa_NC24 |24 |224 | 1440 | 4 | 64 |
| Standardowa_NC24r* |24 |224 | 1440 | 4 | 64 |

1 procesor GPU = połowa karty K80.

* Obsługa technologii RDMA


