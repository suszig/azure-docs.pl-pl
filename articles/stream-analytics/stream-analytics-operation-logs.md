<properties 
    pageTitle="Stream Analytics の操作ログとサービス ログの利用 | Microsoft Azure" 
    description="Stream Analytics 操作ログの使用方法" 
    keywords="service logs"
    services="stream-analytics" 
    documentationCenter="" 
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

# Stream Analytics サービスと操作ログの概要

すべての Azure サービスでは、管理操作に関連する詳細を記録するために操作ログ メッセージがユーザーに提供されます。 Azure Stream Analytics では、ジョブの状態、ジョブの進行状況、エラー メッセージを表示して入力から、処理、出力までのジョブの進行を追跡するなど、この情報をデバッグ目的に使用できます。

## Microsoft Azure 管理ポータルでの操作ログの検出

操作ログには 2 つの方法でアクセスできます。  

- Stream Analytics ジョブのダッシュボード  
- Microsoft Azure ポータルの管理サービス  

## Stream Analytics ジョブのダッシュボード

Stream Analytics ジョブの対応するログへのリンクが、ジョブの [ダッシュボード] タブに表示されます。 そのリンクをクリックすると、その特定のジョブの最新のログが表示されるようにフィルターが設定されます。

  ![管理サービス ログの選択](./media/stream-analytics-operation-logs/01-stream-analytics-operation-logs.png)  

## 管理サービス

Azure ポータルで Stream Analytics および他のサービスの操作ログに手動で移動するには:

1.  をクリックして **管理サービス** で、 [Azure ポータル](https://manage.windowsazure.com)します。
2.  選択 **Stream Analytics** の **型** のジョブの名前と **サービス名**します。  

  ![Stream Analytics の選択](./media/stream-analytics-operation-logs/02-stream-analytics-operation-logs.png)  

## Azure プレビュー ポータルでの監査ログの検出 ##

Azure プレビュー ポータルで、Stream Analytics ジョブの操作ログを検索するクリックして **参照** し、[ **監査ログ**します。

  ![Azure プレビュー ポータルでの Stream Analytics の選択](./media/stream-analytics-operation-logs/06-stream-analytics-operation-logs.png)  

これにより、サブスクリプション内のすべてのリソースに関して、過去 7 日間のイベントが表示されたブレードが開かれます。  イベントの種類を指定するか、時間枠をクリックすると表示フィルター処理すること、 **フィルター** コマンドです。

  ![Azure プレビュー ポータルでの Stream Analytics の選択](./media/stream-analytics-operation-logs/07-stream-analytics-operation-logs.png)  

## ログ詳細の取得

時間範囲と状態でフィルター処理して、ジョブのログを表示できます。

Azure 管理ポータルで] をクリックして、 **詳細** を選択したイベントに関する詳細を表示するウィンドウの下部にボタンをクリックします。 

  ![詳細の選択](./media/stream-analytics-operation-logs/03-stream-analytics-operation-logs.png)  

Azure プレビュー ポータルで、ログ エントリをクリックするとその中に詳細なイベントが表示されます。

  ![Azure プレビュー ポータルでの詳細の選択](./media/stream-analytics-operation-logs/08-stream-analytics-operation-logs.png)  

そこから開くことができます、 **詳細** イベントをクリックしてブレードです。

  ![Azure プレビュー ポータルでの詳細の選択](./media/stream-analytics-operation-logs/09-stream-analytics-operation-logs.png)  

## 失敗したジョブのデバッグ

Microsoft Azure 管理ポータルで [検索] アイコンをクリックし、「failed」と入力します。 エラーが発生したすべてのログの結果が表示されます。 

  ![失敗したジョブのデバッグ](./media/stream-analytics-operation-logs/04-stream-analytics-operation-logs.png)  

Azure プレビュー ポータルでは、表示するメッセージのレベルでフィルター処理できます **重大** イベントです。

  ![Azure プレビュー ポータルでのデバッグ](./media/stream-analytics-operation-logs/10-stream-analytics-operation-logs.png)  

エラーのいずれかを選択し、をクリックして、 **詳細** エラーの詳細についてです。  一部のエラー メッセージでは、問題を軽減する方法についての情報も提供されます。 

  ![操作の詳細](./media/stream-analytics-operation-logs/05-stream-analytics-operation-logs.png)  

アクセスする必要がある場合に備えて [サポート](http://azure.microsoft.com/support/options/) を使用してチームに情報を提供したり、 [MSDN フォーラム](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics), 、操作の詳細を具体的には注意してください、 **相関 ID**します。 

## 問い合わせ
詳細については、参照してください、 [Azure Stream Analytics フォーラム](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## 次のステップ

- [Azure Stream Analytics の概要](stream-analytics-introduction.md)
- [Azure Stream Analytics の使用](stream-analytics-get-started.md)
- [Azure Stream Analytics ジョブのスケーリング](stream-analytics-scale-jobs.md)
- [Stream Analytics Query Language Reference (Stream Analytics クエリ言語リファレンス)](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics management REST API reference (Azure ストリーム分析の管理 REST API リファレンス)](https://msdn.microsoft.com/library/azure/dn835031.aspx)

