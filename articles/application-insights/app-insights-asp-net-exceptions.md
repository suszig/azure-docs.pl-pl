<properties 
    pageTitle="Application Insights を利用し、ASP.NET アプリの障害と例外を診断する" 
    description="要求テレメトリと共に ASP.NET アプリから例外を取り込みます。" 
    services="application-insights" 
    documentationCenter=".net"
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="11/17/2015" 
    ms.author="awills"/>


# Application Insights の設定: 例外の診断

[AZURE.INCLUDE [app-insights-selector-get-started-dotnet](../../includes/app-insights-selector-get-started-dotnet.md)]


使用してアプリケーションを監視することによって [Visual Studio Application Insights][start], 、原因をすばやく診断できるように、例外と他のイベントには、クライアントとサーバーの両方で失敗した要求を関連付けることができます。

ASP.NET アプリケーションを監視するには、する必要がある [Application Insights SDK を追加][greenbrown] そのため、アプリケーションまたは [、IIS サーバーに Status Monitor をインストール][redfield], 、または、アプリが Azure の Web アプリの場合は、追加、 [Application Insights Extension][azure]します。

## 障害の診断 

概要ブレードの [障害] タイルには、例外と失敗した HTTP 要求のグラフ、最も頻繁に発生する障害の原因となる要求 URL の一覧が表示されます。

![障害の選択](./media/app-insights-asp-net-exceptions/012-start.png)

一覧にある失敗した要求の種類を 1 つクリックし、障害の個別発生を表示します。 そこからさらにクリックし、例外またはそれに関連付けられているトレース データを表示します。

![失敗した要求のインスタンスを選択し、例外の詳細で、例外のインスタンスを表示します。](./media/app-insights-asp-net-exceptions/030-req-drill.png)


**または、** 詳しくは、障害] ブレードが表示されている例外の一覧から開始することができます。 個別の例外が表示されるまでクリックしてください。


![ドリル スルー](./media/app-insights-asp-net-exceptions/040-exception-drill.png)

*例外が表示されませんか。 参照してください [例外をキャプチャ](#exceptions)します。*

そこからスタック トレースと各例外の詳細プロパティを確認し、関連するログのトレースまたはその他のイベントを検索できます。


![ドリル スルー](./media/app-insights-asp-net-exceptions/050-exception-properties.png)

[診断検索の詳細について][diagnostic]します。

## 依存関係の障害

A *依存関係* サービス、アプリケーションが通常を介して呼び出す REST API またはデータベースの接続です。 [Application Insights Status Monitor][redfield] 自動的にさまざまな種類の依存関係の呼び出し、呼び出し期間と成功または失敗を測定を監視します。 

依存関係のデータを取得する必要がある [Status Monitor をインストール][redfield] IIS サーバーで、使用して、アプリが Azure の Web アプリの場合、または、 [Application Insights Extension][azure]します。 

依存関係の呼び出し失敗は [障害] ブレードに一覧表示されます。要求詳細と例外詳細の [関連項目] の下にもあります。

*依存関係の障害がありませんか。 それは良かったです。 依存関係のデータを取得していることを確認するには、パフォーマンス ブレードを開き、依存関係の期間] グラフします。*

 

## カスタムのトレースとログ データ

アプリに固有の診断データを取得するには、独自のテレメトリ データを送信するコードを挿入します。 これは、要求、ページ ビュー、およびその他の自動収集されたデータとともに、診断検索に表示されます。 

いくつかのオプションがあります。

