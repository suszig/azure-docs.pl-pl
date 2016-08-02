<properties
    pageTitle="Samouczek NoSQL: zestaw SDK dla platformy .NET usługi DocumentDB | Microsoft Azure"
    description="Samouczek NoSQL, który pokazuje tworzenie bazy danych w trybie online i aplikacji konsolowej C# przy użyciu zestawu SDK dla platformy .NET usługi DocumentDB. Usługa DocumentDB jest bazą danych NoSQL dla formatu JSON."
    keywords="nosql tutorial, online database, c# console application"
    services="documentdb"
    documentationCenter=".net"
    authors="AndrewHoh"
    manager="jhubbard"
    editor="monicar"/>

<tags
    ms.service="documentdb"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="05/16/2016"
    ms.author="anhoh"/>

# Samouczek NoSQL: tworzenie aplikacji konsolowej w języku C# dla usługi DocumentDB

> [AZURE.SELECTOR]
- [.NET](documentdb-get-started.md)
- [Node.js](documentdb-nodejs-get-started.md)

Witamy w samouczku NoSQL dla zestawu SDK dla platformy .NET usługi DocumentDB! W ramach tego samouczka zostanie utworzona aplikacja konsolowa, która tworzy zasoby usługi DocumentDB i wykonuje dla nich zapytania.

Omówione zostaną następujące czynności:

- Tworzenie konta usługi DocumentDB i nawiązywanie z nim połączenia
- Konfigurowanie rozwiązania Visual Studio
- Tworzenie bazy danych w trybie online
- Tworzenie kolekcji
- Tworzenie dokumentów JSON
- Wykonywanie zapytań względem kolekcji
- Zastępowanie dokumentu
- Usuwanie dokumentu
- Usuwanie bazy danych

