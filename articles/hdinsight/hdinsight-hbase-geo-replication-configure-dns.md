<properties 
   pageTitle="2 つの Azure 仮想ネットワーク間の DNS の構成 | Microsoft Azure" 
   description="2 つの仮想ネットワーク間に VPN 接続とドメイン名の解決を構成する方法と、HBase geo レプリケーションを構成する方法について説明します。" 
   services="hdinsight,virtual-network" 
   documentationCenter="" 
   authors="mumian" 
   manager="paulettm" 
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="12/02/2015"
   ms.author="jgao"/>

# 2 つの Azure 仮想ネットワーク間の DNS の構成

> [AZURE.SELECTOR]
- [VPN 接続の構成](../hdinsight-hbase-geo-replication-configure-VNETs.md)
- [DNS の構成](hdinsight-hbase-geo-replication-configure-DNS.md)
- [HBase レプリケーションの構成](hdinsight-hbase-geo-replication.md) 


Azure 仮想ネットワークに DNS サーバーを追加して構成し、仮想ネットワーク内および仮想ネットワーク間の名前解決を処理する方法を説明します。

このチュートリアルの第 2 部では、 [シリーズ][hdinsight-hbase-geo-replication] HBase geo レプリケーションの作成にします。

- [2 つの仮想ネットワーク間に VPN 接続を構成します][hdinsight-hbase-geo-replication-vnet]
- 仮想ネットワーク用に DNS を構成します (このチュートリアル)
- [HBase geo レプリケーションの構成][hdinsight-hbase-geo-replication]


次の図は、2 つの仮想ネットワークで作成した [2 つの仮想ネットワーク間に VPN 接続を構成する][hdinsight-hbase-geo-replication-vnet]:

![HDInsight HBase レプリケーション仮想ネットワークの図][img-vnet-diagram]

##前提条件
このチュートリアルを読み始める前に、次の項目を用意する必要があります。

