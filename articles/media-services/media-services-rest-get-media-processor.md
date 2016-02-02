<properties 
    pageTitle="メディア プロセッサを作成する方法 | Microsoft Azure" 
    description="メディア プロセッサ コンポーネントを作成し、Azure Media Services 用にメディア コンテンツのエンコード、形式の変換、暗号化、または復号化を行う方法について説明します。" 
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
    ms.date="12/05/2015" 
    ms.author="juliako"/>



# 方法: メディア プロセッサ インスタンスを取得する

> [AZURE.SELECTOR]
- [.NET](media-services-get-media-processor.md)
- [REST](media-services-rest-get-media-processor.md)


## 概要

メディア プロセッサは、Media Services のコンポーネントとして、メディア コンテンツのエンコード、形式変換、暗号化、復号化など、特定の処理タスクを担います。 通常、メディア コンテンツのエンコード、暗号化、形式変換を行うタスクの作成時にメディア プロセッサを作成します。

次の表は、利用可能なメディア プロセッサの名前と説明の一覧です。

 メディア プロセッサ名| 説明| 詳細情報
---|---|---
 Azure Media Encoder| Azure Media Encoder を使用してエンコード タスクを実行できます。| [Azure メディア エンコーダー](media-services-encode-asset.md#azure_media_encoder)
 メディア エンコーダー スタンダード| Media Encoder Standard を使用してエンコード タスクを実行できます。| [Azure メディア エンコーダー](media-services-encode-asset.md#media_encoder_standard)
 メディア エンコーダー プレミアム ワークフロー| メディア エンコーダー Premium ワークフローを使用してエンコード タスクを実行できます。| [メディア エンコーダー プレミアム ワークフロー](media-services-encode-asset.md#media_encoder_premium_wokrflow)
 Azure Media Indexer| メディア ファイルとコンテンツを検索可能にすると共に、クローズド キャプション トラックの生成を可能にします。| [Azure Media Indexer によるメディア ファイルのインデックス作成](media-services-index-content.md)します。
 Azure Media Hyperlapse (プレビュー)| ビデオ安定化を使用して、ビデオの "凸凹" を取り除いて滑らかにすることができます。コンテンツをすばやく使用可能なクリップにすることもできます。| [Azure Media Hyperlapse](http://azure.microsoft.com/blog/?p=286281&preview=1&_ppp=61e1a0b3db)</a>
 Storage Decryption| ストレージ暗号化を使用して暗号化されたメディア資産を復号化できます。| 該当なし
 Windows Azure Media Packager| メディア資産を .mp4 からスムーズ ストリーミング形式に変換できます。また、スムーズ ストリーミングから Apple HTTP ライブ ストリーミング (HLS) 形式にメディア資産を変換できます。| [Azure Media Packager 用のタスク プリセット文字列](http://msdn.microsoft.com/library/hh973635.aspx)
 Windows Azure Media Encryptor| PlayReady Protection を使用してメディア資産を暗号化できます。| [Azure Media Packager 用のタスク プリセット文字列](http://msdn.microsoft.com/library/hh973610.aspx)

## MediaProcessor の取得

>[AZURE.NOTE] Media Services REST API を使用する場合は、次のことに考慮します。
>
>Media Services でエンティティにアクセスするときは、HTTP 要求で特定のヘッダー フィールドと値を設定する必要があります。 詳細については、次を参照してください。 [Media Services REST API 開発用セットアップ](media-services-rest-how-to-use.md)します。

>Https://media.windows.net に正常に接続されると、別の Media Services の URI を指定する 301 リダイレクトを受け取ります。 」の説明に従って、新しい URI に後続の呼び出しを行う必要があります [REST API を使用して Media Services に接続する](media-services-rest-connect_programmatically.md)します。



次の REST コールでは、名前 (ここでは **Azure Media Encoder**) でメディア プロセッサ インスタンスを取得する方法を示しています。


要求:

    GET https://media.windows.net/api/MediaProcessors()?$filter=Name%20eq%20'Azure%20Media%20Encoder' HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=juliakoams1&urn%3aSubscriptionId=zbbef702-e769-477b-2233-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423635565&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=6zwXEn7YJzVJbVCNpqDUjBLuE5iUwsdJbWvJNvpY3%2b8%3d
    x-ms-version: 2.11
    Host: media.windows.net

応答:

    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 273
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: 8a291764-4ed7-405d-aa6e-d3ebabb0b3f6
    request-id: dceeb559-48b5-48e1-81d3-d324b6203d51
    x-ms-request-id: dceeb559-48b5-48e1-81d3-d324b6203d51
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Wed, 11 Feb 2015 00:19:56 GMT
    
    {"odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#MediaProcessors","value":[{"Id":"nb:mpid:UUID:1b1da727-93ae-4e46-a8a1-268828765609","Description":"Azure Media Encoder","Name":"Azure Media Encoder","Sku":"","Vendor":"Microsoft","Version":"4.4"}]}

## Media Services のラーニング パス

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## フィードバックの提供

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


## 次のステップ

メディア プロセッサ インスタンスを取得する方法を理解するには、 [資産 [をエンコードする方法][] は、Azure Media Encoder を使用してアセットをエンコードする方法を説明するトピックです。


[how to encode an asset]: media-services-rest-encode-asset.md 
[task preset strings for the azure media encoder]: http://msdn.microsoft.com/library/jj129582.aspx 
[how to: connect to media services programmatically]: ../media-services-rest-connect_programmatically/ 

