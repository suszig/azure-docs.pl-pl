<properties 
    pageTitle="Windows ユニバーサル アプリ Engagement SDK 統合" 
    description="Windows ユニバーサル アプリと Azure Mobile Engagement を統合する方法"                  
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="dwrede" 
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-windows-store" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="07/07/2015" 
    ms.author="piyushjo" />


# Windows ユニバーサル アプリ Engagement SDK 統合

> [AZURE.SELECTOR] 
- [Universal Windows](mobile-engagement-windows-store-integrate-engagement.md) 
- [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md) 
- [iOS](mobile-engagement-ios-integrate-engagement.md) 
- [Android](mobile-engagement-android-integrate-engagement.md) 


この手順では、Windows ユニバーサル アプリケーションで Engagement の分析機能と監視機能をアクティブ化する最も簡単な方法を説明します。

次の手順は、ユーザー、セッション、アクティビティ、クラッシュ、テクニカルに関するすべての統計情報をコンピューティングするのに必要なログのレポートを有効にするためのものです。 イベント、エラー、ジョブなどその他の統計の計算に必要なログのレポートは、エンゲージメント API を使用して手動で行う必要があります (を参照してください [、高度なモバイル エンゲージメント Windows ユニバーサル アプリでタグ付け API を使用する方法](mobile-engagement-windows-store-use-engagement-api.md) これらの統計がアプリケーションに依存するためです。

## サポートされているバージョン

Mobile Engagement SDK for Windows ユニバーサル アプリは次を対象とした Windows ランタイム アプリケーションおよびユニバーサル Windows プラットフォーム アプリケーションにのみ統合できます。

-   Windows 8
-   Windows 8.1
-   Windows Phone 8.1
-   Windows 10 (デスクトップとモバイル ファミリ)

> [AZURE.NOTE] Windows Phone Silverlight を対象とするかどうかを参照してください、 [Windows Phone Silverlight の統合手順](mobile-engagement-windows-phone-integrate-engagement.md)します。

## Mobile Engagement ユニバーサル アプリ SDK をインストールします。

### すべてのプラットフォーム

Mobile Engagement SDK for Windows Universal App は「*MicrosoftAzure.MobileEngagement*」という名称の Nuget パッケージとして利用できます。 これは Visual Studio Nuget Package Manager からインストールできます。

### Windows 8.x および Windows Phone 8.1

NuGet で SDK のリソースを自動的に展開する、 `リソース` アプリケーション プロジェクトのルートにあるフォルダーです。

### Windows 10 ユニバーサル Windows プラットフォーム アプリケーション

NuGet は、まだ UWP アプリケーションで SDK のリソースを自動的にデプロイしません。 NuGet でリソースのデプロイメントが再導入されるまで手動で行う必要があります。

1.  ファイル エクスプ ローラーを開きます。
2.  次の場所に移動 (**x.x.x** をインストールするサービスのバージョン)。
*%USERPROFILE%\\.nuget\packages\MicrosoftAzure.MobileEngagement\\**x.x.x**\\content\win81*
3.  ファイル エクスプローラーから Visual Studio のプロジェクトのルートに、**リソース** フォルダーをドラッグ アンド ドロップします。
4.  Visual Studio でプロジェクトを選択し、**[ソリューション エクスプ ローラー]** の一番上にある **[すべてのファイルを表示]** アイコンをアクティブ化します。
5.  一部のファイルは、プロジェクトには含まれません。 一度にインポートするには、**リソース** フォルダーを右クリックして **[プロジェクトから除外]** を選択してから、もう一度**リソース** フォルダーを右クリックして **[プロジェクトに含める]** を選択し、フォルダー全体を再度含めます。 これで、**リソース** フォルダーのすべてのファイルがプロジェクトに追加されます。

抽出した Engagement パッケージは、*$(Solutiondir) \Packages* または定義した *NuGet.config* ファイルにもあります。

## 機能を追加する

Engagement SDK が適切に機能するには、Windows SDK の機能が一部必要になります。

開いている、 `Package.appxmanifest` ファイルであり、次の機能が宣言されていることを確認します。

-   `インターネット (クライアント)`

## エンゲージメント SDK を初期化する

### Engagement の構成

Engagement の構成がで集中管理、 `Resources\EngagementConfiguration.xml` 、プロジェクトのファイルです。

このファイルを編集して、次の内容を指定します。

-   タグの間のアプリケーション接続文字列 `< connectionString >` と `< \connectionString >`します。

代わりに指定を実行時に行う場合は、 Engagement エージェントを初期化する前に、次のメソッドを呼び出すことができます。

          /* Engagement configuration. */
          EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    
          /* Connection string for my Windows Store App. */
          engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";
    
          /* Initialize Engagement angent with above configuration. */
          EngagementAgent.Instance.Init(e, engagementConfiguration);

アプリケーションの接続文字列が Azure クラシック ポータルに表示されます。

### Engagement の初期化

新しいプロジェクトを作成するときに、 `App.xaml.cs` ファイルを生成します。 このクラスから継承 `アプリケーション` 多くの重要なメソッドが含まれています。 また、Engagement SDK を初期化するためにも使用されます。

変更、 `App.xaml.cs`:

-   追加、 `を使用して` ステートメント。

        using Microsoft.Azure.Engagement;

-   すべての呼び出しに対して 1 回で Engagement の初期化を共有するメソッドを定義します。

    private void InitEngagement(IActivatedEventArgs e)
    {
      EngagementAgent.Instance.Init(e);
    
      // or
    
      EngagementAgent.Instance.Init(e, engagementConfiguration);
    }

-   呼び出す `InitEngagement` で、 `OnLaunched` メソッド。

        protected override void OnLaunched(LaunchActivatedEventArgs e)
        {
          InitEngagement(e);
        }

-   カスタム スキーマ、別のアプリケーションまたはコマンドラインを使用してアプリケーションが起動されるときに、 `OnActivated` メソッドが呼び出されます。 また、アプリをアクティブ化する際は、 Engagement SDK を初期化する必要があります。 これを行うには、オーバーライド `OnActivated` メソッド。

        protected override void OnActivated(IActivatedEventArgs args)
        {
          InitEngagement(args);
        }


> [AZURE.IMPORTANT] Engagement の初期化は、アプリケーションの別の場所には追加しないことを強くお勧めします。

## 基本的なレポート

### 推奨される方法: オーバー ロード、 `ページ` クラス

ユーザー、セッション、アクティビティ、クラッシュ、技術に関する統計の計算に Engagement が必要なすべてのログのレポートをアクティブ化するために行うことができます単にすべての `ページ` サブ クラスから継承、 `EngagementPage` クラスです。

次に、この操作をアプリケーションのページに対して実行する例を示します。 同じことを、アプリケーションのすべてのページに対して実行できます。

#### C# ソース ファイル

ページの変更 `. xaml.cs` ファイル。

-   追加、 `を使用して` ステートメント。

        using Microsoft.Azure.Engagement;

-   置換 `ページ` と `EngagementPage`:

**エンゲージメントを使用しない場合:**

        namespace Example
        {
          public sealed partial class ExamplePage : Page
          {
            [...]
          }
        }

**Engagement を使用する場合:**

        using Microsoft.Azure.Engagement;
    
        namespace Example
        {
          public sealed partial class ExamplePage : EngagementPage 
          {
            [...]
          }
        }

> [AZURE.IMPORTANT] ページをオーバーライドする場合、 `OnNavigatedTo` メソッド、を呼び出してください `ベースです。OnNavigatedTo(e)`します。 それ以外の場合、アクティビティは報告されません (、 `EngagementPage` 呼び出し `StartActivity` 内の `OnNavigatedTo` メソッド)。

#### XAML ファイル

ページの変更 `.xaml` ファイル。

-   次の内容を名前空間宣言に追加します。

        xmlns:engagement="using:Microsoft.Azure.Engagement"

-   置換 `ページ` と `エンゲージメント: EngagementPage`:

**エンゲージメントを使用しない場合:**

        <Page>
            
            ...
        </Page>

**Engagement を使用する場合:**

        <engagement:EngagementPage 
            xmlns:engagement="using:Microsoft.Azure.Engagement">
            
            ...
        </engagement:EngagementPage >

#### 既定の動作をオーバーライドする

既定では、ページのクラス名は、何も付加しないアクティビティ名として報告されます。 クラスが「Page」サフィックスを使用している場合、Engagement はそれも削除します。

名前の既定の動作をオーバーライドする場合は、単純に次の内容をコードに追加します。

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

できないか、オーバー ロードしたくない場合、 `ページ` クラス、代わりに呼び出すことによって、アクティビティを開始できます `EngagementAgent` メソッドを直接します。

呼び出すことをお勧め `StartActivity` 内、 `OnNavigatedTo` 、ページのメソッドです。

            protected override void OnNavigatedTo(NavigationEventArgs e)
            {
              base.OnNavigatedTo(e);
              EngagementAgent.Instance.StartActivity("MyPage");
            }

> [AZURE.IMPORTANT]  セッションは正常に終了してください。
> 
> Windows ユニバーサル SDK は自動的に呼び出し、 `EndActivity` メソッドは、アプリケーションが閉じられるとします。 そのため、 **高** を呼び出すことをお勧め、 `StartActivity` 、ユーザーのアクティビティが変化するごとにされ、 **NEVER** を呼び出す、 `EndActivity` メソッドでは、このメソッドは送信 Engagement サーバーに現在のユーザーがアプリケーションを終了したことは、このはすべてのアプリケーション ログに影響を与えます。

## 詳細な報告

必要に応じて、レポート アプリケーション固有のイベント、エラー、ジョブをすることがありますこれを行うには、使用して、別のメソッドで、 `EngagementAgent` クラスです。 Engagement API により、Engagement のすべての高度な機能を使用できます。

詳細については、次を参照してください。 [、高度なモバイル エンゲージメント Windows ユニバーサル アプリでタグ付け API を使用する方法](../mobile-engagement-windows-store-use-engagement-api/)します。

## 詳細な構成

### 自動クラッシュ レポートを無効にする

Engagement の自動クラッシュ レポート機能を無効にできます。 その場合、未処理の例外が発生しても、Engagement は何も行いません。
> [AZURE.WARNING] この機能を無効にする計画がある場合は、アプリで未処理の例外が発生しても、エンゲージメントはクラッシュを報告せず、**さらに**セッションとジョブを終了しないことに注意してください。

自動クラッシュ レポートを無効にするには、宣言した方法に応じて、構成をカスタマイズするだけです。

#### `EngagementConfiguration.xml` ファイル

Report crash 設定 `false` 間 `< reportCrash >` と `</reportCrash >` タグ。

#### `EngagementConfiguration` 実行時にオブジェクト

EngagementConfiguration オブジェクトを使用して、report crash を false に設定します。

        /* Engagement configuration. */
        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";
    
        /* Disable Engagement crash reporting. */
        engagementConfiguration.Agent.ReportCrash = false;

### バースト モード

既定では、Engagement サービスはログをリアルタイムで報告します。 アプリケーションがログを非常に頻繁に報告する場合、ログをバッファーに格納して、それらすべてを定期的に一度に報告する方が適切です (これを「バースト モード」と呼びます)。

そのためには、次のメソッドを呼び出します。

        EngagementAgent.Instance.SetBurstThreshold(int everyMs);

引数の値は**ミリ秒**単位です。 リアルタイムのログ報告を再度有効にする場合は常に、パラメーターを指定しないか、値 0 を指定して、このメソッドを呼び出します。

バースト モードではわずかにバッテリーの寿命が延びますが、Engagement の監視に影響を与えます。すべてのセッションとジョブの実行時間は、バーストのしきい値に丸められます (つまり、バーストのしきい値よりも短いセッションとジョブは、認識されない場合があります)。 バーストのしきい値は、30000 (30 秒) よりも長くしないことをお勧めします。 保存できるログは 300 項目に制限されていることに気を付けてください。 送信時間が長すぎる場合は、ログがいくつか失われる可能性があります。
> [AZURE.WARNING] また、バーストのしきい値は、1 秒よりも短い期間に設定することはできません。 それを試みた場合、SDK はエラーのトレースを表示し、自動的に既定値 (0 秒) に再設定します。 これにより、SDK はログをリアルタイムで報告するようになります。


[here]: http://www.nuget.org/packages/Capptain.WindowsCS 
[nuget website]: http://docs.nuget.org/docs/start-here/overview 

