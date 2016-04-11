<properties 
    pageTitle="シミュレートされたハイブリッド クラウドのテスト環境 | Microsoft Azure" 
    description="2 つの Azure 仮想ネットワークと 1 つの VNet 間接続を使用して、IT プロや開発テスト用のシミュレートされたハイブリッド クラウド環境を作成します。" 
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

# テスト用のシミュレートされたハイブリッド クラウド環境の設定

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)] リソース マネージャーのモデルです。


このトピックでは、Microsoft Azure と 2 つの Azure 仮想ネットワークを使用してテスト用のシミュレートされたハイブリッド クラウド環境を作成する手順について説明します。 インターネットに直接接続できず、利用可能なパブリック IP アドレスがない場合は、 
[テスト用のハイブリッド クラウド環境をセットアップ](virtual-networks-setup-hybrid-cloud-environment-testing.md) されませんが、直接インターネットに接続し、使用可能なパブリック IP アドレスです。 完成すると次のような構成になります。

![](./media/virtual-networks-setup-simulated-hybrid-cloud-environment-testing/CreateSimHybridCloud_4.png)

この構成は、ハイブリッド クラウド運用環境をシミュレートしています。 構成は次のとおりです。

- Azure 仮想ネットワークでホストされているシミュレートされ、簡素化されたオンプレミス ネットワーク (TestLab 仮想ネットワーク)。
- Azure でホストされているシミュレートされたクロスプレミス仮想ネットワーク (TestVNET)。
- 2 つの仮想ネットワークをつなぐ VNet 間接続。
- TestVNET 仮想ネットワークのセカンダリ ドメイン コントローラー。

この構成を基盤や共通の出発点にして以下を実行できます。

- シミュレートされたハイブリッド クラウド環境でアプリケーションを開発およびテストする。
- コンピューターのテスト構成 (TestLab 仮想ネットワークと TestVNET 仮想ネットワーク内にそれぞれコンピューターを配置) を作成して、ハイブリッド クラウド ベースの IT ワークロードをシミュレートする。

このハイブリッド クラウド テスト環境を設定する手順は、大きく次の 4 つのフェーズに分かれています。

1.  TestLab 仮想ネットワークを構成する。
2.  クロスプレミス仮想ネットワークを作成する。
3.  VNet 間 VPN 接続を作成する。
4.  DC2 を構成する。 

無料評価版にサインアップすることができます、Azure サブスクリプションがない場合 [Azure](http://azure.microsoft.com/pricing/free-trial/)します。 MSDN サブスクリプションがある場合は、次を参照してください。 [MSDN サブスクライバー向けの Azure 特典](http://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/)します。

>[AZURE.NOTE] 仮想マシンと Azure で仮想ネットワーク ゲートウェイが実行されているときに継続的な費用が発生します。 その費用は、無料試用版、MSDN サブスクリプション、または有料のサブスクリプションに対して請求されます。 使用しない場合は、このテスト環境を実行中のコストを削減するには、次を参照してください。 [この環境の継続的な費用を最小限に抑える](#costs) の詳細については、このトピックにします。


## フェーズ 1: TestLab 仮想ネットワークを構成する

」の指示に従って、 [基本構成テスト環境](../virtual-machines/virtual-machines-base-configuration-test-environment.md) TestLab という Azure の virtual network で DC1、APP1、および CLIENT1 コンピューターを構成します。 

ローカル コンピューターで Microsoft Azure 管理ポータルから CORP\User1 の資格情報を使用して DC1 に接続します。 コンピューターとユーザーが認証にローカル ドメイン コントローラーを使用するよう CORP ドメインを構成するために、管理者レベルの Windows PowerShell コマンド プロンプトから次のコマンドを実行します。

    New-ADReplicationSite -Name "TestLab" 
    New-ADReplicationSite -Name "TestVNET"
    New-ADReplicationSubnet â€“Name "10.0.0.0/8" â€“Site "TestLab"
    New-ADReplicationSubnet â€“Name "192.168.0.0/16" â€“Site "TestVNET"

現在の構成は次のようになります。

![](./media/virtual-networks-setup-simulated-hybrid-cloud-environment-testing/CreateSimHybridCloud_1.png)
 
## フェーズ 2: TestVNET 仮想ネットワークを作成する

まず、TestVNET という名前の新しい仮想ネットワークを作成します。

1.  Azure 管理ポータルのタスク バーで、クリックして **新規 > ネットワーク サービス > 仮想ネットワーク > カスタム作成**します。
2.  仮想ネットワークの詳細] ページで、次のように入力します。 **TestVNET** で **名**します。
3.   **場所**, 、適切な場所を選択します。
4.  次へ進む矢印をクリックします。
5.  [DNS サーバーおよび VPN 接続] ページで **DNS サーバー**, 、型 **DC1** で **選択または入力]**, 、次へ進む矢印をクリックします。
6.  [Virtual Network アドレス空間] ページで、次の内容を構成します。
    -  **アドレス空間**, の **開始 IP**, 選択または入力 **192.168.0.0**します。
    -  **サブネット**, 、クリックして **subnet-1** し名前を **TestSubnet**します。 
    -  **CIDR (アドレス数)** 、TestSubnet の列をクリックして **/24 (256)**します。
