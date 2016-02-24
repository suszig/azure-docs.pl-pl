<properties
    pageTitle="Visual Studio で仮想マシン スケール セット テンプレートをデプロイする | Microsoft Azure"
    description="Visual Studio の "リソース グループの配置" を使用して仮想マシン スケール セットをデプロイする方法"
    services="virtual-machines"
    documentationCenter=""
    authors="gbowerman"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/11/2015"
    ms.author="guybo"/>

# Visual Studio で仮想マシン スケール セット テンプレートをデプロイする
[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)] [従来のデプロイ モデル](virtual-machines-create-windows-powershell-service-manager.md)します。

この記事では、Visual Studio の "リソース グループの配置" を使用して Azure 仮想マシン スケール セットをデプロイする方法について説明します。


[Azure の仮想マシンのスケール設定](azure.microsoft.com/blog/azure-vm-scale-sets-public-preview/) は、Azure コンピューティング リソースへの展開し自動スケールのオプションを簡単に統合された類似する仮想マシンのコレクションを管理および負荷分散します。 プロビジョニングおよび展開を使用して VM スケール セット [Azure リソース マネージャー (ARM) テンプレート](https://github.com/Azure/azure-quickstart-templates)します。 ARM テンプレートは、Azure CLI、PowerShell、REST を使ってデプロイできるほか、Visual Studio から直接デプロイすることもできます。 Visual Studio には、"Azure リソース グループの配置" プロジェクトの一環としてデプロイできるサンプル テンプレート一式が用意されています。

Azure リソース グループとしてデプロイすることによって、関連する一連の Azure リソースを集約し、1 回のデプロイ操作で発行することができます。 ここでそれらのについて学習することができます: [作成および Visual Studio を使用して Azure リソース グループを展開する](../vs-azure-tools-resource-groups-deployment-projects-create-deploy/)です。

## 前提条件

Visual Studio で VM スケール セットをデプロイするには、次のものが必要です。

- Visual Studio 2013 または 2015
- Azure SDK 2.7 または 2.8

注: を前提と Visual Studio 2015 を使用している [Azure SDK 2.8](https://azure.microsoft.com/blog/announcing-the-azure-sdk-2-8-for-net/)します。

## プロジェクトの作成

1. Visual Studio 2015 を選択して新しいプロジェクトを作成 **ファイル |新しい |プロジェクト**します。

    ![File New][file_new]

2.  **Visual c# |クラウド**, 、選択 **Azure リソース マネージャー** ARM テンプレートを展開するプロジェクトを作成します。

    ![Create Project][create_project]

3.  テンプレートの一覧から、Linux または Windows の仮想マシン スケール セット テンプレートを選択します。

    ![Select Template][select_Template]

4. プロジェクトの作成後、PowerShell デプロイ スクリプト、Azure リソース マネージャー テンプレート、仮想マシン スケール セットのパラメーター ファイルが表示されます。

    ![Solution Explorer][solution_explorer]

## プロジェクトのカスタマイズ

これで、アプリケーションのニーズに応じてテンプレートを編集してカスタマイズできます。たとえば、VM 拡張機能のプロパティを追加したり、負荷分散の規則を編集したりできます。 既定では、自動スケール規則を簡単に追加できる AzureDiagnostics 拡張機能をデプロイするように VM スケール セット テンプレートが構成されています。 また、パブリック IP アドレスでロード バランサーがデプロイされ、SSH (Linux) または RDP (Windows) で VM インスタンスに接続できるように受信 NAT 規則が設定されます。フロントエンド ポートの範囲の開始は 50000 です。つまり Linux の場合、パブリック IP アドレス (またはドメイン名) のポート 50000 に SSH で接続すると、スケール セットにおける最初の VM のポート 22 にルーティングされます。 同様に、ポート 50001 に接続すると、2 つ目の VM のポート 22 にルーティングされます。

 Visual Studio でテンプレートを編集するときは、JSON アウトラインを使用してパラメーター、変数、リソースを整理することをお勧めします。 スキーマを認識させることによって、デプロイするテンプレートのエラーを事前に Visual Studio で検出することができます。

![JSON Explorer][json_explorer]

## プロジェクトをデプロイする

6. ARM テンプレートを Azure にデプロイして VM スケール セット リソースを作成します。 プロジェクト ノードを右クリックしてで、 **展開 |新しい展開**します。

    ![Deploy Template][5deploy_Template]

7. [リソース グループに配置する] ダイアログで該当するサブスクリプションを選択します。

    ![Deploy Template][6deploy_Template]

8. ここから、テンプレートのデプロイ先となる新しい Azure リソース グループを作成することもできます。

    ![New Resource Group][new_resource]

9. 次に選択、 **パラメーターの編集** ユーザー名とオペレーティング システムのパスワードは、展開の作成に必要なように、テンプレートでは、特定の値を渡すことがパラメーターを入力する] ボタンをクリックします。

    ![Edit Parameters][edit_parameters]

10. クリックして **展開**します。  **出力** ウィンドウ展開の進行状況が表示されます。 なお、アクションを実行する、 **Deploy-azureresourcegroup.ps1** スクリプトです。

    ![Output Window][output_window]

## VM スケール セットについて

Visual Studio で、展開が完了するに新しい VM スケール設定を表示できます **クラウド エクスプ ローラー** (一覧を更新) します。 アプリケーションの開発中に、Visual Studio からクラウド エクスプローラーを使って Azure のリソースを管理できます。 VM スケール セットは、Azure ポータルや Azure リソース エクスプローラーで表示することもできます。

![Cloud Explorer][cloud_explorer]

 ポータルの優れている点は、Azure のインフラストラクチャを Web ブラウザーを使って視覚的に管理できることです。一方、Azure リソース エクスプローラーの利点は、Azure のリソースを手軽に調査し、デバッグできることです。"インスタンス ビュー" を観察したり、着目しているリソースの PowerShell コマンドを表示したりすることができます。 VM スケール セットはプレビュー版ですが、VM スケール セットに関する大半の情報は、リソース エクスプローラーで表示することができます。

## 次のステップ

Visual Studio を使って VM スケール セットを正常にデプロイしたら、実際のアプリケーションの要件に応じてプロジェクトをさらにカスタマイズすることができます。 たとえば、Insights リソースを追加したり、スタンドアロン VM などのインフラストラクチャをテンプレートに追加したり、カスタム スクリプト拡張機能を使ってアプリケーションをデプロイしたりすることによって、自動スケールをセットアップすることが考えられます。 サンプルのテンプレートの適切なソースを参照して、 [Azure クイック スタート テンプレート](https://github.com/Azure/azure-quickstart-templates) GitHub リポジトリ ("vmss"を検索)。

[file_new]: ./media/virtual-machines-vmss-vstemplates/1-FileNew.png
[create_project]: ./media/virtual-machines-vmss-vstemplates/2-CreateProject.png
[select_Template]: ./media/virtual-machines-vmss-vstemplates/3b-SelectTemplateLin.png
[solution_explorer]: ./media/virtual-machines-vmss-vstemplates/4-SolutionExplorer.png
[json_explorer]: ./media/virtual-machines-vmss-vstemplates/10-JsonExplorer.png
[5deploy_Template]: ./media/virtual-machines-vmss-vstemplates/5-DeployTemplate.png
[6deploy_Template]: ./media/virtual-machines-vmss-vstemplates/6-DeployTemplate.png
[new_resource]: ./media/virtual-machines-vmss-vstemplates/7-NewResourceGroup.png
[edit_parameters]: ./media/virtual-machines-vmss-vstemplates/8-EditParameter.png
[output_window]: ./media/virtual-machines-vmss-vstemplates/9-Output.png
[cloud_explorer]: ./media/virtual-machines-vmss-vstemplates/12-CloudExplorer.png

