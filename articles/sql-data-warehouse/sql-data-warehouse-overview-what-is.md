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
ms.date: 2/28/2017
ms.author: jrj;mausher;kevin;barbkess;elbutter
translationtype: Human Translation
ms.sourcegitcommit: bf73ad830226626ddf41cc4ae80e714abf8bcfc2
ms.openlocfilehash: 19e87c61493bd4620120b39a533e9e4b64517538
ms.lasthandoff: 03/01/2017


---
# <a name="what-is-azure-sql-data-warehouse"></a>Co to jest Azure SQL Data Warehouse?
Azure SQL Data Warehouse to oparta na chmurze i skalowalna w poziomie relacyjna baza danych masowego przetwarzania równoległego (MPP, Massively Parallel Processing), która może przetwarzać ogromne ilości danych. 

SQL Data Warehouse:

* Stanowi połączenie funkcji relacyjnej bazy danych programu SQL Server i skalowalnych w poziomie funkcji chmury platformy Azure. 
* Oddziela magazyn od zasobów obliczeniowych.
* Umożliwia zwiększanie, zmniejszanie, wstrzymywanie i wznawianie procesów obliczeniowych. 
* Integruje się na całej platformie Azure.
* Korzysta z programu SQL Server Transact-SQL (T-SQL) i narzędzi.
* Jest zgodna z różnymi wymaganiami dotyczącymi zabezpieczeń prawnych oraz biznesowych, takich jak SOC i ISO.

W tym artykule opisano kluczowe funkcje usługi SQL Data Warehouse.

## <a name="massively-parallel-processing-architecture"></a>Architektura masowego przetwarzania równoległego
Usługa SQL Data Warehouse to system rozproszonych baz danych o architekturze masowego przetwarzana równoległego (MPP). W tle usługa SQL Data Warehouse rozmieszcza dane na wielu nieudostępnianych jednostkach magazynowania i przetwarzania. Dane są przechowywane w warstwie Premium w magazynie lokalnie nadmiarowym, na podstawie której są wykonywane zapytania połączonych dynamicznie węzłów obliczeniowych. W usłudze SQL Data Warehouse do uruchamiania obciążeń i złożonych zapytań zastosowano podejście „dziel i rządź”. Żądania są odbierane przez węzeł kontrolny, optymalizowane na potrzeby dystrybucji, a następnie przekazywane do węzłów obliczeniowych, gdzie wykonują swoje zadania równolegle.

Dzięki oddzieleniu magazynu od zasobów obliczeniowych usługa SQL Data Warehouse może wykonywać następujące zadania:

* Zwiększać i zmniejszać rozmiar magazynu niezależne od zasobów obliczeniowych.
* Zwiększać i zmniejszać moc obliczeniową bez przenoszenia danych.
* Wstrzymywać moc obliczeniową bez zmieniania danych, płacąc tylko za przestrzeń dyskową.
* Wznawiać moc obliczeniową w godzinach działania.

Na poniższym diagramie przedstawiono architekturę bardziej szczegółowo.

![Architektura usługi SQL Data Warehouse][1]

**Węzeł kontrolny:** Węzeł kontrolny optymalizuje zapytania i zarządza nimi. Jest to fronton współdziałający ze wszystkimi aplikacjami i połączeniami. W usłudze SQL Data Warehouse węzeł kontrolny jest obsługiwany przez usługę SQL Database i łączenie z nim wygląda tak samo. Pod powierzchnią węzeł kontrolny koordynuje w całości przenoszenie i obliczanie danych wymagane do uruchamiania zapytań równoległych względem danych rozproszonych. Po przesłaniu zapytania T-SQL do usługi SQL Data Warehouse węzeł kontrolny przekształca je w oddzielne zapytania, które są uruchamiane równolegle na każdym węźle obliczeniowym.

**Węzły obliczeniowe:** Węzły obliczeniowe zasilają usługę SQL Data Warehouse. Są one bazami danych SQL, które przechowują dane i przetwarzają zapytania. Po dodaniu danych usługa SQL Data Warehouse dystrybuuje wiersze do węzłów obliczeniowych. Węzły obliczeniowe to procesy robocze, które uruchamiają zapytania równoległe względem danych. Po zakończeniu przetwarzania przekazują wyniki z powrotem do węzła kontrolnego. Aby zakończyć wykonywanie zapytania, węzeł kontrolny agreguje wyniki i zwraca wynik końcowy.

