<properties
    pageTitle="Azure AD B2C プレビュー | Microsoft Azure"
    description="Azure AD の B2C プレビューで構築できるアプリケーションの種類"
    services="active-directory-b2c"
    documentationCenter=""
    authors="dstrockis"
    manager="msmbaldwin"
    editor=""/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/28/2015"
    ms.author="dastrock"/>

# Azure AD B2C プレビュー: アプリケーションの種類

Azure AD B2C は、さまざまな最新のアプリのアーキテクチャ、業界標準プロトコルに基づいているすべての認証をサポートしている [OAuth 2.0](active-directory-b2c-reference-protocols.md) や [OpenID Connect](active-directory-b2c-reference-protocols.md)します。  このドキュメントでは、利用する言語やプラットフォームを問わず、作成できるアプリの種類について簡単に説明します。  参考にする前に、高レベルのシナリオを理解して [コードに直接ジャンプ](active-directory-b2c-overview.md#getting-started)します。

[AZURE.INCLUDE [active-directory-b2c-preview-note](../../includes/active-directory-b2c-preview-note.md)]

## 基本操作
Azure AD B2C を使用するすべてのアプリに登録する必要があります、 [B2C ディレクトリ](active-directory-b2c-get-started.md) を使用して、 [Azure プレビュー ポータル](https://portal.azure.com)します。  アプリの登録プロセスでは、いくつかの値が収集され、対象のアプリに割り当てられます。

-  **アプリケーション Id** アプリを一意に識別します。
- A **リダイレクト URI** アプリケーションに応答を返信に使用できます。
- その他シナリオに応じた値。  詳細については、学習方法 [アプリの登録](active-directory-b2c-app-registration.md)します。

登録済みのアプリは、Azure AD v2.0 エンドポイントに要求を送ることによって、Azure AD と通信します。

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize
https://login.microsoftonline.com/common/oauth2/v2.0/token
```

Azure AD B2C に送信される各要求の指定、 **ポリシー**します。  ポリシーは Azure AD の動作を制御し、これらのエンドポイントを使用して、高度にカスタマイズ可能なユーザー エクスペリエンスのセットを作成できます。  一般的なポリシーとして、サインアップ ポリシー、サインイン ポリシー、プロファイル編集ポリシーがあります。  慣れていない場合は、次のポリシー、Azure AD B2C 知っておくべき [拡張可能なポリシー フレームワーク](active-directory-b2c-reference-policies.md) に関するページを読む前にします。 

アプリによる v2.0 エンドポイントとのやり取りは、いずれも次のような大まかなパターンに従って行われます。

1. アプリケーションを実行する v2.0 エンドポイントにエンドユーザーに指示する [ポリシー](active-directory-b2c-reference-policies.md)します。
2. ユーザーがポリシーの定義に従ってポリシーを完了します。
4. アプリが v2.0 エンドポイントから何らかの種類のセキュリティ トークンを受け取ります。
5. アプリがセキュリティ トークンを使って、保護された情報 (リソース) にアクセスします。
6. リソース サーバーがセキュリティ トークンを検証し、アクセスを許可できることを確かめます。
7. アプリがセキュリティ トークンを定期的に更新します。

<!-- TODO: Need a page for libraries to link to -->
これらの手順は、作成するアプリの種類に応じて若干異なりますが、細かい処理を代わりに実行してくれるオープン ソース ライブラリが用意されています。

## Web Apps
Web アプリケーション (.NET、PHP、Java、Ruby、Python、ノードなど) をサーバーでホストされ、ブラウザーを通じてアクセスされる Azure AD B2C がサポートしている [OpenID Connect](active-directory-b2c-reference-protocols.md) のすべてのユーザー エクスペリエンスを含め、サインイン、サインアップおよび管理をプロファイリングします。  Azure AD B2C の OpenID Connect の実装では、Web アプリは、これらのユーザーエクスペリエンスを Azure AD に認証要求を発行することで開始します。  この要求の結果は `id_token` であり、これはユーザーの ID を表し、クレームの形でユーザーに関する情報を提供するセキュリティ トークンです。

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

すべてのトークンとすると、アプリケーションを使用可能なクレームの種類については、 [B2C トークン参照](active-directory-b2c-reference-tokens.md)します。

Web アプリを実行するたびに、 [ポリシー](active-directory-b2c-reference-policies.md) これら高レベルの手順を実行します。

![Web アプリのスイムレーン イメージ](./media/active-directory-b2c-apps/webapp.png)

Azure AD エンドポイントから受け取った公開署名キーを使用した id_token の検証は、ユーザーの ID を確認し、以降のページ要求でユーザーを識別するために使用できるセッション Cookie を設定するうえで十分です。

アクションでは、このシナリオを表示するには、web アプリにサインインしているコード サンプルでは、いずれかを実行してください、 [概要](active-directory-b2c-overview.md#getting-started) セクションです。

Web サーバー アプリは、単純なサインインを実行するだけでなく、他のバックエンド Web サービスにアクセスすることが必要な場合もあります。  この場合、web アプリをわずかに異なる実行 [フローの OpenID Connect](active-directory-b2c-reference-oidc.md), 、認証コードを使用してトークンを取得し、トークンを更新します。 このシナリオは以下のとおり、 [Web Api のセクション](#web-apis)です。

<!--, and in our [WebApp-WebAPI Getting Started topic](active-directory-b2c-devquickstarts-web-api-dotnet.md).-->

## Web API
Azure AD B2C を使用して、アプリの RESTful Web API などの Web サービスもセキュリティで保護できます。  Web API では、OAuth 2.0 を使用してデータをセキュリティで保護し、トークンを使用して着信する HTTP 要求を認証できます。  Web API の呼び出し元は、HTTP 要求の認証ヘッダーの中にトークンを追加します。

```
GET /api/items HTTP/1.1
Host: www.mywebapi.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6...
Accept: application/json
...
```

Web API はトークンを使用して API の呼び出し元の ID を検証し、トークン内にエンコードされているクレームから呼び出し元に関する情報を抽出します。  すべてのトークンとすると、アプリケーションを使用可能なクレームの種類については、 [トークンの参照を Azure AD B2C](active-directory-b2c-reference-tokens.md)します。

> [AZURE.NOTE]
    Azure AD B2C プレビューは、現時点では、Web API の既知のクライアントによってアクセスされる Web API のみサポートします。  たとえば、完成したアプリには、iOS アプリ、Android アプリ、Web API を含めることができます。  このアーキテクチャは完全にサポートされます。  現時点でサポートされていないのは、別の iOS アプリなどのサード パーティ クライアントが同じ Web API にアクセスできるようにすることです。  実際には、完成したアプリの各コンポーネントは、1 つのアプリケーション ID を共有する必要があります。

Web API は、Web アプリ、デスクトップ アプリ、モバイル アプリ、シングル ページ アプリ、サーバー サイド デーモン、それ以外の Web API など、あらゆる種類のクライアントからトークンを受信できます。  例として、Web API を呼び出す Web アプリのフロー全体を見てみましょう。

![Web API のスイムレーン イメージ](./media/active-directory-b2c-apps/webapi.png)

Authorization_codes、refresh_tokens、およびトークンを取得するための詳しい手順の詳細については、「、 [OAuth 2.0 プロトコル](active-directory-b2c-reference-oauth-code.md)します。

Azure AD B2C に Web API をセキュリティで保護する方法については、チェック アウトされている Web API のチュートリアル、 [Getting Started] セクション](active-directory-b2c-overview.md#getting-started)です。
    
## Mobile Apps とネイティブ アプリ
モバイル アプリやデスクトップ アプリなどのデバイスにインストールされているアプリは、多くの場合、ユーザーの代わりにバックエンド サービスや Web API にアクセスする必要があります。  管理エクスペリエンスのカスタマイズされた識別情報をネイティブ アプリに追加し、Azure AD B2C を使用してバックエンド サービスを安全に呼び出すと、 [OAuth 2.0 承認コード フロー](active-directory-b2c-reference-oauth-code.md)します。  

このフローで、アプリが実行される [ポリシー](active-directory-b2c-reference-policies.md) し、ユーザーが、ポリシーが完了した後、Azure AD から、authorization_code を受信します。  authorization_code は、現在サインインしているユーザーに代わってバックエンド サービスを呼び出すためのアプリのアクセス許可を表します。  その後、アプリは、authoriztion_code をバックグラウンドで id_token と refresh_token と交換できます。  アプリは id_token を使用して HTTP 要求で Web API に対する認証を実行でき、id_token の有効期限が切れた場合は、refresh_token を使用して新しい id_token を取得できます。

> [AZURE.NOTE]
    Azure AD B2C プレビューは、現時点では、アプリ独自のバックエンド Web サービスにアクセスするために使用される id_token の取得のみをサポートします。  たとえば、完成したアプリには、iOS アプリ、Android アプリ、Web API を含めることができます。  このアーキテクチャは完全にサポートされます。  現時点でサポートされていないのは、iOS アプリが OAuth 2.0 access_token を使用してサード パーティ の Web API にアクセスできるようにすることです。  実際には、完成したアプリの各コンポーネントは、1 つのアプリケーション ID を共有する必要があります。

![ネイティブ アプリのスイムレーン イメージ](./media/active-directory-b2c-apps/native.png)

## 現在のプレビューの制限事項
これらの種類のアプリは、現時点では Azure AD B2C ではサポートされていませんが、一般公開に間に合うように準備中です。  追加の制限事項との Azure AD B2C プレビューの制限については説明で、 [制限記事](active-directory-b2c-limitations.md)します。

### シングル ページ アプリ (JavaScript)
最近多く見かけるようになったシングル ページ アプリのフロントエンドは、主に JavaScript で作成されています。AngularJS、Ember.js、Durandal などの SPA フレームワークが使われることも少なくありません。一般公開される Azure AD サービスでは、これらのアプリは OAuth 2.0 Implicit Flow を使用してサポートされます。ただし、このフローは Azure AD B2C ではまだ利用できません。  サポートされるまでには、あと少し時間がかかります。

### デーモン/サーバー サイド アプリ
長時間実行されるプロセスを含んだアプリや、ユーザーの介入なしで動作するアプリも、セキュリティで保護されたリソース (Web API など) にアクセスする必要があります。  これらのアプリは、OAuth 2.0 Client Credentials Flow で (ユーザーの委任 ID ではなく) アプリの ID を使用して認証を行い、トークンを取得することができます。 

このフローは、現時点では Azure AD B2C ではサポートされていません。つまり、アプリは、対話的なユーザー フローが起こった後でのみトークンを取得できます。  近い将来、クライアント資格情報フローが追加されます。

### Web API チェーン (On-Behalf-Of)
Azure AD B2C によって保護された Web API から、同様に保護されたダウンストリームの別の Web API を呼び出す手法は、多くのアーキテクチャで使用されています。  このシナリオは、バックエンドの Web API から Microsoft Online サービス (Azure AD Graph API など) を呼び出すネイティブ クライアントでよく見られます。

このように Web API を連鎖的に呼び出すシナリオは、OAuth 2.0 Jwt Bearer Credential Grant (On-Behalf-Of Flow) を使用してサポートできます。  ただし、現時点では、Azure AD B2C プレビューに On-Behalf-Of Flow は実装されていません。
