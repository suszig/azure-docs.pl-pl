<properties 
    pageTitle="Azure DocumentDB のデータのモデル化 | Microsoft Azure" 
    description="Azure DocumentDB のような NoSQL ドキュメント データベースのデータをモデル化する方法について説明します。" 
    services="documentdb" 
    authors="ryancrawcour" 
    manager="jhubbard" 
    editor="mimig1" 
    documentationCenter=""/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="11/19/2015" 
    ms.author="ryancraw"/>

#DocumentDB のデータのモデル化#
DocumentDB のようなスキーマのないデータベースでは、データ モデルに対する変更を受け入れることはとても簡単ですが、データについて十分検討する必要があります。 

データをどのように格納するか。 アプリケーションがどのようにデータを取得してクエリを実行するか。 アプリケーションの負荷は読み取りと書き込みのどちらが高いか。

この記事を読むと、次の質問に回答できるようになります。

- ドキュメント データベース内のドキュメントについてどのように考えるか。
- データのモデル化とは何か、なぜ考慮する必要があるか。 
- データのモデル化は、ドキュメント データベースとリレーショナル データベースでどのように異なるか。
- 非リレーショナル データベース内のデータのリレーションシップをどのように表現するか。
- いつデータを埋め込み、いつデータにリンクするか。

##データの埋め込み##
DocumentDB などのドキュメント ストア内のデータをモデル化を開始するときに、エンティティと見なすしよう **内蔵ドキュメント** JSON で表されます。

さらに詳しく説明する前に、少し戻ってリレーショナル データベースにおけるモデル化のしくみを見てみましょう。これは、多くのユーザーが慣れ親しんでいるテーマです。 次の例は、ある個人がどのようにリレーショナル データベースに格納されるかを示しています。 

![リレーショナル データベース モデル](./media/documentdb-modeling-data/relational-data-model.png)

リレーショナル データベースを操作する際に、長年教え込まれてきたことは "正規化" です。

データの正規化には通常、個人などのエンティティを取得し、細かいデータ要素に分解する操作が含まれます。 上の例では、ある個人が複数の連絡先詳細レコードや複数の住所レコードを持っているとします。 さらにもう一歩進み、型などの共通フィールドを抽出することで、連絡先詳細を細分化します。 住所も同様で、各レコードにはのような型 *ホーム* または *ビジネス* 

前提データを正規化するときは、誘導 **冗長なデータを格納しないように** ごとに記録しではなくデータを参照してください。 この例で、連絡先詳細と住所すべてを含む個人のデータを読み取るには、JOIN を使用して実行時にデータを効率的に集約する必要があります。

    SELECT p.FirstName, p.LastName, a.City, cd.Detail
    FROM Person p
    JOIN ContactDetail cd ON cd.PersonId = p.Id
    JOIN ContactDetailType on cdt ON cdt.Id = cd.TypeId
    JOIN Address a ON a.PersonId = p.Id

1 人の個人をその連絡先詳細や住所で更新すると、多数の個別のテーブルへの書き込み操作が必要になります。 

