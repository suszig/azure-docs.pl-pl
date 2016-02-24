<properties 
    pageTitle="ハイブリッド クラウドのテスト環境 | Microsoft Azure" 
    description="シンプル化されたオンプレミス ネットワークを含む、IT プロまたは開発のテスト用のハイブリッド クラウド環境を作成する方法について説明します。" 
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

# テスト用のハイブリッド クラウド環境の設定

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)] リソース マネージャーのモデルです。
 

このトピックでは、Microsoft Azure でテスト用のハイブリッド クラウド環境を作成する手順について説明します。 完成すると次のような構成になります。

![](./media/virtual-networks-setup-hybrid-cloud-environment-testing/CreateHybridCloudVNet_5.png)

この構成では、インターネット上の現在の場所から実際のハイブリッド運用環境をシミュレートします。 構成は次のとおりです。

-  簡略化されたオンプレミス ネットワーク (Corpnet サブネット)。
-  Azure でホストされているクロスプレミス仮想ネットワーク (TestVNET)。
-  サイト間 VPN 接続
-  TestVNET 仮想ネットワークのセカンダリ ドメイン コントローラー。

この構成を基盤や共通の出発点にして以下を実行できます。

-  ハイブリッド クラウド環境でアプリケーションを開発とテストする。
-  コンピューターのテスト構成を作成する。この構成には、ハイブリッド クラウド ベースの IT ワークロードに対応するために、Corpnet サブネット上のコンピューターと TestVNET 仮想ネットワーク内のコンピューターが含まれます。

このハイブリッド クラウド テスト環境を設定する手順は、大きく次の 5 つのフェーズに分かれています。

1.  Corpnet サブネット上のコンピューターを構成する。
2.  RRAS1 を構成する。
3.  クロスプレミスの Azure 仮想ネットワークを作成する。
4.  サイト間 VPN 接続を作成する。
5.  DC2 を構成する。 

