<properties 
    pageTitle="Stream Analytics のストリーミング ジョブを開始する方法 | Microsoft Azure" 
    description="Azure Stream Analytics のストリーミング ジョブを実行する方法 | ラーニング パス セグメント"
    keywords="streaming jobs"
    documentationCenter=""
    services="stream-analytics"
    authors="jeffstokes72" 
    manager="paulettm" 
    editor="cgronlun"/>

<tags 
    ms.service="stream-analytics" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="data-services" 
    ms.date="12/04/2015" 
    ms.author="jeffstok"/>

# Azure Stream Analytics でストリーミング ジョブを実行する方法

ジョブの入力、クエリ、および出力をすべて指定したら、Stream Analytics ジョブを開始できます。

ジョブを開始するには:

1.  ジョブのダッシュ ボードから、Azure ポータルで、クリックして **開始** ページの下部にあります。

    ![[ジョブの開始] ボタン](./media/stream-analytics-run-a-job/1-stream-analytics-run-a-job.png)  

    Azure プレビュー ポータルで、クリックして **開始** ジョブ ページの上部にあります。

    ![Azure プレビュー ポータルの [ジョブの開始] ボタン](./media/stream-analytics-run-a-job/4-stream-analytics-run-a-job.png)  

2.  指定する **出力の開始** ときに、このジョブではその出力を生成を開始するかを決定する値。 既に開始されていないジョブの既定の設定は **ジョブの開始時刻**, 、つまり、ジョブは、データの処理にすぐに開始されます。 指定することも、 **カスタム** (履歴データを使用) するための過去または (将来の時刻までの処理を遅延) する将来の時間。 ジョブが既に開始し、停止すると、オプションの場合の **最終停止時刻** は最後の出力時刻からジョブを再開し、データ損失を回避するために使用できます。  

    ![開始ストリーミング ジョブ時刻](./media/stream-analytics-run-a-job/2-stream-analytics-run-a-job.png)  

    ![Azure プレビュー ポータルの開始ストリーミング ジョブ時刻](./media/stream-analytics-run-a-job/5-stream-analytics-run-a-job.png)  

3.  選択内容を確認します。 ジョブの状態が変わります *開始* は、すぐに移動して *を実行している* 、ジョブが開始します。 進行状況を監視する、 **開始** 操作で、 **通知ハブ**:

    ![ストリーミング ジョブの進捗状況](./media/stream-analytics-run-a-job/3-stream-analytics-run-a-job.png)  

    ![Azure プレビュー ポータルのストリーミング ジョブの進捗状況](./media/stream-analytics-run-a-job/6-stream-analytics-run-a-job.png)  

## 問い合わせ
詳細については、参照してください、 [Azure Stream Analytics フォーラム](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## 次のステップ

- [Azure Stream Analytics の概要](stream-analytics-introduction.md)
- [Azure Stream Analytics の使用](stream-analytics-get-started.md)
- [Azure Stream Analytics ジョブのスケーリング](stream-analytics-scale-jobs.md)
- [Stream Analytics Query Language Reference (Stream Analytics クエリ言語リファレンス)](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics management REST API reference (Azure ストリーム分析の管理 REST API リファレンス)](https://msdn.microsoft.com/library/azure/dn835031.aspx)

