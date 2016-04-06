<properties 
    pageTitle="Azure API Management で成果物を作成して発行する方法" 
    description="Azure API Management で成果物を作成して発行する方法について説明します。" 
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

# Azure API Management で成果物を作成して発行する方法

Azure API Management の成果物には、少なくとも 1 つの API に加え、使用量クォータや使用条件が含まれます。 成果物が発行されると、開発者は成果物をサブスクライブして、成果物の API の利用を開始できます。 このトピックでは、成果物を作成し、API を追加し、発行して開発者が利用できるようにする方法について説明します。

## <a name="create-product"> </a>成果物の作成

操作を API に追加して構成するには、パブリッシャー ポータルを使用します。 パブリッシャー ポータルにアクセスするには、クリックして **管理** 、API Management サービスの Azure クラシック ポータルで。

![パブリッシャー ポータル][api-management-management-console]

>API Management サービス インスタンスを作成がない場合は、次を参照してください。 [API Management サービス インスタンスの作成][] で、 [Azure API Management を使ってみる][] チュートリアルです。

をクリックして **製品** を表示する左側のメニューで、 **製品** ] ページを **[成果物の**です。

![製品][api-management-products]

![新しい成果物][api-management-add-new-product]

成果物のわかりやすい名前を入力して、 **名前** フィールドと [製品の説明、 **説明** フィールドです。

API Management の成果物を指定できます **開く** または **Protected**します。 保護された成果物を使用するには、事前にサブスクライブする必要があります。一方、オープンな成果物は、サブスクライブせずに使用できます。 確認 **サブスクリプションが必要** サブスクリプションが必要な保護の成果物を作成します。 これは、既定の設定です。

確認 **サブスクリプションの承認を必要と** する場合は、管理者が確認し、承認またはこの成果物に対するサブスクリプションの申し込みを拒否します。 チェック ボックスがオフの場合、サブスクリプションの申し込みは自動承認されます。 サブスクリプションの詳細については、次を参照してください。 [成果物のサブスクライバーの表示][]します。

開発者のアカウントで複数回、成果物にサブスクライブできるようにするに、 **複数のサブスクリプションを許可する** チェック ボックスをオンします。 このボックスがオンになっていない場合、各開発者アカウントは、成果物に 1 回だけサブスクライブできます。

![無制限の複数サブスクリプション][api-management-unlimited-multiple-subscriptions]

複数の同時サブスクリプション数を制限するためのチェック、 **への同時サブスクリプション数を制限** チェック ボックスをオンし、サブスクリプションの上限を入力します。 次に示す例では、同時サブスクリプションが開発者アカウントにつき 4 までに制限されています。

![4 つの複数サブスクリプション][api-management-four-multiple-subscriptions]

成果物のすべての新しいオプションが構成されると、クリックして **保存** 新しい成果物を作成します。

![製品][api-management-products-page]

>既定では、新製品は、パブリッシュされたされないおよびのみに表示されます、  **管理者** グループです。

成果物を構成するのには、製品名のをクリックして、 **製品** ] タブをクリックします。

## <a name="add-apis"> </a>成果物への API の追加

 **製品** ページには、構成の 4 つのリンクが含まれています: **概要**, 、**設定**, 、**可視性**, 、および **サブスクライバー**します。  **概要** ] タブは、Api の追加し公開または非公開に製品をします。

![概要][api-management-new-product-summary]

成果物を発行するには、事前に 1 つまたは複数の API を追加する必要があります。 これを行うには、次のようにクリックします。 **成果物への API の追加**します。

![API の追加][api-management-add-apis-to-product]

目的の Api を選択し、クリックして **保存**します。

## <a name="add-description"> </a>成果物への説明情報の追加

 **設定** ] タブをクリックすると、その目的にアクセスできる、Api、その他の有用な情報など、成果物の詳細について説明することができます。 このタブでは、API を呼び出す開発者を対象に、プレーンテキストまたは HTML マークアップ形式で情報を入力できます。

![成果物の設定][api-management-product-settings]

確認 **サブスクリプションが必要** を使用するか、サブスクライブせずに呼び出すことができるオープンな成果物を作成する] チェック ボックスをオフにするサブスクリプションが必要な保護の成果物を作成します。

選択 **サブスクリプションの承認を必要と** を手動ですべての成果物のサブスクリプション要求を承認する場合。 既定では、成果物のサブスクリプションはすべて自動的に許可されます。

