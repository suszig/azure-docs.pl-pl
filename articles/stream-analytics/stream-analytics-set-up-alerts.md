<properties 
    pageTitle="Stream Analytics のクエリのアラートを設定する |Microsoft Azure" 
    description="Stream Analytics のアラートについて理解する" 
    keywords="set up alerts"
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


# Azure Stream Analytics ジョブのアラートを設定する

## 概要: 監視ページ

メトリックが指定した条件に達したときにアラートをトリガーするルールを設定できます。

たとえば、"過去 15 分間に出力イベントの場合 < 100、電子メール通知を電子メール id を送信する: xyz@company.com"です。

ルールは、ポータルで、メトリックに設定することができますまたは構成できる [プログラムを使用して](https://code.msdn.microsoft.com/windowsazure/Receive-Email-Notifications-199e2c9a) 操作ログ データをします。

## Azure ポータルを使用してアラートを設定する

Microsoft Azure 管理ポータルでアラートを設定する方法は 2 つあります。  

1.   **モニター** Stream Analytics ジョブのタブをクリックします。  
2.  Management Services の操作ログ  

## ポータルでジョブの [監視] タブを使用してアラートを設定する

1.  [監視] タブでメトリックを選択し、をクリックして、 **ルールの追加** 、ダッシュ ボードの下部にあるボタンをクリックし、ルールを設定します。  

    ![ダッシュボード](./media/stream-analytics-set-up-alerts/01-stream-analytics-set-up-alerts.png)  

2.  アラートの名前と説明を定義します。  

    ![ルールの作成](./media/stream-analytics-set-up-alerts/02-stream-analytics-set-up-alerts.png)  

3.  しきい値、アラート評価ウィンドウ、およびアラートのアクションを入力します。  

    ![条件の定義](./media/stream-analytics-set-up-alerts/03-stream-analytics-set-up-alerts.png)  

## 操作ログを使用してアラートを設定する

1.  移動して、 **アラート** ] タブで管理サービスで、 [Azure ポータル](https://manage.windowsazure.com)します。  
2.  をクリックして **ルールの追加**  

    ![条件](./media/stream-analytics-set-up-alerts/04-stream-analytics-set-up-alerts.png)  

3.  アラートの名前と説明を定義します。 [サービスの種類] として [Stream Analytics] を選択し、[サービス名] としてジョブの名前を選択します。  

    ![アラートの定義](./media/stream-analytics-set-up-alerts/05-stream-analytics-set-up-alerts.png)  

## Azure プレビュー ポータルでアラートを設定する ##

Azure プレビュー ポータルでは、アラートを有効に興味のあるストリーム分析ジョブを参照してクリックして、 **監視** セクションです。   **メトリック** ブレードが開き、をクリックする、 **アラートの追加** コマンドです。

  ![Azure プレビュー ポータルでの設定](./media/stream-analytics-set-up-alerts/06-stream-analytics-set-up-alerts.png)  

アラート ルールに名前を付け、通知メールに表示される説明を選択できます。

[メトリック] を選択する場合は、メトリックの条件としきい値を選択します。

  ![Azure プレビュー ポータルでのメトリックの選択](./media/stream-analytics-set-up-alerts/07-stream-analytics-set-up-alerts.png)  

Azure プレビュー ポータルでアラートの設定の詳細については、次を参照してください。 [警告通知を受け取る](./azure-portal/insights-receive-alert-notifications.md)します。  

## 問い合わせ
詳細については、参照してください、 [Azure Stream Analytics フォーラム](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## 次のステップ

- [Azure Stream Analytics の概要](stream-analytics-introduction.md)
- [Azure Stream Analytics の使用](stream-analytics-get-started.md)
- [Azure Stream Analytics ジョブのスケーリング](stream-analytics-scale-jobs.md)
- [Stream Analytics Query Language Reference (Stream Analytics クエリ言語リファレンス)](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics management REST API reference (Azure ストリーム分析の管理 REST API リファレンス)](https://msdn.microsoft.com/library/azure/dn835031.aspx)

