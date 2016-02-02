<properties
    pageTitle="Azure AD アプリケーション プロキシの有効化 | Microsoft Azure"
    description="Azure AD アプリケーション プロキシをセットアップする方法について説明します。"
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="StevenPo"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/02/2015"
    ms.author="kgremban"/>


# Azure AD アプリケーション プロキシの有効化

> [AZURE.NOTE] アプリケーション プロキシは、Azure Active Directory の Premium または Basic エディションにアップグレードしている場合にのみ利用できる機能です。 詳細については、次を参照してください。 [Azure Active Directory のエディション](active-directory-editions.md)します。

Microsoft Azure Active Directory アプリケーション プロキシを使用すると、SharePoint サイト、Outlook Web Access、IIS ベースのアプリなどのアプリケーションをプライベート ネットワーク内で発行し、ネットワーク外部のユーザーにセキュリティで保護されたアクセスを提供できます。 従業員は、自宅から自分のデバイスでアプリケーションにログインし、このクラウド ベースのプロキシを使用して認証を行うことができます。

アプリケーション プロキシは、社内ネットワークに "コネクタ" と呼ばれる軽量の Windows サービスをインストールすることによって機能します。 コネクタは、ネットワーク内からプロキシ サービスへの送信接続を維持します。 ユーザーが公開されたアプリケーションにアクセスすると、プロキシはこの接続を使用してアプリケーションへのアクセスを提供します。

この記事では、Azure AD のクラウド ディレクトリに対して Microsoft Azure AD アプリケーション プロキシを有効にする方法のほか、アプリケーション プロキシ コネクタをプライベート ネットワークにインストールし、そのコネクタを Microsoft Azure AD テナント サブスクリプションに登録する方法について説明します。

## アプリケーション プロキシの前提条件

アプリケーション プロキシ サービスを有効にして使用するためには、次の条件を満たしておく必要があります。

- Microsoft Azure AD [基本] または [プレミアム サブスクリプション](active-directory-editions.md) と全体管理者をしたら、Azure AD ディレクトリです。
- アプリケーション プロキシ コネクタをインストールできる Windows Server 2012 R2 または Windows 8.1 以降が実行されているサーバー。 このサーバーは、クラウド内のアプリケーション プロキシ サービスに HTTPS 要求を送信できることと、発行予定のアプリケーションに HTTPS で接続できることが必要です。
- 経路上にファイアウォールが存在する場合、コネクタからアプリケーション プロキシに送信される HTTPS (TCP) 要求を許可するようにファイアウォールを開放する必要があります。 コネクタは、上位ドメイン msappproxy.net に属しているサブドメインと併せて、これらのポートを使用します。 **送信**トラフィックに対しては、以下のポートを**すべて**開放してください。

 ポート番号| 説明
--- | ---
 80| セキュリティ検証用の送信 HTTP トラフィックに使用されます。
 443| Azure AD に対するユーザー認証に使用されます (コネクタ登録プロセスでのみ必要)。
 10100 ～ 10120| プロキシに送り返される LOB HTTP 応答に使用されます。
 9352、5671| コネクタと Azure サービス間で受信要求の通信に使用されます。
 9350| 省略可能。受信要求のパフォーマンス向上に使用されます。
 8080| コネクタのブートストラップ シーケンスのほか、コネクタの自動更新に使用されます。
 9090| コネクタの登録に使用されます (コネクタ登録プロセスでのみ必要)。
 9091| コネクタの信頼証明書の自動更新に使用されます。

ご利用のファイアウォールが送信元ユーザーに応じてトラフィックを監視している場合は、Network Service として実行されている Windows サービスを送信元とするトラフィックに対してこれらのポートを開放します。 また、NT Authority\System に対しては必ずポート 8080 を有効にしてください。


## 手順 1: Azure AD でのアプリケーション プロキシの有効化

1. Azure クラシック ポータルの管理者としてサインインします。
2. Active Directory に移動し、アプリケーション プロキシを有効にするディレクトリを選択します。
3. **[構成]** をクリックし、下へスクロールして [アプリケーション プロキシ] を表示し、[このディレクトリに対してアプリケーション プロキシ サービスを有効にする] を **[有効]** に切り替えます。

    ![アプリケーション プロキシを有効にする](./media/active-directory-application-proxy-enable/app_proxy_enable.png) <p>
