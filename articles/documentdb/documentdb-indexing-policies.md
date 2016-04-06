<properties 
    pageTitle="DocumentDB インデックス作成ポリシー | Microsoft Azure" 
    description="DocumentDB でのインデックス作成のしくみと、インデックス作成ポリシーを構成および変更する方法について説明します。 インデックス作成の自動化とパフォーマンス向上を目的として、DocumentDB でインデックス作成ポリシーを構成します。" 
    keywords="how indexing works, automatic indexing, indexing database, documentdb, azure, Microsoft azure"
    services="documentdb" 
    documentationCenter="" 
    authors="arramac" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="documentdb" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="data-services" 
    ms.date="10/05/2015" 
    ms.author="arramac"/>


# DocumentDB インデックス作成ポリシー

多くの顧客は DocumentDB を使用自動的に処理 [インデックスのすべての側面](documentdb-indexing.md), 、DocumentDB がカスタムの指定をサポートしても **インデックス作成ポリシー** コレクションの作成時にします。 DocumentDB でインデックス作成ポリシーを使用すると、他のインデックス作成データベース プラットフォームで提供されているセカンダリ インデックスよりも、より柔軟性の高い、強力なデザインが利用できるため、スキーマの柔軟性を損なうことがなく、インデックスの構造をカスタマイズできます。 DocumentDB でのインデックス作成のしくみを知るには、インデックス作成ポリシーを管理することで、インデックスのストレージ オーバーヘッド、書き込みとクエリのスループット、クエリの一貫性のバランスをきめ細かく調整できることを理解する必要があります。  

この記事では、DocumentDB インデックス作成ポリシー、インデックス作成ポリシーのカスタマイズの方法、関連するトレードオフの詳細について説明します。 

この記事を読むと、次の質問に回答できるようになります。

- DocumentDB は既定では自動インデックス作成をどのようにサポートするか。
- インデックス作成に含めるプロパティまたは除外するプロパティはどのようにオーバーライドすればよいか。
- 最終的な更新のインデックスはどのように構成すればよいか。
- Order By クエリまたは範囲クエリを実行できるようにインデックス作成を構成するにはどうすればよいか。
- コレクションのインデックス作成ポリシーに変更を加えるにはどうすればよいか。
- さまざまなインデックス作成ポリシーのストレージとパフォーマンスを比較するにはどうすればよいか。

##<a id="CustomizingIndexingPolicy"></a>コレクションのインデックス作成ポリシーのカスタマイズ

開発者は、DocumentDB コレクションで既定のインデックス作成ポリシーをオーバーライドし、次の機能を設定することによって、ストレージ、書き込みやクエリのパフォーマンス、クエリの一貫性の間のトレードオフをカスタマイズできます。

- **含める/除外ドキュメントとインデックス/パス**します。 開発者は、コレクションに挿入または置き換える際に、インデックスに含めたり除外したりする特定のドキュメントを選択できます。 開発者は、特定の JSON プロパティとも呼ばれますを含めたり除外したりもできます。 インデックスに含まれているドキュメント間でインデックスを作成する (ワイルドカード パターンを含め) のパス。
- **インデックスの種類を構成するさまざまな**です。 含まれるパスごとに、開発者はそのデータと予測されるクエリ ワークロード、各パスの数値や文字列の "有効桁数" に基づいて、コレクションに必要とされるインデックスの種類も指定できます。
- **インデックスの更新モードを構成する**です。 DocumentDB は、DocumentDB コレクションのインデックス作成ポリシーを使用して構成できる、3 つのインデックス作成モード (同期 (Consistent)、非同期 (Lazy)、なし (None)) をサポートしています。 

次の .NET コード スニペットは、コレクションの作成時に、カスタムのインデックス作成ポリシーを設定する方法を示します。 ここでは、最大有効桁数で文字列や数値の範囲のインデックスを持つポリシーを設定します。 このポリシーでは、文字列に対して Order By クエリを実行することができます。

    var collection = new DocumentCollection { Id = "myCollection" };
    
    collection.IndexingPolicy.IndexingMode = IndexingMode.Consistent;
    
    collection.IndexingPolicy.IncludedPaths.Add(
        new IncludedPath { 
            Path = "/*", 
            Indexes = new Collection<Index> { 
                new RangeIndex(DataType.String) { Precision = -1 }, 
                new RangeIndex(DataType.Number) { Precision = -1 }
            }
        });

    await client.CreateDocumentCollectionAsync(database.SelfLink, collection);   


