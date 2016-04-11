<properties
    pageTitle="動的パッケージの概要"
    description="このトピックでは、動的パッケージの概要について説明します。"
    authors="Juliako"
    manager="dwrede"
    editor=""
    services="media-services"
    documentationCenter=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/05/2015"
    ms.author="juliako"/>


#動的パッケージ

##概要

Microsoft Azure Media Services を使用して、多数のメディア ソース ファイルの形式、メディア ストリーミング形式、およびコンテンツ保護形式をさまざまなクライアント テクノロジ (iOS、XBOX、Silverlight、Windows 8 など) に配信できます。 これらのクライアントは異なるプロトコルを認識します。たとえば、iOS は HTTP Live Streaming (HLS) V4 形式が必要で、Silverlight および Xbox はスムーズ ストリーミングが必要です。 MPEG DASH、HLS またはスムーズ ストリーミングを認識するクライアントに提供する、一連のアダプティブ ビットレート (マルチビット レート) MP4 (ISO Base Media 14496-12) ファイルまたは一連のアダプティブ ビットレート スムーズ ストリーミング ファイルがある場合、Media Services の動的パッケージを活用する必要があります。

動的パッケージを使用すれば、一連のアダプティブ ビットレート MP4 ファイルまたはアダプティブ ビットレート スムーズ ストリーミング ファイルを含むアセットを作成するだけで済みます。 そうすれば、マニフェストまたはフラグメント要求で指定された形式に基づき、オンデマンド ストリーミング サーバーによって、ユーザーが選択したプロトコルでストリームを受信するようになります。 その結果、保存と課金の対象となるのは、単一のストレージ形式のファイルのみです。Media Services がクライアントからの要求に応じて、適切な応答を構築して返します。

次の図は、従来のエンコードおよび静的パッケージングのワークフローを示しています。

![静的なエンコード](./media/media-services-dynamic-packaging-overview/media-services-static-packaging.png)

次の図は、動的パッケージのワークフローを示しています。

![動的なエンコード](./media/media-services-dynamic-packaging-overview/media-services-dynamic-packaging.png)


>[AZURE.NOTE]動的パッケージングを利用するには、最初予定の配信コンテンツのストリーミング エンドポイントの 1 つ以上のオンデマンド ストリーミング ユニットを取得する必要があります。 詳細については、次を参照してください。 [Media services の規模変更方法](media-services-manage-origins.md#scale_streaming_endpoints)します。

##一般的なシナリオ

1. 入力ファイル (中間ファイル) をアセットにアップロードします。 H.264、MP4、または WMV (サポートされている形式の一覧については、次を参照してください。 [Media Services Encoder でサポートされる形式](media-services-azure-media-encoder-formats))。

1. Mezzanine ファイルを H.264 MP4 アダプティブ ビットレート セットにエンコードします。

1. オンデマンド ロケーターを作成することによって、アダプティブ ビットレート MP4 セットを含むアセットを発行します。

1. コンテンツにアクセスしてストリーミングするストリーミング URL を構築します。

>[AZURE.NOTE]詳細については、動的パッケージでサポートされていないすべての MP4 ファイル形式を参照してください [動的パッケージで形式がサポートされていない](media-services-dynamic-packaging-overview.md#unsupported_formats)します。

##動的ストリーミング用のアセットの準備

動的ストリーミング用にアセットを準備するには、2 つのオプションがあります。

- マスター ファイルをアップロードし、Azure Media Encoder を使用して H.264 MP4 アダプティブ ビットレートのセットを生成します。
- 既存のアダプティブ ビットレート セットをアップロードし、Media Packager を使用してそれらを検証します。

###マスター ファイルをアップロードし、Azure Media Encoder を使用して H.264 MP4 アダプティブ ビットレートのセットを生成します。

アセットをアップロードしてエンコードする方法については、次の記事を参照してください。


使用して、ファイルをアップロード **Azure Classic Portal**, 、**.NET** または **REST API**します。

[AZURE.INCLUDE [media-services-selector-upload-files](../../includes/media-services-selector-upload-files.md)]

使用したエンコード **Azure Media Encoder** を使用して **Azure Classic Portal**, 、**.NET**, 、または **REST API**します。

[AZURE.INCLUDE [media-services-selector-encode](../../includes/media-services-selector-encode.md)]


###既存のアダプティブ ビットレート セットをアップロードし、Media Packager を使用してそれらを検証します。

Media Services Encoder でエンコードされていないアダプティブ ビットレート MP4 ファイルのセットをアップロードする場合、通常はこのタスクを実行します。  [検証アダプティブ ビットレート mp4 エンコーダーでエンコードされた外部](https://msdn.microsoft.com/library/azure/dn750842.aspx) トピックは、このタスクを実現する方法を示します。

##コンテンツをクライアントにストリーミングする

アダプティブ ビットレート セットを作成したら、オンデマンド ロケーターを作成して、スムーズ ストリーミング、MPEG DASH、HLS、および HDS (Adobe Primetime/Access のライセンス所有者にのみ使用) のためのストリーミング URL を構成できます。

ロケーターを作成し、動的パッケージを使用してコンテンツをストリームする方法については、次のトピックを参照してください。

[顧客の概要にコンテンツを配信する](media-services-deliver-content-overview.md)です。

アセット配信ポリシーを使用して構成する **.NET** または **REST API**します。

[AZURE.INCLUDE [media-services-selector-asset-delivery-policy](../../includes/media-services-selector-asset-delivery-policy.md)]

アセットを公開します (ロケータを作成) を使用して **Azure Classic Portal** または **.NET**します。

[AZURE.INCLUDE [media-services-selector-publish](../../includes/media-services-selector-publish.md)]


##<a id="unsupported_formats"></a>動的パッケージでサポートされない形式

動的パッケージでは、次のソース ファイルの形式はサポートされません。

- Dolby Digital Plus の MP4 ファイル。
- Dolby Digital Plus のスムーズ ファイル。

##Media Services のラーニング パス

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##フィードバックの提供

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


