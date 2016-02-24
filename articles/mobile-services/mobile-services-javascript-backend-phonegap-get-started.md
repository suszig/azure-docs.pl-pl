<properties
    pageTitle="PhoneGap/Cordova アプリ向け Azure Mobile Services の使用 | Microsoft Azure"
    description="このチュートリアルでは、iOS、Android、Windows Phone 用に PhoneGap の開発を行う場合に Azure Mobile Services を使用する方法について説明します。"
    services="mobile-services"
    documentationCenter=""
    authors="ggailey777"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="mobile-services"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-phonegap"
    ms.devlang="multiple"
    ms.topic="get-started-article" 
    ms.date="11/06/2015"
    ms.author="ggailey777"/>

# Mobile Services の使用

[AZURE.INCLUDE [mobile-services-selector-get-started](../../includes/mobile-services-selector-get-started.md)]
&nbsp;

[AZURE.INCLUDE [mobile-services-hero-slug](../../includes/mobile-services-hero-slug.md)]

このチュートリアルでは、Azure Mobile Services を使用してアプリケーションにクラウドベースのバックエンド サービスを追加する方法を示します。 このチュートリアルでは、新しいモバイル サービスと、新しいモバイル サービスにアプリ データを保存する簡単な _To do list_ アプリの両方を作成します。

完成したアプリケーションのスクリーンショットは次のようになります。

![][3]

### その他の要件

このチュートリアルを完了するには、次のものが必要です。

+ PhoneGap ツール (Windows Phone 8 プロジェクトの場合は v3.2 以降が必要です)

+ アクティブな Microsoft Azure アカウント。

+ PhoneGap は、複数のプラットフォーム向けの開発をサポートします。 PhoneGap ツール自体に加えて、対象としている各プラットフォーム用のツールをインストールする必要があります。

    - Windows Phone: インストール [Visual Studio 2012 Express for Windows Phone](https://go.microsoft.com/fwLink/p/?LinkID=268374)
    - iOS: インストール [Xcode] (v4.4 以上が必要)
    - Android: [Android Developer Tools] のインストール [Android SDK]
        <br/>(Android 向けのMobile Services SDK は Android 2.2 以降用のアプリケーションをサポートします。 Android 4.2 以降がクイック スタート アプリケーションの実行に必要です。)

## 新しいモバイル サービスを作成する

[AZURE.INCLUDE [mobile-services-create-new-service](../../includes/mobile-services-create-new-service.md)]

## 新しい PhoneGap アプリを作成する

ここでは、モバイル サービスに接続された新しい PhoneGap アプリケーションを作成します。

1.  クリックして、[Azure クラシック ポータル] で、 **Mobile Services**, 、し、先ほど作成したモバイル サービスをクリックします。

2. クイック スタート] タブをクリックして **PhoneGap** [ **プラットフォームの選択]** 展開 **新しい PhoneGap アプリケーションを作成**します。

    ![][0]

    これにより、モバイル サービスに接続された PhoneGap アプリケーションを作成するための簡単な 3 つの手順が表示されます。

    ![][1]

3. まだインストールしていない場合は、PhoneGap および、プラットフォーム開発ツールの少なくとも 1 つ (Windows Phone、iOS、または Android) をダウンロードしてインストールします。

4. クリックして **作成 TodoItem テーブル** アプリケーション データを格納するテーブルを作成します。

5. [ **をダウンロードしてアプリケーションを実行する**, 、] をクリックして **ダウンロード**します。

    これにより、サンプルのプロジェクトがダウンロード _作業の一覧に_ Mobile Services JavaScript SDK と共に、モバイル サービスに接続されているアプリケーションです。 圧縮されたプロジェクト ファイルをローカル コンピューターに保存し、保存場所を書き留めておいてください。

## 新しい PhoneGap アプリケーションを実行する

このチュートリアルの最後に、新しいアプリケーションをビルドして実行します。

1.  圧縮されたプロジェクト ファイルの保存場所を参照し、ファイルをコンピューター上に展開します。

