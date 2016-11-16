---
title: "Samouczek platformy ASP.NET MVC dla usługi DocumentDB: opracowywanie aplikacji sieci Web | Microsoft Docs"
description: "Samouczek platformy ASP.NET MVC, który umożliwia utworzenie aplikacji sieci Web MVC za pomocą usługi DocumentDB. Zapiszesz dane w postaci kodu JSON i uzyskasz do nich dostęp za pomocą aplikacji listy rzeczy do zrobienia hostowanej w usłudze Azure Websites — szczegółowy samouczek dla platformy ASP.NET MVC."
keywords: samouczek asp.net mvc, programowanie aplikacji sieci web, aplikacja sieci web mvc, samouczek krok po kroku asp.net mvc
services: documentdb
documentationcenter: .net
author: syamkmsft
manager: jhubbard
editor: cgronlun
ms.assetid: 52532d89-a40e-4fdf-9b38-aadb3a4cccbc
ms.service: documentdb
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 08/25/2016
ms.author: syamk
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: af5563f875c532c0b902685219818b1cd0945a66


---
# <a name="a-nametoc395809351aaspnet-mvc-tutorial-web-application-development-with-documentdb"></a><a name="_Toc395809351"></a>Samouczek platformy ASP.NET MVC: opracowywanie aplikacji sieci Web za pomocą usługi DocumentDB
> [!div class="op_single_selector"]
> * [.NET](documentdb-dotnet-application.md)
> * [Node.js](documentdb-nodejs-application.md)
> * [Java](documentdb-java-application.md)
> * [Python](documentdb-python-application.md) 
> 
> 

Aby podkreślić, jak możesz efektywnie wykorzystać usługę Azure DocumentDB do zapisywania i odpytywania dokumentów JSON, ten artykuł przedstawia kompletny przewodnik tworzenia aplikacji listy rzeczy do zrobienia używającej usługi Azure DocumentDB. Zadania będą przechowywane jako dokumenty JSON w usłudze Azure DocumentDB.

![Zrzut ekranu aplikacji sieci Web MVC listy rzeczy do zrobienia utworzonej w ramach tego samouczka — szczegółowy samouczek dla platformy ASP.NET MVC](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-image1.png)

