<properties
   pageTitle="Co to jest Azure SQL Data Warehouse | Microsoft Azure"
   description="Rozproszona baza danych klasy korporacyjnej, która może przetwarzać woluminy zawierające petabajty danych relacyjnych i nierelacyjnych. Jest to pierwszy magazyn danych w chmurze w branży, którym umożliwia powiększanie, zmniejszanie i wstrzymanie w ciągu kilku sekund."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/01/2016"
   ms.author="lodipalm;barbkess;mausher;jrj;sonyama;kevin"/>


# Co to jest Azure SQL Data Warehouse?

Usługa Azure SQL Data Warehouse jest skalowaną w poziomie bazą danych opartą na chmurze, która pozwala na przetwarzanie bardzo dużych woluminów danych — zarówno relacyjnych jak i nierelacyjnych. Jest tworzona w oparciu o architekturę MPP, a więc może obsługiwać obciążenie przedsiębiorstwa. 

SQL Data Warehouse:

- Stanowi połączenie funkcji sprawdzonej relacyjnej bazy danych programu SQL Server i skalowalnych w poziomie funkcji chmury Azure. W ciągu kilku sekund można zwiększyć, zmniejszyć, wstrzymać lub wznowić operacje obliczeniowe.  Pozwala to zmniejszyć koszty przez skalowanie procesora CPU w poziomie, gdy wystąpi taka potrzeba, oraz ograniczenie jego użycia poza godzinami szczytu.
- Korzysta z platformy Azure. Jest łatwy do wdrożenia, utrzymywany w sposób ciągły i w pełni odporny na awarie dzięki automatycznemu tworzeniu kopii zapasowych. 
- Uzupełnia ekosystem programu SQL Server.  Można programować za pomocą znanego rozszerzenia SQL Server T-SQL i narzędzi.

W dalszej części artykułu znajduje się więcej informacji na temat kluczowych funkcji usługi SQL Data Warehouse.

## Optymalizacja

### Architektura MPP (Massively Parallel Processing)

Usługa SQL Data Warehouse korzysta z architektury MPP firmy Microsoft, stosowanej do uruchamiania jednych największych na świecie lokalnych magazynów danych.

Obecnie nasza architektura MPP rozprzestrzenia dane w 60 nieudostępnianych jednostkach magazynowych i jednostkach przetwarzania. Dane są przechowywane w obiektach blob magazynu Azure w ramach usługi Premium Storage i połączone z węzłami obliczeniowymi w celu wykonywania zapytań. Taka architektura umożliwia rozwiązanie polegające na rozdzielaniu i zajmowaniu w przypadku uruchamiania złożonych zapytań T-SQL. Podczas przetwarzania węzeł kontroli analizuje zapytania, a następnie każdy węzeł obliczeniowy równolegle „zajmuje” część danych. 

Dzięki połączeniu architektury MPP i możliwości magazynu Azure magazyn danych SQL może:

- Zwiększać i zmniejszać magazyn niezależne od zasobów obliczeniowych.
- Zwiększać i zmniejszać zasoby obliczeniowe bez przenoszenia danych. 
- Wstrzymywać wydajność obliczeniową przy jednoczesnym zachowaniu niezmienionych danych.
- Szybko wznawiać moc obliczeniową.

Architektura została szczegółowo opisana poniżej. 

![Architektura usługi SQL Data Warehouse][1]


- **Węzeł kontrolny:** węzeł kontrolny „steruje” systemem. Jest to fronton współdziałający ze wszystkimi aplikacjami i połączeniami. W usłudze SQL Data Warehouse węzeł kontrolny jest obsługiwany przez usługę SQL Database i łączenie z nim wygląda tak samo. Pod powierzchnią węzeł kontrolny koordynuje w całości przenoszenie i obliczanie danych wymagane do uruchamiania zapytań równoległych względem danych rozproszonych. Po przesłaniu zapytania TSQL do usługi SQL Data Warehouse węzeł kontrolny przekształca je w oddzielne zapytania, które będą uruchamiane równolegle na każdym węźle obliczeniowym.

