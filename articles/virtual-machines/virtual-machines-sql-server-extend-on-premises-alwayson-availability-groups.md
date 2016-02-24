<properties 
    pageTitle="オンプレミスの AlwaysOn 可用性グループの Azure への拡張 | Microsoft Azure"
    description="このチュートリアルでは、クラシック デプロイメント モデルで作成されたリソースを使用し、SQL Server Management Studio (SSMS) のレプリカの追加ウィザードを使用して、Azure に AlwaysOn 可用性グループ レプリカを追加する方法について説明します。"
    services="virtual-machines"
    documentationCenter="na"
    authors="rothja"
    manager="jeffreyg"
    editor="monicar"
    tags="azure-service-management"/>

<tags 
    ms.service="virtual-machines"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="11/13/2015"
    ms.author="jroth" />

# オンプレミスの AlwaysOn 可用性グループの Azure への拡張

AlwaysOn 可用性グループは、セカンダリ レプリカを追加することで、データベースのグループの高可用性を実現します。 これらのレプリカを使用すると、障害発生時にデータベースをフェールオーバーできます。 また、読み取りワークロードやバックアップ タスクをオフロードすることもできます。

SQL Server を含む 1 つまたは複数の Azure VM をプロビジョニングし、その VM をレプリカとしてオンプレミスの可用性グループに追加することで、オンプレミスの可用性グループを Microsoft Azure に拡張できます。

このチュートリアルでは、以下があることを前提としています。

- 有効な Azure サブスクリプション 実行できます [無料評価版にサインアップ](http://azure.microsoft.com/pricing/free-trial)します。

- 既存のオンプレミスの AlwaysOn 可用性グループ。 可用性グループの詳細については、次を参照してください。 [AlwaysOn 可用性グループ](https://msdn.microsoft.com/library/hh510230.aspx)します。

- オンプレミスのネットワークと、Azure Virtual Network 間の接続。 この仮想ネットワークの作成の詳細については、次を参照してください。 [Azure クラシック ポータルでサイト間 VPN の構成](../vpn-gateway/vpn-gateway-site-to-site-create.md)します。

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)] リソース マネージャーのモデルです。

## Azure レプリカ ウィザードの追加

このセクションでは、使用する方法を示します、 **Azure のレプリカの追加ウィザード** 、Azure のレプリカも格納する、AlwaysOn 可用性グループ ソリューションを拡張します。

1. SQL Server Management Studio 内で、展開 **AlwaysOn 高可用性** > **可用性グループ** > **[可用性グループの Name]**します。

1. 右クリック **可用性レプリカ**, 、] をクリックし、 **のレプリカ追加**します。

1. 既定では、 **可用性グループ ウィザードへのレプリカ追加** が表示されます。 クリックして **次**します。  選択した場合、 **今後このページを表示しない** このウィザードでは、この画面の以前の発売時に、ページの下部にあるオプションは表示されません。

    ![SQL](./media/virtual-machines-sql-server-extend-on-premises-alwayson-availability-groups/IC742861.png)

1. 既存のすべてのセカンダリ レプリカに接続する必要があります。 クリックして、 **接続]** または各レプリカの横にあることができます] をクリックして **すべての接続]** 、画面の下部にあります。 認証後、クリックして **次** して、次の画面に進みます。

