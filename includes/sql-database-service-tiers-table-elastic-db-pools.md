<!--
Used in:
sql-database-elastic-pool.md   
sql-database-resource-limits.md
sql-database-service-tiers.md  
-->

### <a name="basic-elastic-pool-limits"></a>Limity podstawowych pul elastycznych

| Rozmiar puli (jednostki eDTU)  | **50** | **100** | **200** | **300** | **400** | **800** | **1200** | **1600** |
|:---|---:|---:|---:| ---: | ---: | ---: | ---: | ---: |
| Maksymalna pojemność magazynu na pulę* | 5 GB| 10 GB| 20 GB| 29 GB| 39 GB| 78 GB| 117 GB| 156 GB|
| Maksymalna liczba baz danych na pulę | 100 | 200 | 500 | 500 | 500 | 500 | 500 | 500 |
| Maksymalna liczba współbieżnych procesów roboczych na pulę | 100 | 200 | 400 | 600 | 800 | 1600 | 2400 | 3200 |
| Maksymalna liczba współbieżnych logowań na pulę | 100 | 200 | 400 | 600 | 800 | 1600 | 2400 | 3200 |
| Maksymalna liczba współbieżnych sesji na pulę | 30000 | 30000 | 30000 | 30000 |30000 | 30000 | 30000 | 30000 |
| Minimalna liczba jednostek eDTU na bazę danych | {0, 5} | {0, 5} | {0, 5} | {0, 5} | {0, 5} | {0, 5} | {0, 5} | {0, 5} | {0, 5} |
| Maksymalna liczba jednostek eDTU na bazę danych | {5} | {5} | {5} | {5} | {5} | {5} | {5} | {5} | {5} |
||||||||

### <a name="standard-elastic-pool-limits"></a>Limity standardowych pul elastycznych

| Rozmiar puli (jednostki eDTU)  | **50** | **100** | **200** | **300** | **400** | **800** | 
|:---|---:|---:|---:| ---: | ---: | ---: | 
| Maksymalna pojemność magazynu na pulę* | 50 GB| 100 GB| 200 GB | 300 GB| 400 GB | 800 GB | 
| Maksymalna liczba baz danych na pulę | 100 | 200 | 500 | 500 | 500 | 500 | 
| Maksymalna liczba współbieżnych procesów roboczych na pulę | 100 | 200 | 400 | 600 |  800 | 1600 |
| Maksymalna liczba współbieżnych logowań na pulę | 100 | 200 | 400 | 600 |  800 | 1600 |
| Maksymalna liczba współbieżnych sesji na pulę | 30000 | 30000 | 30000 | 30000 | 30000 | 30000 |
| Minimalna liczba jednostek eDTU na bazę danych | {0,10,20,<br>50} | {0,10,20,<br>50,100} | {0,10,20,<br>50,100} | {0,10,20,<br>50,100} | {0,10,20,<br>50,100} | {0,10,20,<br>50,100} |
| Maksymalna liczba jednostek eDTU na bazę danych | {10,20,<br>50} | {10,20,<br>50,100} | {10,20,<br>50,100} | {10,20,<br>50,100} | {10,20,<br>50,100} | {10,20,<br>50,100} | 
||||||||

### <a name="standard-elastic-pool-limits-continued-"></a>Limity pul elastycznych Standardowa (ciąg dalszy) **

| Rozmiar puli (jednostki eDTU)  |  **1200** | **1600** | **2000** | **2500** | **3000** |
|:---|---:|---:|---:| ---: | ---: |
| Maksymalna pojemność magazynu na pulę* | 1,2 TB | 1,6 TB | 2 TB | 2,4 TB | 2,9 TB | 
| Maksymalna liczba baz danych na pulę | 500 | 500 | 500 | 500 | 500 | 500 |
| Maksymalna liczba współbieżnych procesów roboczych na pulę |  2400 | 3200 | 4000 | 5000 | 6000 |
| Maksymalna liczba współbieżnych logowań na pulę |  2400 | 3200 | 4000 | 5000 | 6000 |
| Maksymalna liczba współbieżnych sesji na pulę | 30000 | 30000 | 30000 | 30000 | 30000 | 
| Minimalna liczba jednostek eDTU na bazę danych | {0,10,20,<br>50,100} | {0,10,20,<br>50,100} | {0,10,20,<br>50,100} | {0,10,20,<br>50,100} | {0,10,20,<br>50,100} |
| Maksymalna liczba jednostek eDTU na bazę danych | {10,20,<br>50,100} | {10,20,<br>50,100} | {10,20,<br>50,100} | {10,20,<br>50,100} | {10,20,<br>50,100} | 
||||||||

