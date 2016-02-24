## Web API プロジェクトを作成する

新しい ASP.NET WebAPI バックエンドを次のセクションで作成します。その目的には主に次の 3 つがあります。

1. **クライアントの認証**: クライアント要求の認証を要求にユーザーを関連付けるメッセージ ハンドラーが後で追加されます。
2. **クライアント通知の登録**: 後で、クライアント デバイスで通知を受信するための新しい登録を処理するコント ローラーに追加されます。 として登録には、認証されたユーザー名が自動的に追加する、 [タグ](https://msdn.microsoft.com/library/azure/dn530749.aspx)します。
3. **クライアントに通知を送信する**: 後でも追加するためのユーザーがタグに関連付けられているクライアントとデバイスへの安全なプッシュをトリガーできるコント ローラーです。 

次の手順では、新しい ASP.NET WebAPI バックエンドを作成する方法を説明します。 


> [AZURE.NOTE] **重要**: このチュートリアルを開始する前に確認してください、最新の NuGet パッケージ マネージャーがインストールされていること。 確認するには、Visual Studio を起動します。  **ツール** ] メニューをクリックして **拡張機能と更新プログラム**します。 検索 **Visual Studio 2013 用の NuGet パッケージ マネージャー**, 、おくバージョンが 2.8.50313.46 以降。 違う場合は、アンインストールしてから、NuGet パッケージ マネージャーをもう一度インストールしてください。
> 
> ![][B4]

> [AZURE.NOTE] Visual Studio がインストールされている確認 [Azure SDK](http://azure.microsoft.com/downloads/) の web サイトを展開します。

1. Visual Studio または Visual Studio Express を起動します。 クリックして **サーバー エクスプ ローラー** し、Azure アカウントにサインインします。 Visual Studio でアカウントの Web サイト リソースを作成するには、サインインする必要があります。
2. Visual Studio で、[ **ファイル**, 、順にクリックして **新規**, 、し、 **プロジェクト**, 、展開 **テンプレート**, 、**Visual c#**, 、順にクリックして **Web** と **ASP.NET Web アプリケーション**, 、名前を入力します **AppBackend**, 、順にクリック **[ok]**します。 
    
    ![][B1]

3.  **新しい ASP.NET プロジェクト** ダイアログ ボックスで、をクリックして **Web API**, 、] をクリックし、 **[ok]**します。

    ![][B2]

4.  **Microsoft Azure Web アプリの構成** ] ダイアログ ボックスで、サブスクリプションを選択して **App Service プラン** をまだ作成しています。 選択することもできます **新しい app service プランを作成** し、ダイアログ ボックスから 1 つを作成します。 このチュートリアルではデータベースは必要ありません。 クリックして、app service プランを選択すると **OK** プロジェクトを作成します。

    ![][B5]



## WebAPI バックエンドでクライアントを認証する

このセクションでは、という名前の新しいメッセージ ハンドラー クラスを作成します **AuthenticationTestHandler** 、新しいバックエンドに対してです。 このクラスから派生 [DelegatingHandler](https://msdn.microsoft.com/library/system.net.http.delegatinghandler.aspx) されため、バックエンドに送信されるすべての要求を処理できるメッセージ ハンドラーとして追加します。 



1. ソリューション エクスプ ローラーで右クリックし、 **AppBackend** プロジェクトをクリックして **追加**, 、] をクリックし、 **クラス**します。 新しいクラスの名前を **AuthenticationTestHandler.cs**, 、] をクリック **追加** クラスを生成します。 このクラスを使用してユーザーを認証に使用する *基本認証* わかりやすくするためです。 実際のアプリでは、任意の認証スキームを使用できます。

2. AuthenticationTestHandler.cs に次の `using` ステートメントを追加します。

        using System.Net.Http;
        using System.Threading;
        using System.Security.Principal;
        using System.Net;
        using System.Web;

