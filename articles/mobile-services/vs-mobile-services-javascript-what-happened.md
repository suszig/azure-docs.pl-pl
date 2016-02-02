<properties 
    pageTitle="Visual Studio の接続済みサービスを使用して Mobile Services を JavaScript アプリに追加したときの影響" 
    description="Visual Studio の Azure Mobile Services プロジェクトの変更点について説明します。" 
    services="mobile-services" 
    documentationCenter="" 
    authors="TomArcher" 
    manager="douge" 
    editor=""/>

<tags 
    ms.service="mobile-services" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="NA" 
    ms.devlang="JavaScript" 
    ms.topic="article" 
    ms.date="09/23/2015" 
    ms.author="tarcher"/>


# 接続済みの Visual Studio サービスを使用して Azure Mobile Services を追加したときの JavaScript プロジェクトへの影響

> [AZURE.SELECTOR]
> - [作業の開始](vs-mobile-services-javascript-getting-started.md)
> - [変更内容](vs-mobile-services-javascript-what-happened.md)

### プロジェクトの変更点

##### 追加された NuGet パッケージ

**WindowsAzure.MobileServices.WinJS** で Azure Mobile Service ライブラリを含む、NuGet パッケージがインストールされた、 `js\MobileServices.js` ファイルです。

##### Mobile Services 用の接続文字列の値

`Services \mobileservices\settings` フォルダーに新しい JavaScript (.js) ファイル、 **MobileServiceClient** が生成された、選択したモバイル サービスのアプリケーション URL とアプリケーション キーを含みます。


##### Default.html に追加された参照

参照 `MobileServices.js` に追加された設定ファイルと `default.html`します。


##### 追加された接続されたサービス ファイル

サービス フォルダー内に、接続されたサービスの構成ファイルが追加されました。









