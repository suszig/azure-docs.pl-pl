<properties 
    pageTitle="ユニバーサル Windows 8.1 アプリへのプッシュ通知の追加 | Microsoft Azure" 
    description="Azure Notification Hubs を使用して、JavaScript バックエンド モバイル サービスからユニバーサル Windows 8.1 アプリにプッシュ通知を送信する方法について説明します。" 
    services="mobile-services,notification-hubs" 
    documentationCenter="windows" 
    authors="ggailey777" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="mobile-services" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-windows" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="11/10/2015" 
    ms.author="glenga"/>



# Mobile Services アプリへのプッシュ通知の追加

[AZURE.INCLUDE [mobile-services-selector-get-started-push](../../includes/mobile-services-selector-get-started-push.md)]

このトピックでは、Azure のモバイル サービスを JavaScript バックエンドで使用してユニバーサル Windows アプリにプッシュ通知を送信する方法について説明します。 このチュートリアルでは、ユニバーサル Windows アプリ プロジェクトの Azure Notification Hubs を使用したプッシュ通知を有効にします。 完了すると、TodoList テーブルにレコードが挿入されるたびに、モバイル サービスは、JavaScript バックエンドから、登録されたすべての Windows ストア アプリおよび Windows Phone ストア アプリにプッシュ通知を送信します。 作成する通知ハブはモバイル サービスでは無料で、モバイル サービスから独立して管理することができ、他のアプリケーションおよびサービスで使用できます。
>[AZURE.NOTE]このトピックでは、Visual Studio 2013 with Update 3 でツーリングを使用して、Mobile Services からユニバーサル Windows アプリへのプッシュ通知のサポートを追加する方法について説明します。 同じ手順を使用して、モバイル サービスから Windows ストアまたは Windows Phone ストア 8.1 アプリへのプッシュ通知を追加することができます。 Windows Phone 8 または Windows Phone Silverlight 8.1 アプリにプッシュ通知を追加するには、このバージョンを参照してください [モバイル サービスでプッシュ通知を使ってみる](mobile-services-javascript-backend-windows-phone-get-started-push.md)します。

このチュートリアルでは、プッシュ通知を有効にするための、次の基本的な手順について説明します。

1. [アプリのプッシュ通知に登録します。](#register)
2. [サービスを更新してプッシュ通知を送信するには](#update-service)
3. [アプリでプッシュ通知をテストします。](#test)

このチュートリアルを完了するには、以下が必要です。

* アクティブな [Microsoft ストア アカウント](http://go.microsoft.com/fwlink/p/?LinkId=280045)します。
* [Visual Studio 2013 Express for Windows](http://go.microsoft.com/fwlink/?LinkId=257546) Update 3 またはそれ以降のバージョン

## <a id="register"></a>アプリのプッシュ通知に登録します。

[AZURE.INCLUDE [mobile-services-create-new-push-vs2013](../../includes/mobile-services-create-new-push-vs2013.md)]

&nbsp;&nbsp;6. `\Services\MobileServices\your_service_name` プロジェクト フォルダーに移動し、生成された push.register.cs コード ファイルを開き、デバイスのチャネル URL を通知ハブに登録する **UploadChannel** メソッドを見つけます。

&nbsp;&nbsp;7. 共有 App.xaml.cs コード ファイルを開き、新しい **UploadChannel** メソッドの呼び出しが **OnLaunched** イベント ハンドラーに追加されていることを確認します。 これにより、アプリケーションが起動されるたびにデバイスの登録が試行されます。

&nbsp;&nbsp;8. 余分な呼び出しを削除し、共有 App.xaml.cs ファイルでの Windows Phone ストア アプリ プロジェクトへのプッシュ通知を追加するには、前の手順を繰り返して **Mobile Service のクライアント**, 、**UploadChannel** され、残りの `#if... #endif` 条件付きラッパーです。 これで、両方のプロジェクトで **UploadChannel** の 1 回の呼び出しを共有できるようになりました。

& nbsp; & nbsp;統合することにより、生成されたコードを単純にもことに注意してください、 `#if... #endif` ラップされたアプリの両方のバージョンで使用される 1 つのラップされていない定義に [MobileServiceClient] 定義します。

アプリでプッシュ通知が有効にされたので、プッシュ通知を送信するにはモバイル サービスを更新する必要があります。

## <a id="update-service"></a>サービスを更新してプッシュ通知を送信するには

次の手順では、TodoItem テーブルに登録されている挿入スクリプトを更新します。 同様のコードを任意のサーバー スクリプトまたはバックエンド サービスの任意の場所に実装することができます。

[AZURE.INCLUDE [mobile-services-javascript-update-script-notification-hubs](../../includes/mobile-services-javascript-update-script-notification-hubs.md)]


## <a id="test"></a> アプリでプッシュ通知をテストします。

[AZURE.INCLUDE [mobile-services-javascript-backend-windows-universal-test-push](../../includes/mobile-services-javascript-backend-windows-universal-test-push.md)]

## <a name="next-steps"> </a>次のステップ

このチュートリアルでは、Windows ストア アプリで Mobile Services および Notification Hubs を使用してプッシュ通知を送信できるようにするための基本について説明しました。 次に、次のチュートリアルのいずれかを完了します。

+ [認証されたユーザーにプッシュ通知を送信します。](mobile-services-javascript-backend-windows-store-dotnet-push-notifications-app-users.md)
    <br/>タグを使用して、モバイル サービスから認証されたユーザーのみにプッシュ通知を送信する方法について説明します。

+ [登録者へのブロードキャスト通知を送信します。](../notification-hubs-windows-store-dotnet-send-breaking-news.md)
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

* [Azure Mobile Services 向け .NET クライアントを使用する方法]
  <br/>C# Windows アプリからモバイル サービスを使用する方法について説明します。







[submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582 
[my applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039 
[live sdk for windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253 
[get started with mobile services]: mobile-services-dotnet-backend-windows-store-dotnet-get-started.md 
[get started with authentication]: mobile-services-javascript-backend-windows-universal-dotnet-get-started-users.md 
[send push notifications to authenticated users]: mobile-services-javascript-backend-windows-store-dotnet-push-notifications-app-users.md 
[what are notification hubs?]: ../notification-hubs-overview.md 
[how to use a .net client for azure mobile services]: mobile-services-windows-dotnet-how-to-use-client-library.md 
[mobileserviceclient]: http://go.microsoft.com/fwlink/p/?LinkId=302030 

