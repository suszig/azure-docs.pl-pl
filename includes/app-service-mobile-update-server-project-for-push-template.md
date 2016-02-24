バックエンド プロジェクトの種類 & mdash に一致する手順を使用しているいずれか [.NET バックエンド](#dotnet) または [Node.js バックエンド](#nodejs)します。

### <a name="dotnet"></a>.NET バックエンド プロジェクト
1. Visual Studio で、サーバー プロジェクトを右クリックし、をクリックして **NuGet パッケージの管理**, 、検索 `Microsoft.Azure.NotificationHubs`, 、クリックして **インストール**します。 これにより、バックエンドから通知を送信するために必要な Notification Hubs ライブラリがインストールされます。

3. サーバー プロジェクトで開きます **コント ローラー** > **TodoItemController.cs**, 、以下のステートメントを使用します。

        using System.Collections.Generic;
        using Microsoft.Azure.NotificationHubs;
        using Microsoft.Azure.Mobile.Server.Config;
    

2.  **PostTodoItem** 、メソッド呼び出しの後に次のコードを追加 **InsertAsync**:  

        // Get the settings for the server project.
        HttpConfiguration config = this.Configuration;
        MobileAppSettingsDictionary settings = 
            this.Configuration.GetMobileAppSettingsProvider().GetMobileAppSettings();
        
        // Get the Notification Hubs credentials for the Mobile App.
        string notificationHubName = settings.NotificationHubName;
        string notificationHubConnection = settings
            .Connections[MobileAppSettingsKeys.NotificationHubConnectionString].ConnectionString;

        // Create a new Notification Hub client.
        NotificationHubClient hub = NotificationHubClient
        .CreateClientFromConnectionString(notificationHubConnection, notificationHubName);

        // Sending the message so that all template registrations that contain "messageParam"
        // will receive the notifications. This includes APNS, GCM, WNS, and MPNS template registrations.
        Dictionary<string,string> templateParams = new Dictionary<string,string>();
        templateParams["messageParam"] = item.Text + " was added to the list.";

        try
        {
            // Send the push notification and log the results.
            var result = await hub.SendTemplateNotificationAsync(templateParams);

            // Write the success result to the logs.
            config.Services.GetTraceWriter().Info(result.State.ToString());
        }
        catch (System.Exception ex)
        {
            // Write the failure result to the logs.
            config.Services.GetTraceWriter()
                .Error(ex.Message, null, "Push.SendAsync Error");
        }

    このコードは、"messageParam" を含んでいるすべてのテンプレート登録にテンプレート通知を送信するよう、通知ハブに指示を出します。 登録に "messageParam" が使用されている各 PNS で、messageParam の位置に文字列が挿入されます。 これにより、APNS、GCM、WNS、またはその他の任意の PNS に通知を送信できるようになります。

    Notification Hubs のテンプレートの詳細については、次を参照してください。 [テンプレート](notification-hubs-templates.md)します。 

### <a name="nodejs"></a>Node.js バックエンド プロジェクト

1. todoitem.js ファイル内の既存のコードを次のコードに置き換えます。

        var azureMobileApps = require('azure-mobile-apps'),
        promises = require('azure-mobile-apps/src/utilities/promises'),
        logger = require('azure-mobile-apps/src/logger');
    
        var table = azureMobileApps.table();
        
        table.insert(function (context) {
        // For more information about the Notification Hubs JavaScript SDK, 
        // see http://aka.ms/nodejshubs
        logger.info('Running TodoItem.insert');
        
        // Define the template payload.
        var payload = '{"messageParam": context.item.text}'; 
        
        // Execute the insert.  The insert returns the results as a Promise,
        // Do the push as a post-execute action within the promise flow.
        return context.execute()
            .then(function (results) {
                // Only do the push if configured
                if (context.push) {
                    // Send a template notification.
                    context.push.send(null, payload, function (error) {
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

    これにより、新しい ToDo 項目が挿入されたときには item.text を含むテンプレート通知が送信されます。

2. ローカル コンピューターでファイルを編集するときは、サーバー プロジェクトを再発行します。
