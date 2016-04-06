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

#API Management とは

API Management が組織にもたらす利点は、外部のパートナーや社内の開発者に API を公開することによって、社内に眠っているデータやサービスの可能性を発掘できることです。 どの企業も、その業務をデジタル プラットフォームで拡大し、新しい販路と顧客を開拓すると共に、既存の顧客との絆を深めようと模索しています。 API Management は、開発者の取り組み、ビジネス インサイト、分析、セキュリティ、保護を通じて API プログラムの価値を高め、企業にコア コンピテンシーをもたらします。

> [AZURE.VIDEO microsoft-ignite-2015-azure-api-management-and-the-api-economy]

API Management を使用するには、管理者が API を作成します。 API はそれぞれ、少なくとも 1 つの操作で構成され、1 つまたは複数の成果物に追加することができます。 API を使用する開発者は、その API を含んだ成果物をサブスクライブしたうえで、使用ポリシーが適用されていればその範囲の中で、API の操作を呼び出すことができます。

このトピックでは、API Management の重要な概念を簡単に説明します。

>[AZURE.NOTE] 詳細については、次を参照してください。、 [クラウド ベースの API Management: Api の電源を利用する](http://j.mp/ms-apim-whitepaper) に関する PDF ホワイト ペーパー。 CITO Research 社が作成した API Management に関するこの概要ホワイト ペーパーでは、次のトピックが取り上げられています。 
>
> - API の一般的な要件と課題
>     - API の分離とファサードの提供
>     - 開発者の準備時間の短縮
>     - アクセスのセキュリティ保護
>     - 分析とメトリック
> - API Management プラットフォームを使用した制御と把握
> - クラウド ソリューションとオンプレミス ソリューションの利用の違い
> - Azure API Management

## <a name="apis"> </a>API と操作

API Management サービス インスタンスの基礎となるのは API です。 各 API では、一連の開発者が利用できる操作を表します。 各 API は、それを実装するバックエンド サービスへの参照を含んでおり、その操作は、バックエンド サービスに実装されている操作と対応します。 操作は、API Management で細かく設定することができます。URL のマッピング、クエリとパスのパラメーター、要求と応答の内容、操作から返される応答のキャッシュを制御することが可能です。 また、レート制限、クォータ、IP 制限のポリシーを API レベルや個々の操作レベルで導入することもできます。

詳細については、次を参照してください。 [Api を作成する方法][] と [API に操作を追加する方法][]します。


## <a name="products"> </a> 成果物

開発者から見える API の全体像が成果物です。 API Management の成果物には、少なくとも 1 つの API が含まれており、タイトルや説明、使用条件などが設定されます。 製品 **開いている** または **Protected**します。 保護された成果物を使用するには、事前にサブスクライブする必要があります。一方、オープンな成果物は、サブスクライブせずに使用できます。 開発者に使用してもらう準備が整ったら、成果物を発行することができます。 発行された成果物は、開発者が見ることができます (保護された成果物の場合は、開発者がサブスクライブできるようになります)。 サブスクリプションの承認は成果物レベルで行われ、管理者による承認を必須とするか、自動的に承認するかを選ぶことができます。

開発者に成果物の表示を許可するかどうかは、グループを使用して管理します。 成果物の表示の可否はグループに対して付与されます。開発者は、自分が所属するグループから見える成果物を表示してサブスクライブすることができます。 

詳細については、次を参照してください。 [を作成し、成果物の発行方法][] と、次のビデオです。

> [AZURE.VIDEO using-products]

## <a name="groups"> </a> グループ

開発者に成果物の表示を許可するかどうかは、グループを使用して管理します。 API Management には、次に示すシステム グループが用意されています。これらのシステム グループを変更することはできません。

-   **管理者** -Azure サブスクリプション管理者はこのグループのメンバーです。 管理者は、API Management サービス インスタンスを管理します。開発者が使用する API とその操作、成果物は、管理者が作成します。
-   **開発者** - 認証された開発者ポータル ユーザーはこのグループに分類します。 開発者は、API の利用者です。管理者によって作成された API を使用してアプリケーションを構築します。 開発者は、開発者ポータルへのアクセスが認められており、API の操作を呼び出すアプリケーションを構築します。
-   **ゲスト** -見込み客がこのグループに API Management インスタンスの年秋の開発者ポータルへのアクセスなど、開発者ポータル ユーザーが認証されていません。 特定の読み取り専用アクセスを許可することができます (API の閲覧はできるが、呼び出すことはできないなど)。

これらのシステム グループに加えて、管理者がカスタム グループを作成できますか [外部グループが関連付けられている Azure Active Directory テナントを活用する](api-management-howto-aad.md/#how-to-add-an-external-azure-active-directory-group)です。 カスタム グループと外部グループをシステム グループと共に使用することにより、開発者には API 成果物の可視性とアクセスが提供されます。 たとえば、特定のパートナー企業に所属する開発者向けにカスタム グループを 1 つ作成し、関連する API のみが含まれている成果物の API に対してアクセスを許可することができます。 ユーザーは 複数のグループのメンバーになることができます。

詳細については、次を参照してください。  [を作成し、グループを使用する方法][]します。

## <a name="developers"> </a> 開発者

開発者は、API Management サービス インスタンス内のユーザー アカウントです。 開発者を作成または、管理者が参加するよう招待またはからサインインして、 [デベロッパー ポータル][]します。 それぞれの開発者はグループ (複数可) に所属し、そのグループに閲覧が認められている成果物のサブスクリプションを得ることができます。

成果物のサブスクリプションを持つ開発者には、その成果物へのプライマリ キーとセカンダリ キーが付与されます。 成果物の API を呼び出す際は、このキーを使用することになります。

詳細については、次を参照してください。 [を作成または開発者を招待する方法][] と [グループを開発者に関連付ける方法][]します。

## <a name="policies"> </a> ポリシー

ポリシーは、発行者がその構成を通じて API の動作を変更できる、API Management の強力な機能の 1 つです。 API の要求または応答に対して順に実行される一連のステートメントが集まってポリシーが形成されます。 代表的なステートメントとしては、XML 形式から JSON 形式への変換や、(開発者からの呼び出しの回数を制限する) 呼び出しレート制限が挙げられ、他にも数多くのポリシーが利用できます。

ポリシーの式は、ポリシーで特に指定されていない限り、任意の API Management ポリシーで属性値またはテキスト値として使用できます。 などのいくつかのポリシー、 [制御フロー](https://msdn.microsoft.com/library/azure/dn894085.aspx#choose) と [変数の設定](https://msdn.microsoft.com/library/azure/dn894085.aspx#set-variable) ポリシーはポリシー式に基づいています。 詳細については、次を参照してください。 [ポリシーを高度な](https://msdn.microsoft.com/library/azure/dn894085.aspx#AdvancedPolicies), 、[ポリシー式](https://msdn.microsoft.com/library/azure/dn910913.aspx), 、次のビデオを見るとします。

> [AZURE.VIDEO policy-expressions-in-azure-api-management]

API Management のポリシーの一覧については、次を参照してください。 [ポリシーのリファレンス][]します。 使用して、ポリシーの構成の詳細については、次を参照してください。 [API Management ポリシー][]します。 レート制限とクォータ ポリシー成果物の作成に関するチュートリアルでは、次を参照してください。 [を作成して、高度な製品設定を構成する方法][]します。 デモについては、次のビデオをご覧ください。

> [AZURE.VIDEO rate-limits-and-quotas]

## <a name="developer-portal"> </a> 開発者ポータル

開発者ポータルは、開発者が API の使用法を習得したり、操作を確認して呼び出したり、成果物をサブスクライブしたりすることができる場です。 利用予定者は、開発者ポータルにアクセスして、API と操作を閲覧したうえで、サインアップすることができます。 開発者ポータルの URL は、API Management サービス インスタンスの Azure クラシック ポータルのダッシュボードに配置されます。

カスタム コンテンツを追加したり、スタイルをカスタマイズしたり、貴社のブランドを追加したりすることによって、開発者ポータルのルック アンド フィールをカスタマイズすることができます。

[APIs and operations]: #apis
[Products]: #products
[Groups]: #groups
[Developers]: #developers
[Policies]: #policies
[Developer portal]: #developer-portal

[How to create APIs]: api-management-howto-create-apis.md
[How to add operations to an API]: api-management-howto-add-operations.md
[How to create and publish a product]: api-management-howto-add-products.md
[How to create and use groups]: api-management-howto-create-groups.md
[How to associate groups with developers]: api-management-howto-create-groups.md#associate-group-developer
[How create and configure advanced product settings]: api-management-howto-product-with-rules.md
[How to create or invite developers]: api-management-howto-create-or-invite-developers.md
[Policy reference]: api-management-policy-reference.md
[API Management policies]: api-management-howto-policies.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance



 