>[AZURE.NOTE] インデックス作成ポリシーの JSON スキーマは、文字列に対して範囲インデックスをサポートするために REST API バージョン 2015年-06-03 のリリースで変更されました。 .NET SDK の 1.2.0、Java、Python、および Node.js Sdk の 1.1.0 は、新しいポリシー スキーマをサポートします。 古い SDK では、REST API バージョン 2015-04-08 を使用してインデックス作成ポリシーの古いスキーマをサポートしています。
>
>既定では、DocumentDB はハッシュ インデックスに応じたドキュメント内のすべての文字列プロパティと、範囲インデックスを含む数値プロパティのインデックスを作成します。  

### データベース インデックス作成モード

DocumentDB は、DocumentDB コレクションのインデックス作成ポリシーを使用して構成できる、同期 (Consistent)、非同期 (Lazy)、なし (None) の 3 つのインデックス作成モードをサポートしています。

**一貫性のある**: 指定された DocumentDB コレクションに対するクエリ、DocumentDB コレクションのポリシーが「一貫性」として指定されている場合は、次の指定したポイントの読み取りのと同じ一貫性レベル (strong、bounded-staleness、session、eventual の中)。 インデックスは、ドキュメントの更新 (DocumentDB コレクション内のドキュメントの挿入、置換、更新、削除など) の一部として、同期的に更新されます。  同期 (Consistent) インデックス作成では、書き込みスループットの短縮によって、一貫性のあるクエリがサポートされます。 この短縮は、インデックスを作成する必要がある一意のパスの機能と "一貫性レベル" によるものです。 同期 (Consistent) インデックス作成モードは、"すばやく書き込み、即座にクエリ" するワークロード向けです。

**限定的な**: は最大ドキュメント取り込みスループットを許可するように、DocumentDB コレクションを限定的な一貫性を構成できます。 意味クエリは、最終的に一貫性のあります。 インデックスは、DocumentDB コレクションが休止状態 (コレクションのスループット容量がユーザー要求の処理に完全に利用されない場合) の間は非同期的に更新されます。 制約を受けずにドキュメントを取り込む必要のある、"今すぐ取り込み、後でクエリ する" ワークロードでは、"非同期 (lazy)" インデックス作成モードが適しています。

**[なし]**:「なし」のインデックスのモードでマークされたコレクションに関連付けられているインデックスがありません。 インデックス作成ポリシーを "なし (None)" で設定すると、既存のインデックスを削除してしまうという影響があります。

>[AZURE.NOTE] 既存のインデックスを削除するという副作用を持つ"None"にインデックス作成ポリシーを構成します。 アクセス パターンで必要なのが "ID" か "自己リンク" (または両方) のみの場合は、このオプションを使用します。

次に示すサンプルは、すべてのドキュメントの挿入に一貫性のある自動インデックス作成と .NET SDK を使用して DocumentDB コレクションを作成する方法を示します。

次の表は、コレクション用に構成されたインデックス作成モード (同期 (Consistent) と非同期 (Lazy)) に基づいたクエリの一貫性と、クエリ要求に指定された一貫性レベルを示します。 これは、任意のインターフェイス (REST API、SDK) を使用して作成されたクエリや、ストアド プロシージャやトリガーから作成されたクエリに適用されます。 

<table border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td valign="top">
                <p>
                </p>
            </td>
            <td valign="top">
                <p>
                    <strong>一貫性のあります。</strong>
                </p>
            </td>
            <td valign="top">
                <p>
                    <strong>最短一致</strong>
                </p>
            </td>            
        </tr>
        <tr>
            <td valign="top">
                <p>
                    <strong>厳密な</strong>
                </p>
            </td>
            <td valign="top">
                <p>
                    Strong
                </p>
            </td>
            <td valign="top">
                <p>
                    Eventual
                </p>
            </td>            
        </tr>       
        <tr>
            <td valign="top">
                <p>
                    <strong>Bounded Staleness</strong>
                </p>
            </td>
            <td valign="top">
                <p>
                    Bounded Staleness
                </p>
            </td>
            <td valign="top">
                <p>
                    Eventual
                </p>
            </td>            
        </tr>          
        <tr>
            <td valign="top">
                <p>
                    <strong>セッション</strong>
                </p>
            </td>
            <td valign="top">
                <p>
                    Session
                </p>
            </td>
            <td valign="top">
                <p>
                    Eventual
                </p>
            </td>            
        </tr>      
        <tr>
            <td valign="top">
                <p>
                    <strong>最終的です</strong>
                </p>
            </td>
            <td valign="top">
                <p>
                    Eventual
                </p>
            </td>
            <td valign="top">
                <p>
                    Eventual
                </p>
            </td>            
        </tr>         
    </tbody>
</table>

既定では、コレクションが "なし (None)" インデックス作成モードで設定された場合、クエリの処理にスキャンが必要である可能性を通知するためにすべてのクエリでエラーが返されます。 `x-ms-documentdb-enable-scans` ヘッダーを REST API で使用するか、.NET SDK を使用した `EnableScanInQuery` 要求オプションで使用すれば、範囲インデックスがなくても、これらのクエリを実行できます。 ORDER BY を使用するクエリなどの一部のクエリは、`EnableScanInQuery` を使用した場合でも、"なし (None)" での使用は許可されません。

