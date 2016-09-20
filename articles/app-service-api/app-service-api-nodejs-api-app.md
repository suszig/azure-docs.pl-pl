<properties
    pageTitle="Aplikacja interfejsu API środowiska Node.js w usłudze Azure App Service | Microsoft Azure"
    description="Dowiedz się, jak utworzyć interfejs API RESTful środowiska Node.js, a następnie wdrożyć go w aplikacji interfejsu API w usłudze Azure App Service."
    services="app-service\api"
    documentationCenter="node"
    authors="bradygaster"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-api"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="node"
    ms.topic="get-started-article"
    ms.date="05/26/2016"
    ms.author="rachelap"/>

# Kompilowanie interfejsu API RESTful środowiska Node.js i wdrażanie go w aplikacji interfejsu API na platformie Azure

[AZURE.INCLUDE [app-service-api-get-started-selector](../../includes/app-service-api-get-started-selector.md)]

Ten samouczek przedstawia sposób tworzenia prostego interfejsu API środowiska [Node.js](http://nodejs.org) i wdrażania go w [aplikacji interfejsu API](app-service-api-apps-why-best-platform.md) w [usłudze Azure App Service](../app-service/app-service-value-prop-what-is.md) za pomocą usługi [Git](http://git-scm.com). Możesz użyć dowolnego systemu operacyjnego obsługującego środowisko Node.js. Wszystkie czynności zostaną wykonane przy użyciu narzędzi wiersza polecenia, takich jak cmd.exe lub bash.

## Wymagania wstępne

1. Konto platformy Microsoft Azure ([załóż bezpłatne konto tutaj](https://azure.microsoft.com/pricing/free-trial/))
1. Zainstalowane środowisko [Node.js](http://nodejs.org) (w tym przykładzie założono użycie środowiska Node.js w wersji 4.2.2)
2. Zainstalowana usługa [Git](https://git-scm.com/)
1. Konto w serwisie [GitHub](https://github.com/)

Usługa App Service obsługuje wiele sposobów wdrażania kodu w aplikacji interfejsu API. W tym samouczku przedstawiono metodę Git i przyjęto założenie, że dysponujesz podstawową wiedzą z zakresu pracy z usługą Git. Aby uzyskać informacje na temat innych metod wdrażania, zobacz temat [Wdrażanie aplikacji w usłudze Azure App Service](../app-service-web/web-sites-deploy.md).

## Pobieranie przykładowego kodu

1. Otwórz interfejs wiersza polecenia obsługujący polecenia języka Node.js i usługi Git.

1. Przejdź do folderu, którego można używać na potrzeby lokalnego repozytorium Git, i sklonuj [repozytorium GitHub zawierające przykładowy kod](https://github.com/Azure-Samples/app-service-api-node-contact-list).

        git clone https://github.com/Azure-Samples/app-service-api-node-contact-list.git

    Przykładowy interfejs API zawiera dwa punkty końcowe: żądanie Get dotyczące elementu `/contacts` zwraca listę nazw i adresów e-mail w formacie JSON, natomiast element `/contacts/{id}` zwraca tylko wybrany kontakt.

## Tworzenie szkieletu (automatyczne generowanie) kodu Node.js na podstawie metadanych struktury Swagger

[Swagger](http://swagger.io/) to format pliku metadanych, który opisuje interfejs API RESTful. Usługa Azure App Service zapewnia [wbudowaną obsługę metadanych struktury Swagger](app-service-api-metadata.md). W tej sekcji samouczka przedstawiono sposób modelowania przepływu pracy programowania, w którym najpierw następuje utworzenie metadanych struktury Swagger, a następnie użycie ich do tworzenia szkieletu (automatycznego generowania) kodu serwera dla interfejsu API. 

>[AZURE.NOTE] Tę sekcję możesz pominąć, jeśli nie chcesz poznać sposobu tworzenia szkieletu kodu Node.js na postawie pliku metadanych struktury Swagger. Jeśli chcesz tylko wdrożyć przykładowy kod w nowej aplikacji interfejsu API, przejdź bezpośrednio do sekcji [Tworzenie aplikacji interfejsu API na platformie Azure](#createapiapp).

### Instalowanie i wykonywanie narzędzia Swaggerize

1. Wykonaj następujące polecenia, aby globalnie zainstalować moduły NPM **yo** i **generator-swaggerize**.

        npm install -g yo
        npm install -g generator-swaggerize

    Swaggerize jest narzędziem, które generuje kod serwera dla interfejsu API opisanego przez plik metadanych struktury Swagger. Plik Swagger, który będzie używany, nosi nazwę *api.json* i znajduje się w folderze *start* sklonowanego repozytorium.

2. Przejdź do folderu *start*, a następnie wykonaj polecenie `yo swaggerize`. Narzędzie Swaggerize zada serię pytań.  W odpowiedzi na pytanie **what to call this project** (jak chcesz nazwać ten projekt), wpisz „contactlist”, w wierszu **path to swagger document** (ścieżka do dokumentu Swagger) wpisz „api.json”, a w wierszu **Express, Hapi, or Restify** (Express, Hapi lub Restify) — „express”.

        yo swaggerize

    ![Wiersz polecenia narzędzia Swaggerize](media/app-service-api-nodejs-api-app/swaggerize-command-line.png)
    
    **Uwaga**: jeśli podczas wykonywania tego kroku wystąpi błąd, można go naprawić w sposób opisany w następnym kroku.

    Narzędzie Swaggerize tworzy folder aplikacji oraz szkielety mechanizmów obsługi i plików konfiguracji, a także generuje plik **package.json**. Aparat widoku ekspresowego jest używany do generowania strony pomocy programu Swagger.  

3. Jeśli nie można wykonać polecenia `swaggerize` i wystąpi błąd „unexpected token” (nieoczekiwany token) lub „invalid escape sequence” (nieprawidłowa sekwencja ucieczki), usuń przyczynę błędu, edytując wygenerowany plik *package.json*. W wierszu `regenerate` w obszarze `scripts` zmień ukośnik odwrócony poprzedzający element *api.json* na ukośnik, tak aby wiersz wyglądał w następujący sposób:

        "regenerate": "yo swaggerize --only=handlers,models,tests --framework express --apiPath config/api.json"

1. Przejdź do folderu, który zawiera utworzony szkielet kodu (w tym przypadku podfolderu *ContactList*).

1. Uruchom polecenie `npm install`.
    
        npm install
        
2. Zainstaluj moduł NPM **jsonpath**. 

        npm install --save jsonpath
        
    ![Instalowanie modułu jsonpath](media/app-service-api-nodejs-api-app/jsonpath-install.png)

1. Zainstaluj moduł NPM **swaggerize-ui**. 

        npm install --save swaggerize-ui
        
    ![Instalowanie interfejsu użytkownika narzędzia Swaggerize](media/app-service-api-nodejs-api-app/swaggerize-ui-install.png)

### Dostosowywanie utworzonego szkieletu kodu

1. Skopiuj folder **lib** z folderu **start** do folderu **ContactList** utworzonego w procesie tworzenia szkieletu. 

1. Zastąp kod w pliku **handlers/contacts.js** poniższym kodem. 

    Ten kod zawiera dane JSON przechowywane w pliku **lib/contacts.json** obsługiwanym przez plik **lib/contactRepository.js**. Nowy kod contacts.js odpowiada na żądania HTTP dotyczące pobrania wszystkich kontaktów i zwraca je jako ładunek JSON. 

        'use strict';
        
        var repository = require('../lib/contactRepository');
        
        module.exports = {
            get: function contacts_get(req, res) {
                res.json(repository.all())
            }
        };

1. Zastąp kod w pliku **handlers/contacts/{id}.js** poniższym kodem. 

        'use strict';

        var repository = require('../../lib/contactRepository');
        
        module.exports = {
            get: function contacts_get(req, res) {
                res.json(repository.get(req.params['id']));
            }    
        };

1. Zastąp kod w pliku **server.js** poniższym kodem. 

    Zmiany wprowadzone w pliku server.js są wywoływane przy użyciu komentarzy, dzięki czemu są widoczne dla użytkownika. 

        'use strict';

        var port = process.env.PORT || 8000; // first change

        var http = require('http');
        var express = require('express');
        var bodyParser = require('body-parser');
        var swaggerize = require('swaggerize-express');
        var swaggerUi = require('swaggerize-ui'); // second change
        var path = require('path');

        var app = express();

        var server = http.createServer(app);

        app.use(bodyParser.json());

        app.use(swaggerize({
            api: path.resolve('./config/api.json'), // third change
            handlers: path.resolve('./handlers'),
            docspath: '/swagger' // fourth change
        }));

        // change four
        app.use('/docs', swaggerUi({
          docs: '/swagger'  
        }));

        server.listen(port, function () { // fifth and final change
        });

### Testowanie przy użyciu interfejsu API uruchomionego lokalnie

1. Aktywuj serwer przy użyciu pliku wykonywalnego wiersza polecenia Node.js. 

        node server.js

1. Po przejściu do strony **http://localhost:8000/contacts** widoczne będą dane wyjściowe JSON listy kontaktów (lub zostanie wyświetlony monit o ich pobranie, w zależności od przeglądarki). 

    ![Wywołanie interfejsu API wszystkich kontaktów](media/app-service-api-nodejs-api-app/all-contacts-api-call.png)

1. Po przejściu do strony **http://localhost:8000/contacts/2** widoczny będzie kontakt reprezentowany przez daną wartość identyfikatora.

    ![Wywołanie interfejsu API określonego kontaktu](media/app-service-api-nodejs-api-app/specific-contact-api-call.png)

1. Dane JSON programu Swagger są obsługiwane za pośrednictwem punktu końcowego **/swagger**:

    ![Kontakty — dane JSON struktury Swagger](media/app-service-api-nodejs-api-app/contacts-swagger-json.png)

1. Interfejs użytkownika struktury Swagger jest obsługiwany za pośrednictwem punktu końcowego **/docs**. W interfejsie użytkownika struktury Swagger można testować interfejs API przy użyciu zaawansowanych funkcji klienta HTML.

    ![Interfejs użytkownika struktury Swagger](media/app-service-api-nodejs-api-app/swagger-ui.png)

## <a id="createapiapp"></a> Tworzenie nowej aplikacji interfejsu API

W tej sekcji opisano sposób tworzenia nowej aplikacji interfejsu API na platformie Azure za pośrednictwem portalu Azure. Ta aplikacja interfejsu API reprezentuje zasoby obliczeniowe udostępniane na platformie Azure w celu uruchomienia kodu. W kolejnych sekcjach zostanie przedstawiony sposób wdrażania kodu w nowej aplikacji interfejsu API.

1. Przejdź do [Portalu Azure](https://portal.azure.com/). 

1. Kliknij kolejno pozycje **Nowe > Sieci Web i mobilność > Aplikacja interfejsu API**. 

    ![Nowa aplikacja interfejsu API w portalu](media/app-service-api-nodejs-api-app/new-api-app-portal.png)

4. W polu **Nazwa aplikacji** wprowadź wartość unikatową w domenie *azurewebsites.net*, taką jak NodejsAPIApp z dołączoną liczbą (dzięki czemu będzie ona unikatowa). 

    Jeśli na przykład nazwa to `NodejsAPIApp`, adres URL będzie mieć postać `nodejsapiapp.azurewebsites.net`.

    Jeśli wprowadzona nazwa została już użyta przez inną osobę, z prawej strony będzie widoczny czerwony wykrzyknik.

6. Na liście **Grupa zasobów** kliknij pozycję **Nowa**, a następnie w polu **Nazwa nowej grupy zasobów** wprowadź ciąg „NodejsAPIAppGroup” lub inną wybraną nazwę. 

    [Grupa zasobów](../resource-group-overview.md) to kolekcja zasobów platformy Azure, takich jak Aplikacje interfejsu API, bazy danych i maszyny wirtualne. Do celów tego samouczka najlepiej utworzyć nową grupę zasobów, ponieważ ułatwi to usunięcie w jednym kroku wszystkich zasobów platformy Azure utworzonych na potrzeby samouczka.

4. Kliknij pozycję **Plan/lokalizacja usługi App Service**, a następnie kliknij pozycję **Utwórz nowe**.

    ![Tworzenie planu usługi App Service](./media/app-service-api-nodejs-api-app/newappserviceplan.png)

    Poniższe kroki umożliwiają utworzenie planu usługi App Service dla nowej grupy zasobów. Plan usługi App Service określa zasoby obliczeniowe, których używa aplikacja interfejsu API. Jeśli na przykład wybierzesz warstwę Bezpłatna, aplikacja interfejsu API będzie działać na udostępnionych maszynach wirtualnych. W przypadku niektórych warstw płatnych będzie ona działać na specjalnych maszynach wirtualnych. Aby uzyskać informacje o planach usługi App Service, zobacz temat [Omówienie planów usługi App Service](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).

5. W bloku **Plan usługi App Service** wprowadź ciąg „NodejsAPIAppPlan” lub inną wybraną nazwę.

5. Z listy rozwijanej **Lokalizacja** wybierz najbliższą lokalizację.

    To ustawienie służy do określania, w którym centrum danych Azure aplikacja zostanie uruchomiona. Do celów tego samouczka można wybrać dowolny region — nie spowoduje to znaczącej różnicy. Jednak w przypadku aplikacji produkcyjnej serwer powinien znajdować się możliwie najbliżej klientów, którzy uzyskują do niego dostęp, aby zminimalizować [opóźnienia](http://www.bing.com/search?q=web%20latency%20introduction&qs=n&form=QBRE&pq=web%20latency%20introduction&sc=1-24&sp=-1&sk=&cvid=eefff99dfc864d25a75a83740f1e0090).

5. Kliknij kolejno pozycje **Warstwa cenowa > Wyświetl wszystko > F1 Bezpłatna**.

    Do ceków tego samouczka warstwa cenowa Bezpłatna zapewni wystarczającą wydajność.

    ![Wybieranie warstwy cenowej Bezpłatna](./media/app-service-api-nodejs-api-app/selectfreetier.png)

6. W bloku **Plan usługi App Service** kliknij przycisk **OK**.

7. W bloku **Aplikacja interfejsu API** kliknij pozycję **Utwórz**.

## Konfigurowanie nowej aplikacji interfejsu API w celu wdrożenia usługi Git

Wdrażanie kodu w aplikacji interfejsu API odbywa się przez wypychanie zatwierdzeń do repozytorium Git w usłudze Azure App Service. W tej części samouczka utworzysz na platformie Azure poświadczenia i repozytorium Git, które będą używane podczas wdrażania.  

1. Po utworzeniu aplikacji interfejsu API kliknij kolejno pozycje **Usługi App Service > {aplikacja interfejsu API}** na stronie głównej portalu. 

    W portalu zostaną wyświetlone bloki **Aplikacja interfejsu API** i **Ustawienia**.

    ![Bloki Aplikacja interfejsu API i Ustawienia w portalu](media/app-service-api-nodejs-api-app/portalapiappblade.png)

1. W bloku **Ustawienia** przewiń w dół do sekcji **Publikowanie**, a następnie kliknij pozycję **Poświadczenia wdrożenia**.
 
3. W bloku **Ustaw poświadczenia wdrażania** wprowadź nazwę użytkownika i hasło, a następnie kliknij pozycję **Zapisz**.

    Te poświadczenia będą używane do publikowania kodu Node.js w aplikacji interfejsu API. 

    ![Poświadczenia wdrożenia](media/app-service-api-nodejs-api-app/deployment-credentials.png)

1. W bloku **Ustawienia** kliknij kolejno pozycje **Źródło wdrożenia > Wybierz źródło > Lokalne repozytorium Git**, a następnie kliknij przycisk **OK**.

    ![Tworzenie repozytorium Git](media/app-service-api-nodejs-api-app/create-git-repo.png)

1. Po utworzeniu repozytorium Git blok zostanie zmieniony, tak aby widoczne były aktywne wdrożenia. Ponieważ jest to nowe repozytorium, na liście nie ma aktywnych wdrożeń. 

    ![Brak aktywnych wdrożeń](media/app-service-api-nodejs-api-app/no-active-deployments.png)

1. Skopiuj adres URL repozytorium Git. W tym celu przejdź do bloku nowej aplikacji interfejsu API i zapoznaj się z sekcją **Podstawy** bloku. Znajdź pole **Adres URL klonowania Git** w sekcji **Podstawy**. Po ustawieniu kursora na tym adresie URL po prawej stronie będzie widoczna ikona służąca do kopiowania adresu URL do schowka. Kliknij tę ikonę, aby skopiować adres URL.

    ![Pobieranie adresu URL usługi Git z portalu](media/app-service-api-nodejs-api-app/get-the-git-url-from-the-portal.png)

    **Uwaga**: adres URL klonowania Git będzie potrzebny w następnej sekcji, dlatego należy go zanotować.

Posiadając aplikację interfejsu API z wspierającym ją repozytorium Git, możesz wypchnąć kod do repozytorium w celu wdrożenia go w aplikacji interfejsu API. 

## Wdrażanie kodu interfejsu API na platformie Azure

W tej sekcji utworzysz lokalne repozytorium Git zawierające kod serwera dla interfejsu API, a następnie wypchniesz kod z tego repozytorium do wcześniej utworzonego repozytorium na platformie Azure.

1. Skopiuj folder `ContactList` do lokalizacji, której możesz użyć dla nowego lokalnego repozytorium Git. Jeśli kroki opisane w pierwszej części samouczka zostały wykonane, skopiuj element `ContactList` z folderu `start`. W przeciwnym razie skopiuj element `ContactList` z folderu `end`.

1. W narzędziu wiersza polecenia przejdź do nowego folderu, a następnie wykonaj poniższe polecenie w celu utworzenia nowego lokalnego repozytorium Git. 

        git init

     ![Nowe lokalne repozytorium Git](media/app-service-api-nodejs-api-app/new-local-git-repo.png)

1. Wykonaj poniższe polecenie, aby dodać zdalny element usługi Git do repozytorium aplikacji interfejsu API. 

        git remote add azure YOUR_GIT_CLONE_URL_HERE

    **Uwaga**: zastąp ciąg „YOUR_GIT_CLONE_URL_HERE” własnym, wcześniej skopiowanym adresem URL klonowania Git. 

1. Wykonaj poniższe polecenia, aby utworzyć zatwierdzenie zawierające cały kod. 

        git add .
        git commit -m "initial revision"

    ![Dane wyjściowe zatwierdzenia Git](media/app-service-api-nodejs-api-app/git-commit-output.png)

1. Wykonaj polecenie, aby wypchnąć kod do platformy Azure. Po wyświetleniu monitu o hasło wprowadź hasło utworzone wcześniej w Portalu Azure.

        git push azure master

    Spowoduje to zainicjowanie wdrożenia w aplikacji interfejsu API.  

1. W przeglądarce przejdź wstecz do bloku **Wdrożenia** dotyczącego aplikacji interfejsu API i sprawdź, czy jest widoczne wdrożenie w toku. 

    ![Wdrożenie w toku](media/app-service-api-nodejs-api-app/deployment-happening.png)

    Jednocześnie interfejs wiersza polecenia odzwierciedla stan trwającego wdrożenia. 

    ![Wdrożenie Node.js w toku](media/app-service-api-nodejs-api-app/node-js-deployment-happening.png)

    Po zakończeniu wdrożenia blok **Wdrożenia** odzwierciedla pomyślne wdrożenie zmian kodu w aplikacji interfejsu API. 

## Testowanie przy użyciu interfejsu API uruchomionego na platformie Azure
 
3. Skopiuj wartość pola **Adres URL** w sekcji **Podstawy** bloku aplikacji interfejsu API. 

    ![Zakończone wdrożenie](media/app-service-api-nodejs-api-app/deployment-completed.png)

1. Korzystając z klienta interfejsu API REST, takiego jak Postman lub Fiddler (albo przeglądarka sieci Web), podaj adres URL wywołania interfejsu API kontaktów, który stanowi punkt końcowy `/contacts` aplikacji interfejsu API. Adres URL będzie mieć postać: `https://{your API app name}.azurewebsites.net/contacts`

    W przypadku wysyłania żądania GET do tego punktu końcowego otrzymasz dane wyjściowe JSON aplikacji interfejsu API.

    ![Interfejs API z wynikami programu Postman](media/app-service-api-nodejs-api-app/postman-hitting-api.png)

2. W przeglądarce przejdź do punktu końcowego `/docs`, aby wypróbować interfejs użytkownika struktury Swagger podczas działania na platformie Azure.

Po skonfigurowaniu ciągłego dostarczania możesz wprowadzać zmiany kodu i wdrażać je na platformie Azure przez wypychanie zatwierdzeń do repozytorium Git platformy Azure.

## Następne kroki

Na tym etapie pomyślnie zakończono tworzenie aplikacji interfejsu API i wdrażanie w niej kodu interfejsu API środowiska Node.js. W następnym samouczku przedstawiono sposób [korzystania z Aplikacji interfejsu API z poziomu klientów JavaScript przy użyciu specyfikacji CORS](app-service-api-cors-consume-javascript.md).



<!--HONumber=sep16_HO1-->


