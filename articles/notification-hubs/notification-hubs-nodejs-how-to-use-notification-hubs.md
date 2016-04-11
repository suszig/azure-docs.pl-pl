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

##概要

このガイドが通知ハブを使用する方法を示します
Node.js アプリケーションです。 紹介するシナリオ **Android、iOS、Windows Phone と Windows ストア アプリケーションに通知を送信する**です。 通知ハブの詳細については、次を参照してください。、 [次のステップ](#next) セクションです。

##Notification Hubs とは

Azure Notification Hubs によって、モバイル デバイスにプッシュ通知を送信するための、使いやすく拡張性の高いマルチプラットフォーム インフラストラクチャが提供されます。 詳細については、次を参照してください。 [Azure Notification Hubs](http://msdn.microsoft.com/library/windowsazure/jj927170.aspx)します。

##Node.js アプリケーションの作成

空の Node.js アプリケーションを作成します。 Node.js アプリケーションを作成する手順については、次を参照してください。 [作成し、Node.js アプリケーションを Azure Web サイトをデプロイ][nodejswebsite], 、[Node.js クラウド サービス][Node.js Cloud Service] (Windows PowerShell を使用)、または [Web Site with WebMatrix]します。

##アプリケーションを構成して Notification Hub を使用する

Azure Notification Hub を使用するには、Node.js azure パッケージをダウンロードして
。 これには、
REST サービスと通信するための便利なライブラリ セットが含まれています。

### ノード パッケージ マネージャー (NPM) を使用してパッケージを取得する

1.  コマンド ライン インターフェイスを使用して **PowerShell** (Windows)、 **ターミナル** (Mac)、または **Bash** (Unix)、サンプル アプリケーションを作成したフォルダーに移動します。

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

 **NotificationHubService** オブジェクトを使用して、通知ハブを操作できます。 この
次のコードを作成、 **NotificationHubService** という名前の通知ハブのオブジェクト **hubname**します。 付近に追加します
一番上、 **server.js** 、azure にインポートするステートメントの後のファイル
。

    var notificationHubService = azure.createNotificationHubService('hubname','connectionstring');

接続 **connectionstring** から値を取得できます、 [Azure Classic Portal] 、次の手順を実行します。

1.  [Azure Classic Portal], [ **Service Bus**, 、通知ハブが含まれている名前空間を選択します。

2. 選択 **NOTIFICATION HUBS**, を使用するハブを選択します。

3. 選択 **[接続文字列の** から、 **概要** セクションし、接続文字列の値をコピーします。

> [AZURE.NOTE] 使用して接続文字列を取得することも、 **Get-azuresbnamespace** Azure PowerShell のコマンドレットまたは **azure sb 名前空間の表示** コマンドで、Azure コマンド ライン インターフェイス (Azure CLI)。

</div>

##通知の送信方法

 **NotificationHubService** オブジェクトは特定のデバイスやアプリケーションに通知を送信するための次のオブジェクト インスタンスを公開します。

* **Android** -を使用して、 **GcmService** で利用可能なオブジェクト **notificationHubService.gcm**
* **iOS** -を使用して、 **ApnsService** でアクセス可能なオブジェクト **notificationHubService.apns**
* **Windows Phone** -を使用して、 **MpnsService** で利用可能なオブジェクト **します。**
* **Windows ストア アプリケーション** -を使用して、 **WnsService** で利用可能なオブジェクト **notificationHubService.wns**

### Android アプリケーション通知の送信方法

 **GcmService** オブジェクトは、提供、 **送信** Android アプリケーションに通知を送信するために使用されるメソッドです。  **送信** メソッドは、次のパラメーターを受け取ります。

* Tags - タグ識別子。 タグが指定されない場合、通知はすべてのクライアントに送信されます。
* Payload - メッセージの JSON または文字列ペイロード
* Callback - コールバック関数

ペイロード形式の詳細については、のペイロード セクションを参照してください。 [Implementing GCM Server](http://developer.android.com/google/gcm/server.html#payload)します。

次のコードでは、 **GcmService** によって公開されているインスタンス、 **NotificationHubService** すべてのクライアントにメッセージを送信します。

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

 **ApnsService** オブジェクトは、提供、 **送信** iOS アプリケーションに通知を送信するために使用されるメソッドです。  **送信** メソッドは、次のパラメーターを受け取ります。

* Tags - タグ識別子。 タグが指定されない場合、通知はすべてのクライアントに送信されます。
* Payload - メッセージの JSON または文字列ペイロード
* Callback - コールバック関数

ペイロード形式の詳細については、の通知ペイロードを参照してください、 [Local and Push Notification Programming Guide および](http://developer.apple.com/library/ios/#documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/ApplePushService/ApplePushService.html)です。

次のコードでは、 **ApnsService** によって公開されているインスタンス、 **NotificationHubService** すべてのクライアントに警告メッセージを送信します。

    var payload={
        alert: 'Hello!'
      };
    notificationHubService.apns.send(null, payload, function(error){
      if(!error){
        // notification sent
      }
    });

### Windows Phone 通知の送信方法

 **MpnsService** オブジェクトは、提供、 **送信** Windows Phone アプリに通知を送信するために使用するメソッドです。  **送信** メソッドは、次のパラメーターを受け取ります。

* Tags - タグ識別子。 タグが指定されない場合、通知はすべてのクライアントに送信されます。
* Payload - メッセージの XML ペイロード
* TargetName - トースト通知の場合は "toast"。 タイル通知の場合は 'token'。
* NotificationClass - 通知の優先度。 HTTP ヘッダー要素を参照してください [サーバーからのプッシュ通知](http://msdn.microsoft.com/library/hh221551.aspx) が有効な値です。
* Options - 省略可能な要求ヘッダー
* Callback - コールバック関数

有効な TargetName、NotificationClass、ヘッダー オプションについては、次を参照してください。 [サーバーからのプッシュ通知](http://msdn.microsoft.com/library/hh221551.aspx)します。

次のコードでは、 **MpnsService** によって公開されているインスタンス、 **NotificationHubService** 、トースト アラートを送信します。

    var payload = '<?xml version="1.0" encoding="utf-8"?><wp:Notification xmlns:wp="WPNotification"><wp:Toast><wp:Text1>string</wp:Text1><wp:Text2>string</wp:Text2></wp:Toast></wp:Notification>';
    notificationHubService.mpns.send(null, payload, 'toast', 22, function(error){
      if(!error){
        //notification sent
      }
    });

### Windows ストア アプリ通知の送信方法

 **WnsService** オブジェクトは、提供、 **送信** Windows ストア アプリに通知を送信するために使用するメソッドです。   **送信** メソッドは、次のパラメーターを受け取ります。

* Tags - タグ識別子。 タグが指定されない場合、通知はすべてのクライアントに送信されます。
* Payload - XML メッセージ ペイロード
* Type - 通知の種類
* Options - 省略可能な要求ヘッダー
* Callback - コールバック関数

有効な Type と要求ヘッダーの一覧は、次を参照してください。 [プッシュ通知サービスの要求および応答ヘッダー](http://msdn.microsoft.com/library/windows/apps/hh465435.aspx)します。

次のコードでは、 **WnsService** によって公開されているインスタンス、 **NotificationHubService** 、トースト アラートを送信します。

    var payload = '<toast><visual><binding template="ToastText01"><text id="1">Hello!</text></binding></visual></toast>';
    notificationHubService.wns.send(null, payload , 'wns/toast', function(error){
      if(!error){
        // notification sent
      }
    });

## 次のステップ

これで、Notification Hub の基本的な使用方法を理解できました。さらに詳細な情報が必要な場合は、
次のリンク先を参照してください。

-   MSDN リファレンス: Azure Notification Hubs
-   参照してください、 [Azure SDK for Node] GitHub のリポジトリです。

  [Azure SDK for Node]: https://github.com/WindowsAzure/azure-sdk-for-node
  [Next Steps]: #nextsteps
  [What are Service Bus Topics and Subscriptions?]: #what-are-service-bus-topics
  [Create a Service Namespace]: #create-a-service-namespace
  [Obtain the Default Management Credentials for the Namespace]: #obtain-default-credentials
  [Create a Node.js Application]: #Create_a_Nodejs_Application
  [Configure Your Application to Use Service Bus]: #Configure_Your_Application_to_Use_Service_Bus
  [How to: Create a Topic]: #How_to_Create_a_Topic
  [How to: Create Subscriptions]: #How_to_Create_Subscriptions
  [How to: Send Messages to a Topic]: #How_to_Send_Messages_to_a_Topic
  [How to: Receive Messages from a Subscription]: #How_to_Receive_Messages_from_a_Subscription
  [How to: Handle Application Crashes and Unreadable Messages]: #How_to_Handle_Application_Crashes_and_Unreadable_Messages
  [How to: Delete Topics and Subscriptions]: #How_to_Delete_Topics_and_Subscriptions
  [1]: #Next_Steps
  [Topic Concepts]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-topics-01.png
  [Azure Classic Portal]: http://manage.windowsazure.com
  [image]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-03.png
  [2]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-04.png
  [3]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-05.png
  [4]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-06.png
  [5]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-07.png
  [SqlFilter.SqlExpression]: http://msdn.microsoft.com/library/windowsazure/microsoft.servicebus.messaging.sqlfilter.sqlexpression.aspx
  [Azure Service Bus Notification Hubs]: http://msdn.microsoft.com/library/windowsazure/jj927170.aspx
  [SqlFilter]: http://msdn.microsoft.com/library/windowsazure/microsoft.servicebus.messaging.sqlfilter.aspx
  [Web Site with WebMatrix]: /develop/nodejs/tutorials/web-site-with-webmatrix/
  [Node.js Cloud Service]: ../cloud-services-nodejs-develop-deploy-app.md
[Previous Management Portal]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/previous-portal.png
  [nodejswebsite]: /develop/nodejs/tutorials/create-a-website-(mac)/
  [Node.js Cloud Service with Storage]: /develop/nodejs/tutorials/web-app-with-storage/
  [Node.js Web Application with Storage]: /develop/nodejs/tutorials/web-site-with-storage/

