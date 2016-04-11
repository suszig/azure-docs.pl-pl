<properties
    pageTitle="Azure AD B2C プレビュー | Microsoft Azure"
    description="Azure AD B2C プレビューによってサポートされるプロトコルを直接使用してアプリを作成する方法。"
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
    ms.date="09/22/2015"
    ms.author="dastrock"/>

# Azure AD B2C プレビュー: 認証プロトコル

Azure AD B2C では、業界標準のプロトコルである OpenID Connect と OAuth 2.0 をサポートすることによって Identity-as-a-Service (サービスとしての ID) 機能がアプリに提供されます。  このサービスは標準に準拠していますが、これらのプロトコルには、実装によって微妙な違いが存在する場合があります。  ここでは、いずれかのオープン ソース ライブラリを使うのではなく、コードから直接 HTTP 要求を送信して処理する場合に役立つ情報を紹介します。  各特定のプロトコルの詳細に説明する前にこのページに簡単な情報を読み取ることは既に Azure AD B2C 慣れている場合を直接参照することをお勧め [プロトコル リファレンス ガイド](#protocols)します。

<!-- TODO: Need link to libraries above -->

[AZURE.INCLUDE [active-directory-b2c-preview-note](../../includes/active-directory-b2c-preview-note.md)]
    
## 基本操作
使用して Azure AD B2C が必要になるが、B2C ディレクトリで登録されているすべてのアプリ、 [Azure ポータル](https://portal.azure.com)します。  アプリの登録プロセスでは、いくつかの値が収集され、対象のアプリに割り当てられます。

-  **アプリケーション Id** アプリを一意に識別します。
- A **リダイレクト URI** または **パッケージ識別子** アプリケーションに応答を返信に使用できます。
- その他シナリオに応じた値。  詳細については、学習方法 [アプリの登録](active-directory-b2c-app-registration.md)します。

登録されると、アプリケーションは、バージョン 2.0 のエンドポイントに要求を送信することによって Azure AD と通信します。

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize
https://login.microsoftonline.com/common/oauth2/v2.0/token
```

OAuth と OpenID Connect におけるフローはほぼすべて、情報のやり取りに 4 つの当事者が関係します。

![OAuth 2.0 Roles](./media/active-directory-b2c-reference-protocols/protocols_roles.png)

-  **承認サーバー** は Azure AD v2.0 エンドポイント。  ユーザーの本人性確認、リソースへのアクセス権の付与と取り消し、トークンの発行という役割を担います。  ID プロバイダーと呼ばれることもあります。ユーザーの情報とそのアクセス、そしてフロー内の当事者間の信頼関係に関するすべてのことは、承認サーバーによって安全に処理されます。
-  **リソース所有者** 、エンドユーザーは、通常です。  データを所有し、そのデータ (またはリソース) へのアクセスをサード パーティに許可する権限を持つ当事者です。
-  **OAuth クライアント** アプリケーション id によって識別されるアプリは、  通常はこの当事者がエンド ユーザーと情報をやり取りし、承認サーバーにトークンを要求します。  クライアントには、リソース所有者がリソースへのアクセス権を付与する必要があります。
-  **リソース サーバー** はリソースまたはデータが存在します。  承認サーバーを信頼し、OAuth クライアントを安全に認証、承認します。リソースへのアクセスを許可するためにベアラー トークン (access_tokens) が使用されます。

## ポリシー

おそらく、Azure AD の B2C **ポリシー** サービスの最も重要な機能です。  Azure AD B2C は、ポリシーを導入することによって、標準の OAuth 2.0 や OpenID Connect プロトコルを拡張します。ポリシーにより、Azure AD B2C は単なる認証および承認以外の多くの機能を実行できます。
ポリシーでは、サインアップ、サインイン、プロファイルの編集など、コンシューマーの ID エクスペリエンスが完全に記述されています。  ポリシーは、管理 UI で定義し、HTTP 認証要求の特別なクエリ パラメーターを使用して実行できます。  ポリシーは、OAuth 2.0 の標準機能ではありません。
OpenID Connect、ため、それらを理解する時間を行う必要があります。  詳細については、読み取り、 [Azure AD B2C ポリシーのリファレンス ガイド](active-directory-b2c-reference-policies.md)します。


## トークン
Azure AD B2C での OAuth 2.0 および OpenID Connect の実装では、ベアラー トークンが広範囲に使われています (JWT として表現されたベアラー トークンなど)。 ベアラー トークンは、保護されたリソースへの "ベアラー" アクセスを許可する簡易セキュリティ トークンです。 この意味で、"ベアラー" はトークンを提示できる任意の利用者を表します。 利用者がベアラー トークンを受信するには、まず Azure AD による認証が必要となりますが、転送中や保存時にトークンを保護するために必要な対策を講じていない場合、意図しない利用者によって傍受され、使用されるおそれがあります。 一部のセキュリティ トークンには、許可されていない利用者がトークンを使用できないようにするための組み込みメカニズムがありますが、ベアラー トークンにはこのメカニズムがないため、トランスポート層セキュリティ (HTTPS) などのセキュリティで保護されたチャネルで転送する必要があります。 ベアラー トークンが暗号化されずに転送された場合、悪意のある利用者が中間者攻撃によってトークンを取得し、保護されたリソースへの未承認のアクセスに使用する可能性があります。 後で使用するためにベアラー トークンを保存またはキャッシュするときにも、同じセキュリティ原則が適用されます。 常にアプリが送信され、安全な方法でベアラー トークンを格納することを確認します。 ベアラー トークンのセキュリティの詳細を参照してください。 [RFC 6750 セクション 5](http://tools.ietf.org/html/rfc6750)します。

Azure AD B2C で使用されるトークンのさまざまな種類の詳細が記載されている [、Azure AD トークン参照](active-directory-b2c-reference-tokens.md)します。

## プロトコル

要求の例を理解できる状態になったら、最初に以下のチュートリアルのどれかをご覧ください。  いずれも、特定の認証シナリオに対応しています。  これは、右側のフローを特定する必要がある場合
チェック アウト [、種類のアプリを Azure AD B2C 使用して構築できる](active-directory-b2c-apps.md)です。

- [OAuth 2.0 でモバイル アプリケーションおよびネイティブ アプリケーションを作成する](active-directory-b2c-reference-oauth-code.md)
- [Open ID Connect で Web Apps を作成する](active-directory-b2c-reference-oidc.md)
- OAuth 2.0 Implicit Flow で単一ページのアプリを作成する (近日公開予定)
- OAuth 2.0 Client Credentials Flow でデーモンまたはサーバー側プロセスを作成する (近日公開予定)
- OAuth 2.0 Resource Owner Password Credentials Flow でユーザー名とパスワードを使用してトークンを取得する (近日公開予定)
- OAuth 2.0 On Behalf Of Flow を使用して Web API でトークンを取得する (近日公開予定)

<!-- [Call the Azure AD Graph API using the OAuth 2.0 Client Credentials Flow](active-directory-reference-graph.md) -->

