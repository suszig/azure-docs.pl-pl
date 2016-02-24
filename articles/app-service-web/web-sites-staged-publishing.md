<properties
    pageTitle="Azure App Service の Web アプリのステージング環境を設定する"
    description="Azure App Service の Web アプリのステージングされた発行を使用する方法について説明します。"
    services="app-service"
    documentationCenter=""
    authors="cephalin"
    writer="cephalin"
    manager="wpickett"
    editor="mollybos"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/21/2015"
    ms.author="cephalin"/>

# Azure App Service の Web アプリのステージング環境を設定する
<a name="Overview"></a>

Web アプリを展開するときに [App Service](http://go.microsoft.com/fwlink/?LinkId=529714), で実行する場合は、既定の運用スロットではなく別のデプロイ スロットを展開することができます、 **標準** または **Premium** App Service プラン モードです。 デプロイ スロットは、実際には固有のホスト名を持つライブ Web アプリです。 Web アプリのコンテンツと構成の各要素は、(運用スロットを含む) 2 つのデプロイ スロットの間でスワップすることができます。 デプロイ スロットにアプリケーションをデプロイすることには、次のメリットがあります。

- ステージング デプロイ スロットで Web アプリの変更を検証した後に、運用スロットにスワップできます。

- スロットに Web アプリをデプロイした後に運用サイトにスワップすると、運用サイトへのスワップ前にスロットのすべてのインスタンスが準備されます。 これにより、Web アプリをデプロイする際のダウンタイムがなくなります。 トラフィックのリダイレクトはシームレスであるため、スワップ操作によりドロップされる要求はありません。 このワークフロー全体を構成することで自動化できます [自動スワップ](#configure-auto-swap-for-your-web-app) スワップ前の検証が必要ない場合。

- スワップ後も、以前のステージング Web アプリ スロットに元の運用 Web アプリが残っているため、 運用スロットにスワップした変更が想定どおりでない場合は、適切な動作が確認されている元のサイトにすぐに戻すことができます。

サポートされるデプロイ スロット数は、App Service のプラン モードごとに異なります。 Web アプリのモードのサポートのスロットの数を確認するは、「 [App Service の料金](/pricing/details/app-service/)します。

- Web アプリに複数のスロットがある場合は、モードを変更することはできません。

- スケーリングは

- リンク済みリソースの管理は運用サイト スロットでのみサポートされています。  [Azure ポータル](http://go.microsoft.com/fwlink/?LinkId=529715) のみ、一時的に非運用スロットを異なる App Service プラン モードを変更してこの運用スロットに対する影響を回避できます。 なお、2 つのスロットをスワップする前には、非運用スロットと運用スロットを再度同じモードにする必要があります。


[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

<a name="Add"></a>
## Web アプリにデプロイ スロットを追加する ##

Web アプリで実行されている必要があります、 **標準** または **Premium** モードを複数のデプロイメント スロットを有効にするためにします。

1.  [Azure ポータル](https://portal.azure.com/), 、web アプリのブレードを開きます。
2. クリックして **デプロイ スロット**します。 次に、 **デプロイ スロット** ブレードで、をクリックして **スロットの追加**します。

    ![新しい展開スロットの追加][QGAddNewDeploymentSlot]

    > [AZURE.NOTE]
    > Web アプリではない場合、 **標準** または **Premium** モードでは、ステージングされた発行を有効にするためのサポートされているモードを示すメッセージが表示されます。 この時点では、選択するオプションがある **アップグレード** に移動し、 **スケール** 続行する前に、web アプリのタブをクリックします。

2.  **スロットを追加する** ブレードで、スロットの名前を指定し、もう 1 つの既存のデプロイ スロットから web アプリ構成のクローンを作成するかどうかを選択します。 チェック マークをクリックして続行します。

    ![構成ソース][ConfigurationSource1]

    初めてスロットを追加する場合、選択肢は 2 つのみです。運用環境内の既定のスロットから構成を複製するか、または複製しないかのどちらかです。

    複数のスロットを作成すると、運用サイトのスロット以外のスロットから構成を複製できるようになります。

    ![構成ソース][MultipleConfigurationSources]

5.  **デプロイ スロット** ブレードで、[デプロイ スロットを一連のメトリックとその他の web アプリと同じように構成で、スロットのブレードを開く] をクリックします。 **your-web-app-name-deployment-slot-name** 展開スロットが表示されていることを通知する、ブレードの上部に表示されます。

    ![展開スロットのタイトル][StagingTitle]

5. スロットのブレードで、アプリの URL をクリックします。 デプロイ スロットが独自のホスト名を持つライブ アプリであることに注意してください。 デプロイ スロットへのパブリック アクセスを制限するため、次を参照してください。 [App Service Web App-運用環境以外のデプロイ スロットへの web アクセスの禁止](http://ruslany.net/2014/04/azure-web-sites-block-web-access-to-non-production-deployment-slots/)します。

デプロイ スロットを作成した直後は、コンテンツはありません。 スロットには、異なるリポジトリ分岐、またはまったく異なるリポジトリからデプロイできます。 またスロットの構成を変更することもできます。 コンテンツの更新には、デプロイ スロットに関連付けられた発行プロファイルまたはデプロイ資格情報を使用してください。  たとえば、 [git を使用してこのスロットに発行](web-sites-publish-source-control.md)します。

<a name="AboutConfiguration"></a>
## デプロイ スロットの構成 ##
別のデプロイ スロットから構成を複製する場合、複製された構成を編集することができます。 なお、スワップ先のコンテンツに反映される構成要素 (非スロット固有) もあれば、スワップ後にも同じスロットに維持される構成要素 (スロット固有) もあります。 次の一覧では、スロットのスワップ時に変更される構成を示します。

**スワップされる設定**:

- 一般設定 - フレームワーク バージョン、32/64 ビット、Web ソケットなど
- アプリ設定 (スロット固有として構成可能)
- 接続設定 (スロット固有として構成可能)
- ハンドラー マッピング
- 監視と診断の設定
- Web ジョブ コンテンツ

**スワップされない設定**:

- 発行エンドポイント
- カスタム ドメイン名
- SSL 証明書とバインド
- スケールの設定
- Web ジョブ スケジューラ

アプリ設定や接続文字列を (スワップされない) スロット固有の構成を行う、 **アプリケーション設定** 特定のスロットのブレードを選択し、 **スロット設定** 、スロットが固有の構成要素のボックスです。 構成要素をスロット固有としてマークすると、その Web アプリに関連付けられたすべてのデプロイ スロットにわたってその要素がスワップできなくなります。

![スロット設定][SlotSettings]

<a name="Swap"></a>
## デプロイ スロットをスワップするには ##

>[AZURE.IMPORTANT] デプロイ スロットから web アプリを運用環境にスワップする前に、スワップのターゲットである場合と同じように、すべての非スロット固有の設定が構成されていることを確認します。

1. デプロイ スロットをスワップするにはクリックして、 **スワップ** web アプリのコマンド バーまたはデプロイ スロットのコマンド バーのボタンをクリックします。 スワップのソースとターゲットが正しく設定されていることを確認してください。 通常、スワップ ターゲットは運用スロットになります。  

    ![スワップ] ボタン][SwapButtonBar]

3. をクリックして **OK** 操作を完了します。 操作が完了すると、デプロイ スロットはスワップされています。

## Web アプリの自動スワップの構成 ##

自動スワップを使用すると、Web アプリのエンドユーザーにコールド スタートやダウンタイムを強制することなく、Web アプリを連続的にデプロイできる、効率的な DevOps シナリオを実現できます。 デプロイ スロットを運用環境に自動スワップするよう構成した場合、そのスロットにコードの更新をプッシュすると、Web アプリがスロット内で起動されしだい、App Service によって自動的に運用環境へとスワップされます。

>[AZURE.IMPORTANT] を有効にした自動スワップ スロットの場合は、スロット構成がターゲット スロット (通常は運用スロット) のためのもので構成を確認します。

スロットの自動スワップは簡単に構成できます。 次の手順に従ってください。

1.  **デプロイ スロット** ブレードで、選択非運用スロットをクリックして **すべての設定** そのスロットのブレードにします。  

    ![Autoswap1]

2. クリックして **アプリケーション設定**します。 選択 **に** の **自動スワップ**, で目的のターゲット スロットを選択して **自動スワップ スロット**, 、] をクリック **保存** コマンド バーでします。 スロットの構成が、ターゲット スロットに反映させたいとおりの構成になっていることを確認してください。

     **通知** ] タブ、緑色の点滅は **成功** 、操作が完了します。

    ![Autoswap2]

    >[AZURE.NOTE] Web アプリの自動スワップをテストするでの運用環境以外のターゲット スロット最初に選択できます **自動スワップ スロット** 機能を理解します。  

3. デプロイ スロットにコードをプッシュします。 しばらくすると自動スワップが実行され、更新がターゲット スロットの URL に反映されます。

<a name="Multi-Phase"></a>
## Web アプリに複数フェーズのスワップを使用する ##

複数フェーズのスワップを使用すると、スロット固有として設計された構成要素 (接続文字列など) のコンテキストで検証を簡素化できます。 この場合、スワップが実際に有効になる前に、スワップ ターゲットのこのような構成要素をスワップ ソースに適用して検証すると便利です。 スワップ ターゲットの構成要素をスワップ ソースに適用した後は、実行可能な操作として、スワップを完了するか、スワップ ソースの元の構成に戻します。元の構成に戻すと、スワップをキャンセルできます。 複数フェーズのスワップに使用できる Azure PowerShell コマンドレットのサンプルについては、「デプロイ スロット用の Azure PowerShell コマンドレット」をご覧ください。

<a name="Rollback"></a>
## スワップ後に運用環境のアプリをロールバックするには ##
スロットのスワップ後に運用サイトでエラーが見つかった場合は、同じ 2 つのスロットをすぐにスワップして、スロットをスワップ前の状態にロールバックすることができます。

<a name="Delete"></a>
## デプロイ スロットを削除するには##

デプロイ スロットのブレードで、次のようにクリックします。 **削除** コマンド バーにします。  

![デプロイ スロットを削除][DeleteStagingSiteButton]

<!-- ======== AZURE POWERSHELL CMDLETS =========== -->

<a name="PowerShell"></a>
## デプロイ スロット用の Azure PowerShell コマンドレット

Azure PowerShell は、Windows PowerShell から Azure を管理するためのコマンドレットを提供するモジュールです (Azure App Service の Web アプリ デプロイ スロットを管理するためのサポートなど)。

- インストールと Azure PowerShell を構成して、Azure サブスクリプションに Azure PowerShell の認証については、次を参照してください。 [をインストールして、Microsoft Azure PowerShell を構成する方法](../install-configure-powershell.md)します。  

- PowerShell コマンドレットの新しい Azure リソース マネージャー モードを使用するには、`Switch-AzureMode -Name AzureResourceManager` で開始します。

----------

### Web アプリの作成

`New-AzureWebApp -ResourceGroupName [resource group name] -Name [web app name] -Location [location] -AppServicePlan [app service plan name]`

----------

### Web アプリのデプロイ スロットを作成する

`New-AzureWebApp -ResourceGroupName [resource group name] -Name [web app name] -SlotName [deployment slot name] -Location [location] -AppServicePlan [app service plan name]`

----------

### 複数フェーズのスワップを開始し、ターゲット スロットの構成をソース スロットに適用する

`$ParametersObject = @{targetSlot  = "[slot name – e.g. “production”]"}`
`Invoke-AzureResourceAction -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots -ResourceName [web app name]/[slot name] -Action applySlotConfig -Parameters $ParametersObject -ApiVersion 2015-07-01`

----------

### 複数フェーズのスワップの最初のフェーズを元に戻し、ソース スロットの構成を復元する

`Invoke-AzureResourceAction -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots -ResourceName [web app name]/[slot name] -Action resetSlotConfig -ApiVersion 2015-07-01`

----------

### デプロイ スロットをスワップする

`$ParametersObject = @{targetSlot  = "[slot name – e.g. “production”]"}`
`Invoke-AzureResourceAction -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots -ResourceName [web app name]/[slot name] -Action slotsswap -Parameters $ParametersObject -ApiVersion 2015-07-01`

----------

### デプロイ スロットを削除する

`Remove-AzureResource -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots –Name [web app name]/[slot name] -ApiVersion 2015-07-01`

----------

<!-- ======== Azure CLI =========== -->

<a name="CLI"></a>
## デプロイ スロット用の Azure コマンド ライン インターフェイス (Azure CLI) コマンド

Azure CLI には、Web アプリ デプロイ スロットの管理のサポートなど、Azure を使用するためのクロス プラットフォーム コマンドが用意されています。

- インストールして、Azure サブスクリプションに Azure CLI を接続する方法に関する情報を含めて、Azure CLI の構成手順についてを参照してください。 [のインストールと Azure CLI の構成](../xplat-cli-install.md)します。

-  Azure CLI で Azure App Service に使用できるコマンドの一覧を表示するには、`azure site -h` を呼び出してください。

----------
### azure site list
現在のサブスクリプションに web アプリについては、呼び出す **azure サイトの一覧**, 次の例のようにします。

`azure site list webappslotstest`

----------
### azure site create
デプロイ スロットを作成するには **azure サイトを作成** 、既存の web アプリの名前と次の例に示すように、作成するスロットの名前を指定します。

`azure site create webappslotstest --slot staging`

新しいスロットのソース管理を有効にするには、 **- git** オプションは、次の例に示すようにします。

`azure site create --git webappslotstest --slot staging`

----------
### azure site swap
更新したデプロイ スロットを運用アプリをするためを使用して、 **azure サイトのスワップ** コマンドを次の例のように、スワップ操作を実行します。 運用アプリではダウン タイムは発生せず、コールド スタートが行われることもありません。

`azure site swap webappslotstest`

----------
### azure site delete
不要になったデプロイ スロットを削除する、 **azure サイトの削除** コマンドを次の例に示すようにします。

`azure site delete webappslotstest --slot staging`

----------

>[AZURE.NOTE] 場合は、Azure アカウントがサインアップする前に Azure App Service の使用を開始するには、 [App Service の試用](http://go.microsoft.com/fwlink/?LinkId=523751), 、App Service で有効期間の短いスターター web アプリをすぐに作成する場所です。 このサービスの利用にあたり、クレジット カードは必要ありません。契約も必要ありません。

## 次のステップ ##
[Azure App Service Web App – block web access to non-production deployment slots (Azure App Service Web App - 運用環境以外の展開スロットへの Web アクセスの禁止)](http://ruslany.net/2014/04/azure-web-sites-block-web-access-to-non-production-deployment-slots/)

[Microsoft Azure 無料評価版](/pricing/free-trial/)

## 変更内容
* Web サイトから App Service への変更のガイドを参照してください: [Azure App Service と既存の Azure サービスへの影響](http://go.microsoft.com/fwlink/?LinkId=529714)

<!-- IMAGES -->
[QGAddNewDeploymentSlot]:  ./media/web-sites-staged-publishing/QGAddNewDeploymentSlot.png
[AddNewDeploymentSlotDialog]: ./media/web-sites-staged-publishing/AddNewDeploymentSlotDialog.png
[ConfigurationSource1]: ./media/web-sites-staged-publishing/ConfigurationSource1.png
[MultipleConfigurationSources]: ./media/web-sites-staged-publishing/MultipleConfigurationSources.png
[SiteListWithStagedSite]: ./media/web-sites-staged-publishing/SiteListWithStagedSite.png
[StagingTitle]: ./media/web-sites-staged-publishing/StagingTitle.png
[SwapButtonBar]: ./media/web-sites-staged-publishing/SwapButtonBar.png
[SwapConfirmationDialog]:  ./media/web-sites-staged-publishing/SwapConfirmationDialog.png
[DeleteStagingSiteButton]: ./media/web-sites-staged-publishing/DeleteStagingSiteButton.png
[SwapDeploymentsDialog]: ./media/web-sites-staged-publishing/SwapDeploymentsDialog.png
[Autoswap1]: ./media/web-sites-staged-publishing/AutoSwap01.png
[Autoswap2]: ./media/web-sites-staged-publishing/AutoSwap02.png
[SlotSettings]: ./media/web-sites-staged-publishing/SlotSetting.png
 

