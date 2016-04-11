<properties
    pageTitle="キャッシュを追加して Azure API Management のパフォーマンスを向上させる | Microsoft Azure"
    description="API Management のサービスの呼び出しで、遅延、帯域幅の消費、Web サービスの負荷を改善させる方法について説明します。"
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
    ms.topic="get-started-article"
    ms.date="12/16/2015"
    ms.author="sdanie"/>

# キャッシュを追加して Azure API Management のパフォーマンスを向上させる

API Management では、応答のキャッシュ用に操作を構成できます。 応答のキャッシュを行うと、API の遅延、帯域幅の消費、頻繁に変更されないデータの Web サービスの負荷が大幅に小さくなります。

このガイドでは、API の応答のキャッシュを追加して、サンプルの Echo API 操作のポリシーを構成する方法を示します。 その後は、開発者ポータルで操作を呼び出してキャッシュの動作を確認することができます。

>[AZURE.NOTE] ポリシー式を使用してキーによって項目をキャッシュする方法については、次を参照してください。 [Azure API Management でのキャッシュ カスタム](api-management-sample-cache-by-key.md)します。

## 前提条件

このガイドの手順を実行するには、API Management サービスのインスタンスに API と成果物を構成しておく必要があります。 API Management サービス インスタンスを作成がない場合は、次を参照してください。 [API Management サービス インスタンスの作成][] で、 [Azure API Management を使ってみる][] チュートリアルです。

## <a name="configure-caching"> </a>キャッシュ用の操作の構成

この手順では、キャッシュの設定を確認します、 **GET Resource (cached)** 、サンプル Echo API の操作です。

>[AZURE.NOTE] 各 API Management サービス インスタンスには、Echo API をテストし、API Management について学習するために使用できますが事前に定義されています。 詳細については、次を参照してください。 [Azure API Management を使ってみる][]します。

クリックして、 **管理** 、API Management サービスの Azure クラシック ポータルで。 API Management パブリッシャー ポータルが表示されます。

![パブリッシャー ポータル][api-management-management-console]

をクリックして **Api** から、 **API Management** 、左側のメニューをクリックし、 **Echo API**します。

![Echo API][api-management-echo-api]

をクリックして、 **操作** タブをクリックし、をクリックして、 **GET Resource (cached)** からの操作、 **操作** ] ボックスの一覧です。

![Echo API 操作][api-management-echo-api-operations]

クリックして、 **キャッシュ** をこの操作のキャッシュ設定を表示するタブをクリックします。

![[キャッシュ] タブ][api-management-caching-tab]

操作に対してキャッシュを有効にするには選択、 **を有効にする** チェック ボックスをオンします。 この例では、キャッシュは有効になっています。

各操作の応答がキーの値に基づいて、 **クエリ文字列パラメーターごとにキャッシュ** と **ヘッダーごとにキャッシュ** フィールドです。 クエリ文字列パラメーターまたはヘッダーに基づいて複数の応答をキャッシュに格納するには、これらの 2 つのフィールドを使用して構成します。

**期間** キャッシュされた応答の有効期限の間隔を指定します。 この例では、間隔は **3600** 秒で、1 つの時間に相当します。

この例では、最初の要求のキャッシュ構成を使用して、 **GET Resource (cached)** 操作では、バックエンド サービスから応答が返されます。 この応答は、キャッシュに格納され、指定されたヘッダーとクエリ文字列パラメーターによってキー付けされます。 パラメーターが一致する後続の操作の呼び出しに対しては、キャッシュの有効期間が超過するまで、キャッシュに格納された応答が返されます。

## <a name="caching-policies"> </a>キャッシュ ポリシーの確認

この手順で、キャッシュの設定を調べて、 **GET Resource (cached)** 、サンプル Echo API の操作です。

キャッシュの設定が構成されている場合操作で、 **キャッシュ** ] タブのキャッシュ操作のポリシーが追加されます。 これらのポリシーは、ポリシー エディターで表示および編集できます。

をクリックして **ポリシー** から、 **API Management** 、左側のメニューを選択し、 **Echo API/GET Resource (cached)** から、 **操作** ドロップダウン リストです。

![ポリシー スコープの操作][api-management-operation-dropdown]

ポリシー エディターにこの操作のポリシーが表示されます。

![API Management ポリシー エディター][api-management-policy-editor]

