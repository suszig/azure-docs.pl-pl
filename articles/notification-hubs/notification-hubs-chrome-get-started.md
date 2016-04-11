<properties
    pageTitle="Azure Notification Hubs の使用 (Chrome アプリ) | Microsoft Azure"
    description="このチュートリアルでは、Azure Notification Hubs を使用して Chrome アプリケーションにプッシュ通知を送信する方法について学習します。"
    services="notification-hubs"
    documentationCenter=""
    authors="wesmc7777"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-chrome"
    ms.devlang="JavaScript"
    ms.topic="hero-article"
    ms.date="10/20/2015"
    ms.author="wesmc"/>

# Notification Hubs の使用 (Chrome アプリ)

[AZURE.INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

このトピックでは、Azure Notification Hubs を使用して Chrome アプリケーションにプッシュ通知を送信する方法について説明します。

Chrome アプリケーション通知を使用する主な利点の 1 つは、通知が Google Chrome ブラウザーのコンテキスト内に表示されることです。 Chrome アプリケーションをブラウザーで実行したり開いたりする必要はありません (ただし、Chrome ブラウザー自体は実行しておく必要があります)。 また、Chrome の通知ウィンドウにはすべての通知がまとめて表示されます。

>[AZURE.NOTE] これは一般的なブラウザー内プッシュ通知は、Chrome アプリケーションに固有: を参照してください [Chrome Apps Overview] 詳細です。 Chrome アプリケーションは以前は "パッケージ型アプリケーション" として知られており、単純な "ホスト型アプリケーション" とは異なります。 参照してください [Installable Web Apps] 違いについては、です。 Chrome アプリケーションは、Apache Cordova を使用してモバイル (Android や iOS) で実行することもできます。 参照してください [Chrome Apps on Mobile] の詳細。

このチュートリアルでは、Google Cloud Messaging (GCM) を使用してプッシュ通知を受信する Chrome アプリケーションを作成します。 このチュートリアルを完了すると、その Chrome アプリケーションをインストールしたすべての Chrome ユーザーにプッシュ通知をブロードキャストできるようになります。

このチュートリアルでは、プッシュ通知を有効にするための、次の基本的な手順について説明します。

* [Google Cloud Messaging を有効にする](#register)
* [通知ハブを構成する](#configure-hub)
* [通知ハブに Chrome アプリケーションを接続する](#connect-app)
* [Chrome アプリケーションに通知を送信する](#send)
* [次のステップ](#next-steps)

このチュートリアルでは、Notification Hubs を使用した簡単なブロードキャスト シナリオのデモンストレーションを行います。 GCM と Azure Notification Hubs の構成は android では、後の構成と同じ [Google Cloud Messaging for Chrome] は推奨されていませんし、同じ GCM で Android デバイスと Chrome インスタンスの両方がサポートようになりました。

Notification Hubs を使用してデバイスの特定のユーザーとグループに対応する方法を理解するために、「次のステップ」セクションに記載されているチュートリアルも一緒に参照してください。

>[AZURE.NOTE] このチュートリアルを完了するには、アクティブな Azure アカウントが必要です。 アカウントがない場合は、無料試用版のアカウントを数分で作成することができます。 詳細については、「[Azure の無料評価版サイト](http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%notification-hubs-chrome-get-started%2F)」をご覧ください。

##<a id="register"></a>Google Cloud Messaging を有効にする

1. 移動し、 [Google Cloud Console] 、web サイトは、Google アカウント資格情報でサインインし、順にクリックして、 **プロジェクトの作成** ] ボタンをクリックします。 適切な提供 **プロジェクト名**, 、順にクリックし、 **作成** ] ボタンをクリックします。

    ![][1]

2. 書き留めて、 **プロジェクト番号** 上、 **プロジェクト** 作成したプロジェクトのページです。 してこれを使用する、 **GCM 送信者 ID** GCM に登録する Chrome アプリケーションにします。

    ![][2]

3. 左のウィンドウで **APIs & auth**, 、下へスクロールし、切り替えを有効にする] をクリックして **Google Cloud Messaging for Android**します。 有効にする必要はありません **Google Cloud Messaging for Chrome**します。

    ![][3]

4. 左のウィンドウで **資格情報** > **Create New Key** > **サーバー キー** > **作成**します。

    ![][4]

5. サーバーのメモ **API キー**します。 次の手順では、Notification Hubs でこのキーを構成して、GCM にプッシュ通知を送信できるようにします。

    ![][5]

##<a id="configure-hub"></a>通知ハブを構成する

1. サインイン、 [Azure Classic Portal], 、クリックして **+ 新規** 、画面の左下にあります。

2. クリックして **App Services** > **Service Bus** > **通知ハブ** > **簡易作成**します。 通知ハブの名前を入力を目的のリージョンを選択してクリックして **新しい通知ハブの作成**します。

    ![][6]

4. 前の手順で作成した Notification Hub に移動します。 通知ハブを含む、名前空間をクリックして (通常 ***通知ハブの名前*-ns**)。

    ![][7]

5. クリックして、 **Notification Hubs** 上部にあるタブをクリックします。

    ![][8]

6. クリックして、 **構成** 上部にあるタブをクリックします。

    ![][9]

7.  **構成** ] タブで、下へスクロールして、 **google cloud messaging 設定**, 、入力、 **API キー** ] をクリックして、前のセクションで取得した値 **保存**します。

    ![][10]

8. 選択、 **ダッシュ ボード** 上部にあるタブをクリックして **接続情報** 下部にあります。

    ![][11]

9. メモしておきます **DefaultListenSharedAccessSignature** (これは、通知ハブに登録する Chrome アプリケーションで必要になります) と **DefaultFullSharedAccessSignature** (これは、通知を送信する必要があります)。

    ![][12]

これで、Notification Hub が GCM と連動するように構成されました。また、追加の構成を行うために必要な接続文字列を取得しました。

##<a id="connect-app"></a>通知ハブに Chrome アプリケーションを接続する

###新しい Chrome アプリケーションの作成

次のサンプルに基づいて [Chrome App GCM Sample] して Chrome アプリケーションを作成することをお勧めします。 以降のセクションでは、Azure Notification Hubs に関連する手順を重点的に説明しています。 この Chrome アプリケーションのソースをダウンロードすることをお勧め [Chrome App Notification Hub Sample]します。

Chrome アプリケーションは JavaScript を使用して作成されます。推奨される任意のワード エディターを使用して作成を行うことができます。 この Chrome アプリケーションの外観は次のようになります。

    ![][15]

2. フォルダーを作成し、名前を付けます **ChromePushApp** またはするものなどです。

3. ダウンロード **cryto js ライブラリ** から [crypto-js library] このフォルダーにします。 このライブラリのフォルダーが 2 つのサブフォルダーを含める: **コンポーネント** と **プログラムのロールアップ**します。

4. 作成、 **manifest.json** ファイルです。 すべての Chrome アプリケーションは、アプリケーションのメタデータ (具体的にはアプリケーションに対するアクセス許可) を記述するマニフェスト ファイルに基づいています。

        {
          "name": "NH-GCM Notifications",
          "description": "Chrome platform app.",
          "manifest_version": 2,
          "version": "0.1",
          "app": {
            "background": {
              "scripts": ["background.js"]
            }
          },
          "permissions": ["gcm", "storage", "notifications", "https://*.servicebus.windows.net/*"],
          "icons": { "128": "gcm_128.png" }
        }

    注、 **権限** 要素は、この Chrome アプリが GCM からプッシュ通知を受け取ることがあることを指定します。 また、この要素では、Chrome アプリケーションが REST を呼び出して登録を行うための Azure Notification Hubs の URI も指定する必要があります。
    このサンプルでは、元の GCM サンプルから再利用されるソースにあるアイコン ファイル (gcm_128.png) を使用します。 任意の画像を使用できます。

5. という名前のファイルを作成する **含む background.js** を次のコード。

        // Returns a new notification ID used in the notification.
        function getNotificationId() {
          var id = Math.floor(Math.random() * 9007199254740992) + 1;
          return id.toString();
        }

        function messageReceived(message) {
          // A message is an object with a data property that
          // consists of key-value pairs.

          // Concatenate all key-value pairs to form a display string.
          var messageString = "";
          for (var key in message.data) {
            if (messageString != "")
              messageString += ", "
            messageString += key + ":" + message.data[key];
          }
          console.log("Message received: " + messageString);

          // Pop up a notification to show the GCM message.
          chrome.notifications.create(getNotificationId(), {
            title: 'GCM Message',
            iconUrl: 'gcm_128.png',
            type: 'basic',
            message: messageString
          }, function() {});
        }

        var registerWindowCreated = false;

        function firstTimeRegistration() {
          chrome.storage.local.get("registered", function(result) {

            registerWindowCreated = true;
            chrome.app.window.create(
              "register.html",
              {  width: 520,
                 height: 500,
                 frame: 'chrome'
              },
              function(appWin) {}
            );
          });
        }

        // Set up a listener for GCM message event.
        chrome.gcm.onMessage.addListener(messageReceived);

        // Set up listeners to trigger the first-time registration.
        chrome.runtime.onInstalled.addListener(firstTimeRegistration);
        chrome.runtime.onStartup.addListener(firstTimeRegistration);

    これは、HTML の Chrome アプリケーション ウィンドウがポップアップ ファイル (**register.html**) も、ハンドラーを定義し、 **messageReceived** を受信したプッシユ通知を処理します。

6. という名前のファイルを作成する **register.html** Chrome アプリの UI を定義します。 このサンプルでは使用注 *CryptoJS v3.1.2*します。 その他のバージョンをダウンロードした場合は、スクリプトの src のパスを修正してください。

        <html>

        <head>
        <title>GCM Registration</title>
        <script src="register.js"></script>
        <script src="CryptoJS v3.1.2/rollups/hmac-sha256.js"></script>
        <script src="CryptoJS v3.1.2/components/enc-base64-min.js"></script>
        </head>

        <body>

        Sender ID:<br/><input id="senderId" type="TEXT" size="20"><br/>
        <button id="registerWithGCM">Register with GCM</button>
        <br/>
        <br/>
        <br/>

        Notification Hub Name:<br/><input id="hubName" type="TEXT" style="width:400px"><br/><br/>
        Connection String:<br/><textarea id="connectionString" type="TEXT" style="width:400px;height:60px"></textarea>

        <br/>

        <button id="registerWithNH" disabled="true">Register with Azure Notification Hubs</button>

        <br/>
        <br/>

        <textarea id="console" type="TEXT" readonly style="width:500px;height:200px;background-color:#e5e5e5;padding:5px"></textarea>

        </body>

        </html>

7. という名前のファイルを作成する **register.js** 次のコードです。 このファイルは、基盤となるスクリプトを指定します。 **register.html**します。 Chrome アプリケーションではインライン実行が許可されないため、UI 用の基盤となるスクリプトを別途作成する必要があります。

        var registrationId = "";
        var hubName        = "", connectionString = "";
        var originalUri    = "", targetUri = "", endpoint = "", sasKeyName = "", sasKeyValue = "", sasToken = "";

        window.onload = function() {
           document.getElementById("registerWithGCM").onclick = registerWithGCM;  
           document.getElementById("registerWithNH").onclick = registerWithNH;
           updateLog("You have not registered yet. Please provider sender ID and register with GCM and then with  Notification Hubs.");
        }

        function updateLog(status) {
          currentStatus = document.getElementById("console").innerHTML;
          if (currentStatus != "") {
            currentStatus = currentStatus + "\n\n";
          }

          document.getElementById("console").innerHTML = currentStatus  + status;
        }

        function registerWithGCM() {
          var senderId = document.getElementById("senderId").value.trim();
          chrome.gcm.register([senderId], registerCallback);

          // Prevent register button from being clicked again before the registration finishes.
          document.getElementById("registerWithGCM").disabled = true;
        }

        function registerCallback(regId) {
          registrationId = regId;
          document.getElementById("registerWithGCM").disabled = false;

          if (chrome.runtime.lastError) {
            // When the registration fails, handle the error and retry the
            // registration later.
            updateLog("Registration failed: " + chrome.runtime.lastError.message);
            return;
          }

          updateLog("Registration with GCM succeeded.");
          document.getElementById("registerWithNH").disabled = false;

          // Mark that the first-time registration is done.
          chrome.storage.local.set({registered: true});
        }

        function registerWithNH() {
          hubName = document.getElementById("hubName").value.trim();
          connectionString = document.getElementById("connectionString").value.trim();
          splitConnectionString();
          generateSaSToken();
          sendNHRegistrationRequest();
        }

        // From http://msdn.microsoft.com/library/dn495627.aspx
        function splitConnectionString()
        {
          var parts = connectionString.split(';');
          if (parts.length != 3)
          throw "Error parsing connection string";

          parts.forEach(function(part) {
            if (part.indexOf('Endpoint') == 0) {
            endpoint = 'https' + part.substring(11);
            } else if (part.indexOf('SharedAccessKeyName') == 0) {
            sasKeyName = part.substring(20);
            } else if (part.indexOf('SharedAccessKey') == 0) {
            sasKeyValue = part.substring(16);
            }
          });

          originalUri = endpoint + hubName;
        }

        function generateSaSToken()
        {
          targetUri = encodeURIComponent(originalUri.toLowerCase()).toLowerCase();
          var expiresInMins = 10; // 10 minute expiration

          // Set expiration in seconds.
          var expireOnDate = new Date();
          expireOnDate.setMinutes(expireOnDate.getMinutes() + expiresInMins);
          var expires = Date.UTC(expireOnDate.getUTCFullYear(), expireOnDate
            .getUTCMonth(), expireOnDate.getUTCDate(), expireOnDate
            .getUTCHours(), expireOnDate.getUTCMinutes(), expireOnDate
            .getUTCSeconds()) / 1000;
          var tosign = targetUri + '\n' + expires;

          // Using CryptoJS.
          var signature = CryptoJS.HmacSHA256(tosign, sasKeyValue);
          var base64signature = signature.toString(CryptoJS.enc.Base64);
          var base64UriEncoded = encodeURIComponent(base64signature);

          // Construct authorization string.
          sasToken = "SharedAccessSignature sr=" + targetUri + "&sig="
                          + base64UriEncoded + "&se=" + expires + "&skn=" + sasKeyName;
        }

        function sendNHRegistrationRequest()
        {
          var registrationPayload =
          "<?xml version=\"1.0\" encoding=\"utf-8\"?>" +
          "<entry xmlns=\"http://www.w3.org/2005/Atom\">" +
              "<content type=\"application/xml\">" +
                  "<GcmRegistrationDescription xmlns:i=\"http://www.w3.org/2001/XMLSchema-instance\" xmlns=\"http://schemas.microsoft.com/netservices/2010/10/servicebus/connect\">" +
                      "<GcmRegistrationId>{GCMRegistrationId}</GcmRegistrationId>" +
                  "</GcmRegistrationDescription>" +
              "</content>" +
          "</entry>";

          // Update the payload with the registration ID obtained earlier.
          registrationPayload = registrationPayload.replace("{GCMRegistrationId}", registrationId);

          var url = originalUri + "/registrations/?api-version=2014-09";
          var client = new XMLHttpRequest();

          client.onload = function () {
            if (client.readyState == 4) {
              if (client.status == 200) {
                updateLog("Notification Hub Registration succesful!");
                updateLog(client.responseText);
              } else {
                updateLog("Notification Hub Registration did not succeed!");
                updateLog("HTTP Status: " + client.status + " : " + client.statusText);
                updateLog("HTTP Response: " + "\n" + client.responseText);
              }
            }
          };

          client.onerror = function () {
                updateLog("ERROR - Notification Hub Registration did not succeed!");
          }

          client.open("POST", url, true);
          client.setRequestHeader("Content-Type", "application/atom+xml;type=entry;charset=utf-8");
          client.setRequestHeader("Authorization", sasToken);
          client.setRequestHeader("x-ms-version", "2014-09");

          try {
              client.send(registrationPayload);
          }
          catch(err) {
              updateLog(err.message);
          }
        }

    上記のスクリプトの注意事項は以下のとおりです。
    - *window.onload* UI の 2 つのボタンのボタン クリック イベントを定義します。 1 つは GCM への登録を行い、もう 1 つは、GCM への登録後に返される登録 ID を使用して Azure Notification Hubs への登録を行います。
    - *updateLog* 関数では、単純なログ関数を定義します。
    - *registerWithGCM* により、最初のボタン クリック ハンドラーは、 **chrome.gcm.register** Chrome アプリケーション インスタンスを登録する GCM の呼び出しです。
    - *registerCallback* 上記の GCM 登録の呼び出しが戻るときに呼び出されるコールバック関数です。
    - *registerWithNH* は 2 つ目のボタン クリックのハンドラーは Notification Hubs に登録します。 取得 **hubName** と **connectionString** (これは、ユーザーが指定した) Notification Hubs 登録の REST API 呼び出しをしています。
    - *splitConnectionString* と *generateSaSToken* はすべての REST API 呼び出しで送信する必要があります SaS トークン作成の JavaScript 実装です。 詳細については、次を参照してください。 [の一般的な概念](http://msdn.microsoft.com/library/dn495627.aspx)します。
    - *sendNHRegistrationRequest* HTTP REST 呼び出しを行う関数です。
    - *registrationPayload* 登録 XML ペイロードを定義します。 詳細については、次を参照してください。 [Create Registration NH REST API]します。 このペイロードでは、GCM から受信した ID を使用して登録 ID を更新します。
    - *クライアント* のインスタンスは、 **XMLHttpRequest** HTTP POST 要求を行うにを使用します。 マイクロソフト更新を **承認** ヘッダーを **sasToken**します。 この呼び出しが正常に完了すると、Chrome アプリケーション インスタンスが Azure Notification Hubs に登録されます。


最後に、フォルダーについて次のビューが表示されます。
    ![][21]

###Chrome アプリケーションのセットアップとテスト

1. Chrome ブラウザーを開きます。 開いている **拡張機能のクロム** を有効にして **開発者モード**します。

    ![][16]

2. クリックして **Load unpacked extension** し、ファイルを作成したフォルダーに移動します。 また、オプションとして使用することができます、 **Chrome アプリ & Extensions Developer Tool**します。 このツールはそれ自体が Chrome アプリケーションであり (Chrome Web Store からインストールされます)、Chrome アプリケーション開発用の高度なデバッグ機能を提供します。

    ![][17]

3. Chrome アプリケーションの作成時にエラーが発生しなかった場合は、作成した Chrome アプリケーションが表示されます。

    ![][18]

4. 入力、 **プロジェクト番号** ことから取得した前の手順、 **Google Cloud Console** 送信者 ID、およびクリックとして **Register with GCM**します。 メッセージが表示する必要があります **が成功した GCM 登録します。**

    ![][19]

5. 入力、 **通知ハブの名前** と **DefaultListenSharedAccessSignature** 前の手順では、ポータルとをクリックしてから取得した **Azure 通知ハブに登録**します。 メッセージが表示する必要があります **通知ハブの登録に成功しました。** Azure Notification Hubs の登録を含む登録応答の詳細の id

    ![][20]  

##<a name="send"></a>Chrome アプリケーションに通知を送信する

このチュートリアルでは、.NET コンソール アプリケーションを使用して通知を送信します。 ただし、通知は <a href="http://msdn.microsoft.com/library/windowsazure/dn223264.aspx">REST インターフェイス</a>を介してどのバックエンドからも Notification Hubs を使用して送信できます。

通知ハブと統合されている Azure Mobile Services バックエンドから通知を送信する方法の例は、作業開始 Mobile Services でプッシュ通知の使用」を参照してください ([.NET バックエンド](../mobile-services-javascript-backend-android-get-started-push.md) | [JavaScript バックエンド](../mobile-services-javascript-backend-android-get-started-push.md))。  
REST Api を使用して通知を送信する方法の例は、「Java、PHP、または Python から Notification Hubs を使用する方法」を参照してください ([Java](notification-hubs-java-backend-how-to.md) | [PHP](notification-hubs-php-backend-how-to.md) | [Python](notification-hubs-python-backend-how-to.md))。

1. Visual Studio から、 **ファイル** メニューの [ **新規** し **プロジェクト**します。 [ **Visual C# の場合**, 、] をクリックして **Windows** と **コンソール アプリケーション**, 、順にクリック **OK**します。  これで、新しいコンソール アプリケーション プロジェクトが作成されます。

2.  **ツール** ] メニューのをクリックして **ライブラリ パッケージ マネージャー** し **パッケージ マネージャー コンソール**します。 これで、パッケージ マネージャー コンソールが表示されます。

3. コンソール ウィンドウで、次のコマンドを実行します。

        Install-Package Microsoft.Azure.NotificationHubs

    これにより、<a href="http://nuget.org/packages/  WindowsAzure.ServiceBus/">WindowsAzure.ServiceBus NuGet パッケージ</a>を使用して Azure Service Bus SDK に参照を追加します。

4. ファイルを開く **Program.cs** し、以下の追加 `using` ステートメント。

        using Microsoft.Azure.NotificationHubs;

5.  **プログラム** クラスを次のメソッドを追加します。

        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("<connection string with full access>", "<hub name>");
            String message = "{\"data\":{\"message\":\"Hello Chrome from Azure Notification Hubs\"}}";
            await hub.SendGcmNativeNotificationAsync(message);
        }

    置換することを確認、 *ハブ名* プレース ホルダーをポータルに表示される通知ハブの名前を **Notification Hubs** ] タブをクリックします。 また、接続文字列プレース ホルダーを接続文字列に置き換えて **DefaultFullSharedAccessSignature** で取得したセクションでは、「通知ハブを構成します。"

    >[AZURE.NOTE] 持つ接続文字列を使用することを確認 **完全** アクセスではなく **リッスン** アクセスします。  **リッスン** アクセス文字列には、通知を送信するアクセス許可はありません。

5. 次の行を追加、 **Main** メソッド。

         SendNotificationAsync();
         Console.ReadLine();

6. Chrome ブラウザーが開いていることを確認します。 Chrome アプリケーションを開く必要はありません。 次の通知のポップアップがデスクトップに表示されます。

    ![][13]

7. Chrome の実行中に、(Windows の) タスク バーの Chrome 通知ウィンドウを使用してすべての通知を確認することもできます。

    ![][14]

## <a name="next-steps"> </a>次のステップ

この簡単な例では、Chrome アプリケーションに通知をブロードキャストしました。
Notification Hubs の詳細について学習 [Notification Hubs Overview]します。
このチュートリアルを参照して、特定のユーザーを対象に [Azure Notification Hubs Notify Users]します。 対象グループごとにユーザーをセグメント化するかどうか、確認できる [Azure Notification Hubs breaking news]します。

<!-- Images. -->
[1]: ./media/notification-hubs-chrome-get-started/GoogleConsoleCreateProject.PNG
[2]: ./media/notification-hubs-chrome-get-started/GoogleProjectNumber.png
[3]: ./media/notification-hubs-chrome-get-started/EnableGCM.png
[4]: ./media/notification-hubs-chrome-get-started/CreateServerKey.png
[5]: ./media/notification-hubs-chrome-get-started/ServerKey.png
[6]: ./media/notification-hubs-chrome-get-started/CreateNH.png
[7]: ./media/notification-hubs-chrome-get-started/NHNamespace.png
[8]: ./media/notification-hubs-chrome-get-started/NamespaceConfigure.png
[9]: ./media/notification-hubs-chrome-get-started/NHConfigure.png
[10]: ./media/notification-hubs-chrome-get-started/NHConfigureGCM.png
[11]: ./media/notification-hubs-chrome-get-started/NHDashboard.png
[12]: ./media/notification-hubs-chrome-get-started/NHConnString.png
[13]: ./media/notification-hubs-chrome-get-started/ChromeNotification.png
[14]: ./media/notification-hubs-chrome-get-started/ChromeNotificationWindow.png
[15]: ./media/notification-hubs-chrome-get-started/ChromeApp.png
[16]: ./media/notification-hubs-chrome-get-started/ChromeExtensions.png
[17]: ./media/notification-hubs-chrome-get-started/ChromeLoadExtension.png
[18]: ./media/notification-hubs-chrome-get-started/ChromeAppLoaded.png
[19]: ./media/notification-hubs-chrome-get-started/ChromeAppGCM.png
[20]: ./media/notification-hubs-chrome-get-started/ChromeAppNH.png
[21]: ./media/notification-hubs-chrome-get-started/FinalFolderView.png

<!-- URLs. -->
[Chrome App Notification Hub Sample]: http://google.com
[Google Cloud Console]: http://cloud.google.com/console
[Azure Classic Portal]: https://manage.windowsazure.com/
[Notification Hubs Overview]: http://msdn.microsoft.com/library/jj927170.aspx
[Chrome Apps Overview]: https://developer.chrome.com/apps/about_apps
[Chrome App GCM Sample]: https://github.com/GoogleChrome/chrome-app-samples/tree/master/samples/gcm-notifications
[Installable Web Apps]: https://developers.google.com/chrome/apps/docs/
[Chrome Apps on Mobile]: https://developer.chrome.com/apps/chrome_apps_on_mobile
[Create Registration NH REST API]: http://msdn.microsoft.com/library/azure/dn223265.aspx
[crypto-js library]: http://code.google.com/p/crypto-js/
[GCM with Chrome Apps]: https://developer.chrome.com/apps/cloudMessaging
[Google Cloud Messaging for Chrome]: https://developer.chrome.com/apps/cloudMessagingV1
[Azure Notification Hubs Notify Users]: notification-hubs-aspnet-backend-windows-dotnet-notify-users.md
[Azure Notification Hubs breaking news]: notification-hubs-windows-store-dotnet-send-breaking-news.md


