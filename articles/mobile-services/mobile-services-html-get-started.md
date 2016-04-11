<properties
    pageTitle="HTML/JavaScript アプリ向け Azure Mobile Services の使用 | Microsoft Azure"
    description="次のチュートリアルに従って、HTML 開発用の Azure Mobile Services を使用します。"
    services="mobile-services"
    documentationCenter=""
    authors="ggailey777"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="mobile-services"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-html5"
    ms.devlang="javascript"
    ms.topic="get-started-article" 
    ms.date="11/30/2015"
    ms.author="glenga"/>


# <a name="getting-started"> </a>Mobile Services の使用

[AZURE.INCLUDE [mobile-services-selector-get-started](../../includes/mobile-services-selector-get-started.md)]
&nbsp;

[AZURE.INCLUDE [mobile-services-hero-slug](../../includes/mobile-services-hero-slug.md)]

##概要 

このチュートリアルでは、Azure Mobile Services を使用して HTML アプリケーションにクラウドベースのバックエンド サービスを追加する方法について説明します。 このチュートリアルでは、新しいモバイル サービスと、新しいモバイル サービスにアプリ データを保存する簡単な *To do list* アプリの両方を作成します。 このチュートリアルの次のビデオ バージョンもご覧ください。 

> [AZURE.VIDEO mobile-get-started-html]
 
完成したアプリケーションのスクリーンショットは次のようになります。

![][0]

このチュートリアルを完了することは、HTML アプリの他のすべての Mobile Services チュートリアルの前提条件です。 Phonegap/cordova アプリでは、次を参照してください。、、 [Phonegap/cordova バージョン](mobile-services-javascript-backend-phonegap-get-started.md) このチュートリアルのです。

##前提条件

このチュートリアルを完了するには、以下が必要です。

+ ローカル コンピューターで次のいずれかの Web サーバーが実行されている必要があります。

    +  **Windows で**: IIS Express。 IIS Express がインストールされている、 [Microsoft Web Platform Installer]します。
    +  **MacOS x**: Python。 既にインストールされています。
    +  **Linux で**: Python。 インストールする必要があります、 [latest version of Python]します。

    このアプリケーションは任意の Web サーバーを使用してホストすることができますが、これらの Web サーバーは、ダウンロードしたスクリプトでサポートされています。  

+ HTML5 をサポートする Web ブラウザー。
+ Azure アカウント。 アカウントがない場合は、無料の試用アカウントを数分で作成することができます。 詳細については、[Azure の無料試用版サイト](http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started-html%2F"%20target="_blank)をご覧ください。 


## <a name="create-new-service"> </a>新しいモバイル サービスを作成する

[AZURE.INCLUDE [mobile-services-create-new-service](../../includes/mobile-services-create-new-service.md)]

## 新しい HTML アプリを作成する

モバイル サービスを作成したら、Azure クラシック ポータルの簡単なクイック スタートに従って、新しいアプリケーションを作成するか、既存のアプリを変更して、モバイル サービスに接続することができます。

ここでは、モバイル サービスに接続された新しい HTML アプリを作成します。

1.   [Azure classic portal], 、] をクリックして **Mobile Services**, 、作成したモバイル サービスをクリックします。


2. クイック スタート] タブをクリックして **Windows** [ **プラットフォームの選択]** 展開 **新しい HTML アプリを作成**します。

    ![][6]

    これにより、モバイル サービスに接続された HTML アプリを作成し、ホストするための簡単な 3 つの手順が表示されます。

    ![][7]

3. クリックして **作成 TodoItems テーブル** アプリケーション データを格納するテーブルを作成します。

4. [ **をダウンロードしてアプリケーションを実行する**, 、] をクリックして **ダウンロード**します。

    これにより、サンプルの web サイトのファイルがダウンロード _作業の一覧に_ 、モバイル サービスに接続されているアプリケーションです。 圧縮されたファイルをローカル コンピューターに保存し、保存場所を書き留めておいてください。