- **Węzły obliczeniowe:** węzły obliczeniowe zasilają usługę SQL Data Warehouse. Są to bazy danych SQL, które przetwarzają kroki zapytań i zarządzają danymi. Po dodaniu danych usługa SQL Data Warehouse dystrybuuje wiersze, używając węzłów obliczeniowych. Węzłami obliczeniowymi są również procesy robocze, które uruchamiają zapytania równoległe względem danych. Po zakończeniu przetwarzania przekazują wyniki z powrotem do węzła kontrolnego. Aby zakończyć wykonywanie zapytania, węzeł kontrolny agreguje wyniki i zwraca wynik końcowy.


- **Magazyn:** dane są przechowywane w obiektach blob magazynu Azure. Gdy węzły obliczeniowe współdziałają z danymi, zapisują i odczytują bezpośrednio do i z magazynu obiektów blob. Ponieważ magazyn Azure rozszerza się w sposób przezroczysty i nieograniczony, w usłudze SQL Data Warehouse dzieje się tak samo. Ponieważ zasoby obliczeniowe i magazyn są niezależne od siebie, usługa SQL Data Warehouse może automatycznie skalować magazyn niezależnie od skalowania zasobów obliczeniowych i na odwrót.  Usługa Azure Storage jest również całkowicie odporna na uszkodzenia oraz usprawnia proces tworzenia kopii zapasowych i przywracania danych.
   

- **Usługa przenoszenia danych:** Usługa przenoszenia danych (Data Movement Service, DMS) to technologia firmy Microsoft do przenoszenia danych między węzłami. Usługa DMS umożliwia węzłom obliczeniowym dostęp do danych, które są im niezbędne do sprzęgania i agregacji. Usługa DMS nie jest usługą platformy Azure. Jest to usługa systemu Windows uruchamiana razem z usługą SQL Database na wszystkich węzłach. Ponieważ usługa DMS uruchamia się w tle, użytkownik nie wchodzi z nią w bezpośrednią interakcję. Jednak jeśli przyjrzymy się planom zapytań, zauważymy, że zawierają one operacje DMS, gdyż przenoszenie danych jest w jakiejś formie niezbędne do uruchamiania każdego zapytania równolegle.


### Zoptymalizowana wydajność zapytań

Oprócz strategii dzielenia i zajmowania rozwiązanie MPP jest wspomagane pewną liczbą optymalizacji wydajności zależnych od magazynowania danych, m.in.:

- Optymalizator zapytań rozproszonych i zestaw złożonych statystyk obejmujących wszystkie dane. Przy użyciu informacji dotyczących rozmiaru i dystrybucji danych usługa może zoptymalizować zapytania przez ocenę kosztów określonych operacji zapytań rozproszonych.

- Zaawansowane algorytmy i techniki zintegrowane z procesem przenoszenia danych, aby wydajnie przenosić dane między zasobami obliczeniowymi, co jest niezbędne do wykonania zapytania. Te operacje przenoszenia danych są wbudowane, a wszystkie optymalizacje usługi przenoszenia danych następują automatycznie.

- Domyślnie klastrowane indeksy magazynu kolumn. Za pomocą magazynu kolumn w usłudze SQL Data Warehouse można uzyskać nawet pięciokrotnie większe wzmocnienie kompresji w porównaniu z tradycyjnym zorientowanym na wiersze magazynem i nawet dziesięciokrotnie większe wzmocnienie wydajności zapytań. Zapytania analityczne, które muszą przeskanować dużą liczbę wierszy, działają doskonale na indeksach magazynu kolumn. 

## Skalowalność

