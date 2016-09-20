<properties
    pageTitle="Wprowadzenie do usług Azure Mobile Services i Sencha"
    description="Postępuj zgodnie z tym samouczkiem, aby rozpocząć tworzenie rozwiązań z wykorzystaniem usług Mobile Services i platformy aplikacji mobilnych Sencha HTML5."
    services="mobile-services"
    documentationCenter=""
    authors="ggailey777"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="mobile-services"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-sencha"
    ms.devlang="multiple"
    ms.topic="get-started-article"
    ms.date="07/21/2016"
    ms.author="glenga"/>

# <a name="getting-started"> </a>Wprowadzenie do usługi Mobile Services i platformy Sencha Touch

[AZURE.INCLUDE [mobile-service-note-mobile-apps](../../includes/mobile-services-note-mobile-apps.md)]

&nbsp;


[AZURE.INCLUDE [mobile-services-selector-get-started](../../includes/mobile-services-selector-get-started.md)]
&nbsp;

[AZURE.INCLUDE [mobile-services-hero-slug](../../includes/mobile-services-hero-slug.md)]

##Omówienie

Ten samouczek pokazuje, jak wykorzystywać usługi Azure Mobile Services w aplikacji platformy Sencha Touch. Pozwala utworzyć prostą aplikację z *listą czynności do wykonania* za pomocą platformy Sencha Touch, która korzysta z usługi mobilnej zdefiniowanej za pośrednictwem klasycznego portalu Azure. Ten samouczek jest przeznaczony dla zaawansowanych programistów aplikacji Web mających dobrą znajomość języka JavaScript i znających platformę Sencha Touch.

Jeśli wolisz oglądać wersję wideo, ten klip opisuje te same kroki, które są zawarte w samouczku. Na klipie wideo Arthur Kay objaśnia sposób tworzenia aplikacji platformy Sencha Touch za pomocą zaplecza usług Azure Mobile Services.

> [AZURE.VIDEO getting-started-with-sencha-touch]


Zrzut ekranu ukończonej aplikacji jest pokazany poniżej:

![][0]

##Wymagania

