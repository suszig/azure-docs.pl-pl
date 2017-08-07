<!--
Used in:
sql-database-elastic-pool.md   
sql-database-resource-limits.md
sql-database-service-tiers.md  
-->
 
### <a name="basic-elastic-pool-limits"></a>Limity podstawowych pul elastycznych

| Rozmiar puli (jednostki eDTU)  | **50** | **100** | **200** | **300** | **400** | **800** | **1200** | **1600** |
|:---|---:|---:|---:| ---: | ---: | ---: | ---: | ---: |
| Maksymalny rozmiar magazynu danych na pulę* | 5 GB | 10 GB | 20 GB | 29 GB | 39 GB | 78 GB | 117 GB | 156 GB |
| Maksymalna pojemność magazynu OLTP w pamięci na pulę | Nie dotyczy | Nie dotyczy | Nie dotyczy | Nie dotyczy | Nie dotyczy | Nie dotyczy | Nie dotyczy | Nie dotyczy |
| Maksymalna liczba baz danych na pulę | 100 | 200 | 500 | 500 | 500 | 500 | 500 | 500 |
| Maksymalna liczba współbieżnych procesów roboczych (żądań) na pulę | 100 | 200 | 400 | 600 | 800 | 1600 | 2400 | 3200 |
| Maksymalna liczba współbieżnych logowań na pulę | 100 | 200 | 400 | 600 | 800 | 1600 | 2400 | 3200 |
| Maksymalna liczba współbieżnych sesji na pulę | 30000 | 30000 | 30000 | 30000 |30000 | 30000 | 30000 | 30000 |
| Minimalna liczba jednostek eDTU na bazę danych | 0, 5 | 0, 5 | 0, 5 | 0, 5 | 0, 5 | 0, 5 | 0, 5 | 0, 5 |
| Maksymalna liczba jednostek eDTU na bazę danych | 5 | 5 | 5 | 5 | 5 | 5 | 5 | 5 |
| Maksymalny rozmiar magazynu danych na bazę danych | 2 GB | 2 GB | 2 GB | 2 GB | 2 GB | 2 GB | 2 GB | 2 GB | 
||||||||

### <a name="standard-elastic-pool-limits"></a>Limity standardowych pul elastycznych

| Rozmiar puli (jednostki eDTU)  | **50** | **100** | **200**** | **300**** | **400**** | **800****| 
|:---|---:|---:|---:| ---: | ---: | ---: | 
| Maksymalny rozmiar magazynu danych na pulę* | 50 GB| 100 GB| 200 GB | 300 GB| 400 GB | 800 GB | 
| Maksymalna pojemność magazynu OLTP w pamięci na pulę | Nie dotyczy | Nie dotyczy | Nie dotyczy | Nie dotyczy | Nie dotyczy | Nie dotyczy | 
| Maksymalna liczba baz danych na pulę | 100 | 200 | 500 | 500 | 500 | 500 | 
| Maksymalna liczba współbieżnych procesów roboczych (żądań) na pulę | 100 | 200 | 400 | 600 |  800 | 1600 |
| Maksymalna liczba współbieżnych logowań na pulę | 100 | 200 | 400 | 600 |  800 | 1600 |
| Maksymalna liczba współbieżnych sesji na pulę | 30000 | 30000 | 30000 | 30000 | 30000 | 30000 |
| Minimalna liczba jednostek eDTU na bazę danych** | 0, 10, 20, 50 | 0, 10, 20, 50, 100 | 0, 10, 20, 50, 100, 200 | 0, 10, 20, 50, 100, 200, 300 | 0, 10, 20, 50, 100, 200, 300, 400 | 0, 10, 20, 50, 100, 200, 300, 400, 800 |
| Maksymalna liczba jednostek eDTU na bazę danych** | 10, 20, 50 | 10, 20, 50, 100 | 10, 20, 50, 100, 200 | 10, 20, 50, 100, 200, 300 | 10, 20, 50, 100, 200, 300, 400 | 10, 20, 50, 100, 200, 300, 400, 800 | 
| Maksymalny rozmiar magazynu danych na bazę danych | 50 GB | 100 GB | 200 GB | 250 GB | 250 GB | 250 GB |
||||||||

### <a name="standard-elastic-pool-limits-continued"></a>Limity standardowych pul elastycznych (ciąg dalszy) 

