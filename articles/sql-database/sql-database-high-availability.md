---
title: "Wysoka dostępność — usługa Azure SQL Database | Dokumentacja firmy Microsoft"
description: "Więcej informacji na temat możliwości wysokiej dostępności usługi baza danych SQL Azure i funkcje"
services: sql-database
author: anosov1960
manager: craigg
ms.service: sql-database
ms.topic: article
ms.date: 03/16/2018
ms.author: sashan
ms.reviewer: carlrab
ms.openlocfilehash: 8deb78ba108aafc3297e6b96d6d88d0c56c60afd
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/17/2018
---
# <a name="high-availability-and-azure-sql-database"></a>Baza danych SQL wysokiej dostępności i platformy Azure
Od chwili rozpoczęcia oferty PaaS bazy danych SQL Azure firma Microsoft wprowadziła Obietnica jej klientów, które wysokiej dostępności (HA) korzysta z wbudowanej w usługi i klientów nie są wymagane do działania, Dodaj logikę specjalnych lub podejmować decyzje dotyczące wysokiej dostępności. Firma Microsoft udostępnia pełną kontrolę nad HA system konfiguracji i obsłudze, zaoferować klientom umowy dotyczącej poziomu usług. HA umowy SLA stosuje się do bazy danych SQL w regionie i nie zapewnia ochrony w przypadku niepowodzenia obszar całkowity, który jest wystąpiły z przyczyn będących poza kontrolą firmy Microsoft (na przykład klęski żywiołowej, plik war, czynności terroryzmu, zamieszek, akcji dla instytucji rządowych, lub sieci lub na urządzeniu niepowodzenie zewnętrznych w centrach danych firmy Microsoft, w tym klientów w lokacjach lub między lokacjami klienta i centrum danych firmy Microsoft).

Aby uprościć miejsce problem wysokiej dostępności, firma Microsoft wykorzystuje następujące założenia:
1.  Awarie sprzętu i oprogramowania są nieuniknione
2.  Personel operacyjny popełnione, prowadzących do błędów
3.  Planowane operacji obsługi powodować awarie 

Gdy takie poszczególne zdarzenia są rzadko w skali chmury, ich co tydzień, jeśli nie każdego dnia. 

## <a name="fault-tolerant-sql-databases"></a>Odpornej na uszkodzenia bazy danych SQL
Klienci najbardziej interesujących odporność własnych baz danych i mniej planuje się odporności usługi baza danych SQL jako całość. czas działania 99,99% dla usługi nie ma znaczenia, jeśli "bazy danych" % 0,01 baz danych, które nie działają. Wszystkie bazy danych musi być odpornej na uszkodzenia i środki zaradcze błędów nigdy nie powinno spowodować utratę zatwierdzić transakcji. 

W przypadku danych bazy danych SQL korzysta z magazynu zdalnego (r) oparte na usłudze Azure Premium Storage stronicowe obiekty BLOB i Magazyn lokalny (LS) oparte na bezpośrednie dołączonych dysków/wirtualne dyski twarde. 
- Magazyn lokalny jest używana w baz danych Premium i pule, które są przeznaczone dla misji krytycznych aplikacji OLTP wysokiego IOPS. 
- Magazyn zdalny jest używana do Basic i Standard warstwy usług, przeznaczone dla obciążeń biznesowych budżetu ukierunkowane, które wymagają magazynu i mocy niezależne skalowanie obliczeniowej. Korzystają z jednego stronicowy obiekt blob dla plików dziennika i bazy danych i magazynu wbudowanych mechanizmów replikacji i trybu failover.

W obu przypadkach replikacji, wykrywanie błędów i pracy awaryjnej mechanizmów bazy danych SQL są w pełni zautomatyzowanego, a działanie bez udziału człowieka. Taka architektura jest przeznaczona do upewnij się, że przekazane dane nigdy nie zostaną utracone i że trwałość danych ma pierwszeństwo przed wszystkie inne.

Najważniejsze korzyści:
- Klienci w pełni wykorzystać zreplikowanych baz danych bez konieczności konfigurowania lub obsługa skomplikowane sprzętu, oprogramowania, systemów operacyjnych lub w środowiskach wirtualizacji.
- Właściwości ACID pełne relacyjnych baz danych są obsługiwane przez system.
- Praca awaryjna pełni są automatyzowane bez utraty danych zatwierdzone.
- Routing połączeń z repliką podstawową dynamicznie jest zarządzane przez usługę z logiką aplikacji, nie wymagane.
- Wysoki poziom nadmiarowości automatycznych podano bez dodatkowych opłat.

