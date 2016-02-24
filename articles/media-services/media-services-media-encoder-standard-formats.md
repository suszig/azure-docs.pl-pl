<properties 
    pageTitle="Media Encoder Standard の形式とコーデック" 
    description="このトピックでは、Azure Media Encoder Standard の形式とコーデックの概要を示します。" 
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
    ms.date="10/15/2015"  
    ms.author="juliako"/>

#Media Encoder Standard の形式とコーデック


このドキュメントでは、Media Encoder Standard で使用できる一般的なインポートおよびエクスポート ファイル形式の一覧を示しています。


##入力コンテナー/ファイル形式

ファイル形式 (ファイル拡張子) |サポートされています。
---|---|---|---
(H.264 および AAC コーデックでの) FLV (.flv) |[はい] 
MXF (.mxf) |[はい] 
GXF (.gxf) |[はい] 
MPEG2 PS MPEG2 TS、3 gp (.ts、.ps、.3gp、.3gpp、.mpg) |[はい] 
Windows Media ビデオ (WMV)/ASF (.wmv、.asf) |[はい] 
AVI (非圧縮 8 ビット/10 ビット) (.avi) |[はい] 
MP4 (.mp4、.m4a、.m4v)/ISMV (.isma、.ismv) |[はい] 
[Microsoft Digital ビデオ recording (dvr-ms)](https://msdn.microsoft.com/library/windows/desktop/dd692984) (dvr-ms) |[はい] 
Matroska/WebM (.mkv) |[はい] 
WAVE/WAV (.wav) |[はい] 
QuickTime (.mov) |[はい]
 
###入力コンテナーのオーディオ形式 

Media Encoder Standard の入力コンテナーは次のオーディオ形式に対応しています。

- オーディオ トラックにインターリーブ ステレオまたは 5.1 サンプルが含まれる MXF、GXF、QuickTime ファイル。

または

- オーディオが個別 PCM トラックとして送信されるが、(ステレオまたは 5.1 への) チャネル マッピングをファイル メタデータから推測できる MXF、GXF、QuickTime ファイル。

明示的/ユーザー指定のチャネル マッピングが近い将来にサポートされる予定です。


##入力ビデオ コーデック

入力ビデオ Codecs|サポートされています。
---|---|---|---
AVC 8 ビット/10 ビット、最大 4:2:2 など AVCIntra | 8 ビット 4:2:0 および 4:2:2 
Avid DNxHD (MXF) |[はい] 
Dvcpro/dvcprohd (MXF) |[はい] 
(AVI ファイル) 内のデジタル ビデオ (DV) |[はい]
JPEG 2000                                           |[はい] 
Mpeg-2 (XDCAM、XDCAM HD、XDCAM IMX、CableLabs ®、D10 など最大 422 プロファイルおよびハイ レベル;) |最大 422 プロファイル 
MPEG-1                                              |[はい] 
VC-1/WMV9                                           |[はい] 
Canopus HQ/HQX |なし 
MPEG-4 Part 2                                       |[はい] 
[Theora](https://en.wikipedia.org/wiki/Theora)      |[はい] 
Yuv420 非圧縮または中間 |[はい]
Apple ProRes 422 |[はい]
Apple ProRes 422 LT |[はい]
Apple ProRes 422 HQ |[はい]
Apple ProRes Proxy|[はい]
Apple ProRes 4444 |[はい]
Apple ProRes 4444 XQ |[はい]



##入力オーディオ コーデック

入力オーディオ Codecs|サポートされています。
---|---|---|---
AAC (AAC-LC、Aac-he、aac-hev2 最大 5.1) |[はい] 
MPEG レイヤー 2 |[はい] 
MP3 (mpeg-1 オーディオ レイヤー 3) |[はい] 
Windows Media Audio|[はい] 
WAV/PCM|[はい] 
[FLAC](https://en.wikipedia.org/wiki/FLAC)</a>|[はい] 
[傑作](https://en.wikipedia.org/wiki/Opus _(audio_format) |[はい] 
[Vorbis](https://en.wikipedia.org/wiki/Vorbis)</a>|[はい] 
AMR (アダプティブ マルチ レート) |[はい]
AES (SMPTE 331m および 302 M、aes3-2003) |なし 
Dolby® E                                    |なし 
Dolby ® Digital (AC3) |なし 
Dolby ® Digital Plus (e-ac3) |なし 


##出力形式とコーデック

次の表では、エクスポートでサポートされるコーデックおよびファイル形式の一覧を示します。


ファイル形式|ビデオ コーデック|オーディオ コーデック
---|---|---
MP4 <br/><br/>(マルチビットレートの MP4 コンテナーを含む) |H.264 (High、Main、Baseline Profile)|AAC-LC、HE-AAC v1、HE-AAC v2 
MPEG2-TS |H.264 (High、Main、Baseline Profile)|AAC-LC、HE-AAC v1、HE-AAC v2 



##Media Services のラーニング パス

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##フィードバックの提供

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##関連項目

[Azure Media Services を使用してオンデマンド コンテンツをエンコードする](media-services-encode-asset.md)

[メディア エンコーダー スタンダードを使用したエンコード方法](media-services-dotnet-encode-with-media-encoder-standard.md)