7.  [完了] アイコンをクリックします。 仮想ネットワークが作成されるまで待ってから、次に進みます。

」の手順を次に、に従って [をインストールして、ローカル コンピューターに Azure PowerShell をインストールする Azure PowerShell を構成する方法](../install-configure-powershell.md)します。

次に、TestVNET 仮想ネットワークの新しいクラウド サービスを作成します。 一意の名前を選ぶ必要があります。 たとえば、名前を付けます **testvnet-***UniqueSequence*, を *UniqueSequence* 、組織の略称です。 たとえば、組織が Tailspin Toys をという名前の場合は、クラウド サービスを名前でした **Testvnet-tailspin**します。

名前が一意かどうかは、ローカル コンピューターで次の Azure PowerShell コマンドを使用することで確認できます。

    Test-AzureName -Service <Proposed cloud service name>

このコマンドで "False" が返された場合は、提案した名前は一意です。 次のコマンドでクラウド サービスを作成します。

    New-AzureService -Service <Unique cloud service name> -Location "<Same location name as your virtual network>"

現在の構成は次のようになります。

![](./media/virtual-networks-setup-simulated-hybrid-cloud-environment-testing/CreateSimHybridCloud_2.png)
 
##フェーズ 3: VNet 間接続を作成する

まず、各仮想ネットワークのアドレス空間を表すローカル ネットワークを作成します。