5.  **構成** いることを確認] タブで、 `localhost` で既に表示されている、 **ホスト名からの要求を許可する** の下に一覧表示 **クロス オリジン リソース共有 (CORS)**します。 そうでない場合は、入力 `localhost` で、 **ホスト名** フィールドをクリックして **保存**します。

    ![][9]

    > [AZURE.IMPORTANT] Localhost 以外の web サーバーにクイック スタート アプリケーションを配置する場合は、web サーバーのホスト名を追加する必要があります、 **ホスト名からの要求を許可する** ] ボックスの一覧です。 詳細については、次を参照してください。 [クロス オリジン リソース共有](http://msdn.microsoft.com/library/windowsazure/dn155871.aspx)します。

## HTML アプリのホストと実行

このチュートリアルの最後に、ローカル コンピューターの新しいアプリケーションをホストして実行します。

1. 圧縮されたプロジェクト ファイルを保存した場所を参照し、ファイル、コンピューター上に展開しのいずれかから次のコマンド ファイル、 **server** サブフォルダーです。

    + **windows の起動時** (Windows コンピューター)
    + **launch-mac.command** (Mac OS X コンピューター)
    + **launch-linux.sh** (Linux コンピューター)

    > [AZURE.NOTE] Windows コンピューターで次のように入力します。 `R` PowerShell が要求すると、スクリプトを実行することを確認します。 Web ブラウザーでは、インターネットからダウンロードしたスクリプトであるため、実行しないよう警告されることがあります。 その場合は、ブラウザーがスクリプトの読み込みを開始するよう要求する必要があります。

    これにより、新しいアプリケーションをホストする Web サーバーがローカル コンピューター上で起動します。

2. Web ブラウザーで URL <a href="http://localhost:8000/" target="_blank">http://localhost:8000/</a> を開いて、アプリケーションを開始します。

3. アプリケーションで、意味のあるテキストをなど入力 _に関するチュートリアルを完了_, で、 **Enter 新しいタスク**, 、] をクリックし、 **追加**します。

    ![][10]

    これで、Azure でホストされている新しいモバイル サービスに POST 要求が送信されます。 要求のデータは TodoItem テーブルに挿入されます。 テーブルに格納された項目はモバイル サービスによって返され、データはアプリケーションの 2 番目の列に表示されます。

    > [AZURE.NOTE] モバイル サービスを照会および挿入 page.js ファイル内にあるデータにアクセスするコードを確認できます。

4. 戻り、 [Azure classic portal], をクリックして、 **データ** ] タブでをクリックし、 **TodoItems** テーブルです。

    ![][11]

    これで、アプリケーションによってテーブルに挿入されたデータを参照できます。

    ![][12]

## <a name="next-steps"> </a>次のステップ
クイック スタートはこれで完了です。Mobile Services で重要になるこれ以外の作業については、以下のトピックを参照してください。

* **[Add authentication to your app]**  
  ID プロバイダーを使用してアプリケーションのユーザーを認証する方法について説明します。

* **[Mobile Services HTML/JavaScript How-to Conceptual Reference]**  
  HTML/JavaScript で Mobile Services を使用する方法について説明します。


[AZURE.INCLUDE [app-service-disqus-feedback-slug](../../includes/app-service-disqus-feedback-slug.md)]

<!-- Anchors. -->
[Getting started with Mobile Services]:#getting-started
[Create a new mobile service]:#create-new-service
[Define the mobile service instance]:#define-mobile-service-instance
[Next Steps]:#next-steps

<!-- Images. -->
[0]: ./media/mobile-services-html-get-started/mobile-quickstart-completed-html.png

[6]: ./media/mobile-services-html-get-started/mobile-portal-quickstart-html.png
[7]: ./media/mobile-services-html-get-started/mobile-quickstart-steps-html.png

[9]: ./media/mobile-services-html-get-started/mobile-services-set-cors-localhost.png
[10]: ./media/mobile-services-html-get-started/mobile-quickstart-startup-html.png
[11]: ./media/mobile-services-html-get-started/mobile-data-tab.png
[12]: ./media/mobile-services-html-get-started/mobile-data-browse.png


<!-- URLs. -->
[Add authentication to your app]: mobile-services-html-get-started-users.md

[Azure classic portal]: https://manage.windowsazure.com/
[Microsoft Web Platform Installer]:  http://go.microsoft.com/fwlink/p/?LinkId=286333
[latest version of Python]: http://go.microsoft.com/fwlink/p/?LinkId=286342
[Mobile Services HTML/JavaScript How-to Conceptual Reference]: mobile-services-html-how-to-use-client-library.md
[Cross-origin resource sharing]: http://msdn.microsoft.com/library/azure/dn155871.aspx
 