3. AuthenticationTestHandler.cs で、`AuthenticationTestHandler` クラス定義を次のコードに置き換えます。 

    次の 3 つの条件がすべて該当する場合、このハンドラーは、要求を承認します。
    * 要求が含まれている、 *承認* ヘッダー。 
    * 要求を使用して *基本的な* 認証します。 
    * ユーザー名の文字列とパスワードの文字列には、同じ文字列があります。

    それ以外の場合、要求は拒否されます。 これは正規の認証や認証アプローチではありません。 このチュートリアルでの非常に単純な例です。

    要求メッセージが認証されが承認されている場合、 `AuthenticationTestHandler`, 、基本認証ユーザーは、現在の要求に添付されますが、 [HttpContext](https://msdn.microsoft.com/library/system.web.httpcontext.current.aspx)します。 HttpContext のユーザー情報には追加を後でもう 1 つのコント ローラー (RegisterController) で使用する [タグ](https://msdn.microsoft.com/library/azure/dn530749.aspx) 通知登録の要求。

        public class AuthenticationTestHandler : DelegatingHandler
        {
            protected override Task<HttpResponseMessage> SendAsync(
            HttpRequestMessage request, CancellationToken cancellationToken)
            {
                var authorizationHeader = request.Headers.GetValues("Authorization").First();
    
                if (authorizationHeader != null && authorizationHeader
                    .StartsWith("Basic ", StringComparison.InvariantCultureIgnoreCase))
                {
                    string authorizationUserAndPwdBase64 =
                        authorizationHeader.Substring("Basic ".Length);
                    string authorizationUserAndPwd = Encoding.Default
                        .GetString(Convert.FromBase64String(authorizationUserAndPwdBase64));
                    string user = authorizationUserAndPwd.Split(':')[0];
                    string password = authorizationUserAndPwd.Split(':')[1];
    
                    if (verifyUserAndPwd(user, password))
                    {
                        // Attach the new principal object to the current HttpContext object
                        HttpContext.Current.User =
                            new GenericPrincipal(new GenericIdentity(user), new string[0]);
                        System.Threading.Thread.CurrentPrincipal =
                            System.Web.HttpContext.Current.User;
                    }
                    else return Unauthorized();
                }
                else return Unauthorized();
    
                return base.SendAsync(request, cancellationToken);
            }
    
            private bool verifyUserAndPwd(string user, string password)
            {
                // This is not a real authentication scheme.
                return user == password;
            }
    
            private Task<HttpResponseMessage> Unauthorized()
            {
                var response = new HttpResponseMessage(HttpStatusCode.Forbidden);
                var tsc = new TaskCompletionSource<HttpResponseMessage>();
                tsc.SetResult(response);
                return tsc.Task;
            }
        }

    > [AZURE.NOTE] **セキュリティに関する注意**: `AuthenticationTestHandler` クラスは、本当の認証を提供しません。 基本認証を模倣するためだけに使用されるため、安全ではありません。 実稼働のアプリケーションとサービスでは、セキュリティで保護された認証メカニズムを実装する必要があります。               

4. 末尾に次のコードを追加、 `Register` メソッドで、 **App_Start/WebApiConfig.cs** メッセージ ハンドラーを登録するクラス。

        config.MessageHandlers.Add(new AuthenticationTestHandler());

5. 変更を保存します。

## WebAPI バックエンドを使用して通知を登録する

このセクションでは、Notification Hubs のクライアント ライブラリを使用して、通知用にユーザーとデバイスを登録する要求を処理する新しいコントローラーを WebAPI バックエンドに追加します。 コントローラーでは、`AuthenticationTestHandler` で認証され、HttpContext に添付されたユーザーにユーザー タグを追加します。 タグは、文字列の形式 `"username:<actual username>"` になります。


 

1. ソリューション エクスプ ローラーで右クリックし、 **AppBackend** クリックしてプロジェクト **NuGet パッケージの管理**します。

2. 左側にあるをクリックして **オンライン**, 、検索と **Microsoft.Azure.NotificationHubs** で、 **検索** ボックス。

3. 結果一覧でクリックして **Microsoft Azure Notification Hubs**, 、] をクリックし、 **インストール**します。 インストールが完了したら、NuGet パッケージ マネージャーのウィンドウを閉じます。

    使用して、Azure Notification Hubs SDK への参照を追加します <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">Microsoft.Azure.Notification ハブ NuGet パッケージ</a>.

