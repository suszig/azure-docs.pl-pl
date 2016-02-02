<properties
    pageTitle="Windows Phone Silverlight アプリの Azure Mobile Engagement の概要"
    description="Windows Phone Silverlight アプリで Azure Mobile Engagement の分析機能やプッシュ通知を使用する方法について説明します。"
    services="mobile-engagement"
    documentationCenter="windows"
    authors="piyushjo"
    manager="dwrede"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows-phone"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="09/22/2015"
    ms.author="piyushjo" />


# Windows Phone Silverlight アプリの Azure Mobile Engagement の概要

> [AZURE.SELECTOR]
- [Windows Universal](mobile-engagement-windows-store-dotnet-get-started.md)
- [Windows Phone Silverlight](mobile-engagement-windows-phone-get-started.md)
- [iOS | Obj C](mobile-engagement-ios-get-started.md)
- [iOS | Swift](mobile-engagement-ios-swift-get-started.md)
- [Android](mobile-engagement-android-get-started.md)
- [Cordova](mobile-engagement-cordova-get-started.md)


このトピックでは、Azure モバイル エンゲージメントを使用してアプリの使用状況を把握し、Windows Phone Silverlight アプリケーションのセグメント化されたユーザーにプッシュ通知を送信する方法について説明します。
このチュートリアルでは、モバイル エンゲージメントを使用した簡単なブロードキャスト シナリオのデモンストレーションを行います。 チュートリアル内で空白の Windows Phone Silverlight アプリを作成し、このアプリで基本的なデータを収集して、Microsoft Push Notification Service (MPNS) を使用してプッシュ通知を受信します。
> [AZURE.NOTE] Windows Phone 8.1 (Silverlight 以外) を対象とする場合を参照してください、 [Windows Universal チュートリアル](mobile-engagement-windows-store-dotnet-get-started.md)します。

このチュートリアルには、次のものが必要です。

+ Visual Studio 2013
+ [MicrosoftAzure.MobileEngagement] Nuget パッケージ

> [AZURE.IMPORTANT] Windows Phone Silverlight アプリ向けの他のすべてのモバイル エンゲージメント チュートリアルは、このチュートリアルを完了していることが前提条件となっています。このチュートリアルを完了するには、アクティブな Azure アカウントが必要です。 アカウントがない場合は、無料試用版アカウントを数分で作成することができます。 詳細については、「 <a href="http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fwww.windowsazure.com%2Fen-us%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started%2F" target="_blank">Azure 無料試用版</a>します。

## <a id="setup-azme"></a>モバイル エンゲージメントの Windows の設定 Phone アプリ

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal.md)]

## <a id="connecting-app"></a>アプリをモバイル エンゲージメントのバックエンドに接続します。

このチュートリアルでは、データを収集してプッシュ通知を送信するために必要な最小限のセットである「基本的な統合」について説明します。 完全な統合のドキュメントについては記載されて、 [モバイル エンゲージメント Windows Phone SDK 統合](../mobile-engagement-windows-phone-sdk-overview/)

統合のデモンストレーションを行うために、Visual Studio で基本的なアプリを作成します。

### 新しい Windows Phone Silverlight プロジェクトを作成する

次の手順では、以前のバージョンの Visual Studio と類似した手順で、Visual Studio 2015 を使用します。

1. Visual Studio を起動し、**ホーム**画面で **[新しいプロジェクト]** を選択します。

2. ポップアップで、**[Windows 8]**、**[Windows Phone]**、**[空のアプリ (Windows Phone Silverlight)]** の順に選択します。 アプリの **[名前]** と **[ソリューション名]** を入力し、**[OK]** をクリックします。

    ![][1]

3. ターゲットとして **Windows Phone 8.0** または **Windows Phone 8.1** を選択できます。

これで、Azure Mobile Engagement SDK を統合する新しい Windows Phone Silverlight アプリが作成されました。

### アプリを Mobile Engagement のバックエンドに接続する

1. プロジェクトで、[MicrosoftAzure.MobileEngagement] nuget パッケージをインストールします。

2. 開いている `WMAppManifest.xml` ([プロパティ] フォルダー)、次が宣言されていることを確認し、(に追加されていない場合) で、 `< 機能/>` タグ。

        <Capability Name="ID_CAP_NETWORKING" />
        <Capability Name="ID_CAP_IDENTITY_DEVICE" />

    ![][2]

