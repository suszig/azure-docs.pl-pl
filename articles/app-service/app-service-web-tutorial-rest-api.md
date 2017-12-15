---
title: "Aplikacja interfejsu API środowiska Node.js w usłudze Azure App Service | Microsoft Docs"
description: "Dowiedz się, jak utworzyć interfejs API RESTful środowiska Node.js, a następnie wdrożyć go w aplikacji interfejsu API w usłudze Azure App Service."
services: app-service\api
documentationcenter: node
author: bradygaster
manager: erikre
editor: 
ms.assetid: a820e400-06af-4852-8627-12b3db4a8e70
ms.service: app-service-api
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: tutorial
ms.date: 06/13/2017
ms.author: rachelap
ms.custom: mvc, devcenter
ms.openlocfilehash: 2170ac7df3b894c8d19b432abdcfef5c7fd75ff4
ms.sourcegitcommit: 0e4491b7fdd9ca4408d5f2d41be42a09164db775
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/14/2017
---
# <a name="build-a-nodejs-restful-api-and-deploy-it-to-an-api-app-in-azure"></a>Kompilowanie interfejsu API RESTful środowiska Node.js i wdrażanie go w aplikacji interfejsu API na platformie Azure

W tym przewodniku Szybki start pokazano, jak utworzyć interfejs API REST napisany w języku Node.js platformy [Express](http://expressjs.com/) za pomocą definicji [Swagger](http://swagger.io/) i wdrożyć go na platformie Azure. Aplikację tworzy się za pomocą narzędzi wiersza polecenia, następnie konfiguruje zasoby przy użyciu [interfejsu wiersza polecenia platformy Azure](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) i wdraża aplikację za pomocą usługi Git.  Gdy wszystko będzie gotowe, uzyskasz funkcjonalny interfejs API REST działający na platformie Azure.

## <a name="prerequisites"></a>Wymagania wstępne

* [Git](https://git-scm.com/)
* [Node.js i NPM](https://nodejs.org/)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Jeśli zdecydujesz się zainstalować interfejs wiersza polecenia i korzystać z niego lokalnie, ten temat będzie wymagał interfejsu wiersza polecenia platformy Azure w wersji 2.0 lub nowszej. Uruchom polecenie `az --version`, aby dowiedzieć się, jaka wersja jest używana. Jeśli konieczna będzie instalacja lub uaktualnienie, zobacz [Instalowanie interfejsu wiersza polecenia platformy Azure 2.0]( /cli/azure/install-azure-cli). 

## <a name="prepare-your-environment"></a>Przygotowywanie środowiska

1. W oknie terminala uruchom następujące polecenie, aby sklonować przykład na maszynę lokalną.

    ```bash
    git clone https://github.com/Azure-Samples/app-service-api-node-contact-list
    ```

2. Przejdź do katalogu, który zawiera przykładowy kod.

    ```bash
    cd app-service-api-node-contact-list
    ```

3. Zainstaluj [narzędzie Swaggerize](https://www.npmjs.com/package/swaggerize-express) na maszynie lokalnej. Swaggerize to narzędzie generujące kod Node.js dla interfejsu API REST na podstawie definicji Swagger.

    ```bash
    npm install -g yo
    npm install -g generator-swaggerize
    ```

## <a name="generate-nodejs-code"></a>Generowanie kodu Node.js 

W tej sekcji samouczka przedstawiono sposób modelowania przepływu pracy programowania, w którym najpierw następuje utworzenie metadanych struktury Swagger, a następnie użycie ich do tworzenia szkieletu (automatycznego generowania) kodu serwera dla interfejsu API. 

Zmień katalog na folder *start*, a następnie uruchom polecenie `yo swaggerize`. Narzędzie Swaggerize utworzy projekt Node.js dla Twojego interfejsu API na podstawie definicji Swagger w pliku *api.json*.

```bash
cd start
yo swaggerize --apiPath api.json --framework express
```

Kiedy w narzędziu Swaggerize zostanie wyświetlony monit o podanie nazwy projektu, użyj nazwy *ContactList*.
   
   ```bash
   Swaggerize Generator
   Tell us a bit about your application
   ? What would you like to call this project: ContactList
   ? Your name: Francis Totten
   ? Your github user name: fabfrank
   ? Your email: frank@fabrikam.net
   ```
   
## <a name="customize-the-project-code"></a>Dostosowywanie kodu projektu

1. Skopiuj folder *lib* do folderu *ContactList* utworzonego przez polecenie `yo swaggerize`, a następnie zmień katalog na *ContactList*.

    ```bash
    cp -r lib/ ContactList/
    cd ContactList
    ```

2. Zainstaluj moduły NPM `jsonpath` i `swaggerize-ui`. 

    ```bash
    npm install --save jsonpath swaggerize-ui
    ```

3. Zastąp kod w pliku *handlers/contacts.js* następującym kodem: 
    ```javascript
    'use strict';

    var repository = require('../lib/contactRepository');

    module.exports = {
        get: function contacts_get(req, res) {
            res.json(repository.all())
        }
    };
    ```
    Ten kod korzysta z danych JSON przechowywanych w pliku *lib/contacts.json* obsługiwanym przez plik *lib/contactRepository.js*. Nowy kod *contacts.js* zwraca wszystkie kontakty w repozytorium jako ładunek JSON. 

4. Zastąp kod w pliku **handlers/contacts/{id}.js** następującym kodem:

    ```javascript
    'use strict';

    var repository = require('../../lib/contactRepository');

    module.exports = {
        get: function contacts_get(req, res) {
            res.json(repository.get(req.params['id']));
        }    
    };
    ```

    Ten kod pozwala użyć zmiennej ścieżki w celu zwrócenia tylko kontaktu o podanym identyfikatorze.

5. Zastąp kod w pliku **server.js** następującym kodem:

    ```javascript
    'use strict';

    var port = process.env.PORT || 8000; 

    var http = require('http');
    var express = require('express');
    var bodyParser = require('body-parser');
    var swaggerize = require('swaggerize-express');
    var swaggerUi = require('swaggerize-ui'); 
    var path = require('path');
    var fs = require("fs");
    
    fs.existsSync = fs.existsSync || require('path').existsSync;

    var app = express();

    var server = http.createServer(app);

    app.use(bodyParser.json());

    app.use(swaggerize({
        api: path.resolve('./config/swagger.json'),
        handlers: path.resolve('./handlers'),
        docspath: '/swagger' 
    }));

    // change four
    app.use('/docs', swaggerUi({
        docs: '/swagger'  
    }));

    server.listen(port, function () { 
    });
    ```   

    Ten kod wprowadza niewielkie zmiany umożliwiające pracę z usługą Azure App Service i ujawniające interaktywny interfejs internetowy dla Twojego interfejsu API.

### <a name="test-the-api-locally"></a>Testowanie interfejsu API lokalnie

1. Uruchamianie aplikacji Node.js
    ```bash
    npm start
    ```
    
2. Przejdź pod adres http://localhost:8000/contacts w celu wyświetlenia danych JSON dla całej listy kontaktów.
   
   ```json
    {
        "id": 1,
        "name": "Barney Poland",
        "email": "barney@contoso.com"
    },
    {
        "id": 2,
        "name": "Lacy Barrera",
        "email": "lacy@contoso.com"
    },
    {
        "id": 3,
        "name": "Lora Riggs",
        "email": "lora@contoso.com"
    }
   ```

3. Przejdź pod adres http://localhost:8000/contacts/2 w celu wyświetlenia kontaktu o wartości `id` równej 2.
   
    ```json
    { 
        "id": 2,
        "name": "Lacy Barrera",
        "email": "lacy@contoso.com"
    }
    ```

4. Przetestuj interfejs API za pomocą interfejsu internetowego struktury Swagger pod adresem http://localhost:8000/docs.
   
    ![Interfejs sieci web struktury Swagger](media/app-service-web-tutorial-rest-api/swagger-ui.png)

## <a id="createapiapp"></a> Tworzenie aplikacji interfejsu API

W tej sekcji interfejs wiersza polecenia platformy Azure w wersji 2.0 zostanie użyty do utworzenia zasobów, które będą hostować interfejs API w usłudze Azure App Service. 

1.  Zaloguj się do subskrypcji platformy Azure za pomocą polecenia [az login](/cli/azure/?view=azure-cli-latest#az_login) i postępuj zgodnie z instrukcjami wyświetlanymi na ekranie.

    ```azurecli-interactive
    az login
    ```

2. Jeśli masz wiele subskrypcji platformy Azure, zmień domyślną subskrypcję na żądaną.

    ````azurecli-interactive
    az account set --subscription <name or id>
    ````

3. [!INCLUDE [Create resource group](../../includes/app-service-api-create-resource-group.md)] 

4. [!INCLUDE [Create app service plan](../../includes/app-service-api-create-app-service-plan.md)]

5. [!INCLUDE [Create API app](../../includes/app-service-api-create-api-app.md)] 


## <a name="deploy-the-api-with-git"></a>Wdrażanie interfejsu API za pomocą usługi Git

Wdróż swój kod w aplikacji interfejsu API, wypychając zatwierdzenia z lokalnego repozytorium Git do usługi Azure App Service.

1. [!INCLUDE [Configure your deployment credentials](../../includes/configure-deployment-user-no-h.md)] 

2. Zainicjuj nowe repozytorium w katalogu *ContactList*. 

    ```bash
    git init .
    ```

3. Wyklucz z usługi Git katalog *node_modules* utworzony przez narzędzie npm we wcześniejszym kroku samouczka. Utwórz nowy plik `.gitignore` w bieżącym katalogu i dodaj następujący tekst w nowym wierszu w dowolnym miejscu pliku.

    ```
    node_modules/
    ```
    Upewnij się, że folder `node_modules` jest ignorowany, używając polecenia `git status`.

4. Zatwierdź zmiany w repozytorium.
    ```bash
    git add .
    git commit -m "initial version"
    ```

5. [!INCLUDE [Push to Azure](../../includes/app-service-api-git-push-to-azure.md)]  
 
## <a name="test-the-api--in-azure"></a>Testowanie interfejsu API na platformie Azure

1. Otwórz w przeglądarce adres http://nazwa_aplikacji.azurewebsites.net/contacts. Zwrócone zostaną te same dane JSON, co przy wykonywaniu żądania lokalnie we wcześniejszej części tego samouczka.

   ```json
   {
       "id": 1,
       "name": "Barney Poland",
       "email": "barney@contoso.com"
   },
   {
       "id": 2,
       "name": "Lacy Barrera",
       "email": "lacy@contoso.com"
   },
   {
       "id": 3,
       "name": "Lora Riggs",
       "email": "lora@contoso.com"
   }
   ```

2. W przeglądarce przejdź do punktu końcowego `http://app_name.azurewebsites.net/docs`, aby wypróbować interfejs użytkownika struktury Swagger działający na platformie Azure.

    ![Swagger Ii](media/app-service-web-tutorial-rest-api/swagger-azure-ui.png)

    Teraz możesz wdrażać aktualizacje do przykładowej aplikacji API na platformie Azure przez proste wypychanie zatwierdzeń do repozytorium Git platformy Azure.

## <a name="clean-up"></a>Czyszczenie

Aby wyczyścić wszystkie zasoby utworzone w tym przewodniku szybkiego startu, uruchom następujące polecenie interfejsu wiersza polecenia platformy Azure:

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="next-step"></a>Następny krok 
> [!div class="nextstepaction"]
> [Map an existing custom DNS name to Azure Web Apps (Mapowanie istniejącej niestandardowej nazwy DNS na aplikacje internetowe platformy Azure)](app-service-web-tutorial-custom-domain.md)

