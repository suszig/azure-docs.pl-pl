<properties 
    pageTitle="Azure API Management で API に操作を追加する方法" 
    description="Azure API Management で API に操作を追加する方法について説明します。" 
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

# Azure API Management で API に操作を追加する方法

API Management 内の API を使用するためには、操作を追加する必要があります。 このガイドでは、API Management 内の API に各種の操作を追加して構成する方法を説明します。

## <a name="add-operation"> </a>操作の追加

操作を API に追加して構成するには、パブリッシャー ポータルを使用します。 パブリッシャー ポータルにアクセスするには、クリックして **管理** 、API Management サービスの Azure クラシック ポータルで。

![パブリッシャー ポータル][api-management-management-console]

>API Management サービス インスタンスを作成がない場合は、次を参照してください。 [API Management サービス インスタンスの作成][] で、 [Azure API Management を使ってみる][] チュートリアルです。

パブリッシャー ポータルで、必要な API を選択し、選択、 **操作** ] タブをクリックします。 

![操作][api-management-operations]

クリックして **操作の追加** 新しい操作を追加します。  **新しい操作** が表示されます、 **署名** タブが既定で選択されます。

![追加操作][api-management-add-operation]

指定、 **HTTP 動詞** ドロップ ダウン リストから選択します。

![HTTP メソッド][api-management-http-method]

1 つ以上の URL パス セグメントと 0 個以上のクエリ文字列パラメーターから構成される URL フラグメントを入力して、URL テンプレートを定義します。 URL テンプレートは、API のベース URL に付加され、単一の HTTP 操作を識別します。 URL テンプレートには、中かっこで識別される 1 つまたは複数の名前付き変数部分を含めることができます。 テンプレート パラメーターと呼ばれるこれらの変数部分には、要求が API Management プラットフォームによって処理されるときに要求の URL から抽出された値が動的に割り当てられます。

![URL テンプレート][api-management-url-template]

必要な場合は、指定、 **書き換え URL テンプレート**します。 これにより、標準 URL テンプレートを使用してフロントエンドで受け取った要求を処理する一方で、書き換えテンプレートに従って変換された URL を介してバックエンドを呼び出すことができます。 書き換えテンプレートでは、URL テンプレートのテンプレート パラメーターを使用する必要があります。 次の例に、前の例の Web サービスでパス セグメントとしてエンコードされるコンテンツ タイプを、URL テンプレートを使用して API Management プラットフォーム経由で発行される API のクエリ パラメーターとして渡す方法を示します。

![URL テンプレートの再書き込み][api-management-url-template-rewrite]

操作の呼び出し元は、`/customers?customerid=ALFKI` という形式を使用します。この値は、バックエンド サービスが起動されたときに `/Customers('ALFKI')` にマップされます。


**表示** 名および **説明** 操作の説明を提供し、ドキュメントを開発者ポータルでこの API を使用している開発者に提供するために使用します。

![説明][api-management-description]

操作の説明はプレーン テキストまたは html 形式で指定することができます、 **説明** テキスト ボックスです。

## <a name="operation-caching"> </a>操作のキャッシュ

応答のキャッシュを使用すると、API コンシューマーによって認識される遅延が小さくなります。さらに、帯域幅の消費が減り、API を実装する HTTP Web サービスの負荷が小さくなります。 

簡単かつ迅速には、操作のキャッシュを有効にする、選択、 **キャッシュ** ] タブで確認し、 **を有効にする** チェック ボックスをオンします。

![キャッシュ][api-management-caching-tab]

**期間** 操作の応答のキャッシュに保持する期間を指定します。 既定値は、3,600 秒 (1 時間) です。

キャッシュ キーは、それぞれ異なるキャッシュ キーに対応する応答が別個のキャッシュ値を受け取るように、応答を区別するために使用されます。 必要に応じて、特定のクエリ文字列パラメーターとキャッシュ キー値の計算で使用する HTTP ヘッダーを入力、 **クエリ文字列パラメーターごとにキャッシュ** と **ヘッダーごとにキャッシュ** それぞれのテキスト ボックスです。 いずれも指定すると、すべての要求の URL と HTTP ヘッダーの値がキャッシュ キーの生成に使用される: **Accept** と **Accept-charset**します。

>キャッシュおよびキャッシュ ポリシーの詳細については、次を参照してください。 [操作をキャッシュする方法については、Azure API Management で結果][]します。


## <a name="request-parameters"> </a>要求パラメーター

操作のパラメーターは、[パラメーター] タブで管理します。 指定されたパラメーター、 **URL テンプレート** 上、 **署名** ] タブは自動的に追加し、変更するには、URL テンプレートの編集をします。 追加のパラメーターは手動で入力できます。

新しいクエリ パラメーターを追加する] をクリックして **クエリ パラメーターの追加** し、次の情報を入力します。

-   **名前** -パラメーター名。
-   **説明** -(省略可能) パラメーターの簡単な説明です。
-   **型** -パラメーターの型を使用して、ドロップダウンの一覧から選択します。
-   **値** -このパラメーターに割り当てることのできる値です。 値の 1 つを既定値としてマークすることができます (オプション)。
-   **必要な** -チェック ボックスにより、パラメーターを必須にします。 

![要求パラメーター][api-management-request-parameters]

## <a name="request-body"> </a>Request body

操作 (PUT、POST など) で本文が許可される場合や本文が必要になる場合は、すべてのサポートされている表現形式 (json、XML など) でその例を示すことができます。 

>要求本文はドキュメント化の目的でのみ使用され、検証されません。

要求本文を入力するに切り替えます、 **本文** ] タブをクリックします。

クリックして **表現の追加**, 、目的のコンテンツ タイプの名前 (application/json) の入力を開始、ドロップダウン リストで選択および選択した形式で目的の要求本文の例をテキスト ボックスに貼り付けます。 

![Request body][api-management-request-body]

表現を追加指定することも、省略可能なテキストの説明を **説明** テキスト ボックスです。

## <a name="responses"> </a>応答

操作の結果として考えられるすべての状態コードの応答例を提供するようお勧めします。 たとえば、状態コードごとに、対応する応答本文の例を複数 (サポートされるコンテンツ タイプごとに 1 つ) 設けます。 

応答を追加するには、次のようにクリックします。 **追加** 目的の状態コードを入力して表示します。 この例では、ステータス コード **200 OK**します。 ドロップダウン リストにコードが表示されたら、目的のコードを選択します。応答コードが作成され、操作に追加されます。

![応答コード][api-management-response-code]

クリックして **表現の追加**, 、目的のコンテンツ タイプの名前 (application/json) の入力を開始し、ドロップダウンを選択します。

![本分コンテンツの種類][api-management-response-body-content-type]

選択した形式の応答本文の例をボックスに貼り付けます。 

![応答本文][api-management-response-body]

必要に応じて、追加のオプションの説明に、 **説明** テキスト ボックスです。

クリックして、操作が構成されると、 **保存**します。


## <a name="next-steps"> </a>次のステップ

これで、操作が API に追加されました。次のステップでは、API を成果物に関連付けた後で発行して、開発者が操作を呼び出すことができるようにします。

-   [Azure API Management で成果物を作成して発行する方法][]

[api-management-management-console]: ./media/api-management-howto-add-operations/api-management-management-console.png
[api-management-operations]: ./media/api-management-howto-add-operations/api-management-operations.png
[api-management-add-operation]: ./media/api-management-howto-add-operations/api-management-add-operation.png
[api-management-http-method]: ./media/api-management-howto-add-operations/api-management-http-method.png
[api-management-url-template]: ./media/api-management-howto-add-operations/api-management-url-template.png
[api-management-url-template-rewrite]: ./media/api-management-howto-add-operations/api-management-url-template-rewrite.png
[api-management-description]: ./media/api-management-howto-add-operations/api-management-description.png
[api-management-caching-tab]: ./media/api-management-howto-add-operations/api-management-caching-tab.png
[api-management-request-parameters]: ./media/api-management-howto-add-operations/api-management-request-parameters.png
[api-management-request-body]: ./media/api-management-howto-add-operations/api-management-request-body.png
[api-management-response-code]: ./media/api-management-howto-add-operations/api-management-response-code.png
[api-management-response-body-content-type]: ./media/api-management-howto-add-operations/api-management-response-body-content-type.png
[api-management-response-body]: ./media/api-management-howto-add-operations/api-management-response-body.png


[api-management-contoso-api]: ./media/api-management-howto-add-operations/api-management-contoso-api.png

[api-management-add-new-api]: ./media/api-management-howto-add-operations/api-management-add-new-api.png
[api-management-api-settings]: ./media/api-management-howto-add-operations/api-management-api-settings.png
[api-management-api-settings-credentials]: ./media/api-management-howto-add-operations/api-management-api-settings-credentials.png
[api-management-api-summary]: ./media/api-management-howto-add-operations/api-management-api-summary.png
[api-management-echo-operations]: ./media/api-management-howto-add-operations/api-management-echo-operations.png

[Add an operation]: #add-operation
[Operation caching]: #operation-caching
[Request parameters]: #request-parameters
[Request body]: #request-body
[Responses]: #responses
[Next steps]: #next-steps

[Get started with Azure API Management]: api-management-get-started.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance

[How to add operations to an API]: api-management-howto-add-operations.md
[How to create and publish a product]: api-management-howto-add-products.md
[How to cache operation results in Azure API Management]: api-management-howto-cache.md