3. ここでモバイル エンゲージメント アプリの前にコピーした接続文字列を貼り付け、貼り付けることで、 `Resources\EngagementConfiguration.xml` ファイル間、 `< connectionString >` と `</connectionString >` タグ。

    ![][3]

4. `App.xaml.cs` ファイル。

    a. 追加、 `を使用して` ステートメント。

            using Microsoft.Azure.Engagement;

    b. SDK を初期化、 `Application_Launching` メソッド。

            private void Application_Launching(object sender, LaunchingEventArgs e)
            {
              EngagementAgent.Instance.Init();
            }

    c. 次を挿入、 `Application_Activated`:

            private void Application_Activated(object sender, ActivatedEventArgs e)
            {
               EngagementAgent.Instance.OnActivated(e);
            }


## <a id="monitor"></a>リアルタイム監視を有効にします。

データを送信してユーザーがアクティブであることを確認するには、少なくとも 1 つの画面 (アクティビティ) を Mobile Engagement のバックエンドに送信する必要があります。

1. MainPage.xaml.cs に、追加、 `を使用して` ステートメント。

        using Microsoft.Azure.Engagement;

2. **PhoneApplicationPage** の前にある **MainPage** の基本クラスを **EngagementPage** に置き換えます。

        class MainPage : EngagementPage 

3. `MainPage.xml` ファイル。

    a. 次の内容を名前空間宣言に追加します。

         xmlns:engagement="clr-namespace:Microsoft.Azure.Engagement;assembly=Microsoft.Azure.Engagement.EngagementAgent.WP"

    b. 置換 `電話: PhoneApplicationPage` XML タグ名に `エンゲージメント: EngagementPage`します。

## <a id="monitor"></a>リアルタイム監視のアプリを接続します。

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

## <a id="integrate-push"></a>プッシュ通知とアプリ内メッセージングを有効にします。

Mobile Engagement を導入すると、キャンペーンとの関連でプッシュ通知とアプリ内メッセージングを利用し、ユーザーに情報を提供できます。 このモジュールは、モバイル エンゲージメント ポータルで REACH として呼び出されます。
次のセクションでは、それらを受信するようにアプリをセットアップします。

### アプリが WNS プッシュ通知を受信できるようにする

新しい機能を追加、 `WMAppManifest.xml` ファイル。

        ID_CAP_PUSH_NOTIFICATION
        ID_CAP_WEBBROWSERCOMPONENT

   ![][5]

### REACH SDK を初期化する

1. `App.xaml.cs`, 、呼び出す `EngagementReach.Instance.Init();` で、 **Application_Launching** 、エージェントの初期化の直後に、関数。

        private void Application_Launching(object sender, LaunchingEventArgs e)
        {
           EngagementAgent.Instance.Init();
           EngagementReach.Instance.Init();
        }

2. `App.xaml.cs`, 、呼び出す `EngagementReach.Instance.OnActivated(e);` で、 **Application_Activated** 、エージェントの初期化の直後に、関数。

        private void Application_Activated(object sender, ActivatedEventArgs e)
        {
           EngagementAgent.Instance.OnActivated(e);
           EngagementReach.Instance.OnActivated(e);
        }


設定が完了しました。 それでは、この基本的な統合が正しく設定されているかどうかを検証してみましょう。

## <a id="send"></a>アプリへ通知を送信します。

[AZURE.INCLUDE [Create Windows Push campaign](../../includes/mobile-engagement-windows-push-campaign.md)]

アプリが開いている場合、通知はアプリ内通知としてデバイスに表示されます。アプリが開いていない場合は、次のようにトースト通知として表示されます。

![][6]




[microsoftazure.mobileengagement]: http://go.microsoft.com/?linkid=9874664 
[mobile engagement windows phone sdk documentation]: ../mobile-engagement-windows-phone-integrate-engagement/ 
[1]: ./media/mobile-engagement-windows-phone-get-started/project-properties.png 
[2]: ./media/mobile-engagement-windows-phone-get-started/wmappmanifest-capabilities.png 
[3]: ./media/mobile-engagement-windows-phone-get-started/add-connection-string.png 
[5]: ./media/mobile-engagement-windows-phone-get-started/reach-capabilities.png 
[6]: ./media/mobile-engagement-windows-phone-get-started/push-screenshot.png 

