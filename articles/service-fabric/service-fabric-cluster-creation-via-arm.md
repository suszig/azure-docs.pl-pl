<properties
   pageTitle="ARM テンプレートを使用した Service Fabric クラスターのセットアップ | Microsoft Azure"
   description="ARM テンプレートを使用した Service Fabric クラスターのセットアップ。"
   services="service-fabric"
   documentationCenter=".net"
   authors="ChackDan"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="11/19/2015"
   ms.author="chackdan"/>

# ARM テンプレートを使用した Service Fabric クラスターのセットアップ

このページは、ARM テンプレートを使用して Service Fabric クラスターをセットアップする際に役立ちます。 サブスクリプションに、このクラスターを構成する IaaS VM をデプロイできるだけのコア数が割り当てられていることが前提となります。

## 前提条件

- セキュリティで保護されたクラスターを設定する場合は、X 509 をアップロードすることを確認、キー コンテナーに証明書。 ソースの資格情報コンテナーの URL、証明書の URL と証明書の拇印が必要です。
-  参照してください [サービス ファブリック クラスター セキュリティ](service-fabric-cluster-security.md) する方法の詳細についてです。

## サンプル ARM テンプレートの入手

1. ARM テンプレートのサンプルについては、「 [github では Azure クイック スタート テンプレート ギャラリー ](https://github.com/Azure/azure-quickstart-templates)します。 Service fabric のすべてのテンプレートが、名前"service fabric.."で開始します。 . 「ファブリック」のリポジトリを検索するか、同様のサンプル テンプレートのセットまで下へスクロールします。
2. 探しているテンプレートをすばやく見つけることができるように、テンプレートには次のように名前が付けられています。
    1. [service-fabric-unsecure-cluster-5-node-1-nodetype](http://go.microsoft.com/fwlink/?LinkId=716923) を 5 つのノードの 1 つのノードの安全でないクラスタ テンプレートを示します。 
    3. [service-fabric-secure-cluster-5-node-1-nodetype-wad](http://go.microsoft.com/fwlink/?LinkID=716924) を有効になっている WAD は、5 ノード 1 つのノードのセキュリティで保護されたクラスター テンプレートを示します。 
    4. [service-fabric-secure-cluster-10-node-2-nodetype-wad](http://go.microsoft.com/fwlink/?LinkId=716925) を有効になっている WAD は、10 ノードの 2 ノードのセキュリティで保護されたクラスター テンプレートを示します。 
    

## カスタム ARM テンプレートの作成

2. ここでどちらかを選択します。 
    1. サンプル テンプレートを取得する [github では Azure クイック スタート テンプレート ギャラリー ](https://github.com/Azure/azure-quickstart-templates) し、変更することです。
    2. Azure ポータルにログインし、Service Fabric ポータル ページを使用して、カスタマイズするテンプレートを生成します。 このプロセスを次に示します。
3. Azure ポータルにログオン [http://aka.ms/servicefabricportal](http://aka.ms/servicefabricportal)します。
2. 」の説明に従って、クラスターの作成のプロセスに従って進みます [ポータルを使用して Service Fabric クラスターを作成する](service-fabric-cluster-creation-via-portal.md) , にはクリックしないで ***作成**, 、代わりの概要」に移動し、テンプレートをダウンロードします。

 ![DownloadTemplate][DownloadTemplate]

## Azure PS を使用した ARM テンプレートの Azure へのデプロイ

参照してください [PS を使用して展開する ARM テンプレート ](resource-group-template-deploy.md) PowerShell を使用してテンプレートをデプロイする方法の詳細なガイダンスについてです。

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
##次のステップ
- [Service Fabric Cluster security](service-fabric-cluster-security.md) 
- [Managing your Service Fabric applications in Visual Studio](service-fabric-manage-application-in-visual-studio.md).
- [Service Fabric Health model introduction](service-fabric-health-introduction.md)

<!--Image references-->
[DownloadTemplate]: ./media/service-fabric-cluster-creation-via-arm/DownloadTemplate.png


