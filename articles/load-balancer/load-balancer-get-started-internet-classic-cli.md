<properties 
   pageTitle="Azure CLI を使用したインターネットに接続するクラシック デプロイメント モデルのロード バランサーの作成の開始 | Microsoft Azure"
   description="Azure CLI を使用し、インターネットに接続するクラシック デプロイメント モデルのロード バランサーを作成する方法について説明します"
   services="load-balancer"
   documentationCenter="na"
   authors="joaoma"
   manager="carolz"
   editor=""
   tags="azure-service-management"
/>
<tags  
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="11/06/2015"
   ms.author="joaoma" />


# Azure CLI を使用したインターネットに接続するロード バランサー (クラシック) の作成の開始

[AZURE.INCLUDE [load-balancer-get-started-internet-classic-selectors-include.md](../../includes/load-balancer-get-started-internet-classic-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)] この記事では、従来のデプロイ モデルについて説明します。 こともできます [インターネットへの Azure リソース マネージャーを使用してロード バランサーを作成する方法について](load-balancer-get-started-internet-arm-ps.md)します。

[AZURE.INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]


## CLI を使用したインターネットに接続するロード バランサーの作成手順

このガイドでは、前述のシナリオに基づいてインターネット ロード バランサーを作成する方法を説明します。

1. Azure CLI を初めて使用する場合は、次を参照してください。 [のインストールと Azure CLI の構成](xplat-cli.md) Azure アカウントとサブスクリプションを選択する時点までの指示に従います。

2. 次に示すように、**azure config mode** コマンドを実行して、以下に示すようにクラシック モードに切り替えます。

        azure config mode asm

    予想される出力:

        info:    New mode is asm



## エンドポイントとロード バランサー セットの作成

このシナリオでは、仮想マシン "web1" と "web2" が作成済みであることを前提としています。 
このガイドでは、ポート 80 をパブリック ポートとして、ポート 80 をローカル ポートとして、ロード バランサー セットを作成します。 ポート 80 には、ロード バランサー セット "lbset" というプローブ ポートも構成されます。


### 手順 1.

最初のエンドポイントを作成し、ロード バランサーを使用して設定 `azure ネットワーク、vm エンドポイントを作成` 仮想マシン"web1"にします。

    azure vm endpoint create web1 80 -k 80 -o tcp -t 80 -b lbset 

使用されるパラメーター:

**-k** -仮想マシンのローカル ポート<br>
**-o** -プロトコル<BR>
**-t** -プローブ ポート<BR>
**-b** -ロード バランサーの名前<BR>

## 手順 2.

ロード バランサー セットに 2 番目の仮想マシン "web2" を追加します。

    azure vm endpoint create web2 80 -k 80 -o tcp -t 80 -b lbset

## 手順 3.

ロード バランサーの構成を使用することを確認 `azure vm のショー` します。

    azure vm show web1

次のように出力されます。

    data:    DNSName "contoso.cloudapp.net"
    data:    Location "East US"
    data:    VMName "web1"
    data:    IPAddress "10.0.0.5"
    data:    InstanceStatus "ReadyRole"
    data:    InstanceSize "Standard_D1"
    data:    Image "a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-2015
    6-en.us-127GB.vhd"
    data:    OSDisk hostCaching "ReadWrite"
    data:    OSDisk name "joaoma-1-web1-0-201509251804250879"
    data:    OSDisk mediaLink "https://XXXXXXXXXXXXXXX.blob.core.windows.
    /vhds/joaomatest-web1-2015-09-25.vhd"
    data:    OSDisk sourceImageName "a699494373c04fc0bc8f2bb1389d6106__Windows-Se
    r-2012-R2-20150916-en.us-127GB.vhd"
    data:    OSDisk operatingSystem "Windows"
    data:    OSDisk iOType "Standard"
    data:    ReservedIPName ""
    data:    VirtualIPAddresses 0 address "XXXXXXXXXXXXXXXX"
    data:    VirtualIPAddresses 0 name "XXXXXXXXXXXXXXXXXXXX"
    data:    VirtualIPAddresses 0 isDnsProgrammed true
    data:    Network Endpoints 0 loadBalancedEndpointSetName "lbset"
    data:    Network Endpoints 0 localPort 80
    data:    Network Endpoints 0 name "tcp-80-80"
    data:    Network Endpoints 0 port 80
    data:    Network Endpoints 0 loadBalancerProbe port 80
    data:    Network Endpoints 0 loadBalancerProbe protocol "tcp"
    data:    Network Endpoints 0 loadBalancerProbe intervalInSeconds 15
    data:    Network Endpoints 0 loadBalancerProbe timeoutInSeconds 31
    data:    Network Endpoints 0 protocol "tcp"
    data:    Network Endpoints 0 virtualIPAddress "XXXXXXXXXXXX"
    data:    Network Endpoints 0 enableDirectServerReturn false
    data:    Network Endpoints 1 localPort 5986
    data:    Network Endpoints 1 name "PowerShell"
    data:    Network Endpoints 1 port 5986
    data:    Network Endpoints 1 protocol "tcp"
    data:    Network Endpoints 1 virtualIPAddress "XXXXXXXXXXXX"
    data:    Network Endpoints 1 enableDirectServerReturn false
    data:    Network Endpoints 2 localPort 3389
    data:    Network Endpoints 2 name "Remote Desktop"
    data:    Network Endpoints 2 port 58081
    info:    vm show command OK

## 仮想マシン用のリモート デスクトップ エンドポイントの作成

転送用のネットワーク トラフィックのリモート デスクトップのエンドポイントを作成するには、パブリック ポートを使用して、特定のバーチャル マシンのローカル ポートから `、azure の vm エンドポイントを作成`します。

    azure vm endpoint create web1 54580 -k 3389 

## ロード バランサーからの仮想マシンの削除

ロード バランサー セットに関連付けられているエンドポイントを仮想マシンから削除する必要があります。 エンドポイントを削除すると、その仮想マシンはそのロード バランサー セットには属さなくなります。

 上記の例を使用して、削除する仮想マシン"web1"に対して作成されたエンドポイント ロード バランサー"lbset"コマンドを使用して `、azure の vm エンドポイントを削除`します。

    azure vm endpoint delete web1 tcp-80-80

>[AZURE.NOTE] コマンドを使用してエンドポイントを管理する他のオプションを表示する `azure vm endpoint--ヘルプ`


## 次のステップ

[内部ロード バランサーの構成の開始します。](load-balancer-internal-getstarted.md)

[ロード バランサー分散モードを構成します。](load-balancer-distribution-mode.md)

[ロード バランサーのアイドル TCP タイムアウト設定を構成します。](load-balancer-tcp-idle-timeout.md)