### <a name="premium-elastic-pool-limits"></a>Limity pul elastycznych Premium

| Rozmiar puli (jednostki eDTU)  | **125** | **250** | **500** | **1000** | **1500** | 
|:---|---:|---:|---:| ---: | ---: | 
| Maksymalna pojemność magazynu na pulę* | 250 GB| 500 GB| 750 GB| 750 GB| 750 GB| 
| Maksymalna liczba baz danych na pulę | 50 | 100 | 100 | 100 | 100 |  
| Maksymalna liczba współbieżnych procesów roboczych na pulę | 200 | 400 | 800 | 1600 |  2400 | 
| Maksymalna liczba współbieżnych logowań na pulę | 200 | 400 | 800 | 1600 |  2400 |
| Maksymalna liczba współbieżnych sesji na pulę | 30000 | 30000 | 30000 | 30000 | 30000 | 
| Minimalna liczba jednostek eDTU na bazę danych | {0,25,50,75,<br>125} | {0,25,50,75,<br>125,250} | {0,25,50,75,<br>125,250,500} | {0,25,50,75,<br>125,250,500,<br>1000} | {0,25,50,75,<br>125,250,500,<br>1000,1500} | 
| Maksymalna liczba jednostek eDTU na bazę danych | {25,50,75,<br>125} | {25,50,75,<br>125,250} | {25,50,75,<br>125,250,500} | {25,50,75,<br>125,250,500,<br>1000} | {25,50,75,<br>125,250,500,<br>1000,1500} |  
||||||||

### <a name="premium-elastic-pool-limits-continued-"></a>Limity pul elastycznych Premium (ciąg dalszy) **

| Rozmiar puli (jednostki eDTU)  |  **2000** | **2500** | **3000** | **3500** | **4000** |
|:---|---:|---:|---:| ---: | ---: | 
| Maksymalna pojemność magazynu na pulę* | 750 GB | 750 GB | 750 GB | 750 GB | 750 GB |
| Maksymalna liczba baz danych na pulę | 100 | 100 | 100 | 100 | 100 | 
| Maksymalna liczba współbieżnych procesów roboczych na pulę |  3200 | 4000 | 4800 | 5600 | 6400 |
| Maksymalna liczba współbieżnych logowań na pulę |  3200 | 4000 | 4800 | 5600 | 6400 |
| Maksymalna liczba współbieżnych sesji na pulę | 30000 | 30000 | 30000 | 30000 | 30000 | 
| Minimalna liczba jednostek eDTU na bazę danych | {0,25,50,75,<br>125,250,500,<br>1000,1750} | {0,25,50,75,<br>125,250,500,<br>1000,1750} | {0,25,50,75,<br>125,250,500,<br>1000,1750} | {0,25,50,75,<br>125,250,500,<br>1000,1750} |  {0,25,50,75,<br>125,250,500,<br>1000,1750,4000} | 
| Maksymalna liczba jednostek eDTU na bazę danych | {25,50,75,<br>125,250,500,<br>1000,1750} | {25,50,75,<br>125,250,500,<br>1000,1750} | {25,50,75,<br>125,250,500,<br>1000,1750} | {25,50,75,<br>125,250,500,<br>1000,1750} | {25,50,75,<br>125,250,500,<br>1000,1750,4000} | 
||||||||

\* Elastyczna baza danych współużytkuje magazyn w puli, dzięki czemu magazyn bazy danych jest ograniczony do mniejszego z pozostałych magazynów puli lub maksymalnego rozmiaru magazynu na bazę danych

\** Rozmiary pul wynoszące 3000 eDTU dla puli elastycznej Standardowa i 4000 eDTU dla puli elastycznej Premium są obecnie dostępne wyłącznie za pośrednictwem programu PowerShell oraz interfejsu API REST. Wkrótce te rozmiary pul zostaną udostępnione w witrynie Azure Portal. 


<!--HONumber=Dec16_HO1-->


