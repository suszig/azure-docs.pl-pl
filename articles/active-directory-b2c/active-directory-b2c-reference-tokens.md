<properties
    pageTitle="Azure AD B2C プレビュー | Microsoft Azure"
    description="Azure AD B2C プレビューで発行されるトークンの種類。"
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

# Azure AD B2C Preview: トークン リファレンス

Azure AD B2C がそれぞれの処理のセキュリティ トークンのいくつかの型を生成 [認証フロー](active-directory-b2c-apps.md)します。 このドキュメントでは、各トークンの種類の形式、セキュリティ特性、内容について説明します。

[AZURE.INCLUDE [active-directory-b2c-preview-note](../../includes/active-directory-b2c-preview-note.md)]

## トークンの種類

Azure AD B2C をサポートしています、 [OAuth 2.0 認証プロトコル](active-directory-b2c-reference-protocols.md), 、access_tokens と refresh_tokens の両方を利用します。  認証およびによるサインインもサポート [OpenID Connect](active-directory-b2c-reference-protocols.md), 、トークン、id_token の 3 つ目の種類が導入されています。  これらの各トークンは、「ベアラー トークン」として表されます。

ベアラー トークンは、保護されたリソースへの "ベアラー" アクセスを許可する簡易セキュリティ トークンです。 この意味で、"ベアラー" はトークンを提示できる任意の利用者を表します。 利用者がベアラー トークンを受信するには、まず Azure AD による認証が必要となりますが、転送中や保存時にトークンを保護するために必要な対策を講じていない場合、意図しない利用者によって傍受され、使用されるおそれがあります。 一部のセキュリティ トークンには、許可されていない利用者がトークンを使用できないようにするための組み込みメカニズムがありますが、ベアラー トークンにはこのメカニズムがないため、トランスポート層セキュリティ (HTTPS) などのセキュリティで保護されたチャネルで転送する必要があります。 ベアラー トークンが暗号化されずに転送された場合、悪意のある利用者が中間者攻撃によってトークンを取得し、保護されたリソースへの未承認のアクセスに使用する可能性があります。 後で使用するためにベアラー トークンを保存またはキャッシュするときにも、同じセキュリティ原則が適用されます。 アプリケーションでは、常に安全な方法でベアラー トークンを転送および保存してください。 ベアラー トークンのセキュリティの詳細を参照してください。 [RFC 6750 セクション 5](http://tools.ietf.org/html/rfc6750)します。

Azure AD B2C によって発行されるトークンの多くは、JSON Web トークン (JWT) として実装されます。  JWT は、2 つのパーティ間で情報を転送する、コンパクトで URL の安全な手段です。  JWT に含まれる情報は「要求」と呼ばれ、トークンのベアラーとサブジェクトに関する情報のアサーションです。  JWT の要求は、伝送用にエンコードおよびシリアル化された JSON オブジェクトです。  Azure AD B2C によって発行される JWT は署名されますが、暗号化されないため、デバッグの目的で JWT の内容を簡単に検査できます。  いくつかのツールがあるかなど使用可能な [calebb.net](https://calebb.net)します。 JWTs の詳細についてを参照することができます、 [JWT 仕様](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html)します。

## id_token

id_token は、アプリが Azure AD B2C の `authorize` および `token` エンドポイントから受け取るセキュリティ トークンの形式です。  として表されます [JWTs](#types-of-tokens), 、アプリにユーザーを識別するために使用できるクレームが含まれるとします。  `authorize` エンドポイントから取得した id_token は、Web アプリケーションへのユーザーのサインインによく使用されます。  `token` エンドポイントから取得した id_token は、同じアプリケーションまたはサービスの 2 つのコンポーネント間で通信するときに HTTP 要求に入れて送信されます。  必要に応じて id_token で要求を使用できます。一般には、アカウント情報の表示や、アプリ内でのアクセス制御の決定に使用されます。  

id_token は署名されますが、この時点では暗号化されません。  アプリや API は、id_token を受け取る、する必要があります [、署名の検証](#validating-tokens) トークンの信頼性を保証すると、トークンの有効性を証明するために、いくつかの要求を検証します。  シナリオの要件によって、アプリによって検証されたクレームが異なる場合がいくつかを使用する必要がある [一般的なクレームの妥当性検査](#validating-tokens) すべてのシナリオでアプリを実行する必要があります。

以下では、id_token のクレームの詳細および id_token のサンプルを示します。  id_token 内のクレームは特定の順序では返されないことに注意してください。  さらに、随時、新しいクレームが id_token に導入される可能性があります。アプリは、新しいクレームが導入されても問題ないようにする必要があります。  次の一覧のクレームは、この記事の執筆時点で、アプリで解釈できることが保証されているものです。 実際には、サンプル id_token 内のクレームを調べることによってに貼り付ける方法で参照してください [calebb.net](https://calebb.net)します。  必要に応じて、さらに詳しい情報がで見つからないかどうか、 [仕様の OpenID Connect](http://openid.net/specs/openid-connect-core-1_0.html)します。

#### id_token のサンプル
```
// Line breaks for display purposes only

eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6IklkVG9rZW5TaWduaW5nS2V5Q29udGFpbmVyIn0.
eyJleHAiOjE0NDIzNjAwMzQsIm5iZiI6MTQ0MjM1NjQzNCwidmVyIjoiMS4wIiwiaXNzIjoiaHR0cHM6Ly9s
b2dpbi5taWNyb3NvZnRvbmxpbmUuY29tLzc3NTUyN2ZmLTlhMzctNDMwNy04YjNkLWNjMzExZjU4ZDkyNS92
Mi4wLyIsImFjciI6ImIyY18xX3NpZ25faW5fc3RvY2siLCJzdWIiOiJOb3Qgc3VwcG9ydGVkIGN1cnJlbnRs
eS4gVXNlIG9pZCBjbGFpbS4iLCJhdWQiOiI5MGMwZmU2My1iY2YyLTQ0ZDUtOGZiNy1iOGJiYzBiMjlkYzYi
LCJpYXQiOjE0NDIzNTY0MzQsImF1dGhfdGltZSI6MTQ0MjM1NjQzNCwiaWRwIjoiZmFjZWJvb2suY29tIn0.
h-uiKcrT882pSKUtWCpj-_3b3vPs3bOWsESAhPMrL-iIIacKc6_uZrWxaWvIYkLra5czBcGKWrYwrAC8ZvQe
DJWZ50WXQrZYODEW1OUwzaD_I1f1HE0c2uvaWdGXBpDEVdsD3ExKaFlKGjFR2V7F-fPThkVDdKmkUDQX3bVc
yyj2V2nlCQ9jd7aGnokTPfLfpOjuIrTsAdPcGpe5hfSEuwYDmqOJjGs9Jp1f-eSNEiCDQOaTBSvr479L5ptP
XWeQZyX2SypN05Rjr05bjZh3j70ZUimiocfJzjibeoDCaQTz907yAg91WYuFOrQxb-5BaUoR7K-O7vxr2M-_
CQhoFA

```

#### id_token 内のクレーム

Azure AD B2C では、トークンの内容を細かく制御できます。  構成する [ポリシー](active-directory-b2c-reference-policies.md) アプリがその操作を必要とする要求で特定の一連のユーザー データを送信します。  これらの要求は、ユーザーのなどの標準プロパティを含めることができます `displayName` と `emailAddress`, 、だけでなく [カスタム ユーザー属性](active-directory-b2c-reference-custom-attr) B2C ディレクトリで定義することができます。  ただし、受け取るすべての id_token には特定のセキュリティ関連クレームのセットが格納されており、アプリケーションではそれを使用してユーザーと要求を安全に認証できます。  以下で示すのは、Azure AD B2C によって発行されるすべての id_token に存在するものと考えられるクレームです。これら以外のクレームはすべてポリシーによって決定されます。

| 名前 | クレーム | 値の例 | 説明 |
| ----------------------- | ------------------------------- | ------------ | --------------------------------- |
| 対象となる読者 | `aud` | `90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6` | トークンの受信者を示します。  id_token では、受信者はアプリ登録ポータルでアプリに割り当てられるアプリのアプリケーション ID です。  アプリでは、この値を検証し、一致しない場合はトークンを拒否する必要があります。 |
| Issuer | `iss` | `https://login.microsoftonline.com/775527ff-9a37-4307-8b3d-cc311f58d925/v2.0/` | トークンを作成して返した Security Token Service (STS)、およびユーザーが認証された Azure AD ディレクトリを示します。  アプリでは、発行者クレームを検証し、トークンが v2.0 エンドポイントからのものであることを確認する必要があります。 |
| 発行時刻 | `iat` | `1438535543` | トークンが発行された日時です。エポック時間で表されます。 |
| 期限切れ日時 | `exp` | `1438539443` | トークンが無効になる日時です。エポック時間で表されます。  アプリでは、このクレームを使用してトークンの有効期間の有効性を確認する必要があります。  |
| 期間の開始時刻 | `nbf` | `1438535543` |  トークンが有効になる日時です。エポック時間で表されます。 これは通常、発行日時と同じです。  アプリでは、このクレームを使用してトークンの有効期間の有効性を確認する必要があります。  |
| バージョン | `ver` | `1.0` | id_token のバージョンです。Azure AD で定義されています。 |
| コード ハッシュ | `c_hash` | `SGCPtt01wxwfgnYZy2VJtQ` | コード ハッシュは、id_token が OAuth 2.0 認証コードと共に発行される場合にのみ、id_token に含まれます。  これを使用して、認証コードの信頼性を検証できます。  参照してください、 [仕様の OpenID Connect](http://openid.net/specs/openid-connect-core-1_0.html) この検証を実行する方法の詳細についてです。 |
| アクセス トークン ハッシュ | `at_hash` | `SGCPtt01wxwfgnYZy2VJtQ` | アクセス トークン ハッシュは、id_token が OAuth 2.0 アクセス トークンと共に発行される場合にのみ、id_token に含まれます。  これを使用して、アクセス トークンの信頼性を検証できます。  参照してください、 [仕様の OpenID Connect](http://openid.net/specs/openid-connect-core-1_0.html) この検証を実行する方法の詳細についてです。 |
| nonce | `nonce` | `12345` | nonce は、トークンのリプレイ攻撃を緩和するための戦略です。  アプリでは、`nonce` クエリ パラメーターを使用して、承認要求で nonce を指定できます。  要求で指定した値は、id_token の `nonce` 要求で変更されずに出力されます。  これにより、アプリではこの値を要求で指定した値と比較して検証できます。この値は、アプリのセッションと特定の id_token を関連付けます。  アプリでは、id_token 検証プロセスの間にこの検証を実行する必要があります。 |
| [件名] | `sub` | `Not supported currently. Use oid claim.` | トークンが情報をアサートするプリンシパルです (アプリのユーザーなど)。 この値は変更不可で、再割り当ても再利用もできません。したがってこの値を使用すると、トークンを使用してリソースにアクセスする場合などに安全に承認チェックができます。  ただし、サブジェクト クレームは Azure AD B2C プレビューではまだ実装されていません。  サブジェクト クレームを使用して承認するのではなく、オブジェクト ID の `oid` クレームを含むようにポリシーを構成し、ユーザーの識別にはその値を使用する必要があります。 |
| 認証コンテキスト クラスの参照 | `acr` | `b2c_1_sign_in` | id_token の取得に使用されたポリシーの名前です。  |
| 認証時刻 | ' auth_time | `1438535543` | ユーザーが資格情報を最後に入力した時刻 (エポック時間) です。 |



## アクセス トークン

現時点では、Azure AD B2C はアクセス トークンを発行しません。  この初期プレビュー段階では、2 つの異なるパーティ間の認証はサポートされていません。ただし、id_token は同じアプリケーションのコンポーネント間の通信に使用できます。  アプリケーションと Azure AD B2C のパブリック プレビューでサポートされる認証シナリオの詳細についてを参照してください [B2C アプリ記事](active-directory-b2c-apps.md)します。

## 更新トークン

更新トークンは、OAuth 2.0 のフローで新しい id_token と access_token を取得するためにアプリで使用できるセキュリティ トークンです。  ユーザーが介入しなくても、アプリはユーザーに代わってリソースへの長期的なアクセスを実現できます。

更新トークン応答を受信するためにアプリを要求する必要があります、 `offline_acesss` スコープ。   詳細について、 `offline_access` の範囲を参照してください、 [Azure AD B2C プロトコル リファレンス](active-directory-b2c-reference-protocols.md)します。

更新トークンは、現在も将来的にも常に、アプリに対して完全に非透過的です。  Azure AD によって発行され、Azure AD によってのみ検査および解釈できます。  有効期間は長いですが、アプリを作成するときに更新トークンが一定期間残っているものと期待することはできません。  更新トークンは、いつでもさまざまな理由で無効になる可能性があります。  アプリで更新トークンが有効かどうかを知る唯一の方法は、Azure AD に対してトークン要求を行って更新トークンを利用することです。

新しいトークンに対して更新トークンを利用すると (そして、アプリが `offline_access` スコープを許可されている場合は)、トークン応答で新しい更新トークンを受け取ります。  新しく発行された更新トークンを保存し、要求で使用したものを置き換える必要があります。  これにより、可能な限り長く更新トークンが有効であることが保証されます。

## トークンの検証

現時点で、アプリにおいて実行する必要があるトークンの検証は、id_token の検証だけです。  id_token を検証するには、アプリは id_token の署名と id_token 内のクレームの両方を検証する必要があります。

<!-- TODO: Link -->
優先言語にもよりますが、JWT を検証するためのさまざまなオープンソース ライブラリを入手できます。  独自の検証ロジックを実装するより、オープンソース ライブラリを試してみることをお勧めします。  ライブラリを正しく利用する方法を模索するとき、ここにある情報が役に立つでしょう。

#### 署名の検証
JWT には 3 つのセグメントがあり、`.` 文字で区切られています。  最初のセグメントと呼ばれる、 **ヘッダー**, 、として 2 番目、 **本文**, 、3 番目の **署名**します。  署名セグメントを使用して id_token の信頼性を検証し、アプリで信頼できることを確認できます。

id_token は、RSA 256 などの業界標準の非対称暗号アルゴリズムを使用して署名されます。 id_token のヘッダーには、トークンの署名に使用されたキーと暗号方法に関する情報が含まれます。

```
{
        "typ": "JWT",
        "alg": "RS256",
        "kid": "GvnPApfWMdLRi8PDmisFn7bprKg"
}
```

`alg` クレームはトークンの署名に使用されたアルゴリズムを示し、`kid` または `x5t` クレームはトークンの署名に使用された特定の公開キーを示します。

いつでも、Azure AD は公開/秘密キー ペアの特定セットのいずれかを使用して、id_token に署名できます。  Azure AD は定期的に使用可能なキー セットをローテーションするので、このキー変更を自動的に処理するようにアプリを作成する必要があります。  Azure AD によって使用される公開キーの更新を確認する適切な頻度は、約 24 時間です。

Azure AD B2C には、OpenID Connect メタデータ エンドポイントがあって、アプリは、このエンドポイントを使用することで、Azure AD B2C に関する情報を実行時にフェッチすることができます。  この情報には、エンドポイント、トークンの内容、トークンの署名キーが含まれます。  B2C ディレクトリにはポリシー別の JSON メタデータ ドキュメントがあります。  たとえば、`fabrikamb2c.onmicrosoft.com` の `b2c_1_sign_in` ポリシーのメタデータ ドキュメントは次の場所にあります。

`https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/v2.0/.well-known/openid-configuration?p=b2c_1_sign_in`

署名の検証に必要な署名キー データは、次の場所にある OpenID Connect メタデータのドキュメントを使用して入手できます。

```
https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/v2.0/.well-known/openid-configuration?p=b2c_1_sign_in
```

`fabrikamb2c.onmicrosoft.com` はユーザーの認証に使用される B2C ディレクトリ、`b2c_1_sign_in` は id_token の取得に使用されるポリシーです。  id_token の署名で使用されたポリシー (とメタデータのフェッチ場所) は 2 つの方法で判断できます。  最初の方法です。ポリシー名が id_token の `acr` 要求に含まれています。  本文を Base 64 でデコードし、結果の JSON 文字列を逆シリアル化することによって、JWT の本文からクレームを解析できます。  `acr` クレームは、id_token の発行に使用されたポリシーの名前です。  もう 1 つの方法です。要求を発行するとき、`state` パラメーターの値に含まれるポリシーを符号化し、それから復号化して使用されたポリシーを判断します。  いずれの方法も完全に有効です。 

メタデータ ドキュメントは、OpenID Connect 認証の実行に必要なさまざまなエンドポイントの場所など、役に立つ情報を含む JSON オブジェクトです。  また、トークンの署名に使用される公開キーのセットの場所を示す `jwks_uri` も含まれます。  次に示すのがその場所ですが、メタデータ ドキュメントを使用して `jwks_uri` を解析することにより動的にその場所をフェッチするのが最善の方法です。

`https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/discovery/v2.0/keys?p=b2c_1_sign_in`

この URL に存在する JSON ドキュメントには、特定の時点で使用されているすべての公開キー情報が含まれます。  アプリでは、`kid` または `x5t` 要求を JWT ヘッダーで使用して、特定のトークンの署名に使用されたこのドキュメント内の公開キーを選択できます。  その後、正しい公開キーと指定されたアルゴリズムを使用して、署名の検証を実行できます。

署名の検証の実行は、このドキュメントの対象範囲外です。必要な場合は、役に立つオープン ソース ライブラリが数多く提供されています。

#### クレームの検証
アプリまたは API は、id_token を受け取ったら、id_token 内のクレームに対していくつかのチェックを実行する必要があります。  学習した内容は次のとおりです。

-  **対象ユーザー** - が要求された id_token がアプリケーションに付与するものであることを確認します。
-  **Not Before** と **期限** された id_token の有効期限が切れていないことを確認する - 要求します。
-  **発行者** 方向の要求のトークンが実際に発行されたことをアプリに Azure AD によってことを確認します。
-  **Nonce** - としてトークン リプレイ攻撃を緩和します。

これらの要求の予期される値の詳細については、上記に含まれる、 [id_token セクション](#id_tokens)です。

## トークンの有効期間

以下で示すトークンの有効期間は、アプリの開発およびデバッグで役に立つので、理解を深めるのためにのみ提供されています。  これらの有効期間が一定で変わらないものと想定してアプリを作成しないでください。随時、変更される可能性があります。

| トークン | 有効期間 | 説明 |
| ----------------------- | ------------------------------- | ------------ |
| id_token | 1 時間 | 通常、id_token の有効期間は 1 時間です。  Web アプリでは、この同じ有効期間を使用してユーザーとの独自のセッションを維持することも (推奨)、完全に異なるセッション有効期間を選択することもできます。  アプリで新しい id_token を取得する必要がある場合は、Azure AD に対して新しいサインイン要求を行うことだけが必要です。  ユーザーに Azure AD との有効なブラウザー セッションがある場合は、資格情報を再度入力する必要はない場合があります。 |
| 更新トークン | 最大 14 日 | 1 つの更新トークンは、最大 14 日間有効です。  ただし、更新トークンはさまざまな理由でいつでも無効になる可能性があるので、アプリでは、失敗するまで、または新しい更新トークンに置き換えるまで、更新トークンの使用を続行する必要がありますに。  更新トークンは、ユーザーが自分の資格情報を入力してから 90 日経過した場合も無効になります。 |
| 承認コード | 5 分 | 承認コードは有効期間が意図的に短くされており、受け取ったらすぐに access_token、id_token、refresh_token に利用する必要があります。 |


