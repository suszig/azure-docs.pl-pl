<properties 
    pageTitle="Windows Runtime 8.1 ユニバーサル アプリへのプッシュ通知の追加 | Azure Mobile Apps" 
    description="Azure App Service Mobile Apps と Azure Notification Hubs を使用して Windows アプリにプッシュ通知を送信する方法について説明します。" 
    services="app-service\mobile,notification-hubs" 
    documentationCenter="windows" 
    authors="ggailey777" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="app-service-mobile" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-windows" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="11/25/2015" 
    ms.author="glenga"/>

# Windows Runtime 8.1 ユニバーサル アプリへのプッシュ通知の追加

[AZURE.INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]
&nbsp;  
[AZURE.INCLUDE [app-service-mobile-note-mobile-services](../../includes/app-service-mobile-note-mobile-services.md)]

##概要

このトピックでは、Azure App Service Mobile Apps と Azure Notification Hubs を使用して Windows Runtime 8.1 ユニバーサル アプリにプッシュ通知を送信する方法を説明します。 このシナリオでは、新しい項目が追加されると、Mobile App バックエンドによって、Windows Notification Service (WNS) に登録されているすべての Windows アプリにプッシュ通知が送信されます。

このチュートリアルは、App Service Mobile App のクイック スタートに基づいています。 このクイック スタート チュートリアルを完了してこのチュートリアルを開始する前におく必要があります [Windows アプリを作成する](../app-service-mobile-windows-store-dotnet-get-started.md)です。 ダウンロードしたクイック スタートのサーバー プロジェクトを使用しない場合は、プッシュ通知拡張機能パッケージをプロジェクトに追加する必要があります。 サーバーの拡張機能パッケージの詳細については、次を参照してください。 [Azure モバイル アプリの .NET バックエンド サーバー SDK と連携](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md)します。 

##前提条件

このチュートリアルを完了するには、以下が必要です。

