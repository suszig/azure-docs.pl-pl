<properties
   pageTitle="Azure Active Directory 開発者ガイド | Microsoft Azure"
   description="この記事は、Azure Active Directory の開発者向けリソースの包括的なガイドです。"
   services="active-directory"
   documentationCenter="dev-center-name"
   authors="msmbaldwin"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="12/10/2015"
   ms.author="mbaldwin"/>



# Azure Active Directory 開発者ガイド

## 概要

Identity Management As A Service (IDMaaS) プラットフォームとして Azure Active Directory を使用することにより、開発者は ID 管理をアプリケーションに効果的に統合することができます。 次の記事では、Azure Active Directory の実装と重要な機能の概要について説明しています。 順番にお読みにジャンプしたりをお勧め [概要](#getting-started) 詳細に分析ができる場合です。


1. [Azure Active Directory 統合のメリット](active-directory-how-to-integrate.md): Azure Active Directory との統合がセキュリティで保護されたサインインと承認に最適なソリューションをな理由を説明します。

1. [Active Directory の認証シナリオ](active-directory-authentication-scenarios.md): アプリケーションへのサインオンを提供する Azure Active Directory の簡略化された認証のメリットを利用します。

1. [アプリケーションを Azure Active Directory と統合する](active-directory-integrating-applications.md): 追加、更新、および Azure Active Directory、および統合されたアプリケーションのブランド化ガイドラインについては、アプリケーションを削除する方法について説明します。

1. [Azure Active Directory Graph API](active-directory-graph-api.md): プログラムで使用する REST API エンドポイントを介して Azure Active Directory にアクセスする Azure Active Directory Graph API を使用します。 Azure AD Graph API は、を通じてアクセスも [Graph](https://graph.microsoft.io/), 、統合する複数の Microsoft クラウド サービスの Api と 1 つのアクセス トークンを使用して、1 つの REST API エンドポイントを介してアクセスできるようにする API。

1. [Azure Active Directory 認証ライブラリ](active-directory-authentication-libraries.md): 簡単に Azure 認証ライブラリを使用してアクセス トークンを取得するユーザーを認証します。


## 使用の開始

次のチュートリアルは複数のプラットフォームに対応しているため、Azure Active Directory を使用した開発をすぐに開始できます。 前提条件として行う必要があります [Azure Active Directory テナントを取得する](active-directory-howto-tenant.md)します。

### モバイル アプリケーションと PC アプリケーションのクイック スタート ガイド

| [![iOS](./media/active-directory-developers-guide/ios.png)] (active directory devquickstarts ios.md)| [![Android](./media/active-directory-developers-guide/android.png)] (active directory devquickstarts android.md)| [![.NET](./media/active-directory-developers-guide/net.png)] (active directory devquickstarts dotnet.md)| [![Windows Phone](./media/active-directory-developers-guide/windows.png)] (active directory devquickstarts windowsphone.md)| [![Windows ストア](./media/active-directory-developers-guide/windows.png)] (active directory devquickstarts windowsstore.md)| [![Xamarin](./media/active-directory-developers-guide/xamarin.png)] (active directory devquickstarts xamarin.md)| [![Cordova](./media/active-directory-developers-guide/cordova.png)] (active directory devquickstarts cordova.md)
|:--:|:--:|:--:|:--:|:--:|:--:|:--:
| [iOS](active-directory-devquickstarts-ios.md)| [Android](active-directory-devquickstarts-android.md)| [.NET](active-directory-devquickstarts-dotnet.md)| [Windows Phone](active-directory-devquickstarts-windowsphone.md)| [Windows ストア](active-directory-devquickstarts-windowsstore.md)| [Xamarin](active-directory-devquickstarts-xamarin.md)| [Cordova](active-directory-devquickstarts-cordova.md)

### Web アプリケーション クイック スタート ガイド

| [![.NET](./media/active-directory-developers-guide/net.png)] (アクティブ-ディレクトリ-devquickstarts-web アプリケーション-dotnet.md)| [![Java](./media/active-directory-developers-guide/java.png)] (アクティブ-ディレクトリ-devquickstarts-web アプリケーション-java.md)| [![Javascript](./media/active-directory-developers-guide/javascript.png)] (active directory devquickstarts angular.md)| [![Node.js](./media/active-directory-developers-guide/nodejs.png)] (アクティブ-ディレクトリ-devquickstarts-openidconnect-nodejs.md)
|:--:|:--:|:--:|:--:|
| [.NET](active-directory-devquickstarts-webapp-dotnet.md)| [Java](active-directory-devquickstarts-webapp-java.md)| [Javascript](active-directory-devquickstarts-angular.md)| [Node.js](active-directory-devquickstarts-openidconnect-nodejs.md)

### Web API クイック スタート ガイド

| [![.NET](./media/active-directory-developers-guide/net.png)] (アクティブ-ディレクトリ-devquickstarts-webapi-dotnet.md)| [![Node.js](./media/active-directory-developers-guide/nodejs.png)] (アクティブ-ディレクトリ-devquickstarts-webapi-nodejs.md)
|:--:|:--:|
| [.NET](active-directory-devquickstarts-webapi-dotnet.md)| [Node.js](active-directory-devquickstarts-webapi-nodejs.md)

### ディレクトリのクエリに関するクイック スタート ガイド

| [![.NET](./media/active-directory-developers-guide/graph.png)] (アクティブ-ディレクトリのグラフ-api-quickstart.md)|
|:--:|
| [Graph API](active-directory-graph-api-quickstart.md)|

## 方法

次の記事では、Azure Active Directory を使用して特定のタスクを実行する方法について説明しています。

- [Azure Active Directory テナントを取得します。](active-directory-howto-tenant.md)
- [Azure Active Directory アプリケーション ギャラリーでのアプリケーションを一覧表示します。](active-directory-app-gallery-listing.md)
- [Azure Active Directory のアプリケーション マニフェストを理解します。](active-directory-application-manifest.md)
- [Office 365 の api アプリを作成します。](https://msdn.microsoft.com/office/office365/howto/getting-started-Office-365-APIs)
- [販売者ダッシュ ボードに Office 365 用の web アプリを提出します。](https://msdn.microsoft.com/office/office365/howto/submit-web-apps-seller-dashboard)
- [両方個人と会社または学校のアカウントでユーザーがサインインするアプリを構築する方法をプレビューします。](active-directory-appmodel-v2-overview.md)
- [サインアップすると、コンシューマーにサインインするアプリを構築する方法をプレビュー。](active-directory-b2c-overview.md)


## リファレンス

以下の記事は、REST と認証ライブラリ API、プロトコル、エラー、コード サンプル、エンドポイントの基礎的なリファレンスです。

### サポート

- [質問をタグ付け](http://stackoverflow.com/questions/tagged/azure-active-directory): タグを検索してスタック オーバーフローについての Azure Active Directory の検索ソリューション [azure active ディレクトリ](http://stackoverflow.com/questions/tagged/azure-active-directory) と [adal](http://stackoverflow.com/questions/tagged/adal)します。

### コード

- [Azure Active Directory のオープン ソース ライブラリ](http://github.com/AzureAD): を使用して、ライブラリのソースを検索する最も簡単な方法は、当社 [ライブラリ リスト](active-directory-authentication-libraries.md)します。

- [Azure Active Directory のサンプル](http://github.com/AzureADSamples): サンプルの一覧を移動する最も簡単な方法を使用して、 [コード サンプルのインデックス](active-directory-code-samples.md)します。


### Graph API

- [Graph API リファレンス](https://msdn.microsoft.com/library/azure/hh974476.aspx): Azure Active Directory Graph API の REST リファレンスです。 [Graph API の対話の参照を表示](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog)します。

- [Graph API のアクセス許可スコープ](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/graph-api-permission-scopes): OAuth 2.0 アクセス許可スコープのアプリケーションに、テナント内のディレクトリ データに対するアクセスを制御するために使用します。


### 認証プロトコル

- [SAML 2.0 プロトコル リファレンス](https://msdn.microsoft.com/library/azure/dn195591.aspx): SAML 2.0 プロトコルにより、アプリケーションで、ユーザーにシングル サインオン エクスペリエンスを提供します。

- [OAuth 2.0 プロトコル リファレンス](https://msdn.microsoft.com/library/azure/dn645545.aspx): OAuth 2.0 プロトコルを web アプリケーションへのアクセスを承認して、Azure Active Directory テナントで web Api を使用することができます。

- [OpenID Connect 1.0 プロトコル リファレンス](https://msdn.microsoft.com/library/azure/dn645541.aspx): OpenID Connect 1.0 プロトコルは OAuth 2.0、拡張認証プロトコルとして使用するためです。

- [Ws-federation 1.2 プロトコル リファレンス](https://msdn.microsoft.com/library/azure/dn903702.aspx): Web Services Federation バージョン 1.2 仕様で、Ws-federation 1.2 プロトコルを指定します。

- [サポートされているトークンとクレームの種類](active-directory-token-and-claims.md): このガイドを理解し、評価する SAML 2.0 トークンおよび JSON Web トークン (JWT) トークン内のクレームを使用することができます。

## ビデオ

### Build 2015

Azure Active Directory を使用したアプリケーションの開発に関する次の概要プレゼンテーションには、エンジニアリング チームで実務に携わっている話し手が登場します。 プレゼンテーションでは、IDMaaS、認証、ID フェデレーション、シングル サインオンなどの基本的なトピックを取り上げています。

- [最新のアプリケーションのサービスとして azure Active Directory: Id 管理](http://azure.microsoft.com/documentation/videos/build-2015-azure-active-directory-identity-management-as-a-service-for-modern-applications)
- [Azure Active Directory での最新の web アプリケーションを開発します。](http://azure.microsoft.com/documentation/videos/build-2015-develop-modern-web-applications-with-azure-active-directory)
- [Azure Active Directory での最新のネイティブ アプリケーションを開発します。](http://azure.microsoft.com/documentation/videos/build-2015-develop-modern-native-applications-with-azure-active-directory)

### Azure Friday

[Azure Friday](http://azure.microsoft.com/documentation/videos/azure-friday/) は金曜日 1:1 ビデオ一連の定期的な専用の (10 ~ 15 分) の短いインタビューさまざまな Azure のトピックに関する専門家で構成されています。 ページにあるサービス フィルター機能を使用すると、すべての Azure Active Directory ビデオを視聴することができます。

- [Azure Id 101](http://azure.microsoft.com/documentation/videos/azure-identity-basics/)
- [Azure Id 102](http://azure.microsoft.com/documentation/videos/azure-identity-creating-active-directory/)
- [Azure Id 103](http://azure.microsoft.com/documentation/videos/azure-identity-application-to-authenticate/)

## ソーシャル

- [Active Directory チームのブログ](http://blogs.technet.com/b/ad/): Azure Active Directory の世界では最新の話題です。

- [Azure Active Directory Graph チームのブログ](http://blogs.msdn.com/b/aadgraphteam): Azure Active Directory については、Graph API に固有です。

- [クラウド Id](http://www.cloudidentity.net): からプリンシパル Azure Active Directory PM、サービスとしての id 管理に関する考察します。

- [Twitter で azure Active Directory](https://twitter.com/azuread): 140 文字以下の Azure Active Directory のお知らせします。