次の表は、EnableScanInQuery を指定した場合のインデックス作成モード (同期 (Consistent)、非同期 (Lazy)、なし (None)) に基づいたクエリの一貫性を示します

<table border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td valign="top">
                <p>
                </p>
            </td>
            <td valign="top">
                <p>
                    <strong>一貫性のあります。</strong>
                </p>
            </td>
            <td valign="top">
                <p>
                    <strong>最短一致</strong>
                </p>
            </td>       
            <td valign="top">
                <p>
                    <strong>[なし]</strong>
                </p>
            </td>             
        </tr>
        <tr>
            <td valign="top">
                <p>
                    <strong>厳密な</strong>
                </p>
            </td>
            <td valign="top">
                <p>
                    Strong
                </p>
            </td>
            <td valign="top">
                <p>
                    Eventual
                </p>
            </td>    
            <td valign="top">
                <p>
                    Strong
                </p>
            </td>                
        </tr>       
        <tr>
            <td valign="top">
                <p>
                    <strong>Bounded Staleness</strong>
                </p>
            </td>
            <td valign="top">
                <p>
                    Bounded Staleness
                </p>
            </td>
            <td valign="top">
                <p>
                    Eventual
                </p>
            </td>      
            <td valign="top">
                <p>
                    Bounded Staleness
                </p>
            </td> 
        </tr>          
        <tr>
            <td valign="top">
                <p>
                    <strong>セッション</strong>
                </p>
            </td>
            <td valign="top">
                <p>
                    Session
                </p>
            </td>
            <td valign="top">
                <p>
                    Eventual
                </p>
            </td>   
            <td valign="top">
                <p>
                    Session
                </p>
            </td>             
        </tr>      
        <tr>
            <td valign="top">
                <p>
                    <strong>最終的です</strong>
                </p>
            </td>
            <td valign="top">
                <p>
                    Eventual
                </p>
            </td>
            <td valign="top">
                <p>
                    Eventual
                </p>
            </td>      
            <td valign="top">
                <p>
                    Eventual
                </p>
            </td>              
        </tr>         
    </tbody>
</table>

次に示すコード サンプルは、すべてのドキュメントの挿入に一貫性のあるインデックス作成モードで .NET SDK を使用して DocumentDB コレクションを作成する方法を示します。

     // Default collection creates a hash index for all string and numeric    
     // fields. Hash indexes are compact and offer efficient
     // performance for equality queries.
     
     var collection = new DocumentCollection { Id ="defaultCollection" };
     
     collection.IndexingPolicy.IndexingMode = IndexingMode.Consistent;
     
     collection = await client.CreateDocumentCollectionAsync(database.SelfLink, collection);


### インデックスのパス

DocumentDB は JSON ドキュメントとインデックスをツリーとしてモデル化し、ツリー内のパスに対するポリシーを調整できるようにします。 この詳細についてを参照して [DocumentDB インデックス作成の概要](documentdb-indexing.md)します。 ドキュメントでインデックス作成に含めたり除外したりするパスを選択できます。 これにより、クエリのパターンが事前にわかっている場合に、書き込みパフォーマンスの向上とインデックス ストレージの削減が可能になります。

インデックスのパスはルート (/) で始まり、通常、ですか? プレフィックスの複数の可能な値があることを示すワイルドカード演算子です。 たとえば、select * FROM Families F WHERE F.familyName ="Andersen"/familyName/のインデックスのパスを含める必要がありますか。 コレクションのインデックス ポリシー。

