---
title: "Azure DB rozwiązania Cosmos — często zadawane pytania | Dokumentacja firmy Microsoft"
description: "Odpowiedzi na często zadawane pytania dotyczące bazy danych z rozwiązania Cosmos platformy Azure, usługa globalnie rozproszone i wiele modeli bazy danych. Więcej informacji na temat pojemności, poziomów wydajności i skalowania."
keywords: "Pytania dotyczące bazy danych, często zadawane pytania, usługi documentdb, azure, platformy Microsoft azure"
services: cosmos-db
author: mimig1
manager: jhubbard
editor: monicar
documentationcenter: 
ms.assetid: b68d1831-35f9-443d-a0ac-dad0c89f245b
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/02/2017
ms.author: mimig
ms.openlocfilehash: 091446fd45b09913dee70dbb4c7e5ebbca02819b
ms.sourcegitcommit: 295ec94e3332d3e0a8704c1b848913672f7467c8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/06/2017
---
# <a name="azure-cosmos-db-faq"></a>Często zadawane pytania dotyczące usługi Azure rozwiązania Cosmos bazy danych
## <a name="azure-cosmos-db-fundamentals"></a>Podstawowe informacje na temat usługi Azure DB rozwiązania Cosmos
### <a name="what-is-azure-cosmos-db"></a>Co to jest Azure DB rozwiązania Cosmos?
Azure DB rozwiązania Cosmos jest usługą bazy danych replikowany globalnie, wiele modeli, ułatwiająca oferująca zaawansowaną obsługę zapytań dane bez schematu oraz konfigurowalną i niezawodną wydajności i umożliwia szybkie opracowywanie. Jego wszystkich odbywa się za pośrednictwem zarządzana platforma, która nie jest obsługiwana przez możliwości i zasięg Microsoft Azure. 

Azure DB rozwiązania Cosmos jest odpowiednie rozwiązanie dla aplikacji sieci web, mobilnych, gier, i aplikacji IoT przewidywalnej przepływności, wysoką dostępność, małych opóźnień i model danych bez schematu są wymagania dotyczące klucza. Zapewnia elastyczność schematu i rozbudowane indeksowanie i zawiera obsługę transakcyjną wielu dokumentów dzięki zintegrowanemu środowisku JavaScript. 

