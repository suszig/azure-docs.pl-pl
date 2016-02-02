<properties
    pageTitle="クラウド内のオンプレミスの ID インフラストラクチャの監視"
    description="これは、Azure AD Connect Health の内容と使用する理由を説明するページです。"
    services="active-directory"
    documentationCenter=""
    authors="billmath"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="12/16/2015"
    ms.author="billmath"/>


# クラウド内のオンプレミスの ID インフラストラクチャと同期サービスの監視

Azure AD Connect Health では、オンプレミスの ID インフラストラクチャと、Azure AD Connect から使用できる同期サービスを監視、分析できます。 アラート、パフォーマンス、使用パターン、構成設定を表示する機能があり、Office 365 に対して信頼性の高い接続を維持するなどさまざまなことができます。 これらは、対象のサーバーにインストールされているエージェントを使用して実現されます。

![What is Azure AD Connect Health](./media/active-directory-aadconnect-health/aadconnecthealth2.png)


この情報は、すべて Azure AD Connect Health ポータルで参照できます。 Azure AD Connect Health ポータルを使用すると、アラート、パフォーマンスの監視、および使用状況の分析を表示できます。 この情報はすべて 1 つの使いやすい場所に表示されるので、必要な情報を探して時間を無駄にすることはありません。

![What is Azure AD Connect Health](./media/active-directory-aadconnect-health/usage.png)

Azure AD Connect Health の今後の更新には、他の ID コンポーネントの監視や分析が追加される予定です。 このように ID というレンズを通した単一のダッシュボードが提供されることで、より堅牢で健全な統合環境を設定できるようになります。ユーザーはこの環境を利用して作業能力を向上させることができます。


<center>![Azure AD Connect Health とは](./media/active-directory-aadconnect-health/logo1.png)</center>




## Azure AD Connect Health を使用する理由

オンプレミスのディレクトリと Azure AD を統合すると、クラウドとオンプレミス両方のリソースにアクセスするための共通の ID が提供されるため、ユーザーの生産性が向上します。 ただし、この統合には課題があります。ユーザーが任意のデバイスからオンプレミスとクラウド両方のリソースに安全にアクセスできるように、環境を健全に保つ必要があります。 Azure AD Connect Health では、Office 365 や他の Azure AD アプリケーションにアクセスするときに使用するオンプレミスの ID インフラストラクチャを、簡単なクラウドベースのアプローチで監視し分析することができます。 オンプレミスの各 ID サーバーにエージェントをインストールするぐらいにシンプルです。

Azure AD Connect Health for AD FS は、Windows Server 2008/2008 R2 の AD FS 2.0 と Windows Server 2012/2012R2 の AD FS をサポートします。 これらには、エクストラネット アクセスの認証をサポートする AD FS プロキシまたは Web アプリケーション プロキシ サーバーも含まれます。 Azure AD Connect Health for AD FS の主な機能セットは次のとおりです。

- Azure AD などの AD FS が保護するアプリケーションへの信頼性の高いアクセスを保証するためにアラートを表示し対処する
- 重大なアラートの電子メール通知を送信する
- 容量計画を決定するパフォーマンス データを表示する
- 異常を判定するため、または容量計画のベースラインを確立するために AD FS ログイン パターンを詳細に表示する


Azure AD Connect Health for Sync は、オンプレミスの Active Directory と Azure Active Directory の間で実行された同期を監視し、情報を提供します。 Azure AD Connect Health for Sync の主な機能は次のとおりです。

- オンプレミスのインフラストラクチャと Azure Active Directory の間で信頼性の高い同期が実行されるように、アラートを表示し、対処する
- 重大なアラートの電子メール通知を送信する
- パフォーマンス データを表示する

次のビデオでは、Azure AD Connect Health の概要について説明します。

[AZURE.VIDEO azure-ad-connect-health--monitor-you-identity-bridge]



## Azure ポータルでの作業開始

Azure Active Directory Connect Health の使用を開始するには、次の手順に従います。

