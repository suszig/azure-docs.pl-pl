---
title: "Tworzenie średniej stosu na maszynie Wirtualnej systemu Linux na platformie Azure | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak utworzyć stosu bazy danych MongoDB, Express AngularJS i Node.js (średnia) na maszynie Wirtualnej systemu Linux na platformie Azure."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: davidmu1
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 08/08/2017
ms.author: davidmu
ms.custom: mvc
ms.openlocfilehash: 1d74ead08dfb63276afb08bdcb7f4e3e3db5bfd3
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-mongodb-express-angularjs-and-nodejs-mean-stack-on-a-linux-vm-in-azure"></a>Tworzenie stosu bazy danych MongoDB, Express AngularJS i Node.js (średnia) na maszynie Wirtualnej systemu Linux na platformie Azure

W tym samouczku przedstawiono sposób wykonania stosu bazy danych MongoDB, Express AngularJS i Node.js (średnia) na maszynie Wirtualnej systemu Linux na platformie Azure. ŚREDNIE stosu, który można utworzyć umożliwia dodawanie, usuwanie i wyświetlanie listy książek w bazie danych. Omawiane kwestie:

> [!div class="checklist"]
> * Tworzenie maszyny wirtualnej z systemem Linux
> * Instalowanie środowiska Node.js
> * Instalowanie bazy danych MongoDB i konfigurowanie serwera
> * Instalowanie Express i konfigurowanie tras do serwera
> * Dostęp do trasy z AngularJS
> * Uruchamianie aplikacji

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Jeśli wybierzesz do zainstalowania i używania interfejsu wiersza polecenia lokalnie, w tym samouczku wymaga używasz interfejsu wiersza polecenia Azure w wersji 2.0.4 lub nowszej. Uruchom polecenie `az --version`, aby dowiedzieć się, jaka wersja jest używana. Jeśli konieczna będzie instalacja lub uaktualnienie, zobacz [Instalowanie interfejsu wiersza polecenia platformy Azure 2.0]( /cli/azure/install-azure-cli).


## <a name="create-a-linux-vm"></a>Tworzenie maszyny wirtualnej z systemem Linux