**Magazyn:** Dane są przechowywane w usłudze Azure Blob Storage. Gdy węzły obliczeniowe współdziałają z danymi, zapisują i odczytują bezpośrednio do i z magazynu obiektów blob. Ponieważ magazyn Azure rozszerza się w sposób przezroczysty i znaczny, w usłudze SQL Data Warehouse dzieje się tak samo. Ponieważ zasoby obliczeniowe i magazyn są niezależne od siebie, usługa SQL Data Warehouse może automatycznie skalować magazyn niezależnie od skalowania zasobów obliczeniowych i na odwrót. Usługa Azure Blob Storage jest również całkowicie odporna na uszkodzenia oraz usprawnia proces tworzenia kopii zapasowych i przywracania danych.

**Data Movement Service:** Usługa Data Movement Service (DMS) przenosi dane między węzłami. Usługa DMS umożliwia węzłom obliczeniowym dostęp do danych, które są im niezbędne do sprzęgania i agregacji. Usługa DMS nie jest usługą platformy Azure. Jest to usługa systemu Windows uruchamiana razem z usługą SQL Database na wszystkich węzłach. Usługa DMS to proces działający w tle, z którym nie jest możliwa bezpośrednia interakcja. Można jednak przyjrzeć się planom zapytań i na ich podstawie odczytać, kiedy są uruchamiane operacje DMS, gdyż przenoszenie danych jest niezbędne do uruchamiania każdego zapytania równolegle.

## <a name="optimized-for-data-warehouse-workloads"></a>Optymalizacja pod kątem obciążeń magazynu danych
Rozwiązanie MPP jest wspomagane przez kilka optymalizacji wydajności specyficznych dla magazynowania danych, takich jak:

* Optymalizator zapytań rozproszonych i zestaw złożonych statystyk obejmujących wszystkie dane. Przy użyciu informacji dotyczących rozmiaru i dystrybucji danych usługa może zoptymalizować zapytania przez ocenę kosztów określonych operacji zapytań rozproszonych.
* Zaawansowane algorytmy i techniki zintegrowane z procesem przenoszenia danych, aby wydajnie przenosić dane między zasobami obliczeniowymi, co jest niezbędne do wykonania zapytania. Te operacje przenoszenia danych są wbudowane, a wszystkie optymalizacje usługi Data Movement Service następują automatycznie.
* Domyślnie klastrowane indeksy **magazynu kolumn**. Za pomocą magazynu kolumn w usłudze SQL Data Warehouse można uzyskać średnio pięciokrotnie większe wzmocnienie kompresji w porównaniu z tradycyjnym zorientowanym na wiersze magazynem i nawet dziesięciokrotny (lub większy) wzrost wzmocnienia wydajności zapytań. Zapytania analityczne, które muszą przeskanować dużą liczbę wierszy, działają lepiej na indeksach magazynu kolumn.


## <a name="predictable-and-scalable-performance-with-data-warehouse-units"></a>Przewidywalna i skalowalna wydajność dzięki jednostkom magazynu danych
W usłudze SQL Data Warehouse są wykorzystywane podobne technologie co w usłudze SQL Database, dzięki czemu użytkownicy mogą oczekiwać spójnej i przewidywalnej wydajności zapytań analitycznych. Z punktu widzenia użytkowników wydajność powinna być skalowana liniowo, kiedy dodają oni lub odejmują węzły obliczeniowe. Przydzielanie zasobów do usługi SQL Data Warehouse jest mierzone w jednostkach magazynu danych (Data Warehouse Units, DWU). Jednostki DWU są miarą podstawowych zasobów, takich jak procesor CPU, pamięć i IOPS, które są przydzielone do usługi SQL Data Warehouse. Zwiększenie liczby jednostek DWU powoduje zwiększenie ilości zasobów i wydajności. Jednostki DWU pozwalają w szczególności zagwarantować, że:

* Możesz skalować magazyn danych, nie martwiąc się o podstawowy sprzęt ani oprogramowanie.
* Możesz przewidzieć wzrost wydajności dla poziomu jednostek DWU przed zmianą mocy obliczeniowej magazynu danych.
* Podstawowy sprzęt lub oprogramowanie wystąpienia mogą ulec zmianie lub zostać przeniesione bez wpływu na wydajność przetwarzania obciążenia.
* Firma Microsoft może udoskonalić podstawową architekturę usługi bez wpływu na wydajność przetwarzania obciążenia.
* Firma Microsoft może szybko poprawić wydajność usługi SQL Data Warehouse w sposób, który jest skalowalny i ma równomierny wpływ na system.

Jednostki magazynu danych udostępniają trzy metryki, które są ściśle powiązane z wydajnością przetwarzania obciążenia wynikającego z magazynowania danych. Następujące kluczowe metryki dotyczące obciążenia są skalowane liniowo względem liczby jednostek DWU.

**Skanowanie/agregacja:** standardowe zapytanie magazynowania danych, które skanuje dużą liczbę wierszy, a następnie wykonuje złożoną agregację. Jest to operacja intensywnie korzystająca z operacji we/wy i procesora CPU.