| Rozmiar puli (jednostki eDTU)  |  **1200**** | **1600**** | **2000**** | **2500**** | **3000**** |
|:---|---:|---:|---:| ---: | ---: |
| Maksymalny rozmiar magazynu danych na pulę* | 1,2 TB | 1,6 TB | 2 TB | 2,4 TB | 2,9 TB | 
| Maksymalna pojemność magazynu OLTP w pamięci na pulę | Nie dotyczy | Nie dotyczy | Nie dotyczy | Nie dotyczy | Nie dotyczy | 
| Maksymalna liczba baz danych na pulę | 500 | 500 | 500 | 500 | 500 | 
| Maksymalna liczba współbieżnych procesów roboczych (żądań) na pulę |  2400 | 3200 | 4000 | 5000 | 6000 |
| Maksymalna liczba współbieżnych logowań na pulę |  2400 | 3200 | 4000 | 5000 | 6000 |
| Maksymalna liczba współbieżnych sesji na pulę | 30000 | 30000 | 30000 | 30000 | 30000 | 
| Minimalna liczba jednostek eDTU na bazę danych** | 0, 10, 20, 50, 100, 200, 300, 400, 800, 1200 | 0, 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600 | 0, 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600, 2000 | 0, 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600, 2000, 2500 | 0, 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600, 2000, 2500, 3000 |
| Maksymalna liczba jednostek eDTU na bazę danych** | 10, 20, 50, 100, 200, 300, 400, 800, 1200 | 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600 | 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600, 2000 | 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600, 2000, 2500 | 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600, 2000, 2500, 3000 | 
| Maksymalny rozmiar magazynu danych na bazę danych | 250 GB | 250 GB | 250 GB | 250 GB | 250 GB | 
||||||||

### <a name="premium-elastic-pool-limits"></a>Limity pul elastycznych Premium

| Rozmiar puli (jednostki eDTU)  | **125** | **250** | **500** | **1000** | **1500*****| 
|:---|---:|---:|---:| ---: | ---: | 
| Maksymalny rozmiar magazynu danych na pulę* | 250 GB | 500 GB | 750 GB | 1 TB | 1,5 TB | 
| Maksymalna pojemność magazynu OLTP w pamięci na pulę | 1 GB| 2 GB| 4 GB| 10 GB| 12 GB| 
| Maksymalna liczba baz danych na pulę | 50 | 100 | 100 | 100 | 100 |  
| Maksymalna liczba współbieżnych procesów roboczych (żądań) na pulę | 200 | 400 | 800 | 1600 |  2400 | 
| Maksymalna liczba współbieżnych logowań na pulę | 200 | 400 | 800 | 1600 |  2400 |
| Maksymalna liczba współbieżnych sesji na pulę | 30000 | 30000 | 30000 | 30000 | 30000 | 
| Minimalna liczba jednostek eDTU na bazę danych | 0, 25, 50, 75, 125 | 0, 25, 50, 75, 125, 250 | 0, 25, 50, 75, 125, 250, 500 | 0, 25, 50, 75, 125, 250, 500, 1000 | 0, 25, 50, 75, 125, 250, 500, 1000 | 
| Maksymalna liczba jednostek eDTU na bazę danych | 25, 50, 75, 125 | 25, 50, 75, 125, 250 | 25, 50, 75, 125, 250, 500 | 25, 50, 75, 125, 250, 500, 1000 | 25, 50, 75, 125, 250, 500, 1000 |
| Maksymalny rozmiar magazynu danych na bazę danych | 250 GB | 500 GB | 500 GB | 500 GB | 500 GB | 
||||||||

### <a name="premium-elastic-pool-limits-continued"></a>Limity pul elastycznych Premium (ciąg dalszy) 

