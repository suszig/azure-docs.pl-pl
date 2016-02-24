
<properties
    pageTitle="Android アプリ用 Azure Mobile Services の使用"
    description="次のチュートリアルに従って、Android 開発用の Azure Mobile Services を使用します。"
    services="mobile-services"
    documentationCenter="android"
    authors="RickSaling"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="mobile-services"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="get-started-article"
    ms.date="11/05/2015"
    ms.author="ricksal"/>


# <a name="getting-started"> </a>Mobile Services の使用

[AZURE.INCLUDE [mobile-service-note-mobile-apps](../../includes/mobile-services-note-mobile-apps.md)]

&nbsp;

[AZURE.INCLUDE [mobile-services-selector-get-started](../../includes/mobile-services-selector-get-started.md)]
&nbsp;

[AZURE.INCLUDE [mobile-services-hero-slug](../../includes/mobile-services-hero-slug.md)]

このチュートリアルでは、Azure Mobile Services を使用して Android アプリにクラウドベースのバックエンド サービスを追加する方法を示します。 このチュートリアルでは、新しいモバイル サービスと、新しいモバイル サービスにアプリ データを保存する簡単な _To do list_ アプリの両方を作成します。 作成するモバイル サービスは、サーバー側ビジネス ロジックのために Visual Studio を使用してサポートされる .NET 言語を使用し、モバイル サービスを管理します。 JavaScript でサーバー側ビジネス ロジックを記述できるモバイル サービスを作成するには、参照してください、 [JavaScript バックエンド バージョン](mobile-services-android-get-started.md) このトピックのです。

完成したアプリケーションのスクリーンショットは次のようになります。

![](./media/mobile-services-dotnet-backend-android-get-started/mobile-quickstart-completed-android.png)

[Android Developer Tools] [Android Studio] このチュートリアルを完了する必要を含む、Android Studio 統合開発環境、および最新の Android プラットフォームです。 このほか、Android のバージョン 4.2 以降が必要になります。

ダウンロードしたクイック スタート プロジェクトには、Android 向けの Mobile Services SDK が含まれています。

> [AZURE.IMPORTANT] このチュートリアルを完了するには、Azure アカウントが必要です。 アカウントがない場合は、Azure 試用版にサインアップして最大 10 の無料モバイル サービスを取得し、試用期間が終わった後でも使用し続けることができます。 詳細については、[Azure の無料試用版サイト](http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=AE564AB28)をご覧ください。


## <a name="create-new-service"> </a>新しいモバイル サービスを作成する

[AZURE.INCLUDE [mobile-services-dotnet-backend-create-new-service](../../includes/mobile-services-dotnet-backend-create-new-service.md)]

## ローカル コンピューターにモバイル サービスをダウンロードする

モバイル サービスを作成したので、個人用のモバイル サービス プロジェクトをダウンロードして、ローカル コンピューターまたは仮想マシンで動かすことができます。

1. 作成したモバイル サービスをクリックし、[クイック スタート] タブをクリックして **Android** [ **プラットフォームの選択]** 展開 **新しい Android アプリを作成**します。

    ![][1]

2. ダウンロードしてインストールをまだ行っていない場合、 [Visual Studio Professional 2013](https://go.microsoft.com/fwLink/p/?LinkID=391934), 、以降のバージョン。

3. ステップ 2. で、次のようにクリックします。 **ダウンロード** [ **のダウンロードとクラウドにサービスを発行**します。

    これにより、モバイル サービスを実装する Visual Studio プロジェクトがダウンロードされます。 圧縮されたプロジェクト ファイルをローカル コンピューターに保存し、保存場所を書き留めておいてください。

## モバイル サービスをテストする

[AZURE.INCLUDE [mobile-services-dotnet-backend-test-local-service](../../includes/mobile-services-dotnet-backend-test-local-service.md)]

## モバイル サービスを発行する

[AZURE.INCLUDE [mobile-services-dotnet-backend-publish-service](../../includes/mobile-services-dotnet-backend-publish-service.md)]

## 新しい Android アプリを作成する

ここでは、モバイル サービスに接続された新しい Android アプリを作成します。

1.  [Azure classic portal], 、] をクリックして **Mobile Services**, 、作成したモバイル サービスをクリックします。

2. クイック スタート] タブをクリックして **Android** [ **プラットフォームの選択]** 展開 **新しい Android アプリを作成**します。

    ![][2]

