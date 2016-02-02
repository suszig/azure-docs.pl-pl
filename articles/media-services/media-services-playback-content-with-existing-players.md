<properties 
    pageTitle="コンテンツの再生" 
    description="このトピックでは、コンテンツの再生に使用できる既存のプレーヤーのリストを示します。" 
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
    ms.date="11/03/2015"
    ms.author="juliako"/>



# 既存のプレーヤーによるコンテンツの再生

Azure Media Services は、スムーズ ストリーミング、HTTP ライブ ストリーミング、Mpeg-dash など、人気のある多くのストリーミング形式をサポートします。 このトピックでは、ストリームのテストに使用できる既存のプレーヤーを示します。
>[AZURE.NOTE]動的にパッケージ化または暗号化されたコンテンツを再生するには、コンテンツを配信する予定のストリーミング エンドポイントのストリーミング ユニットを必ず 1 つ以上取得するようにしてください。 ストリーミング ユニットのスケーリングについてを参照してください: [ストリーミング ユニットの規模変更方法](media-services-manage-origins.md#scale_streaming_endpoints)します。

### Azure クラシック ポータル Media Services コンテンツ プレーヤー

**Azure クラシック ポータル**には、ビデオのテストに使用できるコンテンツ プレーヤーが用意されています。

目的のビデオをクリックして (だかどうかを確認 [発行](media-services-manage-content.md#publish)) ] をクリックし、 **再生** 、ポータルの下部にあるボタンをクリックします。

いくつかの考慮事項が適用されます。

- **Media Services コンテンツ プレーヤー**を既定のストリーミング エンドポイントから再生します。 既定以外のストリーミング エンドポイントから再生する場合は、別のプレーヤーを使用します たとえば、 [Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html)します。


![AMSPlayer][amsplayer]

### Azure Media Player

使用 [Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html) を再生するための次の形式でコンテンツ (クリアまたはプロテクト)。

- スムーズ ストリーミング
- MPEG DASH
- HLS
- Progressive MP4


### Flash Player

#### トークンを使用した AES 暗号化

[http://aestoken.azurewebsites.net]("http://aestoken.azurewebsites.net)

### Silverlight Players

#### Monitoring

[http://smf.cloudapp.net/healthmonitor](http://smf.cloudapp.net/healthmonitor)

#### トークンを使用した PlayReady

[http://sltoken.azurewebsites.net](http://sltoken.azurewebsites.net)

### DASH Players

[http://dashplayer.azurewebsites.net](http://dashplayer.azurewebsites.net)

[http://dashif.org](http://dashif.org)

### その他

次を使用して HLS URL をテストすることもできます。

- **Safari** (iOS デバイス) または
- **3ivx HLS Player** (Windows)

## ビデオ プレーヤーの開発

独自のプレーヤーの開発方法については、を参照してください [ビデオ プレーヤーの開発](media-services-develop-video-players.md)。




## Media Services のラーニング パス

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## フィードバックの提供

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]



[amsplayer]: ./media/media-services-playback-content-with-existing-players/media-services-portal-player.png 

