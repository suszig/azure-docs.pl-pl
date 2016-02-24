<properties 
    pageTitle="Azure トラフィック マネージャーによる Azure の Web アプリのトラフィックの制御" 
    description="ここでは、Azure の web アプリに関連する Azure トラフィック マネージャーの概要情報が紹介します。" 
    services="app-service\web" 
    documentationCenter="" 
    authors="cephalin" 
    writer="cephalin" 
    manager="wpickett" 
    editor="mollybos"/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="12/10/2015" 
    ms.author="cephalin"/>

# Azure トラフィック マネージャーによる Azure の Web アプリのトラフィックの制御

> [AZURE.NOTE] ここでは、Azure App Service Web Apps に関連する Microsoft Azure トラフィック マネージャーの概要情報が紹介します。 Azure トラフィック マネージャー自体の詳細については、この記事の最後にある関連情報を参照してください。

## はじめに
Azure トラフィック マネージャーを使用すると、Web クライアントからの要求を Azure App Service の Web アプリに振り分ける方法を制御できます。 Web アプリのエンドポイントが Azure トラフィック マネージャーのプロファイルに追加されると、Azure トラフィック マネージャーは Web アプリの状態 (実行中、停止、または削除済み) を追跡して、トラフィックを受信する必要のあるエンドポイントを決定できるようになります。

## 負荷分散方法
Azure トラフィック マネージャーは 3 つの異なる負荷分散方法を使用します。 これらには、Azure の web アプリに関連する次の一覧に記述されます。 

* **フェールオーバー**: さまざまなリージョンに web アプリの複製をした場合は、すべての web クライアント トラフィックを処理する 1 つの web アプリを構成するには、このメソッドを使用して、最初の web アプリが使用できなくなった場合に、トラフィックを処理する別のリージョンで別の web アプリを構成します。 
    
* **ラウンド ロビン**: さまざまなリージョンに web アプリの複製があれば、このメソッドを使用してさまざまなリージョンに web アプリ間でトラフィックを均等に分散することができます。 
    
* **パフォーマンス**: [パフォーマンス メソッドがクライアントへの最短の往復時間に基づいてトラフィックを分散します。 この方法は同じリージョン内または異なるリージョン内の Web アプリに使用できます。 

Azure Traffic Manager で負荷分散の詳細については、次を参照してください。 [Traffic Manager の負荷分散方法について](../traffic-manager/traffic-manager-load-balancing-methods.md)します。

##Web アプリとトラフィック マネージャーのプロファイル 
Web アプリのトラフィック制御を構成するには、Azure トラフィック マネージャーでプロファイルを作成し、前に説明している 3 つの負荷分散方法のいずれかをプロファイルで指定します。その後、トラフィックを制御するエンドポイント (この場合は Web アプリ) をプロファイルに追加します。 Web アプリの状態 (実行中、停止、または削除済み) は定期的にプロファイルに反映されて、その状態に応じて Azure トラフィック マネージャーはトラフィックを振り分けることができます。

Azure トラフィック マネージャーを Azure で使用する場合は、次の点に留意してください。

* 同じリージョンでの Web アプリのみの展開の場合、Web アプリはそのモードには関係なく、フェールオーバーとラウンド ロビンの機能を既に備えています。

* 同じリージョンでの Web アプリのデプロイで、Azure の別のクラウド サービスと連携させる場合、両方の種類のエンドポイントを組み合わせたハイブリッドのシナリオが可能です。

* リージョンごとに 1 つのみの Web アプリ エンドポイントをプロファイルで指定することもできます。 1 つのリージョンのエンドポイントとして Web アプリを選択すると、そのリージョン内の残りの Web アプリはそのプロファイルで選択できなくなります。

* Azure Traffic Manager プロファイルで指定した web アプリのエンドポイントが表示され、 **ドメイン名** セクション プロファイルでは、web アプリの構成] ページは構成できませんがあります。

* Web アプリをプロファイルに追加した後、 **サイトの URL** web のダッシュ ボードでアプリのポータル ページが表示されます、web アプリのカスタム ドメインの URL を 1 つを設定している場合。 それ以外の場合は、Traffic Manager のプロファイルの URL (`contoso.trafficmgr.com` など) が表示されます。 Web アプリとトラフィック マネージャーの URL の直接のドメイン名が [web アプリの構成ページに表示される、 **ドメイン名** セクションです。

* カスタム ドメイン名は予期したとおりに機能しますが、それらのドメイン名を Web アプリに追加するだけでなく、トラフィック マネージャーの URL を参照するように DNS マップを構成する必要もあります。 Azure web アプリ用のカスタム ドメインを設定する方法については、次を参照してください。 [Azure の web サイトのカスタム ドメイン名の構成](web-sites-custom-domain-name.md)します。

* 標準モードの Web アプリのみを Azure トラフィック マネージャーのプロファイルに追加できます。

## 次のステップ

概念と技術的概要についての Azure Traffic Manager は、次を参照してください。 [Traffic Manager の概要](../traffic-manager/traffic-manager-overview.md)します。 

Azure Traffic Manager で負荷分散の詳細については、次を参照してください。 [Traffic Manager の負荷分散方法について](../traffic-manager/traffic-manager-load-balancing-methods.md)します。

トラフィック マネージャーの使用方法 (Web アプリ関連) の詳細については、次のブログの投稿を参照してください: 
[Azure の Web サイト Azure Traffic Manager を使用して](http://blogs.msdn.com/b/waws/archive/2014/03/18/using-windows-azure-traffic-manager-with-waws.aspx) と [Azure Traffic Manager は Azure の Web サイトを統合できるようになりました](http://azure.microsoft.com/blog/2014/03/27/azure-traffic-manager-can-now-integrate-with-azure-web-sites/)します。
 

