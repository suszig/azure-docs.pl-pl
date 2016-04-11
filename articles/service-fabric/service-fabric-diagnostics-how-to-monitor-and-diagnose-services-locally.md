<properties
   pageTitle="Microsoft Azure Service Fabric のサービスをローカルで監視して診断する方法"
   description="この記事には、ローカル開発用コンピューターで Microsoft Azure Service Fabric を使用して作成したサービスを監視および診断する方法について説明します。"
   services="service-fabric"
   documentationCenter=".net"
   authors="kunaldsingh"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="11/04/2015"
   ms.author="kunalds"/>


# ローカル コンピューターの開発のセットアップでのサービスの監視と診断
監視、検出、診断およびトラブルシューティングでは、ユーザー エクスペリエンスの中断を最小限に抑えてサービスを続行できます。 これは実際にデプロイされる運用環境では重要ですが、有効性は、実際の環境のセットアップに移動するときに動作するように、サービスを開発している間、同様のモデルを採用するかどうかで異なります。 Service Fabric によりサービスの開発者は、1 台のコンピューターのローカルの開発から実際の運用クラスターのセットアップに至るまで、シームレスに操作できる診断を簡単に実装できます。

## トレースとログ記録に ETW を使用する理由
[Windows イベント トレーシング](https://msdn.microsoft.com/library/windows/desktop/bb968803.aspx) (ETW) は、Service Fabric メッセージをトレースするための推奨されるテクノロジです。 その理由は、次のとおりです。

* ETW は、高速です。 コードの実行時間にほとんど影響を与えない追跡テクノロジによって構成されています。
* ETW トレースは、ローカル開発環境から実際のクラスターのセットアップに至るまでシームレスに機能します。 つまり、コードを実際のクラスターにデプロイする準備ができたら、トレース コードを書き直す必要はありません。
* Service Fabric のシステム コードも内部のトレースに ETW を使用します。 これを使用すると、アプリケーションのトレースと Service Fabric のシステム トレースが交互に配置されて表示され、アプリケーションのコードと基盤となるシステムのイベントの間の順番と相互関係を簡単に理解できます。
* ETW イベントを表示する Service Fabric のVisual Studio ツールが内蔵でサポートされます。


## Visual Studio で Service Fabric のシステム イベントを表示する

Service Fabric は ETW イベントを出力して、アプリケーションの開発者が、今プラットフォームで何が起こっているのか理解できるようにします。 に備えてをまだ行っていないして手順に従います [Visual Studio で、最初のアプリケーションを作成する](./service-fabric-create-your-first-application-in-visual-studio.md) トレース メッセージを示す診断イベント ビューアーを使用してアプリケーションを開始および実行します。 

1. [診断イベント] ウィンドウが自動的に表示されない場合は、Visual Studio の [サーバー エクスプローラー] タブに移動して、Service Fabric のクラスターを右クリックし、コンテキスト メニューで [診断イベントの表示] を選択します。

  ![Visual Studio の診断イベント ビューアーを開く](./media/service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally/ServerExViewDiagEvents.png)

2. 各イベントには、イベントの送信元のノード、アプリケーション、およびサービスを示す標準的なメタデータ情報があります。 また、ウィンドウ上部にある [イベントのフィルター] ボックスを使用して、ノード名やサービス名でフィルターするなど、イベントの一覧をフィルターすることもできます。 また、イベントの詳細を確認する際には、ウィンドウの上にあるボタンを使用して停止し、イベントが失われることなく後で再開できます。

  ![Visual Studio の診断イベント ビューアー](./media/service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally/DiagEventsExamples2.png)

## アプリケーション コードの独自のカスタム トレースを追加する
Service Fabric の Visual Studio プロジェクト テンプレートには、サンプル コードが含まれています。 コードは、Visual Studio ETW ビューアーに表示されるカスタム アプリケーション コードの ETW トレースと Service Fabric からのシステム トレースを追加する方法を示しています。 このメソッドの利点は、メタデータがトレースに自動的に追加され、Visual Studio の診断ビューアーがそれらを表示するよう既に構成されていることです。

作成されたプロジェクト、 **サービス テンプレート** (ステートレスまたはステートフル) を検索、 `RunAsync` 実装します。

1. `RunAsync` メソッドで `ServiceEventSource.Current.ServiceMessage` を呼び出すと、アプリケーション コードからのカスタムの ETW トレースの例が表示されます。
2.  **ServiceEventSource.cs** ファイル用のオーバー ロードに見つかります、 `ServiceEventSource.ServiceMessage` メソッド パフォーマンス上の理由により、頻度の高いイベントを使用する必要があります。

作成されたプロジェクト、 **アクター テンプレート** (ステートレスまたはステートフル)。

1. 開いている、 **"ProjectName".cs** ファイル *ProjectName* Visual Studio プロジェクト用に選択した名前を指定します。  
2. コードを見つけます `ActorEventSource.Current.ActorMessage(this, "Doing Work");` で、 *DoWorkAsync* メソッドです。  これは、アプリケーション コードからのカスタムの ETW トレースの例です。  
3. ファイルに **ActorEventSource.cs**, のオーバー ロードが見つかります、 `ActorEventSource.ActorMessage` メソッド パフォーマンス上の理由により、頻度の高いイベントを使用する必要があります。

カスタムの ETW トレースをサービスのコードに追加した後で、もう一度アプリケーションをビルド、デプロイ、および実行して、診断ビューアーでイベントを表示することができます。 F5 キーを押してアプリケーションをデバッグする場合、診断ビューアーが自動的に開きます。

## 次のステップ
上記のローカルの診断でアプリケーションに追加したトレース コードは、Azure のクラスターでアプリケーションを実行すると、これらのイベントの表示に使用できるツールでは機能のチェック アウト、ツールのさまざまなオプションについて説明しを設定する方法を説明する次の記事です。
* [WAD (Windows Azure 診断) とオペレーション インサイトを使用して azure Service Fabric クラスターからのログの収集](service-fabric-diagnostics-how-to-setup-wad-operational-insights.md)
* [Service Fabric アプリケーション トレース ストアとして ElasticSearch を使用する](service-fabric-diagnostic-how-to-use-elasticsearch.md)

