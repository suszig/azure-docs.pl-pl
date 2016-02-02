バックエンド プロジェクトの種類 & mdash に一致する手順を使用しているいずれか [.NET バックエンド](#dotnet) または [Node.js バックエンド](#nodejs)します。

### <a name="dotnet"></a>.NET バックエンド プロジェクト

1. Visual Studio で、サーバー プロジェクトを右クリックし、をクリックして **NuGet パッケージの管理**, 、検索 `Microsoft.Azure.NotificationHubs`, 、クリックして **インストール**します。 これにより、Notification Hubs のクライアント ライブラリがインストールされます。

2. Controllers フォルダーで TodoItemController.cs を開き、次の追加 `を使用して` ステートメント。

        using Microsoft.Azure.Mobile.Server.Config;
        using Microsoft.Azure.NotificationHubs;

3. 置き換える、 `PostTodoItem` メソッドを次のコード。

     public async Task<IHttpActionResult> PostTodoItem(TodoItem item)
     {
         TodoItem current = await InsertAsync(item);
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
    
         // Android payload
         var androidNotificationPayload = "{ \"data\" : {\"message\":\"" + item.Text + "\"}}";
    
         try
         {
             // Send the push notification and log the results.
             var result = await hub.SendGcmNativeNotificationAsync(androidNotificationPayload);
    
             // Write the success result to the logs.
             config.Services.GetTraceWriter().Info(result.State.ToString());
         }
         catch (System.Exception ex)
         {
             // Write the failure result to the logs.
             config.Services.GetTraceWriter()
                 .Error(ex.Message, null, "Push.SendAsync Error");
         }
         return CreatedAtRoute("Tables", new { id = current.Id }, current);
     }


### <a name="nodejs"></a>Node.js バックエンド プロジェクト

1. これをまだ実行していない場合 [クイック スタート プロジェクトをダウンロード](app-service-mobile-node-backend-how-to-use-server-sdk.md#download-quickstart) またはその他の使用、 [Azure ポータルでのオンライン エディター](app-service-mobile-node-backend-how-to-use-server-sdk.md#online-editor)します。

1. todoitem.js ファイル内の既存のコードを次のコードに置き換えます。

     var azureMobileApps = require('azure-mobile-apps'),
     promises = require('azure-mobile-apps/src/utilities/promises'),
     logger = require('azure-mobile-apps/src/logger');
    
     var table = azureMobileApps.table();
    
     table.insert(function (context) {
     // For more information about the Notification Hubs JavaScript SDK, 
     // see http://aka.ms/nodejshubs
     logger.info('Running TodoItem.insert');
    
     // Define the GCM payload.
     var payload = {
         "data": {
             "message": context.item.text
         }
     };   
    
     // Execute the insert.  The insert returns the results as a Promise,
     // Do the push as a post-execute action within the promise flow.
     return context.execute()
         .then(function (results) {
             // Only do the push if configured
             if (context.push) {
                 // Send a GCM native notification.
                 context.push.gcm.send(null, payload, function (error) {
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

 これにより、新しい ToDo 項目が挿入されたときには item.text を含む GCM 通知が送信されます。

2. ローカル コンピューターでファイルを編集するときは、サーバー プロジェクトを再発行します。





