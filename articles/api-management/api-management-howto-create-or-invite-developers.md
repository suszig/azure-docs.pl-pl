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

新しい開発者を作成する] をクリックして **管理** 、API Management サービスの Azure クラシック ポータルで。 API Management パブリッシャー ポータルが表示されます。 API Management サービス インスタンスを作成がない場合は、次を参照してください。 [API Management サービス インスタンスの作成][] で、 [Azure API Management を使ってみる][] チュートリアルです。

![パブリッシャー ポータル][api-management-management-console]

をクリックして **ユーザー** から、 **API Management** 、左側のメニューをクリックし、 **ユーザーを追加**します。

![開発者を作成する][api-management-create-developer]

入力、 **電子メール**, 、**パスワード**, 、および **名** をクリックして、新しい開発者の **保存**します。

![開発者を作成する][api-management-add-new-user]

新しく作成された開発者アカウントは、既定では、 **Active**, に関連付けられていると、 **開発者** グループです。

![新しい開発者][api-management-new-developer]

開発者アカウントに含まれる、 **active** 状態は、すべてのサブスクリプションを持つ、Api にアクセスするために使用できます。 新しく作成した開発者を他のグループに関連付けるを参照してください。 [グループを開発者に関連付ける方法][]します。

## <a name="invite-developer"> </a>開発者の招待

開発者の招待] をクリックして **ユーザー** から、 **API Management** 、左側のメニューをクリックし、 **ユーザーの招待**します。

![開発者を招待する][api-management-invite-developer]

開発者の名前と電子メール アドレスを入力し、クリックして **招待**します。

![開発者を招待する][api-management-invite-developer-window]

確認メッセージが表示されます。ただし、新しく招待した開発者は、招待を受け入れるまで一覧に表示されません。 

![招待の確認][api-management-invite-developer-confirmation]

開発者を招待すると、招待された開発者に電子メールが送信されます。 この電子メールはテンプレートを使用して生成され、カスタマイズできます。 詳細については、次を参照してください。 [電子メール テンプレートの構成][]します。

招待が受け入れられると、そのアカウントがアクティブになります。

## <a name="block-developer"> </a> 開発者アカウントの非アクティブ化および再アクティブ化

新しく作成または招待された開発者アカウントは、既定では、 **Active**します。 開発者アカウントを無効にする] をクリックして **ブロック**します。 ブロックされている開発者アカウントを再アクティブ化するにはクリックして **Activate**します。 ブロックされている開発者アカウントは、開発者ポータルにアクセスすることも、API を呼び出すこともできません。

![開発者をブロックする][api-management-new-developer]

## <a name="next-steps"> </a>次のステップ

開発者アカウントを作成した後は、このアカウントをロールに関連付け、成果物と API をサブスクライブできます。 詳細については、次を参照してください。 [を作成し、グループを使用する方法][]します。


[api-management-management-console]: ./media/api-management-howto-create-or-invite-developers/api-management-management-console.png
[api-management-add-new-user]: ./media/api-management-howto-create-or-invite-developers/api-management-add-new-user.png
[api-management-create-developer]: ./media/api-management-howto-create-or-invite-developers/api-management-create-developer.png
[api-management-invite-developer]: ./media/api-management-howto-create-or-invite-developers/api-management-invite-developer.png
[api-management-new-developer]: ./media/api-management-howto-create-or-invite-developers/api-management-new-developer.png
[api-management-invite-developer-window]: ./media/api-management-howto-create-or-invite-developers/api-management-invite-developer-window.png
[api-management-invite-developer-confirmation]: ./media/api-management-howto-create-or-invite-developers/api-management-invite-developer-confirmation.png
[api-management-]: ./media/api-management-howto-create-or-invite-developers/api-management-.png



[Create a new developer]: #create-developer
[Invite a developer]: #invite-developer
[Deactivate or reactivate a developer account]: #block-developer
[Next steps]: #next-steps
[How to create and use groups]: api-management-howto-create-groups.md
[How to associate groups with developers]: api-management-howto-create-groups.md#associate-group-developer

[Get started with Azure API Management]: api-management-get-started.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance
[Configure email templates]: api-management-howto-configure-notifications.md#email-templates