1.  ローカル コンピューターで Azure 管理ポータルでは、次のようにクリックします。 **新規 > ネットワーク サービス > 仮想ネットワーク > ローカル ネットワークの追加**します。
2.  指定のローカル ネットワークの詳細] ページで、次のように入力します。 **TestLabLNet** で **名**, 、型 **131.107.0.1** で **VPN デバイスの IP アドレス**, 、し、右矢印をクリックします。
3.  指定するアドレス空間] ページで、 **開始**P 型 **10.0.0.0**します。
4.   **CIDR (アドレス数)**, [ **/24 (256)**, 、チェック マークをクリックします。
5.  クリックして **新しい > ネットワーク サービス > 仮想ネットワーク > ローカル ネットワークの追加**します。
6.  指定のローカル ネットワークの詳細] ページで、次のように入力します。 **[testvnetlnet]** で **名**, 、型 **131.107.0.2** で **VPN デバイスの IP アドレス**, 、し、右矢印をクリックします。
7.  指定するアドレス空間] ページで、 **開始 IP**, 、型 **192.168.0.0**します。
8.   **CIDR (アドレス数)**, [ **/24 (256)**, 、チェック マークをクリックします。

VPN デバイスの IP アドレス 131.107.0.1 と 131.107.0.2 は、2 つの仮想ネットワーク用のゲートウェイを構成するまでの一時的なプレースホルダー値です。

次に、サイト間 VPN 接続と、もう一方の仮想ネットワークに対応するローカル ネットワークを使用するように各仮想ネットワークを構成します。

1.  ローカル コンピューターで Azure 管理ポータルでは、次のようにクリックします。 **ネットワーク** 左側のウィンドウでことを確認し、 **ステータス** の列 **TestLab** に設定されている **Created**します。
2.  クリックして **TestLab**, 、] をクリックし、 **構成**します。 [TestLab] ページで、 **サイト対サイト接続** ] をクリックして **ローカル ネットワークに接続する**です。 
3.   **ローカル ネットワーク**, [ **TestVNETLNet**します。
4.  クリックして **保存** タスク バーにします。 場合によっては、をクリックする必要があります **ゲートウェイ サブネットの追加** Azure VPN ゲートウェイで使用されるサブネットを作成します。
5.  をクリックして **ネットワーク** 左側のウィンドウでことを確認し、 **ステータス** TestVNET の列に設定されて **Created**します。
6.  クリックして **TestVNET**, 、] をクリックし、 **構成**します。 [TestVNET] ページで、 **サイト対サイト接続** ] をクリックして **ローカル ネットワークに接続する**です。 
7.   **ローカル ネットワーク**, [ **TestLabLNet**します。
8.  クリックして **保存** タスク バーにします。 場合によっては、をクリックする必要があります **ゲートウェイ サブネットの追加** Azure VPN ゲートウェイで使用されるサブネットを作成します。

次に、2 つの仮想ネットワーク用の仮想ネットワーク ゲートウェイを作成します。

1.  Azure 管理ポータルで、上、 **ネットワーク** ] ページで [ **TestLab**します。 ダッシュ ボード] ページのステータスを確認する必要があります **The Gateway Was Not Created**します。
2.  タスク バーで、クリックして **ゲートウェイの作成**, 、クリックして **動的ルーティング**します。 クリックして **はい** 入力を求められたらします。 ゲートウェイが完了して、状態が変化するまでの待機 **接続**します。 この処理には数分かかります。
3.  ダッシュ ボード] ページで、メモ、 **ゲートウェイの IP アドレス**です。 これは、TestLab 仮想ネットワーク用の Azure VPN Gateway のパブリック IP アドレスです。 VNet 間接続を構成する際に必要になるため、この IP アドレスをメモします。
4.  タスク バーで、クリックして **キーの管理**, とをクリップボードにコピーするキーの横にあるコピー アイコンをクリックします。 このキーをドキュメントに貼り付けて保存します。 VNet 間接続を構成する際に、このキーの値が必要になります。
5.  [ネットワーク] ページをクリックして **TestVNET**します。 ダッシュ ボード] ページのステータスを確認する必要があります **The Gateway Was Not Created**します。
6.  タスク バーで、クリックして **ゲートウェイの作成**, 、クリックして **動的ルーティング**します。 クリックして **はい** 入力を求められたらします。 ゲートウェイが完了して、状態が変化するまでの待機 **接続**します。 この処理には数分かかります。
7.  ダッシュ ボード] ページで、メモ、 **ゲートウェイの IP アドレス**です。 これは、TestVNET 仮想ネットワーク用の Azure VPN Gateway のパブリック IP アドレスです。 VNet 間接続を構成する際に必要になるため、この IP アドレスをメモします。

次に、仮想ネットワーク ゲートウェイを作成して取得したパブリック IP アドレスを使用して、TestLabLNet と TestVNETLNet ローカル ネットワークを構成します。

1.  Azure 管理ポータルの [ネットワーク] ページからクリックして **ローカル ネットワーク**します。 
2.  クリックして **TestLabLNet**, 、] をクリックし、 **編集** タスク バーでします。
3.  ローカル ネットワークの詳細ページの指定に (前の手順で手順 3.) から、TestLab 仮想ネットワークの仮想ネットワーク ゲートウェイの IP アドレスを入力 **VPN デバイスの IP アドレス (省略可能)**, 、右矢印をクリックします。
4.  [アドレス空間を指定します] ページで、チェック マークをクリックします。
5.  [ローカル ネットワーク] ページで、次のようにクリックします。 **TestVNETLNet**, 、順にクリック **編集** タスク バーでします。
6.  指定して、ローカル ネットワークの詳細] ページで (前の手順で手順 7.) から、TestVNET 仮想ネットワーク用の仮想ネットワーク ゲートウェイの IP アドレスを入力 **VPN デバイスの IP アドレス (省略可能)**, 、右矢印をクリックします。
7.  [アドレス空間を指定します] ページで、チェック マークをクリックします。

