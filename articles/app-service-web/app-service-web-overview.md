<properties
    pageTitle="Web Apps の概要"
    description="App Service Web Apps ついてさらに詳しく説明します"
    services="app-service\web"
    documentationCenter=""
    authors="jaime-espinosa"
    manager="wpickett"
    editor="jimbe"/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/27/2015"
    ms.author="jaime.espinosa"/>



# Web Apps の概要

[App Service](http://go.microsoft.com/fwlink/?LinkId=529714) プロの開発者によって web、モバイル、および統合シナリオで豊富な機能を完全に管理されたプラットフォームは、です。 Azure App Service を利用することで、ビジネスの規模に対応するミッション クリティカルな Web アプリをすばやく作成し、デプロイできます。

機能を活用 [App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714) 言語やフレームワークに依存して理解を使用する Azure Cloud にすばやくアプリケーションを配置し、二度インフラストラクチャについて心配することがなく、コードを継続的に改善します。

![Web Marketplace](./media/app-service-web-overview/marketplace.png)

## 単なる Web サイト以上のもの

現代の企業は、これまでにないほど洗練された方法で顧客と交流しています。 あらゆる種類の企業が、企業 Web サイトをビジネスに不可欠な部分と見なし、ビジネス プランにおける主要なコンポーネントであると考えています。 この重要度に対応するために、企業は、機敏性、セキュリティ、スケーラビリティをもたらすプラットフォームを求めています。 さらに、企業では、既存のビジネス システムにリンクする機能、新しいコードを迅速にデプロイする機能、世界各地でインスタンスを起動する機能を必要としています。 Azure App Service と Web Apps を利用することで、企業は迅速かつ費用効率に優れた方法で顧客の要求に応えることができます。

## なぜ Web Apps なのか

Azure App Service Web Apps は、エンタープライズ レベルの Web アプリを数秒で構築、デプロイ、スケーリングできる、完全に管理されたプラットフォームです。 アプリケーションのコードにさえ集中していれば、Azure が代わりにインフラストラクチャをスケーリングし、安全にアプリケーションを実行します。 Web Apps の特徴

- **扱いやすく、高速** - 好みの言語、フレームワーク、IDE で、既に持っているスキルを活用してコーディングします。 バージョン管理、更新、シングル サインオン、ID ブローカー、分離ストレージ、パフォーマンス監視などを、たった数回のクリックで既存の Web アプリに追加できます。 豊富なギャラリーにアクセスし、開発を加速するための構成要素として使用します。 継続的な統合、稼働中のサイトでのデバッグ、業界をリードする Visual Studio IDE などの最先端機能により、圧倒的な開発者生産性が体験できます。
- **エンタープライズ レベル** - Web Apps は、安全でミッション クリティカルなアプリケーションの構築およびホスト向けに設計されています。 オンプレミス リソースに安全に接続する Active Directory 統合のビジネス アプリを構築し、それを ISO、SOC2、および PCI 準拠の、セキュリティで保護されたクラウド プラットフォーム上でホストします。 エンタープライズ レベルの SLA を満足しながら、これらすべてを実現します。
- **グローバル スケール** - Web Apps は、グローバル データセンター インフラストラクチャにおいて可用性と自動スケーリングを提供できるように最適化されています。 要求に応じて、アプリケーションのスケール アップやスケール ダウンが容易に実行できます。 さらに、さまざまな地理的リージョンにわたって高い可用性が得られます。 データのレプリケートや複数の場所でのサービスのホストがすばやく簡単なため、新しいリージョンへの展開が、クリック 1 回ほどの手間で実行できます。

## Web Apps の概念

- **Web Apps ギャラリー** - 常に増え続ける、既存のアプリケーション テンプレートのリストから選択します。 Wordpress、Joomla、Drupal など、OSS アプリ コミュニティのベスト アプリを、ワンクリック インストールで利用できます。 .NET MVC、Django、CakePHP などのフレームワークを活用すると、アプリケーションの開発プロセスがスムーズに開始できます。
- **自動スケーリング** - Web Apps では、将来の顧客負荷に対処するため、スケール アップやスケール アウトをすばやく実行できます。 VM の数とサイズを手動で選択するか、auto-scaling を設定して、負荷またはスケジュールに応じてサーバーをスケーリングします。
- **継続的な統合** - VSTS、GitHub、TeamCity、Hudson、または BitBucket との継続的な統合とデプロイのワークフローを設定します。これにより、コードのチェックインまたは統合テストが成功するたびに、Web アプリを自動的にビルド、テスト、デプロイすることができます。
- **デプロイ スロット** の実装 [ステージングされたデプロイメント ][slots] を Azure App Service で運用環境の web アプリと同じ運用前環境でコードを確認します。 結果に満足したら、スワップ操作を実行することにより、ダウンタイムなしで新しいバージョンのアプリをリリースします。
- **運用環境でのテスト** - ステージングされたデプロイメントを次のレベルに移行し、A/B テストを実行して、構成可能な一部のライブ トラフィックで新しいコードを検証します。
- **Web ジョブ** - Web Apps VM でプログラムまたはスクリプトを実行します。 複数の VM で実行するため、継続的に、またはスケジュールやスケールに基づきジョブを実行します。 使用する Azure [web ジョブ SDK ][webjobs] Azure Storage または Service Bus と統合します。
- **ハイブリッド接続** --を使用して内部設置型データにアクセス [ハイブリッド接続](../integration-hybrid-connection-overview.md) と [VNET](../app-service-web/web-sites-integrate-with-vnet.md)します。

## Getting Started (概要)

を Web アプリを開始するには、次の [ASP.NET web アプリの作成 ][create] チュートリアルです。

Azure App Service プラットフォームの詳細については、次を参照してください。 [Azure App Service ][appservice]します。
>[AZURE.NOTE] 場合は、Azure アカウントがサインアップする前に Azure App Service の使用を開始するには、 [App Service の試用](http://go.microsoft.com/fwlink/?LinkId=523751), 、App Service で有効期間の短いスターター web アプリをすぐに作成する場所です。 このサービスの利用にあたり、クレジット カードは必要ありません。契約も必要ありません。

## 変更内容

* Web サイトから App Service への変更のガイドを参照してください: [Azure App Service と既存の Azure サービスへの影響](http://go.microsoft.com/fwlink/?LinkId=529714)


[appservice]: ../app-service/app-service-value-prop-what-is.md 
[create]: web-sites-dotnet-get-started.md 
[webjobs]: websites-dotnet-webjobs-sdk-get-started.md 
[slots]: web-sites-staged-publishing.md 

