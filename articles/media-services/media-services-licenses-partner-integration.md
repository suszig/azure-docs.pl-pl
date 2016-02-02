<properties 
    pageTitle="パートナーを使用して Azure Media Services に Widevine ライセンスを配信する" 
    description="この記事では、PlayReady と Widevine DRM の両方を使用して AMS で動的に暗号化されたストリームを、Azure Media Services (AMS) を使用して配信する方法について説明します。PlayReady ライセンスは Media Services PlayReady サーバーから取得し、Widevine ライセンスは castLabs ライセンス サーバーから取得します。" 
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
    ms.date="10/07/2015"  
    ms.author="juliako"/>


# パートナーを使用して Azure Media Services に Widevine ライセンスを配信する

## 概要

Microsoft Azure Media Services を使用すると、Widevine DRM で保護された MPEG-DASH を配信できます。Widevine DRM は、共通暗号化 (CENC) 仕様に従って暗号化されています。

Media Services .NET SDK バージョン 3.5.2 以降、Media Services を使用すると Widevine ライセンス テンプレートを構成し、Widevine ライセンスを取得できます。 Widevine ライセンスを配信するため、以下の AMS パートナーを使用することもできます。 [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/), 、[EZDRM](http://ezdrm.com/), 、[castLabs](http://castlabs.com/company/partners/azure/)します。

## castLabs

使用する [castLabs](http://castlabs.com/company/partners/azure/) Widevine ライセンスを配信します。 詳細については、を参照してください [Azure Media Services に DRM を配信する castLabs を使用してライセンス](media-services-castlabs-integration.md)。

## Axinom

使用する [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/) Widevine ライセンスを配信します。 詳細については、を参照してください [Azure Media Services にライセンスを配信 DRM を使用して Axinom](media-services-axinom-integration.md)。


## Media Services のラーニング パス

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## フィードバックの提供

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## 関連項目

[PlayReady と Widevine の動的共通暗号化を使用します。](media-services-protect-with-drm.md)

[Mingfei のブログ](https://azure.microsoft.com/blog/azure-media-services-adds-google-widevine-packaging-for-delivering-multi-drm-stream/)






