<properties 
    pageTitle="Azure API Management で Azure Active Directory を使用して開発者アカウントを認証する方法" 
    description="API Management で Azure Active Directory を使用してユーザーを認証する方法について説明します。" 
    services="api-management" 
    documentationCenter="API Management" 
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

# Azure API Management で Azure Active Directory を使用して開発者アカウントを認証する方法


## 概要
このガイドでは、1 つ以上の Azure Active Directory 内のすべてのユーザーに対して開発者ポータルへのアクセスを有効にする方法について説明します。 また、Azure Active Directory のユーザーが含まれた外部グループを追加することで Azure Active Directory ユーザーのグループを管理する方法についても説明します。

>このガイドの手順を実行するには、アプリケーションの作成先となる Azure Active Directory が事前に必要です。

## Azure Active Directory を使用して開発者アカウントを認証する方法

クリックして、 **管理** 、API Management サービスの Azure クラシック ポータルで。 API Management パブリッシャー ポータルが表示されます。

![パブリッシャー ポータル][api-management-management-console]

>API Management サービス インスタンスを作成がない場合は、次を参照してください。 [API Management サービス インスタンスの作成][] で、 [Azure API Management を使ってみる][] チュートリアルです。

クリックして **セキュリティ** から、 **API Management** メニューをクリックして左 **外部 Id**します。

![外部 ID][api-management-security-external-identities]

クリックして **Azure Active Directory**します。 書き留めて、 **リダイレクト URL** Azure クラシック ポータルで Azure Active Directory に切り替えます。

![外部 ID][api-management-security-aad-new]

クリックして、 **追加** 新しい Azure Active Directory アプリケーションを作成するボタンをクリックし、選択 **[組織が開発中のアプリケーションを追加**します。

![新しい Azure Active Directory アプリケーションの追加][api-management-new-aad-application-menu]

アプリケーションの名前を入力 **Web アプリケーションや Web API**, 、[次へ] をクリックします。

![新しい Azure Active Directory アプリケーション][api-management-new-aad-application-1]

 **サインオン URL**, 、コピー、 **リダイレクト URL** から、 **Azure Active Directory** のセクションで、 **外部 Id** ] タブで、パブリッシャー ポータルを取り出して、 **- aad** というサフィックスを URL の末尾からです。 この例では、 **サインオン URL** は `https://aad03.portal.current.int-azure-api.net/signin`です。 

 **アプリ ID URL**, 、Azure Active Directory の既定のドメインまたはカスタム ドメインのいずれかを入力し、一意の文字列を追加します。 この例で、既定のドメインの **https://contoso5api.onmicrosoft.com** のサフィックスを持つ使用 **/api** 指定します。

![新しい Azure Active Directory アプリケーションのプロパティ][api-management-new-aad-application-2]

切り替えて、し、新しいアプリケーションを作成して保存するに、チェック ボタンをクリックして、 **構成** ] タブをクリックして新しいアプリケーションを構成します。

![作成された新しい Azure Active Directory アプリケーション][api-management-new-aad-app-created]

複数の Azure Active ディレクトリは、このアプリケーションに使用すると、クリックして **はい** の **アプリケーションはマルチ テナント**します。 既定値は **いいえ**します。

![アプリケーションはマルチテナントです][api-management-aad-app-multi-tenant]

コピー、 **リダイレクト URL** から、 **Azure Active Directory** のセクションで、 **外部 Id** [と] タブ、パブリッシャー ポータルに貼り付けること、 **応答 URL** テキスト ボックスです。 

![応答 URL][api-management-aad-reply-url]

構成] タブの下部までスクロールして、 **アプリケーションのアクセス許可** ボックスの一覧を確認および **ディレクトリ データを読み取る**します。

![アプリケーションのアクセス許可][api-management-aad-app-permissions]

選択、 **アクセス許可の委任** ボックスの一覧を確認し、 **サインオンを有効にして、ユーザーのプロファイルを読み取る**します。

![委任されたアクセス許可][api-management-aad-delegated-permissions]

>アプリケーションと委任されたアクセス許可の詳細については、次を参照してください。 [Graph API にアクセスする][]です。

コピー、 **クライアント Id** をクリップボードにします。

![クライアント ID][api-management-aad-app-client-id]

パブリッシャー ポータルに戻りに貼り付けて、 **クライアント Id** Azure Active Directory のアプリケーション構成からコピーします。

![クライアント ID][api-management-client-id]

Azure Active Directory の構成に戻り、をクリックして、 **の期間を選択** ボックスの一覧、 **キー** セクションし、間隔を指定します。 この例では **1 年間** を使用します。