- Pobierz i zainstaluj platformę [Sencha Touch](http://wwww.sencha.com/products/touch/download" target="_blank").

- Pobierz i zainstaluj narzędzie [Sencha Cmd](http://www.sencha.com/products/sencha-cmd/download" target="_blank").

- Środowisko Java Runtime Environment (JRE) lub Java Development Kit (jeśli tworzysz aplikacje dla systemu Android)
- Język Ruby i rozszerzenie SASS gem.

## <a name="create-new-service"> </a>Tworzenie nowej usługi mobilnej

[AZURE.INCLUDE [mobile-services-create-new-service](../../includes/mobile-services-create-new-service.md)]

##Tworzenie tabeli czynności do wykonania

Po utworzeniu usługi mobilnej można użyć funkcji szybkiego startu w klasycznym portalu Azure do utworzenia nowej tabeli bazy danych, która będzie używana w usłudze mobilnej.

1. W [klasyczny portal Azure] kliknij pozycję **Mobile Services**, a następnie kliknij wcześniej utworzoną usługę mobilną.

2. Na karcie szybkiego startu kliknij przycisk **HTML** w polu **Choose platform** (Wybierz platformę) i rozwiń obiekt **Create a new HTML app** (Utwórz nową aplikację HTML).

    ![Usługa mobilna — szybki start — html](./media/partner-sencha-mobile-services-get-started/mobile-portal-quickstart-html.png)

    Zostaną wyświetlane trzy łatwe kroki umożliwiające utworzenie i hostowanie aplikacji HTML połączonej z usługą mobilną.

    ![Usługa mobilna — szybki start — html](./media/partner-sencha-mobile-services-get-started/mobile-quickstart-steps-html.png)

3. Kliknij pozycję **Create TodoItems table** (Utwórz tabelę zadań do wykonania), aby utworzyć tabelę do przechowywania danych aplikacji.

    > [AZURE.NOTE] Nie pobieraj aplikacji HTML z klasycznego portalu Azure. Zamiast tego w poniższej sekcji ręcznie utworzymy aplikację platformy Sencha Touch.


1. Zwróć uwagę na pola **appKey** i **appUrl** w klasycznym portalu Azure. Użyjesz ich w innych częściach tego samouczka.

    ![klucz aplikacji](./media/partner-sencha-mobile-services-get-started/mobile-app-key-portal.png)

1. Na karcie **Konfiguracja** sprawdź, czy nazwa `localhost` znajduje się już na liście **Zezwalaj na żądania z nazw hostów** w obszarze **Współużytkowanie zasobów między źródłami (CORS)**. Jeśli nie, wpisz ciąg `localhost` w polu **Nazwa hosta**, a następnie kliknij pozycję **Zapisz**.

    ![Konfiguracja CORS hosta lokalnego](./media/partner-sencha-mobile-services-get-started/mobile-services-set-cors-localhost.png)

##Tworzenie aplikację platformy Touch

Tworzenie aplikacji szablonu Sencha Touch jest prostym zadaniem przy użyciu narzędzia Sencha Cmd i doskonałym sposobem na szybkie uruchomienie aplikacji.

W katalogu, w którym została zainstalowana platforma Touch, wykonaj następujące polecenie:

    $ sencha generate app Basic /path/to/application

Spowoduje to utworzenie szablonu aplikacji Touch z nazwą aplikacji „Basic”. Aby uruchomić aplikację, wystarczy wskazać w przeglądarce katalog , ścieżkę i aplikację, po czym powinna pojawić się standardowa przykładowa aplikacja platformy Touch.

##Instalowanie rozszerzeń Sencha Touch dla platformy Azure

Rozszerzenia dla platformy Azure jest instalowane ręcznie lub jako pakiet Sencha. Wybór metody zależy od użytkownika.

###Instalacja ręczna

W większości aplikacji platformy Touch dodanie zewnętrznej biblioteki lub klas wymaga jedynie pobrania pakietu, rozpakowania go w katalogu aplikacji i skonfigurowania w module ładującym Touch lokalizacji biblioteki.

Rozszerzenia Azure można dodać ręcznie do aplikacji, wykonując następujące czynności:

1. Pobierz [stąd](https://market.sencha.com/extensions/sencha-extensions-for-microsoft-azure) pakiet rozszerzeń Azure. (W celu uzyskania dostępu do tego obszaru można użyć identyfikatora forów Sencha).

2. Skopiuj pakiet rozszerzeń Azure z katalogu pobierania do miejsca, w którym ma się ostatecznie znaleźć, i rozpakuj go:

        $ cd /path/to/application
        $ mv /download-location/azure.zip .
        $ unzip azure.zip

    Spowoduje to utworzenie katalogu **azure** zawierającego pełny pakiet źródłowy, przykłady i dokumentację. Źródła będą znajdować się w katalogu **azure/src**.


###Instalacja pakietu Sencha

> [AZURE.NOTE] Tej metody można używać tylko po wygenerowaniu swojej aplikacji za pomocą polecenia <code>sencha generate app</code>.

Wszystkie aplikacje generowane przez narzędzie Sencha Cmd mają jako katalog główny folder „packages”. Można skonfigurować lokalizację tego folderu, ale niezależnie od jego lokalizacji rolą folderu „packages” jest służenie jako magazyn wszystkich pakietów używane przez aplikację (lub aplikacje, jeśli utworzono obszar roboczy Sencha).

Ponieważ Ext.Azure jest „pakietem” narzędzia Sencha Cmd, można łatwo zainstalować kod źródłowy w aplikacji korzystając z narzędzia Sencha Cmd. (Aby uzyskać więcej informacji, zobacz [Pakiety narzędzia Sencha Cmd](http://docs.sencha.com/cmd/6.x/cmd_packages/cmd_packages.html)).

Aby pobrać i zainstalować pakiet rozszerzeń Azure z repozytorium pakietów Sencha, należy dodać nazwę pakietu do swojego pliku **app.json** i skompilować aplikację:

1. Dodaj pakiet Azure do odpowiedniej sekcji pliku app.json:

        {
            "name": "Basic",
            "requires": [
                "touch-azure"
            ]
        }

2. Skompiluj swoją aplikację za pomocą narzędzia **sencha cmd**, aby pobrać i zainstalować pakiet:

        $ sencha app build

Zarówno polecenie **sencha app build** jak i **sencha app refresh** spowodują wykonanie czynności potrzebnych do zintegrowania pakietu w aplikacji. Zwykle po zmianie wymagań dotyczących pakietu konieczne będzie uruchomienie polecenia **sencha app refresh**, tak aby uaktualnić metadane wymagane do obsługi trybu „dev mode”.

Niezależnie od uruchamianego polecenia, narzędzie Sencha Cmd pobierze i rozpakuje pakiet do folderu „packages”. Po tej operacji w swoim obszarze roboczym znajdziesz folder „packages/touch-azure”.

##Dołączanie i konfigurowanie usługi Azure

**Nazwa pliku**: app.js

Teraz, gdy rozszerzenie Azure zostało pobrane i zainstalowane w katalogu aplikacji, następnym krokiem jest poinformowanie aplikacji o miejscu przechowywania plików źródłowych i zażądanie tych plików:

1. Skonfiguruj program Sencha Loader, podając lokalizację kodu źródłowego:

        Ext.Loader.setConfig({
            enabled : true,
            paths   : {
                'Ext'       : 'touch/src',
                'Ext.azure' : '/path-to/azure-for-touch/azure/src'
            }
        });


2. Wymagane pliki klas Azure:

        Ext.application({

            requires: [ 'Ext.azure.Azure' ],

            // ...

        });


3. Konfigurowanie usługi Azure

    Pakiet Azure jest inicjowany przez wywołanie metody **Ext.Azure.init** w sekcji uruchamiania aplikacji. Do tej metody jest przekazywany obiekt konfiguracji zawierający poświadczenia usługi mobilnej, jak również inne poświadczenia i funkcje, które chcesz wykorzystać.

    Obiekt konfiguracji można przekazać bezpośrednio do metody init, jednak zaleca się utworzenie właściwości konfiguracji aplikacji Sencha o nazwie **azure** i umieszczenie w niej wszystkich odpowiednich informacji. Wartość tej właściwości można następnie przekazać do metody Ext.Azure.init.

    Po utworzeniu usługi mobilnej Azure (zobacz [Wprowadzenie do usługi Azure](http://senchaazuredocs.azurewebsites.net/#!/guide/getting_started)) zostaje do niej przypisany klucz aplikacji i adres URL. Należy przekazać te informacje do pakietu Azure, aby mógł połączyć się z usługą.

    Ten przykład przedstawia bardzo prostą konfigurację i inicjalizację platformy Azure, zawierającą tylko klucz aplikacji i adres URL:

        Ext.application({
            name: 'Basic',

            requires: [ 'Ext.azure.Azure' ],

            azure: {
                appKey: 'myazureservice-access-key',
                appUrl: 'myazure-service.azure-mobile.net'
            },

            launch: function() {

                // Call Azure initialization

                Ext.Azure.init(this.config.azure);

           }
        });

    Aby uzyskać więcej informacji na temat opcji konfiguracji platformy Azure, zapoznaj się z dokumentacją interfejsu API Ext.Azure.


Gratulacje! Aplikacja powinna mieć teraz dostęp do usługi mobilnej.

##Tworzenie aplikacji ToDo

Po skonfigurowaniu aplikacji zawierającej rozszerzenie Azure i dostarczeniu jej wraz z poświadczeniami swojej usługi mobilnej, możesz teraz przystąpić do utworzenia aplikacji Touch, która wykorzystuje usługę mobilną do wyświetlania i edytowania danych listy ToDo przechowywanych w usłudze.

###Konfigurowanie serwera danych proxy Azure

**Nazwa pliku**: app/model/TodoItem.js

Aplikacja Touch będzie się komunikować z usługą mobilną za pośrednictwem serwera proxy danych. Serwer proxy wykonuje całą pracę, zarówno wysyłania żądań do usługi mobilnej, jak i odbieranie od niej danych. Dzięki połączeniu z modelem danych i magazynem aplikacji Touch, cała praca wymagana do przetwarzania zdalnych danych i pobierania ich do aplikacji jest wykonywana przez samą aplikację Touch.

Modele platformy Sencha Touch udostępniają definicje rekordów danych, które będą wykorzystywane w aplikacji. Rekordy nie tylko definiują pola danych, ale udostępniają konfigurację serwera proxy, który będzie obsługiwał komunikację pomiędzy aplikacją i usługą Azure Mobile Services.

W poniższym kodzie można znaleźć definicję pól modelu (oraz ich typy), jak również konfigurację serwera proxy. Podczas konfigurowania serwera proxy należy podać mu typ (w tym przypadku „azure”), nazwę tabeli usługi mobilnej (ToDoItem) i inne parametry opcjonalne. W tym przykładzie na serwerze proxy zostanie włączone stronicowanie, dzięki czemu będzie można poruszać się bez zakłóceń po liście w obu kierunkach.

Serwer proxy Azure automatycznie ustawi za pomocą odpowiednich operacji CRUD wszystkie nagłówki HTTP oczekiwane przez interfejs API Azure (w tym poświadczenia z uwierzytelnieniami, jeśli takie istnieją).

    Ext.define('Basic.model.TodoItem', {
        extend : 'Ext.data.Model',

        requires : [
            'Ext.azure.Proxy'
        ],

        config : {
            idProperty : 'id',
            useCache   : false,

            fields     : [
                {
                    name : 'id',
                    type : 'int'
                },
                {
                    name : 'text',
                    type : 'string'
                },
                {
                    name : 'complete',
                    type : 'boolean'
                }
            ],

            proxy : {
                type               : 'azure',
                tableName          : 'TodoItem',
                enablePagingParams : true
            }
        }
    });


###Przechowywanie zadań do wykonania

**Nazwa pliku**: app/store/TodoItems.js

Magazyny Sencha Touch służą do przechowywania kolekcji rekordów danych (modeli), które mogą służyć jako źródła dla składników do wyświetlania rekordów na wiele różnych sposobów. Są wśród nich siatki, wykresy, listy itp.

W tym miejscu definiujemy magazyn, który będzie używany do przechowywania wszystkich elementów listy ToDo, które są pobierane z magazynu usługi mobilnej Azure. Należy zauważyć, że konfiguracja magazynu zawiera nazwę typu modelu (Basic.model.TodoItem - zdefiniowany powyżej). Definiuje to strukturę rekordów, które będą przechowywane w magazynie.

Istnieje kilka dodatkowych opcji konfiguracji magazynu, takich jak określanie rozmiaru strony (8 rekordów), zaś sortowanie rekordów dla tego magazynu jest wykonywane zdalnie przez mobilną usługę Azure (sortowania nie jest wykonywane lokalnie w obrębie samego magazynu).

    Ext.define('Basic.store.TodoItems', {
        extend : 'Ext.data.Store',

        requires : [
            'Basic.model.TodoItem'
        ],

        config : {
            model        : 'Basic.model.TodoItem',
            pageSize     : 8,
            remoteSort   : true,
            remoteFilter : true
        }
    });


###Wyświetlanie i edytowanie elementów listy ToDo

**Nazwa pliku**: app/view/DataItem.js

Teraz, kiedy zdefiniowano strukturę każdego elementu ToDo i utworzono magazyn dla wszystkich rekordów, powinniśmy pomyśleć o sposobie wyświetlania tych informacji użytkownikowi aplikacji. Zwykle informacje użytkownika są wyświetlane w **widokach**. Widok może być dowolnym składnikiem aplikacji Touch, samodzielnym lub połączonym z innymi składnikami.

Poniższy widok składa się z elementu ListItem, który definiuje sposób wyświetlania każdego rekordu wraz z pewnymi przyciskami, które umożliwią usunięcia każdego elementu.

    Ext.define('Basic.view.DataItem', {
        extend : 'Ext.dataview.component.ListItem',
        xtype  : 'basic-dataitem',

        requires : [
            'Ext.Button',
            'Ext.layout.HBox',
            'Ext.field.Checkbox'
        ],

        config : {
            checkbox : {
                docked     : 'left',
                xtype      : 'checkboxfield',
                width      : 50,
                labelWidth : 0
            },

            text : {
                flex : 1
            },

            button : {
                docked   : 'right',
                xtype    : 'button',
                ui       : 'plain',
                iconMask : true,
                iconCls  : 'delete',
                style    : 'color: red;'
            },

            dataMap : {
                getText : {
                    setHtml : 'text'
                },

                getCheckbox : {
                    setChecked : 'complete'
                }
            },

            layout : {
                type : 'hbox',
                align: 'stretch'
            }
        },

        applyCheckbox : function(config) {
            return Ext.factory(config, Ext.field.Checkbox, this.getCheckbox());
        },

        updateCheckbox : function (cmp) {
            if (cmp) {
                this.add(cmp);
            }
        },

        applyButton : function(config) {
            return Ext.factory(config, Ext.Button, this.getButton());
        },

        updateButton : function (cmp) {
            if (cmp) {
                this.add(cmp);
            }
        }

    });


###Tworzenie widoku głównego

**Nazwa pliku**: app/view/Main.js

Teraz, po zdefiniowaniu układu poszczególnych elementów listy ToDo (p. wyżej), chcemy obudować tę listę pełnym interfejsem użytkownika, który definiuje właściwą listę elementów, tytuł aplikacji oraz przycisk służący do dodawania nowego zadania.

    Ext.define('Basic.view.Main', {
        extend : 'Ext.dataview.List',
        xtype  : 'main',

        requires : [
            'Ext.TitleBar',
            'Ext.dataview.List',
            'Ext.data.Store',
            'Ext.plugin.PullRefresh',
            'Ext.plugin.ListPaging',
            'Basic.view.DataItem'
        ],

        config : {
            store : 'TodoItems',

            useSimpleItems : false,
            defaultType    : 'basic-dataitem',

            plugins : [
                {
                    xclass          : 'Ext.plugin.PullRefresh',
                    pullRefreshText : 'Pull down to refresh!'
                },
                {
                    xclass     : 'Ext.plugin.ListPaging',
                    autoPaging : true
                }
            ],

            scrollable : {
                direction     : 'vertical',
                directionLock : true
            },

            items : [
                {
                    docked : 'top',
                    xtype  : 'titlebar',
                    title  : 'Azure Mobile - Basic Data Example'
                },
                {
                    xtype  : 'toolbar',
                    docked : 'bottom',
                    items  : [
                        {
                            xtype       : 'textfield',
                            placeHolder : 'Enter new task',
                            flex        : 1
                        },
                        {
                            xtype  : 'button',
                            action : 'add',
                            text   : 'Add'
                        }
                    ]
                }
            ]
        }
    });

###Łączenie wszystkich elementów

**Nazwa pliku**: app/controller/Main.js

Ostatnim krokiem w naszej aplikacji jest reakcja na naciśnięcie przycisków (usuwanie, zapisywanie itp.) i dodanie logiki stojącej za tymi żądaniami. Platforma Sencha Touch wykorzystuje kontrolery, które oczekują na te zdarzenia i odpowiednio reagują.

    Ext.define('Basic.controller.Main', {
        extend : 'Ext.app.Controller',

        config : {
            refs : {
                todoField : 'main toolbar textfield',
                main      : 'main'
            },

            control : {
                'button[action=add]'    : {
                    tap : 'onAddItem'
                },
                'button[action=reload]' : {
                    tap : 'onReload'
                },

                main : {
                    activate      : 'loadInitialData',
                    itemdoubletap : 'onItemEdit'
                },

                'basic-dataitem checkboxfield' : {
                    change : 'onItemCompleteTap'
                },

                'basic-dataitem button' : {
                    tap : 'onItemDeleteTap'
                }
            }
        },

        loadInitialData : function () {
            Ext.getStore('TodoItems').load();
        },

        onItemDeleteTap : function (button, e, eOpts) {
            var store    = Ext.getStore('TodoItems'),
                dataItem = button.up('dataitem'),
                rec      = dataItem.getRecord();

            rec.erase({
                success: function (rec, operation) {
                    store.remove(rec);
                },
                failure: function (rec, operation) {
                    Ext.Msg.alert(
                        'Error',
                        Ext.util.Format.format('There was an error deleting this task.<br/><br/>    Status Code: {0}<br/>Status Text: {1}',
                        operation.error.status,
                        operation.error.statusText)
                    );
                }
            });
        },

        onItemCompleteTap : function (checkbox, newVal, oldVal, eOpts) {
            var dataItem = checkbox.up('dataitem'),
                rec      = dataItem.getRecord(),
                recVal   = rec.get('complete');

            // this check is needed to prevent an issue where multiple creates get triggered from one create
            if (newVal !== recVal) {
                rec.set('complete', newVal);
                rec.save({
                    success: function (rec, operation) {
                        rec.commit();
                    },
                    failure: function (rec, operation) {
                        // since there was a failure doing the update on the server then silently reject the change
                        rec.reject(true);
                        Ext.Msg.alert(
                            'Error',
                            Ext.util.Format.format('There was an error updating this task.<br/><br/>Status Code: {0}<br/>Status Text: {1}',
                            operation.error.status,
                            operation.error.statusText)
                        );
                    }
                });
            }
        },

        onItemEdit : function (list, index, target, record, e, eOpts) {
            var rec = list.getSelection()[0];

            Ext.Msg.prompt('Edit', 'Rename task',
                function (buttonId, value) {
                    if (buttonId === 'ok') {
                        rec.set('text', value);
                        rec.save({
                            success: function (rec, operation) {
                                rec.commit();
                            },
                            failure: function (rec, operation) {
                                // since there was a failure doing the update on the server then reject the change
                                rec.reject();
                                Ext.Msg.alert(
                                    'Error',
                                    Ext.util.Format.format('There was an error updating this task.<br/><br/>Status Code: {0}<br/>Status Text: {1}',
                                    operation.error.status,
                                    operation.error.statusText)
                                );
                            }
                        });
                    }
                },
                null,
                false,
                record.get('text')
            );
        },

        onReload : function () {
            Ext.getStore('TodoItems').load();
        },

        onAddItem : function () {
            var me = this,
                rec,
                store = Ext.getStore('TodoItems'),
                field = me.getTodoField(),
                value = field.getValue();

            if (value === '') {
                Ext.Msg.alert('Error', 'Please enter Task name', Ext.emptyFn);
            }
            else {
                rec = Ext.create('Basic.model.TodoItem', {
                    complete : false,
                    text     : value
                });
                //store.insert(0, rec); //insert at the top
                //store.sync();
                rec.save({
                    success: function (rec, operation) {
                        store.insert(0, rec); //insert at the top
                        field.setValue('');
                    },
                    failure: function (rec, operation) {
                        Ext.Msg.alert(
                            'Error',
                            Ext.util.Format.format('There was an error creating this task.<br/><br/>Status Code: {0}<br/>Status Text: {1}',
                            operation.error.status,
                            operation.error.statusText)
                        );
                    }
                });
            }
        }
    });

###Zebranie wszystkich elementów

**Nazwa pliku**: app.js

Ostatnią czynnością jest zakończenie edytowania głównego pliku aplikacji oraz podanie informacji o zdefiniowanych modelach, magazynach, widokach i kontrolerach. Pliki źródłowe dla tych zasobów są ładowane automatycznie do aplikacji. Na koniec jest wywoływana metoda uruchamiania, która tworzy i wyświetla widok głównej aplikacji „Basic.main.View”.


    Ext.Loader.setConfig({
        enabled : true,
        paths   : {
            'Ext'       : 'touch/src',
            'Ext.azure' : 'packages/azure/src'
        }
    });

    Ext.application({
        name : 'Basic',

        requires : [
            'Ext.MessageBox',
            'Ext.azure.Azure'
        ],

        views : [
            'Main'
        ],

        controllers : [
            'Main'
        ],

        stores : [
            'TodoItems'
        ],

        azure : {
            appUrl : 'YOUR_APP_URL.azure-mobile.net',
            appKey : 'YOUR_APP_KEY'
        },

        icon : {
            '57'  : 'resources/icons/Icon.png',
            '72'  : 'resources/icons/Icon~ipad.png',
            '114' : 'resources/icons/Icon@2x.png',
            '144' : 'resources/icons/Icon~ipad@2x.png'
        },

        isIconPrecomposed : true,

        startupImage : {
            '320x460'   : 'resources/startup/320x460.jpg',
            '640x920'   : 'resources/startup/640x920.png',
            '768x1004'  : 'resources/startup/768x1004.png',
            '748x1024'  : 'resources/startup/748x1024.png',
            '1536x2008' : 'resources/startup/1536x2008.png',
            '1496x2048' : 'resources/startup/1496x2048.png'
        },

        launch : function () {
            // Destroy the #appLoadingIndicator element
            Ext.fly('appLoadingIndicator').destroy();

            // Initialize Azure
            Ext.Azure.init(this.config.azure);

            // Initialize the main view
            Ext.Viewport.add(Ext.create('Basic.view.Main'));
        },

        onUpdated : function () {
            Ext.Msg.confirm(
                "Application Update",
                "This application has just successfully been updated to the latest version. Reload now?",
                function (buttonId) {
                    if (buttonId === 'yes') {
                        window.location.reload();
                    }
                }
            );
        }
    });

###Hostowanie i uruchamianie aplikacji Sencha Touch

Ostatnim krokiem tego samouczka jest hostowanie i uruchomienie nowej aplikacji na komputerze lokalnym.

  1. W terminalu przejdź do lokalizacji rozpakowanej aplikacji.

  2. Używając narzędzia Sencha Cmd, uruchom następujące polecenie:

    * *sencha app refresh* : spowoduje to wydanie polecenia narzędzia Sencha Cmd lokalizującego wszystkie zależności aplikacji i pobranie wszystkich wymaganych pakietów (na przykład [rozszerzeń Sencha Touch dla platformy Azure](https://market.sencha.com/extensions/sencha-extensions-for-microsoft-azure)).

    * *sencha web start*: spowoduje uruchomienie lokalnego serwera sieci Web służącego do testowania aplikacji.

    ![sencha web start](./media/partner-sencha-mobile-services-get-started/sencha-web-start.png)

  3. W przeglądarce otwórz adres URL wyświetlany na terminalu, aby uruchomić aplikację (np. http://localhost:1841).

  4. W aplikacji wpisz łatwy do rozpoznania tekst, na przykład „Ukończ samouczek”, a następnie kliknij pozycję **Dodaj**.

    ![nowe zadanie do wykonania](./media/partner-sencha-mobile-services-get-started/new-todo-item.png)

    Spowoduje to wysłanie żądania POST do nowej usługi mobilnej, która jest hostowana na platformie Azure. Dane z żądania zostaną wstawione do tabeli TodoItem.

  5. W [klasyczny portal Azure] kliknij kartę **Dane**, a następnie kliknij tabelę TodoItems.

    ![Tabela zadań to wykonania (Todo)](./media/partner-sencha-mobile-services-get-started/mobile-data-tab.png)

    Pozwoli to przeglądać dane wstawiane przez aplikację do tabeli.

    ![przeglądaj tabelę zadań do wykonania](./media/partner-sencha-mobile-services-get-started/mobile-data-browse.png)

##Następne kroki
Po zakończeniu procedury szybkiego startu dowiedz się, jak wykonać dodatkowe ważne zadania w usłudze Mobile Services przy użyciu programu Sencha:

[Pobierz](https://github.com/arthurakay/sencha-touch-azure-example) pełną aplikację przykładową z dodatkowymi stylami i funkcjami, aby zobaczyć, co może zrobić platforma Sencha Touch!

Następnie dowiedz się więcej na temat rozszerzeń Touch Sencha dla platformy Azure:

  * [Omówienie](http://docs.sencha.com/touch-azure/1.0.0/#!/guide/data_filters) przykładowej aplikacji
  * Uzyskaj pomoc na [forach Sencha](http://www.sencha.com/forum)
  * Przeglądaj [dokumentację oprogramowania Sencha](http://docs.sencha.com/)
  * Korzystanie z oprogramowania Sencha i usług Azure Mobile Services: [(wideo)](http://channel9.msdn.com/Shows/Cloud+Cover/Episode-126-Using-Sencha-With-Windows-Azure-Mobile-Services)


##Dodatkowe zasoby

  * [Pobierz platformę Sencha Touch](http://pages.sencha.com/touch-for-azure.html)
  * [Rozszerzenia Sencha Touch dla platformy Azure](https://market.sencha.com/extensions/sencha-extensions-for-microsoft-azure)


##Podsumowanie

Opisany tutaj przykład znajduje się w rozszerzeniu Touch Sencha dla platformy Azure i jest zlokalizowany w katalogu przykładów jako przykład Basic Data. Istnieje kilka innych przykładów przedstawiających inne funkcje tego rozszerzenia, a także zawierających szczegółowe komentarze i wyjaśnienia.

Więcej informacji na temat rozpoczynania pracy z platformą Sencha Touch można znaleźć w pełnym zestawie [przewodników](http://docs.sencha.com/touch/#!/guide)


[AZURE.INCLUDE [app-service-disqus-feedback-slug](../../includes/app-service-disqus-feedback-slug.md)]

<!-- images -->
[0]: ./media/partner-sencha-mobile-services-get-started/finished-app.png

[klasyczny portal Azure]: https://manage.windowsazure.com/


<!--HONumber=sep16_HO1-->


