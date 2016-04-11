<properties
    pageTitle="Azure Notification Hubs の使用 (Windows ストア アプリ) | Microsoft Azure"
    description="このチュートリアルでは、Azure Notification Hubs を使用して Windows ストアまたは Windows Phone 8.1 アプリケーション (Silverlight 以外) にプッシュ通知する方法について学習します。"
    services="notification-hubs"
    documentationCenter="windows"
    authors="wesmc7777"
    manager="dwrede"
    editor="dwrede"/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="12/14/2015"
    ms.author="wesmc"/>

# Notification Hubs の使用 (Windows ストア アプリ)

[AZURE.INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

##概要

このチュートリアルでは、Azure Notification Hubs を使用して Windows ストアまたは Windows Phone 8.1 アプリケーション (Silverlight 以外) にプッシュ通知を送信する方法について説明します。 Windows Phone 8.1 Silverlight を対象とする場合を参照してください、 [Windows Phone](notification-hubs-windows-phone-get-started.md) バージョンです。
このチュートリアルでは、Windows プッシュ通知サービス (WNS) を使用してプッシュ通知を受信する、空の Windows ストア アプリを作成します。 完了すると、通知ハブを使用して、アプリケーションを実行するすべてのデバイスにプッシュ通知をブロードキャストできるようになります。


## 開始する前に

[AZURE.INCLUDE [notification-hubs-hero-slug](../../includes/notification-hubs-hero-slug.md)]

このチュートリアルの完全なコードは GitHub でご覧 [ここ](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/GetStartedWindowsUniversal)します。



##前提条件

このチュートリアルには、次のものが必要です。

+ Microsoft Visual Studio Express 2013 for Windows Update 2 以降<br/>ユニバーサル アプリ プロジェクトを作成するにはこのバージョンの Visual Studio が必要です。 Windows ストア アプリのみを作成する場合は、Visual Studio 2012 Express for Windows 8 が必要です。

+ アクティブな Windows ストア アカウント

+ アクティブな Azure アカウント <br/>アカウントがない場合は、無料試用版のアカウントを数分で作成することができます。 詳細については、[Azure の無料試用版サイト](http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-windows-store-dotnet-get-started%2F)をご覧ください。

このチュートリアルを完了することは、Windows ストア アプリケーションの他のすべての Notification Hubs チュートリアルの前提条件です。

##アプリケーションを Windows ストアに登録する

Windows ストア アプリにプッシュ通知を送信するには、アプリケーションを Windows ストアに関連付ける必要があります。 さらに、WNS に統合するために通知ハブを構成する必要があります。

1. アプリを登録済みでない場合に移動、 [Windows デベロッパー センター](http://go.microsoft.com/fwlink/p/?LinkID=266582), 、Microsoft アカウントでサインインし、[クリックして **新しいアプリを作成する**です。


2. クリックして、アプリの名前を入力 **アプリ名の予約**します。

    ![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hubs-win8-app-name.png)

    これでアプリケーションの新しい Windows ストア登録が作成されます。

3. Visual Studio を使用して新しい Visual c# ストア アプリ プロジェクトを作成、 **空のアプリケーション** テンプレートです。

    ![][2]

4. ソリューション エクスプ ローラーで、Windows ストア アプリ プロジェクトを右クリックし、をクリックして **ストア**, 、クリックして **アプリケーションを... ストアと関連付ける**します。

    ![][3]

     **アプリケーションを Windows ストアと関連付ける** ウィザードが表示されます。

5. ウィザードで、次のようにクリックします。 **サインイン** してから、Microsoft アカウントでサインインします。

6. 手順 2. で登録したアプリケーションをクリックし **次**, 、] をクリックし、 **関連付ける**します。

    ![][4]

    この操作により、必要な Windows ストア登録情報がアプリケーション マニフェストに追加されます。

7. (省略可能) Windows Phone ストア アプリ プロジェクトのステップ 4.～6. を繰り返します。  

8. 戻り、 [Windows デベロッパー センター](http://go.microsoft.com/fwlink/p/?LinkID=266582) 新しいアプリに] ページで [ **サービス**, 、] をクリックして **プッシュ通知**, 、順にクリック **Live サービス サイト** [ **Windows プッシュ通知サービス (WNS) と Microsoft Azure Mobile Services**します。

    ![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hubs-win8-app-live-services.png)

9.  **アプリ設定** ] タブの値をメモしておいてください **クライアント シークレット** と **パッケージ セキュリティ id (SID)**します。

    ![][6]

    > [AZURE.WARNING]
    クライアント シークレットとパッケージ SID は、重要なセキュリティ資格情報です。 これらの値は、他のユーザーと共有したり、アプリケーションで配信したりしないでください。

##通知ハブを構成する

[AZURE.INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

<ol start="7">
<li><p>選択、 <b>構成</b> 上部にあるタブで、入力、 <b>クライアント シークレット</b> と <b>パッケージ SID</b> ] をクリックして、前のセクションで WNS から取得される値 <b>保存</b>します。</p>
</li>
</ol>

&emsp;&emsp;![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-configure-wns.png)


これで、通知ハブが WNS と連動するように構成されました。接続文字列にアプリケーションを登録し、通知を送信できます。

##通知ハブにアプリケーションを接続する

1. Visual Studio でソリューションを右クリックし、をクリックして **NuGet パッケージの管理**します。

    これを表示、 **NuGet パッケージの管理** ] ダイアログ ボックス。

