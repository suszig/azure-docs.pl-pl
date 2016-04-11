<properties
    pageTitle="Azure API Management で API を保護する | Microsoft Azure"
    description="クォータとスロットル (レート制限) ポリシーを使用して、API を保護する方法について説明します。"
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
    ms.date="12/07/2015"
    ms.author="sdanie"/>

# Azure API Management でレート制限を使用して API を保護する

このガイドでは、Azure API Management でレート制限やクォータ ポリシーを構成することによって、いかに簡単にバックエンド API の保護を追加できるかを示します。

このチュートリアルでは、開発者が毎分最大 10 回、1 週間に最大 200 回まで呼び出すことができる "無料試用版" の API 成果物を作成します。 次に、API を発行し、レート制限ポリシーをテストします。

>[AZURE.NOTE] 既に、製品が構成されているし、このチュートリアルで使用する、途中のレッスンできますに [呼び出しレート制限とクォータ ポリシーの構成][] を無料の評価版成果物の代わりに、製品を使用するチュートリアルに従います。

## <a name="create-product"> </a>成果物を作成するには

この手順では、サブスクリプションの承認が不要な無料試用版の成果物を作成します。

クリックして、 **管理** 、API Management サービスの Azure クラシックにします。 API Management パブリッシャー ポータルが表示されます。

![パブリッシャー ポータル][api-management-management-console]

>API Management サービス インスタンスを作成がない場合は、次を参照してください。 [API Management サービス インスタンスの作成][] で、 [Azure API Management を使ってみる][] チュートリアルです。

クリックして **製品** で、 **API Management** を表示する左側のメニュー、 **製品** ページです。

![製品の追加][api-management-add-product]

クリックして **[成果物の** を表示する、 **[新しい成果物** ] ダイアログ ボックス。

![新しいの製品を追加][api-management-new-product-window]

 **タイトル** ボックスに、入力 **無料評価版**します。

 **説明** ボックスに、次のテキストを入力します。
 **サブスクライバーは、最大 200 呼び出し/週のアクセスが拒否された後の 10 呼び出し/分を実行することになります。**

API Management の成果物は、保護することも開くこともできます。 保護された成果物を使用するには、事前にサブスクライブする必要があります。 オープンな成果物は、サブスクリプションがなくても使用できます。 いることを確認 **サブスクリプションが必要** サブスクリプションが必要な保護の成果物を作成時に選択します。 これは、既定の設定です。

確認し、拒否したりするサブスクリプションは、この製品を試行する場合は、管理者は、選択 **サブスクリプションの承認を必要と**です。 このチェック ボックスがオフの場合、サブスクリプションの申し込みは自動承認されます。 この例では、サブスクリプションを自動的に承認するため、チェック ボックスはオフにしてください。

開発者アカウントに複数回を新しい成果物にサブスクライブできるように、選択、 **複数の同時サブスクリプションを許可する** チェック ボックスをオンします。 このチュートリアルでは複数の同時サブスクリプションを使用しないため、これはオフのままにしておいてください。

すべての値を入力したら、クリックして **保存** 、成果物を作成します。

![製品が追加されました][api-management-product-added]

既定では、新しい成果物がユーザーに表示、 **管理者** グループです。 追加しようとして、 **開発者** グループです。 をクリックして **無料評価版**, 、順にクリックし、 **可視性** ] タブをクリックします。

>API Management では、開発者に成果物の表示を許可するかどうかが、グループを使用して管理されます。 成果物の表示の可否はグループに対して付与されます。開発者は、自分が所属するグループから見える成果物を表示してサブスクライブすることができます。 詳細については、次を参照してください。 [を作成して Azure API Management でグループを使用する方法][]します。

![開発者グループの追加][api-management-add-developers-group]

選択、 **開発者** チェック ボックスをオンにし **保存**します。

## <a name="add-api"> </a>成果物に API を追加するには

このチュートリアル ステップでは、新しい無料試用版の成果物に Echo API を追加します。

>それぞれの API Management サービス インスタンスには、Echo API があらかじめ構成されています。API Management を体験、学習する目的で使用することができます。 詳細については、次を参照してください。 [Azure API Management を使ってみる][]します。

をクリックして **製品** から、 **API Management** 、左側のメニューをクリックし、 **無料評価版** 製品を構成します。

![成果物の構成][api-management-configure-product]

クリックして **成果物への API の追加**します。

![API を製品に追加][api-management-add-api]

選択 **Echo API**, 、クリックして **保存**します。

![Echo API の追加][api-management-add-echo-api]

## <a name="policies"> </a>呼び出しレート制限ポリシーとクォータ ポリシーを構成するには

レート制限とクォータは、ポリシー エディターで構成します。 クリックして **ポリシー** 下にある、 **API Management** 左側のメニュー。  **製品** 一覧で、クリックして **無料評価版**します。

![製品のポリシー][api-management-product-policy]

クリックして **ポリシーの追加** ポリシー テンプレートをインポートし、レート制限とクォータ ポリシーの作成を開始します。

![ポリシーの追加][api-management-add-policy]

ポリシーを挿入するには、いずれかにカーソルを配置、 **受信** または **送信** ポリシー テンプレートのセクションです。 レート制限ポリシーとクォータ ポリシーは inbound ポリシーなので、カーソルを inbound 要素に置きます。

![ポリシー エディター][api-management-policy-editor-inbound]

2 つのポリシーを追加このチュートリアルでは、 [呼び出しレート制限][] と [使用量クォータを設定する][] ポリシーです。

![ポリシー ステートメント][api-management-limit-policies]

カーソルを配置した後、 **受信** ポリシー要素の横にある矢印をクリックして **呼び出しレート制限** そのポリシー テンプレートを挿入します。

    <rate-limit calls="number" renewal-period="seconds">
    <api name="name" calls="number">
    <operation name="name" calls="number" />
    </api>
    </rate-limit>

**呼び出しレート制限** 製品レベルで使用することができ、API レベルや個々 の操作名レベルも使用できます。 製品レベルのみのポリシーの使用このチュートリアルでは削除してください、 **api** と **操作** からの要素、 **レート制限** 要素でのみ外部 **レート制限** 要素は、次の例で示すようにします。

    <rate-limit calls="number" renewal-period="seconds">
    </rate-limit>

無料評価版成果物の最大許容される呼び出しレートは 1 分あたりの呼び出しを 10 回のように入力 **10** の値として、 **呼び出し** 属性、および **60** の **renewal-period 中** 属性です。

    <rate-limit calls="10" renewal-period="60">
    </rate-limit>

構成する、 **使用量クォータを設定する** ポリシー、新しく追加されたのすぐ下にカーソルを置きます **レート制限** 内の要素、 **受信** 要素の左側にある矢印をクリックし、 **使用量クォータを設定する**です。

    <quota calls="number" bandwidth="kilobytes" renewal-period="seconds">
    <api name="name" calls="number" bandwidth="kilobytes">
    <operation name="name" calls="number" bandwidth="kilobytes" />
    </api>
    </quota>

このポリシーは、製品レベルで適用するためのものでも、ので、削除、 **api** と **操作** 次の例で示すように、要素の名前します。

    <quota calls="number" bandwidth="kilobytes" renewal-period="seconds">
    </quota>

クォータは、一定時間あたりの呼び出し回数、帯域幅、またはその両方を基準にすることができます。 このチュートリアルでは調整は行いませんに基づいて帯域幅、削除、 **帯域幅** 属性です。

    <quota calls="number" renewal-period="seconds">
    </quota>

この無料試用版の成果物には、呼び出し回数を 1 週間あたり 200 回とするクォータを割り当てます。 指定 **200** の値として、 **呼び出し** 属性があり、指定 **604800** の値として、 **renewal-period 中** 属性です。

    <quota calls="200" renewal-period="604800">
    </quota>

>ポリシーの間隔は秒単位で指定します。 1 週間の秒数は、日数 (7) に 1 日の時間数 (24)、1 時間 (60) の分数、1 分間の秒数 (60) を掛けて求めることができます (7 * 24 * 60 * 60 = 604800)。

完成したポリシーは、次のようになります。

    <policies>
        <inbound>
            <rate-limit calls="10" renewal-period="60">
            </rate-limit>
            <quota calls="200" renewal-period="604800">
            </quota>
            <base />

    </inbound>
    <outbound>

        <base />

        </outbound>
    </policies>

必要なポリシーが構成され、クリックして **保存**します。

![ポリシーの保存][api-management-policy-save]

## <a name="publish-product"> </a> 成果物を発行するには

API を追加し、ポリシーを構成したら、成果物を開発者が使用できるように発行する必要があります。 をクリックして **製品** から、 **API Management** 、左側のメニューをクリックし、 **無料評価版** 製品を構成します。

![成果物の構成][api-management-configure-product]

をクリックして **発行**, 、順にクリック **はい、その発行** を確認します。

![成果物の発行][api-management-publish-product]

## <a name="subscribe-account"> </a>成果物のサブスクリプションを開発者アカウントに追加するには

発行された成果物は、開発者がサブスクライブして使用できます。

>API Management インスタンスの管理者には、すべての成果物に対するサブスクリプションが自動的に設定されます。 このチュートリアル ステップでは、いずれかの開発者アカウント (管理者以外) に、無料試用版の成果物のサブスクリプションを追加します。 ご利用の開発者アカウントが Administrators ロールに属し、既にサブスクリプションが存在する場合でも、この手順に従ってかまいません。

クリックして **ユーザー** 上、 **API Management** 、左側のメニューし、開発者アカウントの名前をクリックします。 使用してこの例では、 **Clayton Gragg** 開発者アカウント。

![開発者の構成][api-management-configure-developer]

クリックして **サブスクリプションを追加する**です。

![サブスクリプションの追加][api-management-add-subscription-menu]

選択 **無料評価版**, 、] をクリックし、 **購読**します。

![サブスクリプションの追加][api-management-add-subscription]

>[AZURE.NOTE] このチュートリアルでは、無料評価版成果物の複数の同時サブスクリプションは使用できません。 有効になっている場合は、次の例に示すように、サブスクリプションに名前を付けるように促されます。

![サブスクリプションの追加][api-management-add-subscription-multiple]

クリックすると **購読**, に製品が表示されます、 **サブスクリプション** 、ユーザーの一覧です。

![サブスクリプションを追加しました][api-management-subscription-added]

## <a name="test-rate-limit"> </a>操作を呼び出してレート制限をテストするには

無料試用版成果物の構成と発行は以上で完了です。今度は、いくつかの操作を呼び出して、レート制限ポリシーをテストしてみましょう。
クリックして、開発者ポータルに切り替えて **デベロッパー ポータル** ] メニューの右上にします。

![開発者ポータル][api-management-developer-portal-menu]

クリックして **Api** 上部のメニューをクリックし **Echo API**します。

![開発者ポータル][api-management-developer-portal-api-menu]

クリックして **GET Resource**, 、] をクリックし、 **コンソールを開く**します。

![コンソールを開く][api-management-open-console]

既定のパラメーターの値はそのままにし、対象となる無料試用版の成果物のサブスクリプション キーを選択します。

![サブスクリプション キー][api-management-select-key]

>[AZURE.NOTE] 複数のサブスクリプションがあれば、必ずのキーを選択する **無料評価版**, 、前の手順で構成されたポリシーを有効になりませんそれ以外の場合。

クリックして **HTTP Get**, 、応答を表示します。 注、 **応答ステータス** の **200 OK**します。

![操作の結果][api-management-http-get-results]

クリックして **HTTP Get** 1 分あたり 10 個の呼び出しのレート制限ポリシーを超える頻度でします。 レート制限ポリシーを超過すると後の応答のステータス **が多すぎる要求 429** が返されます。

![操作の結果][api-management-http-get-429]

 **応答ヘッダー** と **応答内容** 再試行が成功するまで、領域が残りの時間を指定します。

1 分間に 10 回という呼び出しレート制限ポリシーが有効な場合、レート制限超過となった連続 10 回の呼び出しの 1 回目から 60 秒が経過するまで、その成果物に対する以降の呼び出しはエラーになります。 この例の場合、残り時間は 43 秒です。

## <a name="next-steps"> </a>次のステップ

-   [その他のトピックもチェックして、 [高度な API の構成を使ってみる][] チュートリアルです。
-   詳細およびレート制限とクォータの設定のデモについては、次のビデオをご覧ください。

> [AZURE.VIDEO rate-limits-and-quotas]


[api-management-management-console]: ./media/api-management-howto-product-with-rules/api-management-management-console.png
[api-management-add-product]: ./media/api-management-howto-product-with-rules/api-management-add-product.png
[api-management-new-product-window]: ./media/api-management-howto-product-with-rules/api-management-new-product-window.png
[api-management-product-added]: ./media/api-management-howto-product-with-rules/api-management-product-added.png
[api-management-add-policy]: ./media/api-management-howto-product-with-rules/api-management-add-policy.png
[api-management-policy-editor-inbound]: ./media/api-management-howto-product-with-rules/api-management-policy-editor-inbound.png
[api-management-limit-policies]: ./media/api-management-howto-product-with-rules/api-management-limit-policies.png
[api-management-policy-save]: ./media/api-management-howto-product-with-rules/api-management-policy-save.png
[api-management-configure-product]: ./media/api-management-howto-product-with-rules/api-management-configure-product.png
[api-management-add-api]: ./media/api-management-howto-product-with-rules/api-management-add-api.png
[api-management-add-echo-api]: ./media/api-management-howto-product-with-rules/api-management-add-echo-api.png
[api-management-developer-portal-menu]: ./media/api-management-howto-product-with-rules/api-management-developer-portal-menu.png
[api-management-publish-product]: ./media/api-management-howto-product-with-rules/api-management-publish-product.png
[api-management-configure-developer]: ./media/api-management-howto-product-with-rules/api-management-configure-developer.png
[api-management-add-subscription-menu]: ./media/api-management-howto-product-with-rules/api-management-add-subscription-menu.png
[api-management-add-subscription]: ./media/api-management-howto-product-with-rules/api-management-add-subscription.png
[api-management-developer-portal-api-menu]: ./media/api-management-howto-product-with-rules/api-management-developer-portal-api-menu.png
[api-management-open-console]: ./media/api-management-howto-product-with-rules/api-management-open-console.png
[api-management-http-get]: ./media/api-management-howto-product-with-rules/api-management-http-get.png
[api-management-http-get-results]: ./media/api-management-howto-product-with-rules/api-management-http-get-results.png
[api-management-http-get-429]: ./media/api-management-howto-product-with-rules/api-management-http-get-429.png
[api-management-product-policy]: ./media/api-management-howto-product-with-rules/api-management-product-policy.png
[api-management-add-developers-group]: ./media/api-management-howto-product-with-rules/api-management-add-developers-group.png
[api-management-select-key]: ./media/api-management-howto-product-with-rules/api-management-select-key.png
[api-management-subscription-added]: ./media/api-management-howto-product-with-rules/api-management-subscription-added.png
[api-management-add-subscription-multiple]: ./media/api-management-howto-product-with-rules/api-management-add-subscription-multiple.png

[How to add operations to an API]: api-management-howto-add-operations.md
[How to add and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: ../api-management-monitoring.md
[Add APIs to a product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Get started with Azure API Management]: api-management-get-started.md
[How to create and use groups in Azure API Management]: api-management-howto-create-groups.md
[View subscribers to a product]: api-management-howto-add-products.md#view-subscribers
[Get started with Azure API Management]: api-management-get-started.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance
[Next steps]: #next-steps

[Create a product]: #create-product
[Configure call rate limit and quota policies]: #policies
[Add an API to the product]: #add-api
[Publish the product]: #publish-product
[Subscribe a developer account to the product]: #subscribe-account
[Call an operation and test the rate limit]: #test-rate-limit
[Get started with advanced API configuration]: api-management-get-started-advanced.md

[Limit call rate]: https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRate
[Set usage quota]: https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuota


