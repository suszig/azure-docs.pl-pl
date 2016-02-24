<properties 
    pageTitle="Azure オンデマンド メディア エンコーダーの概要と比較" 
    description="このトピックでは、Azure オンデマンド メディア エンコーダーの概要と比較を提供します。" 
    services="media-services" 
    documentationCenter="" 
    authors="juliako,anilmur" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/29/2015"  
    ms.author="juliako"/>

#Azure オンデマンド メディア エンコーダーの概要と比較

##エンコードの概要

Azure Media Services には、クラウド内のメディア エンコーディングに使用できる複数のオプションが用意されています。

Media Services を使い始める場合、コーデックとファイル形式の違いを理解することが重要です。
コーデックは圧縮/展開アルゴリズムを実装するソフトウェアで、ファイル形式は圧縮されたビデオを保持するコンテナーです。

Media Services には動的パッケージ化機能があり、アダプティブ ビットレート MP4 またはスムーズ ストリーミングでエンコードされたコンテンツを、Media Services でサポートされるストリーミング形式 (MPEG DASH、HLS、スムーズ ストリーミング、HDS) でそのまま配信することができます。つまり、これらのストリーミング形式に再度パッケージ化する必要がありません。

活用するために [動的パッケージング](media-services-dynamic-packaging-overview.md), 、以下を実行する必要があります。

