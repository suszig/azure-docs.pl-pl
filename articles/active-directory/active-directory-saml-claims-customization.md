<properties
    pageTitle="Azure Active Directory での事前に統合されたアプリの SAML トークンで発行された要求のカスタマイズ| Microsoft Azure"
    description="Azure Active Directory において事前に統合されたアプリの SAML トークンで発行された要求をカスタマイズする方法を学習します"
    services="active-directory"
    documentationCenter=""
    authors="asmalser-msft"
    manager="stevenpo"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="11/18/2015"
    ms.author="asmalser"/>

#Azure Active Directory での事前に統合されたアプリの SAML トークンで発行された要求のカスタマイズ

現在 Azure Active Directory は、SAML 2.0 プロトコルを使用したシングル サインオンをサポートする 150 以上のアプリケーションを含め、何千もの事前統合済みのアプリケーションを Azure AD アプリケーション ギャラリーでサポートしています。 SAML を使用し、Azure AD を介してアプリケーションを認証する場合、Azure AD は、(HTTP 302 リダイレクトを介して) アプリケーションにトークンを送信します。次に、アプリケーションはトークンを検証し、ユーザー名とパスワードを要求する代わりにトークンを使用してユーザーをログインさせます。 これらの SAML トークンには、「要求」と呼ばれる、ユーザーに関する情報が含まれています。

ID 用語で「要求」とは、そのユーザーに発行するトークンの中にあるユーザーに関する ID プロバイダーが提示した情報を指します。  [SAML トークン](http://en.wikipedia.org/wiki/SAML_2.0), 、このデータは通常、SAML 属性ステートメントに含まれ、ユーザーの一意の ID は通常、SAML サブジェクトで表されます。

既定では、Azure AD は、NameIdentifier 要求、Azure AD でのユーザー名の値 (この値は、ユーザーを一意に識別します) を含む、SAML トークンを発行します。 また、SAML トークンには、ユーザーの電子メール アドレス、姓名を含むその他の要求も含まれています。

表示または編集アプリケーションに SAML トークンで発行された要求をアプリケーション レコードを選択し、ポータル、Azure の管理で開く、 **属性** アプリケーションの下にあるタブをクリックします。

![][1]

SAML トークンで発行された要求を編集しなければならない理由、2 つの理由が考えられます。
• アプリケーションは、別のクレームの Uri のセットが必要または要求の値に書き込まれています 
•Your アプリケーションは、Azure Active Directory に格納されているユーザー名 (AKA ユーザー プリンシパル名) 以外の場合に NameIdentifier 要求するような方法で展開されています。 

SAML トークンの属性テーブル内の行のいずれかにマウスを置いた際に、右側に表示される、鉛筆の形のアイコンを選択して、既定の要求値のいずれかを編集できます。 (NameIdentifier) 以外を使用して要求を削除することも、 **X** ] アイコンを使用して新しいクレームを追加し、 **[ユーザー属性の追加** ] ボタンをクリックします。

##NameIdentifier 要求の編集

別のユーザー名を使用して、デプロイされたアプリケーションの問題を解決するには、NameIdentifier 要求の横にある鉛筆アイコンをクリックします。 これにより、次のいくつかのオプションを示すダイアログ ボックスが表示されます。

![][2]

 **属性値** メニューの [ **user.mail** ディレクトリ内のユーザーの電子メール アドレスを選択したりする NameIdentifier 要求を設定する **user.onpremisessamaccountname** の内部設置型 Azure AD から同期されて SAM アカウント名、ユーザーに設定します。 

電子メール アドレスまたはユーザー プリンシパル名 (例:"joesmith"joesmith@contoso.com ではなく) を通じて渡されるユーザー名の最初の部分のみで結果のいずれかからドメイン サフィックスを削除するのには特殊な ExtractMailPrefix() 関数を使用することもできます。

![][3]

##要求の追加

新しい要求を追加する場合は、ディレクトリに格納されているユーザーの属性に値を設定するのと同様に、属性名 (SAML 仕様のように厳密に URI パターンに従う必要はありません) を指定できます。

![][4]

など、組織がクレームとしてのユーザーが所属する部門を送信する必要がある場合 (例: 売上) を入力し、すべての要求の値は期待されて、アプリケーションで] を選択 **user.department** 値として。

特定のユーザーに対し、選択された属性に格納されている値がない場合は、トークンでその要求は発行されません。

**注:** 、 **user.onpremisesecurityidentifier** と **user.onpremisesamaccountname** からユーザー データを同期する内部設置型 Active Directory の AAD Connect ツールの最新のプレビューを使用する場合にのみサポートされています。 Connect ツールのプレビューは、次のリンクからダウンロードすることができます。  

http://connect.microsoft.com/site1164/Downloads/DownloadDetails.aspx?DownloadID=53949
    
<!--Image references-->
[1]: ./media/active-directory-saml-claims-customization/claimscustomization1.png
[2]: ./media/active-directory-saml-claims-customization/claimscustomization2.png
[3]: ./media/active-directory-saml-claims-customization/claimscustomization3.png
[4]: ./media/active-directory-saml-claims-customization/claimscustomization4.png

