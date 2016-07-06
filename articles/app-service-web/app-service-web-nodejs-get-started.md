<properties
    pageTitle="Rozpoczynanie pracy z aplikacjami sieci Web Node.js w usłudze Azure App Service"
    description="Dowiedz się, jak wdrożyć aplikację Node.js w aplikacji sieci Web w usłudze Azure App Service."
    services="app-service\web"
    documentationCenter="nodejs"
    authors="cephalin"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="nodejs"
    ms.topic="get-started-article"
    ms.date="06/01/2016"
    ms.author="cephalin"/>

# Rozpoczynanie pracy z aplikacjami sieci Web Node.js w usłudze Azure App Service

[AZURE.INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

W tym samouczku przedstawiono sposób tworzenia prostej aplikacji [Node.js][NODEJS] i wdrażania jej w [aplikacji sieci Web] w [usłudze Azure App Service] przy użyciu środowiska wiersza polecenia, takiego jak cmd.exe lub bash. Instrukcje podane w tym samouczku można wykonać w dowolnym systemie operacyjnym, w którym można uruchomić środowisko Node.js.

<a name="prereq"></a>
## Wymagania wstępne

- **Node.js** ([Kliknij tutaj, aby zainstalować][NODEJS])
- **Bower** ([Kliknij tutaj, aby zainstalować][BOWER])
- **Yeoman** ([Kliknij tutaj, aby zainstalować][YEOMAN])
- **Git** ([Kliknij tutaj, aby zainstalować][GIT])
- **Azure CLI** ([Kliknij tutaj, aby zainstalować ][Interfejs wiersza polecenia platformy Azure])
- Konto platformy Microsoft Azure. Jeśli nie masz konta, możesz [utworzyć konto bezpłatnej wersji próbnej] lub [aktywować korzyści dla subskrybentów programu Visual Studio].

## Tworzenie i wdrażanie prostej aplikacji sieci Web Node.js

1. Otwórz wybrany terminal wiersza polecenia i zainstaluj [Generator aplikacji Express dla narzędzia Yeoman].

        npm install -g generator-express

2. `CD` — za pomocą tej komendy przejdź do katalogu roboczego i wygeneruj aplikację Express przy użyciu następującej składni:

        yo express
        
    Po wyświetleniu monitu wybierz następujące opcje:  

    `? Would you like to create a new directory for your project?` **Yes**  
    `? Enter directory name` **{nazwa_aplikacji}**  
    `? Select a version to install:` **MVC**  
    `? Select a view engine to use:` **Jade**  
    `? Select a css preprocessor to use (Sass Requires Ruby):` **None**  
    `? Select a database to use:` **None**  
    `? Select a build tool to use:` **Grunt**

3. `CD` — za pomocą tej komendy przejdź do katalogu głównego nowej aplikacji i uruchom ją, aby upewnić się, że jest uruchamiana w środowisku projektowania:

        npm start

    W przeglądarce przejdź do strony <http://localhost:3000>, aby upewnić się, że strona główna aplikacji Express jest wyświetlana. Po upewnieniu się, że aplikacja działa prawidłowo, zatrzymaj ją przy użyciu skrótu klawiaturowego `Ctrl-C`.
    
1. Zaloguj się do platformy Azure w następujący sposób (do wykonania tych czynności jest wymagany [interfejs wiersza polecenia platformy Azure](#prereq)):

        azure login

    Postępuj zgodnie z wyświetlanymi instrukcjami, aby kontynuować logowanie w przeglądarce za pomocą konta Microsoft zawierającego subskrypcję Azure.

2. Upewnij się, że jesteś nadal w katalogu głównym aplikacji, a następnie utwórz zasób aplikacji usługi App Service na platformie Azure przy użyciu unikatowej nazwy aplikacji za pomocą poniższego polecenia. Na przykład http://{nazwa_aplikacji}.azurewebsites.net

        azure site create --git {appname}

    Postępuj zgodnie z wyświetlanymi instrukcjami, aby wybrać region platformy Azure, w którym aplikacja ma zostać wdrożona. Jeśli nigdy nie skonfigurowano poświadczeń wdrożenia Git/FTP dla subskrypcji platformy Azure, zostanie również wyświetlony monit o ich utworzenie.

3. Otwórz plik ./config/config.js z katalogu głównego aplikacji i zmień port produkcyjny na `process.env.port`. Produkcyjny obiekt JSON powinien wyglądać tak, jak w następującym przykładzie. Jeśli ta ścieżka folderu/pliku nie istnieje w katalogu głównym aplikacji, utwórz folder o nazwie **config** w katalogu głównym aplikacji i skopiuj do niego następujący kod JSON .

        production: {
            root: rootPath,
            app: {
                name: 'express1'
            },
            port: process.env.port,
        }

    Umożliwia to aplikacji Node.js odpowiadanie na żądania sieci Web przy użyciu domyślnego portu, na którym nasłuchuje program iisnode.
    
4. Zapisz zmiany, a następnie użyj narzędzia git, aby wdrożyć aplikację na platformie Azure:

        git add .
        git commit -m "{your commit message}"
        git push azure master

    Generator aplikacji Express zapewnia plik gitignore, dlatego polecenie `git push` nie spowoduje zmniejszenia przepustowości przez próbę przekazania katalogu node_modules/.

5. Na koniec uruchom działającą aplikację Azure w przeglądarce:

        azure site browse

    Powinna zostać wyświetlona aplikacja sieci Web Node.js działająca w usłudze Azure App Service.
    
    ![Przykład wyświetlania wdrożonej aplikacji.][deployed-express-app]

## Aktualizowanie aplikacji sieci Web Node.js

Aby zaktualizować aplikację sieci Web Node.js działającą w usłudze App Service, uruchom polecenia `git add`, `git commit` i `git push`, jak podczas pierwszego wdrożenia aplikacji sieci Web.
     
## Sposób wdrażania aplikacji Node.js przez usługę App Service

Usługa Azure App Service uruchamia aplikacje Node.js przy użyciu programu [iisnode]. Interfejs wiersza polecenia platformy Azure oraz aparat Kudu (wdrożenie Git) współdziałają ze sobą w celu zapewnienia usprawnionego środowiska podczas tworzenia i wdrażania aplikacji Node.js z poziomu wiersza polecenia. 

- `azure site create --git` rozpoznaje wspólny wzorzec plików server.js i app.js środowiska Node.js i tworzy plik iisnode.yml w katalogu głównym. Ten plik służy do dostosowywania programu iisnode.
- Po uruchomieniu polecenia `git push azure master` aparat Kudu automatyzuje następujące zadania wdrażania:

    - Jeśli plik package.json znajduje się w katalogu głównym repozytorium, uruchom polecenie `npm install --production`.
    - Wygeneruj plik Web.config dla programu iisnode wskazujący skrypt początkowy w pliku package.json (np. server.js lub app.js).
    - Dostosuj plik Web.config, aby przygotować aplikację do debugowania przy użyciu narzędzia Node-Inspector.
    
## Korzystanie ze środowiska Node.js

Jeśli tworzysz aplikacje przy użyciu popularnego środowiska Node.js, takiego jak [Sails.js][SAILSJS] lub [MEAN.js][MEANJS], możesz je wdrożyć w usłudze App Service. Popularne środowiska Node.js mają swoje osobliwości, a ich zależności pakietów są stale aktualizowane. Jednak usługa App Service udostępnia dzienniki stdout i stderr, dlatego możesz uzyskać dokładne informacje dotyczące działania aplikacji i wprowadzić odpowiednie zmiany. Aby uzyskać więcej informacji, zobacz [Pobieranie dzienników stdout i stderr z programu iisnode](#iisnodelog).

W następujących samouczkach przedstawiono sposób pracy z określonym środowiskiem w usłudze App Service:

- [Wdrażanie aplikacji sieci Web Sails.js przy użyciu usługi Azure App Service]
- [Tworzenie aplikacji czatu Node.js przy użyciu biblioteki Socket.IO w usłudze Azure App Service]
- [Sposób użycia platformy io.js z aplikacjami Web Apps w usłudze Azure App Service]

## Korzystanie z określonego aparatu Node.js

W typowym przepływie pracy można określić, aby usługa App Service korzystała z konkretnego aparatu Node.js, tak jak zwykle w pliku package.json.
Na przykład:

    "engines": {
        "node": "5.5.0"
    }, 

Aparat wdrażania Kudu określa, który aparat Node.js ma zostać użyty, w następującej kolejności:

- Najpierw sprawdź, czy w pliku określono element `nodeProcessCommandLine`. Jeśli tak, użyj tego elementu.
- Następnie sprawdź, czy w pliku package.json określono element `"node": "..."` w obiekcie `engines`. Jeśli tak, użyj tego elementu.
- Wybierz domyślną wersję środowiska Node.js.

<a name="iisnodelog"></a>
## Pobieranie dzienników stdout i stderr z programu iisnode

Aby odczytać dzienniki programu iisnode, wykonaj następujące czynności.

> [AZURE.NOTE] Po wykonaniu tych czynności pliki dzienników mogą nie istnieć do momentu wystąpienia błędu.

1. Otwórz plik iisnode.yml udostępniany przez interfejs wiersza polecenia platformy Azure.

2. Ustaw następujące dwa parametry: 

        loggingEnabled: true
        logDirectory: iisnode
    
    Razem informują one program iisnode w usłudze App Service, aby dane wyjściowe stdout i stderr były umieszczane w katalogu D:\home\site\wwwroot\**iisnode**.

3. Zapisz zmiany, a następnie wypchnij je do platformy Azure przy użyciu następujących poleceń Git:

        git add .
        git commit -m "{your commit message}"
        git push azure master
   
   Program iisnode jest teraz skonfigurowany. W następnych krokach przedstawiono sposób uzyskiwania dostępu do tych dzienników.
     
4. W przeglądarce przejdź do konsoli debugowania aparatu Kudu dla aplikacji, która znajduje się pod adresem:

        https://{appname}.scm.azurewebsites.net/DebugConsole 

    Zauważ, że ten adres URL różni się od adresu URL aplikacji ciągiem „ *.scm.* ” dodanym do nazwy DNS. W przypadku pominięcia tego ciągu w adresie URL wystąpi błąd 404.

5. Przejdź do katalogu D:\home\site\wwwroot\iisnode.

    ![Przechodzenie do lokalizacji plików dziennika programu iisnode.][iislog-kudu-console-find]

6. Kliknij ikonę **Edytuj** dla dziennika, który chcesz odczytać. Jeśli chcesz, możesz również kliknąć ikonę **Pobierz** lub **Usuń**.

    ![Otwieranie pliku dziennika programu iisnode][iislog-kudu-console-open]

    Spowoduje to wyświetlenie dziennika w celu ułatwienia debugowania wdrożenia usługi App Service.
    
    ![Sprawdzanie pliku dziennika programu iisnode][iislog-kudu-console-read]

## Debugowanie aplikacji przy użyciu narzędzia Node-Inspector

Jeśli do debugowania aplikacji Node.js używasz narzędzia Node-Inspector, możesz go użyć, aby debugować aplikację usługi App Service. Narzędzie Node-Inspector jest wstępnie zainstalowane w instalacji programu iisnode dla usługi App Service. W przypadku wdrożenia przy użyciu programu Git plik Web.config automatycznie wygenerowany przez aparat Kudu zawiera już całą konfigurację niezbędną do włączenia narzędzia Node-Inspector.

Aby włączyć narzędzie Node-Inspector, wykonaj następujące czynności:

1. Otwórz plik iisnode.yml w katalogu głównym repozytorium i określ następujące parametry: 

        debuggingEnabled: true
        debuggerExtensionDll: iisnode-inspector.dll

3. Zapisz zmiany, a następnie wypchnij je do platformy Azure przy użyciu następujących poleceń Git:

        git add .
        git commit -m "{your commit message}"
        git push azure master
   
4. Teraz przejdź do pliku uruchamiania aplikacji określonego w skrypcie uruchamiana w pliku package.json, dodając ciąg „/debug” do adresu URL. Na przykład:

        http://{appname}.azurewebsites.net/server.js/debug
    
    Lub:
    
        http://{appname}.azurewebsites.net/app.js/debug

## Więcej zasobów

- [Specifying a Node.js version in an Azure application (Określanie wersji środowiska Node.js w aplikacji Azure)](../nodejs-specify-node-version-azure-apps.md)
- [How to debug a Node.js web app in Azure App Service (Jak debugować aplikację sieci Web Node.js w usłudze Azure App Service)](web-sites-nodejs-debug.md)
- [Using Node.js Modules with Azure applications (Używanie modułów Node.js z aplikacjami platformy Azure)](../nodejs-use-node-modules-azure-apps.md)
- [Azure App Service Web Apps: Node.js (Aplikacje sieci Web w usłudze Azure App Service: Node.js)](http://blogs.msdn.com/b/silverlining/archive/2012/06/14/windows-azure-websites-node-js.aspx)
- [Node.js Developer Center (Centrum deweloperów środowiska Node.js)](/develop/nodejs/)
- [Rozpoczynanie pracy z aplikacjami sieci Web w usłudze Azure App Service](app-service-web-get-started.md)
- [Exploring the Super Secret Kudu Debug Console (Szczegółowe informacje o ściśle tajnej konsoli debugowania aparatu Kudu)]

<!-- URL List -->

[Interfejs wiersza polecenia platformy Azure]: ../xplat-cli-install.md
[Usługa Azure App Service]: ../app-service/app-service-value-prop-what-is.md
[Aktywuj swoje korzyści subskrybenta programu Visual Studio]: http://go.microsoft.com/fwlink/?LinkId=623901
[BOWER]: http://bower.io/
[Tworzenie aplikacji czatu Node.js przy użyciu biblioteki Socket.IO w usłudze Azure App Service]: ./web-sites-nodejs-chat-app-socketio.md
[Wdrażanie aplikacji sieci Web Sails.js przy użyciu usługi Azure App Service]: ./app-service-web-nodejs-sails.md
[Exploring the Super Secret Kudu Debug Console (Szczegółowe informacje o ściśle tajnej konsoli debugowania aparatu Kudu)]: /documentation/videos/super-secret-kudu-debug-console-for-azure-web-sites/
[Generator aplikacji Express dla narzędzia Yeoman]: https://github.com/petecoop/generator-express
[GIT]: http://www.git-scm.com/downloads
[Sposób użycia platformy io.js z aplikacjami Web Apps w usłudze Azure App Service]: ./web-sites-nodejs-iojs.md
[iisnode]: https://github.com/tjanczuk/iisnode/wiki
[MEANJS]: http://meanjs.org/
[NODEJS]: http://nodejs.org
[SAILSJS]: http://sailsjs.org/
[utworzyć konto bezpłatnej wersji próbnej]: http://go.microsoft.com/fwlink/?LinkId=623901
[aplikacji sieci Web]: ./app-service-web-overview.md
[YEOMAN]: http://yeoman.io/

<!-- IMG List -->

[deployed-express-app]: ./media/app-service-web-nodejs-get-started/deployed-express-app.png
[iislog-kudu-console-find]: ./media/app-service-web-nodejs-get-started/iislog-kudu-console-navigate.png
[iislog-kudu-console-open]: ./media/app-service-web-nodejs-get-started/iislog-kudu-console-open.png
[iislog-kudu-console-read]: ./media/app-service-web-nodejs-get-started/iislog-kudu-console-read.png



<!---HONumber=Jun16_HO2-->


