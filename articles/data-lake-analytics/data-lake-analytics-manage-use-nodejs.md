---
title: Zarządzanie usługą Azure Data Lake Analytics za pomocą zestawu Azure SDK dla środowiska Node.js | Microsoft Docs
description: Dowiedz się, jak zarządzać kontami, źródłami danych, zadaniami i użytkownikami usługi Data Lake Analytics za pomocą zestawu Azure SDK dla środowiska Node.js
services: data-lake-analytics
documentationcenter: ''
author: saveenr
manager: kfile
editor: cgronlun
ms.assetid: 9de1bcf4-b15b-4d0b-9284-8889ecf0c438
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/05/2016
ms.author: saveenr
ms.openlocfilehash: f11695df74e30f7ba9b1459f443a720208e542c4
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/16/2018
---
# <a name="manage-azure-data-lake-analytics-using-azure-sdk-for-nodejs"></a>Zarządzanie usługą Azure Data Lake Analytics za pomocą zestawu Azure SDK dla środowiska Node.js
[!INCLUDE [manage-selector](../../includes/data-lake-analytics-selector-manage.md)]

Zestaw Azure SDK dla środowiska Node.js może służyć do zarządzania kontami, zadaniami i katalogami usługi Azure Data Lake Analytics. Aby wyświetlić temat zarządzania za pomocą innych narzędzi, kliknij łącze powyżej.

W tej chwili obsługiwane są:

* **Środowisko Node.js w wersji 0.10.0 lub wyższej**
* **Wersja interfejsu API REST dla konta: 2015-10-01-preview**
* **Wersja interfejsu API REST dla katalogu: 2015-10-01-preview**
* **Wersja interfejsu API REST dla zadania: 2016-03-20-preview**

## <a name="features"></a>Funkcje
* Zarządzanie kontami: tworzenie, pobieranie, wyświetlanie, aktualizowanie i usuwanie.
* Zarządzanie zadaniami: przesyłanie, pobieranie, wyświetlanie i anulowanie.
* Zarządzanie katalogami: pobieranie i wyświetlanie.

## <a name="how-to-install"></a>Jak zainstalować
```bash
npm install azure-arm-datalake-analytics
```

## <a name="authenticate-using-azure-active-directory"></a>Uwierzytelnianie za pomocą usługi Azure Active Directory
 ```javascript
 var msrestAzure = require('ms-rest-azure');
 //user authentication
 var credentials = new msRestAzure.UserTokenCredentials('your-client-id', 'your-domain', 'your-username', 'your-password', 'your-redirect-uri');
 //service principal authentication
 var credentials = new msRestAzure.ApplicationTokenCredentials('your-client-id', 'your-domain', 'your-secret');
 ```

## <a name="create-the-data-lake-analytics-client"></a>Tworzenie klienta Data Lake Analytics
```javascript
var adlaManagement = require("azure-arm-datalake-analytics");
var acccountClient = new adlaManagement.DataLakeAnalyticsAccountClient(credentials, 'your-subscription-id');
var jobClient = new adlaManagement.DataLakeAnalyticsJobClient(credentials, 'azuredatalakeanalytics.net');
var catalogClient = new adlaManagement.DataLakeAnalyticsCatalogClient(credentials, 'azuredatalakeanalytics.net');
```

## <a name="create-a-data-lake-analytics-account"></a>Tworzenie konta Data Lake Analytics
```javascript
var util = require('util');
var resourceGroupName = 'testrg';
var accountName = 'testadlaacct';
var location = 'eastus2';

// A Data Lake Store account must already have been created to create
// a Data Lake Analytics account. See the Data Lake Store readme for
// information on doing so. For now, we assume one exists already.
var datalakeStoreAccountName = 'existingadlsaccount';

// account object to create
var accountToCreate = {
  tags: {
    testtag1: 'testvalue1',
    testtag2: 'testvalue2'
  },
  name: accountName,
  location: location,
  properties: {
    defaultDataLakeStoreAccount: datalakeStoreAccountName,
    dataLakeStoreAccounts: [
      {
        name: datalakeStoreAccountName
      }
    ]
  }
};

client.account.create(resourceGroupName, accountName, accountToCreate, function (err, result, request, response) {
  if (err) {
    console.log(err);
    /*err has reference to the actual request and response, so you can see what was sent and received on the wire.
      The structure of err looks like this:
      err: {
        code: 'Error Code',
        message: 'Error Message',
        body: 'The response body if any',
        request: reference to a stripped version of http request
        response: reference to a stripped version of the response
      }
    */
  } else {
    console.log('result is: ' + util.inspect(result, {depth: null}));
  }
});
```

## <a name="get-a-list-of-jobs"></a>Pobieranie listy zadań
```javascript
var util = require('util');
var accountName = 'testadlaacct';
jobClient.job.list(accountName, function (err, result, request, response) {
  if (err) {
    console.log(err);
  } else {
    console.log('result is: ' + util.inspect(result, {depth: null}));
  }
});
```

## <a name="get-a-list-of-databases-in-the-data-lake-analytics-catalog"></a>Pobieranie listy baz danych w katalogu Data Lake Analytics
```javascript
var util = require('util');
var accountName = 'testadlaacct';
catalogClient.catalog.listDatabases(accountName, function (err, result, request, response) {
  if (err) {
    console.log(err);
  } else {
    console.log('result is: ' + util.inspect(result, {depth: null}));
  }
});
```

## <a name="see-also"></a>Zobacz też
* [Zestaw Microsoft Azure SDK dla środowiska Node.js](https://github.com/azure/azure-sdk-for-node)
* [Zestaw Microsoft Azure SDK dla środowiska Node.js — Data Lake Store Management](https://github.com/Azure/azure-sdk-for-node/tree/autorest/lib/services/dataLake.Store)

