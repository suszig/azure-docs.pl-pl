<properties
    pageTitle="プロキシ アプリケーションでネイティブ クライアント アプリケーションの発行を有効にする方法 | Microsoft Azure"
    description="ネイティブ クライアント アプリケーションが Azure AD Application Proxy Connector と通信して、オンプレミス アプリケーションに対して安全なリモート アクセスを提供する方法について説明します。"
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="stevenpo"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="11/02/2015"
    ms.author="kgremban"/>

# ネイティブ クライアント アプリケーションからプロキシ アプリケーションを操作できるようにする方法
Azure Active Directory アプリケーション プロキシは、SharePoint、Outlook Web Access、カスタムの基幹業務アプリケーションなどのブラウザー アプリケーションを公開するために幅広く使用されています。 ネイティブ クライアントから使用される HTTP バックエンド アプリケーションを公開するときにも使用できます。 この場合、標準の Authorize HTTP ヘッダーで送信された Azure AD 発行トークンをサポートしている必要があります。


![](./media/active-directory-application-proxy-native-client/richclientflow.png)


このようなアプリケーションを発行するために推奨される方法は、すべての認証ハッスルに対応し、多様なクライアント環境をサポートする Azure AD 認証ライブラリを使用することです。 アプリケーション プロキシに組み込まれて、 [ネイティブ アプリケーション対 Web API のシナリオ](active-directory-authentication-scenarios.md#native-application-to-web-api)します。 その手順は次のとおりです。

1. 他のアプリケーションと同様にプロキシ アプリケーションを発行し、ユーザーを割り当てて Premium または Basic ライセンスを付与します。 詳細については、次を参照してください。  [アプリケーション プロキシを使用してアプリケーションを公開](active-directory-application-proxy-publish.md)します。
2. ネイティブ アプリケーションを次のように構成します。
  3. Microsoft Azure 管理ポータルにサインインします。
  4. 左側のメニューの [Active Directory] アイコンをクリックし、目的のディレクトリをクリックします。
  5. 上部のメニューで、[アプリケーション] をクリックします。 ディレクトリに追加されているアプリケーションがない場合、このページには [アプリケーションの追加] リンクだけが表示されます。 このリンクをクリックします。または、コマンド バーの [追加] をクリックすることもできます。
  4.  **実行する操作** ] ページで、リンクをクリックして **[組織が開発中のアプリケーションを追加**します。
  5.  **アプリケーションについてお聞かせ** ] ページで、アプリケーションの名前を指定して選択 **ネイティブ クライアント アプリケーション** 電話機やコンピューターなどのデバイスにインストールされているアプリケーションを表します。 完了したら、ページの右下にある矢印アイコンをクリックします。
  6.  **アプリのプロパティ** ] ページで、提供、 **リダイレクト URI** ネイティブ クライアント アプリケーション ページの下部にある右上隅のチェック ボックスをクリックします。 </br>アプリケーションが追加されると、アプリケーションのクイック スタート ページが表示されます。
8. ディレクトリ内の他のアプリケーションに公開するネイティブ アプリケーションを有効にします。
  9. 上部のメニューをクリックして **アプリケーション**, を新しいネイティブ アプリケーションを選択してクリックして **構成**します。
  10. 下へスクロールして、 **他のアプリケーションに対するアクセス許可** セクションです。 をクリックして、 **アプリケーション追加** ] ボタンをクリックし、ネイティブ アプリケーションのアクセスを許可するプロキシ アプリケーションを選択し、右下隅のチェック マークをクリックします。  **委任されたアクセス許可** ドロップ ダウン メニューでは、新しいアクセス許可を選択します。 </br>

![](./media/active-directory-application-proxy-native-client/delegate_native_app.png) </br></br>
4. Active Directory Authentication Library (ADAL) の認証コンテキストのネイティブ アプリケーション コードを編集して、次の情報を含めます。

        // Acquire Access Token from AAD for Proxy Application
        AuthenticationContext authContext = new AuthenticationContext("https://login.microsoftonline.com/<TenantId>");
        AuthenticationResult result = authContext.AcquireToken("< Frontend Url of Proxy App >",
                                                        "< Client Id of the Native app>",
                                                        new Uri("< Redirect Uri of the Native App>"),
                                                        PromptBehavior.Never);

        //Use the Access Token to access the Proxy Application
        HttpClient httpClient = new HttpClient();
        httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);
        HttpResponseMessage response = await httpClient.GetAsync("< Proxy App API Url >");

変数は次のように置き換えてください。


- **TenantId** 、アプリケーションの URL に GUID を記載 **構成** ] ページで、直後に「Directory/」です。
- **フロント エンド URL** プロキシ アプリケーションに入力しにある [フロント エンドの url、 **構成** プロキシ アプリのページです。
- **クライアント Id** ネイティブ アプリのご覧、 **構成** ネイティブ アプリケーションのページです。
- **ネイティブ アプリのリダイレクト URI** ご覧、 **構成** ネイティブ アプリケーションのページです。

![](./media/active-directory-application-proxy-native-client/new_native_app.png)
</br> </br>ネイティブ アプリケーション フローの詳細については、次を参照してください。 [ネイティブ アプリケーション対 web API](active-directory-authentication-scenarios.md#native-application-to-web-api)します。






## 次の手順
アプリケーション プロキシを使ってできることは他にもたくさんあります。


- [独自のドメイン名でアプリケーションを発行する](active-directory-application-proxy-custom-domains.md)
- [シングル サインオンを有効にする](active-directory-application-proxy-sso-using-kcd.md)
- [要求に対応するアプリケーションを利用する](active-directory-application-proxy-claims-aware-apps.md)
- [条件付きアクセスを有効にする](active-directory-application-proxy-conditional-access.md)


### アプリケーション プロキシに関するその他の情報
- [オンライン ヘルプ](active-directory-application-proxy-enable.md)
- [アプリケーション プロキシに関するブログ](http://blogs.technet.com/b/applicationproxyblog/)
- [Channel 9 ビデオ](http://channel9.msdn.com/events/Ignite/2015/BRK3864)

## その他のリソース
* [Azure への組織としてのサインアップ](sign-up-organization.md)
* [Azure ID](fundamentals-identity.md)