3. まだ行っていない、ダウンロードしてローカル コンピューターまたは仮想マシンで [Android Developer Tools] [Android SDK] をインストールします。

4. [ **をダウンロードしてアプリケーションを実行する**, 、] をクリックして **ダウンロード**します。

    これにより、サンプルのプロジェクトがダウンロード _作業の一覧に_ 、モバイル サービスに接続されているアプリケーションです。 圧縮されたプロジェクト ファイルをローカル コンピューターに保存し、保存場所を書き留めておいてください。

## Android アプリを実行する

[AZURE.INCLUDE [mobile-services-run-your-app](../../includes/mobile-services-android-get-started.md)]

## <a name="next-steps"> </a>次のステップ
クイック スタートはこれで完了です。Mobile Services で重要になるこれ以外の作業については、以下のトピックを参照してください。

* [アプリにプッシュ通知の追加]
  <br/>アプリケーションにごく基本的なプッシュ通知を送信する方法について説明します。

* [アプリに認証の追加]
  <br/>アプリの登録されている特定のユーザーに、バックエンド データへのアクセスを制限する方法について説明します。

* [Mobile Services .NET バックエンドのトラブルシューティングを行う]
  <br/> Mobile Services の .NET バックエンドで発生する問題を診断して解決する方法について説明します。

<!-- Anchors. -->
[Getting started with Mobile Services]:#getting-started
[Create a new mobile service]:#create-new-service
[Define the mobile service instance]:#define-mobile-service-instance
[Next Steps]:#next-steps

<!-- Images. -->
[0]: ./media/mobile-services-dotnet-backend-android-get-started/mobile-quickstart-completed-android.png
[1]: ./media/mobile-services-dotnet-backend-android-get-started/mobile-quickstart-steps-vs-AS.png
[2]: ./media/mobile-services-dotnet-backend-android-get-started/mobile-quickstart-steps-android-AS.png


[6]: ./media/mobile-services-dotnet-backend-android-get-started/mobile-portal-quickstart-android.png
[7]: ./media/mobile-services-dotnet-backend-android-get-started/mobile-quickstart-steps-android.png
[8]: ./media/mobile-services-dotnet-backend-android-get-started/mobile-eclipse-quickstart.png

[10]: ./media/mobile-services-dotnet-backend-android-get-started/mobile-quickstart-startup-android.png
[11]: ./media/mobile-services-dotnet-backend-android-get-started/mobile-data-tab.png
[12]: ./media/mobile-services-dotnet-backend-android-get-started/mobile-data-browse.png

[14]: ./media/mobile-services-dotnet-backend-android-get-started/mobile-services-import-android-workspace.png
[15]: ./media/mobile-services-dotnet-backend-android-get-started/mobile-services-import-android-project.png

<!-- URLs. -->
[Get started (Eclipse)]: mobile-services-dotnet-backend-android-get-started-ec.md
[Add push notifications to your app]: mobile-services-dotnet-backend-android-get-started-push.md
[Add authentication to your app]: mobile-services-dotnet-backend-android-get-started-auth.md
[Android SDK]: https://go.microsoft.com/fwLink/p/?LinkID=280125
[Android Studio]: https://developer.android.com/sdk/index.html
[Mobile Services Android SDK]: https://go.microsoft.com/fwLink/p/?LinkID=266533
[Troubleshoot a Mobile Services .NET backend]: mobile-services-dotnet-backend-how-to-troubleshoot.md

[Azure classic portal]: https://manage.windowsazure.com/

