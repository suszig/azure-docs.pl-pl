<properties 
    pageTitle="Mobile Services アプリケーション へのプッシュ通知の追加 (Windows Phone) | Microsoft Azure" 
    description="Azure Mobile Services と Notification Hubs を使用して Windows Phone アプリにプッシュ通知を送信する方法について説明します。" 
    services="mobile-services,notification-hubs" 
    documentationCenter="windows" 
    authors="ggailey777" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="mobile-services" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-windows-phone" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="12/07/2015" 
    ms.author="glenga"/>



# Mobile Services アプリへのプッシュ通知の追加

[AZURE.INCLUDE [mobile-services-selector-get-started-push](../../includes/mobile-services-selector-get-started-push.md)]

## 概要

このトピックでは、Azure Mobile Services を使用して Windows Phone Silverlight アプリケーションにプッシュ通知を送信する方法について説明します。 このチュートリアルでは、クイック スタート プロジェクトへの Azure Notification Hubs を使用したプッシュ通知を有効にします。 完了すると、モバイル サービスは、レコードが挿入されるたびに Notification Hubs を使用してプッシュ通知を送信します。 作成する通知ハブはモバイル サービスでは無料で、モバイル サービスから独立して管理することができ、他のアプリケーションおよびサービスで使用できます。

このチュートリアルは、TodoList サンプル アプリケーションに基づいています。 このチュートリアルを開始する前に、プロジェクトに接続して、モバイル サービスにトピック [Mobile Services を追加、既存のアプリケーション] を完了する必要があります。 モバイル サービスが接続されていない場合は、プッシュ通知の追加ウィザードによってこの接続を作成できます。
>[AZURE.NOTE]Windows Phone 8.1 ストア アプリにプッシュ通知を送信するには、次の [Windows ストア アプリ](../mobile-services-javascript-backend-windows-store-dotnet-get-started-push.md) このチュートリアルのバージョン。

## <a id="update-app"></a> アプリケーションを更新する通知に登録するには

アプリケーションがプッシュ通知を受信するには、通知チャネルを登録する必要があります。

1. Visual Studio で App.xaml.cs ファイルを開き、次の追加 `を使用して` ステートメント。

        using Microsoft.Phone.Notification;

3. 次のコードを App.xaml.cs に追加します。

     public static HttpNotificationChannel CurrentChannel { get; private set; }
    
     private void AcquirePushChannel()
     {
         CurrentChannel = HttpNotificationChannel.Find("MyPushChannel");
    
         if (CurrentChannel == null)
         {
             CurrentChannel = new HttpNotificationChannel("MyPushChannel");
             CurrentChannel.Open();
             CurrentChannel.BindToShellToast();
         }
    
         CurrentChannel.ChannelUriUpdated +=
             new EventHandler<NotificationChannelUriEventArgs>(async (o, args) =>
             {
    
                 // Register for notifications using the new channel
                 await MobileService.GetPush()
                     .RegisterNativeAsync(CurrentChannel.ChannelUri.ToString());
             });
     }

 このコードは Windows Phone 8.x "Silverlight" で使用される Microsoft Push Notification Service (MPNS) からアプリの ChannelURI を取得し、それをプッシュ通知用に登録します。
 >[AZURE.NOTE]このチュートリアルでは、モバイル サービスにより、トースト通知がデバイスに送信されます。 タイル通知を送信する場合は、チャネルの **BindToShellTile** メソッドを呼び出す必要があります。

4. App.xaml.cs 内で、**Application_Launching** イベント ハンドラーの先頭に、次に示す新しい **AcquirePushChannel** メソッドへの呼び出しを追加します。

        AcquirePushChannel();

    これにより、ページが読み込まれるたびに登録が要求されるようになります。 アプリケーションでは、この登録が常に最新の状態となるように、定期的な登録のみ行うことができます。

5. **F5** キーを押してアプリケーションを実行します。 登録キーを示すポップアップ ダイアログが表示されます。

6.  ソリューション エクスプローラーで、[**プロパティ**] を展開して WMAppManifest.xml ファイルを開き、[**機能**] タブをクリックして、**ID___CAP___PUSH_NOTIFICATION** 機能がオンであることを確認します。

    ![VS で通知を有効にする](./media/mobile-services-javascript-backend-windows-phone-get-started-push/mobile-app-enable-push-wp8.png)

    これにより、アプリケーションでトースト通知の使用が有効になります。

## <a id="update-scripts"></a> プッシュ通知を送信するサーバー スクリプトを更新します。

最後に、通知を送信するためには、TodoItem テーブルの挿入操作に登録されているスクリプトを更新する必要があります。

1. **[TodoItem]** をクリックし、**[スクリプト]** タブをクリックして、**[挿入]** を選択します。

