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


# <a name="getting-started"> </a>モバイル サービスを使ってみる

[AZURE.INCLUDE [mobile-service-note-mobile-apps](../../includes/mobile-services-note-mobile-apps.md)]

&nbsp;


[AZURE.INCLUDE [mobile-services-selector-get-started](../../includes/mobile-services-selector-get-started.md)]
&nbsp;

[AZURE.INCLUDE [mobile-services-hero-slug](../../includes/mobile-services-hero-slug.md)]

このチュートリアルでは、Azure Mobile Services を使用して Xamarin.iOS アプリにクラウドベースのバックエンド サービスを追加する方法を示します。 このチュートリアルでは、新しいモバイル サービスと、新しいモバイル サービスにアプリケーション データを保存する簡単な *To do list* アプリケーションの両方を作成します。

このチュートリアルのビデオを見る場合は、下のクリップでこのチュートリアルと同じ手順が表示されます。

ビデオ:「を取得する開始と Xamarin と Azure モバイル サービス」Craig dunn 氏 Xamarin のデベロッパー エバンジェ リスト (継続時間: 10時 05分分)

> [AZURE.VIDEO getting-started-with-xamarin-and-mobile-services]

完成したアプリケーションのスクリーンショットは次のようになります。

![][0]

このチュートリアルを完了する必要があります XCode および [Xamarin Studio] OS X または Xamarin Visual Studio プラグインの Windows で Visual Studio の。 サンプルは iOS 5.0 以降で実行します。
> [AZURE.IMPORTANT] このチュートリアルを完了するには、Azure アカウントが必要です。 アカウントがない場合は、Azure 試用版にサインアップして最大 10 の無料モバイル サービスを取得し、試用期間が終わった後でも使用し続けることができます。 詳細については、「 [Azure 無料試用版](http://azure.microsoft.com/pricing/free-trial/)します。

## <a name="create-new-service"> </a>新しいモバイル サービスの作成

[AZURE.INCLUDE [mobile-services-create-new-service](../../includes/mobile-services-create-new-service.md)]

## 新しい Xamarin.iOS アプリケーションを作成する

モバイル サービスを作成したら、Azure クラシック ポータルの簡単なクイック スタートに従って、新しいアプリケーションを作成するか、既存のアプリを変更して、モバイル サービスに接続することができます。

ここでは、モバイル サービスに接続された新しい Xamarin.iOS アプリを作成します。

1.  クリックして、[Azure クラシック ポータル] で、 **Mobile Services**, 、し、先ほど作成したモバイル サービスをクリックします。

2. [クイック スタート] タブの **[プラットフォームの選択]** で **[Xamarin.iOS]** を選択し、**[新しい Xamarin.iOS アプリを作成する]** を展開します。

    ![][6]

    これにより、モバイル サービスに接続された Xamarin.iOS アプリケーションを作成するための簡単な 3 つの手順が表示されます。

    ![][7]

3. まだ行っていない、ダウンロードして Xcode (推奨は、最新バージョンの Xcode 6.0 以降です) をインストールし、[Xamarin Studio] です。

4. **[TodoItems テーブルを作成する]** をクリックして、アプリケーション データを格納するテーブルを作成します。

5. **[アプリをダウンロードして実行する]** の下の **[ダウンロード]** をクリックします。

    これにより、Mobile Services に接続されている _To do list_ サンプル アプリケーションのプロジェクトがダウンロードされ、Xamarin.iOS 向け Azure Mobile Services コンポーネントを参照します。 圧縮されたプロジェクト ファイルをローカル コンピューターに保存し、保存場所を書き留めておいてください。

## 新しい Xamarin.iOS アプリケーションを実行する

このチュートリアルの最後に、新しいアプリケーションをビルドして実行します。

1. 圧縮されたプロジェクト ファイルの保存場所を参照し、ファイルをコンピューター上に展開して、Xamarin Studio または Visual Studio で **XamarinTodoQuickStart.iOS.sln** ソリューション ファイルを開きます。

    ![][8]

    ![][9]

2. **[実行]** をクリックしてプロジェクトをビルドし、このプロジェクトの既定である iPhone エミュレーターでアプリケーションを開始します。

3. アプリケーションで、たとえば、tutorial_ の意味のあるテキストを入力し、正符号 (**+**) のアイコン。

    ![][10]

    これで、Azure でホストされている新しいモバイル サービスに POST 要求が送信されます。 要求のデータは TodoItem テーブルに挿入されます。 テーブルに格納された項目がモバイル サービスによって返され、データが一覧に表示されます。
    > [AZURE.NOTE] モバイル サービスにアクセスして TodoService.cs C# ファイルにあるデータを照会および挿入するコードを確認できます。

4. ポータルに戻り、[Azure クラシック] をクリックして、 **データ** ] タブでをクリックし、 **TodoItems** テーブルです。

    ![][11]

    これで、アプリケーションによってテーブルに挿入されたデータを参照できます。

    ![][12]


## 次のステップ

クイック スタートはこれで完了です。Mobile Services で重要になるこれ以外の作業については、以下のトピックを参照してください。

* [オフライン データの同期の開始を取得]
  クイック スタートでオフライン データの同期を使用して、アプリケーションの応答性と信頼性を高める方法について説明します。

* [認証の使用の開始を取得]
  ID プロバイダーを使用してアプリケーションのユーザーを認証する方法について説明します。

* [開始プッシュ通知の使用]
  アプリケーションにごく基本的なプッシュ通知を送信する方法について説明します。

* [Azure Mobile Services 向け Xamarin コンポーネント クライアントを使用する方法](partner-xamarin-mobile-services-how-to-use-client-library.md)
   モバイル サービスのクエリ、データの処理、およびカスタム Api にアクセスする方法について説明します。


[AZURE.INCLUDE [app-service-disqus-feedback-slug](../../includes/app-service-disqus-feedback-slug.md)]





[getting started with mobile services]: #getting-started 
[create a new mobile service]: #create-new-service 
[define the mobile service instance]: #define-mobile-service-instance 
[next steps]: #next-steps 
[0]: ./media/partner-xamarin-mobile-services-ios-get-started/mobile-quickstart-completed-ios.png 
[6]: ./media/partner-xamarin-mobile-services-ios-get-started/mobile-portal-quickstart-xamarin-ios.png 
[7]: ./media/partner-xamarin-mobile-services-ios-get-started/mobile-quickstart-steps-xamarin-ios.png 
[8]: ./media/partner-xamarin-mobile-services-ios-get-started/mobile-xamarin-project-ios-xs.png 
[9]: ./media/partner-xamarin-mobile-services-ios-get-started/mobile-xamarin-project-ios-vs.png 
[10]: ./media/partner-xamarin-mobile-services-ios-get-started/mobile-quickstart-startup-ios.png 
[11]: ./media/partner-xamarin-mobile-services-ios-get-started/mobile-data-tab.png 
[12]: ./media/partner-xamarin-mobile-services-ios-get-started/mobile-data-browse.png 
[get started with offline data sync]: mobile-services-xamarin-ios-get-started-offline-data.md 
[get started with authentication]: partner-xamarin-mobile-services-ios-get-started-users.md 
[get started with push notifications]: partner-xamarin-mobile-services-ios-get-started-push.md 
[xamarin studio]: http://xamarin.com/download 
[mobile services ios sdk]: https://go.microsoft.com/fwLink/p/?LinkID=266533 
[azure classic portal]: https://manage.windowsazure.com/ 

