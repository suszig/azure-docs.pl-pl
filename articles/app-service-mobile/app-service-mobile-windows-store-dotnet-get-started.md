<properties
    pageTitle="Azure App Service Mobile Apps で Windows Runtime 8.1 アプリを作成する | Microsoft Azure"
    description="このチュートリアルに従って、Azure モバイル アプリ バックエンドを使用して、C#、Visual Basic、または JavaScript で Windows ストア用の開発を開始します。"
    services="app-service\mobile"
    documentationCenter="windows"
    authors="ggailey777"
    manager="dwrede"
    editor=""/> 

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="11/16/2015"
    ms.author="glenga"/>


# Create a Windows app (Windows アプリの作成)

[AZURE.INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]
&nbsp;
[AZURE.INCLUDE [app-service-mobile-note-mobile-services](../../includes/app-service-mobile-note-mobile-services.md)]

## 概要

このチュートリアルでは、ユニバーサル Windows アプリにクラウドベースのバックエンド サービスを追加する方法を示します。 詳細については、次を参照してください。 [モバイル アプリとは](app-service-mobile-value-prop.md)します。

[AZURE.INCLUDE [app-service-mobile-windows-universal-get-started](../../includes/app-service-mobile-windows-universal-get-started.md)]

## 前提条件

このチュートリアルを完了するには、以下が必要です。

* アクティブな Azure アカウント アカウントがない場合、Azure 試用版にサインアップして、最大 10 件の無料モバイル アプリを入手できます。このアプリは評価終了後も使用できます。 詳細については、「 [Azure 無料試用版](http://azure.microsoft.com/pricing/free-trial/)します。

* [Visual Studio Community 2013] 以降のバージョン。

>[AZURE.NOTE] 場合は、Azure アカウントにサインアップする前に Azure App Service の使用を開始するには、 [App Service の試用](https://tryappservice.azure.com/?appServiceName=mobile)します。 有効期間が短いスターター モバイル アプリを App Service ですぐに作成できます。このサービスの利用にあたり、クレジット カードは必要ありません。契約も必要ありません。

## 新しい Azure Mobile App バックエンドの作成

新しいモバイル アプリ バックエンドを作成するには、次の手順に従います。

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

これで、モバイル クライアント アプリケーションで使用できる Azure モバイル アプリ バックエンドのプロビジョニングが完了しました。 次は、簡単な "todo list" バックエンドのサーバー プロジェクトをダウンロードして、それを Azure に発行します。

## サーバー プロジェクトの構成

[AZURE.INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## クライアント プロジェクトのダウンロードおよび実行

モバイル アプリ バックエンドを構成した後は、新しいクライアント アプリを作成したり、既存のアプリを Azure に接続するように変更したりできます。 このセクションでは、モバイル アプリ バックエンドに接続するようにカスタマイズされたユニバーサル Windows アプリ テンプレート プロジェクトをダウンロードします。

1. モバイル アプリ バックエンドの **[開始]** ブレードに戻り、**[新しいアプリの作成]**、**[ダウンロード]** の順にクリックして、圧縮されたプロジェクト ファイルをローカル コンピューターに抽出します。

3. (省略可能) サーバー プロジェクトとともに、ユニバーサル Windows アプリ プロジェクトをソリューションに追加します。 これにより、必要であれば、同じ Visual Studio ソリューションでアプリとバックエンドの両方をより簡単にデバッグおよびテストできるようになります。

4. Windows ストア アプリをスタートアップ プロジェクトとして使用し、F5 キーを押してプロジェクトをリビルドし、Windows ストア アプリを起動します。

5. アプリで、**[Insert a TodoItem]** テキスト ボックスに意味のあるテキスト (たとえば、「*チュートリアルの完了*」) を入力し、**[Save]** をクリックします。

    ![](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-quickstart-startup.png)

    これで、Azure でホストされている新しいモバイル アプリ バックエンドに POST 要求が送信されます。

6. デバッグを停止、を右クリックして、 `< アプリ名 >。WindowsPhone` プロジェクトをクリックして **スタートアップ プロジェクトとして設定**, 、f5 キーを押します。

    ![](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-quickstart-completed-wp8.png)

    Windows アプリの開始後に、前の手順で保存したデータがモバイル アプリから読み込まれることに注目してください。

## 次のステップ

* [アプリへの認証を追加します。 ](app-service-mobile-windows-store-dotnet-get-started-users.md)
  <br/>Id プロバイダーを使用してアプリケーションのユーザーを認証する方法について説明します。

* [アプリへのプッシュ通知を追加します。](app-service-mobile-windows-store-dotnet-get-started-push.md)
  <br/>アプリにごく基本的なプッシュ通知を送信する方法について説明します。





[mobile app sdk]: http://go.microsoft.com/fwlink/?LinkId=257545 
[azure portal]: https://portal.azure.com/ 
[visual studio community 2013]: https://go.microsoft.com/fwLink/p/?LinkID=534203 

