<properties 
    pageTitle="Azure API Management で API とその操作、成果物を作成する方法" 
    description="API Management で API とその操作、成果物を作成する方法について説明します。" 
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
    ms.date="11/03/2015" 
    ms.author="sdanie"/>


# Azure API Management で API とその操作、成果物を作成する方法

Azure API Management では、API とその操作を成果物に追加します。開発者は、成果物に追加された API を使用してアプリケーションを構築することができます。 このセクションでは、API を作成してそこに操作を追加し、そのうえで、開発者が利用できるように API を成果物に関連付けて発行する方法を説明します。

## <a name="create-apis"> </a>Api を作成する方法

API Management における API は、クライアント アプリケーションから呼び出すことのできる一連の操作を表します。 新しい API は、パブリッシャー ポータルで作成します。

このガイドでは、API Management で新しい API を作成して構成する方法を説明します。

-   [Api のを作成する方法][]

## <a name="add-operations"> </a>API に操作を追加する方法

API Management 内の API を使用するためには、操作を追加する必要があります。 このガイドでは、API Management 内の API に各種の操作を追加して構成する方法を説明します。

-   [操作を API のに追加する方法][]

API とその操作を 1 回のステップで WADL 形式または Swagger 形式にインポートすることもできます。

-   [操作で、API の定義をインポートする方法][]

## <a name="add-product"> </a>を作成して成果物を発行する方法

API Management の成果物には、少なくとも 1 つの API に加え、使用量クォータや使用条件が含まれます。 成果物が発行されると、開発者は成果物をサブスクライブして、成果物の API の利用を開始できます。 これらのトピックでは、成果物の作成し、API を追加し、開発者に発行するためのガイドを提供します。

-   [追加し、製品を公開する方法][]
-   [作成して、高度な製品設定のを構成する方法][]


[create a product]: #create-product 
[add apis to a product]: #add-apis 
[add descriptive information to a product]: #add-description 
[publish a product]: #publish-product 
[make a product visible to developers]: #make-visible 
[view subscribers to a product]: #view-subscribers 
[next steps]: #next-steps 
[api-management-]: ./media/ 
[how to create apis]: api-management-howto-create-apis.md 
[how to add operations to an api]: api-management-howto-add-operations.md 
[how to add and publish a product]: api-management-howto-add-products.md 
[monitoring and analytics]: ../api-management-monitoring.md 
[how to import the definition of an api with operations]: api-management-howto-import-api.md 
[how create and configure advanced product settings]: api-management-howto-product-with-rules.md 