1. へのサインイン [Microsoft Azure ポータル](https://portal.azure.com/)。
2. Azure Active Directory Connect Health にアクセスするには、Marketplace に移動して検索するか、Marketplace を選択して [セキュリティ + ID] を選択します。
3. 概要ブレード (ブレードは、全体的なビューの 1 つの部分です。 ウィンドウまたはフライアウトと考えることができます) で、**[作成]** をクリックします。 ディレクトリの情報を含む別のブレードが開きます。
4. ディレクトリのブレードで、**[作成]** をクリックします。 Azure Active Directory Premium ライセンスをお持ちでない場合、Azure AD Connect Health を使用するにはこのライセンスが必要になります。 Azure AD Premium の詳細については、Azure AD Premium の概要に関するページを参照してください。

>[AZURE.NOTE]Azure AD Connect Health のインスタンスでデータを表示するためには、あらかじめ対象サーバーに Azure AD Connect Health エージェントをインストールしておく必要があります。 Azure AD Connect Health エージェントをダウンロードするには、最初のブレードで [クイック スタート]、[ツールの取得] の順に選択します。 使用して直接エージェントをダウンロードすることも、 [リンク](#download-the-agent) 以下です。 Azure Active Directory Connect Health を使用するには、次の手順に従います。



### Azure AD Connect Health ポータルおよびサービス

Azure AD Connect Health ポータルを使用すると、アラート、パフォーマンスの監視、および使用状況の分析を表示できます。 Azure AD Connect Health に初めてアクセスすると、最初のブレードが表示されます。 ブレードは、ウィンドウと考えることができます。 最初のブレードには、クイック スタート、サービス、構成が表示されます。 スクリーン ショットの下に、それぞれの簡単な説明を示します。 サービスのセクションには、Azure AD Connect Health が監視しているアクティブなサービスとそのサービスのインスタンスが表示されます。

![Azure AD Connect Health Portal](./media/active-directory-aadconnect-health/portal2.png)

- **[クイック スタート]** – これを選択すると [クイック スタート] ブレードが開きます。 ここで、[ツールの取得] を選択して Azure AD Connect Health エージェントをダウンロードしたり、ドキュメントにアクセスしたり、フィードバックを提供したりできます。
- **[Active Directory フェデレーション サービス]** – Azure AD Connect Health が現在監視しているすべての AD FS サービスを表します。 いずれかのインスタンスを選択すると、ブレードが開き、そのサービス インスタンスに関する情報が表示されます。 この情報には、概要、プロパティ、アラート、監視、使用状況の分析などが含まれます。
- [構成] – 以下の機能のオン/オフを切り替えることができます。
<ol>
1. Azure AD Connect Health エージェントを最新バージョンに自動更新する - Azure AD Connect Health エージェントの最新バージョンが使用可能になると、自動的に更新されます。 この機能は、既定では有効になっています。
2. Microsoft がトラブルシューティングの目的でのみ Azure AD ディレクトリの正常性データにアクセスすることを許可する - この機能が有効になっていると、ユーザーに表示されているデータと同じデータを Microsoft でも表示できます。 これは問題のトラブルシューティングと支援に役立ちます。 この機能は、既定では無効になっています。




## 必要条件

次の表に、Azure AD Connect Health を使用するための要件を示します。

| 要件| 説明|
| ----------- | ---------- |
| Azure AD Premium| Azure AD Connect Health は Azure AD Premium の機能です。使用するためには Azure AD Premium が必要となります。</br></br>詳細については、次を参照してください [Azure AD Premium の概要](active-directory-get-started-premium.md)。</br>。</br>開始 30 日間の無料評価版を参照してください [の評価を開始します](https://azure.microsoft.com/trial/get-started-active-directory/)。| .
| Azure AD Connect Health を有効にする (作成する) には、Azure AD のグローバル管理者であること| 既定では、Azure AD Connect Health を有効化 (作成) し、Azure AD Connect Health 内のすべてのすべての情報にアクセスしたり、すべての操作を実行したりできるのは、グローバル管理者のみです。追加情報を参照してください。 [Azure AD ディレクトリの管理](active-directory-administer.md)します。<br><br> ロールベースのアクセス制御を使用して、組織内の Azure AD Connect Health を他のユーザーへのアクセスを許可できます。詳細については、を参照してください [ロールベースのアクセス制御の Azure AD Connect Health](active-directory-aadconnect-health-operations.md#manage-access-with-role-based-access-control)。</br></br>**重要:** エージェントをインストールするときに使用するアカウントは、職場アカウントまたは組織アカウントにする必要があり、Microsoft アカウントであることはできません。詳細については、を参照してください [組織として Azure にサインアップ](sign-up-organization.md)|
| AD FS で利用状況分析機能を使用する場合、AD FS 監査が有効になっていること| AD FS で利用状況分析機能を使用する予定がある場合は、AD FS の監査を有効にする必要があります。</br></br>を参照してください [、Azure AD Connect のインストールの AD FS 用の正常性エージェント](active-directory-aadconnect-health-agent-install.md#installing-the-azure-ad-connect-health-agent-for-ad-fs)。
| Azure AD Connect Health エージェントの要件を満たしていること| エージェント固有の要件については、次の表を参照してください。

次の表に、Azure AD Connect Health を使用するためのエージェントの要件を示します。

| 要件| 説明|
| ----------- | ---------- |
| Azure AD Connect Health エージェントが個々のサーバーにインストールされていること| ポータルに表示されるデータを Azure AD Connect Health から得るためには、対象となるサーバーにエージェントがインストールされている必要があります。</br></br>など、AD FS オンプレミス インフラストラクチャにデータを取得するのには、エージェントを AD FS サーバーにインストールする必要があります。これには AD FS プロキシ サーバーや Web アプリケーション プロキシ サーバーが含まれます。 </br></br>エージェントのインストールに関する情報を参照してください、 [Azure AD 接続正常性エージェントのインストール](active-directory-aadconnect-health-agent-install.md).</br></br>**重要:** エージェントをインストールするときに使用するアカウントは、職場アカウントまたは組織アカウントにする必要があり、Microsoft アカウントであることはできません。 詳細については、を参照してください [組織として Azure にサインアップ](sign-up-organization.md)|
| Azure AD Connect Health for Sync エージェント| このエージェントは、Azure AD Connect の最新バージョンと共に自動的にインストールされます。</br></br>起動した場合は、その他の作業する必要はありません。Azure AD Connect をインストールするときに、エージェントをインストールします。</br></br>既にインストールされている Azure AD Connect がある場合は、ダウンロードできる最新バージョンにアップグレードする必要があります。 [ここ](http://www.microsoft.com/download/details.aspx?id=47594)します。
| Azure サービスのエンドポイントに対する送信接続| エージェントをインストールしたり実行したりするためには、以下に示した、Azure AD Connect Health サービスのエンド ポイントへの接続が必要となります。接続が許可リストに、次の項目が追加されたことを確認する送信をブロックする場合: </br></br><li>**新しい**: https://management.azure.com </li><li>**新しい**: & #42;。>.blob.core.windows.net </li><li>**新しい**: & #42;。queue.core.windows.net</li><li>& #42;。.servicebus.windows.net - ポート: 5671</li><li>https://&#42;.adhybridhealth.azure.com/</li><li>https://&#42;.table.core.windows.net/</li><li>https://policykeyservice.dc.ad.msft.net/</li><li>https://login.windows.net</li><li>https://login.microsoftonline.com</li><li>https://secure.aadcdn.microsoftonline-p.com</li>|
| エージェントを実行するサーバー上のファイアウォール ポート| このエージェントには、次のファイアウォール ポートをエージェントを Azure AD Health サービス エンドポイントと通信するために開くことが必要です。</br></br><li>TCP/UDP port 80</li><li>TCP/UDP port 443</li><li>TCP/UDP port 5671</li>
| IE セキュリティ強化が有効になっている場合は以下の Web サイトが許可されていること| 次の web サイトは、エージェントがインストールされているサーバーで IE セキュリティ強化が有効になっているかどうかに許可する必要があります。</br></br><li>https://login.microsoftonline.com</li><li>https://secure.aadcdn.microsoftonline-p.com</li><li>https://login.windows.net</li><li>Azure Active Directory の使用例によって、組織のフェデレーション サーバーの信頼: https://sts.contoso.com</li>

## エージェントのダウンロード

使用を開始するには、次のいずれかを実行します。


- AD FS の Azure AD Connect Health を使用して開始するには、ここで、エージェントの最新バージョンをダウンロードできます:  [ダウンロード Azure AD Connect Health エージェントの AD FS 用](http://go.microsoft.com/fwlink/?LinkID=518973)。エージェントをインストールする前に、Marketplace からサービスを追加したことを確認します。
- Azure AD Connect Health for Sync の使用を開始するには、こちら (Azure AD Connect Health for Sync エージェントのダウンロード) から最新バージョンのエージェントをダウンロードします。 Health エージェントは、Azure AD Connect のインストール時に併せてインストールされます。 Azure AD Connect では、以前のバージョンからのインプレース アップグレードがサポートされています。


## 関連リンク

* [Azure AD 接続正常性エージェントのインストール](active-directory-aadconnect-health-agent-install.md)
* [Azure AD Connect Health の操作](active-directory-aadconnect-health-operations.md)
* [Azure AD Connect の使用正常性と AD FS](active-directory-aadconnect-health-adfs.md)
* [同期のための Connect Health の Azure AD の使用](active-directory-aadconnect-health-sync.md)
* [Azure AD Connect Health の FAQ](active-directory-aadconnect-health-faq.md)





