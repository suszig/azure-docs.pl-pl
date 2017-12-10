| Zasób | Bezpłatna | Współdzielona (wersja zapoznawcza) | Podstawowa | Standardowa | Premium (wersja zapoznawcza)</th> |
| --- | --- | --- | --- | --- | --- |
| [Aplikacje sieci Web, mobilnych lub interfejsu API](https://azure.microsoft.com/services/app-service/) na [planu usługi aplikacji](../articles/app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)<sup>1</sup> |10 |100 |Nieograniczone<sup>2</sup> |Nieograniczone<sup>2</sup> |Nieograniczone<sup>2</sup> |
| [Aplikacje logiki](https://azure.microsoft.com/services/app-service/logic/) na [planu usługi aplikacji](../articles/app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)</a><sup>1</sup> |10 |10 |10 |20 na podstawowe |20 na podstawowe |
| [Plan usługi App Service](../articles/app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) |1 na region |10 dla grupy zasobów |100 dla każdej grupy zasobów |100 dla każdej grupy zasobów |100 dla każdej grupy zasobów |
| Wystąpienie typu obliczania |Udostępniona |Udostępniona |W wersji dedykowanej<sup>3</sup> |W wersji dedykowanej<sup>3</sup> |W wersji dedykowanej<sup>3</sup></p> |
| [Skalowalny w poziomie](../articles/app-service/web-sites-scale.md) (maksymalnej liczby wystąpień) |1 udostępnionego |1 udostępnionego |3 w wersji dedykowanej<sup>3</sup> |10 w wersji dedykowanej<sup>3</sup> |(50 w ASE) w wersji dedykowanej 20<sup>3,4</sup> |
| Magazyn<sup>5</sup> |1 GB<sup>5</sup> |1 GB<sup>5</sup> |10 GB<sup>5</sup> |50 GB<sup>5</sup> |500 GB<sup>4,5</sup></p> |
| Czas procesora CPU (5 min)<sup>6</sup> |3 minuty |3 minuty |Nieograniczone, płatności na standardowy [stawki](https://azure.microsoft.com/pricing/details/app-service/)</a> |Bez ograniczeń, płatności stawkami standardowymi |Bez ograniczeń, płatności stawkami standardowymi |
| Czas procesora CPU (dzień)<sup>6</sup> |60 minut |240 minut |Nieograniczone, płatności na standardowy [stawki](https://azure.microsoft.com/pricing/details/app-service/)</a> |Bez ograniczeń, płatności stawkami standardowymi |Bez ograniczeń, płatności stawkami standardowymi |
| Pamięć (1 godz.) |1024 MB na plan usługi aplikacji |1024 MB dla aplikacji |Nie dotyczy |Nie dotyczy |Nie dotyczy |
| Przepustowość |165 MB |Nieograniczone, [szybkości transferu danych](https://azure.microsoft.com/pricing/details/data-transfers/) Zastosuj |Bez ograniczeń, transfer danych, które opłaty |Bez ograniczeń, transfer danych, które opłaty |Bez ograniczeń, transfer danych, które opłaty |
| Architektura aplikacji |32-bitowa |32-bitowa |32-bitowe/64-bitowych |32-bitowe/64-bitowych |32-bitowe/64-bitowych |
| Dla każdego wystąpienia w sieci Web Sockets<sup>7</sup> |5 |35 |350 |Nieograniczona liczba |Nieograniczona liczba |
| Współbieżne [debugera połączeń](../articles/app-service/web-sites-dotnet-troubleshoot-visual-studio.md) według aplikacji |1 |1 |1 |5 |5 |
| [poddomeny azurewebsites.NET FTP/S i protokołu SSL](../articles/app-service/app-service-web-tutorial-custom-ssl.md) |X |X |X |X |X |
| [Domena niestandardowa](../articles/app-service/app-service-web-tutorial-custom-domain.md) obsługuje | |X |X |X |X |
| Domena niestandardowa [Obsługa protokołu SSL](../articles/app-service/app-service-web-tutorial-custom-ssl.md) | | |Nieograniczone połączeń SNI SSL |Nieograniczone SSL SNI i 1 połączeń IP SSL włączone |Nieograniczone SSL SNI i 1 połączeń IP SSL włączone |
| Zintegrowana usługa równoważenia obciążenia | |X |X |X |X |
| [Zawsze włączone](../articles/app-service/web-sites-configure.md) | | |X |X |X |
| [Zaplanowane kopie zapasowe](../articles/app-service/web-sites-backup.md) | | | | Zaplanowane tworzenie kopii zapasowych co 2 godziny, maksymalnie 12 kopii zapasowych dziennie (Ręczne + zaplanowane) | Zaplanowane tworzenie kopii zapasowych co godzinę, maksymalnie 50 kopii zapasowych dziennie (Ręczne + zaplanowane) |
| [Automatyczne skalowanie](../articles/app-service/web-sites-scale.md) | | | |X |X |
| [Zadania Webjob](../articles/app-service/web-sites-create-web-jobs.md)<sup>8</sup> |X |X |X |X |X |
| [Harmonogram systemu Azure](https://azure.microsoft.com/services/scheduler/) obsługuje | |X |X |X |X |
| [Monitorowanie punktu końcowego](../articles/app-service/web-sites-monitor.md) | | |X |X |X |
| [Miejsc przejściowych](../articles/app-service/web-sites-staged-publishing.md) | | | |5 |20 |
| Domeny niestandardowe dla aplikacji</a> | |500 |500 |500 |500 |
| Umowa SLA | |<p> |99,9% |99.95%<sup>10</sup> |99.95%<sup>9</sup> |

<sup>1</sup>aplikacji i przydziały pamięci masowej są na plan usługi aplikacji, o ile nie zaznaczono inaczej.  
<sup>2</sup>rzeczywista liczba aplikacji, które mogą być hostowane na tych komputerach zależy działanie aplikacji, rozmiar wystąpień maszyn i odpowiednie wykorzystania zasobów.  
<sup>3</sup>dedykowanego wystąpienia może być o różnych rozmiarach. Zobacz [App Service — ceny](https://azure.microsoft.com/pricing/details/data-transfers/pricing/details/app-service/) więcej szczegółów.  
<sup>4</sup>warstwy premium umożliwia maksymalnie 50 oblicza wystąpień (pod warunkiem dostępności) i 500 GB miejsca na dysku, używając środowiska usługi App Service i 20 obliczeniowe inaczej wystąpień i 250 GB pamięci masowej.  
<sup>5</sup>limit magazynu jest łączny rozmiar zawartości przez wszystkie aplikacje w tym samym planie usługi aplikacji. Więcej opcji magazynu są dostępne w [środowiska usługi aplikacji](../articles/app-service/environment/app-service-web-configure-an-app-service-environment.md#storage)  
<sup>6</sup>te zasoby są ograniczone przez fizyczne zasoby dedykowanego wystąpienia (rozmiar wystąpienia i liczba wystąpień).  
<sup>7</sup>Skala aplikacji w warstwie podstawowa na dwa wystąpienia, masz 350 równoczesnych połączeń dla każdego z dwóch wystąpień.  
<sup>8</sup>uruchomić niestandardowe pliki wykonywalne lub skrypty na żądanie, zgodnie z harmonogramem lub wystąpienie stale jako zadania w tle w usłudze App Service. Do ciągłego wykonywania zadań w sieci Web jest wymagana opcja Zawsze włączona. Do wykonywania zadań w sieci Web według harmonogramu jest wymagana usługa Azure Scheduler w warstwie Bezpłatna lub Standardowa. Nie ma żadnego wstępnie zdefiniowanego limitu liczby zadań Webjob, które może być uruchamiane w wystąpieniu usługi App Service, ale istnieją praktycznych limitów, które są zależne od kodu aplikacja próbuje zrobić.   
<sup>9</sup>poziomie 99,95% podane w przypadku wdrożeń, które użycia wielu wystąpień z usługi Azure Traffic Manager skonfigurowany dla trybu failover.  