![キー][api-management-aad-key-before-save]

クリックして **保存** を構成を保存し、キーを表示します。 キーをクリップボードにコピーします。

>このキーを書き留めておきます。 Azure Active Directory の構成ウィンドウを閉じると、キーは再表示できません。

![キー][api-management-aad-key-after-save]

パブリッシャー ポータルに戻りにキーを貼り付け、 **クライアント シークレット** テキスト ボックスです。

![クライアント シークレット][api-management-client-secret]

**許可されたテナント** API Management サービス インスタンスの Api にアクセスするディレクトリを指定します。 アクセス許可を付与する Azure Active Directory インスタンスのドメインを指定します。 複数のドメインを指定する場合は、改行文字、スペース、またはコンマで区切ります。

![許可されたテナント][api-management-client-allowed-tenants]

複数のドメインを指定することができます、 **許可されたテナント** セクションです。 アプリケーションが登録されている元のドメインとは別のドメインからユーザーがログインするには、別のドメインの全体管理者がアプリケーションにディレクトリ データへのアクセス許可を付与する必要があります。 アクセス許可を与えるには、グローバル管理者ログインをクリックしてアプリケーション **Accept**します。 次の例では `miaoaad.onmicrosoft.com` に追加されている **許可されたテナント** そのドメインからのグローバル管理者が初めてログインしようとします。

![アクセス許可][api-management-permissions-form]

>全体管理者がアクセス許可を付与する前に全体管理者以外のユーザーがログインしようとすると、ログインに失敗し、エラー画面が表示されます。

必要な構成を指定すると、クリックして **保存**します。

![保存][api-management-client-allowed-tenants-save]

指定された Azure Active Directory 内のユーザーは、次の手順に従って、開発者ポータルにログインできます、変更が保存さたら [Azure Active Directory アカウントを使用して開発者ポータルにログイン][]します。

## 外部の Azure Active Directory グループを追加する方法

Azure Active Directory 内のユーザーのアクセスを有効にした後は、Azure Active Directory のグループを API Management に追加することで、グループ内の開発者と目的の成果物の関連付けを管理しやすくすることができます。

> 外部の Azure Active Directory グループを構成するには、先に前のセクションの手順を実行して、[外部 ID] タブで Azure Active Directory を構成する必要があります。 

外部の Azure Active Directory グループの追加は、 **可視性** グループへのアクセスを許可する製品のタブをクリックします。 クリックして **製品**, 、目的の成果物の名前をクリックします。

![成果物の構成][api-management-configure-product]

切り替えて、 **可視性** タブをクリックし、クリックして **Azure Active Directory からグループを追加**します。

![グループの追加][api-management-add-groups]

選択、 **Azure Active Directory テナント** ドロップダウン リスト ボックスの一覧し、目的のグループの名前を入力、 **グループ** テキスト ボックスを追加します。

![グループの選択][api-management-select-group]

このグループの名前は記載されて、 **グループ** 次の例のように、Azure Active Directory 一覧を表示します。

![Azure Active Directory グループの一覧][api-management-aad-groups-list]

クリックして **追加** をグループ名を検証し、グループを追加します。 この例では、 **Contoso 5 Developers** 外部グループが追加されました。 

![グループが追加された][api-management-aad-group-added]

クリックして **保存** を新しいグループの選択を保存します。

1 つの製品から Azure Active Directory グループを構成すると、使用可能になるチェック ボックス、 **可視性** API Management サービス インスタンスでその他の製品] タブをクリックします。

確認してが追加されたら、外部グループのプロパティを構成からグループの名前をクリックして、 **グループ** ] タブをクリックします。

![グループの管理][api-management-groups]

ここで編集することができます、 **名前** と **説明** グループのです。

![グループの編集][api-management-edit-group]

構成した Azure Active Directory のユーザーは、開発者ポータルにログインし、次のセクションの指示に従って、可視性を有効化されたグループの表示とサブスクライブを実行できます。

## Azure Active Directory アカウントを使用して開発者ポータルにログインする方法

前のセクションで構成されている Azure Active Directory アカウントを使用して開発者ポータルにログインして、新しいブラウザー ウィンドウを使用して、開き、 **サインオン URL** をクリックして、Active Directory のアプリケーション構成から **Azure Active Directory**します。

![開発者ポータル][api-management-dev-portal-signin]

Azure Active Directory で、ユーザーの 1 つの資格情報を入力し、クリックして **サインイン**します。

![サインイン][api-management-aad-signin]

