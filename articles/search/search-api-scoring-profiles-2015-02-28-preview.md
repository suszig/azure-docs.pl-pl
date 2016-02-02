<properties
    pageTitle="スコアリング プロファイル (Azure Search REST API バージョン 2015-02-28-Preview) | Microsoft Azure | ホスト型クラウド検索サービス"
    description="Azure Search は、ユーザーが定義したスコアリング プロファイルに基づき、ランク付けされた結果を調整するホスト型のクラウド検索サービスです。"
    services="search"
    documentationCenter=""
    authors="HeidiSteen"
    manager="mblythe"
    editor=""/>

<tags
    ms.service="search"
    ms.devlang="rest-api"
    ms.workload="search"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.author="heidist"
    ms.date="11/04/2015" />


# スコアリング プロファイル (Azure Search REST API バージョン 2015-02-28-Preview)

> [AZURE.NOTE] スコアリング プロファイルの説明、 [2015年-02-28-preview](search-api-2015-02-28-preview.md)します。 現在の違いはありません、 `2015年-02-28` で [MSDN](http://msdn.microsoft.com/library/azure/mt183328.aspx) と `2015年-02-28-preview` バージョンがここで説明します。

## 概要

スコアリングとは、検索結果に返されるすべての項目の検索スコアを計算することです。 スコアは、現在の検索操作のコンテキストにおける項目の関連性を示すインジケーターです。 スコアが高いほど、項目の関連性が高くなります。 検索結果内の項目は、項目ごとに計算された検索スコアに基づいて、順位の高い順に並べられます。

Azure Search では、既定のスコアリングを使用して初期スコアを計算しますが、スコアリング プロファイルを介して計算をカスタマイズすることができます。 スコアリング プロファイルでは、検索結果の項目の順位付けをより細かく制御できます。 たとえば、収益の可能性に基づいて項目をブーストしたり、より新しい項目を昇格させたり、場合によっては在庫期間が長すぎる項目をブーストしたりできます。

スコアリング プロファイルは、インデックス定義の一部であり、フィールド、関数、およびパラメーターで構成されます。

スコアリング プロファイルとはどのようなものかを説明するために、次の例に 'geo' という名前の簡単なプロファイルを示します。 内の検索語句を含む項目をブーストこの、 `hotelName` フィールドです。 使用して、 `距離` 関数は、現在の場所から 10 キロメートル内にある項目を優先します。 他のユーザーが語句 'inn' の検索を実行し、'inn' がたまたまホテル名の一部であった場合、'inn' を含むホテルが記載されているドキュメントが検索結果で上位に表示されます。

    "scoringProfiles": [
      {
        "name": "geo",
        "text": {
          "weights": { "hotelName": 5 }
        },
        "functions": [
          {
            "type": "distance",
            "boost": 5,
            "fieldName": "location",
            "interpolation": "logarithmic",
            "distance": {
              "referencePointParameter": "currentLocation",
              "boostingDistance": 10
            }
          }
        ]
      }
    ]

このスコアリング プロファイルを使用するには、クエリを作成して、クエリ文字列にプロファイルを指定します。 次のクエリでクエリ パラメーターに注意してください。 `scoringProfile = geo` 要求にします。

    GET /indexes/hotels/docs?search=inn&scoringProfile=geo&scoringParameter=currentLocation:-122.123,44.77233&api-version=2015-02-28-Preview

このクエリは、語句 'inn' を検索し、現在の場所を渡します。 このクエリがなどその他のパラメーターを含むメモ `scoringParameter`します。 クエリ パラメーターについては、「 [ドキュメントの検索 (Azure Search API)](search-api-2015-02-28-preview/#SearchDocs.md)します。

クリックして [例](#example) スコアリング プロファイルの詳細な例を確認します。

## 既定のスコアリングとは

スコアリングでは、順位に従って並べられる結果セット内の各項目の検索スコアを計算します。 検索結果セット内のすべての項目にそれぞれ検索スコアが割り当てられ、最上位から最下位まで順位が付けられます。 アプリケーションには、より高いスコアを持つ項目が返されます。 既定では、上位 50 個が返された場合、使用することが、 `$top` パラメーター (最大 1 回の応答で 1000 個) の項目のより小さいか大きい数を取得します。

既定では、検索スコアは、データとクエリの統計プロパティに基づいて計算されます。 Azure Search はクエリ文字列内の検索語句を含むドキュメントを検索 (に応じて一部またはすべてを `searchMode`)、検索語句の多数のインスタンスを含むドキュメントを優先します。 データ コーパス全体での語句の出現頻度は低いがドキュメント内でよく使用されている場合、検索スコアはより高くなります。 関連性を計算するこのアプローチの基礎となる手法は、TF-IDF (単語の出現頻度 - 逆文書頻度) と呼ばれています。

カスタムの並べ替えが行われないと仮定した場合、結果は検索スコアによって順位付けされてから、呼び出し元のアプリケーションに返されます。 場合 `$top` スコアが返される、最も高い検索を持つ 50 個の項目を指定しません。

検索スコアの値は、結果セット全体で繰り返すことができます。 たとえば、スコアが 1.2 である項目が 10 個、スコアが 1.0 である項目が 20 個、スコアが 0.5 である項目が 20 個あるとします。 同じ検索スコアを持つ項目が複数ヒットした場合、同じスコアを持つ項目の順序付けは定義されていないので安定しません。 もう一度クエリを実行すると、項目の位置が変わる可能性があります。 同一スコアの項目が 2 つ存在する場合、最初に表示される項目を特定することはできません。

## カスタム スコアリングを使用する場合

既定の順位付けの動作ではビジネス目標を達成するに至らない場合、1 つ以上のスコアリング プロファイルを作成する必要があります。 たとえば、検索の関連性で、新しく追加された項目を優先するとします。 同様に、利益率を含んでいるフィールド、または他に収益の可能性を示す何らかのフィールドがあるとします。 ビジネスに利点をもたらすブースティング ヒットは、スコアリング プロファイルの使用を決定する上で重要な要因になる場合があります。

関連性に基づく順序付けも、スコアリング プロファイルを通して実装されます。 価格、日付、評価、または関連性での並べ替えが可能な、以前に使用した検索結果ページを検討してしてください。 Azure Search では、スコアリング プロファイルによって「関連性」オプションが開始されます。 関連性の定義は、ビジネス目標および提供する検索機能の種類に基づいて、ユーザーによって制御されます。

<a name="example"></a>
## 例

前述のように、カスタマイズされたスコアリングは、インデックス スキーマに定義されたスコアリング プロファイルを介して実装されます。

この例は、2 つのスコア付けプロファイルを持つインデックスのスキーマを示します (`boostGenre`, 、`newAndHighlyRated`)。 いずれかのプロファイルをクエリ パラメーターとして指定したクエリを、このインデックスに対して実行すると、該当するプロファイルを使用して結果セットのスコアリングが行われます。

[Try this example](search-get-started-scoring-profiles.md).

    {
      "name": "musicstoreindex",
      "fields": [
        { "name": "key", "type": "Edm.String", "key": true },
        { "name": "albumTitle", "type": "Edm.String" },
        { "name": "albumUrl", "type": "Edm.String", "filterable": false },
        { "name": "genre", "type": "Edm.String" },
        { "name": "genreDescription", "type": "Edm.String", "filterable": false },
        { "name": "artistName", "type": "Edm.String" },
        { "name": "orderableOnline", "type": "Edm.Boolean" },
        { "name": "rating", "type": "Edm.Int32" },
        { "name": "tags", "type": "Collection(Edm.String)" },
        { "name": "price", "type": "Edm.Double", "filterable": false },
        { "name": "margin", "type": "Edm.Int32", "retrievable": false },
        { "name": "inventory", "type": "Edm.Int32" },
        { "name": "lastUpdated", "type": "Edm.DateTimeOffset" }
      ],
      "scoringProfiles": [
        {
          "name": "boostGenre",
          "text": {
            "weights": {
              "albumTitle": 1,
              "genre": 5 ,
              "artistName": 2
            }
          }
        },
        {
          "name": "newAndHighlyRated",
          "functions": [
            {
              "type": "freshness",
              "fieldName": "lastUpdated",
              "boost": 10,
              "interpolation": "quadratic",
              "freshness": {
                "boostingDuration": "P365D"
              }
            },
            {
              "type": "magnitude",
              "fieldName": "rating",
              "boost": 10,
              "interpolation": "linear",
              "magnitude": {
                "boostingRangeStart": 1,
                "boostingRangeEnd": 5,
                "constantBoostBeyondRange": false
              }
            }
          ]
        }
      ],
      "suggesters": [
        {
          "name": "sg",
          "searchMode": "analyzingInfixMatching",
          "sourceFields": ["albumTitle", "artistName"]
        }
      ]
    }

## ワークフロー

カスタムのスコアリング動作を実装するには、インデックスを定義するスキーマにスコアリング プロファイルを追加します。 インデックスには複数のスコアリング プロファイルを含めることができますが、クエリには一度に 1 つのプロファイルしか指定できません。

始まる、 [テンプレート](#bkmk_template) ここで説明します。

名前を指定します。 スコアリング プロファイルは省略できますが、追加する場合は、名前が必要です フィールドの名前付け規則に従ってください (文字で始め、特殊文字や予約語の使用は避ける)。 参照してください [名前付け規則](http://msdn.microsoft.com/library/azure/dn857353.aspx) の詳細。

スコアリング プロファイルの本文は、重み付けされたフィールドと関数によって構成されます。

<table>
<thead>
<tr><td><b>要素</b></td><td><b>説明</b></td></tr></thead>
  <tbody>
    <tr>
      <td>
        <b>Weights</b>
      </td>
      <td>
        相対的な重みをフィールドに割り当てる名前と値のペアを指定します。 [例](#example), 、albumTitle、genre、および artistName のフィールドはブースト 1、5、および null の場合、それぞれします。なぜ genre は他のものよりも大幅に高くブーストされるのでしょうか? ある程度同じようなデータに対して検索を実行するかどうか ('genre' の場合は、 `musicstoreindex`)、相対的な重みをより分散する必要があります。たとえば、 `musicstoreindex`, 、両方はジャンルとしておよびされるジャンルの説明に「ロック」が表示されます。ジャンルがジャンルの説明を上回るようにする場合は、genre フィールドの相対的な重みをより高くする必要があります。
      </td>
    </tr>
    <tr>
      <td>
        <b>関数</b>
      </td>
      <td>
        特定のコンテキストに対して追加の計算が必要な場合に使用されます。有効な値は `freshness`, 、`マグニチュード`, 、`距離` と `タグ`します。各関数には、固有のパラメーターがあります。
        <p>
          - `freshness` ブーストする場合に使用する必要がありますの項目の新しさまたは古によってです。この関数は、日時フィールドでのみ使用できます (Edm.DataTimeOffset)。注、 `boostingDuration` 属性は freshness 関数でのみ使用します。
          </p><p>
            - `マグニチュード` 高に基づいてブーストするまたは、数値が低いときに使用する必要があります。この関数を呼び出すシナリオとしては、利益率、最高価格、最低価格、またはダウンロード回数に基づくブーストがあります。この関数は、倍精度浮動小数点フィールドと整数フィールドでのみ使用できます。
          </p><p>
             `マグニチュード` 関数で、逆のパターン (たとえば、低価格の項目をブーストよりも高価格の項目) にする場合、範囲を高から低を反転ことができます。範囲の価格 $100 から $ 1 を設定して `boostingRangeStart` を 100 と `boostingRangeEnd` を 1 に、価格がより低い項目をブーストします。
          </p><p>
            - `距離` 近接性または地理的場所に基づいてブーストする場合に使用する必要があります。この関数でのみ使用できます `Edm.GeographyPoint` フィールドです。
          </p><p>
            - `タグ` ドキュメントと検索クエリの間で共通のタグによってブーストする場合に使用する必要があります。この関数でのみ使用できます `Edm.String` と '(Collection(Edm.String) fields.
          </p>
             <p><b>関数の使用に関する規則</b>
            </p>
            関数型 (freshness、magnitude、distance、tag) は小文字にする必要があります。
            <br />
            関数に null または空の値を含めることはできません。具体的には、フィールド名を含めた場合、それを何かに設定する必要があります。
            <br />
             関数はフィルターの適用が可能なフィールドにのみ適用できます。参照してください [Create Index](search-api-2015-02-28/#createindex) 詳細については、可能なフィールドです。
             <br />
             関数は、インデックスのフィールド コレクションで定義されているフィールドにのみ適用できます。
         </td>
</tr>
  </tbody>
</table>

インデックスが定義されたら、インデックス スキーマをアップロードし、ドキュメントをアップロードして、インデックスを構築します。 参照してください [Create Index](search-api-2015-02-28-preview/#createindex) と [更新プログラムのドキュメントの追加または](search-api-2015-02-28-preview/#AddOrUpdateDocuments) 手順については、これらの操作です。 インデックスが構築されると、検索データと連携する機能的なスコアリング プロファイルが使用できるようになります。

<a name="bkmk_template"></a>
## テンプレート

このセクションでは、スコアリング プロファイルの構文とテンプレートを示します。 参照してください [属性参照のインデックス](#bkmk_indexref) 属性の説明については、次のセクションにします。

    ...
    "scoringProfiles": [
      {
        "name": "name of scoring profile",
        "text": (optional, only applies to searchable fields) {
          "weights": {
            "searchable_field_name": relative_weight_value (positive #'s),
            ...
          }
        },
        "functions": (optional) [
          {
            "type": "magnitude | freshness | distance | tag",
            "boost": # (positive number used as multiplier for raw score != 1),
            "fieldName": "...",
            "interpolation": "constant | linear (default) | quadratic | logarithmic",
    
            "magnitude": {
              "boostingRangeStart": #,
              "boostingRangeEnd": #,
              "constantBoostBeyondRange": true | false (default)
            }
    
            // (- or -)
    
            "freshness": {
              "boostingDuration": "..." (value representing timespan over which boosting occurs)
            }
    
            // (- or -)
    
            "distance": {
              "referencePointParameter": "...", (parameter to be passed in queries to use as reference location)
              "boostingDistance": # (the distance in kilometers from the reference location where the boosting range ends)
            }
    
            // (- or -)
    
            "tag": {
              "tagsParameter": "..." (parameter to be passed in queries to specify list of tags to compare against target field)
            }
          }
        ],
        "functionAggregation": (optional, applies only when functions are specified)
            "sum (default) | average | minimum | maximum | firstMatching"
      }
    ],
    "defaultScoringProfile": (optional) "...",
    ...

<a name="bkmk_indexref"></a>
## インデックスの属性のリファレンス

**注**
スコアリング関数は、フィルター フィールドにのみ適用できます。

<table border="1">
<tr>
<th>属性</th>
<th>説明</th>
</tr>
<tr>
<td>名前</td>   <td>が必要です。   これは、スコアリング プロファイルの名前です。これは、フィールドの場合と同じ名前付け規則に従います。先頭を英字にする必要があります、ドット、コロンを含めることはできませんまたは @ 記号、'azureSearch' (大文字小文字を区別) という句で始めることもできません。 </td>
</tr><tr>
<td>テキスト</td>   <td>重みプロパティが含まれています。</td>
</tr><tr>
<td>重み</td>    <td>省略可能です。    フィールドの名前と相対的な重みを指定する名前と値のペア。相対的な重みは正の整数にする必要があります。最大値は int32.MaxValue です。対応する重みなしでフィールド名を指定することができます。重みを使用して、フィールドの別の相対的な重要度を指定します。</td>
<tr>
<td>関数</td>  <td>省略可能です。  注: スコアリング関数はフィルター フィールドにのみ適用できます。</td>
</tr><tr>
<td>型</td>   <td>スコアリング関数の場合に必要です。   使用する関数の型を示します。有効な値には、magnitude、freshness、distance および tag があります。各スコアリング プロファイルには複数の関数を含めることができます。関数名は、小文字にする必要があります。</td>
</tr><tr>
<td>Boost</td>  <td>スコアリング関数の場合に必要です。  生のスコアの乗数として使用される正の数値。これは、1 に等しいことができません。</td>
</tr><tr>
<td>Fieldname</td>  <td>スコアリング関数の場合に必要です。  スコアリング関数は、インデックスのフィールド コレクションに属し、フィルターの適用が可能なフィールドにのみ適用できます。さらに、各関数型には追加の制限 (freshness は日時フィールドで使用され、magnitude は整数または倍精度浮動小数点フィールドで使用され、distance は場所フィールドで使用され、tag は文字列または文字列コレクションで使用される) が導入されています。関数の定義ごとに指定できるフィールドは 1 つだけです。たとえば、2 回同じプロファイルで magnitude を使用する必要になりますフィールドごとに 1 つ、2 つの定義 magnitude を含めます。</td>
</tr><tr>
<td>補間</td>  <td>スコアリング関数の場合に必要です。  スコア ブースティングが範囲の始点から範囲の終点に向かって高くなる場合の傾斜を定義します。有効な値には、線形 (既定値)、定数、二次方程式、および対数があります。参照してください [補間設定](#bkmk_interpolation) 詳細です。</td>
</tr><tr>
<td>magnitude</td>  <td>、magnitude スコアリング関数は数値フィールドの値の範囲に基づいてランキングを変更するために使用します。  最も一般的な使用例には、次のようなものがあります。
<br>
- 星評価:"星の評価] フィールドの値に基づいて、スコアリングを変更します。 2 つの項目が該当する場合は、評価の高い方の項目が最初に表示されます。
<br>
- 余白: 2 つのドキュメントが該当する場合、小売業者することをまず販売利益を持つドキュメントをブーストできます。
<br>
- クリックして回数: 製品やページに対する操作を追跡するアプリケーションをクリックし、場合は、最大トラフィックを示す傾向にある項目をブーストする magnitude を使用する可能性があります。
<br>
- ダウンロード回数: アプリケーションのダウンロードを追跡、magnitude 関数によりをブーストするアイテムのある最もダウンロードします。
<tr>
<td>magnitude |boostingRangeStart</td> <td>magnitude をスコアリングする範囲の開始値を設定します。値は整数または倍精度浮動小数点にする必要があります。1 ～ 4 の星評価を対象とする場合、これは 1 になります。50% 以上の余白を 50 になります。</td>
</tr><tr>
<td>magnitude |boostingRangeEnd</td>   <td>magnitude をスコアリングする範囲の終了値を設定します。   値は整数または倍精度浮動小数点にする必要があります。1 ~ 4 の星評価を 4 になります。</td>
</tr><tr>
<td>magnitude |constantBoostBeyondRange</td>   <td>有効な値は、true または false (既定)。   true に設定すると、対象フィールドの値が範囲の上限を超えているドキュメントに、フル ブーストが継続的に適用されます。False の場合、この関数のブーストが範囲外になる、対象フィールドの値にあるドキュメントに適用されません。</td>
</tr><tr>
<td>freshness</td>  <td>DateTimeOffset フィールドの値に基づいて項目の順位付けのスコアを変更する、freshness スコアリング関数を使用します。  たとえば、最近の日付を持つ項目に、より古い日付を持つ項目よりも高い順位を付けることができます。(将来の日付を使用して、カレンダー イベントなどの項目に順位を付けることもできます。たとえば、現在に近い日付の項目を遠い日付の項目よりも高い順位にすることができます)現在のサービス リリースでは、範囲の一端が現在の時刻に固定されます。もう一方の端がに基づいて過去の時刻、 `boostingDuration`します。将来の時刻の範囲をブーストするには、負の数を使用して `boostingDuration`します。最大および最小の範囲からのブースティングの変化率は、スコアリング プロファイルに適用した補間によって決定されます (下図参照)。適用されるブースティング係数を反転するには、ブースト係数として 1 より小さい値を選択します。</td>
</tr><tr>
<td>freshness |boostingDuration</td>   <td>設定、特定のドキュメントの期限を過ぎるとブースティングは停止します。   参照してください [boostingDuration ][#bkmk_boostdur] 構文と例については、次のセクションにします。</td>
</tr><tr>
<td>距離</td>   <td>相対参照の地理的な場所には長短までに基づいてドキュメントのスコアに影響するまでの距離スコアリング関数を使用します。   参照の場所は、パラメーターにクエリの一部として指定した (を使用して、 `scoringParameterquery` オプションの文字列) に lon, lat 引数。</td>
</tr><tr>
<td>距離 |referencePointParameter</td> <td>場所。 scoringParameter を参照するために使用するクエリで渡されるパラメーターがクエリ パラメーターです。参照してください [ドキュメントの検索](search-api-2015-02-28-preview/#SearchDocs) のクエリ パラメーターの説明。</td>
</tr><tr>
<td>距離 |boostingDistance</td>    <td>ブースティング範囲の終了位置からの参照の場所からキロメートル単位で距離を表す数値。</td>
</tr><tr>
<td>タグ</td>    <td>ドキュメントと検索クエリ内のタグに基づいてドキュメントのスコアに影響を与える、tag スコアリング関数を使用します。    検索クエリと共通のタグを持つドキュメントがブーストされます。検索クエリのタグは、各検索要求でスコアリング パラメーターとして提供されます (を使用して、 `scoringParameterquery` 文字列オプション)。</td>
</tr><tr>
<td>タグ |tagsParameter</td>    <td>タグを特定の要求。 scoringParameter はクエリ パラメーターを指定するためにクエリに渡されるパラメーター。    参照してください [ドキュメントの検索](search-api-2015-02-28-preview/#SearchDocs) のクエリ パラメーターの説明。</td>
</tr><tr>
<td>functionAggregation</td>    <td>省略可能です。    関数を指定した場合にのみ適用されます。有効な値には、sum (default)、average、minimum、maximum、および firstMatching があります。検索スコアは、複数の変数 (複数の関数など) から計算される単一の値です。この属性は、すべての関数のブーストを、基本ドキュメント スコアに適用される 1 つの集計ブーストに集約する方法を示します。基本スコアは、ドキュメントと検索クエリから計算された、tf-idf 値に基づきます。</td>
</tr><tr>
<td>defaultScoringProfile</td>  <td>スコアリング プロファイルが指定されていない場合は、検索要求を実行する場合、既定のスコアリングで使用される (tf-idf のみ)。
既定のスコアリング プロファイル名は、Azure search は検索要求に特定のプロファイルが指定されていない場合は、そのプロファイルを使用してここでは、設定できます。 </td>
</tr>
</table>

<a name="bkmk_interpolation"></a>
## 補間の設定

補間を使用することにより、スコア ブースティングが範囲の始点から範囲の終点に向かって高くなる場合の傾斜を定義できます。 次の補間を使用できます。

- `線形`  一定の減少量では最大および最小の範囲内にあるアイテムのアイテムに適用されるブーストが行われます。 線形はスコアリング プロファイルの既定の補間です。

- `定数`    順位の結果には、開始と終了範囲内にある項目について、一定のブーストが適用されます。

- `二次`   ブーストが一定の割合が、線形補間と比較して二次方程式の場合は最初にペースで低下し、低下し、終了範囲に近づくとにつれてにします。 tag スコアリング関数では、この補間オプションは使用できません。

- `対数` ブーストが一定の割合が、線形補間と比較して対数の場合は最初にペースで低下し、低下し、終了範囲に近づくと速いにします。 tag スコアリング関数では、この補間オプションは使用できません。

<a name="Figure1"></a>
 ![][1]

<a name="bkmk_boostdur"></a>
## boostingDuration の設定

`boostingDuration` は freshness 関数の属性です。 これを使用して特定のドキュメントに対して有効期限を設定します。この期限が過ぎるとブースティングは停止します。 たとえば、10 日のプロモーション期間中に製品ラインまたはブランドをブーストするには、該当するドキュメントに対して 10 日の期間を "P10D" で指定します。 また、翌週に予定しているイベントをブーストするには、"-P7D" と指定します。

`boostingDuration` XSD"dayTimeDuration"値 (ISO 8601 期間値の制限されたサブセット) として書式設定する必要があります。 使用されるパターンは次のとおりです。

     [-]P[nD][T[nH][nM][nS]]

次の表に、いくつかの例を示します。

<table>
<thead>
<tr>
<td><b>時間</b></td> <td><b>boostingDuration</b></td>
</tr>
</thead>
<tbody>
<tr>
<td>1 日</td>  <td>"P1D"</td>
</tr><tr>
<td>2 日と 12 時間</td>    <td>"P2DT12H"</td>
</tr><tr>
<td>約 15 分</td> <td>"PT15M"</td>
</tr><tr>
<td>30 日 と 5 時間 10 分 6.334 秒</td>    <td>"P30DT5H10M6.334S"</td>
</tr>
</tbody>
</table>

例については、次を参照してください。 [XML スキーマ: データ型 (W3.org web サイト)](http://www.w3.org/TR/xmlschema11-2/)します。

**関連項目**
[Azure Search サービス REST API](http://msdn.microsoft.com/library/azure/dn798935.aspx) msdn <br/>
[(Azure Search API) のインデックスを作成](http://msdn.microsoft.com/library/azure/dn798941.aspx) msdn<br/>
[検索インデックスにスコア付けプロファイルを追加](http://msdn.microsoft.com/library/azure/dn798928.aspx) msdn<br/>



[1]: ./media/search-api-scoring-profiles-2015-02-28-Preview/scoring_interpolations.png 

