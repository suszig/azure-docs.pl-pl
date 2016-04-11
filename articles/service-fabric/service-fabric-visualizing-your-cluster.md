<properties
   pageTitle="Service Fabric Explorer を使用したクラスターの視覚化 | Microsoft Azure"
   description="Service Fabric Explorer は、Microsoft Azure Service Fabric クラスター内のクラウド アプリケーションとノードを検査および管理するための便利な GUI ツールです。"
   services="service-fabric"
   documentationCenter=".net"
   authors="jessebenson"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/30/2015"
   ms.author="jesseb"/>

# Service Fabric Explorer を使用したクラスターの視覚化

Service Fabric Explorer は、Service Fabric クラスター内のアプリケーションとノードを検査および管理するための Web ベースのツールです。 Service Fabric Explorer は、Service Fabric クラスター内で直接ホストされるので、ローカル クラスターが実行されているかどうかに関係なく、いつでも使用できます。

## Service Fabric Explorer への接続

手順を実行している場合 [開発環境を準備](service-fabric-get-started.md), 、http://localhost:19080/エクスプ ローラーに移動して、ローカル クラスターで Service Fabric エクスプ ローラーを起動することができます。

>[AZURE.NOTE] を使用するインターネット Explorer(IE) Service Fabric エクスプ ローラーで、リモート クラスターを管理する場合は、IE のいくつかの設定を構成する必要があります。 移動して **ツール]、[互換表示設定** ボックスをオフに **イントラネット サイトを互換表示で表示** 正しく読み込まれるすべての情報を確認します。

## Service Fabric Explorer のレイアウトについて

左側のツリーを使用して、Service Fabric Explorer 内を移動できます。 ツリーのルートでは、クラスター ダッシュボードにクラスターの概要 (アプリケーションとノードの正常性の概要など) が表示されます。

![Service Fabric Explorer のクラスター ダッシュボード][sfx-cluster-dashboard]

### クラスター マップ

Service Fabric クラスターのノードは障害ドメインの 2 次元グリッドにわたり配置され、ハードウェアに障害が発生したり、アプリケーションをアップグレードしたりするとき、アプリケーションの可用性を維持できるようにドメインをアップグレードします。 クラスター マップを利用し、現在のクラスターの配置を表示できます。

![Service Fabric Explorer のクラスター マップ][sfx-cluster-map]

### アプリケーションとサービスの表示

クラスターには、2 つのサブツリーが含まれています。1 つはアプリケーション、もう 1 つはノードのサブツリーです。

アプリケーション ビューでは、Service Fabric の論理階層 (アプリケーション、サービス、パーティション、レプリカ) 内を移動できます。

アプリケーションの下の例では **MyApp** 2 つのサービスの構成される **MyStatefulService** と **WebService**します。  **MyStatefulService** ステートフルでは、1 つのプライマリとセカンダリ レプリカが 2 つのパーティションが含まれています。 これに対して、WebSvcService はステートレスであり、インスタンスが 1 つだけ含まれています。

![Service Fabric Explorer のアプリケーション ビュー][sfx-application-tree]

ツリーの各レベルでは、メイン ウィンドウにその項目の関連情報が表示されます。 たとえば、特定のサービスの正常性状態とバージョンを確認できます。

![Service Fabric Explorer のメイン ウィンドウ][sfx-service-essentials]

### クラスターのノードの表示

ノード ビューには、クラスターの物理的なレイアウトが表示されます。 特定のノードについて、そのノードでコードがデプロイされているアプリケーション、正確に言うと、そこで現在実行されているレプリカを調べることができます。

## Service Fabric Explorer を使用した操作の実行

Service Fabric Explorer では、クラスター内のノード、アプリケーション、サービスに対する操作を簡単に呼び出すことができます。

たとえば、アプリケーション インスタンスを削除するには、左側のツリーでアプリケーションを選択し、[操作]、[アプリケーションの削除] の順に選択します。

![Service Fabric Explorer でのアプリケーションの削除][sfx-delete-application]

多くの操作は破壊的であるため、操作を実行する前に確認を求められます。

>[AZURE.NOTE] PowerShell またはオートメーションを有効にすると、REST API を使用して Service Fabric エクスプ ローラーを使用して実行できるすべてのアクションを実行することもできます。



## リモート Service Fabric クラスターへの接続

Service Fabric Explorer は Web ベースであり、クラスター内で実行されるので、クラスターのエンドポイントがわかっており、エンドポイントにアクセスするための十分なアクセス許可があれば、どのブラウザーからでもアクセスできます。

### リモート クラスターの Service Fabric Explorer エンドポイントの検出

特定のクラスターの Service Fabric Explorer に到達するために、ブラウザーを次の場所にポイントします。

http://&lt;your-cluster-endpoint&gt;:19080/Explorer

完全な URL は Azure ポータルの [Cluster Essentials] ウィンドウにもあります。

### セキュリティで保護されたクラスターへの接続

Service Fabric クラスターに接続するために、クライアントに証明書の提示を要求することによって、Service Fabric クラスターへのアクセスを制御できます。

セキュリティで保護されたクラスターで Service Fabric Explorer に接続しようとすると、アクセスを取得するために、ブラウザーから証明書の提示を求められます。

## 次のステップ

- [Testability の概要](service-fabric-testability-overview.md)します。
- [Visual Studio で Service Fabric アプリケーションの管理](service-fabric-manage-application-in-visual-studio.md)します。
- [PowerShell を使用した Service Fabric アプリケーションのデプロイメント](service-fabric-deploy-remove-applications.md)

<!--Image references-->
[sfx-cluster-dashboard]: ./media/service-fabric-visualizing-your-cluster/SfxClusterDashboard.png
[sfx-cluster-map]: ./media/service-fabric-visualizing-your-cluster/SfxClusterMap.png
[sfx-application-tree]: ./media/service-fabric-visualizing-your-cluster/SfxApplicationTree.png
[sfx-service-essentials]: ./media/service-fabric-visualizing-your-cluster/SfxServiceEssentials.png
[sfx-delete-application]: ./media/service-fabric-visualizing-your-cluster/SfxDeleteApplication.png


