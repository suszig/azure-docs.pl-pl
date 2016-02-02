1. Visual Studio で、更新、 `PostTodoItem` メソッド定義を次のコード。

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
    
         // Create the notification hub client.
         NotificationHubClient hub = NotificationHubClient
             .CreateClientFromConnectionString(notificationHubConnection, notificationHubName);
    
         // Get the current user SID and create a tag for the current user.
         var claimsPrincipal = this.User as ClaimsPrincipal;
         string sid = claimsPrincipal.FindFirst(ClaimTypes.NameIdentifier).Value;
         string userTag = "_UserId:" + sid;
    
         // Define push message text.
         var messageText = string.Format("New item added: {0}", item.Text);
    
         // Build a dictionary for the template.
         var notification = new Dictionary<string, string> { { "message", messageText } };
    
         try
         {
             // Send a template notification to the user ID.
             await hub.SendTemplateNotificationAsync(notification, userTag);
         }
         catch (System.Exception)
         {
             throw new HttpResponseException(System.Net.HttpStatusCode.InternalServerError);
         }
         return CreatedAtRoute("Tables", new { id = current.Id }, current);
     }

 このコードは、現在のユーザー SID でタグ付けされた登録のみにテンプレート通知を送信します。 SID 値は、認証プロバイダーのトークンから派生するので変更される可能性があります。

2. サーバー プロジェクトを発行します。