> [!NOTE]
> Architektura opisane wysokiej dostępności jest mogą ulec zmianie bez uprzedzenia. 

## <a name="data-redundancy"></a>Nadmiarowość danych

Rozwiązania wysokiej dostępności w bazie danych SQL jest oparty na [Always ON](/sql/database-engine/availability-groups/windows/always-on-availability-groups-sql-server) technologii z programu SQL Server i ułatwia działa zarówno LS, jak i RS baz danych z minimalnym różnice. W elemencie LS konfiguracji zawsze służy ON trwałości znajduje się w usługach RS jest ona używana w przypadku dostępności (RTO niski). 

## <a name="local-storage-configuration"></a>Lokalny magazyn konfiguracji

W tej konfiguracji każda baza danych jest przełączony w tryb online przez usługę zarządzania (MS) w kręgu formantu. Jedna replika podstawowa i co najmniej dwóch replik pomocniczych (zestawu kworum) znajdują się w pierścień dzierżawy obejmującej trzech niezależnych fizycznych podsystemami w tym samym centrum danych. Wszystkie odczyty i zapisy są wysyłane przez bramę (GW) do repliki podstawowej i zapisy asynchroniczne są replikowane w replikach pomocniczych. Baza danych SQL wykorzystuje schemat zatwierdzania na podstawie kworum, gdy dane są zapisywane do serwera podstawowego i co najmniej jedna replika pomocnicza przed zatwierdzeniem transakcji.

[Sieci szkieletowej usług](../service-fabric/service-fabric-overview.md) pracy awaryjnej systemu automatycznie odtwarza repliki, ponieważ awarii węzłów i przechowuje członkostwa zestawu kworum węzłów odbiegać i Dołącz do systemu. Planowana konserwacja jest dokładnie skoordynowany sposób, aby zapobiec zestawu kworum mniejszego niż liczba minimalna repliki (zwykle 2). Ten model działa dobrze w przypadku baz danych Premium, ale wymaga nadmiarowości zarówno do obliczeń, jak i magazynów składników i powoduje wyższe koszty.

## <a name="remote-storage-configuration"></a>Konfiguracja magazynu zdalnego

W przypadku konfiguracji zdalnego magazynu (warstwy Basic i Standard) dokładnie jeden kopia jest przechowywana w zdalnego magazynu obiektów blob, za pomocą funkcji systemów pamięci masowej trwałości, nadmiarowości i wykrywania bit zaprojektowana. 

W poniższym diagramie przedstawiono architekturę wysokiej dostępności:
 
![Architektura wysokiej dostępności](./media/sql-database-high-availability/high-availability-architecture.png)

## <a name="failure-detection-and-recovery"></a>Wykrywanie awarii i odzyskiwanie 
Na dużą skalę Rozproszony system potrzebuje systemu wykrywania awarii wysoce niezawodne, w którym można niezawodnie, wykrywanie błędów szybkie i możliwie najbliżej do klienta. W przypadku bazy danych SQL to system opiera się na sieć szkieletowa usług Azure. 

Z repliką podstawową jest od razu widoczne czy i kiedy nie powiodła się repliką podstawową i nie może kontynuować pracy, ponieważ wszystkie odczyty i zapisy została wykonana w replice podstawowej najpierw. Proces podwyższania poziomu repliki pomocniczej do stanu podstawowy ma celu czasu odzyskiwania (RTO) = 30 sekund i cel punktu odzyskiwania (RPO) = 0. Aby zmniejszyć wpływ 30 s RTO, najlepszym rozwiązaniem jest próbuj ponownie połączyć się z mniejszych czas oczekiwania połączenia nieudanych prób.

Gdy replika pomocnicza nie powiedzie się, baza danych jest dół minimalnego kworum zestaw, z nie zapasowego. Sieć szkieletowa usług zainicjuje proces ponownej konfiguracji podobny do procesu, znajdujący się na błąd repliką podstawową, więc po krótkim okresie oczekiwania do ustalenia, czy błąd jest trwały, innej repliki pomocniczej jest tworzony. W przypadku tymczasowego stanu usług, takich jak uszkodzenie systemu operacyjnego lub uaktualnienie nowej repliki niż wbudowany natychmiast umożliwia węzeł nie powiodło się, aby zamiast tego Uruchom ponownie. 