Utwórz nową grupę zasobów o [Tworzenie grupy az](https://docs.microsoft.com/cli/azure/group#az_group_create) polecenie i Utwórz Maszynę wirtualną systemu Linux z [tworzenia maszyny wirtualnej az](https://docs.microsoft.com/cli/azure/vm#az_vm_create) polecenia. Grupa zasobów platformy Azure to logiczny kontener przeznaczony do wdrażania zasobów platformy Azure i zarządzania nimi.

W poniższym przykładzie użyto interfejsu wiersza polecenia Azure, aby utworzyć grupę zasobów o nazwie *myResourceGroupMEAN* w *eastus* lokalizacji. Maszyna wirtualna jest tworzony o nazwie *myVM* z kluczy SSH, jeśli nie już istnieją w domyślnej lokalizacji klucza. Aby użyć określonego zestawu kluczy, użyj opcji ssh klucz wartość.

```azurecli-interactive
az group create --name myResourceGroupMEAN --location eastus
az vm create \
    --resource-group myResourceGroupMEAN \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --admin-password 'Azure12345678!' \
    --generate-ssh-keys
az vm open-port --port 3300 --resource-group myResourceGroupMEAN --name myVM
```

Po utworzeniu maszyny Wirtualnej, interfejsu wiersza polecenia Azure zawiera informacje podobne do poniższego przykładu: 

```azurecli-interactive
{
  "fqdns": "",
  "id": "/subscriptions/{subscription-id}/resourceGroups/myResourceGroupMEAN/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "eastus",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "13.72.77.9",
  "resourceGroup": "myResourceGroupMEAN"
}
```
Zwróć uwagę na element `publicIpAddress`. Ten adres jest używany na potrzeby uzyskiwania dostępu do maszyny wirtualnej.

Użyj następującego polecenia, aby utworzyć sesję SSH z maszyną Wirtualną. Upewnij się użyć poprawne publicznego adresu IP. W naszym przykładzie powyżej naszych IP adres był 13.72.77.9.

```bash
ssh azureuser@13.72.77.9
```

## <a name="install-nodejs"></a>Instalowanie środowiska Node.js

[Node.js](https://nodejs.org/en/) jest oparty na jego Chrome V8 JavaScript aparatu środowiska wykonawczego języka JavaScript. Node.js jest używana w tym samouczku do ustawiania Express trasy i kontrolery AngularJS.

Na maszynie Wirtualnej za pomocą powłoki bash, który został otwarty przy użyciu protokołu SSH zainstaluj środowisko Node.js.

```bash
sudo apt-get install -y nodejs
```

## <a name="install-mongodb-and-set-up-the-server"></a>Instalowanie bazy danych MongoDB i konfigurowanie serwera
[Bazy danych MongoDB](http://www.mongodb.com) przechowuje dane w dokumentach elastyczną i notacji JSON. Pola w bazie danych może się różnić od dokument i struktury danych można zmienić w czasie. Dla naszej aplikacji przykład dodajemy książki rekordów do bazy danych MongoDB, zawierające nazwa, numer isbn, autora i liczba stron. 

1. Na maszynie Wirtualnej za pomocą powłoki bash, który został otwarty przy użyciu protokołu SSH należy ustawić klucz bazy danych MongoDB.

    ```bash
    sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 0C49F3730359A14518585931BC711F9BA15703C6
    echo "deb [ arch=amd64 ] http://repo.mongodb.org/apt/ubuntu trusty/mongodb-org/3.4 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.4.list
    ```

2. Zaktualizuj Menedżera pakietów przy użyciu klucza.
  
    ```bash
    sudo apt-get update
    ```

3. Instalowanie bazy danych MongoDB.

    ```bash
    sudo apt-get install -y mongodb
    ```

4. Uruchom serwer.

    ```bash
    sudo service mongodb start
    ```

5. Należy również zainstalować [treści analizator](https://www.npmjs.com/package/body-parser-json) pakietu, aby pomóc nam przetworzyć JSON przekazano żądania do serwera.

    Zainstaluj Menedżera pakietów npm.

    ```bash
    sudo apt-get install npm
    ```

    Zainstaluj pakiet analizator treści.
    
    ```bash
    sudo npm install body-parser
    ```

6. Utwórz folder o nazwie *książki* i Dodaj plik do niej o nazwie *server.js* zawierający konfiguracji serwera sieci web.

    ```node.js
    var express = require('express');
    var bodyParser = require('body-parser');
    var app = express();
    app.use(express.static(__dirname + '/public'));
    app.use(bodyParser.json());
    require('./apps/routes')(app);
    app.set('port', 3300);
    app.listen(app.get('port'), function() {
        console.log('Server up: http://localhost:' + app.get('port'));
    });
    ```

## <a name="install-express-and-set-up-routes-to-the-server"></a>Instalowanie Express i konfigurowanie tras do serwera

[Express](https://expressjs.com) jest minimalną i elastyczną Node.js platforma aplikacji sieci web zapewniająca funkcje sieci web i aplikacji dla urządzeń przenośnych. Express używany w tym samouczku do przekazywania informacji do i z naszej bazy danych MongoDB skoroszyt. [Mongoose](http://mongoosejs.com) rozwiązaniem jest proste, na podstawie schematu do modelu danych aplikacji. Mongoose jest używana w tym samouczku zapewnienie schematu książki dla bazy danych.

1. Zainstaluj Express i Mongoose.

    ```bash
    sudo npm install express mongoose
    ```

2. W *książki* folderu, Utwórz folder o nazwie *aplikacje* i Dodaj plik o nazwie *routes.js* z ekspresowej trasy zdefiniowane.

    ```node.js
    var Book = require('./models/book');
    module.exports = function(app) {
      app.get('/book', function(req, res) {
        Book.find({}, function(err, result) {
          if ( err ) throw err;
          res.json(result);
        });
      }); 
      app.post('/book', function(req, res) {
        var book = new Book( {
          name:req.body.name,
          isbn:req.body.isbn,
          author:req.body.author,
          pages:req.body.pages
        });
        book.save(function(err, result) {
          if ( err ) throw err;
          res.json( {
            message:"Successfully added book",
            book:result
          });
        });
      });
      app.delete("/book/:isbn", function(req, res) {
        Book.findOneAndRemove(req.query, function(err, result) {
          if ( err ) throw err;
          res.json( {
            message: "Successfully deleted the book",
            book: result
          });
        });
      });
      var path = require('path');
      app.get('*', function(req, res) {
        res.sendfile(path.join(__dirname + '/public', 'index.html'));
      });
    };
    ```

3. W *aplikacje* folderu, Utwórz folder o nazwie *modele* i Dodaj plik o nazwie *book.js* z konfiguracją modelu książki zdefiniowane.  

    ```node.js
    var mongoose = require('mongoose');
    var dbHost = 'mongodb://localhost:27017/test';
    mongoose.connect(dbHost);
    mongoose.connection;
    mongoose.set('debug', true);
    var bookSchema = mongoose.Schema( {
      name: String,
      isbn: {type: String, index: true},
      author: String,
      pages: Number
    });
    var Book = mongoose.model('Book', bookSchema);
    module.exports = mongoose.model('Book', bookSchema); 
    ```

## <a name="access-the-routes-with-angularjs"></a>Dostęp do trasy z AngularJS

[AngularJS](https://angularjs.org) zapewnia platformę sieci web do tworzenia dynamicznych widoków w aplikacji sieci web. W tym samouczku używamy AngularJS do łączenia z naszą stronę sieci web z Express i wykonywać działania na naszych książki bazy danych.

1. Zmień katalog kopii zapasowej do *— książki* (`cd ../..`), a następnie utwórz folder o nazwie *publicznego* i Dodaj plik o nazwie *script.js* z konfiguracją kontrolera zdefiniowane.

    ```node.js
    var app = angular.module('myApp', []);
    app.controller('myCtrl', function($scope, $http) {
      $http( {
        method: 'GET',
        url: '/book'
      }).then(function successCallback(response) {
        $scope.books = response.data;
      }, function errorCallback(response) {
        console.log('Error: ' + response);
      });
      $scope.del_book = function(book) {
        $http( {
          method: 'DELETE',
          url: '/book/:isbn',
          params: {'isbn': book.isbn}
        }).then(function successCallback(response) {
          console.log(response);
        }, function errorCallback(response) {
          console.log('Error: ' + response);
        });
      };
      $scope.add_book = function() {
        var body = '{ "name": "' + $scope.Name + 
        '", "isbn": "' + $scope.Isbn +
        '", "author": "' + $scope.Author + 
        '", "pages": "' + $scope.Pages + '" }';
        $http({
          method: 'POST',
          url: '/book',
          data: body
        }).then(function successCallback(response) {
          console.log(response);
        }, function errorCallback(response) {
          console.log('Error: ' + response);
        });
      };
    });
    ```
    
2. W *publicznego* folderu, Utwórz plik o nazwie *index.html* zdefiniowana strona sieci web.

    ```html
    <!doctype html>
    <html ng-app="myApp" ng-controller="myCtrl">
      <head>
        <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.6.4/angular.min.js"></script>
        <script src="script.js"></script>
      </head>
      <body>
        <div>
          <table>
            <tr>
              <td>Name:</td> 
              <td><input type="text" ng-model="Name"></td>
            </tr>
            <tr>
              <td>Isbn:</td>
              <td><input type="text" ng-model="Isbn"></td>
            </tr>
            <tr>
              <td>Author:</td> 
              <td><input type="text" ng-model="Author"></td>
            </tr>
            <tr>
              <td>Pages:</td>
              <td><input type="number" ng-model="Pages"></td>
            </tr>
          </table>
          <button ng-click="add_book()">Add</button>
        </div>
        <hr>
        <div>
          <table>
            <tr>
              <th>Name</th>
              <th>Isbn</th>
              <th>Author</th>
              <th>Pages</th>
            </tr>
            <tr ng-repeat="book in books">
              <td><input type="button" value="Delete" data-ng-click="del_book(book)"></td>
              <td>{{book.name}}</td>
              <td>{{book.isbn}}</td>
              <td>{{book.author}}</td>
              <td>{{book.pages}}</td>
            </tr>
          </table>
        </div>
      </body>
    </html>
    ```

##  <a name="run-the-application"></a>Uruchamianie aplikacji

1. Zmień katalog kopii zapasowej do *książki* (`cd ..`) i uruchomić serwera, uruchamiając polecenie:

    ```bash
    nodejs server.js
    ```

2. Otwórz przeglądarkę sieci web, adres zarejestrowane dla maszyny Wirtualnej. Na przykład *http://13.72.77.9:3300*. Powinien zostać wyświetlony przypominać następującą stronę:

    ![Rekord książki](media/tutorial-mean/meanstack-init.png)

3. Wprowadź dane do pól tekstowych i kliknij przycisk **Dodaj**. Na przykład:

    ![Dodaj rekord książki](media/tutorial-mean/meanstack-add.png)

4. Po odświeżeniu strony, powinien zostać wyświetlony ekran podobny do tej strony:

    ![Lista rekordów książki](media/tutorial-mean/meanstack-list.png)

5. Można kliknąć przycisk **usunąć** i usuwania rekordu książki z bazy danych.

## <a name="next-steps"></a>Następne kroki

W tym samouczku, utworzyć aplikacji sieci web, który śledzi książki rekordów przy użyciu średniej stosu na maszynie Wirtualnej systemu Linux. W tym samouczku omówiono:

> [!div class="checklist"]
> * Tworzenie maszyny wirtualnej z systemem Linux
> * Instalowanie środowiska Node.js
> * Instalowanie bazy danych MongoDB i konfigurowanie serwera
> * Instalowanie Express i konfigurowanie tras do serwera
> * Dostęp do trasy z AngularJS
> * Uruchamianie aplikacji

Przejście do następnym samouczku, aby dowiedzieć się, jak zabezpieczyć serwerów sieci web za pomocą certyfikatów SSL.

> [!div class="nextstepaction"]
> [Zabezpieczenia serwera sieci web przy użyciu protokołu SSL](tutorial-secure-web-server.md)
