<properties 
    pageTitle="Application Insights for Windows desktop apps and services" 
    description="Application Insights を使用して Windows デスクトップ アプリの使用状況とパフォーマンスを分析します。" 
    services="application-insights" 
    documentationCenter="windows"
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="11/05/2015" 
    ms.author="awills"/>

# Windows デスクトップ アプリ、サービス、worker ロールに対する Application Insights

*Application Insights はプレビュー段階です。*

[AZURE.INCLUDE [app-insights-selector-get-started](../../includes/app-insights-selector-get-started.md)]

Application Insights を使用すると、デプロイしたアプリケーションの使用状況とパフォーマンスを監視できます。

デスクトップ アプリ、バックグラウンド サービス、worker ロールなどの Windows アプリケーションはすべて、Application Insights SDK を使用して、Application Insights にテレメトリを送信します。 クラス ライブラリ プロジェクトに Application Insights SDK を追加することもできます。

(たとえば、パフォーマンス カウンターまたは依存関係の呼出しを監視するために) どの標準データ コレクターを選択するか、単にコア API を使用して、独自のテレメトリを記述できます。


## <a name="add"></a> Application Insights リソースの作成


1.   [Azure ポータル][portal], 、新しい Application Insights リソースを作成します。 アプリケーションの種類には、Windows ストア アプリを選択します。 

    ![[新規]、[Application Insights] の順にクリックする](./media/app-insights-windows-desktop/01-new.png)

    (アプリケーションの種類の選択の概要ブレードで使用できるプロパティの内容を設定する [メトリックス エクスプ ローラー][metrics].)

2.  インストルメンテーション キーをコピーします。

    ![[プロパティ] をクリックし、キーを選択して、Ctrl キーを押しながら C キーを押す](./media/app-insights-windows-desktop/02-props.png)

## <a name="sdk"></a>アプリケーションに SDK をインストールする


1. Visual Studio で、デスクトップ アプリ プロジェクトの NuGet パッケージを編集します。

    ![プロジェクトを右クリックし、[Nuget パッケージの管理] を選択する](./media/app-insights-windows-desktop/03-nuget.png)

2. Application Insights Windows Server パッケージである Microsoft.ApplicationInsights.WindowsServer をインストールします。

    !["Application Insights" の検索](./media/app-insights-windows-desktop/04-ai-nuget.png)

    *他のパッケージを使用することができますか。*

    はい。 独自のテレメトリを送信するためだけに API を使用する場合は、コア API (Microsoft.ApplicationInsights) を選択してください。 Windows Server パッケージには、コア API に加え、他にも多くのパッケージ (パフォーマンス カウンターや依存関係の監視など) が自動的に含まれます。 

    (ただし、Microsoft.ApplicationInsights.Windows は使用しないでください。これは、Windows ストア アプリ向けです。)

3. InstrumentationKey を設定します。

    * コア API パッケージ Microsoft.ApplicationInsights のみをインストールする場合は、コードにキーを設定する必要があります。たとえば、main() で次のように設定します。 

    `TelemetryConfiguration.Active.InstrumentationKey = "` *キー* `";` 

    その他のパッケージの 1 つをインストールした場合は、コードを使用してキーを設定するか、ApplicationInsights.config 内でキーを設定します。
 
    `<InstrumentationKey>`*実際のキー*`</InstrumentationKey>` 

    ApplicationInsights.config を使用する場合を確認してくださいソリューション エクスプ ローラーでそのプロパティに設定されます **ビルド アクション = コンテンツ、出力ディレクトリにコピーのコピーを =**です。

## <a name="telemetry"></a>利用統計情報呼び出しを挿入する

作成、 `TelemetryClient` インスタンスし [テレメトリを送信する使用][api]します。


たとえば、Windows フォーム アプリケーションでは、次のように記述します。