次に、両方のゲートウェイの事前共有キーを、同じ値 (Microsoft Azure 管理ポータルで TestLab 仮想ネットワークに対して自動的に決定されたキーの値) を使用するように構成します。 ローカル コンピューターで管理者レベルの Azure PowerShell コマンド プロンプトから次のコマンドを実行して、TestLab の事前共有キーの値を設定します。

    $preSharedKey="<The preshared key for the TestLab virtual network>"
    Set-AzureVNetGatewayKey -VNetName TestVNET -LocalNetworkSiteName TestLabLNet â€“SharedKey $preSharedKey

次に、ローカル コンピューターで Azure 管理ポータルの [ネットワーク] ページでをクリックして、 **TestLab** 仮想ネットワーク] をクリックして **ダッシュ ボード**, 、] をクリックし、 **接続** タスク バーでします。 TestLab 仮想ネットワークが接続された状態になるまで待ちます。

現在の構成は次のようになります。

![](./media/virtual-networks-setup-simulated-hybrid-cloud-environment-testing/CreateSimHybridCloud_3.png)
 
## フェーズ 4: DC2 を構成する

まず、DC2 用の Azure の仮想マシンを作成します。 ローカル コンピューターで Azure PowerShell コマンド プロンプトから次のコマンドを実行します。

    $ServiceName="<Your cloud service name from Phase 2>"
    $cred=Get-Credential â€“Message "Type the name and password of the local administrator account for DC2."
    $image = Get-AzureVMImage | where { $_.ImageFamily -eq "Windows Server 2012 R2 Datacenter" } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1
    $vm1=New-AzureVMConfig -Name DC2 -InstanceSize Medium -ImageName $image
    $vm1 | Add-AzureProvisioningConfig -Windows -AdminUsername $cred.GetNetworkCredential().Username -Password $cred.GetNetworkCredential().Password
    $vm1 | Set-AzureSubnet -SubnetNames TestSubnet
    $vm1 | Set-AzureStaticVNetIP -IPAddress 192.168.0.4
    $vm1 | Add-AzureDataDisk -CreateNew -DiskSizeInGB 20 -DiskLabel ADFiles â€“LUN 0 -HostCaching None
    New-AzureVM â€“ServiceName $ServiceName -VMs $vm1 -VNetName TestVNET

次に、新しい DC2 仮想マシンにログオンします。

1.  Azure 管理ポータルの左側のウィンドウで、クリックして **仮想マシン**, 、] をクリックし、 **を実行している** で、 **ステータス** DC2 の列です。
2.  タスク バーで、クリックして **接続**します。 
3.  DC2.rdp を開くダイアログ ボックスが表示されたら、クリックして **開く**します。
4.  リモート デスクトップ接続のメッセージ ボックスが表示されたら、クリックして **接続**します。
5.  資格情報の入力を求められたら、次の情報を使用します。
- [名前]: **dc2 \\**[ローカル管理者アカウント名]
- パスワード: [ローカル管理者アカウントのパスワード]
6.  証明書に関するリモート デスクトップ接続のメッセージ ボックスが表示されたら、クリックして **はい**します。

続いて、基本的な接続テストを行うためのトラフィックを許可するよう Windows ファイアウォール規則を構成します。 DC2 で管理者レベルの Windows PowerShell コマンド プロンプトから次のコマンドを実行します。

    Set-NetFirewallRule -DisplayName "File and Printer Sharing (Echo Request - ICMPv4-In)" -enabled True
    ping dc1.corp.contoso.com

ping コマンドで IP アドレス 10.0.0.4 からの応答が 4 回成功する必要があります。 これは VNet 間接続を介したトラフィックのテストです。

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

次に、DC2 を corp.contoso.com ドメインのレプリカ ドメイン コントローラーとして構成します。 DC2 で Windows PowerShell コマンド プロンプトから次のコマンドを実行します。

    Install-WindowsFeature AD-Domain-Services -IncludeManagementTools
    Install-ADDSDomainController -Credential (Get-Credential CORP\User1) -DomainName "corp.contoso.com" -InstallDns:$true -DatabasePath "F:\NTDS" -LogPath "F:\Logs" -SysvolPath "F:\SYSVOL"