Architektura usługi SQL Data Warehouse wprowadza oddzielny magazyn i oddzielne zasoby obliczeniowe, umożliwiając niezależne skalowanie obu elementów. Szybka i łatwa do wdrożenia struktura usługi SQL Database pozwala na to, by dodatkowe zasoby obliczeniowe były dostępne niemal natychmiast. W ramach uzupełnienia tej usługi używa się obiektów blob magazynu Azure. Korzystanie z obiektów blob nie tylko zapewnia stabilny, replikowany magazyn, ale również infrastrukturę umożliwiającą proste i niedrogie rozszerzanie.  Przy użyciu tej kombinacji magazynu w skali chmury oraz zasobów obliczeniowych Azure usługa SQL Data Warehouse umożliwia naliczanie opłat za wykonywanie i przechowywanie zapytań tylko w razie potrzeby. Aby zmienić ilość zasobów obliczeniowych, wystarczy przesunąć suwak w portalu Azure w lewo lub w prawo, ale można tę operację również zaplanować za pomocą usługi T-SQL i programu PowerShell.

Wraz z możliwością pełnej kontroli ilości zasobów obliczeniowych niezależnie od magazynu usługa SQL Data Warehouse pozwala na całkowite wstrzymanie magazynu danych. Przy zachowaniu magazynu w niezmienionej formie wszystkie zasoby obliczeniowe zostają uwolnione do głównej puli platformy Azure, co powoduje natychmiastowe oszczędności. W razie potrzeby po prostu wznów przetwarzanie i korzystaj z danych oraz zasobów obliczeniowych dla obciążenia.

Użycie zasobów obliczeniowych w magazynie danych SQL jest mierzone przy użyciu jednostek magazynu danych SQL (jednostek DWU). Jednostki DWU określają podstawową moc magazynu danych i mają na celu zagwarantowanie, że użytkownik będzie zawsze dysponować standardową wydajnością powiązaną z magazynem.  W szczególności jednostki DWU pozwalają zagwarantować, że:

- Użytkownik może efektywnie skalować dane, nie martwiąc się o podstawowy sprzęt ani oprogramowanie.

- Przed wprowadzeniem zmiany rozmiaru magazynu danych można zapoznać się z wydajnością mierzoną w jednostkach DWU.

- Podstawowy sprzęt lub oprogramowanie wystąpienia mogą ulec zmianie lub zostać przeniesione bez wpływu na wydajność obciążenia.

- Firma Microsoft może wprowadzać zmiany w podstawowej architekturze usługi bez wpływu na wydajność obciążenia.

- Ponieważ firma Microsoft szybko poprawia wydajność magazynu danych SQL, możemy zagwarantować, że odbywa się to w sposób skalowalny i ma równomierny wpływ na system.

### Jednostki magazynu danych

Jednostki magazynu danych traktujemy jako środek do określania trzech dokładnych metryk, które naszym zdaniem są ściśle powiązane z wydajnością obciążenia magazynowania danych. Chcemy, by dla ogólnej dostępności te kluczowe metryki obciążenia skalowały liniowo z jednostkami DWU wybranymi do magazynu danych.

**Skanowanie/agregacja:** ta metryka obciążenia używa standardowego zapytania magazynowania danych, które skanuje dużą liczbę wierszy, a następnie wykonuje złożoną agregację. Jest to intensywna operacja we/wy i procesora CPU.

**Obciążenie** ta metryka mierzy możliwość pozyskiwania danych w usłudze. Obciążenia są wykonywane przy użyciu programu PolyBase, który ładuje reprezentatywny zestaw danych z obiektu blob magazynu Azure. Ta metryka obciąża aspekt sieciowy i procesora CPU usługi.

**CREATE TABLE AS SELECT (CTAS):** mierzy możliwość tworzenia kopii tabeli. Obejmuje to odczytywanie danych z magazynu, ich dystrybucję w węzłach urządzenia i ponowne zapisywanie w magazynie. Jest to intensywna operacja procesora CPU i sieci.

### Kiedy skalować

Chcemy, aby użycie jednostek DWU było proste. Jeśli potrzebujesz szybszych efektów, zwiększ liczbę jednostek DWU i opłać większą wydajność.  Jeśli potrzebujesz mniejszej mocy obliczeniowej, zmniejsz liczbę jednostek DWU i zapłać tylko za to, co jest potrzebne. Sytuacje, w których możesz rozważać zmianę liczby jednostek DWU:

- Jeśli nie musisz uruchamiać zapytań, np. wieczorami lub w weekendy, wstrzymaj zasoby obliczeniowe, aby anulować wszystkie uruchomione zapytania i usunąć wszystkie jednostki DWU przydzielone do magazynu danych.

- Podczas wykonywania rozległych operacji ładowania i przekształcania danych można skalować w górę, aby zapewnić szybszy dostęp do danych.

- Aby zrozumieć, jaka jest idealna wartość DWU, skaluj w górę i w dół oraz uruchom kilka zapytań po załadowaniu danych. Ponieważ skalowanie odbywa się szybko, możesz wypróbować różne poziomy wydajności, nie poświęcając na to więcej niż godzinę.

> [AZURE.NOTE] Pamiętaj, że z uwagi na architekturę usługi SQL Data Warehouse w przypadku mniejszych woluminów danych wydajność może odbiegać od oczekiwań.  Zalecamy rozpoczęcie od woluminów danych o rozmiarze co najmniej 1 TB, aby uzyskać rzeczywiste wskazanie korzyści związanych z wydajnością.

## Integracja

Usługa SQL Data Warehouse jest oparta na sprawdzonym aparacie relacyjnej bazy danych programu SQL Server i zawiera wiele funkcji, których można się spodziewać po magazynie danych przedsiębiorstwa. Jeśli znasz już język Transact-SQL, łatwo jest wykorzystać tę wiedzę w usłudze SQL Data Warehouse. Niezależnie od tego, czy jesteś użytkownikiem zaawansowanym, czy dopiero zaczynasz, przykłady przedstawione w dokumentacji ułatwią rozpoczęcie pracy. Ogólnie należy zwrócić uwagę na sposób, w jaki tworzymy elementy językowe w usłudze SQL Data Warehouse:

- Usługa SQL Data Warehouse używa składni języka Transact-SQL (TSQL) programu SQL Server w wielu operacjach i obsługuje szeroką gamę tradycyjnych struktur SQL, takich jak przechowywane procedury, funkcje zdefiniowane przez użytkownika, partycjonowanie tabel, indeksy i sortowanie.

- Usługa SQL Data Warehouse zawiera także pewną liczbę najnowocześniejszych funkcji programu SQL Server, takich jak klastrowane indeksy magazynu kolumn, integracja programu PolyBase oraz inspekcja danych (wraz z oceną zagrożenia).

- Usługa SQL Data Warehouse jest nadal w fazie opracowywania, więc niektóre elementy języka TSQL, które są mniej typowe dla obciążeń magazynowania danych lub nowsze dla programu SQL Server, mogą być obecnie niedostępne. Aby uzyskać więcej informacji związanych z tym tematem, zobacz dokumentację dotyczącą migracji.

Przy użyciu języka Transact-SQL i korzystając z tych samych funkcji występujących w usługach SQL Server, SQL Data Warehouse, SQL Database i systemie Analytics Platform System, można opracować rozwiązanie, które odpowiada potrzebom związanym z danymi. Można określić miejsce przechowywania danych na podstawie wydajności, zabezpieczeń i wymagań skali, a następnie w zależności od potrzeb przekazać dane między różnymi systemami.

Oprócz przyjmowania obszaru powierzchni TSQL programu SQL Server usługa SQL Data Warehouse integruje się również z wieloma narzędziami znanymi użytkownikom programu SQL Server. Firma Microsoft skupia się w szczególności na kilku, integrując z usługą SQL Data Warehouse kilka kategorii narzędzi, m.in.:

**Tradycyjne narzędzia programu SQL Server:** pełna integracja z usługami SQL Server Analysis Services oraz usługami integracji i raportowania jest dostępna w usłudze SQL Data Warehouse.

**Narzędzia oparte na chmurze:** usługi SQL Data Warehouse można używać razem z szeregiem nowych narzędzi w systemie Azure. Jest ona silnie zintegrowana z takimi usługami Azure jak Data Factory, Stream Analytics, Machine Learning oraz z programem Power BI.

