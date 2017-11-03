---
title: "Tworzenie aplikacji sieci web za pomocą interfejsu API Azure rozwiązania Cosmos DB dla bazy danych MongoDB | Dokumentacja firmy Microsoft"
description: "Samouczek bazy danych Azure rozwiązania Cosmos, który tworzy aplikacji sieci web bazy danych w trybie online za pomocą interfejsu API bazy danych mongodb."
keywords: "Przykłady bazy danych mongodb"
services: cosmos-db
author: AndrewHoh
manager: jhubbard
editor: 
documentationcenter: 
ms.assetid: 61a2ab3a-2fc3-4d49-a263-ed87c66628f6
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 05/10/2017
ms.author: anhoh
ms.custom: mvc
ms.openlocfilehash: a0598d32b5bad240c0a5d77a6e19285115a9f6b0
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-cosmos-db-connect-to-a-mongodb-app-using-net"></a>Azure rozwiązania Cosmos bazy danych: Połącz w aplikacji bazy danych MongoDB przy użyciu platformy .NET

Azure Cosmos DB to rozproszona globalnie wielomodelowa usługa bazy danych firmy Microsoft. Dzięki wykorzystaniu dystrybucji globalnej i możliwości skalowania poziomego opartego na usłudze Azure Cosmos DB, można szybko tworzyć i za pomocą zapytań badać bazy danych dokumentów, par klucz/wartość i grafów. 

W tym samouczku pokazano, jak utworzyć konto bazy danych rozwiązania Cosmos Azure przy użyciu portalu Azure i sposobu tworzenia bazy danych i kolekcji do przechowywania danych przy użyciu [API bazy danych MongoDB](mongodb-introduction.md). 

Ten samouczek obejmuje następujące zadania:

> [!div class="checklist"]
> * Tworzenie konta usługi Azure Cosmos DB 
> * Aktualizowanie parametrów połączenia
> * Tworzenie aplikacji bazy danych MongoDB na maszynie wirtualnej 


## <a name="create-a-database-account"></a>Tworzenie konta bazy danych

Zacznijmy od utworzenia konta Azure DB rozwiązania Cosmos w portalu Azure.  

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F). 
[!INCLUDE [cosmos-db-emulator-mongodb](../../includes/cosmos-db-emulator-mongodb.md)]