Funkcje Always ON do trybu failover baz danych konfiguracji magazynu zdalnego, jest używany podczas uaktualnienia bazy danych SQL. W tym nowe wystąpienie programu SQL jest wykonana wcześniej jako część zdarzenie planowane uaktualnienia i dołącza i odzyskuje plik bazy danych z magazynu zdalnego. W przypadku awarii procesu lub inne zdarzenia, nieplanowane Windows Fabric zarządza dostępności wystąpienia i jako ostatni krok odzyskiwania dołączenie pliku zdalnego.

## <a name="zone-redundant-configuration-preview"></a>Nadmiarowe konfiguracji strefy (wersja zapoznawcza)

Domyślnie replik zestawu kworum w przypadku konfiguracji z magazynu lokalnego zostaną utworzone w tym samym centrum danych. Wraz z wprowadzeniem [stref dostępności Azure](../availability-zones/az-overview.md), masz możliwość umieszczenia innej repliki w zestawach kworum dostępności różnych stref w tym samym regionie. Aby wyeliminować pojedynczy punkt awarii, pierścień kontroli również jest zduplikowany w wielu strefach jako trzy pierścienie bramy (GW). Routing do pierścień bramy jest kontrolowany przez [usługi Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md) (ATM). Ponieważ konfiguracji nadmiarowe strefy nie tworzy nadmiarowość dodatkowej bazy danych, użycie stref dostępności w warstwie usług Premium znajduje się w temacie bez dodatkowych kosztów. Wybierając nadmiarowa strefy baza danych, możesz wprowadzić baz danych Premium odporne do znacznie większy zestaw awarii, w tym datacenter poważnej awarii, bez wprowadzania żadnych zmian logiki aplikacji. Możesz również przeprowadzić konwersję istniejącej bazy danych — warstwa Premium ani puli konfiguracji nadmiarowe strefy.

Ponieważ strefy nadmiarowe kworum zestawie replik w różnych centrach danych, z niektórych odległość między nimi, opóźnienie sieci zwiększona może zwiększyć czas zatwierdzenia i w związku z tym wpływ na wydajność niektórych obciążeń OLTP. Możesz zawsze wrócić do konfiguracji strefy jednym wyłączenie ustawienia nadmiarowość strefy. Ten proces jest rozmiar operacji danych i jest podobny do aktualizacji (SLO), cel poziomu usługi regularne. Po zakończeniu procesu bazy danych lub puli migracji z nadmiarowych pierścień strefy pierścień jednej strefie lub odwrotnie.

> [!IMPORTANT]
> Strefy nadmiarowe baz danych i elastyczne pule są obsługiwane tylko w warstwie usług Premium. W publicznej wersji zapoznawczej, kopie zapasowe i inspekcji rekordy są przechowywane w magazynie RA-GRS i dlatego nie można automatycznie dostępne w przypadku awarii całej strefy. 

W poniższym diagramie przedstawiono wersja nadmiarowe strefy architektury wysokiej dostępności:
 
![Wysoka dostępność architektura obszar strefowo nadmiarowy](./media/sql-database-high-availability/high-availability-architecture-zone-redundant.png)

## <a name="conclusion"></a>Podsumowanie
Baza danych SQL Azure jest ściśle zintegrowana z platformą Azure i wysoce zależy od usługi sieć szkieletowa wykrywania awarii i odzyskiwania na obiektach blob magazynu Azure do ochrony danych i stref dostępności wyższych odporności na uszkodzenia. W tym samym czasie bazy danych Azure SQL w pełni wykorzystuje technologię zawsze włączone z programu SQL Server pole produkt replikacji i trybu failover. Kombinacja tych technologii umożliwia aplikacjom w pełni wykorzystać zalety modelu magazynu mieszane i obsługuje najbardziej wymagających umów SLA. 

## <a name="next-steps"></a>Kolejne kroki

- Dowiedz się więcej o [stref dostępności Azure](../availability-zones/az-overview.md)
- Dowiedz się więcej o [usługi sieci szkieletowej](../service-fabric/service-fabric-overview.md)
- Dowiedz się więcej o [usługi Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md) 
