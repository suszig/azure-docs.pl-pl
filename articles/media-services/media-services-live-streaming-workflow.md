<properties 
    pageTitle="Azure Media Services を使用したライブ ストリーミング配信" 
    description="このトピックでは、ライブ ストリーミングに関連する主要なコンポーネントの概要を説明します。" 
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


#Azure Media Services を使用したライブ ストリーミング イベントの配信

##概要

ライブ ストリーミングを使用する場合は、通常、次のコンポーネントが関連しています。 

- イベントのブロードキャストに使用されるカメラ。
- カメラからの信号をライブ ストリーミング サービスに送信されるストリームに変換するライブ ビデオ エンコーダー。 
  
    必要に応じて、複数のライブ エンコーダー。 非常に高い可用性と高品質が要求される重要なライブ イベントでは、アクティブ/アクティブの冗長エンコーダーを使用して、データを失わないシームレスなフェールオーバーを実現することをお勧めします。
- 次の操作を実行できるライブ ストリーミング サービス。 
    - さまざまなライブ ストリーミング プロトコル (RTMP、スムーズ ストリーミングなど) を使用してライブ コンテンツを取り込む。 
    - ストリームをアダプティブ ビットレート ストリームにエンコードする。
    - ライブ ストリームをプレビューする。
    - 後でストリーミングするために、取り込んだコンテンツを保存する (ビデオ オン デマンド)。
    - コンテンツを一般的なストリーミング プロトコル (MPEG DASH、Smooth、HLS、HDS など) を使用して顧客に配信したり、再配布のための Content Delivery Network (CDN) に直接配信する。 
    
        
**Microsoft Azure Media Services** (AMS) は、取り込み、エンコード、プレビュー、保存、およびライブ ストリーミング コンテンツを配信する機能を提供します。

コンテンツを顧客に配信する場合、その目標は、異なるネットワーク条件におけるさまざまなデバイスに高品質のビデオを配信することにあります。 品質とネットワーク条件に対応するには、ライブ エンコーダーを使用して、ストリームをマルチビットレート (アダプティブ ビットレート) ビデオ ストリームにエンコードします。  さまざまなデバイスでのストリーミングに対応をするには、Media Services を使用して [動的パッケージング](media-services-dynamic-packaging-overview.md) 、ストリームをさまざまなプロトコルを動的に再パッケージ化します。 Media Services で配信がサポートされるアダプティブ ビットレート ストリーミング テクノロジは、HTTP ライブ ストリーミング (HLS)、Smooth Streaming、MPEG DASH、HDS (Adobe PrimeTime/Access のライセンスが必要) です。

Azure Media Services で **チャネル**, 、**プログラム**, 、および **Streamingendpoint** などのすべてのライブ ストリーミング機能取り込み、フォーマット、DVR、セキュリティ、スケーラビリティ、および冗長性を処理します。 

A **チャネル** ライブ ストリーミング コンテンツを処理するためのパイプラインを表します。 現時点では、チャネルは次の方法でライブ入力ストリームを受信できます。


- オンプレミスのライブ エンコーダーは、RTP (MPEG-TS)、RTMP、スムーズ ストリーミング (Fragmented MP4) のいずれかの形式で、シングル ビットレート ストリームを Media Services による Live Encoding が有効なチャネルに送信します。 次に、受信したシングル ビットレート ストリームのマルチ ビットレート (アダプティブ) ビデオ ストリームへのライブ エンコードがチャネルで実行されます。 Media Services は、要求に応じて、ストリームを顧客に配信します。

- 内部設置型のライブ エンコーダーがマルチビット レートを送信 **RTMP** または **スムーズ ストリーミング** (Fragmented MP4) をチャネルにします。 マルチ ビットレートのスムーズ ストリーミングを出力するライブ エンコーダーとして、Elemental、Envivio、Cisco を使用できます。  Adobe Flash Live、Telestream Wirecast、Tricaster トランスコーダーは、RTMP を出力するライブ エンコーダーです。 取り込んだストリームは、通過 **チャネル**s 追加の処理なし。 ライブ エンコーダーは、ライブ エンコードが有効になっていないチャネルにシングル ビットレート ストリームも送信できますが、これはお勧めしません。 Media Services は、要求に応じて、ストリームを顧客に配信します。


##Azure Media Services を使用してライブ エンコードの実行が有効なチャネルを操作する


次の図は、Media Services による Live Encoding の実行が有効なチャネルのライブ ストリーミング ワークフローに関連する AMS プラットフォームの主要な部分を示しています。  

![ライブ ワークフロー][live-overview1]

詳細については、次を参照してください。 [Azure Media Services によるライブ エンコードの実行を有効になっているチャネルを操作](media-services-manage-live-encoder-enabled-channels.md)します。 


##オンプレミスのエンコーダーからマルチ ビットレートのライブ ストリームを受信するチャネルを操作する


次の図は、ライブ ストリーミング ワークフローに関連する AMS プラットフォームの主要な部分を示しています。

![ライブ ワークフロー][live-overview2]

詳細については、次を参照してください [その受信マルチ チャネルを操作の内部設置型のエンコーダーからビットレートのライブ ストリーム。
](media-services-manage-channels-overview.md) 



##Media Services のラーニング パス

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##フィードバックの提供

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##関連トピック

[Media Services の概念](media-services-concepts.md)

[Azure Media Services の Fragmented MP4 ライブ インジェスト仕様](media-services-fmp4-live-ingest-overview.md)





[live-overview1]: ./media/media-services-live-streaming-workflow/media-services-live-streaming-new.png

[live-overview2]: ./media/media-services-live-streaming-workflow/media-services-live-streaming-current.png
 
