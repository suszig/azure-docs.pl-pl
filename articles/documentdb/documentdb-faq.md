<properties 
    pageTitle="Pytania dotyczące bazy danych usługi DocumentDB — często zadawane pytania | Microsoft Azure" 
    description="Odpowiedzi na często zadawane pytania dotyczące usługi Azure DocumentDB, która jest usługą bazy danych dokumentów NoSQL dla formatu JSON. Odpowiedzi na pytania dotyczące pojemności, poziomów wydajności i skalowania bazy danych." 
    keywords="Database questions, frequently asked questions, documentdb, azure, Microsoft azure"
    services="documentdb" 
    authors="mimig1" 
    manager="jhubbard" 
    editor="monicar" 
    documentationCenter=""/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="03/30/2016" 
    ms.author="mimig"/>


#Często zadawane pytania dotyczące usługi DocumentDB

## Pytania dotyczące bazy danych usługi Microsoft Azure DocumentDB — podstawy

### Co to jest usługa Microsoft Azure DocumentDB? 
Usługa Microsoft Azure DocumentDB to wysoce skalowalna usługa bazy danych dokumentów NoSQL, oferująca zaawansowaną obsługę zapytań w oparciu o dane bez schematu oraz konfigurowalną i niezawodną wydajność, a także umożliwiająca błyskawiczne wdrażanie — wszystko dzięki zarządzanej platformie wspieranej przez możliwości i zasięg platformy Microsoft Azure. DocumentDB to odpowiednie rozwiązanie dla aplikacji sieci Web, aplikacji mobilnych, gier oraz aplikacji Internetu rzeczy, w przypadku których kluczowe znaczenie mają przewidywalna przepływność, skalowalność, małe opóźnienia i model danych bez schematu. Usługa DocumentDB zapewnia elastyczność schematu i rozbudowane indeksowanie za pomocą natywnego modelu danych JSON, a także oferuje obsługę transakcyjną wielu dokumentów dzięki zintegrowanemu środowisku JavaScript.  
  
