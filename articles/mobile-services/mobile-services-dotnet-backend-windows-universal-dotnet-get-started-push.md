<properties
    pageTitle="ユニバーサル Windows 8.1 アプリへのプッシュ通知の追加 | Microsoft Azure"
    description="Azure Notification Hubs を使用して .NET バックエンド モバイル サービスからユニバーサル Windows 8.1 アプリにプッシュ通知を送信する方法について説明します。"
    services="mobile-services,notification-hubs"
    documentationCenter="windows"
    authors="ggailey777"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="mobile-services"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows-store"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="11/11/2015"
    ms.author="glenga"/>


# Mobile Services アプリへのプッシュ通知の追加

[AZURE.INCLUDE [mobile-service-note-mobile-apps](../../includes/mobile-services-note-mobile-apps.md)]

&nbsp;


[AZURE.INCLUDE [mobile-services-selector-get-started-push](../../includes/mobile-services-selector-get-started-push.md)]

## 概要

このトピックでは、Azure Mobile Services を .NET バックエンドで使用してユニバーサル Windows アプリにプッシュ通知を送信する方法について説明します。 このチュートリアルでは、ユニバーサル Windows アプリ プロジェクトの Azure Notification Hubs を使用したプッシュ通知を有効にします。 完了すると、TodoList テーブルにレコードが挿入されるたびに、モバイル サービスは、.NET バックエンドから、登録されたすべての Windows ストア アプリおよび Windows Phone ストア アプリにプッシュ通知を送信します。 作成する通知ハブはモバイル サービスでは無料で、モバイル サービスから独立して管理することができ、他のアプリケーションおよびサービスで使用できます。

このチュートリアルを完了するには、以下が必要です。

