<properties
   pageTitle="Marketplace 用のデータ サービスを作成するためのガイド |Microsoft Azure"
   description="Azure Marketplace で購入できるデータ サービスを作成、認定、デプロイする方法について詳しく説明します。"
   services="marketplace-publishing"
   documentationCenter=""
   authors="HannibalSII"
   manager=""
   editor=""/>

   <tags
      ms.service="marketplace-publishing"
      ms.devlang="na"
      ms.topic="article"
      ms.tgt_pltfrm="na"
      ms.workload="na"
      ms.date="11/30/2015"
      ms.author="hascipio; avikova" />

# CSDL を使用して既存の Web サービスを OData にマップするためのノード スキーマについて
このドキュメントでは、OData プロトコルを CSDL にマップするためのノード構造について説明します。 ノード構造は整形式 XML である点に注意してください。 このため、OData マッピングを設計する際は、ルート、親、子のスキーマが適用されます。

## 無視される要素
以下に示すのは、Web サービスのメタデータのインポート時に Azure Marketplace バックエンドで使用されない上位の CSDL 要素 (XML ノード) です。 これらは表示される場合がありますが、無視されます。

| 要素 | Scope |
|----|----|
| Using 要素 | ノード、サブ ノード、およびすべての属性 |
| Documentation 要素 | ノード、サブ ノード、およびすべての属性 |
| ComplexType 要素 | ノード、サブ ノード、およびすべての属性 |
| Association 要素 | ノード、サブ ノード、およびすべての属性 |
| 拡張プロパティ | ノード、サブ ノード、およびすべての属性 |
| EntityContainer 要素 | 次の属性のみが無視されます: *拡張* と *AssociationSet* |
| Schema | 次の属性のみが無視されます: *名前空間* |
| FunctionImport 要素 | 次の属性のみが無視されます: *モード* (ln の既定値が前提です) |
| EntityType | 次のサブ ノードだけが無視されます: *キー* と *PropertyRef* |

各 CSDL XML ノードに対する変更 (追加された要素と無視される要素) について、次で詳しく説明します。

## FunctionImport ノード
FunctionImport ノードは、エンド ユーザーにサービスを公開する 1 つの URL (エントリ ポイント) を表します。 このノードでは、URL を指定する方法、エンド ユーザーに提供するパラメーターの種類、およびこれらのパラメーターを提供する方法を記述できます。

