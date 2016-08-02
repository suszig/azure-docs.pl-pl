<properties
    pageTitle="Jak utworzyć konto usługi DocumentDB | Microsoft Azure"
    description="Utwórz bazę danych NoSQL przy użyciu usługi Azure DocumentDB. Wykonaj te instrukcje, aby utworzyć konto usługi DocumentDB i rozpocząć tworzenie błyskawicznie szybkiej bazy danych NoSQL o światowej skali." 
    keywords="build a database"
    services="documentdb"
    documentationCenter=""
    authors="mimig1"
    manager="jhubbard"
    editor="monicar"/>

<tags
    ms.service="documentdb"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="05/16/2016"
    ms.author="mimig"/>

# Jak utworzyć konto usługi DocumentDB przy użyciu portalu Azure

> [AZURE.SELECTOR]
- [Portal Azure](documentdb-create-account.md)
- [Interfejs wiersza polecenia platformy Azure oraz usługa ARM](documentdb-automation-resource-manager-cli.md)

Aby utworzyć bazę danych przy użyciu usługi Microsoft Azure DocumentDB, będą potrzebne:

- Konto platformy Azure. Jeśli go jeszcze nie masz, możesz utworzyć [bezpłatne konto platformy Azure](https://azure.microsoft.com/free). 
- Konto usługi DocumentDB.  

Konto usługi DocumentDB można utworzyć za pomocą portalu Azure, szablonów usługi Azure Resource Manager lub interfejsu wiersza polecenia platformy Azure. W tym artykule przedstawiono, jak utworzyć konto bazy danych przy użyciu portalu Azure. Aby utworzyć konto przy użyciu usługi Azure Resource Manager lub interfejsu wiersza polecenia platformy Azure, zobacz [Automate DocumentDB database account creation](documentdb-automation-resource-manager-cli.md) (Automatyzacja tworzenia konta bazy danych usługi DocumentDB).

Dopiero zaczynasz korzystać z usługi DocumentDB? Obejrzyj [to](https://azure.microsoft.com/documentation/videos/create-documentdb-on-azure/) czterominutowe nagranie, w którym Scott Hanselman pokazuje, jak wykonać typowe zadania w portalu online.

[AZURE.INCLUDE [documentdb-create-dbaccount](../../includes/documentdb-create-dbaccount.md)]

## Następne kroki

Teraz, gdy masz konto usługi DocumentDB, następnym krokiem jest utworzenie bazy danych usługi DocumentDB. Bazę danych można utworzyć, korzystając z jednego z następujących sposobów:

- portalu Azure, zgodnie z opisem w artykule [Create a DocumentDB database using the Azure portal](documentdb-create-database.md) (Tworzenie bazy danych usługi DocumentDB za pomocą portalu Azure);
- przykładów kodu dla języka C# .NET w projekcie [DatabaseManagement](https://github.com/Azure/azure-documentdb-net/tree/master/samples/code-samples/DatabaseManagement) z repozytorium [azure-documentdb-net](https://github.com/Azure/azure-documentdb-net/tree/master/samples/code-samples) w witrynie GitHub;
- kompleksowych samouczków dla następujących technologii: [.NET](documentdb-get-started.md), [.NET MVC](documentdb-dotnet-application.md), [Java](documentdb-java-application.md), [Node.js](documentdb-nodejs-application.md) lub [Python](documentdb-python-application.md);
- [zestawów SDK DocumentDB](documentdb-sdk-dotnet.md). Dla usługi DocumentDB są dostępne zestawy SDK dla technologii .NET, Java, Python, Node.js oraz JavaScript API.


Po utworzeniu bazy danych należy [dodać co najmniej jedną kolekcję](documentdb-create-collection.md) do bazy danych, a następnie [dodać dokumenty](documentdb-view-json-document-explorer.md) do kolekcji.

Po umieszczeniu dokumentów w kolekcji można użyć [języka DocumentDB SQL](documentdb-sql-query.md) do [wykonywania zapytań](documentdb-sql-query.md#executing-queries) względem dokumentów za pomocą dostępnego w portalu [Eksploratora zapytań](documentdb-query-collections-query-explorer.md), [interfejsu REST API](https://msdn.microsoft.com/library/azure/dn781481.aspx) lub jednego z [zestawów SDK](documentdb-sdk-dotnet.md).

Aby dowiedzieć się więcej na temat usługi DocumentDB, zapoznaj się z tymi zasobami:

-   [Ścieżka szkoleniowa dotycząca usługi DocumentDB](https://azure.microsoft.com/documentation/learning-paths/documentdb/)
-   [DocumentDB resource model and concepts (Pojęcia i model zasobów bazy danych DocumentDB)](documentdb-resources.md)



<!--HONumber=Jun16_HO2-->


