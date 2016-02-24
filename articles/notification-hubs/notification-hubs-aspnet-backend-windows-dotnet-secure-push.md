<properties
    pageTitle="Azure Notification Hubs の安全なプッシュ"
    description="Azure でセキュリティで保護されたプッシュ通知を送信する方法について説明します。 コード サンプルは .NET API を使用して C# で記述されています。"
    documentationCenter="windows"
    authors="wesmc7777"
    manager="dwrede"
    editor=""
    services="notification-hubs"/>

<tags
    ms.service="notification-hubs" 
    ms.workload="mobile"
    ms.tgt_pltfrm="windows"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/05/2015"
    ms.author="wesmc"/>

#Azure Notification Hubs の安全なプッシュ

> [AZURE.SELECTOR]
- [Windows ユニバーサル](notification-hubs-windows-dotnet-secure-push.md)
- [iOS](notification-hubs-aspnet-backend-ios-secure-push.md)
- [Android](notification-hubs-aspnet-backend-android-secure-push.md)


##概要

Microsoft Azure でプッシュ通知がサポートされたことで、マルチプラットフォームに対応し、簡単に使用できる、スケールアウトされたプッシュ通知インフラストラクチャを利用できるようになりました。これにより、モバイル プラットフォーム向けアプリケーション (コンシューマー用途およびエンタープライズ用途) にプッシュ通知機能を実装する作業が大幅に簡略化されます。

規制やセキュリティの制約により、アプリケーションでは、標準のプッシュ通知インフラストラクチャからは転送できないものを通知に含める必要がある場合があります。 このチュートリアルでは、クライアントのデバイスとアプリケーションのバックエンドとの間の安全で認証された接続を通して機密情報を送信することによって、同じエクスペリエンスを実現する方法について説明します。

大まかには、フローは次のようになります。

1. アプリケーションのバックエンド:
    - バックエンド データベースに安全なペイロードを格納します。
    - この通知の ID をデバイスに送信します (安全でない情報は送信しません)。
2. 通知を受け取ったときのデバイスのアプリケーション:
    - デバイスは、安全なペイロードを要求するバックエンドにアクセスします。
    - アプリケーションはデバイスに通知としてペイロードを表示できます。

重要なのは、前のフロー (およびこのチュートリアル) では、デバイスは、ユーザーがログインした後、認証トークンをローカル ストレージに保存すると想定していることです。 デバイスはこのトークンを使用して通知の安全なペイロードを取得できるため、これによって完全にシームレスなエクスペリエンスが保証されます。 アプリケーションがデバイスに認証トークンを格納しない、またはそれらのトークンが期限切れの場合、デバイスのアプリケーションは、通知を受け取ったときにアプリケーションの起動を促す一般的な通知を表示する必要があります。 その後、アプリケーションはユーザーを認証し、通知ペイロードを表示します。

この安全なプッシュのチュートリアルでは、プッシュ通知を安全に送信する方法を説明します。 チュートリアルに基づいて記述、 [ユーザーへの通知](notification-hubs-aspnet-backend-windows-dotnet-notify-users.md) チュートリアルでは、最初にそのチュートリアルの手順を完了する必要がありますので。

> [AZURE.NOTE] このチュートリアルを作成し、」の説明に従って通知ハブを構成 [Notification Hubs (Windows ストア) の概要](notification-hubs-windows-store-dotnet-get-started.md)します。
また、Windows Phone 8.1 には (Windows Phone ではなく) Windows の資格情報が必要で、Windows Phone 8.0 または Silverlight 8.1 ではバックグラウンド タスクが機能しないことに注意してください。 Windows ストア アプリケーションの場合は、アプリケーションでロック画面が有効 (Appmanifest でチェック ボックスをオンにする) な場合にだけバックグラウンド タスクから通知を受信できます。

[AZURE.INCLUDE [notification-hubs-aspnet-backend-securepush](../../includes/notification-hubs-aspnet-backend-securepush.md)]

## Windows Phone プロジェクトを変更する

1.  **NotifyUserWindowsPhone** プロジェクトに、次のコードをプッシュ バック グラウンド タスクを登録する App.xaml.cs に追加します。 `OnLaunched()` メソッドの最後に次のコード行を追加します。

        RegisterBackgroundTask();

2. 引き続き App.xaml.cs で、`OnLaunched()` の直後に次のコードを追加します。

        private async void RegisterBackgroundTask()
        {
            if (!Windows.ApplicationModel.Background.BackgroundTaskRegistration.AllTasks.Any(i => i.Value.Name == "PushBackgroundTask"))
            {
                var result = await BackgroundExecutionManager.RequestAccessAsync();
                var builder = new BackgroundTaskBuilder();

                builder.Name = "PushBackgroundTask";
                builder.TaskEntryPoint = typeof(PushBackgroundComponent.PushBackgroundTask).FullName;
                builder.SetTrigger(new Windows.ApplicationModel.Background.PushNotificationTrigger());
                BackgroundTaskRegistration task = builder.Register();
            }
        }

3. App.xaml.cs ファイルの先頭に次の `using` ステートメントを追加します。

        using Windows.Networking.PushNotifications;
        using Windows.ApplicationModel.Background;

4.  **ファイル** Visual Studio のメニューをクリックして **すべて保存**します。

## プッシュ バックグラウンド コンポーネントを作成する

次の手順では、プッシュ バックグラウンド コンポーネントを作成します。

1. ソリューション エクスプ ローラーで、ソリューションの最上位ノードを右クリックし (**Solution SecurePush** ここでは)、順にクリックして **追加**, 、] をクリックし、 **新しいプロジェクト**します。

