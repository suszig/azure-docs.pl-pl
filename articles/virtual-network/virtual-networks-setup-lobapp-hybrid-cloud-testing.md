<properties 
    pageTitle="LOB アプリケーションのテスト環境 | Microsoft Azure" 
    description="IT プロまたは開発のテスト用のハイブリッド クラウド環境で Web ベースの基幹業務アプリケーションを作成する方法について説明します。" 
    services="virtual-network" 
    documentationCenter="" 
    authors="JoeDavies-MSFT" 
    manager="timlt" 
    editor=""
    tags="azure-service-management"/>

<tags 
    ms.service="virtual-network" 
    ms.workload="infrastructure-services" 
    ms.tgt_pltfrm="Windows" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/10/2015" 
    ms.author="josephd"/>

# テスト用のハイブリッド クラウドでの Web ベース LOB アプリケーションの設定

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)] リソース マネージャーのモデルです。


このトピックでは、Microsoft Azure でホストされるイントラネット基幹業務 (LOB) アプリケーションをテストするためにハイブリッド クラウド環境を作成する手順について説明します。 完成すると次のような構成になります。

![](./media/virtual-networks-setup-lobapp-hybrid-cloud-testing/CreateLOBAppHybridCloud_3.png)

Azure でホストされる LOB アプリケーションの運用環境の例は、次を参照してください。、 **基幹業務アプリケーション** アーキテクチャ ブルー プリントをご覧 [Microsoft ソフトウェア アーキテクチャのダイアグラムとブルー プリント](http://msdn.microsoft.com/dn630664)します。

この構成では、インターネット上の自分の場所から Azure 運用環境の LOB アプリケーションをシミュレートします。 構成は次のとおりです。

- 簡略化されたオンプレミス ネットワーク (Corpnet サブネット)。
- Azure でホストされているクロスプレミス仮想ネットワーク (TestVNET)。
- サイト間 VPN 接続
- TestVNET 仮想ネットワーク内に基幹業務サーバー、SQL Server、セカンダリ ドメイン コントローラー

この構成を基盤や共通の出発点にして以下を実行できます。

- Azure の SQL Server 2014 データベース バックエンドを使用してインターネット インフォメーション サービス (IIS) でホストされている LOB アプリケーションを開発し、テストする。
- このハイブリッド クラウドをベースとした IT ワークロードのテストを実行する。

このハイブリッド クラウド テスト環境を設定する手順は、大きく次の 3 つのフェーズに分かれています。

1.  テスト用のハイブリッド クラウド環境を設定する。
2.  SQL Server コンピューター (SQL1) を構成する。
3.  LOB サーバー (LOB1) を構成する。

無料試用版にサインアップすることができます、Azure のサブスクリプションがあるない場合 [Azure](http://azure.microsoft.com/pricing/free-trial/)します。 MSDN サブスクリプションがある場合は、次を参照してください。 [MSDN サブスクライバー向けの Azure 特典](http://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/)します。

## フェーズ 1: ハイブリッド クラウド環境を設定する

」の指示に従って、 [テスト用のハイブリッド クラウド環境をセットアップ](virtual-networks-setup-hybrid-cloud-environment-testing.md) トピックです。 このテスト環境では Corpnet サブネット上に APP1 サーバーを配置する必要がないため、シャットダウンしてかまいません。

現在の構成は次のようになります。

![](./media/virtual-networks-setup-lobapp-hybrid-cloud-testing/CreateLOBAppHybridCloud_1.png)

> [AZURE.NOTE] フェーズ 1 の場合もシミュレートされたハイブリッド クラウド テスト環境を設定することができます。 参照してください [テスト用のシミュレートされたハイブリッド クラウド環境をセットアップ](virtual-networks-setup-simulated-hybrid-cloud-environment-testing.md) 手順については、です。
 
## フェーズ 2: SQL Server コンピューター (SQL1) を構成する

DC2 コンピューターが起動されていない場合は、Microsoft Azure 管理ポータルから起動します。

続いて、ローカル コンピューターで Azure PowerShell コマンド プロンプトから次のコマンドを実行して、SQL1 用に Azure 仮想マシンを作成します。 これらのコマンドを実行する前に、変数の値を入力し、削除、< と > 文字です。

    $storageacct="<Name of the storage account for your TestVNET virtual network>"
    $ServiceName="<The cloud service name for your TestVNET virtual network>"
    $cred1=Get-Credential -Message "Type the name and password of the local administrator account for SQL1."
    $cred2=Get-Credential -UserName "CORP\User1" -Message "Now type the password for the CORP\User1 account."
    Set-AzureStorageAccount -StorageAccountName $storageacct
    $image= Get-AzureVMImage | where { $_.ImageFamily -eq "SQL Server 2014 RTM Standard on Windows Server 2012 R2" } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1
    $vm1=New-AzureVMConfig -Name SQL1 -InstanceSize Large -ImageName $image
    $vm1 | Add-AzureProvisioningConfig -AdminUsername $cred1.GetNetworkCredential().Username -Password $cred1.GetNetworkCredential().Password -WindowsDomain -Domain "CORP" -DomainUserName "User1" -DomainPassword $cred2.GetNetworkCredential().Password -JoinDomain "corp.contoso.com"
    $vm1 | Set-AzureSubnet -SubnetNames TestSubnet
    $vm1 | Add-AzureDataDisk -CreateNew -DiskSizeInGB 100 -DiskLabel SQLFiles -LUN 0 -HostCaching None
    New-AzureVM -ServiceName $ServiceName -VMs $vm1 -VNetName TestVNET

次に、新しい SQL1 仮想マシンに接続 *ローカル管理者アカウントを使用して*します。

1.  Azure 管理ポータルの左側のウィンドウで、クリックして **仮想マシン**, 、] をクリックし、 **を実行している** SQL1 の [状態] 列にします。
2.  タスク バーで、クリックして **接続**します。 
3.  SQL1.rdp を開くダイアログ ボックスが表示されたら、クリックして **開く**します。
4.  リモート デスクトップ接続のメッセージ ボックスが表示されたら、クリックして **接続**します。
5.  資格情報の入力を求められたら、次の情報を使用します。
    - [名前]: **SQL1\\**[ローカル管理者アカウント名]
    - パスワード: [ローカル管理者アカウントのパスワード]
6.  証明書に関するリモート デスクトップ接続のメッセージ ボックスが表示されたら、クリックして **はい**します。

続いて、基本的な接続テストと SQL Server のトラフィックを許可するように Windows ファイアウォールの規則を構成します。 SQL1 で管理者レベルの Windows PowerShell コマンド プロンプトから次のコマンドを実行します。

    New-NetFirewallRule -DisplayName "SQL Server" -Direction Inbound -Protocol TCP -LocalPort 1433,1434,5022 -Action allow 
    Set-NetFirewallRule -DisplayName "File and Printer Sharing (Echo Request - ICMPv4-In)" -enabled True
    ping dc1.corp.contoso.com

ping コマンドで IP アドレス 10.0.0.1 からの応答が 4 回成功する必要があります。

次に、余っているデータ ディスクを新しいボリュームとして追加し、ドライブ文字 F: を割り当てます。

1.  サーバー マネージャーの左側のウィンドウで、クリックして **ファイルおよび記憶域サービス**, 、] をクリックし、 **ディスク**します。
2.  コンテンツ ウィンドウでの **ディスク** グループで、[ **disk 2** (で、 **パーティション** に設定 **不明な**)。
3.  クリックして **タスク**, 、] をクリックし、 **新しいボリューム**します。
4.  前に新しいボリューム ウィザードのページを開始でクリックして **次**します。
5.  選択して、サーバーとディスク] ページで、をクリックして **ディスク 2**, 、クリックして **次**します。 メッセージが表示されたら、クリックして **OK**します。
6.  [ボリューム] ページのサイズの指定] をクリックして **次**します。
7.  ドライブ文字またはフォルダーのページへの割り当て、をクリックして **次**します。
8.  クリックして選択し、ファイル システムの設定] ページで、 **次**します。
9.  選択内容の確認] ページをクリックして **作成**します。
10. 完了したら、クリックして **閉じる**します。

