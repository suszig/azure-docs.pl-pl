<properties 
    pageTitle="Azure RemoteApp で QuickBooks をデプロイする | Microsoft Azure" 
    description="Azure RemoteApp で QuickBooks を共有する方法について説明します。" 
    services="remoteapp" 
    documentationCenter="" 
    authors="ericorman" 
    manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/01/2015" 
    ms.author="elizapo" />




# Azure RemoteApp で QuickBooks をデプロイする方法

次の情報を使用して、QuickBooks を Azure RemoteApp のアプリとして共有します。


ハイブリッドまたはクラウド コレクションで、QuickBooks 2015 Enterprise を Azure RemoteApp と共有できます。 会社のファイルは、Azure RemoteApp サーバーとは別の QuickBooks データベース サーバーを実行している VM に格納する必要があります。 会社のファイルは Azure RemoteApp イメージに格納しないでください。データの損失が発生します。 標準の Windows ネットワークキング経由でアクセス可能な QuickBooks データベース サーバーとの外部共有で QuickBooks ファイルをホストできるのは、QuickBooks Enterprise のみです。
> [AZURE.IMPORTANT] 会社のファイルをホストしている QuickBooks データベース サーバーは、Azure RemoteApp コレクションと同じ VNET 内の別の VM に配置する必要があります。  

## QuickBooks のデプロイ手順

1. Azure VM を作成し、QuickBooks と QuickBooks データベース サーバーをインストールし、会社のファイルを Azure VM に格納します。 ファイアウォールの規則を適切に構成します。
2. QuickBooks にインストール、 [カスタム イメージ](remoteapp-imageoptions.md) を作成し、 [Azure RemoteApp コレクション](remoteapp-collections.md), 、クラウドまたはハイブリッド、企業のファイルで、QuickBooks のデータベース サーバーをホストしている VM が存在する正確な同じ VNET 内のいずれかです。
3.  [発行](remoteapp-publish.md) QuickBooks アプリケーションをユーザーに
4.  Azure RemoteApp がホストする QuickBooks クライアントを起動し、標準の Windows ネットワークキングを使用して QuickBooks データベース サーバーをホストするサーバーに移動し、会社のファイルを開きます。

## ドキュメントの参照

- QuickBooks [でサポートされる構成](http://enterprisesuite.intuit.com/products/enterprise-solutions/technical/#top)
- QuickBooks [展開オプション](http://enterprisesuite.intuit.com/everythingenterprise/launchpad/new-user/)

Ignite プレゼンもご覧いただけます [の基礎の Microsoft Azure RemoteApp の管理](https://channel9.msdn.com/Events/Ignite/2015/BRK3868) -1時 02分: 45 QuickBooks 部分を取得するには、高速順方向です。

## デプロイメント アーキテクチャ

![QuickBooks + Azure RemoteApp のデプロイメント](./media/remoteapp-quickbooks/ra-quickbooks.png)




