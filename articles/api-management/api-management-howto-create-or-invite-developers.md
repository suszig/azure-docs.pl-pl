<properties 
    pageTitle="Azure API Management でユーザー アカウントを管理する方法" 
    description="Azure API Management でユーザーを作成または招待する方法について説明します。" 
    services="api-management" 
    documentationCenter="" 
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


# Azure API Management でユーザー アカウントを管理する方法

API Management では、開発者は、API Management を使用して公開された API のユーザーになります。 このガイドでは、開発者を作成する方法と、API Management インスタンスで公開した API および成果物を案内する招待メッセージを開発者に送信する方法について説明します。

## <a name="create-developer"> </a>新しい開発者の作成

新しい開発者を作成するには、API Management サービスの Azure クラシック ポータルで **[管理]** をクリックします。 API Management パブリッシャー ポータルが表示されます。 API Management サービス インスタンスを作成がない場合は、次を参照してください。 [API Management サービス インスタンスの [] を作成する][] で、 [Azure API Management の [] を使ってみる][] チュートリアルです。

![パブリッシャー ポータル][api-management-management-console]

左側の **[API Management]** メニューの **[ユーザー]** をクリックし、**[ユーザーの追加]** をクリックします。

![開発者を作成する][api-management-create-developer]

新しい開発者の**電子メール**、**パスワード**、および**名前**を入力し、**[保存]** をクリックします。

![Create developer][api-management-add-new-user]

既定では、新しく作成された開発者アカウントは**アクティブ**になり、**開発者**グループに関連付けられます。

![New developer][api-management-new-developer]

**[アクティブ]** 状態の開発者アカウントを使用すると、サブスクリプションがあるすべての API にアクセスできます。 新しく作成した開発者を他のグループに関連付けるを参照してください。 [開発者 [] でグループに関連付ける方法][]します。

## <a name="invite-developer"> </a>開発者の招待

開発者を招待するには、左側の **[API Management]** メニューの **[ユーザー]** をクリックし、**[ユーザーの招待]** をクリックします。

![開発者を招待する][api-management-invite-developer]

開発者の名前と電子メール アドレスを入力し、**[招待]** をクリックします。

![開発者を招待する][api-management-invite-developer-window]

確認メッセージが表示されます。ただし、新しく招待した開発者は、招待を受け入れるまで一覧に表示されません。

![招待の確認][api-management-invite-developer-confirmation]

開発者を招待すると、招待された開発者に電子メールが送信されます。 この電子メールはテンプレートを使用して生成され、カスタマイズできます。 詳細については、次を参照してください。 [構成電子メール テンプレートの []][]します。

招待が受け入れられると、そのアカウントがアクティブになります。

## <a name="block-developer"> </a> 非アクティブ化または再アクティブ化の開発者アカウント

既定では、新しく作成または招待された開発者アカウントは**アクティブ**になります。 開発者アカウントを非アクティブ化するには、**[ブロック]** をクリックします。 ブロックされている開発者アカウントを再アクティブ化するには、**[アクティブ化]** をクリックします。 ブロックされている開発者アカウントは、開発者ポータルにアクセスすることも、API を呼び出すこともできません。

![開発者をブロックする][api-management-new-developer]

## <a name="next-steps"> </a>次のステップ

開発者アカウントを作成した後は、このアカウントをロールに関連付け、成果物と API をサブスクライブできます。 詳細については、次を参照してください。 [を作成して使用する方法 [] にグループ化][]します。



[api-management-management-console]: ./media/api-management-howto-create-or-invite-developers/api-management-management-console.png 
[api-management-add-new-user]: ./media/api-management-howto-create-or-invite-developers/api-management-add-new-user.png 
[api-management-create-developer]: ./media/api-management-howto-create-or-invite-developers/api-management-create-developer.png 
[api-management-invite-developer]: ./media/api-management-howto-create-or-invite-developers/api-management-invite-developer.png 
[api-management-new-developer]: ./media/api-management-howto-create-or-invite-developers/api-management-new-developer.png 
[api-management-invite-developer-window]: ./media/api-management-howto-create-or-invite-developers/api-management-invite-developer-window.png 
[api-management-invite-developer-confirmation]: ./media/api-management-howto-create-or-invite-developers/api-management-invite-developer-confirmation.png 
[api-management-]: ./media/api-management-howto-create-or-invite-developers/api-management-.png 
[create a new developer]: #create-developer 
[invite a developer]: #invite-developer 
[deactivate or reactivate a developer account]: #block-developer 
[next steps]: #next-steps 
[how to create and use groups]: api-management-howto-create-groups.md 
[how to associate groups with developers]: api-management-howto-create-groups.md#associate-group-developer 
[get started with azure api management]: api-management-get-started.md 
[create an api management service instance]: api-management-get-started.md#create-service-instance 
[configure email templates]: api-management-howto-configure-notifications.md#email-templates 