CORP\User1 のパスワードとディレクトリ サービス復元モード (DSRM) のパスワードの両方を入力し、DC2 を再起動するよう求められます。

これで TestVNET 仮想ネットワーク独自の DNS サーバー (DC2) が設定されたので、この DNS サーバーを使用するように TestVNET 仮想ネットワークを構成する必要があります。

1.  Azure 管理ポータルの左側のウィンドウで、クリックして **ネットワーク**, 、クリックして **TestVNET**します。
2.  クリックして **構成**します。
3.   **DNS サーバー**, 、削除、10.0.0.4 のエントリ。
4.   **DNS サーバー**, 、エントリを追加 **DC2** 名として、 **192.168.0.4** の IP アドレスとします。 
5.  下部にあるコマンド バーで、クリックして **保存**, 、] をクリックし、 **はい** 入力を求められたらします。 TestVNet ネットワークの更新が完了するまで待ちます。

現在の構成は次のようになります。

![](./media/virtual-networks-setup-simulated-hybrid-cloud-environment-testing/CreateSimHybridCloud_4.png)
 
これで、シミュレートされたハイブリッド クラウド環境でテストする準備が整いました。

このテスト環境では、次の構成を構築することもできます。

- [SharePoint イントラネット ファーム](virtual-networks-setup-sharepoint-hybrid-cloud-testing.md)
- [Web ベースの LOB アプリケーション](virtual-networks-setup-lobapp-hybrid-cloud-testing.md)
- [Office 365 ディレクトリ同期 (DirSync) サーバー](virtual-networks-setup-dirsync-hybrid-cloud-testing.md)

## その他のリソース

[テスト用のハイブリッド クラウド環境の設定](virtual-networks-setup-hybrid-cloud-environment-testing.md)

[VNet 間の接続の構成](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md)

[基本構成テスト環境](../virtual-machines/virtual-machines-base-configuration-test-environment.md)

[Azure ハイブリッド クラウド テスト環境](../virtual-machines/virtual-machines-hybrid-cloud-test-environments.md)

[Azure インフラストラクチャ サービス実装ガイドライン](../virtual-machines/virtual-machines-infrastructure-services-implementation-guidelines.md)

## <a id="costs"></a>この環境の継続的な費用を最小限に抑える

この環境で稼働中の仮想マシンの費用を最小限に抑えるためには、できるだけ迅速に必要なテストとデモンストレーションを行ってからそれらの仮想マシンを削除するか、使用していない間は仮想マシンをシャットダウンします。 たとえば、Azure Automation と Runbook を使用して、各営業日の終わりに TestLab と Test_VNET 仮想ネットワーク内の仮想マシンを自動的にシャットダウンすることができます。 詳細については、次を参照してください。 [Azure オートメーションの使用](../automation-create-runbook-from-samples.md)します。 企業ネットワーク サブネット上の仮想マシンを再度起動する場合は、最初に DC1 を起動します。

Azure VPN Gateway は、2 台 1 組みの Azure の仮想マシンとして実装されており、継続的な費用が発生します。 詳細については、次を参照してください。 [Virtual Network 料金](http://azure.microsoft.com/pricing/details/virtual-network/)します。 2 つの VPN Gateway (それぞれ TestLab 用と TestVNET 用) の費用を最小限に抑えるためには、テスト環境を作成し、できる限り迅速に必要なテストとデモンストレーションを行うか、次の手順でゲートウェイを削除します。
 
1.  ローカル コンピューターで Azure 管理ポータルでは、次のようにクリックします。 **ネットワーク** 左のウィンドウで **TestLab**, 、] をクリックし、 **ダッシュ ボード**します。
2.  タスク バーで、クリックして **ゲートウェイの削除**します。 クリックして **はい** 入力を求められたらします。 ゲートウェイが削除され、その状態に変更するを待って **The Gateway Was Not Created**します。
3.  クリックして **ネットワーク** 左のウィンドウで **TestVNET**, 、順にクリック **ダッシュ ボード**します。
4.  タスク バーで、クリックして **ゲートウェイの削除**します。 クリックして **はい** 入力を求められたらします。 ゲートウェイが削除され、その状態に変更するを待って **The Gateway Was Not Created**します。

ゲートウェイを削除した後で、このテスト環境を復元する場合は、まず新しいゲートウェイを作成する必要があります。

