<properties
    pageTitle="Azure App Service を使用した Xamarin.Android アプリへのプッシュ通知の追加"
    description="Azure App Service と Azure Notification Hubs を使用して、Xamarin.Android アプリにプッシュ通知を送信する方法について説明します。"
    services="app-service\mobile" 
    documentationCenter="xamarin" 
    authors="ggailey777"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-android"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="12/02/2015" 
    ms.author="glenga"/>

# Xamarin.Android アプリへのプッシュ通知の追加

[AZURE.INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]
&nbsp;  
[AZURE.INCLUDE [app-service-mobile-note-mobile-services](../../includes/app-service-mobile-note-mobile-services.md)]

##概要

このチュートリアルへのプッシュ通知を追加、 [Xamarin.Android quickstart] プロジェクトのプッシュ通知が送信されるたびに、レコードが挿入されるようにします。 このチュートリアルに基づいて、 [Xamarin.Android quickstart] チュートリアルで、先に完了する必要があります。 ダウンロードしたクイック スタートのサーバー プロジェクトを使用しない場合は、プッシュ通知拡張機能パッケージをプロジェクトに追加する必要があります。 サーバーの拡張機能パッケージの詳細については、次を参照してください。 [Azure モバイル アプリの .NET バックエンド サーバー SDK と連携](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md)します。

##前提条件

このチュートリアルには、次のものが必要です。

+ アクティブな Google アカウント。 Google アカウントにサインアップすることができます [accounts.google.com](http://go.microsoft.com/fwlink/p/?LinkId=268302)します。
   
+ [Google Cloud Messaging クライアント コンポーネント](http://components.xamarin.com/view/GCMClient/)します。 このコンポーネントは、チュートリアル中に追加します。

+ 完了した [Xamarin.Android quickstart] チュートリアルです。


##<a name="create-hub"></a>通知ハブを作成する

[AZURE.INCLUDE [app-service-mobile-create-notification-hub](../../includes/app-service-mobile-create-notification-hub.md)]

##<a id="register"></a>Google Cloud Messaging を有効にする

[AZURE.INCLUDE [Enable GCM](../../includes/mobile-services-enable-google-cloud-messaging.md)]

##プッシュ要求を送信するように Mobile App バックエンドを構成する

[AZURE.INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push.md)]

##<a id="update-server"></a>サーバー プロジェクトをプッシュ通知を送信するように更新する

[AZURE.INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]

##<a id="configure-app"></a>プッシュ通知のクライアント プロジェクトを構成する

[AZURE.INCLUDE [mobile-services-xamarin-android-push-configure-project](../../includes/mobile-services-xamarin-android-push-configure-project.md)]

##<a id="add-push"></a>アプリケーションにプッシュ通知コードを追加する

[AZURE.INCLUDE [app-service-mobile-xamarin-android-push-add-to-app](../../includes/app-service-mobile-xamarin-android-push-add-to-app.md)]

## <a name="test"></a>アプリケーションでプッシュ通知をテストする

エミュレーターで仮想デバイスを使用して、アプリをテストできます。 エミュレーターで実行するときに必要な追加の構成手順があります。

1. 次に示すように Android Virtual Device (AVD) Manager で Google API がターゲットとして設定された仮想デバイスに対してデプロイまたはデバッグする必要があります。 

    ![](./media/app-service-mobile-xamarin-android-get-started-push/google-apis-avd-settings.png)

2. クリックして、Android デバイスに Google アカウントを追加 **アプリ** > **設定** > **アカウントを追加**, 、次の画面の指示を使用するのには、新規に作成するデバイスに既存の Google アカウントを追加します。

    ![](./media/app-service-mobile-xamarin-android-get-started-push/add-google-account.png)

3. これまでと同様に ToDoList アプリを実行し、新しい ToDo 項目を挿入します。 今回は、通知領域に通知アイコンが表示されます。 通知ドロアーを開いて、通知の全文を表示できます。

    ![](./media/app-service-mobile-xamarin-android-get-started-push/android-notifications.png)


<!-- URLs. -->
[Xamarin.Android quickstart]: app-service-mobile-xamarin-android-get-started.md

[Google Cloud Messaging Client Component]: http://components.xamarin.com/view/GCMClient/
[Xamarin.Android]: http://xamarin.com/download/
[Azure Mobile Services Component]: http://components.xamarin.com/view/azure-mobile-services/


