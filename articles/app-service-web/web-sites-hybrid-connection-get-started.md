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


# Azure App Service のハイブリッド接続を使用してオンプレミスのリソースにアクセスする

Azure App Service の Web アプリケーションは、SQL Server、MySQL、HTTP Web APIs、Mobile Services、ほとんどのカスタム Web サービスなど、静的 TCP ポートを使用する任意のオンプレミスのリソースに接続できます。 ここでは、App Service の Web アプリケーションとオンプレミスの SQL Server データベース間のハイブリッド接続の作成方法を示します。
> [AZURE.NOTE] ハイブリッド接続機能の Web Apps 部分はでのみ使用できますが、 [Azure ポータル](https://portal.azure.com)します。 BizTalk サービスでの接続を作成するを参照してください。 [ハイブリッド接続](http://go.microsoft.com/fwlink/p/?LinkID=397274)します。  

## 前提条件

- Azure サブスクリプション。 無料サブスクリプションについては、次を参照してください。 [Azure 無料試用版](http://azure.microsoft.com/pricing/free-trial/)します。

- ハイブリッド接続でオンプレミスの SQL Server または SQL Server Express のデータベースを使用するには、TCP/IP が静的ポートで有効になっている必要があります。 SQL Server は静的ポート 1433 を使用するため、SQL Server で既定のインスタンスを使用することをお勧めします。 インストールとハイブリッド接続で使用するための SQL Server Express の構成については、次を参照してください。 [ハイブリッド接続を使用して Azure の web サイトから内部設置型 SQL Server への接続](http://go.microsoft.com/fwlink/?LinkID=397979)します。

- 後でこの記事で説明するオンプレミスの Hybrid Connection Manager のエージェントをインストールするコンピューターの条件は次のとおりです。

    - ポート 5671 で Azure に接続できること
    - オンプレミスのリソースの *hostname*:*portnumber* に到達できること

> [AZURE.NOTE] この記事の手順では、オンプレミスのハイブリッド接続のエージェントをホストするコンピューターからブラウザーを使用していると想定しています。


## Azure ポータルで Web アプリを作成する

> [AZURE.NOTE] このチュートリアルで使用する場合、Azure ポータルで web アプリが既に場合スキップする [ハイブリッド接続および BizTalk サービスを作成](#CreateHC) から開始しています。

1. 左上隅にある、 [Azure ポータル](https://portal.azure.com), 、クリックして **新規** > **Web + モバイル** > **Web アプリ**します。

    ![新しい Web アプリ][newwebsite]

2. **[Web アプリ]** ブレードで、URL を入力し、**[作成]** をクリックします。

    ![Web サイト名][websitecreationblade]

3. しばらくすると、Web アプリケーションが作成され、Web アプリケーションのブレードが表示されます。 ブレードは縦方向にスクロールできるダッシュボードで、サイトを管理することができます。

    ![Website running][websiterunningblade]

4. サイトがライブかどうかを確認するには、**[参照]** アイコンをクリックして、既定のページを表示します。

    ![[参照] をクリックして Web アプリを確認する][browse]

    ![既定の Web アプリ ページ][defaultwebsitepage]

次に、Web アプリケーションに対するハイブリッド接続と BizTalk サービスを作成します。

<a name="CreateHC"></a>
## ハイブリッド接続および BizTalk サービスを作成する

1. Web アプリ ブレードで、**[すべての設定]**、**[ネットワーク]**、**[ハイブリッド接続エンドポイントを構成する]** の順にクリックします。

    ![ハイブリッド接続][createhchcicon]

2. ハイブリッド接続ブレードで、**[追加]** をクリックします。


3. **[ハイブリッド接続の追加]** ブレードが開きます。 これは最初のハイブリッド接続であるため、**[新しいハイブリッド接続]** があらかじめ選択され、**[ハイブリッド接続の作成]** ブレードが開きます。

    ![Create a hybrid connection][twincreatehcblades]

    **[ハイブリッド接続の作成] ブレード**で、次の手順を実行します。
    - **[名前]** に、接続の名前を入力します。
    - **[ホスト名]** に、リソースをホストする内部設置型のコンピューターの名前を入力します。
    - **[ポート]** には、内部設置型のリソースが使用するポート番号 (SQL Server の既定のインスタンスの場合は 1433) を入力します。
    - **[BizTalk サービス]** をクリックします。

4. **[BizTalk サービスの作成]** ブレードが開きます。 BizTalk サービスの名前を入力し、**[OK]** をクリックします。

    ![BizTalk サービスの作成][createhccreatebts]

    **[BizTalk サービスの作成]** ブレードが閉じ、**[ハイブリッド接続の作成]** ブレードに戻ります。

5. [ハイブリッド接続の作成] ブレードで、**[OK]** をクリックします。

    ![[OK] をクリック][createbtscomplete]

6. 処理が完了すると、ポータルの通知領域に接続の作成が完了したことが通知されます。

7. Web アプリのブレードの **[ハイブリッド接続]** アイコンは、ハイブリッド接続が 1 つ作成されたことを示しています。

    ![1 つのハイブリッド接続が作成された][createhconeconnectioncreated]

これで、クラウド ハイブリッド接続のインフラストラクチャの重要な部分が完了しました。 次に、対応するオンプレミスの部分を作成します。

<a name="InstallHCM"></a>
## オンプレミスのハイブリッド接続マネージャーをインストールして接続を完了する

1. Web アプリのブレードで、**[すべての設定]**、**[ネットワーク]**、**[ハイブリッド接続エンドポイントを構成する]** の順にクリックします。

    ![ハイブリッド接続アイコン][hcicon]

2. **[ハイブリッド接続]** ブレードで、最近追加されたエンドポイントの **[状態]** 列に **[未接続]** と表示されています。 接続をクリックして構成します。

    ![Not connected][notconnected]

    ハイブリッド接続ブレードが開きます。

    ![NotConnectedBlade][notconnectedblade]

3. ブレードで、**[リスナーのセットアップ]** をクリックします。

    ![Click Listener Setup][clicklistenersetup]

4. **[ハイブリッド接続のプロパティ]** ブレードが開きます。 **内部設置型の Hybrid Connection Manager** で、**[インストールするにはここをクリックします]** をクリックします。

    ![Click here to install][clicktoinstallhcm]

5. [アプリケーションの実行 - セキュリティの警告] ダイアログで、**[実行]** を選択します。

    ![Choose Run to continue][applicationrunwarning]

6.  **[ユーザー アカウント制御]** ダイアログで、**[はい]** を選択します。

    ![Choose Yes][uac]

7. Hybrid Connection Manager がダウンロードされ、インストールされます。

    ![Installing][hcminstalling]

8. インストールが完了したら、**[閉じる]** をクリックします。

    ![Click Close][hcminstallcomplete]

    **[ハイブリッド接続]** ブレードで、**[状態]** 列に **[接続]** と表示されています。

    ![Connected Status][hcstatusconnected]

これで、ハイブリッド接続のインフラストラクチャが完成しました。この接続を使用するハイブリッド アプリケーションを作成できます。
>[AZURE.NOTE] 場合は、Azure アカウントがサインアップする前に Azure App Service の使用を開始するには、 [App Service の試用](http://go.microsoft.com/fwlink/?LinkId=523751), 、App Service で有効期間の短いスターター web アプリをすぐに作成する場所です。 このサービスの利用にあたり、クレジット カードは必要ありません。契約も必要ありません。

<a name="NextSteps"></a>
## 次のステップ

- ハイブリッド接続を使用する ASP.NET web アプリケーションの作成方法の詳細については、次を参照してください。 [ハイブリッド接続を使用して Azure の web サイトから内部設置型 SQL Server への接続](http://go.microsoft.com/fwlink/?LinkID=397979)します。

- モバイル サービスでハイブリッド接続の使用方法の詳細については、次を参照してください。 [ハイブリッド接続を使用して Azure のモバイル サービスから内部設置型 SQL Server への接続](../mobile-services-dotnet-backend-hybrid-connections-get-started.md)します。

### その他のリソース

[ハイブリッド接続の概要](http://go.microsoft.com/fwlink/p/?LinkID=397274)

[Josh twist によるハイブリッド接続 (チャネル 9 ビデオ) が導入されています](http://channel9.msdn.com/Shows/Azure-Friday/Josh-Twist-introduces-hybrid-connections)

[ハイブリッド接続の web サイト](http://azure.microsoft.com/services/biztalk-services/)

[BizTalk サービス: ダッシュ ボード、モニター、スケール、構成、およびハイブリッド接続] タブ](../biztalk-dashboard-monitor-scale-tabs/)

[シームレスなアプリケーションの移植性 (Channel 9 のビデオ) と実際のハイブリッド クラウドの構築](http://channel9.msdn.com/events/TechEd/NorthAmerica/2014/DCIM-B323#fbid=)

[ハイブリッド接続 (チャネル 9 ビデオ) を使用して Azure Mobile Services から内部設置型 SQL Server に接続します。](http://channel9.msdn.com/Series/Windows-Azure-Mobile-Services/Connect-to-an-on-premises-SQL-Server-from-Azure-Mobile-Services-using-Hybrid-Connections)

## 変更内容

* Web サイトから App Service への変更のガイドを参照してください: [Azure App Service と既存の Azure サービスへの影響](http://go.microsoft.com/fwlink/?LinkId=529714)



[new]: ./media/web-sites-hybrid-connection-get-started/B01New.png 
[newwebsite]: ./media/web-sites-hybrid-connection-get-started/B02NewWebsite.png 
[websitecreationblade]: ./media/web-sites-hybrid-connection-get-started/B03WebsiteCreationBlade.png 
[websiterunningblade]: ./media/web-sites-hybrid-connection-get-started/B04WebSiteRunningBlade.png 
[browse]: ./media/web-sites-hybrid-connection-get-started/B05Browse.png 
[defaultwebsitepage]: ./media/web-sites-hybrid-connection-get-started/B06DefaultWebSitePage.png 
[createhchcicon]: ./media/web-sites-hybrid-connection-get-started/C01CreateHCHCIcon.png 
[createhcaddhc]: ./media/web-sites-hybrid-connection-get-started/C02CreateHCAddHC.png 
[twincreatehcblades]: ./media/web-sites-hybrid-connection-get-started/C03TwinCreateHCBlades.png 
[createhccreatebts]: ./media/web-sites-hybrid-connection-get-started/C04CreateHCCreateBTS.png 
[createbtscomplete]: ./media/web-sites-hybrid-connection-get-started/C05CreateBTScomplete.png 
[createhcsuccessnotification]: ./media/web-sites-hybrid-connection-get-started/C06CreateHCSuccessNotification.png 
[createhconeconnectioncreated]: ./media/web-sites-hybrid-connection-get-started/C07CreateHCOneConnectionCreated.png 
[hcicon]: ./media/web-sites-hybrid-connection-get-started/D01HCIcon.png 
[notconnected]: ./media/web-sites-hybrid-connection-get-started/D02NotConnected.png 
[notconnectedblade]: ./media/web-sites-hybrid-connection-get-started/D03NotConnectedBlade.png 
[clicklistenersetup]: ./media/web-sites-hybrid-connection-get-started/D04ClickListenerSetup.png 
[clicktoinstallhcm]: ./media/web-sites-hybrid-connection-get-started/D05ClickToInstallHCM.png 
[applicationrunwarning]: ./media/web-sites-hybrid-connection-get-started/D06ApplicationRunWarning.png 
[uac]: ./media/web-sites-hybrid-connection-get-started/D07UAC.png 
[hcminstalling]: ./media/web-sites-hybrid-connection-get-started/D08HCMInstalling.png 
[hcminstallcomplete]: ./media/web-sites-hybrid-connection-get-started/D09HCMInstallComplete.png 
[hcstatusconnected]: ./media/web-sites-hybrid-connection-get-started/D10HCStatusConnected.png 

