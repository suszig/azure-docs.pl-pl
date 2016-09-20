<properties
   pageTitle="Zarządzanie usługą Azure Data Lake Analytics za pomocą zestawu Azure SDK dla środowiska Node.js | Azure"
   description="Dowiedz się, jak zarządzać kontami, źródłami danych, zadaniami i użytkownikami usługi Data Lake Analytics za pomocą zestawu Azure SDK dla środowiska Node.js"
   services="data-lake-analytics"
   documentationCenter=""
   authors="edmacauley"
   manager="paulettm"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="05/16/2016"
   ms.author="edmaca"/>

# Zarządzanie usługą Azure Data Lake Analytics za pomocą zestawu Azure SDK dla środowiska Node.js


[AZURE.INCLUDE [manage-selector](../../includes/data-lake-analytics-selector-manage.md)]

Zestaw Azure SDK dla środowiska Node.js może służyć do zarządzania kontami, zadaniami i katalogami usługi Azure Data Lake Analytics. Aby wyświetlić temat zarządzania za pomocą innych narzędzi, kliknij łącze powyżej.

W tej chwili obsługiwane są:

  *  **Środowisko Node.js w wersji 0.10.0 lub wyższej**
  *  **Wersja interfejsu API REST dla konta: 2015-10-01-preview**
  *  **Wersja interfejsu API REST dla katalogu: 2015-10-01-preview**
  *  **Wersja interfejsu API REST dla zadania: 2016-03-20-preview**

## Funkcje

- Zarządzanie kontami: tworzenie, pobieranie, wyświetlanie, aktualizowanie i usuwanie.
- Zarządzanie zadaniami: przesyłanie, pobieranie, wyświetlanie, anulowanie.
- Zarządzanie katalogami: pobieranie, wyświetlanie, tworzenie (hasła), aktualizowanie (hasła), usuwanie (hasła).

## Jak zainstalować

```bash
npm install azure-arm-datalake-analytics
```

## Uwierzytelnianie za pomocą usługi Azure Active Directory

 ```javascript
 var msrestAzure = require('ms-rest-azure');
 //user authentication
 var credentials = new msRestAzure.UserTokenCredentials('your-client-id', 'your-domain', 'your-username', 'your-password', 'your-redirect-uri');
 //service principal authentication
 var credentials = new msRestAzure.ApplicationTokenCredentials('your-client-id', 'your-domain', 'your-secret');
 ```

## Tworzenie klienta Data Lake Analytics

```javascript
var adlaManagement = require("azure-arm-datalake-analytics");
var acccountClient = new adlaManagement.DataLakeAnalyticsAccountClient(credentials, 'your-subscription-id');
var jobClient = new adlaManagement.DataLakeAnalyticsJobClient(credentials, 'azuredatalakeanalytics.net');
var catalogClient = new adlaManagement.DataLakeAnalyticsCatalogClient(credentials, 'azuredatalakeanalytics.net');
```

## Tworzenie konta Data Lake Analytics

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

## Pobieranie listy zadań

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

## Pobieranie listy baz danych w katalogu Data Lake Analytics
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

## Zobacz też

- [Zestaw Microsoft Azure SDK dla środowiska Node.js](https://github.com/azure/azure-sdk-for-node)
- [Zestaw Microsoft Azure SDK dla środowiska Node.js — Data Lake Store Management](https://github.com/Azure/azure-sdk-for-node/tree/autorest/lib/services/dataLake.Store)


<!--HONumber=sep16_HO1-->


