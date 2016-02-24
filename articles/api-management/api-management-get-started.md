<properties
    pageTitle="Azure API Management での最初の API の管理 | Microsoft Azure"
    description="API の作成方法、操作の追加方法、API Management の基本操作について説明します。"
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
    ms.topic="hero-article"
    ms.date="12/07/2015"
    ms.author="sdanie"/>

# Azure API Management での最初の API の管理

## <a name="overview"> </a>概要

このガイドでは、Azure API Management の基本的な使用方法を簡単に説明し、実際に API を呼び出します。

## <a name="concepts"> </a>Azure API Management とは

Azure API Management を任意のバックエンドで実行し、それに基づいて本格的な API プログラムを起動できます。

一般的なシナリオは、次のとおりです。

* **モバイル インフラストラクチャのセキュリティ保護** API キーを持つアクセスをゲートでは、調整、または JWT トークンの検証などの高度なセキュリティ ポリシーを使用してを使用して DOS 攻撃を防止します。
* **ISV パートナー エコシステムを有効にする** 開発者ポータルを通してパートナーの高速オンボードを提供することにより、パートナー使用の整っていない内部実装から分離する API ファサードを構築します。
* **内部 API プログラムを実行している** 組織が可用性と Api に対する最新の変更に関して通信するための一元的な場所を提供することにより、組織のアカウントに基づいてアクセスをゲートすべてに基づいて API ゲートウェイとバックエンド間でセキュリティで保護されたチャネル。


システムは、次のコンポーネントで構成されます。

*  **API ゲートウェイ** エンドポイントです。
  * API 呼び出しを受け入れ、バックエンドにルーティングします。
  * API キー、JWT トークン、証明書、その他の資格情報を検証します。
  * 使用量クォータとレート制限を適用します
  * コードを変更せずにその場で API を変換します。
  * セットアップ時にバックエンドの応答をキャッシュします。
  * 分析目的で呼び出しメタデータを記録します。

*  **パブリッシャー ポータル** API プログラムを設定する管理インターフェイスです。 このインターフェイスを使用して、次の操作を行います。
    * API スキーマを定義またはインポートします。
    * API を製品にパッケージします。
    * API のクォータや変換などのポリシーを設定します。
    * 分析から洞察を得ます。
    * ユーザーを管理します。

*  **デベロッパー ポータル** 機能メイン web プレゼンスとして開発者にとっては、それらことができます。
    * API のドキュメントを読みます。
    * 対話型コンソールを使用して API を試します。
    * API キーを取得するために、アカウントを作成してサブスクライブします。
    * 自分自身の使用に関する分析にアクセスします。


## <a name="create-service-instance"> </a>API Management インスタンスの作成

>[AZURE.NOTE] このチュートリアルを完了するには、Azure アカウントが必要です。 アカウントがない場合は、無料試用版のアカウントを数分で作成することができます。 詳細については、[Azure の無料試用版サイト][]をご覧ください。

API Management を使用するにあたって最初に行うことは、サービス インスタンスの作成です。 サインイン、 [Azure Classic Portal][] ] をクリック **新規**, 、**App Services**, 、**API Management**, 、**作成**します。

![API Management の新しいインスタンス][api-management-create-instance-menu]

 **URL**, 、サービス URL に使用する一意のサブドメイン名を指定します。

必要な選択 **サブスクリプション** と **地域** サービス インスタンスのです。 選択内容をしたら、クリックして、 **次** ] ボタンをクリックします。

![新しい API Management サービス][api-management-create-instance-step1]

入力 **Contoso Ltd.** の **組織名**, に電子メール アドレスを入力し、 **管理者の電子メール** フィールドです。

>[AZURE.NOTE] この電子メール アドレスは、API Management システムからの通知に使用されます。 詳細については、次を参照してください。 [と Azure API Management でテンプレートを電子メール通知を構成する方法][]します。

![新しい API Management サービス][api-management-create-instance-step2]

API Management サービス インスタンスは、Developer、Standard、Premium の 3 つのレベルで利用できます。 既定では、新しい API Management サービス インスタンスは、Developer レベルで作成されます。 Standard または Premium 階層を選択する、 **詳細設定** チェック ボックスをオンし、次の画面で希望のレベルを選択します。

>[AZURE.NOTE] Developer レベルでは、開発、テスト、および高可用性が問題ではない、パイロット API プログラム用です。 Standard レベルと Premium レベルでは、より多くのトラフィックを処理するために予約ユニット数を拡張できます。 Standard レベルと Premium レベルでは、最も高い処理能力とパフォーマンスを備えた、API Management サービスが提供されます。 このチュートリアルは、どのレベルを使用しても完了できます。 API Management レベルの詳細については、次を参照してください。 [API Management の料金][]します。

チェック ボックスをクリックすると、サービス インスタンスが作成されます。

![新しい API Management サービス][api-management-instance-created]

サービス インスタンスが作成されたら、次は、API の作成またはインポートを行います。

## <a name="create-api"> </a>API のインポート

API は、クライアント アプリケーションから呼び出すことのできる一連の操作で構成されます。 API の操作は、既存の Web サービスに引き渡されます。