- メザニン (ソース) ファイルを一連のアダプティブ ビットレート MP4 ファイルまたはアダプティブ ビットレート スムーズ ストリーミング ファイルにエンコードします (エンコードの手順は、このチュートリアルの後半で説明しています)。
- コンテンツに配信するストリーミング エンドポイントの 1 つ以上のオンデマンド ストリーミング ユニットを取得します。 詳細については、次を参照してください。 [スケール、オンデマンド ストリーミングの予約ユニット方法](media-services-manage-origins.md#scale_streaming_endpoints/)します。

Media Services は次のオンデマンド エンコーダーをサポートしてます。これらについてはこの記事で説明します。

- **メディア エンコーダー スタンダード**
- **Azure Media Encoder**
- **メディア エンコーダー Premium ワークフロー**

この記事には、オンデマンド メディア エンコーダーの簡単な説明と、詳しい情報を提供する記事のリンクが含まれています。 このトピックではまた、エンコーダーを比較します。

既定では、1 つの Media Services アカウントにつき、同時に 1 つのアクティブなエンコーディング タスクを実行できます。 エンコード ユニットを予約して、複数のエンコード タスク (購入したエンコード予約ユニットごとに 1 つ) を同時に実行できます。 詳細については、次を参照してください。 [エンコード ユニットの拡張](media-services-portal-encoding-units.md)します。

##メディア エンコーダー スタンダード

###概要

メディア エンコーダー スタンダードを使用することをお勧めします。 ただし、これは現在、Azure クラシック ポータルでは公開されていません。

Azure Media Encoder と比較すると、このエンコーダーではサポートされる入力または出力の形式とコーデックの数が多くなっています。 他には次のような利点があります。

- 入力ファイルの作成方法に対する許容範囲が広い
- Azure Media Encoder より H.264 コーデックの品質が優れている
- より新しく、より柔軟なパイプラインの上に構築される
- より堅牢で回復力が高い

###使用方法

[メディア エンコーダー スタンダードを使用したエンコード方法](media-services-dotnet-encode-with-media-encoder-standard.md)

###形式

[形式とコーデック](media-services-media-encoder-standard-formats.md)

###プリセット

メディア エンコーダー スタンダードが構成されている説明されているエンコーダー プリセットのいずれかを使用して [ここ](http://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409)します。

###入力メタデータと出力メタデータ

エンコーダーの入力のメタデータが説明されている [ここ](http://msdn.microsoft.com/library/azure/dn783120.aspx)します。

エンコーダーの出力のメタデータが説明されている [ここ](http://msdn.microsoft.com/library/azure/dn783217.aspx)します。

###サムネイル

サムネイルを生成する方法については、次を参照してください。 [メディア エンコーダー スタンダードを使用してサムネイルを生成する方法](media-services-dotnet-generate-thumbnail-with-mes.md)します。

###オーディオまたはビデオのオーバーレイ

現在、サポートされていません。

###関連項目

[Media Services ブログ](https://azure.microsoft.com/blog/2015/07/16/announcing-the-general-availability-of-media-encoder-standard/)
 
##Azure Media Encoder

###概要

Azure Media Encoder は Media Services がサポートするエンコーダーの 1 つです。 2015 年 7 月から始めてを使用する推奨は [メディア エンコーダー スタンダード](media-services-encode-asset.md#media_encoder_standard)します。

###使用方法

[Azure Media Encoder を使用したエンコード方法](media-services-dotnet-encode-asset.md)

###形式

[形式とコーデック](media-services-azure-media-encoder-formats.md)

###プリセット

Azure Media Encoder が説明されているエンコーダー プリセットのいずれかを使用して構成されている [ここ](https://msdn.microsoft.com/library/azure/dn619392.aspx)します。 取得することも、実際の Azure メディア エンコーダーのプリセット ファイル [ここ](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/Azure%20Media%20Encoder)します。

###入力メタデータと出力メタデータ

エンコーダーの入力のメタデータが説明されている [ここ](http://msdn.microsoft.com/library/azure/dn783120.aspx)します。

エンコーダーの出力のメタデータが説明されている [ここ](http://msdn.microsoft.com/library/azure/dn783217.aspx)します。

###サムネイル

[サムネイルの作成](https://msdn.microsoft.com/library/hh973624.aspx)

###オーディオまたはビデオのオーバーレイ

[オーバーレイを作成する](media-services-azure-media-customize-ame-presets.md#creating-overlays)です。

###命名規則

[出力ファイル名を変更する方法](media-services-azure-media-customize-ame-presets.md#controlling-azure-media-encoder-output-file-names)

###関連項目

[ドルビー デジタル プラスを使ったメディアのエンコード](media-services-encode-with-dolby-digital-plus.md)

##メディア エンコーダー Premium ワークフロー

###概要

[Azure Media Services のプレミアム エンコーディングの紹介 (ブログの投稿)](http://azure.microsoft.com/blog/2015/03/05/introducing-premium-encoding-in-azure-media-services)

###使用方法

メディア エンコーダー プレミアム ワークフロー は複雑なワークフローを使用して構成されています。 ワークフロー ファイルを作成しを使用して更新、 [ワークフロー デザイナー](media-services-workflow-designer.md) ツールです。

[Azure Media Services でプレミアム エンコーディングを使用する方法 (ブログの投稿](http://azure.microsoft.com/blog/2015/03/06/how-to-use-premium-encoding-in-azure-media-services)

##<a id="compare_encoders"></a>エンコーダーの比較

###<a id="billing"></a>各エンコーダーで使用される課金メーター

メディア プロセッサ名|適用される価格|メモ
---|---|---
**メディア エンコーダー スタンダード** |エンコーダー|エンコード タスクは、出力のサイズに応じて課金されます (gb)、指定された割合 [こちら] [1]「エンコーダー」列での資産です。
**Azure Media Encoder** |エンコーダー|エンコード タスクは、出力のサイズに応じて課金されます (gb)、指定された割合 [こちら] [1]「エンコーダー」列での資産です。
**メディア エンコーダー Premium ワークフロー** |プレミアム エンコーダー|エンコード タスクは、出力のサイズに応じて課金されます (gb)、指定された割合 [こちら] [1]「PREMIUM エンコーダー」列での資産です。


ここでのエンコード機能を比較 **メディア エンコーダー スタンダード**, 、**Azure Media Encoder**, 、および **メディア エンコーダー プレミアム ワークフロー**します。


###入力コンテナー/ファイル形式

入力コンテナー/ファイル形式|メディア エンコーダー スタンダード|Azure Media Encoder|メディア エンコーダー プレミアム ワークフロー
---|---|---|---
Adobe® Flash® F4V           |あり|なし     |あり
MXF/SMPTE 377M              |あり|制限あり|あり
GXF                         |あり|なし     |あり
MPEG-2 トランスポート ストリーム    |あり|はい    |あり
MPEG-2 プログラム ストリーム      |あり|はい    |あり
MPEG-4/MP4                  |あり|はい    |あり
Windows Media/ASF           |あり|はい    |あり
AVI (非圧縮 8-bit/10-bit)|あり|はい   |あり
3GPP/3GPP2                  |あり|はい    |いいえ
スムーズ ストリーミング ファイル形式 (PIFF 1.3)|あり|はい|いいえ
[Microsoft Digital Video Recording (DVR-MS)](https://msdn.microsoft.com/library/windows/desktop/dd692984)|あり|なし|いいえ
Matroska/WebM               |あり|なし|いいえ
QuickTime (.mov) |あり|なし|いいえ

###入力ビデオ コーデック

入力ビデオ コーデック|メディア エンコーダー スタンダード|Azure Media Encoder|メディア エンコーダー プレミアム ワークフロー
---|---|---|---
AVC 8-bit/10-bit、最大 4:2:2 (AVCIntra を含む)   |8 ビット 4:2:0 および 4:2:2|8 ビット 4:2:0 のみ|あり
Avid DNxHD (MXF)                                 |あり|なし|あり
DVCPro/DVCProHD (MXF)                            |あり|なし|あり
JPEG2000                                            |あり|なし|あり
MPEG-2 (XDCAM、XDCAM HD、XDCAM IMX、CableLabs®、D10 など、最大 422 プロファイルおよびハイ レベル)|最大 422 プロファイル|最大 422 プロファイル|あり
MPEG-1                                              |あり|はい|あり
Windows Media ビデオ/VC-1                            |あり|はい|あり
Canopus HQ/HQX                                      |いいえ|あり|いいえ
MPEG-4 Part 2                                       |あり|なし|いいえ
[Theora](https://en.wikipedia.org/wiki/Theora)      |あり|なし|いいえ
Apple ProRes 422    |あり|なし|いいえ
Apple ProRes 422 LT |あり|なし|いいえ
Apple ProRes 422 HQ |あり|なし|いいえ
Apple ProRes プロキシ|あり|なし|いいえ
Apple ProRes 4444 |あり|なし|いいえ
Apple ProRes 4444 XQ |あり|なし|いいえ

###入力オーディオ コーデック

入力オーディオ コーデック|メディア エンコーダー スタンダード|Azure Media Encoder|メディア エンコーダー プレミアム ワークフロー
---|---|---|---
AES (SMPTE 331M および 302M、AES3-2003)        |いいえ|いいえ|あり
Dolby® E                                    |いいえ|いいえ|あり
Dolby® Digital (AC3)                        |いいえ|はい|あり
Dolby® Digital Plus (E-AC3)                 |いいえ|いいえ|あり
AAC (AAC-LC、AAC-HE、AAC-HEv2。最大 5.1)|あり|はい|あり
MPEG Layer 2|あり|はい|あり
MP3 (MPEG-1 Audio Layer 3)|あり|はい|あり
Windows Media オーディオ|あり|はい|あり
WAV/PCM|あり|はい|あり
[FLAC](https://en.wikipedia.org/wiki/FLAC)</a>|あり|なし|いいえ
[傑作](https://en.wikipedia.org/wiki/Opus _(audio_format) |あり|なし|いいえ
[Vorbis](https://en.wikipedia.org/wiki/Vorbis)</a>|あり|なし|いいえ


###出力コンテナー/ファイル形式

出力コンテナー/ファイル形式|メディア エンコーダー スタンダード|Azure Media Encoder|メディア エンコーダー プレミアム ワークフロー
---|---|---|---
Adobe® Flash® F4V|いいえ|いいえ|あり
MXF (OP1a、XDCAM および AS02)|いいえ|いいえ|あり
DPP (AS11 を含む)|いいえ|いいえ|あり
GXF|いいえ|いいえ|あり
MPEG-4/MP4|あり|はい|あり
MPEG TS|あり|なし|あり
Windows Media/ASF|いいえ|はい|あり
AVI (非圧縮 8-bit/10-bit)|いいえ|いいえ|あり
スムーズ ストリーミング ファイル形式 (PIFF 1.3)|いいえ|はい|あり

###出力ビデオ コーデック

出力ビデオ コーデック|メディア エンコーダー スタンダード|Azure Media Encoder|メディア エンコーダー プレミアム ワークフロー
---|---|---|---
AVC (H.264、8-bit、最大ハイ プロファイル、レベル 5.2、4K Ultra HD、AVC Intra)|8 ビット 4:2:0 のみ|8 ビット 4:2:0 のみ (最大 1080p)|あり
Avid DNxHD (MXF)|いいえ|いいえ|あり
DVCPro/DVCProHD (MXF)|いいえ|いいえ|あり
MPEG-2 (XDCAM、XDCAM HD、XDCAM IMX、CableLabs®、D10 など、最大 422 プロファイルおよびハイ レベル)|いいえ|いいえ|あり
MPEG-1|いいえ|いいえ|あり
Windows Media ビデオ/VC-1|いいえ|はい|あり
JPEG サムネール作成|いいえ|はい|あり

###出力オーディオ コーデック

出力オーディオ コーデック|メディア エンコーダー スタンダード|Azure Media Encoder|メディア エンコーダー プレミアム ワークフロー
---|---|---|---
AES (SMPTE 331M および 302M、AES3-2003)|いいえ|いいえ|あり
Dolby® Digital (AC3)|いいえ|はい|あり
Dolby® Digital Plus (E-AC3) 最大 7.1|いいえ|最大 5.1|あり
AAC (AAC-LC、AAC-HE、AAC-HEv2。最大 5.1)|あり|はい|あり
MPEG Layer 2|いいえ|いいえ|あり
MP3 (MPEG-1 Audio Layer 3)|いいえ|いいえ|あり
Windows Media オーディオ|いいえ|はい|あり




##Media Services のラーニング パス

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##フィードバックの提供

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


##関連記事:

- [クォータと制限](media-services-quotas-and-limitations.md)

 
<!--Reference links in article-->
[1]: http://azure.microsoft.com/pricing/details/media-services/

