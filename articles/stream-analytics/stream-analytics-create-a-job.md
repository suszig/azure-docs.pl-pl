<properties 
    pageTitle="Stream Analytics のデータ分析処理ジョブを作成する方法 | Microsoft Azure" 
    description="Stream Analytics のデータ分析処理ジョブの作成 | ラーニング パス セグメント。"
    keywords="data analytics processing"
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

# Stream Analytics のデータ分析処理ジョブを作成する方法

Azure Stream Analytics の最上位のリソースは Stream Analytics ジョブです。  ジョブは、1 つ以上の入力データ ソース、データ変換を表すクエリ、結果が書き込まれる 1 つ以上の出力ターゲットで構成されます。 ユーザーはこれらのジョブを使用して、ストリーミング データ シナリオのデータ分析処理を実行できます。

Stream Analytics の使用を開始するには、まず新しい Stream Analytics ジョブを作成します。  ジョブが開始されるまでこのアクションによる課金への影響はないことに注意してください。

1.  オンライン上でサインイン [Microsoft Azure ポータル](http://manage.windowsazure.com) または Azure プレビュー ポータル。
2.  Azure ポータル: **[新規**, 、順にクリックして **Data Services**, 、] をクリックし、 **Stream Analytics** と **簡易作成**します。

    ![データ分析処理ジョブ ウィザード](./media/stream-analytics-create-a-job/1-stream-analytics-create-a-job.png)  

    Azure プレビュー ポータルで、[新規] をクリックし、次に [Data + Analytics]、[Azure Stream Analytics] の順にクリックします。  

    ![データ分析処理ジョブの作成](./media/stream-analytics-create-a-job/4-stream-analytics-create-a-job.png)  

3.  Stream Analytics ジョブに必要な構成を指定します。
    -  **ジョブ名** ボックスで、Stream Analytics ジョブを識別する名前を入力します。 ときに、 **ジョブ名** 検証されると、ジョブ名] ボックスに緑色のチェック マークが表示されます。  **ジョブ名** 英数字のみを含めることがおよび '-' 文字、および 3 ~ 63 文字である必要があります。
    - 使用 **地域** Azure ポータルでまたは **場所** 、Azure プレビュー ポータルでジョブを実行する地理的な場所を指定します。
    - 選択するかとして使用するストレージ アカウントを作成する場合は、Azure ポータルを使用して、 **地域の監視ストレージ アカウント**します。 このストレージ アカウントを使用して、この地域内で実行されるすべての Stream Analytics ジョブの監視データを格納します。
    - Azure プレビュー ポータルを使用して、指定、新しいまたは既存 **リソース グループ** 、アプリケーションの関連リソースを保持するためにします。

4.  新しい Stream Analytics ジョブのオプションが構成されると、クリックして **Stream Analytics ジョブの作成**します。 Stream Analytics ジョブが作成されるまで数分かかる場合があります。 Notification Hub で進行状況を監視し、状態を確認できます。

    ![データ分析処理ジョブの通知ハブ](./media/stream-analytics-create-a-job/2-stream-analytics-create-a-job.png)  

    ![Azure プレビュー ポータルのデータ分析処理ジョブのジョブの作成](./media/stream-analytics-create-a-job/5-stream-analytics-create-a-job.png)  

5.  新しいジョブは、状態が表示されます **Created**します。 注意して、 **開始** ボタンが無効になります。 ジョブが開始できるようになるには、ジョブの入力、クエリ、出力を構成する必要があります。

    ![データ分析処理ジョブのジョブの状態](./media/stream-analytics-create-a-job/3-stream-analytics-create-a-job.png)  

    ![Azure プレビュー ポータルのデータ分析処理ジョブのジョブの状態](./media/stream-analytics-create-a-job/6-stream-analytics-create-a-job.png)  

## 問い合わせ
詳細については、参照してください、 [Azure Stream Analytics フォーラム](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## 次のステップ

- [Azure Stream Analytics の概要](stream-analytics-introduction.md)
- [Azure Stream Analytics の使用](stream-analytics-get-started.md)
- [Azure Stream Analytics ジョブのスケーリング](stream-analytics-scale-jobs.md)
- [Stream Analytics Query Language Reference (Stream Analytics クエリ言語リファレンス)](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics management REST API reference (Azure ストリーム分析の管理 REST API リファレンス)](https://msdn.microsoft.com/library/azure/dn835031.aspx)
