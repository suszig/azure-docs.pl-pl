---
title: "Samouczek usługi Azure DB rozwiązania Cosmos dystrybucji globalne dla interfejsu API usługi DocumentDB | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak skonfigurować bazy danych Azure rozwiązania Cosmos dystrybucji globalnego przy użyciu interfejsu API usługi DocumentDB."
services: cosmos-db
keywords: "globalne dystrybucji, z usługi documentdb"
documentationcenter: 
author: mimig1
manager: jhubbard
editor: cgronlun
ms.assetid: 8b815047-2868-4b10-af1d-40a1af419a70
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 05/10/2017
ms.author: mimig
ms.custom: mvc
ms.openlocfilehash: c3d0d46ac12faa6b1e28edbeadd97c1a987bed1e
ms.sourcegitcommit: 7136d06474dd20bb8ef6a821c8d7e31edf3a2820
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/05/2017
---
# <a name="how-to-setup-azure-cosmos-db-global-distribution-using-the-documentdb-api"></a>Konfigurowanie bazy danych Azure rozwiązania Cosmos dystrybucji globalnego przy użyciu interfejsu API usługi DocumentDB

W tym artykule zostanie przedstawiony sposób instalacji bazy danych Azure rozwiązania Cosmos globalne dystrybucji, a następnie nawiąż połączenie przy użyciu interfejsu API usługi DocumentDB za pomocą portalu Azure.

W tym artykule opisano następujące zadania: 

> [!div class="checklist"]
> * Skonfiguruj globalne dystrybucji przy użyciu portalu Azure
> * Skonfigurować globalne dystrybucji za pomocą [interfejsów API usługi DocumentDB](documentdb-introduction.md)

<a id="portal"></a>
[!INCLUDE [cosmos-db-tutorial-global-distribution-portal](../../includes/cosmos-db-tutorial-global-distribution-portal.md)]


## <a name="connecting-to-a-preferred-region-using-the-documentdb-api"></a>Łączenie z preferowanego regionu przy użyciu interfejsu API usługi DocumentDB

Aby korzystać z [globalne dystrybucji](distribute-data-globally.md), aplikacje klienckie można określić listy uporządkowanej preferencji regionów ma być używany do wykonywania operacji dokumentu. Można to zrobić przez ustawienie zasad połączenia. Na podstawie konfiguracji konta bazy danych Azure rozwiązania Cosmos, bieżącej dostępności regionalnych i na liście preferencji określone, optymalny punkt końcowy zostanie wybrany przez zestaw SDK usługi DocumentDB do wykonywania zapisu i operacji odczytu.

Ta lista preferencji został określony podczas inicjowania połączenia przy użyciu zestawów SDK usługi DocumentDB. Zestawy SDK zaakceptować opcjonalny parametr "PreferredLocations" oznacza to uporządkowana lista regionów platformy Azure.

Zestaw SDK będzie automatycznie wysyłać zapisuje wszystkie dane w bieżącej zapisu regionu.

Wszystkie operacje odczytu zostaną wysłane do pierwszy dostępny region na liście PreferredLocations. Jeśli żądanie kończy się niepowodzeniem, klient się nie powieść w dół na liście, aby następny region i tak dalej.

Zestawy SDK podejmie próbę odczytu z określonych w PreferredLocations regionów. Tak na przykład, jeśli konto bazy danych jest dostępne w czterech regionów, ale klient tylko określa dwóch regionach read(non-write) PreferredLocations, następnie odczyty nie zostanie obsłużona poza region odczytu, która nie jest określona w PreferredLocations. Jeśli określonych w PreferredLocations regionów odczytu nie są dostępne, odczyty zostanie obsłużona poza region zapisu.

Aplikacja może sprawdź bieżący punkt końcowy zapisu i odczytu punktu końcowego wybrany przez zestaw SDK, sprawdzając dwie właściwości WriteEndpoint i ReadEndpoint dostępne w wersji zestawu SDK 1.8 i powyżej.

Jeśli nie ustawiono właściwości PreferredLocations, zostanie obsłużona wszystkie żądania z bieżącego obszaru zapisu.

## <a name="net-sdk"></a>Zestaw SDK .NET
Zestaw SDK może służyć bez wprowadzania żadnych zmian kodu. W takim przypadku zestawu SDK automatycznie kieruje zarówno odczytuje i zapisuje bieżący obszar zapisu.

W wersji 1.8 i później zestawu .NET SDK parametr ConnectionPolicy dla konstruktora DocumentClient ma właściwość o nazwie Microsoft.Azure.Documents.ConnectionPolicy.PreferredLocations. Ta właściwość jest typu kolekcji `<string>` i powinien zawierać listę nazw regionu. Ciągi sformatowane w kolumnie Nazwa regionu na [regiony platformy Azure] [ regions] strony nie może zawierać spacji przed lub po pierwszy i ostatni znak odpowiednio.

Bieżący zapisu i odczytu punkty końcowe są odpowiednio dostępne w DocumentClient.WriteEndpoint i DocumentClient.ReadEndpoint.

> [!NOTE]
> Nie należy traktować jako długotrwałe stałe adresy URL punktów końcowych. Usługa może aktualizować je w dowolnym momencie. Zestaw SDK obsługuje automatycznie tej zmiany.
>
>

```csharp
// Getting endpoints from application settings or other configuration location
Uri accountEndPoint = new Uri(Properties.Settings.Default.GlobalDatabaseUri);
string accountKey = Properties.Settings.Default.GlobalDatabaseKey;
  
ConnectionPolicy connectionPolicy = new ConnectionPolicy();

//Setting read region selection preference
connectionPolicy.PreferredLocations.Add(LocationNames.WestUS); // first preference
connectionPolicy.PreferredLocations.Add(LocationNames.EastUS); // second preference
connectionPolicy.PreferredLocations.Add(LocationNames.NorthEurope); // third preference

// initialize connection
DocumentClient docClient = new DocumentClient(
    accountEndPoint,
    accountKey,
    connectionPolicy);

// connect to DocDB
await docClient.OpenAsync().ConfigureAwait(false);
```

