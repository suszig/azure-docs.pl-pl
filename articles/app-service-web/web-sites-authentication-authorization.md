<properties 
    pageTitle="Azure App Service での認証には、Active Directory を使用します" 
    description="Azure アプリ サービスの Web アプリにデプロイされている基幹業務アプリケーションのさまざまな認証オプションと承認オプションについて説明します。" 
    services="app-service" 
    documentationCenter="" 
    authors="cephalin" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="web" 
    ms.date="12/10/2015" 
    ms.author="cephalin"/>


# Azure App Service での認証には、Active Directory を使用します

[Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714) エンタープライズ基幹業務アプリケーション シナリオを使用する内部設置型環境またはパブリック インターネットからアクセスしているかどうかは、ユーザーのシングル サインオン (SSO) サポートされます。 統合できます [Azure Active Directory](http://azure.microsoft.com/services/active-directory/) (AAD) または内部設置型では、Active Directory フェデレーション サービス (AD FS) などのトークン サービス (STS) を保護、内部の Active Directory (AD) ユーザーを認証し、それらを適切に承認します。

## 円滑な認証と承認

ボタンを数回クリックすることで、Web アプリの認証と承認を有効にできます。 すべての Azure Web アプリのチェックボックス形式の構成は、基幹業務 Web アプリの基本的なアクセス制御を提供します。 これは、Web アプリのコンテンツへのアクセスをユーザーに許可する前に、選択した Azure AD テナントに対して HTTPS および認証を強制することによって実現されます。 詳細については、次を参照してください。 [Web Apps の認証/承認](http://azure.microsoft.com/blog/2014/11/13/azure-websites-authentication-authorization/)します。
>[AZURE.NOTE] 現在、この機能はプレビュー段階にあります。

## 認証と承認の手動での実装

多くのシナリオで、ログインとログアウト ページ、カスタム承認ロジック、マルチ テナント アプリケーションの動作など、アプリケーションの認証と承認の動作をカスタマイズできます。 このような場合、機能の柔軟性を高めるために、手動で認証および承認を構成する方がよい場合があります。 2 つの主なオプションを次に示します

-   [Azure AD](web-sites-dotnet-lob-application-azure-ad.md) -Azure AD と web アプリの認証と承認を実装することができます。 ID プロバイダーとして Azure AD を使用することには、次の特性があります。
    -   などの一般的な認証プロトコルをサポートしている [OAuth 2.0](http://oauth.net/2/), 、[OpenID Connect](http://openid.net/connect/), 、および [SAML 2.0](http://en.wikipedia.org/wiki/SAML_2.0)します。 サポートされているプロトコルの完全な一覧については [Azure Active Directory の認証プロトコル](http://msdn.microsoft.com/library/azure/dn151124.aspx)します。
    -   オンプレミスのインフラストラクチャなしで、Azure のみの ID プロバイダーを使用できます。
    -   ディレクトリ同期も、オンプレミスの (管理された内部設置型) ADで構成できます。
    -   オンプレミスの AD ドメインからディレクトリ同期される Azure AD によって、AD のユーザーがイントラネットおよびインターネットからアクセスするときに、Web アプリへのスムーズな SSO エクスペリエンスが実現します。 イントラネットからの AD ユーザーは、統合認証を通じて、自動的に Web アプリにアクセスできます。 インターネットからの AD ユーザーは、Windows 資格情報を使用して、Web アプリにログインできます。
    -   SSO を提供 [Azure AD によってサポートされているすべてのアプリケーション](/marketplace/active-directory/), 、Azure、Office 365、Dynamics CRM Online、Microsoft Intune、および数千のサードパーティ製のクラウド アプリケーションなどです。
    -   Azure AD の管理を委任する [証明書利用者](http://en.wikipedia.org/wiki/Relying_party) ままでなければならないアプリケーション機密性の高いディレクトリ データへのアクセス中に、管理者以外のユーザー ロールにアプリケーションをグローバル管理者が構成されます。
    -   すべての証明書利用者アプリケーションに対して、汎用的な一連の要求タイプを送信します。 クレームの種類の一覧では、次を参照してください。 [サポートされているトークンとクレームの種類](http://msdn.microsoft.com/library/azure/dn195587.aspx)します。 要求はカスタマイズできません。
    -   [Azure AD Graph API](http://msdn.microsoft.com/library/azure/hh974476.aspx) Azure AD 内アプリケーションのディレクトリ データへのアクセスを有効にします。
-   [内部設置型には、AD FS などのトークン サービス (STS) がセキュリティで保護された](../web-sites-dotnet-lob-application-adfs/) -を実装する認証と承認 web アプリの AD FS のような内部設置型の STS を使用します。 オンプレミスの AD FS の使用には、次の特性があります。
    -   AD FS トポロジは、社内にデプロイする必要があり、コストと管理のオーバーヘッドが伴います。
    -   会社のポリシーによって、AD データを社内に保存することが要求される場合に最適です。
    -   AD FS 管理者のみが構成できる [証明書利用者信頼し、要求規則](http://technet.microsoft.com/library/dd807108.aspx)します。
    -   管理できる [クレーム](http://technet.microsoft.com/library/ee913571.aspx) アプリケーションごとにします。
    -   企業のファイアウォール経由でオンプレミスの AD のデータにアクセスするためには、個別のソリューションが必要です。

>[AZURE.NOTE] 場合は、Azure アカウントがサインアップする前に Azure App Service の使用を開始するには、 [App Service の試用](http://go.microsoft.com/fwlink/?LinkId=523751), 、App Service で有効期間の短いスターター web アプリをすぐに作成する場所です。 このサービスの利用にあたり、クレジット カードは必要ありません。契約も必要ありません。

## 変更内容

* Web サイトから App Service への変更のガイドを参照してください: [Azure App Service と既存の Azure サービスへの影響](http://go.microsoft.com/fwlink/?LinkId=529714)






