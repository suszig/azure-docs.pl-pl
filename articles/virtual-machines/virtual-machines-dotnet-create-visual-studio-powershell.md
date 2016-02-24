<properties
    pageTitle="Visual Studio を使用して Web プロジェクト用の VM を作成する | Microsoft Azure"
    description="Web アプリケーション用の仮想マシンを作成します"
    services="virtual-machines"
    documentationCenter=""
    authors="TomArcher"
    manager="douge"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-multiple"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/19/2015"
    ms.author="tarcher"/>

# Visual Studio での Web アプリケーション用仮想マシンの作成

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)] リソース マネージャーのモデルです。

Azure 用の Web アプリケーション プロジェクトを作成するとき、Azure 内で仮想マシンをプロビジョニングできます。 さらに、他のソフトウェアで仮想マシンを構成したり、診断やデバッグのために仮想マシンを使用したりすることができます。

Web アプリケーションを作成するときに仮想マシンを作成するには、次の手順に従います。

1. Visual Studio で、[ **ファイル** > **新規** > **プロジェクト** > **Web**, を選択し **ASP.NET Web アプリケーション** (下にある、 **Visual c#** または **Visual Basic** ノード)。
2.  **新しい ASP.NET プロジェクト** ] ダイアログ ボックスで、web アプリケーションの種類を選択 (右下隅) に、ダイアログ ボックスの [Azure] セクションのことを確認、 **クラウドでホスト** ] チェック ボックスをオン (このチェック ボックスがというラベルが付いた **リモート リソースを作成する** いくつかインストールで)。

    ![][0]

3. ドロップダウン リストで、Microsoft Azure でこの例では、次のように選択します。 **仮想マシン (v1)**, 、クリックして、 **[ok]** ] ボタンをクリックします。
4. サインインを要求されたら Azure にサインインします。  **バーチャル マシンの作成** ] ダイアログ ボックスが表示されます。

    ![][2]

5.  **DNS 名** ボックスで、仮想マシンの名前を入力します。 DNS 名は、Azure 上で一意の名前にする必要があります。 入力した名前が利用できない場合には、赤色の感嘆符が表示されます。
6.  **イメージ** 一覧で、仮想マシンで使用するイメージを選択します。 標準の Azure 仮想マシン イメージだけでなく、Azure にアップロードしたイメージを選択することもできます。
7. ままにして、 **を有効にする IIS および Web Deploy** ] チェック ボックスを別の web サーバーをインストールする予定がない場合に選択します。 Web デプロイを無効にすると、Visual Studio からの発行ができません。 IIS と Web デプロイは、パッケージ化された Windows Server のイメージ (カスタム イメージも含む) に追加できます。
8.  **サイズ** ボックスの一覧で、仮想マシンのサイズを選択します。
9. この仮想マシンのサインイン資格情報を指定します。 この情報は、リモート デスクトップからこの仮想マシンにアクセスする際に必要なため、メモしておきます。
10.  **場所** ボックスの一覧で、仮想マシンをホストするリージョンを選択します。
11. クリックして、 **[ok]** 仮想マシンの作成を開始するボタンをクリックします。 [操作の進行状況を行うことができる、 **出力** ウィンドウです。

    ![][3]

12. パブリッシュされたスクリプトで作成、仮想マシンがプロビジョニングされると、 **PublishScripts** 、ソリューション内のノードです。 発行スクリプトにより、Azure 内で仮想マシンが実行およびプロビジョニングされます。  **出力** ウィンドウに状態が表示されます。 このスクリプトは、仮想マシンを作成するために以下の操作を行います。

    * 既存の仮想マシンがない場合は、作成します。
    * `devtest` で始まる名前のストレージ アカウントを作成します。ただし、指定リージョンにこのストレージ アカウントがない場合に限ります。
    * 仮想マシン用のコンテナーとしてクラウド サービスを作成し、Web アプリケーション用の Web ロールを作成します。
    * 仮想マシンで Web デプロイを構成します。
    * 仮想マシンで IIS および ASP.NET を構成します。

    ![][4]

13. (必要に応じて) 新しい仮想マシンに接続できます。  **サーバー エクスプ ローラー**, 、展開、 **仮想マシン** ] ノードを選択してそのショートカット メニューで、作成した仮想マシンのノード **リモート デスクトップを使用して接続**します。 または、 **クラウド エクスプ ローラー** ことができます **ポータルで開く** ショートカット メニューの [ある仮想マシンに接続します。

 ![][5]


## 次のステップ

パブリッシュするスクリプトをカスタマイズする場合に詳細な情報を読み取る [開発環境およびテスト環境に発行する Windows PowerShell スクリプトを使用して](http://msdn.microsoft.com/library/dn642480.aspx)します。

[0]: ./media/virtual-machines-dotnet-create-visual-studio-powershell/CreateVM_NewProject.PNG
[1]: ./media/dotnet-visual-studio-create-virtual-machine/CreateVM_SignIn.PNG
[2]: ./media/virtual-machines-dotnet-create-visual-studio-powershell/CreateVM_CreateVM.PNG
[3]: ./media/virtual-machines-dotnet-create-visual-studio-powershell/CreateVM_Provisioning.png
[4]: ./media/virtual-machines-dotnet-create-visual-studio-powershell/CreateVM_SolutionExplorer.png
[5]: ./media/virtual-machines-dotnet-create-visual-studio-powershell/VS_Create_VM_Connect.png

