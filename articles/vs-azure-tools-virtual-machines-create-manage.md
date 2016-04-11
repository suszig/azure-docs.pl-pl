<properties
   pageTitle="Visual Studio での Azure 仮想マシンの作成と管理 | Microsoft Azure"
   description="Visual Studio での Azure 仮想マシンの作成と管理"
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="virtual-machines"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/30/2015"
   ms.author="tarcher" />

# Visual Studio での Azure Virtual Machines の作成と管理

Azure への仮想マシンの作成は、Visual Studio のサーバー エクスプローラーを使って行うことができます。

## サーバー エクスプローラーで Azure 仮想マシンを作成する

内の仮想マシンを作成できますが、 [Azure 管理ポータル](http://go.microsoft.com/fwlink/?LinkID=253103), 、サーバー エクスプ ローラーでコマンドを使用して、Azure で仮想マシンを作成することもできます。 仮想マシンは、負荷分散された共通のパブリック エンドポイントの内側に置くフロント エンドなどとして使用できます。

### 新しい仮想マシンを作成するには

1. サーバー エクスプ ローラーで開く、 **Azure** ノードとクリック **仮想マシン**します。

1. コンテキスト メニューで、次のようにクリックします。 **バーチャル マシンの作成**します。

     **バーチャル マシンの新規作成** ウィザードが表示されます。

    ![[仮想マシンの作成] コマンド](./media/vs-azure-tools-virtual-machines-create-manage/IC718342.png)

1.  **、サブスクリプションを選択する** ] ページで仮想マシンを作成するときに使用するサブスクリプションを選択してクリックして **次**します。

    Azure にサインインしていない場合はクリックして **サイン イン** サインインします。 ご利用の Azure サブスクリプションをまだ選択していない場合は、ドロップダウン ボックスから選択します。