1.  ローカル コンピューターで Azure 管理ポータルでは、次のようにクリックします。 **ネットワーク** 左側のウィンドウをクリックし **TestLab**します。 ダッシュ ボード] ページのステータスを確認する必要があります **The Gateway Was Not Created**します。
2.  タスク バーで、クリックして **ゲートウェイの作成**, 、クリックして **動的ルーティング**します。 クリックして **はい** 入力を求められたらします。 ゲートウェイが完了して、状態が変化するまでの待機 **接続**します。 この処理には数分かかります。
3.  ダッシュ ボード] ページで、メモ、 **ゲートウェイの IP アドレス**です。 これは、TestLab 仮想ネットワーク用の Azure VPN Gateway の新しいパブリック IP アドレスです。 この IP アドレスは、TestLabLNet ローカル ネットワークを再構成するために必要です。
4.  タスク バーで、クリックして **キーの管理**, 、クリップボードにコピーするキーの横にあるコピー アイコンをクリックします。 このキーの値をドキュメントに貼り付けて保存します。 このキーの値は、TestVNET 仮想ネットワーク用の VPN Gateway を再構成するために必要です。
5.  ローカル コンピューターで Azure 管理ポータルでは、次のようにクリックします。 **ネットワーク** 左側のウィンドウをクリックし **TestVNET**します。 ダッシュ ボード] ページのステータスを確認する必要があります **The Gateway Was Not Created**します。
6.  タスク バーで、クリックして **ゲートウェイの作成**, 、クリックして **動的ルーティング**します。 クリックして **はい** 入力を求められたらします。 ゲートウェイが作成され、ゲートウェイの状態が [接続中] に変わるまで待ちます。 この処理には数分かかります。
7.  ダッシュ ボード] ページで、メモ、 **ゲートウェイの IP アドレス**です。 これは、TestVNET 仮想ネットワーク用の Azure VPN Gateway の新しいパブリック IP アドレスです。 この IP アドレスは、TestVNETLNet ローカル ネットワークを再構成するために必要です。

次に、仮想ネットワーク ゲートウェイを作成して取得した新しいパブリック IP アドレスを使用して、TestLabLNet と TestVNETLNet ローカル ネットワークを構成します。

1.  Azure 管理ポータルの [ネットワーク] ページからクリックして **ローカル ネットワーク**します。 
2.  クリックして **TestLabLNet**, 、] をクリックし、 **編集** タスク バーでします。
3.  ローカル ネットワークの詳細ページの指定に (前の手順で手順 3.) から、TestLab 仮想ネットワークの仮想ネットワーク ゲートウェイの IP アドレスを入力 **VPN デバイスの IP アドレス (省略可能)**, 、右矢印をクリックします。
4.  [アドレス空間を指定します] ページで、チェック マークをクリックします。
5.  [ローカル ネットワーク] ページで、次のようにクリックします。 **TestVNETLNet**, 、順にクリック **編集** タスク バーでします。
6.  指定して、ローカル ネットワークの詳細] ページで (前の手順で手順 7.) から、TestVNET 仮想ネットワーク用の仮想ネットワーク ゲートウェイの IP アドレスを入力 **VPN デバイスの IP アドレス (省略可能)**, 、右矢印をクリックします。
7.  [アドレス空間を指定します] ページで、チェック マークをクリックします。

次に、両方のゲートウェイの事前共有キーを、同じ値 (Microsoft Azure 管理ポータルで TestLab 仮想ネットワークに対して自動的に決定されたキーの値) を使用するように構成します。 ローカル コンピューターで管理者レベルの Azure PowerShell コマンド プロンプトから次のコマンドを実行して、TestLab の事前共有キーの値を設定します。

    $preSharedKey="<The preshared key for the TestLab virtual network>"
    Set-AzureVNetGatewayKey -VNetName TestVNET -LocalNetworkSiteName TestLabLNet â€“SharedKey $preSharedKey

次に、Azure 管理ポータルの [ネットワーク] ページでをクリックして、 **TestLab** バーチャル ネットワーク、およびクリック **接続** タスク バーでします。 TestLab 仮想ネットワークが TestVNET ローカル ネットワークに接続された状態になるまで待ちます。
 

