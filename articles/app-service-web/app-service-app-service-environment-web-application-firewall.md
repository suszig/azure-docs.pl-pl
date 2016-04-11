<properties 
    pageTitle="App Service 環境の Web アプリケーション ファイアウォール (WAF) を構成する" 
    description="App Service 環境の前に Web アプリケーション ファイアウォールを構成する方法について説明します。" 
    services="app-service\web" 
    documentationCenter="" 
    authors="naziml" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/15/2015" 
    ms.author="naziml"/>    

# App Service 環境の Web アプリケーション ファイアウォール (WAF) を構成する

## 概要 ##
などの web アプリケーション ファイアウォール、 [Barracuda WAF for Azure](https://www.barracuda.com/programs/azure) に用意されている、 [Azure Marketplace](http://azure.microsoft.com/marketplace/partners/barracudanetworks/waf-byol/) SQL 注入、クロスサイト スクリプティング、マルウェアのアップロードをブロックする着信 web トラフィックを調べることによって、web アプリケーション、アプリケーション DDoS などその他の攻撃を保護します。 さらに、データ損失防止 (DLP) のためにバックエンド Web サーバーからの応答を検査します。 App Service 環境が提供する分離と追加スケーリングと組み合わせることで、悪意のある要求と大量のトラフィックに対処する必要がある、ビジネスに不可欠な Web アプリケーションをホストする理想的な環境が用意されます。

+[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## セットアップ ##
このドキュメントでは、複数の負荷分散されたBarracuda WAF インスタンスの背後に App Service 環境を構成して、WAF からのトラフィックのみが App Service 環境に到着でき、DMZ からアクセスできないようにします。 さらに、Azure Traffic Manager を Barracuda WAF インスタンスの前に配置して、Azure のデータセンターとリージョン全体で負荷が分散されるようにします。 このセットアップの高レベルの図は次のようになります。

![アーキテクチャ][Architecture] 

## App Service 環境の構成 ##
App Service 環境を構成するのを参照してください [ドキュメント](app-service-web-how-to-create-an-app-service-environment.md) サブジェクトにします。 App Service 環境を作成したらを作成できます [Web Apps](app-service-web-overview.md), 、[API Apps](app-service-api-apps-why-best-platform.md) と [Mobile Apps](app-service-mobile-value-prop-preview.md) が確実に保護する次のセクションで構成する WAF の背後にあるこの環境にします。

## Barracuda WAF クラウド サービスを構成する ##
Barracuda には、 [詳細な記事](https://techlib.barracuda.com/WAF/AzureDeploy) その WAF を Azure の仮想マシンに展開する方法です。 ただし、これらの手順に従うときは、冗長性を持たせ、単一障害点の発生を防ぐために、少なくとも 2 つの WAF インスタンスの VM を同じクラウド サービスにデプロイします。

### クラウド サービスへのエンドポイントの追加 ###
2 つ用意した以上の WAF VM インスタンスをクラウド サービスとを使用できます、 [Azure ポータル](https://portal.azure.com) 次の図に示すように、アプリケーションで使用される HTTP および HTTPS のエンドポイントを追加します。

![エンドポイントを構成する][ConfigureEndpoint]

アプリケーションがその他のエンドポイントを使用する場合は、それらもこの一覧に追加してください。 

### 管理ポータルを使用した Barracuda WAF の構成 ###
Barracuda WAF は、管理ポータルによる構成で TCP ポート 8000 を使用します。 WAF VM の複数のインスタンスがあるため、ここに示す手順を 各 VM インスタンスに対して繰り返す必要があります。 


> 注: WAF 構成が完了したら、WAF を安全に保つために、すべての WAF VM から TCP/8000 エンドポイントを削除してください。

次の図に示すように、管理エンドポイントを追加して、Barracuda WAF を構成します。

![管理エンドポイントを追加する][AddManagementEndpoint]
 
ブラウザーを使用して、クラウド サービスの管理エンドポイントを参照します。 クラウド サービスは test.cloudapp.net、http://test.cloudapp.net:8000 を参照してそのエンドポイントにアクセスします。 次のようなログイン ページが表示されます。WAF VM のセットアップ フェーズで指定した資格情報を使用してログインできます。

![管理ログイン ページ][ManagementLoginPage]

ログインすると、次の図のような、WAF 保護に関する基本的な統計情報を示すダッシュボードが表示されます。

![管理ダッシュボード][ManagementDashboard]

[サービス] タブをクリックして、保護対象のサービスの WAF を構成できます。 Barracuda WAF の構成の詳細については役に立ちます [ドキュメント](https://techlib.barracuda.com/waf/getstarted1)します。 次の例では、HTTP と HTTPS でトラフィックを提供する Azure Web アプリが構成されています。

![管理ダッシュボードでサービスを追加する][ManagementAddServices]

> 注: App Service 環境内でアプリケーションがどのように構成され、どのような機能が使用されているかに応じて、トラフィックを 80 と 443 以外の TCP ポートに転送する必要があります (例: Web アプリの IP SSL を設定している場合)。 App Service 環境で使用されるネットワーク ポートの一覧を参照してください [着信トラフィックの制御ドキュメント](app-service-app-service-environment-control-inbound-traffic.md) ネットワーク ポートのセクションです。

## Microsoft Azure Traffic Manager の構成 (省略可能) ##
ロードする、アプリケーションが複数の地域で使用できる場合は背後でそれらバランスを取る [Azure Traffic Manager](traffic-manager.md)します。 操作を追加できるようにしたうえでエンドポイント、 [Azure クラシック ポータル](https://manage.azure.com) 次の図に示すように、Traffic Manager プロファイルで、WAF のクラウド サービス名を使用します。 

![Traffic Manager のエンドポイント][TrafficManagerEndpoint]

アプリケーションが認証を必要とする場合は、Traffic Manager がアプリケーションの使用可能性をチェックする ping を実行するための、認証を必要としないリソースがあることを確認します。 構成] セクションの下の URL を構成する、 [Azure クラシック ポータル](https://manage.azure.com) 次のようにします。

![Traffic Manager を構成する][ConfigureTrafficManager]

Traffic Manager の ping を WAF からアプリケーションに転送するには、次の例に示すように、Barracuda WAF に Web サイト変換を設定して、トラフィックをアプリケーションに転送します。

![Web サイトの変換][WebsiteTranslations]

## App Service 環境へのトラフィックをネットワーク リソース グループを使用して保護する##
次の [着信トラフィックの制御ドキュメント](app-service-app-service-environment-control-inbound-traffic.md) App Service 環境内に、クラウド サービスの VIP アドレスを使用してのみ、WAF からのトラフィックの制限の詳細。 このタスクを TCP ポート 80 に対して実行するサンプル Powershell コマンドを次に示します。


    Get-AzureNetworkSecurityGroup -Name "RestrictWestUSAppAccess" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP Barracuda" -Type Inbound -Priority 201 -Action Allow -SourceAddressPrefix '191.0.0.1'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP

SourceAddressPrefix を、WAF のクラウド サービスの仮想 IP アドレス (VIP) に置き換えます。

> 注: クラウド サービスの VIP は、クラウド サービスを削除して再作成すると変更されます。 これを行った場合は、ネットワーク リソース グループ内の IP アドレスを必ず更新してください。 
 
<!-- IMAGES -->
[Architecture]: ./media/app-service-app-service-environment-web-application-firewall/Architecture.png
[ConfigureEndpoint]: ./media/app-service-app-service-environment-web-application-firewall/ConfigureEndpoint.png
[AddManagementEndpoint]: ./media/app-service-app-service-environment-web-application-firewall/AddManagementEndpoint.png
[ManagementAddServices]: ./media/app-service-app-service-environment-web-application-firewall/ManagementAddServices.png
[ManagementDashboard]: ./media/app-service-app-service-environment-web-application-firewall/ManagementDashboard.png
[ManagementLoginPage]: ./media/app-service-app-service-environment-web-application-firewall/ManagementLoginPage.png
[TrafficManagerEndpoint]: ./media/app-service-app-service-environment-web-application-firewall/TrafficManagerEndpoint.png
[ConfigureTrafficManager]: ./media/app-service-app-service-environment-web-application-firewall/ConfigureTrafficManager.png
[WebsiteTranslations]: ./media/app-service-app-service-environment-web-application-firewall/WebsiteTranslations.png

