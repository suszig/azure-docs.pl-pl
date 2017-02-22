---
title: Co to jest Azure SQL Data Warehouse? | Microsoft Docs
description: "Rozproszona baza danych klasy korporacyjnej, która może przetwarzać woluminy zawierające petabajty danych relacyjnych i nierelacyjnych. Jest to pierwszy magazyn danych w chmurze w branży, którym umożliwia powiększanie, zmniejszanie i wstrzymanie w ciągu kilku sekund."
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: bjhubbard
editor: 
ms.assetid: 4006c201-ec71-4982-b8ba-24bba879d7bb
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 10/31/2016
ms.author: jrj;mausher;kevin;barbkess
translationtype: Human Translation
ms.sourcegitcommit: 6241eb0e7ea091dffcb0ae770f8d89f24a19eb67
ms.openlocfilehash: ff2f688d42924edb1596cb2db474a58748f2b44c


---
# <a name="what-is-azure-sql-data-warehouse"></a>Co to jest Azure SQL Data Warehouse?
Azure SQL Data Warehouse to oparta na chmurze i skalowalna w poziomie baza danych, która może przetwarzać ogromne ilości danych relacyjnych i nierelacyjnych. Jest tworzona w oparciu o architekturę MPP, a więc może obsługiwać obciążenie przedsiębiorstwa.

SQL Data Warehouse:

* Stanowi połączenie funkcji relacyjnej bazy danych programu SQL Server i skalowalnych w poziomie funkcji chmury platformy Azure. W ciągu kilku sekund lub minut można zwiększyć, zmniejszyć, wstrzymać lub wznowić operacje obliczeniowe. Zmniejsza koszty przez skalowanie procesora CPU w poziomie, gdy wystąpi taka potrzeba, oraz ograniczenie jego użycia poza godzinami szczytu.
* Korzysta z platformy Azure. Jest łatwa do wdrożenia, bezproblemowa w utrzymaniu i w pełni odporna na uszkodzenia dzięki automatycznemu tworzeniu kopii zapasowych.
* Uzupełnia ekosystem programu SQL Server. Można programować za pomocą znanego rozszerzenia programu SQL Server Transact-SQL (T-SQL) i narzędzi.

W tym artykule opisano kluczowe funkcje usługi SQL Data Warehouse.

## <a name="massively-parallel-processing-architecture"></a>Architektura masowego przetwarzania równoległego
Usługa SQL Data Warehouse to system rozproszonych baz danych o architekturze masowego przetwarzana równoległego (MPP). Dzieląc dane i możliwości przetwarzania między wiele węzłów, usługa SQL Data Warehouse może zaoferować bardzo dużą skalowalność — znacznie większą niż w przypadku jakiegokolwiek pojedynczego systemu.  W tle usługa SQL Data Warehouse rozmieszcza dane na wielu nieudostępnianych jednostkach magazynowania i przetwarzania. Dane są przechowywane w magazynie lokalnie nadmiarowym warstwy Premium i połączone z węzłami obliczeniowymi w celu wykonywania zapytań. Dzięki tej architekturze w usłudze SQL Data Warehouse do uruchamiania obciążeń i złożonych zapytań zastosowano podejście „dziel i rządź”. Żądania są odbierane przez węzeł kontrolny, optymalizowane, a następnie przekazywane do węzłów obliczeniowych, gdzie wykonują swoje zadania równolegle.

Dzięki połączeniu architektury MPP i możliwości usługi Azure Storage usługa SQL Data Warehouse może:

* Zwiększać i zmniejszać magazyn niezależne od zasobów obliczeniowych.
* Zwiększać i zmniejszać zasoby obliczeniowe bez przenoszenia danych.
* Wstrzymywać wydajność obliczeniową przy jednoczesnym zachowaniu niezmienionych danych.
* Szybko wznawiać moc obliczeniową.

Na poniższym diagramie przedstawiono architekturę bardziej szczegółowo.

