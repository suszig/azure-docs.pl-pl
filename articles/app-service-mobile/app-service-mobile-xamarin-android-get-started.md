<properties
    pageTitle="Xamarin Android アプリに Azure Mobile Apps を使用する"
    description="次のチュートリアルに従って、Xamarin Android 開発用の Azure Mobile Apps を使用します"
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="ggailey777"
    manager="dwrede"
    editor="" />

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-android"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="11/17/2015"
    ms.author="glenga" />

#Xamarin.Android アプリの作成

[AZURE.INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]
&nbsp;  
[AZURE.INCLUDE [app-service-mobile-note-mobile-services](../../includes/app-service-mobile-note-mobile-services.md)]
 
##概要

このチュートリアルでは、Xamarin Android アプリケーションにクラウドベースのバックエンド サービスを追加する方法を示します。 詳細については、次を参照してください。 [モバイル アプリとは](app-service-mobile-value-prop.md)です。 

完成したアプリケーションのスクリーンショットは次のようになります。

![][0]

Xamarin Android アプリケーションの他のすべての Mobile Apps のチュートリアルを行う前に、このチュートリアルを完了してください。
 
##前提条件

このチュートリアルを完了するには、以下が必要です。

* アクティブな Azure アカウント。 アカウントがない場合、Azure 試用版にサインアップして、最大 10 件の無料 Mobile Apps を入手できます。このアプリは評価終了後も使用できます。 詳細については、[Azure の無料試用版サイト](http://azure.microsoft.com/pricing/free-trial/)をご覧ください。
 
* [Visual Studio Community 2013] 以降。  Visual Studio Community 2013 をインストールする場合は、[Xamarin] を個別にインストールします。  Visual Studio 2015 をインストールするときに、Xamarin ツールをインストールできます。
 
>[AZURE.NOTE] 場合は、Azure アカウントがサインアップする前に Azure App Service の使用を開始するには、 [App Service の試用](https://tryappservice.azure.com/?appServiceName=mobile), 、App Service で有効期間の短いスターター モバイル アプリをすぐに作成する場所です。 このサービスの利用にあたり、クレジット カードは必要ありません。契約も必要ありません。

## 新しい Azure Mobile App バックエンドの作成

新しいモバイル アプリ バックエンドを作成するには、次の手順に従います。

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

これで、モバイル クライアント アプリケーションで使用できる Azure モバイル アプリ バックエンドのプロビジョニングが完了しました。 次は、簡単な "todo list" バックエンドのサーバー プロジェクトをダウンロードして、それを Azure に発行します。

## サーバー プロジェクトの構成

[AZURE.INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## Xamarin.Android アプリのダウンロードと実行

1. [ **Xamarin.Android プロジェクトの実行をダウンロードして**, 、] をクリックして、 **ダウンロード** ] ボタンをクリックします。

    これにより、モバイル アプリに接続されているクライアント アプリケーションが含まれているプロジェクトがダウンロードされます。 圧縮されたプロジェクト ファイルをローカル コンピューターに保存し、保存場所を書き留めておいてください。

2. キーを押して、 **f5 キーを押して** キー、プロジェクトをビルドし、アプリケーションを開始します。 

3. アプリケーションで、意味のあるテキストをなど入力 _に関するチュートリアルを完了_ ] をクリックし、 **追加** ] ボタンをクリックします。

    ![][10]

    これで、Azure でホストされている新しいモバイル アプリ バックエンドに POST 要求が送信されます。 要求のデータは TodoItem テーブルに挿入されます。 テーブルに格納された項目がモバイル アプリ バックエンドによって返され、データが一覧に表示されます。

    > [AZURE.NOTE] ToDoActivity.cs c# ファイルにあるデータ照会および挿入するようにモバイル アプリ バックエンドにアクセスするコードを確認できます。

##次のステップ

* [アプリへの認証を追加します。 ](app-service-mobile-xamarin-android-get-started-users.md)
  <br/>Id プロバイダーを使用してアプリケーションのユーザーを認証する方法について説明します。


<!-- Images. -->
[0]: ./media/app-service-mobile-xamarin-android-get-started/mobile-quickstart-completed-android.png
[6]: ./media/app-service-mobile-xamarin-android-get-started/mobile-portal-quickstart-xamarin.png
[8]: ./media/app-service-mobile-xamarin-android-get-started/mobile-xamarin-project-android-vs.png
[9]: ./media/app-service-mobile-xamarin-android-get-started/mobile-xamarin-project-android-xs.png
[10]: ./media/app-service-mobile-xamarin-android-get-started/mobile-quickstart-startup-android.png

<!-- URLs. -->
[Azure Portal]: https://azure.portal.com/
[Xamarin]: http://xamarin.com/download
[Xcode]: https://go.microsoft.com/fwLink/?LinkID=266532&clcid=0x409
[Xamarin for Windows]: https://go.microsoft.com/fwLink/?LinkID=330242&clcid=0x409
 
[Visual Studio Community 2013]: https://go.microsoft.com/fwLink/p/?LinkID=534203