追加情報が必要な場合は、登録フォームが表示されることがあります。 登録フォームに入力し、クリックして **サインアップ**します。

![登録][api-management-complete-registration]

ユーザーが API Management サービス インスタンスの開発者ポータルにログインしました。

![登録の完了][api-management-registration-complete]



[api-management-management-console]: ./media/api-management-howto-aad/api-management-management-console.png
[api-management-security-external-identities]: ./media/api-management-howto-aad/api-management-security-external-identities.png
[api-management-security-aad-new]: ./media/api-management-howto-aad/api-management-security-aad-new.png
[api-management-new-aad-application-menu]: ./media/api-management-howto-aad/api-management-new-aad-application-menu.png
[api-management-new-aad-application-1]: ./media/api-management-howto-aad/api-management-new-aad-application-1.png
[api-management-new-aad-application-2]: ./media/api-management-howto-aad/api-management-new-aad-application-2.png
[api-management-new-aad-app-created]: ./media/api-management-howto-aad/api-management-new-aad-app-created.png
[api-management-aad-app-permissions]: ./media/api-management-howto-aad/api-management-aad-app-permissions.png
[api-management-aad-app-client-id]: ./media/api-management-howto-aad/api-management-aad-app-client-id.png
[api-management-client-id]: ./media/api-management-howto-aad/api-management-client-id.png
[api-management-aad-key-before-save]: ./media/api-management-howto-aad/api-management-aad-key-before-save.png
[api-management-aad-key-after-save]: ./media/api-management-howto-aad/api-management-aad-key-after-save.png
[api-management-client-secret]: ./media/api-management-howto-aad/api-management-client-secret.png
[api-management-client-allowed-tenants]: ./media/api-management-howto-aad/api-management-client-allowed-tenants.png
[api-management-client-allowed-tenants-save]: ./media/api-management-howto-aad/api-management-client-allowed-tenants-save.png
[api-management-aad-delegated-permissions]: ./media/api-management-howto-aad/api-management-aad-delegated-permissions.png
[api-management-dev-portal-signin]: ./media/api-management-howto-aad/api-management-dev-portal-signin.png
[api-management-aad-signin]: ./media/api-management-howto-aad/api-management-aad-signin.png
[api-management-complete-registration]: ./media/api-management-howto-aad/api-management-complete-registration.png
[api-management-registration-complete]: ./media/api-management-howto-aad/api-management-registration-complete.png
[api-management-aad-app-multi-tenant]: ./media/api-management-howto-aad/api-management-aad-app-multi-tenant.png
[api-management-aad-reply-url]: ./media/api-management-howto-aad/api-management-aad-reply-url.png
[api-management-permissions-form]: ./media/api-management-howto-aad/api-management-permissions-form.png
[api-management-configure-product]: ./media/api-management-howto-aad/api-management-configure-product.png
[api-management-add-groups]: ./media/api-management-howto-aad/api-management-add-groups.png
[api-management-select-group]: ./media/api-management-howto-aad/api-management-select-group.png
[api-management-aad-groups-list]: ./media/api-management-howto-aad/api-management-aad-groups-list.png
[api-management-aad-group-added]: ./media/api-management-howto-aad/api-management-aad-group-added.png
[api-management-groups]: ./media/api-management-howto-aad/api-management-groups.png
[api-management-edit-group]: ./media/api-management-howto-aad/api-management-edit-group.png

[How to add operations to an API]: api-management-howto-add-operations.md
[How to add and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: api-management-monitoring.md
[Add APIs to a product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Get started with Azure API Management]: api-management-get-started.md
[Get started with advanced API configuration]: api-management-get-started-advanced.md
[API Management policy reference]: api-management-policy-reference.md
[Caching policies]: api-management-policy-reference.md#caching-policies
[Create an API Management service instance]: api-management-get-started.md#create-service-instance

[http://oauth.net/2/]: http://oauth.net/2/
[WebApp-GraphAPI-DotNet]: https://github.com/AzureADSamples/WebApp-GraphAPI-DotNet
[Accessing the Graph API]: http://msdn.microsoft.com/library/azure/dn132599.aspx#BKMK_Graph

[Prerequisites]: #prerequisites
[Configure an OAuth 2.0 authorization server in API Management]: #step1
[Configure an API to use OAuth 2.0 user authorization]: #step2
[Test the OAuth 2.0 user authorization in the Developer Portal]: #step3
[Next steps]: #next-steps

[Log in to the Developer portal using an Azure Active Directory account]: #Log-in-to-the-Developer-portal-using-an-Azure-Active-Directory-account