![Architektura usługi SQL Data Warehouse][1]

**Węzeł kontrolny:** Węzeł kontrolny optymalizuje zapytania i zarządza nimi. Jest to fronton współdziałający ze wszystkimi aplikacjami i połączeniami. W usłudze SQL Data Warehouse węzeł kontrolny jest obsługiwany przez usługę SQL Database i łączenie z nim wygląda tak samo. Pod powierzchnią węzeł kontrolny koordynuje w całości przenoszenie i obliczanie danych wymagane do uruchamiania zapytań równoległych względem danych rozproszonych. Po przesłaniu zapytania T-SQL do usługi SQL Data Warehouse węzeł kontrolny przekształca je w oddzielne zapytania, które są uruchamiane równolegle na każdym węźle obliczeniowym.

**Węzły obliczeniowe:** Węzły obliczeniowe zasilają usługę SQL Data Warehouse. Są one bazami danych SQL, które przechowują dane i przetwarzają zapytania. Po dodaniu danych usługa SQL Data Warehouse dystrybuuje wiersze do węzłów obliczeniowych. Węzły obliczeniowe to procesy robocze, które uruchamiają zapytania równoległe względem danych. Po zakończeniu przetwarzania przekazują wyniki z powrotem do węzła kontrolnego. Aby zakończyć wykonywanie zapytania, węzeł kontrolny agreguje wyniki i zwraca wynik końcowy.

**Magazyn:** Dane są przechowywane w usłudze Azure Blob Storage. Gdy węzły obliczeniowe współdziałają z danymi, zapisują i odczytują bezpośrednio do i z magazynu obiektów blob. Ponieważ magazyn Azure rozszerza się w sposób przezroczysty i znaczny, w usłudze SQL Data Warehouse dzieje się tak samo. Ponieważ zasoby obliczeniowe i magazyn są niezależne od siebie, usługa SQL Data Warehouse może automatycznie skalować magazyn niezależnie od skalowania zasobów obliczeniowych i na odwrót. Usługa Azure Blob Storage jest również całkowicie odporna na uszkodzenia oraz usprawnia proces tworzenia kopii zapasowych i przywracania danych.

**Data Movement Service:** Usługa Data Movement Service (DMS) przenosi dane między węzłami. Usługa DMS umożliwia węzłom obliczeniowym dostęp do danych, które są im niezbędne do sprzęgania i agregacji. Usługa DMS nie jest usługą platformy Azure. Jest to usługa systemu Windows uruchamiana razem z usługą SQL Database na wszystkich węzłach. Ponieważ usługa DMS uruchamia się w tle, użytkownik nie wchodzi z nią w bezpośrednią interakcję. Jednak jeśli przyjrzymy się planom zapytań, zauważymy, że zawierają one operacje DMS, gdyż przenoszenie danych jest niezbędne do uruchamiania każdego zapytania równolegle.

## <a name="optimized-for-data-warehouse-workloads"></a>Optymalizacja pod kątem obciążeń magazynu danych
Rozwiązanie MPP jest wspomagane pewną liczbą optymalizacji wydajności specyficznych dla magazynowania danych, takich jak:

* Optymalizator zapytań rozproszonych i zestaw złożonych statystyk obejmujących wszystkie dane. Przy użyciu informacji dotyczących rozmiaru i dystrybucji danych usługa może zoptymalizować zapytania przez ocenę kosztów określonych operacji zapytań rozproszonych.
* Zaawansowane algorytmy i techniki zintegrowane z procesem przenoszenia danych, aby wydajnie przenosić dane między zasobami obliczeniowymi, co jest niezbędne do wykonania zapytania. Te operacje przenoszenia danych są wbudowane, a wszystkie optymalizacje usługi Data Movement Service następują automatycznie.
* Domyślnie klastrowane indeksy **magazynu kolumn**. Za pomocą magazynu kolumn w usłudze SQL Data Warehouse można uzyskać średnio pięciokrotnie większe wzmocnienie kompresji w porównaniu z tradycyjnym zorientowanym na wiersze magazynem i nawet dziesięciokrotny (lub większy) wzrost wzmocnienia wydajności zapytań. Zapytania analityczne, które muszą przeskanować dużą liczbę wierszy, działają doskonale na indeksach magazynu kolumn.

