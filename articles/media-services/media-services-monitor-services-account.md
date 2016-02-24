<properties 
    pageTitle="メディア サービス アカウントの監視" 
    description="Azure における Media Services アカウントの監視の構成方法について説明します。" 
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
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



#<a id="monitormediaservicesaccount"></a>Media Services アカウントの監視方法

Azure メディア サービスのダッシュボードには、使用状況のメトリックおよびアカウント情報が表示され、それをメディア サービス アカウントの管理に利用できます。

監視できるのは、キューに格納されたエンコード ジョブの数、失敗したエンコード タスク、エンコーダーからの入出力データによって表されるアクティブなエンコード ジョブ、およびメディア サービス アカウントに関連付けられた BLOB ストレージの使用状況です。 さらに、顧客に対してコンテンツをストリーミング配信している場合は、さまざまなストリーミング メトリックも取得できます。 データの監視期間は、過去 6 時間、24 時間、または 7 日間から選択できます。
 
>[AZURE.NOTE] 追加のコストは、Azure クラシック ポータルでストレージ データの監視に関連付けられます。 詳細については、次を参照してください。 [Storage Analytics と課金](http://go.microsoft.com/fwlink/?LinkId=256667)します。

##<a id="configuremonitoring"></a>方法: メディア サービス アカウントの監視

1.  [Azure Classic Portal](http://go.microsoft.com/fwlink/?LinkID=256666), 、] をクリックして **Media Services**, 、ダッシュ ボードを Media Services アカウント名をクリックします。 

    ![MediaServices_Dashboard][ダッシュ ボード]

2. エンコード ジョブまたはデータを監視するには、Media Services に対するエンコード ジョブの送信を開始するか、Azure メディア オンデマンド ストリーミングを使用して顧客に対するコンテンツのストリーミング配信を開始します。 約 1 時間後に、ダッシュボードに監視データが表示されるようになります。

##<a id="configuringstorage"></a>方法: BLOB ストレージの使用状況の監視 (省略可能)
1. クリックして、 **ストレージ アカウント** 下にある名前、 **概要** セクションです。
2. [ストレージ アカウント] ページで、をクリックして、 **の構成] ページ** リンク、および下へスクロールして、 **監視** 、Blob、テーブル、およびキュー サービスを次に示す設定。

    >[AZURE.NOTE] Blob は、Media Services で唯一サポートされているストレージの種類です。

    ![StorageOptions][storage_options_scoped]

3.  **監視**, 、Blob の監視レベルおよびデータ保有ポリシーを設定します。

-  監視レベルを設定するには、以下のいずれかを選択します。

      **最小** -Blob、テーブル、およびキュー サービスに対して集計される、受信/送信、可用性、待機時間、および成功のパーセンテージなどのメトリックを収集します。

      **詳細な** - 最小レベルのメトリックでは、Azure Storage サービス API のストレージ操作ごとのメトリックの同じ収集に加えてします。 詳細メトリックにより、アプリケーションの操作中に発生する問題を詳しく分析できます。 

      **オフ** の監視をオフにします。 既存の監視データは、保有期間が経過するまで残ります。

- データの保持ポリシーを設定する **保有期間 (日)**, 、データを 1 から 365 日を保持する日数の番号を入力します。 保有ポリシーを設定しない場合は、「0」(ゼロ) を入力します。 保有ポリシーがない場合、監視データを削除する責任はユーザーが負います。 使用しない古い分析データがコストをかけずに自動的に削除されるように、アカウントのストレージ分析データをどの程度の期間保持するかに基づいて、データ保有ポリシーを設定することをお勧めします。

4. 監視の構成が完了したら、クリックして **保存**します。
Media Services のメトリックと同様に、約 1 時間後に、ダッシュボードに監視データが表示されるようになります。
メトリックは、ストレージ アカウントの $MetricsTransactionsBlob、$MetricsTransactionsTable、$MetricsTransactionsQueue、$MetricsCapacityBlob という名前の 4 つのテーブルに保存されます。 詳細については、次を参照してください。 [Storage Analytics Metrics](http://go.microsoft.com/fwlink/?LinkId=256668)します。



##Media Services のラーニング パス

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##フィードバックの提供

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
ストリーミングのワークフローを必要に応じて] (http://azure.microsoft.com/documentation/learning-paths/media-services-streaming-on-demand/)


<!-- Images -->
[dashboard]: ./media/media-services-monitor-services-account/media-services-dashboard.png
[storage_options_scoped]: ./media/media-services-monitor-services-account/storagemonitoringoptions_scoped.png

 
