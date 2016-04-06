<properties
pageTitle="Azure Blob Storage のインデックスを Azure Search で作成する"
description="Azure Search で Azure Blob Storage のインデックスを作成し、ドキュメントからテキストを抽出する方法について説明します。"
services="search"
documentationCenter=""
authors="chaosrealm"
manager="pablocas"
editor="" />

<tags
ms.service="search"
ms.devlang="rest-api"
ms.workload="search" ms.topic="article"  
ms.tgt_pltfrm="na"
ms.date="12/11/2015"
ms.author="eugenesh" />

# Azure Blob Storage 内ドキュメントのインデックスを Azure Search で作成する

かなりこれで、Azure Search できたをいくつかの一般的なデータ ソースのためのインデクサーを使用して、「自動的」インデックス [Azure SQL Database](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers-2015-02-28.md) と [Azure DocumentDB](../documentdb/documentdb-search-indexer.md)します。

現在は、Azure Blob Storage に格納されているドキュメントについてもインデックスを作成できるように、その機能を強化しているところです。 PDF、Office ドキュメント、HTML ページなど、BLOB に格納されているドキュメントのインデックスをもっと簡単に作成できるようにしてほしいという要望が多くのお客様から寄せられました。 これまで、そのようなことを実現するためには、独自にテキストを抽出して Azure Search インデックスにドキュメントを追加するコードを記述する必要がありました。 

> [AZURE.IMPORTANT] 現在この機能はプレビュー段階です。 バージョンを使用して REST API でのみ使用可能になる **2015年-02-28-preview**します。 プレビュー版の API は、テストと評価を目的としたものです。運用環境での使用は避けてください。

## BLOB インデックスの設定

作成および管理する Azure Search REST API を使用する設定を Azure Blob ストレージのインデクサーの構成は、 **インデクサー** と **データ ソース** 」の説明に従って [ここ](https://msdn.microsoft.com/library/azure/dn946891.aspx)します。 今後は、Azure Search .NET SDK と Azure ポータルに BLOB インデックスの作成機能が追加される予定です。

データ ソースでは、インデックスを作成するデータ、データにアクセスするために必要な資格情報、および Azure Search がデータの変更 (新しい行、変更された行、削除された行) を効率よく識別できるようにするポリシーを指定します。 データ ソースは、複数のインデクサーから使用できるように、独立したリソースとして定義します。

インデクサーは、データ ソースと検索対象のインデックスをつなげるリソースです。

BLOB インデクサーを設定するには、次の手順に従います。

1. Azure ストレージ アカウント内のコンテナー (と必要に応じてそのコンテナー内のフォルダー) を参照する `azureblob` タイプのデータ ソースを作成します。
    - ストレージ アカウントの接続文字列を `credentials.connectionString` パラメーターとして指定します。
    - コンテナー名を指定します。 必要に応じて `query` パラメーターを使用し、フォルダーを対象にすることもできます。
2. 既存のターゲット インデックスにデータ ソースを接続してインデクサーを作成します (まだインデックスがない場合はインデックスを作成してください)。

次の例でこれを具体的に示します。

    POST https://[service name].search.windows.net/datasources?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "<my storage connection string>" },
        "container" : { "name" : "my-container", "query" : "my-folder" }
    }   

次に、データ ソースとターゲット インデックスとを参照するインデクサーを作成します。 次に例を示します。

    POST https://[service name].search.windows.net/indexers?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "blob-indexer",
      "dataSourceName" : " blob-datasource ",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" }
    }


## サポートされるドキュメントの形式

BLOB インデクサーは、次の形式のドキュメントからテキストを抽出できます。

- PDF
- Microsoft Office 形式: DOCX/DOC、XLSX/XLS、PPTX/PPT、MSG (Outlook 電子メール)  
- HTML
- XML
- ZIP
- プレーン テキスト ファイル (JSON を含む)  

## ドキュメント抽出プロセス

Azure Search は、各ドキュメント (BLOB) のインデックスを次のように作成します。

- ドキュメントのテキスト コンテンツ全体が、`content` という名前の文字列フィールドに抽出されます。 現時点では、1 つの BLOB から複数のドキュメントを抽出することはできません。
    - たとえば CSV ファイルは、1 つのドキュメントとしてインデックスが作成されます。
    - 複合ドキュメントや埋め込みドキュメント (PDF が添付された Outlook 電子メールを埋め込んだ Word 文書、ZIP アーカイブなど) も、1 つのドキュメントとしてインデックスが作成されます。

- ユーザー指定のメタデータのプロパティが BLOB に存在する場合、それらのプロパティは、そのまま抽出されます。 メタデータ プロパティは、ドキュメントの抽出プロセスの特定の側面を制御するを参照してくださいも使用できる [コントロール ドキュメントの抽出にカスタム メタデータを使用して](#CustomMetadataControl) 詳細です。

- 標準的な BLOB のメタデータのプロパティは、次のフィールドに抽出されます。

    - **metadata\_storage\_name** (Edm.String) - blob のファイル名。 たとえば、/my-container/my-folder/subfolder/resume.pdf という BLOB がある場合、このフィールドの値は `resume.pdf` になります。

    - **metadata\_storage\_path** (Edm.String) で、ストレージ アカウントなど、blob の完全な URI です。 たとえば、`https://myaccount.blob.core.windows.net/my-container/my-folder/subfolder/resume.pdf` のように指定します。

    - **metadata\_storage\_content\_type** (Edm.String) - コンテンツの blob をアップロードするために使用するコードで指定された種類。 たとえば、「`application/octet-stream`」のように入力します。

    - **metadata\_storage\_last\_modified** (Edm.DateTimeOffset) - 最後に変更された blob のタイムスタンプ。 インデックスの初回作成後に最初から作成し直さなくても済むよう、変更された BLOB を Azure Search が特定するために、このタイムスタンプが使用されます。

    - **metadata\_storage\_size** (Edm.Int64) - blob のサイズ (バイト単位)。

    - **metadata\_storage\_content\_md5** (Edm.String) の使用可能な場合は、blob のコンテンツの MD5 ハッシュ。

- 各ドキュメントの形式に固有のメタデータのプロパティが示されているフィールドに抽出されます [ここ](#ContentSpecificMetadata)します。

検索インデックスに対し、ここに挙げたすべてのプロパティのフィールドを定義する必要はありません。実際のアプリケーションで必要となるプロパティだけを取り込んでください。 

> [AZURE.NOTE] 多くの場合、既存のインデックス内のフィールド名はドキュメントの抽出中に生成されたフィールドの名前と同じになります。 使用する **フィールド マッピング** 検索インデックス内のフィールド名を Azure Search で提供されるプロパティ名にマップします。 フィールドのマッピングの例を次に示します。 

## ドキュメントのキー フィールドの選択と各種フィールド名の処理

Azure Search では、ドキュメントがそのキーによって一意に識別されます。 それぞれの検索インデックスに、Edm.String 型のキー フィールドが 1 つだけ存在している必要があります。 キー フィールドは、インデックスに追加するドキュメントごとに必要となります (唯一の必須フィールド)。  
   
抽出されたフィールドとインデックスのキー フィールドとのマッピングは、慎重に検討する必要があります。 その例を次に示します。

- **metadata\_storage\_name** - これは、便利な候補となりますが、されることに注意 1) 名前可能性がありますいない一意で、他のフォルダー内の同じ名前と 2) 名を使用した blob がある場合があります、破線などのドキュメント キーとして無効な文字を含めることができます。 無効な文字は、インデクサーのプロパティで `base64EncodeKeys` オプションを有効にすることによって処理することができます。その場合、API 呼び出し (Lookup など) にドキュメント キーを渡すとき、必ずエンコードしてください (たとえば、.net を使えば、 [UrlTokenEncode メソッド](https://msdn.microsoft.com/library/system.web.httpserverutility.urltokenencode.aspx) 目的のため)。

- **metadata\_storage\_path** - 一意性、により、完全なパスが、パスが確実に含まれる `/` な文字が [ドキュメント キーに無効な](https://msdn.microsoft.com/library/azure/dn857353.aspx)です。  この場合も、`base64EncodeKeys` オプションを使用してキーをエンコーディングすることができます。

- いずれの選択肢も利用できない場合の最終的な手段として、独自にメタデータのプロパティを BLOB に追加する方法があります。 ただし、この方法を選んだ場合、BLOB のアップロード プロセスで、該当するメタデータのプロパティをすべての BLOB に追加する必要があります。 キーは必須のプロパティであるため、そのプロパティを持たない BLOB については、インデックスが一切作成されません。

> [AZURE.IMPORTANT] Azure Search は自動的に使用されているインデックスのキー フィールドの明示的なマッピングがない場合は、 `metadata_storage_path` (上記で 2 つ目のオプション)、キーとキーの base 64 エンコードは有効にするとします。

この例では、`metadata_storage_name` フィールドをドキュメント キーにしましょう。 また、既存のインデックスには、`key` という名前のキー フィールドと、ドキュメントのサイズを格納するための `fileSize` フィールドが存在するものとします。 それらを適切に対応付けるために、インデクサーを作成または更新するときに、次のフィールド マッピングを指定します。

    "fieldMappings" : [
      { "sourceFieldName" : "metadata_storage_name", "targetFieldName" : "key" },
      { "sourceFieldName" : "metadata_storage_size", "targetFieldName" : "fileSize" }
    ]

以下に示したのは、それらを反映したコードです。既存のインデクサーに対してフィールドのマッピングを追加し、キーの base-64 エンコーディングを有効にしています。

    PUT https://[service name].search.windows.net/indexers/blob-indexer?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
      "dataSourceName" : " blob-datasource ",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" },
      "fieldMappings" : [
        { "sourceFieldName" : "metadata_storage_name", "targetFieldName" : "key" },
        { "sourceFieldName" : "metadata_storage_size", "targetFieldName" : "fileSize" }
      ],
      "parameters" : { "base64EncodeKeys": true }
    }

> [AZURE.NOTE] フィールド マッピングの詳細については、次を参照してください。 [今回](search-indexers-customization.md)します。

## インデックスの増分作成と削除の検出

スケジュールに従って実行するように BLOB のインデクサーを設定すると、その BLOB の `LastModified` タイムスタンプから判断された変更済みの BLOB のみインデックスが再構築されます。

> [AZURE.NOTE] 変更検出ポリシーを指定する必要はありません: 増分インデックスを有効にするために自動的にします。

特定のドキュメントをインデックスから削除するよう指定する場合は、論理削除方式を使用してください。つまり、該当する BLOB を削除するのではなく、それらが削除されたことを示すカスタムのメタデータ プロパティをデータ ソースに追加し、論理削除の検出ポリシーを設定します。

> [AZURE.WARNING] 削除検出ポリシーを使用する代わりに blob を削除する場合、対応するドキュメントは、検索インデックスから削除されません。

たとえば、以下に示すポリシーでは、メタデータのプロパティ `IsDeleted` の値が `true` であるとき、BLOB が削除されたと見なされます。

    PUT https://[service name].search.windows.net/datasources?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "<your storage connection string>" },
        "container" : { "name" : "my-container", "query" : "my-folder" },
        "dataDeletionDetectionPolicy" : {
            "@odata.type" :"#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",   
            "softDeleteColumnName" : "IsDeleted",
            "softDeleteMarkerValue" : "true"
        }
    }   

<a name="ContentSpecificMetadata"></a>
## コンテンツの種類ごとのメタデータのプロパティ

以下の表は、各ドキュメント形式に関して実行される処理と、Azure Search によって抽出されるメタデータのプロパティをまとめたものです。

ドキュメントの形式/コンテンツの種類 | コンテンツの種類ごとのメタデータのプロパティ | 処理の詳細
-------------------------------|-------------------------------------------|-------------------
HTML (`text/html`) | `metadata_content_encoding`<br/>`metadata_content_type`<br/>`metadata_language`<br/>`metadata_description`<br/>`metadata_keywords`<br/>`metadata_title` | HTML マークアップを削除し、テキストを抽出します。
PDF (`application/pdf`) | `metadata_content_type`<br/>`metadata_language`<br/>`metadata_author`<br/>`metadata_title`| テキストを抽出します。埋め込みドキュメントも対象となります (画像を除く)。
DOCX (application/vnd.openxmlformats-officedocument.wordprocessingml.document) | `metadata_content_type`<br/>`metadata_author`<br/>`metadata_character_count`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_page_count`<br/>`metadata_word_count` | テキストを抽出します。埋め込みドキュメントも対象となります。
DOC (application/msword) | `metadata_content_type`<br/>`metadata_author`<br/>`metadata_character_count`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_page_count`<br/>`metadata_word_count` | テキストを抽出します。埋め込みドキュメントも対象となります。
XLSX (application/vnd.openxmlformats-officedocument.spreadsheetml.sheet) | `metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified` | テキストを抽出します。埋め込みドキュメントも対象となります。
XLS (application/vnd.ms-excel) | `metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified` | テキストを抽出します。埋め込みドキュメントも対象となります。
PPTX (application/vnd.openxmlformats-officedocument.presentationml.presentation) | `metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_slide_count`<br/>`metadata_title` | テキストを抽出します。埋め込みドキュメントも対象となります。
PPT (application/vnd.ms-powerpoint) | `metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_slide_count`<br/>`metadata_title` | テキストを抽出します。埋め込みドキュメントも対象となります。
MSG (application/vnd.ms-outlook) | `metadata_content_type`<br/>`metadata_message_from`<br/>`metadata_message_to`<br/>`metadata_message_cc`<br/>`metadata_message_bcc`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_subject` | テキストを抽出します。添付ファイルも対象となります。
ZIP (application/zip) | `metadata_content_type` | アーカイブ内のすべてのドキュメントからテキストを抽出します。
XML (application/xml) | `metadata_content_type`</br>`metadata_content_encoding`</br> | XML マークアップを削除し、テキストを抽出します。 </td>
JSON (application/json) | `metadata_content_type`</br>`metadata_content_encoding` | テキストを抽出します。<br/>メモ: JSON blob から複数のドキュメント フィールドを抽出する場合は、くださいに投票して [この UserVoice の推奨事項](https://feedback.azure.com/forums/263029-azure-search/suggestions/11113539-extract-document-structure-from-json-blobs)
プレーン テキスト (text/plain) | `metadata_content_type`</br>`metadata_content_encoding`</br> | 

<a name="CustomMetadataControl"></a>
## カスタム メタデータを使ったドキュメント抽出の制御

BLOB のインデックス作成とドキュメント抽出プロセスは、メタデータのプロパティを BLOB に追加することによって、ある程度制御することができます。 現在サポートされているプロパティは次のとおりです。

プロパティ名 | プロパティ値 | 説明
--------------|----------------|------------
AzureSearch_Skip | "true" | BLOB を完全にスキップするようそのインデクサーに指示するプロパティです。メタデータもコンテンツも、一切抽出されません。 特定のコンテンツ タイプをスキップする必要があるときに利用できます。また、特定の BLOB で何度もエラーが発生し、インデックス作成プロセスが中断されるときにも利用できます。

## Azure Search の品質向上にご協力ください

機能要求や改善のためのアイデアがある場合は、くださいに達し、ことで、 [UserVoice サイト](https://feedback.azure.com/forums/263029-azure-search)します。


