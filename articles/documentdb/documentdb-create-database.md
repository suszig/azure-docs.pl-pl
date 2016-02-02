<properties 
    pageTitle="DocumentDB でデータベースを作成する方法 |Microsoft Azure" 
    description="JSON の NoSQL ドキュメント データベースである Azure DocumentDB 用のオンライン サービス ポータルを使用して、データベースを作成する方法について説明します。無料評価版を今すぐ入手してください。" 
    keywords="how to create a database" 
    services="documentdb" 
    authors="mimig1" 
    manager="jhubbard" 
    editor="monicar" 
    documentationCenter=""/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="11/18/2015" 
    ms.author="mimig"/>


# DocumentDB 用のデータベースの作成方法

Microsoft Azure DocumentDB を使用するが必要な [DocumentDB アカウント](documentdb-create-account.md), 、データベース、コレクション、およびドキュメントです。 このトピックでは、Microsoft Azure ポータルで DocumentDB データベースを作成する方法について説明します。

![[参照] ボタン、](./media/documentdb-create-database/docdb-database-creation-1-3.png)

1.  [Azure ポータル](https://portal.azure.com/), 、ジャンプバー、[ **DocumentDB アカウント**します。

2.  **[DocumentDB アカウント]** ブレードで、DocumentDB NoSQL データベースを追加するデータベースのあるアカウントを選択します。 表示されているすべてのアカウントを取得していない場合は、する必要があります [DocumentDB アカウントの作成](documentdb-create-account.md)します。

3. **[DocumentDB アカウント]** ブレードで、**[データベースの追加]** をクリックします。

4. **[データベースの追加]** ブレードで、新しいデータベースの ID を入力します。 名前が検証されると、緑色のチェック マークが **[ID]** ボックスに表示されます。

5. 画面下部の **[OK]** をクリックすると、新しいデータベースが作成されます。

7. 新しいデータベースは、**[DocumentDB アカウント]** ブレードの **[データベース]** レンズに表示されます。

    ![[DocumentDB アカウント] ブレードの新しいデータベースのスクリーン ショット](./media/documentdb-create-database/docdb-database-creation-7.png)

## DocumentDB データベースを作成するその他の方法

データベースは、ポータルを使用して作成する必要はありません、それらを作成することもを使用して、 [DocumentDB Sdk](https://msdn.microsoft.com/library/azure/dn781482.aspx)します。 C# の場合、DocumentDB .NET SDK を使用してデータベースを作成する方法を示したコード サンプルを参照してください、 [Program.cs](https://github.com/Azure/azure-documentdb-net/blob/master/samples/code-samples/DatabaseManagement/Program.cs) DatabaseManagement プロジェクトで使用可能なファイル、 [azure documentdb ネット](https://github.com/Azure/azure-documentdb-net) リポジトリ [GitHub.com](https://github.com)します。

## 次のステップ

次の手順で、DocumentDB のデータベースを作成する方法を理解する [コレクションを作成](documentdb-create-collection.md)します。

コレクションを作成したら [JSON ドキュメントを追加](../documentdb-view-json-document-explorer.md) ポータルで、Document Explorer を使用して、 [ドキュメントをインポート](documentdb-import-data.md) DocumentDB データ移行ツールを使用してコレクションにかのいずれかを使用して、 [DocumentDB Sdk](https://msdn.microsoft.com/library/azure/dn781482.aspx) CRUD 操作を実行します。 DocumentDB には、.NET、Java、Python、Node.js、および JavaScript API の SDK があります。 .NET コード サンプルが作成、削除、更新、およびドキュメントを削除する方法を示す、次を参照してください。 [Program.cs](https://github.com/Azure/azure-documentdb-net/blob/master/samples/code-samples/DocumentManagement/Program.cs) GitHub.com 上の azure documentdb ネット リポジトリにある DocumentManagement プロジェクトにします。

使用することができます、コレクションにドキュメントを用意した後 [DocumentDB SQL](documentdb-sql-query.md) に [クエリを実行する](documentdb-sql-query.md#executing-queries) を使用してドキュメントに対して、 [クエリ エクスプ ローラー](documentdb-query-collections-query-explorer.md) ポータルで、 [REST API](https://msdn.microsoft.com/library/azure/dn781481.aspx), 、またはのいずれか、 [Sdk](https://msdn.microsoft.com/library/azure/dn781482.aspx)します。





