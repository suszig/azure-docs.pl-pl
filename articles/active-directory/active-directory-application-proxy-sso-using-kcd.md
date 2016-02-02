<properties
    pageTitle="アプリケーション プロキシを使用したシングル サインオン | Microsoft Azure"
    description="Azure AD アプリケーション プロキシを使用してシングル サインオンを提供する方法について説明します。"
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
    ms.date="10/19/2015"
    ms.author="kgremban"/>




# アプリケーション プロキシを使用したシングル サインオン

シングル サインオンは、Azure AD の重要な要素です。 それは、最高のユーザー エクスペリエンスを提供します。このエクスペリエンスでは、ユーザーがクラウドにサインインすると、すべてのセキュリティ検証がクラウドで発生し (事前認証)、オンプレミス アプリケーションに要求が送信されるときにアプリケーション プロキシ コネクタがユーザーの代理となり、バックエンド アプリケーションは、それがドメイン参加デバイスからの正規ユーザーであると考えます。
[](.media/active-directory-application-proxy-sso-using-kcd/app_proxy_sso_diff_id_diagram.png)
Azure AD アプリケーション プロキシでは、シングル サインオン (SSO) は、ユーザーのエクスペリエンスを行うことができます。 シングル サインオンを使用するアプリを発行するには、次の手順に従います。


- オンプレミス IWA アプリケーションへの SSO のための KCD とアプリケーション プロキシの使用
- Windows 以外のアプリの SSO
- オンプレミス ID とクラウド ID が同一でない場合の SSO の操作

## オンプレミス IWA アプリケーションへの SSO のための KCD とアプリケーション プロキシの使用

統合 Windows 認証 (IWA) を使用してアプリケーションへのシングル サインオン (SSO) ができるようにするには、アプリケーション プロキシ コネクタがユーザーの代理でトークンを送受信できるように Active Directory で設定します。
> [AZURE.IMPORTANT] アプリケーション プロキシは、Azure Active Directory の Premium または Basic エディションにアップグレードしている場合にのみ利用できる機能です。 詳細については、次を参照してください。 [Azure Active Directory のエディション](active-directory-editions.md)します。


### ネットワーク図

![Microsoft AAD 認証のフロー図](./media/active-directory-application-proxy-sso-using-kcd/AuthDiagram.png)

この図は、IWA を使用するオンプレミス アプリケーションにユーザーがアクセスしようとしたときの流れを説明するものです。 全体的な流れは次のとおりです。

1. ユーザーは、オンプレミスのアプリケーションにアプリケーション プロキシをとおしてアクセスするための URL を入力します。
2. この要求がアプリケーション プロキシによって Azure AD 認証サービスにリダイレクトされて、事前認証が行われます。 この時点で、Azure AD の認証および承認のポリシーのうち、該当するものが適用されます (たとえば多要素認証)。 ユーザーの正当性が確認された場合は、Azure AD によってトークンが作成されてユーザーに送信されます。
3. ユーザーは、このトークンをアプリケーション プロキシに渡します。
4. アプリケーション プロキシはこのトークンを検証し、ユーザー プリンシパル名 (UPN) をトークンから取り出してから、要求、UPN、およびサービス プリンシパル名 (SPN) をコネクタに送信します。この送信は、二重認証済みのセキュリティで保護されたチャネルをとおして行われます。
5. コネクタは、オンプレミス AD との KCD (Kerberos の制約付き委任) ネゴシエーションを実行します。このときに、ユーザーの代理でアプリケーションに対する Kerberos トークンを取得します。
6. Active Directory は、そのアプリケーション用の Kerberos トークンをコネクタに送信します。
7. コネクタは、AD から受信した Kerberos トークンを使用して、元の要求をアプリケーション サーバーに送信します。
8. アプリケーションから応答がコネクタに送信されます。この応答はアプリケーション プロキシ サービスに返され、最終的にユーザーに返されます。

### 前提条件