4. 画面の下部の **[今すぐダウンロード]** をクリックします。 この操作により、ダウンロード ページが表示されます。 ライセンス条項を読んで同意し、**[ダウンロード]** をクリックして、アプリケーション プロキシ コネクタの Windows インストーラー ファイル (.exe) を保存します。

## 手順 2: コネクタのインストールと登録

1. 実行 `AADApplicationProxyConnectorInstaller.exe` を準備したサーバー上 (上記の前提条件アプリケーション プロキシを参照してください)。
2. ウィザードの指示に従ってインストールします。
3. インストール時に、Azure AD テナントのアプリケーション プロキシにコネクタを登録するように求められます。
<p>-Azure AD 全体管理者の資格情報を入力します。
<p>コネクタを登録する管理者は、たとえばでアプリケーション プロキシ サービスを有効した同じディレクトリに、テナント ドメインが contoso.com の場合は、管理者 admin@contoso.com またはそのドメイン内の他の任意のエイリアスです。また、Azure AD テナントのグローバル管理者である必要があります。グローバル管理者のテナントは、Microsoft Azure の資格情報とは異なる場合があります。
<p>-に IE セキュリティ強化の構成が設定されている場合 **に** Azure AD コネクタをインストールするサーバーで、登録画面がブロックされています。その場合は、エラー メッセージに示された指示に従って、アクセスを許可します。Internet Explorer セキュリティ強化の構成が無効になっていることを確認します。
<p>-コネクタの登録が成功しなかった場合は、次を参照してください。 [アプリケーション プロキシのトラブルシューティング](active-directory-application-proxy-troubleshoot.md)します。

4. インストールが完了したら、次に示す 2 つの新しいサービスがサーバーに追加されます。 接続を可能にするコネクタ サービスと、定期的にコネクタの新しいバージョンをチェックし必要に応じてコネクタを更新する自動更新サービスです。 インストール ウィンドウで、インストールを完了する完了] をクリックします。
    ![アプリケーション プロキシ コネクタ サービス!](./media/active-directory-application-proxy-enable/app_proxy_services.png) <p>
5. プリケーション プロキシを使用してアプリケーションを発行する準備がこれで整いました。

高可用性を確保するには、追加のコネクタを少なくとも 1 つデプロイしてください。 追加のコネクタをデプロイするには、上記の手順 2. と手順 3. を繰り返します。 各コネクタは個別に登録する必要があります。

コネクタをアンインストールする場合は、コネクタ サービスと更新サービスを両方ともアンインストールしてから、コンピューターを再起動してサービスを完全に削除するようにしてください。


## 関連項目

アプリケーション プロキシを使ってできることは他にもたくさんあります。

- [アプリケーション プロキシによるアプリケーションを発行します。](active-directory-application-proxy-publish.md)
- [ドメイン名を使用してアプリケーションを公開します。](active-directory-application-proxy-custom-domains.md)
- [シングル サインオンの有効化にします。](active-directory-application-proxy-sso-using-kcd.md)
- [条件付きアクセスを有効にします。](active-directory-application-proxy-conditional-access.md)
- [クレーム対応アプリケーションの使用](active-directory-application-proxy-claims-aware-apps.md)
- [アプリケーション プロキシが発生した問題をトラブルシューティングします。](active-directory-application-proxy-troubleshoot.md)

## アプリケーション プロキシに関するその他の情報

- [こちらをご覧オンライン ヘルプ](active-directory-application-proxy-enable.md)
- [アプリケーション プロキシに関するブログします。](http://blogs.technet.com/b/applicationproxyblog/)
- [Channel 9 のビデオを見る。](http://channel9.msdn.com/events/Ignite/2015/BRK3864)

## その他のリソース

* [組織として Azure にサインアップします。](sign-up-organization.md)
* [Azure Id](fundamentals-identity.md)
* [アプリケーション プロキシによるアプリケーションを発行します。](active-directory-application-proxy-publish.md)





