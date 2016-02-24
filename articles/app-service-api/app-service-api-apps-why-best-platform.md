<properties 
    pageTitle="API Apps の概要" 
    description="Azure App Service が RESTful API を開発、発行、ホストするために最適なプラットフォームである理由について説明します。" 
    services="app-service\api" 
    documentationCenter=".net" 
    authors="tdykstra" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-api" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="11/30/2015" 
    ms.author="tdykstra"/>

# API Apps の概要

API Apps はによって提供される、4 つの種類のアプリのいずれかの [Azure App Service](../app-service/app-service-value-prop-what-is.md)します。

![](./media/app-service-api-apps-why-best-platform/appservicesuite.png)

[App Service](../app-service/app-service-value-prop-what-is.md) 完全に管理されたプラットフォームによって web、モバイル、および統合シナリオで豊富な機能です。 App Service の API Apps は、クラウドとオンプレミスでの API のビルド、ホスト、および使用を容易にする機能を提供します。 App Service で API アプリとしてデプロイします。 API と、エンタープライズ レベルのセキュリティ、簡単なアクセス制御、ハイブリッド接続、SDK の自動生成、およびシームレスな統合の恩恵を [Logic Apps](../app-service-logic/app-service-logic-what-are-logic-apps.md)します。

## API Apps を使う理由

API Apps には、次の機能が用意されています。

- **簡単な利用** -のサポートが組み込まれて [API の Swagger メタデータ](#concepts) 独自の Api は、さまざまなクライアントによって簡単に利用します。  C#、Java、Javascript などのさまざまな言語で API のクライアント コードを自動的に生成します。 簡単に構成 [CORS](#concepts) なしのコードを変更します。 詳細については、次を参照してください。 [API の検出およびコードの生成時に App Service API Apps メタデータ](app-service-api-metadata.md) と [CORS を使用して JavaScript から API アプリを使用する](app-service-api-cors-consume-javascript.md)です。 

- **簡単なアクセス制御** -コードを変更せずに認証されていないアクセスから API アプリを保護します。 組み込みの認証サービスでは、他のサービスからのアクセスまたはユーザーを表すクライアントからのアクセスから API をセキュリティで保護します。 サポートされている ID プロバイダーには、Azure Active Directory やサード パーティ プロバイダー ( Facebook、Twitter など) が含まれます。 クライアントでは、Active Directory 認証ライブラリ (ADAL) または Mobile Apps SDK を使用できます。 詳細については、次を参照してください。 [App Service の展開の認証/承認](/blog/announcing-app-service-authentication-authorization/) と [変更内容の App Service API Apps](app-service-api-whats-changed.md)します。

- **Visual Studio の統合** -Visual Studio の専用ツールの作成、展開、使用、デバッグ、および API アプリを管理する作業を効率化します。 詳細については、次を参照してください。 [.NET 用 Azure SDK 2.8.1 を発表](/blog/announcing-azure-sdk-2-8-1-for-net/)します。

- **Logic Apps との統合** -作成した API アプリで使用できる [App Service Logic Apps](../app-service-logic/app-service-logic-what-are-logic-apps.md)します。  方法について説明 [ロジック アプリを使用して App Service でホストされている、カスタム API を使用して](../app-service-logic/app-service-logic-custom-hosted-api.md)します。 API Apps をロジック アプリと統合する方法の継続的な変更点については、次を参照してください。 [変更内容の App Service API Apps](app-service-api-whats-changed.md)します。   

- **として既存の api では** -既存の Api を API アプリの機能の活用 - API アプリにコードをデプロイするだけのコードを変更する必要はありません。 API では、App Service でサポートされている任意の言語またはフレームワークを使用できます。たとえば、ASP.NET、C#、Java、PHP、Node.js、Python などを使用できます。

さらに、API Apps、Web Apps、および Mobile Apps の機能は互換性があります。 つまり、API Apps のインスタンスで、Web 開発とモバイル開発用の機能と Web Apps と Mobile Apps が提供するホスティングを活用できます。 この逆もあてはまります。たとえば、Web アプリを使用して API をホストでき、同時に Swagger メタデータを活用してクライアント コードを生成し、CORS を活用してブラウザーのクロス ドメイン アクセスを実行できます。 詳細については、次を参照してください。、 [Web Apps の概要](../app-service-web/app-service-web-overview.md) と [Mobile Apps の概要](../app-service-mobile/app-service-mobile-value-prop.md)します。

>[AZURE.NOTE] 使用する [Azure API Management](../api-management/api-management-key-concepts.md) App Service API Apps でホストされている Api をクライアントへのアクセスを制御します。 API Apps では認証サービスを提供します。その他に、API Apps ではなく API Management が提供するアクセス管理機能もあります。たとえば、エンドポイント統合やスロットルなどです。

## API Apps の概念 ##

- **Swagger** -ドキュメントと検出 API アプリで既定で使用される rest ベースの API のためのフレームワークです。 詳細については、次を参照してください。 [http://swagger.io/](http://swagger.io/)します。
- **クロス オリジン リソース共有 (CORS)** -から読み込まれた web ページよりも、別のドメインでホストされている API を呼び出すには、ブラウザーで実行される JavaScript をできる機構です。 詳細については、次を参照してください。 [CORS を使用して JavaScript から API アプリを使用する](app-service-api-cors-consume-javascript.md)です。 
- **トリガー** -REST API を [ロジック アプリ](../app-service-logic/app-service-logic-what-are-logic-apps.md) 、特定の条件が満たされたときに、ワークフロー プロセスを開始するを呼び出すことができます。 たとえば、API アプリは、Twitter フィード内の特定の語句を検索するためにロジック アプリから定期的に呼び出すメソッドを提供できます。 詳細については、次を参照してください。 [API アプリ トリガー](app-service-api-dotnet-triggers.md)します。
- **アクション** -REST API を [ロジック アプリ](../app-service-logic/app-service-logic-what-are-logic-apps.md) データを処理するワークフローがトリガーによって開始された後に呼び出すことができます。 たとえば、API アプリは、Twitter のトリガーによって検出されたツイートに応答するためにロジック アプリから呼び出されるメソッドを提供できます。 アクションは、Swagger API の定義によって公開される API のメソッドです。

## 使用の開始

を API アプリを開始するには、次の [API アプリを使ってみる](app-service-api-dotnet-get-started.md) チュートリアルです。

API アプリを使用して既知の問題の一覧を表示するには、次を参照してください。 [API Apps の既知の問題にフォーラムの投稿](https://social.msdn.microsoft.com/Forums/en-US/7f8b42f2-ac0d-48b8-a35e-3b4934e1c25e/api-app-known-issues?forum=AzureAPIApps)します。

Azure App Service プラットフォームの詳細については、次を参照してください。 [Azure App Service](../app-service/app-service-value-prop-what-is.md)します。