1.  **レプリカの指定** ] ページで、複数のタブが上部に一覧表示されます: **レプリカ**, 、**エンドポイント**, 、**バックアップの設定]**, 、および **リスナー**します。  **レプリカ** ] タブ、[ **Azure レプリカの追加...** を Azure のレプリカの追加ウィザードを起動します。

    ![SQL](./media/virtual-machines-sql-server-extend-on-premises-alwayson-availability-groups/IC742863.png)

1. 以前に Azure 管理証明書をインストールしている場合は、ローカルの Windows 証明書ストアから既存の Azure 管理証明書を選択します。 以前に Azure サブスクリプションを利用したことがある場合は、Azure サブスクリプションの ID を選択するか入力します。 [ダウンロード] をクリックすると、Azure 管理証明書をダウンロードしてインストールし、Azure アカウントを使用しているサブスクリプションの一覧をダウンロードすることができます。

    ![SQL](./media/virtual-machines-sql-server-extend-on-premises-alwayson-availability-groups/IC742864.png)

1. ページの各フィールドに、レプリカをホストする Azure 仮想マシン (VM) を作成するために使用する値を入力します。

    |Setting|Description|
|---|---|
|**イメージ**|OS と SQL サーバーの目的の組み合わせを選択します。
|**VM サイズ**|ビジネス needs| に最適な VM のサイズを選択します。
|**VM 名**|新しいバーチャル マシンの一意の名前を指定します。 名前必要があります 3 ~ 15 文字を含む、ことができます文字、数字、およびハイフンのみを含める必要があります先頭の文字と文字または数字で終わる |。
|**VM ユーザー名**|管理者アカウントに、されたになるユーザー名を指定します。
|**VM 管理者パスワード**|新しいアカウントを作成 | のパスワードを指定します。
|**パスワードの確認入力**|新しいアカウントを作成 | のパスワードを確認します。
|**仮想ネットワーク**|新しい VM が使用する Azure の仮想ネットワークを指定します。 仮想ネットワークの詳細については、次を参照してください [仮想ネットワークの概要](..\virtual-network\virtual-networks-overview.md)。 |。
|**仮想ネットワーク サブネット**|新しい VM が use| 必要がある仮想ネットワーク サブネットを指定します。
|**ドメイン**|ドメインの事前入力されている値が correct| であることを確認します。
|**ドメイン ユーザー名**|ローカル クラスター nodes| のローカル管理者グループにあるアカウントを指定します。
|**パスワード**|ドメイン ユーザー名 | のパスワードを指定します。

1. クリックして **OK** 展開設定を検証します。

1. 次に、法律条項が表示されます。 読み、クリックして **OK** これらの条項に同意する場合。

1.  **レプリカの指定** ページが再び表示されます。 新しい Azure レプリカの設定を確認、 **レプリカ**, 、**エンドポイント**, 、および **バックアップの設定]** タブです。 ビジネス要件に合わせて設定を変更します。  これらのタブに表示されるパラメーターの詳細については、次を参照してください。 [レプリカの指定] ページ (新しい可用性グループ ウィザード/追加レプリカ ウィザード)](https://msdn.microsoft.com/library/hh213088.aspx)します。[リスナー] タブを使用して、Azure のレプリカが含まれている可用性グループ リスナーを作成できないことに注意してください。 また、ウィザードを起動する前にリスナーを既に作成している場合は、そのリスナーが Azure ではサポートされないことを示すメッセージが表示されます。 内のリスナーを作成する方法を紹介します **可用性グループ リスナーを作成** セクションです。

    ![SQL](./media/virtual-machines-sql-server-extend-on-premises-alwayson-availability-groups/IC742865.png)

1. クリックして **次**します。

1. 使用するデータの同期方法の選択、 **[初期データの同期** ページを **次**します。 ほとんどの場合、選択 **完全なデータ同期**します。 データの同期方法の詳細については、次を参照してください。 [選択初期データの同期] ページ (AlwaysOn 可用性グループ ウィザード)](https://msdn.microsoft.com/library/hh231021.aspx)します。

1. 結果を確認、 **検証** ページです。 未解決の問題を修正し、必要に応じて、検証を再実行します。 クリックして **次**します。

    ![SQL](./media/virtual-machines-sql-server-extend-on-premises-alwayson-availability-groups/IC742866.png)

1. 設定を確認、 **概要** ] ページの [ **完了**します。

1. プロビジョニング プロセスが開始されます。 ウィザードが正常に完了すると、クリックして **閉じる** 、ウィザードを終了します。

>[AZURE.NOTE] Azure のレプリカの追加ウィザード内のログ ファイルを作成する <Users>\ < ユーザー名 > \appdata\local\sql します。 このログ ファイルは、障害が発生した Azure レプリカのデプロイのトラブルシューティングに使用できます。 ウィザードで操作の実行に失敗すると、プロビジョニングされた VM が削除されるなど、前のすべての操作がロールバックされます。

## 可用性グループ リスナーの作成

可用性グループを作成したら、レプリカに接続するクライアントのリスナーを作成する必要があります。 リスナーは、着信接続をプライマリ レプリカまたは読み取り専用のセカンダリ レプリカに転送します。 リスナーの詳細については、次を参照してください。 [Azure で AlwaysOn 可用性グループ用の ILB リスナーを構成する](virtual-machines-sql-server-configure-ilb-alwayson-availability-group-listener.md)です。

## 次のステップ

使用するだけでなく、 **Azure のレプリカの追加ウィザード** を Azure に AlwaysOn 可用性グループを拡張することがありますを移動する一部の SQL Server ワークロード完全に Azure です。 最初に、次を参照してください。 [Azure で SQL Server 仮想マシンをプロビジョニング](virtual-machines-provision-sql-server.md)します。

Azure Vm での SQL Server の実行に関連するその他のトピックを参照してください。 [Azure 仮想マシン上の SQL Server](virtual-machines-sql-server-infrastructure-services.md)します。