1. アプリケーション (たとえば SharePoint Web アプリケーション) が統合 Windows 認証を使用するように設定されていることを確認します。 詳細については、次を参照してください。 [Kerberos 認証のサポートを有効にする](https://technet.microsoft.com/library/dd759186.aspx), 、または SharePoint を参照してください [SharePoint 2013 で Kerberos 認証の計画](https://technet.microsoft.com/library/ee806870.aspx)します。
2. アプリケーションのサービス プリンシパル名を作成します。
3. コネクタを実行するサーバーと、発行しようとしているアプリケーションを実行するサーバーが、同じドメインに参加していることを確認します。 ドメインへの参加の詳細については、次を参照してください。 [コンピューターをドメインに参加](https://technet.microsoft.com/library/dd807102.aspx)します。


### Active Directory の構成

Active Directory の構成は、アプリケーション プロキシ コネクタと公開されたサーバーが同じドメイン内にあるかどうかによって異なります。

#### 同じドメインにあるコネクタと公開されたサーバー

1. Active Directory で、**[ツール]**、**[ユーザーとコンピューター]** の順に移動します。
2. コネクタを実行しているサーバーを選択します。
3. 右クリックして、**[プロパティ]**、**[委任]** の順に選択します。
4. **[指定されたサービスへの委任でのみこのコンピューターを信頼する]** を選択し、**[このアカウントが委任された資格情報を提示できるサービス]** の下で、アプリケーション サーバーのサービス プリンシパル名 (SPN) ID の値を追加します。
5. これで、アプリケーション プロキシ コネクタは、AD において、リストで定義されたアプリケーションに対してユーザーの代理となることができるようになります。

![Connector-SVR のプロパティ ウィンドウのスクリーン ショット](./media/active-directory-application-proxy-sso-using-kcd/Properties.jpg)

#### それぞれ異なるドメインにあるコネクタと公開されたサーバー

1. ドメイン間での KCD を使用するための前提条件の一覧は、次を参照してください。 [ドメイン間で Kerberos の制約付き委任](https://technet.microsoft.com/library/hh831477.aspx)します。
2. Windows 2012 r2 を使用して、 `で principalsallowedtodelegateto` プロパティを公開しているサーバーがここでは、コネクタ サーバー用に委任するアプリケーション プロキシを有効にするコネクタ サーバーを `sharepointserviceaccount` 委任サーバーは、 `connectormachineaccount`します。

     $connector= Get-ADComputer -Identity connectormachineaccount -server dc.connectordomain.com
    
     Set-ADComputer -Identity sharepointserviceaccount -PrincipalsAllowedToDelegateToAccount $connector
    
     Get-ADComputer sharepointserviceaccount -Properties PrincipalsAllowedToDelegateToAccount



>[AZURE.NOTE] `sharepointserviceaccount` SPS コンピューター アカウントまたは SPS アプリケーション プールを実行しているサービス アカウントであることができます。


### Azure ポータルの構成

1. 説明に従って、アプリケーションを発行 [アプリケーション プロキシを使用してアプリケーションを公開](active-directory-application-proxy-publish.md)します。 **[事前認証方法]** で **[Azure Active Directory]** が選択されていることを確認してください。
2. アプリケーションがアプリケーションの一覧に表示されたら、アプリケーションを選択して **[構成]** をクリックします。
3. **プロパティ**, 、 **内部認証方法** に **統合 Windows 認証**.<br>![アプリケーションの構成を高度な](./media/active-directory-application-proxy-sso-using-kcd/cwap_auth2.png)

4. アプリケーション サーバーの**[内部アプリケーション SPN]** を入力します。 この例では、公開されたアプリケーションの SPN は、http/lob.contoso.com です。

>[AZURE.IMPORTANT] Azure Active Directory での UPN は、オンプレミスの Active Directory での UPN と同一であることが必要です。同一でない場合は事前認証ができなくなります。 Azure Active Directory がオンプレミスの Active Directory と同期していることを確認してください。

| | |
| --- | --- |
| 内部認証方法| 事前認証に Azure AD を使用する場合、内部認証方法を設定すると、ユーザーがこのアプリケーションへのシングル サインオン (SSO) を活用できるようになります。<br><br> 選択 **統合 Windows 認証** (IWA) 場合は、アプリケーションで IWA を使用して、このアプリケーションに対して SSO を有効にする Kerberos 制約付き委任 (KCD) を構成することができます。IWA を使用するアプリケーションは KCD を使用して構成する必要があります。そうしないと、アプリケーション プロキシでこれらのアプリケーションを発行できません。<br><br> 選択 **None** 、アプリケーションが IWA を使用しない場合。|
| 内部アプリケーション SPN| これは、オンプレミスの Azure AD で構成されている、オンプレミス アプリケーションのサービス プリンシパル名 (SPN) です。SPN は、KCD を使用しているアプリケーションの Kerberos トークンを取得するために、アプリケーション プロキシ コネクタによって使用されます。|


## Windows 以外のアプリの SSO

Azure AD Application Proxy での Kerberos 委任フローは、Azure AD がクラウドでユーザーを認証するときから始まります。要求がオンプレミスに到着すると、Azure AD アプリケーション プロキシ コネクタは、ローカルの Active Directory と交信することで、ユーザーに代わって Kerberos チケットを発行します。このプロセスは Kerberos の制約付き委任 (KCD) と呼ばれます。次のフェーズで、要求がこの Kerberos チケットを使用してバックエンド アプリケーションに送信されます。このような要求を送信する方法を定義するプロトコルはたくさんあります。ほとんどの Windows 以外のサーバーには、Azure AD アプリケーション プロキシのサポートが追加されましたネゴシエート/SPNego が予想されます。<br>![](./media/active-directory-application-proxy-sso-using-kcd/app_proxy_sso_nonwindows_diagram.png)

### 部分的に委任された ID

通常、非 Windows アプリケーションは、ユーザー名または SAM アカウント名、電子メール アドレスではなく (username@domain) の形式でユーザー id を取得します。 これは、より包括的でドメインの重複がないことを保証する UPN を使用するほとんどの Windows ベースのシステムとは異なります。
このため、アプリケーション プロキシでは、Kerberos チケットに表示される ID をアプリケーションごとに選択できます。これらのオプションの一部は、電子メール アドレスの形式を受け入れないシステムに適しています。<br>![](./media/active-directory-application-proxy-sso-using-kcd/app_proxy_sso_diff_id_upn.png)
部分 ID を使用する場合に、その ID が組織のすべてのドメインまたはフォレストで一意でない場合は、これらのアプリケーションを 2 つの異なるコネクタ グループを使用して 2 度発行できます。これは各アプリケーションのユーザーが異なり、そのコネクタを異なるドメインに参加させることができるためです。


## オンプレミス ID とクラウド ID が同一でない場合の SSO の操作

他に構成されていない限り、アプリケーション プロキシは、ユーザーのクラウド ID とオンプレミス ID は完全に同じであるとみなします。 アプリケーションごとに、シングル サインオンを実行するときに使用する ID を構成できます。
この機能により、オンプレミス ID とクラウド ID が異なる多くの組織で、ユーザーに別のユーザー名とパスワードの入力を要求することなく、クラウドからオンプレミスのアプリにシングル サインオンさせることができます。 次のような組織が含まれます。


- 内部的に複数のドメインがある (joe@us.contoso.com、joe@eu.contoso.com) とクラウド (joe@contoso.com) 内の 1 つのドメイン。

- 内部的にルーティング不可能なドメイン名がある (joe@contoso.usa) と、クラウドで有効な 1 つです。

- 内部でドメイン名を使用していない (joe)。

- オンプレミスとクラウドで異なるエイリアスを使用している。 Joej@contoso.com と joe-johns@contoso.com 例:
これは、Windows 以外のバックエンド サーバーで非常に一般的なシナリオである電子メール アドレス形式のアドレスを受け入れないアプリケーションでも役立ちます。
### クラウド ID とオンプレミス ID が異なる場合の SSO の設定

1. Azure AD Connect の設定を、メイン ID が電子メール アドレス (mail) になるように構成します。これについては、同期設定のユーザー プリンシパル名のフィールドを変更することでカスタマイズ プロセスの一部として行います。<br>![](./media/active-directory-application-proxy-sso-using-kcd/app_proxy_sso_diff_id_connect_settings.png)
注: これらの設定は、ユーザーが Office365、Windows10 デバイス、および Azure AD を ID ストアとして使用する他のアプリケーションにログインする方法も決定します。
2. 変更するアプリケーションの [アプリケーションの構成] 設定で、使用する **[委任されたログイン ID]** を選択します。

- ユーザー プリンシパル名: joe@contoso.com

- 別のユーザー プリンシパル名: joed@contoso.local

- ユーザー プリンシパル名のユーザー名部分: joe

- 代替ユーザー プリンシパル名のユーザー名部分: joed

- オンプレミスのソフトウェア アセット管理アカウント名: オンプレミスのドメイン コント ローラーの構成に依存
![](./media/active-directory-application-proxy-sso-using-kcd/app_proxy_sso_diff_id_upn.png)

### ID が異なる場合の SSO のトラブルシューティング

SSO のプロセスでエラーがある場合に表示されますコネクタ コンピューターのイベント ログで説明したよう [トラブルシューティング](active-directory-application-proxy-troubleshoot.md)します。
ただし、場合によっては、バックエンド アプリケーションが他のさまざまなHTTP 応答に応答している間に、要求が正常に送信されることがあります。 このような場合のトラブルシューティングは、アプリケーション プロキシ セッションのイベント ログで、コネクタ コンピューターのイベント番号 24029 を調べることから始める必要があります。 委任のために使用されたユーザー ID は、イベント詳細の [ユーザー] フィールドに表示されます (次の例では “joe@contoso55.com”)。 セッション ログを有効にするには選択 ** [分析およびデバッグ ログ **イベント ビューアーの表示] メニューにします。







## 関連項目

アプリケーション プロキシを使ってできることは他にもたくさんあります。


- [アプリケーション プロキシによるアプリケーションを発行します。](active-directory-application-proxy-publish.md)
- [ドメイン名を使用してアプリケーションを公開します。](active-directory-application-proxy-custom-domains.md)
- [条件付きアクセスを有効にします。](active-directory-application-proxy-conditional-access.md)
- [クレーム対応アプリケーションの使用](active-directory-application-proxy-claims-aware-apps.md)
- [アプリケーション プロキシが発生した問題をトラブルシューティングします。](active-directory-application-proxy-troubleshoot.md)

## アプリケーション プロキシに関するその他の情報

- [こちらをご覧オンライン ヘルプ](active-directory-application-proxy-enable.md)
- [アプリケーション プロキシに関するブログします。](http://blogs.technet.com/b/applicationproxyblog/)
- [Channel 9 のビデオを見る。](http://channel9.msdn.com/events/Ignite/2015/BRK3864)




[1]: ./media/active-directory-application-proxy-sso-using-kcd/AuthDiagram.png 
[2]: ./media/active-directory-application-proxy-sso-using-kcd/Properties.jpg 