## <a name="nodejs-javascript-and-python-sdks"></a>NodeJS, JavaScript i zestawy SDK Python
Zestaw SDK może służyć bez wprowadzania żadnych zmian kodu. W takim przypadku zestawu SDK będą automatycznie bezpośrednie, odczytów i zapisów do bieżącego zapisu regionu.

W wersji 1.8 i nowszych każdego zestawu SDK, ConnectionPolicy parametr dla konstruktora DocumentClient nową właściwość o nazwie DocumentClient.ConnectionPolicy.PreferredLocations. Ten parametr jest tablicą ciągów przyjmuje listę nazw regionu. Nazwy są sformatowane w kolumnie Nazwa regionu w [regiony platformy Azure] [ regions] strony. Umożliwia także stałe wstępnie zdefiniowane w obiekcie wygody AzureDocuments.Regions

Bieżący zapisu i odczytu punkty końcowe są odpowiednio dostępne w DocumentClient.getWriteEndpoint i DocumentClient.getReadEndpoint.

> [!NOTE]
> Nie należy traktować jako długotrwałe stałe adresy URL punktów końcowych. Usługa może aktualizować je w dowolnym momencie. Zestaw SDK zostanie automatycznie obsłużyć tej zmiany.
>
>

Poniżej podano przykładowy kod NodeJS/JavaScript. Python i Java są zgodne z tego samego wzorca.

```java
// Creating a ConnectionPolicy object
var connectionPolicy = new DocumentBase.ConnectionPolicy();

// Setting read region selection preference, in the following order -
// 1 - West US
// 2 - East US
// 3 - North Europe
connectionPolicy.PreferredLocations = ['West US', 'East US', 'North Europe'];

// initialize the connection
var client = new DocumentDBClient(host, { masterKey: masterKey }, connectionPolicy);
```

## <a name="rest"></a>REST
Gdy konto bazy danych został udostępniony w wielu regionach, klienci mogą wykonywać kwerendę jego dostępność, wykonując na następujący identyfikator URI żądania GET.

    https://{databaseaccount}.documents.azure.com/

Usługa zwróci listę regionów i ich odpowiednich bazy danych Azure rozwiązania Cosmos punktu końcowego identyfikatorów URI dla replik. Bieżący obszar zapisu zostanie wskazany w odpowiedzi. Klient może następnie wybrać odpowiednie punktu końcowego wszystkie kolejne żądania interfejsu API REST w następujący sposób.

Przykład odpowiedzi

    {
        "_dbs": "//dbs/",
        "media": "//media/",
        "writableLocations": [
            {
                "Name": "West US",
                "DatabaseAccountEndpoint": "https://globaldbexample-westus.documents.azure.com:443/"
            }
        ],
        "readableLocations": [
            {
                "Name": "East US",
                "DatabaseAccountEndpoint": "https://globaldbexample-eastus.documents.azure.com:443/"
            }
        ],
        "MaxMediaStorageUsageInMB": 2048,
        "MediaStorageUsageInMB": 0,
        "ConsistencyPolicy": {
            "defaultConsistencyLevel": "Session",
            "maxStalenessPrefix": 100,
            "maxIntervalInSeconds": 5
        },
        "addresses": "//addresses/",
        "id": "globaldbexample",
        "_rid": "globaldbexample.documents.azure.com",
        "_self": "",
        "_ts": 0,
        "_etag": null
    }


* Przejdź do zapisu wskazany identyfikator URI żądania PUT, POST i DELETE
* Pobiera wszystkie i inne żądania tylko do odczytu (na przykład kwerendy) może go do dowolnego punktu końcowego wybór klienta

Zapisu żądania do regionów tylko do odczytu zakończy się niepowodzeniem z kodem błędu HTTP 403 ("Dostęp zabroniony").

Jeśli region zapisu zmieni się po fazie początkowe wykrywanie klienta, kolejnych zapisów do poprzedniego regionu zapisu zakończy się niepowodzeniem z kodem błędu HTTP 403 ("Dostęp zabroniony"). Klienta, należy UZYSKAĆ listę regionów ponownie, aby pobrać regionu zaktualizowane zapisu.

To wszystko, która kończy w tym samouczku. Znajdują się informacje dotyczące zarządzania spójności konta globalnie replikowanych odczytując [poziomów spójności w usłudze Azure DB rozwiązania Cosmos](consistency-levels.md). I uzyskać więcej informacji na temat sposobu globalnej replikacji bazy danych działa w usłudze Azure DB rozwiązania Cosmos, zobacz [dystrybucji danych globalnie z bazy danych Azure rozwiązania Cosmos](distribute-data-globally.md).

## <a name="next-steps"></a>Następne kroki

W tym samouczku wykonaniu następujących czynności:

> [!div class="checklist"]
> * Skonfiguruj globalne dystrybucji przy użyciu portalu Azure
> * Skonfiguruj globalne dystrybucji przy użyciu interfejsów API usługi DocumentDB

Możesz teraz przejść do następnym samouczku, aby dowiedzieć się, jak opracowywać lokalnie przy użyciu emulatora lokalnej bazy danych Azure rozwiązania Cosmos.

> [!div class="nextstepaction"]
> [Opracowywanie lokalnie w emulatorze](local-emulator.md)

[regions]: https://azure.microsoft.com/regions/