2.  各プラットフォームについてここに示す手順に従って、プロジェクトを開いて実行します。

    + **Windows Phone 8**

        1. Windows Phone 8: .sln ファイルを開き、 **\wp8** Visual Studio 2012 Express for Windows Phone でのフォルダーです。

        2. キーを押して、 **f5 キーを押して** キーをプロジェクトをリビルドし、アプリケーションを開始します。

        ![][2]

    + **iOS**

        1. プロジェクトを開き、 **プラットフォーム/ios** Xcode 内のフォルダーです。

        2. キーを押して、 **実行** 、プロジェクトをビルドし、既定でこのプロジェクトである iPhone エミュレーターでアプリケーションを開始] ボタンをクリックします。

        ![][3]

    + **Android**

        1. Eclipse で [ **ファイル** し **インポート**, 、展開 **Android**, 、] をクリックして **既存の Android Code into workspace]**, 、] をクリックし、 **次です。**

        2. をクリックして **参照**, 展開したプロジェクト ファイルの場所を [参照] をクリックして、 **OK**, を TodoActivity プロジェクトがオンになっているかどうかを確認し、をクリックして **完了**します。 <p>これにより、プロジェクト ファイルが現在のワークスペースにインポートされます。</p>

        3.  **実行** ] メニューのをクリックして **実行** を Android エミュレーターでプロジェクトを開始します。

            ![][4]

        >[AZURE.NOTE]プロジェクトを Android エミュレーターで実行できるようにするを少なくとも 1 つ Android Virtual Device (AVD) を定義する必要があります。 これらのデバイスを作成および管理するには、AVD Manager を使用します。


3. 示したモバイル エミュレーターのいずれかでアプリケーションを起動すた後には、テキスト ボックスにテキストを入力し、 **追加**します。

    これで、Azure でホストされている新しいモバイル サービスに POST 要求が送信されます。 要求のデータが挿入される、 **TodoItem** テーブルです。 テーブルに格納された項目がモバイル サービスによって返され、データが一覧に表示されます。

    > [AZURE.IMPORTANT] メイン プロジェクトが PhoneGap ツールで再ビルドされる場合、このプラットフォーム プロジェクトへの変更が上書きされます。 以下のセクションで説明されているように、代わりの方法として、変更はプロジェクトのルートの www ディレクトリで行います。

4. ポータルに戻り、[Azure クラシック] をクリックして、 **データ** ] タブでをクリックし、 **TodoItem** テーブルです。

    ![](./media/mobile-services-javascript-backend-phonegap-get-started/mobile-data-tab.png)

    これで、アプリケーションによってテーブルに挿入されたデータを参照できます。

    ![](./media/mobile-services-javascript-backend-phonegap-get-started/mobile-data-browse.png)


## アプリケーションを更新して、各プラットフォーム用にプロジェクトを再ビルドする

1. ´www´ ディレクトリ (この場合は ´todolist/www´) のコード ファイルを変更します。

2. すべてのターゲット プラットフォームのツールがシステム パスでアクセスできることを確認します。

2. ルート プロジェクト ディレクトリでコマンド プロンプトを開き、次のプラットフォームに固有のコマンドのいずれかを実行します。

    + **Windows Phone**

        Visual Studio 開発者用コマンド プロンプトで次のコマンドを実行します。

            phonegap local build wp8

    + **iOS**

        ターミナルを開き、次のコマンドを実行します。

            phonegap local build ios

    + **Android**

        コマンド プロンプトまたはターミナル ウィンドウを開き、次のコマンドを実行します。

            phonegap local build android

4. 前のセクションで説明したように、適切な開発環境で各プロジェクトを開きます。

>[AZURE.NOTE]モバイル サービスを照会および挿入して js/index.js ファイルにあるデータにアクセスするコードを確認できます。

## 次のステップ
クイック スタートはこれで完了です。Mobile Services で重要になるこれ以外の作業については、以下のトピックを参照してください。

* **[アプリに認証の追加]**  
  ID プロバイダーを使用してアプリケーションのユーザーを認証する方法について説明します。  

* **[アプリへのプッシュ通知を追加します。](https://msdn.microsoft.com/magazine/dn879353.aspx)**  
  登録を行い、アプリにプッシュ通知を送信する方法について説明します。

* **[モバイル サービス Html/javascript の使用方法の概念リファレンス](mobile-services-html-how-to-use-client-library.md)**  
  カスタム Api を呼び出して、認証を実行し、データにアクセスする JavaScript クライアント ライブラリを使用する方法について説明します。

[AZURE.INCLUDE [app-service-disqus-feedback-slug](../../includes/app-service-disqus-feedback-slug.md)]

<!-- Images. -->
[0]: ./media/mobile-services-javascript-backend-phonegap-get-started/portal-screenshot1.png
[1]: ./media/mobile-services-javascript-backend-phonegap-get-started/portal-screenshot2.png
[2]: ./media/mobile-services-javascript-backend-phonegap-get-started/mobile-portal-quickstart-wp8.png
[3]: ./media/mobile-services-javascript-backend-phonegap-get-started/mobile-portal-quickstart-ios.png
[4]: ./media/mobile-services-javascript-backend-phonegap-get-started/mobile-portal-quickstart-android.png

<!-- URLs. -->
[Add authentication to your app]: mobile-services-html-get-started-users.md
[Android SDK]: https://go.microsoft.com/fwLink/p/?LinkID=280125
[Azure classic portal]: https://manage.windowsazure.com/
[Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[Visual Studio 2012 Express for Windows Phone]: https://go.microsoft.com/fwLink/p/?LinkID=268374
 