* アクティブな [Microsoft ストア アカウント](http://go.microsoft.com/fwlink/p/?LinkId=280045)します。
* <a href="https://go.microsoft.com/fwLink/p/?LinkID=391934" target="_blank">Visual Studio Community 2013</a>します。

## <a id="register"></a>アプリのプッシュ通知に登録します。

[AZURE.INCLUDE [mobile-services-create-new-push-vs2013](../../includes/mobile-services-create-new-push-vs2013.md)]

&nbsp;&nbsp;6. `\Services\MobileServices\your_service_name` プロジェクト フォルダーに移動し、生成された push.register.cs コード ファイルを開き、デバイスのチャネル URL を通知ハブに登録する **UploadChannel** メソッドを見つけます。

&nbsp;&nbsp;7. 共有 App.xaml.cs コード ファイルを開き、新しい **UploadChannel** メソッドの呼び出しが **OnLaunched** イベント ハンドラーに追加されていることを確認します。 これにより、アプリケーションが起動されるたびにデバイスの登録が試行されます。

&nbsp;&nbsp;8. 直前の手順を繰り返して、Windows Phone ストア アプリ プロジェクトへのプッシュ通知を追加し、続いて共有 App.xaml.cs ファイルで、**UploadChannel** の余分の呼び出しと残りの `#if...#endif` 条件付きラッパーを削除します。 これで、両方のプロジェクトで **UploadChannel** の 1 回の呼び出しを共有できるようになりました。
> [AZURE.NOTE] 統合することによって生成されたコードを単純化することもできます、 `#if... #endif` ラップ [MobileServiceClient](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient.aspx) アプリの両方のバージョンで使用される 1 つのラップされていない定義を定義します。

アプリでプッシュ通知が有効にされたので、プッシュ通知を送信するにはモバイル サービスを更新する必要があります。

## <a id="update-service"></a>サービスを更新してプッシュ通知を送信するには

次の手順では、既存の TodoItemController クラスを更新して、新しい項目が挿入されたときにプッシュ通知を登録済みのすべてのデバイスに送信します。 似たようなコードを実装するには任意のカスタム [ApiController](https://msdn.microsoft.com/library/system.web.http.apicontroller.aspx), 、[TableController](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobile.service.tables.tablecontroller.aspx), 、またはバックエンド サービスで他の場所。

[AZURE.INCLUDE [mobile-services-dotnet-backend-update-server-push](../../includes/mobile-services-dotnet-backend-update-server-push.md)]

## <a id="local-testing"></a> ローカル テストのためのプッシュ通知を有効にします。

[AZURE.INCLUDE [mobile-services-dotnet-backend-configure-local-push-vs2013](../../includes/mobile-services-dotnet-backend-configure-local-push-vs2013.md)]

このセクションの残りの手順はオプションです。 これらの手順により、ローカル コンピューターで実行されているモバイル サービスに対してアプリをテストできます。 Azure で実行されているモバイル サービスを使用してプッシュ通知をテストする場合は、最後のセクションまでスキップしてもかまいません。 これは、既にプッシュ通知の追加ウィザードで、Azure で実行されているサービスに接続するようにアプリが構成されているためです。
>[AZURE.NOTE]運用環境のモバイル サービスをテストや開発作業を行う目的で使用しないでください。 モバイル サービス プロジェクトは必ずテスト用の個別のステージング サービスに発行してください。

&nbsp;&nbsp;5. 共有 App.xaml.cs プロジェクト ファイルを開き、いずれかを Azure で実行されているモバイル サービスにアクセスする [MobileServiceClient] クラスの新しいインスタンスを作成するコード行を探します。

&nbsp;&nbsp;6. このコードをコメント アウトは、同じ名前の新しい [MobileServiceClient] を作成するコードを追加して、次のように、コンス トラクターで、ローカル ホストの URL を使用します。

    // This MobileServiceClient has been configured to communicate with your local
    // test project for debugging purposes.
    public static MobileServiceClient todolistClient = new MobileServiceClient(
        "http://localhost:4584"
    );

& nbsp; & nbsp;この [MobileServiceClient] を使用して、アプリケーションは Azure でホストされているバージョンではなくローカル サービスに接続します。 Azure でホストされているモバイル サービスに対してアプリを実行する場合は、[MobileServiceClient] の定義を元に戻す変更します。

## <a id="test"></a> アプリでプッシュ通知をテストします。

[AZURE.INCLUDE [mobile-services-dotnet-backend-windows-universal-test-push](../../includes/mobile-services-dotnet-backend-windows-universal-test-push.md)]

## <a name="next-steps"> </a>次のステップ

このチュートリアルでは、Windows ストア アプリで Mobile Services および Notification Hubs を使用してプッシュ通知を送信できるようにするための基本について説明しました。 次に、チュートリアルを完了する、次へ]、[認証されたユーザーに送信プッシュ通知] タグを使用してモバイル サービスから認証されたユーザーのみにプッシュ通知を送信する方法を示したを検討してください。

Mobile Services と Notification Hubs については次のトピックを参照してください。

* [既存のアプリケーションへの Mobile Services の追加 ][get started with data]
  <br/>格納して、モバイル サービスを使用してデータのクエリについて説明します。

* [アプリへの認証の追加 ][get started with authentication]
  <br/>別のアカウントの種類のモバイル サービスを使用して、アプリのユーザーを認証する方法について説明します。

* [通知ハブは何ですか?]
  <br/>通知を配信するアプリケーションにすべての主要なクライアントのプラットフォームの通知ハブの動作について説明します。

* [Notification Hubs アプリケーションをデバッグします。](http://go.microsoft.com/fwlink/p/?linkid=386630)
  </br>ガイドのトラブルシューティングと Notification Hubs ソリューションのデバッグを取得します。

* [Azure Mobile Services 向け .NET クライアントを使用する方法]
  <br/>C# Windows アプリからモバイル サービスを使用する方法について説明します。







[submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582 
[my applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039 
[live sdk for windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253 
[get started with mobile services]: mobile-services-dotnet-backend-windows-store-dotnet-get-started.md 
[get started with data]: mobile-services-dotnet-backend-windows-universal-dotnet-get-started-data.md 
[get started with authentication]: mobile-services-dotnet-backend-windows-universal-dotnet-get-started-users.md 
[send push notifications to authenticated users]: mobile-services-dotnet-backend-windows-store-dotnet-push-notifications-app-users.md 
[what are notification hubs?]: ../notification-hubs-overview.md 
[how to use a .net client for azure mobile services]: mobile-services-windows-dotnet-how-to-use-client-library.md 
[mobileserviceclient]: http://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient.aspx 