Aby uzyskać więcej pytania bazy danych, odpowiedzi i instrukcje dotyczące instalowania i korzystania z tej usługi, zobacz [stronę dokumentacji bazy danych Azure rozwiązania Cosmos](https://azure.microsoft.com/documentation/services/cosmos-db/).

### <a name="what-happened-to-documentdb"></a>Co się stało z usługi DocumentDB?
Interfejs API usługi DocumentDB jest jednym z obsługiwanych interfejsów API i modele danych dla bazy danych Azure rozwiązania Cosmos. Ponadto bazy danych rozwiązania Cosmos Azure obsługuje możesz przy użyciu interfejsu API programu Graph (wersja zapoznawcza), tabela interfejsu API (wersja zapoznawcza) i interfejsu API bazy danych MongoDB. Aby uzyskać więcej informacji, zobacz [pytania klientów usługi DocumentDB](#moving-to-cosmos-db).

### <a name="how-do-i-get-to-my-documentdb-account-in-the-azure-portal"></a>Jak uzyskać mojego konta usługi DocumentDB w portalu Azure
W portalu Azure kliknij ikonę bazy danych Azure rozwiązania Cosmos w okienku po lewej stronie. Jeśli masz konto usługi DocumentDB przed masz teraz konto bazy danych Azure rozwiązania Cosmos, bez żadnych zmian rozliczeniowego.

### <a name="what-are-the-typical-use-cases-for-azure-cosmos-db"></a>Jakie są typowe zastosowania dla bazy danych rozwiązania Cosmos Azure?
Azure DB rozwiązania Cosmos jest dobrym rozwiązaniem w przypadku nowych sieci web, mobilnych, gier, i ważne jest, gdzie skalowania automatycznego, przewidywalną wydajność, szybkie kolejność milisekund czasy odpowiedzi oraz możliwość wykonywania kwerend danych bez schematu aplikacji IoT. Azure DB rozwiązania Cosmos pozwala na szybkie opracowywanie i obsługę ciągłej iteracji modeli danych aplikacji. Zawartość wygenerowaną przez użytkowników i dane aplikacji są [typowe przypadki użycia bazy danych Azure rozwiązania Cosmos](use-cases.md). 

### <a name="how-does-azure-cosmos-db-offer-predictable-performance"></a>Jak bazy danych rozwiązania Cosmos Azure oferuje przewidywalną wydajność?
A [jednostki żądania](request-units.md) (RU) to miara przepływności w usłudze Azure DB rozwiązania Cosmos. Przepływność 1 RU odpowiada przepływności pobierania dokumentu 1 KB. Każdej operacji w usłudze Azure DB rozwiązania Cosmos, w tym odczytów, zapisy zapytania SQL i wykonywaniem procedur składowanych, ma wartość RU deterministyczna, która jest oparta na przepływności wymaganej do ukończenia tej operacji. Zamiast planowania procesora CPU, we/wy i pamięci oraz ich wpływ na przepływność aplikacji, można traktować pod względem jednej miary RU.

Istnieje możliwość rezerwowania każdego kontenera Azure DB rozwiązania Cosmos z aprowizowaną przepływnością wyrażoną jako RUs przepływności na sekundę. W przypadku aplikacji o dowolnej skali testu wydajności poszczególnych żądań w celu pomiaru ich wartości RU i udostępnić kontenera do obsługi całkowitej liczby jednostek żądania dla wszystkich żądań. Można także skalowanie w górę lub skali kontenera na przepływność rozwijających się potrzeb aplikacji. Aby uzyskać więcej informacji na temat jednostek żądania i określania z kontenera musi zobacz [szacowanie potrzeb w zakresie przepustowości](request-units.md#estimating-throughput-needs) , a następnie spróbuj [Kalkulator przepływności](https://www.documentdb.com/capacityplanner). Termin *kontenera* odnosi się tutaj do odwołuje się do kolekcji interfejsu API usługi DocumentDB, wykres interfejsu API programu Graph, kolekcji bazy danych MongoDB interfejsu API i interfejsu API tabeli tabeli. 

### <a name="how-does-azure-cosmos-db-support-various-data-models-such-as-keyvalue-columnar-document-and-graph"></a>Jak usługa Azure DB rozwiązania Cosmos obsługuje różne modele danych, takie jak klucz/wartość, kolumnowy, dokumentów i wykres

Klucz/wartość, tabela kolumnowy, dokumentów i danych wykresu modele są wszystkie obsługiwane z powodu ARS (atomami, rekordów i sekwencji) projektowania tej bazy danych rozwiązania Cosmos Azure jest wbudowany w program. Atomami, rekordów i sekwencji można łatwo zamapowane i zaprojektowana różne modele danych. Interfejsy API do podzbioru modeli są dostępne prawa teraz (usługi DocumentDB, bazy danych MongoDB, tabeli i interfejsów API Graph) i inni użytkownicy specyficzne dla modeli dodatkowe dane będą dostępne w przyszłości.

Azure DB rozwiązania Cosmos ma schematu o niesprecyzowanym aparatu indeksowania może automatycznie indeksowania wszystkie dane, które go wysyła strumień bez żadnego schematu lub indeksów pomocniczych od dewelopera. Aparat zależy od zestawu układów logicznych indeksu (odwrócony, kolumnowy, drzewo), które oddziel układu magazynu z indeksu i podsystemy przetwarzania zapytań. Rozwiązania cosmos bazy danych ma również możliwość obsługi zestawu danych przesyłanych w sieci protokołów i interfejsów API w sposób extensible i efektywnie tłumaczyć je do modelu danych core (1) i układów logicznych indeksu (2) co jednoznacznie może obsługiwać wiele modeli danych natywnie.

### <a name="is-azure-cosmos-db-hipaa-compliant"></a>Azure rozwiązania Cosmos DB HIPAA jest zgodne?
Tak, bazy danych Azure rozwiązania Cosmos jest zgodny z ustawą HIPAA. Ustawa HIPAA określa wymogi dotyczące wykorzystywania, ujawniania i ochrony informacji o zdrowiu umożliwiających indywidualną identyfikację osoby. Aby uzyskać więcej informacji, zobacz [Centrum zaufania Microsoft](https://www.microsoft.com/en-us/TrustCenter/Compliance/HIPAA).

### <a name="what-are-the-storage-limits-of-azure-cosmos-db"></a>Jakie są limity magazynu bazy danych rozwiązania Cosmos Azure?
Nie ma żadnego limitu całkowitej ilości danych, które kontener może przechowywać w usłudze Azure DB rozwiązania Cosmos.

### <a name="what-are-the-throughput-limits-of-azure-cosmos-db"></a>Jakie są ograniczenia przepływności bazy danych rozwiązania Cosmos Azure?
Nie ma żadnego limitu całkowitej przepływności obsługujące przez kontener w usłudze Azure DB rozwiązania Cosmos. Klucza będzie rozłożyć obciążenie przybliżeniu równomiernie między wystarczająco dużą liczbę kluczy partycji.

### <a name="how-much-does-azure-cosmos-db-cost"></a>Ile kosztuje bazy danych Azure rozwiązania Cosmos
Aby uzyskać więcej informacji, zapoznaj się [Azure DB rozwiązania Cosmos szczegóły cennika](https://azure.microsoft.com/pricing/details/cosmos-db/) strony. Azure opłaty za użycie rozwiązania Cosmos bazy danych są określane przez liczbę kontenerów elastycznie, liczbę godzin kontenery były w trybie online, i udostępnionej przepływności dla każdego kontenera. Termin *kontenery* odnosi się tutaj do kolekcji interfejsu API usługi DocumentDB, wykres interfejsu API programu Graph, kolekcji bazy danych MongoDB interfejsu API i tabele tabeli interfejsu API. 

### <a name="is-a-free-account-available"></a>Jest dostępne bezpłatne konto?
Tak, można założyć konto ograniczone czasowo za darmo, bez zobowiązań. Aby utworzyć konto, odwiedź stronę [bezpłatnie spróbuj Azure DB rozwiązania Cosmos](https://azure.microsoft.com/try/cosmosdb/) lub Dowiedz się więcej w [spróbuj Azure rozwiązania Cosmos DB — często zadawane pytania](#try-cosmos-db).

Jeśli jesteś nowym użytkownikiem usługi Azure, możesz zarejestrować się w celu [bezpłatne konto platformy Azure](https://azure.microsoft.com/free/), które zapewnia 30 dni i i środki na wypróbowanie usług platformy Azure. Jeśli masz subskrypcję programu Visual Studio, przysługuje Ci również [kredytów systemu Azure w warstwie bezpłatna](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) na korzystanie z dowolnej usługi Azure. 

Można również użyć [Azure rozwiązania Cosmos DB emulatora](local-emulator.md) umożliwiające opracowanie i przetestowanie aplikacji lokalnie przez bezpłatne, bez tworzenia subskrypcji platformy Azure. Po zakończeniu jak aplikacja działa w emulatorze DB rozwiązania Cosmos Azure, możesz przełączyć się do korzystania z konta bazy danych Azure rozwiązania Cosmos w chmurze.

### <a name="how-can-i-get-additional-help-with-azure-cosmos-db"></a>Jak można uzyskać dodatkową pomoc dotyczącą bazy danych rozwiązania Cosmos Azure?
Jeśli potrzebujesz pomocy dotrzeć do nas na [przepełnienie stosu](http://stackoverflow.com/questions/tagged/azure-cosmosdb) lub [MSDN forum](https://social.msdn.microsoft.com/forums/azure/en-US/home?forum=AzureDocumentDB), lub zgodnie z harmonogramem Wykorzystaj rozmów z bazy danych Azure rozwiązania Cosmos zespołu inżynieryjnego przez wysyłanie poczty do [ askcosmosdb@microsoft.com ](mailto:askcosmosdb@microsoft.com). 

<a id="try-cosmos-db"></a>
## <a name="try-azure-cosmos-db-subscriptions"></a>Spróbuj subskrypcji bazy danych Azure rozwiązania Cosmos

Można teraz korzystać z obsługi bazy danych Azure rozwiązania Cosmos ograniczone czasowo, bez subskrypcji, bezpłatnie i zobowiązań. Aby zarejestrować subskrypcję spróbuj rozwiązania Cosmos bazy danych Azure, przejdź do [bezpłatnie spróbuj Azure DB rozwiązania Cosmos](https://azure.microsoft.com/try/cosmosdb/). Ta subskrypcja jest oddzielony od [bezpłatnej wersji próbnej Azure](https://azure.microsoft.com/free/)i mogą być używane razem z bezpłatnej wersji próbnej platformy Azure lub Azure płatnej subskrypcji. 

Spróbuj bazy danych Azure rozwiązania Cosmos subskrypcje są wyświetlane w portalu Azure obok innych subskrypcji skojarzonych z identyfikatorem użytkownika 

Do subskrypcji spróbuj rozwiązania Cosmos bazy danych Azure mają zastosowanie następujące warunki:

* Jeden kontener na subskrypcji dla interfejsu API tabeli kont, Gremlin (interfejs API programu Graph) i SQL (interfejsu API usługi DocumentDB).
* Maksymalnie 3 kolekcje na subskrypcję dla konta bazy danych MongoDB.
* 10 GB pojemności.
* Globalne replikacja jest dostępna w następujących [regiony platformy Azure](https://azure.microsoft.com/regions/): środkowe stany USA, Europa Północna, Europa i Azja południowo-wschodnia
* Maksymalna przepustowość 5 K RU/s.
* Subskrypcji wygaśnie po 24 godzinach i może zostać rozszerzony do maksymalnie 48 godzin.
* Nie można utworzyć biletów pomocy technicznej platformy Azure dla kont spróbuj rozwiązania Cosmos bazy danych platformy Azure; jednak obsługiwane dla subskrybentów z istniejących plany pomocy technicznej. 

## <a name="set-up-azure-cosmos-db"></a>Konfigurowanie bazy danych Azure rozwiązania Cosmos
### <a name="how-do-i-sign-up-for-azure-cosmos-db"></a>Jak tworzyć konta dla bazy danych rozwiązania Cosmos Azure?
Azure DB rozwiązania Cosmos jest dostępne w portalu Azure. Najpierw Utwórz subskrypcję platformy Azure. Po zarejestrowaniu się interfejsu API usługi DocumentDB, interfejsu API programu Graph (wersja zapoznawcza), tabela interfejsu API (wersja zapoznawcza) lub konto bazy danych MongoDB interfejsu API można dodać do subskrypcji platformy Azure.

### <a name="what-is-a-master-key"></a>Co to jest klucz główny?
Klucz główny jest tokenem zabezpieczającym dostęp do wszystkich zasobów dla konta. Osoby, które mają klucz odczytu i zapisu dostęp do wszystkich zasobów w ramach konta bazy danych. Podczas dystrybucji kluczy głównych należy zachować ostrożność. Podstawowy klucz główny i pomocniczy klucz główny są dostępne na **klucze** bloku [portalu Azure][azure-portal]. Aby uzyskać więcej informacji o kluczach, zobacz [wyświetlanie, kopiowanie i ponowne generowanie kluczy dostępu](manage-account.md#keys).

### <a name="what-are-the-regions-that-preferredlocations-can-be-set-to"></a>Co to są regionów, które można ustawić PreferredLocations? 
Wartość PreferredLocations można ustawić dla każdego regiony platformy Azure, w których są dostępne rozwiązania Cosmos bazy danych. Aby uzyskać listę dostępnych regionów, zobacz [regiony platformy Azure](https://azure.microsoft.com/regions/).

### <a name="is-there-anything-i-should-be-aware-of-when-distributing-data-across-the-world-via-the-azure-datacenters"></a>Czy jest coś I należy zwrócić uwagę podczas dystrybucję danych na świecie za pośrednictwem centrach danych platformy Azure? 
Azure DB rozwiązania Cosmos jest obecny we wszystkich regionach platformy Azure, jak określono w [regiony platformy Azure](https://azure.microsoft.com/regions/) strony. Co nowego centrum danych, ponieważ jest usługi podstawowej, ma obecności bazy danych Azure rozwiązania Cosmos. 

Po ustawieniu regionu, należy pamiętać, że bazy danych Azure rozwiązania Cosmos szanuje chmur suwerenne i dla instytucji rządowych. Oznacza to jeśli tworzysz konto w regionie suwerennych nie może replikować poza tym suwerennych regionie. Podobnie nie można włączyć replikacji w innych lokalizacjach suwerennych poza konta. 

## <a name="develop-against-the-documentdb-api"></a>Tworzenie bazy danych DocumentDB interfejsu API

### <a name="how-do-i-start-developing-against-the-documentdb-api"></a>Jak rozpocząć tworzenie oprogramowania dla interfejsu API usługi DocumentDB
Interfejs API usługi DocumentDB Microsoft jest dostępna w [portalu Azure][azure-portal]. Najpierw musisz zarejestrować subskrypcji platformy Azure. Po utworzeniu konta dla subskrypcji platformy Azure, można dodać kontenera interfejsu API usługi DocumentDB do subskrypcji platformy Azure. Aby uzyskać instrukcje dotyczące dodawania konta bazy danych rozwiązania Cosmos platformy Azure, zobacz [Tworzenie konta bazy danych Azure DB rozwiązania Cosmos](create-documentdb-dotnet.md#create-account). Jeśli masz konto usługi DocumentDB w przeszłości masz teraz konto bazy danych Azure rozwiązania Cosmos. 

[Zestawy SDK](documentdb-sdk-dotnet.md) są dostępne dla języków .NET, Python, Node.js, JavaScript i Java. Deweloperzy mogą również używać [interfejsy API RESTful protokołu HTTP](/rest/api/documentdb/) do interakcji z zasobami Azure DB rozwiązania Cosmos z różnych platform i języków.

### <a name="can-i-access-some-ready-made-samples-to-get-a-head-start"></a>Można uzyskać dostęp do niektórych gotowe próbek, aby utworzyć?
Przykłady dotyczące interfejsu API usługi DocumentDB [.NET](documentdb-dotnet-samples.md), [Java](https://github.com/Azure/azure-documentdb-java), [Node.js](documentdb-nodejs-samples.md), i [Python](documentdb-python-samples.md) zestawów SDK są dostępne w serwisie GitHub.


### <a name="does-the-documentdb-api-database-support-schema-free-data"></a>Baza danych interfejsu API usługi DocumentDB obsługuje dane bez schematu?
Tak, interfejs API usługi DocumentDB umożliwia aplikacjom przechowywanie dowolnych dokumentów JSON bez definicji schematu lub wskazówek. Dane są natychmiast dostępne dla zapytania dzięki interfejsowi zapytań Azure rozwiązania Cosmos bazy danych SQL.  

### <a name="does-the-documentdb-api-support-acid-transactions"></a>Interfejs API usługi DocumentDB obsługuje transakcje ACID?
Tak, interfejs API usługi DocumentDB obsługuje transakcje dla wielu dokumentów wyrażone jako procedury składowane JavaScript i wyzwalaczy. Transakcje są ograniczone do jednej partycji w ramach każdej kolekcji i wykonywane przy użyciu semantyki ACID jako "wszystkie lub żadne," odizolowana od innego współbieżnie wykonywanego kodu i żądań użytkownika. Jeśli istnieją wyjątki zgłaszane przez wykonanie kodu aplikacji JavaScript po stronie serwera, cała transakcja zostanie wycofana. Aby uzyskać więcej informacji na temat transakcji, zobacz [bazy danych programu transakcji](programming.md#database-program-transactions).

### <a name="what-is-a-collection"></a>Co to jest kolekcja?
Kolekcja jest grupą dokumentów i ich skojarzonej logiki aplikacji JavaScript. Kolekcja to płatna jednostka, której [koszt](performance-levels.md) jest określana przez przepływność i użyto magazynu. Kolekcje mogą znajdować się na partycji lub serwerów i mogą być skalowane do obsługi praktycznie nieograniczonej ilości magazynu lub przepływności.

Kolekcje są również jednostkami rozliczeniowymi dla bazy danych Azure rozwiązania Cosmos. Każda kolekcja jest rozliczane godzinowo, oparta na udostępnionej przepływności i używać miejsca do magazynowania. Aby uzyskać więcej informacji, zobacz [cennik usługi Azure rozwiązania Cosmos DB](https://azure.microsoft.com/pricing/details/cosmos-db/). 

### <a name="how-do-i-create-a-database"></a>Jak utworzyć bazę danych?
Bazy danych można tworzyć przy użyciu [portalu Azure](https://portal.azure.com), zgodnie z opisem w [dodania kolekcji](create-documentdb-dotnet.md#create-collection), jednego z [zestawów SDK DB rozwiązania Cosmos Azure](documentdb-sdk-dotnet.md), lub [interfejsów API REST](/rest/api/documentdb/). 

### <a name="how-do-i-set-up-users-and-permissions"></a>Jak skonfigurować użytkowników i uprawnienia?
Można utworzyć użytkowników i uprawnień przy użyciu jednej z [zestawów SDK interfejsu API DB rozwiązania Cosmos](documentdb-sdk-dotnet.md) lub [interfejsów API REST](/rest/api/documentdb/).  

### <a name="does-the-documentdb-api-support-sql"></a>Interfejs API usługi DocumentDB obsługuje SQL?
Język zapytań SQL jest rozszerzonym podzbiorem funkcji zapytań, który jest obsługiwany przez program SQL. Języka zapytań usługi Azure rozwiązania Cosmos bazy danych SQL zawiera sformatowanego operatorów hierarchicznej i rozszerzalność dzięki funkcji oparte na języku JavaScript, zdefiniowane przez użytkownika (UDF). Gramatyka JSON umożliwia modelowanie dokumentów JSON jako drzewa z etykietami węzły, które są używane zarówno przez techniki automatycznego indeksowania bazy danych Azure rozwiązania Cosmos i dialekt zapytań SQL Azure DB rozwiązania Cosmos. Aby uzyskać informacji dotyczących używania gramatyki SQL, zobacz [QueryDocumentDB] [ query] artykułu.

### <a name="does-the-documentdb-api-support-sql-aggregation-functions"></a>Interfejs API usługi DocumentDB obsługuje funkcje agregacji SQL?
Interfejs API usługi DocumentDB obsługuje agregacji o małych opóźnieniach na dowolnym poziomie za pomocą funkcji agregujących `COUNT`, `MIN`, `MAX`, `AVG`, i `SUM` za pośrednictwem gramatyki SQL. Aby uzyskać więcej informacji, zobacz [funkcje agregujące](documentdb-sql-query.md#Aggregates).

### <a name="how-does-the-documentdb-api-provide-concurrency"></a>W jaki sposób interfejsu API usługi DocumentDB zapewnia współbieżność?
Interfejs API usługi DocumentDB obsługuje optymistycznej współbieżności sterowanie Współbieżnością za za pomocą tagów jednostki HTTP lub elementów ETag. Każdy zasób interfejsu API usługi DocumentDB ma element ETag i element ETag jest ustawiony na serwerze każdej aktualizacji dokumentu. Nagłówek ETag i bieżącą wartość są uwzględnione w wszystkie wiadomości odpowiedzi. Elementy etag może służyć nagłówka If-Match, aby serwer mógł zdecydować, czy można zaktualizować zasobu. Wartość If-Match jest wartość ETag do porównania. Jeśli wartość ETag odpowiada serwera wartość ETag, zasób jest aktualizowana. Jeśli element ETag nie jest już aktualny, serwer odrzuca operację, podając "HTTP 412 niepowodzenie warunku wstępnego" Kod odpowiedzi. Następnie klient pobiera ponownie zasobów w celu uzyskania bieżącej wartości ETag dla zasobu. Ponadto elementy etag można nagłówka If-None-Match do określenia, czy ponownie pobrać zasobu jest wymagana.

Aby użyć optymistycznej współbieżności w programie .NET, użyj [AccessCondition](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.accesscondition.aspx) klasy. Przykładowy .NET [Program.cs](https://github.com/Azure/azure-documentdb-dotnet/blob/master/samples/code-samples/DocumentManagement/Program.cs) w przykładowym DocumentManagement w witrynie GitHub.

### <a name="how-do-i-perform-transactions-in-the-documentdb-api"></a>Jak wykonywać transakcje w interfejsie API usługi DocumentDB?
Interfejs API usługi DocumentDB obsługuje transakcje zintegrowane z językiem za pomocą procedur składowanych JavaScript i wyzwalaczy. Wszystkie operacje bazy danych wewnątrz skryptów są wykonywane w ramach izolacji migawki. Jeśli jest to kolekcja jednej partycji, obejmuje wykonanie do kolekcji. Jeśli kolekcja jest podzielona na partycje, wykonanie obejmuje dokumentów z taką samą wartość klucza partycji w kolekcji. Migawka wersji dokumentów (elementy ETag) jest wykonywana na początku transakcji i zatwierdzana tylko wtedy, gdy działanie skryptu zakończy się pomyślnie. Jeśli kod JavaScript zwraca błąd, transakcja zostaje wycofana. Aby uzyskać więcej informacji, zobacz [programowania w języku JavaScript po stronie serwera dla bazy danych Azure rozwiązania Cosmos](programming.md).

### <a name="how-can-i-bulk-insert-documents-into-cosmos-db"></a>Jak można I wstawiania zbiorczego dokumenty w bazie danych rozwiązania Cosmos?
Użytkownik może wstawiania zbiorczego dokumentów do bazy danych Azure rozwiązania Cosmos w jeden z dwóch sposobów:

* Narzędzie migracji danych, zgodnie z opisem w [narzędzie migracji bazy danych dla bazy danych Azure rozwiązania Cosmos](import-data.md).
* Procedury składowane, zgodnie z opisem w [programowania w języku JavaScript po stronie serwera dla bazy danych Azure rozwiązania Cosmos](programming.md).

### <a name="does-the-documentdb-api-support-resource-link-caching"></a>Czy buforowanie linków zasobów pomocy technicznej interfejsu API usługi DocumentDB?
Tak, ponieważ bazy danych rozwiązania Cosmos Azure jest usługą RESTful, linki zasobów są niezmienne i mogą być buforowane. Klienci usługi DocumentDB interfejsu API można określić nagłówek "If-None-Match" dla odczytów względem dowolnego zasobu typu dokumentu lub kolekcji, a następnie zaktualizuj ich lokalne kopie, po zmianie wersji serwera.

### <a name="is-a-local-instance-of-documentdb-api-available"></a>Jest dostępne lokalne wystąpienie interfejsu API usługi DocumentDB?
Tak. [Azure rozwiązania Cosmos DB emulatora](local-emulator.md) zapewnia emulacji o wysokiej wierności usługi DB rozwiązania Cosmos. Obsługuje funkcje, które są takie same jak rozwiązania Cosmos bazy danych Azure, w tym obsługa tworzenia i badania dokumentów JSON, inicjowania obsługi administracyjnej i skalowanie kolekcje i wykonywania procedury składowane i wyzwalaczy. Mogą tworzyć i testować aplikacje przy użyciu emulatora usługi Azure DB rozwiązania Cosmos i wdrożyć je na platformie Azure w skali globalnej, wprowadzając zmiany do punktu końcowego połączenia dla bazy danych Azure rozwiązania Cosmos konfiguracji pojedynczego.

## <a name="develop-against-the-api-for-mongodb"></a>Tworzenie do interfejsu API dla bazy danych MongoDB
### <a name="what-is-the-azure-cosmos-db-api-for-mongodb"></a>Co to jest API Azure rozwiązania Cosmos bazy danych dla bazy danych MongoDB?
API Azure rozwiązania Cosmos bazy danych dla bazy danych MongoDB jest warstwy zgodności, który umożliwia aplikacjom łatwo i w przezroczysty sposób komunikowania się z natywnego aparatu bazy danych DB rozwiązania Cosmos Azure za pomocą istniejących, obsługiwane społeczności Apache bazy danych MongoDB z interfejsów API i sterowników. Deweloperzy mogą teraz używać do tworzenia aplikacji, które korzystają z bazy danych Azure rozwiązania Cosmos istniejących łańcuchów Narzędzia bazy danych MongoDB i umiejętności. Deweloperom korzystać z unikatowych możliwości bazy danych rozwiązania Cosmos platformy Azure, w tym obsługi automatycznego indeksowania, tworzenia kopii zapasowej, umów dotyczących poziomu finansowo kopii zapasowej usług (SLA) i tak dalej.

### <a name="how-do-i-connect-to-my-api-for-mongodb-database"></a>Jak nawiązać Moje interfejsu API dla bazy danych MongoDB
Najszybszy sposób nawiązania połączenia interfejsu API Azure rozwiązania Cosmos bazy danych dla bazy danych MongoDB ma head za pośrednictwem do [portalu Azure](https://portal.azure.com). Przejdź do swojego konta, a następnie w menu nawigacji po lewej stronie kliknij **Szybki Start**. Szybki Start to najlepszy sposób pozyskania wstawki kodu do połączenia z bazą danych. 

Azure DB rozwiązania Cosmos wymusza wymogi ograniczeniami zabezpieczeń. Azure DB rozwiązania Cosmos kont wymagają uwierzytelniania i zapewnienia bezpiecznej komunikacji za pośrednictwem protokołu SSL, dlatego należy użyć TLSv1.2.

Aby uzyskać więcej informacji, zobacz [Connect do interfejsu API dla bazy danych MongoDB](connect-mongodb-account.md).

### <a name="are-there-additional-error-codes-for-an-api-for-mongodb-database"></a>Czy istnieją dodatkowe kody błędów dla interfejsu API dla bazy danych MongoDB?
Oprócz często występujące kody błędów bazy danych MongoDB API bazy danych MongoDB ma własne kody błędu:


| Błąd               | Kod  | Opis  | Rozwiązanie  |
|---------------------|-------|--------------|-----------|
| TooManyRequests     | 16500 | Całkowita liczba zużywane jednostki żądania przekroczył wskaźnik elastycznie jednostka żądania dla kolekcji i został ograniczony. | Należy wziąć pod uwagę skalowanie przepływność kolekcji z portalu Azure lub podjęciem ponownej próby. |
| ExceededMemoryLimit | 16501 | Jako usługę wielodostępną operacja przekroczyła przydział pamięci klienta. | Zmniejsz zakres operacji przy użyciu bardziej restrykcyjnego zapytania kryteria lub skontaktuj się z pomocy technicznej z [portalu Azure](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade). <br><br>Przykład:  *&nbsp; &nbsp; &nbsp; &nbsp;db.getCollection('users').aggregate ([<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{$match: {Nazwa: "Adama"}}, <br> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;{$sort: {wieku: -1}}<br>&nbsp;&nbsp;&nbsp;&nbsp;])*) |

## <a name="develop-with-the-table-api-preview"></a>Tworzenie tabeli interfejsu API (wersja zapoznawcza)

### <a name="terms"></a>Warunki 
Rozwiązania Cosmos DB tabeli interfejsu API Azure (wersja zapoznawcza) odwołuje się do oferty Azure DB rozwiązania Cosmos dla tabeli modelu danych ogłaszane w kompilacji 2017 premium. 

### <a name="how-can-i-use-the-new-table-api-preview-offering"></a>Jak używać nową ofertę tabeli interfejsu API (wersja zapoznawcza)? 
Interfejs API Azure rozwiązania Cosmos DB tabeli są dostępne w [portalu Azure][azure-portal]. Najpierw musisz zarejestrować subskrypcji platformy Azure. Po zarejestrowaniu się możesz dodać konto interfejsu API Azure rozwiązania Cosmos DB tabeli do subskrypcji platformy Azure, a następnie dodaj tabel do Twojego konta. 

Okresie używania wersji zapoznawczej gdy [zestawów SDK](../cosmos-db/table-sdk-dotnet.md) są dostępne dla platformy .NET, możesz rozpocząć pracę, wykonując [API tabeli](../cosmos-db/create-table-dotnet.md) artykułu szybki start.

### <a name="do-i-need-a-new-sdk-to-use-the-table-api-preview"></a>Należy nowy zestaw SDK, aby za pomocą interfejsu API tabeli (wersja zapoznawcza)? 
Tak, [tabeli Premium magazynu systemu Windows Azure (wersja zapoznawcza) zestawu SDK](https://www.nuget.org/packages/WindowsAzure.Storage-PremiumTable) jest dostępny na NuGet i jest wymagany do użycia interfejsu API Azure rozwiązania Cosmos bazy danych tabeli. Dodatkowe informacje są dostępne na [interfejsu API Azure rozwiązania Cosmos DB tabeli .NET: Pobierz i informacje o wersji](https://github.com/Microsoft/azure-docs-pr/cosmos-db/table-sdk-dotnet.md) strony. 

### <a name="how-do-i-provide-feedback-about-the-sdk-or-bugs"></a>Jak wyrazić opinię na temat zestawu SDK lub usterki?
Twoja opinia można udostępniać w jednym z następujących sposobów:

* [Uservoice](https://feedback.azure.com/forums/599062-azure-cosmos-db-table-api)
* [Forum MSDN](https://social.msdn.microsoft.com/forums/azure/en-US/home?forum=AzureDocumentDB)
* [Witryna Stackoverflow](http://stackoverflow.com/questions/tagged/azure-cosmosdb)

### <a name="what-is-the-connection-string-that-i-need-to-use-to-connect-to-the-table-api-preview"></a>Co to jest parametry połączenia, które należy użyć do nawiązania połączenia interfejsu API tabeli (wersja zapoznawcza)?
Parametry połączenia są:
```
DefaultEndpointsProtocol=https;AccountName=<AccountNamefromCosmos DB;AccountKey=<FromKeysPaneofCosmosDB>;TableEndpoint=https://<AccountNameFromDocumentDB>.documents.azure.com
```
Parametry połączenia można uzyskać ze strony klucze w portalu Azure. 

### <a name="how-do-i-override-the-config-settings-for-the-request-options-in-the-new-table-api-preview"></a>Jak zastąpić ustawienia konfiguracji dla opcji żądania w nowym interfejsem API tabeli (wersja zapoznawcza)?
Aby uzyskać informacje o ustawieniach konfiguracji, zobacz [możliwości bazy danych Azure rozwiązania Cosmos](../cosmos-db/tutorial-develop-table-dotnet.md#azure-cosmos-db-capabilities). Można zmienić ustawienia, dodając je do pliku app.config w sekcji appSettings w aplikacji klienta.

    <appSettings>
        <add key="TableConsistencyLevel" value="Eventual|Strong|Session|BoundedStaleness|ConsistentPrefix"/>
        <add key="TableThroughput" value="<PositiveIntegerValue"/>
        <add key="TableIndexingPolicy" value="<jsonindexdefn>"/>
        <add key="TableUseGatewayMode" value="True|False"/>
        <add key="TablePreferredLocations" value="Location1|Location2|Location3|Location4>"/>....
    </appSettings>


### <a name="are-there-any-changes-for-customers-who-are-using-the-existing-azure-table-storage-sdk"></a>Dla klientów, którzy korzystają z istniejącego magazynu tabel Azure SDK są wszystkie zmiany?
Brak. Nie wprowadzono żadnych zmian dotyczących istniejących lub nowych klientów korzystających z istniejącego magazynu tabel Azure SDK. 

### <a name="how-do-i-view-table-data-that-is-stored-in-azure-cosmos-db-for-use-with-the-table-api-review"></a>Jak wyświetlić tabeli danych przechowywanych w usłudze Azure DB rozwiązania Cosmos do użycia przy użyciu interfejsu API tabeli (Przegląd)? 
Aby przeglądać dane, można użyć portalu Azure. Umożliwia także kodu interfejsu API tabeli (wersja zapoznawcza) lub narzędzia wymienione w następnej odpowiedzi. 

### <a name="which-tools-work-with-the-table-api-preview"></a>Narzędzia pracować z interfejsem API tabeli (wersja zapoznawcza)? 
Można użyć starszej wersji programu Azure Explorer (0.8.9).

Narzędzia elastyczność podjęcie parametry połączenia w formacie określonym wcześniej może obsłużyć nowy interfejs API tabeli (wersja zapoznawcza). Lista narzędzi tabeli są przekazywane w [narzędzi klienta magazynu Azure](../storage/common/storage-explorers.md) strony. 

### <a name="do-powershell-or-azure-cli-work-with-the-new-table-api-preview"></a>Programu PowerShell lub interfejsu wiersza polecenia Azure działają z nowym interfejsem API tabeli (wersja zapoznawcza)?
Planujemy dodanie obsługi dla środowiska PowerShell i interfejsu wiersza polecenia Azure API tabeli (wersja zapoznawcza). 

### <a name="is-the-concurrency-on-operations-controlled"></a>W operacji kontrolowane jest współbieżność?
Tak, optymistycznej współbieżności jest zapewniana przez użycie mechanizmu ETag. 

### <a name="is-the-odata-query-model-supported-for-entities"></a>Model zapytań OData jest obsługiwana dla jednostek? 
Tak, z tabeli interfejsu API (wersja zapoznawcza) obsługuje zapytania OData i zapytań LINQ. 

### <a name="can-i-connect-to-the-azure-table-storage-and-the-azure-cosmos-db-table-api-preview-side-by-side-in-the-same-application"></a>Można podłączyć do magazynu tabel Azure i rozwiązania Cosmos DB tabeli interfejsu API Azure (wersja zapoznawcza) obok siebie w tej samej aplikacji? 
Tak, można połączyć się przez utworzenie dwóch osobnych wystąpień CloudTableClient, każdy wskazujący własny identyfikator URI przy użyciu parametrów połączenia.

### <a name="how-do-i-migrate-an-existing-azure-table-storage-application-to-this-new-offering"></a>Jak przeprowadzić migrację istniejącej aplikacji magazynu tabel Azure z tą ofertą nowe?
Aby skorzystać z nową ofertę interfejsu API Azure rozwiązania Cosmos DB tabeli na podstawie istniejących danych magazynu tabeli, skontaktuj się z [ askcosmosdb@microsoft.com ](mailto:askcosmosdb@microsoft.com). 

### <a name="what-is-the-roadmap-for-this-service-and-when-will-you-offer-other-standard-table-api-functionality"></a>Co to jest plan dla tej usługi, a kiedy zostanie oferujesz innych standardowych funkcji API tabeli?
Planujemy dodanie obsługi tokeny sygnatury dostępu Współdzielonego, ServiceContext, statystykę, po stronie klienta szyfrowania, analizy i inne funkcje, jak możemy kontynuować kierunku po Użytkownik może Prześlij nam opinię na temat [Uservoice](https://feedback.azure.com/forums/599062-azure-cosmos-db-table-api). 

### <a name="how-is-expansion-of-the-storage-size-done-for-this-service-if-for-example-i-start-with-n-gb-of-data-and-my-data-will-grow-to-1-tb-over-time"></a>W jaki sposób rozszerzania rozmiaru magazynu zrobić dla tej usługi, jeśli na przykład uruchomić z  *n*  GB danych i danych wzrośnie do 1 TB w czasie? 
Azure DB rozwiązania Cosmos zaprojektowano w celu zapewnienia nieograniczony magazyn wykorzystaniem skalowanie w poziomie. Usługę można monitorować i skutecznie zwiększyć pamięć masową. 

### <a name="how-do-i-monitor-the-table-api-preview-offering"></a>Jak monitorować oferty tabeli interfejsu API (wersja zapoznawcza)?
Można użyć interfejsu API tabeli (wersja zapoznawcza) **metryki** okienku w portalu Azure do monitorowania żądań i wykorzystania magazynu. 

### <a name="how-do-i-calculate-the-throughput-i-require"></a>Sposób obliczania przepływność, którą potrzebuję?
Narzędzia do szacowania pojemności służy do obliczania TableThroughput, które są wymagane dla operacji. Aby uzyskać więcej informacji, zobacz [szacowania jednostek żądania i przechowywanie danych](https://www.documentdb.com/capacityplanner). Ogólnie rzecz biorąc można reprezentować jednostki jako JSON i podanie liczby operacji. 

### <a name="can-i-use-the-new-table-api-preview-sdk-locally-with-the-emulator"></a>Można użyć nowy interfejs API tabeli (wersja zapoznawcza) zestawu SDK lokalnie emulatorze?
Tak, można użyć interfejsu API tabeli (wersja zapoznawcza) w lokalnym emulatorze, korzystając z nowego zestawu SDK. Aby pobrać nowe emulatora, przejdź do [użyć emulatora usługi Azure rozwiązania Cosmos bazy danych dla lokalnych projektowania i testowania](local-emulator.md). Wartość StorageConnectionString w pliku app.config musi być:

```
DefaultEndpointsProtocol=https;AccountName=localhost;AccountKey=C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==;TableEndpoint=https://localhost:8081`. 
```

### <a name="can-my-existing-azure-table-storage-application-work-with-the-table-api-preview"></a>Moje istniejącej aplikacji magazynu tabel Azure można pracować przy użyciu interfejsu API tabeli (wersja zapoznawcza)? 
Powierzchni nowy interfejs API tabeli (wersja zapoznawcza) jest zgodna z istniejącego magazynu tabel Azure SDK create, delete, aktualizacji i operacje kwerend w interfejsu API programu .NET. Upewnij się, czy masz klucz wiersza, ponieważ tabela interfejsu API (wersja zapoznawcza) wymaga zarówno klucz partycji i klucz wiersza. Firma Microsoft planuje również dodać więcej obsługę zestawu SDK, jak możemy kontynuować kierunku GA tej oferty usługi.

### <a name="do-i-need-to-migrate-my-existing-azure-table-storage-applications-to-the-new-sdk-if-i-do-not-want-to-use-the-table-api-preview-features"></a>Należy przeprowadzić migrację istniejącego magazynu tabel Azure aplikacje do nowej wersji zestawu SDK, jeśli nie chce korzystać z funkcji API tabeli (wersja zapoznawcza)?
Nie można tworzyć i korzystać z istniejących zasobów magazynu tabel Azure bez przeszkód dowolnego rodzaju. Jednak jeśli nie używasz nowy interfejs API tabeli (wersja zapoznawcza), nie mogą korzystać z automatycznego indeksu, spójności dodatkowych opcji lub globalnego dystrybucji. 

### <a name="how-do-i-add-replication-of-the-data-in-the-table-api-preview-across-multiple-regions-of-azure"></a>Jak dodać replikacji danych w interfejsie API tabeli (wersja zapoznawcza) w wielu regionach platformy Azure?
Przy użyciu portalu Azure DB rozwiązania Cosmos [ustawienia globalne replikacji](tutorial-global-distribution-documentdb.md#portal) Aby dodać obszary, które są odpowiednie dla twojej aplikacji. Do opracowywania aplikacji globalnie rozproszone, należy również dodać aplikacji z informacjami o PreferredLocation ustawioną lokalny region zapewniające małych opóźnieniach odczytu. 

### <a name="how-do-i-change-the-primary-write-region-for-the-account-in-the-table-api-preview"></a>Jak zmienić regionu podstawowego zapisu dla konta w interfejsie API tabeli (wersja zapoznawcza)?
W okienku portalu globalnej replikacji bazy danych Azure rozwiązania Cosmos umożliwia dodawanie regionu, a następnie przełączyć się do regionu wymagane. Aby uzyskać instrukcje, zobacz [programowania z użyciem konta bazy danych Azure rozwiązania Cosmos w przypadku](regional-failover.md). 

### <a name="how-do-i-configure-my-preferred-read-regions-for-low-latency-when-i-distribute-my-data"></a>Jak skonfigurować Moje preferowanych regionów odczytu dla małych opóźnień I dystrybucji danych? 
Aby odczytać z lokalnej lokalizacji, należy użyć klucza PreferredLocation w pliku app.config. Istniejących aplikacji interfejsu API tabeli (wersja zapoznawcza) zwraca błąd, jeśli ustawiono LocationMode. Ten kod, należy usunąć, ponieważ premium tabeli interfejsu API (wersja zapoznawcza) przejmuje te informacje z pliku app.config. Aby uzyskać więcej informacji, zobacz [możliwości bazy danych Azure rozwiązania Cosmos](../cosmos-db/tutorial-develop-table-dotnet.md#azure-cosmos-db-capabilities).

### <a name="how-should-i-think-about-consistency-levels-in-the-table-api-preview"></a>Jak należy traktować poziomów spójności w interfejsie API tabeli (wersja zapoznawcza)? 
Azure DB rozwiązania Cosmos zapewnia dobrze uzasadnione kompromis między spójności, dostępnością i opóźnieniem. Azure DB rozwiązania Cosmos oferuje pięć poziomów spójności deweloperom tabeli interfejsu API (wersja zapoznawcza), aby można było wybrać model prawo spójności na poziomie tabeli i tworzenie pojedynczych żądania podczas wykonywania zapytań. Gdy klient nawiąże połączenie, może określać poziomu spójności. Można zmienić poziom przez ustawienie wartości klucza TableConsistencyLevel app.config. 

Interfejs API tabeli (wersja zapoznawcza) zawiera małe opóźnienia odczytuje z "odczytać własne zapisy" z spójność sesji jako domyślny. Aby uzyskać więcej informacji, zobacz [poziomy spójności](consistency-levels.md). 

Domyślnie Magazyn tabel Azure zapewnia wysoki poziom spójności w ramach regionu i spójność Eventual w lokalizacji dodatkowej. 

### <a name="does-the-azure-cosmos-db-table-api-offer-more-consistency-levels-than-azure-table-storage"></a>Interfejs API Azure rozwiązania Cosmos DB tabeli oferuje więcej poziomów spójności niż w przypadku magazynu tabel Azure?
Tak, aby uzyskać informacje na temat korzystania z bazy danych Azure rozwiązania Cosmos Rozproszony charakter, zobacz [poziomy spójności](consistency-levels.md). Ponieważ dla poziomów spójności podano gwarancji, można używać ich bez obaw. Aby uzyskać więcej informacji, zobacz [możliwości bazy danych Azure rozwiązania Cosmos](../cosmos-db/tutorial-develop-table-dotnet.md#azure-cosmos-db-capabilities).

### <a name="when-global-distribution-is-enabled-how-long-does-it-take-to-replicate-the-data"></a>Podczas dystrybucji globalnego jest włączone, jak długo trwa replikowanie danych?
Azure DB rozwiązania Cosmos zatwierdza danych trwałym w regionie lokalnych i wypychanie danych do innych regionów natychmiast w sprawie milisekundy. Replikacja jest zależne tylko od czasu Rundy (RTT) w centrum danych. Aby dowiedzieć się więcej na temat możliwości dystrybucji globalnej bazy danych rozwiązania Cosmos Azure, zobacz [bazy danych Azure rozwiązania Cosmos: globalnie rozproszoną bazę danych usługi Azure](distribute-data-globally.md).

### <a name="can-the-read-request-consistency-level-be-changed"></a>Można zmienić poziomu spójności żądanie odczytu?
Z bazy danych rozwiązania Cosmos platformy Azure można ustawić poziomu spójności na poziomie kontenera (dla tabeli). Za pomocą zestawu SDK, można zmienić poziom, podając wartość klucza TableConsistencyLevel w pliku app.config. Możliwe wartości to: silne, ograniczone nieaktualności sesji, prefiks spójne i Eventual. Aby uzyskać więcej informacji, zobacz [danych dostosowywalne poziomy spójności w usłudze Azure DB rozwiązania Cosmos](consistency-levels.md). Pomysł klucza jest, że nie można ustawić spójności żądania poziomu więcej niż ustawienie dla tabeli. Na przykład nie można ustawić poziomu spójności dla tabeli Eventual i na poziomie spójności żądania na silne. 

### <a name="how-does-the-table-api-preview-account-handle-failover-if-a-region-goes-down"></a>Jak konto tabeli interfejsu API (wersja zapoznawcza) obsługuje tryb failover Jeśli region przestanie działać? 
Rozwiązania Cosmos DB tabeli interfejsu API Azure (wersja zapoznawcza) obiektowy globalnie rozproszone platformy Azure DB rozwiązania Cosmos. Aby upewnić się, że aplikacja może tolerować przestoju centrum danych, należy włączyć co najmniej jeden region więcej konta w portalu Azure DB rozwiązania Cosmos [programowania z użyciem konta bazy danych Azure rozwiązania Cosmos w przypadku](regional-failover.md). Przy użyciu portalu można ustawić priorytet regionu [programowania z użyciem konta bazy danych Azure rozwiązania Cosmos w przypadku](regional-failover.md). 

Możesz dodać dowolną liczbę regionów, jak dla konta i kontrolować, gdzie ona może przełączyć zapewniając priorytet trybu failover. Oczywiście korzystanie z bazy danych, musisz podać aplikacji istnieje zbyt. Jeśli tak zrobisz, klienci nie wystąpi Przestój. Klient SDK jest automatycznie homing. Oznacza to, że może wykryć regionu, który nie działa i automatycznie przełączyć nowy region.

### <a name="is-the-table-api-preview-enabled-for-backups"></a>Interfejs API tabeli (wersja zapoznawcza) jest włączona dla kopii zapasowych?
Tak, rozwiązania Cosmos DB tabeli interfejsu API Azure (wersja zapoznawcza) obiektowy platformy Azure DB rozwiązania Cosmos kopii zapasowych. Kopie zapasowe są wykonywane automatycznie. Aby uzyskać więcej informacji, zobacz [Online kopii zapasowej i przywracania bazy danych Azure rozwiązania Cosmos](online-backup-and-restore.md).

 
### <a name="does-the-table-api-preview-index-all-attributes-of-an-entity-by-default"></a>Powoduje API tabeli (wersja zapoznawcza) indeksowania wszystkie atrybuty obiektu domyślnie
Tak, wszystkie atrybuty obiektu jest indeksowana domyślnie w programie Azure DB rozwiązania Cosmos. Aby uzyskać więcej informacji, zobacz [bazy danych Azure rozwiązania Cosmos: zasady indeksowania](indexing-policies.md). 

### <a name="does-this-mean-i-do-not-have-to-create-multiple-indexes-to-satisfy-the-queries"></a>Jest to średnia I nie trzeba tworzyć wiele indeksów do zaspokojenia zapytania? 
Tak, bazy danych rozwiązania Cosmos Azure zapewnia automatyczne indeksowanie wszystkie atrybuty bez żadnych definicji schematu. Ta Automatyzacja zwalnia deweloperom fokusu w aplikacji, a nie na tworzenie indeksu i zarządzanie nimi. Aby uzyskać więcej informacji, zobacz [bazy danych Azure rozwiązania Cosmos: zasady indeksowania](indexing-policies.md).

### <a name="can-i-change-the-indexing-policy"></a>Czy można zmienić zasady indeksowania?
Tak, można zmienić zasady indeksowania, podając definicję indeksu. Aby uzyskać więcej informacji, zobacz [możliwości bazy danych Azure rozwiązania Cosmos](../cosmos-db/tutorial-develop-table-dotnet.md#azure-cosmos-db-capabilities). Należy poprawnie kodowania i wprowadzić ustawienia. 

W formacie json ciąg w pliku app.config:
```
{
  "indexingMode": "consistent",
  "automatic": true,
  "includedPaths": [
    {
      "path": "/somepath",
      "indexes": [
        {
          "kind": "Range",
          "dataType": "Number",
          "precision": -1
        },
        {
          "kind": "Range",
          "dataType": "String",
          "precision": -1
        } 
      ]
    }
  ],
  "excludedPaths": 
[
 {
      "path": "/anotherpath"
 }
]
}
```

### <a name="azure-cosmos-db-as-a-platform-seems-to-have-lot-of-capabilities-such-as-sorting-aggregates-hierarchy-and-other-functionality-will-you-be-adding-these-capabilities-to-the-table-api"></a>Azure DB rozwiązania Cosmos jako platforma wydaje się, że ma wiele funkcji, takich jak sortowanie, agregacje, hierarchii oraz innych funkcji. Będzie można dodawanie tych funkcji do interfejsu API tabeli? 
W wersji zapoznawczej interfejsu API tabeli zapewnia te same funkcje zapytania jako magazynu tabel Azure. Azure DB rozwiązania Cosmos obsługuje również sortowanie, agregacje, dane geograficzne zapytania hierarchii i szeroki zakres funkcji wbudowanych. Firma Microsoft udostępni dodatkowe funkcje w interfejsie API tabeli w przyszłej aktualizacji. Aby uzyskać więcej informacji, zobacz [kwerendy SQL dla interfejsu API Azure rozwiązania Cosmos bazy danych usługi DocumentDB](../documentdb/documentdb-sql-query.md).
 
### <a name="when-should-i-change-tablethroughput-for-the-table-api-preview"></a>Podczas zmiany TableThroughput dla tabeli interfejsu API (wersja zapoznawcza)?
Należy zmienić TableThroughput, gdy stosuje się jeden z następujących warunków:
* Przeprowadzasz wyodrębniania, przekształcania i ładowania (ETL) danych lub chcesz przekazać dużą ilość danych w krótkim czasie. 
* Potrzebujesz więcej przepustowości z kontenera w wewnętrznej. Na przykład zobacz przepływności używane jest większa niż udostępnionej przepływności, czy możesz są pobierania ograniczane. Aby uzyskać więcej informacji, zobacz [przepływności zestawu dla kontenerów bazy danych Azure rozwiązania Cosmos](set-throughput.md).

### <a name="can-i-scale-up-or-scale-down-the-throughput-of-my-table-api-preview-table"></a>Można skalować lub dół przepływność tabeli tabeli interfejsu API (wersja zapoznawcza)? 
Tak, można użyć portalu Azure DB rozwiązania Cosmos skali okienku można skalować przepływność. Aby uzyskać więcej informacji, zobacz [przepływności zestawu](set-throughput.md).

### <a name="is-a-default-tablethroughput-set-for-newly-provisioned-tables"></a>Jest domyślnie ustawione dla nowo aprowizowanej tabel TableThroughput?
Tak, jeśli nie zastępują TableThroughput przy użyciu pliku app.config i nie należy używać kontenera utworzone wcześniej w usłudze Azure DB rozwiązania Cosmos, usługa tworzy tabelę z przepływności 400.
 
### <a name="is-there-any-change-of-pricing-for-existing-customers-of-azure-table-storage"></a>Istnieje już zmiany ceny dla istniejących klientów z magazynem tabel Azure?
Brak. Nie została zmieniona w cenie dla istniejących klientów usługi Magazyn tabel Azure. 

### <a name="how-is-the-price-calculated-for-the-table-api-preview"></a>Sposób obliczania ceny dla tabeli interfejsu API (wersja zapoznawcza)? 
Cena rozwiązania Cosmos DB tabeli interfejsu API Azure (wersja zapoznawcza) jest zależna od TableThroughput przydzielone. 

### <a name="how-do-i-handle-any-throttling-in-table-api-preview-offering"></a>Jak obsługiwać żadnych ograniczania przepustowości w ofercie tabeli interfejsu API (wersja zapoznawcza) 
Jeśli szybkość żądania przekracza pojemność udostępnionej przepływności dla podstawowej kontenera, wystąpi błąd i zestawu SDK ponowi wywołanie, stosując zasady ponawiania.

### <a name="why-do-i-need-to-choose-a-throughput-apart-from-partitionkey-and-rowkey-to-take-advantage-of-the-azure-cosmos-db-table-api-preview"></a>Dlaczego należy wybrać przepływności oprócz PartitionKey i RowKey, aby móc korzystać z rozwiązania Cosmos DB tabeli interfejsu API Azure (wersja zapoznawcza)
Azure DB rozwiązania Cosmos ustawia przepływności domyślnego kontenera sieci, jeśli nie zostanie określona w pliku app.config. 

Azure DB rozwiązania Cosmos zapewnia gwarancje wydajność i opóźnienia z górne granice operacji. Gwarancja jest możliwe, gdy aparat można wymusić ładu na operacje dzierżawcy. Ustawienie TableThroughput zapewnia uzyskać gwarantowanej przepustowości i opóźnień, ponieważ platforma rezerwuje tej pojemności i gwarantującej Powodzenie operacyjnej. 

Przy użyciu specyfikacji przepływności, elastycznie zmianą jego korzystać z sezonowości aplikacji, spełniają potrzeb w zakresie przepustowości i ograniczenia kosztów.

### <a name="azure-storage-sdk-has-been-very-inexpensive-for-me-because-i-pay-only-to-store-the-data-and-i-rarely-query-the-new-azure-cosmos-db-offering-seems-to-be-charging-me-even-though-i-have-not-performed-a-single-transaction-or-stored-anything-can-you-please-explain"></a>Zestawu SDK usługi Magazyn Azure zostało niedrogie dla mnie, ponieważ I Zapłać tylko do przechowywania danych i I rzadko zapytania. Nową ofertę bazy danych Azure rozwiązania Cosmos wydaje się być ładowania mnie, nawet jeśli nie ma I wykonać jednej transakcji lub niczego przechowywane. Sprawdź jakie?

Azure DB rozwiązania Cosmos została zaprojektowana jako globalnie rozproszone, na podstawie umowy SLA systemu z gwarancją dostępności, opóźnienia i przepływności. Podczas rezerwowania przepustowości w usłudze Azure DB rozwiązania Cosmos gwarantowane, w odróżnieniu od przepustowości innych systemów. Azure DB rozwiązania Cosmos zapewnia dodatkowe funkcje, które żądane klientów, takie jak indeksów pomocniczych i dystrybucji globalnego.  

### <a name="i-never-get-a-quota-full-notification-indicating-that-a-partition-is-full-when-i-ingest-data-into-azure-table-storage-with-the-table-api-preview-i-do-get-this-message-is-this-offering-limiting-me-and-forcing-me-to-change-my-existing-application"></a>Nigdy nie pojawia się powiadomienie "przydział pełna" (co oznacza, że partycji jest pełny) po pozyskiwania danych do magazynu tabel Azure. Przy użyciu tabeli interfejsu API (wersja zapoznawcza) pojawia się w tej wiadomości. Jest to oferta ograniczenie mnie i wymuszania, żeby można było zmienić mojej istniejącej aplikacji?

Azure DB rozwiązania Cosmos jest system na podstawie umowy SLA zawiera nieograniczone skali gwarancje opóźnień, przepustowości, dostępności i spójności. W celu zapewnienia wydajności premium gwarantowane, upewnij się, że rozmiar danych i indeks są łatwe w zarządzaniu i skalowalność. 10 GB limit liczby jednostek lub elementów na klucz partycji jest upewnij się, że udostępniamy doskonałą wydajność wyszukiwania i zapytania. Aby upewnić się, że aplikacja skaluje dobrze nawet w przypadku usługi Azure Storage, zaleca się, że możesz *nie* utworzyć partycję gorących wszystkie informacje są przechowywane w jednej partycji i badając ją. 

### <a name="so-partitionkey-and-rowkey-are-still-required-with-the-new-table-api-preview"></a>Dlatego PartitionKey i RowKey są nadal wymagane z nowym interfejsem API tabeli (wersja zapoznawcza)? 
Tak. Ponieważ powierzchni interfejsu API tabeli (wersja zapoznawcza) jest podobny do magazynu tabel Azure SDK, klucz partycji zapewnia efektywną dystrybucję danych. Klucz wiersza jest unikatowy w ramach tej partycji. Klucz wiersza musi być obecny i nie może mieć wartości null jak standardowy zestaw SDK. Długość RowKey wynosi 255 bajtów i długość PartitionKey wynosi 100 bajtów (wkrótce można zwiększyć 1 KB). 

### <a name="what-are-the-error-messages-for-the-table-api-preview"></a>Co to są komunikaty o błędach dla interfejsu API tabeli (wersja zapoznawcza)?
Ponieważ ta wersja zapoznawcza jest zgodny z magazynem tabel Azure, większość błędów przypisze do błędy z tabeli standardowa. 

### <a name="why-do-i-get-throttled-when-i-try-to-create-lot-of-tables-one-after-another-in-the-table-api-preview"></a>Dlaczego I pobrać ograniczany podczas próby utworzenia jeden po drugim wiele tabel w interfejsie API tabeli (wersja zapoznawcza)?
Azure DB rozwiązania Cosmos jest system na podstawie umowy SLA zawiera czas oczekiwania, przepływności i dostępności gwarancje spójności. Ponieważ jest inicjowana systemu rezerwuje zasobów, aby zagwarantować te wymagania. Wykryto szybkość szybkie tworzenie tabel i ograniczany. Firma Microsoft zaleca przyjrzeć się częstotliwość tworzenia tabel i obniżyć go do mniej niż 5 na minutę. Należy pamiętać, że API tabeli (wersja zapoznawcza) jest inicjowana systemu. Obecnie obsługi administracyjnej, rozpocznie się zwrócić dla niego. 

## <a name="develop-against-the-graph-api-preview"></a>Opracowywanie względem wykresu interfejsu API (wersja zapoznawcza)
### <a name="how-can-i-apply-the-functionality-of-graph-api-preview-to-azure-cosmos-db"></a>Jak można stosować funkcje interfejsu API programu Graph (wersja zapoznawcza) do bazy danych rozwiązania Cosmos Azure?
Biblioteka rozszerzenia można użyć do zastosowania funkcji interfejsu API programu Graph (wersja zapoznawcza). Ta biblioteka jest nazywana Microsoft Azure wykresy, i jest dostępny na NuGet. 

### <a name="it-looks-like-you-support-the-gremlin-graph-traversal-language-do-you-plan-to-add-more-forms-of-query"></a>Wygląda na to, obsługują język przechodzenie Gremlin wykresu. Czy planowane jest dodanie więcej formularzy zapytania?
Tak, firma Microsoft planuje dodanie innych mechanizmów dla zapytania w przyszłości. 

### <a name="how-can-i-use-the-new-graph-api-preview-offering"></a>Jak używać nową ofertę interfejsu API programu Graph (wersja zapoznawcza)? 
Aby rozpocząć pracę, należy wykonać [interfejsu API programu Graph](../cosmos-db/create-graph-dotnet.md) artykułu szybki start.

<a id="moving-to-cosmos-db"></a>
## <a name="questions-from-documentdb-customers"></a>Pytania klientów usługi DocumentDB
### <a name="why-are-you-moving-to-azure-cosmos-db"></a>Dlaczego należy przechodzenia do bazy danych rozwiązania Cosmos Azure? 

Azure DB rozwiązania Cosmos jest duży dalej zwiększenie globalnie rozproszone, baz danych z chmury na dużą skalę. Jako klient usługi DocumentDB teraz ma dostęp do systemu przełomowe i możliwości oferowane przez bazy danych Azure rozwiązania Cosmos.

Azure DB rozwiązania Cosmos jest uruchomiona jako usługa "Florencji projektu" w 2010, aby rozwiązać problemy związane z napotykają deweloperom tworzenie aplikacji na dużą skalę w firmie Microsoft. Wyzwania tworzenia globalnie rozproszone aplikacje nie są unikatowe do firmy Microsoft, więc wprowadziliśmy pierwsza generacja tej technologii dostępnych w 2015 dla deweloperów platformy Azure w postaci usługi Azure DocumentDB. 

Od tego czasu dodane nowe funkcje i wprowadzono istotne nowe funkcje. Azure DB rozwiązania Cosmos jest wynik. W ramach tej wersji, klienci usługi DocumentDB, z danymi, automatycznie i bezproblemowo stają się klientów z bazy danych Azure rozwiązania Cosmos. Te możliwości są w obszarach core aparatu bazy danych, a także globalne dystrybucji, elastyczną skalowalność i branży, kompleksowe umów SLA. W szczególności firma Microsoft usprawnionych aparat bazy danych Azure DB rozwiązania Cosmos wydajnie mapowania wszystkich modeli danych popularnych systemów typu i interfejsów API do właściwego modelu danych z bazy danych Azure rozwiązania Cosmos. 

Bieżący reprezentację developer uwzględniającym tej pracy jest nowa funkcja obsługi [Gremlin](../cosmos-db/graph-introduction.md) i [API tabeli](../cosmos-db/table-introduction.md). I to tylko początek. Planujemy dodać innych popularnych interfejsów API i nowszych modelach danych w czasie z więcej poprawę wydajności i magazynu w skali globalnej. 

Ważne jest, aby podkreślić, że DocumentDB [dialekt SQL](../documentdb/documentdb-sql-query.md) zawsze był tylko jeden z wielu interfejsów API, które obsługują podstawowej bazy danych rozwiązania Cosmos platformy Azure. Dla deweloperów, którzy Użyj w pełni zarządzana usługa, takie jak bazy danych Azure rozwiązania Cosmos tylko interfejs do usługi jest interfejsów API, które są udostępniane przez usługę. Naprawdę żadne zmiany dla istniejących klientów usługi DocumentDB. W usłudze Azure DB rozwiązania Cosmos możesz uzyskać dokładnie tego samego interfejsu API SQL, który udostępnia usługi DocumentDB. I obecnie i w przyszłości można uzyskać dostęp innych funkcji wcześniej niedostępny 

Inny reprezentację naszych ciągłość pracy jest podstawą rozszerzonej globalne i elastyczną skalowalność przepływność i magazyn. Wprowadzono kilka ulepszeń podstawowych podsystemu dystrybucji globalnego. Jedną z wielu takich developer uwzględniającym funkcji jest modelu spójne prefiksu spójności, co sprawia, że całkowita pięć modeli dobrze zdefiniowany spójności. Firma Microsoft opublikuje wiele możliwości bardziej interesujące ich. 

### <a name="what-do-i-need-to-do-to-ensure-that-my-documentdb-resources-continue-to-run-on-azure-cosmos-db"></a>Co należy zrobić, aby upewnić się, że nadal działać dla bazy danych Azure rozwiązania Cosmos zasoby usługi DocumentDB?

Należy nie wprowadzać zmian w ogóle. Zasoby usługi DocumentDB są teraz zasobów bazy danych Azure rozwiązania Cosmos, a nie było bez przerwy w działaniu usługi, gdy wystąpił to przeniesienie.

### <a name="what-changes-do-i-need-to-make-for-my-app-to-work-with-azure-cosmos-db"></a>Jakie zmiany należy do mojej aplikacji do pracy z bazy danych rozwiązania Cosmos Azure?

Nie wprowadzono żadnych zmian, aby. Nie zmieniono nazw klas, obszary nazw i NuGet w pakiecie. Zawsze firma Microsoft zaleca zachowywanie z zestawów SDK aktualne w celu korzystać z najnowszych funkcji i ulepszeń. 

### <a name="whats-changed-in-the-azure-portal"></a>Co się zmieniło w portalu Azure?

Usługa DocumentDB nie jest już widoczna w portalu jako usługi Azure. W tym miejscu jest nową ikonę bazy danych Azure rozwiązania Cosmos, jak pokazano na poniższej ilustracji. Dostępnych wszystkich kolekcji, które znajdowały się przed i nadal można skalować przepływność, Zmień poziomy spójności i monitora umów SLA. Zostały rozszerzone możliwości Eksploratora danych (wersja zapoznawcza). Można teraz wyświetlać i edycji dokumentów, tworzenie i uruchamianie zapytań i pracować z procedur składowanych, wyzwalaczy i funkcji zdefiniowanej przez użytkownika z jednej strony, jak pokazano na poniższej ilustracji: 

![Blok Azure rozwiązania Cosmos bazy danych kolekcji](./media/faq/cosmos-db-data-explorer.png)

### <a name="are-there-changes-to-pricing"></a>Czy istnieją zmiany cennika?

Nie, kosztów działających aplikacji dla bazy danych Azure rozwiązania Cosmos jest taka sama sprzed.

### <a name="are-there-changes-to-the-slas"></a>Czy istnieją zmiany wymogów umów SLA?

Nie, umowy SLA dla dostępności, spójności, opóźnienia i przepływności nie uległy zmianie, nadal są wyświetlane w portalu. Aby uzyskać więcej informacji, zobacz [umowy SLA dla bazy danych Azure rozwiązania Cosmos](https://azure.microsoft.com/support/legal/sla/cosmos-db/).
   
![Zadania do wykonania aplikacji z przykładowymi danymi](./media/faq/azure-cosmosdb-portal-metrics-slas.png)


[azure-portal]: https://portal.azure.com
[query]: documentdb-sql-query.md