4. 送信される、セキュリティで保護された別の通知を表す新しいクラス ファイルを作成します。 完全な実装では、通知はデータベースに格納されます。 わかりやすくするために、このチュートリアルではメモリに格納します。 ソリューション エクスプ ローラーで右クリックし、 **モデル** フォルダー] をクリックして **追加**, 、] をクリックし、 **クラス**します。 新しいクラスの名前を **Notifications.cs**, 、クリックして **追加** クラスを生成します。 

    ![][B6]

5. Notifications.cs で、ファイルの先頭に次の `using` ステートメントを追加します。

        using Microsoft.Azure.NotificationHubs;

6. 置換、 `Notifications` クラスの次のように定義してに 2 つのプレース ホルダーを通知ハブとハブの名前の (フル アクセス) を持つ接続文字列に置き換えてください (で使用可能な [Azure Classic Portal](http://manage.windowsazure.com))。

        public class Notifications
        {
            public static Notifications Instance = new Notifications();
        
            public NotificationHubClient Hub { get; set; }

            private Notifications() {
                Hub = NotificationHubClient.CreateClientFromConnectionString("<your hub's DefaultFullSharedAccessSignature>", 
                                                                             "<hub name>");
            }
        }



7. 次に、私たちはという名前の新しいコント ローラーを作成します **RegisterController**します。 ソリューション エクスプ ローラーで右クリックし、 **コント ローラー** フォルダーを順にクリックして **追加**, 、] をクリックし、 **コント ローラー**します。 クリックして、 **Web API 2 コント ローラー--空** 項目をクリックして **追加**します。 新しいクラスの名前を **RegisterController**, 、クリックして **追加** コント ローラーを生成するためです。

    ![][B7]

    ![][B8]

8. RegisterController.cs に次の `using` ステートメントを追加します。

        using Microsoft.Azure.NotificationHubs;
        using Microsoft.Azure.NotificationHubs.Messaging;
        using AppBackend.Models;
        using System.Threading.Tasks;
        using System.Web;

9. `RegisterController` クラス定義内で、次のコードを追加します。 このコードでは、HttpContext に添付されるユーザーのユーザー タグを追加することに注意してください。 ユーザーは認証され、追加したメッセージ フィルター `AuthenticationTestHandler` で HttpContext に添付されます。 また、要求されたタグを登録する権限をユーザーが持っていることを確認するコードをオプションで追加することもできます。

        private NotificationHubClient hub;

        public RegisterController()
        {
            hub = Notifications.Instance.Hub;
        }

        public class DeviceRegistration
        {
            public string Platform { get; set; }
            public string Handle { get; set; }
            public string[] Tags { get; set; }
        }

        // POST api/register
        // This creates a registration id
        public async Task<string> Post(string handle = null)
        {
            string newRegistrationId = null;
            
            // make sure there are no existing registrations for this push handle (used for iOS and Android)
            if (handle != null)
            {
                var registrations = await hub.GetRegistrationsByChannelAsync(handle, 100);

                foreach (RegistrationDescription registration in registrations)
                {
                    if (newRegistrationId == null)
                    {
                        newRegistrationId = registration.RegistrationId;
                    }
                    else
                    {
                        await hub.DeleteRegistrationAsync(registration);
                    }
                }
            }

            if (newRegistrationId == null) 
                newRegistrationId = await hub.CreateRegistrationIdAsync();

            return newRegistrationId;
        }

        // PUT api/register/5
        // This creates or updates a registration (with provided channelURI) at the specified id
        public async Task<HttpResponseMessage> Put(string id, DeviceRegistration deviceUpdate)
        {
            RegistrationDescription registration = null;
            switch (deviceUpdate.Platform)
            {
                case "mpns":
                    registration = new MpnsRegistrationDescription(deviceUpdate.Handle);
                    break;
                case "wns":
                    registration = new WindowsRegistrationDescription(deviceUpdate.Handle);
                    break;
                case "apns":
                    registration = new AppleRegistrationDescription(deviceUpdate.Handle);
                    break;
                case "gcm":
                    registration = new GcmRegistrationDescription(deviceUpdate.Handle);
                    break;
                default:
                    throw new HttpResponseException(HttpStatusCode.BadRequest);
            }

            registration.RegistrationId = id;
            var username = HttpContext.Current.User.Identity.Name;

            // add check if user is allowed to add these tags
            registration.Tags = new HashSet<string>(deviceUpdate.Tags);
            registration.Tags.Add("username:" + username);

            try
            {
                await hub.CreateOrUpdateRegistrationAsync(registration);
            }
            catch (MessagingException e)
            {
                ReturnGoneIfHubResponseIsGone(e);
            }

            return Request.CreateResponse(HttpStatusCode.OK);
        }

        // DELETE api/register/5
        public async Task<HttpResponseMessage> Delete(string id)
        {
            await hub.DeleteRegistrationAsync(id);
            return Request.CreateResponse(HttpStatusCode.OK);
        }

        private static void ReturnGoneIfHubResponseIsGone(MessagingException e)
        {
            var webex = e.InnerException as WebException;
            if (webex.Status == WebExceptionStatus.ProtocolError)
            {
                var response = (HttpWebResponse)webex.Response;
                if (response.StatusCode == HttpStatusCode.Gone)
                    throw new HttpRequestException(HttpStatusCode.Gone.ToString());
            }
        }

10. 変更を保存します。

## WebAPI バックエンドから通知を送信する

このセクションでは、ASP.NET WebAPI バックエンドの Azure Notification Hubs サービス管理ライブラリを使用して、ユーザー名タグに基づく通知を送信する手段をクライアント デバイスに提供する新しいコントローラーを追加します。


1. もう 1 つの新しいコント ローラーがという名前を作成 **NotificationsController**します。 作成したときと同じ方法で作成、 **RegisterController** 前のセクションでします。

2. NotificationsController.cs に次の `using` ステートメントを追加します。

        using AppBackend.Models;
        using System.Threading.Tasks;
        using System.Web;

3. 次のメソッドを追加、 **NotificationsController** クラスです。

    このコードでは、プラットフォーム通知サービス (PNS) の `pns` パラメーターに基づく通知の種類を送信します。 値 `to_tag` 設定に使用される、 *username* メッセージにタグ付けします。 このタグは、アクティブな通知ハブ登録のユーザー名のタグと一致する必要があります。 通知メッセージは、POST 要求の本文からプルされ、ターゲット PNS に合わせた形式に設定されます。 

    サポートされているデバイスで通知の受信に使用されるプラットフォーム通知サービス (PNS) に応じて、各種形式を使用したさまざまな通知がサポートされています。 たとえば、Windows デバイスを使用できる、 [WNS と通知をトースト](https://msdn.microsoft.com/library/windows/apps/br230849.aspx) 別 PNS では直接サポートされないものです。 したがって、バックエンドが、通知を、サポートを計画しているデバイスの PNS でサポートされている形式に設定する必要があります。 上の適切な送信 API を使用して、 [NotificationHubClient クラス](https://msdn.microsoft.com/library/azure/microsoft.azure.notificationhubs.notificationhubclient_methods.aspx)

        public async Task<HttpResponseMessage> Post(string pns, [FromBody]string message, string to_tag)
        {
            var user = HttpContext.Current.User.Identity.Name;
            string[] userTag = new string[2];
            userTag[0] = "username:" + to_tag;
            userTag[1] = "from:" + user;

            Microsoft.Azure.NotificationHubs.NotificationOutcome outcome = null;
            HttpStatusCode ret = HttpStatusCode.InternalServerError;

            switch (pns.ToLower())
            {
                case "wns":
                    // Windows 8.1 / Windows Phone 8.1
                    var toast = @"<toast><visual><binding template=""ToastText01""><text id=""1"">" + 
                                "From " + user + ": " + message + "</text></binding></visual></toast>";
                    outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, userTag);
                    break;
                case "apns":
                    // iOS
                    var alert = "{\"aps\":{\"alert\":\"" + "From " + user + ": " + message + "\"}}";
                    outcome = await Notifications.Instance.Hub.SendAppleNativeNotificationAsync(alert, userTag);
                    break;
                case "gcm":
                    // Android
                    var notif = "{ \"data\" : {\"message\":\"" + "From " + user + ": " + message + "\"}}";
                    outcome = await Notifications.Instance.Hub.SendGcmNativeNotificationAsync(notif, userTag);
                    break;
            }

            if (outcome != null)
            {
                if (!((outcome.State == Microsoft.Azure.NotificationHubs.NotificationOutcomeState.Abandoned) ||
                    (outcome.State == Microsoft.Azure.NotificationHubs.NotificationOutcomeState.Unknown)))
                {
                    ret = HttpStatusCode.OK;
                }
            }

            return Request.CreateResponse(ret);
        }


4. キーを押して **f5 キーを押して** アプリケーションを実行し、これまで、正しく作業したことを確認します。 アプリにより、Web ブラウザーが起動し、ASP.NET ホーム ページが表示される必要があります。 

##新しい WebAPI バックエンドを発行する

1. 次に、このアプリを Azure の Web サイトにデプロイして、すべてのデバイスからアクセスできるようにします。 右クリックし、 **AppBackend** プロジェクトし、選択 **発行**します。

2. 選択 **Microsoft Azure Web Apps** 発行先として。

    ![][B15]

3. Azure アカウントでログインし、既存または新規の Web アプリを選択します。

    ![][B16]

4. 書き留めて、 **送信先 URL** プロパティに、 **接続** ] タブをクリックします。 この URL として参照、 *バックエンド エンドポイント* このチュートリアルで後述します。 クリックして **発行**します。

    ![][B18]


[B1]: ./media/notification-hubs-aspnet-backend-notifyusers/notification-hubs-secure-push1.png
[B2]: ./media/notification-hubs-aspnet-backend-notifyusers/notification-hubs-secure-push2.png
[B3]: ./media/notification-hubs-aspnet-backend-notifyusers/notification-hubs-secure-push3.png
[B4]: ./media/notification-hubs-aspnet-backend-notifyusers/notification-hubs-secure-push4.png
[B5]: ./media/notification-hubs-aspnet-backend-notifyusers/notification-hubs-secure-push5.png
[B6]: ./media/notification-hubs-aspnet-backend-notifyusers/notification-hubs-secure-push6.png
[B7]: ./media/notification-hubs-aspnet-backend-notifyusers/notification-hubs-secure-push7.png
[B8]: ./media/notification-hubs-aspnet-backend-notifyusers/notification-hubs-secure-push8.png
[B14]: ./media/notification-hubs-aspnet-backend-notifyusers/notification-hubs-secure-push14.png
[B15]: ./media/notification-hubs-aspnet-backend-notifyusers/notification-hubs-notify-users15.PNG
[B16]: ./media/notification-hubs-aspnet-backend-notifyusers/notification-hubs-notify-users16.PNG
[B18]: ./media/notification-hubs-aspnet-backend-notifyusers/notification-hubs-notify-users18.PNG