- **Azure サブスクリプション**します。 参照してください [取得 Azure 無料試用版](http://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/)します。

- **Azure PowerShell を実行できるワークステーション**します。 参照してください [のインストールおよび使用 Azure PowerShell](http://azure.microsoft.com/documentation/videos/install-and-use-azure-powershell/)します。

    PowerShell スクリプトを実行する前に、次のコマンドレットを使用して Azure サブスクリプションに接続されていることを確認します。

        Add-AzureAccount

    Azure サブスクリプションが複数ある場合は、次のコマンドレットを使用して、現在のサブスクリプションを設定します。

        Select-AzureSubscription <AzureSubscriptionName>

- **VPN で接続された 2 つの Azure 仮想ネットワーク**します。  手順については、次を参照してください。 [2 つの Azure 仮想ネットワーク間の VPN 接続を構成する][hdinsight-hbase-geo-replication-vnet]です。

>[AZURE.NOTE] Azure のサービス名と仮想マシン名は一意である必要があります。 このチュートリアルで使用する名前は、Contoso-[Azure Service/VM name]-[EU/US] です。 CONTOSO-VNET-EU は北ヨーロッパ データ センターでは、Azure の仮想ネットワークです。CONTOSO-DNS-US の DNS サーバーでは、米国東部の VM は、します。 データ センターです。 独自の名前を使用する必要があります。
 
 
##DNS サーバーとして使用する Azure 仮想マシンの作成

**Contoso-VNet-EU 内に Contoso-DNS-EU という名前の仮想マシンを作成するには**

1.  クリックして **新規**, 、**コンピューティング**, 、**仮想マシン**, 、**ギャラリーから**します。
2.  選択 **Windows Server 2012 R2 Datacenter**します。
3.  次のように入力します。
    - **仮想マシン名**: - CONTOSO-DNS-EU
    - **新しいユーザー名**: 
    - **新しいパスワード**: 
4.  次のように入力します。
    - **クラウド サービス**: 新しいクラウド サービスの作成
    - **リージョン/アフィニティ グループ/仮想ネットワーク**: (CONTOSO-VNET-EU オンにします)
    - **仮想ネットワーク サブネット**: subnet-1
    - **ストレージ アカウント**: 自動的に生成されたストレージ アカウントを使用
    
        クラウド サービス名は、仮想マシンの名前と同じになります。 この場合は、Contoso-DNS-EU となります。 これ以降の仮想マシンについては、同じクラウド サービスを使用することを選択できます。  同じクラウド サービスに属するすべての仮想マシンは、同じ仮想ネットワークとドメインのサフィックスを共有します。

        ストレージ アカウントは、仮想マシンのイメージ ファイルを格納するために使用されます。 
    - **エンドポイント**: (下にスクロールして [ **DNS**) 

仮想マシンが作成された後、内部 IP と外部 IP を確認します。

1.  仮想マシン名をクリックして **- CONTOSO-DNS-EU**します。
2.  クリックして **ダッシュ ボード**します。
3.  次の内容を記録します。
    - パブリック仮想 IP アドレス
    - 内部 IP アドレス


**Contoso-VNet-US 内に Contoso-DNS-US という名前の仮想マシンを作成するには** 

- 同じ手順を繰り返し、以下の値で仮想マシンを作成します。
    - 仮想マシン名: Contoso-DNS-US
    - リージョン/アフィニティ グループ/仮想ネットワーク: (Contoso-VNET-US を選択)
    - 仮想ネットワーク サブネット: Subnet-1
    - ストレージ アカウント: 自動的に生成されたストレージ アカウントを使用
    - エンドポイント: ([DNS] を選択)

##2 つの仮想マシンへの静的 IP アドレスの設定

DNS サーバーには静的 IP アドレスが必要です。  この手順は、Azure クラシック ポータルから行うことはできません。 Azure PowerShell を使用します。

**2 つの仮想マシンに静的 IP アドレスを構成するには**

1. Windows PowerShell ISE を開きます。
2. 次のコマンドレットを実行します。  

        Add-AzureAccount
        Select-AzureSubscription [YourAzureSubscriptionName]
        
        Get-AzureVM -ServiceName Contoso-DNS-EU -Name Contoso-DNS-EU | Set-AzureStaticVNetIP -IPAddress 10.1.0.4 | Update-AzureVM
        Get-AzureVM -ServiceName Contoso-DNS-US -Name Contoso-DNS-US | Set-AzureStaticVNetIP -IPAddress 10.2.0.4 | Update-AzureVM 

    ServiceName はクラウド サービス名です。 DNS サーバーはクラウド サービスの最初の仮想マシンであるため、クラウド サービス名は仮想マシンと同じ名前です。

    実際の名前と一致するように ServiceName および Name を更新することが必要になる場合があります。


##2 つの仮想マシンの DNS サーバー ロールの追加

**Contoso-DNS-EU の DNS サーバー ロールを追加するには**

1.  Azure クラシック ポータルでは、次のようにクリックします。 **仮想マシン** 左側です。 
2.  クリックして **- CONTOSO-DNS-EU**します。
3.  クリックして **ダッシュ ボード** 上部にあります。
4.  クリックして **接続** 下から RDP を使用して仮想マシンに接続するための指示に従います。
2.  RDP セッション内で、左下隅の Windows ボタンをクリックしてスタート画面を開きます。
3.  クリックして、 **サーバー マネージャー** を並べて表示します。
4.  クリックして **の役割と機能の追加**します。
5.  クリックして **次へ]**
6.  選択 **役割ベースまたは機能ベースのインストール**, 、クリックして **次**します。
7.  (これは強調表示されます既に) DNS 仮想マシンを選択し、クリックして **次**します。
8.  確認 **DNS サーバー**します。
9.  クリックして **機能の追加**, 、] をクリックし、 **続行**します。
10. クリックして **次** 3 回クリックして **インストール**します。 

**Contoso-DNS-US の DNS サーバー ロールを追加するには**

- DNS ロールを追加する手順を繰り返します **- CONTOSO-DNS-US**します。

##仮想ネットワークへの DNS サーバーの割り当て

**2 つの DNS サーバーを登録するには**

1.  Azure クラシック ポータルでは、次のようにクリックします。 **新規**, 、**ネットワーク サービス**, 、**仮想ネットワーク**, 、**DNS サーバーの登録**します。
2.  次のように入力します。
    - **名前**: - CONTOSO-DNS-EU
    - **DNS サーバーの IP アドレス**: 10.1.0.4 – IP アドレスの DNS サーバーの仮想マシンの IP アドレスと一致する必要があります。
     
3.  手順を繰り返し、次の設定で Contoso-DNS-US を登録します。
    - **名前**: - CONTOSO-DNS-US
    - **DNS サーバーの IP アドレス**:「10.2.0.4

**2 つの仮想ネットワークに 2 つの DNS サーバーを割り当てるには**

1.  クリックして **ネットワーク** クラシック ポータルで左側のウィンドウからです。
2.  クリックして **- CONTOSO-VNET-EU**します。
3.  クリックして **構成**します。
4.  選択 **- CONTOSO-DNS-EU** で、 **dns サーバー** セクションです。
5.  をクリックして **保存** ページ、およびクリックの下部にある **はい** を確認します。
6.  割り当てる手順を繰り返して、 **- CONTOSO-DNS-US** DNS サーバーを **Contoso VNet US** 仮想ネットワークです。

仮想ネットワークにデプロイされているすべての仮想マシンを再起動し、DNS サーバーの構成を更新する必要があります。

**仮想マシンを再起動するには**

1. Azure クラシック ポータルでは、次のようにクリックします。 **仮想マシン** 左側です。
2. クリックして **- CONTOSO-DNS-EU**します。
3. クリックして **ダッシュ ボード** 上部にあります。
4. クリックして **再起動** 下部にあります。
5. 再起動する同じ手順を繰り返して **- CONTOSO-DNS-US**します。


##DNS 条件付フォワーダーの構成

各仮想ネットワーク上の DNS サーバーは、その仮想ネットワーク内の DNS 名のみを解決できます。 ピア仮想ネットワークでの名前解決のためにピア DNS サーバーを指し示すには、条件付フォワーダーを構成する必要があります。

条件付フォワーダーを構成するには、2 つの DNS サーバーのドメイン サフィックスを知っている必要があります。 DNS サフィックスは、仮想マシンの作成時に使用した Cloud Services の構成によって異なることがあります。 仮想ネットワークで使用されている DNS サフィックスごとに、条件付フォワーダーを追加する必要があります。 

**2 つの DNS サーバーのドメイン サフィックスを調べるには**

1. Rdp 接続 **- CONTOSO-DNS-EU**します。
2. Windows PowerShell コンソールまたはコマンド プロンプトを開きます。
3. 実行 **ipconfig**, 、して書き留めます **接続専用 DNS サフィックス**します。
4. RDP セッションを閉じないでください。引き続き必要になります。 
5. 確認する同じ手順を繰り返して、 **接続専用 DNS サフィックス** の **- CONTOSO-DNS-US**します。


**DNS フォワーダーを構成するには**
 
1.  RDP セッションから **- CONTOSO-DNS-EU**, 、左下の Windows キーをクリックします。
2.  クリックして **管理ツール**します。
3.  クリックして **DNS**します。
4.  左側のウィンドウで展開 **DSN**, 、**- CONTOSO-DNS-EU**します。
5.  次の情報を入力します。
    - **DNS ドメイン**: - CONTOSO-DNS-US の DNS サフィックスを入力します。 例: Contoso-DNS-US.b5.internal.cloudapp.net
    - **マスター サーバーの IP アドレス**:「10.2.0.4」これは、- CONTOSO-DNS-US の IP アドレスを入力します。
6.  キーを押して **ENTER**, 、] をクリックし、 **OK**します。  以上で、Contoso-DNS-EU から Contoso-DNS-US の IP アドレスを解決できるようになります。
7.  同じ手順を繰り返し、次の値を使用して、Contoso-DNS-US 仮想マシン上の DNS サービスへの DNS フォワーダーを追加します。
    - **DNS ドメイン**: - CONTOSO-DNS-EU の DNS サフィックスを入力します。 
    - **マスター サーバーの IP アドレス**:「10.2.0.4」これは、- CONTOSO-DNS-EU の IP アドレスを入力します。

##仮想ネットワーク間での名前解決のテスト

これで、仮想ネットワークの間でのホスト名の解決をテストできます。 既定では、Ping はファイアウォールによってブロックされます。  nslookup を使用して、ピア ネットワーク内の DNS サーバー仮想マシン (FQDN を使用する必要があります) を解決できます。  


##次のステップ

このチュートリアルでは、VPN 接続を使用する仮想ネットワーク間での名前解決を構成する方法を説明しました。 このシリーズの他の 2 つの記事は次のような内容です。

- [2 つの Azure 仮想ネットワーク間の VPN 接続の構成][hdinsight-hbase-geo-replication-vnet]
- [HBase geo レプリケーションの構成][hdinsight-hbase-geo-replication]



[hdinsight-hbase-geo-replication]: hdinsight-hbase-geo-replication.md
[hdinsight-hbase-geo-replication-vnet]: hdinsight-hbase-geo-replication-configure-VNets.md
[powershell-install]: ../install-configure-powershell.md

[img-vnet-diagram]: ./media/hdinsight-hbase-geo-replication-configure-DNS/HDInsight.HBase.VPN.diagram.png 

