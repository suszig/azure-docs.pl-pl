<properties
    pageTitle="複数の Azure リージョンに Azure API Management サービス インスタンスをデプロイする方法"
    description="複数の Azure リージョンに Azure API Management サービス インスタンスをデプロイする方法について説明します。" 
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

# 複数の Azure リージョンに Azure API Management サービス インスタンスをデプロイする方法

API Management では複数リージョンのデプロイメントがサポートされており、API パブリッシャーは 1 つの API Management サービスを任意の数の Azure リージョンに分散できます。 これにより、地理的に分散した API コンシューマーによって認識される要求待ち時間が短くなり、1 つのリージョンがオフラインになった場合でもサービスの可用性を向上できます。 

API Management サービスが最初に作成されるが含まれている 1 つだけ [単位][] のプライマリ リージョンに指定されている単一 Azure リージョンに配置します。 リージョンは Azure クラシック ポータルで簡単に追加できます。 各リージョンには API Management のゲートウェイ サーバーがデプロイされており、呼び出しのトラフィックは最も近いゲートウェイにルーティングされます。 リージョンがオフラインになった場合、トラフィックは自動的に次に最も近いゲートウェイにリダイレクトされます。 

> [AZURE.IMPORTANT] 複数リージョンのデプロイメントは、 **[Premium][]** 層です。

## <a name="add-region"> </a>新しいリージョンに API Management サービス インスタンスをデプロイする

クリックして、 **管理** 、API Management サービスの Azure クラシック ポータルで。 API Management パブリッシャー ポータルが表示されます。

![パブリッシャー ポータル][api-management-management-console]

>API Management サービス インスタンスを作成がない場合は、次を参照してください。 [API Management サービス インスタンスの作成][] で、 [Azure API Management を使ってみる][] チュートリアルです。

移動し、 **スケール** Azure クラシック ポータルで API Management サービス インスタンス] タブをクリックします。 

![[スケール] タブ][api-management-scale-service]

新しいリージョンをデプロイするには、プライマリ リージョンの下のドロップダウン リストをクリックし、リストからリージョンを選択します。

![リージョンの追加][api-management-add-region]

リージョンを選択したら、[ユニット] ボックスの一覧から新しいリージョンのユニット数を選択します。

![ユニットの指定][api-management-select-units]

目的のリージョンとユニット数を構成すると、クリックして **保存**します。

## <a name="remove-region"> </a>リージョンから API Management サービス インスタンスを削除する

リージョンから API Management サービス インスタンスを削除するに移動、 **スケール** Azure クラシック ポータルで API Management サービス インスタンス] タブをクリックします。 

![[スケール] タブ][api-management-scale-service]

クリックして、 **X** を削除する目的のリージョンの右側にします。  

![リージョンの削除][api-management-remove-region]

目的のリージョンが削除されると、クリックして **保存**します。


[api-management-management-console]: ./media/api-management-howto-deploy-multi-region/api-management-management-console.png

[api-management-scale-service]: ./media/api-management-howto-deploy-multi-region/api-management-scale-service.png
[api-management-add-region]: ./media/api-management-howto-deploy-multi-region/api-management-add-region.png
[api-management-select-units]: ./media/api-management-howto-deploy-multi-region/api-management-select-units.png
[api-management-remove-region]: ./media/api-management-howto-deploy-multi-region/api-management-remove-region.png

[Create an API Management service instance]: api-management-get-started.md#create-service-instance
[Get started with Azure API Management]: api-management-get-started.md

[Deploy an API Management service instance to a new region]: #add-region
[Delete an API Management service instance from a region]: #remove-region

[unit]: http://azure.microsoft.com/pricing/details/api-management/
[Premium]: http://azure.microsoft.com/pricing/details/api-management/