開発者のアカウントで複数回、成果物にサブスクライブできるようにするに、 **複数のサブスクリプションを許可する** チェック ボックスをオンし、必要に応じて制限を指定します。 このボックスがオンになっていない場合、各開発者アカウントは、成果物に 1 回だけサブスクライブできます。

必要に応じて入力、 **使用条件** どのサブスクライバーが製品を使用するために受け入れる必要があります、製品の使用条件を記述するフィールドです。

## <a name="publish-product"> </a>成果物の発行

成果物に含まれる API を呼び出すには、あらかじめ成果物を発行しておく必要があります。  **概要** 製品のタブをクリックして **発行**, 、順にクリック **はい、その発行** を確認します。 以前に発行された成果物をプライベートにする] をクリックして **発行の取り消し**します。

![成果物の発行][api-management-publish-product]

## <a name="make-visible"> </a>開発者への成果物の公開

 **可視性** ] タブでは、開発者ポータルで製品を参照し、製品にサブスクライブすることがロールを選択することができます。

![成果物の可視性][api-management-product-visiblity]

有効にする、またはグループ内の開発者に対して成果物の可視性を無効にする、または、グループの横のチェック ボックスをオフにし、] をクリックし、 **保存**します。

>詳細については、次を参照してください。 [を作成して Azure API Management で開発者アカウントを管理グループを使用する方法][]します。

## <a name="view-subscribers"> </a>成果物のサブスクライバーの表示

 **サブスクライバー** タブには、製品にサブスクライブしている開発者が一覧表示されます。 開発者の名前をクリックすると、それぞれの開発者の詳細および設定が表示されます。 この例では、成果物をサブスクライブしている開発者は 1 人もいません。

![開発者][api-management-developer-list]

## <a name="next-steps"> </a>次のステップ

目的の API を追加して成果物を発行すると、開発者が成果物をサブスクライブして API を呼び出せるようになります。 これらの項目だけでなく高度な成果物の構成を説明するチュートリアルについてを参照してください。 [を作成して、Azure API Management で高度な製品設定を構成する方法][]します。

製品の操作の詳細については、次のビデオをご覧ください。

> [AZURE.VIDEO using-products]

[Create a product]: #create-product
[Add APIs to a product]: #add-apis
[Add descriptive information to a product]: #add-description
[Publish a product]: #publish-product
[Make a product visible to developers]: #make-visible
[View subscribers to a product]: #view-subscribers
[Next steps]: #next-steps

[api-management-management-console]: ./media/api-management-howto-add-products/api-management-management-console.png
[api-management-add-product]: ./media/api-management-howto-add-products/api-management-add-product.png
[api-management-add-new-product]: ./media/api-management-howto-add-products/api-management-add-new-product.png
[api-management-unlimited-multiple-subscriptions]: ./media/api-management-howto-add-products/api-management-unlimited-multiple-subscriptions.png
[api-management-four-multiple-subscriptions]: ./media/api-management-howto-add-products/api-management-four-multiple-subscriptions.png
[api-management-products-page]: ./media/api-management-howto-add-products/api-management-products-page.png
[api-management-new-product-summary]: ./media/api-management-howto-add-products/api-management-new-product-summary.png
[api-management-add-apis-to-product]: ./media/api-management-howto-add-products/api-management-add-apis-to-product.png
[api-management-product-settings]: ./media/api-management-howto-add-products/api-management-product-settings.png
[api-management-publish-product]: ./media/api-management-howto-add-products/api-management-publish-product.png
[api-management-product-visiblity]: ./media/api-management-howto-add-products/api-management-product-visibility.png
[api-management-developer-list]: ./media/api-management-howto-add-products/api-management-developer-list.png



[api-management-products]: ./media/api-management-howto-add-products/api-management-products.png
[api-management-]: ./media/api-management-howto-add-products/
[api-management-]: ./media/api-management-howto-add-products/


[How to add operations to an API]: api-management-howto-add-operations.md
[How to create and publish a product]: api-management-howto-add-products.md
[Get started with Azure API Management]: api-management-get-started.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance
[Next steps]: #next-steps
[How to create and use groups to manage developer accounts in Azure API Management]: api-management-howto-create-groups.md
[How create and configure advanced product settings in Azure API Management]: api-management-howto-product-with-rules.md 

