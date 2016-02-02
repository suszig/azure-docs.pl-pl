<properties 
    pageTitle="DocumentDB データベース質問 - よく寄せられる質問 | Microsoft Azure" 
    description="JSON 用の NoSQL ドキュメント データベース サービスである Azure DocumentDB に関してよく寄せられる質問に対する回答を示します。容量、パフォーマンス レベル、およびスケーリングに関するデータベース質問に回答します。" 
    keywords="Database questions, frequently asked questions, documentdb, azure, Microsoft azure"
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
    ms.date="12/03/2015" 
    ms.author="mimig"/>



# DocumentDB に関してよく寄せられる質問

## Microsoft Azure DocumentDB の基礎に関するデータベース質問

### Microsoft Azure DocumentDB とは

Microsoft Azure DocumentDB は、スケーラブルな NoSQL ドキュメントのサービスとしてのデータベースです。Microsoft Azure の機能と能力に支えられた管理プラットフォームによって、スキーマフリーのデータに高度なクエリを実行することができ、構成可能な信頼性の高いパフォーマンスの提供、および迅速な開発が可能となります。 DocumentDB は、予測可能なスループット、遅延時間の短縮、およびスキーマフリー データ モデルが重要な要件となる、Web アプリケーションやモバイル アプリケーションに適したソリューションです。 DocumentDB では、ネイティブ JSON データ モデルでスキーマの柔軟性と豊富なインデックス作成機能が提供され、統合 JavaScript でマルチドキュメント トランザクションがサポートされます。

