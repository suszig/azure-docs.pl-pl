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

このガイドでは、プロセスの最初のステップである、API Management で新しい API を作成して構成する方法について説明します。 操作の追加と成果物の発行に関する詳細については、次を参照してください。 [API の [] に操作を追加する方法][] と [を作成し、製品の公開方法][]します。

## <a name="create-new-api"> </a>新しい API を作成します。

API を作成および構成するには、パブリッシャー ポータルを使用します。 パブリッシャー ポータルにアクセスするには、API Management サービスの Azure クラシック ポータルで **[管理]** をクリックします。

![パブリッシャー ポータル][api-management-management-console]

>API Management サービス インスタンスを作成がない場合は、次を参照してください。 [API Management サービス インスタンスの [] を作成する][] で、 [Azure API Management の [] を使ってみる][] チュートリアルです。

左側の **[API Management]** メニューの **[API]** をクリックし、**[API の追加]** をクリックします。

![Create API][api-management-create-api]

**[新しい API の追加]** ウィンドウを使用して、新しい API を構成します。

![新しい API を追加する][api-management-add-new-api]

新しい API を構成するには、次のフィールドを使用します。

-   **[Web API 名]** は、API に対する一意のわかりやすい名前です。 開発者ポータルとパブリッシャー ポータルには、この名前が表示されます。
-   **[Web サービスの URL]** は、API が実装されている HTTP サービスの URL です。 要求は、API Management によってこのアドレスに転送されます。
-   **[Web API URL サフィックス]** は、API Management サービスのベース URL に付加されます。 ベース URL は、API Management サービス インスタンスによってホストされるすべての API に共通です。 API Management では API がサフィックスによって識別されるため、サフィックスは、特定の発行者のすべての API で一意である必要があります。
-   **[Web API の URL スキーム]** により、API へのアクセスに使用できるプロトコルが決まります。 既定では、HTTP が指定されます。
-   必要に応じてこの新しい API を成果物に追加するには、**[製品 (オプション)]** ドロップダウンをクリックして、成果物を選択します。 この手順を複数回繰り返して、API を複数の成果物に追加できます。

必要な値を構成した後、**[保存]** をクリックします。 新しい API が作成されると、その API の概要ページがパブリッシャー ポータルに表示されます。

![API の概要][api-management-api-summary]

## <a name="configure-api-settings"> </a>API の構成設定

API の構成を確認および編集するには、**[設定]** タブを使用します。 **[Web API 名]**、**[Web サービスの URL]**、および **[Web API URL サフィックス]** は、最初に API の作成時に設定され、ここで編集できます。 **[説明]** には、オプションの説明を入力します。**[Web API の URL スキーム]** により、API へのアクセスに使用できるプロトコルが決まります。

![API 設定][api-management-api-settings]

API を実装するバックエンド サービスのゲートウェイ認証を構成するには、**[セキュリティ]** タブを選択します。 **[資格情報]** ボックスの一覧を使用して、**[HTTP 基本]** 認証または **[クライアント証明書]** 認証を構成できます。 HTTP 基本認証を使用するには、単に必要な資格情報を入力します。 クライアント証明書認証の使用方法の詳細については、次を参照してください。 [[Azure API Management] 証明書の認証にクライアントを使用して、バックエンド サービスをセキュリティで保護する方法][]します。

**[セキュリティ]** タブを使用して、OAuth 2.0 を使用するように**ユーザー承認**を構成することもできます。 詳細については、次を参照してください。 [を [] で Azure API Management OAuth 2.0 を使用して開発者アカウントを認証する方法][]します。

![基本認証設定][api-management-api-settings-credentials]

**[保存]** をクリックして、API 設定に加えた変更を保存します。

## <a name="next-steps"> </a>次のステップ

API を作成し、設定を構成した後は、操作を API に追加します。さらに、API を成果物に追加し、開発者が利用できるように発行します。 詳細については、次の記事を参照してください。

-   [操作を API のに追加する方法][]
-   [作成して、製品を公開する方法][]






[api-management-create-api]: ./media/api-management-howto-create-apis/api-management-create-api.png 
[api-management-management-console]: ./media/api-management-howto-create-apis/api-management-management-console.png 
[api-management-add-new-api]: ./media/api-management-howto-create-apis/api-management-add-new-api.png 
[api-management-api-settings]: ./media/api-management-howto-create-apis/api-management-api-settings.png 
[api-management-api-settings-credentials]: ./media/api-management-howto-create-apis/api-management-api-settings-credentials.png 
[api-management-api-summary]: ./media/api-management-howto-create-apis/api-management-api-summary.png 
[api-management-echo-operations]: ./media/api-management-howto-create-apis/api-management-echo-operations.png 
[what is an api?]: #what-is-api 
[create a new api]: #create-new-api 
[configure api settings]: #configure-api-settings 
[configure api operations]: #configure-api-operations 
[next steps]: #next-steps 
[how to add operations to an api]: api-management-howto-add-operations.md 
[how to create and publish a product]: api-management-howto-add-products.md 
[get started with azure api management]: api-management-get-started.md 
[create an api management service instance]: api-management-get-started.md#create-service-instance 
[how to secure back-end services using client certificate authentication in azure api management]: api-management-howto-mutual-certificates.md 
[how to authorize developer accounts using oauth 2.0 in azure api management]: api-management-howto-oauth2.md 