## <a name="predictable-and-scalable-performance"></a>Przewidywalna i skalowalna wydajność
Usługa SQL Data Warehouse oddziela zasoby magazynowe od obliczeniowych, co umożliwia niezależne skalowanie obu elementów. Usługę SQL Data Warehouse można szybko i łatwo skalować w celu błyskawicznego dodania kolejnych zasobów obliczeniowych. Jako uzupełnienia tej usługi używa się usługi Azure Blob Storage. Obiekty blob nie tylko zapewniają stabilny, replikowany magazyn, ale również infrastrukturę umożliwiającą proste i niedrogie rozszerzanie. Dzięki tej kombinacji magazynu w skali chmury oraz zasobów obliczeniowych Azure usługa SQL Data Warehouse umożliwia naliczanie opłat za wykonywanie i przechowywanie zapytań tylko w razie potrzeby. Aby zmienić ilość zasobów obliczeniowych, wystarczy przesunąć suwak w witrynie Azure Portal w lewo lub w prawo, ale można tę operację również zaplanować za pomocą języka T-SQL i programu PowerShell.

Wraz z możliwością pełnej kontroli ilości zasobów obliczeniowych niezależnie od magazynu usługa SQL Data Warehouse pozwala na całkowite wstrzymanie magazynu danych, co oznacza, że nie trzeba płacić za zasoby obliczeniowe, gdy te nie są potrzebne. Magazyn pozostaje w niezmienionej formie, a wszystkie zasoby obliczeniowe zostają uwolnione do głównej puli platformy Azure, co daje oszczędności. W razie potrzeby po prostu wznów przetwarzanie i korzystaj z danych oraz zasobów obliczeniowych dla obciążenia.

## <a name="data-warehouse-units"></a>Jednostki magazynu danych
Przydzielanie zasobów do usługi SQL Data Warehouse jest mierzone w jednostkach magazynu danych (Data Warehouse Units, DWU). Jednostki DWU są miarą podstawowych zasobów, takich jak procesor CPU, pamięć i IOPS, które są przydzielone do usługi SQL Data Warehouse. Zwiększenie liczby jednostek DWU powoduje zwiększenie ilości zasobów i wydajności. Jednostki DWU pozwalają w szczególności zagwarantować, że:

* Możesz łatwo skalować dane, nie martwiąc się o podstawowy sprzęt ani oprogramowanie.
* Możesz przewidzieć wzrost wydajności dla poziomu jednostek DWU przed zmianą rozmiaru magazynu danych.
* Podstawowy sprzęt lub oprogramowanie wystąpienia mogą ulec zmianie lub zostać przeniesione bez wpływu na wydajność przetwarzania obciążenia.
* Firma Microsoft może wprowadzać zmiany w podstawowej architekturze usługi bez wpływu na wydajność przetwarzania obciążenia.
* Firma Microsoft może szybko poprawić wydajność usługi SQL Data Warehouse w sposób, który jest skalowalny i ma równomierny wpływ na system.

Jednostki magazynu danych udostępniają trzy dokładne metryki, które są ściśle powiązane z wydajnością przetwarzania obciążenia wynikającego z magazynowania danych. Celem jest, aby kluczowe metryki obciążenia mogły być skalowane liniowo z jednostkami DWU wybranymi dla magazynu danych.

**Skanowanie/agregacja:** ta metryka obciążenia używa standardowego zapytania magazynowania danych, które skanuje dużą liczbę wierszy, a następnie wykonuje złożoną agregację. Jest to operacja intensywnie korzystająca z operacji we/wy i procesora CPU.

