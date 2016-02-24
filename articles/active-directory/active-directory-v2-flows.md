<properties
    pageTitle="アプリ モデル v2.0 のアプリの種類 | Microsoft Azure"
    description="Azure AD アプリ モデル v2.0 パブリック プレビューでサポートされているアプリの種類とシナリオ。"
    services="active-directory"
    documentationCenter=""
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/09/2015"
    ms.author="dastrock"/>

# アプリ モデル v2.0 のプレビュー: アプリの種類
V2.0 アプリ モデルは、さまざまな最新のアプリのアーキテクチャ、業界標準プロトコルに基づいているすべての認証をサポートしている [OAuth 2.0](active-directory-v2-protocols.md#oauth2-authorization-code-flow) や [OpenID Connect](active-directory-v2-protocols.md#openid-connect-sign-in-flow)します。  このドキュメントでは、利用する言語やプラットフォームを問わず、作成できるアプリの種類について簡単に説明します。  参考にする前に、高レベルのシナリオを理解して [コードに直接ジャンプ](active-directory-appmodel-v2-overview.md#getting-started)します。

> [AZURE.NOTE]
    この情報は、v2.0 アプリ モデルのパブリック プレビューに関するものです。  一般的に使用できる Azure AD と統合する方法については、サービスを参照してください、 [Azure Active Directory 開発者ガイド 』](active-directory-developers-guide.md)します。

## 基本操作
バージョン 2.0 のアプリケーション モデルを使用するすべてのアプリに登録する必要があります [apps.dev.microsoft.com](https://apps.dev.microsoft.com)します。  アプリの登録プロセスでは、いくつかの値が収集され、対象のアプリに割り当てられます。

-  **アプリケーション Id** アプリを一意に識別します。
- A **リダイレクト URI** アプリケーションに応答を返信に使用できます。
- その他シナリオに応じた値。  詳細については、学習方法 [アプリの登録](active-directory-v2-app-registration.md)します。

登録済みのアプリは、v2.0 エンドポイントに要求を送ることによって、Azure AD と通信を行います。

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize
https://login.microsoftonline.com/common/oauth2/v2.0/token
```

アプリによる v2.0 エンドポイントとのやり取りは、いずれも次のような大まかなパターンに従って行われます。

1. サインインのために、アプリがエンド ユーザーを v2.0 エンドポイントにリダイレクトします。
2. ユーザーがユーザー名とパスワードを入力するなどして、認証を行います。
3. ユーザーがアプリから要求されたアクセス許可を付与し、アプリがユーザーの代理で処理することを承認します。
4. アプリが v2.0 エンドポイントから何らかの種類のセキュリティ トークンを受け取ります。
5. アプリがセキュリティ トークンを使って、保護された情報 (リソース) にアクセスします。
6. リソース サーバーがセキュリティ トークンを検証し、アクセスを許可できることを確かめます。
7. アプリがセキュリティ トークンを定期的に更新します。

<!-- TODO: Need a page for libraries to link to -->
これら 7 つの手順は、作成しているアプリの種類に応じて若干異なりますが、細かい処理を代わりに実行してくれるオープン ソースのライブラリが用意されています。  詳細はについては、 [permissions, consent, and scopes](active-directory-v2-scopes.md), 、または具体的な例をいくつかの調査に読み取り。

## Web Apps
Web アプリ用 (.NET、PHP、Java、Ruby、Python、ノードなど)、ブラウザーを使用してアクセスされる、v2.0 アプリ モデルは、ユーザー サインインを使用して [OpenID Connect](active-directory-v2-protocols.md#openid-connect-sign-in-flow)します。  OpenID Connect では、Web アプリは `id_token` を受け取ります。これは、ユーザーの ID を検証し、要求の形でユーザーに関する情報を提供するセキュリティ トークンです。

```
// Partial raw id_token
eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6ImtyaU1QZG1Cd...

// Partial content of a decoded id_token
{
    "name": "John Smith",
    "email": "john.smith@gmail.com",
    "oid": "d9674823-dffc-4e3f-a6eb-62fe4bd48a58"
    ...
}
```

すべてのトークンとすると、アプリケーションを使用可能なクレームの種類については、 [v2.0 トークン参照](active-directory-v2-tokens.md)します。

Web サーバー アプリにおけるサインイン認証フローは、主に次のステップで構成されます。

![Web アプリのスイムレーン イメージ](../media/active-directory-v2-flows/convergence_scenarios_webapp.png)

v2.0 エンドポイントから受け取った公開署名キーを使用した id_token の検証は、ユーザーの ID を確認し、後続のページ要求でユーザーを識別するために使用できるセッション Cookie を設定するうえで十分です。

アクションでは、このシナリオを表示するには、web アプリにサインインしているコード サンプルでは、いずれかを実行してください、 [概要](active-directory-appmodel-v2-overview.md#getting-started) セクションです。

Web サーバー アプリは、単純なサインインを実行するだけでなく、REST API をはじめとする他の Web サービスにアクセスすることが必要な場合もあります。  この場合は、web サーバー アプリは、結合された OpenID Connect と OAuth 2.0 に参加することができますフローしますを使用して、 [OAuth 2.0 承認コード フロー](active-directory-v2-protocols.md#oauth2-authorization-code-flow)します。 このシナリオは、以下で示されて、 [Web Api のセクションで](#web-apis), 、し、[、 [WebApp WebAPI 作業の開始トピック](active-directory-v2-devquickstarts-webapp-webapi-dotnet.md)します。

## Web API
v2.0 アプリ モデルを使用すると、アプリの RESTful Web API など、Web サービスもセキュリティで保護できます。  Web API では、id_token とセッション Cookie の代わりに OAuth 2.0 access_token を使ってデータをセキュリティ保護し、受信要求を認証します。  Web API の呼び出し元は、HTTP 要求の承認ヘッダーの末尾に access_token を追加します。

```
GET /api/items HTTP/1.1
Host: www.mywebapi.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6...
Accept: application/json
...
```

Web API はその access_token を使って API の呼び出し元の ID を検証し、access_token でエンコードされている要求から呼び出し元に関する情報を抽出します。  すべてのトークンとすると、アプリケーションを使用可能なクレームの種類については、 [v2.0 トークン参照](active-directory-v2-tokens.md)します。

Web API 移すことができるユーザーで/選択-オプトアウトの特定の機能やデータの電源とも呼ばれる、アクセス許可を公開することで [スコープ](active-directory-v2-scopes.md)します。  呼び出し元のアプリがスコープに対するアクセス許可を取得するには、ユーザーがフローの途中でスコープに同意する必要があります。  v2.0 エンドポイントはユーザーにアクセス許可を求め、Web API が受け取るすべての access_token にこれらのアクセス許可を記録します。  Web API で必要なことは、各呼び出しで受け取った access_token を検証することと、適切な承認確認を行うことだけです。

Web API は、すべての種類のアプリ (Web サーバー アプリ、デスクトップ アプリとモバイル アプリ、シングル ページ アプリ、サーバー側のデーモン、さらにそれ以外の Web API など) から access_token を受信できます。  例として、Web API を呼び出す Web サーバー アプリのフロー全体を見てみましょう。

![Web API のスイムレーン イメージ](../media/active-directory-v2-flows/convergence_scenarios_webapp_webapi.png)

Authorization_codes、refresh_tokens、および access_tokens の取得の詳細な手順の詳細については、「、 [OAuth 2.0 プロトコル](active-directory-v2-protocols-oauth-code.md)します。

アプリ モデルのバージョン 2.0、OAuth 2.0 access_tokens と Web API をセキュリティで保護する方法については、チェック アウトの Web API コード サンプル、 [Getting Started] セクション](active-directory-appmodel-v2-overview.md#getting-started)です。


## Mobile Apps とネイティブ アプリ
多くの場合、モバイル アプリやデスクトップ アプリなど、デバイスにインストールされているアプリは、データを格納し、ユーザーの代わりにさまざまな処理を実行するバックエンド サービスや Web API にアクセスする必要があります。  これらのアプリケーションはバージョン 2.0 のモデルを使用して、バックエンド サービスでのサインインと承認を追加でき、 [OAuth 2.0 承認コード フロー](active-directory-v2-protocols-oauth-code.md)します。  

このフローでは、アプリはユーザーのサインインの際に v2.0 エンドポイントから authorization_code を受け取ります。これは、現在サインインしているユーザーの代わりにバックエンド サービスを呼び出すアプリのアクセス許可を表します。  次に、アプリはバックグラウンドで authoriztion_code を OAuth 2.0 access_token および refresh_token と交換します。  アプリはその access_token を使って HTTP 要求で Web API に対して認証を実行します。また、access_token の有効期限が切れた場合は、refresh_token を使って新しい access_token を取得します。

![ネイティブ アプリのスイムレーン イメージ](../media/active-directory-v2-flows/convergence_scenarios_native.png)

## シングル ページ アプリ (JavaScript)
最近多く見かけるようになったシングル ページ アプリのフロントエンドは、主に JavaScript で作成されています。AngularJS、Ember.js、Durandal などの SPA フレームワークが使われることも少なくありません。Azure AD アプリ モデル v2.0 を使用してこれらのアプリをサポートしている、 [OAuth 2.0 の暗黙的なフロー](active-directory-v2-protocols-implicit.md)します。

このフローでは、アプリは v2.0 認証エンドポイントから直接トークンを受け取るので、バックエンドのサーバー間の交換は実行されません。  これにより、すべての認証ロジックとセッション処理は、余分なページのリダイレクトを実行することなく、JavaScript クライアント内ですべて行うことができます。

このシナリオの動作を表示するには、単一ページ アプリケーションのコード サンプルでは、いずれかを実行してください、 [概要](active-directory-appmodel-v2-overview.md#getting-started) セクションです。

## 現在のプレビューの制限事項
これらの種類のアプリは、現時点では v2.0 アプリ モデルのプレビューでサポートされていませんが、一般公開に間に合うように準備中です。  追加の制限事項と v2.0 アプリ モデルのパブリック プレビューの制限がで説明されている、 [v2.0 プレビューの制限の記事](active-directory-v2-limitations.md)します。

### デーモン/サーバー サイド アプリ
長時間実行されるプロセスを含んだアプリや、ユーザーの介入なしで動作するアプリも、セキュリティで保護されたリソース (Web API など) にアクセスする必要があります。  これらのアプリを認証およびなくを使用してアプリケーションの id (ユーザーの委任された id) を使用してトークンを取得、 [フローの OAuth 2.0 クライアント資格情報](active-directory-v2-protocols.md#oauth2-client-credentials-grant-flow)します。  

v2.0 アプリ モデルでは、このフローが現在サポートされていません。つまりアプリがトークンを取得できるのは、対話的なユーザー サインイン フローの後に限られます。  近い将来、クライアント資格情報フローが追加されます。  一般的に使用できる Azure AD では、サービス クライアントの資格情報のフローを表示したい場合は、チェック アウト、 [GitHub 上のデーモン サンプル](https://github.com/AzureADSamples/Daemon-DotNet)します。

### Web API の連鎖 (On-Behalf-Of)
v2.0 アプリ モデルによって保護された Web API から、同じように保護された別の下流の Web API を呼び出す手法は、多くのアーキテクチャで使われています。  このシナリオは、バックエンドの Web API から Microsoft Online サービス (Office 365、Graph API など) を呼び出すネイティブ クライアントでよく見られます。

呼ばれる、OAuth 2.0 Jwt ベアラー資格情報付与を使用してこのチェーンの Web API のシナリオをサポートできる、 [On-Behalf-Of フロー](active-directory-v2-protocols.md#oauth2-on-behalf-of-flow)します。  しかし現時点では v2.0 アプリ モデル プレビューに On-Behalf-Of フローが実装されていません。  一般的に使用できる Azure AD でのこのフローの動作を確認するサービス型で、チェック アウト、 [GitHub の代理としてのコード サンプル](https://github.com/AzureADSamples/WebAPI-OnBehalfOf-DotNet)します。