2. insert 関数を次のコードに置き換え、**[保存]** をクリックします。

     function insert(item, user, request) {
     // Define a payload for the Windows Phone toast notification.
     var payload = '<?xml version="1.0" encoding="utf-8"?>' +
         '<wp:Notification xmlns:wp="WPNotification"><wp:Toast>' +
         '<wp:Text1>New Item</wp:Text1><wp:Text2>' + item.text + 
         '</wp:Text2></wp:Toast></wp:Notification>';
    
     request.execute({
         success: function() {
             // If the insert succeeds, send a notification.
             push.mpns.send(null, payload, 'toast', 22, {
                 success: function(pushResponse) {
                     console.log("Sent push:", pushResponse);
                     request.respond();
                     },              
                     error: function (pushResponse) {
                         console.log("Error Sending push:", pushResponse);
                         request.respond(500, { error: pushResponse });
                         }
                     });
                 }
             });      
     }

 この insert スクリプトによって、挿入が成功した後、すべての Windows Phone アプリケーション登録にプッシュ通知が (挿入された項目のテキストと共に) 送信されます。

3. **[プッシュ]** タブをクリックし、**[非認証プッシュ通知を有効にします]** をオンにして、**[保存]** をクリックします。

    これにより、非認証モードで MPNS に接続するモバイル サービスがプッシュ通知を送信できるようになります。
    >[AZURE.NOTE]このチュートリアルでは、非認証モードで MPNS を使用します。 このモードでは、MPNS はデバイス チャネルに送信できる通知の数を制限します。 この制限を削除するには、**[アップロード]** をクリックして証明書を生成してアップロードし、その証明書を選択する必要があります。 証明書を生成する方法の詳細については、「Setting up an authenticated web service to send push notifications for Windows Phone (認証済み Web サービスを、Windows Phone のプッシュ通知を送信するように設定する)」を参照してください。

## <a id="test"></a> アプリでプッシュ通知をテストします。

1. Visual Studio で、F5 キーを押してアプリケーションを実行します。
    >[AZURE.NOTE] Windows Phone エミュレーターでテストする場合、401 認証エラー "RegistrationAuthorizationException" が発生する場合があります。 これは、中に発生する可能性が、 `RegisterNativeAsync()` 方法により、呼び出しが、Windows Phone エミュレーターがホスト PC の時計を同期します。 セキュリティ トークンで拒否される場合があります。 これを解決するには、テストする前にエミュレーターの時計を手動で設定します。

5. アプリケーションのテキストボックスに「hello push」と入力して、**[保存]** をクリックし、すぐに [開始] ボタンまたは [戻る] ボタンを押してアプリケーションを閉じます。

    ![アプリケーションにテキストを入力する](./media/mobile-services-javascript-backend-windows-phone-get-started-push/mobile-quickstart-push3-wp8.png)

    これにより、追加された項目を保存するための挿入要求がモバイル サービスに送信されます。 デバイスは **hello push** というトースト通知を受信します。

    ![受信したトースト通知](./media/mobile-services-javascript-backend-windows-phone-get-started-push/mobile-quickstart-push5-wp8.png)
    >[AZURE.NOTE]アプリケーションが開いている場合は通知を受信しません。 アプリがアクティブな間は、トースト通知を受信するには、処理、 [ShellToastNotificationReceived](http://msdn.microsoft.com/library/windowsphone/develop/microsoft.phone.notification.httpnotificationchannel.shelltoastnotificationreceived.aspx) イベントです。

## <a name="next-steps"> </a>次のステップ

このチュートリアルでは、Windows ストア アプリで Mobile Services および Notification Hubs を使用してプッシュ通知を送信できるようにするための基本について説明しました。 次に、次のチュートリアルのいずれかを完了します。

+ [登録者へのブロードキャスト通知を送信します。](../notification-hubs-windows-phone-send-breaking-news.md)
    <br/>ユーザーの登録およびに興味のあるカテゴリに関してプッシュ通知を受信する方法について説明します。

+ [登録者へのプラットフォーム固有の通知を送信します。](../notification-hubs-aspnet-cross-platform-notify-users.md)
    <br/>テンプレートを使用して、バックエンドでプラットフォームに固有のペイロードを作成することがなく、モバイル サービスからプッシュ通知を送信する方法について説明します。


Mobile Services と Notification Hubs については次のトピックを参照してください。

* [Azure Notification Hubs - 診断ガイドライン](../notification-hubs-diagnosing.md)
    <br/>プッシュ通知の問題のトラブルシューティングを行う方法について説明します。

* [認証の使用の開始を取得]
  <br/>別のアカウントの種類のモバイル サービスを使用して、アプリのユーザーを認証する方法について説明します。

* [通知ハブは何ですか?]
  <br/>通知を配信するアプリケーションにすべての主要なクライアントのプラットフォームの通知ハブの動作について説明します。

* [モバイル サービス .NET の使用方法の概念リファレンス]
  <br/>.NET で Mobile Services を使用する方法について説明します。

* [モバイル サービスのサーバー スクリプト リファレンス]
  <br/>モバイル サービスにビジネス ロジックを実装する方法について説明します。








[submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582 
[my applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039 
[live sdk for windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253 
[add mobile services to an existing app]: mobile-services-windows-phone-get-started-data.md 
[get started with authentication]: mobile-services-windows-phone-get-started-users.md 
[setting up an authenticated web service to send push notifications for windows phone]: http://msdn.microsoft.com/library/windowsphone/develop/ff941099(v=vs.105).aspx 
[mobile services server script reference]: http://go.microsoft.com/fwlink/?LinkId=262293 
[mobile services .net how-to conceptual reference]: mobile-services-windows-dotnet-how-to-use-client-library.md 
[what are notification hubs?]: ../notification-hubs-overview.md 