**Obciążenie** możliwość pozyskiwania danych w usłudze. Obciążenia są obsługiwane z największą wydajnością za pomocą programu PolyBase z obiektów blob usługi Azure Storage lub z usługi Azure Data Lake. Ta metryka obciąża aspekt sieciowy i procesora CPU usługi.

**Create Table As Select (CTAS):** metryka CTAS mierzy możliwość tworzenia kopii tabeli. Obejmuje to odczytywanie danych z magazynu, ich dystrybucję w węzłach urządzenia i ponowne zapisywanie w magazynie. Jest to operacja intensywnie korzystająca z procesora CPU, we/wy i sieci.

## <a name="built-on-sql-server"></a>Oparta na programie SQL Server
Usługa SQL Data Warehouse jest oparta na aparacie relacyjnej bazy danych programu SQL Server i zawiera wiele funkcji, których oczekuje się od magazynu danych przedsiębiorstwa. Jeśli znasz już język T-SQL, łatwo wykorzystasz tę wiedzę w usłudze SQL Data Warehouse. Niezależnie od tego, czy jesteś użytkownikiem zaawansowanym, czy dopiero zaczynasz, przykłady przedstawione w dokumentacji ułatwią rozpoczęcie pracy. Ogólnie należy zwrócić uwagę na sposób, w jaki tworzymy elementy językowe w usłudze SQL Data Warehouse:

* Usługa SQL Data Warehouse używa składni języka T-SQL do wykonywania wielu operacji. Obsługuje także szeroką gamę tradycyjnych struktur SQL, takich jak procedury składowane, funkcje zdefiniowane przez użytkownika, partycjonowanie tabel, indeksy i sortowanie.
* Usługa SQL Data Warehouse zawiera także różne nowsze funkcje programu SQL Server, takie jak klastrowane indeksy **magazynu kolumn**, integracja programu PolyBase oraz inspekcja danych (wraz z oceną zagrożenia).
* Niektóre elementy języka T-SQL, które są mniej typowe dla obciążeń magazynowania danych lub są nowsze dla programu SQL Server, mogą być obecnie niedostępne. Aby uzyskać więcej informacji, zapoznaj się z [dokumentacją dotyczącą migracji][Migration documentation].

Przy użyciu języka Transact-SQL i korzystając z tych samych funkcji występujących w usługach SQL Server, SQL Data Warehouse, SQL Database i systemie Analytics Platform System, można opracować rozwiązanie, które odpowiada potrzebom związanym z danymi. Można określić miejsce przechowywania danych na podstawie wydajności, zabezpieczeń i wymagań skali, a następnie w zależności od potrzeb przekazać dane między różnymi systemami.

## <a name="data-protection"></a>Ochrona danych
Usługa SQL Data Warehouse przechowuje wszystkie dane w lokalnie nadmiarowym magazynie platformy Azure w warstwie Premium. Wiele synchronicznych kopii danych jest przechowywanych w lokalnym centrum danych, co gwarantuje przezroczystą ochronę danych przed zlokalizowanymi awariami. Ponadto usługa SQL Data Warehouse automatycznie tworzy kopie zapasowe aktywnych (niewstrzymanych) baz danych w regularnych odstępach czasu przy użyciu programu Azure Storage Snapshots. Aby dowiedzieć się więcej na temat zasad tworzenia kopii zapasowych i przywracania, zobacz artykuł [Backup and restore overview][Backup and restore overview] (Omówienie tworzenia kopii zapasowych i przywracania).

## <a name="integrated-with-microsoft-tools"></a>Integracja z narzędziami firmy Microsoft
Usługa SQL Data Warehouse jest zintegrowana z wieloma narzędziami, które mogą już być znane użytkownikom programu SQL Server. Do tych narzędzi należą:

**Tradycyjne narzędzia programu SQL Server:** Usługa SQL Data Warehouse jest w pełni zintegrowana z usługami SQL Server Analysis Services, Integration Services i Reporting Services.

**Narzędzia oparte na chmurze:** Usługę SQL Data Warehouse można zintegrować z różnymi usługami na platformie Azure, takimi jak Data Factory, Stream Analytics, Machine Learning oraz z usługą Power BI. Bardziej kompletny wykaz znajdziesz w artykule [Integrated tools overview][Integrated tools overview] (Przegląd zintegrowanych narzędzi).

**Narzędzia innych firm:** Wiele innych firm dostarczających narzędzia ma certyfikat integracji swoich narzędzi z usługą SQL Data Warehouse. Pełną listę znajdziesz w artykule [SQL Data Warehouse solution partners][SQL Data Warehouse solution partners] (Partnerzy rozwiązania SQL Data Warehouse).

## <a name="hybrid-data-sources-scenarios"></a>Hybrydowe scenariusze źródeł danych
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

