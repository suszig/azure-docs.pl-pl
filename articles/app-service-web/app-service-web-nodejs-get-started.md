---
title: "Tworzenie aplikacji sieci Web Node.js dla usługi Azure App Service | Microsoft Docs"
description: "Dowiedz się, jak wdrożyć aplikację Node.js w aplikacji sieci Web w usłudze Azure App Service."
services: app-service\web
documentationcenter: nodejs
author: cephalin
manager: erikre
editor: 
ms.assetid: fb2b90c8-02b6-4700-929b-5de9a35d67cc
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: get-started-article
ms.date: 12/16/2016
ms.author: cephalin
translationtype: Human Translation
ms.sourcegitcommit: 0921b01bc930f633f39aba07b7899ad60bd6a234
ms.openlocfilehash: 3405fa7bf960ff1ba4b9f789e5e89f3f75fe96d4
ms.lasthandoff: 02/28/2017


---
# <a name="develop-nodejs-web-apps-for-azure-app-service"></a>Tworzenie aplikacji sieci Web Node.js dla usługi Azure App Service
[!INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

W tym samouczku przedstawiono sposób tworzenia prostej aplikacji [Node.js] i wdrażania jej w usłudze [Azure App Service] przy użyciu środowiska wiersza polecenia, takiego jak cmd.exe lub bash. Instrukcje podane w tym samouczku można wykonać w dowolnym systemie operacyjnym, w którym można uruchomić środowisko Node.js.

[!INCLUDE [app-service-linux](../../includes/app-service-linux.md)]

<a name="prereq"></a>

## <a name="cli-versions-to-complete-the-task"></a>Wersje interfejsu wiersza polecenia umożliwiające wykonanie zadania

Zadanie można wykonać przy użyciu jednej z następujących wersji interfejsu wiersza polecenia:

- [Interfejs wiersza polecenia platformy Azure w wersji 1.0](app-service-web-nodejs-get-started-cli-nodejs.md) — nasz interfejs wiersza polecenia dla klasycznego modelu wdrażania i modelu wdrażania na potrzeby zarządzania zasobami
- [Interfejs wiersza polecenia platformy Azure w wersji 2.0](app-service-web-nodejs-get-started.md) — nasz interfejs wiersza polecenia nowej generacji dla modelu wdrażania na potrzeby zarządzania zasobami

## <a name="prerequisites"></a>Wymagania wstępne
* [Node.js]
* [Bower]
* [Yeoman]
* [Git]
* [Interfejs wiersza polecenia platformy Azure 2.0 (wersja zapoznawcza)](/cli/azure/install-az-cli2)
* Konto platformy Microsoft Azure. Jeśli nie masz konta, możesz [utworzyć konto bezpłatnej wersji próbnej] lub [aktywować korzyści dla subskrybentów programu Visual Studio].

> [!NOTE]
> Usługę [App Service](https://azure.microsoft.com/try/app-service/) możesz wypróbować, nie mając konta platformy Azure. Utwórz aplikację startową i testuj ją nawet przez godzinę — bez kart kredytowych i bez zobowiązań.
> 
> 

## <a name="create-and-configure-a-simple-nodejs-app-for-azure"></a>Tworzenie i konfigurowanie prostej aplikacji Node.js dla platformy Azure
1. Otwórz wybrany terminal wiersza polecenia i zainstaluj [Generator aplikacji Express dla narzędzia Yeoman].
   
        npm install -g generator-express

2. `CD` — za pomocą tego polecenia przejdź do katalogu roboczego i wygeneruj aplikację Express przy użyciu następującej składni:
   
        yo express
   
    Po wyświetleniu monitu wybierz następujące opcje:  
   
    `? Would you like to create a new directory for your project?` **Yes**  
    `? Enter directory name` **{nazwa_aplikacji}**  
    `? Select a version to install:` **MVC**  
    `? Select a view engine to use:` **Jade**  
    `? Select a css preprocessor to use (Sass Requires Ruby):` **None**  
    `? Select a database to use:` **None**  
    `? Select a build tool to use:` **Grunt**

3. `CD` — za pomocą tego polecenia przejdź do katalogu głównego nowej aplikacji i uruchom ją, aby upewnić się, że jest uruchamiana w środowisku projektowym:
   
        npm start
   
    W przeglądarce przejdź do strony <http://localhost:3000>, aby upewnić się, że strona główna aplikacji Express jest wyświetlana. Po upewnieniu się, że aplikacja działa prawidłowo, zatrzymaj ją przy użyciu skrótu klawiaturowego `Ctrl-C`.

6. Otwórz plik ./config/config.js w katalogu głównym aplikacji i zmień port produkcyjny na `process.env.port`. Właściwość `production` w obiekcie `config` powinna wyglądać tak jak w następującym przykładzie:
   
        production: {
            root: rootPath,
            app: {
                name: 'express1'
            },
            port: process.env.port,
        }
   
    > [!NOTE] 
    > Domyślnie usługa Azure App Service uruchamia aplikacje Node.js z zastosowaniem zmiennych środowiskowych `production` (`process.env.NODE_ENV="production"`).
    > Z tego względu określona w tym miejscu konfiguracja umożliwia aplikacji Node.js odpowiadanie na żądania sieci Web przy użyciu domyślnego portu, na którym nasłuchuje program iisnode.
    >
    >

7. Otwórz plik ./package.json i dodaj właściwość `engines` w sekcji dotyczącej [określania odpowiedniej wersji środowiska Node.js](#version).
   
        "engines": {
            "node": "6.9.1"
        }, 

8. Zapisz zmiany, zainicjuj repozytorium Git w katalogu głównym aplikacji, a następnie zatwierdź swój kod:
   
        git add .
        git add -f config
        git commit -m "{your commit message}"

## <a name="deploy-your-nodejs-app-to-azure"></a>Wdrażanie aplikacji Node.js na platformie Azure

1. Zaloguj się na platformie Azure — wymagany jest [interfejs wiersza polecenia platformy Azure w wersji 2.0 (wersja zapoznawcza](#prereq)):
   
        az login
   
    Postępuj zgodnie z wyświetlanymi instrukcjami, aby kontynuować logowanie w przeglądarce za pomocą konta Microsoft zawierającego subskrypcję Azure.

3. Ustaw użytkownika wdrożenia dla usługi App Service. Kod zostanie później wdrożony przy użyciu tych poświadczeń.
   
        az appservice web deployment user set --user-name <username> --password <password>

3. Utwórz nową [grupę zasobów](../azure-resource-manager/resource-group-overview.md). Aby skorzystać z samouczka dla języka node.js, nie musisz nic na jego temat wiedzieć.

        az group create --location "<location>" --name my-nodejs-app-group

    Aby sprawdzić, jakich wartości można użyć dla lokalizacji `<location>`, użyj polecenia interfejsu wiersza polecenia `az appservice list-locations`.

3. Utwórz nowy, BEZPŁATNY [plan usługi App Service](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md). W przypadku tego samouczka dla języka node.js nie będzie naliczana opłata za aplikacje sieci Web w ramach tego planu.

        az appservice plan create --name my-nodejs-appservice-plan --resource-group my-nodejs-app-group --sku FREE

4. Utwórz nową aplikację sieci Web o unikatowej nazwie wprowadzonej w tagu `<app_name>`.

        az appservice web create --name <app_name> --resource-group my-nodejs-app-group --plan my-nodejs-appservice-plan

5. Skonfiguruj lokalne wdrożenie narzędzia Git dla aplikacji sieci Web za pomocą następującego polecenia:

        az appservice web source-control config-local-git --name <app_name> --resource-group my-nodejs-app-group

    Otrzymasz dane wyjściowe JSON podobne do następujących, co oznacza, że skonfigurowano zdalne repozytorium Git:

        {
        "url": "https://<deployment_user>@<app_name>.scm.azurewebsites.net/<app_name>.git"
        }

6. Dodaj adres URL w formacie JSON jako zdalny obiekt narzędzia Git lokalnego repozytorium Git (nazywanego dla uproszczenia `azure`).

        git remote add azure https://<deployment_user>@<app_name>.scm.azurewebsites.net/<app_name>.git
   
7. Wdróż przykładowy kod w zdalnym obiekcie narzędzia Git `azure`. Gdy zostanie wyświetlony odpowiedni monit, użyj skonfigurowanych wcześniej poświadczeń wdrożenia.

        git push azure master
   
    Generator aplikacji Express udostępnia plik gitignore, dlatego polecenie `git push` nie spowoduje zmniejszenia przepustowości przez próbę przekazania katalogu node_modules/.

9. Na koniec uruchom działającą aplikację Azure w przeglądarce:
   
        az appservice web browse --name <app_name> --resource-group my-nodejs-app-group
   
    Powinna zostać wyświetlona aplikacja sieci Web Node.js działająca w usłudze Azure App Service.
   
    ![Przykład wyświetlania wdrożonej aplikacji.][deployed-express-app]

## <a name="update-your-nodejs-web-app"></a>Aktualizowanie aplikacji sieci Web Node.js
Aby zaktualizować aplikację sieci Web Node.js działającą w usłudze App Service, uruchom polecenia `git add`, `git commit` i `git push`, jak podczas pierwszego wdrożenia aplikacji sieci Web.

## <a name="how-app-service-deploys-your-nodejs-app"></a>Sposób wdrażania aplikacji Node.js przez usługę App Service
Usługa Azure App Service uruchamia aplikacje Node.js przy użyciu programu [iisnode]. Interfejs wiersza polecenia platformy Azure w wersji 2.0 (wersja zapoznawcza) oraz aparat Kudu (wdrożenie Git) współdziałają ze sobą w celu zapewnienia usprawnionego środowiska podczas tworzenia i wdrażania aplikacji Node.js z poziomu wiersza polecenia. 

* Można utworzyć plik iisnode.yml w katalogu głównym i użyć go w celu dostosowania właściwości programu iisnode. Wszystkie ustawienia możliwe do skonfigurowania są udokumentowane [tutaj](https://github.com/tjanczuk/iisnode/blob/master/src/samples/configuration/iisnode.yml).
* Po uruchomieniu polecenia `git push azure master` aparat Kudu automatyzuje następujące zadania wdrażania:
  
  * Jeśli plik package.json znajduje się w katalogu głównym repozytorium, uruchom polecenie `npm install --production`.
  * Wygeneruj plik Web.config dla programu iisnode wskazujący skrypt początkowy w pliku package.json (np. server.js lub app.js).
  * Dostosuj plik Web.config, aby przygotować aplikację do debugowania przy użyciu narzędzia Node-Inspector.

## <a name="use-a-nodejs-framework"></a>Korzystanie ze środowiska Node.js
Jeśli tworzysz aplikacje przy użyciu popularnego środowiska Node.js, takiego jak [Sails.js][SAILSJS] lub [MEAN.js][MEANJS], możesz je wdrożyć w usłudze App Service. Popularne środowiska Node.js mają swoje osobliwości, a ich zależności pakietów są stale aktualizowane. Jednak usługa App Service udostępnia dzienniki stdout i stderr, dlatego możesz uzyskać dokładne informacje dotyczące działania aplikacji i wprowadzić odpowiednie zmiany. Aby uzyskać więcej informacji, zobacz [Pobieranie dzienników stdout i stderr z programu iisnode](#iisnodelog).

W następujących samouczkach przedstawiono sposób pracy z określonym środowiskiem w usłudze App Service:

* [Wdrażanie aplikacji sieci Web Sails.js przy użyciu usługi Azure App Service]
* [Tworzenie aplikacji czatu Node.js przy użyciu biblioteki Socket.IO w usłudze Azure App Service]
* [Sposób użycia platformy io.js z aplikacjami Web Apps w usłudze Azure App Service]

<a name="version"></a>

## <a name="use-a-specific-nodejs-engine"></a>Korzystanie z określonego aparatu Node.js
W typowym przepływie pracy można nakazać usłudze App Service korzystanie z konkretnego aparatu Node.js, tak jak zwykle w pliku package.json.
Na przykład:

    "engines": {
        "node": "6.9.1"
    }, 

Aparat wdrażania Kudu określa, który aparat Node.js ma zostać użyty, w następującej kolejności:

* Najpierw sprawdź, czy w pliku określono element `nodeProcessCommandLine`. Jeśli tak, użyj tego elementu.
* Następnie sprawdź, czy w pliku package.json określono element `"node": "..."` w obiekcie `engines`. Jeśli tak, użyj tego elementu.
* Wybierz domyślną wersję środowiska Node.js.

Zaktualizowaną listę wszystkich obsługiwanych wersji środowiska Node.js/narzędzia NPM w usłudze Azure App Service znajdziesz pod następującym adresem URL dla Twojej aplikacji:

    https://<app_name>.scm.azurewebsites.net/api/diagnostics/runtime

> [!NOTE]
> Zalecane jest jawne zdefiniowanie odpowiedniego aparatu Node.js. Domyślna wersja aparatu Node.js może się zmienić, co spowoduje, że przestanie być odpowiednia dla aplikacji, i doprowadzi do występowania błędów w aplikacji sieci Web platformy Azure.
> 
> 

<a name="iisnodelog"></a>

## <a name="get-stdout-and-stderr-logs-from-iisnode"></a>Pobieranie dzienników stdout i stderr z programu iisnode
Aby odczytać dzienniki programu iisnode, wykonaj następujące czynności.

> [!NOTE]
> Po wykonaniu tych czynności pliki dzienników mogą nie istnieć do momentu wystąpienia błędu.
> 
> 

1. Otwórz plik iisnode.yml udostępniany przez interfejs wiersza polecenia platformy Azure w wersji 2.0 (wersja zapoznawcza).
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
   
    Ten adres URL różni się od adresu URL aplikacji ciągiem „*.scm.*” dodanym do nazwy DNS. W przypadku pominięcia tego ciągu w adresie URL wystąpi błąd 404.
5. Przejdź do katalogu D:\home\site\wwwroot\iisnode.
   
    ![Przechodzenie do lokalizacji plików dziennika programu iisnode.][iislog-kudu-console-find]
6. Kliknij ikonę **Edytuj** dla dziennika, który chcesz odczytać. Jeśli chcesz, możesz również kliknąć ikonę **Pobierz** lub **Usuń**.
   
    ![Otwieranie pliku dziennika programu iisnode][iislog-kudu-console-open]
   
    Spowoduje to wyświetlenie dziennika w celu ułatwienia debugowania wdrożenia usługi App Service.
   
    ![Sprawdzanie pliku dziennika programu iisnode][iislog-kudu-console-read]

## <a name="debug-your-app-with-node-inspector"></a>Debugowanie aplikacji przy użyciu narzędzia Node-Inspector
Jeśli do debugowania aplikacji Node.js używasz narzędzia Node-Inspector, możesz go użyć, aby debugować aplikację usługi App Service. Narzędzie Node-Inspector jest wstępnie zainstalowane w instalacji programu iisnode dla usługi App Service. W przypadku wdrożenia przy użyciu programu Git plik Web.config automatycznie wygenerowany przez aparat Kudu zawiera już całą konfigurację niezbędną do włączenia narzędzia Node-Inspector.

Aby włączyć narzędzie Node-Inspector, wykonaj następujące czynności:

1. Otwórz plik iisnode.yml w katalogu głównym repozytorium i określ następujące parametry: 
   
        debuggingEnabled: true
        debuggerExtensionDll: iisnode-inspector.dll
2. Zapisz zmiany, a następnie wypchnij je do platformy Azure przy użyciu następujących poleceń Git:
   
        git add .
        git commit -m "{your commit message}"
        git push azure master
3. Teraz przejdź do pliku uruchamiania aplikacji określonego w skrypcie uruchamiana w pliku package.json, dodając ciąg „/debug” do adresu URL. Na przykład:
   
        http://{appname}.azurewebsites.net/server.js/debug
   
    Lub:
   
        http://{appname}.azurewebsites.net/app.js/debug

## <a name="more-resources"></a>Więcej zasobów
* [Specifying a Node.js version in an Azure application (Określanie wersji środowiska Node.js w aplikacji Azure)](../nodejs-specify-node-version-azure-apps.md)
* [Best practices and troubleshooting guide for Node.js applications on Azure (Najlepsze praktyki i przewodnik rozwiązywania problemów aplikacji Node.js na platformie Azure)](app-service-web-nodejs-best-practices-and-troubleshoot-guide.md)
* [How to debug a Node.js web app in Azure App Service (Jak debugować aplikację sieci Web Node.js w usłudze Azure App Service)](web-sites-nodejs-debug.md)
* [Using Node.js Modules with Azure applications (Używanie modułów Node.js z aplikacjami platformy Azure)](../nodejs-use-node-modules-azure-apps.md)
* [Azure App Service Web Apps: Node.js (Aplikacje sieci Web w usłudze Azure App Service: Node.js)](http://blogs.msdn.com/b/silverlining/archive/2012/06/14/windows-azure-websites-node-js.aspx)
* [Centrum deweloperów środowiska Node.js](/develop/nodejs/)
* [Rozpoczynanie pracy z aplikacjami sieci Web w usłudze Azure App Service](app-service-web-get-started.md)
* [Exploring the Super Secret Kudu Debug Console (Szczegółowe informacje o ściśle tajnej konsoli debugowania aparatu Kudu)]

<!-- URL List -->

[Azure App Service]: ../app-service/app-service-value-prop-what-is.md
[aktywować korzyści dla subskrybentów programu Visual Studio]: http://go.microsoft.com/fwlink/?LinkId=623901
[Bower]: http://bower.io/
[Tworzenie aplikacji czatu Node.js przy użyciu biblioteki Socket.IO w usłudze Azure App Service]: ./web-sites-nodejs-chat-app-socketio.md
[Wdrażanie aplikacji sieci Web Sails.js przy użyciu usługi Azure App Service]: ./app-service-web-nodejs-sails.md
[Exploring the Super Secret Kudu Debug Console (Szczegółowe informacje o ściśle tajnej konsoli debugowania aparatu Kudu)]: /documentation/videos/super-secret-kudu-debug-console-for-azure-web-sites/
[Generator aplikacji Express dla narzędzia Yeoman]: https://github.com/petecoop/generator-express
[Git]: http://www.git-scm.com/downloads
[Sposób użycia platformy io.js z aplikacjami Web Apps w usłudze Azure App Service]: ./web-sites-nodejs-iojs.md
[iisnode]: https://github.com/tjanczuk/iisnode/wiki
[MEANJS]: http://meanjs.org/
[Node.js]: http://nodejs.org
[SAILSJS]: http://sailsjs.org/
[utworzyć konto bezpłatnej wersji próbnej]: http://go.microsoft.com/fwlink/?LinkId=623901
[web app]: ./app-service-web-overview.md
[Yeoman]: http://yeoman.io/

<!-- IMG List -->

[deployed-express-app]: ./media/app-service-web-nodejs-get-started/deployed-express-app.png
[iislog-kudu-console-find]: ./media/app-service-web-nodejs-get-started/iislog-kudu-console-navigate.png
[iislog-kudu-console-open]: ./media/app-service-web-nodejs-get-started/iislog-kudu-console-open.png
[iislog-kudu-console-read]: ./media/app-service-web-nodejs-get-started/iislog-kudu-console-read.png

