<properties 
    pageTitle="サービス正常性を追跡する" 
    description="Azure でパフォーマンスの低下やサービスの中断がいつ発生したかを検出します。 " 
    authors="stepsic-microsoft-com" 
    manager="kamrani" 
    editor="" 
    services="azure-portal" 
    documentationCenter="na"/>

<tags 
    ms.service="azure-portal" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/08/2015" 
    ms.author="stepsic"/>


# サービス正常性を追跡する

Azure は、サービスの中断やパフォーマンスの低下があるたびに、公開を行います。 Azure ポータルでこれらのイベントを参照して、使用することも、 [REST API](https://msdn.microsoft.com/library/azure/dn931927.aspx) または [.NET SDK](https://www.nuget.org/packages/Microsoft.Azure.Insights/) イベントの完全なセットをプログラムでアクセスします。

## リージョンごとのサービス正常性を確認する

1. サインイン、 [Azure ポータル](https://portal.azure.com/)します。

2. **ホーム** というタイルが表示されます **サービスのヘルス**
    ![ホーム](./media/insights-service-health/Insights_Home.png)

3. このタイルをクリックすると、Azure のすべてのリージョンの一覧が表示されます。 その地域のサービスのヘルス履歴を表示する任意のリージョンをクリックすることができます。
    ![ホーム](./media/insights-service-health/Insights_Regions.png)

4. テーブルで個々のインシデントをクリックし、その詳細を参照することも可能です。

## サービス正常性の完全なログを参照する

2. をクリックして、 **参照** 選択 **監査ログ**します。  
    ![参照ハブ](./media/insights-service-health/Insights_Browse.png)

3. これによってブレードが開き、過去 7 日間にサブスクリプションに影響を与えたイベントがすべて表示されます。 一覧にサービス正常性のエントリが表示されますが、大量にあり一覧で見つけにくい場合があります。

4. **[フィルター]** コマンドをクリックします。

5. 選択 **イベント カテゴリ** 選択 **サービスのヘルス**:
    ![All events](./media/insights-service-health/Insights_Filter.png)

6. **[更新済み]** をクリックします。

7. すべてのサブスクリプションに影響が与えたサービス正常性のイベントが表示されますようになりました。
    ![リソース グループ](./media/insights-service-health/Insights_HealthEvent.png)

8. そこから詳細ブレードに移動すると、イベントの詳細を確認できます。

## 次のステップ

* [警告通知を受け取る](insights-receive-alert-notifications.md) イベントが発生したときにします。
* [サービス メトリックスを監視](insights-how-to-customize-monitoring.md) 、サービスの可用性と応答性を確認します。
* [可用性と任意の web ページの応答性監視](../app-insights-monitor-web-app-availability.md) のかどうか、ページは停止検索できるように、Application Insights を使用します。





