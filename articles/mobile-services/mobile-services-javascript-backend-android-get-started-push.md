<properties
    pageTitle ="プッシュ通知 (Android JavaScript) を使ってみる |Microsoft Azure]
    description =「Azure Mobile Services を使用して Android JavaScript アプリにプッシュ通知を送信する方法について説明します」。
    サービス「モバイル サービス、通知ハブ」を =
    documentationCenter ="android"
    authors ="RickSaling"
    ライター ="ricksal"
    manager="dwrede"
    editor=""/>

<tags
    ms.service=「モバイル サービス」
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="10/06/2015"
    ms.author="ricksal"/>


# Mobile Services Android アプリへのプッシュ通知の追加

[AZURE.INCLUDE [mobile-services-selector-get-started-push](../../includes/mobile-services-selector-get-started-push.md)]

## 概要

このトピックでは、Azure Mobile Services を使用して、Google Cloud Messaging (GCM) を使用する Android アプリにプッシュ通知を送信する方法について説明します。 このチュートリアルの前提条件である、クイック スタート プロジェクトに、プッシュ通知を追加します。 プッシュ通知はモバイル サービスに含まれている Azure Notification Hub で有効になっています。 完了すると、モバイル サービスは、レコードが挿入されるたびにプッシュ通知を送信します。

## 前提条件

[AZURE.INCLUDE [mobile-services-android-prerequisites](../../includes/mobile-services-android-prerequisites.md)]

## サンプル コード

移動する完全なソース コードを確認する [ここ](https://github.com/Azure/mobile-services-samples/tree/master/GettingStartedWithPush)します。

## Google Cloud Messaging を有効にする

[AZURE.INCLUDE [mobile-services-enable-Google-cloud-messaging](../../includes/mobile-services-enable-google-cloud-messaging.md)]

## プッシュ要求を送信するようにモバイル サービスを構成する

[AZURE.INCLUDE [mobile-services-android-configure-push](../../includes/mobile-services-android-configure-push.md)]

## アプリケーションにプッシュ通知を追加する

次の手順は、Google Play サービスをインストールすることです。 Google Cloud Messaging には、マニフェストの **minSdkVersion** プロパティが準拠する必要がある、開発およびテストに関する最小 API レベル要件があります。

古いデバイスを使用している場合は、Google Play Services SDK のセットアップに関するページを参考に、どれだけ小さな値を設定できるか判断し、適切に設定してください。

### プロジェクトへの Google Play Services の追加

[AZURE.INCLUDE [Add Play Services](../../includes/mobile-services-add-google-play-services.md)]

### コードの追加

[AZURE.INCLUDE [mobile-services-android-getting-started-with-push](../../includes/mobile-services-android-getting-started-with-push.md)]


## Azure クラシック ポータルで登録されている挿入スクリプトを更新する

[AZURE.INCLUDE [mobile-services-javascript-backend-android-push-insert-script](../../includes/mobile-services-javascript-backend-android-push-insert-script.md)]


## アプリケーションでプッシュ通知をテストする

Android フォンを USB ケーブルで直接接続するか、エミュレーターで仮想デバイスを使用する方法により、アプリケーションをテストできます。

### テスト用の Android エミュレーターの設定

このアプリケーションをエミュレーターで実行する場合は、Google API をサポートしている Android Virtual Device (AVD) を使用してください。

1. ツールバーの右端から [Android Virtual Device Manager] を選択し、デバイスを選択して右側の [編集] アイコンをクリックします。

    ![](./media/mobile-services-javascript-backend-android-get-started-push/mobile-services-android-virtual-device-manager.png)

2. 選択 **変更** デバイスの説明行で選択 **Google APIs**,  、[ok] をクリックします。

    ![](./media/mobile-services-javascript-backend-android-get-started-push/mobile-services-android-virtual-device-manager-edit.png)

    これで、AVD が Google API を使用するようになります。

### テストの実行

1. **[実行]** メニュー アイテムの **[Run app (アプリを実行)]** をクリックして、アプリケーションを開始します。

2. アプリケーションで、「新しいモバイル サービス タスク」など意味のあるテキストを入力しクリック、 **追加** ] ボタンをクリックします。

    ![](./media/mobile-services-javascript-backend-android-get-started-push/mobile-quickstart-push1-android.png)

3. 画面の上端から下へスワイプし、通知を表示するためのデバイスの通知ドロアーを開きます。


これで、このチュートリアルは終了です。

## トラブルシューティング

### Android SDK バージョンの検証

[AZURE.INCLUDE [Verify SDK](../../includes/mobile-services-verify-android-sdk-version.md)]

## 次のステップ

Mobile Services と Notification Hubs については次のトピックを参照してください。

* [認証の使用の開始を取得]
  <br/>別のアカウントの種類のモバイル サービスを使用して、アプリのユーザーを認証する方法について説明します。

* [通知ハブは何ですか?]
  <br/>通知を配信するアプリケーションにすべての主要なクライアントのプラットフォームの通知ハブの動作について説明します。

* [Notification Hubs アプリケーションをデバッグします。](http://go.microsoft.com/fwlink/p/?linkid=386630)
  </br>ガイドのトラブルシューティングと Notification Hubs ソリューションのデバッグを取得します。

* [モバイル サービス向け Android クライアント ライブラリの使用方法]
  <br/>Android と共に Mobile Services を使用する方法について説明します。

* [モバイル サービスのサーバー スクリプト リファレンス]
  <br/>モバイル サービスにビジネス ロジックを実装する方法について説明します。






[register your app for push notifications and configure mobile services]: #register 
[update the generated push notification code]: #update-scripts 
[insert data to receive notifications]: #test 
[next steps]: #next-steps 
[13]: ./media/mobile-services-windows-store-javascript-get-started-push/mobile-quickstart-push1.png 
[14]: ./media/mobile-services-windows-store-javascript-get-started-push/mobile-quickstart-push2.png 
[submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582 
[my applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039 
[get started with mobile services]: mobile-services-android-get-started.md 
[get started with authentication]: mobile-services-android-get-started-users.md 
[get started with push notifications]: /develop/mobile/tutorials/get-started-with-push-js 
[push notifications to app users]: /develop/mobile/tutorials/push-notifications-to-users-js 
[authorize users with scripts]: /develop/mobile/tutorials/authorize-users-in-scripts-js 
[javascript and html]: /develop/mobile/tutorials/get-started-with-push-js 
[set up google play services sdk]: http://go.microsoft.com/fwlink/?LinkId=389801 
[azure classic portal]: https://manage.windowsazure.com/ 
[how to use the android client library for mobile services]: mobile-services-android-how-to-use-client-library.md 
[gcm object]: http://go.microsoft.com/fwlink/p/?LinkId=282645 
[mobile services server script reference]: http://go.microsoft.com/fwlink/?LinkId=262293 
[what are notification hubs?]: ../notification-hubs-overview.md 
[send broadcast notifications to subscribers]: ../notification-hubs-android-send-breaking-news.md 
[send template-based notifications to subscribers]: ../notification-hubs-android-send-localized-breaking-news.md 