インデックスのパスは * ワイルドカード演算子を使用して、プレフィックスの下のパスの動作を再帰的に指定することもできます。 たとえば、/payload/* を使用して、payload プロパティの下のすべてをインデックス作成から除外できます。

インデックスのパスを指定するための一般的なパターンは次のとおりです。

<table border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td valign="top">
                <p>
                    <strong>パス</strong>
                </p>
            </td>
            <td valign="top">
                <p>
                    <strong>説明またはユース ケース</strong>
                </p>
            </td>
        </tr>
        <tr>
            <td valign="top">
                <p>
                    /*
                </p>
            </td>
            <td valign="top">
                <p>
                    Default path for collection. Recursive and applies to whole document tree.
                </p>
            </td>
        </tr>
        <tr>
            <td valign="top">
                <p>
                    /prop/?
                </p>
            </td>
            <td valign="top">
                <p>
                    Index path required to serve queries like the following (with Hash or Range types respectively):
                </p>
                <p>
                    SELECT * FROM collection c WHERE c.prop = "value"
                </p>
                <p>
                    SELECT * FROM collection c WHERE c.prop &gt; 5
                </p>
                <p>
                    SELECT * FROM collection c ORDER BY c.prop
                </p>                
            </td>
        </tr>
        <tr>
            <td valign="top">
                <p>
                    /prop/*
                </p>
            </td>
            <td valign="top">
                <p>
                    Index path for all paths under the specified label. Works with the following queries
                </p>
                <p>
                    SELECT * FROM collection c WHERE c.prop = "value"
                </p>
                <p>
                    SELECT * FROM collection c WHERE c.prop.subprop &gt; 5
                </p>
                <p>
                    SELECT * FROM collection c WHERE c.prop.subprop.nextprop = "value"
                </p>
                <p>
                    SELECT * FROM collection c ORDER BY c.prop
                </p>
            </td>
        </tr>
        <tr>
            <td valign="top">
                <p>
                    /props/[]/?
                </p>
            </td>
            <td valign="top">
                <p>
                    Index path required to serve iteration and JOIN queries against arrays of scalars like ["a", "b", "c"]:
                </p>
                <p>
                    SELECT tag FROM tag IN collection.props WHERE tag = "value"
                </p>
                <p>
                    SELECT tag FROM collection c JOIN tag IN c.props WHERE tag > 5
                </p>
            </td>
        </tr>
        <tr>
            <td valign="top">
                <p>
                    /props/[]/subprop/?
                </p>
            </td>
            <td valign="top">
                <p>
                    Index path required to serve iteration and JOIN queries against arrays of objects like [{subprop: "a"}, {subprop: "b"}]:
                </p>
                <p>
                    SELECT tag FROM tag IN collection.props WHERE tag.subprop = "value"
                </p>
                <p>
                    SELECT tag FROM collection c JOIN tag IN c.props WHERE tag.subprop = "value"
                </p>
            </td>
        </tr>        
        <tr>
            <td valign="top">
                <p>
                    /prop/subprop/?
                </p>
            </td>
            <td valign="top">
                <p>
                    Index path required to serve queries (with Hash or Range types respectively):
                </p>
                <p>
                    SELECT * FROM collection c WHERE c.prop.subprop = "value"
                </p>
                <p>
                    SELECT * FROM collection c WHERE c.prop.subprop &gt; 5
                </p>
                <p>
                    SELECT * FROM collection c ORDER BY c.prop.subprop
                </p>                
            </td>
        </tr>
    </tbody>
</table>

>[AZURE.NOTE] カスタム インデックスのパスを設定するときに、特定のパスで表されるドキュメント全体のツリーの既定のインデックス作成ルールを指定する必要がある"/*"です。 

次の例では、範囲インデックス作成と 20 バイトのカスタムの有効桁数を使用して、特定のパスを構成します。

    var collection = new DocumentCollection { Id = "rangeSinglePathCollection" };    
    
    collection.IndexingPolicy.IncludedPaths.Add(
        new IncludedPath { 
            Path = "/Title/?", 
            Indexes = new Collection<Index> { 
                new RangeIndex(DataType.String) { Precision = 20 } } 
            });

    // Default for everything else
    collection.IndexingPolicy.IncludedPaths.Add(
        new IncludedPath { 
            Path = "/*" ,
            Indexes = new Collection<Index> {
                new HashIndex(DataType.String) { Precision = 3 }, 
                new RangeIndex(DataType.Number) { Precision = -1 } 
            }
        });
        
    collection = await client.CreateDocumentCollectionAsync(database.SelfLink, pathRange);

### インデックスのデータ型、種類、および有効桁数

パスの指定方法について説明したので、ここからはパスのインデックス作成ポリシーの構成で使用できるオプションを見てみましょう。 すべてのパスに対して 1 つ以上のインデックス作成定義を指定することができます。

- データ型: **文字列**, 、**数** または **ポイント** (パスごとのデータ型ごとに 1 つだけのエントリを含めることができます)
- インデックスの種類: **ハッシュ** (等値クエリ)、 **範囲** (等値、範囲または Order By クエリ) または **空間** (空間クエリ) 
- 有効桁数:  数値の場合は 1 ～ 8 または -1 (最大有効桁数)、文字列の場合は 1 ～ 100 (最大有効桁数)

#### インデックスの種類

DocumentDB は、(文字列、数値、またはその両方に対して構成できる) すべてのパスに対して、Hash と Range の 2 種類のインデックスをサポートします。

- **ハッシュ** 効率的な等値と結合クエリをサポートしています。 ほとんどのケースでは、ハッシュ インデックスには既定値 3 バイトより高い有効桁数は不要です。
- **範囲** 効率的な等値クエリ、範囲クエリをサポートしています (を使用して >、<>、=、< =、! =)、および Order By クエリ。 既定では Order By クエリには、インデックスの最大有効桁数 (-1) も必要です。

DocumentDB では、Point データ型に指定できるすべてのパスに対するインデックスの種類 Spatial もサポートしています。 指定されたパスの値は、`{"type": "Point", "coordinates": [0.0, 10.0]}` のような有効な GeoJSON ポイントである必要があります。

- **空間** 効率的なサポート空間 (内との距離) クエリです。

サポートされているインデックスの種類とそれに対して使用可能なクエリの例を示します。

<table border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td valign="top">
                <p>
                    <strong>インデックスの種類</strong>
                </p>
            </td>
            <td valign="top">
                <p>
                    <strong>説明またはユース ケース</strong>
                </p>
            </td>
        </tr>
        <tr>
            <td valign="top">
                <p>
                    Hash
                </p>
            </td>
            <td valign="top">
                <p>
                    Hash over /prop/? (or /*) can be used to serve the following queries efficiently:
                      SELECT * FROM collection c WHERE c.prop = "value"
                    Hash over /props/[]/? (or /* or /props/*) can be used to serve the following queries efficiently:
                      SELECT tag FROM collection c JOIN tag IN c.props WHERE tag = 5
                </p>
            </td>
        </tr>
        <tr>
            <td valign="top">
                <p>
                    Range
                </p>
            </td>
            <td valign="top">
                <p>
                    Range over /prop/? (or /*) can be used to serve the following queries efficiently:
                        SELECT * FROM collection c WHERE c.prop = "value"
                        SELECT * FROM collection c WHERE c.prop > 5
                        SELECT * FROM collection c ORDER BY c.prop
                </p>
            </td>
        </tr>
        <tr>
            <td valign="top">
                <p>
                    Spatial
                </p>
            </td>
            <td valign="top">
                <p>
                    Range over /prop/? (or /*) can be used to serve the following queries efficiently:
                        SELECT * FROM collection c 
                        WHERE ST_DISTANCE(c.prop, {"type": "Point", "coordinates": [0.0, 10.0]}) < 40
                        選択 * コレクション c ST_WITHIN(c.prop, {"type":"Polygon",...}) 場所から
                </p>
            </td>
        </tr>        
    </tbody>
</table>

既定では、範囲インデックス (有効桁数は任意) が存在しない場合、>= のような範囲演算子を使用したクエリに対してはエラーが返され、このクエリを処理するにはスキャンが必要であることが通知されます。 x-ms-documentdb-enable-scans ヘッダーを REST API で使用するか、または .NET SDK を使用した EnableScanInQuery 要求オプションで使用すれば、範囲インデックスがなくても、範囲クエリを実行することができます。 DocumentDB でインデックスを使用してフィルター処理できるその他のフィルターがクエリにない場合、エラーは返されません。

同じ規則が空間クエリにも適用されます。 既定では、空間インデックスがなく、インデックスからサービスを提供できる他のフィルターがない場合、空間クエリにエラーが返されます。 これらは、x-ms-documentdb-enable-scan/EnableScanInQuery を使用して、スキャンとして実行できます。

#### インデックスの有効桁数

インデックスの有効桁数により、インデックスのストレージ オーバーヘッドとクエリのパフォーマンスをトレードオフできます。 
数値の場合、既定の有効桁数の構成 -1 (最大) を使用することをお勧めします。 JSON では数値は 8 バイトであるため、これは、8 バイトの構成と同じです。 1 ～ 7 などの小さい値の有効桁数を選択すると、特定の範囲内の値が同じインデックス エントリにマップされることになります。 したがって、インデックスのストレージ領域が削減されますが、クエリの実行でより多くのドキュメントを処理しなければならなくなるため、使用されるスループット、つまり要求単位も増えます。

インデックスの有効桁数の構成は、文字列の範囲を使用するとより役立ちます。 文字列は任意の長さにできるため、インデックスの有効桁数を選択することで、文字列の範囲クエリのパフォーマンスと、必要なインデックスのストレージ領域の量に影響を与えることができます。 文字列の範囲インデックスは 1～100 または -1 (最大) で構成できます。 文字列のプロパティに Order By クエリを実行する場合は、対応するパスに有効桁数 -1 を指定する必要があります。

空間インデックスでは、常にポイント用の既定のインデックスの有効桁数を使用するため、オーバーライドすることはできません。 

次の例では、.NET SDK を使用して、コレクション内の範囲インデックスの有効桁数を増やす方法を示します。 この場合、既定のパス"/*" が使用されます。

**カスタム インデックスの有効桁数でコレクションを作成する**

    var rangeDefault = new DocumentCollection { Id = "rangeCollection" };
    
    rangeDefault.IndexingPolicy.IncludedPaths.Add(
        new IncludedPath { 
            Path = "/*", 
            Indexes = new Collection<Index> { 
                new RangeIndex(DataType.String) { Precision = -1 }, 
                new RangeIndex(DataType.Number) { Precision = -1 }
            }
        });

    await client.CreateDocumentCollectionAsync(database.SelfLink, rangeDefault);   


> [AZURE.NOTE] DocumentDB は、クエリが Order By を使用してのに、最大有効桁数でのクエリ パスに対して範囲インデックスが存在しない場合、エラーを返します。 

同様に、インデックス作成からパスを完全に除外することができます。 次の例は、(ルール サブタイプ)、ドキュメントのセクション全体を除外する方法を示しています。 サブツリー) を使用してインデックス作成から、"*"ワイルドカードです。

    var collection = new DocumentCollection { Id = "excludedPathCollection" };
    collection.IndexingPolicy.IncludedPaths.Add(new IncludedPath { Path = "/" });
    collection.IndexingPolicy.ExcludedPaths.Add(new ExcludedPath { Path = "/nonIndexedContent/*");
    
    collection = await client.CreateDocumentCollectionAsync(database.SelfLink, excluded);


## インデックス作成の有効化と無効化

コレクションがすべてのドキュメントのインデックスを自動的に作成するかどうかを選択できます。 既定では、すべてのドキュメントのインデックスが自動的に作成されますが、これを無効にすることができます。 自動インデックス作成が無効になっている場合、自己リンクまたは ID を使用したクエリでのみドキュメントにアクセスできます。

自動インデックス作成が無効になっている場合でも、特定のドキュメントだけを選択してインデックスに追加できます。 反対に、自動インデックス作成を有効にしたまま、特定のドキュメントだけを選択して除外できます。 インデックス作成の有効または無効の構成は、ドキュメントのサブセットだけにクエリを実行する必要がある場合に役立ちます。

たとえば、次の例を使用して明示的にドキュメントを含める方法、 [DocumentDB .NET SDK](https://github.com/Azure/azure-documentdb-java) と [RequestOptions.IndexingDirective](http://msdn.microsoft.com/library/microsoft.azure.documents.client.requestoptions.indexingdirective.aspx) プロパティです。

    // If you want to override the default collection behavior to either
    // exclude (or include) a Document from indexing,
    // use the RequestOptions.IndexingDirective property.
    client.CreateDocumentAsync(defaultCollection.SelfLink,
        new { id = "AndersenFamily", isRegistered = true },
        new RequestOptions { IndexingDirective = IndexingDirective.Include });

## コレクションのインデックス作成ポリシーの変更

DocumentDB を使用すると、その場でコレクションのインデックス作成ポリシーを変更できます。 DocumentDB コレクションでインデックス作成ポリシーを変更すると、インデックス自体の一貫性モデルだけでなく、インデックスを作成できるパスやその有効桁数などのインデックスの構造も変更される可能性があります。 そのため、インデックス作成ポリシーの変更では、以前のインデックスから新しいインデックスへの変換が求められます。 

**オンラインのインデックスの変換**

![インデックス作成のしくみ - DocumentDB オンライン インデックス変換](media/documentdb-indexing-policies/index-transformations.png)

インデックスの変換をオンラインでは、新しいポリシーに従って、古いポリシーごとにインデックスのドキュメントを効率的に変換する意味で行う **書き込み可用性またはプロビジョニング済みスループットに影響を与えずに** のコレクション。 インデックス変換中、SDK、REST API を使ったり、ストアド プロシージャやトリガー内から確立された読み取り操作と書き込み操作の一貫性に影響はありません。 つまり、インデックス作成ポリシーを変更しても、アプリに対するパフォーマンスの低下やダウンタイムはありません。

ただし、インデックス変換の進行中は、インデックス作成モードの構成が同期 (Consistent) か非同期 (Lazy) かに関係なく、最終的にクエリの一貫性は保たれます。 これは、任意のインターフェイス (REST API、SDK) を使用して作成されたクエリや、ストアド プロシージャやトリガーから作成されたクエリにも適用されます。 非同期 (Lazy) インデックス作成と同様に、インデックスの変換は、指定したレプリカで利用可能なスペア リソースを使用して、バックグラウンドで非同期的に行われます。 

インデックスの変換も行われます **その場で** (インプレース)、つまり DocumentDB インデックスの 2 つのコピーを保持し、ない以前のインデックスを新しいをスワップします。 これは、インデックス変換の実行中に追加のディスク領域が必要ないことを意味します。また、コレクションで使用されることもありません。

インデックス作成ポリシーを変更する場合に以前のインデックスから新しいインデックスへの変更の適用方法は、包含/除外パス、インデックスの種類、有効桁数などのその他の値よりも、主にインデックス作成モードの構成によって左右されます。 以前のポリシーと新しいポリシーの両方で同期 (Consistent) インデックス作成を使用する場合、DocumentDB はオンラインでインデックス変換を実行します。 変換の進行中は、同期 (Consistent) インデックス作成モードで別のインデックス作成ポリシーの変更を適用することはできません。

ただし、変換の進行中に、非同期 (Lazy) や、なし (None) インデックス作成モードに移動することは可能です。 

- 非同期 (Lazy) に移動すると、インデックス ポリシーの変更はすぐに有効になり、DocumentDB はインデックスの再作成を非同期的に開始します。 
- なし (None) に移動すると、インデックスはすぐに無効になります。 なし (None) への移動は、進行中の変換を中止して別のインデックス作成ポリシーを最初から開始する場合に便利です。 

新しいを使用して、インデックス作成のポリシーの変更の開始する場合は、.NET SDK を使用している **ReplaceDocumentCollectionAsync** メソッドを使用してインデックス変換の進行状況を追跡、 **IndexTransformationProgress** から応答プロパティ、 **ReadDocumentCollectionAsync** を呼び出します。 その他の SDK と REST API は、インデックス作成ポリシーを変更するための同等のプロパティとメソッドをサポートします。

次のコード スニペットは、コレクションのインデックス作成ポリシーを同期 (Consistent) から非同期 (Lazy) に変更する方法を示しています。

**インデックス作成ポリシーを同期 (Consistent) から非同期 (Lazy) に変更する**

    // Switch to lazy indexing.
    Console.WriteLine("Changing from Default to Lazy IndexingMode.");

    collection.IndexingPolicy.IndexingMode = IndexingMode.Lazy;

    await client.ReplaceDocumentCollectionAsync(collection);


次のように、インデックス変換の進行状況は、ReadDocumentCollectionAsync を呼び出すことで確認できます。

**インデックス変換の進行状況の追跡**

    long smallWaitTimeMilliseconds = 1000;
    long progress = 0;

    while (progress < 100)
    {
        ResourceResponse<DocumentCollection> collectionReadResponse = await     client.ReadDocumentCollectionAsync(collection.SelfLink);
        progress = collectionReadResponse.IndexTransformationProgress;

        await Task.Delay(TimeSpan.FromMilliseconds(smallWaitTimeMilliseconds));
    }

コレクションのインデックスを削除するには、なし (None) インデックス作成モードに移動します。 これは、進行中の変換をキャンセルして、新しい変換をすぐに開始する場合に便利な運用ツールです。

**コレクションのインデックスの削除**

    // Switch to lazy indexing.
    Console.WriteLine("Dropping index by changing to to the None IndexingMode.");

    collection.IndexingPolicy.IndexingMode = IndexingMode.None;

    await client.ReplaceDocumentCollectionAsync(collection);

DocumentDB コレクションに対してインデックス作成ポリシーを変更するのはどのような場合でしょうか。 一般的な使用例を次に示します。

- 通常の操作では一貫した結果を使用し、データの一括インポート時には非同期 (Lazy) インデックス作成に戻す場合
- 現在の DocumentDB コレクションで、インデックスの種類 Spatial が必要な地理空間クエリ、インデックスの種類 Range が必要な Order By や文字列の範囲クエリなど、新しいインデックス作成機能の使用を開始する場合
- インデックスが作成されるプロパティを手動で選択し、時間の経過と共に変更する場合
- インデックス作成時の有効桁数を調整してクエリのパフォーマンスを向上するか、消費されているストレージを減らす場合

>[AZURE.NOTE] ReplaceDocumentCollectionAsync を使用してインデックス作成ポリシーを変更するには、バージョンを必要 > .NET SDK の 1.3.0 以降
>
> インデックス変換を正常に完了するために、コレクションに十分な空きストレージ空間があることを確認してください。 コレクションがそのストレージ クォータに到達すると、インデックス変換が一時停止します。 ストレージ空間が利用可能になると、たとえば、ドキュメントをいくつか削除した場合、インデックス変換は自動的に再開します。

## パフォーマンスのチューニング

DocumentDB API は使用されているインデックス ストレージ、すべての操作のスループット コスト (要求単位) などのパフォーマンス メトリックに関する情報を提供します。 この情報は、さまざまなインデックス作成ポリシーの比較やパフォーマンス チューニングに使用することができます。

コレクションの記憶域のクォータと使用状況を確認するには、HEAD 要求または GET 要求をコレクションのリソースに対して実行し、x-ms-request-quota と x-ms-request-usage のヘッダーを調べます。 .NET SDK で、 [DocumentSizeQuota](http://msdn.microsoft.com/library/dn850325.aspx) と [DocumentSizeUsage](http://msdn.microsoft.com/library/azure/dn850324.aspx) でプロパティ [ResourceResponse < 使わないで >](http://msdn.microsoft.com/library/dn799209.aspx) これらの対応する値が含まれています。

     // Measure the document size usage (which includes the index size) against   
     // different policies.        
     ResourceResponse<DocumentCollection> collectionInfo = await client.ReadDocumentCollectionAsync(collectionSelfLink);  
     Console.WriteLine("Document size quota: {0}, usage: {1}", collectionInfo.DocumentQuota, collectionInfo.DocumentUsage);


書き込み操作のたびにインデックス作成のオーバーヘッドを測定 (作成、更新、または削除) x ms 要求負担ヘッダーを調べて (または同等の [RequestCharge](http://msdn.microsoft.com/library/dn799099.aspx) プロパティ [ResourceResponse < 使わないで >](http://msdn.microsoft.com/library/dn799209.aspx) .NET sdk) をこれらの操作で使用される要求単位の数を計測します。

     // Measure the performance (request units) of writes.     
     ResourceResponse<Document> response = await client.CreateDocumentAsync(collectionSelfLink, myDocument);              
     Console.WriteLine("Insert of document consumed {0} request units", response.RequestCharge);
     
     // Measure the performance (request units) of queries.    
     IDocumentQuery<dynamic> queryable =  client.CreateDocumentQuery(collectionSelfLink, queryString).AsDocumentQuery();                                  
     double totalRequestCharge = 0;
     while (queryable.HasMoreResults)
     {
        FeedResponse<dynamic> queryResponse = await queryable.ExecuteNextAsync<dynamic>(); 
        Console.WriteLine("Query batch consumed {0} request units",queryResponse.RequestCharge);
        totalRequestCharge += queryResponse.RequestCharge;
     }
     
     Console.WriteLine("Query consumed {0} request units in total", totalRequestCharge);

## インデックス作成ポリシーの仕様の変更
インデックス作成ポリシーにおけるスキーマの変更は、2015 年 7 月 7 日に、REST API のバージョン 2015-06-03 により導入されました。 SDK バージョン内の該当するクラスには、そのスキーマに適合する新しい実装が含まれています。 

JSON 仕様に次の変更が実装されました。

- インデックス作成ポリシーでは文字列に対して範囲インデックスをサポートします。
- 各パスのインデックス定義は複数であってもかまいません (各データ型に 1 つ)。
- インデックス作成時の有効桁数は、数字に対する 1 ～ 8、文字列に対する 1 ～ 100、および -1 (最大有効桁数) に対応します。
- パス セグメントに、各パスをエスケープするための二重引用符は必要ありません。 たとえば、/title/? パスを追加することができますか。 代わりに/"title"/ですか?
- 「すべてのパス」を表すルート パスは、/* (/ に加えて) で表現できます。

バージョン 1.1.0 以前の.NET SDK で作成されたカスタム インデックス作成ポリシーを使用してコレクションをプロビジョニングするコードがある場合、SDK バージョン 1.2.0 に移行するには、アプリケーション コードを変更して変更内容に対応する必要があります。 インデックス作成ポリシーを構成するコードがない場合、または以前の SDK バージョンを引き続き使用する場合、変更の必要はありません。

実際に比較を行うため、REST API のバージョン 2015-06-03 と以前のバージョン 2015-04-08 を使用して記述されたカスタム インデックス作成ポリシーの例を示します。

**以前のインデックス作成ポリシー JSON**

    {
       "automatic":true,
       "indexingMode":"Consistent",
       "IncludedPaths":[
          {
             "IndexType":"Hash",
             "Path":"/",
             "NumericPrecision":7,
             "StringPrecision":3
          }
       ],
       "ExcludedPaths":[
          "/\"nonIndexedContent\"/*"
       ]
    }

**現在のインデックス作成ポリシー JSON**

    {
       "automatic":true,
       "indexingMode":"Consistent",
       "includedPaths":[
          {
             "path":"/*",
             "indexes":[
                {
                   "kind":"Hash",
                   "dataType":"String",
                   "precision":3
                },
                {
                   "kind":"Hash",
                   "dataType":"Number",
                   "precision":7
                }
             ]
          }
       ],
       "ExcludedPaths":[
          {
             "path":"/nonIndexedContent/*"
          }
       ]
    }

## 次のステップ

次のリンク先で、インデックス ポリシー管理のサンプルや DocumentDB のクエリ言語の詳細を参照できます。

1.  [DocumentDB .NET のインデックス管理のコード サンプル](https://github.com/Azure/azure-documentdb-net/blob/master/samples/code-samples/IndexManagement/Program.cs)
2.  [DocumentDB ドキュメント コレクションに対する操作](https://msdn.microsoft.com/library/azure/dn782195.aspx)
3.  [DocumentDB SQL を使用したクエリ](documentdb-sql-query.md)

 