無料評価版にサインアップすることができます、Azure サブスクリプションがない場合 [Azure](http://azure.microsoft.com/pricing/free-trial/)します。 MSDN サブスクリプションがある場合は、次を参照してください。 [MSDN サブスクライバー向けの Azure 特典](http://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/)します。

>[AZURE.NOTE] 仮想マシンと Azure で仮想ネットワーク ゲートウェイが実行されているときに継続的な費用が発生します。 その費用は、無料試用版、MSDN サブスクリプション、または有料のサブスクリプションに対して請求されます。 使用しない場合は、このテスト環境を実行中のコストを削減するには、次を参照してください。 [この環境の継続的な費用を最小限に抑える](#costs) の詳細については、このトピックにします。

この構成では、パブリック IP アドレスを使用してインターネットに直接接続された最大 4 台のコンピューターのテスト サブネットが必要です。 これらのリソースを取得していない場合することもできます [テスト用のシミュレートされたハイブリッド クラウド環境をセットアップ](virtual-networks-setup-simulated-hybrid-cloud-environment-testing.md)します。 シミュレートされたハイブリッド クラウド テスト環境に必要なのは Azure サブスクリプションのみです。

## フェーズ 1: Corpnet サブネット上のコンピューターを構成する

「の Corpnet サブネットの構成手順」セクション手順を使用、 [テスト ラボ ガイド: 基本構成の Windows Server 2012 R2](http://www.microsoft.com/download/details.aspx?id=39638) Corpnet という名前のサブネットに DC1、APP1、および CLIENT1 コンピューターを構成します。 **このサブネットは、RRAS1 コンピューターを通じてインターネットに直接接続するため、社内ネットワークから分離する必要があります。** 

次に、CORP\User1 の資格情報を使用して、DC1 にログオンします。 コンピューターとユーザーが認証にローカル ドメイン コントローラーを使用するよう CORP ドメインを構成するために、管理者レベルの Windows PowerShell コマンド プロンプトから次のコマンドを実行します。

    New-ADReplicationSite -Name "TestLab" 
    New-ADReplicationSite -Name "TestVNET"
    New-ADReplicationSubnet “Name "10.0.0.0/8" “Site "TestLab"
    New-ADReplicationSubnet “Name "192.168.0.0/16" “Site "TestVNET

現在の構成は次のようになります。

![](./media/virtual-networks-setup-hybrid-cloud-environment-testing/CreateHybridCloudVNet_1.png)
 
## フェーズ 2: RRAS1 を構成する

RRAS1 は、Corpnet サブネットと TestVNET 仮想ネットワークの間にトラフィック ルーティングと VPN デバイスのサービスを提供します。 RRAS1 には、2 つのネットワーク アダプターが取り付けられている必要があります。

まず、RRAS1 にオペレーティング システムをインストールします。

1.  Windows Server 2012 R2 のインストールを開始します。
2.  指示に従ってインストールを完了し、ローカル管理者アカウントの強力なパスワードを設定します。 ローカル管理者アカウントを使用してログオンします。
3.  インターネットにアクセスできるネットワークに RRAS1 を接続し、Windows Update を実行して Windows Server 2012 R2 の最新の更新プログラムをインストールします。
4.  ネットワーク アダプターの 1 つを Corpnet サブネットに接続し、もう 1 つをインターネットに直接接続します。 RRAS1 は、インターネット ファイアウォールの内側には配置できますが、ネットワーク アドレス変換器 (NAT) の内側には配置しないでください。

次に、RRAS1 の TCP/IP プロパティを構成します。 インターネット サービス プロバイダー (ISP) のアドレス、サブネット マスク (またはプレフィックス長)、既定のゲートウェイと DNS サーバーを含むパブリック IP アドレス構成が必要です。

RRAS1 で管理者レベルの Windows PowerShell コマンド プロンプトから次のコマンドを実行します。 これらのコマンドを実行する前に、変数の値を入力し、削除、< と > 文字です。 表示からネットワーク アダプターの現在の名前を取得できます、 **Get-netadapter** コマンドです。

    $corpnetAdapterName="<Name of the adapter attached to the Corpnet subnet>"
    $internetAdapterName="<Name of the adapter attached to the Internet>"
    [Ipaddress]$publicIP="<Your public IP address>"
    $publicIPpreflength=<Prefix length of your public IP address>
    [IPAddress]$publicDG="<Your ISP default gateway>"
    [IPAddress]$publicDNS="<Your ISP DNS server(s)>"
    Rename-NetAdapter -Name $corpnetAdapterName -NewName Corpnet
    Rename-NetAdapter -Name $internetAdapterName -NewName Internet
    New-NetIPAddress -InterfaceAlias "Internet" -IPAddress $publicIP -PrefixLength $publicIPpreflength “DefaultGateway $publicDG
    Set-DnsClientServerAddress -InterfaceAlias Internet -ServerAddresses $publicDNS
    New-NetIPAddress -InterfaceAlias "Corpnet" -IPAddress 10.0.0.2 -AddressFamily IPv4 -PrefixLength 24
    Set-DnsClientServerAddress -InterfaceAlias "Corpnet" -ServerAddresses 10.0.0.1
    Set-DnsClient -InterfaceAlias "Corpnet" -ConnectionSpecificSuffix corp.contoso.com
    New-NetFirewallRule -DisplayName "Allow ICMPv4-Input" -Protocol ICMPv4
    New-NetFirewallRule -DisplayName "Allow ICMPv4-Output" -Protocol ICMPv4 -Direction Outbound
    Disable-NetAdapterBinding -Name "Internet" -ComponentID ms_msclient
    Disable-NetAdapterBinding -Name "Internet" -ComponentID ms_server
    ping dc1.corp.contoso.com

最後のコマンドで、IP アドレス 10.0.0.1 から 4 つの応答が返ってきていることを確認します。

現在の構成は次のようになります。

![](./media/virtual-networks-setup-hybrid-cloud-environment-testing/CreateHybridCloudVNet_2.png)

## フェーズ 3: クロスプレミスの Azure Virtual Network を作成する

最初に、ログオンして、 [Azure 管理ポータル](https://manage.windowsazure.com/) 、azure サブスクリプションの資格情報し、TestVNET という名前の仮想ネットワークを作成します。

1.  Azure 管理ポータルのタスク バーで、クリックして **新規 > ネットワーク サービス > 仮想ネットワーク > カスタム作成**します。
2.  仮想ネットワークの詳細] ページで、次のように入力します。 **TestVNET** で **名**します。
3.   **場所**, 、現在の場所に適切なデータ センターを選択します。
4.  次へ進む矢印をクリックします。
5.  [DNS サーバーおよび VPN 接続] ページで **DNS サーバー**, 、型 **DC1** で **選択または入力]**, 、型 **10.0.0.1** で **IP アドレス**, 、し、[ **サイト対サイト VPN の構成**します。
6.   **ローカル ネットワーク**, [ **新しいローカル ネットワークを指定して**します。 
7.  次へ進む矢印をクリックします。
8.  [サイト間接続] ページで:
    -  **名**, 、型 **Corpnet**します。 
    -  **VPN デバイスの IP アドレス**, 、RRAS1 のインターネット用インターフェイスに割り当てられたパブリック IP アドレスを入力します。
    -  **アドレス空間**, の **開始 IP** 列に「 **10.0.0.0**します。  **CIDR (アドレス数)**, [ **/8**, 、] をクリックし、 **[アドレス空間の**です。
9.  次へ進む矢印をクリックします。
10. [Virtual Network アドレス空間] ページで、次の内容を構成します。
    -  **アドレス空間**, の **開始 IP**, [ **192.168.0.0**します。
    -  **サブネット**, 、クリックして **subnet-1** し名前を **TestSubnet**します。 
    -  **CIDR (アドレス数)** 、TestSubnet の列をクリックして **/24 (256)**します。
    - クリックして **ゲートウェイ サブネットの追加**します。
11. [完了] アイコンをクリックします。 仮想ネットワークが作成されるまで待ってから、次に進みます。

」の手順を次に、に従って [をインストールして、Azure PowerShell を構成する方法](../install-configure-powershell.md) 、ローカル コンピューターに Azure PowerShell をインストールします。

次に、TestVNET 仮想ネットワークの新しいクラウド サービスを作成します。 一意の名前を選ぶ必要があります。 たとえば、名前を付けます testvnet-*UniqueSequence*, を *UniqueSequence* 、組織の略称です。 たとえば、組織の名前が Tailspin Toys であれば、クラウド サービスに TestVNET-Tailspin という名前を付けることができます。

名前が一意かどうかは、ローカル コンピューターで次の Azure PowerShell コマンドを使用することで確認できます。

    Test-AzureName -Service <Proposed cloud service name>

このコマンドで "False" が返された場合は、提案した名前は一意です。 次のコマンドでクラウド サービスを作成します。

    New-AzureService -Service <Unique cloud service name> -Location "<Same location name as your virtual network>"

次に、TestVNET 仮想ネットワークの新しいストレージ アカウントを作成します。 一意の名前を選ぶ必要があります。 ストレージ アカウントの名前が一意であるかどうかは、次のコマンドを使用することで確認できます。

    Test-AzureName -Storage <Proposed storage account name>

このコマンドで "False" が返された場合は、提案した名前は一意です。 次のコマンドを使用して、ストレージ アカウントを作成して設定します。

    New-AzureStorageAccount -StorageAccountName <Unique storage account name> -Location "<Same location name as your virtual network>"
    Set-AzureStorageAccount -StorageAccountName <Unique storage account name>

現在の構成は次のようになります。

![](./media/virtual-networks-setup-hybrid-cloud-environment-testing/CreateHybridCloudVNet_3.png)

 
## フェーズ 4: サイト間 VPN 接続を作成する

まず、仮想ネットワーク ゲートウェイを作成します。

1.  ローカル コンピューターで Azure 管理ポータルでをクリックして **ネットワーク** 左側のウィンドウでことを確認し、 **ステータス** TestVNET の列に設定されて **Created**します。
2.  クリックして **TestVNET**します。 ダッシュ ボード] ページのステータスを確認する必要があります **ゲートウェイは作成されません**します。
3.  タスク バーで、クリックして **ゲートウェイの作成**, 、クリックして **動的ルーティング**します。 クリックして **はい** 入力を求められたらします。 ゲートウェイが完了して、状態が変化するまでの待機 **接続**します。 この処理には数分かかります。
4.  ダッシュ ボード] ページで、メモ、 **ゲートウェイの IP アドレス**です。 これは、TestVNET 仮想ネットワーク用の Azure VPN Gateway のパブリック IP アドレスです。 この IP アドレスは、RRAS1 を構成する際に必要になります。
5.  タスク バーで、クリックして **キーの管理**, 、クリップボードにコピーするキーの横にあるコピー アイコンをクリックします。 このキーをドキュメントに貼り付けて保存します。 このキー値は、RRAS1 を構成する際に必要になります。 

次に、ルーティングとリモート アクセス サービスを使用して、Corpnet サブネットの VPN デバイスとして動作するように RRAS1 を構成します。 ローカル管理者として RRAS1 にログオンし、Windows PowerShell コマンド プロンプトで次のコマンドを実行します。

    Import-Module ServerManager
    Install-WindowsFeature RemoteAccess -IncludeManagementTools
    Add-WindowsFeature -name Routing -IncludeManagementTools

次に、Azure VPN Gateway からのサイト間 VPN 接続を受信するように RRAS1 を構成します。 RRAS1 を再起動し、ローカル管理者としてログオンして、Windows PowerShell コマンド プロンプトで次のコマンドを実行します。 Azure VPN Gateway の IP アドレスとキー値を入力する必要があります。

    $PresharedKey="<Key value>"
    Import-Module RemoteAccess
    Install-RemoteAccess -VpnType VpnS2S
    Add-VpnS2SInterface -Protocol IKEv2 -AuthenticationMethod PSKOnly -Persistent -NumberOfTries 3 -ResponderAuthenticationMethod PSKOnly -Name S2StoTestVNET -Destination "<IP address of the Azure VPN gateway>" -IPv4Subnet @("192.168.0.0/24:100") -SharedSecret $PresharedKey
    Set-VpnServerIPsecConfiguration -EncryptionType MaximumEncryption
    Set-VpnServerIPsecConfiguration -SADataSizeForRenegotiationKilobytes 33553408
    New-ItemProperty -Path HKLM:\System\CurrentControlSet\Services\RemoteAccess\Parameters\IKEV2 -Name SkipConfigPayload -PropertyType DWord -Value 1
    Restart-Service RemoteAccess

次に、ローカル コンピューター上の Azure 管理ポータルに移動し、TestVNET 仮想ネットワークの状態の表示されるまで待機 **接続**します。

続いて、インターネット上の場所への変換されたトラフィックをサポートするように RRAS1 を構成します。 RRAS1 で:

1.  スタート画面から次のように入力します。 **rras**, 、クリックして **ルーティングとリモート アクセス**します。 
2.  コンソール ツリーでサーバー名を開き、をクリックして **IPv4**します。
3.  右クリック **全般**, 、クリックして **新しいルーティング プロトコル**します。
4.  をクリックして **NAT**, 、] をクリックし、 **OK**します。
5.  コンソール ツリーで、右クリック **NAT**, 、] をクリックして **新しいインターフェイス**, 、] をクリックして **Corpnet**, 、] をクリックし、 **[ok]** 2 回クリックします。
6.  右クリック **NAT**, 、] をクリックして **新しいインターフェイス**, 、] をクリックして **インターネット**, 、] をクリックし、 **OK**します。
7.   **NAT** ] タブ、[ **インターネットに接続されたパブリック インターフェイス**, を選択 **このインターフェイスで NAT を有効にする**, 、順にクリック **[ok]**します。


次に、既定のゲートウェイとして RRAS1 を使用するように DC1、APP1、CLIENT1 を構成します。
 
DC1 で管理者レベルの Windows PowerShell コマンド プロンプトから次のコマンドを実行します。

    New-NetRoute -DestinationPrefix "0.0.0.0/0" -InterfaceAlias "Ethernet" -NextHop 10.0.0.2
    Set-DhcpServerv4OptionValue -Router 10.0.0.2

インターフェイスの名前がイーサネットでない場合を使用して、 **Get-netadapter** コマンド インターフェイス名を特定します。

APP1 で管理者レベルの Windows PowerShell コマンド プロンプトから次のコマンドを実行します。

    New-NetRoute -DestinationPrefix "0.0.0.0/0" -InterfaceAlias "Ethernet" -NextHop 10.0.0.2

CLIENT1 で管理者レベルの Windows PowerShell コマンド プロンプトから次のコマンドを実行します。

    ipconfig /renew

現在の構成は次のようになります。
 

![](./media/virtual-networks-setup-hybrid-cloud-environment-testing/CreateHybridCloudVNet_4.png)


## フェーズ 5: DC2 を構成する

まず、ローカル コンピューターで Azure PowerShell コマンド プロンプトから次のコマンドを実行して、DC2 用に Azure 仮想マシンを作成します。

    $ServiceName="<Your cloud service name from Phase 3>"
    $image = Get-AzureVMImage | where { $_.ImageFamily -eq "Windows Server 2012 R2 Datacenter" } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1
    $vm1=New-AzureVMConfig -Name DC2 -InstanceSize Medium -ImageName $image
    $cred=Get-Credential -Message "Type the name and password of the local administrator account for DC2."
    $vm1 | Add-AzureProvisioningConfig -Windows -AdminUsername $cred.GetNetworkCredential().Username -Password $cred.GetNetworkCredential().Password 
    $vm1 | Add-AzureProvisioningConfig -Windows -AdminUsername $LocalAdminName -Password $LocalAdminPW  
    $vm1 | Set-AzureSubnet -SubnetNames TestSubnet
    $vm1 | Set-AzureStaticVNetIP -IPAddress 192.168.0.4
    $vm1 | Add-AzureDataDisk -CreateNew -DiskSizeInGB 20 -DiskLabel ADFiles -LUN 0 -HostCaching None
    New-AzureVM -ServiceName $ServiceName -VMs $vm1 -VNetName TestVNET


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

ping コマンドで IP アドレス 10.0.0.1 からの応答が 4 回成功する必要があります。 これはサイト間 VPN 接続を介したトラフィックのテストです。

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
3.   **DNS サーバー**, 、削除、 **10.0.0.1** エントリです。
4.   **DNS サーバー**, 、エントリを追加 **DC2** 名として、 **192.168.0.4** の IP アドレスとします。 
5.  下部にあるコマンド バーで、クリックして **保存**します。
6.  Azure 管理ポータルの左側のウィンドウでをクリックして **仮想マシン**, 、クリックして、 **ステータス** DC2 の横の列です。
7.  コマンド バーで、クリックして **再起動**します。 DC2 が再起動するのを待ちます。


現在の構成は次のようになります。

![](./media/virtual-networks-setup-hybrid-cloud-environment-testing/CreateHybridCloudVNet_5.png)

 
これで、ハイブリッド クラウド環境をテストする準備が整いました。

## その他のリソース

[テスト用のハイブリッド クラウドでの SharePoint イントラネット ファームの設定](virtual-networks-setup-sharepoint-hybrid-cloud-testing.md)

[テスト用のハイブリッド クラウドでの Web ベース LOB アプリケーションの設定](virtual-networks-setup-lobapp-hybrid-cloud-testing.md)

[テスト用のハイブリッド クラウドでの Office 365 ディレクトリ同期 (DirSync) の設定](virtual-networks-setup-dirsync-hybrid-cloud-testing.md)

[テスト用のシミュレートされたハイブリッド クラウド環境の設定](virtual-networks-setup-simulated-hybrid-cloud-environment-testing.md)

[Azure ハイブリッド クラウド テスト環境](../virtual-machines/virtual-machines-hybrid-cloud-test-environments.md)

[Azure インフラストラクチャ サービス実装ガイドライン](../virtual-machines/virtual-machines-infrastructure-services-implementation-guidelines.md)

## この環境の継続的な費用を最小限に抑える

この環境で稼働中の仮想マシンの費用を最小限に抑えるためには、できるだけ迅速に必要なテストとデモンストレーションを行ってからそれらの仮想マシンを削除するか、使用していない間は仮想マシンをシャットダウンします。 たとえば、Azure Automation と Runbook を使用して、各営業日の終わりに Test_VNET 仮想ネットワーク内の仮想マシンを自動的にシャットダウンできます。 詳細については、次を参照してください。 [Azure オートメーションの使用](../automation-create-runbook-from-samples.md)します。 

Azure VPN Gateway は、2 台 1 組みの Azure の仮想マシンとして実装されており、継続的な費用が発生します。 詳細については、次を参照してください。 [Virtual Network 料金](http://azure.microsoft.com/pricing/details/virtual-network/)します。 VPN Gateway の費用を最小限に抑えるためには、テスト環境を作成し、できる限り迅速に必要なテストとデモンストレーションを行うか、次の手順でゲートウェイを削除します。 

1.  ローカル コンピューターで Azure 管理ポータルでは、次のようにクリックします。 **ネットワーク** 左のウィンドウで **TestVNET**, 、] をクリックし、 **ダッシュ ボード**します。
2.  タスク バーで、クリックして **ゲートウェイの削除**します。 クリックして **はい** 入力を求められたらします。 ゲートウェイが削除され、その状態に変更するを待って **The Gateway Was Not Created**します。

ゲートウェイを削除した後で、このテスト環境を復元する場合は、まず新しいゲートウェイを作成する必要があります。

1.  ローカル コンピューターで Azure 管理ポータルでは、次のようにクリックします。 **ネットワーク** 左側のウィンドウをクリックし **TestVNET**します。 ダッシュ ボード] ページのステータスを確認する必要があります **The Gateway Was Not Created**します。
2.  タスク バーで、クリックして **ゲートウェイの作成**, 、クリックして **動的ルーティング**します。 クリックして **はい** 入力を求められたらします。 ゲートウェイが完了して、状態が変化するまでの待機 **接続**します。 この処理には数分かかります。
3.  ダッシュ ボード] ページで、メモ、 **ゲートウェイの IP アドレス**です。 これは、TestVNET 仮想ネットワーク用の Azure VPN Gateway の新しいパブリック IP アドレスです。 この IP アドレスは、RRAS1 を再構成する際に必要になります。
4.  タスク バーで、クリックして **キーの管理**, 、クリップボードにコピーするキーの横にあるコピー アイコンをクリックします。 このキーの値をドキュメントに貼り付けて保存します。 このキー値は、RRAS1 を再構成する際に必要になります。 

次に、RRAS1 にローカル管理者としてログオンし、管理者レベルの Windows PowerShell コマンド プロンプトで、次のコマンドを実行します。このコマンドによって、新しいパブリック IP アドレスと事前共有キーを使用して RRAS1 が再構成されます。

    $PresharedKey="<Key value>"
    Set-VpnS2SInterface -Name S2StoTestVNET -Destination "<IP address of the Azure VPN gateway>" -SharedSecret $PresharedKey

次に、ローカル コンピューターで Microsoft Azure 管理ポータルにアクセスし、TestVNET 仮想ネットワークの状態が [接続中] と表示されるまで待ちます。
 