| Rozmiar puli (jednostki eDTU) | **2000***** | **2500***** | **3000***** | **3500***** | **4000*****|
|:---|---:|---:|---:| ---: | ---: | 
| Maksymalny rozmiar magazynu danych na pulę* | 2 TB | 2,5 TB | 3 TB | 3,5 TB | 4 TB |
| Maksymalna pojemność magazynu OLTP w pamięci na pulę | 16 GB | 20 GB | 24 GB | 28 GB | 32 GB |
| Maksymalna liczba baz danych na pulę | 100 | 100 | 100 | 100 | 100 | 
| Maksymalna liczba współbieżnych procesów roboczych (żądań) na pulę |  3200 | 4000 | 4800 | 5600 | 6400 |
| Maksymalna liczba współbieżnych logowań na pulę |  3200 | 4000 | 4800 | 5600 | 6400 |
| Maksymalna liczba współbieżnych sesji na pulę | 30000 | 30000 | 30000 | 30000 | 30000 | 
| Minimalna liczba jednostek eDTU na bazę danych | 0, 25, 50, 75, 125, 250, 500, 1000, 1750 | 0, 25, 50, 75, 125, 250, 500, 1000, 1750 | 0, 25, 50, 75, 125, 250, 500, 1000, 1750 | 0, 25, 50, 75, 125, 250, 500, 1000, 1750 |  0, 25, 50, 75, 125, 250, 500, 1000, 1750, 4000 | 
| Maksymalna liczba jednostek eDTU na bazę danych | 25, 50, 75, 125, 250, 500, 1000, 1750 | 25, 50, 75, 125, 250, 500, 1000, 1750 | 25, 50, 75, 125, 250, 500, 1000, 1750 | 25, 50, 75, 125, 250, 500, 1000, 1750 | 25, 50, 75, 125, 250, 500, 1000, 1750, 4000 | 
| Maksymalny rozmiar magazynu danych na bazę danych | 500 GB | 500 GB | 500 GB | 500 GB | 500 GB | 
||||||||

### <a name="premium-rs-elastic-pool-limits"></a>Limity elastycznych pul Premium RS

| Rozmiar puli (jednostki eDTU)  | **125** | **250** | **500** | **1000** |
|:---|---:|---:|---:| ---: | ---: | 
| Maksymalny rozmiar magazynu danych na pulę* | 250 GB| 500 GB | 750 GB | 750 GB |
| Maksymalna pojemność magazynu OLTP w pamięci na pulę | 1 GB | 2 GB | 4 GB | 10 GB |
| Maksymalna liczba baz danych na pulę | 50 | 100 | 100 | 100 |
| Maksymalna liczba współbieżnych procesów roboczych (żądań) na pulę | 200 | 400 | 800 | 1600 |
| Maksymalna liczba współbieżnych logowań na pulę | 200 | 400 | 800 | 1600 |
| Maksymalna liczba współbieżnych sesji na pulę | 30000 | 30000 | 30000 | 30000 |
| Minimalna liczba jednostek eDTU na bazę danych | 0, 25, 50, 75, 125 | 0, 25, 50, 75, 125, 250 | 0, 25, 50, 75, 125, 250, 500 | 0, 25, 50, 75, 125, 250, 500, 1000 |
| Maksymalna liczba jednostek eDTU na bazę danych | 25, 50, 75, 125 | 25, 50, 75, 125, 250 | 25, 50, 75, 125, 250, 500 | 25, 50, 75, 125, 250, 500, 1000 | 
| Maksymalny rozmiar magazynu danych na bazę danych | 250 GB | 500 GB | 500 GB | 500 GB | 
||||||||

> [!IMPORTANT]
>\* Bazy danych w puli współużytkują magazyn w puli, dzięki czemu magazyn danych w elastycznej puli jest ograniczony do mniejszego z pozostałych magazynów puli lub maksymalnego rozmiaru magazynu na bazę danych. 
>
>
>\*\* Minimalna/maksymalna liczba jednostek eDTU na bazę danych, rozpoczynając od 200 jednostek eDTU i więcej w publicznej wersji zapoznawczej.
>
>\*\*\* Domyślna maksymalna pojemność magazynu danych na pulę dla pul Premium z co najmniej 500 jednostkami eDTU wynosi 750 GB. Aby uzyskać większy maksymalny rozmiar magazynu danych na pulę Premium dla co najmniej 1000 jednostek eDTU, należy jawnie określić ten rozmiar przy użyciu witryny Azure Portal, programu [PowerShell](../articles/sql-database/sql-database-elastic-pool.md#manage-sql-database-elastic-pools-using-powershell), [interfejsu wiersza polecenia platformy Azure](../articles/sql-database/sql-database-elastic-pool.md#manage-sql-database-elastic-pools-using-the-azure-cli) lub [interfejsu API REST](../articles/sql-database/sql-database-elastic-pool.md#manage-sql-database-elastic-pools-using-the-rest-api). Pule Premium z ponad 1 TB pamięci są obecnie dostępne w publicznej wersji zapoznawczej w następujących regionach: Wschodnie stany USA2, Zachodnie stany USA, Administracja USA — Virginia, Europa Zachodnia, Niemcy Środkowe, Azja Południowo-Wschodnia, Japonia Wschodnia, Australia Wschodnia, Kanada Środkowa i Kanada Wschodnia. Maksymalna pojemność magazynu na pulę dla wszystkich regionów jest obecnie ograniczona do 750 GB.
>
