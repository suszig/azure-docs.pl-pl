---
title: "Wprowadzenie do usługi DocumentDB, bazy danych JSON | Microsoft Docs"
description: "Informacje na temat usługi Azure DocumentDB, bazy danych NoSQL opartej na formacie JSON. Ta baza danych dokumentów zapewnia obsługę dużych ilości danych, elastyczną skalowalność i wysoką dostępność."
keywords: "baza danych json, baza danych dokumentów"
services: documentdb
author: mimig1
manager: jhubbard
editor: monicar
documentationcenter: 
ms.assetid: 686cdd2b-704a-4488-921e-8eefb70d5c63
ms.service: documentdb
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 11/16/2016
ms.author: mimig
translationtype: Human Translation
ms.sourcegitcommit: 7f5e33b7f80e3c1e1e3e66b3cab879a5bc30e823
ms.openlocfilehash: 18f2c1876fab914073c8aea67f67724f281a65aa


---
# <a name="introduction-to-documentdb-a-nosql-json-database"></a>Wprowadzenie do usługi DocumentDB — bazy danych NoSQL opartej na formacie JSON
## <a name="what-is-documentdb"></a>Co to jest baza danych DocumentDB?
DocumentDB to w pełni zarządzana baza danych NoSQL oferująca szybkie przetwarzanie, przewidywalną wydajność, wysoką dostępność, elastyczne skalowanie, globalną dystrybucję i łatwość programowania. Jako baza danych NoSQL bez schematu baza danych DocumentDB oferuje zaawansowane, znane możliwości tworzenia zapytań SQL z spójnymi niskimi opóźnieniami związanymi z danymi JSON. Zapewniają one, że 99% operacji odczytu jest obsługiwanych w czasie krótszym niż 10 milisekund, a 99% operacji zapisu — w czasie krótszym niż 15 milisekund. Dzięki tym unikatowym korzyściom usługa DocumentDB stanowi doskonałe rozwiązanie dla aplikacji sieci Web, mobilnych, gier i IoT oraz wielu innych aplikacji wymagających bezproblemowego skalowania i globalnej replikacji.

## <a name="how-can-i-learn-about-documentdb"></a>Jak mogę dowiedzieć się więcej na temat usługi DocumentDB?
Aby szybko poznać usługę DocumentDB i zobaczyć ją w akcji, wykonaj trzy następujące kroki: 

1. Obejrzyj dwuminutowe wideo [Co to jest usługa DocumentDB?](https://azure.microsoft.com/documentation/videos/what-is-azure-documentdb/), które prezentuje zalety korzystania z usługi DocumentDB.
2. Obejrzyj trzyminutowe wideo [Tworzenie bazy danych DocumentDB w portalu Azure](https://azure.microsoft.com/documentation/videos/create-documentdb-on-azure/), które pokazuje, jak rozpocząć pracę z usługą DocumentDB w portalu Azure.
3. Odwiedź witrynę [plac zabaw dla zapytań](http://www.documentdb.com/sql/demo), w której możesz zapoznać się z wykonywaniem różnych działań, aby dowiedzieć się więcej o zaawansowanych funkcjach zapytań dostępnych w usłudze DocumentDB. Następnie przejdź na kartę Piaskownica i uruchamiaj własne niestandardowe zapytania SQL oraz eksperymentuj z usługą DocumentDB.

Wróć do tego artykułu, aby zapoznać się z dalszymi szczegółami.  

## <a name="what-capabilities-and-key-features-does-documentdb-offer"></a>Jakie możliwości i kluczowe funkcje oferuje usługa DocumentDB?
Usługa Azure DocumentDB oferuje następujące kluczowe funkcje i korzyści:

* **Elastycznie skalowalne przepływność i magazyn**: łatwe skalowanie bazy danych JSON usługi DocumentDB w celu dostosowania do potrzeb aplikacji. Dane są przechowywane na dyskach półprzewodnikowych (SSD, solid-state drive) dla zapewnienia przewidywalnych, niskich opóźnień. Usługa DocumentDB obsługuje kontenery do przechowywania danych JSON zwane kolekcjami, które można skalować do niemal nieograniczonego rozmiaru magazynu i aprowizowanej przepływności. W miarę wzrostu aplikacji usługę DocumentDB można bezproblemowo elastycznie skalować z przewidywalną wydajnością. 
* **Replikacja w wielu regionach:** DocumentDB w sposób przezroczysty replikuje dane do wszystkich regionów skojarzonych z kontem usługi DocumentDB, umożliwiając tworzenie aplikacji wymagających globalnego dostępu do danych, a jednocześnie zapewniając kompromis między spójnością, dostępnością i wydajnością z odpowiednimi gwarancjami. Usługa DocumentDB zawiera przezroczyste rozwiązania regionalnej pracy awaryjnej z międzyregionalnymi interfejsami API oraz możliwość elastycznego skalowania przepustowości i magazynów na całym świecie. Dowiedz się więcej w artykule [Distribute data globally with DocumentDB](documentdb-distribute-data-globally.md) (Globalna dystrybucja danych przy użyciu usługi DocumentDB).
* **Zapytania ad hoc z użyciem dobrze znanej składni SQL**: przechowywanie heterogenicznych dokumentów JSON w usłudze DocumentDB i wykonywanie względem nich zapytań przy użyciu dobrze znanej składni SQL. Usługa DocumentDB wykorzystuje wysoce współbieżną, nieblokującą, opartą na strukturze dziennika technologię indeksowania do automatycznego indeksowania całej zawartości dokumentu. Umożliwia to wykonywanie zaawansowanych zapytań w czasie rzeczywistym bez konieczności określania wskazówek schematu, indeksów pomocniczych czy widoków. Więcej informacji zawiera artykuł [Zapytania w usłudze DocumentDB](documentdb-sql-query.md). 
* **Wykonywanie skryptów JavaScript w bazie danych**: wyrażanie logiki aplikacji jako procedur składowanych, wyzwalaczy i funkcji definiowanych przez użytkownika (funkcji UDF) przy użyciu standardowego języka JavaScript. Dzięki temu logika aplikacji może pracować nad danymi bez problemów z niezgodnością między aplikacją a schematem bazy danych. Usługa DocumentDB zapewnia pełne transakcyjne wykonywanie logiki aplikacji JavaScript bezpośrednio wewnątrz aparatu bazy danych. Głęboka integracja języka JavaScript umożliwia wykonywanie operacji INSERT, REPLACE, DELETE i SELECT z poziomu programu JavaScript jako izolowanych transakcji. Aby dowiedzieć się więcej, zobacz [Programowanie po stronie serwera w usłudze DocumentDB](documentdb-programming.md).
* **Możliwość dostosowania poziomu spójności**: możliwość wyboru spośród czterech dobrze zdefiniowanych poziomów spójności w celu osiągnięcia optymalnego kompromisu między wydajnością a spójnością. Dla zapytań i operacji odczytu usługa DocumentDB oferuje cztery różne poziomy spójności: „silna”, „powiązana nieaktualność”, „sesja” i „ostateczna”. Te szczegółowe, dokładnie zdefiniowane poziomy spójności umożliwiają ustalanie optymalnych kompromisów między spójnością, dostępnością i opóźnieniem. Aby dowiedzieć się więcej, zobacz [Maksymalizowanie dostępności i wydajności za pomocą poziomów spójności w usłudze DocumentDB](documentdb-consistency-levels.md).
* **W pełni zarządzana**: eliminuje konieczność zarządzania zasobami maszyn i bazy danych. Ponieważ jest to w pełni zarządzana usługa platformy Microsoft Azure, nie musisz zarządzać maszynami wirtualnymi, wdrażać i konfigurować oprogramowania, zarządzać skalowaniem ani obsługiwać złożonych aktualizacji warstwy danych. Dla każdej bazy danych jest automatycznie tworzona kopia zapasowa i jest ona chroniona przed regionalnymi awariami. Możesz łatwo dodać konto usługi DocumentDB i aprowizować pojemność odpowiednio do potrzeb, co pozwala skupić się na aplikacji zamiast zajmować się obsługą bazy danych i zarządzaniem nią. 
* **Celowa ogólnodostępność**: można szybko rozpocząć pracę, wykorzystując posiadane umiejętności i narzędzia. Programowanie bazy danych DocumentDB jest łatwe, przystępne i nie wymaga wdrażania nowych narzędzi ani niestandardowych rozszerzeń formatu JSON czy języka JavaScript. Możesz uzyskać dostęp do wszystkich funkcji bazy danych, w tym operacji CRUD, zapytań i przetwarzania języka JavaScript przy użyciu prostego interfejsu RESTful protokołu HTTP. Usługa DocumentDB wykorzystuje istniejące formaty, języki i standardy, jednocześnie oferując oparte na nich i zapewniające wysoką wartość możliwości bazy danych.
* **Automatyczne indeksowanie:** domyślnie usługa DocumentDB automatycznie indeksuje wszystkie dokumenty w bazie danych i nie oczekuje ani nie wymaga żadnego schematu lub tworzenia indeksów pomocniczych. Nie chcesz indeksować wszystkiego? Nie martw się, możesz również [zrezygnować ze ścieżki plików JSON](documentdb-indexing-policies.md).

## <a name="a-namedata-managementahow-does-documentdb-manage-data"></a><a name="data-management"></a>Jak usługa DocumentDB zarządza danymi?
Usługa Azure DocumentDB zarządza danymi JSON za pomocą dobrze zdefiniowanych zasobów bazy danych. Te zasoby są replikowane w celu zapewnienia wysokiej dostępności i unikatowo adresowane przez ich logiczny identyfikator URI. Usługa DocumentDB oferuje prosty model programowania RESTful oparty na protokole HTTP dla wszystkich zasobów. 

Konto bazy danych usługi DocumentDB jest unikatową przestrzenią nazw, która umożliwia dostęp do usługi Azure DocumentDB. Przed utworzeniem konta bazy danych musisz mieć subskrypcję platformy Azure, która umożliwia dostęp do różnych usług platformy Azure. 

Wszystkie zasoby w usłudze DocumentDB są modelowane i przechowywane jako dokumenty JSON. Zasoby są zarządzane jako elementy, które są dokumentami JSON zawierającymi metadane, oraz jako źródła danych, które są kolekcjami elementów. Zestawy elementów są zawarte w odpowiednich źródłach danych.

Na poniższym obrazie przedstawiono relacje między zasobami usługi DocumentDB:

![Hierarchiczna relacja między zasobami w usłudze DocumentDB, bazie danych NoSQL opartej na formacie JSON][1] 

Konto bazy danych składa się z zestawu baz danych, każda z nich zawiera wiele kolekcji, a każda kolekcja może zawierać procedury składowane, wyzwalacze, funkcje UDF, dokumenty i powiązane załączniki. Baza danych ma również skojarzonych użytkowników, a każdy użytkownik ma zestaw uprawnień dostępu do różnych innych kolekcji, procedur składowanych, wyzwalaczy, funkcji UDF, dokumentów lub załączników. Bazy danych, użytkownicy, uprawnienia i kolekcje są zasobami zdefiniowanymi przez system za pomocą dobrze znanych schematów, natomiast dokumenty, procedury składowane, wyzwalacze, funkcje UDF i załączniki zawierają dowolną, zdefiniowaną przez użytkownika zawartość JSON.  

## <a name="a-namedevelopa-how-can-i-develop-apps-with-documentdb"></a><a name="develop"></a> Jak można tworzyć aplikacje przy użyciu usługi DocumentDB?
Usługa Azure DocumentDB udostępnia zasoby za pośrednictwem interfejsu API REST, który można wywoływać za pomocą dowolnego języka realizującego żądania HTTP i HTTPS. Ponadto usługa DocumentDB oferuje biblioteki programistyczne dla kilku popularnych języków. Te biblioteki upraszczają wiele aspektów pracy z usługą Azure DocumentDB dzięki obsłudze szczegółów takich jak buforowanie adresów, zarządzanie wyjątkami, automatyczne ponawianie prób i tak dalej. Biblioteki są obecnie dostępne dla następujących języków i platform:  

| Do pobrania | Dokumentacja |
| --- | --- |
| [Zestaw SDK platformy .NET](http://go.microsoft.com/fwlink/?LinkID=402989) |[Biblioteka języka .NET](https://msdn.microsoft.com/library/azure/dn948556.aspx) |
| [Zestaw SDK dla platformy Node.js](http://go.microsoft.com/fwlink/?LinkID=402990) |[Biblioteka języka Node.js](http://azure.github.io/azure-documentdb-node/) |
| [Zestaw SDK Java](http://go.microsoft.com/fwlink/?LinkID=402380) |[Biblioteka języka Java](http://azure.github.io/azure-documentdb-java/) |
| [Zestaw SDK dla języka JavaScript](http://go.microsoft.com/fwlink/?LinkID=402991) |[Biblioteka języka JavaScript](http://azure.github.io/azure-documentdb-js/) |
| Nie dotyczy |[Zestaw SDK dla języka JavaScript po stronie serwera](http://azure.github.io/azure-documentdb-js-server/) |
| [Zestaw SDK dla języka Python](https://pypi.python.org/pypi/pydocumentdb) |[Biblioteka języka Python](http://azure.github.io/azure-documentdb-python/) |

Za pomocą [emulatora usługi Azure DocumentDB](documentdb-nosql-local-emulator.md) możesz lokalnie opracowywać i testować swoją aplikację bez tworzenia subskrypcji platformy Azure i generowania kosztów. Jeśli sposób działania aplikacji w emulatorze usługi DocumentDB jest zadowalający, możesz przełączyć się do korzystania z konta usługi Azure DocumentDB w chmurze.

Poza podstawowymi operacjami tworzenia, odczytu, aktualizacji i usuwania usługa DocumentDB udostępnia interfejs zaawansowanych zapytań SQL na potrzeby pobierania dokumentów JSON oraz obsługę po stronie serwera na potrzeby transakcyjnego wykonywania logiki aplikacji JavaScript. Interfejsy wykonywania zapytań i skryptów są dostępne za pośrednictwem bibliotek wszystkich platform, a także interfejsów API REST. 

### <a name="sql-query"></a>Zapytanie SQL
Usługa Azure DocumentDB obsługuje tworzenie zapytań dla dokumentów za pomocą języka SQL, osadzonego w systemie typów języka JavaScript, oraz wyrażenia z obsługą zapytań relacyjnych, hierarchicznych i przestrzennych. Język zapytań usługi DocumentDB jest prostym, ale zaawansowanym interfejsem do tworzenia zapytań dotyczących dokumentów JSON. Język obsługuje podzbiór gramatyki ANSI SQL i dodaje głęboką integrację obiektów, tablic, konstrukcji obiektów i wywoływania funkcji języka JavaScript. Usługa DocumentDB udostępnia swój model zapytań bez żadnego jawnego schematu lub wskazówek indeksowania od dewelopera.

W usłudze DocumentDB można rejestrować funkcje zdefiniowane przez użytkownika i odwoływać się do nich jako elementu zapytania SQL, rozszerzając w ten sposób gramatykę na potrzeby obsługi niestandardowej logiki aplikacji. Te funkcje UDF są zapisywane jako programy JavaScript i wykonywane w bazie danych. 

Deweloperom platformy .NET usługa DocumentDB oferuje również dostawcę zapytań LINQ jako część [zestawu SDK dla platformy .NET](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.linq.aspx). 

### <a name="transactions-and-javascript-execution"></a>Transakcje i wykonywanie kodu JavaScript
Usługa DocumentDB umożliwia pisanie logiki aplikacji jako nazwanych programów napisanych w całości w języku JavaScript. Te programy są rejestrowane dla kolekcji i mogą wyzwalać operacje bazy danych na dokumentach w ramach danej kolekcji. Kod JavaScript można zarejestrować do wykonania jako wyzwalacz, procedurę składowaną lub funkcję zdefiniowaną przez użytkownika. Wyzwalacze i procedury składowane mogą tworzyć, odczytywać, aktualizować i usuwać dokumenty, natomiast funkcje zdefiniowane przez użytkownika są wykonywane jako część logiki wykonywania zapytania, bez dostępu do zapisu w kolekcji.

Wykonywanie kodu JavaScript w usłudze DocumentDB jest modelowane zgodnie z koncepcjami obsługiwanymi przez systemy relacyjnych baz danych, z użyciem języka JavaScript jako nowoczesnego zamiennika języka Transact-SQL. Cała logika JavaScript jest wykonywana w ramach transakcji ACID otoczenia z izolacją migawki. Jeśli w trakcie wykonywania logiki JavaScript zgłosi ona wyjątek, cała transakcja zostaje przerwana.

## <a name="next-steps"></a>Następne kroki
Masz już konto platformy Azure? Możesz rozpocząć pracę z usługą DocumentDB w witrynie [Azure Portal](https://portal.azure.com/#gallery/Microsoft.DocumentDB) przez [utworzenie konta bazy danych DocumentDB](documentdb-create-account.md).

Nie masz konta platformy Azure? Możesz:

* Możesz utworzyć konto [bezpłatnej wersji próbnej platformy Azure](https://azure.microsoft.com/free/), które zapewnia 30 dni i 200 USD na wypróbowanie usług Azure. 
* Jeśli masz subskrypcję MSDN, przysługuje Ci [150 USD miesięcznie w ramach bezpłatnych środków platformy Azure](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) na korzystanie z dowolnej usługi Azure. 
* Pobierz [emulator usługi Azure DocumentDB](documentdb-nosql-local-emulator.md), aby lokalnie opracowywać aplikację.

Następnie, kiedy zechcesz dowiedzieć się więcej, odwiedź naszą [ścieżkę szkoleniową](https://azure.microsoft.com/documentation/learning-paths/documentdb/), aby przejrzeć listę wszystkich dostępnych zasobów szkoleniowych. 

[1]: ./media/documentdb-introduction/json-database-resources1.png




<!--HONumber=Jan17_HO2-->


