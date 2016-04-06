<properties
    pageTitle="Xamarin iOS アプリケーション用 Mobile Services の使用 | Microsoft Azure"
    description="次のチュートリアルに従って、Xamarin iOS 開発用の Azure Mobile Services を使用します。"
    services="mobile-services"
    documentationCenter="xamarin"
    authors="conceptdev"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="mobile-services"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-ios"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="11/05/2015"
    ms.author="craig.dunn@xamarin.com"/>

# <a name="getting-started"> </a>Mobile Services の使用

[AZURE.INCLUDE [mobile-service-note-mobile-apps](../../includes/mobile-services-note-mobile-apps.md)]

&nbsp;


[AZURE.INCLUDE [mobile-services-selector-get-started](../../includes/mobile-services-selector-get-started.md)]
&nbsp;

[AZURE.INCLUDE [mobile-services-hero-slug](../../includes/mobile-services-hero-slug.md)]

このチュートリアルでは、Azure Mobile Services を使用して Xamarin.iOS アプリにクラウドベースのバックエンド サービスを追加する方法を示します。 このチュートリアルでは、新しいモバイル サービスと、新しいモバイル サービスにアプリ データを保存する簡単な *To do list* アプリの両方を作成します。

このチュートリアルのビデオを見る場合は、下のクリップでこのチュートリアルと同じ手順が表示されます。

ビデオ:「を取得する開始と Xamarin と Azure モバイル サービス」Craig dunn 氏 Xamarin のデベロッパー エバンジェ リスト (継続時間: 10時 05分分)

> [AZURE.VIDEO getting-started-with-xamarin-and-mobile-services]

完成したアプリケーションのスクリーンショットは次のようになります。

![][0]

このチュートリアルを完了するには、XCode が必要です。 と [Xamarin Studio] OS X の Xamarin Visual Studio Windows で Visual Studio のプラグインです。 サンプルは iOS 5.0 以降で実行します。

> [AZURE.IMPORTANT] このチュートリアルを完了するには、Azure アカウントが必要です。 アカウントがない場合は、Azure 試用版にサインアップして、最大 10 件の無料モバイル サービスを入手できます。このサービスは評価終了後も使用できます。 詳細については、[Azure の無料試用版サイト](http://azure.microsoft.com/pricing/free-trial/)をご覧ください。

## <a name="create-new-service"> </a>新しいモバイル サービスを作成する

[AZURE.INCLUDE [mobile-services-create-new-service](../../includes/mobile-services-create-new-service.md)]

## 新しい Xamarin.iOS アプリケーションを作成する

モバイル サービスを作成したら、Azure クラシック ポータルの簡単なクイック スタートに従って、新しいアプリケーションを作成するか、既存のアプリを変更して、モバイル サービスに接続することができます。

ここでは、モバイル サービスに接続された新しい Xamarin.iOS アプリを作成します。

1.   [Azure classic portal], 、] をクリックして **Mobile Services**, 、作成したモバイル サービスをクリックします。

2. クイック スタート] タブをクリックして **Xamarin.iOS** [ **プラットフォームの選択]** 展開 **新しい Xamarin.iOS アプリケーションを作成**します。

    ![][6]

    これにより、モバイル サービスに接続された Xamarin.iOS アプリケーションを作成するための簡単な 3 つの手順が表示されます。

    ![][7]

3. まだ行っていない、ダウンロードして Xcode (推奨は、最新バージョンの Xcode 6.0 以降です) をインストールし、 [Xamarin Studio]します。

4. クリックして **作成 TodoItems テーブル** アプリケーション データを格納するテーブルを作成します。

5.  **をダウンロードしてアプリケーションを実行する**, をクリックして **ダウンロード**します。

    これにより、サンプルのプロジェクトがダウンロード _作業の一覧に_ アプリケーション、モバイル サービスに接続されていて、Xamarin.iOS 向け Azure Mobile Services コンポーネントを参照します。 圧縮されたプロジェクト ファイルをローカル コンピューターに保存し、保存場所を書き留めておいてください。

