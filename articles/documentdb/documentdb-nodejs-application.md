<properties 
    pageTitle="Poznaj środowisko Node.js — samouczek środowiska Node.js dla usługi DocumentDB | Microsoft Azure" 
    description="Poznaj środowisko Node.js! W tym samouczku przedstawiono, jak przy użyciu usługi Microsoft Azure DocumentDB przechowywać dane i uzyskiwać do nich dostęp z poziomu aplikacji sieci Web Node.js Express hostowanej przez usługę Azure Websites." 
    keywords="Programowanie aplikacji, samouczek bazy danych, nauka node.js, samouczek node.js, DocumentDB, Azure, Microsoft Azure"
    services="documentdb" 
    documentationCenter="nodejs" 
    authors="syamkmsft" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="nodejs" 
    ms.topic="hero-article" 
    ms.date="08/25/2016" 
    ms.author="syamk"/>


# <a name="<a-name="_toc395783175"></a>build-a-node.js-web-application-using-documentdb"></a><a name="_Toc395783175"></a>Tworzenie aplikacji sieci Web Node.js za pomocą usługi DocumentDB

> [AZURE.SELECTOR]
- [.NET](documentdb-dotnet-application.md)
- [Node.js](documentdb-nodejs-application.md)
- [Java](documentdb-java-application.md)
- [Python](documentdb-python-application.md)

W tym samouczku środowiska Node.js pokazano, jak przy użyciu usługi Azure DocumentDB przechowywać dane i uzyskiwać do nich dostęp z poziomu aplikacji Node.js Express hostowanej przez usługę Azure Websites.

Zalecamy rozpoczęcie pracy od obejrzenia poniższego klipu wideo, w którym wyjaśniono, jak aprowizować konto bazy danych usługi Azure DocumentDB i zapisać dokumenty JSON w aplikacji Node.js. 

> [AZURE.VIDEO azure-demo-getting-started-with-azure-documentdb-on-nodejs-in-linux]

Następnie wróć do tego samouczka środowiska Node.js, w którym poznasz odpowiedzi na następujące pytania:

- Jak pracować z usługą DocumentDB przy użyciu modułu documentdb menedżera npm?
- Jak wdrożyć aplikację sieci Web w usłudze Azure Websites?

Wykonując kroki tego samouczka bazy danych, utworzysz prostą aplikację do zarządzania zadaniami opartą na sieci Web, która umożliwia tworzenie, pobieranie i kończenie zadań. Zadania będą przechowywane jako dokumenty JSON w usłudze Azure DocumentDB.

![Zrzut ekranu aplikacji My Todo List utworzonej w tym samouczku środowiska Node.js](./media/documentdb-nodejs-application/image1.png)

Nie masz czasu na ukończenie tego samouczka i po prostu chcesz uzyskać kompletne rozwiązanie? Nie ma problemu, możesz pobrać kompletne przykładowe rozwiązanie z witryny [GitHub][].

## <a name="<a-name="_toc395783176"></a>prerequisites"></a><a name="_Toc395783176"></a>Wymagania wstępne

> [AZURE.TIP] Ten samouczek środowiska Node.js zakłada, że masz już pewne doświadczenie w korzystaniu ze środowiska Node.js i usługi Azure Websites.

Przed wykonaniem instrukcji zawartych w tym artykule upewnij się, że masz następujące elementy:

- Aktywne konto platformy Azure. Jeśli jej nie masz, możesz utworzyć bezpłatne konto próbne w zaledwie kilka minut. Aby uzyskać szczegółowe informacje, zobacz [Bezpłatna wersja próbna platformy Azure](https://azure.microsoft.com/pricing/free-trial/).
- [Node.js][] w wersji 0.10.29 lub nowszej.
- [Generator Express](http://www.expressjs.com/starter/generator.html) (można go zainstalować za pomocą polecenia `npm install express-generator -g`)
- [Git][].

## <a name="<a-name="_toc395637761"></a>step-1:-create-a-documentdb-database-account"></a><a name="_Toc395637761"></a>Krok 1. Tworzenie konta bazy danych usługi DocumentDB

Zacznijmy od utworzenia konta usługi DocumentDB. Jeśli masz już konto, możesz przejść do sekcji [Krok 2. Tworzenie nowej aplikacji Node.js](#_Toc395783178).

[AZURE.INCLUDE [documentdb-create-dbaccount](../../includes/documentdb-create-dbaccount.md)]

[AZURE.INCLUDE [documentdb-keys](../../includes/documentdb-keys.md)]

## <a name="<a-name="_toc395783178"></a>step-2:-learn-to-create-a-new-node.js-application"></a><a name="_Toc395783178"></a>Krok 2. Dowiedz się, jak utworzyć nową aplikację Node.js

Teraz nauczysz się, jak utworzyć podstawowy projekt aplikacji Hello World w środowisku Node.js przy użyciu platformy [Express](http://expressjs.com/).

1. Otwórz swój ulubiony terminal.

2. Użyj generatora platformy Express, aby wygenerować nową aplikację o nazwie **todo**.

        express todo

3. Otwórz nowy katalog **todo** i zainstaluj zależności.

        cd todo
        npm install

4. Uruchom nową aplikację.

        npm start

5. Możesz wyświetlić swoją nową aplikację, przechodząc w przeglądarce na adres [http://localhost:3000](http://localhost:3000).

    ![Poznaj środowisko Node.js — zrzut ekranu aplikacji Hello World w oknie przeglądarki](./media/documentdb-nodejs-application/image12.png)

## <a name="<a-name="_toc395783179"></a>step-3:-install-additional-modules"></a><a name="_Toc395783179"></a>Krok 3. Instalowanie dodatkowych modułów

Plik **package.json** jest jednym z plików utworzonych w folderze głównym projektu. Ten plik zawiera listę dodatkowych modułów, które są wymagane dla aplikacji Node.js. Później, podczas wdrażania tej aplikacji w usłudze Azure Websites, ten plik jest używany do określenia, które moduły muszą być zainstalowane na platformie Azure w celu obsługi tej aplikacji. Nadal trzeba zainstalować jeszcze dwa pakiety dla tego samouczka.

1. Z poziomu terminala zainstaluj moduł **async** za pośrednictwem menedżera npm.

        npm install async --save

1. Zainstaluj moduł **documentdb** za pomocą menedżera npm. To jest moduł, w którym dzieje się cała „magia” usługi DocumentDB.

        npm install documentdb --save

3. Szybkie sprawdzenie pliku **package.json** aplikacji powinno pokazać dodatkowe moduły. Ten plik poinformuje platformę Azure, które pakiety pobrać i zainstalować podczas uruchamiania aplikacji. Powinien on przypominać przykład poniżej.

    ![Zrzut ekranu przedstawiający kartę pliku package.json](./media/documentdb-nodejs-application/image17.png)

    W ten sposób węzeł (a potem platforma Azure) otrzyma informacje o tym, że aplikacja zależy od dodatkowych modułów.

## <a name="<a-name="_toc395783180"></a>step-4:-using-the-documentdb-service-in-a-node-application"></a><a name="_Toc395783180"></a>Krok 4. Korzystanie z usługi DocumentDB w aplikacji Node

To kończy całą wstępną instalację i konfigurację. Teraz przejdźmy do tego, po co tutaj jesteśmy, czyli napisania kodu za pomocą usługi Azure DocumentDB.

### <a name="create-the-model"></a>Tworzenie modelu

1. W katalogu projektu utwórz nowy katalog o nazwie **models**.
2. W katalogu **models** utwórz nowy plik o nazwie **taskDao.js**. Ten plik zawiera model dla zadań tworzonych przez naszą aplikację.
3. W tym samym katalogu **models** utwórz nowy plik o nazwie **docdbUtils.js**. Ten plik będzie zawierał pewną ilość przydatnego kodu do ponownego wykorzystania, który będzie używany w naszej aplikacji. 
4. Skopiuj poniższy kod do pliku**docdbUtils.js**

        var DocumentDBClient = require('documentdb').DocumentClient;
            
        var DocDBUtils = {
            getOrCreateDatabase: function (client, databaseId, callback) {
                var querySpec = {
                    query: 'SELECT * FROM root r WHERE r.id= @id',
                    parameters: [{
                        name: '@id',
                        value: databaseId
                    }]
                };
        
                client.queryDatabases(querySpec).toArray(function (err, results) {
                    if (err) {
                        callback(err);
        
                    } else {
                        if (results.length === 0) {
                            var databaseSpec = {
                                id: databaseId
                            };
        
                            client.createDatabase(databaseSpec, function (err, created) {
                                callback(null, created);
                            });
        
                        } else {
                            callback(null, results[0]);
                        }
                    }
                });
            },
        
            getOrCreateCollection: function (client, databaseLink, collectionId, callback) {
                var querySpec = {
                    query: 'SELECT * FROM root r WHERE r.id=@id',
                    parameters: [{
                        name: '@id',
                        value: collectionId
                    }]
                };             
                
                client.queryCollections(databaseLink, querySpec).toArray(function (err, results) {
                    if (err) {
                        callback(err);
        
                    } else {        
                        if (results.length === 0) {
                            var collectionSpec = {
                                id: collectionId
                            };
                            
                            client.createCollection(databaseLink, collectionSpec, function (err, created) {
                                callback(null, created);
                            });
        
                        } else {
                            callback(null, results[0]);
                        }
                    }
                });
            }
        };
                
        module.exports = DocDBUtils;

    > [AZURE.TIP] Funkcja createCollection przyjmuje opcjonalny parametr requestOptions, który może służyć do określania typu oferty dla kolekcji. Jeśli nie podano żadnej wartości requestOptions.offerType, kolekcja zostanie utworzona przy użyciu domyślnego typu oferty.
    >
    > Więcej informacji na temat typów oferty usługi DocumentDB można znaleźć w artykule [Performance levels in DocumentDB](documentdb-performance-levels.md)(Poziomy wydajności w usługi DocumentDB). 
        
3. Zapisz i zamknij plik **docdbUtils.js**.

4. Na początku pliku **taskDao.js** dodaj następujący kod, aby odwołać się do elementu **DocumentDBClient** i pliku **docdbUtils.js**, które zostały utworzone powyżej:

        var DocumentDBClient = require('documentdb').DocumentClient;
        var docdbUtils = require('./docdbUtils');

4. Następnie dodasz kod w celu zdefiniowania i wyeksportowania obiektu Task. Jest on odpowiedzialny za inicjowanie obiektu Task oraz konfigurowanie bazy danych i kolekcji dokumentów, które będą używane.

        function TaskDao(documentDBClient, databaseId, collectionId) {
          this.client = documentDBClient;
          this.databaseId = databaseId;
          this.collectionId = collectionId;
        
          this.database = null;
          this.collection = null;
        }
        
        module.exports = TaskDao;

5. Następnie dodaj poniższy kod, aby zdefiniować dodatkowe metody dla obiektu Task umożliwiające interakcje z danymi przechowywanymi w usłudze DocumentDB.

        TaskDao.prototype = {
            init: function (callback) {
                var self = this;
        
                docdbUtils.getOrCreateDatabase(self.client, self.databaseId, function (err, db) {
                    if (err) {
                        callback(err);
                    } else {
                        self.database = db;
                        docdbUtils.getOrCreateCollection(self.client, self.database._self, self.collectionId, function (err, coll) {
                            if (err) {
                                callback(err);
        
                            } else {
                                self.collection = coll;
                            }
                        });
                    }
                });
            },
        
            find: function (querySpec, callback) {
                var self = this;
        
                self.client.queryDocuments(self.collection._self, querySpec).toArray(function (err, results) {
                    if (err) {
                        callback(err);
        
                    } else {
                        callback(null, results);
                    }
                });
            },
        
            addItem: function (item, callback) {
                var self = this;
        
                item.date = Date.now();
                item.completed = false;
        
                self.client.createDocument(self.collection._self, item, function (err, doc) {
                    if (err) {
                        callback(err);
        
                    } else {
                        callback(null, doc);
                    }
                });
            },
        
            updateItem: function (itemId, callback) {
                var self = this;
        
                self.getItem(itemId, function (err, doc) {
                    if (err) {
                        callback(err);
        
                    } else {
                        doc.completed = true;
        
                        self.client.replaceDocument(doc._self, doc, function (err, replaced) {
                            if (err) {
                                callback(err);
        
                            } else {
                                callback(null, replaced);
                            }
                        });
                    }
                });
            },
        
            getItem: function (itemId, callback) {
                var self = this;
        
                var querySpec = {
                    query: 'SELECT * FROM root r WHERE r.id = @id',
                    parameters: [{
                        name: '@id',
                        value: itemId
                    }]
                };
        
                self.client.queryDocuments(self.collection._self, querySpec).toArray(function (err, results) {
                    if (err) {
                        callback(err);
        
                    } else {
                        callback(null, results[0]);
                    }
                });
            }
        };

6. Zapisz i zamknij plik **taskDao.js**. 

### <a name="create-the-controller"></a>Tworzenie kontrolera

1. W katalogu **routes** projektu utwórz nowy plik o nazwie **tasklist.js**. 
2. Dodaj następujący kod do pliku **tasklist.js**. Służy on do ładowania modułów DocumentDBClient i async, które są używane przez plik **tasklist.js**. Definiuje również funkcję **TaskList**, która przekazuje wystąpienie obiektu **Task** zdefiniowanego wcześniej:

        var DocumentDBClient = require('documentdb').DocumentClient;
        var async = require('async');
        
        function TaskList(taskDao) {
          this.taskDao = taskDao;
        }
        
        module.exports = TaskList;

3. Kontynuuj dodawanie do pliku **tasklist.js** przez dodanie metod **showTasks, addTask** i **completeTasks**:
        
        TaskList.prototype = {
            showTasks: function (req, res) {
                var self = this;
        
                var querySpec = {
                    query: 'SELECT * FROM root r WHERE r.completed=@completed',
                    parameters: [{
                        name: '@completed',
                        value: false
                    }]
                };
        
                self.taskDao.find(querySpec, function (err, items) {
                    if (err) {
                        throw (err);
                    }
        
                    res.render('index', {
                        title: 'My ToDo List ',
                        tasks: items
                    });
                });
            },
        
            addTask: function (req, res) {
                var self = this;
                var item = req.body;
        
                self.taskDao.addItem(item, function (err) {
                    if (err) {
                        throw (err);
                    }
        
                    res.redirect('/');
                });
            },
        
            completeTask: function (req, res) {
                var self = this;
                var completedTasks = Object.keys(req.body);
        
                async.forEach(completedTasks, function taskIterator(completedTask, callback) {
                    self.taskDao.updateItem(completedTask, function (err) {
                        if (err) {
                            callback(err);
                        } else {
                            callback(null);
                        }
                    });
                }, function goHome(err) {
                    if (err) {
                        throw err;
                    } else {
                        res.redirect('/');
                    }
                });
            }
        };


4. Zapisz i zamknij plik **tasklist.js**.
 
### <a name="add-config.js"></a>Dodawanie pliku config.js

1. W katalogu projektu utwórz nowy plik o nazwie **config.js**.
2. Dodaj następujący kod do pliku **config.js**. Służy on do definiowania ustawień konfiguracji i wartości potrzebnych dla aplikacji.

        var config = {}
        
        config.host = process.env.HOST || "[the URI value from the DocumentDB Keys blade on http://portal.azure.com]";
        config.authKey = process.env.AUTH_KEY || "[the PRIMARY KEY value from the DocumentDB Keys blade on http://portal.azure.com]";
        config.databaseId = "ToDoList";
        config.collectionId = "Items";
        
        module.exports = config;

3. W pliku **config.js** zaktualizuj wartości HOST i AUTH_KEY przy użyciu wartości znajdujących się w bloku Klucze Twojego konta usługi DocumentDB w witrynie [Microsoft Azure Portal](https://portal.azure.com):

4. Zapisz i zamknij plik **config.js**.
 
### <a name="modify-app.js"></a>Modyfikowanie pliku app.js

1. W katalogu projektu otwórz plik **app.js**. Ten plik został utworzony wcześniej podczas tworzenia aplikacji sieci Web platformy Express.
2. Dodaj następujący kod na górze pliku **tasklist.js**.
    
        var DocumentDBClient = require('documentdb').DocumentClient;
        var config = require('./config');
        var TaskList = require('./routes/tasklist');
        var TaskDao = require('./models/taskDao');

3. Ten kod definiuje plik konfiguracji, który ma być używany, oraz odczytuje wartości z tego pliku i umieszcza je w pewnych zmiennych, których będziemy wkrótce używać.
4. Zastąp następujące dwa wiersze w pliku **app.js**:

        app.use('/', routes);
        app.use('/users', users); 

      następującym fragmentem kodu:

        var docDbClient = new DocumentDBClient(config.host, {
            masterKey: config.authKey
        });
        var taskDao = new TaskDao(docDbClient, config.databaseId, config.collectionId);
        var taskList = new TaskList(taskDao);
        taskDao.init();
        
        app.get('/', taskList.showTasks.bind(taskList));
        app.post('/addtask', taskList.addTask.bind(taskList));
        app.post('/completetask', taskList.completeTask.bind(taskList));
        app.set('view engine', 'jade');



6. Te wiersze definiują nowe wystąpienie obiektu **TaskDao**, z nowym połączeniem z usługą DocumentDB (przy użyciu wartości odczytanych z pliku **config.js**), inicjują obiekt zadania, a następnie wiążą akcje formularza z metodami w kontrolerze **TaskList**. 

7. Na koniec zapisz i zamknij plik **app.js**. To już prawie koniec.
 
## <a name="<a-name="_toc395783181"></a>step-5:-build-a-user-interface"></a><a name="_Toc395783181"></a>Krok 5. Tworzenie interfejsu użytkownika

Teraz skupimy się na tworzeniu interfejsu użytkownika, aby użytkownik mógł faktycznie wchodzić w interakcję z naszą aplikacją. Utworzona aplikacja Express używa aparatu widoku **Jade**. Więcej informacji na temat aparatu Jade można znaleźć w witrynie [http://jade-lang.com/](http://jade-lang.com/).

1. Plik **layout.jade** w katalogu **views** jest używany jako szablon globalny dla innych plików **jade**. W tym kroku zmodyfikujesz go w celu używania struktury [Twitter Bootstrap](https://github.com/twbs/bootstrap), która jest zestawem narzędzi ułatwiającym projektowanie dobrze wyglądającej witryny sieci Web. 
2. Otwórz plik **layout.jade** znajdujący się w folderze **views** i zastąp zawartość następującym kodem:
    
        doctype html
        html
          head
            title= title
            link(rel='stylesheet', href='//ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.min.css')
            link(rel='stylesheet', href='/stylesheets/style.css')
          body
            nav.navbar.navbar-inverse.navbar-fixed-top
              div.navbar-header
                a.navbar-brand(href='#') My Tasks
            block content
            script(src='//ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.min.js')
            script(src='//ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/bootstrap.min.js')



    Ten kod w praktyce określa, że aparat **Jade** ma renderować kod HTML dla aplikacji, i tworzy **blok** o nazwie **content**, w którym można udostępnić układ dla stron zawartości.
    Zapisz i zamknij ten plik **layout.jade**.

4. Teraz otwórz plik **index.jade**, definiujący widok, który będzie używany przez naszą aplikację, i zastąp zawartość pliku następującym kodem:

        extends layout
        
        block content
          h1 #{title}
          br
        
          form(action="/completetask", method="post")
            table.table.table-striped.table-bordered
              tr
                td Name
                td Category
                td Date
                td Complete
              if (typeof tasks === "undefined")
                tr
                  td
              else
                each task in tasks
                  tr
                    td #{task.name}
                    td #{task.category}
                    - var date  = new Date(task.date);
                    - var day   = date.getDate();
                    - var month = date.getMonth() + 1;
                    - var year  = date.getFullYear();
                    td #{month + "/" + day + "/" + year}
                    td
                      input(type="checkbox", name="#{task.id}", value="#{!task.completed}", checked=task.completed)
            button.btn(type="submit") Update tasks
          hr
          form.well(action="/addtask", method="post")
            label Item Name:
            input(name="name", type="textbox")
            label Item Category:
            input(name="category", type="textbox")
            br
            button.btn(type="submit") Add item

    Rozszerza on układ i udostępnia zawartość dla symbolu zastępczego **content**, który wcześniej widzieliśmy w pliku **layout.jade**.
    
    W tym układzie utworzyliśmy dwa formularze HTML. 
    Pierwszy formularz zawiera tabelę danych i przycisk umożliwiający aktualizowanie elementów przez publikowanie do metody **/completetask** kontrolera.
    Drugi formularz zawiera dwa pola wejściowe i przycisk umożliwiający utworzenie nowego elementu przez publikowanie do metody **/addtask** kontrolera.
    
    To powinno być wszystko, czego potrzebujemy, aby nasza aplikacja działała.

5. Otwórz plik **style.css** w katalogu **public\stylesheets** i zastąp kod następującym:

        body {
          padding: 50px;
          font: 14px "Lucida Grande", Helvetica, Arial, sans-serif;
        }
        a {
          color: #00B7FF;
        }
        .well label {
          display: block;
        }
        .well input {
          margin-bottom: 5px;
        }
        .btn {
          margin-top: 5px;
          border: outset 1px #C8C8C8;
        }

    Zapisz i zamknij plik **style.css**.

## <a name="<a-name="_toc395783181"></a>step-6:-run-your-application-locally"></a><a name="_Toc395783181"></a>Krok 6. Uruchamianie aplikacji lokalnie

1. Aby przetestować aplikację na komputerze lokalnym, uruchom polecenie `npm start` w terminalu, aby uruchomić aplikację i przeglądarkę ze stroną, która wygląda jak obraz poniżej:

    ![Zrzut ekranu aplikacji MyTodo List w oknie przeglądarki](./media/documentdb-nodejs-application/image18.png)


2. Użyj udostępnionych pól Item (Element), Item Name (Nazwa elementu) i Category (Kategoria) do wprowadzenia informacji, a następnie kliknij przycisk **Add Item** (Dodaj element).

3. Ta strona powinna zostać zaktualizowana w celu wyświetlenia nowo utworzonego elementu na liście ToDo.

    ![Zrzut ekranu aplikacji z nowym elementem na liście ToDo](./media/documentdb-nodejs-application/image19.png)

4. Aby zakończyć zadanie, po prostu zaznacz pole wyboru w kolumnie Complete (Zakończ), a następnie kliknij przycisk **Update tasks** (Aktualizuj zadania).

## <a name="<a-name="_toc395783182"></a>step-7:-deploy-your-application-development-project-to-azure-websites"></a><a name="_Toc395783182"></a>Krok 7. Wdrażanie projektu tworzenia aplikacji w usłudze Azure Websites

1. Jeśli jeszcze tego nie zrobiono, włącz repozytorium Git dla usługi Azure Websites. Instrukcje, jak to zrobić, można znaleźć w temacie [Local Git Deployment to Azure App Service](../app-service-web/app-service-deploy-local-git.md) (Lokalne wdrażanie przy użyciu systemu Git w usłudze Azure App Service).

2. Dodaj witrynę sieci Web platformy Azure jako element zdalny narzędzia Git.

        git remote add azure https://username@your-azure-website.scm.azurewebsites.net:443/your-azure-website.git

3. Wdróż przez wypchnięcie do elementu zdalnego.

        git push azure master

4. W ciągu kilku sekund narzędzie Git zakończy publikowanie aplikacji sieci Web i uruchomi przeglądarkę, w której możesz zobaczyć swoje dzieło uruchomione na platformie Azure!

## <a name="<a-name="_toc395637775"></a>next-steps"></a><a name="_Toc395637775"></a>Następne kroki

Gratulacje! Udało Ci się utworzyć swoją pierwszą aplikację sieci Web Node.js Express za pomocą usługi Azure DocumentDB i opublikować ją w usłudze Azure Websites.

Kod źródłowy kompletnej aplikacji referencyjnej można pobrać z witryny [GitHub][].

Aby uzyskać więcej informacji, odwiedź stronę [Centrum deweloperów środowiska Node.js](https://azure.microsoft.com/develop/nodejs/).

[Node.js]: http://nodejs.org/
[Git]: http://git-scm.com/
[Github]: https://github.com/Azure-Samples/documentdb-node-todo-app
 



<!--HONumber=Oct16_HO3-->


