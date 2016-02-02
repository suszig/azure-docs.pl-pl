<properties
    pageTitle="Azure AD B2C プレビュー | Microsoft Azure"
    description="Azure AD で導入された OpenID Connect 認証プロトコルを利用し、Web アプリケーションを構築します。"
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


# Azure AD B2C プレビュー: OpenID Connect による Web サインイン

OpenID Connect は OAuth 2.0 を基盤として開発された認証プロトコルであり、ユーザーを Web アプリケーションに安全にサインインさせるために利用されます。 OpenID Connect の Azure AD B2C の実装を利用することに外部委託するサインアップ、サインイン、
その他の id 管理が Azure AD への web アプリケーション内で発生します。 本ガイドでは、この外注を言語に依存せずに行う方法について説明します。オープンソース ライブラリを利用せず、HTTP メッセージを送受信する方法について説明します。


[AZURE.INCLUDE [active-directory-b2c-preview-note](../../includes/active-directory-b2c-preview-note.md)]

[OpenID Connect](http://openid.net/specs/openid-connect-core-1_0.html) は、OAuth 2.0 拡張 *承認* として使用するためのプロトコル、 *認証* プロトコルで、1 つを実行できるようにサインオン OAuth を使用しています。 概念が導入されて、 `id_token`, 、これはクライアントが、ユーザーの身元を確認し、ユーザーに関する基本的なプロファイル情報を取得できるようにするセキュリティ トークン。 アプリケーションに安全に取得することもできます OAuth 2.0 を拡張するため **access_tokens** によって保護されたリソースへのアクセスに使用できる、 [承認サーバー](active-directory-b2c-reference-protocols.md#the-basics)します。 OpenID Connect は、サーバーでホストされ、ブラウザーでアクセスされる Web アプリケーションを構築している場合に推奨されます。 場合は、モバイルに id 管理を追加する場合や、使用するデスクトップ アプリケーションが Azure AD B2C を使用して、 [OAuth 2.0](active-directory-b2c-reference-oauth-code.md) OpenID Connect ではなく。

Azure AD B2C は、単純な認証と権限付与以上のことができるように標準の OpenID Connect プロトコルを拡張したものです。 それが導入されています、 [* * ポリシー パラメーター * *](active-directory-b2c-reference-poliices.md),、 
など、アプリで発生すると、OpenID Connect を使用してユーザーを追加することができますサインアップ、サインイン、およびプロファイルの管理。 OpenID Connect とポリシーを使用して、これらのエクスペリエンスを実装する方法を紹介は、ここ 
web アプリケーションや get でにアクセスするための access_tokens web Api を使用します。

次の例 HTTP 要求は、サンプル B2C ディレクトリを使用して **fabrikamb2c.onmicrosoft.com**, 、サンプル アプリケーションだけでなく **https://aadb2cplayground.azurewebsites.net** とポリシー。 これらの値を利用し、要求を自由にお試しいただけます。あるいは、独自の値で置換できます。
学習方法 [独自 B2C ディレクトリ、アプリケーション、およびポリシーの取得](#use-your-own-b2c-directory)します。

## 認証要求を送信する

Web アプリは、ユーザーを認証し、ポリシーを実行する必要がある、ユーザーに向け、 `承認/` エンドポイント。 これはフローの対話部分であり、実際、ユーザーはポリシーに基づいて操作します。  
この要求では、クライアントは、ユーザーから取得する必要があるアクセス許可を示します。、 `スコープ` パラメーターとで実行するポリシー、 `p` パラメーター。 なお、改行読みやすくするため)、3 つの例が以下に示します
それぞれ異なるポリシーを使用します。 各要求の動作を感覚的に理解するために、要求をブラウザーに貼り付け、実行してみてください。

#### サインイン ポリシーを使用する

```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code+id_token
&redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
&response_mode=form_post
&scope=openid%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&nonce=12345
&p=b2c_1_sign_in
```

#### サインアップ ポリシーを使用する

```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code+id_token
&redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
&response_mode=form_post
&scope=openid%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&nonce=12345
&p=b2c_1_sign_up
```

#### プロファイル ポリシー編集を使用する

```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code+id_token
&redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
&response_mode=fragment
&scope=openid%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&nonce=12345
&p=b2c_1_edit_profile
```

| パラメーター| | 説明|
| ----------------------- | ------------------------------- | ----------------------- |
| client_id| 必須| アプリケーション Id を [Azure ポータル](https://portal.azure.com) アプリに割り当てられます。|
| response_type| 必須| 含める必要があります `id_token` OpenID Connect のです。使用することができます、web アプリでは、web API を呼び出すためのトークンが必要も場合、 `コード + id_token`, ここで行ったように、します。|
| redirect_uri| 必須| アプリ の redirect_uri。アプリは、この URI で認証応答を送受信することができます。ポータルで登録したいずれかの redirect_uri と完全に一致させる必要があります (ただし、URL エンコードが必要)。|
| scope| 必須| スコープのスペース区切りリスト。1 つのスコープ値は、要求されている両方のアクセス許可を Azure AD を示します。 `Openid` スコープにユーザーをサインインし、ユーザーに関するデータの形式で取得するための権限を示す **id_tokens** (これには何もそれ以上)。 `Offline_access` スコープは、web アプリでは省略可能です。リソースに長期アクセスするためにアプリは **refresh_token** を必要とすることを示します。|
| response_mode| 推奨| 結果として得られた authorization_code をアプリに返す際に使用するメソッドを指定します。'query'、'form_post'、'fragment' のいずれかを指定できます。|
| state| 推奨| 要求に含まれ、かつトークンの応答として返される値。任意の文字列を指定することができます。クロスサイト リクエスト フォージェリ攻撃を防ぐために通常、ランダムに生成された一意の値が使用されます。この状態は、認証要求の前にアプリ内でユーザーの状態 (表示中のページなど) に関する情報をエンコードする目的にも使用されます。|
| nonce| 必須| 要求に追加する (アプリによって生成された) 値。この値が、最終的な id_token に要求として追加されます。アプリでこの値を確認することにより、トークン再生攻撃を緩和することができます。通常この値はランダム化された一意の文字列になっており、要求の送信元を特定する際に使用できます。|
| p| 必須| 実行するポリシーを示します。B2C ディレクトリで作成されたポリシーの名前であり、その値は必ず「b2c_1_」で始まります。ポリシーの詳細について [ここ](active-directory-b2c-reference-policies.md)します。|
| prompt| 省略可能| ユーザーとの必要な対話の種類を指定します。現時点で有効な値は 'login' のみです。この場合ユーザーは要求時に、その資格情報を入力する必要があります。シングル サインオンは作用しません。|

この時点で、ユーザーにポリシーのワークフローの完了が求められます。 たとえば、ユーザー名とパスワードを入力したり、ソーシャル ID でサインインしたり、ディレクトリにサインアップしたりする必要があります。他にも、ポリシーの定義に基づき、多数の手順があります。
Azure AD が、表示されているアプリへの応答を返しますが、ユーザーには、ポリシーが完了すると、 `redirect_uri`, で指定されたメソッドを使用して、 `response_mode` パラメーター。 実行されたポリシーに関係なく、上記のすべての例で応答はまったく同じになります。

正常な応答を使用して、 `response_mode = クエリ` のようになります。

```
GET https://aadb2cplayground.azurewebsites.net/#
id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...
&state=arbitrary_data_you_can_receive_in_the_response
```

| パラメーター| 説明|
| ----------------------- | ------------------------------- |
| id_token| アプリが要求した id_token。この id_token を使用してユーザーの本人性を確認し、そのユーザーとのセッションを開始することができます。含まれる id_tokens とその内容の詳細については、 [Azure AD B2C トークン参照](active-directory-b2c-reference-tokens.md)します。|
| code| 使用した場合、アプリケーションが要求された authorization_code `response_type = コード + id_token`します。アプリは承認コードを使用して、対象リソースのアクセス トークンを要求します。承認コードは有効期間が非常に短く、通常 10 分後には期限切れとなります。|
| state| 要求に state パラメーターが含まれている場合、同じ値が応答にも含まれることになります。要求と応答に含まれる状態値が同一であることをアプリ側で確認する必要があります。|

エラー応答を送信することがありますも、 `redirect_uri` 、アプリケーションが適切に処理するようにします。

```
GET https://aadb2cplayground.azurewebsites.net/#
error=access_denied
&error_description=the+user+canceled+the+authentication
&state=arbitrary_data_you_can_receive_in_the_response
```

| パラメーター| 説明|
| ----------------------- | ------------------------------- |
| error| 発生したエラーの種類を分類したりエラーに対処したりする際に使用するエラー コード文字列。|
| error_description| 認証エラーの根本的な原因を開発者が特定しやすいように記述した具体的なエラー メッセージ。|
| state| 要求に state パラメーターが含まれている場合、同じ値が応答にも含まれることになります。アプリケーションは、要求と応答の状態の値が同じであることを確認する必要があります。|


## id_token を検証する

単に id_token を受け取るだけでは、ユーザーを認証するには不十分です。id_token の署名を検証し、そのトークンに含まれる要求をアプリの要件に従って確認する必要があります。 Azure AD B2C を使用して [JSON Web トークン (JWTs)](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html) と公開キーの暗号化に署名し、トークンが有効であることを確認します。 優先言語にもよりますが、JWT を検証するためのさまざまなオープンソース ライブラリを入手できます。 独自の検証ロジックを実装するより、オープンソース ライブラリを試してみることをお勧めします。 ライブラリを正しく利用する方法を模索するとき、ここにある情報が役に立つでしょう。

Azure AD B2C には、OpenID Connect メタデータ エンドポイントがあって、アプリは、このエンドポイントを使用することで、Azure AD B2C に関する情報を実行時にフェッチすることができます。 この情報には、エンドポイント、トークンの内容、トークンの署名キーが含まれます。 B2C ディレクトリにはポリシー別の JSON メタデータ ドキュメントがあります。 メタデータ図面など、 `b2c_1_sign_in` ポリシーで、 `fabrikamb2c.onmicrosoft.com` にあります。

`https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/v2.0/.well-known/openid-configuration?p=b2c_1_sign_in`

この構成ドキュメントのプロパティの 1 つは、 `jwks_uri`, が同じポリシー設定の値します。

`https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/discovery/v2.0/keys?p=b2c_1_sign_in`します。

id_token の署名で使用されたポリシー (とメタデータのフェッチ場所) は 2 つの方法で判断できます。 ポリシー名が含まれている最初に、 `acr` された id_token に要求します。 詳細な id_token からのクレームを解析する方法については、次を参照してください。、 [トークンの参照を Azure AD B2C](active-directory-b2c-reference-tokens.md)します。 値にポリシーをエンコードするという選択肢も、 `状態` パラメーター、要求を発行し、どのポリシーが使用されたかを判断するをデコードするとします。 いずれの方法も完全に有効です。

OpenID Connect メタデータ エンドポイントからメタデータ ドキュメントを取得したら、このエンドポイントにある RSA256 公開鍵を利用し、id_token の署名を検証できます。 複数のキー時刻で特定の時点には、このエンドポイントで示されている場合がある可能性がありますで識別される各、 `子供`します。 された id_token のヘッダーも含まれます、 `子供` 要求された id_token の署名に使用されたこれらのキーを示します。 参照してください、 [Azure AD B2C トークン参照](active-directory-b2c-reference-tokens.md) 詳細についてを含む [を検証するトークン](active-directory-b2c-reference-tokens.md#validating-tokens) と [重要な情報は署名キー ロール オーバー](active-directory-b2c-reference-tokens.md#validating-tokens)します。


id_token の署名を検証した後に、確認の必要な要求がいくつか存在します。

- 検証する必要があります、 `nonce` トークン リプレイ攻撃を防ぐために要求します。 その値が、サインイン要求で指定した値と一致していることが必要です。
- 検証する必要があります、 `aud` された id_token がアプリに対して発行されたことを確認を要求します。 その値がアプリのアプリケーション ID と一致している必要があります。
- 検証する必要があります、 `iat` と `exp` された id_token の有効期限が切れていないことを確認を要求します。

シナリオに応じてその他の要求も検証することができます。 以下に一般的な検証の例をいくつか挙げます。

- ユーザー/組織がアプリにサインアップ済みであることを確認する。
- 適切な承認/特権がユーザーにあることを確認する。
- 多要素認証など特定の強度の認証が行われたことを確認する。

Id_token 内のクレームの詳細については、次を参照してください。、 [トークンの参照を Azure AD B2C](active-directory-b2c-reference-tokens.md)します。

id_token を十分検証したら、ユーザーとのセッションを開始し、id_token に含まれる要求を使ってそのユーザーに関する情報をアプリの中で取得することができます。 取得した情報は、表示、記録、承認などに利用することができます。

## トークンを取得する

Web アプリに必要なことがポリシーの実行だけであれば、以降のセクションを一部省略できます。 以降のセクションは、Web アプリで Web API を認証呼び出しする必要があり、その呼び出しも Azure AD B2C で保護する場合にのみ適用されます。

取得した authorization_code を引き換えることができます (を使用して `response_type = コード + id_token`) トークンを送信して、目的のリソースのために、 `POST` への要求、 `トークン/` エンドポイント。 トークンを要求する唯一のリソースは、Azure AD B2C プレビューで
アプリのバックエンド web API です。 自分宛てにトークンを要求するために使用する規則は、スコープを使用する、 `openid`:

```
POST fabrikamb2c.onmicrosoft.com/v2.0/oauth2/token?p=b2c_1_sign_in HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/json

{
    "grant_type": "authorization_code",
    "client_id": "90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6",
    "scope": "openid offline_access",
    "code": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...",
    "redirect_uri": "urn:ietf:wg:oauth:2.0:oob",
    "client_secret": "<your-application-secret>"
}
```

| パラメーター| | 説明|
| ----------------------- | ------------------------------- | --------------------- |
| p| 必須| 認証コードの取得に使用されたポリシー。この要求に別のポリシーを使用することはできません。**このパラメーターはクエリ文字列に追加されることに注意してください。**POST 本文ではありません。|
| client_id| 必須| アプリケーション Id を [Azure ポータル](https://portal.azure.com) アプリに割り当てられます。|
| grant_type| 必須| 必要があります `authorization_code` 承認コード フローにします。|
| scope| 必須| スコープのスペース区切りリスト。1 つのスコープ値は、要求されている両方のアクセス許可を Azure AD を示します。 `Openid` スコープにユーザーをサインインし、ユーザーに関するデータの形式で取得するための権限を示す **id_tokens**します。クライアントと同じアプリケーション ID で表され、アプリの独自のバックエンド Web API にトークンを届けるために利用できます。 `Offline_access` スコープは、アプリが必要になっていることを示します、 **refresh_token** リソースにアクセスする有効期間が長いためです。|
| code| 必須| フローの最初の段階で取得した authorization_code。|
| redirect_uri| 必須| authorization_code を受け取った、アプリケーションの redirect_uri。|
| client_secret| 必須| 生成したアプリケーションのシークレット、 [Azure ポータル](https://portal.azure.com)します。このアプリケーション シークレットは重要なセキュリティ アーティファクトであり、サーバーに安全に保管する必要があります。また、慎重を期して、このクライアント シークレットを定期的に変更してください。|

正常なトークン応答は次のようになります。

```
{
    "not_before": "1442340812",
    "token_type": "Bearer",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...",
    "scope": "openid offline_access",
    "id_token_expires_in": "3600",
    "profile_info": "eyJ2ZXIiOiIxLjAiLCJ0aWQiOiI3NzU1MjdmZi05YTM3LTQzMDctOGIzZC1jY...",
    "refresh_token": "AAQfQmvuDy8WtUv-sd0TBwWVQs1rC-Lfxa_NDkLqpg50Cxp5Dxj0VPF1mx2Z...",
    "refresh_token_expires_in": "1209600"
}
```
| パラメーター| 説明|
| ----------------------- | ------------------------------- |
| not_before| トークンが有効と見なされる時間 (エポック時間)。|
| token_type| トークン タイプ値を指定します。Azure AD でサポートされるのは Bearer タイプのみです。|
| id_token| 要求した署名付きの JWT トークン。|
| scope| トークンの有効スコープ。後の利用のためにトークンのキャッシュに利用できます。|
| id_token_expires_in| id_token の有効期間 (秒)。|
| profile_info| base-64 エンコードの JSON 文字列。ネイティブ アプリケーションに表示するユーザーに関する役に立つ情報が含まれる場合があります。その厳密なコンテンツは、ポリシーで構成したアプリケーション要求に基づきます。|
| refresh_token| OAuth 2.0 更新トークン。アプリケーションは、このトークンを使用できる現在のトークンの有効期限が切れた後に、追加のトークンを取得します。Refresh_token は有効期間が長く、リソースへのアクセスを長時間保持するときに利用できます。詳細についてを参照してください、 [B2C トークン参照](active-directory-b2c-reference-tokens.md)します。使用している必要がありますスコープ注 `offline_access` 承認と更新トークンを受信するためにトークンの要求の両方でします。|
| refresh_token_expires_in| 更新トークンの最大有効期間 (秒)。ただし、更新トークンは任意の時点で無効になる場合があります。|

> [AZURE.NOTE]
    この時点で access_token の場所がわからなければ、次の点を考慮してください。 要求すると、 `openid` スコープ、Azure AD の JWT が発行 `id_token` 応答にします。 この `id_token` は技術的には、OAuth 2.0 access_token クライアントとして同じ client_id によって表される、アプリのバックエンド サービスと通信するときにそのために使用できます。  `Id_token` は HTTP authorization ヘッダー内のリソースに送信および要求の認証に使用できる符号付き JWT ベアラー トークンです。 その違いは、 `id_token` 特定のクライアント アプリケーションのアクセスをスコープするためのメカニズムはありません。 ただし、(現行の Azure AD B2C プレビューでそうであるように) クライアント アプリケーションがバックエンド サービスと通信できる唯一のクライアントの場合、そのようなスコープ メカニズムは必要ありません。 Azure AD B2C プレビューで追加のファースト パーティ リソースとサード パーティ リソースと通信する機能がクライアントに与えられるとき、access_tokens が導入されます。 ただし、その時点であってもを使用して `id_tokens` アプリのバックエンドと通信するサービスは残って強く推奨パターンです。 Azure AD B2C プレビューを使用して構築できるアプリケーションの種類の詳細については、次を参照してください。 [今回](active-directory-b2c-apps.md)します。

エラー応答は次のようになります。

```
{
    "error": "access_denied",
    "error_description": "The user revoked access to the app.",
}
```

| パラメーター| 説明|
| ----------------------- | ------------------------------- |
| error| 発生したエラーの種類を分類したりエラーに対処したりする際に使用するエラー コード文字列。|
| error_description| 認証エラーの根本的な原因を開発者が特定しやすいように記述した具体的なエラー メッセージ。|

## トークンを使用する

これで正常に取得した、 `id_token`, 、Web Api バックエンドへの要求トークンを使用するには以下のように、 `承認` ヘッダー。

```
GET /tasks
Host: https://mytaskwebapi.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
```

## トークンを更新する

Id_tokens は有効期間が短く、期限が切れた後もリソースにアクセスし続けるためにはトークンを更新する必要があります。 これを行う別の操作を送信することで `POST` への要求、 `トークン/` を提供するこのときは、エンドポイント、 `refresh_token` の代わりに、 `コード`:

```
POST fabrikamb2c.onmicrosoft.com/v2.0/oauth2/token?p=b2c_1_sign_in HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/json

{
    "grant_type": "refresh_token",
    "client_id": "90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6",
    "scope": "openid offline_access",
    "refresh_token": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...",
    "redirect_uri": "urn:ietf:wg:oauth:2.0:oob",
    "client_secret": "<your-application-secret>"    
}
```

| パラメーター| | 説明|
| ----------------------- | ------------------------------- | -------- |
| p| 必須| 元の更新トークンの取得に使用されたポリシー。この要求に別のポリシーを使用することはできません。**このパラメーターはクエリ文字列に追加されることに注意してください。**POST 本文ではありません。|
| client_id| 必須| アプリケーション Id を [Azure ポータル](https://portal.azure.com) アプリに割り当てられます。|
| grant_type| 必須| 必要があります `refresh_token` 承認コード フローのこの区間にします。|
| scope| 必須| スコープのスペース区切りリスト。1 つのスコープ値は、要求されている両方のアクセス許可を Azure AD を示します。 `Openid` スコープにユーザーをサインインし、ユーザーに関するデータの形式で取得するための権限を示す **id_tokens**します。クライアントと同じアプリケーション ID で表され、アプリの独自のバックエンド Web API にトークンを届けるために利用できます。 `Offline_access` スコープは、アプリが必要になっていることを示します、 **refresh_token** リソースにアクセスする有効期間が長いためです。|
| redirect_uri| 必須| authorization_code を受け取った、アプリケーションの redirect_uri。|
| refresh_token| 必須| フローの第 2 段階で取得した元の refresh_token。使用している必要がありますスコープ注 `offline_access` 承認と更新トークンを受信するためにトークンの要求の両方でします。|
| client_secret| 必須| 生成したアプリケーションのシークレット、 [Azure ポータル](https://portal.azure.com)します。このアプリケーション シークレットは重要なセキュリティ アーティファクトであり、サーバーに安全に保管する必要があります。また、慎重を期して、このクライアント シークレットを定期的に変更してください。|

正常なトークン応答は次のようになります。

```
{
    "not_before": "1442340812",
    "token_type": "Bearer",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...",
    "scope": "openid offline_access",
    "id_token_expires_in": "3600",
    "profile_info": "eyJ2ZXIiOiIxLjAiLCJ0aWQiOiI3NzU1MjdmZi05YTM3LTQzMDctOGIzZC1jY...",
    "refresh_token": "AAQfQmvuDy8WtUv-sd0TBwWVQs1rC-Lfxa_NDkLqpg50Cxp5Dxj0VPF1mx2Z...",
    "refresh_token_expires_in": "1209600"
}
```
| パラメーター| 説明|
| ----------------------- | ------------------------------- |
| not_before| トークンが有効と見なされる時間 (エポック時間)。|
| token_type| トークン タイプ値を指定します。Azure AD でサポートされるのは Bearer タイプのみです。|
| id_token| 要求した署名付きの JWT トークン。|
| scope| トークンの有効スコープ。後の利用のためにトークンのキャッシュに利用できます。|
| id_token_expires_in| id_token の有効期間 (秒)。|
| profile_info| base-64 エンコードの JSON 文字列。ネイティブ アプリケーションに表示するユーザーに関する役に立つ情報が含まれる場合があります。その厳密なコンテンツは、ポリシーで構成したアプリケーション要求に基づきます。|
| refresh_token| OAuth 2.0 更新トークン。アプリケーションは、このトークンを使用できる現在のトークンの有効期限が切れた後に、追加のトークンを取得します。Refresh_token は有効期間が長く、リソースへのアクセスを長時間保持するときに利用できます。詳細についてを参照してください、 [B2C トークン参照](active-directory-b2c-reference-tokens.md)します。|
| refresh_token_expires_in| 更新トークンの最大有効期間 (秒)。ただし、更新トークンは任意の時点で無効になる場合があります。|

エラー応答は次のようになります。

```
{
    "error": "access_denied",
    "error_description": "The user revoked access to the app.",
}
```

| パラメーター| 説明|
| ----------------------- | ------------------------------- |
| error| 発生したエラーの種類を分類したりエラーに対処したりする際に使用するエラー コード文字列。|
| error_description| 認証エラーの根本的な原因を開発者が特定しやすいように記述した具体的なエラー メッセージ。|




## サインアウト要求を送信する

アプリからユーザーをサインアウトしたいときに、アプリの cookie やそれ以外の場合の最後のユーザーとのセッションをオフに不十分です。 サインアウトするには、ユーザーを Azure AD にリダイレクトする必要もあります。 そうしない場合、ユーザーは資格情報を再入力しなくてもアプリで再認証されることがあります。Azure AD のシングル サインオン セッションが有効であるためです。

ユーザーをリダイレクトできるだけで、 `end_session_endpoint` 、同じ OpenID Connect メタデータ ドキュメントの説明に示されている [上](#validate-the-id-token):

```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/logout?
p=b2c_1_sign_in
&post_logout_redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
```

| パラメーター| | 説明|
| ----------------------- | ------------------------------- | ------------ |
| p| 必須| アプリケーションにサインインするためにユーザーが最近使用したポリシー。|
| post_logout_redirect_uri| 推奨| ログアウトの完了後にユーザーをリダイレクトする URL。指定しない場合、Azure AD B2C はユーザーに汎用メッセージを表示します。|

> [AZURE.NOTE]
    ユーザーにし、その間、 `end_session_endpoint` に現在ない効果的にユーザーをサインアウト、ユーザー シングル サイン オンの状態の Azure AD といくつかがクリアされます。それどころか、ユーザーがサインインに利用する IDP を選択すると、資格情報を入力しなくても再認証されます。ソーシャル IDP の場合、そのような行動が予想されます。ユーザーが B2C ディレクトリからサインアウトするとき、Facebook アカウントからの完全なサインアウトを望んでいるとは限りません。ただし、ローカル アカウントの場合、ユーザーのセッションを適切に終了できなければなりません。既知 [制限](active-directory-b2c-limitations.md) Azure AD のサインアウトのローカル アカウントは正しく動作しないことをプレビューします。即時の用語の回避策は送信する、 `ダイアログを表示する (&) ログインを =` 各認証要求は、目的の動作の外観になりますが、B2C ディレクトリ内のアプリケーション間でのシングル サインオンが動作しなくなる内のパラメーターです。

## 独自の B2C ディレクトリを使用する

これらの要求を自分で試す場合、最初に次の 3 つの手順を実行し、上記のサンプルの値を独自の値に置換する必要があります。

- [B2C ディレクトリを作成する](active-directory-b2c-get-started.md), 、し、要求で、ディレクトリの名前を使用します。
- [アプリケーションを作成する](active-directory-b2c-app-registration.md) アプリケーション Id と、redirect_uri を取得します。 アプリに **web app/web api** を追加し、必要に応じて、**アプリケーション シークレット**を作成します。
- [、ポリシーを作成](active-directory-b2c-reference-policies.md) ポリシー名を取得します。






