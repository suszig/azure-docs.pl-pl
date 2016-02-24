<properties 
    pageTitle="データセットの作成" 
    description="Azure Data Factory のデータセットを理解し、その作成方法について学習します。" 
    services="data-factory" 
    documentationCenter="" 
    authors="spelluru" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/12/2015" 
    ms.author="spelluru"/>

# データセット

## 説明
データセットはデータを論理的に説明するものです。 記述されるデータは、単純なバイト、CSV ファイルのような半構造化データから、リレーショナル テーブルやモデルまでさまざまです。 データにアクセスするためのメカニズム (アドレス、プロトコル、認証方式) はリンクされたサービスで定義され、データセット定義で参照されます。

## 構文

    {
        "name": "<name of dataset>",
        "properties":
        {
           "structure": [ ],
           "type": "<type of dataset>",
            "external": <boolean flag to indicate external data>,
            "typeProperties":
            {
            },
            "availability":
            {
    
            },
           "policy": 
            {      
    
            }
        }
    }

**構文の詳細**

| プロパティ | 説明 | 必須 | 既定値 |
| -------- | ----------- | -------- | ------- |
| name | データセットの名前 | あり | 該当なし |
| structure | <p>データセットのスキーマ</p><p>を参照してください [データセット構造](#Structure) 詳細については</p> | いいえ、できません。 | 該当なし |
| type | データセットの型 | あり | 該当なし |
| typeProperties | <p>選択した種類に対応するプロパティ</p><p>を参照してください [Dataset 型](#Type) サポートされる型とそのプロパティの詳細セクションです。</p> | あり | 該当なし |
| 外部 | データセットを Data Factory パイプラインにより明示的に生成するかどうかを指定するブール型のフラグ  | いいえ | false | 
| availability | <p>データセット生成の処理時間枠またはスライシング モデルを定義します。 </p><p>参照してください [データセットの可用性](#Availability) 詳細についてはトピック</p><p>を参照してください [スケジュールと実行](data-factory-scheduling-and-execution.md) データセット スライシング モデルの詳細については資料</p> | あり | 該当なし
| policy | データセット スライスで満たさなければならない基準または条件を定義します。 <p>参照してください [データセット ポリシー](#Policy) 詳細トピック</p> | いいえ | 該当なし |

### 例

という名前のテーブルを表すデータセットの例を次に示します **MyTable** で **Azure SQL データベース**します。 Azure SQL データベースの接続文字列が定義されている、 **AzureSqlLinkedService** このデータセットで参照されています。 このデータセットは毎日スライスされます。  

    {
        "name": "DatasetSample",
        "properties": {
            "type": "AzureSqlTable",
            "linkedServiceName": "AzureSqlLinkedService",
            "typeProperties": 
            {
                "tableName": "MyTable"
            },
            "availability": 
            {
                "frequency": "Day",
                "interval": 1
            }
        }
    }

## <a name="Structure"></a>データセットの構造体

 **構造** セクションはデータセットのスキーマをアサートします。 列の集合と列の種類が含まれます。 各列には次のプロパティが含まれます。

プロパティ | 説明 | 必須 | 既定値  
-------- | ----------- | -------- | -------
name | 列の名前です。 | いいえ | 該当なし 
type | 列のデータ型です。 | いいえ | 該当なし 

### 例

次の例では、データセットに slicetimestamp、projectname、pageviews の 3 つの列があります。

    structure:  
    [ 
        { "name": "slicetimestamp", "type": "String"},
        { "name": "projectname", "type": "String"},
        { "name": "pageviews", "type": "Decimal"}
    ]

## <a name="Type"></a> データセットの型

サポートされるデータ ソースとデータセットの型が並んでいます。 参照されているコネクタのトピックを参照して、 [データ移動アクティビティ](data-factory-data-movement-activities.md) 型およびデータセットの構成の情報の記事です。 

## <a name="Availability"></a> データセットの可用性

データセットの「可用性」セクションにより、データセット生成の処理時間枠またはスライシング モデルが定義されます。 参照してください [スケジュールと実行](data-factory-scheduling-and-execution.md) データセットのスライシングと依存モデルの詳細については資料です。 

| プロパティ | 説明 | 必須 | 既定値 |
| -------- | ----------- | -------- | ------- |
| frequency | データセット スライス生成の時間単位を指定します<p>**。サポートされる頻度**: 分、時間、日、週、月</p> | あり | 該当なし |
| interval | 頻度の乗数を指定<p>"頻度 x 間隔] は、スライスが生成されるどのくらいの頻度を決定します</p><p>。設定するデータセットを時間単位でスライスする場合は、 **頻度** に **時間**, 、および **間隔** に **1**.</p><p>**注:** 間隔を 15 以上に設定することをお勧め分として頻度を指定する場合</p> | あり | 該当なし |
| style | 間隔の開始/終了にスライスを生成するかどうかを指定します。<ul><li>StartOfInterval</li><li>EndOfInterval</li></ul><p>頻度は月に設定されているし、スタイルを EndOfInterval に設定、スライスは月の最終日に生成されます。 スタイルが StartOfInterval に設定されている場合、スライスは月の最初の日に生成します。</p><p>頻度が 1 日に設定されているし、スタイルを EndOfInterval に設定、1 日の過去 1 時間以内に、スライスが生成されます。</p>頻度が 1 時間に設定されているし、スタイルを EndOfInterval に設定、時間の終わりに、スライスが生成されます。 たとえば、午後 1 時 ~ 午後 2 時のスライスでは、午後 2 時、スライスが生成されます。</p> | いいえ | EndOfInterval |
| anchorDateTime | データセット スライスの境界を計算するスケジューラに使用される時間の絶対位置を定義します。 <p>**注:** AnchorDateTime の日付部分が頻度より詳細なかどうかは、細かい部分は無視されます。 たとえば場合、 **間隔** は **1 時間ごと** (頻度: 時間、間隔: 1) と **AnchorDateTime** が含まれています **分および秒**, 、 **分および秒** AnchorDateTime の部分は無視されます。 </p>| いいえ | 01/01/0001 |
| offset | 開始とすべてのデータセット スライスの終了をシフトする Timespan です。 <p>**注:** 結果は結合 shift キーを押し、anchorDateTime とオフセットの両方が指定されている場合。</p> | いいえ | 該当なし |

### anchorDateTime 例

**例:** 2007年で開始する 23 時間データセット スライス-04-19T08:00:00

    "availability": 
    {   
        "frequency": "Hour",        
        "interval": "23",   
        "anchorDateTime":"2007-04-19T08:00:00"  
    }


### offset 例

既定の真夜中の代わりに朝の 6 時に開始する毎日のスライス。 

    "availability":
    {
        "frequency": "Daily",
        "interval": "1",
        "offset": "06:00:00"
    }

この場合、SliceStart は 6 時間シフトし、午前 6 になります。

12 か月間の (頻度 = month、interval = 12) スケジュールの場合、offset: 60.00:00:00 は毎年 3 月 2 日または 3 を意味 (、年初から 60 日場合スタイル = StartOfInterval) 年がうるう年をされているかによって異なります。



## <a name="Policy"></a>データセット ポリシー

データセットのポリシー セクションでは、データセット スライスで満たさなければならない基準または条件を定義します。

### 検証ポリシー

| ポリシー名 | 説明 | 適用先 | 必須 | 既定値 |
| ----------- | ----------- | ---------- | -------- | ------- |
| minimumSizeMB | Azure BLOB のデータが最小サイズ要件 (メガバイト単位) を満たすことを検証します。 | Azure BLOB | いいえ | 該当なし |
|minimumRows | Azure SQL データベースまたは Azure テーブルのデータが最小行数が含まれていることを検証します。 | <ul><li>Azure SQL Database</li><li>Azure テーブル</li></ul> | いいえ | 該当なし

#### 例

**minimumSizeMB:**

    "policy":
    
    {
        "validation":
        {
            "minimumSizeMB": 10.0
        }
    }

**minimumRows**

    "policy":
    {
        "validation":
        {
            "minimumRows": 100
        }
    }

### ExternalData ポリシー

外部データセットは Data Factory のパイプライン実行で生成されないデータセットです。 データセットが「外部」としてマークされている場合、データセット スライス可用性の動作を変更するように ExternalData ポリシーを定義できます。 

| 名前 | 説明 | 必須 | 既定値  |
| ---- | ----------- | -------- | -------------- |
| dataDelay | <p>特定のスライスの外部データの可用性チェックを遅らせる時間。 たとえば場合は、データは、1 時間ごとに使用できることになって、dataDelay で確認するため、外部データが実際に使用し、対応するスライスが準備完了は遅延する場合します。</p><p>現在の時刻にのみ適用されます。たとえば、現在の 1時 00分 PM であるし、この値は 10 分間、検証は午後 1 時 10 分から開始されます。</p><p>この設定には、過去のスライスは影響しません (スライスの終了時刻 + dataDelay を使用してスライス < ようになりました) ある遅延が発生せずに処理されます。</p> <p>時刻 23:59 の時間が必要では、day.hours:minutes:seconds 形式を使用して指定されているよりも大きいです。 たとえば、24 時間を指定するには、24:00:00 ではなく、1.00:00:00 を使用します。 24:00:00 を使用した場合は、24 日間 (24.00:00:00) として処理されます。 1 日 4 時間 1:04:00:00 を指定します。 </p>| いいえ | 0 |
| retryInterval | エラーと次の再試行の間の待機時間です。 現在の時間に適用されます。前の試行に失敗した場合、最後の試行からこの時間が経過するまで待機します。 <p>現在の午後 1時 00分の場合、最初の試行を開始します。 最初の検証チェックを完了するための時間が 1 分のとき、操作に失敗した場合、次の再試行は「1:00pm + 1 分 (チェック時間) + 1 分 (再試行間隔) = 1:02pm」になります。 </p><p>過去のスライスに対してされません遅延します。 再試行はすぐに実行されます。</p> | いいえ | 00:01:00 (1 分) | 
| retryTimeout | 各再試行のタイムアウト期間。<p>10 分に設定すると、検証を 10 分以内に完了する必要があります。 検証を実行するまで 10 分より長くかかる場合、再試行はタイムアウトします。</p><p>検証のすべての試行がタイムアウトした場合、スライスはタイムアウトとしてマークされます。</p> | いいえ | 00:10:00 (10 分) |
| maximumRetry | 外部データの可用性の確認回数です。 許容される最大値は 10 です。 | いいえ | 3 | 

#### その他の例

場合 (たとえば、午前 8 時に毎月の第 3 に) 特定の日付と時刻に 1 か月ごとにパイプラインを実行する必要がありますを使って、 **オフセット** の日付を設定し、時間がタグを実行する必要があります。 

    {
      "name": "MyDataset",
      "properties": {
        "type": "AzureSqlTable",
        "linkedServiceName": "AzureSqlLinkedService",
        "typeProperties": {
          "tableName": "MyTable"
        },
        "availability": {
          "frequency": "Month",
          "interval": 1,
          "offset": "3.08:10:00",
          "style": "StartOfInterval"
        }
      }
    }

