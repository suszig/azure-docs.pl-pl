<properties 
    pageTitle="Azure API Management の OAuth 2.0 を使用して開発者アカウントを認証する方法" 
    description="API Management で OAuth 2.0 を使用してユーザーを承認する方法について説明します。" 
    services="api-management" 
    documentationCenter="" 
    authors="steved0x" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="12/07/2015" 
    ms.author="sdanie"/>

# Azure API Management の OAuth 2.0 を使用して開発者アカウントを認証する方法

多くの Api をサポートして [OAuth 2.0](http://oauth.net/2/) を API をセキュリティで保護し、有効なユーザーのみがアクセスを持っていること、およびだけが利用できるリソースにアクセスできます。 Azure API Management では、対話型の開発者コンソールでそのような API を使用できるようにするために、OAuth 2.0 に対応する API を使用するサービス インスタンスを構成できます。

## <a name="prerequisites"> </a>前提条件

このガイドでは、開発者アカウントに OAuth 2.0 認証を使用するように API Management サービス インスタンスを構成する方法を説明していますが、OAuth 2.0 プロバイダーを構成する方法は説明していません。 手順は似ていて、API Management サービス インスタンスでの OAuth 2.0 の構成に使用される情報は同じですが、各 OAuth 2.0 プロバイダーの構成は異なっています。 このトピックは、Azure Active Directory を OAuth 2.0 プロバイダーとして使用する例を示しています。

>[AZURE.NOTE] Azure Active Directory を使用して OAuth 2.0 を構成する方法については、次を参照してください。、 [WebApp GraphAPI-DotNet][] サンプルです。

## <a name="step1"> </a>API Management で OAuth 2.0 認証サーバーを構成する

クリックして、 **管理** 、API Management サービスの Azure クラシック ポータルで。 API Management パブリッシャー ポータルが表示されます。

![パブリッシャー ポータル][api-management-management-console]

>[AZURE.NOTE] API Management サービス インスタンスを作成がない場合は、次を参照してください。 [API Management サービス インスタンスの作成][] で、 [Azure API Management を使ってみる][] チュートリアルです。

をクリックして **セキュリティ** から、 **API Management** メニューで、左クリック **OAuth 2.0**, 、] をクリックし、 **承認サーバーの追加**します。

![OAuth 2.0][api-management-oauth2]

クリックすると **承認サーバーの追加**, 、新しい認証サーバーのフォームが表示されます。

![新しいサーバー][api-management-oauth2-server-1]

名前とオプションの説明を入力、 **名前** と **説明** フィールドです。 

>[AZURE.NOTE] これらのフィールドが現在の API Management サービス インスタンス内での OAuth 2.0 認証サーバーを識別するために使用し、OAuth 2.0 サーバーからその値になりません。

入力、 **クライアント登録ページ URL**します。 このページでは、ユーザーがアカウントを作成して管理できます。このページは使用される OAuth 2.0 プロバイダーによって異なります。  **クライアント登録ページ URL** を作成し、自分のアカウントを構成するユーザー アカウントの管理をサポートする OAuth 2.0 プロバイダーのユーザーが使用できるページへのポインターします。 この機能を OAuth 2.0 プロバイダーがサポートしている場合でも、組織によってはこの機能を構成または使用していない場合があります。 OAuth 2.0 プロバイダーでユーザーによるアカウント管理が構成されていない場合は、会社の URL、`https://placeholder.contoso.com` のような URL などのプレースホルダー URL を入力してください。

フォームの次のセクションが含まれています、 **認証コード付与タイプ**, 、**Authorization endpoint URL**, 、および **承認要求メソッド** 設定します。

![新しいサーバー][api-management-oauth2-server-2]

指定、 **認証コード付与タイプ** で目的の型をチェックします。 **認証コード** 既定で指定します。

入力、 **Authorization endpoint URL**します。 Azure Active Directory では、この URL は以下の URL のようになります。ここで、`<client_id>` は、使用するアプリケーションを OAuth 2.0 サーバーが識別するためのクライアント ID に置き換えてください。

    https://login.windows.net/<client_id>/oauth2/authorize

 **承認要求メソッド** OAuth 2.0 サーバーに承認要求を送信する方法を指定します。 既定では **取得** が選択されています。

次のセクションでは、 **トークン エンドポイント URL**, 、**クライアントの認証方法**, 、**アクセス トークンの送信方法**, 、および **既定のスコープ** が指定されています。

![新しいサーバー][api-management-oauth2-server-3]

Azure Active Directory OAuth 2.0 サーバーで、 **トークン エンドポイント URL** は、次の形式が、 `<APPID>`  の形式を持つ `yourapp.onmicrosoft.com`です。

    https://login.windows.net/<APPID>/oauth2/token

既定の設定 **クライアントの認証方法** は **基本的な**, 、および  **メソッドを送信するアクセス トークン** は **Authorization ヘッダー**します。 これらの値がと共に、フォームのこのセクションで構成されている、 **既定のスコープ**します。

 **クライアントの資格情報** セクションが含まれています、 **クライアント ID** と **クライアント シークレット**, 、これらは OAuth 2.0 サーバーの作成と構成プロセス中に取得します。 1 回、 **クライアント ID** と **クライアント シークレット** 指定されている、 **redirect_uri** の **認証コード** が生成されます。 この URI は、OAuth 2.0 サーバー構成で応答 URL を構成するために使用されます。

