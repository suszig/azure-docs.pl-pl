<properties
   pageTitle="Azure Web アプリのパフォーマンスをテストする | Microsoft Azure"
   description="Azure Web アプリのパフォーマンスを実行して、アプリでユーザーの負荷がどのように処理されるかを確認します。応答時間を測定し、問題を示す可能性があるエラーを見つけます。"
   services="app-service\web"
   documentationCenter=""
   authors="ecfan"
   manager="douge"
   editor="jimbe"/>

<tags
   ms.service="app-service-web"
   ms.workload="web"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="09/11/2015"
   ms.author="estfan; manasma"/>


# 負荷をかけた状態での Azure Web アプリのパフォーマンス テスト

Web アプリを起動する前または更新プログラムを運用環境にデプロイする前に、パフォーマンスを確認しましょう。 
これにより、アプリをリリースする準備が整っているかどうかをより適切に評価できます。 詳細と思われる
確実に、アプリはピーク使用中または、次のマーケティングのプッシュ時に、トラフィックを処理できます。

パブリック プレビューの段階では、Azure ポータルでアプリのパフォーマンスを無料でテストできます。
これらのテストでは、一定期間にわたってアプリのユーザー負荷をシミュレートし、アプリの応答を測定します。 たとえば、テスト結果は特定の台数をアプリの応答速度を表示します。 
ユーザーです。 また、アプリに存在する問題を示す可能性がある失敗した要求の数も示されます。

![Web アプリのパフォーマンスの問題を見つける][testoverview]

## 開始する前に

*   必要な [Azure サブスクリプション ][azuresubscription],、 
場合は、既に 1 つがあるありません。 学習する方法 
[無料 ][azurefreetrial]します。

*   必要な [Visual Studio Team Services (VSTS) ][whatisvsts] 
パフォーマンス テストの履歴を保持するアカウント。 
パフォーマンス テストを設定すると、新しいアカウントを作成します。 
または、アカウント所有者である場合は、既存のアカウントを使用します。 
[Visual Studio チームのサービス アカウントを使用すれば他に何をでしょうか。](#VSTSAccount)

*   非運用環境にテストするアプリをデプロイします。 
運用環境で使用するプラン以外の App Service プランをアプリで使用するように設定します。 
これで、既存の顧客に影響が及んだり、運用環境のアプリの速度が低下したりすることがなくなります。

## パフォーマンス テストの設定と実行

0.  サインイン、 [Azure ポータルの ][azureportal]します。 
所有している Visual Studio チームのサービス アカウントを使用するには 
アカウント所有者としてサインインします。

0.  Web アプリに移動します。

    ![[すべてを参照] > ][webapp]

0.  **[パフォーマンス テスト]** に移動します。

    ![[ツール] > ][expandedtools]

0.  現在、をリンクさせます [Visual Studio Team Services (VSTS) ][whatisvsts] 
パフォーマンス テストの履歴を保持するアカウント。

    使用できる VSTS アカウントがある場合は、そのアカウントを選択します。 VSO アカウントを持っていない場合は、新しいアカウントを作成します。

    ![既存の VSTS アカウントを選択するか、新しいアカウントを作成します][existingnewvstsaccount]

0.  パフォーマンス テストを作成します。 詳細を設定し、テストを実行します。 
テストの実行中、リアルタイムで結果を確認できます。

    たとえば、昨年の休日セールでクーポンを配布したアプリがあるとします。 
    このイベントは、100 人の同時接続顧客数をピーク負荷として 15 分間にわたって続きました。 
    今年は、顧客の数を 2 倍にしたいと考えています。 向上します。 
    ページを減らすことによって顧客満足度は、2 秒間に 5 秒から時間を読み込みます。 
    そこで、更新したアプリのパフォーマンスを 250 人のユーザーを想定して 15 分間にわたってテストします。

    シミュレーションを行います負荷、アプリケーションの仮想ユーザー (顧客) を生成することによって 
    同時に、web サイトを訪問します。 これが表示されます数 
    要求が失敗したか、応答が低下します。

    ![パフォーマンス テストの作成、設定、実行][newtest]

     *  Web アプリの既定の URL は自動的に追加されます。 
     URL を変更して他のページ (HTTP GET 要求のみ) をテストすることができます。

     *  ローカルの状態をシミュレートし、待機時間を短縮するには 
     負荷を生成するには、ユーザーに最も近い場所を選択します。

    テストが実行されているときのようすを次に示します。 最初の 1 分間に 
    速度が遅いため、ページを読み込みます。

    ![実行中のパフォーマンス テストとリアルタイムのデータ][testrunning]

    ページが非常に高速に読み込むことを学習、テストが完了したら、 
    後は、最初の分です。 これにより、特定する必要のあります。 
    問題のトラブルシューティングを開始します。

    ![パフォーマンス テスト完了、失敗したリクエストなどの結果がわかる][testdone]

ご意見をお待ちしております。 質問や問題、 
お問い合わせください。 <vsoloadtest@microsoft.com>

## Q & A

#### Q: テストの実行時間に関して制限はありますか?

A: はい。Azure ポータルでは、最長 1 時間までテストを実行できます。

#### Q: パフォーマンス テストの実行に使用できる時間はどれだけユーザーに与えられますか。

A: パブリック プレビューした後に、20,000 仮想ユーザー分 (VUMs) を入手します。 
Visual Studio チームのサービス アカウントを使用して 1 か月あたりを解放します。 
Vum までは、数を掛けます仮想ユーザーの数 
テストに分です。 お客様のニーズは、無料の制限を超えた場合 
多くの時間を購入して使用する分だけ払うことができます。

#### Q: これまで使用した VUM はどこで確認できますか。

A: Azure ポータルでこの時間を確認できます。

![自分の VSTS アカウントに進みます][vstsaccount]

![使用した VUM の確認][checktesttime]

<a name="VSOAccount"></a>
#### Q: Visual Studio Team Services アカウントでほかにできることは何ですか。

A: 新しいアカウントを検索するには `https://{accountname}.visualstudio.com`します。 
コードを共有する、ビルド、テスト、作業、およびクラウドですべて] – ソフトウェアの出荷を追跡 
任意のツールと言語を使用します。 方法の詳細について [Visual Studio Team Services ][whatisvsts] 
機能とサービスより簡単に共同作業を行うし、継続的な配置チームに役立ちます。




[webapp]: ./media/app-service-web-app-performance-test/azure-np-web-apps.png 
[testoverview]: ./media/app-service-web-app-performance-test/azure-np-perf-test-overview.png 
[expandedtools]: ./media/app-service-web-app-performance-test/azure-np-web-app-details-tools-expanded.png 
[existingnewvstsaccount]: ./media/app-service-web-app-performance-test/azure-np-no-vso-account.png 
[newtest]: ./media/app-service-web-app-performance-test/azure-np-new-performance-test.png 
[testrunning]: ./media/app-service-web-app-performance-test/azure-np-running-perf-test.png 
[testdone]: ./media/app-service-web-app-performance-test/azure-np-perf-test-done.png 
[vstsaccount]: ./media/app-service-web-app-performance-test/azure-np-vso-accounts.png 
[checktesttime]: ./media/app-service-web-app-performance-test/azure-np-vso-accounts-vum-summary.png 
[azureportal]: https://portal.azure.com 
[azuresubscription]: https://account.windowsazure.com/subscriptions 
[azurefreetrial]: https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F 
[whatisvsts]: https://www.visualstudio.com/products/what-is-visual-studio-online-vs 