```C#

    public partial class Form1 : Form
    {
        private TelemetryClient tc = new TelemetryClient();
        ...
        private void Form1_Load(object sender, EventArgs e)
        {
            // Alternative to setting ikey in config file:
            tc.InstrumentationKey = "key copied from portal";

            // Set session data:
            tc.Context.User.Id = Environment.GetUserName();
            tc.Context.Session.Id = Guid.NewGuid().ToString();
            tc.Context.Device.OperatingSystem = Environment.OSVersion.ToString();

            // Log a page view:
            tc.TrackPageView("Form1");
            ...
        }

        protected override void OnClosing(CancelEventArgs e)
        {
            stop = true;
            if (tc != null)
            {
                tc.Flush(); // only for desktop apps

                // Allow time for flushing:
                System.Threading.Thread.Sleep(1000);
            }
            base.OnClosing(e);
        }

```

いずれかを使用して、 [Application Insights API][api] テレメトリを送信します。 コア API を使用している場合、テレメトリは自動的に送信されません。 通常は次のものを使用します。

* `TrackPageView(pageName)` は、フォーム、ページ、またはタブを切り替えるために使用する。
* `TrackEvent(eventName)` は、他のユーザー アクションに使用する。
* `TrackMetric(name, value)` は、特定のイベントにアタッチされていないメトリックスの定期的なレポートを送信する場合にバックグラウンド タスクで使用する。
* `TrackTrace(logEvent)`  [診断ログの記録][diagnostic]
* `TrackException(exception)` は catch 句に使用する。
* `Flush()` は、アプリを終了する前にすべてのテレメトリを確実に送信するために使用する。 これは、コア API (Microsoft.ApplicationInsights) を使用する場合に限られます。 Web SDK では、この動作が自動的に実装されます (アプリは、インターネットが常に利用可能なコンテキストで実行する場合も参照してください [永続化チャネル](#persistence-channel).)


#### コンテキストの初期化子

ユーザーとセッションの数を表示するために、各 `TelemetryClient` インスタンスに値を設定できます。 または、コンテキストの初期化子を使用して、この加算をすべてのクライアントに実行できます。

```C#

    class UserSessionInitializer: IContextInitializer
    {
        public void Initialize(TelemetryContext context)
        {
            context.User.Id = Environment.UserName;
            context.Session.Id = Guid.NewGuid().ToString();
        }
    }

    static class Program
    {
        ...
        static void Main()
        {
            TelemetryConfiguration.Active.ContextInitializers.Add(
                new UserSessionInitializer());
            ...

```



## <a name="run"></a>プロジェクトの実行

[F5 キーを押してアプリケーションを実行](http://msdn.microsoft.com/library/windows/apps/bg161304.aspx) し、これを使用して、テレメトリを生成します。 

Visual Studio で、送信されたイベント数が表示されます。

![](./media/app-insights-windows-desktop/appinsights-09eventcount.png)



## <a name="monitor"></a>監視データの表示

Azure ポータルで、アプリケーションのブレードに戻ります。

最初のイベントが表示 [診断検索](app-insights-diagnostic-search.md)します。 

大量のデータが予想される場合は、数秒後に [最新の情報に更新] をクリックします。

TrackMetric、または TrackEvent の測定値パラメーターを使用している場合は開きます [メトリック エクスプ ローラー][metrics] し、[フィルター] ブレードを開きます。 メトリックが表示されますが、パイプラインを通過するのに時間がかかることがあるため、場合によっては [フィルター] ブレードを閉じてしばらく待ってから更新する必要があります。



## 永続化チャネル 

インターネット接続が常に利用できるとは限らない状況または速度が遅い状況でアプリが実行される場合は、既定のメモリ内チャネルの代わりに永続化チャネルの使用を検討してください。 

既定のメモリ内チャネルでは、アプリが終了するまでに送信されなかったテレメトリは失われます。 `Flush()` を使用してバッファーに残っているデータを送信できますが、インターネット接続がない場合、または送信が完了する前にアプリがシャットダウンした場合、データは失われます。

これに対し、永続化チャネルは、ポータルに送信する前に、ファイルで製品利用統計情報をバッファーします。 `Flush()` により、データがファイルに格納されていること。 アプリが終了するまでに送信されなかったデータはファイル内に保持されます。 アプリを再起動したときにインターネット接続がある場合、そのデータは送信されます。 接続が利用可能になるまでは、データは、必要である期間だけファイル内に蓄積されます。 

### 永続化チャネルを使用するには

1. NuGet パッケージをインポート [Microsoft.ApplicationInsights.PersistenceChannel](https://www.nuget.org/packages/Microsoft.ApplicationInsights.PersistenceChannel)します。
2. アプリの適切な初期化の場所に、このコードを含めます。
 
    ```C# 

      using Microsoft.ApplicationInsights.Channel;
      using Microsoft.ApplicationInsights.Extensibility;
      ...

      // Set up 
      TelemetryConfiguration.Active.InstrumentationKey = "YOUR INSTRUMENTATION KEY";
 
      TelemetryConfiguration.Active.TelemetryChannel = new PersistenceChannel();
    
    ``` 
3. アプリを終了する前に `telemetryClient.Flush()` を使用して、確実にデータがポータルに送信されるか、ファイルに保存されるようにします。

    Flush() は永続化チャネルでは同期ですが、その他のチャネルでは非同期であることに注意してください。 

 
永続化チャネルは、アプリケーションで作成されるイベントの数が比較的少なく、接続の信頼性が低いことの多いデバイス シナリオに対して最適化されています。 このチャネルは、イベントを最初にディスクに書き込んで確実に保存してから、送信を試行します。 

#### 例

未処理の例外を監視するとします。 `UnhandledException` イベントをサブスクライブします。 コールバックでは、利用統計情報を永続化するかどうかを確認するには、フラッシュする呼び出しが含まれます。
 
```C# 

AppDomain.CurrentDomain.UnhandledException += CurrentDomain_UnhandledException; 
 
... 
 
private void CurrentDomain_UnhandledException(object sender, UnhandledExceptionEventArgs e) 
{ 
    ExceptionTelemetry excTelemetry = new ExceptionTelemetry((Exception)e.ExceptionObject); 
    excTelemetry.SeverityLevel = SeverityLevel.Critical; 
    excTelemetry.HandledAt = ExceptionHandledAt.Unhandled; 
 
    telemetryClient.TrackException(excTelemetry); 
 
    telemetryClient.Flush(); 
} 

``` 

アプリをシャットダウンすると、`%LocalAppData%\Microsoft\ApplicationInsights\` にファイルが作成され、圧縮されたイベントが格納されます。 
 
このアプリケーションを次回起動したときに、チャネルがこのファイルを取得し、可能な場合はテレメトリを Application Insights に配信します。

#### テストの例

```C#

using Microsoft.ApplicationInsights;
using Microsoft.ApplicationInsights.Channel;
using Microsoft.ApplicationInsights.Extensibility;

namespace ConsoleApplication1
{
    class Program
    {
        static void Main(string[] args)
        {
            // Send data from the last time the app ran
            System.Threading.Thread.Sleep(5 * 1000);

            // Set up persistence channel

            TelemetryConfiguration.Active.InstrumentationKey = "YOUR KEY";
            TelemetryConfiguration.Active.TelemetryChannel = new PersistenceChannel();

            // Send some data

            var telemetry = new TelemetryClient();

            for (var i = 0; i < 100; i++)
            {
                var e1 = new Microsoft.ApplicationInsights.DataContracts.EventTelemetry("persistenceTest");
                e1.Properties["i"] = "" + i;
                telemetry.TrackEvent(e1);
            }

            // Make sure it's persisted before we close
            telemetry.Flush();
        }
    }
}

```


永続化チャネルのコードは [github](https://github.com/Microsoft/ApplicationInsights-dotnet/tree/master/src/TelemetryChannels/PersistenceChannel)します。 


## <a name="usage"></a>次のステップ

[アプリの使用状況の追跡][knowUsers]

[診断ログのキャプチャと検索][diagnostic]

[トラブルシューティング][qna]




<!--Link references-->

[diagnostic]: app-insights-diagnostic-search.md
[metrics]: app-insights-metrics-explorer.md
[portal]: http://portal.azure.com/
[qna]: app-insights-troubleshoot-faq.md
[knowUsers]: app-insights-overview-usage.md
[api]: app-insights-api-custom-events-metrics.md
[CoreNuGet]: https://www.nuget.org/packages/Microsoft.ApplicationInsights
 