Ten przewodnik przedstawia, w jaki sposób należy korzystać z usługi DocumentDB na platformie Azure do zapisywania danych i uzyskiwania do nich dostępu za pomocą aplikacji sieci Web platformy ASP.NET MVC hostowanej na platformie Azure. Jeśli szukasz samouczka, który koncentruje się tylko na usłudze DocumentDB, a nie na składnikach platformy ASP.NET MVC, zobacz [Build a DocumentDB C# console application](documentdb-get-started.md) (Tworzenie aplikacji konsolowej w języku C# dla usługi DocumentDB).

> [!TIP]
> Ten samouczek zakłada, że masz już pewne doświadczenie w korzystaniu z platformy ASP.NET MVC i usługi Azure Websites. Jeśli nie znasz platformy ASP.NET lub [wstępnie wymaganych narzędzi](#_Toc395637760), zalecamy pobranie kompletnego przykładowego projektu z usługi [GitHub][GitHub] i postępowanie zgodnie z instrukcjami zawartymi w tym przykładzie. Po jego skompilowaniu możesz przejrzeć ten artykuł, aby przeanalizować kod w kontekście projektu.
> 
> 

## <a name="a-nametoc395637760aprerequisites-for-this-database-tutorial"></a><a name="_Toc395637760"></a>Wymagania wstępne dotyczące tego samouczka bazy danych
Przed wykonaniem instrukcji zawartych w tym artykule upewnij się, że masz następujące elementy:

* Aktywne konto platformy Azure. Jeśli jej nie masz, możesz utworzyć bezpłatne konto próbne w zaledwie kilka minut. Aby uzyskać szczegółowe informacje, zobacz artykuł [Bezpłatna wersja próbna platformy Azure](https://azure.microsoft.com/pricing/free-trial/).
* [Visual Studio 2015](http://www.visualstudio.com/) albo Visual Studio 2013 Update 4 lub nowszy. W przypadku korzystania z pakietu Visual Studio 2013 należy zainstalować [pakiet NuGet Microsoft.Net.Compilers](https://www.nuget.org/packages/Microsoft.Net.Compilers/), aby zapewnić obsługę języka C# 6.0. 
* Zestaw Azure SDK dla platformy .NET w wersji 2.5.1 lub nowszej, który jest dostępny za pośrednictwem [Instalatora platformy Microsoft Web][Instalator platformy Microsoft Web].

Wszystkie zrzuty ekranu w tym artykule wykonano za pomocą programu Visual Studio 2013 z aktualizacją Update 4 i zestawu Azure SDK dla platformy .NET w wersji 2.5.1. Jeśli w Twoim systemie są skonfigurowane inne wersje, możliwe, że Twoje ekrany i opcje nie będą całkiem zgodne, lecz jeśli spełniasz powyższe wymagania wstępne, to rozwiązanie powinno działać.

## <a name="a-nametoc395637761astep-1-create-a-documentdb-database-account"></a><a name="_Toc395637761"></a>Krok 1. Tworzenie konta bazy danych usługi DocumentDB
Zacznijmy od utworzenia konta usługi DocumentDB. Jeśli masz już konto, możesz przejść do kroku [Tworzenie nowej aplikacji platformy ASP.NET MVC](#_Toc395637762).

[!INCLUDE [documentdb-create-dbaccount](../../includes/documentdb-create-dbaccount.md)]

[!INCLUDE [documentdb-keys](../../includes/documentdb-keys.md)]

<br/>
Teraz przeprowadzimy Cię przez proces tworzenia nowej aplikacji platformy ASP.NET MVC od podstaw. 

## <a name="a-nametoc395637762astep-2-create-a-new-aspnet-mvc-application"></a><a name="_Toc395637762"></a>Krok 2. Tworzenie nowej aplikacji platformy ASP.NET MVC
Teraz, gdy masz konto, utwórzmy nowy projekt platformy ASP.NET.

1. W menu **Plik** programu Visual Studio wskaż pozycję **Nowy**, a następnie kliknij pozycję **Projekt**.
   
       The **New Project** dialog box appears.
2. W okienku **Typy projektów** rozwiń element **Szablony**, **Visual C#**, **Sieć Web**, a następnie wybierz pozycję **Aplikacja sieci Web ASP.NET**.
   
      ![Zrzut ekranu okna dialogowego Nowy projekt z wyróżnionym typem projektu Aplikacja sieci Web ASP.NET](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-image10.png)
3. W polu **Nazwa** wpisz nazwę projektu. W tym samouczku jest używana nazwa „todo”. Jeśli wybierzesz inną nazwę, to wszędzie tam, gdzie jest wspomniana przestrzeń nazw todo, musisz dostosować podane przykłady kodu tak, aby używały nazwy Twojej aplikacji. 
4. Kliknij polecenie **Przeglądaj**, aby przejść do folderu, w którym chcesz utworzyć projekt, a następnie kliknij przycisk **OK**.
   
      Zostanie wyświetlone okno dialogowe **Nowy projekt ASP.NET**.
   
      ![Zrzut ekranu okna dialogowego Nowy projekt ASP.NET z wyróżnionym szablonem aplikacji MVC i zaznaczonym polem Hostuj w chmurze](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-image11.png)
5. W okienku szablonów wybierz pozycję **MVC**.
6. Jeśli planujesz hostowanie aplikacji na platformie Azure, wybierz pozycję **Hostuj w chmurze** u dołu po prawej, co spowoduje, że aplikacja będzie hostowana na platformie Azure. Wybraliśmy hostowanie w chmurze i uruchamianie hostowanej aplikacji w witrynie sieci Web platformy Azure. Wybranie tej opcji spowoduje wstępne aprowizowanie witryny sieci Web platformy Azure, co znacznie ułatwi pracę podczas wdrażania ostatecznej, działającej wersji aplikacji. Jeśli chcesz hostować aplikację w innym miejscu lub nie chcesz konfigurować platformy Azure z wyprzedzeniem, po prostu usuń zaznaczenie pola **Hostuj w chmurze**.
7. Kliknij przycisk **OK**, aby umożliwić programowi Visual Studio przygotowanie szkieletu na podstawie pustego szablonu platformy ASP.NET MVC. 
8. W przypadku wybrania hostowania w chmurze zostanie wyświetlone co najmniej jedno dodatkowe okno z monitem o zalogowanie na konto Azure i podanie kilku wartości dotyczących nowej witryny sieci Web. Podaj wszystkie dodatkowe wartości i kontynuuj. 
   
      Pozycja „Serwer bazy danych” nie została tu wybrana, ponieważ nie używamy serwera bazy danych SQL Azure. Zamierzamy utworzyć nowe konto usługi Azure DocumentDB później w witrynie Azure Portal.
   
    Aby uzyskać więcej informacji o wybieraniu **planu usługi App Service** i **grupy zasobów**, zobacz [Azure App Service plans in-depth overview](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) (Szczegółowe omówienie planów usługi Azure App Service).
   
      ![Zrzut ekranu okna dialogowego Konfigurowanie witryny sieci Web platformy Microsoft Azure](./media/documentdb-dotnet-application/image11_1.png)
9. Po zakończeniu tworzenia standardowej aplikacji MVC przez program Visual Studio będzie dostępna pusta aplikacja platformy ASP.NET, którą można uruchomić lokalnie.
   
    Pominiemy uruchamianie projektu lokalnie, ponieważ na pewno wszyscy widzieliśmy aplikację „Hello World” platformy ASP.NET. Przejdźmy prosto do dodawania usługi DocumentDB do tego projektu i tworzenia aplikacji.

## <a name="a-nametoc395637767astep-3-add-documentdb-to-your-mvc-web-application-project"></a><a name="_Toc395637767"></a>Krok 3. Dodawanie usługi DocumentDB do projektu aplikacji sieci Web MVC
Teraz większość podstaw dotyczących platformy ASP.NET MVC potrzebnych dla rozwiązania jest już gotowa. Możemy przejść do rzeczywistego celu tego samouczka, to znaczy dodania usługi Azure DocumentDB do naszej aplikacji sieci Web MVC.

1. Zestaw SDK platformy .NET dla usługi DocumentDB ma postać pakietu NuGet i jest dystrybuowany jako taki pakiet. Aby pobrać pakiet NuGet w programie Visual Studio, użyj menedżera pakietów NuGet w programie Visual Studio, klikając prawym przyciskiem myszy projekt w **Eksploratorze rozwiązań**, a następnie klikając pozycję **Zarządzaj pakietami NuGet**.
   
      ![Zrzut ekranu opcji dostępnych w menu otwieranym prawym przyciskiem myszy dla projektu aplikacji sieci Web w Eksploratorze rozwiązań z wyróżnioną pozycją Zarządzaj pakietami NuGet.](./media/documentdb-dotnet-application/image21.png)
   
    Zostanie wyświetlone okno dialogowe **Zarządzanie pakietami NuGet**.
2. W polu **Przeglądaj** wpisz ciąg ***Azure DocumentDB***.
   
    Korzystając z wyników, zainstaluj pakiet **Biblioteka kliencka usługi Microsoft Azure DocumentDB**. Pakiet usługi DocumentDB zostanie pobrany i zainstalowany razem z wszystkimi zależnościami, takimi jak pakiet Newtonsoft.Json. Kliknij przycisk **OK** w oknie **Podgląd** i **Akceptuję** w oknie **Akceptacja licencji**, aby zakończyć instalację.
   
      ![Zrzut ekranu okna Zarządzanie pakietami NuGet z wyróżnioną pozycją Biblioteka kliencka usługi Microsoft Azure DocumentDB](./media/documentdb-dotnet-application/nuget.png)
   
      Inna możliwość to zainstalowanie pakietu za pomocą konsoli menedżera pakietów. W tym celu w menu **Narzędzia** kliknij pozycję **Menedżer pakietów NuGet**, a następnie kliknij pozycję **Konsola menedżera pakietów**. W wierszu polecenia wpisz następujące polecenie.
   
        Install-Package Microsoft.Azure.DocumentDB
3. Po zainstalowaniu pakietu rozwiązanie Visual Studio powinno przypominać następujące — z dodanymi dwoma odwołaniami: Microsoft.Azure.Documents.Client i Newtonsoft.Json.
   
      ![Zrzut ekranu przedstawiający dwa odwołania dodane do projektu danych JSON w Eksploratorze rozwiązań](./media/documentdb-dotnet-application/image22.png)

## <a name="a-nametoc395637763astep-4-set-up-the-aspnet-mvc-application"></a><a name="_Toc395637763"></a>Krok 4. Konfigurowanie aplikacji platformy ASP.NET MVC
Teraz możemy dodać modele, widoki i kontrolery do aplikacji MVC:

* [Dodaj model](#_Toc395637764).
* [Dodaj kontroler](#_Toc395637765).
* [Dodaj widoki](#_Toc395637766).

### <a name="a-nametoc395637764aadd-a-json-data-model"></a><a name="_Toc395637764"></a>Dodawanie modelu danych JSON
Zacznijmy od utworzenia części reprezentowanej przez literę **M** w nazwie wzorca MVC, modelu. 

1. W **Eksploratorze rozwiązań** kliknij prawym przyciskiem myszy folder **Modele**, kliknij polecenie **Dodaj**, a następnie kliknij pozycję **Klasa**.
   
      Zostanie wyświetlone okno dialogowe **Dodawanie nowego elementu**.
2. Nadaj nowej klasie nazwę **Item.cs** i kliknij polecenie **Dodaj**. 
3. W nowym pliku **Item.cs** dodaj następujący wpis po ostatniej instrukcji *using*.
   
        using Newtonsoft.Json;
4. Teraz zastąp ten kod 
   
        public class Item
        {
        }
   
    następującym kodem.
   
        public class Item
        {
            [JsonProperty(PropertyName = "id")]
            public string Id { get; set; }
   
            [JsonProperty(PropertyName = "name")]
            public string Name { get; set; }
   
            [JsonProperty(PropertyName = "description")]
            public string Description { get; set; }
   
            [JsonProperty(PropertyName = "isComplete")]
            public bool Completed { get; set; }
        }
   
    Wszystkie dane w usłudze DocumentDB zostaną przekazane i zapisane w formacie JSON. Aby kontrolować sposób serializacji/deserializacji obiektów przez program JSON.NET, możesz użyć atrybutu **JsonProperty**, tak jak przedstawiono w klasie **Item**, którą właśnie utworzyliśmy. Nie **musisz** tego robić, lecz chcemy się upewnić, że właściwości są zgodne z konwencją nazewnictwa formatu JSON (camelCase). 
   
    Możesz nie tylko kontrolować format nazwy właściwości umieszczanej w kodzie JSON, ale także całkowicie zmienić nazwy właściwości platformy .NET, tak jak to zrobiono w przypadku właściwości **Description**. 

### <a name="a-nametoc395637765aadd-a-controller"></a><a name="_Toc395637765"></a>Dodawanie kontrolera
Część **M** jest gotowa, teraz utwórzmy część reprezentowaną przez literę **C** w nazwie wzorca MVC, klasę kontrolera (ang. controller).

1. W **Eksploratorze rozwiązań** kliknij prawym przyciskiem myszy folder **Kontrolery**, kliknij polecenie **Dodaj**, a następnie kliknij pozycję **Kontroler**.
   
    Zostanie wyświetlone okno dialogowe **Dodawanie szkieletu**.
2. Wybierz pozycję **Kontroler MVC 5 — pusty**, a następnie kliknij polecenie **Dodaj**.
   
    ![Zrzut ekranu okna dialogowego Dodawanie szkieletu z wyróżnioną opcją Kontroler MVC 5 — pusty](./media/documentdb-dotnet-application/image14.png)
3. Nadaj nowemu kontrolerowi nazwę **ItemController**.
   
    ![Zrzut ekranu okna dialogowego Dodawanie kontrolera](./media/documentdb-dotnet-application/image15.png)
   
    Po utworzeniu pliku rozwiązanie Visual Studio powinno przypominać następujące — z nowym plikiem ItemController.cs w **Eksploratorze rozwiązań**. Nowy plik Item.cs utworzony wcześniej jest także pokazany.
   
    ![Zrzut ekranu przedstawiający Eksploratora rozwiązań rozwiązania Visual Studio z wyróżnionymi nowymi plikami ItemController.cs i Item.cs](./media/documentdb-dotnet-application/image16.png)
   
    Możesz zamknąć plik ItemController.cs, wrócimy do niego później. 

### <a name="a-nametoc395637766aadd-views"></a><a name="_Toc395637766"></a>Dodawanie widoków
Teraz utwórzmy część reprezentowaną przez literę **V** w nazwie wzorca MVC, widoki (ang. view):

* [Dodaj widok indeksu elementów](#AddItemIndexView).
* [Dodaj widok nowego elementu](#AddNewIndexView).
* [Dodaj widok edycji elementu](#_Toc395888515).

#### <a name="a-nameadditemindexviewaadd-an-item-index-view"></a><a name="AddItemIndexView"></a>Dodawanie widoku indeksu elementów
1. W **Eksploratorze rozwiązań** rozwiń folder **Widoki**, kliknij prawym przyciskiem myszy pusty folder **Item** utworzony wcześniej przez program Visual Studio po dodaniu elementu **ItemController**, kliknij polecenie **Dodaj**, a następnie pozycję **Widok**.
   
    ![Zrzut ekranu Eksploratora rozwiązań przedstawiający folder Item utworzony przez program Visual Studio z wyróżnionymi poleceniami Dodaj i Widok](./media/documentdb-dotnet-application/image17.png)
2. W oknie dialogowym **Dodawanie widoku** wykonaj następujące czynności:
   
   * W polu **Nazwa widoku** wpisz ***Index*** (Indeks).
   * W polu **Szablon** wybierz pozycję ***Lista***.
   * W polu **Klasa modelu** wybierz pozycję ***Item (todo.Models)***.
   * Pole **Klasa kontekstu danych** pozostaw puste. 
   * W polu strony układu wpisz wartość ***~/Views/Shared/_Layout.cshtml***.
     
     ![Zrzut ekranu pokazujący okno dialogowe Dodawanie widoku](./media/documentdb-dotnet-application/image18.png)
3. Gdy wszystkie te wartości są ustawione, kliknij przycisk **Dodaj**. Program Visual Studio utworzy nowy widok szablonu. Po zakończeniu otworzy utworzony plik cshtml. Możemy zamknąć ten plik w programie Visual Studio, ponieważ wrócimy do niego później.

#### <a name="a-nameaddnewindexviewaadd-a-new-item-view"></a><a name="AddNewIndexView"></a>Dodawanie widoku nowego elementu
Podobnie jak w przypadku **indeksu elementów** teraz utworzymy nowy widok na potrzeby tworzenia nowych elementów **Item**.

1. W **Eksploratorze rozwiązań** kliknij ponownie prawym przyciskiem myszy folder **Item**, kliknij polecenie **Dodaj**, a następnie kliknij pozycję **Widok**.
2. W oknie dialogowym **Dodawanie widoku** wykonaj następujące czynności:
   
   * W polu **Nazwa widoku** wpisz ***Create*** (Tworzenie).
   * W polu **Szablon** wybierz pozycję ***Tworzenie***.
   * W polu **Klasa modelu** wybierz pozycję ***Item (todo.Models)***.
   * Pole **Klasa kontekstu danych** pozostaw puste.
   * W polu strony układu wpisz wartość ***~/Views/Shared/_Layout.cshtml***.
   * Kliknij pozycję **Dodaj**.

#### <a name="a-nametoc395888515aadd-an-edit-item-view"></a><a name="_Toc395888515"></a>Dodawanie widoku edycji elementu
I w końcu dodaj ostatni widok — na potrzeby edytowania elementu **Item** — w taki sam sposób jak wcześniej.

1. W **Eksploratorze rozwiązań** kliknij ponownie prawym przyciskiem myszy folder **Item**, kliknij polecenie **Dodaj**, a następnie kliknij pozycję **Widok**.
2. W oknie dialogowym **Dodawanie widoku** wykonaj następujące czynności:
   
   * W polu **Nazwa widoku** wpisz ***Edit*** (Edycja).
   * W polu **Szablon** wybierz pozycję ***Edycja***.
   * W polu **Klasa modelu** wybierz pozycję ***Item (todo.Models)***.
   * Pole **Klasa kontekstu danych** pozostaw puste. 
   * W polu strony układu wpisz wartość ***~/Views/Shared/_Layout.cshtml***.
   * Kliknij pozycję **Dodaj**.

Po zakończeniu zamknij wszystkie dokumenty cshtml w programie Visual Studio. Wrócimy do tych widoków później.

## <a name="a-nametoc395637769astep-5-wiring-up-documentdb"></a><a name="_Toc395637769"></a>Krok 5. Podłączanie usługi DocumentDB
Po przygotowaniu standardowych zasobów wzorca MVC możemy zacząć dodawać kod dla usługi DocumentDB. 

W tej sekcji dodamy kod obsługujący następujące operacje:

* [Wyświetlanie niezakończonych elementów](#_Toc395637770).
* [Dodawanie elementów](#_Toc395637771).
* [Edytowanie elementów](#_Toc395637772).

### <a name="a-nametoc395637770alisting-incomplete-items-in-your-mvc-web-application"></a><a name="_Toc395637770"></a>Wyświetlanie niezakończonych elementów w aplikacji sieci Web MVC
Najpierw musisz tutaj dodać klasę, która zawiera całą logikę umożliwiającą połączenie z usługą DocumentDB i używanie jej. Na potrzeby tego samouczka umieściliśmy całą tę logikę w klasie repozytorium o nazwie DocumentDBRepository. 

1. W **Eksploratorze rozwiązań** kliknij prawym przyciskiem myszy projekt, kliknij polecenie **Dodaj**, a następnie kliknij pozycję **Klasa**. Nadaj nowej klasie nazwę **DocumentDBRepository** i kliknij polecenie **Dodaj**.
2. W nowo utworzonej klasie **DocumentDBRepository** dodaj następujące instrukcje *using* powyżej deklaracji *namespace*.
   
        using Microsoft.Azure.Documents; 
        using Microsoft.Azure.Documents.Client; 
        using Microsoft.Azure.Documents.Linq; 
        using System.Configuration;
        using System.Linq.Expressions;
        using System.Threading.Tasks;
   
    Teraz zastąp ten kod 
   
        public class DocumentDBRepository
        {
        }
   
    następującym kodem.
   
        public static class DocumentDBRepository<T> where T : class
        {
            private static readonly string DatabaseId = ConfigurationManager.AppSettings["database"];
            private static readonly string CollectionId = ConfigurationManager.AppSettings["collection"];
            private static DocumentClient client;
   
            public static void Initialize()
            {
                client = new DocumentClient(new Uri(ConfigurationManager.AppSettings["endpoint"]), ConfigurationManager.AppSettings["authKey"]);
                CreateDatabaseIfNotExistsAsync().Wait();
                CreateCollectionIfNotExistsAsync().Wait();
            }
   
            private static async Task CreateDatabaseIfNotExistsAsync()
            {
                try
                {
                    await client.ReadDatabaseAsync(UriFactory.CreateDatabaseUri(DatabaseId));
                }
                catch (DocumentClientException e)
                {
                    if (e.StatusCode == System.Net.HttpStatusCode.NotFound)
                    {
                        await client.CreateDatabaseAsync(new Database { Id = DatabaseId });
                    }
                    else
                    {
                        throw;
                    }
                }
            }
   
            private static async Task CreateCollectionIfNotExistsAsync()
            {
                try
                {
                    await client.ReadDocumentCollectionAsync(UriFactory.CreateDocumentCollectionUri(DatabaseId, CollectionId));
                }
                catch (DocumentClientException e)
                {
                    if (e.StatusCode == System.Net.HttpStatusCode.NotFound)
                    {
                        await client.CreateDocumentCollectionAsync(
                            UriFactory.CreateDatabaseUri(DatabaseId),
                            new DocumentCollection { Id = CollectionId },
                            new RequestOptions { OfferThroughput = 1000 });
                    }
                    else
                    {
                        throw;
                    }
                }
            }
        }
   
   > [!TIP]
   > Podczas tworzenia nowego elementu DocumentCollection możesz podać opcjonalny parametr RequestOptions o wartości OfferType, co umożliwia określenie poziomu wydajności nowej kolekcji. Jeśli ten parametr nie zostanie przekazany, zostanie użyty domyślny typ oferty. Więcej informacji na temat typów oferty usługi DocumentDB można znaleźć w artykule [Poziomy wydajności usługi DocumentDB](documentdb-performance-levels.md).
   > 
   > 
3. Odczytujemy niektóre wartości z konfiguracji, więc otwórz plik **Web.config** aplikacji i dodaj następujące wiersze w sekcji `<AppSettings>`.
   
        <add key="endpoint" value="enter the URI from the Keys blade of the Azure Portal"/>
        <add key="authKey" value="enter the PRIMARY KEY, or the SECONDARY KEY, from the Keys blade of the Azure  Portal"/>
        <add key="database" value="ToDoList"/>
        <add key="collection" value="Items"/>
4. Teraz zaktualizuj wartości dla elementów *endpoint* i *authKey* za pomocą bloku Klucze w witrynie Azure Portal. Użyj wartości **URI** z bloku Klucze jako wartości ustawienia endpoint oraz wartości **KLUCZ PODSTAWOWY** lub **KLUCZ POMOCNICZY** z bloku Klucze jako wartości ustawienia authKey.

    W ten sposób podłączyliśmy repozytorium usługi DocumentDB. Teraz możemy dodać logikę aplikacji.

1. Pierwsza rzecz, którą chcemy udostępnić w aplikacji listy rzeczy do zrobienia, jest wyświetlanie niezakończonych elementów.  Skopiuj i wklej poniższy fragment kodu w dowolnym miejscu klasy **DocumentDBRepository**.
   
        public static async Task<IEnumerable<T>> GetItemsAsync(Expression<Func<T, bool>> predicate)
        {
            IDocumentQuery<T> query = client.CreateDocumentQuery<T>(
                UriFactory.CreateDocumentCollectionUri(DatabaseId, CollectionId))
                .Where(predicate)
                .AsDocumentQuery();
   
            List<T> results = new List<T>();
            while (query.HasMoreResults)
            {
                results.AddRange(await query.ExecuteNextAsync<T>());
            }
   
            return results;
        }
2. Otwórz dodany wcześniej element **ItemController** i dodaj następujące instrukcje *using* powyżej deklaracji namespace.
   
        using System.Net;
        using System.Threading.Tasks;
        using todo.Models;
   
    Jeśli projekt nie ma nazwy „todo”, zaktualizuj pozycję „todo.Models” tak, aby odpowiadała nazwie Twojego projektu.
   
    Teraz zastąp ten kod
   
        //GET: Item
        public ActionResult Index()
        {
            return View();
        }
   
    następującym kodem.
   
        [ActionName("Index")]
        public async Task<ActionResult> IndexAsync()
        {
            var items = await DocumentDBRepository<Item>.GetItemsAsync(d => !d.Completed);
            return View(items);
        }
3. Otwórz plik **Global.asax.cs** i dodaj następujący wiersz do metody **Application_Start**. 
   
        DocumentDBRepository<todo.Models.Item>.Initialize();

W tym momencie rozwiązanie powinno być możliwe do skompilowania bez żadnych błędów.

Jeśli uruchomisz aplikację teraz, przejdziesz do kontrolera **HomeController** i widoku **Index** (Index) tego kontrolera. Jest to domyślne zachowanie dla projektu szablonu MVC wybranego na początku, lecz nie jest to zachowanie, o które nam chodzi. Zmieńmy routing aplikacji MVC, aby zmienić to zachowanie.

Otwórz plik ***App\_Start\RouteConfig.cs***, odszukaj wiersz rozpoczynający się od ciągu „defaults:” i zmień go na podobny do następującego.

        defaults: new { controller = "Item", action = "Index", id = UrlParameter.Optional }

W ten sposób platforma ASP.NET MVC zostanie poinformowana, że jeśli nie określisz wartości elementu URL na potrzeby kontrolowania zachowania routingu, to zamiast elementu **Home** zostanie użyty element **Item** jako kontroler i element **Index** użytkownika jako widok.

Teraz po uruchomieniu aplikacji zostanie wywołany element **ItemController**, który wywoła klasę repozytorium i użyje metody GetItems do zwrócenia wszystkich niezakończonych elementów do widoku **Views**\\**Item**\\**Index**. 

Jeśli skompilujesz i uruchomisz projekt teraz, zobaczysz stronę podobną do następującej.    

![Zrzut ekranu przedstawiający aplikację sieci Web listy rzeczy do zrobienia utworzoną za pomocą tego samouczka bazy danych](./media/documentdb-dotnet-application/image23.png)

### <a name="a-nametoc395637771aadding-items"></a><a name="_Toc395637771"></a>Dodawanie elementów
Umieśćmy kilka elementów w naszej bazie danych, dzięki czemu będziemy mogli zobaczyć coś więcej niż pustą siatkę.

Dodajmy trochę kodu do elementów DocumentDBRepository i ItemController, aby utrwalić rekord w usłudze DocumentDB.

1. Dodaj następującą metodę do klasy **DocumentDBRepository**.
   
       public static async Task<Document> CreateItemAsync(T item)
       {
           return await client.CreateDocumentAsync(UriFactory.CreateDocumentCollectionUri(DatabaseId, CollectionId), item);
       }
   
   Ta metoda po prostu przyjmuje przekazany obiekt i utrwala go w usłudze DocumentDB.
2. Otwórz plik ItemController.cs i dodaj następujący fragment kodu w klasie. W ten sposób platforma ASP.NET MVC wie, co robić w przypadku wywołania akcji **Create** (Utwórz). W tym przypadku po prostu renderuje skojarzony widok Create.cshtml utworzony wcześniej.
   
        [ActionName("Create")]
        public async Task<ActionResult> CreateAsync()
        {
            return View();
        }
   
    Teraz potrzebujemy kodu w tym kontrolerze, który zaakceptuje dane przesłane z widoku **Create** (Tworzenie).
3. Dodaj kolejny blok kodu do klasy ItemController.cs, który poinformuje platformę ASP.NET MVC o tym, co należy zrobić w przypadku tego kontrolera po wysłaniu do niego formularza za pomocą akcji POST.
   
        [HttpPost]
        [ActionName("Create")]
        [ValidateAntiForgeryToken]
        public async Task<ActionResult> CreateAsync([Bind(Include = "Id,Name,Description,Completed")] Item item)
        {
            if (ModelState.IsValid)
            {
                await DocumentDBRepository<Item>.CreateItemAsync(item);
                return RedirectToAction("Index");
            }
   
            return View(item);
        }
   
    Ten kod wywołuje element DocumentDBRepository i używa metody CreateItemAsync, aby utrwalić nowy element zadania do zrobienia w bazie danych. 
   
    **Uwaga dotycząca zabezpieczeń**: atrybut **ValidateAntiForgeryToken** jest tu używany do ochrony aplikacji przed atakami polegającymi na fałszerstwie żądania międzywitrynowego. Do tego jest wymagane nie tylko dodanie tego atrybutu, lecz także obsługa tokenu zapobiegającego fałszerstwu przez widoki. Aby uzyskać więcej informacji na ten temat i przykłady poprawnej implementacji, zobacz [Preventing Cross-Site Request Forgery][Preventing Cross-Site Request Forgery] (Zapobieganie fałszerstwom żądania międzywitrynowego). Kod źródłowy dostępny w usłudze [GitHub][GitHub] zawiera pełną implementację.
   
    **Uwaga dotycząca zabezpieczeń**: korzystamy również z atrybutu **Bind** dla parametru metody, aby ułatwić ochronę przed atakami polegającymi na przesyłaniu zmodyfikowanych akcji POST. Aby poznać więcej szczegółów, zobacz [Basic CRUD Operations in ASP.NET MVC][Basic CRUD Operations in ASP.NET MVC] (Podstawowe operacje CRUD na platformie ASP.NET MVC).

Teraz kod wymagany do dodawania nowych elementów do bazy jest kompletny.

### <a name="a-nametoc395637772aediting-items"></a><a name="_Toc395637772"></a>Edytowanie elementów
Ostatnia rzecz do zrobienia to dodanie możliwości edytowania elementów **Item** w bazie danych i oznaczania ich jako zakończonych. Widok edycji został już dodany do projektu, więc musimy tylko ponownie dodać kod do kontrolera i klasy **DocumentDBRepository**.

1. Dodaj następujący kod do klasy **DocumentDBRepository**.
   
        public static async Task<Document> UpdateItemAsync(string id, T item)
        {
            return await client.ReplaceDocumentAsync(UriFactory.CreateDocumentUri(DatabaseId, CollectionId, id), item);
        }
   
        public static async Task<T> GetItemAsync(string id)
        {
            try
            {
                Document document = await client.ReadDocumentAsync(UriFactory.CreateDocumentUri(DatabaseId, CollectionId, id));
                return (T)(dynamic)document;
            }
            catch (DocumentClientException e)
            {
                if (e.StatusCode == HttpStatusCode.NotFound)
                {
                    return null;
                }
                else
                {
                    throw;
                }
            }
        }
   
    Pierwsza z tych metod, **GetItem**, pobiera z usługi DocumentDB element Item, który jest przekazywany z powrotem do kontrolera **ItemController**, a następnie do widoku **Edit** (Edycja).
   
    Druga metoda, którą właśnie dodaliśmy, zastępuje element **Document** w usłudze DocumentDB wersją elementu **Document** przekazaną z kontrolera **ItemController**.
2. Dodaj następujący kod do klasy **ItemController**.
   
        [HttpPost]
        [ActionName("Edit")]
        [ValidateAntiForgeryToken]
        public async Task<ActionResult> EditAsync([Bind(Include = "Id,Name,Description,Completed")] Item item)
        {
            if (ModelState.IsValid)
            {
                await DocumentDBRepository<Item>.UpdateItemAsync(item.Id, item);
                return RedirectToAction("Index");
            }
   
            return View(item);
        }
   
        [ActionName("Edit")]
        public async Task<ActionResult> EditAsync(string id)
        {
            if (id == null)
            {
                return new HttpStatusCodeResult(HttpStatusCode.BadRequest);
            }
   
            Item item = await DocumentDBRepository<Item>.GetItemAsync(id);
            if (item == null)
            {
                return HttpNotFound();
            }
   
            return View(item);
        }
   
    Pierwsza metoda obsługuje akcję GET protokołu HTTP, która jest wywoływana po kliknięciu przez użytkownika linku **Edit** (Edytuj) w widoku **Index** (Indeks). Ta metoda pobiera element [**Document**](http://msdn.microsoft.com/library/azure/microsoft.azure.documents.document.aspx) z usługi DocumentDB i przekazuje go do widoku **Edit** (Edycja).
   
    Widok **Edit** (Edycja) wykona następnie akcję POST protokołu HTTP dla kontrolera **IndexController**. 
   
    Druga dodana metoda obsługuje przekazywanie zaktualizowanego obiektu do usługi DocumentDB w celu jego utrwalenia w bazie danych.

I gotowe — to wszystko, czego potrzebujemy, aby uruchomić aplikację, wyświetlić niezakończone **elementy**, dodać nowe **elementy** i edytować **elementy**.

## <a name="a-nametoc395637773astep-6-run-the-application-locally"></a><a name="_Toc395637773"></a>Krok 6. Uruchamianie aplikacji lokalnie
Aby przetestować aplikację na lokalnej maszynie, wykonaj następujące czynności:

1. Naciśnij klawisz F5 w programie Visual Studio, aby skompilować aplikację w trybie debugowania. Powinno to spowodować skompilowanie aplikacji i uruchomienie przeglądarki z wyświetloną stroną z pustą siatką, którą widzieliśmy wcześniej:
   
    ![Zrzut ekranu przedstawiający aplikację sieci Web listy rzeczy do zrobienia utworzoną za pomocą tego samouczka bazy danych](./media/documentdb-dotnet-application/image24.png)
   
    Jeśli używasz programu Visual Studio 2013 i wystąpi błąd „Nie można oczekiwać w treści klauzuli catch”, należy zainstalować [pakiet NuGet Microsoft.Net.Compilers](https://www.nuget.org/packages/Microsoft.Net.Compilers/). Można również porównać kod z przykładowym projektem w serwisie [GitHub][GitHub]. 
2. Kliknij link **Create new** (Utwórz nowy) i dodaj wartości w polach **Name** (Nazwa) i **Description** (Opis). Pozostaw pole wyboru **Completed** (Zakończono) niezaznaczone — w przeciwnym przypadku nowy **element** zostanie dodany jako zakończony i nie zostanie wyświetlony na początkowej liście.
   
    ![Zrzut ekranu widoku Create (Tworzenie)](./media/documentdb-dotnet-application/image25.png)
3. Kliknij przycisk **Create** (Utwórz) — nastąpi przekierowanie z powrotem do widoku **Index** (Indeks) i **element** zostanie wyświetlony na liście.
   
    ![Zrzut ekranu widoku Index (Indeks)](./media/documentdb-dotnet-application/image26.png)
   
    Możesz także dodać kilka kolejnych **elementów** do listy rzeczy do zrobienia.
4. Kliknij pozycję **Edit** (Edytuj) obok **elementu** na liście. Zostanie wyświetlony widok **Edit** (Edycja), w którym można zaktualizować dowolną właściwość obiektu, w tym flagę **Completed** (Zakończono). Po zaznaczeniu flagi **Complete** (Zakończono) i kliknięciu przycisku **Save** (Zapisz) **element** zostanie usunięty z listy niezakończonych zadań.
   
    ![Zrzut ekranu przedstawiający widok Index (Indeks) z zaznaczonym polem Completed (Zakończono)](./media/documentdb-dotnet-application/image27.png)
5. Po przetestowaniu aplikacji naciśnij klawisze Ctrl+F5, aby zatrzymać jej debugowanie. Wszystko jest gotowe do wdrożenia.

## <a name="a-nametoc395637774astep-7-deploy-the-application-to-azure-websites"></a><a name="_Toc395637774"></a>Krok 7. Wdrażanie aplikacji w usłudze Azure Websites
Teraz, gdy kompletna aplikacja działa poprawnie z usługą DocumentDB, wdrożymy tę aplikację sieci Web w usłudze Azure Websites. Jeśli wybrano pozycję **Hostuj w chmurze** podczas tworzenia pustego projektu platformy ASP.NET MVC, program Visual Studio zdecydowanie ułatwi pracę i wykona większość zadań za Ciebie. 

1. Aby opublikować aplikację, musisz tylko kliknąć prawym przyciskiem myszy projekt w **Eksploratorze rozwiązań** i kliknąć polecenie **Publikuj**.
   
    ![Zrzut ekranu opcji Publikuj w Eksploratorze rozwiązań](./media/documentdb-dotnet-application/image28.png)
2. Wszystko powinno być już skonfigurowane odpowiednio do Twoich poświadczeń. W rzeczywistości witryna sieci Web została już utworzona na platformie Azure w miejscu określonym przez **docelowy adres URL**. Teraz musisz tylko kliknąć polecenie **Publikuj**.
   
    ![Zrzut ekranu okna dialogowego Publikowanie w sieci Web w programie Visual Studio — szczegółowy samouczek platformy ASP.NET MVC](./media/documentdb-dotnet-application/image29.png)

W ciągu kilku sekund program Visual Studio zakończy publikowanie aplikacji sieci Web i uruchomi przeglądarkę, w której będzie można zobaczyć swoje dzieło działające na platformie Azure.

## <a name="a-nametoc395637775anext-steps"></a><a name="_Toc395637775"></a>Następne kroki
Gratulacje! Udało Ci się utworzyć Twoją pierwszą aplikację sieci Web dla platformy ASP.NET MVC używającą usługi Azure DocumentDB i opublikować ją w usłudze Azure Websites. Kod źródłowy kompletnej aplikacji, w tym funkcji szczegółów i usuwania, które nie zostały uwzględnione w tym samouczku, można pobrać lub sklonować z usługi [GitHub][GitHub]. Jeśli chcesz dodać go do swojej aplikacji, wystarczy pobrać kod i to zrobić.

Aby dodać kolejne funkcje do aplikacji, zapoznaj się z interfejsami API dostępnymi w [bibliotece usługi DocumentDB dla platformy .NET](https://msdn.microsoft.com/library/azure/dn948556.aspx). Możesz ją także współtworzyć za pomocą usługi [GitHub][GitHub]. 

[\*]: https://microsoft.sharepoint.com/teams/DocDB/Shared%20Documents/Documentation/Docs.LatestVersions/PicExportError
[Visual Studio Express]: http://www.visualstudio.com/products/visual-studio-express-vs.aspx
[Instalator platformy Microsoft Web]: http://www.microsoft.com/web/downloads/platform.aspx
[Preventing Cross-Site Request Forgery]: http://go.microsoft.com/fwlink/?LinkID=517254 (Zapobieganie fałszerstwom żądania międzywitrynowego)
[Basic CRUD Operations in ASP.NET MVC]: http://go.microsoft.com/fwlink/?LinkId=317598 (Podstawowe operacje CRUD na platformie ASP.NET MVC)
[GitHub]: https://github.com/Azure-Samples/documentdb-net-todo-app



<!--HONumber=Nov16_HO2-->


