<properties 
    pageTitle="Windows Phone Silverlight Engagement SDK 統合" 
    description="Windows Phone Silverlight アプリと Azure Mobile Engagement を統合する方法"                  
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="dwrede" 
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-windows-phone" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="07/07/2015" 
    ms.author="piyushjo" />


# Windows Phone Silverlight Engagement SDK 統合

> [AZURE.SELECTOR] 
- [Windows Universal](mobile-engagement-windows-store-integrate-engagement.md) 
- [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md) 
- [iOS](mobile-engagement-ios-integrate-engagement.md) 
- [Android](mobile-engagement-android-integrate-engagement.md) 


ここでは、Windows Phone Silverlight アプリケーションで Azure  Mobile Engagement の分析やモニタリング機能を有効にする簡単な方法について説明します。

次の手順は、ユーザー、セッション、アクティビティ、クラッシュ、テクニカルに関するすべての統計情報をコンピューティングするのに必要なログのレポートを有効にするためのものです。 イベント、エラー、ジョブなどその他の統計の計算に必要なログのレポートは、エンゲージメント API を使用して手動で行う必要があります (を参照してください [、高度なモバイル エンゲージメント Windows Phone Silverlight アプリでタグ付け API を使用する方法](mobile-engagement-windows-phone-use-engagement-api.md) の下) これらの統計がアプリケーションに依存するためです。

## サポートされているバージョン

Mobile Engagement SDK for Windows Silverlight は次を対象としたアプリケーションにのみ統合できます。

-   Windows Phone 8.0
-   Windows Phone 8.1 Silverlight

> [AZURE.NOTE] Windows Phone 8.1 (Silverlight 以外) を対象とする場合を参照してください、 [Windows Universal の統合手順](mobile-engagement-windows-store-integrate-engagement.md)します。

## Mobile Engagement Silverlight SDK をインストールする

Mobile Engagement SDK for Windows Silverlight は「*MicrosoftAzure.MobileEngagement*」という名称の Nuget パッケージとして利用できます。 これは Visual Studio Nuget Package Manager からインストールできます。

## 機能を追加する

Engagement SDK が正常に動作するには、Windows Phone Silverlight SDK の一部の機能が必要になります。

開いている、 `WMAppManifest.xml` ファイルし、で、次の機能を宣言するかどうかを必ず、 `機能` パネル。

-   `ID_CAP_NETWORKING`
-   `ID_CAP_IDENTITY_DEVICE`

## エンゲージメント SDK を初期化する

### Engagement の構成

Engagement の構成がで集中管理、 `Resources\EngagementConfiguration.xml` 、プロジェクトのファイルです。

このファイルを編集して次を指定します:

-   タグの間のアプリケーション接続文字列 `< connectionString >` と `< \connectionString >`します。

代わりに指定を実行時に行う場合は、 Engagement エージェントを初期化する前に、次のメソッドを呼び出すことができます。

    /* Engagement configuration. */
    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";
    
    /* Initialize Engagement agent with above configuration. */
    EngagementAgent.Instance.Init(engagementConfiguration);

アプリケーションの接続文字列が Azure クラシック ポータルに表示されます。

### Engagement の初期化

新しいプロジェクトを作成するときに、 `App.xaml.cs` ファイルを生成します。 このクラスから継承 `アプリケーション` 多くの重要なメソッドが含まれています。 また、Engagement SDK を初期化するためにも使用されます。

変更、 `App.xaml.cs`:

-   追加、 `を使用して` ステートメント。

        using Microsoft.Azure.Engagement;

-   挿入 `EngagementAgent.Instance.Init` で、 `Application_Launching` メソッド。

        private void Application_Launching(object sender, LaunchingEventArgs e)
        {
          EngagementAgent.Instance.Init();
        }

-   挿入 `EngagementAgent.Instance.OnActivated` で、 `Application_Activated` メソッド。

        private void Application_Activated(object sender, ActivatedEventArgs e)
        {
           EngagementAgent.Instance.OnActivated(e);
        }


> [AZURE.WARNING] Engagement の初期化は、アプリケーションの別の場所には追加しないことを強くお勧めします。 ただし、注意している、 `EngagementAgent.Instance.Init` メソッドは、UI スレッドではなく、専用のスレッドで実行します。

## 基本的なレポート

### 推奨される方法: オーバー ロード、 `PhoneApplicationPage` クラス

ユーザー、セッション、アクティビティ、クラッシュ、技術に関する統計の計算に Engagement が必要なすべてのログのレポートをアクティブ化するために行うことができます単にすべての `PhoneApplicationPage` サブ クラスから継承、 `EngagementPage` クラスです。

次に、この操作をアプリケーションのページに対して実行する例を示します。 同じことを、アプリケーションのすべてのページに対して実行できます。

#### C# ソース ファイル

ページの変更 `. xaml.cs` ファイル。

-   追加、 `を使用して` ステートメント。

        using Microsoft.Azure.Engagement;

-   置換 `PhoneApplicationPage` と `EngagementPage` :

**エンゲージメントを使用しない場合:**

        namespace Example
        {
          public partial class ExamplePage : PhoneApplicationPage
          {
            [...]
          }
        }

**Engagement を使用する場合:**

        using Microsoft.Azure.Engagement;
    
        namespace Example
        {
          public partial class ExamplePage : EngagementPage 
          {
            [...]
          }
        }

> [AZURE.WARNING] ページが継承する場合、 `OnNavigatedTo` 、メソッド、に注意 `ベースです。OnNavigatedTo(e)` 呼び出します。 呼び出しが行われないと、アクティビティがレポートされません。 実際には、 `EngagementPage` を呼び出して `StartActivity` 内、 `OnNavigatedTo` メソッドです。

