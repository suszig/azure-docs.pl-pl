<properties
   pageTitle="ExpressRoute 回線の構成ワークフロー | Microsoft Azure"
   description="このページでは、ExpressRoute の回線とピアリングを構成するためのワークフローについて段階的に説明します。"
   documentationCenter="na"
   services="expressroute"
   authors="cherylmc"
   manager="carolz"
   editor="" />
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article" 
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/12/2015"
   ms.author="cherylmc"/>

# 回線のプロビジョニングと回線の状態の ExpressRoute ワークフロー
このページでは、サービス プロビジョニングと上位のルーティング構成ワークフローについて段階的に説明します。 

![](./media/expressroute-workflows/expressroute-circuit-workflow.png)

下の図とそれに対応する手順は、ExpressRoute 回線をエンドツーエンドでプロビジョニングするための作業を示しています。 

1. PowerShell を利用し、ExpressRoute 回線を構成します。 指示に従って、 [作成の ExpressRoute 回線](expressroute-howto-circuit-classic.md) 詳細については資料です。

2. サービス プロバイダーに接続を要求します。 このプロセスはさまざまです。 接続の要求方法に関する詳細については、接続プロバイダーにお問い合わせください。

3. PowerShell で ExpressRoute のプロビジョニング状態を確認することで、回線が正常にプロビジョニングされていることを確認します。 

4. ルーティング ドメインを構成します。 接続プロバイダーが代わりにレイヤー 3 を管理する場合、接続プロバイダーが回線のルーティングを構成します。 接続プロバイダーは、レイヤー 2 サービスのみを提供する場合は、」に記載のガイドラインに従ってルーティングを構成する必要があります、 [ルーティング要件を満たす](expressroute-routing.md) と [ルーティング構成](expressroute-howto-routing-classic.md) ページです。

    -  Azure プライベート ピアリングを有効にする - 仮想ネットワーク内にデプロイされている VM / クラウドに接続するには、このピアリングを有効にする必要があります。
    -  Azure パブリック ピアリングを有効にする - パブリック IP アドレスにホストされている Azure サービスに接続する場合、Azure パブリック ピアリングを有効にする必要があります。 Azure プライベート ピアリングの既定のルーティングを有効にする場合、これが Azure リソースにアクセスするための要件となります。
    -  Microsoft ピアリングを有効にする - Office 365 と CRM Online サービスにアクセスするにはこれを有効にする必要があります。 
    
    >[AZURE.IMPORTANT] Microsoft ピアリングを有効にする場合は、Azure AD にアクセスする Azure パブリック ピアリングを有効またことを確認します。 Microsoft に接続するには、インターネットに使用しているプロキシ/エッジとは別のプロキシ/エッジを使用する必要があります。 ExpressRoute とインターネットの両方に同じエッジを使用すると、ルーティングが非同期になり、ネットワークの接続が停止します。


    ![](./media/expressroute-workflows/expressroute-routing-workflow.png)

5. ExpressRoute 回線に仮想ネットワークをリンクする - 仮想ネットワークを ExpressRoute 回線にリンクできます。 指示に従って [Vnet にリンクする](expressroute-howto-linkvnets-classic.md) 、回線に接続します。 VNet は ExpressRoute 回線と同じ Azure サブスクリプションに配置するか、別のサブスクリプションに配置できます。


## ExpressRoute 回線のプロビジョニング状態

各 ExpressRoute 回線には 2 つの状態があります。

- サービス プロバイダーのプロビジョニング状態
- 状態

状態は Microsoft のプロビジョニングの状態を表します。 このプロパティは、次の状態のいずれかになります: *有効*, 、*Enabling*, 、または *無効化*します。 ExpressRoute 回線を使用するには「有効」状態になっている必要があります。

接続プロバイダーのプロビジョニング状態は接続プロバイダー側の状態を表します。 使用できる *NotProvisioned*, 、*プロビジョニング*, 、または *プロビジョニング済み*します。 ExpressRoute 回線を使用するには「プロビジョニング済み」状態になっている必要があります。

