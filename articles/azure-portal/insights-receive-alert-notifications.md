<properties 
    pageTitle="アラート通知の受信" 
    description="アラート ルールの条件を満たすと通知されます。" 
    authors="stepsic-microsoft-com" 
    manager="ronmart" 
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

# アラート通知の受信

監視メトリック、イベント、Azure サービスに基づいて通知を受け取ることができます。 

メトリック値のアラート ルールでは、指定したメトリックの値が割り当てられたしきい値を超えると、アラート ルールがアクティブになり、通知を送信できます。 イベントのアラート ルールは、ルールは、通知を送信できます *すべて* イベント、または、される場合にのみ発生するイベント数。

アラート ルールを作成するときに、サービス管理者と共同管理者、または指定した別の管理者にメール通知を送信するオプションを選択できます。 ルールにアクティブになり、アラート条件が解決されると、メールによる通知が送信されます。

使用することができます、 [REST API](https://msdn.microsoft.com/library/azure/dn931945.aspx) または [.NET SDK](https://www.nuget.org/packages/Microsoft.Azure.Insights/) を構成し、アラート ルールに関する情報をプログラムで取得します。

## アラート ルールを作成する

1.  [ポータル](https://portal.azure.com/), 、] をクリックして **参照**, 、しするリソースの監視に必要とします。

2. をクリックして、  **アラート ルール** タイルに、 **操作** レンズします。

3. クリックして、 **アラートの追加** コマンドです。
    ![アラートを追加します。](./media/insights-receive-alert-notifications/Insights_AddAlert.png)

4. アラート ルールに名前を付け、通知メールに表示される説明を選択できます。

5. 選択すると **メトリック** 条件と、メトリックのしきい値の値を選択します。 これは、Azure がアラート アクティビティの監視とプロットに使用する期間です。
    ![条件としきい値](./media/insights-receive-alert-notifications/Insights_ConditionAndThreshold.png)

6. 選択することもできます **イベント**, と、特定のイベントが発生したときに通知を受け取ります。 
    ![イベント](./media/insights-receive-alert-notifications/Insights_Events.png)
    
>[AZURE.NOTE] この時点でイベントが Web アプリに対してのみサポートされます。 

7. 最後に、担当の管理者にメール通知を送信できます。

クリックした後 **保存**, 、数分間で行うことが形成された選択したメトリックがしきい値を超えるとします。 

## アラート ルールの管理

アラート ルールを作成した後、アラートのプレビューを表示することができますしきい値は、過去のメトリックを比較します。 

![イベント](./media/insights-receive-alert-notifications/Insights_EditAlert.png)


もちろんこのアラート ルールを編集することができ、 **を無効にする** または **を有効にする** 通知の受信を一時的に停止する場合。 

## 次のステップ

* [Webhook を構成するアラートを](insights-webhooks-alerts.md) さまざまなチャネルに通知をルーティング
* [サービス メトリックスを監視](insights-how-to-customize-monitoring.md) 、サービスの可用性と応答性を確認します。
* [監視を有効にし、診断](insights-how-to-use-diagnostics.md) 、サービスに関する詳細な頻度の高いメトリックを収集します。
* [可用性と任意の web ページの応答性監視](../app-insights-monitor-web-app-availability.md) のかどうか、ページは停止検索できるように、Application Insights を使用します。
* [アプリケーションのパフォーマンスを監視](insights-perf-analytics.md) 、クラウド内に、コードが実行する方法に正確に理解したい場合。
* [イベントの表示し、監査ログ](insights-debugging-with-events.md) をサービスで発生したすべてのものを参照してください。
* [サービスのヘルス状態を追跡](insights-service-health.md) Azure パフォーマンスの低下やサービスの中断が発生したことを確認します。
 