SQL1 の Windows PowerShell コマンド プロンプトで、次のコマンドを実行します。

    md f:\Data
    md f:\Log
    md f:\Backup

次に、新しいデータベースとユーザー アカウント アクセス許可に F: ドライブを使用するように SQL Server 2014 を構成します。

1.  スタート画面から次のように入力します。 **SQL Server Management**, 、クリックして **SQL Server 2014 Management Studio**します。
2.   **サーバーへの接続**, 、クリックして **接続**します。
3.  オブジェクト エクスプ ローラーのツリー ペインで右クリックし **SQL1**, 、クリックして **プロパティ**します。
4.   **サーバーのプロパティ** ウィンドウで、をクリックして **データベース設定**します。
5.  検索、 **データベースの既定の場所** し、これらの値を設定します。 
    -  **データ**, 、パスを入力 **f:\Data**します。
    -  **ログ**, 、パスを入力 **f:\Log**します。
    -  **バックアップ**, 、パスを入力 **f:\Backup**します。
    - 注: 新しいデータベースでのみ、これらの場所が使用されます。
6.  クリックして、 **OK** ウィンドウを閉じます。
7.   **オブジェクト エクスプ ローラー** ツリー ウィンドウで、 **セキュリティ**します。
8.  右クリック **ログイン** ] をクリックし、 **新しいログイン**します。
9.   **ログイン名**, 、型 **corp \user1**します。
10.  **サーバーの役割** ] ページで [ **sysadmin**, 、順にクリック **OK**します。
11. Microsoft SQL Server Management Studio を閉じます。

現在の構成は次のようになります。

![](./media/virtual-networks-setup-lobapp-hybrid-cloud-testing/CreateLOBAppHybridCloud_2.png)
 
## フェーズ 3: LOB サーバー (LOB1) を構成する

まず、ローカル コンピューターの Azure PowerShell コマンド プロンプトで次のコマンドを使用して、LOB1 用に Azure 仮想マシンを作成します。

    $ServiceName="<The cloud service name for your TestVNET virtual network>"
    $cred1=Get-Credential -Message "Type the name and password of the local administrator account for LOB1."
    $cred2=Get-Credential -UserName "CORP\User1" -Message "Now type the password for the CORP\User1 account."
    $image = Get-AzureVMImage | where { $_.ImageFamily -eq "Windows Server 2012 R2 Datacenter" } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1
    $vm1=New-AzureVMConfig -Name LOB1 -InstanceSize Medium -ImageName $image
    $vm1 | Add-AzureProvisioningConfig -AdminUsername $cred1.GetNetworkCredential().Username -Password $cred1.GetNetworkCredential().Password -WindowsDomain -Domain "CORP" -DomainUserName "User1" -DomainPassword $cred2.GetNetworkCredential().Password -JoinDomain "corp.contoso.com"
    $vm1 | Set-AzureSubnet -SubnetNames TestSubnet
    New-AzureVM -ServiceName $ServiceName -VMs $vm1 -VNetName TestVNET

次に、CORP\User1 アカウントの資格情報を使用して、LOB1 仮想マシンに接続します。

続いて、基本的な接続テストを行うためのトラフィックを許可するよう Windows ファイアウォール規則を構成します。 LOB1 で管理者レベルの Windows PowerShell コマンド プロンプトから、次のコマンドを実行します。

    Set-NetFirewallRule -DisplayName "File and Printer Sharing (Echo Request - ICMPv4-In)" -enabled True
    ping dc1.corp.contoso.com

ping コマンドで IP アドレス 10.0.0.1 からの応答が 4 回成功する必要があります。

次に、IIS 用に LOB1 を構成し、CLIENT1 からアクセスをテストします。

1.  サーバー マネージャーを実行し、 **の役割と機能の追加**します。
2.  前に開始] ページでクリックして **次**します。
3.  [インストールの種類] ページで、をクリックして **次**します。
4.  [インポート先のサーバー] ページで、をクリックして **次**します。
5.  [サーバーの役割] ページをクリックして **Web サーバー (IIS)** の一覧で **ロール**します。
6.  メッセージが表示されたら、] をクリックして **機能の追加**, 、] をクリックし、 **次**します。
7.  機能の選択] ページで、クリックして **次**します。
8.  Web サーバー (IIS)] ページで、クリックして **次**します。
9.  [役割サービス] ページに、選択や、LOB アプリケーションをテストする必要があるサービスのチェック ボックスをオフにし、クリックして **次**します。
10. インストールの選択の確認] ページで、をクリックして **インストール**します。
11. コンポーネントのインストールが完了するまで待機し、 **閉じる**します。
12. CORP\User1 アカウントの資格情報を使用して CLIENT1 コンピューターにログオンし、Internet Explorer を起動します。
13. アドレス バーに「 **http://lob1/** ENTER キーを押します。 既定の IIS 8 Web ページが表示されます。

