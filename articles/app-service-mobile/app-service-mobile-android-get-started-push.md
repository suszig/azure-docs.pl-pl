<properties
    pageTitle="Azure のモバイル アプリを使用して Android アプリにプッシュ通知を追加します。"
    description="Azure Mobile Apps を使用して Android アプリにプッシュ通知を送信する方法について説明します。"
    services="app-service\mobile"
    documentationCenter="android"
    manager="dwrede"
    editor=""
    authors="ysxu"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="12/02/2015"
    ms.author="yuaxu"/>


# プッシュ通知を Android アプリに追加する

[AZURE.INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]
&nbsp;  
[AZURE.INCLUDE [app-service-mobile-note-mobile-services](../../includes/app-service-mobile-note-mobile-services.md)]

## 概要

このチュートリアルで追加するプッシュ通知 [Android のクイック スタート] プロジェクトをレコードが挿入されるたびにプッシュ通知が送信できるようにします。 このチュートリアルは、先に完了する必要があります [Android のクイック スタート] のチュートリアルに基づきます。 ダウンロードしたクイック スタートのサーバー プロジェクトを使用しない場合は、プッシュ通知拡張機能パッケージをプロジェクトに追加する必要があります。 サーバーの拡張機能パッケージの詳細については、次を参照してください。 [Azure モバイル アプリの .NET バックエンド サーバー SDK と連携](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md)します。

## 前提条件

このチュートリアルを完了するには、以下が必要です。

* [Google アカウント](http://go.microsoft.com/fwlink/p/?LinkId=268302) 検証済みの電子メール アドレスを使用します。
* [Visual Studio Community 2013](https://go.microsoft.com/fwLink/p/?LinkID=391934)& mdash; Node.js バックエンド プロジェクトでは必要ありません。
* 完了、 [クイック スタート チュートリアル](../app-service-mobile-android-get-started.md)します。

## <a name="create-hub"></a>通知ハブを作成します。

[AZURE.INCLUDE [app-service-mobile-create-notification-hub](../../includes/app-service-mobile-create-notification-hub.md)]

## Google Cloud Messaging を有効にする

[AZURE.INCLUDE [mobile-services-enable-google-cloud-messaging](../../includes/mobile-services-enable-google-cloud-messaging.md)]

## プッシュ要求を送信するように Mobile App バックエンドを構成する

[AZURE.INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push.md)]

## <a id="update-service"></a>プッシュ通知を送信するサーバー プロジェクトを更新します。

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-configure-push-google](../../includes/app-service-mobile-dotnet-backend-configure-push-google.md)]

## <a name="publish-the-service"></a>モバイル バックエンドを Azure に発行します。

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-publish-service](../../includes/app-service-mobile-dotnet-backend-publish-service.md)]

## アプリケーションにプッシュ通知を追加する

### Android SDK バージョンの検証

[AZURE.INCLUDE [app-service-mobile-verify-android-sdk-version](../../includes/app-service-mobile-verify-android-sdk-version.md)]

次の手順は、Google Play サービスをインストールすることです。 Google Cloud Messaging には、マニフェストの **minSdkVersion** プロパティが準拠する必要がある、開発およびテストに関する最小 API レベル要件があります。

古いデバイスを使用している場合は、Google Play Services SDK のセットアップに関するページを参考に、どれだけ小さな値を設定できるか判断し、適切に設定してください。

### プロジェクトへの Google Play Services の追加

[AZURE.INCLUDE [Add Play Services](../../includes/app-service-mobile-add-google-play-services.md)]

### コードの追加

[AZURE.INCLUDE [app-service-mobile-android-getting-started-with-push](../../includes/app-service-mobile-android-getting-started-with-push.md)]

## 発行されたモバイル サービスに対してアプリケーションをテストする

Android フォンを USB ケーブルで直接接続するか、エミュレーターで仮想デバイスを使用する方法により、アプリケーションをテストできます。

## <a id="more"></a>以上

* タグを利用すれば、特定の区分に属する顧客にプッシュで的を絞ることができます。 [Azure モバイル アプリの .NET バックエンド サーバー SDK と連携](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) デバイスのインストールにタグを追加する方法を示します。



[android quick start]: app-service-mobile-android-get-started.md 