2. 検索 `WindowsAzure.Messaging.Managed` ] をクリック **インストール**, 、ソリューションでは、すべてのプロジェクトを選択および使用条件に同意します。

    ![][20]

    こうすることで、<a href="http://nuget.org/packages/WindowsAzure.Messaging.Managed/">WindowsAzure.Messaging.Managed NuGet パッケージ</a>を使用して、Windows の Azure メッセージング ライブラリにすべてのプロジェクトの参照がダウンロード、インストール、追加されます。

3. App.xaml.cs プロジェクト ファイルを開き、次の `using` ステートメントを追加します。 ユニバーサル プロジェクトでは、このファイルは `<project_name>.Shared` フォルダーにあります。

        using Windows.Networking.PushNotifications;
        using Microsoft.WindowsAzure.Messaging;
        using Windows.UI.Popups;



4. また、App.xaml.cs で次のコードを追加 **InitNotificationsAsync** メソッドの定義を **アプリ** クラス。

        private async void InitNotificationsAsync()
        {
            var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

            var hub = new NotificationHub("<hub name>", "<connection string with listen access>");
            var result = await hub.RegisterNativeAsync(channel.Uri);

            // Displays the registration ID so you know it was successful
            if (result.RegistrationId != null)
            {
                var dialog = new MessageDialog("Registration successful: " + result.RegistrationId);
                dialog.Commands.Add(new UICommand("OK"));
                await dialog.ShowAsync();
            }

        }

    このコードにより、WNS からアプリケーションのチャネル URI が取得され、そのチャネル URI が通知ハブに登録されます。

    >[AZURE.NOTE]表示される通知ハブの名前に"hub name"プレース ホルダーを置き換えることを確認、 [Azure Classic Portal] で、 **Notification Hubs** ] タブ (たとえば、 **mynotificationhub2** 前の例で)。 接続文字列プレース ホルダーを置き換えることも、 **DefaultListenSharedAccessSignature** 前のセクションで取得した接続文字列。

5. 上部にある、 **OnLaunched** App.xaml.cs のイベント ハンドラーが、新規作成] を次の呼び出しを追加 **InitNotificationsAsync** メソッド。

        InitNotificationsAsync();

    これにより、アプリケーションが起動するたびに必ずチャネル URI が通知ハブに登録されます。

6. ソリューション エクスプ ローラーでダブルクリック **Package.appxmanifest** Windows ストア アプリのし、[ **通知**, 設定 **トースト対応** に **はい**:

    ![][18]

     **ファイル** ] メニューをクリックして **すべてを保存**します。

7. (省略可能) Windows Phone ストア アプリ プロジェクトの前のステップを繰り返します。

8. キーを押して、 **f5 キーを押して** キーをアプリケーションを実行します。 登録キーを示すポップアップ ダイアログが表示されます。

    ![][19]

9. (省略可能) 前の手順を繰り返して、アプリを Windows Phone デバイスに登録する Windows Phone プロジェクトを実行します。



これで、アプリケーションがトースト通知を受信する準備が整いました。

##通知を送信する 

通知を送信することによって、アプリで通知の受信をテストする、 [Azure Classic Portal] 通知ハブで、次の画面に示すように、[デバッグ] タブを使用しています。

![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-debug.png)

プッシュ通知は通常、互換性のあるライブラリを使用して Mobile Services などのバックエンド サービスや ASP.NET に送信されます。 ライブラリがバックエンドで利用できない場合、REST API を直接使用して通知メッセージを送信できます。 

このチュートリアルでは、バックエンド サービスではなく、コンソール アプリケーションの通知ハブに .NET SDK を使用して通知を送信することで例を単純にして、クライアント アプリのテスト手順のみを説明します。 お勧めします [Use Notification Hubs to push notifications to users] 」チュートリアルでは、次に、ASP.NET バックエンドから通知を送信するためです。 ただし、通知の送信には、次の方法も使用できます。