* [TrackEvent()](app-insights-api-custom-events-metrics.md#track-event) 使用パターンを監視するために使われますが、診断検索] で [カスタム イベントも送信するデータが表示されます。 イベントはという名前し、文字列のプロパティや数値のメトリックをすることができますを伝達できます [診断検索の結果をフィルター処理][diagnostic]します。
* [TrackTrace()](app-insights-api-custom-events-metrics.md#track-trace) POST 情報などの長いデータを送信することができます。
* [TrackException()](#exceptions) スタック トレースを送信します。 [例外に関する詳細](#exceptions)します。
* 既に Log4Net、NLog などのログ記録フレームワークを使用する場合は、 [これらのログをキャプチャ][netlogs] は診断検索の要求や例外データの横に表示します。

これらのイベントを表示する [検索][diagnostic], フィルターを開き、カスタム イベント、トレース、または例外を選択します。


![ドリル スルー](./media/app-insights-asp-net-exceptions/viewCustomEvents.png)


> [AZURE.NOTE] アプリでは、多数の製品利用統計情報を生成する場合アダプティブ サンプリング モジュールは自動的にイベントの代表的な分数のみを送信することによって、ポータルに送信される量を削減します。 同じ操作に含まれるイベントは、グループ単位で選択または選択解除されるので、関連するイベントごとに操作できます。 [サンプリングについて説明します。](app-insights-sampling.md)

### 要求の POST データを表示する方法

要求詳細では、POST 呼び出しでアプリに送信されたデータは含まれません。 このデータを報告するには:

* [SDK のインストール][greenbrown] アプリケーション プロジェクトにします。
* 呼び出すアプリケーションにコードを挿入 [Microsoft.ApplicationInsights.TrackTrace()][api]します。 メッセージ パラメーターで POST データを送信します。 許可されるサイズには制限があります。そのため、必要不可欠なデータだけを送信するように努めてください。
* 失敗した要求を調査するときは、関連付けられているトレースを検索します。  

![ドリル スルー](./media/app-insights-asp-net-exceptions/060-req-related.png)


## <a name="exceptions"></a> 例外と関連する診断データのキャプチャ

最初、ポータルにはアプリの障害の原因となる例外の一部しか表示されません。 任意のブラウザーの例外が表示されます (を使用している場合、 [JavaScript SDK][client] web ページで)。 ただし、ほとんどのサーバー例外は IIS によりキャッチされます。それを確認するには、簡単なコードを記述する必要があります。

そのための方法は次のとおりです。

* **例外を明示的に記録** 、例外を報告する例外ハンドラーにコードを挿入します。
* **例外を自動的にキャプチャ** ASP.NET フレームワークを構成しています。 追加しなければならないものはフレームワークの種類によって異なります。

## 例外を明示的に報告する

最も簡単な方法として、例外ハンドラーに TrackException() の呼び出しを挿入します。

JavaScript

    try 
    { ...
    }
    catch (ex)
    {
      appInsights.TrackException(ex, "handler loc",
        {Game: currentGame.Name, 
         State: currentGame.State.ToString()});
    }

C#

    var telemetry = new TelemetryClient();
    ...
    try 
    { ...
    }
    catch (Exception ex)
    {
       // Set up some properties:
       var properties = new Dictionary <string, string> 
         {{"Game", currentGame.Name}};

       var measurements = new Dictionary <string, double>
         {{"Users", currentGame.Users.Count}};

       // Send the exception telemetry:
       telemetry.TrackException(ex, properties, measurements);
    }

VB

    Dim telemetry = New TelemetryClient
    ...
    Try
      ...
    Catch ex as Exception
      ' Set up some properties:
      Dim properties = New Dictionary (Of String, String)
      properties.Add("Game", currentGame.Name)

      Dim measurements = New Dictionary (Of String, Double)
      measurements.Add("Users", currentGame.Users.Count)
  
      ' Send the exception telemetry:
      telemetry.TrackException(ex, properties, measurements)
    End Try

プロパティと測定のパラメーターはオプションですが適しています [フィルター処理と追加][diagnostic] 余分な情報です。 たとえば、複数のゲームを実行できるアプリケーションを使用している場合、1 つのゲームに関連する例外レポートをすべて検索できます。 必要な数だけ項目を各辞書に追加できます。

## ブラウザーの例外

ほとんどのブラウザー例外が報告されます。

Web ページには、コンテンツ配信ネットワークまたはその他のドメインからのスクリプト ファイルが含まれている場合は、スクリプト タグに属性を確認 ```crossorigin="anonymous"```,  、およびサーバーから送られた [CORS ヘッダー](http://enable-cors.org/)します。 これで、これらのリソースから、未処理の JavaScript 例外のスタック トレースと詳細が取得できます。

## Web フォーム

Web フォームの場合、HTTP モジュールは、CustomErrors で構成されたリダイレクトがないとき、例外を回収できます。

ただし、アクティブなリダイレクトがある場合、次の行を Global.asax.cs の Application_Error 関数に追加します。 (Global.asax ファイルがない場合、それを追加します。)

*C#*

    void Application_Error(object sender, EventArgs e)
    {
      if (HttpContext.Current.IsCustomErrorEnabled && Server.GetLastError  () != null)
      {
         var ai = new TelemetryClient(); // or re-use an existing instance

         ai.TrackException(Server.GetLastError());
      }
    }


## MVC

場合、 [CustomErrors](https://msdn.microsoft.com/library/h0hfz6fc.aspx) 構成が `Off`, 、例外が可能になり、 [HTTP モジュール](https://msdn.microsoft.com/library/ms178468.aspx) を収集します。 ただし、`RemoteOnly` (既定) または `On` の場合、例外は消去され、Application Insights が自動回収する例外はなくなります。 で解消できる、 [System.Web.Mvc.HandleErrorAttribute クラス](http://msdn.microsoft.com/library/system.web.mvc.handleerrorattribute.aspx), を異なる MVC バージョン以下のようにオーバーライドされたクラスを適用して ([github ソース](https://github.com/AppInsightsSamples/Mvc2UnhandledExceptions/blob/master/MVC2App/Controllers/AiHandleErrorAttribute.cs))。

    using System;
    using System.Web.Mvc;
    using Microsoft.ApplicationInsights;

    namespace MVC2App.Controllers
    {
      [AttributeUsage(AttributeTargets.Class | AttributeTargets.Method, Inherited = true, AllowMultiple = true)] 
      public class AiHandleErrorAttribute : HandleErrorAttribute
      {
        public override void OnException(ExceptionContext filterContext)
        {
            if (filterContext != null && filterContext.HttpContext != null && filterContext.Exception != null)
            {
                //If customError is Off, then AI HTTPModule will report the exception
                if (filterContext.HttpContext.IsCustomErrorEnabled)
                {   //or reuse instance (recommended!). see note above  
                    var ai = new TelemetryClient();
                    ai.TrackException(filterContext.Exception);
                } 
            }
            base.OnException(filterContext);
        }
      }
    }

#### MVC 2

HandleError 属性をコントローラーの新しい属性で置換します。

    namespace MVC2App.Controllers
    {
       [AiHandleError]
       public class HomeController : Controller
       {
    ...

[サンプル](https://github.com/AppInsightsSamples/Mvc2UnhandledExceptions)

#### MVC 3

Global.asax.cs で `AiHandleErrorAttribute` をグローバル フィルターとして登録します。

    public class MyMvcApplication : System.Web.HttpApplication
    {
      public static void RegisterGlobalFilters(GlobalFilterCollection filters)
      {
         filters.Add(new AiHandleErrorAttribute());
      }
     ...

[サンプル](https://github.com/AppInsightsSamples/Mvc3UnhandledExceptionTelemetry)



#### MVC 4、MVC5

FilterConfig.cs で AiHandleErrorAttribute をグローバル フィルターとして登録します。

    public class FilterConfig
    {
      public static void RegisterGlobalFilters(GlobalFilterCollection filters)
      {
        // Default replaced with the override to track unhandled exceptions
        filters.Add(new AiHandleErrorAttribute());
      }
    }

[サンプル](https://github.com/AppInsightsSamples/Mvc5UnhandledExceptionTelemetry)

## Web API 1.x


System.Web.Http.Filters.ExceptionFilterAttribute を上書きします。

    using System.Web.Http.Filters;
    using Microsoft.ApplicationInsights;

    namespace WebAPI.App_Start
    {
      public class AiExceptionFilterAttribute : ExceptionFilterAttribute
      {
        public override void OnException(HttpActionExecutedContext actionExecutedContext)
        {
            if (actionExecutedContext != null && actionExecutedContext.Exception != null)
            {  //or reuse instance (recommended!). see note above 
                var ai = new TelemetryClient();
                ai.TrackException(actionExecutedContext.Exception);    
            }
            base.OnException(actionExecutedContext);
        }
      }
    }

この上書きされた属性を特定のコントローラーに追加するか、WebApiConfig クラスのグローバル フィルター構成に追加できます。 

    using System.Web.Http;
    using WebApi1.x.App_Start;

    namespace WebApi1.x
    {
      public static class WebApiConfig
      {
        public static void Register(HttpConfiguration config)
        {
            config.Routes.MapHttpRoute(name: "DefaultApi", routeTemplate: "api/{controller}/{id}",
                defaults: new { id = RouteParameter.Optional });
            ...
            config.EnableSystemDiagnosticsTracing();

            // Capture exceptions for Application Insights:
            config.Filters.Add(new AiExceptionFilterAttribute());
        }
      }
    }

[サンプル](https://github.com/AppInsightsSamples/WebApi_1.x_UnhandledExceptions)

例外フィルターが処理できないケースがあります。 次に例を示します。

* コントローラー コンストラクターからスローされる例外。 
* メッセージ ハンドラーからスローされる例外。 
* ルーティング中にスローされる例外。 
* 応答コンテンツのシリアル化中にスローされる例外。 

## Web API 2.x

IExceptionLogger の実装を追加します。

    using System.Web.Http.ExceptionHandling;
    using Microsoft.ApplicationInsights;

    namespace ProductsAppPureWebAPI.App_Start
    {
      public class AiExceptionLogger : ExceptionLogger
      {
        public override void Log(ExceptionLoggerContext context)
        {
            if (context !=null && context.Exception != null)
            {//or reuse instance (recommended!). see note above 
                var ai = new TelemetryClient();
                ai.TrackException(context.Exception);
            }
            base.Log(context);
        }
      }
    }

これを WebApiConfig のサービスに追加します。

    using System.Web.Http;
    using System.Web.Http.ExceptionHandling;
    using ProductsAppPureWebAPI.App_Start;

    namespace WebApi2WithMVC
    {
      public static class WebApiConfig
      {
        public static void Register(HttpConfiguration config)
        {
            // Web API configuration and services

            // Web API routes
            config.MapHttpAttributeRoutes();

            config.Routes.MapHttpRoute(
                name: "DefaultApi",
                routeTemplate: "api/{controller}/{id}",
                defaults: new { id = RouteParameter.Optional }
            );
            config.Services.Add(typeof(IExceptionLogger), new AiExceptionLogger()); 
        }
      }
  }

[サンプル](https://github.com/AppInsightsSamples/WebApi_2.x_UnhandledExceptions)

代替として、次のように操作できます。

2. 唯一の ExceptionHandler を IExceptionHandler のカスタム実装で置換します。 これはフレームワークが送信する応答メッセージを選択できるときにのみ呼び出されます (たとえば、接続が中止されるときではなく)。 
3. 例外フィルター (上の Web API 1.x コントローラーのセクション参照) - 場合によっては呼び出されません。


## WCF

Attribute を拡張し、IErrorHandler と IServiceBehavior を実装するクラスを追加します。

    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.ServiceModel.Description;
    using System.ServiceModel.Dispatcher;
    using System.Web;
    using Microsoft.ApplicationInsights;

    namespace WcfService4.ErrorHandling
    {
      public class AiLogExceptionAttribute : Attribute, IErrorHandler, IServiceBehavior
      {
        public void AddBindingParameters(ServiceDescription serviceDescription,
            System.ServiceModel.ServiceHostBase serviceHostBase,
            System.Collections.ObjectModel.Collection<ServiceEndpoint> endpoints,
            System.ServiceModel.Channels.BindingParameterCollection bindingParameters)
        {
        }

        public void ApplyDispatchBehavior(ServiceDescription serviceDescription, 
            System.ServiceModel.ServiceHostBase serviceHostBase)
        {
            foreach (ChannelDispatcher disp in serviceHostBase.ChannelDispatchers)
            {
                disp.ErrorHandlers.Add(this);
            }
        }

        public void Validate(ServiceDescription serviceDescription, 
            System.ServiceModel.ServiceHostBase serviceHostBase)
        {
        }

        bool IErrorHandler.HandleError(Exception error)
        {//or reuse instance (recommended!). see note above 
            var ai = new TelemetryClient();

            ai.TrackException(error);
            return false;
        }

        void IErrorHandler.ProvideFault(Exception error, 
            System.ServiceModel.Channels.MessageVersion version, 
            ref System.ServiceModel.Channels.Message fault)
        {
        }
      }
    }

サービス実装に属性を追加します。

    namespace WcfService4
    {
        [AiLogException]
        public class Service1 : IService1 
        { 
         ...

[サンプル](https://github.com/AppInsightsSamples/WCFUnhandledExceptions)

## 例外パフォーマンス カウンター

あれば [Status Monitor をインストールされている][redfield] サーバーで、.NET で計測された例外レートのグラフを表示できます。 これには、処理済みの .NET 例外と未処理の .NET 例外の両方が含まれます。

メトリック エクスプ ローラーのブレードを開き、新しいグラフを追加および選択 **例外レート**, 、パフォーマンス カウンターの下にリストされています。 

.NET フレームワークでは、特定の時間間隔で例外数をカウントし、それを時間間隔の長さで割り算することで、例外レートを算出します。 

この値は、TrackException レポートをカウントすることにより、Application Insights ポータルで算出される「例外」数とは異なります。 サンプリングの時間間隔が異なります。さらに、SDK では、すべての処理済みの例外と未処理の例外について TrackException レポートを送信するわけではありません。

<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[azure]: ../insights-perf-analytics.md
[client]: app-insights-javascript.md
[diagnostic]: app-insights-diagnostic-search.md
[greenbrown]: app-insights-asp-net.md
[netlogs]: app-insights-asp-net-trace-logs.md
[redfield]: app-insights-monitor-performance-live-website-now.md
[start]: app-insights-overview.md

 