**Obciążenie** ta metryka mierzy możliwość pozyskiwania danych w usłudze. Obciążenia są wykonywane przy użyciu programu PolyBase, który ładuje reprezentatywny zestaw danych z usługi Azure Blob Storage. Ta metryka obciąża aspekt sieciowy i procesora CPU usługi.

**Create Table As Select (CTAS):** metryka CTAS mierzy możliwość tworzenia kopii tabeli. Obejmuje to odczytywanie danych z magazynu, ich dystrybucję w węzłach urządzenia i ponowne zapisywanie w magazynie. Jest to operacja intensywnie korzystająca z procesora CPU, we/wy i sieci.

## <a name="pause-and-scale-on-demand"></a>Wstrzymywanie i skalowanie na żądanie
Jeśli potrzebujesz szybszych efektów, zwiększ liczbę jednostek DWU i opłać większą wydajność. Jeśli potrzebujesz mniejszej mocy obliczeniowej, zmniejsz liczbę jednostek DWU i zapłać tylko za to, co jest potrzebne. Możesz rozważyć zmianę liczby jednostek DWU w następujących scenariuszach:

* Gdy nie musisz uruchamiać zapytań, na przykład wieczorami lub w weekendy, przełącz zapytania w stan spoczynku. Następnie wstrzymaj zasoby obliczeniowe, aby uniknąć płacenia za jednostki DWU, gdy nie są one potrzebne.
* Gdy zapotrzebowanie systemu jest niskie, rozważ zmniejszenie liczby jednostek DWU. Dostęp do danych nadal będzie możliwy, ale znacznie tańszy.
* Podczas wykonywania rozległych operacji ładowania i przekształcania danych warto skalować w górę, aby zapewnić szybszy dostęp do danych.

Aby przekonać się, jaka jest idealna wartość DWU, skaluj w górę i w dół oraz uruchom kilka zapytań po załadowaniu danych. Ponieważ skalowanie odbywa się szybko, możesz wypróbować różne poziomy wydajności w godzinę lub mniej.  Należy pamiętać, że usługę SQL Data Warehouse zaprojektowano pod kątem przetwarzania dużych ilości danych, dlatego, aby poznać pełnię jej możliwości skalowania — zwłaszcza w przypadku wyższych skal oferowanych przez firmę Microsoft — należy używać dużego zestawu danych wynoszącego lub przekraczającego 1 TB.

## <a name="built-on-sql-server"></a>Oparta na programie SQL Server
Usługa SQL Data Warehouse jest oparta na aparacie relacyjnej bazy danych programu SQL Server i zawiera wiele funkcji, których oczekuje się od magazynu danych przedsiębiorstwa. Jeśli znasz już język T-SQL, łatwo wykorzystasz tę wiedzę w usłudze SQL Data Warehouse. Niezależnie od tego, czy jesteś użytkownikiem zaawansowanym, czy dopiero zaczynasz, przykłady przedstawione w dokumentacji ułatwią rozpoczęcie pracy. Ogólnie należy zwrócić uwagę na sposób, w jaki tworzymy elementy językowe w usłudze SQL Data Warehouse:

* Usługa SQL Data Warehouse używa składni języka T-SQL do wykonywania wielu operacji. Obsługuje także szeroką gamę tradycyjnych struktur SQL, takich jak procedury składowane, funkcje zdefiniowane przez użytkownika, partycjonowanie tabel, indeksy i sortowanie.
* Usługa SQL Data Warehouse zawiera także pewną liczbę nowszych funkcji programu SQL Server, takich jak klastrowane indeksy **magazynu kolumn**, integracja programu PolyBase oraz inspekcja danych (wraz z oceną zagrożenia).
* Niektóre elementy języka T-SQL, które są mniej typowe dla obciążeń magazynowania danych lub są nowsze dla programu SQL Server, mogą być obecnie niedostępne. Aby uzyskać więcej informacji, zapoznaj się z [dokumentacją dotyczącą migracji][Migration documentation].

