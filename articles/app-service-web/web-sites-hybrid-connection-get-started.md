<properties 
    pageTitle="Azure App Service のハイブリッド接続を使用してオンプレミスのリソースにアクセスする" 
    description="静的 TCP ポートを使用するオンプレミスのリソースと Azure App Service の Web アプリケーションの間の接続を作成します。" 
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
    ms.date="11/24/2015" 
    ms.author="cephalin"/>

#Azure App Service のハイブリッド接続を使用してオンプレミスのリソースにアクセスする

Azure App Service の Web アプリケーションは、SQL Server、MySQL、HTTP Web APIs、Mobile Services、ほとんどのカスタム Web サービスなど、静的 TCP ポートを使用する任意のオンプレミスのリソースに接続できます。 ここでは、App Service の Web アプリケーションとオンプレミスの SQL Server データベース間のハイブリッド接続の作成方法を示します。

> [AZURE.NOTE] ハイブリッド接続機能の Web Apps 部分はでのみ使用できますが、 [Azure ポータル](https://portal.azure.com)します。 BizTalk サービスでの接続を作成するを参照してください。 [ハイブリッド接続](http://go.microsoft.com/fwlink/p/?LinkID=397274)します。  

## 前提条件
- Azure サブスクリプション。 無料サブスクリプションについては、次を参照してください。 [Azure 無料試用版](http://azure.microsoft.com/pricing/free-trial/)します。 

- ハイブリッド接続でオンプレミスの SQL Server または SQL Server Express のデータベースを使用するには、TCP/IP が静的ポートで有効になっている必要があります。 SQL Server は静的ポート 1433 を使用するため、SQL Server で既定のインスタンスを使用することをお勧めします。 インストールとハイブリッド接続で使用するための SQL Server Express の構成については、次を参照してください。 [ハイブリッド接続を使用して Azure の web サイトから内部設置型 SQL Server への接続](http://go.microsoft.com/fwlink/?LinkID=397979)します。

- 後でこの記事で説明するオンプレミスの Hybrid Connection Manager のエージェントをインストールするコンピューターの条件は次のとおりです。

    - ポート 5671 で Azure に接続できること
    - 到達できる必要があります、 *hostname*:*portnumber* 内部設置型リソースのです。 

> [AZURE.NOTE] この記事の手順では、内部設置型のハイブリッド接続のエージェントをホストするコンピューターからブラウザーを使用していることを前提としています。


## Azure ポータルで Web アプリを作成する ##

> [AZURE.NOTE] このチュートリアルで使用する場合、Azure ポータルで web アプリが既に場合スキップする [ハイブリッド接続および BizTalk サービスを作成](#CreateHC) から開始しています。

1. 左上隅にある、 [Azure ポータル](https://portal.azure.com), 、クリックして **新規** > **Web + モバイル** > **Web アプリ**します。
    
    ![新しい Web アプリ][NewWebsite]
    
2.  **Web アプリ** ブレードでは、URL を指定して、クリックして **作成**します。 
    
    ![Web サイト名][WebsiteCreationBlade]
    
3. しばらくすると、Web アプリケーションが作成され、Web アプリケーションのブレードが表示されます。 ブレードは縦方向にスクロールできるダッシュボードで、サイトを管理することができます。
    
    ![Web サイト実行][WebSiteRunningBlade]
    
4. クリックすると、ライブ サイトを確認する、 **参照** 既定のページを表示するアイコン。
    
    ![[参照] をクリックして Web アプリを確認する][Browse]
    
    ![既定の Web アプリ ページ][DefaultWebSitePage]
    
次に、Web アプリケーションに対するハイブリッド接続と BizTalk サービスを作成します。

<a name="CreateHC"></a>
## ハイブリッド接続および BizTalk サービスを作成する ##

1. Web アプリのブレードでクリックして **設定をすべて** > **ネットワーク** > **ハイブリッド接続エンドポイントを構成する**です。
    
    ![ハイブリッド接続][CreateHCHCIcon]
    
2. [ハイブリッド接続] ブレードをクリックして **追加**します。
    
    <!-- ![Add a hybrid connnection][CreateHCAddHC]
-->
    
3.  **ハイブリッド接続の追加** ブレードが開きます。  これは、最初のハイブリッド接続であるため、 **新しいハイブリッド接続** オプションが事前選択されていると、 **ハイブリッド接続を作成** ブレードが開きます。
    
    ![ハイブリッド接続の作成][TwinCreateHCBlades]
    
     **ハイブリッド接続の作成] ブレード**:
    -  **名前**, 、接続の名前を指定します。
    -  **Hostname**, 、リソースをホストする内部設置型コンピューターの名前を入力します。
    -  **ポート**, 、内部設置型リソースが (SQL Server の既定のインスタンスの場合は 1433) を使用するポート番号を入力します。
    - クリックして **Biz Talk サービス**


4.  **BizTalk サービスの作成** ブレードが開きます。 BizTalk サービスの名前を入力し、クリックして **OK**します。
    
    ![BizTalk サービスの作成][CreateHCCreateBTS]
    
     **BizTalk サービスの作成** ブレードが閉じに戻ったら、 **ハイブリッド接続を作成** ブレードです。
    
5. ハイブリッド接続を作成] ブレードでクリックして **OK**します。 
    
    ![[OK] をクリック][CreateBTScomplete]
    
6. 処理が完了すると、ポータルの通知領域に接続の作成が完了したことが通知されます。
    <!-TODO

    この手順ではすべてが失敗します。 新しいポータルでは BizTalk サービスを作成できません。 従来のポータルに切り替える
    BizTalk サービスを作成しましたが、接続はできないようです、
    ハイブリッド接続作成の手順を終了すると、次のエラーが表示されます。
    ハイブリッド接続 RelecIoudHC の作成に失敗しました。 この 
    リソース タイプは名前空間に見つかりませんでした。 
    'Microsoft.BizTaIkServices for api version 2014-06-01'.
    
    このエラーは、インスタンスではなく、タイプが見つからなかったことを示しています。
    ![Success notification][CreateHCSuccessNotification]
    -->
7. Web アプリのブレードで、 **ハイブリッド接続** アイコンが 1 のハイブリッド接続が作成されたことを示してしています。
    
    ![1 つのハイブリッド接続が作成された][CreateHCOneConnectionCreated]
    
これで、クラウド ハイブリッド接続のインフラストラクチャの重要な部分が完了しました。 次に、対応する内部設置型の部分を作成します。

<a name="InstallHCM"></a>
## オンプレミスのハイブリッド接続マネージャーをインストールして接続を完了する ##

1. Web アプリのブレードでクリックして **設定をすべて** > **ネットワーク** > **ハイブリッド接続エンドポイントを構成する**です。 
    
    ![ハイブリッド接続アイコン][HCIcon]
    
2.  **ハイブリッド接続** ブレードで、 **ステータス** 最近追加されたエンドポイントの表示の列 **接続されていない**します。 接続をクリックして構成します。
    
    ![Not connected][NotConnected]
    
    ハイブリッド接続ブレードが開きます。
    
    ![NotConnectedBlade][NotConnectedBlade]
    
3. ブレードでクリックして **リスナーのセットアップ**します。
    
    ![[リスナーのセットアップ] をクリック][ClickListenerSetup]
    
4.  **ハイブリッド接続のプロパティ** ブレードが開きます。  **内部設置型 Hybrid Connection Manager**, 、選択 **ここをクリックしてインストールする**です。
    
    ![ここをクリックしてインストール][ClickToInstallHCM]
    
5. アプリケーションの実行のセキュリティ警告ダイアログで選択 **実行** を続行します。
    
    ![[実行] を選択して続行][ApplicationRunWarning]
    
6.   **ユーザー アカウント制御** ] ダイアログ ボックスで選択 **はい**します。
    
    ![[はい] を選択][UAC]
    
7. Hybrid Connection Manager がダウンロードされ、インストールされます。 
    
    ![インストール中][HCMInstalling]
    
8. インストールが完了したら、クリックして **閉じる**します。
    
    ![[閉じる] をクリックしてください][HCMInstallComplete]
    
     **ハイブリッド接続** ブレードで、 **ステータス** 列が表示されます **接続**します。 
    
    ![接続されている状態][HCStatusConnected]

これで、ハイブリッド接続のインフラストラクチャが完成しました。この接続を使用するハイブリッド アプリケーションを作成できます。 

>[AZURE.NOTE] 場合は、Azure アカウントがサインアップする前に Azure App Service の使用を開始するには、 [App Service の試用](http://go.microsoft.com/fwlink/?LinkId=523751), 、App Service で有効期間の短いスターター web アプリをすぐに作成する場所です。 このサービスの利用にあたり、クレジット カードは必要ありません。契約も必要ありません。

<a name="NextSteps"></a>
## 次のステップ ##

- ハイブリッド接続を使用する ASP.NET web アプリケーションの作成方法の詳細については、次を参照してください。 [ハイブリッド接続を使用して Azure の web サイトから内部設置型 SQL Server への接続](http://go.microsoft.com/fwlink/?LinkID=397979)します。

- モバイル サービスでハイブリッド接続の使用方法の詳細については、次を参照してください。 [ハイブリッド接続を使用して Azure のモバイル サービスから内部設置型 SQL Server への接続](../mobile-services-dotnet-backend-hybrid-connections-get-started.md)します。

### その他のリソース

[ハイブリッド接続の概要](http://go.microsoft.com/fwlink/p/?LinkID=397274)

[Josh Twist introduces hybrid connections (Josh Twist によるハイブリッド接続の紹介) (チャネル 9 ビデオ)](http://channel9.msdn.com/Shows/Azure-Friday/Josh-Twist-introduces-hybrid-connections)

[ハイブリッド接続の Web サイト](http://azure.microsoft.com/services/biztalk-services/)

[BizTalk サービス: [ダッシュボード]、[監視]、[スケール]、[構成]、および [ハイブリッド接続] タブ](../biztalk-dashboard-monitor-scale-tabs/)

[シームレスなアプリケーションの移植性を使用して実際のハイブリッド クラウドをビルドする (チャネル 9 ビデオ)](http://channel9.msdn.com/events/TechEd/NorthAmerica/2014/DCIM-B323#fbid=)

[Connect to an on-premises SQL Server from Azure Mobile Services using Hybrid Connections (ハイブリッド接続を使用して Azure Mobile Services から内部設置型の SQL Server に接続する) (チャネル 9 ビデオ)](http://channel9.msdn.com/Series/Windows-Azure-Mobile-Services/Connect-to-an-on-premises-SQL-Server-from-Azure-Mobile-Services-using-Hybrid-Connections)

## 変更内容
* Web サイトから App Service への変更のガイドを参照してください: [Azure App Service と既存の Azure サービスへの影響](http://go.microsoft.com/fwlink/?LinkId=529714)

<!-- IMAGES -->
[New]:./media/web-sites-hybrid-connection-get-started/B01New.png
[NewWebsite]:./media/web-sites-hybrid-connection-get-started/B02NewWebsite.png
[WebsiteCreationBlade]:./media/web-sites-hybrid-connection-get-started/B03WebsiteCreationBlade.png
[WebSiteRunningBlade]:./media/web-sites-hybrid-connection-get-started/B04WebSiteRunningBlade.png
[Browse]:./media/web-sites-hybrid-connection-get-started/B05Browse.png
[DefaultWebSitePage]:./media/web-sites-hybrid-connection-get-started/B06DefaultWebSitePage.png
[CreateHCHCIcon]:./media/web-sites-hybrid-connection-get-started/C01CreateHCHCIcon.png
[CreateHCAddHC]:./media/web-sites-hybrid-connection-get-started/C02CreateHCAddHC.png
[TwinCreateHCBlades]:./media/web-sites-hybrid-connection-get-started/C03TwinCreateHCBlades.png
[CreateHCCreateBTS]:./media/web-sites-hybrid-connection-get-started/C04CreateHCCreateBTS.png
[CreateBTScomplete]:./media/web-sites-hybrid-connection-get-started/C05CreateBTScomplete.png
[CreateHCSuccessNotification]:./media/web-sites-hybrid-connection-get-started/C06CreateHCSuccessNotification.png
[CreateHCOneConnectionCreated]:./media/web-sites-hybrid-connection-get-started/C07CreateHCOneConnectionCreated.png
[HCIcon]:./media/web-sites-hybrid-connection-get-started/D01HCIcon.png
[NotConnected]:./media/web-sites-hybrid-connection-get-started/D02NotConnected.png
[NotConnectedBlade]:./media/web-sites-hybrid-connection-get-started/D03NotConnectedBlade.png
[ClickListenerSetup]:./media/web-sites-hybrid-connection-get-started/D04ClickListenerSetup.png
[ClickToInstallHCM]:./media/web-sites-hybrid-connection-get-started/D05ClickToInstallHCM.png
[ApplicationRunWarning]:./media/web-sites-hybrid-connection-get-started/D06ApplicationRunWarning.png
[UAC]:./media/web-sites-hybrid-connection-get-started/D07UAC.png
[HCMInstalling]:./media/web-sites-hybrid-connection-get-started/D08HCMInstalling.png
[HCMInstallComplete]:./media/web-sites-hybrid-connection-get-started/D09HCMInstallComplete.png
[HCStatusConnected]:./media/web-sites-hybrid-connection-get-started/D10HCStatusConnected.png
 

