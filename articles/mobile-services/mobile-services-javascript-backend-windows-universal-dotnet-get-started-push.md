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

>[AZURE.NOTE]このトピックでは、Visual Studio 2013 Update 3 でツーリングを使用して、モバイル サービスからユニバーサル Windows アプリにプッシュ通知のサポートを追加する方法を説明します。 同じ手順を使用して、モバイル サービスから Windows ストアまたは Windows Phone ストア 8.1 アプリへのプッシュ通知を追加することができます。 Windows Phone 8 または Windows Phone Silverlight 8.1 アプリにプッシュ通知を追加するには、このバージョンを参照してください [モバイル サービスでプッシュ通知を使ってみる](mobile-services-javascript-backend-windows-phone-get-started-push.md)します。

このチュートリアルでは、プッシュ通知を有効にするための、次の基本的な手順について説明します。

1. [アプリケーションをプッシュ通知に登録する](#register)
2. [サービスを更新してプッシュ通知を送信する](#update-service)
3. [アプリケーションでプッシュ通知をテストする](#test)

このチュートリアルを完了するには、以下が必要です。

* アクティブな [Microsoft ストア アカウント](http://go.microsoft.com/fwlink/p/?LinkId=280045)します。
* [Visual Studio 2013 Express for Windows](http://go.microsoft.com/fwlink/?LinkId=257546) Update 3 またはそれ以降のバージョン

##<a id="register"></a>アプリケーションをプッシュ通知に登録する

[AZURE.INCLUDE [mobile-services-create-new-push-vs2013](../../includes/mobile-services-create-new-push-vs2013.md)]

&nbsp;&nbsp;6. 参照、 `\Services\MobileServices\your_service_name` 検査のプロジェクト フォルダーを生成された push.register.cs コード ファイルを開き、 **UploadChannel** 、通知ハブにデバイスの登録メソッドのチャネル URL です。

&nbsp;&nbsp;7. 共有 App.xaml.cs コード ファイルを開き、ことに注意して呼び出しを新しい **UploadChannel** でメソッドが追加された、 **OnLaunched** イベント ハンドラーです。 これにより、アプリケーションが起動されるたびにデバイスの登録が試行されます。

&nbsp;&nbsp;8. 余分な呼び出しを削除し、共有 App.xaml.cs ファイルでの Windows Phone ストア アプリ プロジェクトへのプッシュ通知を追加するには、前の手順を繰り返して **Mobile Service のクライアント**, 、**UploadChannel** され、残りの `#if...#endif` 条件付きラッパーです。 両方のプロジェクトでは、1 回の呼び出しを共有できるよう **UploadChannel**します。 

& nbsp; & nbsp;統合することにより、生成されたコードを単純にもことに注意してください、 `#if...#endif` ラップ [MobileServiceClient] 両方のバージョンのアプリで使用される単一のラップされていない定義を定義します。

アプリでプッシュ通知が有効にされたので、プッシュ通知を送信するにはモバイル サービスを更新する必要があります。 

##<a id="update-service"></a>サービスを更新してプッシュ通知を送信する

次の手順では、TodoItem テーブルに登録されている挿入スクリプトを更新します。 同様のコードを任意のサーバー スクリプトまたはバックエンド サービスの任意の場所に実装することができます。 

[AZURE.INCLUDE [mobile-services-javascript-update-script-notification-hubs](../../includes/mobile-services-javascript-update-script-notification-hubs.md)]


##<a id="test"></a>アプリケーションでプッシュ通知をテストする

[AZURE.INCLUDE [mobile-services-javascript-backend-windows-universal-test-push](../../includes/mobile-services-javascript-backend-windows-universal-test-push.md)]

## <a name="next-steps"> </a>次のステップ

このチュートリアルでは、Windows ストア アプリで Mobile Services および Notification Hubs を使用してプッシュ通知を送信できるようにするための基本について説明しました。 次に、次のチュートリアルのいずれかを完了します。

+ [認証されたユーザーへのプッシュ通知の送信](mobile-services-javascript-backend-windows-store-dotnet-push-notifications-app-users.md)
    <br/>タグを使用して、モバイル サービスから認証されたユーザーのみにプッシュ通知を送信する方法について説明します。

+ [登録者へのブロードキャスト通知の送信](../notification-hubs-windows-store-dotnet-send-breaking-news.md)
    <br/>ユーザーが興味のあるカテゴリに関してプッシュ通知を登録して、プッシュ通知を受信できるようにする方法について説明します。

+ [登録者へのプラットフォーム固有の通知を送信します。](../notification-hubs-aspnet-cross-platform-notify-users.md)
    <br/>テンプレートを使用して、バックエンドでプラットフォームに固有のペイロードを作成することがなく、モバイル サービスからプッシュ通知を送信する方法について説明します。

Mobile Services と Notification Hubs については次のトピックを参照してください。

* [Azure Notification Hubs - 診断ガイドライン](../notification-hubs-diagnosing.md)
    <br/>プッシュ通知の問題のトラブルシューティングを行う方法について説明します。

* [Get started with authentication]
  <br/>Mobile Services を使用して別のアカウントの種類のアプリケーションのユーザーを認証する方法について説明します。

* [What are Notification Hubs?]
  <br/>通知ハブがすべての主要なクライアント プラットフォーム全体のアプリケーションに通知を配信するための動作を説明します。

* [How to use a .NET client for Azure Mobile Services]
  <br/>Mobile Services を C# Windows アプリから使用する方法について説明します。

<!-- Anchors. -->

<!-- Images. -->

<!-- URLs. -->
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253
[Get started with Mobile Services]: mobile-services-dotnet-backend-windows-store-dotnet-get-started.md
[Get started with authentication]: mobile-services-javascript-backend-windows-universal-dotnet-get-started-users.md

[Send push notifications to authenticated users]: mobile-services-javascript-backend-windows-store-dotnet-push-notifications-app-users.md

[What are Notification Hubs?]: ../notification-hubs-overview.md

[How to use a .NET client for Azure Mobile Services]: mobile-services-windows-dotnet-how-to-use-client-library.md
[MobileServiceClient]: http://go.microsoft.com/fwlink/p/?LinkId=302030
 

