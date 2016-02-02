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

App Service プランが含まれているサービスのレベルに基づいています、 [* 料金プランの層 *](/pricing/details/app-service/)します。 **Standard** や **Premium** のように価格レベルを高くすると、Azure 上のリソースの使用方法を決定する際の堅牢性と柔軟性が向上します。 価格レベルを変更すると、サービスに含まれるコア数とメモリ容量が影響を受けます。これを*スケールアップ* (または*スケールダウン*) と呼びます。

価格レベルのスケールアップ以外にも、サービスに含まれるインスタンス数を増やすことができます。 これを*スケールアウト*または*スケールイン*と呼びます。 記事を参照してください [インスタンス数を手動または自動でスケール変更](../insights-how-to-scale.md) について詳しく学習する *スケール アウト* と *スケールイン*します。

App Service プランについては、を参照してください [App Service プランは何ですか?](../web-sites-web-hosting-plan-overview.md)。 と [Azure App Service プランの詳細な概要](azure-web-sites-web-hosting-plans-in-depth-overview.md)します。 料金情報や App Service プランの 1 つの機能では、次を参照してください。 [App Service の料金詳細](/pricing/details/app-service/)します。

最後に、スケーリングの動作が異なる場合は、専用に使用する [App Service 環境](app-service-app-service-environment-intro.md)します。 参照してください [App Service 環境で web アプリのスケーリング](app-service-web-scale-a-web-app-in-an-app-service-environment.md) 詳細です。
> [AZURE.NOTE] **Free** モードから **Basic**、**Standard**、または **Premium** モードに切り替える前に、Azure App Service のサブスクリプションに設定されている使用制限を解除する必要があります。 Microsoft Azure App Service サブスクリプションのオプションを表示または変更、次を参照してください。 [Microsoft Azure サブスクリプションの ][azuresubscriptions]します。

<a name="scalingsharedorbasic"></a>
<a name="scalingstandard"></a>

## 価格レベルのスケーリング

1. ブラウザーで開く、 [Azure ポータルの ][portal] 規模を設定するアプリを参照します。

2. アプリの **[Essentials]** で、**[App Service プラン/価格レベル]** リンクをクリックします。

3. **[価格レベル]** をクリックすると、プランに用意されているサービス レベルの一覧が表示されます。 各レベルには、そのレベルの平均コストを把握できるように、見積もり価格も表示されます。

    ![プランの選択](./media/app-service-scale/ChoosePricingTier.png)

4. レベルを選択したら、**[選択]** をクリックします。

    操作が完了すると、**[通知]** タブに緑色で "**成功**" が点滅します。

Azure での計算の別の階層についても習得できます [ここ](http://go.microsoft.com/fwlink/?LinkId=309169)します。

<a name="ScalingSQLServer"></a>
## スケーリングに関連するリソース

アプリが SQL や Storage などの他のサービスに依存している場合は、これらもニーズに合わせて拡張できます。

1. **[Essentials]** で、**[リソース グループ]** リンクをクリックします。

2. 次に、リソース グループ ブレードの **[概要]** パートで、データベースのいずれか (またはスケールする他のリソース) をクリックします。

    ![リンクされたデータベース](./media/app-service-scale/ResourceGroup.png)

3. リンクされているリソースのブレードで **[価格レベル]** パートをクリックし、パフォーマンス要件に応じていずれかの価格レベルを選択して、**[選択]** をクリックします。

    ![SQL Database のスケーリング](./media/app-service-scale/ScaleDatabase.png)

4. アプリで Storage を使用する場合、Storage をサポートしている価格レベルを選択すると、geo レプリケーションが自動的に設定します。 これに対して、SQL を使用する場合は、SQL Database の高可用性と障害復旧機能を向上させるために、geo レプリケーションを手動で構成する必要があります。 そのためには、**[Geo レプリケーション]** パートをクリックします。

    ![SQL Database の geo レプリケーションの設定](./media/app-service-scale/GeoReplication.png)

<a name="devfeatures"></a>
## 開発者機能

価格レベルに応じて、次の開発者向け機能を使用できます。

### ビット

- **Basic**、**Standard**、および **Premium** レベルでは、64 ビットおよび 32 ビットのアプリケーションがサポートされます。
- **Free** プラン レベルと **Shared** プラン レベルでは、32 ビットのアプリケーションのみがサポートされます。

### デバッガー サポート

- **Free**、**Shared**、および **Basic** モードでは、デバッガー サポートを利用する場合、App Service プランあたりの同時接続数は 1 です。
- **Standard** および **Premium** モードでは、デバッガー サポートを利用する場合、App Service プランあたりの同時接続数は 5 です。

<a name="OtherFeatures"></a>
## その他の機能

- すべての App Service で、その他の機能の詳細については料金など、プランとすべてのユーザー (開発者を含む)、関心のある機能を参照してください [App Service の料金詳細](/pricing/details/web-sites/)します。

>[AZURE.NOTE] 場合は、Azure アカウントがサインアップする前に Azure App Service の使用を開始するには、 [App Service の試用](http://go.microsoft.com/fwlink/?LinkId=523751), 、App Service で有効期間の短いスターター web アプリをすぐに作成する場所です。 このサービスの利用にあたり、クレジット カードは必要ありません。契約も必要ありません。

<a name="Next Steps"></a>
## 次のステップ

- Azure で開始するを参照してください。 [Microsoft Azure の無料試用版](/pricing/free-trial/)します。
- 料金、サポート、および SLA については、次のリンクを参照してください。

    [データ転送の料金詳細](/pricing/details/data-transfers/)

    [Microsoft Azure サポート プラン](/support/plans/)

    [サービス レベル アグリーメント](/support/legal/sla/)

    [SQL Database の料金詳細](/pricing/details/sql-database/)

    [仮想マシンおよびクラウド サービスの Microsoft Azure ][vmsizes]

    [App Service の料金詳細](/pricing/details/app-service/)

    [App Service の料金詳細 - SSL 接続](/pricing/details/web-sites/#ssl-connections)

- Azure App Service で拡張性と回復力のあるアーキテクチャの構築などのベスト プラクティスについては [ベスト プラクティス: Azure App Service Web Apps](http://blogs.msdn.com/b/windowsazure/archive/2014/02/10/best-practices-windows-azure-websites-waws.aspx)します。

- Web Apps の拡張に関するビデオ:

    - [タイミング - Stefan Schackow 共演のビデオは、Azure の web サイトの規模の設定](/documentation/videos/azure-web-sites-free-vs-standard-scaling/)
    - [自動スケール Azure Websites、CPU またはスケジュール済み - Stefan Schackow 共演](/documentation/videos/auto-scaling-azure-web-sites/)
    - [Azure の web サイト スケール - Stefan Schackow 共演](/documentation/videos/how-azure-web-sites-scale/)

## 変更内容

* Web サイトから App Service への変更のガイドを参照してください: [Azure App Service と既存の Azure サービスへの影響](http://go.microsoft.com/fwlink/?LinkId=529714)




[vmsizes]: http://go.microsoft.com/fwlink/?LinkId=309169 
[sqlaccountsbilling]: http://go.microsoft.com/fwlink/?LinkId=234930 
[azuresubscriptions]: http://go.microsoft.com/fwlink/?LinkID=235288 
[portal]: https://portal.azure.com/ 
[resourcegroup]: ./media/web-sites-scale/scale10ResourceGroup.png 
[scaledatabase]: ./media/web-sites-scale/scale11SQLScale.png 
[georeplication]: ./media/web-sites-scale/scale12SQLGeoReplication.png 