現在の構成は次のようになります。

![](./media/virtual-networks-setup-lobapp-hybrid-cloud-testing/CreateLOBAppHybridCloud_3.png)
 
この環境で、Web ベース アプリケーションを LOB1 にデプロイし、Corpnet サブネットから機能とパフォーマンスをテストできます。

## その他のリソース

[Microsoft ソフトウェア アーキテクチャのダイアグラムとブループリント](http://msdn.microsoft.com/dn630664)

[ホストが容易な Web サーバー プラットフォーム (IIS)](http://technet.microsoft.com/library/hh831818)

[テスト用のハイブリッド クラウド環境の設定](virtual-networks-setup-hybrid-cloud-environment-testing.md)

[テスト用のハイブリッド クラウドでの SharePoint イントラネット ファームの設定](virtual-networks-setup-sharepoint-hybrid-cloud-testing.md)

[テスト用のハイブリッド クラウドでの Office 365 ディレクトリ同期 (DirSync) の設定](virtual-networks-setup-dirsync-hybrid-cloud-testing.md)

[テスト用のシミュレートされたハイブリッド クラウド環境の設定](virtual-networks-setup-simulated-hybrid-cloud-environment-testing.md)

[Azure ハイブリッド クラウド テスト環境](../virtual-machines/virtual-machines-hybrid-cloud-test-environments.md)

[Azure インフラストラクチャ サービス実装ガイドライン](../virtual-machines/virtual-machines-infrastructure-services-implementation-guidelines.md)
 

