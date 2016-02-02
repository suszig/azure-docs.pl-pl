<properties 
    pageTitle="オンプレミスのエンコーダーからマルチ ビットレートのライブ ストリームを受信するチャネルを操作する" 
    description="このトピックでは、オンプレミスのエンコーダーからマルチビットレートのライブ ストリームを受信するチャネルの設定方法について説明します。次にストリームは、1 つ以上のストリーミング エンドポイントを介して、HLS、スムーズ ストリーミング、MPEG DASH、HDS のいずれかを使用してクライアントの再生アプリケーションに送信できます。" 
    services="media-services" 
    documentationCenter="" 
    authors="cenkdin,Juliako" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="ne" 
    ms.topic="article" 
    ms.date="12/15/2015"  
    ms.author="juliako"/>


# オンプレミスのエンコーダーからマルチ ビットレートのライブ ストリームを受信するチャネルを操作する

## 概要

Azure Media Services では、**チャネル**は、ライブ ストリーミング コンテンツを処理するためのパイプラインを表します。 **チャネル**は、次の方法でライブ入力ストリームを受信します。

- オンプレミスのライブ エンコーダーは、マルチ ビットレート **RTMP** または **スムーズ ストリーミング** (Fragmented MP4) をチャネルに送信します。 マルチ ビットレートのスムーズ ストリーミングを出力するライブ エンコーダーとして、Elemental、Envivio、Cisco を使用できます。 Adobe Flash Live、Telestream Wirecast、Tricaster トランスコーダーは、RTMP を出力するライブ エンコーダーです。 取り込んだストリームは、追加の処理なしで**チャネル**を通過します。 ライブ エンコーダーは、シングル ビットレート ストリームも送信できますが、これはお勧めしません。 Media Services は、要求に応じて、ストリームを顧客に配信します。


次の図は、オンプレミスのライブのエンコーダーを使用してマルチビットレートの RTMP や Fragmented MP4 (スムーズ ストリーミング) ストリームを出力するライブ ストリーミング ワークフローを表しています。

![ライブ ワークフロー][live-overview]

このトピックは、以下の内容を説明します。

