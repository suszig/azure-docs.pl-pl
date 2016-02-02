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

## 概要

このチュートリアルでは、Azure Notification Hubs を使用して Windows ストアまたは Windows Phone 8.1 アプリケーション (Silverlight 以外) にプッシュ通知を送信する方法について説明します。 Windows Phone 8.1 Silverlight を対象とする場合を参照してください、 [Windows Phone](notification-hubs-windows-phone-get-started.md) バージョンです。
このチュートリアルでは、Windows プッシュ通知サービス (WNS) を使用してプッシュ通知を受信する、空の Windows ストア アプリを作成します。 完了すると、通知ハブを使用して、アプリケーションを実行するすべてのデバイスにプッシュ通知をブロードキャストできるようになります。


## 開始する前に

[AZURE.INCLUDE [notification-hubs-hero-slug](../../includes/notification-hubs-hero-slug.md)]

このチュートリアルの完全なコードは GitHub でご覧 [ここ](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/GetStartedWindowsUniversal)します。



## 前提条件

このチュートリアルには、次のものが必要です。

+ Microsoft Visual Studio Express 2013 for Windows Update 2 以降<br/>、ユニバーサル アプリ プロジェクトを作成するこのバージョンの Visual Studio が必要です。 Windows ストア アプリのみを作成する場合は、Visual Studio 2012 Express for Windows 8 が必要です。

+ アクティブな Windows ストア アカウント

+ アクティブな Azure アカウント <br/>アカウントを持っていない場合は、ほんの数分で無料の試用アカウントを作成することができます。 詳細については、「 [Azure 無料試用版](http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-windows-store-dotnet-get-started%2F)します。

このチュートリアルを完了することは、Windows ストア アプリケーションの他のすべての Notification Hubs チュートリアルの前提条件です。

## アプリケーションを Windows ストアに登録する

Windows ストア アプリにプッシュ通知を送信するには、アプリケーションを Windows ストアに関連付ける必要があります。 さらに、WNS に統合するために通知ハブを構成する必要があります。

