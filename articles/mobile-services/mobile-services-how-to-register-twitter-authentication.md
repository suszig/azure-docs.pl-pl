<properties
    pageTitle="Twitter 認証用の登録 | Microsoft Azure"
    description="Azure Mobile Services アプリケーションで Twitter 認証を使用する方法について説明します。"
    services="mobile-services"
    documentationCenter=""
    authors="ggailey777"
    manager="dwrede"
    editor=""/>


<tags 
    ms.service="mobile-services" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="multiple" 
    ms.topic="article" 
    ms.date="11/30/2015" 
    ms.author="glenga"/>

#Mobile Services での Twitter ログイン用のアプリケーションの登録

[AZURE.INCLUDE [mobile-service-note-mobile-apps](../../includes/mobile-services-note-mobile-apps.md)]

&nbsp;


[AZURE.INCLUDE [mobile-services-selector-register-identity-provider](../../includes/mobile-services-selector-register-identity-provider.md)]

このトピックでは、Twitter を使用して Azure Mobile Services で認証できるようにアプリケーションを登録する方法について説明します。

>[AZURE.NOTE] このチュートリアルはについて [Azure Mobile Services](http://azure.microsoft.com/services/mobile-services/), 、任意のプラットフォームの拡張性の高いモバイル アプリケーションを構築するためのソリューションです。 Mobile Services によって簡単にデータの同期化を行い、ユーザーを認証して、プッシュ通知を送信できます。 このページをサポートしています、 [アプリへの認証の追加](mobile-services-ios-get-started-users.md) チュートリアル アプリにユーザーをサインインさせる方法を説明します。 Mobile Services を初めて使用する場合は、チュートリアル「[Mobile Services の使用](mobile-services-ios-get-started.md)」を完了することをお勧めします。

このトピックの手順を完了するには、検証済みの電子メール アドレスを持つ Twitter アカウントが必要になります。 新しい Twitter アカウントを作成するには、 <a href="http://go.microsoft.com/fwlink/p/?LinkID=268287" target="_blank">twitter.com</a>.

1. 移動し、 [Twitter デベロッパー](http://go.microsoft.com/fwlink/p/?LinkId=268300) web サイト、Twitter アカウント資格情報でサインインし、をクリックして **新しいアプリの作成**します。

2. 型、 **名前**, 、**説明**, 、および **web サイト** 、アプリケーションでは後で、次の URL のいずれかの書式の種類で、値 **コールバック URL**します。

    + **.NET バックエンド**: `https://<mobile_service>.azure-mobile.net/signin-twitter`
    + **JavaScript バックエンド**: `https://<mobile_service>.azure-mobile.net/login/twitter`

     >[AZURE.NOTE]モバイル サービス バックエンドの種類には、正しいリダイレクト URL パスの形式を使用することを確認します。 これが正しくない場合、認証は失敗します。
    &nbsp;

    ![][2]

3.  ページの下部にある読み取り、条項に同意と順にクリックして **Twitter アプリケーションを作成する**です。

    これでアプリケーションが登録され、アプリケーションの詳細が表示されます。

6. クリックして、 **Keys and Access Tokens** ] タブで、アプリのダッシュ ボードとの値をメモしておいてください **コンシューマー キー** と **コンシューマー シークレット**します。

    > [AZURE.NOTE] コンシューマー シークレットは、重要なセキュリティ資格情報です。 このシークレットは、他のユーザーと共有したり、アプリケーションと共に配布したりしないでください。

7. クリックして、 **設定** ] タブで、下へスクロールし、確認、 **sign in with Twitter を使用するには、このアプリケーションを許可する** チェック ボックスをクリックし、 **設定の更新**します。

これで、コンシューマー キーとコンシューマー シークレットの値を Mobile Services に渡すことにより、アプリケーションで Twitter ログインを認証に使用する準備ができました。

<!-- Anchors. -->

<!-- Images. -->
[1]: ./media/mobile-services-how-to-register-twitter-authentication/mobile-services-twitter-developers.png
[2]: ./media/mobile-services-how-to-register-twitter-authentication/mobile-services-twitter-register-app1.png

<!-- URLs. -->

[Twitter Developers]: http://go.microsoft.com/fwlink/p/?LinkId=268300
[Get started with authentication]: /develop/mobile/tutorials/get-started-with-users-dotnet/