* アクティブな [Microsoft ストア アカウント](http://go.microsoft.com/fwlink/p/?LinkId=280045)します。
* [Visual Studio Community 2013](https://go.microsoft.com/fwLink/p/?LinkID=391934)
* 完了、 [クイック スタート チュートリアル](../app-service-mobile-windows-store-dotnet-get-started.md)します。  


##<a name="create-hub"></a>通知ハブを作成する

[AZURE.INCLUDE [app-service-mobile-create-notification-hub](../../includes/app-service-mobile-create-notification-hub.md)]

##アプリケーションをプッシュ通知に登録する

Azure から Windows アプリにプッシュ通知を送信するには、Windows ストア にアプリを送信する必要があります。 その後、サーバー プロジェクトを構成して WNS と統合できます。

[AZURE.INCLUDE [app-service-mobile-register-wns](../../includes/app-service-mobile-register-wns.md)]


##プッシュ通知を送信するようにバックエンドを構成する

[AZURE.INCLUDE [app-service-mobile-configure-wns](../../includes/app-service-mobile-configure-wns.md)]


##<a id="update-service"></a>プッシュ通知を送信するようにサーバーを更新する

アプリでプッシュ通知が有効にされたので、プッシュ通知を送信するようにアプリ バックエンドを更新する必要があります。 その下の手順は、バックエンド プロジェクトの種類 & mdash と一致する使用しているいずれか [.NET バックエンド](#dotnet) または [Node.js バックエンド](#nodejs)します。

### <a name="dotnet"></a>.NET バックエンド プロジェクト

1. Visual Studio で、サーバー プロジェクトを右クリックし、クリックして **NuGet パッケージの管理**, を Microsoft.Azure.NotificationHubs を検索してクリックして **インストール**します。 これにより、Notification Hubs のクライアント ライブラリがインストールされます。

2. 展開 **コント ローラー**, TodoItemController.cs を開き、次の追加ステートメントを使用します。

        using System.Collections.Generic;
        using Microsoft.Azure.NotificationHubs;
        using Microsoft.Azure.Mobile.Server.Config;

3.  **PostTodoItem** 、メソッド呼び出しの後に次のコードを追加 **InsertAsync**: 

        // Get the settings for the server project.
        HttpConfiguration config = this.Configuration;
        MobileAppSettingsDictionary settings =
            this.Configuration.GetMobileAppSettingsProvider().GetMobileAppSettings();
    
        // Get the Notification Hubs credentials for the Mobile App.
        string notificationHubName = settings.NotificationHubName;
        string notificationHubConnection = settings
            .Connections[MobileAppSettingsKeys.NotificationHubConnectionString].ConnectionString;
    
        // Create the notification hub client.
        NotificationHubClient hub = NotificationHubClient
            .CreateClientFromConnectionString(notificationHubConnection, notificationHubName);
    
        // Define a WNS payload
        var windowsToastPayload = @"<toast><visual><binding template=""ToastText01""><text id=""1"">"
                                + item.Text + @"</text></binding></visual></toast>";
        try
        {
            // Send the push notification.
            var result = await hub.SendWindowsNativeNotificationAsync(windowsToastPayload);
    
            // Write the success result to the logs.
            config.Services.GetTraceWriter().Info(result.State.ToString());
        }
        catch (System.Exception ex)
        {
            // Write the failure result to the logs.
            config.Services.GetTraceWriter()
                .Error(ex.Message, null, "Push.SendAsync Error");
        }
    
    このコードは、新しい項目が挿入された後、プッシュ通知を送信するように通知ハブに指示します。

### <a name="nodejs"></a>Node.js バックエンド プロジェクト

1. これをまだ実行していない場合 [クイック スタート プロジェクトをダウンロード](app-service-mobile-node-backend-how-to-use-server-sdk.md#download-quickstart) またはその他の使用、 [Azure ポータルでのオンライン エディター](app-service-mobile-node-backend-how-to-use-server-sdk.md#online-editor)します。

2. todoitem.js ファイル内の既存のコードを次のコードに置き換えます。

        var azureMobileApps = require('azure-mobile-apps'),
        promises = require('azure-mobile-apps/src/utilities/promises'),
        logger = require('azure-mobile-apps/src/logger');
    
        var table = azureMobileApps.table();
        
        table.insert(function (context) {
        // For more information about the Notification Hubs JavaScript SDK,  
        // see http://aka.ms/nodejshubs
        logger.info('Running TodoItem.insert');
        
        // Define the WNS payload that contains the new item Text.
        var payload = "<toast><visual><binding template=\ToastText01\><text id=\"1\">"
                                    + context.item.text + "</text></binding></visual></toast>";
        
        // Execute the insert.  The insert returns the results as a Promise,
        // Do the push as a post-execute action within the promise flow.
        return context.execute()
            .then(function (results) {
                // Only do the push if configured
                if (context.push) {
                    // Send a WNS native toast notification.
                    context.push.wns.sendToast(null, payload, function (error) {
                        if (error) {
                            logger.error('Error while sending push notification: ', error);
                        } else {
                            logger.info('Push notification sent successfully!');
                        }
                    });
                }
                // Don't forget to return the results from the context.execute()
                return results;
            })
            .catch(function (error) {
                logger.error('Error while running context.execute: ', error);
            });
        });

        module.exports = table;  

    これにより、新しい ToDo 項目が挿入されたときには item.text を含む WNS トースト通知が送信されます。

2. ローカル コンピューターでファイルを編集するときは、サーバー プロジェクトを再発行します。 

## <a name="publish-the-service"></a>モバイル バックエンドを Azure に発行する

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-publish-service](../../includes/app-service-mobile-dotnet-backend-publish-service.md)]

##<a id="update-service"></a>アプリケーションにプッシュ通知を追加する

1. 共有を開く **App.xaml.cs** プロジェクト ファイルと、次の追加 `using` ステートメント。

        using System.Threading.Tasks;  
        using Windows.Networking.PushNotifications;       

2. 同じファイルで次のコードを追加 **InitNotificationsAsync** メソッドの定義を **アプリ** クラス。
    
        private async Task InitNotificationsAsync()
        {
            // Get a channel URI from WNS.
            var channel = await PushNotificationChannelManager
                .CreatePushNotificationChannelForApplicationAsync();

            // Register the channel URI with Notification Hubs.
            await App.MobileService.GetPush().RegisterAsync(channel.Uri);
        }
    
    このコードにより、WNS からアプリケーションの ChannelURI が取得され、その ChannelURI が App Service モバイル アプリに登録されます。
    
3. 上部にある、 **OnLaunched** 内のイベント ハンドラー **App.xaml.cs**, 、追加、 **async** 修飾子をメソッドの定義の新規作成] を次の呼び出しを追加し、 **InitNotificationsAsync** 次の例のように、メソッド。

        protected async override void OnLaunched(LaunchActivatedEventArgs e)
        {
            await InitNotificationsAsync();

            // ...
        }

    これにより、アプリケーションが起動されるたびに、有効期限付きの ChannelURI が登録されます。 

4. ソリューション エクスプ ローラーでダブルクリック **Package.appxmanifest** Windows ストア アプリので、 **通知**, 、設定されて **トースト対応** に **はい**します。

     **ファイル** ] メニューをクリックして **すべてを保存**します。

5. Windows Phone ストア アプリ プロジェクトの前の手順を繰り返します。

これで、アプリケーションがトースト通知を受信する準備が整いました。

##<a id="test"></a>アプリケーションでプッシュ通知をテストする

[AZURE.INCLUDE [app-service-mobile-windows-universal-test-push](../../includes/app-service-mobile-windows-universal-test-push.md)]


##<a id="more"></a>詳細

* テンプレートを利用すれば、プラットフォーム間のプッシュやローカライズされたプッシュを柔軟に送信できます。 [Azure モバイル アプリのマネージ クライアントを使用する方法](app-service-mobile-dotnet-how-to-use-client-library.md#how-to-register-push-templates-to-send-cross-platform-notifications) テンプレートを登録する方法を示します。
* タグを利用すれば、特定の区分に属する顧客にプッシュで的を絞ることができます。 [Azure モバイル アプリの .NET バックエンド サーバー SDK と連携](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#how-to-add-tags-to-a-device-installation-to-enable-push-to-tags) デバイスのインストールにタグを追加する方法を示します。

<!-- Anchors. -->

<!-- URLs. -->
[Azure Portal]: https://portal.azure.com/

<!-- Images. -->

