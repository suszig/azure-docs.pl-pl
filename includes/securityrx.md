#Azure のセキュリティに関するガイダンス

##要約

Azure のアプリケーションを開発する場合に
注意する必要のある主要なセキュリティの項目は、ID とアクセスです。
このトピックでは、クラウドでの ID とアクセスに関するセキュリティの主要項目と、
クラウド アプリケーションを保護するための最良の方法について説明します。

##概要

アプリケーションのセキュリティは、アプリケーションの表面で機能します。 アプリケーションの公開先が増えるほど、
セキュリティ上の懸念が拡大します。 次に
自動のバッチ プロセスとして実行されるアプリケーションは、
以下、公開されている web サイトよりも、セキュリティの観点からです。

クラウドに移行すると、
インフラストラクチャやネットワーキングに関する管理者の負担は軽減されます。
世界規模のセキュリティ手法、ツール、テクノロジを備えたデータ センターで管理されるためです。 その一方で、
クラウドの性質上、アプリケーションの公開規模は大きくなるため、
攻撃を受ける可能性が高まります。 そのため、
多くのクラウド テクノロジやサービスは、
エンドポイント コンポーネントまたはメモリ内のコンポーネントとして公開されています。 Azure Storage、Service Bus、SQL
Database (旧 SQL Azure)、およびその他の多くのサービスは、
ネットワーク経由でエンドポイントからアクセスできます。

クラウド アプリケーションでは、
攻撃を食い止める高いセキュリティ標準を設計、開発、および保守するため、
アプリケーション開発者の負担は従来よりも多くなります。
以下の図を見てください (J.D. Meier の Meier の [Azure のセキュリティ
ノート PDF](http://blogs.msdn.com/b/jmeier/archive/2010/08/03/now-available-azure-security-notes-pdf.aspx)): インフラストラクチャ部分によって指定される方法に注意してください、
インフラストラクチャ部分にどのように対応するかに注目してください。
アプリケーション開発者に多くのセキュリティ関連作業が任されています。

![アプリケーションのセキュリティ保護][01]

開発者にとってよい点は、従来のセキュリティ開発手法、
原則、技法すべてを、クラウド アプリケーションの開発にも
適用できるということです。 開発の際に考慮する必要のある主な領域は
次のとおりです。

-   インジェクション攻撃を防ぐため、すべての入力を、種類、長さ、範囲、文字パターン
    について検証します。また、アプリケーションが エコー バックするデータはすべて、
    適切に無害化します。
-   機密データを、ローカルおよびネットワーク上で保護し、
    情報漏えいやデータ改ざんの脅威を抑制します。
-   監査やログ機能を適切に実装し、
    非否認の脅威を抑制します。
-   認証と承認を、プラットフォームが提供する実証済みのメカニズムを使用して実装し、
    ID のなりすましや特権の昇格の脅威を
    抑制します。

脅威、攻撃、脆弱性、および対処法の
対応策は、patterns & practices の「を参照してください [Cheat Sheet: Web
アプリケーションのセキュリティ フレーム](http://msdn.microsoft.com/library/ff649461.aspx) と [セキュリティ Guidance for Applications Index](http://msdn.microsoft.com/library/ff650760.aspx)します。

クラウドにおける認証とアクセス制御のメカニズムは、
内部設置型アプリケーションの場合と大きく異なります。 クラウド アプリケーションでは、
より多くの認証およびアクセス制御のオプションを利用できます。
これが、混乱の原因となり、
実装の欠陥につながります。 クラウド アプリケーションの定義では、
さらに混乱が生じます。 たとえば、アプリケーションをクラウドにデプロイしても、
認証メカニズムは Active Directory が提供することがあります。 反対に、
アプリケーションを内部設置でデプロイし、
認証メカニズムはクラウドで提供されることもあります。
たとえば、Azure Active Directory Access Control (以前の Access Control
Service (ACS)) を使用します。

##脅威、脆弱性、および攻撃

脅威は、機密情報の漏えいやサービスの利用停止など、
避けなければならない潜在的な問題です。
脅威は通常、各脅威の頭文字をとった「STRIDE」という区分で分類されます。

-   **S**poofing または id の盗難
-   **T**ampering データ
-   **R**これがアクションの否認
-   ****漏洩
-   **D**ービス拒否
-   **E**特権の levation

脆弱性はコードの開発でもたらされるバグで、
攻撃者によってアプリケーションの攻撃に利用される可能性があります。 たとえば、
機密データをクリア テキストで送信すると、
トラフィック スニッフィング攻撃によって情報が漏えいする可能性があります。

攻撃者はこれらの脆弱性を利用して、
アプリケーションを攻撃します。 クロス サイト スクリプト (XSS) は、
無害化されていない出力を利用する攻撃です。 もう 1 つの例は、
クリア テキストで送信される機密情報を捕捉する、ネットワークの盗聴です。 それは、
攻撃は、ID のなりすまし攻撃の布石となります。 論点を明確にするため、
問題を脅威、脆弱性、および攻撃に限定します。
Azure にデプロイした Web アプリケーションに関連する
問題を概念化した次の図を参照してください (J.D.
Meier の [Azure セキュリティ ノート PDF](http://blogs.msdn.com/b/jmeier/archive/2010/08/03/now-available-azure-security-notes-pdf.aspx))。

![脅威、脆弱性、および攻撃][02]

開発者は、脆弱性を全体的に制御します。 開発でもたらされる脆弱性が少ないほど、
攻撃者が利用できる方法は
少なくなります。

ID とアクセス関連の脆弱性は、対策しない限り、
STRIDE モデルのすべての脅威に利用される可能性があります。 たとえば、実装が不適切な認証メカニズムは
ID のなりすましの原因となり、その結果、
情報漏えい、データ改ざん、特権の昇格、
またはサービス全体のシャットダウンにつながります。 クラウド アプリケーションの
ID およびアクセス制御の実装における潜在的な脆弱性を発見するため、
次の問いについて考えてみてください。

-   ネットワーク上ではクリア テキストで
    Azure サービスに資格情報を送信しますか。
-   Azure サービスに資格情報をクリア テキストで格納しますか。
-   Azure サービスに強力なパスワード ポリシーを
    適用しますか。
-   資格情報の検証に Azure とカスタムの認証メカニズムの
    どちらを使用しますか。
-   Azure のサービス認証セッションまたはトークンの有効期間を
    適切に制限していますか。
-   配布したクラウド アプリケーションの Azure エントリ ポイントごとに
    アクセス許可を検証しますか。
-   利用する承認メカニズムは、機密情報を漏えいせずに、
    または無制限のアクセスを許可せずに、安全に遮断しますか。

##対処法

攻撃に対する最良の対処法は、
(独自に実装せずに) プラットフォームによって提供される ID やアクセスのメカニズムを
使用することです。 広く利用されている、次の ID およびアクセス テクノロジの導入を検討してください。

**Windows Identity Foundation (WIF)。**WIF は
.NET Framework 4.5 に含まれている .NET ランタイム ライブラリです
(.NET 3.5/4.0 用に単独でダウンロードすることもできます)。 WIF は、WS-Federation や WS-Trust などのプロトコルの処理や、
Security Assertion Markup Language (SAML) などのトークンの処理など、
負荷の高い作業を実行します。そのため、アプリケーション用に複雑なセキュリティ関連コードを
手動で作成する必要はありません。 次の
次のリソースを参照してください。

-   [Windows Identity Foundation 4.5 のサンプル](http://code.msdn.microsoft.com/site/search?f%5B0%5D.Type=SearchText&f%5B0%5D.Value=wif&f%5B1%5D.Type=Topic&f%5B1%5D.Value=claims-based%20authentication) MSDN コード ギャラリーにします。
-   [Visual Studio 11 ベータ向け Windows Identity Foundation 4.5 ツール](http://visualstudiogallery.msdn.microsoft.com/e21bf653-dfe1-4d81-b3d3-795cb104066e) 上
    。
-   [Windows Identity Foundation ランタイム (.Net 3.5/4.0)](http://www.microsoft.com/download/details.aspx?id=17331) MSDN にします。
-   [Windows Identity Foundation 3.5/4.0 サンプルおよび Visual Studio
    2008/2010年テンプレート](http://www.microsoft.com/download/details.aspx?displaylang=en&id=4451) MSDN にします。

**Azure の AD アクセス制御 (旧 ACS)**します。 
Azure AD Access Control は、Security Token
Service (STS) を提供するクラウド サービスです。企業の Active Directory などの
さまざまな ID プロバイダー (IdP) または
Windows Live ID、Microsoft アカウント、Facebook、Google、Yahoo! などのインターネット IdP、Open ID 2.0 ID プロバイダーとの
フェデレーションを利用できます。 Azure AD Access Control の詳しい情報については、
次のリソースを参照してください。

-   [Access Control Service 2.0](http://msdn.microsoft.com/library/gg429786.aspx) 
-   [ACS を使用したシナリオとソリューション](http://msdn.microsoft.com/library/gg185920.aspx)
-   [ACS の利用方法](http://msdn.microsoft.com/library/windowsazure/gg185939.aspx)
-   [A Guide to Claims-Based Identity and Access Control (クレームベースの ID およびアクセス制御)](http://msdn.microsoft.com/library/ff423674.aspx)
-   [Identity Developer Training Kit](http://www.microsoft.com/download/details.aspx?id=14347)
-   [MSDN 提供の Identity Developer Training Course](http://msdn.microsoft.com/IdentityTrainingCourse)

**Active Directory フェデレーション サービス (AD FS).**Active Directory
フェデレーション サービス (AD FS) 2.0 は、
Windows Server に関連する識別情報ソリューション [概要] タブ Active Directory
サポートします。 AD FS 2.0 は各種プロトコル (WS-Trust、WS-Federation、および SAML
) をサポートします。 AD FS の詳しい情報については、次のリソースをご覧ください
。

-   [AD FS 2.0 Content Map (AD FS 2.0 コンテンツ マップ)](http://social.technet.microsoft.com/wiki/contents/articles/2735.ad-fs-2-0-content-map.aspx)
-   [Web SSO の設計][Web SSO Design]
-   [フェデレーション Web SSO の設計][Federated Web SSO Design]

**Azure Shared Access Signature。**Shared Access Signature
を使用すると、BLOB またはコンテナー リソースへのアクセスを調整できます。 この
詳しい情報については、次のリソースを
参照してください。

-   [コンテナーと BLOB へのアクセス管理](http://msdn.microsoft.com/library/ee393343.aspx)
-   [新しいストレージ機能: 共有アクセス署名 (ブログの投稿)](http://blog.smarx.com/posts/new-storage-feature-signed-access-signatures)
-   [Shared Access Signatures Are Easy These Days (最近使いやすくなった共有アクセス署名)](http://blog.smarx.com/posts/shared-access-signatures-are-easy-these-days)

##シナリオ マップ

このセクションでは、このトピックで扱う主なシナリオの概要を説明します。
個々のアプリケーションに最適な ID ソリューションを決定する際の案内図として
想定しています。

-   **フェデレーション認証を ASP.NET Web フォーム アプリケーションです。**イン
    このシナリオでは、Windows Server Active Directory で管理される
    Live ID や Microsoft アカウントなどのインターネット ID、または企業 ID
    を使用して、ASP.NET Web フォームへのアクセスを制御します。
-   **フェデレーション認証を使用する WCF (SOAP) サービス**。この
    Azure AD Access Control によって管理されるサービス ID を使用して、WCF (SOAP)
    サービスへのアクセスを制御します。
-   **フェデレーション認証、Id を使用する WCF (SOAP) サービス
    Active Directory。**このシナリオでは、
    企業の Windows Server Active Directory によって管理される ID を使用して、WCF (SOAP) Web
    サービスへのアクセスを制御します。
-   **フェデレーション認証を使用する WCF (REST) サービス**。この
    シナリオでは、Azure AD Access Control によって管理されるサービス ID を使用して、WCF (REST)
    サービスへのアクセスを制御します。
-   **Live ID を使用する WCF (REST) サービスと Microsoft アカウント、Facebook、
    Google、yahoo!、Open id。**このシナリオでは、Live ID や Microsoft アカウントなどの
    インターネット ID を使用して、WCF (REST) サービスへの
    アクセスを制御します。
-   **使用する REST WCF サービスへの ASP.NET Web アプリケーションは共有 SWT トークンです。**イン
    このシナリオでは、フロントエンド
    ASP.NET Web アプリケーションとダウンストリーム REST サービスを含む
    分散型アプリケーションがあり、エンド ユーザーのコンテキストを物理層でフローさせます。
-   **要求対応でのロールベースのアクセス制御 (RBAC) 承認
    アプリケーションおよびサービスです。**このシナリオでは、ロールに基づいて
    アプリケーションに承認ロジックを実装します。
-   **クレーム対応アプリケーションでクレーム ベースの承認と
    サービスです。**このシナリオでは、複雑な承認ルールに基づいて
    アプリケーションに承認ロジックを実装します。
-   **Azure ストレージ サービスの Id とアクセスのシナリオ**。で
    このシナリオでは、Azure のストレージ BLOB およびコンテナーへの
    共有アクセスを安全に保護する必要があります。
-   **Azure SQL Database の Id とアクセスのシナリオ**。SQL
    SQL Database では、SQL Server 認証のみがサポートされています。 Windows
    認証 (統合セキュリティ) はサポートされていません。 ユーザーは
    SQL Database に接続するたびに、資格情報 (ログイン名とパスワード) を
    SQL Database。
-   **Azure Service Bus の Id とアクセスのシナリオ**。この
    このシナリオでは、Azure Service Bus キューに安全にアクセスする必要があります。
-   **メモリ内キャッシュの Id とアクセスのシナリオ**。このシナリオでは
    メモリ内キャッシュによって管理されるデータに安全にアクセスする必要があります。
-   **Azure Marketplace の Id とアクセスのシナリオ**。この
    このシナリオでは、Azure Marketplace のデータセットに安全にアクセスする
    必要があります。

##Azure の ID とアクセスのシナリオ

このセクションでは、さまざまなアプリケーション アーキテクチャの
一般的な ID とアクセスのシナリオの概要について説明します。 簡単な説明については
「シナリオ マップ」を参照してください。

###フェデレーション認証を使用する ASP.NET Web フォーム アプリケーション

このアプリケーション アーキテクチャ シナリオでは、Web アプリケーションを
Azure または内部設置型システムにデプロイできます。 このアプリケーションでは、
ユーザー認証は、企業の Active Directory または
インターネット ID プロバイダーが行う必要があります。

このシナリオを解決するには、Azure AD Access Control と Windows
Identity Foundation を使用します。

![Azure Active Directory Access Control][03]

このシナリオを実装するには、以下のリソースを参照してください。

-   [方法: ACS を使用して最初の要求対応 ASP.NET アプリケーションを作成する](http://msdn.microsoft.com/library/gg429779.aspx)
-   [方法: ASP.NET Web アプリケーションでログイン ページをホストする](http://msdn.microsoft.com/library/gg185926.aspx)
-   [方法: WIF および ACS を使用して要求対応の ASP.NET アプリケーションで 要求承認を実装する](http://msdn.microsoft.com/library/gg185907.aspx)    
-   [方法: 実装でロール ベースのアクセス制御 (RBAC) 要求対応
    ロール ベースのアクセス制御 (RBAC) を実装する](http://msdn.microsoft.com/library/gg185914.aspx)
-   [ACS と ASP.NET Web アプリケーション間の信頼を構成する方法
    間の信頼を構成する](http://msdn.microsoft.com/library/gg185947.aspx)
-   [コード サンプル:ASP.NET の単純なフォーム](http://msdn.microsoft.com/library/gg185938.aspx)

###サービス ID を使用する WCF (SOAP) サービス

このアプリケーション アーキテクチャ シナリオでは、WCF (SOAP) サービスを
Azure または内部設置型システムにデプロイできます。 このサービスは
ダウンストリーム サービスとして、Web アプリケーションまたはその他の Web
します。 アプリケーション専用の ID を使用して、
アクセスを制御します。 IIS で使用されるアプリケーション プール アカウントの種類の点から
考えてみましょう。テクノロジは異なりますが、
エンド ユーザー アカウントではなくアプリケーション スコープ アカウントを使用して
アクセスされるサービスという点で似ている方法です。

サービス ID 機能を Azure AD Access Control で使用します。
これは、アプリケーションを Windows Server と IIS にデプロイするときに使用する、
IIS アプリケーション プール アカウントと似ています。 WCF (SOAP) サービスで
WIF によって処理される SAML トークンを発行するように
Azure AD Access Control を構成します。

![WCF (SOAP) サービス][04]


このシナリオを実装するには、以下のリソースを参照してください。

-   [方法: X.509 証明書、パスワードを使用してサービス Id を追加します。
    サービス ID を追加する](http://msdn.microsoft.com/library/gg185924.aspx)
-   [方法: を WCF サービスへのクライアント証明書による認証
    認証する](http://msdn.microsoft.com/library/hh289316.aspx)
-   [方法: 認証ユーザー名とパスワードを WCF サービス
    認証する](http://msdn.microsoft.com/library/gg185954.aspx)
-   [サンプル コード: WCF の証明書の認証](http://msdn.microsoft.com/library/gg185952.aspx)
-   [サンプル コード: WCF ユーザー名認証](http://msdn.microsoft.com/library/gg185927.aspx)

###Active Directory のフェデレーション認証、ID を使用する WCF (SOAP) サービス

このアプリケーション アーキテクチャ シナリオでは、WCF (SOAP) サービスを
Azure または内部設置型システムにデプロイできます。 企業の Windows Server Active Directory (AD) によって
管理される ID を使用して、WCF (SOAP) サービスへのアクセスを
制御します。

Windows Server AD FS とのフェデレーションを構成済みの Azure AD Access Control を
使用します。 この場合、Azure AD Access Control でサービス ID を
構成する必要はありません。 WCF (SOAP) サービスにアクセスするエージェントが
AD FS に資格情報を提供し、認証が成功すると、
AD FS によってトークンが発行されます。 このトークンは、
次に Azure AD Access Control に送信され、
再発行されてエージェントに戻ります。 エージェントはこのトークンを使用して、要求を WCF (SOAP) サービスに送信
します。

![AD を使用する WCF (SOAP) サービス][05]

このシナリオを実装するには、以下のリソースを参照してください。

-   [方法: X.509 証明書、パスワードを使用してサービス Id を追加します。
    サービス ID を追加する](http://msdn.microsoft.com/library/gg185924.aspx)
-   [方法: AD FS 2.0 を ID プロバイダーとして構成する](http://msdn.microsoft.com/library/gg185961.aspx)
-   [方法: AD FS 2.0 を構成する管理サービスを使用して、
    エンタープライズ ID プロバイダーとして構成する](http://msdn.microsoft.com/library/gg185905.aspx)
-   [サンプル コード: AD FS 2.0 による WCF フェデレーション認証
](http://msdn.microsoft.com/library/hh127796.aspx)

###サービス ID を使用する WCF (REST) サービス

このシナリオでは、WCF (REST) サービスを Azure または内部設置型システムに 
デプロイできます。 このサービスはダウンストリーム サービスとして、
Web アプリケーションまたはその他の Web サービスからアクセスされます。 アプリケーション専用の ID を使用して、
アクセスを制御します。IIS で使用されるアプリケーション プール アカウントの種類の点から
考えてみましょう。テクノロジは異なりますが、
エンド ユーザー アカウントではなくアプリケーション スコープ アカウントを使用して
アクセスされるサービスという点で似ている方法です。

サービス ID 機能を Azure AD Access Control で使用します。
Simple Web Token (SWT) トークンを発行するように Azure AD Access Control を
構成します。 REST サービス側で SWT トークンを処理するには、
カスタム トークン ハンドラーを実装して WIF
パイプラインに組み込むか、WIF インフラストラクチャを使用せずに "手動で"
解析します。

次の図で考えてみましょう (WIF はオプションです)。

![REST サービス][06]

このシナリオを実装するには、以下のリソースを参照してください。

-   [ACS と対称を使用して WCF サービス間の信頼を構成する方法
    キー](http://msdn.microsoft.com/library/gg185958.aspx)
-   [方法: Azure にデプロイした REST WCF サービスへの認証
    認証する](http://msdn.microsoft.com/library/hh289317.aspx)
-   [Code Sample: ASP.NET Web Service (サンプル コード: ASP.NET Web サービス)](http://msdn.microsoft.com/library/gg983271.aspx)
-   [Code Sample: Windows Phone 7 Application (サンプル コード: Windows Phone 7 アプリケーション)](http://msdn.microsoft.com/library/gg983271.aspx)
-   [REST WCF With SWT Token Issued By Azure Access Control Service (ACS) (Azure Access Control Service (ACS) によって発行される SWT トークンを
    使用する REST WCF)](http://code.msdn.microsoft.com/REST-WCF-With-SWT-Token-123d93c0)

###Live ID/Microsoft アカウント、Facebook、Google、Yahoo!、Open ID を使用する WCF (REST) サービス。

このシナリオでは、WCF (REST) サービスを Azure または内部設置型システムに 
デプロイできます。 Live ID、Microsoft アカウント、Facebook などの
パブリックなインターネット ID を使用して、WCF (REST) サービスへのアクセスを制御します。

Azure AD Access Control を使用して SWT トークンを発行します。 処理する、
SWT トークンを処理するには、カスタム トークン ハンドラーを実装して
WIF パイプラインに組み込むか、WIF インフラストラクチャを使用せずに
"手動で" 解析します。

このシナリオを実装するには、アプリケーションが Web ブラウザーを使用して、
エンド ユーザーの資格情報収集を制御する必要があることに
資格情報。 そのため、REST サービスが ASP.NET Web アプリケーションからアクセスされるシナリオには
適していません。 REST サービスが、
Windows Phone 7 アプリやリッチ デスクトップ クライアントなどの
ユーザー クライアント アプリケーションからアクセスされるシナリオに
適しています。 Web ブラウザーの制御を切り離す主な理由は、
インターネット ID がアクティブなプロファイル シナリオ (Web
サービス シナリオ) をネイティブではサポートしていないためです。 インターネット ID は主に、ブラウザー リダイレクトに依存するパッシブなプロファイル
ブラウザー リダイレクトに依存するシナリオ (web アプリケーション) そのため、web。
Web ブラウザー制御が簡易になります。

次の図で考えてみましょう (WIF はオプションのため表示されていません)。

![WIF はオプション][07]

このシナリオを実装するには、以下のリソースを参照してください。

-   [方法: ACS を使用して Azure にデプロイした REST WCF サービスを認証する](http://msdn.microsoft.com/library/hh289317.aspx)
-   [方法: Google を ID プロバイダーとして構成する](http://msdn.microsoft.com/library/gg185976.aspx)
-   [方法: Facebook を ID プロバイダーとして構成する](http://msdn.microsoft.com/library/gg185919.aspx)
-   [方法: yahoo! を構成します。 Id プロバイダーとして](http://msdn.microsoft.com/library/gg185977.aspx)
-  [ Code Sample: Windows Phone 7 Application (サンプル コード: Windows Phone 7 アプリケーション)](http://msdn.microsoft.com/library/gg983271.aspx)
-   [REST WCF With SWT Token Issued By Azure Access Control Service (ACS) (Azure Access Control Service (ACS) によって発行される SWT トークンを
    使用する REST WCF)](http://code.msdn.microsoft.com/REST-WCF-With-SWT-Token-123d93c0)


###共有 SWT トークンを使用する REST WCF サービスへの ASP.NET Web アプリケーション

このシナリオでは、フロントエンド ASP.NET Web アプリケーションとダウンストリーム REST サービスを含む
分散型アプリケーションがあり、エンド ユーザーのコンテキストを
物理層間で維持します。 これは、承認ロジックまたはログが、
ダウンストリーム REST サービスのエンド ユーザー ID に基づく場合に
必要になることがあります。

SWT トークンを発行するように Azure AD Access Control を構成します。 SWT
トークンはフロント エンド ASP.NET Web アプリケーションに発行され、
ダウンストリーム REST サービスと共有されます。 この場合、Azure AD Access Control で
証明書利用者を 1 つだけ構成します。 ただし、
注意点がいくつかあります。

-   WIF はそのまま使用できる SWT トークン ハンドラーを提供していないため、
    ASP.NET Web アプリケーションで使用されるカスタム トークン ハンドラーを
    実装する必要があります。 ブラウザー リダイレクトに依存する WS-Federation
    プロトコルをサポートするための複雑な作業は、手動で実装せずに
    WIF を利用します。
-   SWT カスタム トークン ハンドラーを実装する場合、ブートストラップ トークンが
    保持されることを確認するため、ブートストラップ トークンにアクセスできることを確認します。
    アクセスできない場合、共有してダウンストリーム REST サービスに
    送信することができません。
-   REST サービス側で WIF を使用する必要はありません。この場合は
    リダイレクトを処理する必要がないため、トークンを "手動で" 解析しても
    かまいません。

![ASP.NET Web アプリケーション][08]

このシナリオを実装するには、以下のリソースを参照してください。

-   [方法: Google を ID プロバイダーとして構成する](http://msdn.microsoft.com/library/gg185976.aspx)
-   [方法: Facebook を ID プロバイダーとして構成する](http://msdn.microsoft.com/library/gg185919.aspx)
-   [方法: yahoo! を構成します。 Id プロバイダーとして](http://msdn.microsoft.com/library/gg185977.aspx)
-   [ASP.NET Web App To REST WCF Service Delegation Using Shared SWT Token (共有 SWT トークンを使用する REST WCF サービス委任への ASP.NET Web
    アプリケーション)](http://code.msdn.microsoft.com/ASPNET-Web-App-To-REST-WCF-b2b95f82)

###要求対応アプリケーションとサービスにおけるロールベースの Access Control (RBAC)

このシナリオでは、ロールに基づいて Web
ユーザー ロールに基づいて、アプリケーションまたはサービス: get の必要な役割を持つユーザー
アクセスすることができ、必要なロールを持っていないユーザーは拒否されます。 アプリケーションは、
"ユーザーは X のロールか" という単純な質問に回答するだけです
。

このシナリオを解決する方法は複数あります。 Azure AD Access Control、
AD Access Control、WIF Claims Authentication Manager、
samlSecurityTokenRequirement マッピング、またはカスタム ロール マネージャーを使用できます。

すべての場合に WIF が使用されます。 WIF は IPrincipal.IsInRole("MyRole") メソッドを
メソッドを呼び出します。 ほとんどの場合で重要となるのが、
トークンに URI が
http://schemas.microsoft.com/ws/2008/06/identity/claims/role
のロールの種類の要求があり、IsInRole メソッドを呼び出すときに WIF が
ロールのメンバーシップを検証できるようにすることです。

**Azure AD アクセス制御**します。 この実装では、 
Azure AD Access Control の要求変換ルール エンジンが使用されます。 キューにメッセージがないときに
入力方向の要求すべてをロールの種類の要求に変換して、
トークンがアプリケーションまたはサービスに到達したときに、
WIF がこのロール要求を解析して IsInRole メソッドの呼び出しが成功したことを
確認できます。

![][09]

**WIF ClaimsAuthenticationManager**します。 この実装では、
ClaimsAuthenticationManager を WIF の機能拡張ポイントとして使用します。 この方法を使用して、
任意の入力方向の要求をアプリケーションでロールの要求の種類に
変換します。 複雑な変換作業が、
コードの作成のみに軽減されます。

![][10]

**samlSecurityTokenRequriement マッピング**します。 この実装では、
samlSecurityTokenRequirement 構成を web.config で使用して、
ロール要求として動作する要求の種類を WIF に通知できます。 たとえば、次のように入力します。
トークンがグループの種類の要求を運ぶ場合、それをロール要求の種類に
マップします。 この方法では、単純なマッピングのみを行うことができます。

![][11]

**カスタム RoleManager。**この実装では、カスタム
RoleManger を実装します。 GetAllRoles() などのカスタム RoleManger インターフェイス メソッドを実装すると、
WIF を使用して入力方向の要求が検査されます。

![][12]

このシナリオを実装するには、以下のリソースを参照してください。

-   [方法: 実装でロール ベースのアクセス制御 (RBAC) 要求対応
    ロール ベースのアクセス制御 (RBAC) を実装する](http://msdn.microsoft.com/library/gg185914.aspx)
-   [方法: 規則を使用してトークン変換ロジックを実装する](http://msdn.microsoft.com/library/gg185955.aspx)
-   [Authorization With RoleManager For Claims Aware (WIF) ASP.NET Web Applications (要求対応 (WIF) ASP.NET Web アプリケーションに対する RoleManager による
    アプリケーション](http://blogs.msdn.com/b/alikl/archive/2010/11/18/authorization-with-rolemanager-for-claims-aware-wif-asp-net-web-applications.aspx)
-   サンプル コード: での IsInRole の要求を使用する [Windows Identity Foundation
    SDK](http://www.microsoft.com/downloads/details.aspx?FamilyID=c148b2df-c7af-46bb-9162-2c9422208504)

###要求対応アプリケーションとサービスにおける要求ベースの承認

このシナリオでは、複雑な承認ロジックを
Web アプリケーションまたはサービスに実装します。IsInRole() メソッドは
承認のニーズを満たしません。 承認方法が
ロールに依存する場合は、前のセクションで概説したロール ベースのアクセス制御を実装することを
検討してください。

ClaimsAuthorizationManager を WIF の機能拡張ポイントとして使用します。
ClaimsAuthorizationManager では外部のアクセス チェックを呼び出すので、
アプリケーションのコードでアクセス チェックを実装するよりもアプリケーション コードが簡素化され、
保守しやすくなります。

![][13]

このシナリオを実装するには、以下のリソースを参照してください。

-   [方法: 規則を使用してトークン変換ロジックを実装する](http://msdn.microsoft.com/library/gg185955.aspx)
-   [方法: 要求要求対応の ASP.NET での承認を実装します。
    アプリケーションで要求承認を実装する](http://msdn.microsoft.com/library/gg185907.aspx)
-   サンプル コード: クレーム ベースの承認」 [Windows Id
    Foundation SDK](http://www.microsoft.com/downloads/details.aspx?FamilyID=c148b2df-c7af-46bb-9162-2c9422208504)


##Azure ストレージ サービスの ID とアクセスのシナリオ

このシナリオでは、Azure のストレージ BLOB およびコンテナーへの
共有アクセスを安全に保護する必要があります。

共有アクセス署名を使用します。 独自のアプリケーションからストレージ サービス アカウントにアクセスするには、
ストレージ サービス アカウントを構成および管理するときに
Azure ポータルから利用できる共有ハッシュを
使用します。 自分のストレージ サービス アカウントで
他のユーザーに BLOB およびコンテナーへのアクセスを許可するには、Shared Access Signature の
URL を使用します。

情報漏えいを防ぐため、情報の管理には特に注意を払ってください。
また、Shared Access Signature の有効期間についても
注意します。

![][14]

このシナリオを解決するには、以下のリソースを参照してください。

-   [コンテナーと BLOB へのアクセス管理](http://msdn.microsoft.com/library/ee393343.aspx)
-   [新しいストレージ機能: 共有アクセス署名 (ブログの投稿)](http://blog.smarx.com/posts/new-storage-feature-signed-access-signatures)
-   [Shared Access Signatures Are Easy These Days (最近使いやすくなった共有アクセス署名)](http://blog.smarx.com/posts/shared-access-signatures-are-easy-these-days)


## Azure SQL Database の ID とアクセスのシナリオ


SQL データベースでは SQL Server 認証のみがサポートされています。 Windows
認証 (統合セキュリティ) はサポートされていません。 ユーザーは
SQL Database に接続するたびに、資格情報 (ログイン名とパスワード) を
SQL Database。 情報漏えいを防ぐため、
ユーザー名とパスワードの管理には特に注意を払ってください。


![][15]


このシナリオを解決するには、次のヘルプ トピックを参照してください。<br/>
[Azure SQL Database 開発: 操作方法に関するトピック](http://msdn.microsoft.com/library/azure/ee621787.aspx)


または、数多くある子トピックのいずれかを参照してください。次に一部のトピックを示します。


- [方法: sqlcmd を使用した Azure SQL Database への接続](http://msdn.microsoft.com/library/azure/ee336280.aspx)
- [方法: ADO.NET を使用して Azure SQL Database に接続する](http://msdn.microsoft.com/library/azure/ee336243.aspx)
- [方法: PHP を使用した Azure SQL Database への接続](http://msdn.microsoft.com/library/azure/ff394110.aspx)
- [方法: JDBC を使用した Azure SQL Database への接続](http://msdn.microsoft.com/library/azure/gg715284.aspx)


またはを参照してください。<br/>
[Azure SQL Database のセキュリティのガイドラインと制限事項](http://msdn.microsoft.com/library/azure/ff394108.aspx#authentication)


##Azure Service Bus の ID とアクセスのシナリオ

Service Bus と Azure AD Access Control には特別な関連があり、
Service Bus の各サービス名前空間が、
同じ名前の Access Control サービス名前空間 (サフィックスは "–sb") と
ペアになっています。 これは、
Service Bus と Access Control が、相互の信頼関係および関連付けられた暗号化シークレットを管理するための
特別なリレーションシップです。 詳細については、
以下のリソースを参照してください。

![][16]

このシナリオを解決するには、以下のリソースを参照してください。

-   [ACS による Service Bus のセキュリティ保護](http://channel9.msdn.com/posts/Securing-Service-Bus-with-ACS) (ビデオ)
-   [ACS による Service Bus のセキュリティ保護](https://skydrive.live.com/view.aspx?cid=123CCD2A7AB10107&resid=123CCD2A7AB10107%211849) (スライド)
-   [Service Bus 認証とアクセス制御サービスによる
    認証](http://msdn.microsoft.com/library/hh403962.aspx)

##メモリ内キャッシュの ID とアクセスのシナリオ

メモリ内キャッシュ (以前の Azure Cache) は、
認証を Azure AD Access Control に依存します。 管理ポータルから利用できる
共有キーを使用します。 キャッシュにアクセスするとき、
コードまたは構成ファイルでキーを使用します。 情報漏えいを防ぐために、
キーを安全な状態で保管してください。

![][17]


このシナリオを解決するには、以下のリソースを参照してください。

-   [方法: Azure プログラムを使用してキャッシュ クライアントを構成します。
    キャッシュ](http://msdn.microsoft.com/library/windowsazure/gg618003.aspx)
-   [方法: アプリケーションの構成を使用してキャッシュ クライアントを構成します。
    構成する (Azure Shared Caching)](http://msdn.microsoft.com/library/windowsazure/gg278346.aspx)
-   [Azure Service Bus および Caching のサンプル](http://msdn.microsoft.com/library/ee706741.aspx) (サンプルをキャッシュします。
    セクション)。

##Azure Marketplace の ID とアクセスのシナリオ

Azure Marketplace データセットへのアクセスは、無料と有料のどちらの場合でも、
ユーザーのアクセスを許可する前に認証する必要があります。 Azure プロジェクトを
作成するときは、認証プロセスをコードに含める必要が
コードにインポートします。 次の一般的なシナリオで考えてみましょう。

###自分のデータセットにアクセスする

このシナリオでは、自分の Marketplace サブスクリプションでデータセットを使用する
アプリケーションを作成しています。 あなたはアプリケーションのユーザーです。
アプリケーションは、Azure、内部設置型のシステム、または Marketplace にデプロイできます。

Marketplace サブスクリプションから利用できる共有キーを
使用します。 Marketplace ポータルを使用して、共有キーを取得します。

![][18]

このシナリオを解決するには、以下のリソースを参照してください。

-   [Marketplace アプリケーションでの HTTP 基本認証の実装](http://msdn.microsoft.com/library/gg193417.aspx)

###ユーザーが自分のデータセットにアクセスする

このシナリオでは、自分のデータセットにユーザーがアクセスするのを許可するアプリケーションを
作成しています。 アプリケーションは、Azure、内部設置型のシステム、または Marketplace に
デプロイできます。

このシナリオを解決するには、OAuth 委任を使用します。 ユーザーは、
Live ID または Microsoft アカウントの資格情報を入力するように求められ、
同意プロセスも適用されます。

![][19]

このシナリオを解決するには、以下のリソースを参照してください。

-   [OAuth Web クライアントの例](http://go.microsoft.com/fwlink/?LinkId=219162)
-   [OAuth リッチ クライアントの例](http://go.microsoft.com/fwlink/?LinkId=219163)

###Marketplace API にアクセスするアプリケーション

このシナリオでは、
Marketplace API にアクセスするアプリケーションを作成しています。 Marketplace API の呼び出しを完了するには、
認証が必要です。 アプリケーションは
Azure Marketplace にデプロイします。

![][20]

認証の実装の詳細については、Marketplace の発行キットを
参照してください。

このシナリオを解決するには、以下のリソースを参照してください。

-   [アプリケーション発行キットのダウンロード](http://go.microsoft.com/fwlink/?LinkId=221323)
-   [アプリケーション用の Azure Marketplace の紹介](https://datamarket.azure.com/)

##セキュリティのヒント

このセクションでは、Windows Identity Foundation と
Azure AD Access Control のセキュリティのヒントについて説明します。 アプリケーションを設計およびデプロイするときに、
テクノロジの基本的なセキュリティ チェックリストとして使用してください。

###Windows Identity Foundation

WIF に関する主なセキュリティのヒントは次のとおりです。 この情報は、
」から [WIF の設計に関する考慮事項](http://msdn.microsoft.com/library/ee517298.aspx) と [Windows Identity Foundation
(WIF)セキュリティを ASP.NET Web アプリケーションの脅威と対策](http://blogs.msdn.com/b/alikl/archive/2010/12/02/windows-identity-foundation-wif-security-for-asp-net-web-applications-threats-amp-countermeasures.aspx)
.

-   **IssuerNameRegistry**します。 信頼済み Security Token Services
    (STS) を指定します。 信頼済み STS のみがリストされていることを確認します。
-   **cookieHandler requireSsl ="true"**します。 セッション
    Cookie を SSL プロトコルで転送することを指定します。
-   **wsfederation's requireHttps ="true"**します。 ID プロバイダーとの
    フェデレーション プロトコル通信を SSL プロトコル経由で行うことを
    指定します。
-   **有効になっている tokenReplayDetection ="true"**します。 トークン再生検出機能を
    有効にすることを指定します。 この機能は、
    使用されたトークンのローカル コピーの管理時にサーバー アフィニティを作成することに注意してください。
-   **audienceUris**します。 トークンの想定されている読者を指定します。 アプリケーションが
    想定しないトークンを受け取ると、
    WIF によって拒否されます。
-   **requestValidation** と **httpRuntime requestValidationType**します。
    ASP.NET 検証機能を有効または無効にします。 「
    説明されている [Windows Identity Foundation (WIF): 潜在的 A
    危険な Request.Form 値が、クライアントから検出されました](http://social.technet.microsoft.com/wiki/contents/articles/1725.windows-identity-foundation-wif-a-potentially-dangerous-request-form-value-was-detected-from-the-client-wresult-t-requestsecurityto.aspx)

###Azure AD Access Control

Azure AD Access Control のデプロイにおいては、
次のセキュリティに関するヒントを検討してください。 この情報はからの抜粋 [ACS セキュリティ
ガイドライン](http://msdn.microsoft.com/library/gg185962.aspx) と [証明書とキーの管理ガイドライン](http://msdn.microsoft.com/library/hh204521.aspx)します。

-   **STS トークンの有効期限**します。 Azure AD Access Control
    の管理ポータルを使用して、トークンの有効期限を短く設定します。
-   **エラー URL 機能の使用時にデータを検証**します。 Azure
    AD Access Control の Error URL 機能では、
    エラー メッセージを送信するアプリケーションのページへの匿名アクセスが必要です。 このページに届くすべてのデータは、
    信頼できないソースからの危険なデータであると想定します。
-   **機密性の高いシナリオ用のトークンを暗号化**します。 トークン上で
    利用できる情報が漏えいする脅威を軽減するため、
    トークンを暗号化することを考慮してください。
-   **Azure に展開するときに RSA を使用して cookie を暗号化する**です。
    WIF は既定で DPAPI を使用して Cookie を暗号化します。 Web ファームおよび Azure 環境にデプロイするときに、
    サーバー アフィニティが作成され、
    例外が発生する場合があります。 Web ファームや Azure のシナリオでは、RSA で 
    代用します。
-   **トークン署名証明書**します。 サービス拒否を防ぐため、トークン署名証明書は
    定期的に更新してください。 Azure AD Access
    Control は、発行するすべてのセキュリティ トークンに署名します。 X.509 証明書は、
    ACS が発行する SAML トークンを使用するアプリケーションをビルドするときに
    署名のために使用されます。 署名証明書の有効期限が切れると、
    トークンの要求時にエラーが発生します。
-   **トークン署名キー**します。 サービス拒否を防ぐため、トークン署名キーは定期的に更新してください。
    定期的に更新してください。 Azure AD Access Control は、
    発行するすべてのセキュリティ トークンに署名します。 256 ビット対称署名キーは、
    ACS が発行する SWT トークンを使用するアプリケーションをビルドするときに
    使用されます。 署名キーの有効期限が切れると、
    トークンの要求時にエラーが発生します。
-   **トークン暗号化証明書**します。 サービス拒否を防ぐため、トークン暗号化証明書は
    定期的に更新してください。 トークン
    暗号化は、証明書利用者のアプリケーションが WS-Trust プロトコル上の
    proof-of-possession トークンを使用する Web サービスである場合に必須になります。
    それ以外の場合、トークン暗号化は任意です。 暗号化
    証明書の有効期限が切れると、トークンの要求時にエラーが発生
    します。
-   **トークン復号化証明書**します。 サービス拒否を防ぐため、トークン復号化証明書は
    定期的に更新してください。 Azure
    AD Access Control は WS-Federation ID プロバイダー (たとえば AD FS 2.0) の
    暗号化されたトークンを受け付けることができます。 Azure AD Access Control でホストされる X.509 証明書は、
    復号化に使用されます。
    復号化証明書の有効期限が切れると、
    トークンの要求時にエラーが発生します。
-   **サービス id 資格情報**します。 サービス拒否を防ぐため、サービス ID 資格情報は
    定期的に更新してください。 サービス ID とは、
    Azure AD Access Control 名前空間でグローバルに構成される資格情報で、
    これを使ってアプリケーションまたはクライアントは
    Azure AD Access Control で直接認証してトークンを受け取ることが
    できます。 Azure AD Access Control サービス ID に
    AD Access Control サービス id に関連付けることができます対称。
    キー、パスワード、および X.509 証明書です。 資格情報の有効期限が切れると、
    例外を受け取ります。
-   **Azure AD Access Control 管理サービス アカウント
    資格情報**します。 サービス拒否を防ぐため、管理サービス資格情報は
    定期的に更新してください。 Azure AD Access Control
    管理サービスでは、Azure AD Access Control 名前空間用の設定を、
    プログラムによって管理および構成 
    できます。 管理サービス アカウントに関連付けることのできる資格情報は
    3 種類あります。 それは、
    対称キー、パスワード、および X.509 証明書です。 次の
    例外を受け取ります。
-   **Ws-federation id プロバイダーの署名と暗号化
    証明書**します。 サービス拒否を防ぐため、
    WS-Federation ID プロバイダーの証明書の有効性を照会します。 WS-Federation
    ID プロバイダー証明書は、そのメタデータを介して利用できます。
    AD FS などの WS-Federation ID プロバイダーの構成時に、
    WS-Federation 署名証明書は、
    URL の一部またはファイルとして利用できる WS-Federation メタデータを介して構成されます。 さらに、
    Azure AD Access Control 管理サービスを使用して
    その証明書の有効性を
    照会します。 証明書の有効期限が切れると、
    例外を受け取ります。

##Azure Websites を使用する共有ホスティング

このトピックで説明したすべてのシナリオおよびソリューションは、
Azure Websites でアプリケーションをホストする場合に有効です。

##Azure Virtual Machines

このトピックで説明したすべてのシナリオおよびソリューションは、
Azure Virtual Machines でアプリケーションをホストする場合に有効です。

##リソース

-   [Identity Developer Training Kit](http://go.microsoft.com/fwlink/?LinkId=214555)
-   [MSDN 提供の Identity Developer Training Course](http://go.microsoft.com/fwlink/?LinkId=214561)
-   [要求ベースの ID およびアクセス制御のガイド](http://go.microsoft.com/fwlink/?LinkId=214562)
-   [Access Control Service](http://msdn.microsoft.com/library/windowsazure/gg429786.aspx)
-   [ACS の利用方法](http://msdn.microsoft.com/library/windowsazure/gg185939.aspx)
-   [Access Control Service v2.0 を使用して、Azure Web ロール ASP.NET Web アプリケーションをセキュリティ保護する](http://social.technet.microsoft.com/wiki/contents/articles/2590.aspx)
-   [Azure AD Access Control Service (ACS) Academy のビデオ](http://social.technet.microsoft.com/wiki/contents/articles/2777.aspx)
-   [Microsoft Security Development Lifecycle (Microsoft セキュリティ開発ライフサイクル)](http://www.microsoft.com/security/sdl/default.aspx)
-   [SDL Threat Modeling Tool 3.1.8](http://www.microsoft.com/download/en/details.aspx?displaylang=en&id=2955)
-   [セキュリティおよびプライバシーのブログ](http://www.microsoft.com/about/twc/en/us/blogs.aspx)
-   [Security Response Center (セキュリティ対応センター)](http://www.microsoft.com/security/msrc/default.aspx)
-   [Security Intelligence Report (セキュリティ インテリジェンス レポート)](http://www.microsoft.com/security/sir/)
-   [Security Development Lifecycles (セキュリティ開発ライフサイクル)](http://www.microsoft.com/security/sdl/default.aspx)
-   [セキュリティ デベロッパー センター (MSDN)](http://msdn.microsoft.com/security/)


[01]:./media/SecurityRX/01_SecuringTheApplication.gif
[02]:./media/SecurityRX/02_ThreatsVulnerabilitiesandAttacks.gif
[03]:./media/SecurityRX/03_WindowsAzureADAccesscontrol.gif
[04]:./media/SecurityRX/04_WCF(SOAP)Service.gif
[05]:./media/SecurityRX/05_AzureADAccessControl.gif
[06]:./media/SecurityRX/06_RESTService.gif
[07]:./media/SecurityRX/07_WIFisOptional.gif
[08]:./media/SecurityRX/08_ASPNETWebApptoREST.gif
[09]:./media/SecurityRX/09_RBAC.gif
[10]:./media/SecurityRX/10_WIFClaimsAuthenticationManager.gif
[11]:./media/SecurityRX/11_SecurityTokenRequriementmapping.gif
[12]:./media/SecurityRX/12_CustomRoleManager.gif
[13]:./media/SecurityRX/13_ClaimsAuthorizationManager.gif
[14]:./media/SecurityRX/14_WindowsAzurestorage.gif
[15]:./media/SecurityRX/15_SQLAzureIdentityandAccessScenarios.gif
[16]:./media/SecurityRX/16_WindowsAzureServiceBusIdentity.gif
[17]:./media/SecurityRX/17_WindowsAzureCacheIdentity.gif
[18]:./media/SecurityRX/18_IAccessMyDataset.gif
[19]:./media/SecurityRX/19_UsersAccessMyDatasets.gif
[20]:./media/SecurityRX/20_ApplicationAccessMarketplaceAPI.gif

[Web SSO Design]: http://technet.microsoft.com/library/dd807033(WS.10).aspx
[Federated Web SSO Design]: http://technet.microsoft.com/library/dd807050(WS.10).aspx

