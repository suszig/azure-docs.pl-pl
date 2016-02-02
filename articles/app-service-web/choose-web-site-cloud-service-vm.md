<properties
    pageTitle="Azure App Service、Cloud Services、および Virtual Machines の比較"
    description="Web アプリケーションをホストするにあたり、どのようなときにどのサービス (Azure App Service、Cloud Services、Virtual Machines) を使用するかについて説明します。"
    services="app-service\web, virtual-machines, cloud-services"
    documentationCenter=""
    authors="tdykstra"
    manager="wpickett"
    editor="jimbe"/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article" 
    ms.date="11/19/2015"
    ms.author="tdykstra"/>


# Azure App Service、Cloud Services、および Virtual Machines の比較

## 概要

Azure web サイトをホストするいくつかの方法を提供しています: [Azure App Service の][], 、[クラウド サービスの][], 、および [仮想マシンの][]します。 それぞれの選択肢についてこの記事で理解を深め、実際の Web アプリケーションに適した方法をお選びください。

Azure App Service は、ほとんどの Web アプリに適しています。 デプロイメントと管理機能がそのプラットフォームに統合され、トラフィックの負荷に応じてサイトのスケールを機敏に調整できるほか、組み込みの負荷分散機能と Traffic Manager によって高い可用性が得られます。 既存のサイトを使用して簡単に Azure App Service に移動する、 [オンライン移行ツール](https://www.migratetoazure.net/), で Web アプリケーション ギャラリーからオープン ソース アプリケーションを使用するかフレームワークと好みのツールを使用して新しいサイトを作成します。  [Web ジョブの][] 機能により、簡単に App Service web アプリにバック グラウンド ジョブ処理を追加します。

サーバーへのリモート アクセス、サーバーのスタートアップ タスクの構成など、Web サーバー環境をもっと細かく制御する必要がある場合は、Azure Cloud Services が通常、最善の選択肢となります。

既にあるアプリケーションを Azure App Service や Azure Cloud Services で実行するためには、かなりの修正が必要という場合、Azure Virtual Machines を選ぶことで、クラウドへの移行を単純化できる場合があります。 ただし、VM の構成、セキュリティ対策、メンテナンスを正しく行うためには、Azure App Service や Cloud Services と比べて、はるかに時間がかかり、IT に関する豊富な知識と経験が要求されます。 Azure Virtual Machines を選択する場合は、VM 環境に対する修正プログラムの適用、更新、管理に伴って日々発生するメンテナンスの労力を考慮してください。

以上に挙げた Azure の Web ホスティング方法ごとに、制御の柔軟性と使いやすさの相対的な度合いを示したのが次の図です。

![ChoicesDiagram][choicesdiagram]

## <a name="scenarios"></a>シナリオと推奨事項

以降、一般的なアプリケーションのシナリオをいくつか取り上げると共に、それぞれのシナリオで最適と思われる Azure Web ホスティングの選択肢を紹介します。

- [オンプレミスのアセットと連係させ、ビジネス アプリケーションを実行するバック グラウンド処理とデータベースのバックエンドでの web フロント エンドが必要です。](#onprem)
- [信頼性の高い方法でも拡大縮小、会社の web サイトをホスティングする必要があり、グローバル サービスに到達します。](#corp)
- [Windows Server 2003 で動作する IIS6 アプリケーションがあります。](#iis6)
- [スモール ビジネスのオーナーです。 したり、安価な方法で自分のサイトをホスティングする必要がありますが、将来の成長を考慮します。](#smallbusiness)
- [Web デザイナーまたはグラフィック デザイナーですしを設計および顧客の web サイトを構築したいです。](#designer)
- [Web フロント エンドを含む多層アプリケーションをクラウドに移行しています。](#multitier)
- [Linux 環境とクラウドに移行するし、アプリケーションは、高度にカスタマイズされた Windows で異なります。](#custom)
- [自分のサイトは、オープン ソース ソフトウェアを使用し、Azure でホストするたいです。](#oss)
- [企業ネットワークに接続する必要がある基幹業務アプリケーションがあるのです。](#lob)
- [REST API またはモバイル クライアント向けの web サービスをホストします](#mobile)


### <a id="onprem"></a> オンプレミスのアセットと連係させ、ビジネス アプリケーションを実行するバック グラウンド処理とデータベースのバックエンドでの web フロント エンドが必要です。

そのような複合的なビジネス アプリケーションには、Azure App Service が最適です。 Azure Websites 向けにアプリケーションを開発することで、負荷分散プラットフォーム上でスケールを自動的に調整し、Active Directory でセキュリティを確保しながら、オンプレミスのリソースに接続することが可能です。 それらのアプリケーションは、ワールドクラスのポータルと API によって簡単に管理できます。さらに、そのアプリケーションが顧客によってどのように利用されているかを、アプリケーション洞察ツールで深く見通すことができます。  [Web ジョブの][] 機能は、バック グラウンド プロセスを実行することができ、ハイブリッド接続中に、web 層の一部としてタスクと [VNET 機能](../fundamentals-introduction-to-azure.md#networking/) 内部設置型リソースへの接続しやすきます。 Azure App Service は Web アプリの 99.9% の稼働率が SLA で保証されています。Azure App Service でできることを次に示します。

* 自己復旧機能と自動修正機能を備えたクラウド プラットフォーム上でアプリケーションを実行し、高い信頼性を確保する。
* グローバルなデータセンターのネットワークを活かしてスケールを自動的に調整する。
* バックアップと復元によって災害復旧に対応する。
* ISO、SOC2、PCI に準拠する。
* Active Directory と連係する。

### <a id="corp"></a> 信頼性の高い方法でも拡大縮小、会社の web サイトをホスティングする必要があり、グローバル サービスに到達します。

Azure App Service は、企業 Web サイトをホスティングするためのソリューションとしてきわめて優れた特長を持っています。 グローバルなデータセンターのネットワークを活かして Web アプリのスケールを短期間で拡張し、すぐに需要に応えることができます。 また、ローカル リーチ、フォールト トレランス、インテリジェントなトラフィック管理が実現されています。 そのすべてが、ワールドクラスの管理ツールを備えたプラットフォームに集約され、サイトの稼働状態とトラフィックの状況を速やかにかつ簡単に把握することができます。 Azure App Service は Web アプリの 99.9% の稼働率が SLA で保証されています。Azure App Service でできることを次に示します。

* 自己復旧機能と自動修正機能を備えたクラウド プラットフォーム上で Web サイトを実行し、高い信頼性を確保する。
* グローバルなデータセンターのネットワークを活かしてスケールを自動的に調整する。
* バックアップと復元によって災害復旧に対応する。
* 統合されたツールを使用してログとトラフィックを管理する。
* ISO、SOC2、PCI に準拠する。
* Active Directory と連係する。

### <a id="iis6"></a> Windows Server 2003 で動作する IIS6 アプリケーションがあります。

Azure App Service は、過去の IIS6 アプリケーションの移行に伴うインフラストラクチャ コストを効果的に抑制します。 Microsoft が作成された [使いやすい移行ツールと詳細な移行ガイド](https://www.movemetowebsites.net/) 互換性をチェックして変更できるようにする必要がある点を確認できるようにします。 Visual Studio や TFS、さらに、広く普及している CMS ツールとの高い親和性により、IIS6 アプリケーションを手間なく直接クラウドにデプロイすることができます。 デプロイ後は、Azure ポータルの堅牢な管理ツールを使ってスケールを調整 (スケールダウンまたはスケールアップ) することにより、必要に応じてコストを抑制したり需要の増大に対応したりることができます。 移行ツールを使用すると、次のことができます。

* 既存の Windows Server 2003 Web アプリケーションをすばやく簡単にクラウドへと移行する。
* 必要であれば、接続先の SQL Database をオンプレミスに維持し、ハイブリッド型のアプリケーションを作成する。
* SQL Database をレガシ アプリケーションと共に自動的に移動する。

### <a id="smallbusiness"></a>スモール ビジネスのオーナーです。 したり、安価な方法で自分のサイトをホスティングする必要がありますが、将来の成長を考慮します。

このシナリオでは、無料で使用を開始して必要なときにさらに機能を追加できる、Azure App Service が最適なソリューションです。 無料の Web アプリにはいずれも、Azure によって提供されるドメインが付属しています (*your_company*.azurewebsites.net)。また、統合デプロイメント/管理ツールや、導入作業を支援するアプリケーション ギャラリーも、このプラットフォームに含まれています。 他にも、ユーザーの要求が増えるにつれて拡大できる多くのサービスや拡張オプションがあります。 Azure App Service を使用すると、次のことができます。

- Free レベルから開始し、必要に応じてスケールアップできます。
- アプリケーション ギャラリーを使用して、WordPress などの一般的な Web アプリケーションをすばやくセットアップできます。
- 必要に応じて、Azure のその他のサービスや機能をアプリケーションに追加できます。
- Web アプリのセキュリティを HTTPS で保護することができます。

### <a id="designer"></a> Web デザイナーまたはグラフィック デザイナーですし、デザインおよび顧客の web サイトを構築します。

Web 開発者とデザイナーのために、Azure App Service は、Git や FTP でのデプロイメントに対応するなど、さまざまなフレームワークおよびツールと容易に連係できるようになっているほか、Visual Studio や SQL Database などのツールやサービスと緊密に統合されています。 App Service を使用すると、次のことができます。

- コマンド ライン ツールを使用して [[スクリプト] タスクを自動化][scripting]します。
- Work with popular languages such as [.Net][dotnet], [PHP[]][], [Node.js][nodejs], and [Python[]][].
- 非常に高い容量に拡張するための 3 つの異なる拡張レベルを選択できます。
- などの他の Azure サービスと統合 [SQL データベース ][sqldatabase], 、[Service Bus ][servicebus] と [ストレージ][], 、パートナーの製品から、または、 [Azure ストア ][azurestore], 、MySQL、MongoDB などです。
- Visual Studio、Git、WebMatrix、WebDeploy、TFS、FTP などのツールと統合できます。

### <a id="multitier"></a>Web フロント エンドを含む多層アプリケーションをクラウドに移行しています。

データベースに接続する Web サーバーなど多層アプリケーションを実行する用途において、Azure SQL Database と緊密に連係する Azure App Service は良い選択といえます。 Web ジョブ機能を使用してバックエンド プロセスを実行することもできます。

サーバーへのリモート アクセス、サーバーのスタートアップ タスクの構成など、さらに細かくサーバー環境を制御する必要がある場合は、1 つまたは複数の階層に Cloud Services を使用してください。

独自のコンピューター イメージの使用を希望される場合や、Cloud Services 上で構成することのできないサーバー ソフトウェア (またはサーバー サービス) を使用する必要がある場合は、1 つまたは複数の階層に Virtual Machines を使用してください。

### <a id="custom"></a>Linux 環境とクラウドに移行するし、アプリケーションは、高度にカスタマイズされた Windows で異なります。

アプリケーションを使用するために、ソフトウェアおよびオペレーティング システムの複雑なインストールまたは構成が必要な場合、Virtual Machines が最良のソリューションであると思われます。 Virtual Machines を使用すると、次のことができます。

- 仮想マシン ギャラリーを使用して、Windows や Linux などのオペレーティング システムから始め、後でアプリケーションの要件に合わせてカスタマイズできます。
- 既存のオンプレミス サーバーのカスタム イメージを作成してアップロードすることで、Azure の仮想マシンで実行できます。

### <a id="oss"></a>自分のサイトは、オープン ソース ソフトウェアを使用し、Azure でホストする必要

そのオープン ソース フレームワークが App Service でサポートされている場合、ご利用のアプリケーションに必要な言語とフレームワークが自動的に構成されます。 App Service を使用すると、次のことができます。

- Use many popular open source languages, such as [.NET][dotnet], [PHP[]][], [Node.js][nodejs], and [Python[]][].
- WordPress、Drupal、Umbraco、DNN、および多くのサード パーティ製 Web アプリケーションをセットアップできます。
- 既存のアプリケーションを移行することも、アプリケーション ギャラリーから新規アプリケーションを作成することもできます。

ご利用のオープン ソース フレームワークが App Service でサポートされていない場合は、他の 2 つの Azure Web ホスティング方法をご利用ください。 Cloud Services を使用する場合、スタートアップ タスクを使用して、必要なオープン ソース ソフトウェア (Windows 上で動作) をインストールおよび構成します。 Virtual Machines を使用する場合、Windows または Linux ベースのマシン イメージにソフトウェアをインストールして構成します。

### <a id="lob"></a>企業ネットワークに接続する必要がある基幹業務アプリケーションがあります。

基幹業務アプリケーションを作成する場合は、Web サイトから社内ネットワーク上のサービスやデータに直接アクセスすることが必要な場合があります。 これは、App Service、Cloud Services、および使用して仮想マシンに対して実行できる、 [Azure Virtual Network サービス](/services/virtual-network/)します。 App Service で使用して、 [VNET 統合機能](http://azure.microsoft.com/blog/2014/09/15/azure-websites-virtual-network-integration/), 、これにより、Azure アプリケーションを場合と同様、企業ネットワーク上を実行します。

### <a id="mobile"></a>REST API またはモバイル クライアント向けの web サービスをホストする必要

HTTP ベースの Web サービスを使用すると、モバイル クライアントを含めて広範囲のクライアントをサポートすることができます。 ASP.NET Web API のようなフレームワークは、REST サービスを作成および使用しやすくするために Visual Studio と統合されています。 これらのサービスは Web エンドポイントから公開されるため、Azure での Web ホスト手法を使用してこのシナリオをサポートすることができます。 ただし、REST API をホストするためには App Service が適切な選択です。 App Service を使用すると、次のことができます。

- Web アプリを迅速に作成して、Azure のグローバルに分散したデータ センターの 1 つで HTTP Web サービスをホストすることができます。
- 既にあるサービスを移行したり、新しいサービスを作成したりすることができます。
- 1 つのインスタンスで可用性の SLA を実現するか、または複数の専用コンピューターにスケール アウトします。
- 発行済みのサイトを使用して、モバイル クライアントを含む HTTP クライアントに REST API を提供します。

また、新しいプレビュー機能での REST Api は Azure App Service: API アプリ。 API アプリに関する詳細については、次を参照してください。 [API apps とは](../app-service-api/app-service-api-apps-why-best-platform.md)します。

## <a name="features"></a>機能の比較

次の表では、最善の選択ができるように、App Service、Cloud Services、および Virtual Machines の機能を比較しています。 各オプションの SLA に関する最新情報は、次を参照してください。 [Azure サービス レベル アグリーメント](/support/legal/sla/)します。

 機能| App Service (Web Apps)| Cloud Services (Web ロール)| Virtual Machines| メモ
---|---|---|---|---
 ほぼ即時のデプロイメント| ○| | | アプリケーションまたはアプリケーションの更新プログラムを Cloud Services にデプロイしたり、VM を作成したりするには、最低でも数分かかります。一方、アプリケーションを Web アプリにデプロイする場合の所要時間は数秒です。
 再デプロイなしでの大型マシンへのスケールアップ| ○| | |
 Web サーバーのインスタンスは、コンテンツと構成を共有します。つまり、スケールする際に、デプロイまたは構成をやり直す必要はありません。| ○| | |
 複数のデプロイメント環境 (運用環境とステージング環境)| ○| ○| |
 OS の自動更新の管理| ○| ○| |
 シームレスなプラットフォームの切り替え (32 ビットと 64 ビット間で簡単に移動)| ○| ○| |
 GIT、FTP によるコードのデプロイ| ○| | ○|
 Web デプロイによるコードのデプロイ| ○| | ○| Cloud Services では、Web 配置を使用して、個々のロール インスタンスに更新プログラムをデプロイできます。ただし、ロールの初回デプロイメントに Web 配置を使用することはできません。また、更新プログラムに Web 配置を使用する場合は、各ロールのインスタンスに対して個別にデプロイする必要があります。運用環境で Cloud Services の SLA を満たすには、複数のインスタンスが必要です。
 WebMatrix サポート| ○| | ○|
 Service Bus、Storage、SQL Database のようなサービスへのアクセス| ○| ○| ○|
 多層アーキテクチャの Web 層または Web サービス層のホスト| ○| ○| ○|
 多層アーキテクチャの中間層のホスト| ○| ○| ○| App Service web apps は REST API 中間層では、簡単にホストされ、 [web ジョブ](http://go.microsoft.com/fwlink/?linkid=390226) 機能でバック グラウンド処理ジョブをホストできます。Web ジョブを専用 Web サイトで実行することにより、その階層のスケーラビリティを個別に確保できます。プレビュー [API apps](../app-service-api/app-service-api-apps-why-best-platform.md) 機能は、REST サービスをホストするためのさらに多くの機能を提供します。
 統合されたサービスとしての MySQL のサポート| ○| ○| ○| Cloud Services は、ClearDB のサービスを介してサービスとしての MySQL を統合できますが、Azure ポータル ワークフローの一部として統合することはできません。
 ASP.NET、クラシック ASP、Node.js、PHP、Python のサポート| ○| ○| ○|
 再デプロイなしでの複数インスタンスへのスケールアウト| ○| ○| ○| Virtual Machines は複数のインスタンスにスケールアウトできますが、そこで実行されるサービスが、このようなスケールアウトに対応できるように記述されていなければなりません。要求を複数のコンピューターにルーティングするためのロード バランサーを構成すると共に、アフィニティ グループを作成して、メンテナンスやハードウェアの障害で全インスタンスが同時に再起動するのを防ぐ必要があります。
 SSL のサポート| ○| ○| ○| App Service Web Apps の場合、カスタム ドメイン名の SSL は Basic モードと Standard モードでのみサポートされます。Web アプリを使用して SSL を使用する方法の詳細については、次を参照してください。 [Azure の web サイトの SSL 証明書を構成する](../app-service-web/web-sites-configure-ssl-certificate.md)します。
 Visual Studio 統合| ○| ○| ○|
 リモート デバッグ| ○| ○| ○|
 TFS によるコードのデプロイ| ○| ○| ○|
 によるネットワークの分離 [Azure Virtual Network](/services/virtual-network/)| ○| ○| ○| 関連項目 [Azure web サイトの仮想ネットワーク統合](/blog/2014/09/15/azure-websites-virtual-network-integration/)
 サポート [Azure Traffic Manager](/services/traffic-manager/)| ○| ○| ○|
 統合エンドポイント監視| ○| ○| ○|
 サーバーへのリモート デスクトップ アクセス| | ○| ○|
 カスタム MSI のインストール| | ○| ○|
 スタートアップ タスクの定義と実行| | ○| ○|
 ETW イベントのリッスン| | ○| ○|

> [AZURE.NOTE]
> 場合は、アカウントにサインアップする前に Azure App Service の使用を開始するには、 <a href="https://trywebsites.azurewebsites.net/">https://trywebsites.azurewebsites.net</a>, することをすぐに作成、有効期間の短いスターター アプリ Azure App Service では無料です。 このサービスの利用にあたり、クレジット カードは必要ありません。契約も必要ありません。


## <a id="nextsteps"></a> 次のステップ

3 つの Web ホスティング方法の詳細については、次のリソースを参照してください。

* [Azure の概要](../fundamentals-introduction-to-azure.md)
* [コンピューティング Azure によって提供されるホスティング オプション](../fundamentals-application-models.md)

アプリケーションに使用するホスティング方法が決まったら、次のリソースで基本事項を確認してください。

* [Azure App Service](/documentation/services/app-service/)
* [Azure クラウド サービス](/documentation/services/cloud-services/)
* [Azure の仮想マシン](/documentation/services/virtual-machines/)


[choicesdiagram]: ./media/choose-web-site-cloud-service-vm/Websites_CloudServices_VMs_3.png 
[azure app service]: /services/app-service/ 
[cloud services]: http://go.microsoft.com/fwlink/?LinkId=306052 
[virtual machines]: http://go.microsoft.com/fwlink/?LinkID=306053 
[cleardb]: http://www.cleardb.com/ 
[webjobs]: http://go.microsoft.com/fwlink/?linkid=390226&clcid=0x409 
[configuring an ssl certificate for an azure website]: http://www.windowsazure.com/develop/net/common-tasks/enable-ssl-web-site/ 
[azurestore]: http://www.windowsazure.com/gallery/store/ 
[scripting]: http://www.windowsazure.com/documentation/scripts/?services=web-sites 
[dotnet]: http://www.windowsazure.com/develop/net/ 
[nodejs]: http://www.windowsazure.com/develop/nodejs/ 
[php]: http://www.windowsazure.com/develop/php/ 
[python]: http://www.windowsazure.com/develop/python/ 
[servicebus]: http://www.windowsazure.com/documentation/services/service-bus/ 
[sqldatabase]: http://www.windowsazure.com/documentation/services/sql-database/ 
[storage]: http://www.windowsazure.com/documentation/services/storage/ 

