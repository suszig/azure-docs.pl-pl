<properties 
    pageTitle="Azure クラシック ポータルを使用して Azure Media Services でメディア コンテンツを管理する方法" 
    description="Azure Media Services でメディア コンテンツを管理する方法について説明します。 ここで取り上げる内容は、アップロード、インデックスの作成、エンコード、暗号化、および発行についてです。" 
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
    ms.date="11/09/2015"
    ms.author="juliako"/>


# Azure クラシック ポータルを使用した Azure Media Services でのコンテンツの管理


このトピックでは、Azure クラシック ポータルを使用して、Media Services アカウントのメディア コンテンツを管理する方法について説明します。

このトピックでは、ポータルから直接、次のコンテンツの操作を実行する方法を説明します。

- コンテンツ情報 (発行状態、発行 URL、サイズ、日付、最終更新日時、資産の暗号化の有無など) も確認できます。
- 新しいコンテンツのアップロード
- コンテンツのインデックス作成
- コンテンツのエンコード
- 暗号化
- コンテンツの発行および発行の取り消し
- コンテンツの再生


##<a id="upload"></a>方法: コンテンツをアップロードする


[AZURE.INCLUDE [media-services-selector-upload-files](../../includes/media-services-selector-upload-files.md)]


1.  [Azreu クラシック ポータル](http://go.microsoft.com/fwlink/?LinkID=256666&clcid=0x409), 、] をクリックして **Media Services** し、Media Services アカウント名をクリックします。
2. [コンテンツ] ページを選択します。 
3. クリックして、 **アップロード** ページまたはポータルの下部にあるボタンをクリックします。 
4.  **コンテンツのアップロード** ダイアログ ボックスで、目的のアセット ファイルを見つけます。 ファイルをクリックし、クリックして **開く** かキーを押して **Enter**します。

    ![UploadContentDialog][uploadcontent]

5. [コンテンツのアップロード] ダイアログで、チェック ボタンをクリックして、ファイル名とコンテンツ名をそのまま使用します。
6. アップロードが開始され、進捗状況はポータルの下部で確認できます。  

    ![JobStatus][status]

アップロードが完了すると、コンテンツ一覧に新しい資産が表示されます。 名前が付いて、慣例的に"**-ソース**"エンコード タスクのソース コンテンツとして新しいコンテンツを追跡するために最後に追加します。

![ContentPage][contentpage]

ファイル サイズの値がない更新される場合、アップロード処理が終了した後、次のようにキーを押して、 **メタデータの同期** ] ボタンをクリックします。 これにより資産のファイル サイズがストレージの実際のファイル サイズと同期され、[コンテンツ] ページに表示される値が更新されます。  

##<a id="index"></a>方法: コンテンツのインデックスを作成する

> [AZURE.SELECTOR]
- [.NET](media-services-index-content.md)
- [ポータル](media-services-manage-content.md#index)

Azure Media Indexer を使用すると、メディア ファイルのコンテンツを検索対応にしたり、字幕やキーワード用にフルテキストのトランスクリプトを生成したりできます。 コンテンツのインデックスは、下記の手順に従って、Azure クラシック ポータルを使用して作成できます。 ただし、対象となるファイルの選択やインデックス作成ジョブの動作を細かく制御する必要がある場合は、Media Services SDK for .NET または REST API を使用してください。 詳細については、次を参照してください。 [Azure Media Indexer によるメディア ファイルのインデックス作成](media-services-index-content.md)します。

Azure クラシック ポータルを使用してコンテンツのインデックスを作成するには、次の手順に従います。

1. インデックスの作成対象となるファイルを選択します。
そのファイル タイプのインデックスを作成できる場合、[コンテンツ] ページの一番下にある [プロセス] ボタンが有効になります。
1. [プロセス] ボタンをクリックします。
2.  **プロセス** ダイアログの選択、 **Azure Media Indexer** プロセッサ。
3. 詳細なプロセス] ダイアログに入力し、 **タイトル** と **説明** 入力メディア ファイルの情報です。