この操作のポリシー定義には使用して確認したキャッシュ構成を定義するポリシーが含まれています、 **キャッシュ** 前の手順でタブをクリックします。

    <policies>
        <inbound>
            <base />
            <cache-lookup vary-by-developer="false" vary-by-developer-groups="false">
                <vary-by-header>Accept</vary-by-header>
                <vary-by-header>Accept-Charset</vary-by-header>
            </cache-lookup>
            <rewrite-uri template="/resource" />
        </inbound>
        <outbound>
            <base />
            <cache-store caching-mode="cache-on" duration="3600" />
        </outbound>
    </policies>

>[AZURE.NOTE] ポリシー エディターでキャッシュ ポリシーに加えられた変更が反映されます、 **キャッシュ** 操作、およびその逆のタブをクリックします。

## <a name="test-operation"> </a>操作の呼び出しとキャッシュのテスト

キャッシュの動作を確認するには、開発者ポータルから操作を呼び出します。 クリックして **デベロッパー ポータル** 右上のメニューにします。

![開発者ポータル][api-management-developer-portal-menu]

クリックして **Api** クリックして上部のメニューで **Echo API**します。

![Echo API][api-management-apis-echo-api]

>アカウントに対して構成されている (またはアカウントから見える) API が 1 つしかない場合、[API] をクリックすると、その API の操作に直接誘導されます。

選択、 **GET Resource (cached)** 操作、およびクリック **コンソールを開く**します。

![コンソールを開く][api-management-open-console]

コンソールを使用すると、開発者ポータルから直接操作を呼び出すことができます。

![コンソール][api-management-console]

既定値はそのまま **param1** と **param2**します。

目的のキーを選択して、 **サブスクリプション キー** ボックスの一覧です。 アカウントのサブスクリプションが 1 つしかない場合は自動的にそのサブスクリプションが選択されます。

入力 **sampleheader:value1** で、 **要求ヘッダー** テキスト ボックスです。

クリックして **HTTP Get** 応答ヘッダーのメモに記録します。

入力 **sampleheader:value2** で、 **要求ヘッダー** テキスト ボックス、およびクリック **HTTP Get**します。

注意してくださいの値 **sampleheader** が **value1** で応答します。 異なる値をいくつか試して、最初の呼び出しでキャッシュに格納された応答が返されることを確かめます。

入力 **25** に、 **param2** フィールドに、クリックして **HTTP Get**します。

値 **sampleheader** 応答では、今すぐ **value2**します。 操作の結果はクエリ文字列によってキー付けされているため、以前のキャッシュに格納された応答は返されません。

## <a name="next-steps"> </a>次のステップ

-   [その他のトピックもチェックして、 [高度な API の構成を使ってみる][] チュートリアルです。
-   キャッシュ ポリシーの詳細については、次を参照してください。 [キャッシュ ポリシー][] で、 [API Management ポリシー リファレンス][]します。
-   ポリシー式を使用してキーによって項目をキャッシュする方法については、次を参照してください。 [Azure API Management でのキャッシュ カスタム](api-management-sample-cache-by-key.md)します。

[api-management-management-console]: ./media/api-management-howto-cache/api-management-management-console.png
[api-management-echo-api]: ./media/api-management-howto-cache/api-management-echo-api.png
[api-management-echo-api-operations]: ./media/api-management-howto-cache/api-management-echo-api-operations.png
[api-management-caching-tab]: ./media/api-management-howto-cache/api-management-caching-tab.png
[api-management-operation-dropdown]: ./media/api-management-howto-cache/api-management-operation-dropdown.png
[api-management-policy-editor]: ./media/api-management-howto-cache/api-management-policy-editor.png
[api-management-developer-portal-menu]: ./media/api-management-howto-cache/api-management-developer-portal-menu.png
[api-management-apis-echo-api]: ./media/api-management-howto-cache/api-management-apis-echo-api.png
[api-management-open-console]: ./media/api-management-howto-cache/api-management-open-console.png
[api-management-console]: ./media/api-management-howto-cache/api-management-console.png


[How to add operations to an API]: api-management-howto-add-operations.md
[How to add and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: api-management-monitoring.md
[Add APIs to a product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Get started with Azure API Management]: api-management-get-started.md
[Get started with advanced API configuration]: api-management-get-started-advanced.md

[API Management policy reference]: https://msdn.microsoft.com/library/azure/dn894081.aspx
[Caching policies]: https://msdn.microsoft.com/library/azure/dn894086.aspx

[Create an API Management service instance]: api-management-get-started.md#create-service-instance

[Configure an operation for caching]: #configure-caching
[Review the caching policies]: #caching-policies
[Call an operation and test the caching]: #test-operation
[Next steps]: #next-steps

