<properties
    pageTitle="Azure Notification Hubs と .NET バックエンドによるユーザーへの通知"
    description="Azure でセキュリティで保護されたプッシュ通知を送信する方法について説明します。 コード サンプルは .NET API を使用して C# で記述されています。"
    documentationCenter="windows"
    authors="wesmc7777"
    manager="dwrede"
    services="notification-hubs"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="11/09/2015"
    ms.author="wesmc"/>

#Azure Notification Hubs と .NET バックエンドによるユーザーへの通知

[AZURE.INCLUDE [notification-hubs-selector-aspnet-backend-notify-users](../../includes/notification-hubs-selector-aspnet-backend-notify-users.md)]


##概要

Azure でプッシュ通知がサポートされたことで、マルチプラットフォームに対応し、簡単に使用できる、スケールアウトされたプッシュ通知インフラストラクチャを利用できるようになりました。これにより、モバイル プラットフォーム向けアプリケーション (コンシューマー用途およびエンタープライズ用途) にプッシュ通知機能を実装する作業が大幅に簡略化されます。 このチュートリアルでは、Azure Notification Hubs を使用して特定のデバイスで特定のアプリケーション ユーザーにプッシュ通知を送信する方法について説明します。 クライアントを認証するために ASP.NET WebAPI バックエンドが使用されます。 認証されたクライアント ユーザーを使用して、タグがバックエンドによって通知登録に自動的に追加されます。 このタグは、バックエンドが特定のユーザーに対する通知を生成するための送信で使用されます。 アプリのバックエンドを使用して通知に登録する方法の詳細については、ガイダンスのトピックを参照してください。 [アプリ バックエンドから登録](http://msdn.microsoft.com/library/dn743807.aspx)します。 このチュートリアルでは、通知ハブをで作成したプロジェクトに基づいて、[開始通知ハブの使用] のチュートリアルです。

また、このチュートリアルは、[安全なプッシュ] このチュートリアルの前提条件です。 このチュートリアルでの手順を完了した後は、[安全なプッシュ] のチュートリアルでは、プッシュ通知を安全に送信するには、このチュートリアルのコードを変更する方法について説明に進むことができます。





## 開始する前に

マイクロソフトでは、お客様からのフィードバックを真剣に受け止めています。 このトピックを完了するうえで問題がある場合、またはこのコンテンツを改善するためのご提案がある場合には、ページの下部でフィードバックを送信できます。

このチュートリアルの完全なコードは GitHub でご覧 [ここ](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/NotifyUsers)します。 



##前提条件

このチュートリアルの前に、次の Mobile Services のチュートリアルを完了している必要があります。

+ [開始通知ハブの使用]<br/>アプリ名の予約、通知ハブを作成し、このチュートリアルで通知を受信するように登録します。 このチュートリアルでは、これらの手順を既に完了することを前提としています。 ない場合は、」の手順に従ってください [Notification Hubs (Windows ストア) の概要](notification-hubs-windows-store-dotnet-get-started.md)。 具体的には、セクションでは、 [アプリを Windows ストア登録](notification-hubs-windows-store-dotnet-get-started.md#register-your-app-for-the-windows-store) と [通知ハブを構成する](notification-hubs-windows-store-dotnet-get-started.md#configure-your-notification-hub)です。 具体的には、入力したことを確認、 **パッケージ SID** と **クライアント シークレット** のポータルで、値、 **構成** 通知ハブのタブをクリックします。 この構成手順で説明した [通知ハブを構成する](notification-hubs-windows-store-dotnet-get-started.md#configure-your-notification-hub)です。 これは重要な手順です。ポータルの資格情報が、選択したアプリケーション名に指定した資格情報と一致しない場合、プッシュ通知は成功しません。




> [AZURE.NOTE] バックエンド サービスとして Mobile Services を使用している場合、 [Mobile Services バージョン](../mobile-services-javascript-backend-windows-store-dotnet-push-notifications-app-users.md) このチュートリアルのです。




[AZURE.INCLUDE [notification-hubs-aspnet-backend-notifyusers](../../includes/notification-hubs-aspnet-backend-notifyusers.md)]

## クライアント プロジェクトのコードを更新する

完了したプロジェクトでコードを更新するこのセクションで、[開始通知ハブの使用] のチュートリアルです。 コードは、既にストアに関連付けられ、通知ハブ用に構成されているはずです。 このセクションでは、新しい WebAPI バックエンドを呼び出すコードを追加し、それを使用して通知の登録と送信を行います。

1. Visual Studio で開き、用に作成したソリューション、[開始通知ハブの使用] チュートリアルです。

2. ソリューション エクスプ ローラーで右クリックし、 **(Windows 8.1)** クリックしてプロジェクト **NuGet パッケージの管理**します。

3. 左側にある [クリックして **オンライン**します。

4.  **検索** ボックスに、入力 **Http クライアント**です。

5. 結果一覧でクリックして **Microsoft HTTP Client Libraries**, 、クリックして **インストール**します。 インストールを完了します。

6. NuGet に戻って **検索** ボックスに、入力 **Json.net**します。 インストール、 **Json.NET** パッケージを見つけて、[NuGet パッケージ マネージャー] ウィンドウを閉じます。

7. 前述の手順を繰り返して、 **(Windows Phone 8.1)** をインストールするプロジェクト、 **JSON.NET** Windows Phone プロジェクトの NuGet パッケージ。


8. ソリューション エクスプ ローラーで、 **(Windows 8.1)** プロジェクトをダブルクリックして **MainPage.xaml** を Visual Studio エディターで開きます。

9.  **MainPage.xaml** XML コードで置き換えます、 `<Grid>` セクションを次のコードです。 このコードは、ユーザーを認証するためのユーザー名とパスワードのテキスト ボックスを追加します。 さらに、通知メッセージと通知を受け取るユーザー名タグのテキスト ボックスも追加します。

        <Grid>
            <Grid.RowDefinitions>
                <RowDefinition Height="Auto"/>
                <RowDefinition Height="*"/>
            </Grid.RowDefinitions>

            <TextBlock Grid.Row="0" Text="Notify Users" HorizontalAlignment="Center" FontSize="48"/>

            <StackPanel Grid.Row="1" VerticalAlignment="Center">
                <Grid>
                    <Grid.RowDefinitions>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition Height="Auto"/>
                    </Grid.RowDefinitions>
                    <Grid.ColumnDefinitions>
                        <ColumnDefinition></ColumnDefinition>
                        <ColumnDefinition></ColumnDefinition>
                        <ColumnDefinition></ColumnDefinition>
                    </Grid.ColumnDefinitions>
                    <TextBlock Grid.Row="0" Grid.ColumnSpan="3" Text="Username" FontSize="24" Margin="20,0,20,0"/>
                    <TextBox Name="UsernameTextBox" Grid.Row="1" Grid.ColumnSpan="3" Margin="20,0,20,0"/>
                    <TextBlock Grid.Row="2" Grid.ColumnSpan="3" Text="Password" FontSize="24" Margin="20,0,20,0" />
                    <PasswordBox Name="PasswordTextBox" Grid.Row="3" Grid.ColumnSpan="3" Margin="20,0,20,0"/>

                    <Button Grid.Row="4" Grid.ColumnSpan="3" HorizontalAlignment="Center" VerticalAlignment="Center"
                                Content="1. Login and register" Click="LoginAndRegisterClick" Margin="0,0,0,20"/>

                    <ToggleButton Name="toggleWNS" Grid.Row="5" Grid.Column="0" HorizontalAlignment="Right" Content="WNS" IsChecked="True" />
                    <ToggleButton Name="toggleGCM" Grid.Row="5" Grid.Column="1" HorizontalAlignment="Center" Content="GCM" />
                    <ToggleButton Name="toggleAPNS" Grid.Row="5" Grid.Column="2" HorizontalAlignment="Left" Content="APNS" />

                    <TextBlock Grid.Row="6" Grid.ColumnSpan="3" Text="Username Tag To Send To" FontSize="24" Margin="20,0,20,0"/>
                    <TextBox Name="ToUserTagTextBox" Grid.Row="7" Grid.ColumnSpan="3" Margin="20,0,20,0" TextWrapping="Wrap" />
                    <TextBlock Grid.Row="8" Grid.ColumnSpan="3" Text="Enter Notification Message" FontSize="24" Margin="20,0,20,0"/>
                    <TextBox Name="NotificationMessageTextBox" Grid.Row="9" Grid.ColumnSpan="3" Margin="20,0,20,0" TextWrapping="Wrap" />
                    <Button Grid.Row="10" Grid.ColumnSpan="3" HorizontalAlignment="Center" Content="2. Send push" Click="PushClick" Name="SendPushButton" />
                </Grid>
            </StackPanel>
        </Grid>


10. ソリューション エクスプ ローラーでの **(Windows Phone 8.1)** プロジェクトを開き、 **MainPage.xaml** と Windows Phone 8.1 の置換 `<Grid>` 上同じコードでセクションです。 次のようなインターフェイスが表示されます。

    ![][13]

11. ソリューション エクスプ ローラーで開く、 **MainPage.xaml.cs** 用のファイル、 **(Windows 8.1)** と **(Windows Phone 8.1)** プロジェクトです。 次の `using` ステートメントを両方のファイルの先頭に追加します。

        using System.Net.Http;
        using Windows.Storage;
        using System.Net.Http.Headers;
        using Windows.Networking.PushNotifications;
        using Windows.UI.Popups;
        using System.Threading.Tasks;

12.  **MainPage.xaml.cs** の **(Windows 8.1)** と **(Windows Phone 8.1)** プロジェクトでは、追加の次のメンバーを `MainPage` クラスです。 必ず、前に取得したバックエンド エンドポイントで `<Enter Your Backend Endpoint>` を置き換えてください。 たとえば、「`http://mybackend.azurewebsites.net`」のように入力します。

        private static string BACKEND_ENDPOINT = "<Enter Your Backend Endpoint>";



13. 内の MainPage クラスに次のコードを追加 **MainPage.xaml.cs** の **(Windows 8.1)** と **(Windows Phone 8.1)** プロジェクトです。

     `PushClick` メソッドは、クリック ハンドラーの **プッシュを送信する** ] ボタンをクリックします。 それは、バックエンドを呼び出して、ユーザー名タグが `to_tag` パラメーターと一致するすべてのデバイスへの通知をトリガーします。 通知メッセージは、要求本文で JSON コンテンツとして送信されます。

     `LoginAndRegisterClick` メソッドは、クリック ハンドラーの **にログインし、登録** ] ボタンをクリックします。 それは、基本認証トークンをローカル ストレージに保存し (これは認証スキームが使用する任意のトークンを表すことに注意してください)、その後で `RegisterClient` を使用して、バックエンドを使用する通知を登録します。


        private async void PushClick(object sender, RoutedEventArgs e)
        {
            if (toggleWNS.IsChecked.Value)
            {
                await sendPush("wns", ToUserTagTextBox.Text, this.NotificationMessageTextBox.Text);
            }
            if (toggleGCM.IsChecked.Value)
            {
                await sendPush("gcm", ToUserTagTextBox.Text, this.NotificationMessageTextBox.Text);
            }
            if (toggleAPNS.IsChecked.Value)
            {
                await sendPush("apns", ToUserTagTextBox.Text, this.NotificationMessageTextBox.Text);

            }
        }

        private async Task sendPush(string pns, string userTag, string message)
        {
            var POST_URL = BACKEND_ENDPOINT + "/api/notifications?pns=" +
                pns + "&to_tag=" + userTag;

            using (var httpClient = new HttpClient())
            {
                var settings = ApplicationData.Current.LocalSettings.Values;
                httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Basic", (string)settings["AuthenticationToken"]);

                try
                {
                    await httpClient.PostAsync(POST_URL, new StringContent("\"" + message + "\"",
                        System.Text.Encoding.UTF8, "application/json"));
                }
                catch (Exception ex)
                {
                    MessageDialog alert = new MessageDialog(ex.Message, "Failed to send " + pns + " message");
                    alert.ShowAsync();
                }
            }
        }

        private async void LoginAndRegisterClick(object sender, RoutedEventArgs e)
        {
            SetAuthenticationTokenInLocalStorage();

            var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

            // The "username:<user name>" tag gets automatically added by the message handler in the backend.
            // The tag passed here can be whatever other tags you may want to use.
            try
            {
                // The device handle used will be different depending on the device and PNS. 
                // Windows devices use the channel uri as the PNS handle.
                await new RegisterClient(BACKEND_ENDPOINT).RegisterAsync(channel.Uri, new string[] { "myTag" });

                var dialog = new MessageDialog("Registered as: " + UsernameTextBox.Text);
                dialog.Commands.Add(new UICommand("OK"));
                await dialog.ShowAsync();
                SendPushButton.IsEnabled = true;
            }
            catch (Exception ex)
            {
                MessageDialog alert = new MessageDialog(ex.Message, "Failed to register with RegisterClient");
                alert.ShowAsync();
            }
        }

        private void SetAuthenticationTokenInLocalStorage()
        {
            string username = UsernameTextBox.Text;
            string password = PasswordTextBox.Password;

            var token = Convert.ToBase64String(System.Text.Encoding.UTF8.GetBytes(username + ":" + password));
            ApplicationData.Current.LocalSettings.Values["AuthenticationToken"] = token;
        }



14. ソリューション エクスプ ローラーで下にある、 **共有** プロジェクトを開き、 **App.xaml.cs** ファイルです。 `OnLaunched()` イベント ハンドラーで、`InitNotificationsAsync()` への呼び出しを見つけます。 `InitNotificationsAsync()` への呼び出しをコメント アウトするか削除します。 上で追加したボタン ハンドラーが通知登録を初期化します。


        protected override void OnLaunched(LaunchActivatedEventArgs e)
        {
            //InitNotificationsAsync();


15. ソリューション エクスプ ローラーで右クリックし、 **共有** プロジェクトををクリックし、 **追加**, 、クリックして **クラス**します。 クラスに名前を **RegisterClient.cs**, 、順にクリックして **OK** クラスを生成します。

    このクラスは、アプリ バックエンドに接続してプッシュ通知を登録するために必要な REST 呼び出しをラップします。 これもローカルに格納されている、 *registrationIds* で詳述するよう通知ハブによって作成された [アプリ バックエンドから登録](http://msdn.microsoft.com/library/dn743807.aspx)します。 メモをクリックすると、ローカル ストレージに格納した認証トークンが使用される、 **にログインし、登録** ] ボタンをクリックします。


16. 次の `using` ステートメントを、RegisterClient.cs file ファイルの先頭に追加します。

        using Windows.Storage;
        using System.Net;
        using System.Net.Http;
        using System.Net.Http.Headers;
        using Newtonsoft.Json;
        using System.Threading.Tasks;
        using System.Linq;

17. `RegisterClient` クラス定義内で、次のコードを追加します。

        private string POST_URL;

        private class DeviceRegistration
        {
            public string Platform { get; set; }
            public string Handle { get; set; }
            public string[] Tags { get; set; }
        }

        public RegisterClient(string backendEndpoint)
        {
            POST_URL = backendEndpoint + "/api/register";
        }

        public async Task RegisterAsync(string handle, IEnumerable<string> tags)
        {
            var regId = await RetrieveRegistrationIdOrRequestNewOneAsync();

            var deviceRegistration = new DeviceRegistration
            {
                Platform = "wns",
                Handle = handle,
                Tags = tags.ToArray<string>()
            };

            var statusCode = await UpdateRegistrationAsync(regId, deviceRegistration);

            if (statusCode == HttpStatusCode.Gone)
            {
                // regId is expired, deleting from local storage & recreating
                var settings = ApplicationData.Current.LocalSettings.Values;
                settings.Remove("__NHRegistrationId");
                regId = await RetrieveRegistrationIdOrRequestNewOneAsync();
                statusCode = await UpdateRegistrationAsync(regId, deviceRegistration);
            }

            if (statusCode != HttpStatusCode.Accepted)
            {
                // log or throw
                throw new System.Net.WebException(statusCode.ToString());
            }
        }

        private async Task<HttpStatusCode> UpdateRegistrationAsync(string regId, DeviceRegistration deviceRegistration)
        {
            using (var httpClient = new HttpClient())
            {
                var settings = ApplicationData.Current.LocalSettings.Values;
                httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Basic", (string) settings["AuthenticationToken"]);

                var putUri = POST_URL + "/" + regId;

                string json = JsonConvert.SerializeObject(deviceRegistration);
                                var response = await httpClient.PutAsync(putUri, new StringContent(json, Encoding.UTF8, "application/json"));
                return response.StatusCode;
            }
        }

        private async Task<string> RetrieveRegistrationIdOrRequestNewOneAsync()
        {
            var settings = ApplicationData.Current.LocalSettings.Values;
            if (!settings.ContainsKey("__NHRegistrationId"))
            {
                using (var httpClient = new HttpClient())
                {
                    httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Basic", (string)settings["AuthenticationToken"]);

                    var response = await httpClient.PostAsync(POST_URL, new StringContent(""));
                    if (response.IsSuccessStatusCode)
                    {
                        string regId = await response.Content.ReadAsStringAsync();
                        regId = regId.Substring(1, regId.Length - 2);
                        settings.Add("__NHRegistrationId", regId);
                    }
                    else
                    {
                        throw new System.Net.WebException(response.StatusCode.ToString());
                    }
                }
            }
            return (string)settings["__NHRegistrationId"];

        }

18. すべての変更を保存します。


## アプリケーションのテスト

1. Windows 8.1 と Windows Phone 8.1 の両方でアプリケーションを起動します。 Windows Phone 8.1 では、エミュレーターまたは実際のデバイスでインスタンスを実行できます。

2. アプリの Windows 8.1 インスタンスで入力して、 **ユーザー名** と **パスワード** 次の画面に示すようにします。 Windows Phone で入力するユーザー名とパスワードとは異なるユーザー名とパスワードを入力する必要があります。


3. をクリックして **にログインし、登録** ログインしたことを示すダイアログを確認します。 これにより、また、 **プッシュを送信する** ] ボタンをクリックします。

    ![][14]

4. Windows Phone 8.1 インスタンスで両方のユーザー名の文字列を入力して、 **ユーザー名** と **パスワード** フィールドをクリックし、 **ログインして登録**します。
5. 次に、 **Recipient Username Tag** フィールドに、Windows 8.1 に登録されているユーザー名を入力します。 通知メッセージを入力し、クリックして **プッシュを送信する**です。

    ![][16]

6. 一致するユーザー名タグが登録されているデバイスだけが通知メッセージを受信します。

    ![][15]

## 次のステップ

* 対象グループごとにユーザーをセグメント化する場合は、[通知ハブを使用したニュース速報の送信] を参照してください。
* Notification Hubs を使用する方法の詳細については、[通知ハブの概要] を参照してください。



[9]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-secure-push9.png
[10]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-secure-push10.png
[11]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-secure-push11.png
[12]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-secure-push12.png
[13]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-wp-ui.png
[14]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-windows-instance-username.png
[15]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-notification-received.png
[16]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-wp-send-message.png



<!-- URLs. -->
[Get started with Notification Hubs]: notification-hubs-windows-store-dotnet-get-started.md
[Secure Push]: notification-hubs-aspnet-backend-windows-dotnet-secure-push.md
[Use Notification Hubs to send breaking news]: notification-hubs-windows-store-dotnet-send-breaking-news.md
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx

