<properties
    pageTitle="Windows ストア JavaScript アプリケーション用 Mobile Services の使用 | Azure Mobile Services"
    description="このチュートリアルでは、JavaScript で Windows ストア用の開発を行う場合に Azure Mobile Services を使用する方法を示します。"
    services="mobile-services"
    documentationCenter="windows"
    authors="ggailey777"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="mobile-services"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows-store"
    ms.devlang="javascript"
    ms.topic="get-started-article"
    ms.date="11/06/2015"
    ms.author="glenga"/>

# Mobile Services の使用

[AZURE.INCLUDE [mobile-services-selector-get-started](../../includes/mobile-services-selector-get-started.md)]
&nbsp;

[AZURE.INCLUDE [mobile-services-hero-slug](../../includes/mobile-services-hero-slug.md)]

このチュートリアルでは、Azure Mobile Services を使用して Windows ストア JavaScript アプリにクラウドベースのバックエンド サービスを追加する方法を示します。 このチュートリアルでは、新しいモバイル サービスと、新しいモバイル サービスにアプリ データを保存する簡単な *To do list* アプリの両方を作成します。 作成するモバイル サービスは、サーバー側ビジネス ロジックのために JavaScript を使用します。 

このチュートリアルを完了するには、以下が必要です。

* アクティブな Azure アカウント アカウントがない場合は、無料試用版のアカウントを数分で作成することができます。 詳細については、[Azure の無料試用版サイト](http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fdocumentation%2Farticles%2Fmobile-services-javascript-backend-windows-store-javascript-get-started%2F)をご覧ください。
* [Visual Studio 2013 Express for Windows]

## 新しいモバイル サービスを作成する

[AZURE.INCLUDE [mobile-services-create-new-service](../../includes/mobile-services-create-new-service.md)]

## 新しい Windows ストア アプリを作成する

モバイル サービスを作成したら、Azure クラシック ポータルの簡単なクイック スタートに従って、モバイル サービスに接続する新しい Windows ストア 8.1 JavaScript アプリを作成できます。

1.  クリックして、[Azure クラシック ポータル] で、 **Mobile Services**, 、し、先ほど作成したモバイル サービスをクリックします。


2. クイック スタート] タブをクリックして **Windows** [ **プラットフォームの選択]** 展開 **新しい Windows ストア アプリを作成**します。

3. まだ行っていない、ダウンロードしてローカル コンピューターまたは仮想マシンで [Visual Studio 2013] [Visual Studio 2013 Express for Windows] をインストールします。

4. クリックして **作成 TodoItem テーブル** アプリケーション データを格納するテーブルを作成します。

5.  **をダウンロードして、アプリを実行**, をアプリの言語を選択してクリックして **ダウンロード**します。

    これにより、サンプルのプロジェクトがダウンロード *作業の一覧に* 、モバイル サービスに接続されているアプリケーションです。 圧縮されたプロジェクト ファイルをローカル コンピューターに保存し、保存場所を書き留めておいてください。

## Windows アプリケーションを実行する

このチュートリアルの最後に、新しいアプリケーションをビルドして実行します。

1. 圧縮されたプロジェクト ファイルの保存場所を参照し、ファイルをコンピューター上に展開して、Visual Studio でソリューション ファイルを開きます。

2. キーを押して、 **f5 キーを押して** キーをプロジェクトをリビルドし、アプリケーションを開始します。

3. アプリケーションで、意味のあるテキストをなど入力 *に関するチュートリアルを完了*, で、 **Insert a TodoItem**, 、] をクリックし、 **保存**します。

    これで、Azure でホストされている新しいモバイル サービスに POST 要求が送信されます。 要求のデータは TodoItem テーブルに挿入されます。 テーブルに格納された項目はモバイル サービスによって返され、データはアプリケーションの 2 番目の列に表示されます。

4. (オプション) アプリを再実行し、アプリケーションの起動後に前の手順で保存したデータがモバイル サービスから読み込まれる点に注目してください。
 
4. ポータルに戻り、[Azure クラシック] をクリックして、 **データ** ] タブでをクリックし、 **TodoItems** テーブルです。

    これで、アプリケーションによってテーブルに挿入されたデータを参照できます。

>[AZURE.NOTE] モバイル サービスを照会および挿入 default.js ファイルであるデータにアクセスするコードを確認できます。

## 次のステップ
これで、クイック スタートが完了したらを使用する方法について、 [Html/javascript 用 Mobile Services クライアント](mobile-services-html-how-to-use-client-library.md)です。 

[AZURE.INCLUDE [app-service-disqus-feedback-slug](../../includes/app-service-disqus-feedback-slug.md)]

<!-- Anchors. -->
[Getting started with Mobile Services]:#getting-started
[Create a new mobile service]:#create-new-service
[Define the mobile service instance]:#define-mobile-service-instance
[Next Steps]:#next-steps

<!-- Images. -->

<!-- URLs. -->
[Visual Studio 2013 Express for Windows]: http://go.microsoft.com/fwlink/?LinkId=257546
[Mobile Services SDK]: http://go.microsoft.com/fwlink/?LinkId=257545
[Azure classic portal]: https://manage.windowsazure.com/