Więcej pytań na temat bazy danych, odpowiedzi i instrukcje dotyczące instalowania i korzystania z tej usługi można znaleźć na [stronie dokumentacji usługi DocumentDB](https://azure.microsoft.com/documentation/services/documentdb/).

### Jakiego rodzaju bazą danych jest usługa DocumentDB?
DocumentDB jest obiektową bazą danych dokumentów NoSQL, która przechowuje dane w formacie JSON.  Baza danych DocumentDB obsługuje zagnieżdżone, niezależne struktury danych, dla których można tworzyć zapytania za pomocą rozbudowanej [gramatyki zapytań SQL](documentdb-sql-query.md) usługi DocumentDB. DocumentDB zapewnia wysoką wydajność przetwarzania transakcyjnego języka JavaScript po stronie serwera za pomocą [procedur składowanych, wyzwalaczy i funkcji zdefiniowanych przez użytkownika](documentdb-programming.md). Baza danych obsługuje również dostosowywalne poziomy spójności dla deweloperów, ze skojarzonymi [poziomami wydajności](documentdb-performance-levels.md).
 
### Czy bazy danych usługi DocumentDB mają tabele takie jak relacyjna baza danych (RDBMS)?
Nie, usługa DocumentDB przechowuje dane w kolekcjach dokumentów JSON.  Informacje dotyczące zasobów usługi DocumentDB można znaleźć w temacie [Pojęcia i model zasobów bazy danych DocumentDB](documentdb-resources.md). 

### Czy bazy danych usługi DocumentDB obsługują dane bez schematu?
Tak, usługa DocumentDB umożliwia aplikacjom przechowywanie dowolnych dokumentów JSON bez definicji schematu lub wskazówek. Dane są natychmiast dostępne dla zapytania dzięki interfejsowi zapytań SQL usługi DocumentDB.   

### Czy usługa DocumentDB obsługuje transakcje ACID?
Tak, usługa DocumentDB obsługuje transakcje dla wielu dokumentów wyrażone jako procedury składowane JavaScript i wyzwalacze. Transakcje są dostosowane do jednej partycji w ramach każdej kolekcji i wykonywane przy użyciu semantyki ACID jako wszystkie lub żadne, odizolowane od innego współbieżnie wykonywanego kodu i żądań użytkownika.  Jeśli istnieją wyjątki zgłaszane przez wykonanie kodu aplikacji JavaScript po stronie serwera, cała transakcja zostanie wycofana. 

### Jakie są typowe zastosowania usługi DocumentDB?  
DocumentDB to dobre rozwiązanie dla nowych aplikacji sieci Web, aplikacji mobilnych, gier i aplikacji Internetu rzeczy, w przypadku których istotne znaczenie mają automatyczna skalowalność, przewidywalna wydajność, krótkie czasy reakcji liczone w milisekundach oraz możliwość obsługi zapytań w oparciu o dane bez schematu. Usługa DocumentDB pozwala na szybkie wdrażanie, opracowywanie i obsługę ciągłej iteracji modeli danych aplikacji. Aplikacje zarządzające zawartością i danymi generowanymi przez użytkowników to [typowe przypadki użycia usługi DocumentDB](documentdb-use-cases.md).  

### W jaki sposób usługa DocumentDB zapewnia przewidywalną wydajność?
Jednostka żądania to miara przepływności w usłudze DocumentDB. Jedna jednostka żądania odpowiada przepływności pobierania dokumentu o rozmiarze 1 KB. Każda operacja w bazie danych DocumentDB, łącznie z odczytami, zapisami, zapytaniami SQL i wykonywaniem procedur składowanych, ma deterministyczną wartość jednostki żądania określoną na podstawie przepływności wymaganej do ukończenia operacji. Dzięki zastosowaniu jednostek żądania nie trzeba myśleć o procesorze CPU, operacjach we/wy i pamięci oraz ich wpływie na przepływność aplikacji.

Każda kolekcja usługi DocumentDB może być zarezerwowana z aprowizowaną przepływnością wyrażoną jako liczba jednostek żądania przepływności na sekundę. W przypadku aplikacji o dowolnej skali można wykonać testy porównawcze poszczególnych żądań w celu pomiaru ich wartości jednostki żądania i udostępnić kolekcje, aby obsługiwały łączną sumę liczby jednostek żądania dla wszystkich żądań. Można również skalować przepływność kolekcji odpowiednio do zmieniających się potrzeb aplikacji. Aby uzyskać więcej informacji na temat jednostek żądania i określania liczby wymaganych kolekcji, przeczytaj artykuł [Zarządzanie wydajnością i pojemnością](documentdb-manage.md).

### Czy usługa DocumentDB jest zgodna z ustawą HIPAA?
Tak, usługa DocumentDB jest zgodna z ustawą HIPAA. Ustawa HIPAA określa wymogi dotyczące wykorzystywania, ujawniania i ochrony informacji o zdrowiu umożliwiających indywidualną identyfikację osoby. Aby uzyskać więcej informacji, zobacz [Centrum zaufania Microsoft](https://www.microsoft.com/en-us/TrustCenter/Compliance/HIPAA).

### Jakie są limity magazynu usługi DocumentDB? 
Nie ma żadnego teoretycznego limitu całkowitej ilości danych, które może przechowywać kolekcja w usłudze DocumentDB. Jeśli chcesz przechowywać więcej niż 250 GB danych w jednej kolekcji, skontaktuj się z [pomocą techniczną](documentdb-increase-limits.md) w celu zwiększenia limitu przydziału konta. 

### Jakie są ograniczenia przepływności usługi DocumentDB? 
Jeśli obciążenie może być dystrybuowane w przybliżeniu równomiernie między wystarczająco dużą liczbę kluczy partycji, teoretycznie nie ma ograniczenia całkowitej przepływności danych, jaką kolekcja może obsługiwać w usłudze DocumentDB. Jeśli chcesz przekraczać 250 000 jednostek żądań/sekundę dla kolekcji lub konta, skontaktuj się z [pomocą techniczną](documentdb-increase-limits.md) w celu zwiększenia limitu przydziału konta. 

### Ile kosztuje usługa Microsoft Azure DocumentDB?
Szczegółowe informacje można znaleźć na stronie [szczegółów cennika usługi DocumentDB](https://azure.microsoft.com/pricing/details/documentdb/). Opłaty za użycie usługi DocumentDB są określane na podstawie liczby kolekcji w użyciu, liczby godzin, przez jaką kolekcje były w trybie online, oraz użytego miejsca do magazynowania i udostępnionej przepływności dla każdej kolekcji. 

### Czy jest dostępne bezpłatne konto?
Jeśli dopiero zaczynasz korzystać z platformy Azure, możesz zarejestrować się i utworzyć [bezpłatne konto platformy Azure](https://azure.microsoft.com/pricing/free-trial/), które zapewnia 30 dni i 200 USD na wypróbowanie usług Azure. Ewentualnie, jeśli masz subskrypcję programu Visual Studio, przysługuje Ci [150 USD miesięcznie w ramach bezpłatnych środków platformy Azure](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) na korzystanie z dowolnej usługi Azure.  

### Jak uzyskać dodatkową pomoc dotyczącą usługi DocumentDB?
Jeśli potrzebujesz pomocy, skontaktuj się z nami za pośrednictwem [witryny Stack Overflow](http://stackoverflow.com/questions/tagged/azure-documentdb), [forów deweloperów MSDN poświęconych usłudze Azure DocumentDB](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureDocumentDB) lub zaplanuj [indywidualną rozmowę z zespołem inżynierów usługi DocumentDB](http://www.askdocdb.com/). Aby być na bieżąco z najnowszymi informacjami i funkcjami usługi DocumentDB, obserwuj nas na [Twitterze](https://twitter.com/DocumentDB).

## Konfigurowanie usługi Microsoft Azure DocumentDB

### Jak zarejestrować się w usłudze Microsoft Azure DocumentDB?
Usługa Microsoft Azure DocumentDB jest dostępna w [portalu Azure][azure-portal].  Najpierw musisz zarejestrować się, aby uzyskać subskrypcję platformy Microsoft Azure.  Po zarejestrowaniu się i uzyskaniu subskrypcji platformy Microsoft Azure można do niej dodać konto usługi DocumentDB. Instrukcje dotyczące dodawania konta usługi DocumentDB zawiera artykuł [Tworzenie konta usługi DocumentDB](documentdb-create-account.md).   

### Co to jest klucz główny?
Klucz główny jest tokenem zabezpieczającym dostęp do wszystkich zasobów dla konta. Osoby, które mają klucz, mają dostęp do odczytu i zapisu dla wszystkich zasobów w ramach konta bazy danych. Podczas dystrybucji kluczy głównych należy zachować ostrożność. Podstawowy klucz główny i pomocniczy klucz główny są dostępne w bloku **Klucze** [portalu Azure][azure-portal]. Aby uzyskać więcej informacji na temat kluczy, zobacz [Wyświetlanie, kopiowanie i ponowne generowanie kluczy dostępu](documentdb-manage-account.md#keys).

### Jak utworzyć bazę danych?
Bazy danych można tworzyć przy użyciu [portalu Azure]() zgodnie z opisem w artykule [Tworzenie bazy danych usługi DocumentDB](documentdb-create-database.md), jednego z [zestawów SDK usługi DocumentDB](documentdb-sdk-dotnet.md) lub za pomocą [interfejsów API REST](https://msdn.microsoft.com/library/azure/dn781481.aspx).  

### Co to jest kolekcja?
Kolekcja jest kontenerem dokumentów JSON i skojarzonej logiki aplikacji JavaScript. Kolekcja to płatna jednostka, w przypadku której [koszt](documentdb-performance-levels.md) jest określany na podstawie poziomu wydajności skojarzonego z tą kolekcją. Kolekcje mogą znajdować się na jednej lub wielu partycjach/serwerach i mogą być skalowane do obsługi praktycznie nieograniczonej ilości magazynu lub przepływności.

Kolekcje są również jednostkami rozliczeniowymi dla usługi DocumentDB. Każda kolekcja jest rozliczana zgodnie z liczbą godzin na podstawie udostępnionej przepływności i używanego miejsca w magazynie. Aby uzyskać więcej informacji, zobacz [cennik usługi DocumentDB](https://azure.microsoft.com/pricing/details/documentdb/).  

### Jak skonfigurować użytkowników i uprawnienia?
Użytkowników i uprawnienia można utworzyć przy użyciu jednego z [zestawów SDK usługi DocumentDB](documentdb-sdk-dotnet.md) lub za pomocą [interfejsów API REST](https://msdn.microsoft.com/library/azure/dn781481.aspx).   

## Pytania dotyczące tworzenia oprogramowania dla usługi Microsoft Azure DocumentDB

### Jak rozpocząć tworzenie oprogramowania dla usługi DocumentDB?
[Zestawy SDK](documentdb-sdk-dotnet.md) są dostępne dla języków .NET, Python, Node.js, JavaScript i Java.  Można również używać interfejsów [API RESTful i usług HTTP](https://msdn.microsoft.com/library/azure/dn781481.aspx) w celu interakcji z zasobami usługi DocumentDB za pomocą różnych platform i języków. 

Przykłady dla zestawów SDK usługi DocumentDB dla języków [.NET](documentdb-dotnet-samples.md), [Java](https://github.com/Azure/azure-documentdb-java), [Node.js](documentdb-nodejs-samples.md) i [Python](documentdb-python-samples.md) są dostępne w witrynie GitHub.

### Czy usługa DocumentDB obsługuje język SQL?
Język zapytań SQL usługi DocumentDB jest rozszerzonym podzbiorem funkcji zapytań obsługiwanych przez język SQL. Język zapytań SQL usługi DocumentDB zapewnia bogaty zestaw operatorów relacyjnych i hierarchicznych oraz rozszerzalność dzięki opartym na języku JavaScript funkcjom definiowanym przez użytkownika. Gramatyka JSON umożliwia modelowanie dokumentów JSON jako drzewa z etykietami jako węzłami drzewa, które to modelowanie jest używane zarówno przez techniki automatycznego indeksowania usługi DocumentDB, jak i dialekt zapytań SQL usługi DocumentDB.  Aby uzyskać szczegółowe informacje dotyczące sposobu używania gramatyki SQL, zobacz [Tworzenie zapytań bazy danych DocumentDB][query].

### Jakie typy danych są obsługiwane przez usługę DocumentDB?
Typy pierwotne danych obsługiwane w usłudze DocumentDB są takie same jak w formacie JSON. Format JSON ma prosty system typów składający się z ciągów, liczb (IEEE754 o podwójnej precyzji) i wartości logicznych — true, false i null.  Bardziej złożone typy danych, takie jak DateTime, Guid, Int64 i Geometry, mogą być reprezentowane w formacie JSON i DocumentDB poprzez utworzenie zagnieżdżonych obiektów przy użyciu operatora {} i tablic przy użyciu operatora []. 

### W jaki sposób usługa DocumentDB zapewnia współbieżność?
Usługa DocumentDB obsługuje optymistyczne sterowanie współbieżnością za pomocą tagów jednostki HTTP lub elementów ETag. Każdy zasób usługi DocumentDB ma element ETag, a klienci usługi DocumentDB dołączają ich najnowszą wersję odczytu w żądaniach zapisu. Jeśli element ETag jest aktualny, zmiana zostaje zatwierdzona. Jeśli wartość została zmieniona zewnętrznie, serwer odrzuca zapis z kodem odpowiedzi „HTTP 412 Niepowodzenie warunku wstępnego”. Klienci muszą odczytać najnowszą wersję zasobu i ponowić żądanie. 

### Jak wykonywać transakcje w usłudze DocumentDB?
Usługa DocumentDB obsługuje transakcje zintegrowane z językiem za pomocą procedur składowanych JavaScript i wyzwalaczy. Wszystkie operacje bazy danych wewnątrz skryptów są wykonywane w ramach izolacji migawki o zakresie dostosowanym do kolekcji, jeśli jest to kolekcja jednej partycji, lub dokumentów o tej samej wartości klucza partycji w kolekcji, jeśli kolekcja jest podzielona na partycje. Migawka wersji dokumentów (elementy ETag) jest wykonywana na początku transakcji i zatwierdzana tylko wtedy, gdy działanie skryptu zakończy się pomyślnie. Jeśli kod JavaScript zwraca błąd, transakcja zostaje wycofana. Aby uzyskać więcej szczegółów, zobacz [DocumentDB server-side programming](documentdb-programming.md) (Programowanie po stronie serwera w usłudze DocumentDB).

### Jak zbiorczo wstawić dokumenty do bazy danych DocumentDB? 
Istnieją trzy sposoby zbiorczego wstawiania dokumentów do bazy danych DocumentDB:

- Narzędzie migracji danych, zgodnie z opisem w artykule [Importowanie danych do usługi DocumentDB](documentdb-import-data.md).
- Eksplorator dokumentów w portalu Azure, zgodnie z opisem w artykule [Zbiorcze dodawanie dokumentów za pomocą Eksploratora dokumentów](documentdb-view-json-document-explorer.md#BulkAdd).
- Procedury składowane, zgodnie z opisem w artykule [DocumentDB server-side programming](documentdb-programming.md) (Programowanie po stronie serwera usługi DocumentDB).

### Czy usługa DocumentDB obsługuje buforowanie linków zasobów?
Tak. Ponieważ usługa DocumentDB jest usługą RESTful, linki zasobów są niezmienne i mogą być buforowane. Klienci usługi DocumentDB mogą określić nagłówek „If-None-Match” dla odczytów względem dowolnego zasobu takiego jak dokument lub kolekcja i aktualizować ich lokalne kopie tylko wtedy, gdy wersja serwerowa się zmieniła. 




[azure-portal]: https://portal.azure.com
[query]: documentdb-sql-query.md
 


<!--HONumber=Jun16_HO2-->


