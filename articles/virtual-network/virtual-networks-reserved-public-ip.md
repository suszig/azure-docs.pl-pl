<properties 
   pageTitle="予約済み IP | Microsoft Azure"
   description="予約済み IP およびそれらを管理する方法について説明します。"
   services="virtual-network"
   documentationCenter="na"
   authors="telmosampaio"
   manager="carmonm"
   editor="tysonn" />
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="12/11/2015"
   ms.author="telmos" />

# 予約済み IP の概要
Azure での IP アドレスは、動的と予約済みという 2 つのカテゴリに分類されます。  Azure で管理されるパブリック IP アドレスは、既定では動的です。 これは、特定のクラウド サービス (VIP) に使用される IP アドレス、または VM やロール インスタンスへの直接アクセスに使用される IP アドレスが、リソースがシャット ダウンまたは割り当て解除された場合に変更される場合があるということです。

IP アドレスが変更されないようにするには、IP アドレスを予約します。 予約済み IP は VIP としてのみ使用できるため、リソースがシャット ダウンしたり割り当てが解除されたりした場合でも、クラウド サービスの IP アドレスは変わりません。 さらに、VIP として使用されている既存の動的 IP を、予約済み IP アドレスに変換できます。

## 予約済み IP が必要になる場合
- **Ip アドレスの予約はサブスクリプションでは、確実に行う**します。 どのような状況でもサブスクリプションから解放されない IP アドレスを予約する必要がある場合は、予約済みパブリック IP を使用します。  
- **クラウド サービスでも停止または割り当て解除された (Vm) を使用する、ip アドレスが必要な**です。 クラウド サービスの VM が停止したり割り当て解除された場合でも、変更されない IP アドレスによるサービスへのアクセスを可能にしたい場合です。
- **Azure の出力トラフィックが予測可能な IP アドレスを使用することを確認する**です。 オンプレミスのファイアウォールが、特定の IP アドレスからのトラフィックのみを許可するよう構成することができます。 IP を予約すると、発信元 IP アドレスがわかるため、IP の変更に合わせてファイアウォール ルールを更新する必要がなくなります。

## FAQ
1. 予約済み IP はすべての Azure サービスに使用できますか。   
  - 予約済み IP は、VIP を使用して公開される VM およびクラウド サービスのインスタンス ロールに対してのみ使用できます。
1. 予約済み IP は、いくつ使用できますか。   
  - 現時点では、すべての Azure サブスクリプションで 20 個の予約済み IP を使用できるようになっています。 しかし、予約済み IP の追加を要求することができます。 参照してください、 [サブスクリプションとサービス制限](../azure-subscription-service-limits/) 詳細については、ページです。