Przy użyciu języka Transact-SQL i korzystając z tych samych funkcji występujących w usługach SQL Server, SQL Data Warehouse, SQL Database i systemie Analytics Platform System, można opracować rozwiązanie, które odpowiada potrzebom związanym z danymi. Można określić miejsce przechowywania danych na podstawie wydajności, zabezpieczeń i wymagań skali, a następnie w zależności od potrzeb przekazać dane między różnymi systemami.

## <a name="data-protection"></a>Ochrona danych
Usługa SQL Data Warehouse przechowuje wszystkie dane w lokalnie nadmiarowym magazynie platformy Azure w warstwie Premium. Wiele synchronicznych kopii danych jest przechowywanych w lokalnym centrum danych, co gwarantuje przezroczystą ochronę danych w przypadku zlokalizowanych awarii. Ponadto usługa SQL Data Warehouse automatycznie tworzy kopie zapasowe aktywnych (niewstrzymanych) baz danych w regularnych odstępach czasu przy użyciu programu Azure Storage Snapshots. Aby dowiedzieć się więcej na temat zasad tworzenia kopii zapasowych i przywracania, zobacz artykuł [Backup and restore overview][Backup and restore overview] (Omówienie tworzenia kopii zapasowych i przywracania).

## <a name="integrated-with-microsoft-tools"></a>Integracja z narzędziami firmy Microsoft
Usługa SQL Data Warehouse jest zintegrowana z wieloma narzędziami, które mogą już być znane użytkownikom programu SQL Server. Należą do nich:

**Tradycyjne narzędzia programu SQL Server:** Usługa SQL Data Warehouse jest w pełni zintegrowana z usługami SQL Server Analysis Services, Integration Services i Reporting Services.

**Narzędzia oparte na chmurze:** Usługi SQL Data Warehouse można używać razem z szeregiem nowych narzędzi platformy Azure, takimi jak Data Factory, Stream Analytics, Machine Learning oraz z usługą Power BI. Bardziej kompletny wykaz znajdziesz w artykule [Integrated tools overview][Integrated tools overview] (Przegląd zintegrowanych narzędzi).

**Narzędzia innych firm:** Duża liczba innych firm dostarczających narzędzia ma certyfikat integracji swoich narzędzi z usługą SQL Data Warehouse. Pełną listę znajdziesz w artykule [SQL Data Warehouse solution partners][SQL Data Warehouse solution partners] (Partnerzy rozwiązania SQL Data Warehouse).

## <a name="hybrid-data-sources-scenarios"></a>Hybrydowe scenariusze źródeł danych
Korzystanie z usługi SQL Data Warehouse z programem PolyBase zapewnia użytkownikom niezrównaną możliwość przenoszenia danych w ekosystemie, umożliwiając konfigurowanie zaawansowanych scenariuszy hybrydowych obejmujących źródła danych nierelacyjnych i lokalnych.

Program PolyBase umożliwia wykorzystywanie danych z innych źródeł przy użyciu znanych poleceń języka T-SQL. Program PolyBase pozwala przesyłać zapytania dotyczące danych nierelacyjnych przechowywanych w usłudze Azure Blob Storage, traktując go jak zwykłą tabelę. Używaj programu PolyBase do przesyłania zapytań dotyczących danych nierelacyjnych lub importowania danych nierelacyjnych do usługi SQL Data Warehouse.

* Program PolyBase używa tabel zewnętrznych w celu uzyskania dostępu do danych nierelacyjnych. Definicje tabel są przechowywane w usłudze SQL Data Warehouse i możesz uzyskać do nich dostęp za pomocą języka SQL i narzędzi w taki sposób, w jaki uzyskuje się dostęp do zwykłych danych relacyjnych.
* Program PolyBase jest niewymagający pod względem integracji. Ma takie same funkcje i funkcjonalność niezależnie od obsługiwanego źródła. Dane odczytywane w programie PolyBase mogą być w różnych formatach, np. w formie plików rozdzielanych lub plików ORC.
* Program PolyBase może służyć do uzyskiwania dostępu do magazynu obiektów blob, który jest również używany jako magazyn klastra usługi HD Insight. Dzięki temu możesz uzyskiwać dostęp do tych samych danych za pomocą narzędzi relacyjnych i nierelacyjnych.