### ExpressRoute 回線の状態

このセクションでは、ExpressRoute 回線の状態を一覧にまとめています。

#### 作成時

PowerShell コマンドレットを実行し、ExpressRoute 回線を作成すると、直後に ExpressRoute 回線の状態が以下のようになります。

    ServiceProviderProvisioningState : NotProvisioned
    Status                           : Enabled


#### 接続プロバイダーが回線にプロビジョニングしているとき

サービス キーを接続プロバイダーに渡し、接続プロバイダーがプロビジョニングを開始すると、直後に ExpressRoute 回線の状態が以下のようになります。

    ServiceProviderProvisioningState : Provisioning
    Status                           : Enabled


#### 接続プロバイダーがプロビジョニングを完了したとき

接続プロバイダーがプロビジョニングを完了すると、直後に ExpressRoute 回線の状態が以下のようになります。

    ServiceProviderProvisioningState : Provisioned
    Status                           : Enabled

回線は「プロビジョニング済み」で「有効」の状態でなければ使用できません。 レイヤー 2 のプロバイダーを使用している場合、この状態にあるときにのみ、回線のルーティングを構成できます。

#### 最初に Microsoft 側でプロビジョニング解除が開始された場合

PowerShell コマンドレットを実行し、ExpressRoute 回線を削除すると、直後に ExpressRoute 回線の状態が以下のようになります。

    ServiceProviderProvisioningState : Provisioned
    Status                           : Disabling

ExpressRoute 回線のプロビジョニングを解除するには、接続プロバイダーに接触する必要があります。 **重要:** Microsoft は、回線のプロビジョニングを解除する PowerShell コマンドレットを実行するまで、回線の請求を継続します。

#### サービス プロバイダー側でプロビジョニング解除が開始された場合

最初にサービス プロバイダーに ExpressRoute 回線のプロビジョニング解除を要請した場合、サービス プロバイダーがプロビジョニング解除を完了すると、回線が以下の状態になります。


    ServiceProviderProvisioningState : NotProvisioned
    Status                           : Enabled

必要に応じて再度有効にしたり、PowerShell コマンドレットを実行して回線を削除したりできます。 **重要:** Microsoft は、回線のプロビジョニングを解除する PowerShell コマンドレットを実行するまで、回線の請求を継続します。


## ルーティング セッション構成の状態

BGP プロビジョニング状態からは、Microsoft エッジで BGP セッションが有効になっているかどうかがわかります。 ピアリングを使用するにはこの状態を有効にする必要があります。

特に Microsoft ピアリングの場合、BGP セッションの状態を確認することが重要です。 呼ばれる別の状態があるだけでなく、プロビジョニング状態 BGP *パブリック プレフィックスの状態をアドバタイズ*します。 アドバタイズされたパブリック プレフィックス状態がである必要があります *構成* 州、BGP セッションをアップして、エンド ツー エンドの機能へのルーティングの両方です。 

アドバタイズされたパブリック プレフィックス状態に設定されている場合、 *検証のために必要な* 状態にある場合、BGP セッションが有効になっていないと、アドバタイズされたプレフィックスは、ルーティングのレジストリのいずれかで AS 番号と一致しませんでした。 

>[AZURE.IMPORTANT] アドバタイズされたパブリック プレフィックスの状態がである場合 *手動検証* 状態にある場合にサポート チケットを開く必要があります [Microsoft サポート](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) に沿って提供の自律システム番号が関連付けられている IP アドレスを所有している証拠を提供するとします。


## 次のステップ

- ExpressRoute 接続を構成します。

    - [ExpressRoute 回線の作成](expressroute-howto-circuit-classic.md)
    - [ルーティングの構成](expressroute-howto-routing-classic.md)
    - [ExpressRoute 回線への VNet のリンク](expressroute-howto-linkvnet-classic.md)