1. 予約済み IP に料金はかかりますか。 
  - 参照してください [予約済み IP アドレスの料金詳細](http://go.microsoft.com/fwlink/?LinkID=398482) の価格情報を設定します。
1. どうやって IP アドレスを予約するのですか。 
  - PowerShell を使用する、または [Azure Management REST API](https://msdn.microsoft.com/library/azure/dn722420.aspx) を特定の地域から予約済み IP を要求します。 Azure はそのリージョンの IP アドレスを予約し、サブスクリプションに関連付けます。 その後、そのリージョンの予約済み IP を使用できます。 管理ポータルを使用して IP アドレスを予約することはできません。
1. 予約済み IP を VNet ベースのアフィニティ グループで使用できますか。  
  - 予約済み IP はリージョン VNet に対してのみサポートされます。 アフィニティ グループに関連付けられている VNet に対してはサポートされません。 VNet と地域またはアフィニティ グループの関連付けの詳細については、次を参照してください。 [地域 Vnet およびアフィニティ グループについて](virtual-networks-migrate-to-regional-vnet.md)します。 

## 予約済み VIP の管理方法

予約済み IP アドレスは、使用する前にサブスクリプションに追加する必要があります。 使用できるパブリック IP アドレスのプールから予約済み IP を作成する、 *中央アメリカ* 場所は、次の PowerShell コマンドを実行します。

    New-AzureReservedIP –ReservedIPName MyReservedIP –Location "Central US"

ただし、予約する IP を指定できないのでご注意ください。 お客様のサブスクリプションで予約されたどのような IP アドレスを表示する次の PowerShell コマンドを実行しの値に注意してください *ReservedIPName* と *アドレス*:

    Get-AzureReservedIP

    ReservedIPName       : MyReservedIP
    Address              : 23.101.114.211
    Id                   : d73be9dd-db12-4b5e-98c8-bc62e7c42041
    Label                : 
    Location             : Central US
    State                : Created
    InUse                : False
    ServiceName          : 
    DeploymentName       : 
    OperationDescription : Get-AzureReservedIP
    OperationId          : 55e4f245-82e4-9c66-9bd8-273e815ce30a
    OperationStatus      : Succeeded

IP アドレスが予約されると、サブスクリプションとの関連付けは、サブスクリプションが削除されるまで維持されます。 上記の予約済み IP を削除するには、次の PowerShell コマンドを実行します。

    Remove-AzureReservedIP -ReservedIPName "MyReservedIP"

## 予約済み IP を新しいクラウド サービスに関連付ける方法
次のスクリプトによって新しい予約済み IP が作成され、という名前の新しいクラウド サービスに関連付けられます *TestService*します。

    New-AzureReservedIP –ReservedIPName MyReservedIP –Location "Central US"
    $image = Get-AzureVMImage|?{$_.ImageName -like "*RightImage-Windows-2012R2-x64*"}
    New-AzureVMConfig -Name TestVM -InstanceSize Small -ImageName $image.ImageName `
    | Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! `
    | New-AzureVM -ServiceName TestService -ReservedIPName MyReservedIP -Location "Central US"

>[AZURE.NOTE] クラウド サービスで使用する予約済み IP を作成するときにもする必要があります、VM を使用して参照 *VIP: & lt; ポート番号 >* 着信通信します。 IP を予約しても、VM に直接接続できないためです。 予約済み IP は、VM がデプロイされたクラウド サービスに割り当てられます。 IP を使用して VM に直接接続するには、インスタンスレベル パブリック IP を構成する必要があります。 インスタンスレベル パブリック IP とは、VM に直接割り当てられているパブリック IP (ILPIP と呼ばれる) の一種です。 この IP は予約できません。 参照してください [インスタンス レベル パブリック IP (ILPIP)](../virtual-networks-instance-level-public-ip) の詳細。

## 予約済み IP を実行中のデプロイから削除する方法
上記のスクリプトで作成した新しいサービスに追加された予約済み IP を削除するには、次の PowerShell コマンドを実行します。

    Remove-AzureReservedIPAssociation -ReservedIPName MyReservedIP -ServiceName TestService

>[AZURE.NOTE] 実行中のデプロイから予約済み IP を削除しても、サブスクリプションから予約は削除されません。 単に IP が、サブスクリプション内の他のリソースで使用できるように解放されるだけだからです。

## 予約済み IP を実行中のデプロイに関連付ける方法
次のスクリプトという名前の新しいクラウド サービスを作成する *TestService2* という名前の新しい VM を含む *TestVM2*, 、という既存の予約済み IP を関連付けます *MyReservedIP* クラウド サービスにします。

    $image = Get-AzureVMImage|?{$_.ImageName -like "*RightImage-Windows-2012R2-x64*"}
    New-AzureVMConfig -Name TestVM2 -InstanceSize Small -ImageName $image.ImageName `
    | Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! `
    | New-AzureVM -ServiceName TestService2 -Location "Central US"
    Set-AzureReservedIPAssociation -ReservedIPName MyReservedIP -ServiceName TestService2

## サービス構成ファイルを使用して、クラウド サービスに予約済み IP を関連付ける方法
サービスの構成 (CSCFG) ファイルを使用すると、クラウド サービスに予約済み IP を関連付けることもできます。 次のサンプル xml という予約済み VIP を使用するクラウド サービスを構成する方法を示しています *MyReservedIP*:。 
    
    <?xml version="1.0" encoding="utf-8"?>
    <ServiceConfiguration serviceName="ReservedIPSample" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration" osFamily="4" osVersion="*" schemaVersion="2014-01.2.3">
      <Role name="WebRole1">
        <Instances count="1" />
        <ConfigurationSettings>
          <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="UseDevelopmentStorage=true" />
        </ConfigurationSettings>
      </Role>
      <NetworkConfiguration>
        <AddressAssignments>
          <ReservedIPs>
           <ReservedIP name="MyReservedIP"/>
          </ReservedIPs>
        </AddressAssignments>
      </NetworkConfiguration>
    </ServiceConfiguration>

## 次のステップ

- について学習 [予約済みプライベート IP アドレスの](../virtual-networks-reserved-private-ip)です。

- について学習 [インスタンス レベル ・ パブリック IP (ILPIP) アドレス](../virtual-networks-instance-level-public-ip)です。

- チェック、 [予約済み IP REST Api](https://msdn.microsoft.com/library/azure/dn722420.aspx)します。

