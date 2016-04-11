<properties 
    pageTitle="Stream Analytics でクエリを記述する方法 | Microsoft Azure" 
    description="Stream Analytics でのクエリ記述とクエリ データ | ラーニング パス セグメント。"
    keywords="how to write queries, query data, write a query, writing queries"
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
    ms.date="12/11/2015" 
    ms.author="jeffstok"/>

# Stream Analytics でクエリを記述する方法

Azure Stream Analytics のストリーム処理ロジックのクエリ記述は、ジョブが開始する前に定義され、ジョブに到着したデータに対して実行される、"継続クエリ" として実装されます。 データ変換は、SQL に似たクエリ言語で表現などの言語拡張機能は大きく、T-SQL のいくつかで追加された [Windowing](https://msdn.microsoft.com/library/azure/dn835019.aspx) 時間的なセマンティクスを表すために使用します。

## クエリの記述: ##

1. Azure 管理ポータルで Stream Analytics ジョブでクリックして **クエリ**します。

    ![クエリの選択](./media/stream-analytics-write-queries/1-stream-analytics-write-queries.png)  

    Azure プレビュー ポータルで、クリックして **クエリ**します。

    ![クエリの選択のプレビュー](./media/stream-analytics-write-queries/query-preview-portal.png)  

2.  作業を始めやすいように、新しいジョブにはクエリ テンプレートが付いています。 クエリ テンプレートは、入力イベントのすべてのフィールドを出力に反映させる「パススルー」クエリを実行します。  

    - ジョブに少なくとも 1 つの入力と出力が定義されている場合、プレースホルダーの "[YourOutputAlias]" および "[YourInputAlias]" フィールドを最初に使用する入力および出力のエイリアスに置き換えることができます。 さらに、ジョブで入力と出力を定義していなくても、Azure ポータルでクエリを作成してテストできます。
    - 単純なパススルーでは対応できない処理を実行する必要がある場合は、クエリの定義を編集できます。 クエリの作成を開始する見てパターンがキャプチャされたいくつかの一般的なクエリ [ここ](stream-analytics-stream-analytics-query-patterns.md)します。  
  
    ![クエリ データ ウィンドウ](./media/stream-analytics-write-queries/2-stream-analytics-write-queries.png)  

## クエリ データが動作することを検証するには: ##

テスト データを含むローカル JSON ファイルを使用してブラウザーでクエリを実行することにより、クエリが意図したとおりに動作することを確認できます。 これを行っても、ジョブが開始したり、課金に影響することはありません。

> [AZURE.NOTE] 現在、ブラウザー内のクエリのテストは Azure プレビュー ポータルではサポートされていません。  

1.  クエリにエラーがないことを確認し (エラーがある場合は [テスト] ボタンが無効になります)、[テスト] ボタンをクリックします。  

    ![クエリ データ テスト](./media/stream-analytics-write-queries/3-stream-analytics-write-queries.png)  

2.  クエリで参照されている各入力に対するファイルの指定を求められます。 この例では、テンプレートのクエリをそのままに使用しているので、ダイアログで要求される入力の名前は "yourinputalias" です。  

    ![テスト データ クエリ](./media/stream-analytics-write-queries/4-stream-analytics-write-queries.png)  

3.  テスト ファイルを参照します。 いくつかのサンプル ファイルが [github](https://github.com/Azure/azure-stream-analytics/tree/master/Sample Data) してもサンプル データから取得できます入力タブで、サンプル データ関数を使用して、独自のデータ ストリーム入力します。  

    ![クエリの入力](./media/stream-analytics-write-queries/5-stream-analytics-write-queries.png)  

4.  ダイアログ ボックスを閉じると、テスト データに対してクエリが実行され、[クエリ] ページの下部に結果が表示されます。  

    ![クエリの概要](./media/stream-analytics-write-queries/6-stream-analytics-write-queries.png)  

## 問い合わせ
詳細については、参照してください、 [Azure Stream Analytics フォーラム](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## 次のステップ

- [Azure Stream Analytics の概要](stream-analytics-introduction.md)
- [Azure Stream Analytics の使用](stream-analytics-get-started.md)
- [Azure Stream Analytics ジョブのスケーリング](stream-analytics-scale-jobs.md)
- [Stream Analytics Query Language Reference (Stream Analytics クエリ言語リファレンス)](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics management REST API reference (Azure ストリーム分析の管理 REST API リファレンス)](https://msdn.microsoft.com/library/azure/dn835031.aspx)