## 新しい Xamarin.iOS アプリケーションを実行する

このチュートリアルの最後に、新しいアプリケーションをビルドして実行します。

1. 圧縮されたプロジェクト ファイルを保存した場所を参照し、ファイル、コンピューター上に展開し、開く、 **XamarinTodoQuickStart.iOS.sln** Xamarin Studio または Visual Studio を使用してソリューション ファイルです。

    ![][8]

    ![][9]

2. キーを押して、 **実行** 、プロジェクトをビルドし、既定でこのプロジェクトである iPhone エミュレーターでアプリケーションを開始] ボタンをクリックします。

3. アプリケーションで、意味のあるテキストをなど入力 _に関するチュートリアルを完了_ プラス記号をクリックして (**+**) のアイコン。

    ![][10]

    これで、Azure でホストされている新しいモバイル サービスに POST 要求が送信されます。 要求のデータは TodoItem テーブルに挿入されます。 テーブルに格納された項目がモバイル サービスによって返され、データが一覧に表示されます。

    > [AZURE.NOTE] モバイル サービスを照会および挿入 TodoService.cs c# ファイルにあるデータにアクセスするコードを確認できます。

4. 戻り、 [Azure classic portal], をクリックして、 **データ** ] タブでをクリックし、 **TodoItems** テーブルです。

    ![][11]

    これで、アプリケーションによってテーブルに挿入されたデータを参照できます。

    ![][12]


## 次のステップ
クイック スタートはこれで完了です。Mobile Services で重要になるこれ以外の作業については、以下のトピックを参照してください。

* [Get started with offline data sync]
  クイック スタートでオフライン データの同期を使用して、アプリケーションの応答性と信頼性を高める方法について説明します。

* [Get started with authentication]
  ID プロバイダーを使用してアプリケーションのユーザーを認証する方法について説明します。

* [Get started with push notifications]
  アプリケーションにごく基本的なプッシュ通知を送信する方法について説明します。

* [Azure モバイル サービス向け Xamarin コンポーネント クライアントを使用する方法](partner-xamarin-mobile-services-how-to-use-client-library.md)
   モバイル サービスのクエリ、データの処理、およびカスタム Api にアクセスする方法について説明します。


[AZURE.INCLUDE [app-service-disqus-feedback-slug](../../includes/app-service-disqus-feedback-slug.md)]

<!-- Anchors. -->
[Getting started with Mobile Services]:#getting-started
[Create a new mobile service]:#create-new-service
[Define the mobile service instance]:#define-mobile-service-instance
[Next Steps]:#next-steps

<!-- Images. -->
[0]: ./media/partner-xamarin-mobile-services-ios-get-started/mobile-quickstart-completed-ios.png
[6]: ./media/partner-xamarin-mobile-services-ios-get-started/mobile-portal-quickstart-xamarin-ios.png
[7]: ./media/partner-xamarin-mobile-services-ios-get-started/mobile-quickstart-steps-xamarin-ios.png
[8]: ./media/partner-xamarin-mobile-services-ios-get-started/mobile-xamarin-project-ios-xs.png
[9]: ./media/partner-xamarin-mobile-services-ios-get-started/mobile-xamarin-project-ios-vs.png
[10]: ./media/partner-xamarin-mobile-services-ios-get-started/mobile-quickstart-startup-ios.png
[11]: ./media/partner-xamarin-mobile-services-ios-get-started/mobile-data-tab.png
[12]: ./media/partner-xamarin-mobile-services-ios-get-started/mobile-data-browse.png


<!-- URLs. -->
[Get started with offline data sync]: mobile-services-xamarin-ios-get-started-offline-data.md
[Get started with authentication]: partner-xamarin-mobile-services-ios-get-started-users.md
[Get started with push notifications]: partner-xamarin-mobile-services-ios-get-started-push.md

[Xamarin Studio]: http://xamarin.com/download
[Mobile Services iOS SDK]: https://go.microsoft.com/fwLink/p/?LinkID=266533

[Azure classic portal]: https://manage.windowsazure.com/


