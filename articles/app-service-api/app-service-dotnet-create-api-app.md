<properties
    pageTitle="Azure App Service での ASP.NET API アプリの作成 | Microsoft Azure"
    description="Visual Studio 2013 を使用して、Azure App Service で ASP.NET API アプリを作成する方法について説明します。"
    services="app-service\api"
    documentationCenter=".net"
    authors="bradygaster"
    manager="wpickett"
    editor="jimbe"/>

<tags
    ms.service="app-service-api"
    ms.workload="na"
    ms.tgt_pltfrm="dotnet"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="10/08/2015"
    ms.author="tdykstra"/>

# Azure App Service での ASP.NET API アプリの作成

[AZURE.INCLUDE [app-service-api-v2-note](../../includes/app-service-api-v2-note.md)]

## 概要

このチュートリアルでとしてクラウドに配置するために構成されている ASP.NET Web API プロジェクトを作成する方法、 [Azure App Service で API アプリ](app-service-api-apps-why-best-platform.md)します。 API アプリとして展開用の既存の Web API プロジェクトを構成する方法については、次を参照してください。 [API アプリとして Web API プロジェクトを構成する](app-service-dotnet-create-api-app-visual-studio.md)です。

これは、テンプレートを使用して Visual Studio プロジェクトを作成する方法のみを示す手軽で簡単なチュートリアルです。  表示すると、系列の最初であるハウツー [展開](app-service-dotnet-deploy-api-app.md) と [デバッグ](../app-service-dotnet-remotely-debug-api-app.md) このチュートリアルで作成した API アプリのプロジェクトです。 API アプリを操作する方法の詳しい内容について詳細を参照してください、 [次のステップ](#next-steps) チュートリアルの末尾にある「します。

[AZURE.INCLUDE [install-sdk-2015-2013](../../includes/install-sdk-2015-2013.md)]

このチュートリアルでは、Azure SDK for .NET 2.6 以降のバージョンが必要です。

## API アプリ プロジェクトの作成

手順については、直接的なプロジェクトの名前を入力する、場合に、入力 **ContactsList**します。

[AZURE.INCLUDE [app-service-api-create](../../includes/app-service-api-create.md)]

[AZURE.INCLUDE [app-service-api-review-metadata](../../includes/app-service-api-review-metadata.md)]

[AZURE.INCLUDE [app-service-api-define-api-app](../../includes/app-service-api-define-api-app.md)]

[AZURE.INCLUDE [app-service-api-direct-deploy-metadata](../../includes/app-service-api-direct-deploy-metadata.md)]

## 次のステップ

API アプリを展開する準備ができました、行うことができる、 [API アプリのデプロイ](app-service-dotnet-deploy-api-app.md) を行うにはチュートリアルです。

API アプリを呼び出すクライアントを自動的に生成されたコードを使用する方法については、次を参照してください。 [.NET クライアントから API アプリを使用する](app-service-api-dotnet-consume.md)です。

API アプリに対して自動的に生成される Swagger メタデータをカスタマイズする方法については、次を参照してください。 [をカスタマイズする Swashbuckle で生成された API 定義](app-service-api-dotnet-swashbuckle-customize.md)します。

作成、削除、および Azure プレビュー ポータルで API アプリの構成方法については、次を参照してください。 [API アプリの管理](app-service-api-manage-in-portal.md)します。

API アプリのユーザーの認証方法の詳細については、次を参照してください。 [API apps と Azure App Service でのモバイル アプリ認証](../app-service/app-service-authentication-overview.md)します。

API Apps の機能については、次を参照してください。 [API apps とは?](app-service-api-apps-why-best-platform.md)します。

