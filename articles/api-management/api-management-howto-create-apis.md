<properties 
    pageTitle="Azure API Management で API を作成する方法" 
    description="Azure API Management で API を作成して構成する方法について説明します。" 
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

# Azure API Management で API を作成する方法

API Management における API は、クライアント アプリケーションから呼び出すことのできる一連の操作を表します。 パブリッシャー ポータルで新しい API を作成した後、目的の操作を追加します。 操作を追加し、API を成果物に追加すると、API を発行できます。 API が発行されると、開発者はそれをサブスクライブして使用できます。

このガイドでは、プロセスの最初のステップである、API Management で新しい API を作成して構成する方法について説明します。 操作の追加と成果物の発行に関する詳細については、次を参照してください。 [API に操作を追加する方法][] と [を作成し、成果物の発行方法][]します。

## <a name="create-new-api"> </a>新しい API の作成

API を作成および構成するには、パブリッシャー ポータルを使用します。 パブリッシャー ポータルにアクセスするには、クリックして **管理** 、API Management サービスの Azure クラシック ポータルで。

![パブリッシャー ポータル][api-management-management-console]

>API Management サービス インスタンスを作成がない場合は、次を参照してください。 [API Management サービス インスタンスの作成][] で、 [Azure API Management を使ってみる][] チュートリアルです。

をクリックして **Api** から、 **API Management** 、左側のメニューをクリックし、 **API に追加する**です。

![API の作成][api-management-create-api]

使用して、 **新しい API を追加** ウィンドウで、新しい API を構成します。

![新しい API を追加する][api-management-add-new-api]

新しい API を構成するには、次のフィールドを使用します。

-   **Web API 名** 、API に対する一意でわかりやすい名前を提供します。 開発者ポータルとパブリッシャー ポータルには、この名前が表示されます。
-   **Web サービスの URL** API を実装する HTTP サービスを参照します。 要求は、API Management によってこのアドレスに転送されます。
-   **Web API URL サフィックス** が API management サービスのベース URL に追加されます。 ベース URL は、API Management サービス インスタンスによってホストされるすべての API に共通です。 API Management では API がサフィックスによって識別されるため、サフィックスは、特定の発行者のすべての API で一意である必要があります。
-   **Web API の URL スキーム** API へのアクセスに使用するプロトコルを決定します。 既定では、HTTP が指定されます。
-   必要に応じてこの新しい API を成果物に追加するには、クリックして、 **製品 (オプション)** ドロップダウンをクリックし、製品を選択します。 この手順を複数回繰り返して、API を複数の成果物に追加できます。

目的の値が構成されると、クリックして **保存**します。 新しい API が作成されると、その API の概要ページがパブリッシャー ポータルに表示されます。

![API の概要][api-management-api-summary]

## <a name="configure-api-settings"> </a>API 設定の構成

使用することができます、 **設定** を確認および API の構成の編集] タブをクリックします。 **Web API 名**, 、**Web サービスの URL**, 、および **Web API URL サフィックス** は最初に、API が作成され、変更できるここで設定します。 **説明** オプションの説明を提供し、 **Web API の URL スキーム** API へのアクセスに使用するプロトコルを決定します。

![API 設定][api-management-api-settings]

API を実装するバックエンド サービスのゲートウェイの認証を構成するには、選択、 **セキュリティ** ] タブをクリックします。  **資格情報を持つ** を構成するのにはドロップダウンを使用できます **HTTP 基本** または **クライアント証明書** 認証します。 HTTP 基本認証を使用するには、単に必要な資格情報を入力します。 クライアント証明書認証の使用方法の詳細については、次を参照してください。 [Azure API Management での証明書の認証にクライアントを使用して、バックエンド サービスをセキュリティで保護する方法][]します。

 **セキュリティ** ] タブは、構成にも使用できる **ユーザー承認** OAuth 2.0 を使用します。 詳細については、次を参照してください。 [Azure API Management で OAuth 2.0 を使用して開発者アカウントを認証する方法][]します。

![基本認証設定][api-management-api-settings-credentials]

クリックして **保存** API 設定に加えたあらゆる変更を保存します。

## <a name="next-steps"> </a>次のステップ

API を作成し、設定を構成した後は、操作を API に追加します。さらに、API を成果物に追加し、開発者が利用できるように発行します。 詳細については、次の記事を参照してください。

-   [API に操作を追加する方法][]
-   [Azure API Management で成果物を作成して発行する方法][]





[api-management-create-api]: ./media/api-management-howto-create-apis/api-management-create-api.png
[api-management-management-console]: ./media/api-management-howto-create-apis/api-management-management-console.png
[api-management-add-new-api]: ./media/api-management-howto-create-apis/api-management-add-new-api.png
[api-management-api-settings]: ./media/api-management-howto-create-apis/api-management-api-settings.png
[api-management-api-settings-credentials]: ./media/api-management-howto-create-apis/api-management-api-settings-credentials.png
[api-management-api-summary]: ./media/api-management-howto-create-apis/api-management-api-summary.png
[api-management-echo-operations]: ./media/api-management-howto-create-apis/api-management-echo-operations.png

[What is an API?]: #what-is-api
[Create a new API]: #create-new-api
[Configure API settings]: #configure-api-settings
[Configure API operations]: #configure-api-operations
[Next steps]: #next-steps

[How to add operations to an API]: api-management-howto-add-operations.md
[How to create and publish a product]: api-management-howto-add-products.md

[Get started with Azure API Management]: api-management-get-started.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance
[How to secure back-end services using client certificate authentication in Azure API Management]: api-management-howto-mutual-certificates.md
[How to authorize developer accounts using OAuth 2.0 in Azure API Management]: api-management-howto-oauth2.md
