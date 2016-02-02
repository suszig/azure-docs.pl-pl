<properties
    pageTitle="Node.js で Notification Hubs を使用する方法"
    description="Notification Hubs を使用して Node.js アプリケーションからプッシュ通知を送信する方法について説明します。"
    services="notification-hubs"
    documentationCenter="nodejs"
    authors="wesmc7777"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="javascript"
    ms.topic="article"
    ms.date="12/10/2015"
    ms.author="wesmc"/>


# Node.js から Notification Hubs を使用する方法

[AZURE.INCLUDE [notification-hubs-backend-how-to-selector](../../includes/notification-hubs-backend-how-to-selector.md)]

## 概要

このガイドが通知ハブを使用する方法を示します
Node.js アプリケーションです。 紹介するシナリオでは、**Android、iOS、Windows Phone、Windows ストア アプリへの通知の送信**を対象としています。 通知ハブの詳細については、次を参照してください。、 [次のステップ](#next) セクションです。

## Notification Hubs とは

Azure Notification Hubs によって、モバイル デバイスにプッシュ通知を送信するための、使いやすく拡張性の高いマルチプラットフォーム インフラストラクチャが提供されます。 詳細については、次を参照してください。 [Azure Notification Hubs](http://msdn.microsoft.com/library/windowsazure/jj927170.aspx)します。

## Node.js アプリケーションの作成

空の Node.js アプリケーションを作成します。 Node.js アプリケーションを作成する手順については、次を参照してください。 [作成 Node.js アプリケーションを Azure の Web サイト ][nodejswebsite], 、[Node.js クラウド Service][node.js cloud service] (Windows PowerShell を使用)、または [WebMatrix による Web サイト]。

## アプリケーションを構成して Notification Hub を使用する

Azure Notification Hub を使用するには、Node.js azure パッケージをダウンロードして
。 これには、
REST サービスと通信するための便利なライブラリ セットが含まれています。

### ノード パッケージ マネージャー (NPM) を使用してパッケージを取得する

1.  **PowerShell** (Windows)、**Terminal** (Mac)、**Bash** (Unix) などのコマンド ライン インターフェイスを使用して、サンプル アプリケーションを作成したフォルダーに移動します。

2.  型 **npm のインストール azure** コマンド ウィンドウにする必要があります
    次のような出力が生成されます。

        azure@0.7.0 node_modules\azure
        |-- dateformat@1.0.2-1.2.3
        |-- xmlbuilder@0.4.2
        |-- node-uuid@1.2.0
        |-- mime@1.2.9
        |-- underscore@1.4.4
        |-- validator@0.4.28
        |-- tunnel@0.0.2
        |-- wns@0.5.3
        |-- xml2js@0.2.6 (sax@0.4.2)
        |-- request@2.16.6 (forever-agent@0.2.0, aws-sign@0.2.0, tunnel-agent@0.2.0, oauth-sign@0.2.0, json-stringify-safe@3.0.0, cookie-jar@0.2.0, node-uuid@1.4.0, qs@0.5.5, hawk@0.10.2, form-data@0.0.7)

3.  手動で実行することができます、 **%.*ls** または **dir** いることを確認するにはコマンド、
    **node \_modules** フォルダーが作成されました。 そのフォルダーで
    **azure** にアクセスするために必要なライブラリを含むパッケージ
    含まれています。

### モジュールのインポート

テキスト エディターを使用して、アプリケーションの
**server.js** 、アプリケーションのファイル。

    var azure = require('azure');

### Azure Notification Hub 接続の設定

**NotificationHubService** オブジェクトを使用すると、通知ハブを操作できます。 この
次のコードを作成、 **NotificationHubService** という名前の通知ハブのオブジェクト **hubname**します。 付近に追加します
一番上、 **server.js** 、azure にインポートするステートメントの後のファイル
。

    var notificationHubService = azure.createNotificationHubService('hubname','connectionstring');

接続 **connectionstring** 値は、次の手順を実行して、[Azure Classic Portal] から取得できます。

1. [Azure クラシック ポータルで] **Service Bus**, 、通知ハブが含まれている名前空間を選択します。

2. **[Notification Hubs]** を選択し、使用するハブを選択します。

3. **[概要]** セクションで **[接続文字列の表示]** を選択し、接続文字列の値をコピーします。

> [AZURE.NOTE] また、Azure PowerShell に用意されている **Get-AzureSbNamespace** コマンドレット、または Azure コマンド ライン ツール (Azure CLI) で **azure sb namespace show** コマンドを使用して、接続文字列を取得することもできます。

</div>

## 通知の送信方法

**NotificationHubService** オブジェクトは、特定のデバイスやアプリケーションに通知を送信するために、次のオブジェクト インスタンスを公開します。

* **Android** - **notificationHubService.gcm** で利用可能な **GcmService** オブジェクトを使用します。
* **iOS** - **notificationHubService.apns** でアクセス可能な **ApnsService** オブジェクトを使用します。
* **Windows Phone** - **notificationHubService.mpns** で利用可能な **MpnsService** オブジェクトを使用します。
* **Windows ストア アプリケーション** - **notificationHubService.wns** で利用可能な **WnsService** オブジェクトを使用します。

### Android アプリケーション通知の送信方法

**GcmService** オブジェクトには、Android アプリケーションに通知を送信するために使用される **send** メソッドが用意されています。 **send** メソッドは、次のパラメーターを受け取ります。

* Tags - タグ識別子。 タグが指定されない場合、通知はすべてのクライアントに送信されます。
* Payload - メッセージの JSON または文字列ペイロード
* Callback - コールバック関数

ペイロード形式の詳細については、のペイロード セクションを参照してください。 [Implementing GCM Server](http://developer.android.com/google/gcm/server.html#payload)します。

次のコードは、**NotificationHubService** によって公開されている **GcmService** インスタンスを使用して、すべてのクライアントにメッセージを送信します。

    var payload = {
      data: {
        msg: 'Hello!'
      }
    };
    notificationHubService.gcm.send(null, payload, function(error){
      if(!error){
        //notification sent
      }
    });

### iOS アプリケーション通知の送信方法

**ApnsService** オブジェクトには、iOS アプリケーションに通知を送信するために使用される **send** メソッドが用意されています。 **send** メソッドは、次のパラメーターを受け取ります。

* Tags - タグ識別子。 タグが指定されない場合、通知はすべてのクライアントに送信されます。
* Payload - メッセージの JSON または文字列ペイロード
* Callback - コールバック関数

ペイロード形式の詳細については、の通知ペイロードを参照してください、 [Local and Push Notification Programming Guide および](http://developer.apple.com/library/ios/#documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/ApplePushService/ApplePushService.html)します。

次のコードは、**NotificationHubService** によって公開されている **ApnsService** インスタンスを使用して、すべてのクライアントにアラート メッセージを送信します。

    var payload={
        alert: 'Hello!'
      };
    notificationHubService.apns.send(null, payload, function(error){
      if(!error){
        // notification sent
      }
    });

### Windows Phone 通知の送信方法

**MpnsService** オブジェクトには、Windows Phone アプリに通知を送信するために使用される **send** メソッドが用意されています。 **send** メソッドは、次のパラメーターを受け取ります。

* Tags - タグ識別子。 タグが指定されない場合、通知はすべてのクライアントに送信されます。
* Payload - メッセージの XML ペイロード
* TargetName - トースト通知の場合は "toast"。 タイル通知の場合は 'token'。
* NotificationClass - 通知の優先度。 HTTP ヘッダー要素を参照してください [サーバーからのプッシュ通知](http://msdn.microsoft.com/library/hh221551.aspx) が有効な値です。
* Options - 省略可能な要求ヘッダー
* Callback - コールバック関数

有効な TargetName、NotificationClass、ヘッダー オプションについては、次を参照してください。 [サーバーからのプッシュ通知](http://msdn.microsoft.com/library/hh221551.aspx)します。

次のコードは、**NotificationHubService** によって公開されている **MpnsService** インスタンスを使用して、トースト アラートを送信します。

    var payload = '<?xml version="1.0" encoding="utf-8"?><wp:Notification xmlns:wp="WPNotification"><wp:Toast><wp:Text1>string</wp:Text1><wp:Text2>string</wp:Text2></wp:Toast></wp:Notification>';
    notificationHubService.mpns.send(null, payload, 'toast', 22, function(error){
      if(!error){
        //notification sent
      }
    });

### Windows ストア アプリ通知の送信方法

**WnsService** オブジェクトには、Windows ストア アプリに通知を送信するために使用される **send** メソッドが用意されています。 **send** メソッドは、次のパラメーターを受け取ります。

* Tags - タグ識別子。 タグが指定されない場合、通知はすべてのクライアントに送信されます。
* Payload - XML メッセージ ペイロード
* Type - 通知の種類
* Options - 省略可能な要求ヘッダー
* Callback - コールバック関数

有効な Type と要求ヘッダーの一覧は、次を参照してください。 [プッシュ通知サービスの要求および応答ヘッダー](http://msdn.microsoft.com/library/windows/apps/hh465435.aspx)します。

次のコードは、**NotificationHubService** によって公開されている **WnsService** インスタンスを使用して、トースト アラートを送信します。

    var payload = '<toast><visual><binding template="ToastText01"><text id="1">Hello!</text></binding></visual></toast>';
    notificationHubService.wns.send(null, payload , 'wns/toast', function(error){
      if(!error){
        // notification sent
      }
    });

## 次のステップ

これで、Notification Hub の基本的な使用方法を理解できました。さらに詳細な情報が必要な場合は、
次のリンク先を参照してください。

-   MSDN リファレンス: [Azure Notification Hubs][]します。
-   GitHub では、[Azure SDK for Node] リポジトリを参照してください。


[azure sdk for node]: https://github.com/WindowsAzure/azure-sdk-for-node 
[next steps]: #nextsteps 
[what are service bus topics and subscriptions?]: #what-are-service-bus-topics 
[create a service namespace]: #create-a-service-namespace 
[obtain the default management credentials for the namespace]: #obtain-default-credentials 
[create a node.js application]: #Create_a_Nodejs_Application 
[configure your application to use service bus]: #Configure_Your_Application_to_Use_Service_Bus 
[how to: create a topic]: #How_to_Create_a_Topic 
[how to: create subscriptions]: #How_to_Create_Subscriptions 
[how to: send messages to a topic]: #How_to_Send_Messages_to_a_Topic 
[how to: receive messages from a subscription]: #How_to_Receive_Messages_from_a_Subscription 
[how to: handle application crashes and unreadable messages]: #How_to_Handle_Application_Crashes_and_Unreadable_Messages 
[how to: delete topics and subscriptions]: #How_to_Delete_Topics_and_Subscriptions 
[1]: #Next_Steps 
[topic concepts]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-topics-01.png 
[azure classic portal]: http://manage.windowsazure.com 
[image]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-03.png 
[2]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-04.png 
[3]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-05.png 
[4]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-06.png 
[5]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-07.png 
[sqlfilter.sqlexpression]: http://msdn.microsoft.com/library/windowsazure/microsoft.servicebus.messaging.sqlfilter.sqlexpression.aspx 
[azure service bus notification hubs]: http://msdn.microsoft.com/library/windowsazure/jj927170.aspx 
[sqlfilter]: http://msdn.microsoft.com/library/windowsazure/microsoft.servicebus.messaging.sqlfilter.aspx 
[web site with webmatrix]: /develop/nodejs/tutorials/web-site-with-webmatrix/ 
[node.js cloud service]: ../cloud-services-nodejs-develop-deploy-app.md 
[previous management portal]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/previous-portal.png 
[nodejswebsite]: /develop/nodejs/tutorials/create-a-website-(mac)/ 
[node.js cloud service with storage]: /develop/nodejs/tutorials/web-app-with-storage/ 
[node.js web application with storage]: /develop/nodejs/tutorials/web-site-with-storage/ 

