---
title: "Tworzenie bazy danych grafów Azure Cosmos DB przy użyciu języka Java | Microsoft Docs"
description: "Przykładowy kod Java, którego można używać do nawiązywania połączeń z danymi grafu i wykonywania zapytań względem nich w usłudze Azure Cosmos DB za pomocą języka Gremlin."
services: cosmos-db
documentationcenter: 
author: dennyglee
manager: jhubbard
editor: 
ms.assetid: daacbabf-1bb5-497f-92db-079910703046
ms.service: cosmos-db
ms.custom: quick start connect, mvc
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 08/07/2017
ms.author: denlee
ms.translationtype: HT
ms.sourcegitcommit: caaf10d385c8df8f09a076d0a392ca0d5df64ed2
ms.openlocfilehash: afa4fe6cdef298e4504ddcf3e344ee6a5c181653
ms.contentlocale: pl-pl
ms.lasthandoff: 08/08/2017

---
# <a name="azure-cosmos-db-create-a-graph-database-using-java-and-the-azure-portal"></a>Azure Cosmos DB: Tworzenie bazy danych grafów przy użyciu języka Java i witryny Azure Portal

Azure Cosmos DB to rozproszona globalnie wielomodelowa usługa bazy danych firmy Microsoft. Dzięki wykorzystaniu dystrybucji globalnej i możliwości skalowania poziomego opartego na usłudze Azure Cosmos DB, można szybko tworzyć i za pomocą zapytań badać bazy danych dokumentów, par klucz/wartość i grafów. 

