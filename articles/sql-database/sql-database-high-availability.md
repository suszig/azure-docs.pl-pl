---
title: "Wysoka dostępność — usługa Azure SQL Database | Dokumentacja firmy Microsoft"
description: "Więcej informacji na temat możliwości wysokiej dostępności usługi baza danych SQL Azure i funkcje"
keywords: 
services: sql-database
documentationcenter: 
author: anosov1960
manager: jhubbard
ms.assetid: 
ms.service: sql-database
ms.custom: 
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: Inactive
ms.date: 12/13/2017
ms.author: sashan
ms.reviewer: carlrab
ms.openlocfilehash: c0a140c959f14c2e8ceaddad5d323f0900be5d2f
ms.sourcegitcommit: fa28ca091317eba4e55cef17766e72475bdd4c96
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/14/2017
---
# <a name="high-availability-and-azure-sql-database"></a>Baza danych SQL wysokiej dostępności i platformy Azure
Od chwili rozpoczęcia oferty PaaS bazy danych SQL Azure firma Microsoft wprowadziła Obietnica dla naszych klientów, które wysokiej dostępności (HA) korzysta z wbudowanej w usługi, a klienci są nie jest wymagana do obsługi, Dodaj logikę specjalne lub podejmować decyzje dotyczące wysokiej dostępności. Firma Microsoft oferuje klientom umowy dotyczącej poziomu usług i zachować pełną kontrolę nad HA system konfiguracji i obsłudze. HA umowy SLA stosuje się do bazy danych SQL w regionie i nie zapewnia ochrony w przypadku awarii obszar całkowity będących czynnikami poza naszych kontrolą (na przykład klęski żywiołowej, plik war, działań terroryzmu zamieszek, akcji dla instytucji rządowych albo sieci lub Niepowodzenie urządzeń zewnętrznych w naszych centrach danych, w tym w lokacji klienta lub między lokacją klienta i przejdź do naszego centrum danych).

Aby uprościć miejsce problem wysokiej dostępności, firma Microsoft wykorzystuje następujące założenia:
1.  Awarie sprzętu i oprogramowania są nieuniknione
2.  Personel operacyjny popełnione, prowadzących do błędów
3.  Planowane operacji obsługi powodować awarie 

Gdy takie poszczególne zdarzenia są rzadko w skali chmury, ich co tydzień, jeśli nie każdego dnia. 

## <a name="fault-tolerant-sql-databases"></a>Odpornej na uszkodzenia bazy danych SQL
Klienci najbardziej interesujących odporność własnych baz danych i mniej planuje się odporności usługi baza danych SQL jako całość. czas działania 99,99% dla usługi nie ma znaczenia, jeśli "bazy danych" % 0,01 baz danych, które nie działają. Wszystkie bazy danych musi być odpornej na uszkodzenia i środki zaradcze błędów nigdy nie powinno spowodować utratę zatwierdzić transakcji. 

W przypadku danych bazy danych SQL korzysta z magazynu zdalnego (r) oparte na usłudze Azure Premium Storage stronicowe obiekty BLOB i Magazyn lokalny (LS) oparte na bezpośrednie dołączonych dysków/wirtualne dyski twarde. 
- Magazyn lokalny jest używana w baz danych Premium i pule, które są przeznaczone dla aplikacji OLTP wysokiego IOPS. 
- Magazyn zdalny jest używana do Basic i Standard warstwy usług, przeznaczone dla małych, zimnych i dużych baz danych, które wymagają magazynu i mocy niezależne skalowanie obliczeniowej. Korzystają z jednego stronicowy obiekt blob dla plików dziennika i bazy danych i magazynu wbudowanych mechanizmów replikacji i trybu failover.

W obu przypadkach replikacji, wykrywanie błędów i pracy awaryjnej mechanizmów bazy danych SQL są w pełni zautomatyzowanego, a działanie bez udziału człowieka. Taka architektura jest przeznaczona do upewnij się, że przekazane dane nigdy nie zostaną utracone i że trwałość danych ma pierwszeństwo przed wszystkie inne.

Najważniejsze korzyści:
- Klienci w pełni wykorzystać zreplikowanych baz danych bez konieczności konfigurowania lub obsługa skomplikowane sprzętu, oprogramowania, systemu operacyjnego lub w środowiskach wirtualizacji.
- Właściwości ACID pełne relacyjnych baz danych są obsługiwane przez system.
- Praca awaryjna pełni są automatyzowane bez utraty danych zatwierdzone.
- Routing połączeń z repliką podstawową dynamicznie jest zarządzane przez usługę z logiką aplikacji, nie wymagane.
- Wysoki poziom nadmiarowości automatycznych podano bez dodatkowych opłat.

> [!NOTE]
> Architektura opisane wysokiej dostępności jest mogą ulec zmianie bez uprzedzenia. 

## <a name="data-redundancy"></a>Nadmiarowość danych

Rozwiązania wysokiej dostępności w bazie danych SQL jest oparty na [AlwaysON](/sql/database-engine/availability-groups/windows/always-on-availability-groups-sql-server) technologii z programu SQL Server i ułatwia działa zarówno LS, jak i RS baz danych z minimalnym różnice. W konfiguracji LS (AlwaysOn) służy do trwałości znajduje się w usługach RS jest używany dla dostępności (RTO niski). 