- [一般的なライブ ストリーミング シナリオ](media-services-manage-channels-overview.md#scenario)
- [チャネルとその関連コンポーネントの説明](media-services-manage-channels-overview.md#channel)
- [考慮事項](media-services-manage-channels-overview.md#considerations)
- [ライブ ストリーミングに関連するタスク](media-services-manage-channels-overview.md#tasks)

## <a id="scenario"></a>一般的なライブ ストリーミング シナリオ

次の手順では、一般的なライブ ストリーミング アプリケーションの作成に関連するタスクを示します。

1. ビデオ カメラをコンピューターに接続します。 複数ビットレ0ートの RTMP や Fragmented MP4 (スムーズ ストリーミング) ストリームを出力するオンプレミスのライブ エンコーダーを起動して構成します。 詳細については、次を参照してください。 [Azure Media Services RTMP サポートおよびライブ エンコーダー](http://go.microsoft.com/fwlink/?LinkId=532824)します。

    この手順は、チャネルを作成した後でも実行できます。

1. チャネルを作成し、起動します。
1. チャネルの取り込み URL を取得します。

    取り込み URL は、ライブ エンコーダーがチャネルにストリームを送信する際に使用されます。
1. チャネルのプレビュー URL を取得します。

    この URL を使用して、チャネルがライブ ストリームを正常に受信できることを確認します。

3. プログラムを作成します。

    Azure クラシック ポータルを使用する場合、プログラムを作成すると資産も作成されます。

    .NET SDK または REST を使用する場合は、プログラムを作成するときに資産を作成し、その資産を使用するように指定する必要があります。
1. プログラムに関連付けられた資産を発行します。

    コンテンツをストリームするストリーミング エンドポイントに少なくとも 1 つのストリーミング予約ユニットがあることを確認します。
1. ストリーミングとアーカイブの開始を準備するときにプログラムを開始します。
2. 必要に応じて、ライブ エンコーダーは、広告の開始を信号通知できます。 広告が出力ストリームに挿入されます。
1. イベントのストリーミングとアーカイブを停止するときにプログラムを停止します。
1. プログラムを削除し、資産を削除 (オプション) します。

[ライブ ストリーミング タスク](media-services-manage-channels-overview.md#tasks) 」セクションを上記の作業を実現する方法を説明するトピックにリンクします。

## <a id="channel"></a>チャネルとその関連コンポーネントの説明

### <a id="channel_input"></a>チャネル入力 (取り込み) の構成

#### <a id="ingest_protocols"></a>取り込みストリーミング プロトコル

Media Services は、次のストリーミング プロトコルを使用したライブのフィードのインジェストをサポートしています。

- **マルチビットレートの Fragmented MP4**

- **マルチビットレートの RTMP**

    **RTMP** のインジェスト ストリーミング プロトコルが選択されている場合、そのチャンネルに 2 つのインジェスト (入力) エンドポイントが作成されます。

    **プライマリ URL**: チャネルのプライマリ RTMP インジェスト エンドポイントの完全修飾 URL を指定します。

    **セカンダリ URL**: チャネルのセカンダリ RTMP インジェスト エンドポイントの完全修飾 URL を指定します。

    特に次のようなシナリオで、インジェスト ストリームの持続性とフォールト トレランスや、エンコーダーのフェールオーバーとフォールト トレランスを向上させるには、セカンダリ URL を使用します。

    - 1 つのエンコーダーがプライマリとセカンダリの両方の URL に二重にプッシュする

        The main purpose of this is to provide more resiliency to network fluctuations and jitters. Some RTMP encoders do not handle network disconnects well. When a network disconnect happens, an encoder may stop encoding and will not send the buffered data when reconnect happens, this causes discontinuities and data lost. Network disconnects can happen because of a bad network or a maintenance on Azure side. Primary/secondary URLs reduce the network issues and also provide a controlled upgrade process. Each time a scheduled network disconnect happens, Media Services manages the primary and secondary disconnect and provides a delayed disconnect between the two which gives time for encoders to keep sending data and reconnect again. The order of the disconnects can be random, but there will be always a delay between primary/secondary or secondary/primary. In this scenario encoder is still the single point of failure.

    - 複数のエンコーダの各エンコーダーが専用のポイントにプッシュする

        This scenario provides both encoder and ingest redundancy. In this scenario encoder1 pushes to the primary URL and encoder2 pushes to secondary URL. When there is an encoder failure other encoder can still keep sending data. Data redundancy can still be maintained because Media Services does not disconnect primary and secondary at the same time. This scenario assumes encoders are time sync and provides exactly same data.  

    - 複数のエンコーダーがプライマリとセカンダリの両方の URL に二重にプッシュする

        In this scenario both encoders push data to both primary and secondary URLs. This provides the best reliability and fault tolerance as well as data redundancy. It can tolerate both encoder failures and also disconnects even if one encoder stops working. This scenario assumes encoders are time sync and provides exactly same data.  


RTMP ライブ エンコーダーについては、次を参照してください。 [Azure Media Services RTMP サポートおよびライブ エンコーダー](http://go.microsoft.com/fwlink/?LinkId=532824)します。

次の考慮事項が適用されます。

- 取り込みポイントにデータを送信するための十分な空きのインターネット接続があるかどうかを確認します。
- セカンダリの取り込み URL を使用する際は、追加の帯域幅が必要です。
- 受信するマルチビットレート ストリームには、最大 10 のビデオ品質レベル (層) と、最大 5 つのオーディオ トラックを含めることができます。
- ビデオ品質レベル (層) の最高の平均ビットレートは、10 Mbps 以下である必要があります。
- すべてのビデオとオーディオ ストリームの平均ビットレートの合計は、25 Mbps である必要があります。
- チャネルやチャネルに関連付けられたプログラムの実行中は、入力プロトコルを変更できません。 別のプロトコルが必要な場合は、入力プロトコルごとに別のチャネルを作成します。
- チャネルに単一ビットレートを取り込むことはできますが、ストリームはチャネルで処理されないため、クライアント アプリケーションも単一ビットレートのストリームを受信することになります (このオプションはお勧めしません)。

#### 取り込み URL (エンドポイント)

チャネルは、ライブ エンコーダーで指定した入力エンドポイント (取り込み URL) を提供するため、エンコーダーはご使用のチャネルにストリームをプッシュできます。

取り込み URL は、チャネルの作成時に取得できます。 これらの URL を取得するために、チャネルが**実行中**状態である必要はありません。 データをチャネルにプッシュする準備ができたら、チャネルを**実行中**状態にする必要があります。 チャネルがデータの取り込みを開始すると、プレビュー URL 経由でストリームをプレビューできます。

Fragmented MP4 (スムーズ ストリーミング) のライブ ストリームを、SSL 接続で取り込むこともできます。 SSL 経由で取り込むには、取り込み URL を HTTPS に更新する必要があります。 現時点では、SSL 経由で RTMP を取り込むことはできません。

#### <a id="keyframe_interval"></a>キーフレームの間隔

オンプレミスのライブ エンコーダーを使用してマルチビットレートのストリームを生成する場合、キーフレームの間隔は GOP 期間を指定します (その外部エンコーダーで使用)。 チャネルがこの受信ストリームを受信したら、スムーズ ストリーミング、DASH、HLS のいずれかの形式でライブ ストリームをクライアントの再生アプリケーションに配信できます。 ライブ ストリーミングの実行中、HLS は常に動的にパッケージ化されます。 既定では、Media Services は自動的に HLS セグメントのパッケージ率 (セグメントあたりのフラグメント) をキーフレームの間隔に基づいて計算します。これは、画像グループ (GOP) とも呼ばれ、ライブ エンコーダーから受信します。

次の表は、セグメントの期間の計算方法を示しています。

 キーフレームの間隔| HLS セグメントのパッケージ率 (FragmentsPerSegment)| 例
---|---|---
 3 秒以下| 3:1| KeyFrameInterval (または GOP) が 2 秒の場合、既定の HLS セグメントのパッケージ率は 3:1 になり、6 秒間の HLS セグメントが作成されます。
 3 ~ 5 秒| 2:1| KeyFrameInterval (または GOP) が 4 秒の場合、既定の HLS セグメントのパッケージ率は 2:1 になり、8 秒間の HLS セグメントが作成されます。
 6 秒以上| 1:1| KeyFrameInterval (または GOP) が 6 秒の場合、既定の HLS セグメントのパッケージ率は 1:1 になり、6 秒間の HLS セグメントが作成されます。


セグメントあたりのフラグメント率は、チャネルの出力を構成して ChannelOutputHls の FragmentsPerSegment を設定すると変更できます。

また、キーフレームの間隔の値は、ChanneInput の KeyFrameInterval プロパティを設定すると変更できます。

KeyFrameInterval を明示的に設定すると、HLS セグメントのパッケージ率 FragmentsPerSegment は、上記の規則を使用して計算されます。

KeyFrameInterval と FragmentsPerSegment の両方を明示的に設定すると、Media Services は設定された値を使用します。


#### 許可された IP アドレス

このチャネルにビデオを発行できる IP アドレスを定義できます。 許可された IP アドレスは、1 つの IP アドレス (例: '10.0.0.1')、IP アドレスと CIDR のサブネットを使用した IP 範囲 (例: ‘10.0.0.1/22’)、または IP アドレスとピリオド区切りのサブネット マスクを使用した IP 範囲 (例: ‘10.0.0.1(255.255.252.0)’) のいずれかの形式で指定できます。

IP アドレスが指定されておらず、規則の定義もない場合は、どの IP アドレスも許可されません。 すべての IP アドレスを許可するには、規則を作成し、0.0.0.0/0 に設定します。

### チャネルのプレビュー

#### プレビュー URL

ストリームをさらに処理して配信する前にプレビューできるプレビュー エンドポイント (プレビュー URL) がチャネルから提供されます。

プレビュー URL は、チャネルの作成時に取得できます。 URL を取得するために、チャネルが**実行中**状態である必要はありません。

チャネルがデータの取り込みを開始した後、ストリームをプレビューできます。

現在、プレビュー ストリームを配信できるのは指定された入力タイプにかかわらず、Fragmented MP4 (スムーズ ストリーミング) 形式のみです。 使用することができます、 [http://smf.cloudapp.net/healthmonitor](http://smf.cloudapp.net/healthmonitor) player スムーズ ストリーミングのテストをします。 また、Azure クラシック ポータルでホストされているプレーヤーを使用してストリームを表示することもできます。


#### 許可された IP アドレス

プレビューのエンドポイントへの接続を許可する IP アドレスを定義できます。 IP アドレスを指定しない場合、すべての IP アドレスが許可されます。 許可された IP アドレスは、1 つの IP アドレス (例: '10.0.0.1')、IP アドレスと CIDR のサブネットを使用した IP 範囲 (例: ‘10.0.0.1/22’)、または IP アドレスとピリオド区切りのサブネット マスクを使用した IP 範囲 (例: ‘10.0.0.1(255.255.252.0)’) のいずれかの形式で指定できます。

### チャネルの出力

詳細については、次を参照してください。、 [キーフレームの間隔](#keyframe_interval) セクションです。


### チャネルのプログラム

チャネルは、ライブ ストリームのセグメントの発行と保存を管理できるプログラムに関連付けられています。 プログラムはチャネルによって管理されます。 チャネルとプログラムの関係は、従来のメディアとよく似ています。チャネルが絶えずコンテンツのストリームを配信するのに対し、プログラムは、そのチャネル上で決まった時間に生じるイベントです。

プログラムの**アーカイブ ウィンドウ**の長さを設定することで、録画されたコンテンツの保持時間を指定できます。 この値は、最小 5 分から最大 25 時間までの範囲で設定できます。 クライアントが現在のライブ位置からさかのぼって検索できる最長時間も、Archive Window (アーカイブ ウィンドウ)の長さによって決まります。 プログラムの放送は、指定された期間継続しますが、ArchiveWindowLength を過ぎたコンテンツは絶えず破棄されていきます。 さらに、このプロパティの値によって、クライアント マニフェストが肥大した場合の最大サイズも決まります。

各プログラムは、ストリーミングされたコンテンツを格納する資産に関連付けられます。 資産は Azure ストレージ アカウント内の BLOB コンテナーにマップされ、資産内のファイルは BLOB としてそのコンテナーに格納されます。 プログラムを発行して顧客がストリームを表示できるようにするには、関連付けられた資産の OnDemand ロケーターを作成する必要があります。 このロケーターを作成すると、ストリーミング URL を構築してクライアントに提供できます。

チャネルは、最大 3 つの同時実行プログラムをサポートするため、同じ受信ストリームのアーカイブを複数作成できます。 これにより、1 つのイベントのさまざまな部分を必要に応じて発行したりアーカイブしたりできます。 たとえば、ビジネス要件によって 1 つのプログラムの 6 時間分をアーカイブする一方、最後の 10 分間のみをブロードキャストする場合があります。 これを実現するには、2 つの同時実行プログラムを作成する必要があります。 1 つのプログラムは 6 時間分のイベントをアーカイブするように設定しますが、プログラムは発行されません。 もう 1 つのプログラムは 10 分間のアーカイブを行うように設定します。このプログラムは発行されます。

新しいイベントには既存のプログラムを再使用できません。 代わりに、「ライブ ストリーミング アプリケーションのプログラミング」セクションにあるように、各イベントで新しいプログラムを作成し、起動します。

ストリーミングとアーカイブの開始を準備するときにプログラムを開始します。 イベントのストリーミングとアーカイブを停止するときにプログラムを停止します。

アーカイブ済みコンテンツを削除するには、プログラムを停止して削除し、次に関連付けられた資産を削除します。 プログラムが資産を使用している場合は資産を削除できません。まずプログラムを削除する必要があります。

プログラムを停止して削除した後も、資産を削除していなければアーカイブ済みコンテンツをオンデマンドでのビデオとしてストリームできます。

アーカイブ済みコンテンツを保持したいが、ストリーミングには使用したくない場合は、ストリーミング ロケーターを削除します。

## <a id="states"></a>チャネルの状態と課金モードの状態をマップする方法

現在のチャネルの状態。 指定できる値は、次のとおりです。

- **停止済み**。 これは、チャネル作成後の初期状態です。 この状態で、チャネルのプロパティを更新できますが、ストリーミングは許可されていません。
- **開始中**。 チャネルを開始しています。 この状態の場合、更新やストリーミングはできません。 エラーが発生した場合は、チャネルは Stopped 状態に戻ります。
- **実行中**。 チャネルは、ライブ ストリームを処理できます。
- **停止中**。 チャネルを停止しています。 この状態の場合、更新やストリーミングはできません。
- **削除中**。 チャネルが削除されました。 この状態の場合、更新やストリーミングはできません。

次の表は、チャネルの状態と課金モードとの対応を示しています。

 チャネルの状態| ポータル UI インジケーター| 課金対象
---|---|---|---
 開始中| 開始中| いいえ (遷移状態)
 実行中| 準備完了 (実行中プログラムなし)<p>または<p>ストリーミング (実行中のプログラムを少なくとも 1 つ)| はい
 停止中| 停止中| いいえ (遷移状態)
 停止済み| 停止済み| いいえ

## <a id="cc_and_ads"></a>クローズド キャプションと広告の挿入

次の表は、サポートされているクローズド キャプションや広告挿入の標準を示しています。

 Standard| メモ
---|---
 CEA-708 と EIA-608 (708/608)| CEA 708 と EIA 608 は、米国とカナダにおける字幕標準を閉じています。<p><p>現時点では、キャプションがサポートされるのみでエンコードされた入力ストリーム内で。608 または 708 キャプションを、Media Services に送信されるエンコードされたストリームに挿入できるライブ Media Encoder を使用する必要があります。Media Services はキャプションが挿入されたコンテンツをユーザーに配信します。
 ismt (スムーズ ストリーミング テキスト トラック) 内の TTML| Media Services の動的パッケージ機能では、クライアントが MPEG DASH、HLS、スムーズ ストリーミングのいずれかの形式でコンテンツをストリームできます。ただし、.ismt (スムーズ ストリーミング テキスト トラック) 内のキャプションのある Fragment MP4 を取り込む場合は、ストリーミングを配信できるのはスムーズ ストリーミング クライアントのみになります。
 SCTE-35| 広告の挿入のキューには、デジタル信号システムが使用されます。ダウンストリームの受信側は信号を使用して、割り当てられた時間のストリームに広告をスプライスします。SCTE 35 は、入力ストリームのスパース トラックとして送信する必要があります。<p><p>現在、唯一サポートされていること信号の伝送が入力ストリーム形式は fragmented MP4 (スムーズ ストリーミング)。サポートされている出力形式も、スムーズ ストリーミングのみです。


## <a id="Considerations"></a>考慮事項

オンプレミスのライブ エンコーダーを使用してマルチビットレートのストリームをチャネルに送信する際は、次の考慮事項を適用します。

- 取り込みポイントにデータを送信するための十分な空きのインターネット接続があるかどうかを確認します。
- 受信するマルチビットレート ストリームには、最大 10 のビデオ品質レベル (10 層) と、最大 5 つのオーディオ トラックを含めることができます。
- ビデオ品質レベル (層) の最高の平均ビットレートは、10 Mbps 以下である必要があります。
- すべてのビデオとオーディオ ストリームの平均ビットレートの合計は、25 Mbps である必要があります。
- チャネルやチャネルに関連付けられたプログラムの実行中は、入力プロトコルを変更できません。 別のプロトコルが必要な場合は、入力プロトコルごとに別のチャネルを作成します。


チャネルと関連コンポーネントの作業に関するその他の考慮事項は次のとおりです。

- ライブ エンコーダーを再構成する際は、毎回チャネルで **Reset** メソッドを呼び出します。 チャネルをリセットする前に、プログラムを停止する必要があります。 チャネルをリセットしたら、プログラムを再起動します。
- チャネルを停止できるのは実行中の状態で、チャネルのすべてのプログラムが停止しているときのみです。
- 既定では、Media Services アカウントに追加できるチャネル数は 5 つのみです。 詳細については、次を参照してください。 [クォータと制限](media-services-quotas-and-limitations.md)します。
- チャネルやチャネルに関連付けられたプログラムの実行中は、入力プロトコルを変更できません。 別のプロトコルが必要な場合は、入力プロトコルごとに別のチャネルを作成します。
- チャネルが**実行中**状態のときにのみ課金されます。 詳細についてを参照してください [この](media-services-manage-channels-overview.md#states) セクションです。

## オンプレミスのエンコーダーからマルチ ビットレートのライブ ストリームを受信するチャネルを作成する方法

内部設置型のライブ エンコーダーの詳細については、次を参照してください。 [サード パーティ ライブ エンコーダーを使用して Azure Media Services で](https://azure.microsoft.com/blog/azure-media-services-rtmp-support-and-live-encoders/)します。

チャネルとプログラムの作成および管理方法を表示する**ポータル**、**.NET**、**REST API** を選択します。

[AZURE.INCLUDE [media-services-selector-manage-channels](../../includes/media-services-selector-manage-channels.md)]



## Media Services のラーニング パス

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## フィードバックの提供

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]



## 関連トピック

[Azure Media Services の Fragmented MP4 ライブ インジェスト仕様](media-services-fmp4-live-ingest-overview.md)

[Azure Media Services によるライブ ストリーミング イベントを配信します。](media-services-live-streaming-workflow.md)

[Media Services の概念](media-services-concepts.md)


[live-overview]: ./media/media-services-manage-channels-overview/media-services-live-streaming-current.png 

