<properties 
    pageTitle="ワークフロー デザイナーを使用して高度なエンコード ワークフローを作成する" 
    description="ワークフロー デザイナーを使用して高度なエンコード ワークフローを作成する方法について説明します。" 
    services="media-services" 
    documentationCenter="" 
    authors="juliako,johndeu,anilmur" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/16/2015"  
    ms.author="juliako"/>


#ワークフロー デザイナーを使用して高度なエンコード ワークフローを作成する

##概要

 **ワークフロー デザイナー** を設計および使用してエンコード用のカスタム ワークフローを構築するために使用する Windows デスクトップ ツールは、 **メディア エンコーダー プレミアム ワークフロー**します。
ワークフロー デザイナー ツールの機能を使用するには、設計およびで実行される複雑なワークフローを作成することができます **メディア エンコーダー プレミアム**します。  

ワークフローには、入力ソース ファイルのプロパティに基づいて顧客の意志決定ロジックとブランチを含めることができます。 
上書き可能なプロパティと動的な値を含むワークフローを作成すれば、最も複雑なエンコーディング タスクをクラウドで簡単に繰り返し、カスタマイズできます。

作成可能なワークフローの例は次のとおりです。

- 解決を求めてソース コンテンツを検査し、必要な出力トラックだけをエンコードする決定基準のワークフロー。  不注意でソース コンテンツをスケールアップし、生成される無駄なトラックをなくするので便利です。
- 複数の入力ファイルを使用し、キャプション、オーバーレイ、結合コンテンツをサポートできます。 

このツールは、変更のいずれかの場合にも使用できる、 [ワークフローを発行](media-services-workflow-designer.md#existing_workflows)します。 

>[AZURE.NOTE]ワークフロー デザイナー ツールのコピーを取得するには、mepd@microsoft.com にお問い合わせください。


作成されたワークフロー ファイルはアセットとしてアップロードした後、メディア ファイルのエンコードに使用できます。 エンコードする方法の詳細について **メディア エンコーダー プレミアム ワークフロー** を使用して **.NET**, を参照してください [高度なメディア エンコーダー プレミアム ワークフローでエンコード](media-services-encode-with-premium-workflow.md)します。

##<a id="existing_workflows"></a>既存のワークフローを変更する

既定値 [ワークフローを発行](media-services-workflow-designer.md#existing_workflows) デザイナー ツールを使用して変更できます。 ワークフロー ファイルは既定値を入手できる [ここ](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows)します。 フォルダーにはこれらのファイルの説明も含まれています。

以下のビデオでは、デザイナーの使用方法を示しています。

###Day 1 - 作業の開始

Day 1 のビデオの内容は次のとおりです。

- デザイナーの概要
- 基本的なワークフロー - "Hello World"
- Azure Media Services のストリーミングで使用する複数の出力 MP4 ファイルの作成

> [AZURE.VIDEO azure-premium-encoder-workflow-designer-training-videos-day-1]

###Day 2

Day 2 のビデオの内容は次のとおりです。

- さまざまなソース ファイル シナリオ - オーディオの処理
- 高度なロジックを含むワークフロー
- グラフ ステージ

> [AZURE.VIDEO azure-premium-encoder-workflow-designer-training-videos-day-2]

###Day 3

Day 3 のビデオの内容は次のとおりです。

- ワークフローと設計図の内部スクリプト
- 現在のエンコーダの制限事項
- Q & A
 
> [AZURE.VIDEO azure-premium-encoder-workflow-designer-training-videos-day-3]



##Media Services のラーニング パス

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##フィードバックの提供

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


サポートする、またはワークフロー デザイナー ツールでのカスタム ワークフローの作成について質問があるを場合は、mepd@microsoft.com で電子メールを送信してください。

##関連項目

[Azure プレミアム エンコーダー ワークフロー デザイナーのトレーニング ビデオ](http://johndeutscher.com/2015/07/06/azure-premium-encoder-workflow-designer-training-videos/)