1.  **仮想マシン イメージの選択** でイメージの種類の選択] ページで、 **イメージの種類** ドロップダウン ボックスの一覧し、の仮想マシン イメージを選択、 **イメージ名** ボックスの一覧です。 完了したら、クリックして **次**します。

    ![[仮想マシン イメージの選択] ページ](./media/vs-azure-tools-virtual-machines-create-manage/IC744137.png)

    以下の種類から選ぶことができます。

    - **公開イメージ** オペレーティング システムと Windows Server および SQL Server などのサーバー ソフトウェアの仮想マシン イメージが一覧表示します。

    - **MSDN イメージ** Visual Studio や Microsoft Dynamics など、MSDN サブスクリプション会員が利用できるソフトウェアの仮想マシン イメージを一覧表示します。

    - **プライベート イメージ** リスト、特殊化および作成した仮想マシン イメージを一般化します。

    特殊なおよび一般化されたバーチャル マシンの詳細については、次を参照してください。 [VM イメージ](http://azure.microsoft.com/blog/2014/04/14/vm-image-blog-post/)します。 参照してください [をテンプレートとして使用する Windows 仮想マシンをキャプチャする方法](http://azure.microsoft.com/documentation/articles/virtual-machines-capture-image-windows-server/) に事前に構成された仮想マシンを迅速に新しいを作成するのに使用できるテンプレートにバーチャル マシンをオフにする方法に関する情報。

    仮想マシン イメージ名をクリックすると、ページの右側にイメージに関する情報が表示されます。

    >[AZURE.NOTE] 仮想マシン イメージを追加することはできません、 **公開イメージ** または **MSDN イメージ** 一覧は読み取り専用なので。 作成するすべての仮想マシンに追加されます、 **プライベート イメージ** ] ボックスの一覧です。

    >Visual Studio レベルのサブスクリプションを持つ MSDN サブスクライバーの場合、Visual Studio と他のいくつかのイメージを含む事前に構築された Azure 仮想マシンを作成できます。 詳細については、次を参照してください [MSDN サブスクライバー向けの Visual studio を使用してイメージ Visual Studio 2013 ギャラリー イメージでの仮想マシンを作成](http://visualstudio2013msdngalleryimage.azurewebsites.net) と [MSDN サブスクリプション](https://www.visualstudio.com/products/msdn-subscriptions-vs)。 |。

1.  **仮想マシン基本設定** ページで、コンピューター名を入力し、サイズ、およびユーザー名とパスワードを含む、バーチャル マシンの仕様を追加します。 完了したら、クリックして **次**します。

    リモート デスクトップを使用するマシンにログインするときに、この新しい名前とパスワードを使用するので、忘れた場合に備えて書き留めておくことをお勧めします。 Visual Studio で Azure の仮想マシンを作成した後に、サイズ、およびその他の設定を変更することができます、 [Azure 管理ポータル](http://go.microsoft.com/fwlink/?LinkID=253103)します。

        >[AZURE.NOTE] 大きなサイズをバーチャル マシンを選択すると、不要な料金が表示される場合があります。 参照してください [Virtual Machines 料金](http://azure.microsoft.com/pricing/details/virtual-machines/) の詳細。

1. Visual Studio で作成した仮想マシンの場合、クラウド サービスが必要です。  **クラウド サービスの設定** ] ページで、仮想マシンのクラウド サービスを選択するか、[ **< [新規作成] >** 、クラウド サービス、または、新しいものを使用するをまだ持っていない場合は、ドロップダウン リストにします。 ストレージ アカウントが必要でも、ので [ストレージ アカウントを選択 (または [新しいストレージ アカウントの作成) で、 **ストレージ アカウント** ボックスの一覧です。 参照してください [Microsoft Azure Storage の概要](./storage/storage-introduction/) の詳細。

1. 仮想ネットワーク (省略可能) を指定する場合、[Virtual Network] ボックスと [サブネット] ボックスの一覧から選択します。

    仮想マシンが可用性セットのメンバーである場合、その仮想マシンは別の障害ドメインにデプロイされます。 参照してください [Azure Virtual Network](http://www.windowsazure.com/services/virtual-network/) の詳細。

1. 仮想マシンの可用性セット (も省略可能) に属している場合は、選択、 **、可用性セットを指定** チェック ボックスをオンし、ドロップダウン リスト ボックスで可用性セットを選択します。 完了したら、選択、 **次** ] ボタンをクリックします。

    可用性セットに仮想マシンを追加しておくと、ネットワークの障害時やローカル ハード ディスクの障害時、予定されたシステム停止時にもアプリケーションを使用できます。 使用する必要があります、 [Azure 管理ポータル](http://go.microsoft.com/fwlink/?LinkID=253103) セットに仮想ネットワーク、サブネット、および可用性を作成します。 参照してください [仮想マシンの可用性を管理](http://www.windowsazure.com/documentation/articles/manage-availability-virtual-machines/) の詳細。

1.  **エンドポイント** ] ページで、バーチャル マシンのユーザーが使用できるようにするパブリックのエンドポイントを指定します。 たとえば、既定で有効になるリモート デスクトップ エンドポイントと PowerShell エンドポイントに加えて、HTTP (ポート 80) を有効にすることができます。 エンドポイントを追加するには、いずれかを選択、 **ポート名** ドロップダウン ボックスの一覧し、を選択し、 **追加** ] ボタンをクリックします。 エンドポイントを削除する選択赤い **X** エンドポイント リストの名前の横にあります。

    ![仮想マシン ウィザードの [エンドポイント] ページ。](./media/vs-azure-tools-virtual-machines-create-manage/IC718351.png)

    使用できるエンドポイントは、仮想マシン用に選択したクラウド サービスによって変わります。 参照してください [Azure サービス エンドポイント](https://azure.microsoft.com/documentation/articles/virtual-machines-set-up-endpoints/) の詳細。

    >[AZURE.NOTE] パブリック エンドポイントを有効にするため、サービスの仮想マシンで使用可能なインターネットに。 仮想マシンにエンドポイントとサービスをインストールしたら、エンドポイントのアクセス制御リスト (ACL) を設定するなど、適切な構成を行ってください。 参照してください [仮想マシンに対してエンドポイントを設定する方法](https://azure.microsoft.com/documentation/articles/virtual-machines-set-up-endpoints/) の詳細。

1. 完了したら、仮想マシンの設定を構成するには、選択、 **作成** 仮想マシンを作成する] ボタンをクリックします。

    Azure 仮想マシンが作成されると、 **Azure のアクティビティ ログ** バーチャル マシン作成操作の進行状況が表示されます。

    ![仮想マシンのアクティビティ ログ - 進行中。](./media/vs-azure-tools-virtual-machines-create-manage/IC744138.png)

    仮想マシン情報のみを表示する、 **仮想マシン** ] タブで、 **Azure のアクティビティ ログ**します。

    ![仮想マシンのアクティビティ ログ - 完了。](./media/vs-azure-tools-virtual-machines-create-manage/IC744139.png)

    操作が正常に完了すると、]、新しいバーチャル マシンが表示されます、 **仮想マシン** サーバー エクスプ ローラー ノードです。 クリックしてにログインすることができます、 **リモート デスクトップを使用して接続** ショートカットです。

    ![サーバー エクスプ ローラーに表示された仮想マシン。](./media/vs-azure-tools-virtual-machines-create-manage/IC744140.png)

## 仮想マシンの管理

仮想マシンの構成ページでは、選択した仮想マシンに関してシャットダウン、接続、更新、チェックポイントの追加を実行できるほか、仮想マシンの設定を表示または変更することもできます。 そのための方法は次のとおりです。

- 仮想マシンのサイズを変更する。

- 仮想マシンで使用する可用性セットを選択する。

- パブリック エンドポイントの設定を追加、削除、変更する。

- 仮想マシンの拡張機能を追加、削除、構成する。

- 仮想マシンに関連付けられているディスクの情報を表示する。

### 仮想マシンの設定の表示と変更

1. サーバー エクスプ ローラーでの仮想マシンを選択して、 **Azure の仮想マシン** ノードです。

1. ショートカット メニューで **構成** 仮想マシンの構成ページを表示します。

    ![Azure の仮想マシンの構成ページ](./media/vs-azure-tools-virtual-machines-create-manage/IC744141.png)

1. 仮想マシンの情報を表示または変更します。

### 仮想マシンの状態の保存と復元

仮想マシンを構成し、ソフトウェアをインストールする際は、仮想マシンにチェックポイントを作成することによって定期的に進捗を保存しておくことをお勧めします。 チェックポイントは、仮想マシンの現在の状態のスナップショット、つまりイメージです。 仮想マシンに問題が発生したとき、または仮想マシンを再構成するときに、最初からやり直すのではなく以前のチェックポイントの状態に復元することにより時間を節約できます。

### 仮想マシンのチェックポイントを作成するには

1. サーバー エクスプ ローラーでの仮想マシンを選択して、 **Azure の仮想マシン** ノードです。

1. ショートカット メニューで **構成** 仮想マシンの構成ページを表示します。

1. [構成] ページで、選択、 **イメージのキャプチャ** ] ボタンをクリックします。

    ![Azure の構成ページのキャプチャ ボタン](./media/vs-azure-tools-virtual-machines-create-manage/IC744142.png)

     **仮想マシンのキャプチャ** ダイアログが表示されます。

    ![Azure の仮想マシンのキャプチャ ダイアログ ボックス](./media/vs-azure-tools-virtual-machines-create-manage/IC744143.png)

1. イメージにラベルと説明を指定します。 既定のラベルと説明が付けられていますが、必要に応じて上書きすることができます。

1. この仮想マシンで既に Sysprep を実行する場合は、選択、 **仮想マシンで Sysprep を実行しました** ボックス。

    Sysprep は、仮想マシンの Windows からシステム固有のデータを削除して、他の仮想マシンで利用できるテンプレートを作成することを主な目的としたツールです。 参照してください [をテンプレートとして使用する Windows 仮想マシンをキャプチャする方法](http://azure.microsoft.com/documentation/articles/virtual-machines-capture-image-windows-server/) の詳細。

1. 完了したらキャプチャの設定を構成するには、選択、 **キャプチャ** 、チェックポイントを作成する] ボタンをクリックします。

    Azure には、チェックポイントが作成されると、 **Azure のアクティビティ ログ** 操作の進行状況が表示されます。

    ![仮想マシンのチェックポイントのキャプチャ](./media/vs-azure-tools-virtual-machines-create-manage/IC744144.png)

    チェックポイント操作が完了したらで表示されます、 **Azure のアクティビティ ログ**します。

    ![チェックポイント操作の完了](./media/vs-azure-tools-virtual-machines-create-manage/IC744145.png)

## 仮想マシンのチェックポイントを管理するには

### 以前に保存した状態に仮想マシンを復元するには

- 説明されている手順に従います [ステップ バイ ステップ: 実行クラウド復元の Microsoft Azure Virtual Machines powershell - 第 2 部](http://blogs.technet.com/b/keithmayer/archive/2014/02/04/step-by-step-perform-cloud-restores-of-windows-azure-virtual-machines-using-powershell-part-2.aspx)します。

### チェックポイントを削除するには

1. 移動して、 [Azure 管理ポータル](http://go.microsoft.com/fwlink/?LinkID=253103)します。

1. 仮想マシンの構成] ページで、選択、 **イメージ** 、ページの上部にあるタブをクリックします。

1. クリックして、削除するチェックポイントを選択して、 **削除**  ページの下部にあるボタンをクリックします。

## 仮想マシンのシャットダウン

1. サーバー エクスプ ローラーでシャット ダウンする仮想マシンを選択して、 **Azure の仮想マシン** ノードです。

1. 選択するか、ショートカット メニューで、 **シャット ダウン** コマンド、または選択 **構成** に仮想マシンの構成] ページを表示して選択し、 **シャット ダウン** ] ボタンをクリックします。

## 次のステップ

バーチャル マシンの作成の詳細については、次を参照してください。 [を実行している Linux の仮想マシンを作成する](virtual-machines-linux-tutorial.md) と [Azure プレビュー ポータルで Windows を実行する仮想マシンを作成する](virtual-machines-windows-tutorial.md)です。


