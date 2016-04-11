<properties 
    pageTitle="DocumentDB データベース コレクションの作成 |Microsoft Azure" 
    description="JSON の管理された NoSQL ドキュメント データベースである Azure DocumentDB 用のオンライン サービス ポータルを使用して、コレクションを作成する方法について説明します。 無料試用版を今すぐ入手してください。" 
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
    ms.date="09/28/2015" 
    ms.author="mimig"/>

# DocumentDB コレクションの作成

Microsoft Azure DocumentDB を使用するが必要、 [DocumentDB アカウント](documentdb-create-account.md), 、 [データベース](documentdb-create-database.md), 、コレクション、およびドキュメントです。 このトピックでは、Azure ポータルで DocumentDB コレクションを作成する方法について説明します。 

![ジャンプバーの [DocumentDB アカウント] 、[DocumentDB アカウント] ブレードのアカウント、[DocumentDB アカウント] ブレードの [データベース] レンズのデータベースを強調表示しているスクリーン ショット](./media/documentdb-create-collection/docdb-database-creation-1-3.png)

1.   [Azure ポータル](https://portal.azure.com/), 、ジャンプバー、[ **DocumentDB アカウント**します。 

2.   **DocumentDB アカウント** ブレードで、コレクションを追加するためのアカウントを選択します。 表示されているすべてのアカウントを取得していない場合は、する必要があります [DocumentDB アカウントの作成](documentdb-create-account.md)します。

3.  **DocumentDB アカウント** ブレードで、選択したアカウントの下にスクロールして、 **データベース** レンズ、およびコレクションを追加するデータベースを選択します。
    
4.  **データベース** ブレードで、をクリックして **コレクションに追加**します。

    ![[データベース] ブレードの [コレクションの追加]、[コレクションの追加] ブレードの設定、および [OK] ボタンを強調表示しているスクリーン ショット](./media/documentdb-create-collection/docdb-collection-creation-4-7.png)

5.  **コレクションの追加** ブレードで、新しいコレクションの ID を入力します。 名前が検証されると、緑色のチェック マークが [ID] ボックスに表示されます。

6. 新しいコレクションの価格レベルを選択します。 作成するそれぞれのコレクションが、課金対象のエンティティになります。 使用可能なパフォーマンス レベルの詳細については、次を参照してください。 [DocumentDB のパフォーマンス レベル](documentdb-performance-levels.md)します。

7. 次のいずれかを選択 **インデックス作成ポリシー**します。 

    - **既定の**です。 このポリシーは、文字列に対しては等値クエリを実行し、数値に対しては ORDER BY、範囲、および等値クエリを使用する場合に最適です。  このポリシーよりもオーバーヘッドが低いインデックスのストレージには **範囲**します。
    - **ハッシュ**します。 このポリシーは、数値と文字列の両方に対して等値クエリを実行する場合に最適です。  インデックスのストレージ オーバーヘッドが最も低いポリシーです。
    - **範囲**します。 このポリシーは、数値と文字列の両方に対してORDER BY、範囲、等値クエリを使用する場合に最適です。  このポリシーよりもオーバーヘッドが高いインデックスのストレージには **既定** または **ハッシュ**します。

    インデックス作成ポリシーの詳細については、次を参照してください。 [DocumentDB インデックス作成ポリシー](documentdb-indexing-policies.md)します。

8. クリックして **OK** 新しいコレクションを作成するには、画面の下部にあります。 


9. 新しいコレクションが表示される、 **コレクション** レンズ、 **データベース** ブレードです。
 
    ![[データベース] ブレードの新しいコレクションのスクリーン ショット](./media/documentdb-create-collection/docdb-collection-creation-8.png)

## DocumentDB コレクションを作成するその他の方法

コレクションには、ポータルを使用して作成する必要はありません、それらを作成することもを使用して、 [DocumentDB Sdk](https://msdn.microsoft.com/library/azure/dn781482.aspx)します。 C# の場合、DocumentDB .NET SDK を使用してコレクションを作成する方法を示したコード サンプルを参照してください、 [Program.cs](https://github.com/Azure/azure-documentdb-net/blob/master/samples/code-samples/CollectionManagement/Program.cs) で使用できる、CollectionManagement プロジェクトのファイル、 [azure documentdb ネット](https://github.com/Azure/azure-documentdb-net) リポジトリ [GitHub.com](https://github.com)します。

## 次のステップ

コレクションを作成できたら、次の手順は、コレクションへのドキュメントの追加またはインポートです。 コレクションへのドキュメントの追加方法には、いくつかの選択肢があります。

- 実行できます [ドキュメントを追加](../documentdb-view-json-document-explorer.md) 、ポータルの Document Explorer を使用しています。
- 実行できます [ドキュメントとデータをインポート](documentdb-import-data.md) DocumentDB データ移行ツールを使用するを使用する SQL Server、MongoDB、Azure テーブル ストレージ、およびその他の DocumentDB コレクションからのデータだけでなく、JSON、CSV ファイルをインポートします。 
- いずれかを使用してドキュメントを追加することも、 [DocumentDB Sdk](https://msdn.microsoft.com/library/azure/dn781482.aspx)します。 DocumentDB には、.NET、Java、Python、Node.js、および JavaScript API の SDK があります。  [Program.cs](https://github.com/Azure/azure-documentdb-net/blob/master/samples/code-samples/DocumentManagement/Program.cs) で使用できる DocumentManagement プロジェクトのファイル、 [azure documentdb ネット](https://github.com/Azure/azure-documentdb-net) リポジトリ [GitHub.com](https://github.com), 、DocumentDB .NET SDK を使用して、ドキュメントに対する CRUD 操作を示します。

使用することができます、コレクションにドキュメントを用意した後 [DocumentDB SQL](documentdb-sql-query.md) に [クエリを実行する](documentdb-sql-query.md#executing-queries) を使用してドキュメントに対して、 [クエリ エクスプ ローラー](documentdb-query-collections-query-explorer.md) ポータルで、 [REST API](https://msdn.microsoft.com/library/azure/dn781481.aspx), 、またはのいずれか、 [Sdk](https://msdn.microsoft.com/library/azure/dn781482.aspx)します。 