![Process][process]

##<a id="encode"></a>方法: コンテンツをエンコードする

> [AZURE.SELECTOR]
- [.NET](media-services-dotnet-encode-asset.md)
- [REST ()](media-services-rest-encode-asset.md)
- [ポータル](media-services-manage-content.md#encode)

インターネット経由でデジタル ビデオを配信するには、メディアを圧縮する必要があります。 Media Services には、コンテンツのエンコード方法 (使用するコーデック、ファイル形式、解像度、ビットレートなど) を指定できる Media Encoder が用意されています。

クライアントに対するアダプティブ ビットレート ストリーミング配信は、Azure Media Services の代表的な用途の 1 つです。 アダプティブ ビットレート ストリーミングでは、現在のネットワーク帯域幅、CPU 使用率などの条件に基づいてビデオが表示されるため、高低のビットレート ストリームの切り替えをクライアント側で行うことができます。 Media Services でサポートされるアダプティブ ビットレート ストリーミング テクノロジは、HTTP ライブ ストリーミング (HLS)、スムーズ ストリーミング、MPEG DASH、HDS (Adobe PrimeTime/Access のライセンスが必要) です。

Media Services には動的パッケージ化機能があり、アダプティブ ビットレート MP4 またはスムーズ ストリーミングでエンコードされたコンテンツを、Media Services でサポートされるストリーミング形式 (MPEG DASH、HLS、スムーズ ストリーミング、HDS) でそのまま配信することができます。つまり、これらのストリーミング形式に再度パッケージ化する必要がありません。

動的パッケージ化機能を利用するには、次の作業が必要となります。

- メザニン (ソース) ファイルを一連のアダプティブ ビットレート MP4 ファイルまたはアダプティブ ビットレート スムーズ ストリーミング ファイルにエンコードします (エンコードの手順は、このチュートリアルの後半で説明しています)。
- コンテンツに配信するストリーミング エンドポイントの 1 つ以上のオンデマンド ストリーミング ユニットを取得します。 詳細については、次を参照してください。 [スケール、オンデマンド ストリーミングの予約ユニット方法](media-services-manage-origins.md#scale_streaming_endpoints/)します。

動的パッケージ化機能を使用した場合、保存と課金の対象となるのは、単一のストレージ形式のファイルのみです。Media Services がクライアントからの要求に応じて適切な応答を構築して返します。

動的パッケージ化機能を使用できることに加え、オンデマンド ストリーミング予約ユニットを使用すると、専用の送信容量を 200 Mbps 単位で購入できます。 既定では、オンデマンド ストリーミングは、サーバー リソース (コンピューティング、送信容量など) を他のユーザーと共有する共有インスタンス モデルとして構成されます。 オンデマンド ストリーミングのスループットを高めるために、オンデマンド ストリーミング占有ユニットの購入をお勧めします。

ここでは、Azure クラシック ポータルを使用して、Azure Media Encoder でコンテンツをエンコードする手順について説明します。

1.  エンコードの対象となるファイルを選択します。
そのファイル タイプのエンコードがサポートされている場合、[コンテンツ] ページの一番下にある [プロセス] ボタンが有効になります。
4.  **プロセス** ダイアログ ボックスで、選択、 **Azure Media Encoder** プロセッサ。
5. いずれかから選択、 **エンコーディング構成**します。

![Process2][process2]


 [Azure Media Encoder 用のタスク プリセット文字列](https://msdn.microsoft.com/library/azure/dn619392.aspx) トピックではどのような各プリセットの **(動的パッケージ化) のアダプティブ ストリーミング用プリセット**, 、**プログレッシブ ダウンロード用プリセット**, 、**アダプティブ ストリーミング用レガシ プリセット**  意味します。


 **他の** 構成は以下について説明します。

+ **PlayReady コンテンツ保護を使用したエンコード**します。 このプリセットは、PlayReady Content Protection を使用してエンコードされた資産を生成します。


既定では、Media Services PlayReady ライセンス サービスが使用されます。 クライアントが PlayReady 暗号化コンテンツを再生するためにライセンスを取得可能な他のサービスを指定するには、REST または Media Services .NET SDK API を使用します。 詳細については、次を参照してください。 [コンテンツを保護する静的暗号化を使用した]() し、設定、 **licenseAcquisitionUrl** Media Encryptor プリセットのプロパティです。 または、動的暗号化を使用し、設定、 **PlayReadyLicenseAcquisitionUrl** 」の説明に従ってプロパティ [PlayReady 動的暗号化とライセンス提供サービス](http://go.microsoft.com/fwlink/?LinkId=507720 )します。
+ **再生 (flash/silverlight) Pc/mac で**します。 このプリセットは、次のような特徴を持つスムーズ ストリーミング資産を作成します。AAC を使用して 96 kbps で CBR エンコードされた 44.1 kHz 16 ビット/サンプルのステレオ オーディオ、および H.264 Main Profile を使用して 3400 ～ 400 kbps の範囲で 6 ビットレートで CBR エンコードされた 720p ビデオ、および 2 秒の GOP。
+ **HTML5 を介した再生 (IE または Chrome と Safari)**します。 このプリセットは、次のような特徴を持つ単一の MP4 ファイルを作成します。AAC を使用して 128 kbps で CBR エンコードされた 44.1 kHz 16 ビット/サンプルのステレオ オーディオ、および H.264 Main Profile を使用して 4500 kbps で CBR エンコードされた 720p ビデオ。
+ **IOS デバイスおよび Pc/mac での再生**します。 このプリセットは、スムーズ ストリーミング資産 (上述) と同じ特徴を持つ資産を作成しますが、Apple HLS ストリームを iOS デバイスに配信するときに使用できる形式で作成します。

5. 次に、わかりやすい出力コンテンツ名を入力するか、既定値をそのまま使用します。 チェック ボタンをクリックすると、エンコード処理が開始され、進捗状況はポータルの下部で確認できます。
6. [OK] をクリックします。

エンコードが完了すると、[コンテンツ] ページにエンコード済みのファイルが表示されます。

エンコード ジョブの進行状況を表示するに切り替えて、 **ジョブ** ページです。

ファイル サイズの値がない更新される場合エンコードが完了した後、キーを押して、 **メタデータの同期** ] ボタンをクリックします。 これにより出力資産のファイル サイズがストレージの実際のファイル サイズと同期され、[コンテンツ] ページに表示される値が更新されます。

##<a id="encrypt"></a>方法: コンテンツを暗号化する


Media Services で AES キーまたは PlayReady DRM を使用して資産を暗号化するには、次の操作を実行します。

- メザニン (ソース) ファイルを一連のアダプティブ ビットレート MP4 ファイルまたはアダプティブ ビットレート スムーズ ストリーミング ファイルにエンコードする (エンコーディングの手順に示されて、 [エンコード](#encode) セクション)。
- コンテンツに配信するストリーミング エンドポイントの 1 つ以上のオンデマンド ストリーミング ユニットを取得します。 詳細については、次を参照してください。 [スケール、オンデマンド ストリーミングの予約ユニット方法](media-services-manage-origins.md#scale_streaming_endpoints/)します。
- 「既定の AES クリア キー サービス ポリシー」または「既定 PlayReady ライセンス サービス ポリシー」を構成します。 詳細については、次を参照してください。 [コンテンツ キー承認ポリシーを構成する](media-services-portal-configure-content-key-auth-policy.md)です。  


    When you are ready to enable encryption, press the **ENCRYPTION** button on the bottom of the **CONTENT** page.

    ![Encrypt][encrypt] 

    Once you enabled encryption, whenever a stream is requested by a player, Media Services uses the specified key to dynamically encrypt your content using AES or PlayReady encryption. To decrypt the stream, the player will request the key from the key delivery service. To decide whether or not the user is authorized to get the key, the service evaluates the authorization policies that you specified for the key.

関連トピック:

- [PlayReady DRM による保護](media-services-rest-deliver-streaming-content.md)
- [AES-128 キーによる保護](media-services-protect-with-aes128.md)

##<a id="publish"></a>方法: コンテンツを発行する

> [AZURE.SELECTOR]
- [.NET](media-services-deliver-streaming-content.md)
- [REST ()](media-services-rest-deliver-streaming-content.md)
- [ポータル](media-services-manage-content.md#publish)

###概要

ストリーミングや、コンテンツのダウンロードに使用できる URL を使用してユーザーを提供するには、は、まず、ロケーターを作成してアセットを「発行」する必要があります。 資産に含まれているファイルには、ロケーターを通じてアクセスできます。 Media Services では、2 種類のロケーターがサポートされています。OnDemandOrigin ロケーターはメディアのストリーミング (MPEG DASH、HLS、スムーズ ストリーミングなど) に、Access Signature (SAS) ロケーターはメディア ファイルのダウンロードに使用します。

Azure クラシック ポータルを使用して資産を発行すると、ロケーターが作成され、OnDemantOrigin ベースの URL (資産に .ism ファイルが含まれている場合) または SAS URL が提供されます。 

SAS URL には次の形式があります。

    {blob container name}/{asset name}/{file name}/{SAS signature}

ストリーミング URL には次の形式があり、スムーズ ストリーミング資産の再生に使用できます。

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

HLS ストリーミング URL を作成するには、(format=m3u8-aapl) を URL に追加します。

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

MPEG DASH ストリーミング URL を作成するには、(format=mpd-time-csf) を URL に追加します。

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)


ロケーターには有効期限があります。 ポータルを使用してアセットを発行すると、有効期限が 100 年のロケーターが作成されます。 

>[AZURE.NOTE] 2015 年 3 月前にロケーターを作成するポータルを使用して 2 つの年の有効期限日がロケーターは作成されます。  

ロケーターの有効期限を更新する [REST](http://msdn.microsoft.com/library/azure/hh974308.aspx#update_a_locator ) または [.NET](http://go.microsoft.com/fwlink/?LinkID=533259) Api です。 SAS ロケーターの有効期限を更新すると、URL が変更されることにご注意ください。 

###Publish

ポータルを使用して資産を発行するには、次の操作を行います。 

1. 資産を選択します。 
2. 次に発行ボタンをクリックします。 
    
 ![PublishedContent][publishedcontent]


## 方法: ポータルでコンテンツを再生する

 **Azure Classic Portal** あるコンテンツ プレーヤーを使用して、ビデオをテストできます。

目的のビデオをクリックし、 **再生** 、ポータルの下部にあるボタンをクリックします。 
 
いくつかの考慮事項が適用されます。

- ビデオが発行されたことを確認します。
-  **MEDIA SERVICES コンテンツ プレーヤー** 既定のストリーミング エンドポイントから再生します。 既定以外のストリーミング エンドポイントから再生する場合は、別のプレーヤーを使用します たとえば、 [Azure Media Services プレーヤー](http://amsplayer.azurewebsites.net/azuremediaplayer.html)します。

![AMSPlayer][AMSPlayer]

##Media Services のラーニング パス

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##フィードバックの提供

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]




<!-- Images -->
[portaloverview]: ./media/media-services-manage-content/media-services-content-page.png
[publishedcontent]: ./media/media-services-manage-content/media-services-upload-content-published.png
[uploadcontent]: ./media/media-services-manage-content/UploadContent.png
[status]: ./media/media-services-manage-content/Status.png
[encoder]: ./media/media-services-manage-content/EncoderDialog2.png
[branding]: ./media/branding-reporting.png
[contentpage]: ./media/media-services-manage-content/media-services-content-page.png
[process]: ./media/media-services-manage-content/media-services-process-video.png
[process2]: ./media/media-services-manage-content/media-services-process-video2.png
[encrypt]: ./media/media-services-manage-content/media-services-encrypt-content.png
[AMSPlayer]: ./media/media-services-manage-content/media-services-portal-player.png 