## <a name="local-storage"></a>Lokalny magazyn

W przypadku LS każda baza danych jest przełączony w tryb online przez usługę zarządzania (MS) w kręgu formantu. Jedna replika podstawowa i co najmniej dwóch replik pomocniczych (zestawu kworum) znajdują się w pierścień dzierżawy obejmującej trzech niezależnych fizycznych podsystemami w tym samym centrum danych. Wszystkie odczyty i zapisy są wysyłane przez bramę (GW) do repliki podstawowej i zapisy asynchroniczne są replikowane w replikach pomocniczych. Baza danych SQL wykorzystuje schemat zatwierdzania na podstawie kworum, gdy dane są zapisywane do serwera podstawowego i co najmniej jedna replika pomocnicza przed zatwierdzeniem transakcji.

[Sieci szkieletowej usług](/azure/service-fabric/service-fabric-overview.md) pracy awaryjnej systemu automatycznie odtwarza repliki, ponieważ awarii węzłów i przechowuje członkostwa zestawu kworum węzłów odbiegać i Dołącz do systemu. Planowana konserwacja jest dokładnie skoordynowany sposób, aby zapobiec zestawu kworum mniejszego niż liczba minimalna repliki (zwykle 2). Ten model działa dobrze w przypadku baz danych Premium, ale wymaga nadmiarowości zarówno do obliczeń, jak i magazynów składników i powoduje wyższe koszty.

## <a name="remote-storage"></a>Magazyn zdalny

W przypadku konfiguracji zdalnego magazynu (warstwy Basic i Standard) dokładnie jedną kopię bazy danych jest przechowywana w zdalnego magazynu obiektów blob, korzystania z możliwości systemów pamięci masowej trwałości, nadmiarowości i wykrywania bit zaprojektowana. 

W poniższym diagramie przedstawiono architekturę wysokiej dostępności:
 
![Architektura wysokiej dostępności](./media/sql-database-high-availability/high-availability-architecture.png)

## <a name="failure-detection--recovery"></a>Wykrywanie błędów i odzyskiwanie 
Na dużą skalę Rozproszony system potrzebuje systemu wykrywania awarii wysoce niezawodne, w którym można niezawodnie, wykrywanie błędów szybkie i możliwie najbliżej do klienta. W przypadku bazy danych SQL to system opiera się na sieć szkieletowa usług Azure. 

Z repliką podstawową jest od razu widoczne czy i kiedy nie powiodła się repliką podstawową i nie może kontynuować pracy, ponieważ wszystkie odczyty i zapisy została wykonana w replice podstawowej najpierw. Proces podwyższania poziomu repliki pomocniczej do stanu podstawowy ma celu czasu odzyskiwania (RTO) = 30 sekund i cel punktu odzyskiwania (RPO) = 0. Aby zmniejszyć wpływ 30 s RTO, najlepszym rozwiązaniem jest próbuj ponownie połączyć się z mniejszych czas oczekiwania połączenia nieudanych prób.

Gdy replika pomocnicza nie powiedzie się, baza danych jest dół minimalnego kworum zestaw, z nie zapasowego. Sieć szkieletowa usług zainicjuje proces ponownej konfiguracji podobny do procesu, znajdujący się na błąd repliką podstawową, więc po krótkim okresie oczekiwania do ustalenia, czy błąd jest trwały, innej repliki pomocniczej jest tworzony. W przypadku tymczasowego stanu usług, takich jak uszkodzenie systemu operacyjnego lub uaktualnienie nowej repliki niż wbudowany natychmiast umożliwia węzeł nie powiodło się, aby zamiast tego Uruchom ponownie. 

Funkcję AlwaysON do trybu failover baz danych konfiguracji magazynu zdalnego, jest używany podczas uaktualniania bazy danych SQL. W tym nowe wystąpienie programu SQL jest wykonana wcześniej jako część zdarzenie planowane uaktualnienia i dołącza i odzyskuje plik bazy danych z magazynu zdalnego. W przypadku awarii procesu lub inne zdarzenia, nieplanowane Windows Fabric zarządza dostępności wystąpienia i jako ostatni krok odzyskiwania dołączenie pliku zdalnego.

## <a name="conclusion"></a>Podsumowanie
Bazy danych SQL Azure jest ściśle zintegrowana z platformą Azure i jest wysoce zależnych na sieć szkieletowa usług wykrywania awarii i odzyskiwania oraz na obiektach blob magazynu Azure do ochrony danych. W tym samym czasie bazy danych Azure SQL korzysta z technologii AlwaysOn z produktu okno programu SQL Server dla replikacji i trybu failover. Kombinacja tych technologii umożliwia aplikacjom w pełni wykorzystać zalety modelu magazynu mieszane i obsługuje najbardziej wymagających umów SLA. 

## <a name="next-steps"></a>Następne kroki

- Dowiedz się więcej o [usługi sieci szkieletowej](/azure/service-fabric/service-fabric-overview.md)
- Dowiedz się więcej o [usługi Azure Traffic Manager](/traffic-manager/traffic-manager-overview.md) 
