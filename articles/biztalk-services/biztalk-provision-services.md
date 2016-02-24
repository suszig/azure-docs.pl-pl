<properties
    pageTitle="Azure ポータルで Azure BizTalk Services を作成する | Microsoft Azure"
    description="Azure ポータルで BizTalk Services をプロビジョニングし、作成する方法について説明します。MABS、WABS"
    services="biztalk-services"
    documentationCenter=""
    authors="MandiOhlinger"
    manager="dwrede"
    editor="cgronlun"/>

<tags
    ms.service="biztalk-services"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="12/01/2015"
    ms.author="mandia"/>



# Azure ポータルを使用して BizTalk Services を作成する

この記事では、Azure ポータルで Azure BizTalk Services を作成する方法を説明します。

> [AZURE.TIP] Azure ポータルにサインインするには、Azure アカウントと Azure サブスクリプションが必要です。 アカウントがない場合は、無料の試用アカウントを数分で作成することができます。 参照してください [Azure 無料評価版](http://go.microsoft.com/fwlink/p/?LinkID=239738)します。

## BizTalk サービスを作成する
選択したエディションによっては、BizTalk サービスの設定の一部が使用できないことがあります。

1. [Azure ポータル](http://go.microsoft.com/fwlink/p/?LinkID=213885)にサインインします。
2. 下部のナビゲーション ウィンドウで [ **新規**:  
![新しいボタンをクリックします。][NEWButton]

3. 選択 **APP SERVICES** > **BIZTALK サービス** > **カスタム作成**:  
![BizTalk サービスを選択し、[カスタム作成][NewBizTalkService]

4. BizTalk サービスの設定を入力します。

    <table border="1">
    <tr>
    <td><strong>BizTalk サービス名</strong></td>
    <td>どのような名前でも入力できますが、具体的な名前を入力してください。 次に例をいくつか示します。<br/><br/>
    <em>mycompany</em>.biztalk.windows.net<br/>
    <em>mycompanymyapplication</em>.biztalk.windows.net<br/>
    <em>myapplication</em>.biztalk.windows.net<br/><br/>入力した名前には ".biztalk.windows.net" が自動的に追加されます。 これが作成のように、BizTalk サービスにアクセスするための URL <strong>https://<em>myapplication</em>.biztalk.windows.net</strong>.
    </td>
    </tr>
    <tr>
    <td><strong>エディション</strong></td>
    <td>テスト/開発フェーズを選択します。 <strong>Developer</strong>. 運用フェーズの場合を使用して、 <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302279">BizTalk サービス: 開発者、基本、標準、およびプレミアム エディションのチャート</a> かどうかを <strong>Premium</strong>。 <strong>Standard</strong>, or <strong>基本</strong> ビジネス シナリオの適切な選択肢です。
    </td>
    </tr>
    <tr>
    <td><strong>リージョン</strong></td>
    <td>BizTalk サービスをホストするリージョンを選択します。</td>
    </tr>
    <tr>
    <td><strong>ドメイン URL</strong></td>
    <td><strong>省略可能</strong>. ドメインの URL は、既定では、します。 <em>YourBizTalkServiceName</em>。 biztalk.windows.net します。 カスタム ドメインを入力することもできます。 たとえば、ドメインが <em>contoso</em>を入力することができます。 <br/><br/>
    <em>MyCompany</em>。 contoso.com<br/>
    <em>MyCompanyMyApplication</em>。 contoso.com<br/>
    <em>MyApplication</em>。 contoso.com<br/>
    <em>YourBizTalkServiceName</em>。 contoso.com<br/>
    </td>
    </tr>
    </table>
次へ進む矢印を選択します。

5. ストレージとデータベースの設定を入力します。

    <table border="1">
    <tr>
    <td><strong>ストレージ アカウントの監視/アーカイブ</strong></td>
    <td>既存のストレージ アカウントを選択するか、新しいストレージ アカウントを作成します。 <br/><br/>新しいストレージ アカウントを作成する場合は、入力、 <strong>ストレージ アカウント名</strong>.</td>
    </tr>
    <tr>
    <td><strong>トラッキング データベース</strong></td>
    <td>既存の Azure SQL データベースを使用する場合に、そのデータベースを別の BizTalk サービスでも使用することはできません。 Azure SQL データベース サーバーの作成時に入力したログイン名とパスワードが必要です。<br/><br/><strong>ヒント</strong> BizTalk サービスと同じリージョンに、トラッキング データベースと監視/アーカイブ ストレージ アカウントを作成します。</td>
    </tr>
    </table>
次へ進む矢印を選択します。

6. データベースの設定を入力します。

    <table border="1">
    <tr>
    <td><strong>名前</strong></td>
    <td>場合に使用できます。 <strong>新しい SQL データベース インスタンスを作成します。</strong> 前の画面で選択されます。
    <br/><br/>
    BizTalk サービスで使用される SQL データベース名を入力します。</td>
    </tr>
    <tr>
    <td><strong>サーバー</strong></td>
    <td>場合に使用できます。 <strong>新しい SQL データベース インスタンスを作成します。</strong> 前の画面で選択されます。
    <br/><br/>
    既存の SQL データベース サーバーを選択するか、新しい SQL データベース サーバーを作成します。</td>
    </tr>
    <tr>
    <td><strong>サーバー ログイン名</strong></td>
    <td>ログイン ユーザー名を入力します。</td>
    </tr>
    <tr>
    <td><strong>サーバー ログイン パスワード</strong></td>
    <td>ログイン パスワードを入力します。</td>
    </tr>
    <tr>
    <td><strong>リージョン</strong></td>
    <td>場合に使用できます。 <strong>新しい SQL データベース インスタンスを作成します。</strong> 選択されます。 SQL データベースをホストするリージョンを選択します。</td>
    </tr>
    </table>

チェック マークをオンにして、ウィザードを完了します。 進行状況アイコンが表示されます。  
![完了時に表示される進行状況アイコン][ProgressComplete]

完了すると、Azure BizTalk サービスが作成され、アプリケーションで使用できる状態になります。 既定の設定で十分です。 を既定の設定を変更する場合は、選択 **BIZTALK サービス** 左側のナビゲーション ウィンドウで、BizTalk サービスを選択します。 その他の設定が表示されます、 [ダッシュ ボード、監視、およびスケール タブ](biztalk-dashboard-monitor-scale-tabs.md) 上部にあります。

BizTalk サービスの状態によっては、一部の操作を完了できません。 これらの操作の一覧を参照してください [BizTalk サービスの状態のチャート](biztalk-service-state-chart.md)します。


## プロビジョニング後の手順

-  [証明書をローカル コンピューターにインストールする](#InstallCert)
-  [運用対応証明書を追加する](#AddCert)
-  [Access Control 名前空間を取得する](#ACS)

#### <a name="InstallCert"></a>証明書をローカル コンピューターにインストールする
BizTalk サービスをプロビジョニングするときに、自己署名証明書が作成されて BizTalk サービス サブスクリプションに関連付けられます。 コンピューターから BizTalk サービス アプリケーションをデプロイする、または BizTalk サービス エンドポイントにメッセージを送信するには、この証明書をダウンロードしてそのコンピューターにインストールする必要があります。

1. [Azure ポータル](http://go.microsoft.com/fwlink/p/?LinkID=213885)にサインインします。
2. 選択 **BIZTALK サービス** 左側のナビゲーション ウィンドウにし、BizTalk サービス サブスクリプションを選択します。
3. 選択、 **ダッシュ ボード** ] タブをクリックします。
4. 選択 **SSL 証明書のダウンロード**:  
![SSL 証明書を変更します。][QuickGlance]
5. 証明書をダブルクリックし、ウィザードを最後まで実行して証明書をインストールします。 [証明書をインストールするかどうかを確認、 **信頼されたルート証明機関** を格納します。

#### <a name="AddCert"></a>運用対応証明書を追加する
BizTalk サービスの作成時に自動的に作成される自己署名証明書は、開発環境に限定して使用するためのものです。 本稼働のときは、この証明書を運用対応証明書で置き換えます。

1.  **ダッシュ ボード** ] タブで [ **SSL 証明書の更新**します。
2. プライベート SSL 証明書 (*CertificateName*.pfx) BizTalk サービス名を含む、パスワードを入力し、チェック マークをクリックします。

#### <a name="ACS"></a>Access Control 名前空間を取得する

1. [Azure ポータル](http://go.microsoft.com/fwlink/p/?LinkID=213885)にサインインします。
2. 選択 **BIZTALK サービス** 左側のナビゲーション ウィンドウにし、BizTalk サービスを選択します。
3. タスク バーで [ **接続情報**:  
![接続情報] を選択します。][ACSConnectInfo]

4. Access Control の値をコピーします。

Visual Studio から BizTalk サービス プロジェクトを展開するときは、この Access Control 名前空間を入力します。 BizTalk サービスの Access Control 名前空間が自動的に作成されます。

Access Control の値は、どのアプリケーションでも使用できます。 Azure BizTalk サービスが作成されると、この Access Control 名前空間によって BizTalk サービス デプロイメントでの認証が制御されます。 サブスクリプションを変更または名前空間の管理でを選択する場合 **ACTIVE DIRECTORY** 左側のナビゲーション ウィンドウで、名前空間を選択します。 タスク バーに、選択可能なオプションが表示されます。

クリックすると **管理** Access Control 管理ポータルを開きます。 BizTalk サービスを使用して Access Control 管理ポータルで **サービス id**:  
![ACS サービス Id、access Control 管理ポータル][ACSServiceIdentities]

Access Control サービス ID は、アプリケーションまたはクライアントが Access Control で直接認証してトークンを受け取るための一連の資格情報です。

> [AZURE.IMPORTANT] BizTalk サービスを使用して **所有者** の既定のサービス id、および **パスワード** 値。 パスワードの値の代わりに対称キーの値を使用する場合、次のエラーが発生する<br/><br/>*。指定された資格情報を持つ Access Control 管理サービス アカウントに接続できませんでした。*

[ACS 名前空間の管理](https://msdn.microsoft.com/library/azure/hh674478.aspx) いくつかのガイドラインと推奨事項を一覧表示します。

## 要件の説明

これらの要件は、無料エディションには適用されません。
<table border="1">
<tr bgcolor="FAF9F9">
        <td><strong>必要なもの</strong></td>
        <td><strong>必要である理由</strong></td>
</tr>
<tr>
<td>Azure サブスクリプション</td>
<td>サブスクリプションでは、Azure ポータルにサインインしたことを決定します。 アカウント保有者でサブスクリプションを作成します。 <a HREF="https://account.windowsazure.com/Subscriptions"> Azure サブスクリプション</a>.
<br/><br/>
Azure のアカウントは、複数のサブスクリプションが含まれると、許可されているすべてのユーザーが管理することができます。 たとえば、Azure アカウント保有者がという名前のサブスクリプションを作成します。 <em>BizTalkServiceSubscription</em> このサブスクリプションへの (たとえば、ContosoBTSAdmins@live.com)、社内の BizTalk 管理者アクセスを提供します。 このシナリオでは、BizTalk 管理者は、Azure ポータルにサインインすると、Azure BizTalk Services を含む、サブスクリプション内のすべてのホステッド サービスに対する完全な管理者権限を持つことになります。 BizTalk 管理者は Azure アカウント保有者ではないため、課金情報へはアクセスできません。
<br/><br/>
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=267577"> Azure ポータルでサブスクリプションとストレージ アカウントを管理します。</a> 詳しくを説明します。
</td>
</tr>
<tr>
<td>Azure SQL Database</td>
<td>テーブル、ビュー、および追跡データを含む BizTalk サービスによって使用されるストアド プロシージャを格納します。
<br/><br/>
BizTalk サービスを作成する場合は、既存 Azure SQL サーバー、Azure SQL Database を使用するか、新しいサーバーやデータベースを自動的に作成します。
<br/><br/>
SQL データベースのスケールは自動的に構成します。 一般的に、BizTalk サービスのスケールは既定の設定で十分です。 スケールを変更すると、料金に影響が及びます。 手順については、「 <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=234930"> Azure SQL データベースのアカウントと課金</a>
<br/><br/>
<strong>メモ</strong>
<br/>
<ul>
<li> 新しい Azure SQL サーバーとデータベースを作成すると、Azure サービスが自動的に有効になります。 BizTalk サービスを使用するには、Azure サービスが有効になっている必要があります。</li>
<li>既存の Azure SQL サーバー上に新しい Azure SQL データベースを作成した場合に、サーバーのファイアウォール ルールが変更されることはありません。 その結果として、他の Azure サービスがこのサーバーのデータベースにアクセスできなくなる可能性があります。</li>
</ul>
</td>
</tr>
<tr>
<td>Azure の Access Control 名前空間</td>
<td>Azure BizTalk サービスで認証を行います。 Visual Studio から BizTalk サービス プロジェクトを展開するときは、この Access Control 名前空間を入力します。 BizTalk サービスを作成するときに Access Control 名前空間が自動的に作成します。</td>
</tr>

<tr>
<td>Azure ストレージ アカウント</td>
<td>次のような BizTalk サービスのデータを保存するためのテーブル、BLOB、およびキューにアクセスできるようになります。

<ul>
<li>BizTalk サービスを監視するログ ファイル。 監視出力にも表示されます、 **監視** Azure ポータル] タブをクリックします</li>
<li>。X12 またはパートナー間の AS2 アグリーメントを作成するには、メッセージ プロパティを格納する、アーカイブ機能を有効にできます。 このデータは、ストレージ アカウントに保存されます。</li>
</ul>
<br/>
BizTalk サービスを作成するときに、既存のストレージ アカウントを使用することも、新しいストレージ アカウントを自動的に作成することもできます。
<br/><br/>
BizTalk サービスのストレージ設定は、既定の設定で十分です。
<br/><br/>
ストレージ アカウントを作成すると、プライマリ キーおよびセカンダリ キーが自動的に作成されます。 これらのキーは、ストレージ アカウントへのアクセスを制御します。 BizTalk サービスは自動的にプライマリ キーを使用します。
<br/><br/>
参照してください <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=285671"> ストレージ</a> の詳細。
</td>
</tr>

<tr>
<td>SSL プライベート証明書</td>
<td>
Azure BizTalk サービスが作成されるときに、BizTalk サービス名が含まれている HTTPS URL も作成されます。 この URL は自動的に、開発専用の自己署名証明書を使用するように構成されます。 本稼働用には、プライベート SSL 証明書が必要です。
<br/><br/>
<strong>重要な SSL 証明書情報</strong>

<ul>
<li>証明書の有効期限は、5 年未満にしてください。</li>
<li>プライベート証明書はすべて、パスワードを必要とします。 このパスワードを知っている必要があります。また、他の管理者にこのパスワードを知らせておくことをお勧めします。</li>
<li>自己署名証明書は、テスト/開発環境で使用されます。 自己署名証明書を使用するときは、証明書を [個人用] 証明書ストアと [信頼されたルート証明機関] 証明書ストアにインポートします。</li>
</ul>
<br/>運用証明書要求を証明機関に送信するときに、次の証明書プロパティを指定します。
<br/>

<ul>
<li><strong>拡張キー使用法</strong>: 少なくとも、Azure BizTalk サービスには、サーバー認証が必要です。</li>
<li><strong>共通名</strong>: Azure BizTalk サービス URL の完全修飾ドメイン名 (FQDN) を入力します。 手順については、「 <a HREF="#BizTalk">BizTalk サービスを作成する</a> 参照してください。</li>
</ul>
<br/>
BizTalk サービスが作成された後で、新しい、または別の証明書を追加できます。
</td>
</tr>
</table>



## ハイブリッド接続と

Azure BizTalk サービスを作成するときに、 **ハイブリッド接続** タブは表示されます。

![[ハイブリッド接続] タブ][HybridConnectionTab]

"ハイブリッド接続" を使用すると、Azure Web サイトや Azure モバイル サービスと、静的 TCP ポートを使用する内部設置型リソース (たとえば SQL Server、MySQL、HTTP Web API、Mobile Services、ほとんどのカスタム Web サービス) とを接続できます。  ハイブリッド接続と BizTalk Adapter サービスは別のものです。 BizTalk Adapter サービスは、Azure BizTalk サービスを内部設置型の基幹業務 (LOB) システムに接続するために使用されます。

 参照してください [ハイブリッド接続](integration-hybrid-connection-overview.md) を作成して、ハイブリッド接続の管理などの詳細を参照してください。


## 次のステップ

これで BizTalk サービスを作成すると、理解、異なる [BizTalk サービス: ダッシュ ボード、監視、およびスケール] タブ](biztalk-dashboard-monitor-scale-tabs.md)します。 BizTalk サービスをアプリケーションで使用する準備ができました。 アプリケーションの作成を開始するには [Azure BizTalk サービス](http://go.microsoft.com/fwlink/p/?LinkID=235197)します。

## 関連項目
- [BizTalk サービス: エディションのチャート](biztalk-editions-feature-chart.md)<br/>
- [BizTalk サービス: サービスの状態のチャート](biztalk-service-state-chart.md)<br/>
- [BizTalk サービス: バックアップと復元](biztalk-backup-restore.md)<br/>
- [BizTalk サービス: 調整](biztalk-throttling-thresholds.md)<br/>
- [BizTalk サービス: 発行者名および発行者キー](biztalk-issuer-name-issuer-key.md)<br/>
- [Azure BizTalk サービス SDK の操作使用開始方法](http://go.microsoft.com/fwlink/p/?LinkID=302335)<br/>
- [ハイブリッド接続と](integration-hybrid-connection-overview.md)

[NewBizTalkService]: ./media/biztalk-provision-services/WABS_NewBizTalkService.png
[NEWButton]: ./media/biztalk-provision-services/WABS_New.png
[ProgressComplete]: ./media/biztalk-provision-services/WABS_ProgressComplete.png
[ACSConnectInfo]: ./media/biztalk-provision-services/WABS_ACSConnectInformation.png
[QuickGlance]: ./media/biztalk-provision-services/WABS_QuickGlance.png
[ACSServiceIdentities]: ./media/biztalk-provision-services/WABS_ACSServiceIdentities.png
[HybridConnectionTab]: ./media/biztalk-provision-services/WABS_HybridConnectionTab.png