データベースの質問、回答、およびデプロイして、このサービスを使用する手順についてを詳細にするを参照してください、 [DocumentDB ドキュメント ページ](http://azure.microsoft.com/documentation/services/documentdb/)します。

### DocumentDB はどのような種類のデータベースですか?

DocumentDB は、JSON 形式でデータを格納する NoSQL ドキュメント指向のデータベースです。 DocumentDB は、高度な DocumentDB で照会できる入れ子になった、自立的データの構造をサポートしている [SQL クエリ文法](documentdb-sql-query.md)します。 DocumentDB では、高パフォーマンス サーバー側 JavaScript のトランザクション処理を通じて [ストアド プロシージャ、トリガー、およびユーザー定義関数](documentdb-programming.md)します。 データベースは、開発者によって調整可能な一貫性レベルもサポートしています。 関連付け [パフォーマンス レベル](documentdb-performance-levels.md)します。

### DocumentDB データベースはリレーショナル データベース (RDBMS) のようなテーブルを保持しますか?

いいえ、DocumentDB は JSON ドキュメントのコレクション内のデータを格納します。 DocumentDB のリソースについては、次を参照してください。 [DocumentDB リソース モデルと概念](documentdb-resources.md)します。

### DocumentDB データベースはスキーマフリー データをサポートしますか?

はい、DocumentDB ではスキーマ定義またはヒントなしで、アプリケーションが任意の JSON ドキュメントを格納できます。 DocumentDB SQL クエリ インターフェイスを使用したクエリに、データをすぐに利用できます。

### DocumentDB は ACID トランザクションをサポートしますか?

はい、DocumentDB では、JavaScript ストアド プロシージャおよびトリガーとして表現されるクロスドキュメント トランザクションをサポートします。 トランザクションは、単一のコレクションを対象とし、他の同時実行されるコードおよびユーザー要求から完全に分離された ACID セマンティクスにより実行されます。 JavaScript アプリケーション コードのサーバー側実行により例外がスローされた場合は、トランザクション全体がロールバックされます。

### DocumentDB の一般的な使用事例にはどのようなものがありますか?

DocumentDB は、拡張性、パフォーマンス、およびスキーマフリー データでのクエリを実行できることが重要である新しい Web アプリケーションやモバイル アプリケーションに適しています。 DocumentDB は、迅速な開発に役立ち、アプリケーション データ モデルの継続的な反復をサポートします。 ユーザーが生成したコンテンツとデータ管理アプリケーションは、 [DocumentDB の一般的な使用事例](documentdb-use-cases.md)します。

### DocumentDB HIPAA は準拠していますか?

DocumentDB は現在 HIPAA に準拠していませんが、Azure サービスは HIPAA 準拠になる予定です。 Microsoft および HIPAA の詳細については、次を参照してください。 [HIPAA、ハイテク Act](https://www.microsoft.com/en-us/TrustCenter/Compliance/HIPAA)します。

### DocumentDB のスケール制限とは何ですか?

ストレージおよびスループットの観点では、DocumentDB アカウントはコレクションを追加してスケールすることができます。 参照してください [DocumentDB の制限](documentdb-limits.md) コレクションの数に対するサービスのクォータのです。 追加のコレクションを必要とする場合にしてください。 [サポートにお問い合わせ](documentdb-increase-limits.md) 、アカウント クォータの増加にします。

### Microsoft Azure DocumentDB の料金はいくらですか?

参照してください、 [DocumentDB の料金詳細](http://go.microsoft.com/fwlink/p/?LinkID=402317) 詳細ページです。 DocumentDB の利用料金は、コレクションがオンラインであった時間数、使用中のコレクションの数によって決まりますと [パフォーマンス レベル](documentdb-performance-levels.md) コレクションごとのです。

### 無料試用版は使用できますか?

サインアップすることを Azure に慣れていない場合、 [Azure 無料評価版](https://azure.microsoft.com/pricing/free-trial/), 、30 日間と 200 ドルにすべての Azure サービスを提供します。 または、Visual Studio サブスクリプションがある場合の資格が [12,500 円 1 か月あたりの無料の Azure クレジットの](http://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) すべての Azure サービスを使用します。

### DocumentDB に関するその他のヘルプはどのようにして得られますか?

ヘルプを必要がある場合くださいに通知私たちに [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-documentdb), 、 [Azure DocumentDB MSDN デベロッパー フォーラム](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureDocumentDB), 、またはスケジュール設定、 [DocumentDB エンジニア リング チームと 1 対 1 のチャット](http://www.askdocdb.com/)します。 DocumentDB の最新のニュースと機能の最新の状態を維持するでフォロー [Twitter](https://twitter.com/DocumentDB)します。

## Microsoft Azure DocumentDB のセットアップ

### どのようにして Microsoft Azure DocumentDB にサインアップしますか?

Microsoft Azure DocumentDB はで使用できる、 [Azure ポータルの ][azure-portal]します。 最初に、Microsoft Azure サブスクリプションにサインアップする必要があります。 Microsoft Azure サブスクリプションにサインアップした後、DocumentDB アカウントを Azure サブスクリプションに追加できます。 DocumentDB アカウントを追加する方法の詳細については、次を参照してください。 [DocumentDB データベース アカウントの作成](documentdb-create-account.md)します。

### マスター キーとは何ですか?

マスター キーは、アカウントのすべてのリソースにアクセスするためのセキュリティ トークンです。 キーを保持する個人には、データベース アカウント内のすべてのリソースへの読み取り/書き込みアクセスが許可されます。 マスター キーを配布するときには十分な注意が必要となります。 プライマリ マスター_キーとセカンダリ マスター_キーで使用できますが、 ** キー **のブレード、 [Azure ポータルの ][azure-portal]します。 キーの詳細については、次を参照してください。 [の表示、コピーおよび再生成するアクセス キー](documentdb-manage-account.md#keys)します。

### どのようにしてデータベースを作成しますか?

使用してデータベースを作成する、 [Azure ポータル]() 」の説明に従って [DocumentDB データベースを作成する](documentdb-create-database.md), 、1 つの [DocumentDB Sdk](https://msdn.microsoft.com/library/azure/dn781482.aspx), 、または、 [REST Api](https://msdn.microsoft.com/library/azure/dn781481.aspx)します。

### コレクションとは何ですか?

コレクションには、JSON ドキュメントのほか、関連する JavaScript アプリケーション ロジックが格納されます。 クエリおよびトランザクションの範囲としてコレクションが指定されます。 多種多様な JSON ドキュメントのセットを 1 つのコレクション内に格納し、自動的にインデックスを作成することができます。

コレクションも DocumentDB の課金エンティティです。 毎月の DocumentDB の利用料金は、コレクションがオンラインであった時間数、使用中のコレクションの数によって決まりますと [パフォーマンス レベル](documentdb-performance-levels.md) コレクションごとのです。 詳細については、次を参照してください。 [DocumentDB の料金](https://azure.microsoft.com/pricing/details/documentdb/)します。

### データベースとコレクションに制限はありますか?

各コレクションは、データベース ストレージとプロビジョニング済みスループットがサポートされている 1 つで、割り当て [パフォーマンス レベル](documentdb-performance-levels.md)します。 サービスで管理される、各リソースに対するクォータも用意されています。 すべての制限の一覧は、次を参照してください。 [DocumentDB の制限](documentdb-limits.md)します。 アカウント制限の変更を要求するを参照してください。 [DocumentDB アカウント制限の引き上げ要求](documentdb-increase-limits.md)します。

### どのようにしてユーザーおよびアクセス許可を設定しますか?

ユーザーとのいずれかを使用してアクセス許可を作成することができます、 [DocumentDB Sdk](https://msdn.microsoft.com/library/azure/dn781482.aspx) により、または、 [REST Api](https://msdn.microsoft.com/library/azure/dn781481.aspx)します。

## Microsoft Azure DocumentDB に対する開発についてのデータベース質問

### どのようにして DocumentDB に対する開発を開始しますか?

[Sdk](https://msdn.microsoft.com/library/azure/dn781482.aspx) .NET、Python、Node.js、JavaScript、および Java を使用します。 開発者が利用しても、 [RESTful HTTP Api](https://msdn.microsoft.com/library/azure/dn781481.aspx) さまざまなプラットフォームおよび言語から DocumentDB リソースとやり取りします。

Samples for the DocumentDB [.NET](https://github.com/Azure/azure-documentdb-net/tree/master/samples/code-samples), [Java](https://github.com/Azure/azure-documentdb-java), [Node.js](https://github.com/Azure/azure-documentdb-node/tree/master/samples), and [Python](https://github.com/Azure/azure-documentdb-python) SDKs are available on GitHub.

### DocumentDB は SQL をサポートしますか?

DocumentDB の SQL クエリ言語は、JavaScript ベースのユーザー定義関数 (UDF) により高度な階層型の関係演算子と機能拡張を提供します。 JSON 文法では、JSON ドキュメントをツリー ノードとしてラベルが付けられたツリーとしてモデル化することができます。これは、DocumentDB の自動的なインデックス作成手法と DocumentDB の SQL クエリ方言の両方で利用されます。 SQL 文法の使用方法の詳細についてを参照してください、 [DocumentDB のクエリ ][query] 記事です。

### DocumentDB でサポートされるデータ型は何ですか?

DocumentDB でサポートされるプリミティブ データ型は JSON と同じです。 JSON には、文字列、数値 (IEEE754 倍精度)、ブール値 (true、false、Null) で構成されるシンプルな型システムがあります。 { } 演算子で入れ子になったオブジェクトを作成し、[ ] 演算子で配列を作成することにより、さらに複雑なデータ型 (DateTime、Guid、Int64、Geometry など) を JSON と DocumentDB の両方で表現できます。

### DocumentDB はどのようにして同時実行を提供しますか?

DocumentDB では、HTTP エンティティ タグ (または ETag) でオプティミスティック同時実行制御 (OCC) をサポートします。 すべての DocumentDB リソースには ETag があり、DocumentDB クライアントでは最新の読み取りバージョンが書き込み要求内に含まれます。 ETag が最新である場合は、変更がコミットされます。 値が外部で変更されている場合、サーバーは "HTTP 412 Precondition failure" 応答コードにより書き込みを拒否します。 クライアントは最新バージョンのリソースを読み取り、要求を再試行する必要があります。

### どのようにして DocumentDB のトランザクションを実行しますか?

DocumentDB では、JavaScript ストアド プロシージャおよびトリガーを介して、統合された言語のトランザクションがサポートされます。 スクリプト内のすべてのデータベース操作は、コレクションを対象としたスナップショット分離により実行されます。 ドキュメント バージョン (ETag) のスナップショットは、トランザクションの開始時に取得され、スクリプトが成功された場合のみコミットされます。 JavaScript がエラーをスローした場合、トランザクションはロールバックされます。 参照してください [DocumentDB サーバー側プログラミング](documentdb-programming.md) 詳細です。

### どのようにして DocumentDB にドキュメントを一括挿入しますか?

DocumentDB にドキュメントを一括挿入するには 3 つの方法があります。

- 」の説明に従って、データ移行ツール [を DocumentDB にデータをインポート](documentdb-import-data.md)します。
- 」の説明に従って、Azure ポータルでエクスプ ローラーを文書化 [ドキュメント エクスプ ローラーでドキュメントの一括追加](documentdb-view-json-document-explorer.md#BulkAdd)します。
- 」の説明に従って、ストアド プロシージャ、 [DocumentDB サーバー側プログラミング](documentdb-programming.md)します。

### DocumentDB はリソース リンク キャッシュをサポートしますか?

はい、DocumentDB は RESTful サービスであるため、リソース リンクは不変であり、キャッシュできます。 DocumentDB クライアントは、ドキュメントやコレクションなどリソースの読み取りに対して "If-None-Match" ヘッダーを指定でき、サーバー バージョンが変更された場合のみローカル コピーを更新できます。





[azure-portal]: https://portal.azure.com 
[query]: documentdb-sql-query.md 