では、同じデータをドキュメント データベース内の自己完結型エンティティとしてモデル化する方法を見ていきましょう。
        
    {
        "id": "1",
        "firstName": "Thomas",
        "lastName": "Andersen",
        "addresses": [
            {            
                "line1": "100 Some Street",
                "line2": "Unit 1",
                "city": "Seattle",
                "state": "WA",
                "zip": 98012
            }
        ],
        "contactDetails": [
            {"email: "thomas@andersen.com"},
            {"phone": "+1 555 555-5555", "extension": 5555}
        ] 
    }

ようになりましたが前に示した方法を使用して **非正規化** 個人レコードを **埋め込み** 1 つの JSON ドキュメントで連絡先詳細やアドレスなど、この個人に関するすべての情報です。
また、固定スキーマに限定されているわけではないので、まったく別の形で連絡先詳細を含めるような柔軟性もあります。 

データベースから個人レコード全体を取得するのは、1 つのコレクションと 1 つのドキュメントに対する 1 回の読み取り操作です。 連絡先詳細や住所で個人レコードを更新することも、1 つのドキュメントに対する 1 回の書き込み操作です。

データを非正規化することにより、アプリケーションが一般的な操作を完了するために発行する必要があるクエリや更新の数は少なくなります。 

###埋め込みをする場合

一般に、埋め込みデータ モデルを使用するのは次のような場合です。

- ある **を含む** エンティティ間のリレーションシップ。
- ある **1-一部** エンティティ間のリレーションシップ。
- 埋め込みデータを **変更頻度の低い**します。
- 埋め込まれているデータが増加しない **際限なく**です。
- 埋め込みデータがある **整数** ドキュメント内のデータにします。

> [AZURE.NOTE] 非正規化データ モデルが向上して通常 **読み取り** パフォーマンス。

###埋め込みをしない場合

ドキュメント データベースでよく使われる方法は、すべてを非正規化してすべてのデータを 1 つのドキュメントに埋め込むことですが、この方法でうまくいかない場合もあります。

この JSON スニペットを見てください。

    {
        "id": "1",
        "name": "What's new in the coolest Cloud",
        "summary": "A blog post by someone real famous",
        "comments": [
            {"id": 1, "author": "anon", "comment": "something useful, I'm sure"},
            {"id": 2, "author": "bob", "comment": "wisdom from the interwebs"},
            …
            {"id": 100001, "author": "jane", "comment": "and on we go ..."},
            …
            {"id": 1000000001, "author": "angry", "comment": "blah angry blah angry"},
            …
            {"id": ∞ + 1, "author": "bored", "comment": "oh man, will this ever end?"},
        ]
    }

これは、典型的なブログや CMS システムをモデル化しようとしたときに、コメントが埋め込まれた投稿エンティティがどのようになるかを示したものです。 この例の問題点は、コメントの配列がである **unbounded**, 、任意の 1 つの投稿のコメントの数に実質的な制限がないことを意味します。 これは、ドキュメントのサイズが著しく大きくなると、問題になります。

> [AZURE.TIP] DocumentDB 内のドキュメントには、最大サイズがあります。 詳細についてを参照してください [DocumentDB の制限](documentdb-limits.md)します。

ドキュメントのサイズが増大すると、ネットワーク経由でデータを送信する機能は、ドキュメントの読み取りや更新と同様に、大きな影響を受けます。

このような場合は、次のモデルを検討することをお勧めします。
        
    Post document:
    {
        "id": 1,
        "name": "What's new in the coolest Cloud",
        "summary": "A blog post by someone real famous",
        "recentComments": [
            {"id": 1, "author": "anon", "comment": "something useful, I'm sure"},
            {"id": 2, "author": "bob", "comment": "wisdom from the interwebs"},
            {"id": 3, "author": "jane", "comment": "....."}
        ]
    }

    Comment documents:
    {
        "postId": 1
        "comments": [
            {"id": 4, "author": "anon", "comment": "more goodness"},
            {"id": 5, "author": "bob", "comment": "tails from the field"},
            ...
            {"id": 99, "author": "angry", "comment": "blah angry blah angry"}
        ]
    },
    {
        "postId": 1
        "comments": [
            {"id": 100, "author": "anon", "comment": "yet more"},
            ...
            {"id": 199, "author": "bored", "comment": "will this ever end?"}
        ]
    }

このモデルでは、投稿自体に最新の 3 つのコメントが埋め込まれており、この場合、配列の上限は固定されています。 その他のコメントは 100 個ずつグループ化され、個別のドキュメントに格納されます。 グループ化するコメントのひとまとまりを 100 に指定したのは、この架空のアプリケーションでユーザーが一度に読み込むことができるコメントの数が 100 個になっているためです。  

また、データの埋め込みが推奨されない事例として、埋め込みデータがドキュメント間で頻繁に使用され、変更される場合があります。 

この JSON スニペットを見てください。

    {
        "id": "1",
        "firstName": "Thomas",
        "lastName": "Andersen",
        "holdings": [
            {
                "numberHeld": 100,
                "stock": { "symbol": "zaza", "open": 1, "high": 2, "low": 0.5 }
            },
            {
                "numberHeld": 50,
                "stock": { "symbol": "xcxc", "open": 89, "high": 93.24, "low": 88.87 }
            }
        ]
    }

これは、ある個人の株式ポートフォリオを表したものです。 各ポートフォリオ ドキュメントに株式情報が埋め込まれています。 株式取引アプリケーションのように関連データが頻繁に変更される環境では、埋め込みデータが頻繁に変更されるので、株が取引されるたびに各ポートフォリオ ドキュメントを絶えず更新しなければならないことになります。

在庫 *zaza* 何百もの 1 つの時間を交換することがあります日と数千のユーザーがあって *zaza* ポートフォリオにします。 上のようなデータ モデルでは、毎日、数千ものポートフォリオ ドキュメントを何回も更新する必要があり、システムの拡張が不十分になる可能性もあります。 

##<a id="Refer"></a>データの参照##

このように、データの埋め込みは多くの場合うまく機能しますが、データを非正規化すると、その効果よりも問題の方が多くなるシナリオがあることもまた事実です。 その場合は、どうすればよいでしょうか。 

エンティティ間のリレーションシップを作成できる場所は、リレーショナル データベースだけではありません。 ドキュメント データベース内で、あるドキュメント内の情報と、他のドキュメント内のデータを実際に関連付けることができます。 ここでは、DocumentDB のリレーショナル データベースや他のドキュメント データベースにより適したシステムを構築しようというわけではなく、シンプルなリレーションシップが適切であり、非常に役立ちます。 

以下の JSON では、前の株式ポートフォリオの例を使用していますが、ここでは株式に関する項目を埋め込むのではなく、ポートフォリオ上で参照しています。 これにより、株式の項目が終日頻繁に変更された場合でも、更新する必要があるのは 1 つの株式ドキュメントのみです。 

    Person document:
    {
        "id": "1",
        "firstName": "Thomas",
        "lastName": "Andersen",
        "holdings": [
            { "numberHeld":  100, "stockId": 1},
            { "numberHeld":  50, "stockId": 2}
        ]
    }
    
    Stock documents:
    {
        "id": 1,
        "symbol": "zaza",
        "open": 1,
        "high": 2,
        "low": 0.5,
        "vol": 11970000,
        "mkt-cap": 42000000,
        "pe": 5.89
    },
    {
        "id": 2,
        "symbol": "xcxc",
        "open": 89,
        "high": 93.24,
        "low": 88.87,
        "vol": 2970200,
        "mkt-cap": 1005000,
        "pe": 75.82
    }
    

ただし、この方法にも欠点があります。アプリケーションで個人のポートフォリオを表示する際に、保有する各株式の情報を表示する必要がある場合、何度もデータベースにアクセスして、各株式ドキュメントの情報を読み込むことが必要になります。 ここで、終日頻繁に発生する書き込み操作の効率を向上させ、この特定のシステムにはあまり影響がないと考えられる読み取り操作の効率を下げるという意思決定を行いました。

> [AZURE.NOTE] 正規化されたデータ モデル **より多くのラウンド トリップを要求することができます** サーバーにします。

### 外部キー
現在は制約の概念がないため、外部キーかどうかは別にして、ドキュメント間のリレーションシップがドキュメント内にあったとしても、実際には "弱いリンク" であり、データベース自体によって検証されることはありません。 ドキュメントが参照しているデータが実際に存在していることを確認したい場合は、アプリケーション内で確認するか、または DocumentDB 上でサーバー側トリガーかストアド プロシージャを使用して確認する必要があります。

###参照を使用する場合
一般に、次のような場合に正規化されたデータ モデルを使用します。

- 表す **一対多** リレーションシップです。
- 表す **多対多** リレーションシップです。
- 関連するデータを **頻繁に変更**します。
- 参照先のデータは、 **unbounded**します。

> [AZURE.NOTE] は、通常は、正規化 **書き込み** パフォーマンス。

###リレーションシップの場所
リレーションシップの拡大により、どのドキュメントに参照を格納するかを決定しやすくなります。

発行元と書籍をモデル化する、以下の JSON を見てみましょう。

    Publisher document:
    {
        "id": "mspress",
        "name": "Microsoft Press",
        "books": [ 1, 2, 3, ..., 100, ..., 1000]
    }

    Book documents:
    {"id": 1, "name": "DocumentDB 101" }
    {"id": 2, "name": "DocumentDB for RDBMS Users" }
    {"id": 3, "name": "Taking over the world one JSON doc at a time" }
    ...
    {"id": 100, "name": "Learn about Azure DocumentDB" }
    ...
    {"id": 1000, "name": "Deep Dive in to DocumentDB" }

発行元あたりの書籍数が少なく、増加率が限定的な場合は、書籍の参照を発行元ドキュメント内に格納することが有効な場合もあります。 ただし、発行元あたりの書籍数に制限がない場合は、このデータ モデルは上の発行元ドキュメントに示すような、拡大し続ける可変配列になります。 

位置を少し入れ替えることにより、モデルは、同じデータを表しながらも、このように大きい可変コレクションを回避できるようになります。

    Publisher document: 
    {
        "id": "mspress",
        "name": "Microsoft Press"
    }
    
    Book documents: 
    {"id": 1,"name": "DocumentDB 101", "pub-id": "mspress"}
    {"id": 2,"name": "DocumentDB for RDBMS Users", "pub-id": "mspress"}
    {"id": 3,"name": "Taking over the world one JSON doc at a time"}
    ...
    {"id": 100,"name": "Learn about Azure DocumentDB", "pub-id": "mspress"}
    ...
    {"id": 1000,"name": "Deep Dive in to DocumentDB", "pub-id": "mspress"}

上の例では、発行元ドキュメントで無制限のコレクションを使用していません。 代わりに、各書籍ドキュメントに発行元への参照が含まれているだけです。

###多対多のリレーションシップのモデル化
リレーショナル データベースで *多対多* だけ他のテーブルからレコードを結合する結合テーブルとリレーションシップがモデル化多くの場合。 

![テーブルの結合](./media/documentdb-modeling-data/join-table.png)

ドキュメントを使用して同じ内容を複製し、次の例のようなデータ モデルを作成したくなるかもしれません。

    Author documents: 
    {"id": 1, "name": "Thomas Andersen" }
    {"id": 2, "name": "William Wakefield" }
    
    Book documents:
    {"id": 1, "name": "DocumentDB 101" }
    {"id": 2, "name": "DocumentDB for RDBMS Users" }
    {"id": 3, "name": "Taking over the world one JSON doc at a time" }
    {"id": 4, "name": "Learn about Azure DocumentDB" }
    {"id": 5, "name": "Deep Dive in to DocumentDB" }
    
    Joining documents: 
    {"authorId": 1, "bookId": 1 }
    {"authorId": 2, "bookId": 1 }
    {"authorId": 1, "bookId": 2 }
    {"authorId": 1, "bookId": 3 }

これは、機能はします。 ただし、作者とその著作、または書籍とその作者の読み込みでは、常に、データベースに対して少なくとも 2 つの追加のクエリが必要になります。 1 つは結合するドキュメントに対するクエリ、もう 1 つは結合される実際のドキュメントを取得するクエリです。 

この結合テーブルで行われる処理が 2 つのデータ要素の結合だけであれば、これを削除してはどうでしょうか。
次の例を検討してみてください。

    Author documents:
    {"id": 1, "name": "Thomas Andersen", "books": [1, 2, 3]}
    {"id": 2, "name": "William Wakefield", "books": [1, 4]}
    
    Book documents: 
    {"id": 1, "name": "DocumentDB 101", "authors": [1, 2]}
    {"id": 2, "name": "DocumentDB for RDBMS Users", "authors": [1]}
    {"id": 3, "name": "Learn about Azure DocumentDB", "authors": [1]}
    {"id": 4, "name": "Deep Dive in to DocumentDB", "authors": [2]}

ここで、作者がわかっていれば、その著作はすぐにわかり、逆に書籍ドキュメントを読み込んでいれば、作者の ID がわかります。 これにより、結合テーブルに対する中間クエリは省略され、アプリケーションで行う必要があるサーバー ラウンド トリップの数が減少します。 

##<a id="WrapUp"></a>ハイブリッド データ モデル##
ここまで、データの埋め込み (非正規化) と参照 (正規化) を見てきましたが、それぞれに長所と短所がありました。 

必ずしもどちらか一方にする必要はなく、両方を多少併用してもかまいません。 

アプリケーション固有の使用パターンと負荷に基づき、埋め込みデータと参照データの併用が理にかなっていて、結果として、適切なパフォーマンス レベルを維持しながらアプリケーション ロジックがシンプルになり、サーバー ラウンド トリップが少なくなる場合もあります。

以下の JSON を検討してみてください。 

    Author documents: 
    {
        "id": 1,
        "firstName": "Thomas",
        "lastName": "Andersen",     
        "countOfBooks": 3,
        "books": [1, 2, 3],
        "images": [
            {"thumbnail": "http://....png"}
            {"profile": "http://....png"}
            {"large": "http://....png"}
        ]
    },
    {
        "id": 2,
        "firstName": "William",
        "lastName": "Wakefield",
        "countOfBooks": 1,
        "books": [1, 4, 5],
        "images": [
            {"thumbnail": "http://....png"}
        ]
    }
    
    Book documents:
    {
        "id": 1,
        "name": "DocumentDB 101",
        "authors": [
            {"id": 1, "name": "Thomas Andersen", "thumbnailUrl": "http://....png"},
            {"id": 2, "name": "William Wakefield", "thumbnailUrl": "http://....png"}
        ]
    },
    {
        "id": 2,
        "name": "DocumentDB for RDBMS Users",
        "authors": [
            {"id": 1, "name": "Thomas Andersen", "thumbnailUrl": "http://....png"},
        ]
    }

ここでは、(主に) 埋め込みモデルに従います。この埋め込みモデルでは、他のエンティティのデータが最上位のドキュメントに埋め込まれていますが、他のデータは参照されています。 

書籍ドキュメントを見ると、作者の配列にいくつか興味深いフィールドがあります。  *Id* は正規化モデルの標準的な技法、作者ドキュメントに戻って参照するために使用してフィールドをフィールドとしても *名前* と *thumbnailUrl*します。 だけ停止したことがお *id* およびその他の情報を取得するアプリケーションにしておきます「リンク」を使用してそれぞれの作者ドキュメントからそれを必要とが、アプリケーションでは各書籍の表示、作者名とサムネイル写真が表示されるので一覧内の書籍ごとのサーバーへのラウンド トリップお保存を非正規化して **一部** 作成者からのデータです。

確かに、作者名が変更された場合や、作者が写真を更新したい場合は、これまでに発行されたすべての書籍に対して更新を行う必要がありましたが、このアプリケーションでは、作者名はそれほど頻繁に変更されないという前提に基づいて、この設計が仕様を満たしていると判断しています。  

この例では **事前計算された集計** 読み取り操作で高価な処理を保存する値。 この例の作者ドキュメントに埋め込まれたデータの一部は、実行時に計算されるデータです。 書籍ドキュメントが作成された新しい書籍が発行されるたびに **と** countOfBooks フィールドは、特定の作成者に存在している書籍ドキュメントの数に基づいて計算された値に設定します。 この最適化は、読み取りの負荷が高く、読み取りを最適化するために書き込み時に計算を実行する余裕のあるシステムに適しています。

モデルに事前に計算されたフィールドを保持する機能を可能に DocumentDB をサポートするため **マルチ ドキュメント トランザクション**します。 多くの NoSQL ストアでは、ドキュメント間のトランザクションを実行できないため、この制限によって "常にすべてを埋め込む" などの設計における決定が提唱されています。 DocumentDB では、ACID トランザクション内で書籍の挿入や作者の更新をすべて実行する、サーバー側トリガー、またはストアド プロシージャを使用できます。 これで **が** 、データの整合性があることを確認するためだけに 1 つのドキュメントにすべてを埋め込みます。

##<a name="NextSteps"></a>次のステップ

この記事における最大の収穫は、スキーマのない状況でのデータのモデル化は以前として重要であると理解できたことです。 

データの要素を画面上に表現する方法が 1 つではないように、データをモデル化する方法も 1 つではありません。 使用するアプリケーションを理解し、アプリケーションがどのようにデータを作成、使用、処理するかを理解することが必要です。 次に、ここで示したガイドラインのいくつかを適用することにより、アプリケーションの当面のニーズに対応するモデルの作成を開始することができます。 アプリケーションの変更が必要な場合にも、スキーマのないデータベースの柔軟性を活用して、その変更を受け入れ、データ モデルを容易に進化させることができます。 

Azure DocumentDB の詳細については、サービスを参照してください [ドキュメント]( ../../services/documentdb/) ページです。 

詳細については、Azure DocumentDB のインデックスのチューニング、資料を参照 [インデックス作成ポリシー](documentdb-indexing-policies.md)します。

シャードに複数のパーティション間でデータを参照する方法を理解する [DocumentDB 内のデータのパーティション分割](documentdb-partition-data.md)します。 

最後に、データ モデリングとマルチ テナント アプリケーションのシャーディングのガイダンスについては、参照してください [Azure DocumentDB でのマルチ テナント アプリケーションの拡張](http://blogs.msdn.com/b/documentdb/archive/2014/12/03/scaling-a-multi-tenant-application-with-azure-documentdb.aspx)します。
 

