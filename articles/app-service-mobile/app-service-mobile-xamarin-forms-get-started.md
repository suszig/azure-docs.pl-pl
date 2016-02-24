<properties
    pageTitle="Xamarin.Forms を使用して Mobile Apps を使用する"
    description="次のチュートリアルに従って、Xamarin.Forms 開発用の Azure Mobile Apps を使用します"
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="wesmc7777"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin"
    ms.devlang="dotnet"
    ms.topic="get-started-article"
    ms.date="11/23/2015"
    ms.author="normesta"/>

#Xamarin.Forms アプリの作成

[AZURE.INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]
&nbsp;  
[AZURE.INCLUDE [app-service-mobile-note-mobile-services](../../includes/app-service-mobile-note-mobile-services.md)]

##概要

このチュートリアルでは、Azure Mobile App バックエンドを使用して Xamarin.Forms モバイル アプリケーションにクラウドベースのバックエンド サービスを追加する方法を示します。  新しいモバイル アプリ バックエンドと簡単なの両方を作成する _Todo リスト_ Xamarin.Forms アプリを Azure にアプリケーション データを保存します。

Xamarin.Forms の他のすべての Azure Mobile Apps のチュートリアルを行う前に、このチュートリアルを完了してください。

##前提条件

このチュートリアルを完了するには、以下が必要です。

* アクティブな Azure アカウント。 アカウントがない場合、Azure 試用版にサインアップして、最大 10 件の無料 Mobile Apps を入手できます。このアプリは評価終了後も使用できます。 詳細については、[Azure の無料試用版サイト](http://azure.microsoft.com/pricing/free-trial/)をご覧ください。
 
* [Visual Studio Community 2013] 以降。  Visual Studio Community 2013 をインストールする場合は、インストール [Xamarin] とは別にします。  Visual Studio 2015 をインストールするときに、Xamarin ツールをインストールできます。

* Mac を [Xcode] v7.0 以降と [Xamarin Studio] をインストールします。 Visual Studio を使用して Windows コンピューターにアプリをビルドする予定の場合でも、ネットワーク接続された Mac にアクセスしてそれを行う必要があります。
 
>[AZURE.NOTE] 場合は、Azure アカウントがサインアップする前に Azure App Service の使用を開始するには、 [App Service の試用](https://tryappservice.azure.com/?appServiceName=mobile), 、App Service で有効期間の短いスターター モバイル アプリをすぐに作成する場所です。 このサービスの利用にあたり、クレジット カードは必要ありません。契約も必要ありません。

## 新しい Azure Mobile App バックエンドの作成

新しいモバイル アプリ バックエンドを作成するには、次の手順に従います。

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]


これで、モバイル クライアント アプリケーションで使用できる Azure モバイル アプリ バックエンドのプロビジョニングが完了しました。 次は、簡単な "todo list" バックエンドのサーバー プロジェクトをダウンロードして、それを Azure に発行します。

## サーバー プロジェクトの構成

以下の手順に従って、Node.js または .NET バックエンドを使用するようにサーバー プロジェクトを構成します。

[AZURE.INCLUDE [app-service-mobile-configure-new-backend](../../includes/app-service-mobile-configure-new-backend.md)] 

 
## (省略可能) バックエンド プロジェクトのローカルなテスト

上で .NET バックエンド構成を選択した場合は、必要に応じて、バックエンドをローカルにテストできます。

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-test-local-service](../../includes/app-service-mobile-dotnet-backend-test-local-service.md)]


##Xamarin.Forms ソリューションのダウンロードと実行

いくつかのオプションがあります。 Mac にソリューションをダウンロードして Xamarin Studio で開くことも、ソリューションを Windows コンピューターにダウンロードして Visual Studio で開くこともできます。 両方の環境を使用して、それらを切り替えることもできます。 次の点を考慮してください。

* Mac でソリューションの iOS プロジェクトを実行する方が簡単です。 必要であれば Visual Studio を Windows コンピューター上で使用できますが、ネットワーク化された Mac に接続する必要があるため、少し複雑になります。 これを行うに知りたい場合は、次を参照してください。 [Installing Xamarin.iOS on Windows]します。
* Android プロジェクトは、Mac または Windows コンピューターのいずれかで実行できます。
* Windows プロジェクトは、Windows コンピューターで Visual Studio を使用することによってのみ実行できます。

これをすべて考慮して、先に進みましょう。

 1. Mac または Windows コンピューターでは、ブラウザーのウィンドウで [Azure ポータル] を開きます。
 2. モバイル アプリの [設定] ブレードをクリックして **開始** ([携帯電話) > **Xamarin.Forms**します。 手順 3 では、をクリックして  **新しいアプリを作成する** が選択されていない場合。  次へ] をクリックして、 **ダウンロード** ] ボタンをクリックします。

    これにより、モバイル アプリに接続されているクライアント アプリケーションが含まれているプロジェクトがダウンロードされます。 圧縮されたプロジェクト ファイルをローカル コンピューターに保存し、保存場所を書き留めておいてください。

 3. ダウンロードしたプロジェクトを抽出し、Xamarin Studio または Visual Studio でそれを開きます。

    ![][9]

    ![][8]

##(省略可能) iOS プロジェクトの実行

このセクションでは、iOS デバイス用の Xamarin iOS プロジェクトを実行します。 iOS デバイスを使用していない場合は、このセクションを省略できます。

####Xamarin Studio で使用する

1. IOS プロジェクトを右クリックし、 **スタートアップ プロジェクトとして設定**します。
2.  **実行** ] メニューのをクリックして **[デバッグ開始]** 、プロジェクトをビルドし、iPhone エミュレーターでアプリケーションを開始します。