## <a name="sla"></a>Umowa SLA
Usługa SQL Data Warehouse oferuje na poziomie produktu umowę dotyczącą poziomu usług (SLA) w ramach umowy SLA usług Microsoft Online Services. Aby uzyskać więcej informacji, zobacz [SLA for SQL Data Warehouse][SLA for SQL Data Warehouse] (Usługa SQL Data Warehouse — umowa SLA). Informacje o innych produktach dotyczące umowy SLA można znaleźć na stronie [Umowy dotyczące poziomu usług] platformy Azure lub pobrać je ze strony [Licencjonowanie zbiorowe][Volume Licensing]. 

## <a name="next-steps"></a>Następne kroki
Teraz, gdy masz już podstawową wiedzę na temat usługi SQL Data Warehouse, możesz dowiedzieć się, jak szybko [utworzyć bazę danych w usłudze SQL Data Warehouse][create a SQL Data Warehouse] i [ładowanie danych przykładowych][load sample data]. Jeśli dopiero zaczynasz korzystać z platformy Azure, [słownik platformy Azure][Azure glossary] może pomóc Ci zaznajomić się z nową terminologią. Możesz też zwrócić uwagę na inne zasoby dotyczące usługi SQL Data Warehouse.  

* [Historie sukcesu klientów]
* [Blogi]
* [Żądania funkcji]
* [Filmy wideo]
* [Blogi zespołu doradczego klientów]
* [Tworzenie biletu pomocy technicznej]
* [Forum MSDN]
* [Forum Stack Overflow]
* [Twitter]

<!--Image references-->
[1]: ./media/sql-data-warehouse-overview-what-is/dwarchitecture.png

<!--Article references-->
[Tworzenie biletu pomocy technicznej]: ./sql-data-warehouse-get-started-create-support-ticket.md
[load sample data]: ./sql-data-warehouse-load-sample-databases.md
[create a SQL Data Warehouse]: ./sql-data-warehouse-get-started-provision.md
[Migration documentation]: ./sql-data-warehouse-overview-migrate.md
[SQL Data Warehouse solution partners]: ./sql-data-warehouse-partner-business-intelligence.md
[Integrated tools overview]: ./sql-data-warehouse-overview-integrate.md
[Backup and restore overview]: ./sql-data-warehouse-restore-database-overview.md
[Azure glossary]: ../azure-glossary-cloud-terminology.md

<!--MSDN references-->

<!--Other Web references-->
[Historie sukcesu klientów]: https://customers.microsoft.com/search?sq=&ff=story_products_services%26%3EAzure%2FAzure%2FAzure%20SQL%20Data%20Warehouse%26%26story_product_families%26%3EAzure%2FAzure%26%26story_product_categories%26%3EAzure&p=0
[Blogi]: https://azure.microsoft.com/blog/tag/azure-sql-data-warehouse/
[Blogi zespołu doradczego klientów]: https://blogs.msdn.microsoft.com/sqlcat/tag/sql-dw/
[Żądania funkcji]: https://feedback.azure.com/forums/307516-sql-data-warehouse
[Forum MSDN]: https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=AzureSQLDataWarehouse
[Forum Stack Overflow]: http://stackoverflow.com/questions/tagged/azure-sqldw
[Twitter]: https://twitter.com/hashtag/SQLDW
[Filmy wideo]: https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse
[SLA for SQL Data Warehouse]: https://azure.microsoft.com/en-us/support/legal/sla/sql-data-warehouse/v1_0/
[Volume Licensing]: http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=37
[Umowy dotyczące poziomu usług]: https://azure.microsoft.com/en-us/support/legal/sla/



<!--HONumber=Jan17_HO3-->


