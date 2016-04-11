<properties 
    pageTitle="API Management の重要概念" 
    description="API、成果物、ロール、グループ、その他 API Management の重要概念について説明します。" 
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

# Azure API Management で API の定義を操作と共にインポートする方法

API Management では、新しい API を作成した後で手動で API に操作を追加できるほか、1 ステップで API を操作と共にインポートすることもできます。

API と操作は、次の形式を使用してインポートできます。

-   WADL
-   Swagger

このガイドでは、1 ステップで新しい API を作成して操作をインポートする方法について説明します。 手動で API を作成して、操作の追加については、次を参照してください。 [Api を作成する方法][] と [API に操作を追加する方法][]します。

## <a name="import-api"> </a>API のインポート

API を作成および構成するには、パブリッシャー ポータルを使用します。 パブリッシャー ポータルにアクセスするには、クリックして **管理** 、API Management サービスの Azure クラシック ポータルで。 API Management サービス インスタンスを作成がない場合は、次を参照してください。 [API Management サービス インスタンスの作成][] で、 [Azure API Management を使ってみる][] チュートリアルです。

![パブリッシャー ポータル][api-management-management-console]

をクリックして **Api** から、 **API Management** 、左側のメニューをクリックし、 **[API のインポート**します。

![API のインポート][api-management-import-apis]

 **API のインポート** ウィンドウには、API 仕様を指定する 3 つの方法に対応する 3 つのタブです。

-   **クリップボードから** 専用のテキスト ボックスに、API 仕様を貼り付けることができます。
-   **ファイルから** を参照して、API 仕様を含むファイルを選択することができます。
-   **URL から** URL、API の仕様を指定することができます。

![API の形式のインポート][api-management-import-api-clipboard]

API 仕様を指定した後は、右側のラジオ ボタンを使用して仕様の形式を指定します。 次の形式がサポートされています。

-   WADL
-   Swagger

次に、入力、 **Web API URL サフィックス**します。 このサフィックスが、API Management サービスのベース URL に付加されます。 ベース URL は、API Management サービスのインスタンスでホストされるすべての API に共通です。 API Management では API がサフィックスによって識別されます。そのため、サフィックスは、特定の API Management サービス インスタンスのすべての API で一意である必要があります。

すべての値が入力されると、クリックして **保存** API および関連する操作を作成します。 

>[AZURE.NOTE] Swagger 形式で basic calculator API をインポートするチュートリアルについては、次を参照してください。 [最初の Azure API Management で Api](api-management-get-started.md)します。

## <a name="export-api"> </a> API のエクスポート

新しい API をインポートする操作に加えて、パブリッシャー ポータルから API の定義をエクスポートすることもできます。 これを行うには、次のようにクリックします。 **API のエクスポート** から、 **の概要] タブ** の、 **API**します。

![API のエクスポート][api-management-export-api]

API は、WADL または Swagger を使用してエクスポートできます。 目的の形式を選択して [ **保存**, 、ファイルを保存する場所を選択します。

![API の形式のエクスポート][api-management-export-api-format]

## <a name="next-steps"> </a>次のステップ

API を作成して操作をインポートした後は、追加の設定を確認して構成する作業、API を成果物に追加する作業、API を発行して開発者が利用できるようにする作業を行います。 詳細については、次のガイドを参照してください。

-   [API 設定の構成][]
-   [Azure API Management で成果物を作成して発行する方法][]




[api-management-management-console]: ./media/api-management-howto-import-api/api-management-management-console.png
[api-management-import-apis]: ./media/api-management-howto-import-api/api-management-api-import-apis.png
[api-management-import-api-clipboard]: ./media/api-management-howto-import-api/api-management-import-api-wizard.png
[api-management-export-api]: ./media/api-management-howto-import-api/api-management-export-api.png
[api-management-export-api-format]: ./media/api-management-howto-import-api/api-management-export-api-format.png

[Import an API]: #import-api
[Export an API]: #export-api
[Configure API settings]: #configure-api-settings
[Next steps]: #next-steps

[Get started with Azure API Management]: api-management-get-started.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance

[How to add operations to an API]: api-management-howto-add-operations.md
[How to create and publish a product]: api-management-howto-add-products.md
[How to create APIs]: api-management-howto-create-apis.md
[How to configure API settings]: api-management-howto-create-apis.md#configure-api-settings

