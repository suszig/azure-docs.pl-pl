<properties 
    pageTitle="ユーザーの登録と成果物のサブスクリプションを委任する方法" 
    description="ユーザーの登録と製品のサブスクリプションを Azure API Management でサード パーティに委任する方法について説明します。" 
    services="api-management" 
    documentationCenter="" 
    authors="antonba" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="12/03/2015" 
    ms.author="antonba"/>

# ユーザーの登録と成果物のサブスクリプションを委任する方法

委任を使用すると、開発者のサインイン/サインアップおよび成果物のサブスクリプション処理を、開発者ポータルの組み込みの機能ではなく、お客様の既存の Web サイトを使用して行うことができます。 これにより、お客様の Web サイトでユーザー データを保持し、独自の方法でこれらのステップの検証を実行できます。

## <a name="delegate-signin-up"> </a>開発者のサインインおよびサインアップ処理の委任

開発者のサインインおよびサインアップ処理をお客様の既存の Web サイトに委任するには、API Management 開発者ポータルから開始される該当する要求のエントリ ポイントとして動作する特殊な委任エンドポイントをサイト上に作成する必要があります。

最終的なワークフローは次のようになります。

1. 開発者が、API Management 開発者ポータルのサインインまたはサインアップ リンクをクリックします。
2. ブラウザーが、委任エンドポイントにリダイレクトされます。
3. 委任エンドポイントにより、ユーザーにサインインまたはサインアップを求める UI にリダイレクトされるか、ユーザーにサインインまたはサインアップを求める UI が表示されます。
4. サインインまたはサインアップが成功すると、ユーザーが最初の API Management 開発者ポータル ページにリダイレクトされます。


これを実現するには、最初に委任エンドポイント経由で要求をルーティングするように API Management を設定します。 API Management パブリッシャー ポータルで、をクリックして **セキュリティ** ] をクリックし、 **委任** ] タブをクリックします。 チェックボックスをクリックして、[サインインおよびサインアップの委任] を有効にします。

![[委任] ページ][api-management-delegation-signin-up]

* 決めてどのような特殊な委任エンドポイントの URL を入力、 **委任エンドポイント URL** フィールドです。 

* 内で、 **委任の認証キー** フィールドは、要求が実際に、Azure API Management から送信されたことを確認する検証のために提供される署名の計算に使用されるシークレットを入力します。 クリックして、 **生成** API managemnet によってランダムにするキーを生成する] ボタンをクリックします。

作成する必要があります、 **委任エンドポイント**します。 委任エンドポイントでは、次に示す操作を実行します。

1. 次の形式の要求を受け取ります。

    > *http://www.yourwebsite.com/apimdelegation?operation=SignIn&returnUrl= {元のページの URL} & salt = {文字列} & sig = {文字列}*

    サインイン/サインアップ処理のためのクエリ パラメーター:
    - **操作**: 委任要求の値はあるは、どのような型を識別 **SignIn** ここで
    - **returnUrl**: ユーザーがサインインまたはサインアップ リンクをクリックしたページの URL
    - **salt**: セキュリティ ハッシュを計算するために使用される特殊な salt 文字列
    - **sig**: 計算したハッシュに固有の比較に使用する計算セキュリティ ハッシュ

2. 要求の送信元が Azure API Management であることを確認します (省略できますが、セキュリティ上強く推奨されます)。

    * に基づいて、文字列の hmac-sha512 ハッシュを計算、 **returnUrl** と **salt** クエリ パラメーター ([example code provided below])。
        > HMAC (**salt** + '\n' + **returnUrl**)
         
    * 値を上記で計算されたハッシュを比較、 **sig** クエリ パラメーターです。 2 つのハッシュ値が等しい場合は、次の手順に移動します。それ以外の場合は、要求を拒否します。

2. 記号と記号アップの要求を受け取っていることを確認します。、 **操作** クエリのパラメーターを設定する"**SignIn**"です。

3. サインインまたはサインアップのための UI をユーザーに表示します。

4. サインアップの場合は、API Management に対応するアカウントを作成する必要があります。 [Create a user] API management REST API です。 このとき、ユーザー ID をユーザー ストア内のユーザー ID と同じに設定するか、または追跡が可能な ID に設定してください。

5. ユーザーが正常に認証されたら、次の操作を行います。

    * [request a single-sign-on (SSO) token] API Management REST API を介して

    * 上記の API 呼び出しによって受け取った SSO URL に returnUrl クエリ パラメーターを付加します。
        > 例: https://customer.portal.azure-api.net/signin-sso?token&returnUrl=/return/url 

    * 上記の手順で生成された URL にユーザーをリダイレクトします。

加え、 **SignIn** 操作を行うこともアカウント管理の前の手順に従って、次の操作のいずれかを使用しています。

-   **ChangePassword**
-   **ChangeProfile**
-   **CloseAccount**

アカウントの管理操作を実行するには、次のクエリ パラメーターを渡す必要があります。

-   **操作**: (ChangePassword、ChangeProfile、または CloseAccount) が委任要求の種類を識別します。
-   **userId**: 管理するアカウントのユーザー id
-   **salt**: セキュリティ ハッシュを計算するために使用される特殊な salt 文字列
-   **sig**: 計算したハッシュに固有の比較に使用する計算セキュリティ ハッシュ

