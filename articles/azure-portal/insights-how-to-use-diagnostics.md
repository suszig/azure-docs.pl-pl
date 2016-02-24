<properties 
    pageTitle="監視と診断の有効化" 
    description="Azure でリソースの診断を設定する方法について説明します。" 
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

# 監視と診断の有効化

 [Azure ポータル](http://portal.azure.com), 、リソースに関する監視と診断の頻度の高い豊富なデータを構成することができます。 使用することも、 [REST API](https://msdn.microsoft.com/library/azure/dn931932.aspx) または [.NET SDK](https://www.nuget.org/packages/Microsoft.Azure.Insights/) プログラムで診断を構成します。

Azure の診断、監視、およびメトリック データは、選択したストレージ アカウントに保存されます。 これにより、ストレージ エクスプローラーから、Power BI、サード パーティ製ツールまで、どれでも好きなツールを使用してデータを読み取ることができます。

## リソースを作成する場合

ほとんどのサービスでは、最初に作成したときに診断を有効にすることができます、 [Azure ポータル](http://portal.azure.com)します。

1. 移動して **新規** し興味のあるリソースを選択します。 

2. 選択 **オプションの構成**します。
    ![診断] ブレード](./media/insights-how-to-use-diagnostics/Insights_CreateTime.png)

3. 選択 **診断**, 、] をクリック **に**します。 診断の保存先となるストレージ アカウントを選択する必要があります。 診断データをストレージ アカウントに送信する際には、ストレージおよびトランザクションの通常のデータ料金が発生します。

4. クリックして **OK** 、リソースを作成します。 

## 既存のリソースの設定の変更

リソースを既に作成済みで、診断設定を変更する (データ コレクションのレベル変更など) 必要がある場合は、Azure ポータルで実行できます。

1. クリックしてリソースの移動、 **設定** コマンドです。

2. 選択 **診断**します。

3.  **診断** ブレードにはすべての診断とそのリソースのコレクションのデータを監視します。 一部のリソースのこともできます、 **保有** データのストレージ アカウントからクリーンアップするには、ポリシーです。 
    ![ストレージの診断](./media/insights-how-to-use-diagnostics/Insights_StorageDiagnostics.png)

4. クリックして設定を選択すると、 **保存** コマンドです。 監視データは、初めて有効にする場合に表示されるまで少し時間がかかることがあります。 

### 仮想マシンのデータ収集のカテゴリ
仮想マシンでは、メトリックとログがすべて 1 分間隔で記録されるので、ご使用のマシンに関する情報はいつでも最新のものとなります。

- **基本的なメトリック** : プロセッサやメモリなどの仮想マシンについての正常性メトリックス 
- **ネットワークおよび web メトリック** : ネットワーク接続と web サービスに関するメトリック
- **.NET メトリック** : 仮想マシンで実行されている .NET と ASP.NET アプリケーションについてのメトリック
- **SQL メトリック** : Microsoft SQL Service をそのパフォーマンス メトリックを実行している場合
- **Windows イベント アプリケーション ログ** : アプリケーション チャネルに送信される Windows イベント
- **Windows イベント システム ログ** : システム チャネルに送信される Windows イベント。 すべてのイベントも含まれます [Microsoft マルウェア対策](http://go.microsoft.com/fwlink/?LinkID=404171&clcid=0x409)します。 
- **Windows イベント セキュリティ ログ** : セキュリティ チャネルに送信される Windows イベント
- **診断インフラストラクチャ ログ** : 診断コレクション インフラストラクチャについてのログ
- **IIS ログ** : IIS サーバーについてのログ

現時点では、Linux の特定のディストリビューションはサポートされていません。また、ゲスト エージェントを仮想マシンにインストールする必要があります。

## 次のステップ

* [警告通知を受け取る](insights-receive-alert-notifications.md) するたびに運用上のイベントが発生したり、メトリックがしきい値を超えます。
* [サービス メトリックスを監視](insights-how-to-customize-monitoring.md) 、サービスの可用性と応答性を確認します。
* [インスタンスの数を自動的にスケール変更](insights-how-to-scale.md) 需要に基づいてサービス スケールかどうかを確認します。
* [アプリケーションのパフォーマンスを監視](insights-perf-analytics.md) 、クラウド内に、コードが実行する方法に正確に理解したい場合。
* [イベントの表示し、監査ログ](insights-debugging-with-events.md) をサービスで発生したすべてのものを参照してください。
* [サービスのヘルス状態を追跡](insights-service-health.md) Azure パフォーマンスの低下やサービスの中断が発生したことを確認します。 
 