Nie masz czasu? Nie martw się! Kompletne rozwiązanie jest dostępne w witrynie [GitHub](https://github.com/Azure-Samples/documentdb-dotnet-getting-started). Przeskocz do sekcji [Pobieranie kompletnego rozwiązania](#GetSolution), aby uzyskać krótkie instrukcje.

Po wszystkim użyj przycisków głosowania u góry lub u dołu tej strony, aby wysłać nam swoją opinię. Jeśli chcesz, abyśmy skontaktowali się z Tobą bezpośrednio, możesz w komentarzach podać swój adres e-mail.

Teraz do dzieła!

## Wymagania wstępne

Upewnij się, że masz:

- Aktywne konto platformy Azure. Jeśli go nie masz, możesz zarejestrować się w celu [utworzenia bezpłatnego konta](https://azure.microsoft.com/free/).
- Program [Visual Studio 2013 / Visual Studio 2015](http://www.visualstudio.com/).
- Program .NET Framework 4.6

## Krok 1. Tworzenie konta usługi DocumentDB

Utwórz konto usługi DocumentDB. Jeśli masz już konto, którego chcesz użyć, możesz przejść od razu do kroku [Konfigurowanie rozwiązania Visual Studio](#SetupVS).

[AZURE.INCLUDE [documentdb-create-dbaccount](../../includes/documentdb-create-dbaccount.md)]

##<a id="SetupVS"></a> Krok 2. Konfigurowanie rozwiązania Visual Studio

1. Otwórz program **Visual Studio 2015** na komputerze.
2. W menu **Plik** wybierz polecenie **Nowy**, a następnie wybierz pozycję **Projekt**.
3. W oknie dialogowym **Nowy projekt** wybierz pozycję **Szablony** / **Visual C#** / **Aplikacja konsolowa**, podaj nazwę projektu, a następnie kliknij przycisk **OK**.
![Zrzut ekranu przedstawiający okno Nowy projekt](./media/documentdb-get-started/nosql-tutorial-new-project-2.png)
4. W **Eksploratorze rozwiązań** kliknij prawym przyciskiem myszy nową aplikację konsolową, która znajduje się w obrębie rozwiązania Visual Studio.
5. Następnie bez opuszczania menu wybierz polecenie **Zarządzaj pakietami NuGet...**
![Zrzut ekranu przedstawiający menu wywoływane po kliknięciu prawym przyciskiem myszy dla projektu](./media/documentdb-get-started/nosql-tutorial-manage-nuget-pacakges.png)
6. Na karcie **NuGet** kliknij pozycję **Przeglądaj** i wpisz ciąg **azure documentdb** w polu wyszukiwania.
7. W wynikach znajdź pozycję **Microsoft.Azure.DocumentDB** i kliknij przycisk **Zainstaluj**.
Identyfikator pakietu dla biblioteki klienta usługi DocumentDB to [Microsoft.Azure.DocumentDB](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB)
![Zrzut ekranu przedstawiający menu NuGet do znajdowania zestawu SDK klienta usługi DocumentDB](./media/documentdb-get-started/nosql-tutorial-manage-nuget-pacakges-2.png)

Wspaniale! Teraz, po zakończeniu konfigurowania, zacznijmy pisanie kodu. Ukończony projekt kodu z tego samouczka można znaleźć w witrynie [GitHub](https://github.com/Azure-Samples/documentdb-dotnet-getting-started/blob/master/src/Program.cs).

##<a id="Connect"></a> Krok 3. Łączenie z kontem usługi DocumentDB

Najpierw dodaj te odwołania na początku aplikacji C# w pliku Program.cs:

    using System;
    using System.Linq;
    using System.Threading.Tasks;

    // ADD THIS PART TO YOUR CODE
    using System.Net;
    using Microsoft.Azure.Documents;
    using Microsoft.Azure.Documents.Client;
    using Newtonsoft.Json;

> [AZURE.IMPORTANT] W celu ukończenia tego samouczka NoSQL upewnij się, że dodano zależności powyżej.

Dodaj teraz te dwie stałe i zmienną *client* poniżej klasy publicznej *Program*.

    public class Program
    {
        // ADD THIS PART TO YOUR CODE
        private const string EndpointUri = "<your endpoint URI>";
        private const string PrimaryKey = "<your key>";
        private DocumentClient client;

Następnie przejdź do [portalu Azure](https://portal.azure.com), aby pobrać identyfikator URI i klucz podstawowy. Identyfikator URI i klucz podstawowy usługi DocumentDB są niezbędne, aby aplikacja wiedziała, z jakim elementem ma się połączyć, i aby usługa DocumentDB ufała połączeniu aplikacji.

W portalu Azure przejdź do swojego konta usługi DocumentDB (z kroku 1).

Kliknij ikonę **kluczy** na pasku **Podstawowe elementy**.
Skopiuj identyfikator URI i zastąp ciąg *<your endpoint URI>* skopiowanym identyfikatorem URI w programie.
Skopiuj klucz podstawowy i zastąp ciąg *<your key>* skopiowanym kluczem podstawowym w programie.

![Zrzut ekranu przedstawiający portal Azure używany przez samouczek NoSQL do tworzenia aplikacji konsolowej C#. Przedstawia konto usługi DocumentDB z wyróżnionym AKTYWNYM centrum, przyciskiem KLUCZE wyróżnionym w bloku konta usługi DocumentDB oraz wartościami IDENTYFIKATOR URI, KLUCZ PODSTAWOWY i KLUCZ POMOCNICZY wyróżnionymi w bloku Klucze.][keys]

Zaczniemy tworzenie aplikacji rozpoczynania pracy, tworząc nowe wystąpienie klasy **DocumentClient**.

Poniżej metody **Main** dodaj to nowe zadanie asynchroniczne o nazwie **GetStartedDemo**, które utworzy nowe wystąpienie klasy **DocumentClient**.

    static void Main(string[] args)
    {
    }

    // ADD THIS PART TO YOUR CODE
    private async Task GetStartedDemo()
    {
        this.client = new DocumentClient(new Uri(EndpointUri), PrimaryKey);
    }

Dodaj następujący kod w celu uruchomienia zadania asynchronicznego z metody **Main**. Metoda **Main** będzie przechwytywać wyjątki i zapisywać je w konsoli.

    static void Main(string[] args)
    {
            // ADD THIS PART TO YOUR CODE
            try
            {
                    Program p = new Program();
                    p.GetStartedDemo().Wait();
            }
            catch (DocumentClientException de)
            {
                    Exception baseException = de.GetBaseException();
                    Console.WriteLine("{0} error occurred: {1}, Message: {2}", de.StatusCode, de.Message, baseException.Message);
            }
            catch (Exception e)
            {
                    Exception baseException = e.GetBaseException();
                    Console.WriteLine("Error: {0}, Message: {1}", e.Message, baseException.Message);
            }
            finally
            {
                    Console.WriteLine("End of demo, press any key to exit.");
                    Console.ReadKey();
            }

Naciśnij klawisz **F5**, aby uruchomić aplikację.

Gratulacje! Pomyślnie nawiązano połączenie z kontem usługi DocumentDB. Teraz przyjrzyjmy się pracy z zasobami usługi DocumentDB.  

## Krok 4. Tworzenie bazy danych
Przed dodaniem kodu na potrzeby tworzenia bazy danych dodaj metodę pomocnika na potrzeby pisania w konsoli.

Skopiuj i wklej metodę **WriteToConsoleAndPromptToContinue** poniżej metody **GetStartedDemo**.

    // ADD THIS PART TO YOUR CODE
    private void WriteToConsoleAndPromptToContinue(string format, params object[] args)
    {
            Console.WriteLine(format, args);
            Console.WriteLine("Press any key to continue ...");
            Console.ReadKey();
    }

[Bazę danych](documentdb-resources.md#databases) usługi DocumentDB można utworzyć za pomocą metody [CreateDatabaseAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdatabaseasync.aspx) klasy **DocumentClient**. Baza danych jest kontenerem logicznym magazynu dokumentów JSON podzielonym na partycje w kolekcjach.

Skopiuj i wklej metodę **CreateDatabaseIfNotExists** poniżej metody **WriteToConsoleAndPromptToContinue**.

    // ADD THIS PART TO YOUR CODE
    private async Task CreateDatabaseIfNotExists(string databaseName)
    {
            // Check to verify a database with the id=FamilyDB does not exist
            try
            {
                    await this.client.ReadDatabaseAsync(UriFactory.CreateDatabaseUri(databaseName));
                    this.WriteToConsoleAndPromptToContinue("Found {0}", databaseName);
            }
            catch (DocumentClientException de)
            {
                    // If the database does not exist, create a new database
                    if (de.StatusCode == HttpStatusCode.NotFound)
                    {
                            await this.client.CreateDatabaseAsync(new Database { Id = databaseName });
                            this.WriteToConsoleAndPromptToContinue("Created {0}", databaseName);
                    }
                    else
                    {
                            throw;
                    }
            }
    }

Skopiuj i wklej następujący kod do metody **GetStartedDemo** poniżej kodu służącego do tworzenia klienta. Spowoduje to utworzenie bazy danych o nazwie *FamilyDB*.

    private async Task GetStartedDemo()
    {
        this.client = new DocumentClient(new Uri(EndpointUri), PrimaryKey);

        // ADD THIS PART TO YOUR CODE
        await this.CreateDatabaseIfNotExists("FamilyDB");

Naciśnij klawisz **F5**, aby uruchomić aplikację.

Gratulacje! Pomyślnie utworzono bazę danych usługi DocumentDB.  

##<a id="CreateColl"></a>Krok 5. Tworzenie kolekcji  

> [AZURE.WARNING] Metoda **CreateDocumentCollectionAsync** utworzy nową kolekcję z zarezerwowaną przepływnością, co ma wpływ na cenę. Aby uzyskać więcej informacji, odwiedź naszą [stronę cennika](https://azure.microsoft.com/pricing/details/documentdb/).

[Kolekcję](documentdb-resources.md#collections) można utworzyć za pomocą metody [CreateDocumentCollectionAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentcollectionasync.aspx) klasy **DocumentClient**. Kolekcja jest kontenerem dokumentów JSON i skojarzonej logiki aplikacji JavaScript.

Skopiuj i wklej metodę **CreateDocumentCollectionIfNotExists** poniżej metody **CreateDatabaseIfNotExists**.

    // ADD THIS PART TO YOUR CODE
    private async Task CreateDocumentCollectionIfNotExists(string databaseName, string collectionName)
    {
        try
        {
            await this.client.ReadDocumentCollectionAsync(UriFactory.CreateDocumentCollectionUri(databaseName, collectionName));
            this.WriteToConsoleAndPromptToContinue("Found {0}", collectionName);
        }
        catch (DocumentClientException de)
        {
            // If the document collection does not exist, create a new collection
            if (de.StatusCode == HttpStatusCode.NotFound)
            {
                DocumentCollection collectionInfo = new DocumentCollection();
                collectionInfo.Id = collectionName;

                // Configure collections for maximum query flexibility including string range queries.
                collectionInfo.IndexingPolicy = new IndexingPolicy(new RangeIndex(DataType.String) { Precision = -1 });

                // Here we create a collection with 400 RU/s.
                await this.client.CreateDocumentCollectionAsync(
                    UriFactory.CreateDatabaseUri(databaseName),
                    new DocumentCollection { Id = collectionName },
                    new RequestOptions { OfferThroughput = 400 });

                this.WriteToConsoleAndPromptToContinue("Created {0}", collectionName);
            }
            else
            {
                throw;
            }
        }
    }

Skopiuj i wklej następujący kod do metody **GetStartedDemo** poniżej kodu służącego do tworzenia bazy danych. Spowoduje to utworzenie kolekcji dokumentów o nazwie *FamilyCollection*.

        this.client = new DocumentClient(new Uri(EndpointUri), PrimaryKey);

        await this.CreateDatabaseIfNotExists("FamilyDB");

        // ADD THIS PART TO YOUR CODE
        await this.CreateDocumentCollectionIfNotExists("FamilyDB", "FamilyCollection");

Naciśnij klawisz **F5**, aby uruchomić aplikację.

Gratulacje! Pomyślnie utworzono kolekcję dokumentów usługi DocumentDB.  

##<a id="CreateDoc"></a>Krok 6. Tworzenie dokumentów JSON
[Dokument](documentdb-resources.md#documents) można utworzyć za pomocą metody [CreateDocumentAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentasync.aspx) klasy **DocumentClient**. Dokumenty są zawartością JSON zdefiniowaną przez użytkownika (dowolną). Można teraz wstawić jeden lub więcej dokumentów. Jeśli masz już dane, które chcesz przechowywać w bazie danych, możesz użyć [narzędzia migracji danych](documentdb-import-data.md) usługi DocumentDB.

Najpierw należy utworzyć klasę **Family**, która będzie reprezentowała obiekty przechowywane w usłudze DocumentDB w tym przykładzie. Zostaną również utworzone podklasy **Parent**, **Child**, **Pet** i **Address**, które są używane w ramach klasy **Family**. Należy pamiętać, że dokumenty muszą mieć właściwość **Id** serializowaną jako **id** w formacie JSON. Utwórz te klasy, dodając następujące podklasy wewnętrzne po metodzie **GetStartedDemo**.

Skopiuj i wklej klasy **Family**, **Parent**, **Child**, **Pet** i **Address** poniżej metody **WriteToConsoleAndPromptToContinue**.

    private void WriteToConsoleAndPromptToContinue(string format, params object[] args)
    {
        Console.WriteLine(format, args);
        Console.WriteLine("Press any key to continue ...");
        Console.ReadKey();
    }

    // ADD THIS PART TO YOUR CODE
    public class Family
    {
        [JsonProperty(PropertyName = "id")]
        public string Id { get; set; }
        public string LastName { get; set; }
        public Parent[] Parents { get; set; }
        public Child[] Children { get; set; }
        public Address Address { get; set; }
        public bool IsRegistered { get; set; }
        public override string ToString()
        {
                return JsonConvert.SerializeObject(this);
        }
    }

    public class Parent
    {
        public string FamilyName { get; set; }
        public string FirstName { get; set; }
    }

    public class Child
    {
        public string FamilyName { get; set; }
        public string FirstName { get; set; }
        public string Gender { get; set; }
        public int Grade { get; set; }
        public Pet[] Pets { get; set; }
    }

    public class Pet
    {
        public string GivenName { get; set; }
    }

    public class Address
    {
        public string State { get; set; }
        public string County { get; set; }
        public string City { get; set; }
    }

Skopiuj i wklej metodę **CreateFamilyDocumentIfNotExists** poniżej metody **CreateDocumentCollectionIfNotExists**.

    // ADD THIS PART TO YOUR CODE
    private async Task CreateFamilyDocumentIfNotExists(string databaseName, string collectionName, Family family)
    {
        try
        {
            await this.client.ReadDocumentAsync(UriFactory.CreateDocumentUri(databaseName, collectionName, family.Id));
            this.WriteToConsoleAndPromptToContinue("Found {0}", family.Id);
        }
        catch (DocumentClientException de)
        {
            if (de.StatusCode == HttpStatusCode.NotFound)
            {
                await this.client.CreateDocumentAsync(UriFactory.CreateDocumentCollectionUri(databaseName, collectionName), family);
                this.WriteToConsoleAndPromptToContinue("Created Family {0}", family.Id);
            }
            else
            {
                throw;
            }
        }
    }

Następnie wstaw dwa dokumenty, jeden dla rodziny Andersen i jeden dla rodziny Wakefield.

Skopiuj i wklej następujący kod do metody **GetStartedDemo** poniżej kodu służącego do tworzenia kolekcji dokumentów.

    await this.CreateDatabaseIfNotExists("FamilyDB");

    await this.CreateDocumentCollectionIfNotExists("FamilyDB", "FamilyCollection");

    // ADD THIS PART TO YOUR CODE
    Family andersenFamily = new Family
    {
            Id = "Andersen.1",
            LastName = "Andersen",
            Parents = new Parent[]
            {
                    new Parent { FirstName = "Thomas" },
                    new Parent { FirstName = "Mary Kay" }
            },
            Children = new Child[]
            {
                    new Child
                    {
                            FirstName = "Henriette Thaulow",
                            Gender = "female",
                            Grade = 5,
                            Pets = new Pet[]
                            {
                                    new Pet { GivenName = "Fluffy" }
                            }
                    }
            },
            Address = new Address { State = "WA", County = "King", City = "Seattle" },
            IsRegistered = true
    };

    await this.CreateFamilyDocumentIfNotExists("FamilyDB", "FamilyCollection", andersenFamily);

    Family wakefieldFamily = new Family
    {
            Id = "Wakefield.7",
            LastName = "Wakefield",
            Parents = new Parent[]
            {
                    new Parent { FamilyName = "Wakefield", FirstName = "Robin" },
                    new Parent { FamilyName = "Miller", FirstName = "Ben" }
            },
            Children = new Child[]
            {
                    new Child
                    {
                            FamilyName = "Merriam",
                            FirstName = "Jesse",
                            Gender = "female",
                            Grade = 8,
                            Pets = new Pet[]
                            {
                                    new Pet { GivenName = "Goofy" },
                                    new Pet { GivenName = "Shadow" }
                            }
                    },
                    new Child
                    {
                            FamilyName = "Miller",
                            FirstName = "Lisa",
                            Gender = "female",
                            Grade = 1
                    }
            },
            Address = new Address { State = "NY", County = "Manhattan", City = "NY" },
            IsRegistered = false
    };

    await this.CreateFamilyDocumentIfNotExists("FamilyDB", "FamilyCollection", wakefieldFamily);

Naciśnij klawisz **F5**, aby uruchomić aplikację.

Gratulacje! Pomyślnie utworzono dwa dokumenty usługi DocumentDB.  

![Diagram pokazujący hierarchiczną relację między kontem, bazą danych w trybie online, kolekcją i dokumentami używanymi przez samouczek NoSQL do tworzenia aplikacji konsolowej C#](./media/documentdb-get-started/nosql-tutorial-account-database.png)

##<a id="Query"></a>Krok 7. Wykonywanie zapytań względem zasobów usługi DocumentDB

Usługa DocumentDB obsługuje zaawansowane [zapytania](documentdb-sql-query.md) względem dokumentów JSON przechowywanych w każdej kolekcji.  Następujący przykładowy kod przedstawia różne zapytania — przy użyciu składni SQL usługi DocumentDB oraz LINQ — które można uruchomić względem dokumentów wstawionych w poprzednim kroku.

Skopiuj i wklej metodę **ExecuteSimpleQuery** poniżej metody **CreateFamilyDocumentIfNotExists**.

    // ADD THIS PART TO YOUR CODE
    private void ExecuteSimpleQuery(string databaseName, string collectionName)
    {
        // Set some common query options
        FeedOptions queryOptions = new FeedOptions { MaxItemCount = -1 };

            // Here we find the Andersen family via its LastName
            IQueryable<Family> familyQuery = this.client.CreateDocumentQuery<Family>(
                    UriFactory.CreateDocumentCollectionUri(databaseName, collectionName), queryOptions)
                    .Where(f => f.LastName == "Andersen");

            // The query is executed synchronously here, but can also be executed asynchronously via the IDocumentQuery<T> interface
            Console.WriteLine("Running LINQ query...");
            foreach (Family family in familyQuery)
            {
                    Console.WriteLine("\tRead {0}", family);
            }

            // Now execute the same query via direct SQL
            IQueryable<Family> familyQueryInSql = this.client.CreateDocumentQuery<Family>(
                    UriFactory.CreateDocumentCollectionUri(databaseName, collectionName),
                    "SELECT * FROM Family WHERE Family.lastName = 'Andersen'",
                    queryOptions);

            Console.WriteLine("Running direct SQL query...");
            foreach (Family family in familyQueryInSql)
            {
                    Console.WriteLine("\tRead {0}", family);
            }

            Console.WriteLine("Press any key to continue ...");
            Console.ReadKey();
    }

Skopiuj i wklej następujący kod do metody **GetStartedDemo** poniżej kodu służącego do tworzenia drugiego dokumentu.

    await this.CreateFamilyDocumentIfNotExists("FamilyDB", "FamilyCollection", wakefieldFamily);

    // ADD THIS PART TO YOUR CODE
    this.ExecuteSimpleQuery("FamilyDB", "FamilyCollection");

Naciśnij klawisz **F5**, aby uruchomić aplikację.

Gratulacje! Pomyślnie wykonano zapytanie względem kolekcji usługi DocumentDB.

Na poniższym diagramie przedstawiono sposób, w jaki składnia zapytania SQL usługi DocumentDB jest wywoływana względem utworzonej kolekcji. Ta sama logika dotyczy również zapytania LINQ.

![Diagram pokazujący zakres i znaczenie zapytania używanego przez samouczek NoSQL do utworzenia aplikacji konsolowej C#](./media/documentdb-get-started/nosql-tutorial-collection-documents.png)

Słowo kluczowe [FROM](documentdb-sql-query.md#from-clause) jest opcjonalne w zapytaniu, ponieważ zapytania usługi DocumentDB mają już zakres określony jako jedna kolekcja. W związku z tym klauzula "FROM Families f" może być zamieniona na "FROM root r" lub dowolną inną wybraną nazwę zmiennej. Usługa DocumentDB będzie wnioskowała, że zmienne Families, root lub wybrana nazwa zmiennej domyślnie odwołują się do bieżącej kolekcji.

##<a id="ReplaceDocument"></a>Krok 8. Zastępowanie dokumentu JSON

Usługa DocumentDB obsługuje zastępowanie dokumentów JSON.  

Skopiuj i wklej metodę **ReplaceFamilyDocument** poniżej metody **ExecuteSimpleQuery**.

    // ADD THIS PART TO YOUR CODE
    private async Task ReplaceFamilyDocument(string databaseName, string collectionName, string familyName, Family updatedFamily)
    {
            try
            {
                    await this.client.ReplaceDocumentAsync(UriFactory.CreateDocumentUri(databaseName, collectionName, familyName), updatedFamily);
                    this.WriteToConsoleAndPromptToContinue("Replaced Family {0}", familyName);
            }
            catch (DocumentClientException de)
            {
                    throw de;
            }
    }

Skopiuj i wklej następujący kod do metody **GetStartedDemo** poniżej kodu służącego do wykonywania zapytania. Po zastąpieniu dokumentu spowoduje to ponowne uruchomienie tego samego zapytania, aby wyświetlić zmieniony dokument.

    await this.CreateFamilyDocumentIfNotExists("FamilyDB", "FamilyCollection", wakefieldFamily);

    this.ExecuteSimpleQuery("FamilyDB", "FamilyCollection");

    // ADD THIS PART TO YOUR CODE
    // Update the Grade of the Andersen Family child
    andersenFamily.Children[0].Grade = 6;

    await this.ReplaceFamilyDocument("FamilyDB", "FamilyCollection", "Andersen.1", andersenFamily);

    this.ExecuteSimpleQuery("FamilyDB", "FamilyCollection");

Naciśnij klawisz **F5**, aby uruchomić aplikację.

Gratulacje! Pomyślnie zastąpiono dokument usługi DocumentDB.

##<a id="DeleteDocument"></a>Krok 9. Usuwanie dokumentu JSON

Usługa DocumentDB obsługuje usuwanie dokumentów JSON.  

Skopiuj i wklej metodę **DeleteFamilyDocument** poniżej metody **ReplaceFamilyDocument**.

    // ADD THIS PART TO YOUR CODE
    private async Task DeleteFamilyDocument(string databaseName, string collectionName, string documentName)
    {
            try
            {
                    await this.client.DeleteDocumentAsync(UriFactory.CreateDocumentUri(databaseName, collectionName, documentName));
                    Console.WriteLine("Deleted Family {0}", documentName);
            }
            catch (DocumentClientException de)
            {
                            throw de;
            }
    }

Skopiuj i wklej następujący kod do metody **GetStartedDemo** poniżej kodu służącego do drugiego wykonywania zapytania.

    await this.ReplaceFamilyDocument("FamilyDB", "FamilyCollection", "Andersen.1", andersenFamily);

    this.ExecuteSimpleQuery("FamilyDB", "FamilyCollection");

    // ADD THIS PART TO CODE
    await this.DeleteFamilyDocument("FamilyDB", "FamilyCollection", "Andersen.1");

Naciśnij klawisz **F5**, aby uruchomić aplikację.

Gratulacje! Pomyślnie usunięto dokument usługi DocumentDB.

##<a id="DeleteDatabase"></a>Krok 10. Usuwanie bazy danych

Usunięcie utworzonej bazy danych spowoduje usunięcie bazy danych i wszystkich zasobów podrzędnych (kolekcji, dokumentów itd.).

Skopiuj i wklej następujący kod do metody **GetStartedDemo** poniżej kodu służącego do usuwania dokumentu, aby usunąć całą bazę danych i wszystkie zasoby podrzędne.

    this.ExecuteSimpleQuery("FamilyDB", "FamilyCollection");

    await this.DeleteFamilyDocument("FamilyDB", "FamilyCollection", "Andersen.1");

    // ADD THIS PART TO CODE
    // Clean up/delete the database
    await this.client.DeleteDatabaseAsync(UriFactory.CreateDatabaseUri("FamilyDB"));

Naciśnij klawisz **F5**, aby uruchomić aplikację.

Gratulacje! Pomyślnie usunięto bazę danych usługi DocumentDB.

##<a id="Run"></a>Krok 11. Uruchamianie całej aplikacji konsolowej C#

Naciśnij klawisz F5 w programie Visual Studio, aby skompilować aplikację w trybie debugowania.

Powinny zostać wyświetlone dane wyjściowe aplikacji rozpoczynania pracy. Dane wyjściowe będą pokazywały wyniki dodanych zapytań i powinny odpowiadać poniższemu przykładowemu tekstowi.

    Created FamilyDB
    Press any key to continue ...
    Created FamilyCollection
    Press any key to continue ...
    Created Family Andersen.1
    Press any key to continue ...
    Created Family Wakefield.7
    Press any key to continue ...
    Running LINQ query...
        Read {"id":"Andersen.1","LastName":"Andersen","District":"WA5","Parents":[{"FamilyName":null,"FirstName":"Thomas"},{"FamilyName":null,"FirstName":"Mary Kay"}],"Children":[{"FamilyName":null,"FirstName":"Henriette Thaulow","Gender":"female","Grade":5,"Pets":[{"GivenName":"Fluffy"}]}],"Address":{"State":"WA","County":"King","City":"Seattle"},"IsRegistered":true}
    Running direct SQL query...
        Read {"id":"Andersen.1","LastName":"Andersen","District":"WA5","Parents":[{"FamilyName":null,"FirstName":"Thomas"},{"FamilyName":null,"FirstName":"Mary Kay"}],"Children":[{"FamilyName":null,"FirstName":"Henriette Thaulow","Gender":"female","Grade":5,"Pets":[{"GivenName":"Fluffy"}]}],"Address":{"State":"WA","County":"King","City":"Seattle"},"IsRegistered":true}
    Replaced Family Andersen.1
    Press any key to continue ...
    Running LINQ query...
        Read {"id":"Andersen.1","LastName":"Andersen","District":"WA5","Parents":[{"FamilyName":null,"FirstName":"Thomas"},{"FamilyName":null,"FirstName":"Mary Kay"}],"Children":[{"FamilyName":null,"FirstName":"Henriette Thaulow","Gender":"female","Grade":6,"Pets":[{"GivenName":"Fluffy"}]}],"Address":{"State":"WA","County":"King","City":"Seattle"},"IsRegistered":true}
    Running direct SQL query...
        Read {"id":"Andersen.1","LastName":"Andersen","District":"WA5","Parents":[{"FamilyName":null,"FirstName":"Thomas"},{"FamilyName":null,"FirstName":"Mary Kay"}],"Children":[{"FamilyName":null,"FirstName":"Henriette Thaulow","Gender":"female","Grade":6,"Pets":[{"GivenName":"Fluffy"}]}],"Address":{"State":"WA","County":"King","City":"Seattle"},"IsRegistered":true}
    Deleted Family Andersen.1
    End of demo, press any key to exit.

Gratulacje! Pomyślnie ukończono ten samouczek NoSQL i utworzono działającą aplikację konsolową C#.

##<a id="GetSolution"></a> Pobieranie kompletnego rozwiązania samouczka NoSQL
Do utworzenia rozwiązania GetStarted, które zawiera wszystkie przykłady w tym artykule, będą potrzebne następujące elementy:

- Aktywne konto platformy Azure. Jeśli go nie masz, możesz zarejestrować się w celu [utworzenia bezpłatnego konta](https://azure.microsoft.com/free/).
-   [Konto usługi DocumentDB][documentdb-create-account].
-   Rozwiązanie [GetStarted](https://github.com/Azure-Samples/documentdb-dotnet-getting-started) dostępne w witrynie GitHub.

Aby przywrócić odwołania do zestawu SDK dla platformy .NET usługi DocumentDB w programie Visual Studio, kliknij prawym przyciskiem myszy rozwiązanie **GetStarted** w Eksploratorze rozwiązań, a następnie kliknij polecenie **Włącz przywracanie pakietów NuGet**. Następnie w pliku App.config zaktualizuj wartości EndpointUrl i AuthorizationKey zgodnie z opisem w kroku [Łączenie z kontem usługi DocumentDB](#Connect).

## Następne kroki

- Potrzebujesz bardziej złożonego samouczka NoSQL platformy ASP.NET MVC? Zobacz [Build a web application with ASP.NET MVC using DocumentDB](documentdb-dotnet-application.md) (Tworzenie aplikacji sieci Web na platformie ASP.NET MVC przy użyciu usługi DocumentDB).
- Czy chcesz wykonać testowanie wydajności i skalowania w usłudze DocumentDB? Zobacz [Performance and Scale Testing with Azure DocumentDB](documentdb-performance-testing.md) (Testowanie wydajności i skali w usłudze Azure DocumentDB).
-   Dowiedz się, jak [monitorować konto usługi DocumentDB](documentdb-monitor-accounts.md).
-   Uruchom zapytania względem naszego przykładowego zestawu danych na [placu zabaw dla zapytań](https://www.documentdb.com/sql/demo).
-   Dowiedz się więcej o modelu programowania w sekcji Dla deweloperów [strony dokumentacji usługi DocumentDB](https://azure.microsoft.com/documentation/services/documentdb/).

[documentdb-create-account]: documentdb-create-account.md
[documentdb-manage]: documentdb-manage.md
[klucze]: media/documentdb-get-started/nosql-tutorial-keys.png



<!--HONumber=Jun16_HO2-->


