<properties
    pageTitle="Azure App Service でのフライト デプロイ (ベータ テスト)"
    description="アプリの新機能を運用環境下でテストしたり更新のベータ テストを実施したりする方法について説明したエンド ツー エンドのチュートリアルです。 継続的パブリッシング、スロット、トラフィック ルーティング、Application Insights との統合など、App Service の各種機能を組み合わせて実現しています。"
    services="app-service\web"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/16/2015"
    ms.author="cephalin"/>
# Azure App Service でのフライト デプロイ (ベータ テスト)

このチュートリアルを行う方法を示します *flighting 展開* のさまざまな機能を統合することにより [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) と [Azure Application Insights](/services/application-insights/)します。 

*Flighting* 展開プロセスは、新しい機能や、実際の顧客の限られた数の変更を検証して、実稼働環境で主要なテストは、 
シナリオです。 ベータ テストと酷似しており、"Controlled Test Flight (制御された環境で行われるテスト フライト)" と呼ばれることもあります。 Web プレゼンスと多くの大規模な企業は、このアプローチを使用します。 
実際のアプリケーションの更新は、上の事前検証を取得 [アジャイル開発](https://en.wikipedia.org/wiki/Agile_software_development)します。 Azure アプリケーション 
サービスでは、継続的な発行と同じ DevOps シナリオを実装する Application Insights と実稼働環境でテストを統合することができます。 利点 
この方法は次のとおりです。

- **実際のフィードバックを得る _する前に_ 更新が実稼働環境にリリースされた** -ことだけを解放するとすぐにフィードバックを得るよりの取得 
解放する前にフィードバックします。 製品ライフ サイクルの中でいつでも自由に、実際のユーザーのトラフィックと振る舞いに基づいて更新版をテストすることができます。
- **強化 [継続的なテスト駆動開発 (CTDD)](https://en.wikipedia.org/wiki/Continuous_test-driven_development)** - でテストを統合することで 
継続的インテグレーションで実稼働環境で Application Insights のインストルメンテーション、ユーザーの検証が発生した早い段階で自動的に、製品寿命で 
サイクルです。 テストを手動で実施するよりも短時間で済みます。
- **テストのワークフローを最適化** の継続的な監視のインストルメンテーションで実稼働環境でテストを自動化することによりの目標を達成することができます可能性があります 
さまざまな種類の 1 つのテストを処理するよう [統合](https://en.wikipedia.org/wiki/Integration_testing),、 
[回帰](https://en.wikipedia.org/wiki/Regression_testing), 、[ユーザビリティ](https://en.wikipedia.org/wiki/Usability_testing), 、アクセシビリティ、ローカライズ、 
[パフォーマンス](https://en.wikipedia.org/wiki/Software_performance_testing), 、[セキュリティ](https://en.wikipedia.org/wiki/Security_testing), と 
[受け入れ](https://en.wikipedia.org/wiki/Acceptance_testing)します。

フライト デプロイの目的は単にライブ トラフィックをルーティングすることではありません。 そのような展開入力把握するため、できるだけ早く、予期しないバグであるかどうか 
パフォーマンスの低下、ユーザー エクスペリエンスの問題です。 注目すべき点は、実際の利用者を対象にテストが実施されることです。 したがってそうしなければ、行う必要がありますを設定していることを確認 
次の手順の情報に基づいて判断するために必要なすべてのデータを収集するために、flighting 展開します。 このチュートリアルは、データを収集する方法を示します 
Application Insights で New Relic や、シナリオに適したその他のテクノロジを使用することができます。 

## 学習内容

このチュートリアルでは、次のシナリオを組み合わせながら、運用環境の App Service アプリをテストする方法について説明します。

- [実稼働トラフィックをルーティング](app-service-web-test-in-production-get-start.md) ベータ版のアプリへ
- [アプリをインストルメント化](app-insights-web-track-usage.md) 便利なメトリックを取得するには
- ベータ版のアプリを継続的にデプロイし、実際のアプリのメトリックを追跡する
- 運用環境のアプリとベータ版のアプリとでメトリックを比較し、コードに加えた変更の結果を確認する

## 前提条件

-   Azure アカウント
-   A [GitHub](https://github.com/) アカウント
- ダウンロードする visual Studio 2015 -、 [Community edition](https://www.visualstudio.com/en-us/products/visual-studio-express-vs.aspx)します。
-   Git Shell (と共にインストールされる [GitHub for Windows](https://windows.github.com/))-これにより、同じセッションで Git と PowerShell の両方のコマンドを実行します。
-   最新 [Azure PowerShell](https://github.com/Azure/azure-powershell/releases/download/v0.9.8-September2015/azure-powershell.0.9.8.msi) ビット
-   以下の事柄の基礎知識:
    -   [Azure リソース マネージャー](resource-group-overview.md) テンプレートのデプロイ (を参照してください [予測可能な Azure で複雑なアプリケーション展開](app-service-deploy-complex-application-predictably.md))
    -   [Git](http://git-scm.com/documentation)
    -   [PowerShell](https://technet.microsoft.com/library/bb978526.aspx)

> [AZURE.NOTE] Azure アカウントをこのチュートリアルを完了する必要があります。
> + ことができます [Azure アカウントを無料で開く](/pricing/free-trial/?WT.mc_id=A261C142F) -クレジット有料の Azure サービスを使用することができ、アカウントは維持し、使用する無料の Web Apps などの Azure サービスこれらの使用後にもします。
> + 実行できます [Visual Studio のサブスクライバーの特典を有効に](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F) に、Visual Studio のサブスクリプション クレジットにより、毎月提供する Azure の有料サービスを使用することができます。
>
> 場合は、Azure アカウントがサインアップする前に Azure App Service の使用を開始するには、 [App Service の試用](http://go.microsoft.com/fwlink/?LinkId=523751), 、App Service で有効期間の短いスターター web アプリをすぐに作成する場所です。 このサービスの利用にあたり、クレジット カードは必要ありません。契約も必要ありません。

## 運用 Web アプリをセットアップする

>[AZURE.NOTE] このチュートリアルで使用するスクリプトでは、GitHub リポジトリからの継続的な発行を自動的に構成されます。 これを行うには、GitHub 資格情報が既に Azure に保存されている必要があります。保存されていない場合、スクリプト化されたデプロイは、Web アプリに対するソース管理設定を構成しようとした時点で失敗します。
>
>Azure で、GitHub の資格情報を格納する web アプリを作成、 [Azure ポータル](https://portal.azure.com) と [GitHub のデプロイを構成する](web-sites-publish-source-control.md#Step7)です。 この操作を行うのは 1 回だけです。

一般的な DevOps シナリオでは、Azure でライブ実行されているアプリケーションがあり、継続的パブリッシングを通してそれを変更します。 このシナリオでは、開発とテストの済んだテンプレートを運用環境にデプロイします。

1.  自分専用のフォークを作成、 [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) リポジトリです。 フォークの作成方法の詳細については、次を参照してください。 [、リポジトリをフォーク](https://help.github.com/articles/fork-a-repo/)します。 フォークが作成されたら、ブラウザーでそれを確認できます。

    ![](./media/app-service-agile-software-development/production-1-private-repo.png)

2.  Git Shell セッションを開きます。 Git Shell をまだがあるない場合は、インストール [GitHub for Windows](https://windows.github.com/) ようになりました。
3.  次のコマンドを実行して、フォークのローカル クローンを作成します。

        git clone https://github.com/<your_fork>/ToDoApp.git

4.  ローカル クローンを作成したらに移動 *& lt; repository_root >*\ARMTemplates、および次のように、一意のサフィックスを持つ deploy.ps1 スクリプトを実行します。

        .\deploy.ps1 –RepoUrl https://github.com/<your_fork>/todoapp.git -ResourceGroupSuffix <your_suffix>

4.  メッセージが表示されたら、データベースにアクセスするためのユーザー名とパスワードを入力します。 データベースの資格情報は、リソース グループを更新するときにもう一度指定する必要があるため、忘れないように注意してください。

    Azure のさまざまなリソースのプロビジョニングの進行状況が表示されます。 デプロイが完了すると、スクリプトによってアプリケーションがブラウザー内に起動し、わかりやすいビープ音が鳴ります。
    ![](./media/app-service-web-test-in-production-controlled-test-flight/00.1-app-in-browser.png)

6.  Git Shell セッションに戻り、次を実行します。

        .\swap –Name ToDoApp<your_suffix>

    ![](./media/app-service-web-test-in-production-controlled-test-flight/00.2-swap-to-production.png)

7.  スクリプトが完了したら、戻って、フロント エンドのアドレスを参照する (http://ToDoApp*& lt; your_suffix >*.azurewebsites.net/) を実稼働環境で実行されているアプリケーションを参照してください。
5.  ログイン、 [Azure ポータル](https://portal.azure.com) を見て何が作成されたとします。

    同じリソース グループ内に 2 つの Web アプリがあり、1 つは名前に `Api`サフィックスが付いていることを確認できます。 リソース グループ ビューを表示している場合は、SQL Database とサーバー、App Service プラン、および Web アプリのステージング スロットも表示されます。 さまざまなリソースを参照し、それらを比較 *& lt; repository_root >*\ARMTemplates\ProdAndStage.json をテンプレート内の構成方法を確認します。

    ![](./media/app-service-web-test-in-production-controlled-test-flight/00.3-resource-group-view.png)

運用アプリケーションのセットアップは以上で完了です。  ここで、アプリのユーザビリティが低いというフィードバックが寄せられ、 調査することになったとしましょう。 フィードバックを得るためのインストルメントをアプリに実装します。

## 調査: 監視とメトリックに必要なインストルメントをクライアント アプリに実装する

5. 開いている *& lt; repository_root >*Visual Studio で \src\MultiChannelToDo.sln します。
6. ソリューションを右クリックしてすべての Nuget パッケージの復元 > **ソリューションの NuGet パッケージの管理** > **復元**します。
6. 右クリックし **MultiChannelToDo.Web** > **Application Insights テレメトリ追加** > **設定を構成する** > ToDoApp をリソース グループの変更*& lt; your_suffix >* > **Application Insights をプロジェクトに追加**します。
7. Azure ポータルのブレードを表示、 **MultiChannelToDo.Web** Application Insight リソースです。 次に、 **アプリケーションの正常性** パートをクリックして **ブラウザー ページ読み込みのデータを収集する方法について説明** > コードをコピーします。
7. コピーした JS インストルメンテーション コードを追加 *& lt; repository_root >*直前に、\src\MultiChannelToDo.Web\app\Index.cshtml `<heading>` タグ。 Application Insights リソースの一意のインストルメンテーション キーが格納されています。

        <script type="text/javascript">
        var appInsights=window.appInsights||function(config){
            function s(config){t[config]=function(){var i=arguments;t.queue.push(function(){t[config].apply(t,i)})}}var t={config:config},r=document,f=window,e="script",o=r.createElement(e),i,u;for(o.src=config.url||"//az416426.vo.msecnd.net/scripts/a/ai.0.js",r.getElementsByTagName(e)[0].parentNode.appendChild(o),t.cookie=r.cookie,t.queue=[],i=["Event","Exception","Metric","PageView","Trace"];i.length;)s("track"+i.pop());return config.disableExceptionTracking||(i="onerror",s("_"+i),u=f[i],f[i]=function(config,r,f,e,o){var s=u&&u(config,r,f,e,o);return s!==!0&&t["_"+i](config,r,f,e,o),s}),t
        }({
            instrumentationKey:"<your_unique_instrumentation_key>"
        });

        window.appInsights=appInsights;
        appInsights.trackPageView();
        </script>

11. 本体の一番下に次のコードを追加し、マウス クリックのカスタム イベントを Application Insights に送信します。

        <script>
            $(document.body).find("*").click(function(event) {

                appInsights.trackEvent(event.target.tagName + ": " + event.target.className);
            });
        </script>

    ユーザーが Web アプリ内の任意の位置でクリックするとそのたびに、この JavaScript スニペットによって、カスタム イベントが Application Insights に送信されます。

12. Git Shell で、必要な変更を GitHub のフォークにコミットしてプッシュします。 しばらくすると、クライアントによってブラウザーが最新の情報に更新されます。

        git add -A :/
        git commit -m "add AI configuration for client app"
        git push origin master

6.  デプロイしたアプリの変更を運用環境にスワップします。

        .\swap –Name ToDoApp<your_suffix>

13. 構成した Application Insights リソースにアクセスします。 [カスタム イベント] をクリックします。

    ![](./media/app-service-web-test-in-production-controlled-test-flight/01-custom-events.png)

    カスタム イベントのメトリックが表示されない場合、数分間待つし、クリックして **更新**します。

以下のようなグラフが表示されたとします。

![](./media/app-service-web-test-in-production-controlled-test-flight/02-custom-events-chart-view.png)

その下には、次のようなイベント グリッドが表示されます。

![](./media/app-service-web-test-in-production-controlled-test-flight/03-custom-event-grid-view.png)

ToDoApp のアプリケーション コードに従って、 **] ボタンをクリック** 送信] ボタンに対応するイベントと **入力** イベントは、テキスト ボックスに対応します。 ここまでは問題ありません。 ただし、そうは to do 項目のクリックや非常に数回のクリックの適切な量 (、 **LI** イベント)。

このことから現行のフォームでは、UI 上でクリックできる領域が一部のユーザーに伝わっていない可能性が疑われます。また、その原因は、リスト項目とそのアイコンにユーザーがポインタを重ねたときのカーソルの形状がテキスト選択スタイルになっていること、という仮説を立てることができます。

![](./media/app-service-web-test-in-production-controlled-test-flight/04-to-do-list-item-ui.png)

これは極端な例ですが、 それでも、アプリに改良を加えてフライト デプロイを実施し、ユーザビリティに関するフィードバックを実際の利用者から得ることは可能です。

### 監視とメトリックに必要なインストルメントをサーバー アプリに実装する
ここで少し脱線します。このチュートリアルのシナリオで取り上げているのは、クライアント アプリのみですが、 片手落ちにならないようサーバー側のアプリもセットアップしましょう。

6. 右クリックし **MultiChannelToDo** > **Application Insights テレメトリ追加** > **設定を構成する** > ToDoApp をリソース グループの変更*& lt; your_suffix >* > **Application Insights をプロジェクトに追加**します。
12. Git Shell で、必要な変更を GitHub のフォークにコミットしてプッシュします。 しばらくすると、クライアントによってブラウザーが最新の情報に更新されます。

        git add -A :/
        git commit -m "add AI configuration for server app"
        git push origin master

6.  デプロイしたアプリの変更を運用環境にスワップします。

        .\swap –Name ToDoApp<your_suffix>

これで完了です。

## 調査: スロット固有のタグをクライアント アプリのメトリックに追加する

このセクションでは、複数のデプロイ スロットを構成し、スロット固有のテレメトリを同じ Application Insights リソースに送信します。 こうすることで、異なるスロット (デプロイ環境) からのトラフィック間でテレメトリ データを比較できるので、アプリに対する変更の効果が見やすくなります。 同時に、運用アプリについては必要に応じて監視を継続できるよう、運用環境のトラフィックを他のトラフィックから分離することができます。

クライアントの動作のデータを収集するため、 [、JavaScript コードにテレメトリ初期化子を追加](app-insights-api-custom-events-metrics.md#js-initializer) index.cshtml にします。 サーバー側のパフォーマンスをテストする場合は、たとえば、行うことも同様に、サーバー コードで (を参照してください [カスタム イベントおよびメトリックを Application Insights API]((app-insights-api-custom-events-metrics.md))します。

1. まず、先ほど `<heading>` タグに追加した JavaScript ブロックに、以下の 2 つの `//` コメントに挟まれたコードを追加します。

        window.appInsights = appInsights;

        // Begin new code
        appInsights.queue.push(function () {
            appInsights.context.addTelemetryInitializer(function (envelope) {
                var telemetryItem = envelope.data.baseData;
                telemetryItem.properties = telemetryItem.properties || {};
                telemetryItem.properties["Environment"] = "@System.Configuration.ConfigurationManager.AppSettings["environment"]";
            });
        });
        // End new code

        appInsights.trackPageView();

    `appInsights` オブジェクトから送信されるすべてのテレメトリには、この初期化子コードによって、`Environment` というカスタム プロパティが追加されます。

2. 次に、このカスタム プロパティを追加、 [スロット設定](web-sites-staged-publishing.md#AboutConfiguration) Azure で web アプリのです。 そのためには、Git Shell セッションで次のコマンドを実行します。

        $app = Get-AzureWebsite -Name todoapp<your_suffix> -Slot production
        $app.AppSettings.Add("environment", "Production")
        $app.SlotStickyAppSettingNames.Add("environment")
        $app | Set-AzureWebsite -Name todoapp<your_suffix> -Slot production

    プロジェクトに含まれている Web.config には最初から `environment` アプリケーション設定が定義されています。 この設定を用いアプリをローカルでテストしたときは、目的とするメトリックが `VS Debugger` でタグ付けされます。 ただし、開発者がその変更を Azure にプッシュするときには、Web アプリの構成に含まれている方の `environment` アプリケーション設定が検出されて使用されます。また、目的のメトリックには、`Production` でタグ付けされます。

3. コードの変更を GitHub のフォークにコミットしてプッシュし、新しいアプリをユーザーが使うまで待ちます (ブラウザーを最新の情報に更新する必要があります)。 新しいプロパティが Application Insights `MultiChannelToDo.Web` リソースに反映されるまでに約 15 分かかります。

        git add -A :/
        git commit -m "add environment property to AI events for client app"
        git push origin master

4. 移動すれば、 **カスタム イベント** ブレードをもう一度でメトリックをフィルター処理と `Environment=Production`です。 このフィルターによって、運用スロットにおける新しいカスタム イベントをすべて表示できます。

    ![](./media/app-service-web-test-in-production-controlled-test-flight/05-filter-on-production-environment.png)

5. をクリックして、 **お気に入り** のように現在のメトリックス エクスプ ローラーの設定を保存するボタン **カスタム イベント: 実稼働**します。 後でデプロイ スロット ビューとの間で簡単に切り替えることができます。

    > [AZURE.TIP] さらに強力な分析を検討してください。 [Application Insights リソースを Power BI と統合する](app-insights-export-power-bi.md)です。

### スロット固有のタグをサーバー アプリのメトリックに追加する
こちらも片手落ちにならないようサーバー側のアプリもセットアップしましょう。 ただし、JavaScript でインストルメントを実装したクライアント アプリとは異なり、サーバー アプリのスロット固有タグは .NET コードで実装します。

1. 開いている *& lt; repository_root >*\src\MultiChannelToDo\Global.asax.cs します。 以下のコード ブロックを名前空間の右中かっこの直前に追加します。

        namespace MultiChannelToDo
        {
                ...

                // Begin new code
            public class ConfigInitializer
            : ITelemetryInitializer
            {
                void ITelemetryInitializer.Initialize(ITelemetry telemetry)
                {
                    telemetry.Context.Properties["Environment"] = System.Configuration.ConfigurationManager.AppSettings["environment"];
                }
            }
                // End new code
        }

2. 以下の `using` ステートメントをファイルの冒頭に追加して名前解決のエラーを修正します。

        using Microsoft.ApplicationInsights.Channel;
        using Microsoft.ApplicationInsights.Extensibility;

3. `Application_Start()` メソッドの先頭に以下のコードを追加します。

        TelemetryConfiguration.Active.TelemetryInitializers.Add(new ConfigInitializer());

3. コードの変更を GitHub のフォークにコミットしてプッシュし、新しいアプリをユーザーが使うまで待ちます (ブラウザーを最新の情報に更新する必要があります)。 新しいプロパティが Application Insights `MultiChannelToDo` リソースに反映されるまでに約 15 分かかります。

        git add -A :/
        git commit -m "add environment property to AI events for server app"
        git push origin master

## 更新: ベータ ブランチを設定する

2. 開いている *& lt; repository_root >*\ARMTemplates\ProdAndStagetest.json と検索、 `appsettings` リソース (検索 `"name": "appsettings"`)。 全部で 4 つ存在します (スロットごとに 1 つ)。 

2. 各 `appsettings` リソース、追加、  `"environment": "[parameters('slotName')]"` の末尾にアプリケーション設定、 `properties` 配列。 前の行の最後に必ずコンマを付けてください。

    ![](./media/app-service-web-test-in-production-controlled-test-flight/06-arm-app-setting-with-slot-name.png)
    
    テンプレートのすべてのスロットに `environment` アプリ設定を追加しました。
    
2. 同じファイルで、`slotconfignames` リソースを探します (`"name": "slotconfignames"` を検索)。 全部で 2 つ存在します (アプリごとに 1 つ)。

2. 各 `slotconfignames` リソースについて、`appSettingNames` 配列の最後に `"environment"` を追加します。 前の行の最後に必ずコンマを付けてください。

    これで両方のアプリの各デプロイ スロットに `environment` アプリケーション設定が適用されます。  

3. Git Shell セッションから、先ほどと同じリソース グループ サフィックスで次のコマンドを実行します。

        git checkout -b beta
        git push origin beta
        .\deploy.ps1 -RepoUrl https://github.com/<your_fork>/ToDoApp.git -ResourceGroupSuffix <your_suffix> -SlotName beta -Branch beta

4. 確認のメッセージが表示されたら、先ほどと同じ SQL Database の資格情報を指定してください。 次に、リソース グループを更新するよう求められたら、「`Y`」と入力し、`ENTER` キーを押します。

    スクリプトの完了後、元のリソース グループに含まれていたリソースをすべて維持したうえで、最初に作成した "Staging" スロットと同じ構成を持った "beta" という名前のスロットが新たに作成されます。

    >[AZURE.NOTE] さまざまな展開環境を作成するには、このメソッドは、メソッドからさまざまな [Azure App Service でのアジャイル ソフトウェア開発](app-service-agile-software-development.md)します。 ここで作成しているのはデプロイ スロットを含んだデプロイ環境です。リソース グループを含んだデプロイ環境ではありません。 リソース グループによるデプロイ環境を利用した場合、運用環境を開発者から遠ざけることができますが、運用環境でのテストは困難となります。運用環境でのテストは、スロットから成るデプロイ環境の方が簡単です。

必要であれば、次の方法でアルファ版のアプリを作成することもできます。

    git checkout -b alpha
    git push origin alpha
    .\deploy.ps1 -RepoUrl https://github.com/<your_fork>/ToDoApp.git -ResourceGroupSuffix <your_suffix> -SlotName beta -Branch alpha

このチュートリアルでは、このままベータ版アプリを使用することにします。

## 更新: 更新をベータ版アプリにプッシュする

改良の対象となるアプリに戻ります。

1. 現在位置がベータ ブランチであることを確認します。

        git checkout beta

2.  *& Lt; repository_root >*\src\MultiChannelToDo.Web\app\Index.cshtml、検索、 `<li>` タグを追加、 `style="cursor:pointer"` 属性は、次のようにします。

    ![](./media/app-service-web-test-in-production-controlled-test-flight/07-change-cursor-style-on-li.png)

3. Azure にコミットしてプッシュします。

4. Http://todoapp に移動して、ベータ版のスロットに変更を今すぐ反映されることを確認*& lt; your_suffix >*-beta.azurewebsites.net/です。 まだ変更が反映されていない場合は、ブラウザーを最新の情報に更新して新しい JavaScript コードを取得してください。

    ![](./media/app-service-web-test-in-production-controlled-test-flight/08-verify-change-in-beta-site.png)

変更内容がベータ スロットで実行されていることを確認できたら、フライト デプロイを実行することができます。

## 検証: ベータ版アプリにトラフィックをルーティングする

このセクションでは、ベータ版のアプリにトラフィックをルーティングします。 デモンストレーションを単純化するために、ここではユーザー トラフィックの大部分をベータ版アプリにルーティングします。 実際には、ルーティングするトラフィックの量は、具体的な条件によって異なります。 たとえば、microsoft.com と同等の規模があるサイトでは、トラフィック全体の 1% 未満でも、有益なデータを集めることが可能です。

1. 運用環境のトラフィックをベータ スロットにルーティングするには、Git Shell セッションから次のコマンドを実行します。

        $siteName = "ToDoApp<your suffix>"
        $rule = New-Object Microsoft.WindowsAzure.Commands.Utilities.Websites.Services.WebEntities.RampUpRule
        $rule.ActionHostName = "$siteName-beta.azurewebsites.net"
        $rule.ReroutePercentage = 50
        $rule.Name = "beta"
        Set-AzureWebsite $siteName -Slot Production -RoutingRules $rule

  運用環境のトラフィックの 50% をベータ版アプリの URL (`ActionHostName` プロパティで指定) にルーティングするよう `ReroutePercentage=50` プロパティで指定しています。

2. Http://ToDoApp に移動して*& lt; your_suffix >*. azurewebsites.net します。 50% のトラフィックがベータ スロットにリダイレクトされているはずです。

3. Application Insights リソースで、environment="beta" としてメトリックをフィルタリングします。

    > [AZURE.NOTE] このフィルター処理されたビューを別のお気に入りとして保存する場合、運用とベータ版のビューのメトリックス エクスプ ローラー ビュー簡単に反転することができます。

Application Insights で、次のような画面が表示されているとします。

![](./media/app-service-web-test-in-production-controlled-test-flight/09-test-beta-site-in-production.png)

`<li>` タグは他のタグに比べてクリック数が多いだけでなく、先ほどと比べても、`<li>` タグのクリック数は大幅に増えていることがわかります。 このことから、`<li>` タグを更新したことで、クリック可能なタグであることにユーザーが気付き、既に完了したタスクをアプリから消去し始めた、と考えられます。

フライト デプロイのデータから、新しい UI は運用環境での利用に耐えると判断することができます。

## 本番稼働: 新しいコードを運用環境に移す

以上で、更新したコードを運用環境に移動する準備は整いました。 優れた新機能は、更新プログラムが既に検証されていることがわかっている _する前に_ を運用環境にプッシュされます。 不安を抱えずにデプロイすることができます。 更新の対象が AngularJS クライアント アプリであったため、今回検証したのはクライアント側のコードだけです。 仮にバックエンドの Web API アプリに変更を加えた場合、同じようにして簡単に変更内容を検証することができます。

1. Git Shell から次のコマンドを実行してトラフィック ルーティング ルールを削除します。

        Set-AzureWebsite $siteName -Slot Production -RoutingRules @()

2. 次の Git コマンドを実行します。

        git checkout master
        git pull origin master
        git merge beta
        git push origin master

2. 新しいコードをステージング スロットに配置するのには数分待ってから、起動 http://ToDoApp*& lt; your_suffix >*-staging.azurewebsites.net ステージング スロットで新しい更新プログラムしたことを確認します。 フォークの master ブランチはアプリのステージング スロットにリンクされていることに注意してください。

3. ここで、ステージング スロットを運用スロットにスワップします。

        cd <ROOT>\ToDoApp\ARMTemplates
        .\swap.ps1 -Name todoapp<your_suffix>

## 概要 ##

Azure App Service を使用すると、中小規模の企業が顧客向けのアプリを運用環境で簡単にテストすることができます。従来そのようなテストを実施できるのは大企業だけでした。 このチュートリアルでは、App Service と Application Insights との統合によるフライト デプロイを中心に、DevOps 手法で運用環境下のテストを実現するために必要な情報を紹介しました。この情報が皆様のお役に立てればさいわいです。 

## その他のリソース ##

-   [Azure App Service を使用したアジャイル ソフトウェア開発](app-service-agile-software-development.md)
-   [Azure App Service の Web アプリのステージング環境を設定する](web-sites-staged-publishing.md)
-   [Azure で複雑なアプリケーションを予測どおりにデプロイする](app-service-deploy-complex-application-predictably.md)
-   [Azure リソース マネージャーのテンプレートの作成](resource-group-authoring-templates.md)
-   [JSONLint - JSON Validator に関するページ](http://jsonlint.com/)
-   [Git のブランチ機能 - 基本的なブランチとマージに関するページ](http://www.git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging)
-   [Azure PowerShell](powershell-install-configure.md)
-   [Project Kudu Wiki](https://github.com/projectkudu/kudu/wiki)

