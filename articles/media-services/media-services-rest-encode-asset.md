<properties 
    pageTitle="Azure Media Encoder を使用して資産をエンコードする方法" 
    description="Azure メディア エンコーダーを使用して Media Services でメディア コンテンツをエンコードする方法について説明します。コード サンプルでは REST API を使用しています。" 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/07/2015"
    ms.author="juliako"/>



# Azure Media Encoder を使用して資産をエンコードする方法

> [AZURE.SELECTOR]
- [.NET](media-services-dotnet-encode-asset.md)
- [REST](media-services-rest-encode-asset.md)
- [Portal](media-services-manage-content.md#encode)


## 概要

インターネット経由でデジタル ビデオを配信するには、メディアを圧縮する必要があります。 デジタル ビデオ ファイルはサイズがとても大きく、インターネット経由で配信したり、顧客のデバイスでうまく表示したりできない可能性があります。 エンコードはビデオやオーディオを圧縮するプロセスです。圧縮することでメディア コンテンツが見やすくなります。

エンコード ジョブは Media Services で最も一般的な処理の 1 つです。 エンコード ジョブを作成することで、メディア ファイルをあるエンコードから別のエンコードに変換できるようになります。 エンコードには、Media Services の組み込み Media Encoder を使用できます。 Media Services のパートナーから提供されているエンコーダーを使うこともできます。サードパーティのエンコーダーは Azure Marketplace から入手できます。 エンコーディング タスクの詳細を指定するには、エンコーダー用に定義されたプリセット文字列を使用するか、プリセット構成ファイルを使用します。 使用できるプリセットの種類を表示するには、次を参照してください。 [Azure Media Services 用のタスク プリセット](https://msdn.microsoft.com/library/azure/dn619392.aspx)します。 サード パーティのエンコーダーを使用している場合は [ファイルを検証](https://msdn.microsoft.com/library/azure/dn750842.aspx)します。


各ジョブは実行する処理の種類に応じて 1 つまたは複数のタスクを持つことができます。 REST API を使って、2 つの方法のいずれかでジョブおよびジョブに関連するタスクを作成できます。

- タスクは、Job エンティティの Tasks ナビゲーション プロパティまたは
- OData バッチ処理を使用して、インラインで定義できます。


常に中間ファイルをアダプティブ ビットレート MP4 セットにエンコードしを使用して、目的の形式に変換することを推奨、 [Dynamic Packaging](https://msdn.microsoft.com/library/azure/jj889436.aspx)します。 動的パッケージを活用するには、コンテンツの配信元となるストリーミング エンドポイントのオンデマンド ストリーミング ユニットを 1 つ以上取得する 必要があります。 詳細については、次を参照してください。 [Media services の規模変更方法](media-services-manage-origins.md#scale_streaming_endpoints)します。

出力資産がストレージで暗号化されている場合は、資産配信のポリシーを構成する必要があります。 詳細については、次を参照してください。 [アセット配信ポリシーを構成する](media-services-rest-configure-asset-delivery-policy.md)します。

>[AZURE.NOTE]メディア プロセッサの参照を開始する前に、正しいメディア プロセッサの ID を使用していることを確認してください。 詳細については、次を参照してください。 [メディア プロセッサの取得](media-services-rest-get-media-processor.md)します。

## 1 つのエンコード タスクを持つジョブの作成

>[AZURE.NOTE] Media Services REST API を使用する場合は、次のことに考慮します。
>
>Media Services でエンティティにアクセスするときは、HTTP 要求で特定のヘッダー フィールドと値を設定する必要があります。 詳細については、次を参照してください。 [Media Services REST API 開発用セットアップ](media-services-rest-how-to-use.md)します。

>Https://media.windows.net に正常に接続されると、別の Media Services の URI を指定する 301 リダイレクトを受け取ります。 」の説明に従って、新しい URI に後続の呼び出しを行う必要があります [REST API を使用して Media Services に接続する](media-services-rest-connect_programmatically.md)します。


次の例では、1 つのタスクが設定されたジョブを作成して公開し、特定の解像度と質でビデオをエンコードする方法について説明します。 Azure メディア エンコーダーでエンコードをするときは、指定されているタスク構成プリセットを使用することができます [ここ](https://msdn.microsoft.com/library/azure/dn619389.aspx)します。

要求:

    POST https://media.windows.net/API/Jobs HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer <token value>
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000
    Host: media.windows.net
    
    
    {"Name" : "NewTestJob", "InputMediaAssets" : [{"__metadata" : {"uri" : "https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3Aaab7f15b-3136-4ddf-9962-e9ecb28fb9d2')"}}],  "Tasks" : [{"Configuration" : "H264 Broadband 720p", "MediaProcessorId" : "nb:mpid:UUID:1b1da727-93ae-4e46-a8a1-268828765609",  "TaskBody" : "<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset>JobOutputAsset(0)</outputAsset></taskBody>"}]}

応答:

    HTTP/1.1 201 Created
    
    . . . 

### 出力アセットの名前を設定します。

次の例では、assetName 属性を設定する方法を示します。

    { "TaskBody" : "<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset assetName=\"CustomOutputAssetName\">JobOutputAsset(0)</outputAsset></taskBody>"}

## 考慮事項

- TaskBody プロパティでは、リテラル XML を使用して、タスクが使用するアセットの入力または出力数を定義する必要があります。 タスクのトピックには、XML のための XML スキーマ定義が含まれます。
- 各内部値を TaskBody の定義に <inputAsset> と <outputAsset> JobInputAsset(value)) または joboutputasset (として設定する必要があります。
- タスクは、複数の出力資産を持つことができます。 1 つの JobOutputAsset(x) はジョブ内のタスクの出力として一度だけ使用できます。
- タスクの入力資産として、JobInputAsset または JobOutputAsset を指定できます。
- タスクは、サイクルを形成することはできません。
- JobInputAsset または JobOutputAsset に渡す値パラメーターは、Asset のインデックス値を表します。 実際の Asset は、Job エンティティ定義の InputMediaAssets および OutputMediaAssets ナビゲーション プロパティで定義されます。
- Media Services は OData v3 上に構築されるため、 InputMediaAssets および OutputMediaAssets ナビゲーション プロパティ コレクション内の個々の資産は、"__metadata : uri" の名前と値のペアによって参照されます。
- InputMediaAssets は、Media Services で作成した1 つまたは複数の資産にマップされます。 OutputMediaAssets はシステムによって作成されます。 既存の資産は参照しません。
- OutputMediaAssets は、assetName 属性を使用して名前を付けることができます。この属性が存在しない場合、OutputMediaAsset の名前の内部テキ スト値になります、 <outputAsset> 要素は、ジョブ名の値またはジョブ Id 値 (Name プロパティが定義されていない場合) のいずれかのサフィックスが付いています。たとえば、"Sample"に assetName の値を設定する場合は、OutputMediaAsset Name プロパティは "Sample" に指定されます。ただし、assetName の値を設定せずジョブ名を "NewJob" に設定した場合は、OutputMediaAsset Name は "JobOutputAsset(value)_NewJob" になります。


## チェーン タスクによるジョブの作成

アプリケーション シナリオの多くで、一連のタスク処理が開発者によって作成されます。 Media Services では、一連のチェーン タスクを作成できます。 各タスクはそれぞれ異なる処理手順を実行し、異なるメディア プロセッサを使うことができます。 チェーン タスクでは、あるタスクから別のタスクに資産を渡しながら一連のタスクを順番に実行できます。 ただし、ジョブで実行されるタスクが必ず順番に実行されなければならないということではありません。 チェーン タスクを作成するときは、一連の **ITask** オブジェクトは 1 つの **IJob** オブジェクト内で作成されます。
>[AZURE.NOTE] 現時点では、ジョブごとに設定できるタスクは 30 までです。 30 以上のタスクをつなげる必要がある場合、別のジョブを作成してタスクを設定します。


    POST https://media.windows.net/api/Jobs HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer <token value>
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000
    
    {  
       "Name":"NewTestJob",
       "InputMediaAssets":[  
          {  
             "__metadata":{  
                "uri":"https://testrest.cloudapp.net/api/Assets('nb%3Acid%3AUUID%3A910ffdc1-2e25-4b17-8a42-61ffd4b8914c')"
             }
          }
       ],
       "Tasks":[  
          {  
             "Configuration":"H264 Adaptive Bitrate MP4 Set 720p",
             "MediaProcessorId":"nb:mpid:UUID:1b1da727-93ae-4e46-a8a1-268828765609",
             "TaskBody":"<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset>JobOutputAsset(0)</outputAsset></taskBody>"
          },
          {  
             "Configuration":"H264 Smooth Streaming 720p",
             "MediaProcessorId":"nb:mpid:UUID:1b1da727-93ae-4e46-a8a1-268828765609",
             "TaskBody":"<?xml version=\"1.0\" encoding=\"utf-16\"?><taskBody><inputAsset>JobOutputAsset(0)</inputAsset><outputAsset>JobOutputAsset(1)</outputAsset></taskBody>"
          }
       ]
    }

### 考慮事項

タスクのチェーンを有効にするには、

- ジョブに少なくとも 2 つのタスクがある必要があります。
- タスクの入力がジョブ内の別のタスクの出力である、1 つ以上のタスクがある必要があります。

## OData バッチ処理の使用

次の例では、OData バッチ処理を使用して、ジョブとタスクを作成する方法を示します。 バッチ処理については、次を参照してください。 [Open Data Protocol (OData) バッチ処理](http://www.odata.org/documentation/odata-version-3-0/batch-processing/)します。

    POST https://media.windows.net/api/$batch HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Content-Type: multipart/mixed; boundary=batch_a01a5ec4-ba0f-4536-84b5-66c5a5a6d34e
    Accept: multipart/mixed
    Accept-Charset: UTF-8
    Authorization: Bearer <token>
    x-ms-version: 2.11
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000
    Host: media.windows.net
    
    
    --batch_a01a5ec4-ba0f-4536-84b5-66c5a5a6d34e
    Content-Type: multipart/mixed; boundary=changeset_122fb0a4-cd80-4958-820f-346309967e4d
    
    --changeset_122fb0a4-cd80-4958-820f-346309967e4d
    Content-Type: application/http
    Content-Transfer-Encoding: binary
    
    POST https://media.windows.net/api/Jobs HTTP/1.1
    Content-ID: 1
    Content-Type: application/json
    Accept: application/json
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    Accept-Charset: UTF-8
    Authorization: Bearer <token>
    x-ms-version: 2.11
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000
    
    {"Name" : "NewTestJob", "InputMediaAssets@odata.bind":["https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3A2a22445d-1500-80c6-4b34-f1e5190d33c6')"]}
    
    --changeset_122fb0a4-cd80-4958-820f-346309967e4d
    Content-Type: application/http
    Content-Transfer-Encoding: binary
    
    POST https://media.windows.net/api/$1/Tasks HTTP/1.1
    Content-ID: 2
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    Accept-Charset: UTF-8
    Authorization: Bearer <token>
    x-ms-version: 2.11
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000
    
    {  
       "Configuration":"H264 Adaptive Bitrate MP4 Set 720p",
       "MediaProcessorId":"nb:mpid:UUID:1b1da727-93ae-4e46-a8a1-268828765609",
       "TaskBody":"<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset assetName=\"Custom output name\">JobOutputAsset(0)</outputAsset></taskBody>"
    }
    
    --changeset_122fb0a4-cd80-4958-820f-346309967e4d--
    --batch_a01a5ec4-ba0f-4536-84b5-66c5a5a6d34e--

## JobTemplate を使用したジョブの作成

共通する一連のタスクを使って複数のアセットを処理するときは、JobTemplates を使用して、既定のタスク プリセットやタスクの順序などを指定すると便利です。

次の例では、TaskTemplate をインラインで定義して JobTemplate を作成しています。 この TaskTemplate では、Azure メディア エンコーダーを MediaProcessor として、アセット ファイルをエンコードしていますが、他の MediaProcessors を使用してもかまいません。


    POST https://media.windows.net/API/JobTemplates HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer <token value>
    Host: media.windows.net
    
    
    {"Name" : "NewJobTemplate25", "JobTemplateBody" : "<?xml version=\"1.0\" encoding=\"utf-8\"?><jobTemplate><taskBody taskTemplateId=\"nb:ttid:UUID:071370A3-E63E-4E81-A099-AD66BCAC3789\"><inputAsset>JobInputAsset(0)</inputAsset><outputAsset>JobOutputAsset(0)</outputAsset></taskBody></jobTemplate>", "TaskTemplates" : [{"Id" : "nb:ttid:UUID:071370A3-E63E-4E81-A099-AD66BCAC3789", "Configuration" : "H264 Smooth Streaming 720p", "MediaProcessorId" : "nb:mpid:UUID:2e7aa8f3-4961-4e0c-b4db-0e0439e524f5", "Name" : "SampleTaskTemplate2", "NumberofInputAssets" : 1, "NumberofOutputAssets" : 1}] }

>[AZURE.NOTE]GUID 識別子は、他の Media Services エンティティとは異なり、TaskTemplate ごとに新たに定義し、要求本文の taskTemplateId と Id プロパティに設定する必要があります。 コンテンツ ID スキームについては、「Azure Media Services エンティティの識別」で説明されているスキームに従ってください。 また、JobTemplates を更新することはできません。 最新の変更内容を基に新たに作成する必要があります。


成功した場合、次の応答が返されます。

    HTTP/1.1 201 Created
    
    . . .

次の例では、JobTemplate ID を参照してジョブを作成する方法を示します。

    POST https://media.windows.net/API/Jobs HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer <token value>
    Host: media.windows.net
    
    
    {"Name" : "NewTestJob", "InputMediaAssets" : [{"__metadata" : {"uri" : "https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3A3f1fe4a2-68f5-4190-9557-cd45beccef92')"}}], "TemplateId" : "nb:jtid:UUID:15e6e5e6-ac85-084e-9dc2-db3645fbf0aa"}

成功した場合、次の応答が返されます。

    HTTP/1.1 201 Created
    
    . . . 

## Media Services のラーニング パス

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## フィードバックの提供

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


## 次のステップ

アセットをエンコードするジョブを作成する方法を理解するには、 [方法を確認してジョブの進行状況 Media Services による](media-services-rest-check-job-progress.md) トピックです。


## 関連項目

[メディア プロセッサを取得します。](media-services-rest-get-media-processor.md)