* **REST インターフェイス**: バックエンド プラットフォームを使用して通知をサポートすることができます、 [REST インターフェイス](http://msdn.microsoft.com/library/windowsazure/dn223264.aspx)します。

* **Microsoft Azure Notification Hubs .NET SDK**: で、Nuget パッケージ マネージャーを実行している Visual Studio の [Install-package Microsoft.Azure.NotificationHubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/)します。

* **Node.js** : [Node.js から Notification Hubs の使用方法](notification-hubs-nodejs-how-to-use-notification-hubs.md)します。

* **Azure Mobile Services**: Notification Hubs と統合されている Azure Mobile Services バックエンドから通知を送信する方法の例は、作業開始 Mobile Services でプッシュ通知の使用」を参照してください ([.NET バックエンド](../mobile-services/mobile-services-javascript-backend-windows-store-dotnet-get-started-push.md) | [JavaScript バックエンド](../mobile-services/mobile-services-javascript-backend-windows-store-dotnet-get-started-push.md))。

* **Java または PHP**: REST Api を使用して通知を送信する方法の例は、「JAVA/PHP から Notification Hubs を使用する方法」を参照してください ([Java](notification-hubs-java-backend-how-to.md) | [PHP](notification-hubs-php-backend-how-to.md))。


## (省略可能) コンソール アプリケーションから通知を送信する


.NET コンソール アプリケーションを使用して通知を送信するには、次の手順を実行します。 

1. ソリューションを右クリックし、選択 **追加** と **新しいプロジェクト]**, 、し [ **Visual C# の場合**, 、] をクリックして **Windows** と **コンソール アプリケーション**, 、] をクリック **OK**します。

    ![][13]

    これにより、新しい Visual C# コンソール アプリケーションがソリューションに追加されます。 個別のソリューションでこの操作を行うこともできます。

2. Visual Studio で、[ **ツール**, 、クリックして **NuGet パッケージ マネージャー**, 、順にクリック **パッケージ マネージャー コンソール**します。

    Visual Studio のパッケージ マネージャー コンソールが表示されます。

3. パッケージ マネージャー コンソール] ウィンドウで、設定、 **既定のプロジェクト** 、新しいコンソール アプリケーション プロジェクトにし、コンソール ウィンドウには、次のコマンドを実行します。

        Install-Package Microsoft.Azure.NotificationHubs

    これにより <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">Microsoft.Azure.Notification Hubs NuGet パッケージ</a>を利用して Azure Notification Hubs SDK に参照が追加されます。

    ![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-package-manager.png)


4. Program.cs ファイルを開き、次の `using` ステートメントを追加します。

        using Microsoft.Azure.NotificationHubs;

5.  **プログラム** クラスを次のメソッドを追加します。

        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient
                .CreateClientFromConnectionString("<connection string with full access>", "<hub name>");
            var toast = @"<toast><visual><binding template=""ToastText01""><text id=""1"">Hello from a .NET App!</text></binding></visual></toast>";
            await hub.SendWindowsNativeNotificationAsync(toast);
        }

    表示される通知ハブの名前に"hub name"プレース ホルダーを置き換えることを確認、 [Azure Classic Portal] 上、 **Notification Hubs** ] タブをクリックします。 また、接続文字列プレース ホルダーを接続文字列に置き換えて **DefaultFullSharedAccessSignature** で取得したセクションでは、「通知ハブを構成します。"

    >[AZURE.NOTE]持つ接続文字列を使用することを確認 **完全** アクセスではなく **リッスン** アクセスします。 リッスン アクセス文字列には通知を送信するアクセス許可はありません。

6. 次の行を追加、 **Main** メソッド。

         SendNotificationAsync();
         Console.ReadLine();

7. Visual Studio で、コンソール アプリケーション プロジェクトを右クリックし、クリックして **スタートアップ プロジェクトとして設定** をスタートアップ プロジェクトとして設定します。 キーを押します、 **f5 キーを押して** キーをアプリケーションを実行します。

    ![][14]

    登録したすべてのデバイスでトースト通知を受信します。 トースト バナーをクリックまたはタップすると、アプリケーションが読み込まれます。

サポートされているすべてのペイロードが記載、 [toast catalog], 、[tile catalog], 、および [badge overview] MSDN のトピックにします。

##次のステップ

この簡単な例では、すべての Windows デバイスにブロードキャスト通知を送信します。 お勧めします [Use Notification Hubs to push notifications to users] 」チュートリアルでは、次の手順です。 このチュートリアルでは、特定のユーザーに対してタグを使用して ASP.NET バックエンドから通知を送信する方法について説明しています。

対象グループごとにユーザーをセグメント化する場合は、「 [Use Notification Hubs to send breaking news]します。 

Notification Hubs に関する一般的な情報については、次を参照してください。 [Notification Hubs Guidance]します。






<!-- Images. -->
[2]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-create-windows-universal-app.png
[3]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-associate-win8-app.png
[4]: ./media/notification-hubs-windows-store-dotnet-get-started/mobile-services-select-app-name.png
[6]: ./media/notification-hubs-windows-store-dotnet-get-started/mobile-services-win8-app-push-auth.png
[11]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-configure-wns.png
[13]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-create-console-app.png
[14]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-windows-toast.png
[15]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-scheduler1.png
[16]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-scheduler2.png
[18]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-win8-app-toast.png
[19]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-windows-reg.png
[20]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-windows-universal-app-install-package.png

<!-- URLs. -->
[Azure Classic Portal]: https://manage.windowsazure.com/
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx

[Use Notification Hubs to push notifications to users]: notification-hubs-aspnet-backend-windows-dotnet-notify-users.md
[Use Notification Hubs to send breaking news]: notification-hubs-windows-store-dotnet-send-breaking-news.md

[toast catalog]: http://msdn.microsoft.com/library/windows/apps/hh761494.aspx
[tile catalog]: http://msdn.microsoft.com/library/windows/apps/hh761491.aspx
[badge overview]: http://msdn.microsoft.com/library/windows/apps/hh779719.aspx


