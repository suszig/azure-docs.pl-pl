<properties 
   pageTitle="Azure SDK for .NET 2.6 リリース ノート" 
   description="Azure SDK for .NET 2.6 リリース ノート" 
   services="app-service/web" 
   documentationCenter=".net" 
   authors="Juliako" 
   manager="dwrede" 
   editor=""/>

<tags
   ms.service="app-service"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration" 
   ms.date="12/02/2015"
   ms.author="juliako"/>


# Azure SDK for .NET 2.6 リリース ノート

このドキュメントには、Azure SDK for .NET 2.6 リリースのリリース ノートが含まれます。 

Azure SDK 2.6 では、クラウド サービス ロールにターゲット .NET Framework を手動でインストールした場合に、.NET 4.5.2 または .NET 4.6 をターゲットとするクラウド サービス アプリケーション (PaaS) を開発できます。 参照してください [.NET クラウド サービスの役割をインストールする](http://go.microsoft.com/fwlink/?LinkID=309796)です。


##Service Bus の更新

- Event Hubs: 

    - Event Hubs の追加の発行者エンドポイントを公開することで、イベントの送信時に、対象のアクセス権を制御できるようになりました。
    - Event Hubs の機能と安定性が強化されました。
    - WebSocket を使用したメッセージングと Event Hubs の Amqp プロトコルのサポートが追加されました。

##HDInsight Tools for Visual Studio の更新

- **IntelliSense の強化**: リモート メタデータの推奨事項

    HDInsight Tools for Visual Studio で、Hive スクリプトの編集時にリモート メタデータの取得がサポートされるようになりました。 たとえば、「 **選択 * FROM** とすべてのテーブル名が表示されます。 また、テーブルを指定した後、列名も表示されます。

- **HDInsight Emulator のサポート**

    HDInsight Tools for Visual Studio で HDInsight Emulator への接続がサポートされるため、コストを導入することなくローカルで Hive スクリプトを開発し、HDInsight クラスターに対してこれらのスクリプトを実行できるようになりました。 

    詳細についてを参照してください [このマニュアル](http://go.microsoft.com/fwlink/?LinkID=529540&clcid=0x409)します。

- **HDInsight Tools for Visual Studio のサポートの汎用 Hadoop クラスター** (プレビュー)

    HDInsight Tools for Visual Studio に汎用 Hadoop クラスターのサポートが追加されたため、HDInsight Tools for Visual Studio を使用して、次のことを実行できます。

    - クラスターに接続 
    - 強化された IntelliSense とオートコンプリート サポートを使用して、Hive クエリを記述 
    - 直感的な UI で、クラスター内のすべてのジョブを表示 

    詳細についてを参照してください [このマニュアル](http://go.microsoft.com/fwlink/?LinkID=529540&clcid=0x409)します。

##In-Role Cache の更新

- **キャッシュのインロール キャッシュ** を使用する更新された **Microsoft Azure Storage SDK** バージョン 4.3 します。 これまで、 **In-role Cache** Azure Storage SDK バージョン 1.7 を使用していた。

    Azure SDK 2.5 以前を使用しているお客様は、Azure SDK 2.6 に更新し、新しいバージョンの Azure Storage SDK に移行する必要があります。 

    現時点では、Azure Storage バージョン 2011-08-18 は、2016 年 8 月 1 日に削除される予定です。 Azure SDK 2.5 以下から 2.6 へのすべての In-Role Cache の移行は、この日までに完了する必要があります。 Azure Storage バージョン 2011年-08-18 の提供終了の詳細については、次を参照してください。 [Microsoft Azure ストレージ サービスのバージョンの削除の更新: 2016 に拡張子](http://blogs.msdn.com/b/windowsazurestorage/archive/2015/10/19/microsoft-azure-storage-service-version-removal-update-extension-to-2016.aspx)します。

>[AZURE.IMPORTANT]Azure マネージ キャッシュ サービスと Azure ロール内キャッシュの [2016 年 11 月 30 日、提供終了発表します。 提供終了に備えて、Azure Redis Cache に移行することをお勧めします。 日付と移行のガイドラインの詳細については、次を参照してください。 [どの Azure Cache を利用に最適なでしょうか。](../redis-cache/cache-faq.md#which-azure-cache-offering-is-right-for-me)

##Azure App Service ツール

Azure SDK 2.6 リリースで次の項目が更新されました。

- Azure 発行に、デプロイ ターゲットとして Azure API Apps が追加されました。
- API Apps プロビジョニング機能により、ユーザーは API アプリの作成やプロビジョニング機能を使用できるようになります。
- Server Explorer で、リソース グループにより Web、モバイル、API Apps をグループ化して、新しい App Service ノードが反映されるようになりました。
- ほとんどの C# プロジェクトに追加された Azure API App Client ジェスチャが追加されました。これにより、Azure のサブスクリプションで実行する Swagger 対応 API Apps の自動生成が可能になります。
- Server Explorer の API Apps ツールと App Service ノードがVisual Studio 2013 でのみ利用できるようになりました。 

##Azure リソース マネージャー ツールの更新

Azure リソース マネージャー ツールで Virtual Machines、Networking、Storage のテンプレートを使用できるようになりました。 JSON 編集操作にテンプレート用の新しいアウトライン表示と、JSON スニペットを使用してテンプレートを編集する機能が追加されました。 Visual Studio からデプロイしたテンプレートでプロジェクトで指定した PowerShell スクリプトが使用されるため、スクリプトに対する変更が Visual Studio でも使用されます。

##Cloud Services の診断機能の強化

Azure SDK 2.6 において、Azure コンピューティング エミュレーターでの診断ログの収集と、それらの開発スとレージへの転送機能が復活しました。 アプリケーションのエミュレーターでの実行時に生成される診断ログ (アプリケーションのトレース ログ、Event Tracing for Windows (ETW) ログ、パフォーマンス カウンター、インフラストラクチャ ログ、Windows イベント ログなど) を開発ストレージに転送して、診断ログがローカル コンピューターで機能していることを確認できます。 

診断ストレージ アカウントがサービス構成 (.cscfg) ファイルで指定されるため、さまざまな環境にあった診断ストレージ アカウントを簡単に使用できるようになります。 接続文字列の Azure SDK 2.4 と Azure SDK 2.6 での動作に注目すべきいくつかの違いが見られます。 診断のストレージ接続文字列を使用する方法と、プロジェクトへの影響の詳細については「 [Azure クラウド サービス診断の構成](http://go.microsoft.com/fwlink/?LinkID=532784)します。

##重大な変更

###Azure リソース マネージャー ツール 

-  **Cloud Deployment Projects** Azure SDK 2.5 で使用できるプロジェクトの種類の名前は **Azure リソース グループ**します。
- **Cloud Deployment Projects** 2.6 で Azure SDK 2.5 で作成されたプロジェクトの種類を使用できますが、Visual Studio からテンプレートをデプロイは失敗します。 ただし、PowerShell スクリプトでのデプロイは引き続き機能します。  使用する方法の詳細について **Cloud Deployment Projects** この記事を読む 2.6 で [投稿](http://go.microsoft.com/fwlink/?LinkID=534086)します。
 
##既知の問題

- エミュレーターで診断ログを収集するには、64 ビットのオペレーティング システムが必要です。 32 ビットのオペレーティング システムで実行した場合、診断ログは収集されません。 これは、他のエミュレーター機能には影響しません。 

- 2015 年 4 月 29 日にリリースされた Azure SDK 2.6 には 2 つの問題がありました。 

    - Azure SDK 2.6 がコンピューターにインストールされていると、Visual Studio 2015 でユニバーサル アプリを読み込むことができませんでした。
    - Cloud Service プロジェクトのデバッグは、Visual Studio 2013 と Visual Studio 2015 では失敗します。Visual Studio が応答しなくなり、「エミュレーター用の診断を構成しています」というメッセージを含むダイアログ ボックスを表示したままクラッシュします。
    
    Azure SDK 2.6 の更新プログラムは、2015 年 5 月 18 日にリリースされました。 更新されたバージョンは 2.6.30508.1601 です。前述の 2 つの問題に対する修正が含まれています。 SDK のビルドは、[コントロール パネル]、[プログラムと機能] の順にクリックし、[Microsoft Azure Tools for Microsoft Visual Studio 2013 – v 2.6] で確認できます。 [バージョン] 列にビルド番号が表示されます。

    上記の問題が解消されない場合は、Azure 2.6 SDK の最新バージョンをインストール [VS 2012](http://go.microsoft.com/fwlink/p/?linkid=323511&clcid=0x409), 、[VS 2013](http://go.microsoft.com/fwlink/p/?linkid=323510&clcid=0x409) または [VS 2015](http://go.microsoft.com/fwlink/?linkid=518003&clcid=0x409)します。
 
##関連項目

[Azure SDK for .NET および API のサポートと提供終了に関する情報](https://msdn.microsoft.com/library/azure/dn479282.aspx/)