####Visual Studio で使用する
1. IOS プロジェクトを右クリックし、 **スタートアップ プロジェクトとして設定**します。
2.  **ビルド** ] メニューのをクリックして **Configuration Manager**します。 
3.  **Configuration Manager** ダイアログ ボックスで、 **ビルド** と **展開** iOS プロジェクトのチェック ボックスをオンします。
4. キーを押して、 **f5 キーを押して** キー、プロジェクトをビルドし、iPhone エミュレーターでアプリケーションを開始します。

アプリケーションになど意味のあるテキストを入力 _学習 Xamarin_ ] をクリックし、 **+** ] ボタンをクリックします。

![][10]

これで、Azure でホストされている新しいモバイル アプリ バックエンドに POST 要求が送信されます。 要求のデータは TodoItem テーブルに挿入されます。 テーブルに格納された項目がモバイル アプリ バックエンドによって返され、データが一覧に表示されます。

> [AZURE.NOTE]
> ソリューションのポータブル クラス ライブラリ プロジェクトの TodoItemManager.cs C# ファイルに、モバイル アプリ バックエンドにアクセスするコードが表示されます。

##(省略可能) Android プロジェクトの実行

このセクションでは、Android 用の Xamarin Android プロジェクトを実行します。 Android デバイスを使用していない場合は、このセクションを省略できます。

####Xamarin Studio で使用する

1. Android プロジェクトを右クリックし、 **スタートアップ プロジェクトとして設定**します。
2.  **実行** ] メニューのをクリックして **[デバッグ開始]** 、プロジェクトをビルドし、Android エミュレーターでアプリケーションを開始します。

####Visual Studio で使用する
1. Android プロジェクトを右クリックし、 **スタートアップ プロジェクトとして設定**します。
4.  **ビルド** ] メニューのをクリックして **Configuration Manager**します。 
5.  **Configuration Manager** ダイアログ ボックスで、 **ビルド** と **展開** Android プロジェクトのチェック ボックスをオンします。
6. キーを押して、 **f5 キーを押して** キー、プロジェクトをビルドし、Android エミュレーターでアプリケーションを開始します。

アプリケーションになど意味のあるテキストを入力 _学習 Xamarin_ ] をクリックし、 **+** ] ボタンをクリックします。

![][11]

これで、Azure でホストされている新しいモバイル アプリ バックエンドに POST 要求が送信されます。 要求のデータは TodoItem テーブルに挿入されます。 テーブルに格納された項目がモバイル アプリ バックエンドによって返され、データが一覧に表示されます。

> [AZURE.NOTE]
> ソリューションのポータブル クラス ライブラリ プロジェクトの TodoItemManager.cs C# ファイルに、モバイル アプリ バックエンドにアクセスするコードが表示されます。


##(省略可能) Windows プロジェクトの実行


このセクションでは、Windows デバイス用の Xamarin WinApp プロジェクトを実行します。 Windows デバイスを使用していない場合は、このセクションを省略できます。


####Visual Studio で使用する
1. Windows プロジェクトを右クリックし、をクリックし、 **スタートアップ プロジェクトとして設定**します。
4.  **ビルド** ] メニューのをクリックして **Configuration Manager**します。 
5.  **Configuration Manager** ダイアログ ボックスで、 **ビルド** と **展開** 選択した Windows プロジェクトのチェック ボックスをオンします。
6. キーを押して、 **f5 キーを押して** キー、プロジェクトをビルドし、Windows のエミュレーターでアプリケーションを開始します。

アプリケーションになど意味のあるテキストを入力 _学習 Xamarin_ ] をクリックし、 **+** ] ボタンをクリックします。
    
これで、Azure でホストされている新しいモバイル アプリ バックエンドに POST 要求が送信されます。 要求のデータは TodoItem テーブルに挿入されます。 テーブルに格納された項目がモバイル アプリ バックエンドによって返され、データが一覧に表示されます。

![][12]
    
> [AZURE.NOTE]
> ソリューションのポータブル クラス ライブラリ プロジェクトの TodoItemManager.cs C# ファイルに、モバイル アプリ バックエンドにアクセスするコードが表示されます。

<!-- Anchors. -->
[Getting started with mobile app backends]:#getting-started
[Create a new mobile app backend]:#create-new-service
[Next Steps]:#next-steps


<!-- Images. -->
[6]: ./media/app-service-mobile-xamarin-forms-get-started/xamarin-forms-quickstart.png
[8]: ./media/app-service-mobile-xamarin-forms-get-started/xamarin-forms-quickstart-vs.png
[9]: ./media/app-service-mobile-xamarin-forms-get-started/xamarin-forms-quickstart-xs.png
[10]: ./media/app-service-mobile-xamarin-forms-get-started/mobile-quickstart-startup-ios.png
[11]: ./media/app-service-mobile-xamarin-forms-get-started/mobile-quickstart-startup-android.png
[12]: ./media/app-service-mobile-xamarin-forms-get-started/mobile-quickstart-startup-windows.png


<!-- URLs. -->
[Visual Studio Professional 2013]: https://go.microsoft.com/fwLink/p/?LinkID=257546
[Mobile app SDK]: http://go.microsoft.com/fwlink/?LinkId=257545
[Azure Portal]: https://portal.azure.com/


[Xamarin Studio]: http://xamarin.com/download
[Xamarin]: http://xamarin.com/download
[Xcode]: https://go.microsoft.com/fwLink/?LinkID=266532&clcid=0x409
[Xamarin for Windows]: https://go.microsoft.com/fwLink/?LinkID=330242&clcid=0x409
[Installing Xamarin.iOS on Windows]: http://developer.xamarin.com/guides/ios/getting_started/installation/windows/
 