![新しいサーバー][api-management-oauth2-server-4]

場合 **認証コード付与タイプ** に設定されている **リソース所有者パスワード**, 、 **リソース所有者パスワードの資格情報** セクションがそれらの資格情報の指定に使用されます。 それ以外の場合、空白のままです。

![新しいサーバー][api-management-oauth2-server-5]

フォームが完了したら、クリックして **保存** を API Management OAuth 2.0 認証サーバーの構成を保存します。 サーバーの構成が保存された後、次のセクションで説明されているように、この構成を使用するように API を構成できます。

## <a name="step2"> </a>OAuth 2.0 ユーザー認証を使用するように API を構成する

クリックして **Api** から、 **API Management** ] メニューの [左の [必要な API の名前をクリックして **セキュリティ**, 、し、チェック ボックスをオンに **OAuth 2.0**します。

![ユーザー認証][api-management-user-authorization]

目的の **承認サーバー** ] をクリックし、ドロップダウン一覧から **保存**します。

![ユーザー認証][api-management-user-authorization-save]

## <a name="step3"> </a>開発者ポータルで OAuth 2.0 ユーザー認証をテストする

OAuth 2.0 認証サーバーを構成して、そのサーバーを使用するように API を構成した後、開発者ポータルに移動して API を呼び出すことにより、そのサーバーをテストできます。  クリックして **デベロッパー ポータル** 右上のメニューにします。

![開発者ポータル][api-management-developer-portal-menu]

クリックして **Api** し、上部のメニューで **Echo API**します。

![Echo API][api-management-apis-echo-api]

>[AZURE.NOTE] 構成されている (またはアカウントから見える) API 1 つだけの場合は、Api をクリックしに直接誘導その API の操作です。

選択、 **GET Resource** 操作をクリックして **コンソールを開く**, 、し、[ **認証コード** ドロップダウン リストからです。

![コンソールを開く][api-management-open-console]

 **認証コード** は選択すると、ポップアップ ウィンドウで、OAuth 2.0 プロバイダーのサインイン フォームで表示されます。 この例では、サインイン フォームは Azure Active Directory によって提供されています。

>[AZURE.NOTE] ポップアップを無効にした場合、ブラウザーで有効にするように求められます。 それらを有効にした後、選択 **認証コード** もう一度サインイン フォームが表示されます。

![サインイン][api-management-oauth2-signin]

サインインするいると、 **要求ヘッダー** のみが含まれて、 `Authorization : Bearer` 、要求を承認ヘッダー。

![要求ヘッダー トークン][api-management-request-header-token]

これで、残りのパラメーター用に必要な値を構成して、要求を送信できます。 

## 次のステップ

OAuth 2.0 と API Management の使用に関する詳細については、以下を参照して、ビデオ、および付随する [記事](api-management-howto-protect-backend-with-aad.md)します。

> [AZURE.VIDEO protecting-web-api-backend-with-azure-active-directory-and-api-management]

[api-management-management-console]: ./media/api-management-howto-oauth2/api-management-management-console.png
[api-management-oauth2]: ./media/api-management-howto-oauth2/api-management-oauth2.png
[api-management-user-authorization]: ./media/api-management-howto-oauth2/api-management-user-authorization.png
[api-management-user-authorization-save]: ./media/api-management-howto-oauth2/api-management-user-authorization-save.png
[api-management-oauth2-signin]: ./media/api-management-howto-oauth2/api-management-oauth2-signin.png
[api-management-request-header-token]: ./media/api-management-howto-oauth2/api-management-request-header-token.png
[api-management-developer-portal-menu]: ./media/api-management-howto-oauth2/api-management-developer-portal-menu.png
[api-management-open-console]: ./media/api-management-howto-oauth2/api-management-open-console.png
[api-management-oauth2-server-1]: ./media/api-management-howto-oauth2/api-management-oauth2-server-1.png
[api-management-oauth2-server-2]: ./media/api-management-howto-oauth2/api-management-oauth2-server-2.png
[api-management-oauth2-server-3]: ./media/api-management-howto-oauth2/api-management-oauth2-server-3.png
[api-management-oauth2-server-4]: ./media/api-management-howto-oauth2/api-management-oauth2-server-4.png
[api-management-oauth2-server-5]: ./media/api-management-howto-oauth2/api-management-oauth2-server-5.png
[api-management-apis-echo-api]: ./media/api-management-howto-oauth2/api-management-apis-echo-api.png


[How to add operations to an API]: api-management-howto-add-operations.md
[How to add and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: api-management-monitoring.md
[Add APIs to a product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Get started with Azure API Management]: api-management-get-started.md
[Get started with advanced API configuration]: api-management-get-started-advanced.md
[API Management policy reference]: api-management-policy-reference.md
[Caching policies]: api-management-policy-reference.md#caching-policies
[Create an API Management service instance]: api-management-get-started.md#create-service-instance

[http://oauth.net/2/]: http://oauth.net/2/
[WebApp-GraphAPI-DotNet]: https://github.com/AzureADSamples/WebApp-GraphAPI-DotNet

[Prerequisites]: #prerequisites
[Configure an OAuth 2.0 authorization server in API Management]: #step1
[Configure an API to use OAuth 2.0 user authorization]: #step2
[Test the OAuth 2.0 user authorization in the Developer Portal]: #step3
[Next steps]: #next-steps