詳細については、このノードがある [http://msdn.microsoft.com/library/cc716710 (v=vs.100).aspx][MSDNFunctionImportLink]

[MSDNFunctionImportLink]:'http://msdn.microsoft.com/library/cc716710(v=vs.100).aspx'

FunctionImport ノードで公開されている追加の属性 (または属性への追加事項) を次に示します。

**d:BaseUri** -
Marketplace に公開されている REST リソースの URI テンプレートです。 Marketplace では、テンプレートを使用して、REST Web サービスに対するクエリを作成します。 URI テンプレートには、パラメーターのプレースホルダーが {parameterName} の形式で含まれています。parameterName はパラメーターの名前です。 例: apiVersion = {apiVersion} します。
パラメーターは、URI パラメーターとして、または URI パスの一部として表記できます。 パス内に表記する場合は、常に必須になります (null 許容型としてマークすることはできません)。 *例:* `d:BaseUri="http://api.MyWeb.com/Site/{url}/v1/visits?start={start}&amp;end={end}&amp;ApiKey=3fadcaa&amp;Format=XML"`

**名前** -インポートされた関数の名前。  CSDL で定義されている他の名前と同じ名前にすることはできません。  例: Name="GetModelUsageFile"

**EntitySet** *(省略可能)* - 関数は、エンティティ型の値のコレクションを返す場合、 **EntitySet** 設定する必要があるエンティティのコレクションが属しています。 それ以外の場合、 **EntitySet** 属性を使用しないでください。 *例:* `EntitySet="GetUsageStatisticsEntitySet"`

**ReturnType** *(省略可能)* -URI によって返される要素の種類を指定します。  関数が値を返さない場合は、この属性を使用しないでください。 次の型がサポートされています。

 - **コレクション (<Entity type name>)**: 定義済みのエンティティ型のコレクションを指定します。 名前は、EntityType ノードの Name 属性内にあります。 たとえば、Collection(WXC.HourlyResult) のようになります。
 - **生 (<mime type>)**: 生のドキュメントや blob をユーザーに返されるを指定します。 たとえば、Raw(image/jpeg) のようになります。その他の例:

  - ReturnType="Raw(text/plain)"
  - ReturnType="Collection(sage.DeleteAllUsageFilesEntity)"*

**d:Paging** -REST リソースでのページングの処理方法を指定します。 パラメーター値は中かっこで囲みます。例: page={$page}&itemsperpage={$size}。使用できるオプションは次のとおりです。

- **None:** ページングが利用できません。
- **Skip:** ページングは、論理"skip"および"take"(上) を使って表されます。 m 個の要素をスキップした後、次の n 個の要素を返します。 パラメーター値: $skip
- **Take:** Take は、次の N 要素を返します。 パラメーター値: $take
- **PageSize:** ページングは、論理ページとサイズ (1 ページあたりの項目) を使って表されます。 Page は返される現在のページを表します。 パラメーター値: $page
- **サイズ:** サイズ ページごとに返される項目の数を表します。 パラメーター値: $size

**d:AllowedHttpMethods** *(省略可能)* -REST リソースによって処理する動詞を指定します。 また、受け入れられる動詞を、指定した値のみに制限します。  既定値 = POST  *例:* `d:AllowedHttpMethods="GET"` はオプション。

- **GET:** データを返すために通常使用
- **投稿:** 通常、新しいデータの挿入に使用
- **PUT:** 通常のデータを更新するために使用
- **[削除]:** データを削除するために使用

FunctionImport ノード内の追加の (CSDL ドキュメントに記載されていない) 子ノードは次のとおりです。

**d:RequestBody** *(省略可能)* -要求本文を使用して、要求が送信される本文を期待しているかを指定します。 パラメーターは、要求本文内で指定できます。 パラメーターは中かっこで囲みます。たとえば、{parameterName} のようになります。 これらのパラメーターは、コンテンツ プロバイダーのサービスに転送される本文へのユーザー入力からマップされます。 requestBody 要素には、httpMethod という名前の属性があります。 この属性では、2 つの値を使用できます。

- **投稿:** 要求が HTTP POST である場合に使用
- **GET:** 場合、要求は HTTP GET を使用します。

    例:

        `<d:RequestBody d:httpMethod="POST">
        <![CDATA[
        <req1:Request xmlns:r1="http://schemas.mysite.com//generic/requests/1" Version="1.0">
        <req1:Query>{Query}</req1:Query><req1:AppId>D453474</req1:AppId>
        <req:DestinationSchemas><req1:Schema>Generic.RequestResponse[1.0]</req1:Schema>
        </req1:DestinationSchemas>
        </req1: Request>
        ]]>
        </d:RequestBody>`

**d:Namespaces** と **d:Namespace** -このノードは、関数インポート (URI エンドポイント) によって返される XML で定義されている名前空間を示します。 バックエンド サービスによって返される XML には、返すコンテンツを区別するために、任意の数の名前空間が含まれる場合があります。 **すべてのこれら名前空間、d:Map または d:Match の XPath クエリで使用されている場合は、リストに表示される必要があります。**d:Namespaces ノードには、d:Namespace ノードのセット/一覧が含まれています。 ノードごとに、バックエンド サービスの応答で使用される名前空間が 1 つ記述されます。 d:Namespace ノードの属性を次に示します。

-   **d:Prefix:** 、XML の結果、サービスによって返される例: f:FirstName、ここで、f は、プレフィックス f:LastName に示すように、名前空間のプレフィックス。
- **d:Uri:** 結果ドキュメントで使用される名前空間の完全な URI です。 プレフィックスが実行時に解決される値を表します。

**d:ErrorHandling** *(省略可能)* -このノードには、エラー処理のための条件が含まれています。 各条件が、コンテンツ プロバイダーのサービスによって返される結果に対して検証されます。 提示された HTTP エラー コードに条件が一致する場合、エンド ユーザーにエラー メッセージが返されます。

**d:ErrorHandling** *(省略可能)* と **d:Condition** *(省略可能)* -条件ノードは、コンテンツ プロバイダーのサービスによって返される結果にチェックインされている 1 つの条件を保持します。 次のとおり、 **必要** 属性。

- **d:Match:** 特定のノードと値が、コンテンツ プロバイダーに存在するかどうかが検証される XPath 式の XML を出力します。 XPath は出力に対して実行され、条件が一致する場合は true を返し、一致しない場合は false を返します。
- **d:HttpStatusCode:** によって返される Marketplace の場合は、条件、HTTP ステータス コードと一致します。 Marketplace は、HTTP 状態コードを通じてユーザーにエラーを伝えます。 HTTP 状態コードの一覧については、「http://en.wikipedia.org/wiki/HTTP_status_code です。
- **d:ErrorMessage:** と共に返される – HTTP 状態コード: エンドユーザーにエラー メッセージ。 これは、機密情報が含まれていない、わかりやすいエラー メッセージです。

**d:Title** *(省略可能)* -関数のタイトルを表現できます。 タイトルの値は次のものから取得されます。

- サービス要求から返される応答内のタイトルを検索する場所を指定する省略可能なマップ属性 (XPath)。
- ノードの値として指定されたタイトル。

**d:Rights** *(省略可能)* -関数に関連付けられている権限 (例: 著作権)。 権限の値は次のものから取得されます。

- サービス要求から返される応答内の権限を検索する場所を指定する省略可能なマップ属性 (XPath)。
-   ノードの値として指定された権限。

**d:Description** *(省略可能)* - 関数の説明を短きます。 説明の値は次のものから取得されます。

- サービス要求から返される応答内の説明を検索する場所を指定する省略可能なマップ属性 (XPath)。
- ノードの値として指定された説明。

**d:EmitSelfLink** - *上の例「返されるデータを使用する 'ページング' の FunctionImport」を参照してください*

**d:EncodeParameterValue** -OData 拡張のオプション

**d:QueryResourceCost** -OData 拡張のオプション

**d:Map** -OData 拡張のオプション

**d:Headers** -OData 拡張のオプション

**d:Headers** -OData 拡張のオプション

**d:Value** -OData 拡張のオプション

**d:HttpStatusCode** -OData 拡張のオプション

**d:ErrorMessage** -OData 拡張のオプション

## Parameter ノード

このノードは、FunctionImport ノードで指定された URI テンプレート/要求本文の一部として公開されている 1 つのパラメーターを表します。

「パラメーター要素」のノードに関する非常に役立つ詳細情報のドキュメント ページにある [http://msdn.microsoft.com/library/ee473431.aspx](http://msdn.microsoft.com/library/ee473431.aspx)  (を使用して、 **その他のバージョン** 、ドキュメントを表示するために必要な場合に、別のバージョンを選択するドロップダウン)。 *例:* `<Parameter Name="Query" Nullable="false" Mode="In" Type="String" d:Description="Query" d:SampleValues="Rudy Duck" d:EncodeParameterValue="true" MaxLength="255" FixedLength="false" Unicode="false" annotation:StoreGeneratedPattern="Identity"/>`

| パラメーター属性 | 必須 | 値 |
|----|----|----|
| 名前 | あり | パラメーターの名前。 大文字と小文字が区別されます。  BaseUri の大文字小文字と照合されます。 **例:** `<Property Name="IsDormant" Type="Byte" />` |
| 型 | あり | パラメーターの型。 値がある必要があります、 **EDMSimpleType** またはモデルのスコープ内にある複合型です。 詳細については、6 つ目のトピック「サポートされているパラメーター/プロパティの型」を参照してください。  (大文字と小文字が区別されます。 先頭の文字は大文字で、残りは小文字です。)参照してください  [http://msdn.microsoft.com/library/bb399548(v=VS.100).aspx][MSDNParameterLink]. **例:** `<Property Name="LimitedPartnershipID " Type="Int32" />` |
| Mode | いいえ | ****, 、Out、または InOut パラメーターは、入力、出力、または入力/出力パラメーターかどうかによって異なります。 (Azure Marketplace で使用できるのは "In" のみです)。**例:** `<Parameter Name="StudentID" Mode="In" Type="Int32" />` |
| MaxLength | いいえ | パラメーターの許容される最大長 **例:** `<Property Name="URI" Type="String" MaxLength="100" FixedLength="false" Unicode="false" />` |
| Precision | いいえ | パラメーターの説明 **例:** `<Property Name="PreviousDate" Type="DateTime" Precision="0" />` |
| スケール | いいえ | パラメーターの小数点以下桁数 **例:** `<Property Name="SICCode" Type="Decimal" Precision="10" Scale="0" />` |

[MSDNParameterLink]:'http://msdn.microsoft.com/library/bb399548(v=VS.100).aspx'

CSDL の仕様に追加された属性を次に示します。

| パラメーター属性 | 説明 |
|----|----|
| **d:Regex** *(省略可能)* | パラメーターの入力値を検証するために使用する正規表現の式。 入力値が式と一致しない場合、値は拒否されます。 これを利用して、入力可能な値のセットとして、たとえば ^[0-9]+?$ を指定して数字のみに制限することもできます。 **例:** ' < パラメーター名 = モードを"name"="In"の種類 =「文字列」d: null 許容 ="false"d:Regex ="^ [- A-ZA-Z] * $"d:Description d:SampleValues"にスペースや英数字以外の英語以外の文字を含むことができない名前"= ="George|John|Thomas|James"/>' |
| **d:Enum** *(省略可能)* | パラメーターに有効な値のパイプ区切り一覧。 値の型は、パラメーターの定義済みの型に一致する必要があります。 例: ' 英語|メトリック|生`. Enum will display as a selectable dropdown list of parameters in the UI (service explorer). **Example:** `< パラメーター名「所要時間」の種類の = ="String"モード"In"Nullable = ="true"d:Enum ="の 1 年間|勤続 5 年|10years"/>' |
| **d: null 許容** *(省略可能)* | パラメーターを null に指定できるかどうかを定義できます。 既定値は true です。 ただし、URI テンプレートでパスの一部として公開されているパラメーターを null に指定することはできません。 これらのパラメーターでこの属性が false に設定されている場合、ユーザーの入力は上書きされます。 パラメーターを null に指定できるかどうかを定義できます。 既定値は true です。 ただし、URI テンプレートでパスの一部として公開されているパラメーターを null に指定することはできません。 これらのパラメーターでこの属性が false に設定されている場合、ユーザーの入力は上書きされます。 **例:** `<Parameter Name="BikeType" Type="String" Mode="In" Nullable="false"/>` |
| **d:SampleValue** *(省略可能)* | UI でクライアントにメモとして表示するサンプル値。  複数の値をパイプで区切って追加できます。|」をご覧ください。 **例:** ' < パラメーター名"BikeOwner"型の = ="String"モード「で」d:SampleValues = ="George|John|Thomas|James"/>' |
| **d:UriTemplate** | |

## EntityType ノード

このノードは、Marketplace からエンド ユーザーに返される型の 1 つを表します。 また、コンテンツ プロバイダーのサービスによって返される出力とエンド ユーザーに返される値のマッピングも含まれています。

詳細については、このノードがある [http://msdn.microsoft.com/library/bb399206.aspx](http://msdn.microsoft.com/library/bb399206.aspx) (を使用して、 **その他のバージョン** 、ドキュメントを表示するために必要な場合に、別のバージョンを選択するドロップダウンです)。

| 属性名 | 必須 | 値 |
|----|----|----|
| 名前 | あり | エンティティ型の名前。 **例:** `<EntityType Name="ListOfAllEntities" d:Map="//EntityModel">` |
| BaseType | いいえ | 定義するエンティティ型の基本の型である別のエンティティ型の名前。 **例:** `<EntityType Name="PhoneRecord" BaseType="dqs:RequestRecord">` |

CSDL の仕様に追加された属性を次に示します。

**d:Map** -サービスの出力に対して実行される XPath 式。 ここでは、繰り返し処理されるエントリ ノードのセットが存在する ATOM フィードのように、サービスの出力に、繰り返し処理される要素のセットが含まれていることを前提にしています。 各繰り返しノードには 1 つのレコードが格納されます。 XPath は、各レコードの値を保持しているコンテンツ プロバイダーのサービス結果内の繰り返しノードをポイントするように指定されます。 サービスの出力例:

        `<foo>
          <bar> … content … </bar>
          <bar> … content … </bar>
          <bar> … content … </bar>
        </foo>`

各 bar ノードが出力内で繰り返しノードになっており、エンド ユーザーに返される実際のコンテンツが含まれているため、XPath 式は /foo/bar のようになります。

**キー** -Marketplace でこの属性は無視されます。 一般に、REST ベースの Web サービスでは主キーは公開されません。


## Property ノード

このノードには、レコードのプロパティの 1 つが含まれています。

詳細については、このノードがある [http://msdn.microsoft.com/library/bb399546.aspx](http://msdn.microsoft.com/library/bb399546.aspx) (を使用して、 **その他のバージョン** 、ドキュメントを表示するために必要な場合に、別のバージョンを選択するドロップダウンです)。*例:*
        `<EntityType Name="MetaDataEntityType" d:Map="/MyXMLPath">
        <Property Name="Name"   Type="String" Nullable="true" d:Map="./Service/Name" d:IsPrimaryKey="true" DefaultValue=”Joe Doh” MaxLength="25" FixedLength="true" />
        ...
        </EntityType>`

| AttributeName | 必須 | 値 |
|----|----|----|
| 名前 | あり | プロパティの名前。 |
| 型 | あり | プロパティ値の型。 プロパティ値の型である必要があります、 **EDMSimpleType** またはモデルのスコープ内にある複合型 (完全修飾名で示されます)。 詳細については、「概念モデルの型 (CSDL)」を参照してください。 |
| Nullable | いいえ | **True** (既定値) または **False** プロパティが null 値を持っているかどうかによってです。 注: で示された CSDL のバージョンで、 [http://schemas.microsoft.com/ado/2006/04/edm](http://schemas.microsoft.com/ado/2006/04/edm) 名前空間、複合型プロパティの null 値である必要があります ="False"です。 |
| DefaultValue | いいえ | プロパティの既定値。 |
|MaxLength | いいえ | プロパティ値の最大長。 |
| FixedLength | いいえ | **True** または **False** fiexed 長の文字列としてプロパティの値を格納するかどうかによって異なります。 |
| Precision | いいえ | 数値に保持する最大桁数を表します。 |
| スケール | いいえ | 数値に保持する小数点以下の最大桁数を表します。 |
| Unicode | いいえ | **True** または **False** プロパティ値であるかどうかに応じて Unicode 文字列として格納します。 |
| Collation | いいえ | データ ソースで使用される照合順序を指定する文字列。 |
| ConcurrencyMode | いいえ | **[なし]** (既定値) または **固定**します。 値が設定されている場合 **固定**, 、オプティミスティック同時実行チェックでプロパティ値が使用されます。 |

CSDL の仕様に追加されたその他の属性を次に示します。

**d:Map** -サービスに対して実行する XPath 式を出力し、出力の 1 つのプロパティを抽出します。 指定された XPath は、EntityType ノードの XPath で選択された繰り返しノードへの相対パスになります。 また、絶対 XPath を指定して、各出力ノードに静的リソースを含めることもできます。たとえば、元のサービス出力に 1 回だけ表記されている著作権情報を、OData 出力の各行に表示することができます。 サービスの例:

        `<foo>
          <bar>
           <baz0>… value …</baz0>
           <baz1>… value …</baz1>
           <baz2>… value …</baz2>
          </bar>
        </foo>`

この例でコンテンツ プロバイダーのサービスから baz0 ノードを取得する XPath 式は ./bar/baz0 になります。

**d:CharMaxLength** -文字列型の最大長を指定することができます。 「DataService CSDL の例」を参照してください。

**d:IsPrimaryKey** の列がテーブルまたはビューの主キーであることを示します。 「DataService CSDL の例」を参照してください。

**d:isExposed** -テーブルのスキーマが公開されているかどうかを判断 (一般に true)。 「DataService CSDL の例」を参照してください。

**d:IsView** *(省略可能)* - true の場合、このテーブルではなく、ビューに基づいています。  「DataService CSDL の例」を参照してください。

**d:Tableschema** -DataService CSDL の例を参照してください。

**d:ColumnName** -テーブル/ビューの列の名前を指定します。  「DataService CSDL の例」を参照してください。

**d:IsReturned** -は、サービスがクライアントには、この値を公開するかどうかを決定するブール値。  「DataService CSDL の例」を参照してください。

**d:IsQueryable** -は、列をデータベース クエリで使用できるかどうかを決定するブール値。   「DataService CSDL の例」を参照してください。

**d:OrdinalPosition** -は、列の位置を表す数値 x、テーブルまたはビューの外観の x は 1 からテーブルの列の数にします。  「DataService CSDL の例」を参照してください。

**d:DatabaseDataType** -データベース、つまり SQL データ型の列のデータ型です。 「DataService CSDL の例」を参照してください。

## サポートされているパラメーター/プロパティの型
パラメーターおよびプロパティのサポートされている型を次に示します  (大文字と小文字が区別されます)。

| プリミティブ型 | 説明 |
|----|----|
| Null | 値がないことを表します。 |
| Boolean | 2 値論理という数学的概念を表します。|
| Byte | 8 ビットの符号なし整数値。|
|DateTime| 西暦 1753 年 1 月 1 日 12時 00分: 00 深夜にまでの値で日付と時刻を表します 午後 11時 59分: 59、~ 西暦 9999 年 12 月|
|小数点 | 固定の有効桁数および小数点以下桁数を指定して数値を表します。 この型は、負の 10^255 + 1 から正の 10^255 -1 の間の数値を記述できます。|
| Double | およそ ± 2.23e -308 から ± 1.79e +308 の範囲の値を表すことができる有効桁数 15 桁の浮動小数点数を表します。 **Exel エクスポートの問題のための 10 進数を使用します。**|
| Single | およそ ± 1.18e -38 から ± 3.40e +38 の範囲の値を表すことができる有効桁数 7 桁の浮動小数点数を表します。|
|Guid |16 バイト (128 ビット) の一意の識別子値を表します。 |
|Int16|16 ビットの符号付き整数値を表します。 |
|Int32|32 ビットの符号付き整数値を表します。 |
|Int64|64 ビットの符号付き整数値を表します。 |
|String | 固長定または可変長の文字データを表します。 |


## 関連項目
- 全体的な OData マッピング プロセスと目的を理解する場合は、この記事を読む [データ サービスの OData マッピング](marketplace-publishing-data-service-creation-odata-mapping.md) 定義、構造、および指示を確認します。
- この記事を参照している例を確認したい場合は、 [データ サービスの OData マッピング例](marketplace-publishing-data-service-creation-odata-mapping-examples.md) コードの構文とコンテキストを理解しているサンプル コードを参照してください。
- この記事を参照データ サービスを Azure Marketplace に発行するための指定されたパスに戻り、 [データ サービスの発行ガイド](marketplace-publishing-data-service-creation.md)します。

