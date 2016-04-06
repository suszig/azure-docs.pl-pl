<properties
    pageTitle="App Service API Apps - 変更内容 | Microsoft Azure"
    description="Azure App Service の API Apps の新機能について説明します。"
    services="app-service\api"
    documentationCenter=".net"
    authors="mohitsriv"
    manager="wpickett"
    editor="tdykstra"/>

<tags
    ms.service="app-service-api"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="11/29/2015"
    ms.author="mohisri"/>

# App Service API Apps - 変更内容

2015 年 11 月に Connect() イベント、多数の Azure App Service への改良でした [発表](https://azure.microsoft.com/blog/azure-app-service-updates-november-2015/)します。 この機能強化では、Mobile および Web Apps との適合性の向上、コンセプト カウントの抑制、デプロイおよび実行時のパフォーマンスの向上を実現するために基礎となる変更が API Apps に加えられました。 2015 年 11 月 30 日以降、Microsoft Azure 管理ポータルまたは最新のツールを使用して作成する新しい API アプリでは、これらの変更が反映されます。 この記事では、これらの変更内容について説明するほか、既存のアプリを再デプロイして、強化された機能を利用する方法を説明します。


> [AZURE.NOTE] API アプリの初期プレビューには、2 つの主なシナリオがサポートされている: 1) は、カスタム Api Logic Apps で使用するためのロジック アプリ、または独自のクライアントと 2) は、Marketplace API (多くの場合、SaaS コネクタ) で使用するためです。 この資料では、1 番目のシナリオ「カスタム API」を取り上げます。 Marketplace API の場合、機能強化された Logic Apps デザイナー エクスペリエンスと基になる接続基盤は 2016 年の早い時期に導入されます。 既存の Marketplace APIs は引き続き Logic Apps デザイナーで使用できます。

## 機能の変更
API Apps の主な機能である認証、CORS、および API メタデータは、App Service に直接移されました。 この変更により、それらの機能はすべて Web、Mobile、および API Apps にわたって使用できます。 実際には、3 つすべてが同じ共有 **Microsoft.Web/sites** リソース マネージャーでリソースの種類。 API Apps ゲートウェイはもはや必要なくなり、API Apps では提供されません。 これにより、API Management ゲートウェイが 1 つのみになるので、Azure API Management も使いやすくなります。

![API Apps の概要](./media/app-service-api-whats-changed/api-apps-overview.png)

この API Apps 更新における重要な設計原則は、お客様が、API を現状のまま、使用する言語に取り入れることができるようにすることです。  API が既に Web アプリまたはモバイル アプリ* としてデプロイされている場合は、アプリを再デプロイしなくても新しい機能は利用できます。

> [AZURE.NOTE] * では、API Apps preview している場合の移行のガイドラインを記載しています。

### 認証
API Apps、Mobile Services/Apps、および Web Apps の既存の設定不要の認証機能は統合され、管理ポータルの Azure App Service 認証ブレード 1 つで利用できるようになりました。 App Service での認証サービスの概要については、次を参照してください。 [App Service の展開の認証/承認](https://azure.microsoft.com/blog/announcing-app-service-authentication-authorization/)します。

API シナリオの場合は、関連の新機能が複数あります。

- **Azure Active Directory を直接使用するためのサポート**, 、クライアント コードのセッション トークンの AAD トークンを交換することなし: クライアントは、ベアラー トークンの仕様に従って、承認ヘッダーに AAD トークンをだけ含めることができます。 これはまた、クライアントまたはサーバー側で App Service 固有の SDK が必要ないことを意味します。 
- **サービス間または「内部」アクセス**: デーモン プロセスまたは他のクライアント、インターフェイスのない Api にアクセスする必要がある場合は、AAD サービス プリンシパルを使用して、トークンの要求し、に渡す App Service の認証と、アプリケーションでします。
- **承認を遅延**: 多くのアプリケーション、アプリケーションのさまざまな部分の変化のアクセス制限があります。 おそらく、公開したい API もあればサインインを求める API もあるでしょう。 元の認証/承認の機能は、オール オア ナッシングで、すべてのサイトでログインを必要としています。 このオプションはまだ存在しますが、別の方法として、App Service でユーザーを認証した後、アプリケーション コードで、アクセスを許可するかどうか決定を下すことができます。
 
認証の新機能に関する詳細については、次を参照してください。 [Azure App Service で API Apps の認証と承認](app-service-api-authentication.md)します。 以前の API アプリ モデルから新しいに既存の API アプリを移行する方法については、次を参照してください。 [既存の API アプリを移行する](#migrating-existing-api-apps) この記事で後述します。
 
### CORS
コンマ区切りではなく **MS_CrossDomainOrigins** アプリを設定するようになりましたブレードにある CORS を構成するため、Azure 管理ポータル。 また、構成できますリソース マネージャーの Azure PowerShell、CLI などのツールを使用して、または [リソース エクスプ ローラー](https://resources.azure.com/)します。 設定、 **cors** プロパティを **Microsoft.Web/sites/config** のリソースの種類、 **& lt; ロールまたは web サイトの名前と gt;** リソースです。 次に例を示します。

    {
        "cors": {
            "allowedOrigins": [
                "https://localhost:44300"
            ]
        }
    } 

### API のメタデータ
Web、Mobile、および API Apps にわたって API 定義ブレードを使用できるようになりました。 管理ポータルでは、相対 URL か、または API の Swagger 2.0 表現をホストするエンドポイントを指す絶対 URL を指定できます。 あるいは、リソース マネージャーのツールを使用して構成することもできます。 設定、 **apiDefinition** プロパティを **Microsoft.Web/sites/config** のリソースの種類、 **& lt; ロールまたは web サイトの名前と gt;** リソースです。 次に例を示します。

    {
        "apiDefinition":
        {
            "url": "https://myStorageAccount.blob.core.windows.net/swagger/apiDefinition.json"
        }
    }

この時点で、メタデータ エンドポイントには認証なしでパブリックにアクセスできる必要があります。これは、多くのダウンストリーム クライアント (例: Visual Studio REST API クライアント生成や PowerApps の "API の追加" フロー) がそれを使用できるようにするためです。 すなわち、App Service 認証を使用し、アプリ自体から API 定義を公開するには、前述した認証の遅延オプションを使用することが必要になります。そうすれば、Swagger メタデータへのルートはパブリックになります。

## 管理ポータル
選択すると **新規 > Web + モバイル > API アプリ** 、ポータルでは、資料に記載された新しい機能を反映する API アプリを作成します。 **[参照] > API Apps** これらの新しい API アプリを表示になります。 API アプリを参照すると、ブレードでは、Web および Mobile Apps の場合と同じレイアウトと機能を共有しています。 唯一の違いは、クイック スタート コンテンツと設定の順序付けです。

リソース グループ内のすべてのリソースを参照すると、前のプレビュー機能を備えた既存の API アプリ (または Logic Apps で作成した Marketplace API アプリ) が引き続き Logic Apps デザイナーに表示されます。 前のプレビュー機能を備えた API アプリを作成する必要がある場合、パッケージ利用可能で、検索可能なとして Azure Marketplace で **Web + モバイル > API Apps (プレビュー)**します。

## Visual Studio

ほとんどの Web Apps ツールは、同じ基になるを共有しているために、新しい API アプリを使用して機能 **Microsoft.Web/sites** リソースの種類。 ただし、Azure Visual Studio ツールは 2.8.1 以降のバージョンにアップグレードする必要があります。API に固有の機能が多数公開されているからです。 SDK をダウンロード、 [Azure ダウンロード ページ](https://azure.microsoft.com/downloads/)します。

App Service の種類の合理化では、発行は、統合も **発行 > Microsoft Azure App Service**:

![API Apps の発行](./media/app-service-api-whats-changed/api-apps-publish.png)

SDK 2.8.1 に関する詳細についてに発表を読む [ブログの投稿](https://azure.microsoft.com/blog/announcing-azure-sdk-2-8-1-for-net/)します。

あるいは、手動で管理ポータルから発行プロファイルをインポートして発行を有効にすることもできます。 ただし、クラウド エクスプローラー、コードの生成、API アプリの選択/作成には、SDK 2.8.1 またはそれ以降のバージョンが必要です。

前のプレビュー機能を備えた既存の API アプリに発行する機能は、SDK 2.8.1 で引き続き利用できます。 プロジェクトが発行済みである場合、それ以上の操作は必要ありません。 発行を設定するには、次のように選択します。 **API Apps (クラシック)** から、 **他のオプション** [発行] ダイアログ ボックスの一覧です。

## 既存の API アプリを移行する
カスタム API が API Apps の以前のプレビュー バージョンにデプロイされている場合は、2015 年 12 月 31 日までに API Apps の新しいモデルに移行する必要があります。 以前のモデルも新しいモデルも App Service でホストされる Web API をベースにしているため、既存のコードの大部分が再利用できます。

### ホスティングと再デプロイ
再デプロイの手順は、既存の Web API を App Service にデプロイする場合と同じです。 手順:

1. 空の API アプリを作成します。 そのためには、ポータルで [新規]、[API アプリ] の順に操作するか、Visual Studio の発行を使用するか、または Resource Manager ツールを使用します。 Resource Manager ツールまたはテンプレートを使用している場合は、設定、 **種類** 値を **api** 上、 **Microsoft.Web/sites** リソースの種類をクイック スタートを持ち、API のシナリオ向けの管理ポータルで設定します。
2. App Service でサポートされている任意のデプロイ メカニズムを使用して、接続し、プロジェクトを空の API アプリにデプロイします。 読み取り [Azure App Service の展開のドキュメント](../app-service-web/web-sites-deploy.md) の詳細。 
  
### 認証
App Service 認証サービスでは、以前の API Apps モデルで使用されていたのと同じ機能をサポートします。 セッション トークンを使用し、SDK を必要とする場合は、次のクライアント SDK とサーバー SDK を使用します。

- クライアント: [Azure のモバイル クライアント SDK](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)
- サーバー: [Microsoft Azure モバイル アプリ .NET 認証拡張機能](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Authentication/) 

App Service のアルファ版 SDK については、次の SDK が廃止になりました。

- クライアント: [Microsoft Azure app Service SDK](http://www.nuget.org/packages/Microsoft.Azure.AppService)
- サーバー: [Microsoft.Azure.AppService.ApiApps.Service](http://www.nuget.org/packages/Microsoft.Azure.AppService.ApiApps.Service)

ただし、特に、Azure Active Directory では、AAD トークンを直接使用する場合、App Service 固有の SDK は必要ありません。

### 内部アクセス
以前の API Apps モデルには、組み込みの内部アクセス レベルが含まれていました。 この場合、要求に署名するために SDK を使用する必要がありました。 前述したように、新しい API Apps モデルでは、サービス間認証の替わりとして、App Service 固有の SDK を必要としない AAD サービス プリンシパルを使用できます。 詳細について [Azure App Service で API アプリのサービス プリンシパルの認証](app-service-api-dotnet-service-principal-auth.md)します。

### 探索
以前の API Apps モデルは、同じゲートウェイの背後にある同じリソース グループ内で実行時に他の API アプリを検出するための API を備えていました。 この API は、マイクロサービス パターンを実装するアーキテクチャで特に便利です。 この API は直接サポートされませんが、次に示す複数のオプションを使用できます。

1. Azure リソース マネージャーの API を使用して探索します。
2. App Service でホストされる API の前に Azure API Management を配置します。 Azure API Management はファサードとして機能し、内部のトポロジが変更された場合でも、安定した外部 URL を指定できます。
3. 独自の探索用 API アプリをビルドし、起動時に探索用アプリにその他の API アプリが登録されるようにします。
4. デプロイ時に、すべての API アプリ (およびクライアント) のアプリ設定を他の API アプリのエンドポイントに設定します。 これはテンプレートのデプロイで実行可能であり、API Apps で URL を制御できるようになります。

### Logic Apps
Logic Apps デザイナーでは、2016 年の早い時期に新しい API Apps モデルとのシームレスな統合が特別に追加されます。 そのため、Logic Apps に組み込まれた HTTP コネクタは、任意の HTTP エンドポイントを呼び出すことができるほか、サービス プリンシパル認証をサポートします。この認証は、App Service 認証サービスでもネイティブにサポートされています。 App Service でホストされる API でロジック アプリで利用する方法について説明 [ロジック アプリを使用して App Service でホストされている、カスタム API を使用して](../app-service-logic/app-service-logic-custom-hosted-api.md)します。

### <a id="documentation"></a> 以前の API Apps モデルに関するドキュメント
いくつか [azure.microsoft.com](https://azure.microsoft.com) 以前の API Apps モデル用に作成された記事は、不要になった新しいモデルに適用され、サイトから削除されます。 その Url は、新しいモデルで動作する最も近いにリダイレクトされますが、以前の記事では、引き続き参照できます、 [azure.microsoft.com の GitHub ドキュメント リポジトリ](https://github.com/Azure/azure-content)します。 ほとんどの場合は、記事に記載の [記事/アプリ-サービスの api](https://github.com/Azure/azure-content/tree/master/articles/app-service-api) フォルダーです。 以前の API アプリをサポートしたり、Marketplace から新しいコネクタ API アプリを作成したりする場合に参考となる可能性の高いページを以下に示します。

* [認証の概要](https://github.com/Azure/azure-content/tree/master/articles/app-service/app-service-authentication-overview.md)
* [API アプリの保護](https://github.com/Azure/azure-content/tree/master/articles/app-service-api/app-service-api-dotnet-add-authentication.md)
* [内部 API アプリを使用する](https://github.com/Azure/azure-content/tree/master/articles/app-service-api/app-service-api-dotnet-consume-internal.md)
* [クライアント フロー認証の使用](https://github.com/Azure/azure-content/tree/master/articles/app-service-api/app-service-api-authentication-client-flow.md)
* [SaaS コネクタ API アプリのデプロイおよび構成](https://github.com/Azure/azure-content/tree/master/articles/app-service-api/app-service-api-connnect-your-app-to-saas-connector.md)
* [新しいゲートウェイを使用する API アプリのプロビジョニング](https://github.com/Azure/azure-content/tree/master/articles/app-service-api/app-service-api-arm-new-gateway-provision.md)
* [API アプリをデバッグする](https://github.com/Azure/azure-content/tree/master/articles/app-service-api/app-service-api-dotnet-debug.md)
* [SaaS プラットフォームへの接続](https://github.com/Azure/azure-content/tree/master/articles/app-service-api/app-service-api-dotnet-connect-to-saas.md)
* [Logic Apps 向けの API アプリの拡張](https://github.com/Azure/azure-content/tree/master/articles/app-service-api/app-service-api-optimize-for-logic-apps.md)
* [API アプリ トリガー](https://github.com/Azure/azure-content/tree/master/articles/app-service-api/app-service-api-dotnet-triggers.md)

## 次のステップ
詳細については、記事では、参照、 [API Apps のドキュメント セクション](https://azure.microsoft.com/documentation/services/app-service/api/)です。 これらの記事は更新され、API Apps の新しいモデルを反映しています。 さらに、移行に関する詳細またはガイダンスについては、次のフォーラムを参照してください。

- [MSDN フォーラム](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureAPIApps)
- [スタック オーバーフロー](http://stackoverflow.com/questions/tagged/azure-api-apps)


