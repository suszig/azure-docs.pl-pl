---
title: "Jak rozpocząć pracę z magazynu tabel i Visual Studio połączone usługi (platformy ASP.NET Core) | Dokumentacja firmy Microsoft"
description: "Jak rozpocząć pracę z magazynem tabel Azure w projekcie platformy ASP.NET Core w programie Visual Studio po połączeniu z kontem magazynu za pomocą programu Visual Studio połączone usługi"
services: storage
documentationcenter: 
author: kraigb
manager: ghogen
editor: 
ms.assetid: c3c451d1-71ff-4222-a348-c41c98a02b85
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: kraigb
ms.openlocfilehash: 8d05fe3ed9a5c66f186a930d4107162c1f322c05
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-get-started-with-azure-table-storage-and-visual-studio-connected-services"></a>Jak rozpocząć pracę z magazynem tabel Azure i programu Visual Studio połączone usługi
[!INCLUDE [storage-try-azure-tools-tables](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>Omówienie
W tym artykule opisano sposób rozpoczęcie pracy z magazynem tabel Azure w programie Visual Studio po utworzony lub odwołanie do konta magazynu Azure w projekcie platformy ASP.NET Core za pomocą programu Visual Studio **dodać usług połączonych** okna dialogowego.

Usługa Azure Table storage umożliwia przechowywania dużych ilości danych strukturalnych. Usługa jest magazynem danych NoSQL, który przyjmuje uwierzytelnione wywołania z wewnątrz lub na zewnątrz w chmurze Azure. Tabele Azure idealnie nadają się do przechowywania strukturalnych danych nierelacyjnych.

**Dodać usług połączonych** operacji instaluje odpowiednie pakiety NuGet dostęp do magazynu Azure do projektu i dodaje ten ciąg połączenia dla konta magazynu do plików konfiguracji projektu.

Aby uzyskać więcej ogólnych informacji o korzystaniu z magazynem tabel Azure, zobacz [Rozpoczynanie pracy z magazynem tabel Azure przy użyciu platformy .NET](../storage/storage-dotnet-how-to-use-tables.md).

Aby rozpocząć pracę, należy najpierw utwórz tabelę na koncie magazynu. Poniżej opisano sposób tworzenia tabeli platformy Azure w kodzie. Możemy również opisano sposób wykonywania tabeli podstawowej i jednostki operacje, takie jak dodawanie, modyfikowanie, Odczyt i Odczyt jednostek tabeli. Przykłady są napisane w języku C\# kodu i używanie biblioteki klienta magazynu Azure dla platformy .NET.

**Uwaga** -niektórych interfejsów API, które wykonywania wywołań limit magazynu Azure w ASP.NET Core są asynchroniczne. Zobacz [programowanie asynchroniczne z Async i Await](http://msdn.microsoft.com/library/hh191443.aspx) Aby uzyskać więcej informacji. Poniższy kod przyjęto założenie, że są używane metody programowania asynchronicznego.

## <a name="access-tables-in-code"></a>Dostęp do tabel w kodzie
Aby uzyskać dostęp do tabel w projektach platformy ASP.NET Core, musisz obejmują następujące elementy do plików źródłowych C# które uzyskują dostęp do magazynu tabel platformy Azure.

1. Upewnij się, że deklaracje przestrzeni nazw w górnej części pliku C# Uwzględnij je **przy użyciu** instrukcje.
   
        using Microsoft.Framework.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Table;
        using System.Threading.Tasks;
        using LogLevel = Microsoft.Framework.Logging.LogLevel;
2. Pobierz **CloudStorageAccount** obiekt, który reprezentuje informacje o koncie magazynu. Użyj następującego kodu można pobrać parametry połączenia magazynu, a informacje o koncie magazynu z konfiguracji usługi Azure.
   
        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
            CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
   
    **Uwaga** -korzystać ze wszystkich powyższych kodu przed kod w następujących przykładach.
3. Pobierz **CloudTableClient** obiekt, aby odwoływać się do obiektów tabeli na koncie magazynu.  
   
        // Create the table client.
        CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
4. Pobierz **CloudTable** obiektu odwołania, aby odwoływać się do określonej tabeli i jednostek.
   
        // Get a reference to a table named "peopleTable"
        CloudTable table = tableClient.GetTableReference("peopleTable");

## <a name="create-a-table-in-code"></a>Utwórz tabelę w kodzie
Aby utworzyć tabeli platformy Azure, po prostu dodaj wywołanie **CreateIfNotExistsAsync()**.

    // Create the CloudTable if it does not exist
    await table.CreateIfNotExistsAsync();

## <a name="add-an-entity-to-a-table"></a>Dodawanie jednostki do tabeli
Aby dodać jednostkę do tabeli należy utworzyć klasę, która definiuje właściwości jednostki. Poniższy kod definiuje klasę jednostki nazywane **CustomerEntity** używającej imienia klienta jako klucza wiersza i nazwiska jako klucza partycji.

    public class CustomerEntity : TableEntity
    {
        public CustomerEntity(string lastName, string firstName)
        {
            this.PartitionKey = lastName;
            this.RowKey = firstName;
        }

        public CustomerEntity() { }

        public string Email { get; set; }

        public string PhoneNumber { get; set; }
    }

Operacje tabeli obejmujące jednostki są wykonywane przy użyciu **CloudTable** obiekt został utworzony we wcześniejszej części "Dostęp do tabel w kodzie." **TableOperation** obiekt reprezentuje operacji do wykonania. W poniższym przykładzie przedstawiono sposób tworzenia **CloudTable** obiektu i **CustomerEntity** obiektu. Aby przygotować operację, **TableOperation** służy do wstawiania jednostek klienta w tabeli. Na koniec operacji jest wykonywana przez wywołanie CloudTable.ExecuteAsync.

    // Create a new customer entity.
    CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
    customer1.Email = "Walter@contoso.com";
    customer1.PhoneNumber = "425-555-0101";

    // Create the TableOperation that inserts the customer entity.
    TableOperation insertOperation = TableOperation.Insert(customer1);

    // Execute the insert operation.
    await peopleTable.ExecuteAsync(insertOperation);

## <a name="insert-a-batch-of-entities"></a>Zbiorcze wstawianie jednostek
Wiele jednostek można wstawiać do tabeli w operacji zapisu pojedynczego. Poniższy przykład kodu tworzy dwa obiekty jednostki ("Jan Kowalski" i "Ben Smith"), dodanie ich do **TableBatchOperation** przy użyciu **Wstaw** metody, a następnie uruchamia przez wywołanie operacji CloudTable.ExecuteBatchAsync.

    // Create the batch operation.
    TableBatchOperation batchOperation = new TableBatchOperation();

    // Create a customer entity and add it to the table.
    CustomerEntity customer1 = new CustomerEntity("Smith", "Jeff");
    customer1.Email = "Jeff@contoso.com";
    customer1.PhoneNumber = "425-555-0104";

    // Create another customer entity and add it to the table.
    CustomerEntity customer2 = new CustomerEntity("Smith", "Ben");
    customer2.Email = "Ben@contoso.com";
    customer2.PhoneNumber = "425-555-0102";

    // Add both customer entities to the batch insert operation.
    batchOperation.Insert(customer1);
    batchOperation.Insert(customer2);

    // Execute the batch operation.
    await peopleTable.ExecuteBatchAsync(batchOperation);

## <a name="get-all-of-the-entities-in-a-partition"></a>Pobieranie wszystkich jednostek w partycji
Aby sprawdzić tabeli dla wszystkich jednostek w partycji, użyj **TableQuery** obiektu. Poniższy przykład kodu określa filtr jednostek, gdzie „Smith” jest kluczem partycji. W tym przykładzie drukowane są pola każdej jednostki w wynikach zapytania w konsoli.

    // Construct the query operation for all customer entities where PartitionKey="Smith".
    TableQuery<CustomerEntity> query = new TableQuery<CustomerEntity>().Where(TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, "Smith"));

    // Print the fields for each customer.
    TableContinuationToken token = null;
    do
    {
        TableQuerySegment<CustomerEntity> resultSegment = await peopleTable.ExecuteQuerySegmentedAsync(query, token);
        token = resultSegment.ContinuationToken;

        foreach (CustomerEntity entity in resultSegment.Results)
        {
            Console.WriteLine("{0}, {1}\t{2}\t{3}", entity.PartitionKey, entity.RowKey,
            entity.Email, entity.PhoneNumber);
        }
    } while (token != null);

## <a name="get-a-single-entity"></a>Pobierz pojedynczy element
Można napisać zapytanie do pobrania jednej, określonej jednostki. Poniższy kod używa **TableOperation** obiekt, aby określić klienta o nazwie "Ben Smith". Ta metoda zwraca tylko jedną jednostkę zamiast kolekcji, a zwrócona wartość w **TableResult.Result** jest **CustomerEntity** obiektu. Określenie kluczy partycji i wiersza w zapytaniu jest najszybszym sposobem na pobranie jednej jednostki z **tabeli** usługi.

    // Create a retrieve operation that takes a customer entity.
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

    // Execute the retrieve operation.
    TableResult retrievedResult = await peopleTable.ExecuteAsync(retrieveOperation);

    // Print the phone number of the result.
    if (retrievedResult.Result != null)
       Console.WriteLine(((CustomerEntity)retrievedResult.Result).PhoneNumber);
    else
       Console.WriteLine("The phone number could not be retrieved.");

## <a name="delete-an-entity"></a>Usuwanie jednostki
Po możesz znaleźć, można usunąć jednostki. Następujący kod szuka jednostki klienta o nazwie "Ben Smith", a następnie przypadku ich znalezienia, usuwa ją.

    // Create a retrieve operation that expects a customer entity.
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

    // Execute the operation.
    TableResult retrievedResult = peopleTable.Execute(retrieveOperation);

    // Assign the result to a CustomerEntity object.
    CustomerEntity deleteEntity = (CustomerEntity)retrievedResult.Result;

    // Create the Delete TableOperation and then execute it.
    if (deleteEntity != null)
    {
       TableOperation deleteOperation = TableOperation.Delete(deleteEntity);

       // Execute the operation.
       await peopleTable.ExecuteAsync(deleteOperation);

       Console.WriteLine("Entity deleted.");
    }

    else
       Console.WriteLine("Couldn't delete the entity.");

## <a name="next-steps"></a>Następne kroki
[!INCLUDE [vs-storage-dotnet-tables-next-steps](../../includes/vs-storage-dotnet-tables-next-steps.md)]

