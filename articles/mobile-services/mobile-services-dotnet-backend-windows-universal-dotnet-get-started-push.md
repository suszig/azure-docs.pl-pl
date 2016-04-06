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

##概要
このトピックでは、Azure Mobile Services を .NET バックエンドで使用してユニバーサル Windows アプリにプッシュ通知を送信する方法について説明します。 このチュートリアルでは、ユニバーサル Windows アプリ プロジェクトの Azure Notification Hubs を使用したプッシュ通知を有効にします。 完了すると、TodoList テーブルにレコードが挿入されるたびに、モバイル サービスは、.NET バックエンドから、登録されたすべての Windows ストア アプリおよび Windows Phone ストア アプリにプッシュ通知を送信します。 作成する通知ハブはモバイル サービスでは無料で、モバイル サービスから独立して管理することができ、他のアプリケーションおよびサービスで使用できます。

このチュートリアルを完了するには、以下が必要です。

* アクティブな [Microsoft ストア アカウント](http://go.microsoft.com/fwlink/p/?LinkId=280045)します。
* <a href="https://go.microsoft.com/fwLink/p/?LinkID=391934" target="_blank">Visual Studio Community 2013</a>。

##<a id="register"></a>アプリケーションをプッシュ通知に登録する

[AZURE.INCLUDE [mobile-services-create-new-push-vs2013](../../includes/mobile-services-create-new-push-vs2013.md)]

&nbsp;&nbsp;6. 参照、 `\Services\MobileServices\your_service_name` 検査のプロジェクト フォルダーを生成された push.register.cs コード ファイルを開き、 **UploadChannel** 、通知ハブにデバイスの登録メソッドのチャネル URL です。

&nbsp;&nbsp;7. 共有 App.xaml.cs コード ファイルを開き、ことに注意して呼び出しを新しい **UploadChannel** でメソッドが追加された、 **OnLaunched** イベント ハンドラーです。 これにより、アプリケーションが起動されるたびにデバイスの登録が試行されます。

&nbsp;&nbsp;8. 余分な呼び出しを削除し、共有 App.xaml.cs ファイルでの Windows Phone ストア アプリ プロジェクトへのプッシュ通知を追加するには、前の手順を繰り返して **UploadChannel** され、残りの `#if...#endif` 条件付きラッパーです。 両方のプロジェクトでは、1 回の呼び出しを共有できるよう **UploadChannel**します。

> [AZURE.NOTE] 統合することによって生成されたコードを単純化することもできます、 `#if...#endif` ラップ [MobileServiceClient](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient.aspx) 両方のバージョンのアプリで使用される単一のラップされていない定義を定義します。

アプリでプッシュ通知が有効にされたので、プッシュ通知を送信するにはモバイル サービスを更新する必要があります。

##<a id="update-service"></a>サービスを更新してプッシュ通知を送信する

次の手順では、既存の TodoItemController クラスを更新して、新しい項目が挿入されたときにプッシュ通知を登録済みのすべてのデバイスに送信します。 似たようなコードを実装するには任意のカスタム [ApiController](https://msdn.microsoft.com/library/system.web.http.apicontroller.aspx), 、[TableController](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobile.service.tables.tablecontroller.aspx), 、またはバックエンド サービスで他の場所。

[AZURE.INCLUDE [mobile-services-dotnet-backend-update-server-push](../../includes/mobile-services-dotnet-backend-update-server-push.md)]

##<a id="local-testing"></a>ローカル テストのためにプッシュ通知を有効にする

[AZURE.INCLUDE [mobile-services-dotnet-backend-configure-local-push-vs2013](../../includes/mobile-services-dotnet-backend-configure-local-push-vs2013.md)]

このセクションの残りの手順はオプションです。 これらの手順により、ローカル コンピューターで実行されているモバイル サービスに対してアプリをテストできます。 Azure で実行されているモバイル サービスを使用してプッシュ通知をテストする場合は、最後のセクションまでスキップしてもかまいません。 これは、既にプッシュ通知の追加ウィザードで、Azure で実行されているサービスに接続するようにアプリが構成されているためです。

>[AZURE.NOTE]テストするための運用環境のモバイル サービスを使用しないでや開発作業をします。 モバイル サービス プロジェクトは必ずテスト用の個別のステージング サービスに発行してください。

&nbsp;&nbsp;5. 共有 App.xaml.cs プロジェクト ファイルを開き、いずれかの新しいインスタンスを作成するコードの行を探します、 [MobileServiceClient] Azure で実行されているモバイル サービスにアクセスするクラス。

&nbsp;&nbsp;6. このコードのコメント アウトし、新たに作成するコードを追加 [MobileServiceClient] の名前が同じで、次のように、コンス トラクターで、ローカル ホストの URL を使用します。

    // This MobileServiceClient has been configured to communicate with your local
    // test project for debugging purposes.
    public static MobileServiceClient todolistClient = new MobileServiceClient(
        "http://localhost:4584"
    );

& nbsp; & nbsp;これを使用して [MobileServiceClient], 、アプリは Azure でホストされているバージョンではなくローカルのサービスに接続します。 Azure でホストされているモバイル サービスに対してアプリを実行する場合は、元に戻す変更 [MobileServiceClient] 定義します。

##<a id="test"></a>アプリケーションでプッシュ通知をテストする

[AZURE.INCLUDE [mobile-services-dotnet-backend-windows-universal-test-push](../../includes/mobile-services-dotnet-backend-windows-universal-test-push.md)]

## <a name="next-steps"> </a>次のステップ

このチュートリアルでは、Windows ストア アプリで Mobile Services および Notification Hubs を使用してプッシュ通知を送信できるようにするための基本について説明しました。 次のチュートリアルを完了してください [Send push notifications to authenticated users], 、タグを使用してモバイル サービスから認証されたユーザーのみにプッシュ通知を送信する方法を説明します。

Mobile Services と Notification Hubs については次のトピックを参照してください。

* [既存のアプリケーションへの Mobile Services の追加][Get started with data]
  <br/>Mobile Services を使用してデータの格納およびクエリを実行する方法について説明します。

* [アプリへの認証の追加][Get started with authentication]
  <br/>Mobile Services を使用して別のアカウントの種類のアプリケーションのユーザーを認証する方法について説明します。

* [What are Notification Hubs?]
  <br/>通知ハブがすべての主要なクライアント プラットフォーム全体のアプリケーションに通知を配信するための動作を説明します。

* [通知ハブのデバッグ](http://go.microsoft.com/fwlink/p/?linkid=386630)
  </br>Notification Hubs ソリューションのトラブルシューティングとデバッグについて説明します。

* [How to use a .NET client for Azure Mobile Services]
  <br/>Mobile Services を C# Windows アプリから使用する方法について説明します。

<!-- Anchors. -->

<!-- Images. -->

<!-- URLs. -->
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253
[Get started with Mobile Services]: mobile-services-dotnet-backend-windows-store-dotnet-get-started.md
[Get started with data]: mobile-services-dotnet-backend-windows-universal-dotnet-get-started-data.md
[Get started with authentication]: mobile-services-dotnet-backend-windows-universal-dotnet-get-started-users.md

[Send push notifications to authenticated users]: mobile-services-dotnet-backend-windows-store-dotnet-push-notifications-app-users.md

[What are Notification Hubs?]: ../notification-hubs-overview.md

[How to use a .NET client for Azure Mobile Services]: mobile-services-windows-dotnet-how-to-use-client-library.md
[MobileServiceClient]: http://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient.aspx