Ten przewodnik Szybki start tworzy bazę danych grafów przy użyciu narzędzi witryny Azure Portal dla usługi Azure Cosmos DB. Ponadto ten przewodnik Szybki start pokazuje, jak szybko utworzyć aplikację konsolową Java przy użyciu bazy danych grafów, korzystając ze sterownika OSS [Gremlin Java](https://mvnrepository.com/artifact/org.apache.tinkerpop/gremlin-driver). Instrukcje podane w tym przewodniku Szybki start można wykonać w dowolnym systemie operacyjnym, w którym można uruchomić oprogramowanie Java. Realizując ten przewodnik Szybki start, zaznajomisz się z tworzeniem i modyfikowaniem zasobów grafów z wykorzystaniem interfejsu użytkownika lub programowo, zgodnie z preferencjami. 

## <a name="prerequisites"></a>Wymagania wstępne

* [Zestaw Java Development Kit (JDK) 1.7+](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
    * W systemie Ubuntu uruchom polecenie `apt-get install default-jdk`, aby zainstalować zestaw JDK.
    * Upewnij się, że zmienna środowiskowa JAVA_HOME wskazuje folder, w którym zainstalowano zestaw JDK.
* [Pobierz](http://maven.apache.org/download.cgi) i [zainstaluj](http://maven.apache.org/install.html) archiwum binarne [Maven](http://maven.apache.org/)
    * W systemie Ubuntu możesz uruchomić polecenie `apt-get install maven`, aby zainstalować narzędzie Maven.
* [Git](https://www.git-scm.com/)
    * W systemie Ubuntu możesz uruchomić polecenie `sudo apt-get install git`, aby zainstalować usługę Git.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-database-account"></a>Tworzenie konta bazy danych

Przed utworzeniem bazy danych grafów musisz utworzyć konto bazy danych Gremlin (Graph) z użyciem usługi Azure Cosmos DB.

[!INCLUDE [cosmos-db-create-dbaccount-graph](../../includes/cosmos-db-create-dbaccount-graph.md)]

## <a name="add-a-graph"></a>Dodawanie grafu

Teraz możesz użyć narzędzia Eksplorator danych w witrynie Azure Portal, aby utworzyć bazę danych grafów. 

1. W witrynie Azure Portal w menu nawigacyjnym po lewej kliknij pozycję **Eksplorator danych (wersja zapoznawcza)**. 
2. W bloku **Eksplorator danych (wersja zapoznawcza)** kliknij pozycję **Nowy graf**, a następnie wypełnij stronę, korzystając z poniższych informacji.

    ![Eksplorator danych w witrynie Azure Portal](./media/create-graph-java/azure-cosmosdb-data-explorer.png)

    Ustawienie|Sugerowana wartość|Opis
    ---|---|---
    Identyfikator bazy danych|sample-database|Identyfikator nowej bazy danych. Nazwy baz danych muszą zawierać od 1 do 255 znaków i nie mogą zawierać znaków `/ \ # ?` ani mieć spacji na końcu.
    Identyfikator grafu|sample-graph|Identyfikator nowego grafu. W przypadku nazw grafów obowiązują takie same wymagania dotyczące znaków, jak dla identyfikatorów baz danych.
    Pojemność magazynu| 10 GB|Pozostaw wartość domyślną. To jest pojemność magazynu bazy danych.
    Przepływność|400 jednostek żądania|Pozostaw wartość domyślną. Później możesz skalować przepływność, aby zmniejszyć opóźnienie.
    RU/m|Wyłączone|Pozostaw wartość domyślną. Jeśli później będzie konieczna obsługa skokowych wzrostów obciążenia, będzie można włączyć funkcję [RU/m](request-units-per-minute.md).
    Klucz partycji|Pozostaw puste|Na potrzeby tego przewodnika Szybki start pozostaw klucz partycji pusty.

3. Po wypełnieniu formularza kliknij przycisk **OK**.

## <a name="clone-the-sample-application"></a>Klonowanie przykładowej aplikacji

Teraz sklonujemy aplikację wykresu z repozytorium GitHub, ustawimy parametry połączenia i uruchomimy ją. Zobaczysz, jak łatwo jest pracować programowo z danymi. 

1. Otwórz okno terminalu usługi Git, na przykład git bash, i za pomocą polecenia `cd` przejdź do katalogu roboczego.  

2. Uruchom następujące polecenie w celu sklonowania przykładowego repozytorium. 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-graph-java-getting-started.git
    ```

## <a name="review-the-code"></a>Przeglądanie kodu

Dokonajmy szybkiego przeglądu działań wykonywanych w aplikacji. Otwórz plik `Program.java` z folderu \src\GetStarted i znajdź następujące wiersze kodu. 

* Element `Client` języka Gremlin jest inicjowany z poziomu konfiguracji w pliku `src/remote.yaml`.

    ```java
    cluster = Cluster.build(new File("src/remote.yaml")).create();
    ...
    client = cluster.connect();
    ```

* Wykonywanie serii kroków języka Gremlin przy użyciu metody `client.submit`.

    ```java
    ResultSet results = client.submit(gremlin);

    CompletableFuture<List<Result>> completableFutureResults = results.all();
    List<Result> resultList = completableFutureResults.get();

    for (Result result : resultList) {
        System.out.println(result.toString());
    }
    ```

## <a name="update-your-connection-string"></a>Aktualizowanie parametrów połączenia

1. Otwórz plik src/remote.yaml. 

3. Uzupełnij wartości *hostów*, *nazwy użytkownika* oraz *hasła* w pliku src/remote.yaml. Nie trzeba zmieniać pozostałych ustawień.

    Ustawienie|Sugerowana wartość|Opis
    ---|---|---
    Hosts|[***.graphs.azure.com]|Zobacz zrzut ekranu pod tą tabelą. Jest to wartość identyfikatora URI Gremlin na stronie Przegląd w witrynie Azure Portal w nawiasie kwadratowym z usuniętym fragmentem końcowym :443/.<br><br>Tę wartość można również pobrać z karty Klucze, używając wartości identyfikatora URI po usunięciu ciągu https://, zmianie dokumentów na grafy i usunięciu fragmentu końcowego :443/.
    Nazwa użytkownika|/dbs/sample-database/colls/sample-graph|Zasób w postaci `/dbs/<db>/colls/<coll>`, gdzie `<db>` jest nazwą istniejącej bazy danych, a `<coll>` oznacza nazwę istniejącej kolekcji.
    Hasło|*Twój podstawowy klucz główny*|Zobacz drugi zrzut ekranu pod tą tabelą. Ta wartość to klucz podstawowy, który można pobrać z pola Klucz podstawowy na stronie Klucze w witrynie Azure Portal. Skopiuj wartość przy użyciu przycisku kopiowania po prawej stronie pola.

    W przypadku wartości Hosty skopiuj wartość **Identyfikator URI Gremlin** ze strony **Przegląd**. Jeżeli wartość jest pusta, zobacz instrukcje w wierszu Hosty w powyższej tabeli dotyczącej tworzenia identyfikatora URI Gremlin z poziomu bloku Klucze.
![Wyświetlanie i kopiowanie wartości Identyfikator URI Gremlin na stronie Przegląd w witrynie Azure Portal](./media/create-graph-java/gremlin-uri.png)

    Jako wartość hasła skopiuj **Klucz podstawowy** z bloku **Klucze**: ![Wyświetlanie i kopiowanie klucza podstawowego w witrynie Azure Portal, strona Klucze](./media/create-graph-java/keys.png)

## <a name="run-the-console-app"></a>Uruchamianie aplikacji konsolowej

1. W oknie terminalu usługi Git za pomocą polecenia `cd` przejdź do folderu azure-cosmos-db-graph-java-getting-started.

2. W oknie terminalu usługi Git wpisz polecenie `mvn package`, aby zainstalować wymagane pakiety języka Java.

3. W oknie terminalu usługi Git uruchom polecenie `mvn exec:java -D exec.mainClass=GetStarted.Program`, aby uruchomić aplikację Java.

W oknie terminalu zostaną wyświetlone wierzchołki dodawane do grafu. Po zakończeniu działania programu przejdź z powrotem do witryny Azure Portal w przeglądarce internetowej. 

<a id="add-sample-data"></a>
## <a name="review-and-add-sample-data"></a>Przeglądanie i dodawanie przykładowych danych

Teraz możesz wrócić do Eksploratora danych i zobaczyć wierzchołki dodane do grafu, a także dodać kolejne punkty danych.

1. W Eksploratorze danych rozwiń element **sample-database**/**sample-graph**, kliknij pozycję **Graph**, a następnie kliknij przycisk **Zastosuj filtr**. 

   ![Tworzenie nowych dokumentów w Eksploratorze danych w witrynie Azure Portal](./media/create-graph-java/azure-cosmosdb-data-explorer-expanded.png)

2. Na liście **Wyniki** zwróć uwagę na nowych użytkowników dodanych do grafu. Wybierz użytkownika **ben** i zwróć uwagę, że jest połączony z użytkownikiem robin. Wierzchołki w eksploratorze programu Graph możesz przenosić, a także powiększać i pomniejszać oraz rozszerzać rozmiar powierzchni eksploratora programu Graph. 

   ![Nowe wierzchołki grafu w Eksploratorze danych w witrynie Azure Portal](./media/create-graph-java/azure-cosmosdb-graph-explorer-new.png)

3. Dodajmy kilku nowych użytkowników do grafu, korzystając z Eksploratora danych. Kliknij przycisk **Nowy wierzchołek**, aby dodać dane do grafu.

   ![Tworzenie nowych dokumentów w Eksploratorze danych w witrynie Azure Portal](./media/create-graph-java/azure-cosmosdb-data-explorer-new-vertex.png)

4. Wprowadź etykietę *osoba*, a następnie wprowadź następujące klucze i wartości, aby utworzyć pierwszy wierzchołek grafu. Zauważ, że możesz utworzyć unikatowe właściwości dla każdej osoby w grafie. Tylko klucz id jest wymagany.

    key|wartość|Uwagi
    ----|----|----
    id|ashley|Unikatowy identyfikator wierzchołka. Jeśli nie określono identyfikatora, zostanie on wygenerowany.
    płeć|kobieta| 
    techniczne | java | 

    > [!NOTE]
    > W tym przewodniku Szybki start tworzymy kolekcję niepartycjonowaną. Niemniej jednak, jeśli utworzysz kolekcję partycjonowaną poprzez określenie klucza partycji podczas tworzenia kolekcji, musisz uwzględnić klucz partycji jako klucz w każdym nowym wierzchołku. 

5. Kliknij przycisk **OK**. Może być konieczne rozszerzenie ekranu w celu wyświetlenia przycisku **OK** u dołu ekranu.

6. Kliknij przycisk **Nowy wierzchołek** ponownie i dodaj kolejnego nowego użytkownika. Wprowadź etykietę *osoba* i wprowadź następujące klucze oraz wartości:

    key|wartość|Uwagi
    ----|----|----
    id|rakesh|Unikatowy identyfikator wierzchołka. Jeśli nie określono identyfikatora, zostanie on wygenerowany.
    płeć|mężczyzna| 
    szkoła|MIT| 

7. Kliknij przycisk **OK**. 

8. Kliknij przycisk **Zastosuj filtr** z domyślnym filtrem `g.V()`. Wszyscy użytkownicy będą teraz wyświetlani na liście **Wyniki**. W miarę dodawania większej ilości danych można używać filtrów do ograniczania wyników. Domyślnie Eksplorator danych używa filtru `g.V()`, aby pobierać wszystkie wierzchołki w grafie, ale możesz go zmienić na inne [zapytanie grafu](tutorial-query-graph.md), np. `g.V().count()`, aby zwrócić liczbę wszystkich wierzchołków w grafie w formacie JSON.

9. Teraz możemy połączyć użytkowników rakesh i ashley. Upewnij się, że użytkownik **ashley** został wybrany na liście **Wyniki**, a następnie kliknij przycisk edycji obok pozycji **Cele** u dołu po prawej. Może być konieczne rozszerzenie okna w celu wyświetlenia obszaru **Właściwości**.

   ![Zmiana celu wierzchołka w grafie](./media/create-graph-java/azure-cosmosdb-data-explorer-edit-target.png)

10. W polu **Cel** wpisz *rakesh*, a w polu **Etykieta krawędzi** wpisz *zna*, a następnie kliknij pole wyboru.

   ![Dodawanie połączenia między użytkownikami ashley i rakesh w Eksploratorze danych](./media/create-graph-java/azure-cosmosdb-data-explorer-set-target.png)

11. Teraz wybierz użytkownika **rakesh** z listy wyników, aby zobaczyć, że użytkownicy ashley i rakesh są połączeni. 

   ![Dwa wierzchołki połączone w Eksploratorze danych](./media/create-graph-java/azure-cosmosdb-graph-explorer.png)

    Eksplorator danych umożliwia również tworzenie procedur składowanych, funkcji definiowanych przez użytkownika (UDF) i wyzwalaczy w celu wykonania logiki biznesowej po stronie serwera oraz skalowania przepływności. Eksplorator danych udostępnia wszystkie wbudowane programowe procedury dostępu do danych w interfejsach API, ale umożliwia łatwy dostęp do danych za pośrednictwem witryny Azure Portal.



## <a name="review-slas-in-the-azure-portal"></a>Przeglądanie umów SLA w witrynie Azure Portal

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Jeśli nie zamierzasz w przyszłości korzystać z tej aplikacji, wykonaj następujące czynności, aby usunąć wszystkie zasoby utworzone w witrynie Azure Portal w ramach tego przewodnika Szybki start: 

1. W menu znajdującym się po lewej stronie w witrynie Azure Portal kliknij pozycję **Grupy zasobów**, a następnie kliknij nazwę utworzonego zasobu. 
2. Na stronie grupy zasobów kliknij pozycję **Usuń**, wpisz w polu tekstowym nazwę zasobu do usunięcia, a następnie kliknij pozycję **Usuń**.

## <a name="next-steps"></a>Następne kroki

W tym przewodniku Szybki start wyjaśniono sposób tworzenia konta usługi Azure Cosmos DB, tworzenia grafu za pomocą Eksploratora danych i uruchamiania aplikacji. Teraz możesz tworzyć bardziej złożone zapytania i implementować zaawansowaną logikę przechodzenia grafu za pomocą języka Gremlin. 

> [!div class="nextstepaction"]
> [Wykonywanie zapytań przy użyciu języka Gremlin](tutorial-query-graph.md)