**Narzędzia innych firm:** duża liczba dostawców narzędzi innych firm zapewnia certyfikowaną integrację swoich narzędzi z usługą SQL Data Warehouse. Zobacz pełną listę.

## Połączenie hybrydowe

Korzystanie z usługi SQL Data Warehouse w programie PolyBase zapewnia użytkownikom niezrównaną możliwość przenoszenia danych w ekosystemie, powodując odblokowanie możliwości konfigurowania zaawansowanych scenariuszy hybrydowych obejmujących źródła danych nierelacyjnych i lokalnych.

Program PolyBase jest łatwy w użyciu i umożliwia wykorzystanie danych z innych źródeł przy użyciu znanych poleceń języka T-SQL. Program PolyBase pozwala przesyłać zapytania dotyczące danych nierelacyjnych przechowywanych w magazynie obiektów blob Azure, traktując go jak zwykłą tabelę. Użyj programu PolyBase do przesyłania zapytań dotyczących danych nierelacyjnych lub importowania danych nierelacyjnych do usługi SQL Data Warehouse.

- Program PolyBase używa tabel zewnętrznych w celu uzyskania dostępu do danych nierelacyjnych. Definicje tabel są przechowywane w usłudze SQL Data Warehouse i można do ich uzyskać dostęp za pomocą języka SQL i narzędzi w taki sposób, w jaki uzyskuje się dostęp do zwykłych danych relacyjnych.

- Program PolyBase jest niewymagający pod względem integracji. Ma takie same funkcje i funkcjonalność niezależnie od obsługiwanego źródła. Dane odczytywane w programie PolyBase mogą być w różnych formatach, np. w formie plików rozdzielanych lub plików ORC.

- Program PolyBase może służyć do uzyskania dostępu do magazynu obiektów blob, który jest również używany jako magazyn klastra wglądu HD, umożliwiając najnowocześniejszy dostęp do tych samych danych za pomocą narzędzi relacyjnych i nierelacyjnych.

## Następne kroki

Po zdobyciu podstawowej wiedzy o usłudze SQL Data Warehouse możesz zapoznać się z informacjami o [obciążeniu magazynu danych], [instrukcjami aprowizacji] usługi SQL Data Warehouse i [instrukcjami ładowania przykładowych danych].  Możesz też zwrócić uwagę na inne zasoby dotyczące usługi SQL Data Warehouse.  

- [Blogi] 
- [Żądania funkcji]
- [Filmy wideo]
- [Blogi zespołu CAT]
- [Tworzenie biletu pomocy technicznej]
- [Forum MSDN]
- [Forum Stack Overflow]
- [Twitter]


<!--Image references-->
[1]: ./media/sql-data-warehouse-overview-what-is/dwarchitecture.png

<!--Article references-->
[Tworzenie biletu pomocy technicznej]: ./sql-data-warehouse-get-started-create-support-ticket.md
[obciążeniu magazynu danych]: ./sql-data-warehouse-overview-workload.md
[instrukcjami ładowania przykładowych danych]: ./sql-data-warehouse-get-started-manually-load-samples.md
[instrukcjami aprowizacji]: ./sql-data-warehouse-get-started-provision.md

<!--MSDN references-->

<!--Other Web references-->
[Blogi]: https://azure.microsoft.com/blog/tag/azure-sql-data-warehouse/
[Blogi zespołu CAT]: https://blogs.msdn.microsoft.com/sqlcat/tag/sql-dw/
[Żądania funkcji]: https://feedback.azure.com/forums/307516-sql-data-warehouse
[Forum MSDN]: https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureSQLDataWarehouse
[Forum Stack Overflow]: http://stackoverflow.com/questions/tagged/azure-sqldw
[Twitter]: https://twitter.com/hashtag/SQLDW
[Filmy wideo]: https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse



<!----HONumber=Jun16_HO2-->