#### XAML ファイル

ページの変更 `.xaml` ファイル。

-   名前空間宣言に追加します :

        xmlns:engagement="clr-namespace:Microsoft.Azure.Engagement;assembly=Microsoft.Azure.Engagement.EngagementAgent.WP"

-   置換 `電話: PhoneApplicationPage` と `エンゲージメント: EngagementPage` :

**エンゲージメントを使用しない場合:**

        <phone:PhoneApplicationPage>
            
        </phone:PhoneApplicationPage>

**Engagement を使用する場合:**

        <engagement:EngagementPage 
            xmlns:engagement="clr-namespace:Microsoft.Azure.Engagement;assembly=Microsoft.Azure.Engagement.EngagementAgent.WP">
    
            
        </engagement:EngagementPage >

#### 既定の動作をオーバーライドします。

既定では、ページのクラス名は、何も付加しないアクティビティ名として報告されます。 クラスが「Page」サフィックスを使用している場合、Engagement はそれも削除します。

既定の動作の名前をオーバーライドする場合は、次をコードに追加します。

        // in the .xaml.cs file
        protected override string GetEngagementPageName()
        {
           /* your code */
           return "new name";
        }

アクティビティに関する追加情報を報告する場合は、次の内容をコードに追加します。

        // in the .xaml.cs file
        protected override Dictionary<object,object> GetEngagementPageExtra()
        {
           /* your code */
           return extra;
        }

内からこれらのメソッドが呼び出されます、 `OnNavigatedTo` 、ページのメソッドです。

### 代替の方法: 呼び出す `StartActivity()` 手動で

できないか、オーバー ロードしたくない場合、 `PhoneApplicationPage` クラス、代わりに呼び出すことによって、アクティビティを開始できます `EngagementAgent` メソッドを直接します。

呼び出すことをお勧め `StartActivity` 内、 `OnNavigatedTo` PhoneApplicationPage のメソッドです。

        protected override void OnNavigatedTo(NavigationEventArgs e)
        {
           base.OnNavigatedTo(e);
           EngagementAgent.Instance.StartActivity("MyPage");
        }

> [AZURE.IMPORTANT] セッションは正常に終了してください。
>
> SDK は自動的に呼び出し、 `EndActivity` メソッドは、アプリケーションが閉じられるとします。 そのため、 **高** を呼び出すことをお勧め、 `StartActivity` 、ユーザーのアクティビティが変化するごとにされ、 **NEVER** を呼び出す、 `EndActivity` メソッドです。 このメソッドは、現在のユーザーがアプリケーションを終了したと Engagement サーバーに通知するため、すべてのアプリケーション ログが影響を受けるからです。

## 詳細な報告

必要に応じて、レポート アプリケーション固有のイベント、エラー、ジョブをすることがありますこれを行うには、使用して、別のメソッドで、 `EngagementAgent` クラスです。 Engagement API により、Engagement のすべての高度な機能を使用できます。

詳細については、次を参照してください。 [、高度なモバイル エンゲージメント Windows Phone Silverlight アプリでタグ付け API を使用する方法](../mobile-engagement-windows-phone-use-engagement-api/)します。

## 詳細な構成

### 自動クラッシュ レポートを無効にする

Engagement の自動クラッシュ レポート機能を無効にできます。 その場合、未処理の例外が発生しても、Engagement は何も行いません。
> [AZURE.WARNING] この機能を無効にする場合は、アプリで未処理のクラッシュが発生した場合に Engagement からクラッシュを送信せず、**さらに**セッションとジョブも閉じられませんのでご注意ください。

自動クラッシュ レポートを無効にするには、宣言されている方法に基づいて構成をカスタマイズします:

#### `EngagementConfiguration.xml` ファイル

Report crash 設定 `false` 間 `< reportCrash >` と `</reportCrash >` タグ。

#### `EngagementConfiguration` 実行時にオブジェクト

EngagementConfiguration オブジェクトを使用して、report crash を false に設定します。

        /* Engagement configuration. */
    
        EngagementConfiguration engagementConfiguration = new EngagementConfiguration(); engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";
        /\* Disable Engagement crash reporting. \*/ engagementConfiguration.Agent.ReportCrash = false;

### バースト モード

既定では、Engagement サービスはログをリアルタイムで報告します。 アプリケーションがログを非常に頻繁に報告する場合、ログをバッファーに格納して、それらすべてを定期的に一度に報告する方が適切です (これを「バースト モード」と呼びます)。

そのためには、次のメソッドを呼び出します。

        EngagementAgent.Instance.SetBurstThreshold(int everyMs);

引数の値は**ミリ秒**単位です。 リアルタイムのログ報告を再度有効にする場合は常に、パラメーターを指定しないか、値 0 を指定して、このメソッドを呼び出します。

バースト モードではわずかにバッテリーの寿命が延びますが、Engagement の監視に影響を与えます。すべてのセッションとジョブの実行時間は、バーストのしきい値に丸められます (つまり、バーストのしきい値よりも短いセッションとジョブは、認識されない場合があります)。 バーストのしきい値は、30000 (30 秒) よりも長くしないことをお勧めします。 保存できるログは 300 項目に制限されていることに気を付けてください。 送信時間が長すぎる場合は、ログがいくつか失われる可能性があります。
> [AZURE.WARNING] バーストのしきい値は、1 秒よりも短くすることはできません。 1 秒未満にしようとすると、SDK でエラーのトレースが表示され、自動的に既定値の 0 秒にリセットされます。 これにより、SDK はログをリアルタイムで報告するようになります。





