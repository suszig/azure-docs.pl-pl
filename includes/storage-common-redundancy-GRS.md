Magazyn geograficznie nadmiarowy (GRS) replikuje dane w regionie pomocniczym będący setki odległości od regionu podstawowego. Jeśli konto magazynu ma GRS włączone, dane są trwałe nawet w przypadku pełnej regionalnej awarii lub awarii, w którym regionie podstawowym nie jest możliwe do odzyskania.

Dla konta magazynu z GRS włączone aktualizacja najpierw przywiązuje dużą wagę do regionu podstawowego, w których są replikowane trzy razy. Aktualizacja jest następnie replikowane asynchronicznie w regionie pomocniczym, których również są replikowane trzy razy.

W wypadku magazynu GRS podstawowych i pomocniczych regionów zarządzania replik w domenach awarii oddzielne i uaktualnienia domen w obrębie jednostki skalowania magazynu zgodnie z opisem z LRS.

Kwestie do rozważenia:

* Ponieważ asynchroniczną replikację obejmuje opóźnienia, w przypadku regionalnej awarii jest możliwe, że zmiany, które nie zostały jeszcze zreplikowane w regionie pomocniczym zostaną utracone, jeśli nie można odzyskać dane z regionu podstawowego.
* Replika nie jest dostępna, chyba że Microsoft inicjuje trybu failover w regionie pomocniczym. Jeśli Microsoft Zainicjuj tryb failover w regionie pomocniczym, będą mieć odczytu i zapisu do tych danych po przejściu w tryb failover została ukończona. Aby uzyskać więcej informacji, zobacz [wskazówki dotyczące odzyskiwania po awarii](../articles/storage/common/storage-disaster-recovery-guidance.md). 
* Jeśli aplikacja będzie można odczytać z regionu pomocniczego, użytkownik powinien włączyć RA-GRS.

Po utworzeniu konta magazynu, należy wybrać regionie podstawowym konta. Region pomocniczy jest określany na podstawie regionu podstawowego i nie można zmienić. W poniższej tabeli przedstawiono par regionu podstawowego i pomocniczego.

| Podstawowy | Pomocniczy |
| --- | --- |
| Środkowo-północne stany USA | Środkowo-południowe stany USA |
| Środkowo-południowe stany USA | Środkowo-północne stany USA |
| Wschodnie stany USA | Zachodnie stany USA |
| Zachodnie stany USA | Wschodnie stany USA |
| Wschodnie stany USA 2 | Środkowe stany USA |
| Środkowe stany USA | Wschodnie stany USA 2 |
| Europa Północna | Europa Zachodnia |
| Europa Zachodnia | Europa Północna |
| Azja Południowo-Wschodnia | Azja Wschodnia |
| Azja Wschodnia | Azja Południowo-Wschodnia |
| Chiny Wschodnie | Chiny Północne |
| Chiny Północne | Chiny Wschodnie |
| Japonia Wschodnia | Japonia Zachodnia |
| Japonia Zachodnia | Japonia Wschodnia |
| Brazylia Południowa | Środkowo-południowe stany USA |
| Australia Wschodnia | Australia Południowo-Wschodnia |
| Australia Południowo-Wschodnia | Australia Wschodnia |
| Indie Południowe | Indie Środkowe |
| Indie Środkowe | Indie Południowe |
| Indie Zachodnie | Indie Południowe |
| Administracja USA — Iowa | Administracja USA — Wirginia |
| Administracja USA — Wirginia | Administracja USA — Teksas |
| Administracja USA — Teksas | Administracja USA — Arizona |
| Administracja USA — Arizona | Administracja USA — Teksas |
| Kanada Środkowa | Kanada Wschodnia |
| Kanada Wschodnia | Kanada Środkowa |
| Zachodnie Zjednoczone Królestwo | Południowe Zjednoczone Królestwo |
| Południowe Zjednoczone Królestwo | Zachodnie Zjednoczone Królestwo |
| Niemcy Środkowe | Niemcy Północno-Wschodnie |
| Niemcy Północno-Wschodnie | Niemcy Środkowe |
| Zachodnie stany USA 2 | Środkowo-zachodnie stany USA |
| Środkowo-zachodnie stany USA | Zachodnie stany USA 2 |

Aby uzyskać aktualne informacje o obsługiwane przez platformę Azure, zobacz [regiony platformy Azure](https://azure.microsoft.com/regions/).

>[!NOTE]  
> Region pomocniczy Virginia wersji dla instytucji rządowych USA jest Texas nam wersji dla instytucji rządowych. Wcześniej Virginia nam wersji dla instytucji rządowych wykorzystana Iowa nam wersji dla instytucji rządowych jako regionie pomocniczym. Równoczesnym wykorzystaniem Iowa nam wersji dla instytucji rządowych jako regionu pomocniczego konta magazynu są migrowane do nas wersji dla instytucji rządowych Texas jako region pomocniczej. 
> 
> 