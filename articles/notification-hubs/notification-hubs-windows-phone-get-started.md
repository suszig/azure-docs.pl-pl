<properties
    pageTitle="Azure Notification Hubs の使用 (Windows Phone) | Microsoft Azure"
    description="このチュートリアルでは、Azure Notification Hubs を使用して Windows Phone 8 または Windows Phone 8.1 Silverlight アプリケーションにプッシュ通知を送信する方法について説明します。"
    services="notification-hubs"
    documentationCenter="windows"
    authors="wesmc7777"
    manager="dwrede"
    editor="dwrede"/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows-phone"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="12/14/2015"
    ms.author="wesmc"/>


# Notification Hubs の使用 (Windows Phone)

[AZURE.INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

## 概要

このチュートリアルでは、Azure Notification Hubs を使用して Windows Phone 8 または Windows Phone 8.1 Silverlight アプリケーションにプッシュ通知を送信する方法について説明します。 Windows Phone 8.1 (Silverlight 以外) を対象とする場合を参照してください、 [Windows ユニバーサル](notification-hubs-windows-store-dotnet-get-started.md) バージョンです。
このチュートリアルでは、Microsoft プッシュ通知サービス (MPNS) を使用してプッシュ通知を受信する空の Windows Phone 8 アプリケーションを作成します。 完了すると、通知ハブを使用して、アプリケーションを実行するすべてのデバイスにプッシュ通知をブロードキャストできるようになります。
> [AZURE.NOTE] Notification Hubs Windows Phone SDK では、Windows Phone 8.1 Silverlight アプリで Windows プッシュ通知サービス (WNS) を使用できません。 Windows Phone 8.1 Silverlight アプリで (MPNS ではなく) WNS を使用するには、REST API を使用する「Notification Hubs - Windows Phone Silverlight チュートリアル」のサンプルの手順に従ってください。

このチュートリアルでは、Notification Hubs を使用した簡単なブロードキャスト シナリオのデモンストレーションを行います。

## 前提条件

このチュートリアルには、次のものが必要です。

+ [Visual Studio 2012 Express for Windows Phone]、またはそれ以降のバージョン。

このチュートリアルを完了することは、Windows Phone 8 アプリケーションの他のすべての Notification Hubs チュートリアルの前提条件です。
> [AZURE.NOTE] このチュートリアルを完了するには、アクティブな Azure アカウントが必要です。 アカウントがない場合は、無料試用版アカウントを数分で作成することができます。 詳細については、「 [Azure 無料試用版](http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-windows-phone-get-started%2F)します。

## 通知ハブを作成する

[AZURE.INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

<ol start="7">
<li><p>[ <b>構成</b> タブをクリックし、をクリックします <b>非認証プッシュ通知を有効にする</b> チェック ボックスをオンにします <b>Windows Phone 通知設定</b> 」セクションを参照してください。</p>
</li>
</ol>

(& a) です。 (& a) です。![](./media/notification-hubs-windows-phone-get-started/notification-hub-pushauth.png)

これでハブが作成され、Windows Phone に向けて非認証通知を送信するように構成されました。
> [AZURE.NOTE] このチュートリアルでは、非認証モードで MPNS を使用します。 MPNS 非認証モードでは、各チャネルに送信できる通知に制限があります。 通知ハブではサポート [MPNS 認証モード](http://msdn.microsoft.com/library/windowsphone/develop/ff941099.aspx) によって、証明書をアップロードすることができます。

## Notification Hub にアプリを接続する

1. Visual Studio で、新しい Windows Phone 8 アプリケーションを作成します。

    ![][13]

    Visual Studio 2013 Update 2 以降では、代わりに Windows Phone Silverlight アプリケーションを作成します。

    ![][11]

2. Visual Studio でソリューションを右クリックし、**[NuGet パッケージの管理]** をクリックします。

    **[NuGet パッケージの管理]** ダイアログ ボックスが表示されます。

3. 検索 `WindowsAzure.Messaging.Managed` ] をクリック **インストール**, 、し、その後、使用条件に同意します。

    ![][20]

    これをダウンロード、インストール、および Windows 用 Azure メッセージング ライブラリへの参照を使用して、追加、 <a href="http://nuget.org/packages/WindowsAzure.Messaging.Managed/">WindowsAzure.Messaging.Managed NuGet パッケージ</a>します。

4. App.xaml.cs ファイルを開き、次の追加 `を使用して` ステートメント。

        using Microsoft.Phone.Notification;
        using Microsoft.WindowsAzure.Messaging;

5. App.xaml.cs 内の **Application_Launching** メソッドの先頭に次のコードを追加します。

     var channel = HttpNotificationChannel.Find("MyPushChannel");
     if (channel == null)
     {
         channel = new HttpNotificationChannel("MyPushChannel");
         channel.Open();
         channel.BindToShellToast();
     }
    
     channel.ChannelUriUpdated += new EventHandler<NotificationChannelUriEventArgs>(async (o, args) =>
     {
         var hub = new NotificationHub("<hub name>", "<connection string>");
         var result = await hub.RegisterNativeAsync(args.ChannelUri.ToString());
    
         System.Windows.Deployment.Current.Dispatcher.BeginInvoke(() =>
         {
             MessageBox.Show("Registration :" + result.RegistrationId, "Registered", MessageBoxButton.OK);
         });
     });

 必ず、ハブの名前と、前のセクションで取得した **DefaultListenSharedAccessSignature** という接続文字列を挿入してください。
 このコードにより、MPNS からアプリケーションのチャネル URI が取得され、そのチャネル URI が通知ハブに登録されます。 これにより、アプリケーションが起動するたびに必ずチャネル URI も通知ハブに登録されます。
 >[AZURE.NOTE]このチュートリアルでは、トースト通知がデバイスに送信されます。 タイル通知を送信する場合は、チャネルの **BindToShellTile** メソッドを呼び出す必要があります。 通知をタイル トーストの両方をサポートするには、両方を呼び出す **BindToShellTile** と  **BindToShellToast**します。

6. ソリューション エクスプローラーで **[プロパティ]** を展開して WMAppManifest.xml ファイルを開き、**[機能]** タブで **ID_CAP_PUSH_NOTIFICATION** 機能がオンであることを確認します。

    ![][14]

    これにより、アプリケーションでプッシュ通知の受信が有効になります。

7. F5 キーを押してアプリケーションを実行します。

    登録メッセージが表示されます。

8. アプリケーションを閉じます。 トースト通知を受信するには、アプリケーションを閉じる必要があります。

## バックエンドから通知を送信する

使用して任意のバックエンドから Notification Hubs を使用して、通知を送信することができます、 <a href="http://msdn.microsoft.com/library/windowsazure/dn223264.aspx">REST インターフェイス</a>します。 このチュートリアルでは、.NET コンソール アプリケーションを使用して通知を送信します。 通知ハブと統合されている Azure Mobile Services バックエンドから通知を送信する方法の例は、作業開始 Mobile Services でプッシュ通知の使用」を参照してください ([.NET バックエンド](../mobile-services-javascript-backend-windows-phone-get-started-push.md) | [JavaScript backend](../mobile-services-javascript-backend-windows-phone-get-started-push.md)). REST Api を使用して通知を送信する方法の例は、「JAVA/PHP から Notification Hubs を使用する方法」を参照してください ([Java](notification-hubs-java-backend-how-to.md) | [PHP](notification-hubs-php-backend-how-to.md)).

1. ソリューションを右クリックして **[追加]**、**[新しいプロジェクト]** の順に選択し、**[Visual C#]** で **[Windows]**、**[コンソール アプリケーション]** の順にクリックして、**[OK]** をクリックします。

    ![][6]

    これにより、新しい Visual C# コンソール アプリケーションがソリューションに追加されます。 個別のソリューションでこの操作を行うこともできます。

4. **[ツール]**、**[ライブラリ パッケージ マネージャー]**、**[パッケージ マネージャー コンソール]** の順にクリックします。

    これで、パッケージ マネージャー コンソールが表示されます。

5.  パッケージ マネージャー コンソール ウィンドウで **[既定のプロジェクト]** に新しいコンソール アプリケーション プロジェクトを設定した後、そのコンソール ウィンドウから次のコマンドを実行します。

        Install-Package Microsoft.Azure.NotificationHubs

    使用して、Azure Notification Hubs SDK に参照が追加、 <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">Microsoft.Azure.Notification ハブ NuGet パッケージ</a>します。

6. Program.cs ファイルを開き、次の追加 `を使用して` ステートメント。

        using Microsoft.Azure.NotificationHubs;

6. **Program** クラスで、次のメソッドを追加します。

        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient
                .CreateClientFromConnectionString("<connection string with full access>", "<hub name>");
            string toast = "<?xml version=\"1.0\" encoding=\"utf-8\"?>" +
                "<wp:Notification xmlns:wp=\"WPNotification\">" +
                   "<wp:Toast>" +
                        "<wp:Text1>Hello from a .NET App!</wp:Text1>" +
                   "</wp:Toast> " +
                "</wp:Notification>";
            await hub.SendMpnsNativeNotificationAsync(toast);
        }

    "hub name" プレースホルダーは、ポータルの **[通知ハブ]** タブに表示される通知ハブの名前に置き換えてください。 また、接続文字列プレースホルダーを、「通知ハブを構成する」で取得した **DefaultFullSharedAccessSignature** という接続文字列に置き換えます。
    >[AZURE.NOTE]**リッスン** アクセスではなく**フル** アクセスを持つ接続文字列を使用してください。 リッスン アクセス文字列には通知を送信するアクセス許可はありません。

4. 次に、**Main** メソッド内に次の行を追加します。

         SendNotificationAsync();
         Console.ReadLine();

5. Windows Phone エミュレーターを実行し、アプリを閉じて、コンソール アプリケーション プロジェクトを既定のスタートアップ プロジェクトとして設定します。次に、F5 キーを押して、アプリを実行します。

    トースト通知を受信します。 トースト バナーをタップすると、アプリが読み込まれます。

[トースト カタログ] で、使用できるすべてのペイロードを見つけることができ、[タイル カタログ] MSDN のトピックにします。

## 次のステップ

この簡単な例では、すべての Windows Phone 8 デバイスに通知をブロードキャストしました。 特定のユーザーをターゲットとするには、[ユーザーにプッシュ通知を Notification Hubs を使用する] このチュートリアルを参照してください。 対象グループごとにユーザーを区分するには、[通知ハブを使用したニュース速報の送信] を読み取ることができます。 [通知ハブの概要] で、通知ハブを使用する方法について説明します。






[6]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-console-app.png 
[7]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-from-portal.png 
[8]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-from-portal2.png 
[9]: ./media/notification-hubs-windows-phone-get-started/notification-hub-select-from-portal.png 
[10]: ./media/notification-hubs-windows-phone-get-started/notification-hub-select-from-portal2.png 
[11]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-wp-silverlight-app.png 
[12]: ./media/notification-hubs-windows-phone-get-started/notification-hub-connection-strings.png 
[13]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-wp-app.png 
[14]: ./media/notification-hubs-windows-phone-get-started/mobile-app-enable-push-wp8.png 
[15]: ./media/notification-hubs-windows-phone-get-started/notification-hub-pushauth.png 
[20]: ./media/notification-hubs-windows-phone-get-started/notification-hub-windows-universal-app-install-package.png 
[213]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-console-app.png 
[visual studio 2012 express for windows phone]: https://go.microsoft.com/fwLink/p/?LinkID=268374 
[notification hubs guidance]: http://msdn.microsoft.com/library/jj927170.aspx 
[mpns authenticated mode]: http://msdn.microsoft.com/library/windowsphone/develop/ff941099(v=vs.105).aspx 
[use notification hubs to push notifications to users]: notification-hubs-aspnet-backend-windows-dotnet-notify-users.md 
[use notification hubs to send breaking news]: notification-hubs-windows-phone-send-breaking-news.md 
[toast catalog]: http://msdn.microsoft.com/library/windowsphone/develop/jj662938(v=vs.105).aspx 
[tile catalog]: http://msdn.microsoft.com/library/windowsphone/develop/hh202948(v=vs.105).aspx 
[notification hub - wp silverlight tutorial]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/PushToSLPhoneApp 

