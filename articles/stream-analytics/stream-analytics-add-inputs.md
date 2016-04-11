<properties 
    pageTitle="Stream Analytics にデータ入力を追加する方法 | Microsoft Azure " 
    description="Stream Analytics ジョブへの入力の追加 | ラーニング パス セグメント。"
    keywords="data input, streaming data"
    documentationCenter=""
    services="stream-analytics"
    authors="jeffstokes72" 
    manager="paulettm" 
    editor="cgronlun"
/>

<tags 
    ms.service="stream-analytics" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="data-services" 
    ms.date="12/04/2015" 
    ms.author="jeffstok"
/>


# Stream Analytics ジョブにストリーミング データ入力を追加する方法

Azure Stream Analytics ジョブを 1 つ以上のデータ入力に接続できます。各データ入力では、既存のデータ ソースへの接続が定義されています。 データがそのデータ ソースに送信されると、Stream Analytics ジョブによって使用され、リアルタイムでストリーミング データとして処理されます。 Stream Analytics が最初のクラスとの統合 [Azure Event Hubs](http://azure.microsoft.com/services/event-hubs/) と [Azure Blob ストレージ](./storage/storage-dotnet-how-to-use-blobs.md) 内と、ジョブのサブスクリプションの外部の両方です。 Stream Analytics には、データ ストリームと参照データという 2 種類の入力があります。

- **データ ストリーム**:
    Stream Analytics のジョブには、ジョブで使用および変換される少なくとも 1 つのデータ ストリームの入力を含める必要があります。 Azure BLOB ストレージと Azure Event Hubs は、データ ストリーム入力ソースとしてサポートされます。 Azure Event Hubs は、接続されているデバイス、サービス、アプリケーションからイベント ストリームを収集するために使用されます。 ストリームとしてバルク データを取り込むための入力ソースとして、Azure BLOB ストレージを使用できます。  
- **参照データ**:
    Stream Analytics には、補助型の入力と呼ばれる参照データの 2 番目の型がサポートしています。  このデータは、動的なものではなく、静的またはあまり変更されないものです。  通常は、高度なデータ セットを作成するための検索およびデータ ストリームとの関連付けを実行するために使用されます。  現在、Azure BLOB ストレージは参照データをサポートする唯一の入力ソースです。  

Stream Analytics ジョブに入力を追加するには:

1. Azure ポータルのクリックで **入力** 順にクリック **、入力の追加** Stream Analytics ジョブにします。

    ![ストリーミング データのデータ入力の追加](./media/stream-analytics-add-inputs/1-stream-analytics-add-inputs.png)  

    Azure プレビュー ポータルで、 **入力** Stream Analytics ジョブに並べて表示します。  

    ![ストリーミング データのデータ入力の追加](./media/stream-analytics-add-inputs/7-stream-analytics-add-inputs.png)  

2. 入力の種類を指定します。 いずれか **データ ストリーム** または **参照データ**します。

    ![適切なデータ入力の追加、ストリーミングまたは参照](./media/stream-analytics-add-inputs/2-stream-analytics-add-inputs.png)  

    ![適切なデータ入力の追加、ストリーミングまたは参照](./media/stream-analytics-add-inputs/8-stream-analytics-add-inputs.png)  

3. データ ストリーム入力を作成する場合は、入力のソースの種類を指定します。  参照データを作成する場合は、現時点でサポートされるのは BLOB ストレージだけなので、この手順はスキップできます。

    ![データ ストリームのデータ入力の追加](./media/stream-analytics-add-inputs/3-stream-analytics-add-inputs.png)  

    ![データ ストリームのデータ入力の追加](./media/stream-analytics-add-inputs/9-stream-analytics-add-inputs.png)  

4. [入力のエイリアス] ボックスに、この入力のわかりやすい名前を入力します。  この名前は、後で入力を参照するためにジョブのクエリで使用されます。

    データ ソースに接続するために必要な他の接続プロパティを入力します。 これらのフィールドが入力とソースの種類の種類によって異なり、詳細に定義されて [ここ](stream-analytics-create-a-job.md)します。  

    ![イベント ハブのデータ入力の追加](./media/stream-analytics-add-inputs/4-stream-analytics-add-inputs.png)  

5. 入力データのシリアル化の設定を指定します。
    - クエリ期待どおりの機能を確認するには指定、 **イベントのシリアル化形式** 受信するデータ。  サポートされているシリアル化形式は、JSON、CSV、Avro です。
    - 確認、 **エンコード** データ。  現時点でサポートされているエンコード形式は UTF-8 だけです。

    ![データ入力のデータ シリアル化の設定](./media/stream-analytics-add-inputs/5-stream-analytics-add-inputs.png)  

    ![データ入力のデータ シリアル化の設定](./media/stream-analytics-add-inputs/10-stream-analytics-add-inputs.png)  

6. 入力の作成が完了すると、Stream Analytics は入力ソースに接続できることを確認します。  Notification Hub でテスト接続操作の状態を表示できます。

    ![ストリーミング データ入力のテスト接続](./media/stream-analytics-add-inputs/6-stream-analytics-add-inputs.png)  

    ![ストリーミング データ入力のテスト接続](./media/stream-analytics-add-inputs/11-stream-analytics-add-inputs.png)  

## 問い合わせ
詳細については、参照してください、 [Azure Stream Analytics フォーラム](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## 次のステップ

- [Azure Stream Analytics の概要](stream-analytics-introduction.md)
- [Azure Stream Analytics の使用](stream-analytics-get-started.md)
- [Azure Stream Analytics ジョブのスケーリング](stream-analytics-scale-jobs.md)
- [Stream Analytics Query Language Reference (Stream Analytics クエリ言語リファレンス)](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics management REST API reference (Azure ストリーム分析の管理 REST API リファレンス)](https://msdn.microsoft.com/library/azure/dn835031.aspx)
