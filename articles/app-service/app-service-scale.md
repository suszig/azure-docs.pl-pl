<properties 
    pageTitle="Azure App Service における価格レベルのスケール" 
    description="Azure App Service で Web Apps、Mobile Apps、API Apps、Logic Apps をスケールする方法 (自動スケールを含む) について説明します。" 
    services="app-service" 
    documentationCenter="" 
    authors="stepsic-microsoft-com" 
    manager="wpickett" 
    editor="mollybos"/>

<tags 
    ms.service="app-service" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/29/2015" 
    ms.author="stepsic"/>

# Azure App Service における価格レベルのスケール

パフォーマンスの向上と Microsoft Azure でのアプリのスループットは、使用することができます、 [Azure ポータル](https://portal.azure.com/) から App Service プランのスケール アップする **Free** に **Shared**, 、**基本的な**, 、**標準**, 、または **Premium**します。 

App Service プランが含まれているサービスのレベルに基づいています、 [*価格レベル* 計画の](/pricing/details/app-service/)です。 などの高い価格レベル、 **標準** と **Premium**, 、堅牢性と Azure 上のリソースの使用方法を決定する柔軟性が向上します。 数に影響を与える価格レベルを変更するコアとメモリの量、サービスに含まれるとすると呼ばれる *スケール アップ* (または *スケール ダウン*)。

価格レベルのスケールアップ以外にも、サービスに含まれるインスタンス数を増やすことができます。 呼ばれる *のスケール アウト* または *スケールイン*します。 記事を参照してください [インスタンス数を手動または自動でスケール変更](../insights-how-to-scale.md) について詳しく学習する *スケール アウト* と *スケールイン*します。

App Service プランについては、次を参照してください。 [App Service プランは何ですか。](../web-sites-web-hosting-plan-overview.md)  [Azure App Service プランの詳細な概要](azure-web-sites-web-hosting-plans-in-depth-overview.md)します。 料金情報および各 App Service プランの機能では、「 [App Service の料金詳細](/pricing/details/app-service/)します。

最後に、スケーリングの動作が異なる場合は、専用に使用する [App Service 環境](app-service-app-service-environment-intro.md)します。 参照してください [App Service 環境で web アプリのスケーリング](app-service-web-scale-a-web-app-in-an-app-service-environment.md) 詳細です。

> [AZURE.NOTE] 切り替える前に、 **Free** モード **基本的な**, 、**標準**, 、または **Premium** モードでは、Azure App Service サブスクリプションのインプレース支出 cap を削除する必要があります最初です。 Microsoft Azure App Service サブスクリプションのオプションを表示または変更、次を参照してください。 [Microsoft Azure サブスクリプションの][azuresubscriptions]です。

<a name="scalingsharedorbasic"></a>
<a name="scalingstandard"></a>

## 価格レベルのスケーリング

1. ブラウザーで開く、 [Azure ポータル][portal] 規模を設定するアプリを参照します。
    
2.  **Essentials** 、アプリをクリックして、 **App Service プラン/価格レベル** リンクします。

3. クリックして **価格レベル**, 、プランの使用可能なサービス レベルの一覧が表示されます。 各レベルには、そのレベルの平均コストを把握できるように、見積もり価格も表示されます。 
    
    ![プランの選択](./media/app-service-scale/ChoosePricingTier.png)
    
4. クリックして、層を選択すると **選択**します。
    
     **通知** ] タブ、緑色の点滅は **成功** 、操作が完了します。
 
Azure での計算の別の階層についても習得できます [ここ](http://go.microsoft.com/fwlink/?LinkId=309169)します。
    
<a name="ScalingSQLServer"></a>
##スケーリングに関連するリソース
アプリが SQL や Storage などの他のサービスに依存している場合は、これらもニーズに合わせて拡張できます。

1.  **Essentials**, をクリックして、 **リソース グループ** リンクします。

2. 次に、 **概要** リソース グループ ブレードで、データベース (またはスケールする他のリソース) のクリックされた 1 つの一部です。

    ![リンクされたデータベース](./media/app-service-scale/ResourceGroup.png)
    
3. いるリンク済みリソースのブレードをクリックして、 **価格レベル** パート、パフォーマンス要件に基づき層のいずれかを選択してクリックして **選択**します。 
    
    ![SQL Database のスケーリング](./media/app-service-scale/ScaleDatabase.png)
    
4. アプリで Storage を使用する場合、Storage をサポートしている価格レベルを選択すると、geo レプリケーションが自動的に設定します。 これに対して、SQL を使用する場合は、SQL Database の高可用性と障害復旧機能を向上させるために、geo レプリケーションを手動で構成する必要があります。 これを行うには、クリックして、 **Geo レプリケーション** 部分です。
    
    ![SQL Database の geo レプリケーションの設定](./media/app-service-scale/GeoReplication.png)
    
<a name="devfeatures"></a>
## 開発者機能
価格レベルに応じて、次の開発者向け機能を使用できます。

### ビット ###

-  **基本的な**, 、**標準**, 、および **Premium** レベル 64 ビットおよび 32 ビット アプリケーションがサポートされます。
-  **Free** と **Shared** 層 32 ビット アプリケーションのみがサポートを計画します。

### デバッガー サポート ###

- デバッガーのサポートは提供されて、 **Free**, 、**共有**, 、および **基本的な** モードでは、App Service プランあたりの同時接続数 1 です。
- デバッガーのサポートは提供されて、 **標準** と **Premium** モードでは、App Service プランあたり 5 つの同時接続します。

<a name="OtherFeatures"></a>
## その他の機能

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
    
    [App Service の料金の詳細](/pricing/details/app-service/)
    
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
[ResourceGroup]: ./media/web-sites-scale/scale10ResourceGroup.png
[ScaleDatabase]: ./media/web-sites-scale/scale11SQLScale.png
[GeoReplication]: ./media/web-sites-scale/scale12SQLGeoReplication.png
 

