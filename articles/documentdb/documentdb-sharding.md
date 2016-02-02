<properties 
    pageTitle=".NET SDK を使用して DocumentDB 内のデータをパーティション分割する方法 | Microsoft Azure" 
    description="Azure DocumentDB .NET SDK を使用して、データをパーティション分割 (シャード) したり、複数のコレクションに要求をルーティングしたりする方法について説明します。" 
    services="documentdb" 
    authors="arramac" 
    manager="jhubbard" 
    editor="cgronlun" 
    documentationCenter=""/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/05/2015" 
    ms.author="arramac"/>


# .NET SDK を使用して DocumentDB 内のデータをパーティション分割する方法

Azure DocumentDB は、ドキュメント データベース サービスを使用してコレクションをプロビジョニングすることによって、アカウントをシームレスに拡張することができます、 [Sdk](https://msdn.microsoft.com/library/azure/dn781482.aspx) と [REST Api](https://msdn.microsoft.com/library/azure/dn781481.aspx) (とも呼ばれます **シャーディング**)。 パーティション分割されたアプリケーションの開発を容易にし、パーティション分割タスクに必要なボイラー プレート コードの量を減らすために、複数のパーティションにスケール アウトされるアプリケーションを構築しやすくする機能を .NET SDK に追加しました。

この記事では、.NET SDK のクラスとインターフェイスについて確認し、それらを使用してパーティション分割されたアプリケーションを開発する方法を見ていきます。

## DocumentDB SDK を使用したパーティション分割

パーティション分割を詳しく見る前に、パーティション分割に関連する基本的な DocumentDB の概念を確認しましょう。 各 Azure DocumentDB データベース アカウントは、一連のデータベースで構成されます。それぞれのデータベースには複数のコレクションが含まれ、それぞれのコレクションにはストアド プロシージャ、トリガー、UDF、ドキュメント、および関連する添付ファイルが含まれています。 コレクションは DocumentDB 内でパーティションとして扱うことができ、次のような性質を備えています。

- コレクションは物理的なパーティションで、論理的なコンテナーではありません。 そのため、同じコレクション内の複数のドキュメントに対してクエリや処理を行う場合に、パフォーマンスが向上します。
- コレクションは、ストアド プロシージャやトリガーなど、ACID トランザクションの境界です。
- コレクションではスキーマを適用しないため、同じ種類だけでなく、異なる種類の複数の JSON ドキュメントに対しても使用できます。

以降のバージョンで [Azure DocumentDB .NET SDK の 1.1.0](http://www.nuget.org/packages/Microsoft.Azure.DocumentDB/), 、データベースに対して直接ドキュメントの操作を行うことができます。 内部的に、 [DocumentClient](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.aspx) 適切なコレクションに要求をルーティングするデータベースの指定した PartitionResolver を使用します。

各 PartitionResolver クラスはの具象実装、 [IPartitionResolver](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.ipartitionresolver.aspx) を 3 つのメソッドを持つインターフェイス [GetPartitionKey](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.ipartitionresolver.getpartitionkey.aspx), 、[ResolveForCreate](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.ipartitionresolver.resolveforcreate.aspx) と [ResolveForRead](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.ipartitionresolver.resolveforread.aspx)します。 LINQ クエリと ReadFeed 反復子では、ResolveForRead メソッドを内部的に使用して、要求のパーティション キーと一致するすべてのコレクションを反復処理します。 同様に、作成操作では、ResolveForCreate メソッドを使用して、適切なパーティションに作成をルーティングします。 置換、削除、および読み取りでは、使用するドキュメント内に当該コレクションへの参照が既に含まれているため、必要な変更はありません。

SDK を使用して 2 つの標準的なパーティション分割手法、ハッシュ、範囲参照をサポートする 2 つのクラスも含まれています、 [HashPartitionResolver](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.partitioning.hashpartitionresolver.aspx) と [RangePartitionResolver](https://msdn.microsoft.com/library/azure/mt126047.aspx)します。 これらのクラスを使用すれば、パーティション分割ロジックをアプリケーションに簡単に追加できます。

## パーティション分割ロジックの追加と PartitionResolver の登録

作成する方法を示したスニペットをここでは、 [HashPartitionResolver](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.partitioning.hashpartitionresolver.aspx) し、データベースの DocumentClient に登録します。

```cs
// Create some collections to partition data.
DocumentCollection collection1 = await client.CreateDocumentCollectionAsync(...);
DocumentCollection collection2 = await client.CreateDocumentCollectionAsync(...);

// Initialize a HashPartitionResolver using the "UserId" property and the two collection self-links.
HashPartitionResolver hashResolver = new HashPartitionResolver(
    u => ((UserProfile)u).UserId, 
    new string[] { collection1.SelfLink, collection2.SelfLink });

// Register the PartitionResolver with the database.
this.client.PartitionResolvers[database.SelfLink] = hashResolver;
```

## パーティションでのドキュメントの作成

PartitionResolver が登録されたら、以下に示すように、データベースに対して直接作成とクエリを実行できます。 この例では、SDK は PartitionResolver を使用して、ユーザー ID を抽出してハッシュを生成し、その値を使用して適切なコレクションに作成操作をルーティングしています。

```cs
Document johnDocument = await this.client.CreateDocumentAsync(
    database.SelfLink, new UserProfile("J1", "@John", Region.UnitedStatesEast));
Document ryanDocument = await this.client.CreateDocumentAsync(
    database.SelfLink, new UserProfile("U4", "@Ryan", Region.AsiaPacific, UserStatus.AppearAway));
```

## パーティションに対するクエリの作成

使用してクエリを実行できる、 [CreateDocumentQuery](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.linq.documentqueryable.createdocumentquery.aspx) データベースとパーティション キーを渡してメソッドです。 クエリは、データベース内の、パーティション キーにマップされたコレクションすべてに対して結果セットを 1 つ返します。

```cs
// Query for John's document by ID - uses PartitionResolver to restrict the query to the partitions 
// containing @John. Again the query uses the database self link, and relies on the hash resolver 
// to route the appropriate collection.
var query = this.client.CreateDocumentQuery<UserProfile>(
    database.SelfLink, null, partitionResolver.GetPartitionKey(johnProfile))
    .Where(u => u.UserName == "@John");
johnProfile = query.AsEnumerable().FirstOrDefault();
```

## データベース内のすべてのコレクションに対するクエリの作成

次に示すように、パーティション キーの引数を省略することで、データベース内のすべてのコレクションに対してクエリを実行し、結果を列挙できます。

```cs
// Query for all "Available" users. Here since there is no partition key, the query is serially executed 
// across each partition/collection and returns a single result-set. 
query = this.client.CreateDocumentQuery<UserProfile>(database.SelfLink)
    .Where(u => u.Status == UserStatus.Available);
foreach (UserProfile activeUser in query)
{
    Console.WriteLine(activeUser);
}
```

## ハッシュ パーティション リゾルバー

ハッシュ パーティション分割では、ハッシュ関数の値に基づいてパーティションが割り当てられるため、要求やデータを複数のパーティションに均等に分配できます。 このアプローチは一般的に、多種多様なクライアントによって生成または消費されるデータのパーティション分割に使用され、ユーザー プロファイル、カタログ項目、IoT (Internet of Things: モノのインターネット) テレメトリ データなどを格納するのに役立ちます。

**ハッシュ パーティション分割:**
![ハッシュ パーティション分割で要求が複数のパーティションに均等に分配されることを示す図](media/documentdb-sharding/partition-hash.png "Hash partitioning")

*N* 個のコレクションに対するシンプルなハッシュ パーティション スキームでは、ドキュメントに対し、*hash(d) mod N* を計算して配置するコレクションを決定します。 このシンプルな手法には、コレクションの追加や削除ではうまく機能しないという問題点があります。これらの操作では、ほぼすべてのデータを再配置する必要が生じるためです。 [コンシス テント ハッシュ](http://citeseerx.ist.psu.edu/viewdoc/summary?doi=10.1.1.23.3738) は、追加またはコレクションを削除中に必要なデータ移動の量を最小限に抑えるハッシュ スキームを実装することによってこの問題に対処するよく知られたアルゴリズムです。

[HashPartitionResolver](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.partitioning.hashpartitionresolver.aspx) クラスで指定されたハッシュ関数に対してコンシス テント ハッシュ リングを構築するためのロジックを実装して、 [IHashGenerator](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.partitioning.ihashgenerator.aspx) インターフェイスです。 HashPartitionResolver は既定で MD5 ハッシュ関数を使用しますが、独自のハッシュ実装に置き換えることができます。 HashPartitionResolver の内部では、16 個のハッシュが "仮想ノード" のように各コレクションのハッシュ リング上に作成されます。これにより、複数のコレクションに対してドキュメントが均等に分配されます。ハッシュの個数を変更して、クライアント側の計算量とデータの偏りのバランスをとることもできます。

**したコンシス テント ハッシュ HashPartitionResolver と:**
![HashPartitionResolver によるハッシュ リング作成を示す図](media/documentdb-sharding/HashPartitionResolver.JPG "Consistent hashing")

## 範囲パーティション リゾルバー

範囲パーティション分割では、パーティション キーが特定の範囲内にあるかどうかに基づいてパーティションが割り当てられます。この手法は、タイム スタンプ プロパティ (たとえば、2015 年 4 月 1 日から 2015 年 4 月 14 日までの eventTime) を用いたパーティション分割に使用されるのが一般的です。 [RangePartitionResolver](https://msdn.microsoft.com/library/azure/mt126047.aspx) クラスでは、範囲、マッピングを維持できます。<T\> とコレクションの自己リンクします。

[範囲] \<T\>] (https://msdn.microsoft.com/library/azure/mt126048.aspx) IComparable\ を実装する任意の型の範囲を管理する単純なクラスは、<T\> と IEquatable\<T\> などの文字列または数値です。読み取りと作成では、任意の範囲を渡すことができ、要求された範囲とパーティションの範囲の交差がリゾルバーによって特定されて、候補となるすべてのコレクションが識別されます。この機能は、時系列データに対する範囲クエリを実行する際に役立ちます。

**範囲パーティション分割:**

![範囲パーティション分割で要求が複数のパーティションに均等に分配されることを示す図](media/documentdb-sharding/partition-range.png "Range partitioning")

範囲パーティション分割の特殊なケースとして、範囲が単一の離散値のみの場合があります。このようなケースは "検索パーティション分割" とも呼ばれます。 この手法は、リージョンごとのパーティション分割 (たとえば、ノルウェー、デンマーク、スウェーデンを含むスカンジナビア地域についてのパーティション) か、マルチ テナント アプリケーション内のテナントのパーティション分割に使用されるのが一般的です。

## サンプル

見て、  [DocumentDB パーティション分割のサンプル Github プロジェクト](https://github.com/Azure/azure-documentdb-net/tree/master/samples/code-samples/Partitioning) これら Partitionresolver を使用して、特定のユース ケースに合わせて独自のリゾルバーを実装するために拡張する方法のコード スニペットを含む次のようにします。

* GetPartitionKey に任意のラムダ式を指定し、これを使用して複合パーティション キーを実装したり、複数の種類のオブジェクトをそれぞれ異なる方法でパーティション分割したりする方法。
* 単純なを作成する方法 [LookupPartitionResolver](https://github.com/Azure/azure-documentdb-net/tree/master/samples/code-samples/Partitioning/Partitioners/LookupPartitionResolver.cs) 手動のルックアップ テーブルを使用して、パーティション分割を実行します。 このパターンは一般的に、リージョン、テナント ID、アプリケーション名などの離散値に基づくパーティション分割に使用されます。
* 作成する方法、 [ManagedPartitionResolver](https://github.com/Azure/azure-documentdb-net/tree/master/samples/code-samples/Partitioning/Partitioners/ManagedHashPartitionResolver.cs) 名前付けスキーム、IndexingPolicy、および新しいコレクションに対して登録する必要があるストアド プロシージャを定義するテンプレートに基づいて自動的にコレクションを作成します。
* スキームのないを作成する方法 [SpilloverPartitionResolver](https://github.com/Azure/azure-documentdb-net/tree/master/samples/code-samples/Partitioning/Partitioners/SpilloverPartitionResolver.cs) 古いコレクションがいっぱいになると単に新しいコレクションを作成します。
* PartitionResolver の状態を JSON としてシリアル化および逆シリアル化する方法。これにより、状態をプロセス間で共有したり、シャットダウンをまたいで維持したりできます。 状態は、構成ファイルだけでなく、DocumentDB コレクションにも保持できます。
* A [DocumentClientHashPartitioningManager](https://github.com/Azure/azure-documentdb-net/tree/master/samples/code-samples/Partitioning/Util/DocumentClientHashPartitioningManager.cs) クラスに基づいて動的に追加して、パーティション分割されたデータベースにパーティションを削除するコンシス テント ハッシュします。 内部的に使用する [TransitionHashPartitionResolver](https://github.com/Azure/azure-documentdb-net/tree/master/samples/code-samples/Partitioning/Partitioners/TransitionHashPartitionResolver.cs) ルートの読み取りと書き込みを使用して、古いパーティション スキーム (ReadCurrent)、新しいからの読み取りに 4 つのモードのいずれかの移行中に (ReadNext)、両方 (ReadBoth) からの結果の結合] または [(なし) の移行時に使用できなきます。

サンプルはオープン ソースです。他の DocumentDB 開発者にも役立つような投稿でプル リクエストを送信することをお勧めします。 参照してください、 [投稿に関するガイドライン](https://github.com/Azure/azure-documentdb-net/blob/master/Contributing.md) 投稿方法のガイダンスです。
>[AZURE.NOTE] コレクションの作成は DocumentDB によって速度が制限されているため、ここに示したサンプル メソッドの一部では、処理が完了するまでに数分かかる場合があります。

## FAQ

**DocumentDB がサーバー側のパーティション分割ではなくクライアント側のパーティション分割をサポートしているのはなぜですか。**

DocumentDB では複数の理由からクライアント側のパーティション分割をサポートしています。

- 開発者からコレクションの概念を分離しようとすると、一貫性のあるインデックス作成/クエリ実行、高可用性、ACID トランザクションの保証のうち、いずれかの面で妥協せざるを得なくなります。
- ドキュメント データベースでは、多くの場合、パーティション分割戦略を定義するうえで柔軟性が必要ですが、サーバー側アプローチではこれに対応できない場合があります。

**他のプラットフォーム (Node.js、Java、Python) ではパーティション分割がサポートされていないのはなぜですか。**

Microsoft では、.NET SDK のお客様からいただいたフィードバックを基に、他のプラットフォームにもパーティション分割のサポートを段階的にロールアウトする予定です。

**パーティション スキームへのコレクションの追加や削除はどのようにすればよいですか。**

パーティション再分割を実装する方法の例については、サンプル プロジェクト内の DocumentClientHashPartitioningManager の実装を参照してください。

**パーティション分割構成を保持したり、他のクライアントと共有したりするにはどのようにすればよいですか。**

パーティションの状態を JSON としてシリアル化し、構成ファイルのほか DocumentDB コレクションにも格納できます。 この例については、サンプル プロジェクト内の RunSerializeDeserializeSample メソッドを参照してください。

**さまざまなパーティション分割手法を連結するにはどのようにすればよいですか。**

複数の PartitionResolver を連結するには、1 つ以上の既存のリゾルバーを内部で使用する独自の IPartitionResolver を実装します。 この例については、サンプル プロジェクト内の TransitionHashPartitionResolver を参照してください。

## 参照

* [Github のパーティション分割のコード サンプル](https://github.com/Azure/azure-documentdb-net/tree/master/samples/code-samples/Partitioning)
* [DocumentDB の概念を使用してデータをパーティション分割](documentdb-partition-data.md)
* [DocumentDB のコレクションとパフォーマンス レベル](documentdb-performance-levels.md)
* [MSDN の DocumentDB .NET SDK に関するドキュメント](https://msdn.microsoft.com/library/azure/dn948556.aspx)
* [DocumentDB .NET のサンプル](https://github.com/Azure/azure-documentdb-net)
* [DocumentDB の制限](documentdb-limits.md)
* [パフォーマンスのヒントの DocumentDB ブログ](http://azure.microsoft.com/blog/2015/01/20/performance-tips-for-azure-documentdb-part-1-2/)





