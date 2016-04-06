<properties
    pageTitle="Azure App Service Mobile Apps で iOS アプリを作成する | Microsoft Azure"
    description="このチュートリアルに従って、Objective-C または Swift で iOS を開発用するための Azure モバイル アプリ バックエンドの使用を開始します。"
    services="app-service\mobile"
    documentationCenter="ios"
    authors="krisragh"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="na"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="hero-article"
    ms.date="11/30/2015"
    ms.author="krisragh"/>

#iOS アプリの作成

[AZURE.INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]
&nbsp;  
[AZURE.INCLUDE [app-service-mobile-note-mobile-services](../../includes/app-service-mobile-note-mobile-services.md)]

## 概要

このチュートリアルは、クラウド ベースのバックエンド サービスを使用する方法を示します ([Azure Mobile Apps](app-service-mobile-value-prop.md)) iOS モバイル アプリケーションにします。  新しいモバイル バックエンドが作成され、単純なを使用して _Todo リスト_ Azure にアプリケーション データを格納する iOS アプリケーション。

## 前提条件

このチュートリアルを完了するには、以下が必要です。

*  [アクティブな Azure アカウント](http://azure.microsoft.com/pricing/free-trial/)

* PC と [Visual Studio Community 2013] 以降

* Xcode v7.0 以降がインストールされた Mac

* [Azure のモバイル iOS フレームワーク](https://go.microsoft.com/fwLink/?LinkID=529823), 、ダウンロードしたクイック スタート プロジェクトの一部として自動的に追加されます。

## 新しい Azure モバイル アプリ バックエンドを作成する

新しいモバイル アプリ バックエンドを作成するには、次の手順に従います。

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

これで、モバイル クライアント アプリケーションで使用できる Azure モバイル アプリ バックエンドのプロビジョニングが完了しました。 次は、簡単な "todo list" バックエンドのサーバー プロジェクトをダウンロードして、それを Azure に発行します。

## サーバー プロジェクトのダウンロード

1. PC で、次を参照してください。、 [Azure portal]します。 をクリックして **すべてを参照** > **Mobile Apps**, 、先ほど作成したモバイル アプリ バックエンドを順にクリックします。

2. モバイル アプリ] ブレードで [ **設定**, 、し [ **モバイル アプリ**, をクリックして **クイック スタート** > **iOS (Objective C)**します。 Swift 場合は、クリックして **クイック スタート** > **iOS (Swift)** 代わりにします。

## サーバー プロジェクトの構成

[AZURE.INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]


## iOS アプリのダウンロードと実行

[AZURE.INCLUDE [app-service-mobile-ios-run-app](../../includes/app-service-mobile-ios-run-app.md)]


<!-- Images. -->

<!-- URLs -->
[Azure portal]: https://portal.azure.com/
[Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[Visual Studio Community 2013]: https://go.microsoft.com/fwLink/p/?LinkID=534203


