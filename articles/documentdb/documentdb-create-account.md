<properties 
    pageTitle="NoSQL データベース アカウントの作成 - 無料試用版 | Microsoft Azure" 
    description="JSON の管理された NoSQL ドキュメント データベースである Azure DocumentDB 用のオンライン データベース クリエーターを使用して、データベース アカウントを作成する方法について説明します。 無料試用版を今すぐ入手してください。"
    keywords="Free trial, online database creator, create a database, create database, documentdb, azure, Microsoft azure"
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
    ms.date="12/03/2015" 
    ms.author="mimig"/>

# Azure ポータルを使用して DocumentDB データベース アカウントを作成する

> [AZURE.SELECTOR]
- [Azure ポータル](documentdb-create-account.md)
- [Azure CLI および ARM](documentdb-automation-resource-manager-cli.md)

Microsoft Azure DocumentDB を使用するには、Azure ポータル、Azure リソース マネージャー、または Azure コマンドライン インターフェイス (CLI) を使用して、DocumentDB データベース アカウントを作成する必要があります。 この記事では、Azure ポータルを使用してデータベース アカウントを作成する方法について説明します。 Azure リソース マネージャーまたは Azure CLI を使用して、アカウントを作成するを参照してください。 [自動化の DocumentDB データベース アカウントの作成](documentdb-automation-resource-manager-cli.md)します。 

DocumentDB を初めて使用する場合は、 ウォッチ [この](http://azure.microsoft.com/documentation/videos/create-documentdb-on-azure/) 4 分のビデオをオンライン ポータルで最も一般的なタスクを完了する方法については「Scott Hanselman 氏によるします。

[AZURE.INCLUDE [documentdb-create-dbaccount](../../includes/documentdb-create-dbaccount.md)]

## 次のステップ

DocumentDB アカウントを作成できたら、次の手順として DocumentDB データベースを作成します。 データベースを作成するには、次のいずれかを使用します。

- C# .NET のサンプル、 [DatabaseManagement](https://github.com/Azure/azure-documentdb-net/tree/master/samples/code-samples/DatabaseManagement) のプロジェクト、 [azure documentdb ネット](https://github.com/Azure/azure-documentdb-net/tree/master/samples/code-samples) GitHub のリポジトリです。
- Azure ポータルに明記されている [Azure ポータルを使用して DocumentDB データベースを作成する](documentdb-create-database.md)です。
- 包括的なチュートリアル: [.NET](documentdb-get-started.md), 、[.NET MVC](documentdb-dotnet-application.md), 、[Java](documentdb-java-application.md), 、[Node.js](documentdb-nodejs-application.md), 、または [Python](documentdb-python-application.md)します。
-  [DocumentDB Sdk](documentdb-sdk-dotnet.md)します。 DocumentDB には、.NET、Java、Python、Node.js、および JavaScript API の SDK があります。 


データベースを作成した後にする必要があります。 [1 つまたは複数のコレクションに追加](documentdb-create-collection.md) し、データベースに [ドキュメントの追加](documentdb-view-json-document-explorer.md) 、コレクションにします。 

使用することができます、コレクションにドキュメントを用意した後 [DocumentDB SQL](documentdb-sql-query.md) に [クエリを実行する](documentdb-sql-query.md#executing-queries) を使用してドキュメントに対して、 [クエリ エクスプ ローラー](documentdb-query-collections-query-explorer.md) ポータルで、 [REST API](https://msdn.microsoft.com/library/azure/dn781481.aspx), 、またはのいずれか、 [Sdk](documentdb-sdk-dotnet.md)します。

DocumentDB の詳細については、以下の資料を参照してください。

-   [DocumentDB のラーニング パス](https://azure.microsoft.com/documentation/learning-paths/documentdb/)
-   [DocumentDB のリソース モデルと概念](documentdb-resources.md)

 