## <a name="delegate-product-subscription"> </a>成果物のサブスクリプション処理の委任

成果物のサブスクリプション処理を委任するしくみは、ユーザーのサインイン/サインアップ処理の委任と似ています。 最終的なワークフローは次のようになります。

1. 開発者が API Management 開発者ポータルで成果物を選択し、[サブスクライブ] ボタンをクリックします。
2. ブラウザーが、委任エンドポイントにリダイレクトされます。
3. 委任エンドポイントにより、必要な成果物のサブスクリプション操作が実行されます。ここでは任意の操作を実行でき、課金情報を要求する別のページへのリダイレクトしたり、追加の質問をたずねたりできます。また、ユーザーによる操作を求めることなく単に情報を保存することもあります。


機能を有効にする、 **委任** ページをクリックして **成果物のサブスクリプションを委任**します。

次に、次の操作を実行するように委任エンドポイントを設定します。


1. 次の形式の要求を受け取ります。

    > *http://www.yourwebsite.com/apimdelegation?operation= {操作} & productId = {サブスクライブする成果物} & userId = {要求元のユーザー} & salt = {文字列} & sig = {文字列}*

    成果物のサブスクリプション処理のためのクエリ パラメーター:
    - **操作**: 委任要求の種類を識別します。 成果物のサブスクリプション要求の有効なオプションを次に示します。
        - "Subscribe": ID (下記参照) を指定して、ユーザーと特定の成果物をサブスクライブする要求
        - サブスクリプションを「解除」: ユーザーの成果物のサブスクリプションを解除する要求
        - 「更新」: (例: がする有効期限近づいている) サブスクリプションを更新するための要求。
    - **productId**: ユーザーの要求をサブスクライブする成果物の ID
    - **userId**: 要求の対象ユーザーの ID
    - **salt**: セキュリティ ハッシュを計算するために使用される特殊な salt 文字列
    - **sig**: 計算したハッシュに固有の比較に使用する計算セキュリティ ハッシュ


2. 要求の送信元が Azure API Management であることを確認します (省略できますが、セキュリティ上強く推奨されます)。

    * に基づいて、文字列の hmac-sha512 を計算、 **productId**, 、**userId** と **salt** クエリ パラメーター。
        > HMAC (**salt** + '\n' + **productId** + '\n' + **userId**)
         
    * 値を上記で計算されたハッシュを比較、 **sig** クエリ パラメーターです。 2 つのハッシュ値が等しい場合は、次の手順に移動します。それ以外の場合は、要求を拒否します。
    
3. 要求された操作の種類に基づいて、成果物のサブスクリプション処理を行う **操作** -例: 課金、追加の質問などです。

4. API Management の成果物をユーザーが、ユーザー側の成果物を正常にサブスクライブするには、サブスクライブ [calling the REST API for product subscription]します。

## <a name="delegate-example-code"> </a> コード例 ##

次のコード サンプルを実行する方法を表示する、 *委任検証キー*, 、渡された returnUrl の有効性を証明を使用して、署名の検証を HMAC を作成に、パブリッシャー ポータルの [委任] 画面で設定されます。 同じコードは、わずかに変更するだけで、productId と userId に対して機能します。

**returnUrl のハッシュを生成する C# のコード**

    using System.Security.Cryptography;

    string key = "delegation validation key";
    string returnUrl = "returnUrl query parameter";
    string salt = "salt query parameter";
    string signature;
    using (var encoder = new HMACSHA512(Convert.FromBase64String(key)))
    {
        signature = Convert.ToBase64String(encoder.ComputeHash(Encoding.UTF8.GetBytes(salt + "\n" + returnUrl)));
        // change to (salt + "\n" + productId + "\n" + userId) when delegating product subscription
        // compare signature to sig query parameter
    }


**returnUrl のハッシュを生成する NodeJS のコード**

    var crypto = require('crypto');
    
    var key = 'delegation validation key'; 
    var returnUrl = 'returnUrl query parameter';
    var salt = 'salt query parameter';
    
    var hmac = crypto.createHmac('sha512', new Buffer(key, 'base64'));
    var digest = hmac.update(salt + '\n' + returnUrl).digest();
    // change to (salt + "\n" + productId + "\n" + userId) when delegating product subscription
    // compare signature to sig query parameter
    
    var signature = digest.toString('base64');

## 次のステップ

委任に関する詳細については、次のビデオをご覧ください。

> [AZURE.VIDEO delegating-user-authentication-and-product-subscription-to-a-3rd-party-site]

[Delegating developer sign-in and sign-up]: #delegate-signin-up
[Delegating product subscription]: #delegate-product-subscription
[request a single-sign-on (SSO) token]: http://go.microsoft.com/fwlink/?LinkId=507409
[create a user]: http://go.microsoft.com/fwlink/?LinkId=507655#CreateUser
[calling the REST API for product subscription]: http://go.microsoft.com/fwlink/?LinkId=507655#SSO
[Next steps]: #next-steps
[example code provided below]: #delegate-example-code

[api-management-delegation-signin-up]: ./media/api-management-howto-setup-delegation/api-management-delegation-signin-up.png 