2. 展開 **ストア アプリ**, 、順にクリックして **Windows Phone アプリ**, 、クリックして **Windows ランタイム コンポーネント (Windows Phone)**します。 プロジェクトの名前 **PushBackgroundComponent**, 、順にクリック **OK** プロジェクトを作成します。

    ![][12]

3. ソリューション エクスプ ローラーで右クリックし、 **PushBackgroundComponent (Windows Phone 8.1)** プロジェクトををクリックし、 **追加**, 、クリックして **クラス**します。 新しいクラスの名前を **PushBackgroundTask.cs**します。 クリックして **追加** クラスを生成します。

4. 内容全体を置き換える、 **PushBackgroundComponent** 名前空間定義を次のコードで、プレース ホルダー `{back-end endpoint}` をバックエンドのデプロイ時に取得したバックエンド エンドポイント。

        public sealed class Notification
            {
                public int Id { get; set; }
                public string Payload { get; set; }
                public bool Read { get; set; }
            }

            public sealed class PushBackgroundTask : IBackgroundTask
            {
                private string GET_URL = "{back-end endpoint}/api/notifications/";

                async void IBackgroundTask.Run(IBackgroundTaskInstance taskInstance)
                {
                    // Store the content received from the notification so it can be retrieved from the UI.
                    RawNotification raw = (RawNotification)taskInstance.TriggerDetails;
                    var notificationId = raw.Content;

                    // retrieve content
                    BackgroundTaskDeferral deferral = taskInstance.GetDeferral();
                    var httpClient = new HttpClient();
                    var settings = ApplicationData.Current.LocalSettings.Values;
                    httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Basic", (string)settings["AuthenticationToken"]);

                    var notificationString = await httpClient.GetStringAsync(GET_URL + notificationId);

                    var notification = JsonConvert.DeserializeObject<Notification>(notificationString);

                    ShowToast(notification);

                    deferral.Complete();
                }

                private void ShowToast(Notification notification)
                {
                    ToastTemplateType toastTemplate = ToastTemplateType.ToastText01;
                    XmlDocument toastXml = ToastNotificationManager.GetTemplateContent(toastTemplate);
                    XmlNodeList toastTextElements = toastXml.GetElementsByTagName("text");
                    toastTextElements[0].AppendChild(toastXml.CreateTextNode(notification.Payload));
                    ToastNotification toast = new ToastNotification(toastXml);
                    ToastNotificationManager.CreateToastNotifier().Show(toast);
                }
            }

5. ソリューション エクスプ ローラーで右クリックし、 **PushBackgroundComponent (Windows Phone 8.1)** クリックしてプロジェクト **NuGet パッケージの管理**します。

6. 左側にある [クリックして **オンライン**します。

7.  **検索** ボックスに、入力 **Http クライアント**です。

8. 結果一覧でクリックして **Microsoft HTTP Client Libraries**, 、クリックして **インストール**します。 インストールを完了します。

9. NuGet に戻って **検索** ボックスに、入力 **Json.net**します。 インストール、 **Json.NET** をパッケージ化し、NuGet パッケージ マネージャー ウィンドウを閉じます。

10. 次の追加 `using` ステートメントの先頭に、 **PushBackgroundTask.cs** ファイル。

        using Windows.ApplicationModel.Background;
        using Windows.Networking.PushNotifications;
        using System.Net.Http;
        using Windows.Storage;
        using System.Net.Http.Headers;
        using Newtonsoft.Json;
        using Windows.UI.Notifications;
        using Windows.Data.Xml.Dom;

11. ソリューション エクスプ ローラーでの **NotifyUserWindowsPhone (Windows Phone 8.1)** プロジェクトを右クリックして **参照**, 、] をクリックし、 **参照の追加]**します。 参照マネージャー] ダイアログ ボックスで、チェック ボックスをオン] の横に **PushBackgroundComponent**, 、] をクリックし、 **OK**します。

12. ソリューション エクスプ ローラーでダブルクリック **Package.appxmanifest** で、 **NotifyUserWindowsPhone (Windows Phone 8.1)** プロジェクトです。  **通知**, 、設定されて **トースト対応** に **はい**します。

    ![][3]

13. まだ **Package.appxmanifest**, をクリックして、 **宣言** ] メニューの上部にあります。  **使用可能な宣言** ] ボックスの一覧をクリックして **バック グラウンド タスク**, 、順にクリック **追加**します。

14.  **Package.appxmanifest**, [ **プロパティ**, 、確認 **プッシュ通知を送信**します。

15.  **Package.appxmanifest**, [ **アプリ設定**, 、型 **PushBackgroundComponent.PushBackgroundTask** で、 **エントリ ポイント** フィールドです。

    ![][13]

16.  **ファイル** ] メニューをクリックして **すべてを保存**します。

## アプリケーションの実行

アプリケーションを実行するには、以下の手順に従います。

1. Visual Studio で、実行、 **AppBackend** Web API アプリケーションです。 ASP.NET Web ページが表示されます。

2. Visual Studio で、実行、 **NotifyUserWindowsPhone (Windows Phone 8.1)** Windows Phone アプリケーションです。 Windows Phone エミュレーターが自動的に起動し、アプリケーションを読み込みます。

3.  **NotifyUserWindowsPhone** アプリケーションの UI では、ユーザー名とパスワードを入力します。 文字列は任意ですが、値は同じである必要があります。

4.  **NotifyUserWindowsPhone** アプリケーションの UI でクリックして **にログインし、登録**します。 クリックして **Send push**します。

[3]: ./media/notification-hubs-aspnet-backend-windows-dotnet-secure-push/notification-hubs-secure-push3.png
[12]: ./media/notification-hubs-aspnet-backend-windows-dotnet-secure-push/notification-hubs-secure-push12.png
[13]: ./media/notification-hubs-aspnet-backend-windows-dotnet-secure-push/notification-hubs-secure-push13.png