API は、手動で作成して操作を追加することも、インポートすることもできます。 このチュートリアルでは、Microsoft によって提供され、Azure でホストされる、サンプル電卓 Web サービスの API をインポートします。

>[AZURE.NOTE] API を作成して操作を手動で追加する方法については、次を参照してください。 [Api を作成する方法](api-management-howto-create-apis.md) と [API に操作を追加する方法](api-management-howto-add-operations.md)します。

API の構成は、Azure クラシック ポータルから発行者ポータルにアクセスして行います。 クリックして、パブリッシャー ポータルにアクセス **管理** 、API Management サービスの Azure クラシック ポータルで。

![パブリッシャー ポータル][api-management-management-console]

電卓 API をインポートする] をクリックして **Api** から、 **API Management** 、左側のメニューをクリックし、 **API のインポート**します。

![API ボタンのインポート][api-management-import-api]

電卓 API を構成するには、次の手順を実行します。

1. をクリックして **URL から**, 、入力 **http://calcapi.cloudapp.net/calcapi.json** に、 **仕様ドキュメント URL** テキスト ボックス、およびクリック、 **Swagger** オプション ボタンをクリックします。
2. 型 **calc** に、 **Web API URL サフィックス** テキスト ボックスです。
3. 内のクリックして、 **製品 (オプション)** ] を選択 **スターター**します。
4. クリックして **保存** 、API をインポートします。

![新しい API を追加する][api-management-import-new-api]

API がインポートされると、API の概要ページがパブリッシャー ポータルに表示されます。

![API の概要][api-management-imported-api-summary]

API セクションにはいくつかのタブがあります。  **概要** ] タブには、基本的なメトリックと API に関する情報が表示されます。  [設定](api-management-howto-create-apis.md#configure-api-settings) API の構成を表示および編集] タブを使用します。  [操作](api-management-howto-add-operations.md) タブは、API の操作を管理するために使用します。  **セキュリティ** 基本認証を使用してバックエンド サーバー用のゲートウェイ認証を構成する] タブを使用できますか [相互証明書認証](api-management-howto-mutual-certificates.md), 、し構成する [OAuth 2.0 を使用してユーザーの承認](api-management-howto-oauth2.md)します。   **問題** をご利用の Api を使用して、開発者によって報告された問題を表示] タブを使用します。  **製品** タブは、この API が含まれる製品を構成するために使用します。

すべての API Management インスタンスは、2 つのサンプル成果物を既定で備えています。

-   **スターター**
-   **無制限**

このチュートリアルでは、API がインポートされたときに Basic Calculator API がスターター製品に追加されました。

API を呼び出すためには、その API へのアクセスを提供する成果物を開発者が事前にサブスクライブする必要があります。 開発者は、開発者ポータルで成果物にサブスクライブすることができます。また管理者がパブリッシャー ポータルで、開発者を成果物にサブスクライブすることもできます。 このチュートリアルで先ほど API Management インスタンスを作成したので、管理者になっています。既定で、既にすべての製品にサブスクライブしていることになります。

## <a name="call-operation"> </a>開発者ポータルから操作を呼び出す

開発者ポータルには、API の操作を見てテストするための便利な環境が用意されており、操作を直接呼び出すことができます。 このチュートリアルの手順では、Basic Calculator API を呼び出します **2 つの整数を追加** 操作します。 クリックして **デベロッパー ポータル** 上にあるメニューから、パブリッシャー ポータルの右です。

![開発者ポータル][api-management-developer-portal-menu]

クリックして **Api** をクリックし、上部のメニューから **Basic Calculator** 利用可能な操作を表示します。

![開発者ポータル][api-management-developer-portal-calc-api]

API および操作とともにサンプルの説明とパラメーターがインポートされることに注意してください。これは、この操作を使用する開発者のためのドキュメントとして提供されます。 操作を手動で追加するときに、これらの説明の追加もできます。

呼び出す、 **2 つの整数を追加** 操作をクリックして **やって**します。

![試してみる][api-management-developer-portal-calc-api-console]

パラメーターのいくつかの値を入力または、既定を保持し、] をクリックし、 **送信**します。

![HTTP Get][api-management-invoke-get]

操作が呼び出されると、開発者ポータルに表示されます、 **応答ステータス**, 、 **応答ヘッダー**, 、および **応答内容**します。

![Response][api-management-invoke-get-response]

## <a name="view-analytics"> </a>分析結果の表示

基本的な電卓の分析結果を表示する、パブリッシャー ポータルに戻って選択して切り替える **管理** 上にあるメニューから、開発者ポータルの右です。

![管理][api-management-manage-menu]

パブリッシャー ポータルの既定のビューは、 **ダッシュ ボード**, 、API Management インスタンスの概要を提供します。

![ダッシュボード][api-management-dashboard]

グラフの上にマウスを置いた **Basic Calculator** に特定の期間中は、API の使用方法については、特定のメトリックを参照してください。

>[AZURE.NOTE] 場合は、グラフに線が表示されない、開発者ポータルに戻りと API を呼び出し、しばらく待ってから、およびダッシュ ボードに戻る。

をクリックして **の詳細を表示** を表示されたメトリックの拡大版を含む、API の概要] ページを表示します。

![分析][api-management-mouse-over]

![概要][api-management-api-summary-metrics]

詳細なメトリックとレポートをクリックして **Analytics** から、 **API Management** 左側のメニュー。

![概要][api-management-analytics-overview]

 **Analytics** セクションが次の 4 つのタブには。

-   **ひとめで** 全体的な使用状況と正常性のメトリック、だけでなく開発者、成果物、Api、および上位の操作を提供します。
-   **使用状況** API 呼び出しと帯域幅、地理的な表現を含むの詳細を提供します。
-   **正常性** 重点を置いていますステータス コード、キャッシュの成功率、応答時間、API とサービスの応答時間。
-   **アクティビティ** 開発者、製品、API、および操作で特定のアクティビティにドリル ダウン レポートを提供します。

## <a name="next-steps"> </a>次のステップ

-   [その他のトピックもチェックして、 [高度な API の構成を使ってみる][] チュートリアルです。

[Azure Free Trial]: http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=api_management_hero_a

[Create an API Management instance]: #create-service-instance
[Create an API]: #create-api
[Add an operation]: #add-operation
[Add the new API to a product]: #add-api-to-product
[Subscribe to the product that contains the API]: #subscribe
[Call an operation from the Developer Portal]: #call-operation
[View analytics]: #view-analytics
[Next steps]: #next-steps


[How to manage developer accounts in Azure API Management]: api-management-howto-create-or-invite-developers.md
[Configure API settings]: api-management-howto-create-apis.md#configure-api-settings
[How to configure notifications and email templates in Azure API Management]: api-management-howto-configure-notifications.md
[Responses]: api-management-howto-add-operations.md#responses
[How create and publish a product]: api-management-howto-add-products.md
[Get started with advanced API configuration]: api-management-get-started-advanced.md
[API Management pricing]: http://azure.microsoft.com/pricing/details/api-management/

[Azure Classic Portal]: https://manage.windowsazure.com/

[api-management-management-console]: ./media/api-management-get-started/api-management-management-console.png
[api-management-create-instance-menu]: ./media/api-management-get-started/api-management-create-instance-menu.png
[api-management-create-instance-step1]: ./media/api-management-get-started/api-management-create-instance-step1.png
[api-management-create-instance-step2]: ./media/api-management-get-started/api-management-create-instance-step2.png
[api-management-instance-created]: ./media/api-management-get-started/api-management-instance-created.png
[api-management-import-api]: ./media/api-management-get-started/api-management-import-api.png
[api-management-import-new-api]: ./media/api-management-get-started/api-management-import-new-api.png
[api-management-imported-api-summary]: ./media/api-management-get-started/api-management-imported-api-summary.png
[api-management-calc-operations]: ./media/api-management-get-started/api-management-calc-operations.png
[api-management-list-products]: ./media/api-management-get-started/api-management-list-products.png
[api-management-add-api-to-product]: ./media/api-management-get-started/api-management-add-api-to-product.png
[api-management-add-myechoapi-to-product]: ./media/api-management-get-started/api-management-add-myechoapi-to-product.png
[api-management-api-added-to-product]: ./media/api-management-get-started/api-management-api-added-to-product.png
[api-management-developers]: ./media/api-management-get-started/api-management-developers.png
[api-management-add-subscription]: ./media/api-management-get-started/api-management-add-subscription.png
[api-management-add-subscription-window]: ./media/api-management-get-started/api-management-add-subscription-window.png
[api-management-subscription-added]: ./media/api-management-get-started/api-management-subscription-added.png
[api-management-developer-portal-menu]: ./media/api-management-get-started/api-management-developer-portal-menu.png
[api-management-developer-portal-calc-api]: ./media/api-management-get-started/api-management-developer-portal-calc-api.png
[api-management-developer-portal-calc-api-console]: ./media/api-management-get-started/api-management-developer-portal-calc-api-console.png
[api-management-invoke-get]: ./media/api-management-get-started/api-management-invoke-get.png
[api-management-invoke-get-response]: ./media/api-management-get-started/api-management-invoke-get-response.png
[api-management-manage-menu]: ./media/api-management-get-started/api-management-manage-menu.png
[api-management-dashboard]: ./media/api-management-get-started/api-management-dashboard.png

[api-management-add-response]: ./media/api-management-get-started/api-management-add-response.png
[api-management-add-response-window]: ./media/api-management-get-started/api-management-add-response-window.png
[api-management-developer-key]: ./media/api-management-get-started/api-management-developer-key.png
[api-management-mouse-over]: ./media/api-management-get-started/api-management-mouse-over.png
[api-management-api-summary-metrics]: ./media/api-management-get-started/api-management-api-summary-metrics.png
[api-management-analytics-overview]: ./media/api-management-get-started/api-management-analytics-overview.png
[api-management-analytics-usage]: ./media/api-management-get-started/api-management-analytics-usage.png
[api-management-]: ./media/api-management-get-started/api-management-.png
[api-management-]: ./media/api-management-get-started/api-management-.png

