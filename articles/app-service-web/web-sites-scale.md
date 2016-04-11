<properties 
    pageTitle="Azure App Service の Web アプリをスケーリングする" 
    description="Azure App Service での、自動スケールを含む Web アプリのスケール アップとスケール ダウンの方法について説明します。" 
    services="app-service" 
    documentationCenter="" 
    authors="cephalin" 
    manager="wpickett" 
    editor="mollybos"/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="12/10/2015" 
    ms.author="cephalin"/>

# Azure App Service の Web アプリをスケーリングする #

パフォーマンスの向上と Microsoft Azure での web アプリのスループットは、使用することができます、 [Azure ポータル](http://go.microsoft.com/fwlink/?LinkId=529715) スケール、 [App Service](http://go.microsoft.com/fwlink/?LinkId=529714) から計画 **Free** モードを **共有**, 、**基本的な**, 、**標準**, 、または **Premium** モードです。 

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

Azure で Web アプリをスケール アップするには、2 つの操作が必要です。App Service プランのモードを上位レベルに変更することと、上位レベルのサービスに切り替えた後、特定の設定を構成することです。 この記事では、この 2 つのトピックについて説明します。 などのサービス レベルを高く **標準** と **Premium** モードは、堅牢性と Azure 上のリソースの使用方法を決定する際の柔軟性を提供します。

スケール設定は適用に数秒を要するのみで、App Service プランに含まれるすべての Web アプリに反映されます。 コードの変更やアプリケーションの再デプロイは必要ありません。

App Service プランについては、次を参照してください。 [App Service プランは何ですか。](../web-sites-web-hosting-plan-overview.md)  [Azure App Service プランの詳細な概要](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)します。 料金情報および各 App Service プランの機能では、「 [App Service の料金詳細](/pricing/details/web-sites/)します。  

> [AZURE.NOTE] Web アプリを切り替える前に、 **Free** モード **基本的な**, 、**標準**, 、または **Premium** モードでは、Azure App Service サブスクリプションのインプレース支出 cap を削除する必要があります最初です。 Microsoft Azure App Service サブスクリプションのオプションを表示または変更、次を参照してください。 [Microsoft Azure サブスクリプションの][azuresubscriptions]です。

<a name="scalingsharedorbasic"></a>
<!-- ===================================== -->
## 共有または基本モードへの規模の変更
<!-- ===================================== -->

1. ブラウザーで開く、 [Azure ポータル][portal]します。
    
2. Web アプリのブレードで、[ **すべての設定**, 、] をクリックし、 **スケール**, 、] をクリックし、 **インスタンスを追加し、パフォーマンスを向上させるのには、Free プランからアップグレード**します。
    
    ![プランの選択][ChooseWHP]
    
4.  **価格レベルの選択** ブレードで、いずれかを選択 **共有** または **基本的な** モードでは、順にクリックして **選択**します。
    
     **通知** ] タブ、緑色の点滅は **成功** 、操作が完了します。 
    
5. スライド、 **インスタンス** バーを左から右をクリックし、インスタンスの数を増やす **保存** コマンド バーにします。 インスタンス サイズのオプションでは使用できません **Shared** モードです。 これらのインスタンス サイズの詳細については、次を参照してください。 [仮想マシンと Microsoft Azure のクラウド サービスのサイズ][vmsizes]します。
    
    ![基本モードのインスタンス サイズ][ChooseBasicInstances]
    
     **通知** ] タブ、緑色の点滅は **成功** 、操作が完了します。 
    
<a name="scalingstandard"></a>
<!-- ================================= -->
## 標準またはプレミアム モードへの規模の変更
<!-- ================================= -->

> [AZURE.NOTE] App Service を切り替える前にする予定 **標準** または **Premium** モードでは、Microsoft Azure App Service サブスクリプションの場所に設定されている使用削除する必要があります。 解除しないと、請求期間が終了する前に使用制限に達した場合に、Web アプリが使用できなくなるおそれがあります。 Microsoft Azure App Service サブスクリプションのオプションを表示または変更、次を参照してください。 [Microsoft Azure サブスクリプションの][azuresubscriptions]です。

1. スケーリングする **標準** または **Premium** モードでは、同じは最初にスケールするときの手順として次 **共有** または **基本的な**, を選択し、 **標準** または **Premium** モードで **価格レベルを選択**, 、順にクリックして **選択**します。 
    
     **通知** ] タブ、緑色の点滅は **成功** 操作が完了したら、および **自動スケール モード** が有効になります。
    
    ![Standard または Premium モードへの拡張][ScaleStandard]
    
    スライドしても、 **インスタンス** バーを手動でスケール インスタンスをさらと同様に、 **基本的な** モードを上記のようにします。 ただし、ここでは方法を学習して **自動スケール モード**します。 
    
2.  **自動スケール モード**, [ **パフォーマンス** パフォーマンス メトリックに基づいた自動スケールします。
    
    ![自動スケール モードがパフォーマンスに設定されています][Autoscale]
    
3.  **インスタンス範囲**, 、App Service プランに自動的に対応するインスタンスの最小値と最大数を定義する 2 つのスライダーを移動します。 このチュートリアルでは、最大のスライダーを移動 **6** インスタンス。
    
4. クリックして **保存** コマンド バーにします。
    
4. [ **ターゲット メトリック**, 、] をクリックして **>** を既定のメトリックの自動スケール規則を構成します。  
    
    ![ターゲット メトリックの設定][SetTargetMetrics]
    
    CPU、メモリ、ディスク キュー、HTTP キュー、データ フローなど、さまざまなパフォーマンス メトリックの自動スケール規則を構成することができます。 ここでは、次のように CPU の割合の自動スケールを構成します。
    
    - 過去 10 分間の CPU が 70% を超える場合は、インスタンスを 1 増やす
    - 過去 5 分間の CPU が 90% を超える場合は、インスタンスを 3 増やす
    - 過去 30 分間の CPU が 50% 未満の場合は、インスタンスを 1 減らす 
    
    
4. ままにして **メトリック** ドロップダウンは **CPU 使用率の**です。
    
5.  **スケール アップルール**, 、最初のルールを設定して構成 **条件** に **大きい**, 、**しきい値** に **70** (%) **Over 過去** に **10** (分) **をスケール アップ** に **1** (インスタンス) と **クール ダウン** に **10** (分)。 
    
    ![1 つ目の自動スケール ルールの設定][SetFirstRule]
    
    >[AZURE.NOTE]  **下がる** 設定では、この規則をもう一度スケール変更する前のスケール アクションの後に待機する時間を指定します。
    
6. をクリックして **スケール アップ規則の追加**, 、設定して 2 番目のルールを構成 **条件** に **大きい**, 、**しきい値** に **90** (%)、 **Over 過去** に **1** (分) **をスケール アップ** に **3** (インスタンス)、および **クール ダウン** に **1** (分) です。
    
    ![2 つ目の自動スケール ルールの設定][SetSecondRule]
    
5.  **スケール ダウンのルール**, を設定して 3 番目の規則を構成 **条件** に **少ない**, 、**しきい値** に **50** (%)、 **Over 過去** に **30** (分) **でスケール ダウン** に **1** (インスタンス) と **クール ダウン** に **60** (分) です。 
    
    ![3 つ目の自動スケール ルールの設定][SetThirdRule]
    
7. クリックして **保存** コマンド バーにします。 自動スケール規則を反映今すぐ必要があります、 **スケール** ブレードです。 
    
    ![自動スケール ルール設定の結果][SetRulesFinal]

<a name="ScalingSQLServer"></a>
##Web アプリに接続されている SQL Server データベースのスケール設定
(App Service のプラン モードにかかわらず) 1 つ以上の SQL Server データベースが Web アプリにリンクされている場合、ニーズに合わせてすばやくスケールを設定できます。

1. リンクされたデータベースのいずれかのスケーリングで、web アプリのブレードを開き、 [Azure ポータル][portal]します。  **Essentials** 折りたたみ可能なドロップダウン リストで、をクリックして、 **リソース グループ** リンクします。 次に、 **概要** リソース グループ ブレードで、クリックされた 1 つのリンク先のデータベースの一部です。

    ![リンクされたデータベース][ResourceGroup]
    
2. リンクされた SQL データベースのブレードでをクリックして、 **価格レベル** パート、パフォーマンス要件に応じて層のいずれかを選択してクリックして **選択**します。 
    
    ![SQL Database のスケーリング][ScaleDatabase]
    
3. また、geo レプリケーションを設定して、SQL Database の高可用性と障害復旧機能を向上させることもできます。 これを行うには、クリックして、 **Geo レプリケーション** 部分です。
    
    ![SQL Database の geo レプリケーションの設定][GeoReplication]

<a name="devfeatures"></a>
## 開発者機能
Web アプリのモードに応じて、次の開発者向け機能を使用できます。

### ビット ###

-  **基本的な**, 、**標準**, 、および **Premium** モードは、64 ビットおよび 32 ビット アプリケーションをサポートします。
-  **Free** と **Shared** プラン モードは、32 ビット アプリケーションのみをサポートします。

### デバッガー サポート ###

- デバッガーのサポートは提供されて、 **Free**, 、**共有**, 、および **基本的な** モードでは、App Service プランあたりの同時接続数 1 です。
- デバッガーのサポートは提供されて、 **標準** と **Premium** モードでは、App Service プランあたり 5 つの同時接続します。

<a name="OtherFeatures"></a>
## その他の機能

### Web エンドポイントの監視 ###

- Web エンドポイントの監視が含まれて、 **基本的な**, 、**標準**, 、および **Premium** モードです。 Web エンドポイントの監視の詳細については、次を参照してください。 [Web アプリの監視方法](web-sites-monitor.md)します。

- すべての App Service で、その他の機能の詳細については料金など、プランとすべてのユーザー (開発者を含む)、関心のある機能を参照してください [App Service の料金詳細](/pricing/details/web-sites/)します。

>[AZURE.NOTE] 場合は、Azure アカウントがサインアップする前に Azure App Service の使用を開始するには、 [App Service の試用](http://go.microsoft.com/fwlink/?LinkId=523751), 、App Service で有効期間の短いスターター web アプリをすぐに作成する場所です。 このサービスの利用にあたり、クレジット カードは必要ありません。契約も必要ありません。

<a name="Next Steps"></a>   
## 次のステップ

- Azure で開始するを参照してください。 [Microsoft Azure の無料試用版](/pricing/free-trial/)します。
- 料金、サポート、および SLA については、次のリンクを参照してください。
    
    [データ転送の料金詳細](/pricing/details/data-transfers/)
    
    [Azure のサポート プラン](/support/plans/)
    
    [サービス レベル アグリーメント](/support/legal/sla/)
    
    [SQL データベースの料金詳細](/pricing/details/sql-database/)
    
    [Virtual Machine and Cloud Service Sizes for Microsoft Azure (Microsoft Azure の仮想マシンおよびクラウド サービスのサイズ)][vmsizes]
    
    [App Service の料金の詳細](/pricing/details/web-sites/)
    
    [App Service の料金の詳細 - SSL 接続](/pricing/details/web-sites/#ssl-connections)

- Azure App Service で拡張性と回復力のあるアーキテクチャの構築などのベスト プラクティスについては [ベスト プラクティス: Azure App Service Web Apps](http://blogs.msdn.com/b/windowsazure/archive/2014/02/10/best-practices-windows-azure-websites-waws.aspx)します。

- Web Apps の拡張に関するビデオ:
    
    - [Azure Websites のスケール設定のタイミング - Stefan Schackow 共演のビデオ](/documentation/videos/azure-web-sites-free-vs-standard-scaling/)
    - [Azure Websites、CPU、またはスケジュールの自動スケール - Stefan Schackow 共演](/documentation/videos/auto-scaling-azure-web-sites/)
    - [Azure Websites のスケールを設定する方法 - Stefan Schackow 共演](/documentation/videos/how-azure-web-sites-scale/)

## 変更内容
* Web サイトから App Service への変更のガイドを参照してください: [Azure App Service と既存の Azure サービスへの影響](http://go.microsoft.com/fwlink/?LinkId=529714)

<!-- LINKS -->
[vmsizes]:http://go.microsoft.com/fwlink/?LinkId=309169
[SQLaccountsbilling]:http://go.microsoft.com/fwlink/?LinkId=234930
[azuresubscriptions]:http://go.microsoft.com/fwlink/?LinkID=235288
[portal]: https://portal.azure.com/

<!-- IMAGES -->
[ChooseWHP]: ./media/web-sites-scale/scale1ChooseWHP.png
[ChooseBasicInstances]: ./media/web-sites-scale/scale2InstancesBasic.png
[SaveButton]: ./media/web-sites-scale/05SaveButton.png
[BasicComplete]: ./media/web-sites-scale/06BasicComplete.png
[ScaleStandard]: ./media/web-sites-scale/scale3InstancesStandard.png
[Autoscale]: ./media/web-sites-scale/scale4AutoScale.png
[SetTargetMetrics]: ./media/web-sites-scale/scale5AutoScaleTargetMetrics.png
[SetFirstRule]: ./media/web-sites-scale/scale6AutoScaleFirstRule.png
[SetSecondRule]: ./media/web-sites-scale/scale7AutoScaleSecondRule.png
[SetThirdRule]: ./media/web-sites-scale/scale8AutoScaleThirdRule.png
[SetRulesFinal]: ./media/web-sites-scale/scale9AutoScaleFinal.png
[ResourceGroup]: ./media/web-sites-scale/scale10ResourceGroup.png
[ScaleDatabase]: ./media/web-sites-scale/scale11SQLScale.png
[GeoReplication]: ./media/web-sites-scale/scale12SQLGeoReplication.png
 


