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


# Mobile Services での Twitter ログイン用のアプリケーションの登録

[AZURE.INCLUDE [mobile-service-note-mobile-apps](../../includes/mobile-services-note-mobile-apps.md)]

&nbsp;


[AZURE.INCLUDE [mobile-services-selector-register-identity-provider](../../includes/mobile-services-selector-register-identity-provider.md)]

このトピックでは、Twitter を使用して Azure Mobile Services で認証できるようにアプリケーションを登録する方法について説明します。
>[AZURE.NOTE] このチュートリアルはについて [Azure Mobile Services](http://azure.microsoft.com/services/mobile-services/), 、任意のプラットフォームの拡張性の高いモバイル アプリケーションを構築するためのソリューションです。 Mobile Services によって簡単にデータの同期化を行い、ユーザーを認証して、プッシュ通知を送信できます。 このページをサポートしています、 [アプリへの認証の追加](mobile-services-ios-get-started-users.md) チュートリアル アプリにユーザーをサインインさせる方法を説明します。 初めてのモバイル サービスを使用する場合は、このチュートリアルを完了してください [モバイル サービスを使ってみる](mobile-services-ios-get-started.md)します。

このトピックの手順を完了するには、検証済みの電子メール アドレスを持つ Twitter アカウントが必要になります。 新しい Twitter アカウントを作成するには <a href="http://go.microsoft.com/fwlink/p/?LinkID=268287" target="_blank">twitter.com</a>します。

1. 移動し、 [Twitter デベロッパー](http://go.microsoft.com/fwlink/p/?LinkId=268300) web サイト、Twitter アカウント資格情報でサインインし、をクリックして **新しいアプリの作成**します。

2. **[Name]**、**[Description]**、**[Website]** にアプリの値を入力し、次のいずれかの URL 形式を **[Callback URL]** に入力します。

    + **.NET バックエンド**: `https://<mobile_service>.azure-mobile.net/signin-twitter`
    + **JavaScript バックエンド**: `https://<mobile_service>.azure-mobile.net/login/twitter`
     >[AZURE.NOTE]Mobile Services バックエンドの種類として、正しいリダイレクト URL パスの形式を使用してください。 これが正しくない場合、認証は失敗します。
    &nbsp;

    ![][2]

3.  ページの下部に記載されている条項を読んで同意し、**[Create your Twitter application]** をクリックします。

    これでアプリケーションが登録され、アプリケーションの詳細が表示されます。

6. アプリケーションのダッシュ ボードで **[Keys and Access Tokens]** タブをクリックし、**[Consumer key]** と **[Consumer secret]** の値をメモします。
    > [AZURE.NOTE] コンシューマー シークレットは、重要なセキュリティ資格情報です。 このシークレットは、他のユーザーと共有したり、アプリケーションと共に配布したりしないでください。

7. **[Settings]** タブをクリックして下方向へスクロールし、**[Allow this application to be used to sign in with Twitter]** チェック ボックスをオンにして、**[Update Settings]** をクリックします。

これで、コンシューマー キーとコンシューマー シークレットの値を Mobile Services に渡すことにより、アプリケーションで Twitter ログインを認証に使用する準備ができました。







[1]: ./media/mobile-services-how-to-register-twitter-authentication/mobile-services-twitter-developers.png 
[2]: ./media/mobile-services-how-to-register-twitter-authentication/mobile-services-twitter-register-app1.png 
[twitter developers]: http://go.microsoft.com/fwlink/p/?LinkId=268300 
[get started with authentication]: /develop/mobile/tutorials/get-started-with-users-dotnet/ 