> [!TIP]
> * Masz już konto bazy danych rozwiązania Cosmos Azure? Jeśli tak, przejdź do [konfigurowanie rozwiązania Visual Studio](#SetupVS)
> * Czy miał konto usługi Azure DocumentDB? Jeśli tak, Twoje konto jest kontem bazy danych Azure rozwiązania Cosmos i możesz przejść od razu do [konfigurowanie rozwiązania Visual Studio](#SetupVS).  
> * Jeśli używasz emulatora usługi Azure rozwiązania Cosmos bazy danych, wykonaj kroki opisane w temacie [Azure rozwiązania Cosmos DB emulatora](local-emulator.md) skonfigurować emulatora i przejść od razu do [konfigurowanie rozwiązania Visual Studio](#SetupVS). 
>
>

[!INCLUDE [cosmos-db-create-dbaccount-mongodb](../../includes/cosmos-db-create-dbaccount-mongodb.md)]

## <a name="update-your-connection-string"></a>Aktualizowanie parametrów połączenia

1. W portalu Azure w **bazy danych Azure rozwiązania Cosmos** wybierz interfejsu API dla konta bazy danych MongoDB. 
2. Na pasku po lewej stronie w bloku konta, kliknij **szybki start**. 
3. Wybierz platformy (*sterownik .NET*, *sterownika Node.js*, *powłoki MongoDB*, *sterownika Java*, *Python sterownik*). Jeśli nie widzisz z sterownika lub narzędzia wymienione, nie martw się, firma Microsoft stale dokumentu więcej wstawki kodu połączenia. 
4. Kopiowanie i wklejanie fragmentu kodu w aplikacji bazy danych MongoDB i jest gotowy do działania.

## <a name="set-up-your-mongodb-app"></a>Konfigurowanie aplikacji bazy danych MongoDB

Można użyć [tworzenie aplikacji sieci web na platformie Azure, łączącego się bazy danych MongoDB uruchomione na maszynie wirtualnej](../app-service/app-service-web-tutorial-nodejs-mongodb-app.md) samouczek z minimalnym modyfikacji, aby szybko skonfigurować aplikacji bazy danych MongoDB (albo lokalnie lub opublikowane do aplikacji sieci web platformy Azure) łączące do interfejsu API dla konta bazy danych MongoDB.  

1. Postępuj zgodnie z samouczka w jeden modyfikacji.  Zastąp kod Dal.cs to:

    ```csharp   
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Web;
    using MyTaskListApp.Models;
    using MongoDB.Driver;
    using MongoDB.Bson;
    using System.Configuration;
    using System.Security.Authentication;
   
    namespace MyTaskListApp
    {
        public class Dal : IDisposable
        {
            //private MongoServer mongoServer = null;
            private bool disposed = false;
   
            // To do: update the connection string with the DNS name
            // or IP address of your server. 
            //For example, "mongodb://testlinux.cloudapp.net
            private string connectionString = "mongodb://localhost:27017";
            private string userName = "<your user name>";
            private string host = "<your host>";
            private string password = "<your password>";
   
            // This sample uses a database named "Tasks" and a 
            //collection named "TasksList".  The database and collection 
            //will be automatically created if they don't already exist.
            private string dbName = "Tasks";
            private string collectionName = "TasksList";
   
            // Default constructor.        
            public Dal()
            {
            }
   
            // Gets all Task items from the MongoDB server.        
            public List<MyTask> GetAllTasks()
            {
                try
                {
                    var collection = GetTasksCollection();
                    return collection.Find(new BsonDocument()).ToList();
                }
                catch (MongoConnectionException)
                {
                    return new List<MyTask>();
                }
            }
   
            // Creates a Task and inserts it into the collection in MongoDB.
            public void CreateTask(MyTask task)
            {
                var collection = GetTasksCollectionForEdit();
                try
                {
                    collection.InsertOne(task);
                }
                catch (MongoCommandException ex)
                {
                    string msg = ex.Message;
                }
            }
   
            private IMongoCollection<MyTask> GetTasksCollection()
            {
                MongoClientSettings settings = new MongoClientSettings();
                settings.Server = new MongoServerAddress(host, 10255);
                settings.UseSsl = true;
                settings.SslSettings = new SslSettings();
                settings.SslSettings.EnabledSslProtocols = SslProtocols.Tls12;
   
                MongoIdentity identity = new MongoInternalIdentity(dbName, userName);
                MongoIdentityEvidence evidence = new PasswordEvidence(password);
   
                settings.Credentials = new List<MongoCredential>()
                {
                    new MongoCredential("SCRAM-SHA-1", identity, evidence)
                };
   
                MongoClient client = new MongoClient(settings);
                var database = client.GetDatabase(dbName);
                var todoTaskCollection = database.GetCollection<MyTask>(collectionName);
                return todoTaskCollection;
            }
   
            private IMongoCollection<MyTask> GetTasksCollectionForEdit()
            {
                MongoClientSettings settings = new MongoClientSettings();
                settings.Server = new MongoServerAddress(host, 10255);
                settings.UseSsl = true;
                settings.SslSettings = new SslSettings();
                settings.SslSettings.EnabledSslProtocols = SslProtocols.Tls12;
   
                MongoIdentity identity = new MongoInternalIdentity(dbName, userName);
                MongoIdentityEvidence evidence = new PasswordEvidence(password);
   
                settings.Credentials = new List<MongoCredential>()
                {
                    new MongoCredential("SCRAM-SHA-1", identity, evidence)
                };
                MongoClient client = new MongoClient(settings);
                var database = client.GetDatabase(dbName);
                var todoTaskCollection = database.GetCollection<MyTask>(collectionName);
                return todoTaskCollection;
            }
   
            # region IDisposable
   
            public void Dispose()
            {
                this.Dispose(true);
                GC.SuppressFinalize(this);
            }
   
            protected virtual void Dispose(bool disposing)
            {
                if (!this.disposed)
                {
                    if (disposing)
                    {
                    }
                }
   
                this.disposed = true;
            }
   
            # endregion
        }
    }
    ```

2. Zmodyfikuj następujące zmienne w pliku Dal.cs na ustawienia konta, na stronie klucze w portalu Azure:

    ```csharp   
    private string userName = "<your user name>";
    private string host = "<your host>";
    private string password = "<your password>";
    ```

3. Przy użyciu tej aplikacji!

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Jeśli nie zamierzasz w przyszłości korzystać z tej aplikacji, wykonaj następujące czynności, aby usunąć wszystkie zasoby utworzone w witrynie Azure Portal w ramach tego samouczka. 

1. W menu znajdującym się po lewej stronie w witrynie Azure Portal kliknij pozycję **Grupy zasobów**, a następnie kliknij nazwę utworzonego zasobu. 
2. Na stronie grupy zasobów kliknij pozycję **Usuń**, wpisz w polu tekstowym nazwę zasobu do usunięcia, a następnie kliknij pozycję **Usuń**.

## <a name="next-steps"></a>Następne kroki

W tym samouczku wykonaniu następujących czynności:

> [!div class="checklist"]
> * Tworzenie konta usługi Azure Cosmos DB 
> * Aktualizowanie parametrów połączenia
> * Tworzenie aplikacji bazy danych MongoDB na maszynie wirtualnej

Można przejść do następnego samouczek i zaimportować dane z bazy danych MongoDB do bazy danych Azure rozwiązania Cosmos.  

> [!div class="nextstepaction"]
> [Importowanie danych z bazy danych MongoDB do usługi Azure Cosmos DB](mongodb-migrate.md)