1. アプリを登録済みでない場合に移動、 [Windows デベロッパー センター](http://go.microsoft.com/fwlink/p/?LinkID=266582), 、Microsoft アカウントでサインインし、[クリックして **新しいアプリを作成する**します。

2. アプリの名前を入力し、**[アプリの名前の予約]** をクリックします。

    ![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hubs-win8-app-name.png)

    これでアプリケーションの新しい Windows ストア登録が作成されます。

3. Visual Studio で、**[空のアプリケーション]** テンプレートを使用して新しい Visual C# ストア アプリ プロジェクトを作成します。

    ![][2]

4. ソリューション エクスプローラーで Windows ストア アプリ プロジェクトを右クリックし、**[ストア]**、**[アプリケーションをストアと関連付ける]** の順にクリックします。

    ![][3]

    **アプリケーションを Windows ストアと関連付ける**ウィザードが表示されます。

5. ウィザードで **[サインイン]** をクリックし、Microsoft アカウントでサインインします。

6. ステップ 2. で登録したアプリケーションをクリックし、**[次へ]**、**[関連付け]** の順にクリックします。

    ![][4]

    この操作により、必要な Windows ストア登録情報がアプリケーション マニフェストに追加されます。

7. (省略可能) Windows Phone ストア アプリ プロジェクトのステップ 4.～6. を繰り返します。

8. 戻り、 [Windows デベロッパー センター](http://go.microsoft.com/fwlink/p/?LinkID=266582) 新しいアプリに] ページで [ **サービス**, 、] をクリックして **プッシュ通知**, 、順にクリック **Live サービス サイト** [ **Windows プッシュ通知サービス (WNS) と Microsoft Azure Mobile Services**します。

    ![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hubs-win8-app-live-services.png)

9. **[アプリケーション設定]** タブで、**[クライアント シークレット]** と **[パッケージ セキュリティ ID (SID)]** の値をメモしておきます。

    ![][6]
    > [AZURE.WARNING]
    クライアント シークレットとパッケージ SID は、重要なセキュリティ資格情報です。 これらの値は、他のユーザーと共有したり、アプリケーションで配信したりしないでください。

## 通知ハブを構成する

[AZURE.INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

<ol start="7">
<li><p>選択します <b>構成</b> 上部にあるタブを入力します <b>クライアント シークレット</b> と <b>パッケージ SID</b> 値を前のセクションで WNS から取得し、クリックします。 <b>保存</b>.</p>
</li>
</ol>

(& a) です。 (& a) です。![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-configure-wns.png)


これで、通知ハブが WNS と連動するように構成されました。接続文字列にアプリケーションを登録し、通知を送信できます。

## 通知ハブにアプリケーションを接続する

1. Visual Studio でソリューションを右クリックし、**[NuGet パッケージの管理]** をクリックします。

    **[NuGet パッケージの管理]** ダイアログ ボックスが表示されます。

2. 検索 `WindowsAzure.Messaging.Managed` ] をクリック **インストール**, 、ソリューションでは、すべてのプロジェクトを選択および使用条件に同意します。

    ![][20]

    これをダウンロード、インストール、および参照を追加、すべてのプロジェクトで Azure メッセージング ライブラリに windows を使用して、 <a href="http://nuget.org/packages/WindowsAzure.Messaging.Managed/">WindowsAzure.Messaging.Managed NuGet パッケージ</a>します。

3. App.xaml.cs プロジェクト ファイルを開き、次の追加 `を使用して` ステートメントです。ユニバーサル プロジェクトではこのファイルにある、 `< project_name > です。共有` フォルダーです。

        using Windows.Networking.PushNotifications;
        using Microsoft.WindowsAzure.Messaging;
        using Windows.UI.Popups;

4. さらに、App.xaml.cs で、次の **InitNotificationsAsync** メソッドの定義を **App** クラスに追加します。

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
 >[AZURE.NOTE][Azure クラシック ポータル] に表示される通知ハブの名前に"hub name"プレース ホルダーを置き換えることを確認、 **Notification Hubs** ] タブ (たとえば、 **mynotificationhub2** 前の例では)。 また、接続文字列プレースホルダーを、先に取得した **DefaultListenSharedAccessSignature** 接続文字列に置き換えます。

5. App.xaml.cs 内で、**OnLaunched** イベント ハンドラーの先頭に、次に示す新しい **InitNotificationsAsync** メソッドへの呼び出しを追加します。

        InitNotificationsAsync();

    これにより、アプリケーションが起動するたびに必ずチャネル URI が通知ハブに登録されます。

6. ソリューション エクスプローラーで Windows ストア アプリの **Package.appxmanifest** をダブルクリックし、**[通知]** で **[トースト対応]** を **[はい]** に設定します。

    ![][18]

    **[ファイル]** メニューの **[すべて保存]** をクリックします。

7. (省略可能) Windows Phone ストア アプリ プロジェクトの前のステップを繰り返します。

8. **F5** キーを押してアプリケーションを実行します。 登録キーを示すポップアップ ダイアログが表示されます。

    ![][19]

9. (省略可能) 前の手順を繰り返して、アプリを Windows Phone デバイスに登録する Windows Phone プロジェクトを実行します。



これで、アプリケーションがトースト通知を受信する準備が整いました。

## 通知を送信する

次の画面に示すように、[Azure クラシック ポータル] で、通知ハブを [デバッグ] タブを使用して、通知を送信することによって、アプリケーションで通知を受信をテストすることができます。

![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-debug.png)

プッシュ通知は通常、互換性のあるライブラリを使用して Mobile Services などのバックエンド サービスや ASP.NET に送信されます。 ライブラリがバックエンドで利用できない場合、REST API を直接使用して通知メッセージを送信できます。

このチュートリアルでは、バックエンド サービスではなく、コンソール アプリケーションの通知ハブに .NET SDK を使用して通知を送信することで例を単純にして、クライアント アプリのテスト手順のみを説明します。 [ユーザーにプッシュ通知を Notification Hubs を使用する] このチュートリアルは、ASP.NET バックエンドから通知を送信するための次の手順としてお勧めします。 ただし、通知の送信には、次の方法も使用できます。

* **REST インターフェイス**: バックエンド プラットフォームを使用して通知をサポートすることができます、 [REST インターフェイス](http://msdn.microsoft.com/library/windowsazure/dn223264.aspx)します。

* **Microsoft Azure Notification Hubs .NET SDK**: で、Nuget パッケージ マネージャーを実行している Visual Studio の [Install-package Microsoft.Azure.NotificationHubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/)します。

* **Node.js** : [Node.js から Notification Hubs の使用方法](notification-hubs-nodejs-how-to-use-notification-hubs.md)します。

* **Azure Mobile Services**: Notification Hubs と統合されている Azure Mobile Services バックエンドから通知を送信する方法の例は、作業開始 Mobile Services でプッシュ通知の使用」を参照してください ([.NET バックエンド](../mobile-services/mobile-services-javascript-backend-windows-store-dotnet-get-started-push.md) | [JavaScript backend](../mobile-services/mobile-services-javascript-backend-windows-store-dotnet-get-started-push.md)).

* **Java または PHP**: REST Api を使用して通知を送信する方法の例は、「JAVA/PHP から Notification Hubs を使用する方法」を参照してください ([Java](notification-hubs-java-backend-how-to.md) | [PHP](notification-hubs-php-backend-how-to.md)).


## (省略可能) コンソール アプリケーションから通知を送信する

.NET コンソール アプリケーションを使用して通知を送信するには、次の手順を実行します。

1. ソリューションを右クリックして **[追加]**、**[新しいプロジェクト]** の順に選択し、**[Visual C#]** で **[Windows]**、**[コンソール アプリケーション]** の順にクリックして、**[OK]** をクリックします。

    ![][13]

    これにより、新しい Visual C# コンソール アプリケーションがソリューションに追加されます。 個別のソリューションでこの操作を行うこともできます。

2. Visual Studio で、**[ツール]**、**[Nuget パッケージ マネージャー]**、**[パッケージ マネージャー コンソール]** の順にクリックします。

    Visual Studio のパッケージ マネージャー コンソールが表示されます。

3. パッケージ マネージャー コンソール ウィンドウで **[既定のプロジェクト]** に新しいコンソール アプリケーション プロジェクトを設定した後、そのコンソール ウィンドウから次のコマンドを実行します。

        Install-Package Microsoft.Azure.NotificationHubs

    使用して、Azure Notification Hubs SDK に参照が追加、 <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">Microsoft.Azure.Notification ハブ NuGet パッケージ</a>します。

    ![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-package-manager.png)

4. Program.cs ファイルを開き、次の追加 `を使用して` ステートメント。

        using Microsoft.Azure.NotificationHubs;

5. **Program** クラスで、次のメソッドを追加します。

        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient
                .CreateClientFromConnectionString("<connection string with full access>", "<hub name>");
            var toast = @"<toast><visual><binding template=""ToastText01""><text id=""1"">Hello from a .NET App!</text></binding></visual></toast>";
            await hub.SendWindowsNativeNotificationAsync(toast);
        }

    "Hub name"プレース ホルダーの [Azure クラシック ポータル] に表示される通知ハブの名前を置き換えます確認、 **Notification Hubs** ] タブをクリックします。 また、接続文字列プレースホルダーを、「通知ハブを構成する」で取得した **DefaultFullSharedAccessSignature** という接続文字列に置き換えます。
    >[AZURE.NOTE]**リッスン** アクセスではなく**フル** アクセスを持つ接続文字列を使用してください。 リッスン アクセス文字列には通知を送信するアクセス許可はありません。

6. **Main** メソッド内に、次の行を追加します。

         SendNotificationAsync();
         Console.ReadLine();

7. Visual Studio でコンソール アプリケーション プロジェクトを右クリックし、**[スタートアップ プロジェクトに設定]** をクリックすることにより、それをスタートアップ プロジェクトとして設定します。 **F5** キーを押して、アプリケーションを実行します。

    ![][14]

    登録したすべてのデバイスでトースト通知を受信します。 トースト バナーをクリックまたはタップすると、アプリケーションが読み込まれます。

サポートされているすべてのペイロードが記載されて [トースト カタログ] を [タイル カタログ]、および MSDN のトピックを [バッジの概要] です。

## 次のステップ

この簡単な例では、すべての Windows デバイスにブロードキャスト通知を送信します。 [ユーザーにプッシュ通知を Notification Hubs を使用する] このチュートリアルは、次の手順としてお勧めします。 このチュートリアルでは、特定のユーザーに対してタグを使用して ASP.NET バックエンドから通知を送信する方法について説明しています。

対象グループごとにユーザーをセグメント化する場合は、[通知ハブを使用したニュース速報の送信] を参照してください。

Notification Hubs に関する一般的な情報については、[通知ハブの概要] を参照してください。









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
[azure classic portal]: https://manage.windowsazure.com/ 
[notification hubs guidance]: http://msdn.microsoft.com/library/jj927170.aspx 
[use notification hubs to push notifications to users]: notification-hubs-aspnet-backend-windows-dotnet-notify-users.md 
[use notification hubs to send breaking news]: notification-hubs-windows-store-dotnet-send-breaking-news.md 
[toast catalog]: http://msdn.microsoft.com/library/windows/apps/hh761494.aspx 
[tile catalog]: http://msdn.microsoft.com/library/windows/apps/hh761491.aspx 
[badge overview]: http://msdn.microsoft.com/library/windows/apps/hh779719.aspx 

