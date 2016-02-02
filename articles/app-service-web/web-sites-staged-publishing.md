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
## Web アプリにデプロイ スロットを追加する

複数のデプロイ スロットを有効にするには、Web アプリが **Standard** または **Premium** モードで実行されている必要があります。

1. [Azure ポータル](https://portal.azure.com/), 、web アプリのブレードを開きます。
2. **[デプロイ スロット]** をクリックします。 次に、**[デプロイ スロット]** ブレードで **[スロットの追加]** をクリックします。

    ![新しいデプロイ スロットの追加][qgaddnewdeploymentslot]
    > [AZURE.NOTE]
    > Web アプリがまだ **Standard** または **Premium** モードでない場合は、ステージングされた発行を有効にするためのモードを示すメッセージが表示されます。 その場合は、操作を継続する前に、**[アップグレード]** を選択して Web アプリの **[スケール]** タブに移動できます。

2. **[スロットの追加]** ブレードで、スロット名を設定し、別の既存のデプロイ スロットから Web アプリ構成を複製するかどうかを選択します。 チェック マークをクリックして続行します。

    ![構成元][configurationsource1]

    初めてスロットを追加する場合、選択肢は 2 つのみです。運用環境内の既定のスロットから構成を複製するか、または複製しないかのどちらかです。

    複数のスロットを作成すると、運用サイトのスロット以外のスロットから構成を複製できるようになります。

    ![構成元][multipleconfigurationsources]

5. **デプロイ スロット** ブレードで、[デプロイ スロットを一連のメトリックとその他の web アプリと同じように構成で、スロットのブレードを開く] をクリックします。 **your-web-app-name-deployment-slot-name** 展開スロットが表示されていることを通知する、ブレードの上部に表示されます。

    ![デプロイ スロットのタイトル][stagingtitle]

5. スロットのブレードで、アプリの URL をクリックします。 デプロイ スロットが独自のホスト名を持つライブ アプリであることに注意してください。 デプロイ スロットへのパブリック アクセスを制限するため、次を参照してください。 [App Service Web App-運用環境以外のデプロイ スロットへの web アクセスの禁止](http://ruslany.net/2014/04/azure-web-sites-block-web-access-to-non-production-deployment-slots/)します。

デプロイ スロットを作成した直後は、コンテンツはありません。 スロットには、異なるリポジトリ分岐、またはまったく異なるリポジトリからデプロイできます。 またスロットの構成を変更することもできます。 コンテンツの更新には、デプロイ スロットに関連付けられた発行プロファイルまたはデプロイ資格情報を使用してください。 たとえば、 [git を使用してこのスロットに発行](web-sites-publish-source-control.md)します。

<a name="AboutConfiguration"></a>
## デプロイ スロットの構成

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

アプリ設定や接続文字列をスロット固有として構成する (スワップされないようにする) には、特定のスロットの **[アプリケーション設定]** ブレードにアクセスし、スロット固有にする構成要素の **[スロット設定]** ボックスを選択します。 構成要素をスロット固有としてマークすると、その Web アプリに関連付けられたすべてのデプロイ スロットにわたってその要素がスワップできなくなります。

![スロットの設定][slotsettings]

<a name="Swap"></a>
## デプロイ スロットをスワップするには

>[AZURE.IMPORTANT] Web アプリをデプロイ スロットからを運用環境にスワップする前に、スロット固有でないすべての設定が、スワップ ターゲットへと反映させたいとおりに構成されていることを確認してください。

1. デプロイ スロットをスワップするには、Web アプリのコマンド バーか、またはデプロイ スロットのコマンド バーで **[スワップ]** ボタンをクリックします。 スワップのソースとターゲットが正しく設定されていることを確認してください。 通常、スワップ ターゲットは運用スロットになります。

    ![[スワップ] ボタン][swapbuttonbar]

3. **[OK]** をクリックして操作を完了します。 操作が完了すると、デプロイ スロットはスワップされています。

## Web アプリの自動スワップの構成

自動スワップを使用すると、Web アプリのエンドユーザーにコールド スタートやダウンタイムを強制することなく、Web アプリを連続的にデプロイできる、効率的な DevOps シナリオを実現できます。 デプロイ スロットを運用環境に自動スワップするよう構成した場合、そのスロットにコードの更新をプッシュすると、Web アプリがスロット内で起動されしだい、App Service によって自動的に運用環境へとスワップされます。
>[AZURE.IMPORTANT] 自動スワップを有効にする場合は、スロット構成がターゲット スロット (通常は運用スロット) に反映させたいとおりの構成になっていることを確認してください。

スロットの自動スワップは簡単に構成できます。 次の手順に従ってください。

1. **[デプロイ スロット]** ブレードで非運用スロットを選択し、そのスロットのブレードの **[すべての設定]** をクリックします。

    ![][autoswap1]

2. **[アプリケーションの設定]** をクリックします。 **[自動スワップ]** の **[オン]** を選択し、**[自動スワップ スロット]** で目的のターゲット スロットを選択して、コマンド バーの **[保存]** をクリックします。 スロットの構成が、ターゲット スロットに反映させたいとおりの構成になっていることを確認してください。

    操作が完了すると、**[通知]** タブに緑色で "**成功**" が点滅します。

    ![][autoswap2]
    >[AZURE.NOTE] Web アプリの自動スワップ機能に事前に慣れておきたい場合は、まず **[自動スワップ スロット]** で運用スロット以外のターゲット スロットを選択することで、機能をテストすることができます。  

3. デプロイ スロットにコードをプッシュします。 しばらくすると自動スワップが実行され、更新がターゲット スロットの URL に反映されます。

<a name="Multi-Phase"></a>
## Web アプリに複数フェーズのスワップを使用する

複数フェーズのスワップを使用すると、スロット固有として設計された構成要素 (接続文字列など) のコンテキストで検証を簡素化できます。 この場合、スワップが実際に有効になる前に、スワップ ターゲットのこのような構成要素をスワップ ソースに適用して検証すると便利です。 スワップ ターゲットの構成要素をスワップ ソースに適用した後は、実行可能な操作として、スワップを完了するか、スワップ ソースの元の構成に戻します。元の構成に戻すと、スワップをキャンセルできます。 複数フェーズのスワップに使用できる Azure PowerShell コマンドレットのサンプルについては、「デプロイ スロット用の Azure PowerShell コマンドレット」をご覧ください。

<a name="Rollback"></a>
## スワップ後に運用環境のアプリをロールバックするには

スロットのスワップ後に運用サイトでエラーが見つかった場合は、同じ 2 つのスロットをすぐにスワップして、スロットをスワップ前の状態にロールバックすることができます。

<a name="Delete"></a>
## デプロイ スロットを削除するには

デプロイ スロットのブレードで、コマンド バーの **[削除]** をクリックします。

![デプロイ スロットの削除][deletestagingsitebutton]



<a name="PowerShell"></a>
## デプロイ スロット用の Azure PowerShell コマンドレット

Azure PowerShell は、Windows PowerShell から Azure を管理するためのコマンドレットを提供するモジュールです (Azure App Service の Web アプリ デプロイ スロットを管理するためのサポートなど)。

- インストールと Azure PowerShell を構成して、Azure サブスクリプションに Azure PowerShell の認証については、次を参照してください。 [をインストールして、Microsoft Azure PowerShell を構成する方法](../install-configure-powershell.md)します。

- PowerShell の新しい Azure リソース マネージャー モードを使用するために、コマンドレットに次の開始: `Switch-azuremode-名前 AzureResourceManager`します。

----------


### Web アプリの作成

`新しい AzureWebApp ResourceGroupName [リソース グループ名]-[web アプリ名] という名前の場所 [location] AppServicePlan [app service プラン名]`

----------


### Web アプリのデプロイ スロットを作成する

`新しい AzureWebApp ResourceGroupName [リソース グループ名]-[展開スロット名] の [web アプリ名] - SlotName-場所 [location] AppServicePlan [app service プラン名]`

----------


### 複数フェーズのスワップを開始し、ターゲット スロットの構成をソース スロットに適用する

`$ParametersObject = @{targetSlot =「[スロット名]: 例:"production"]」}`
`[リソース グループ名] を呼び出す AzureResourceAction ResourceGroupName ResourceType Microsoft.Web/sites/slots ResourceName [web アプリ名]、[スロットの名前] - アクション applySlotConfig-パラメーター $ParametersObject ApiVersion 2015-07-01`

----------


### 複数フェーズのスワップの最初のフェーズを元に戻し、ソース スロットの構成を復元する

`[リソース グループ名] を呼び出す AzureResourceAction ResourceGroupName ResourceType Microsoft.Web/sites/slots ResourceName [web アプリ名]、[スロットの名前] - アクション resetSlotConfig ApiVersion 2015-07-01`

----------


### デプロイ スロットをスワップする

`$ParametersObject = @{targetSlot =「[スロット名]: 例:"production"]」}`
`[リソース グループ名] を呼び出す AzureResourceAction ResourceGroupName ResourceType Microsoft.Web/sites/slots ResourceName [web アプリ名]、[スロットの名前] - アクション slotsswap-パラメーター $ParametersObject ApiVersion 2015-07-01`

----------


### デプロイ スロットを削除する

`Remove-azureresource ResourceGroupName [リソース グループ名] - ResourceType Microsoft.Web/sites/slots – [web アプリ名] の名前/[スロットの名前] - ApiVersion 2015-07-01`

----------




<a name="CLI"></a>
## デプロイ スロット用の Azure コマンド ライン インターフェイス (Azure CLI) コマンド

Azure CLI には、Web アプリ デプロイ スロットの管理のサポートなど、Azure を使用するためのクロス プラットフォーム コマンドが用意されています。

- インストールして、Azure サブスクリプションに Azure CLI を接続する方法に関する情報を含めて、Azure CLI の構成手順についてを参照してください。 [のインストールと Azure CLI の構成](../xplat-cli-install.md)します。

-  Azure CLI で Azure App Service の使用可能なコマンドの一覧を表示呼び出して `azure サイト h`します。

----------

### azure site list

現在のサブスクリプションの Web アプリに関する情報については、次の例に示すように、**azure site list** を呼び出します。

`azure のサイト リスト webappslotstest`

----------

### azure site create

デプロイ スロットを作成するには、次の例に示すように、**azure site create** を呼び出し、既存の Web アプリの名前と作成するスロットの名前を指定します。

`azure のサイトを作成 webappslotstest - ステージング スロット`

新しいスロットのソース管理を有効にするには、次の例に示すように、**--git** オプションを使用します。

`azure のサイトの作成 - git webappslotstest - ステージング スロット`

----------

### azure site swap

更新したデプロイ スロットを運用アプリにするには、次の例に示すように、**azure site swap** コマンドを使用してスワップ操作を実行します。 運用アプリではダウン タイムは発生せず、コールド スタートが行われることもありません。

`azure サイト スワップ webappslotstest`

----------

### azure site delete

不要になったデプロイ スロットを削除するには、次の例に示すように、**azure site delete** コマンドを使用します。

`azure のサイトの削除 webappslotstest - ステージング スロット`

----------

>[AZURE.NOTE] 場合は、Azure アカウントがサインアップする前に Azure App Service の使用を開始するには、 [App Service の試用](http://go.microsoft.com/fwlink/?LinkId=523751), 、App Service で有効期間の短いスターター web アプリをすぐに作成する場所です。 このサービスの利用にあたり、クレジット カードは必要ありません。契約も必要ありません。

## 次のステップ

[Azure App Service Web App-運用環境以外のデプロイ スロットへの web アクセスの禁止](http://ruslany.net/2014/04/azure-web-sites-block-web-access-to-non-production-deployment-slots/)

[Microsoft Azure の無料評価版](/pricing/free-trial/)

## 変更内容

* Web サイトから App Service への変更のガイドを参照してください: [Azure App Service と既存の Azure サービスへの影響](http://go.microsoft.com/fwlink/?LinkId=529714)



[qgaddnewdeploymentslot]: ./media/web-sites-staged-publishing/QGAddNewDeploymentSlot.png 
[addnewdeploymentslotdialog]: ./media/web-sites-staged-publishing/AddNewDeploymentSlotDialog.png 
[configurationsource1]: ./media/web-sites-staged-publishing/ConfigurationSource1.png 
[multipleconfigurationsources]: ./media/web-sites-staged-publishing/MultipleConfigurationSources.png 
[sitelistwithstagedsite]: ./media/web-sites-staged-publishing/SiteListWithStagedSite.png 
[stagingtitle]: ./media/web-sites-staged-publishing/StagingTitle.png 
[swapbuttonbar]: ./media/web-sites-staged-publishing/SwapButtonBar.png 
[swapconfirmationdialog]: ./media/web-sites-staged-publishing/SwapConfirmationDialog.png 
[deletestagingsitebutton]: ./media/web-sites-staged-publishing/DeleteStagingSiteButton.png 
[swapdeploymentsdialog]: ./media/web-sites-staged-publishing/SwapDeploymentsDialog.png 
[autoswap1]: ./media/web-sites-staged-publishing/AutoSwap01.png 
[autoswap2]: ./media/web-sites-staged-publishing/AutoSwap02.png 
[slotsettings]: ./media/web-sites-staged-publishing/SlotSetting.png 

