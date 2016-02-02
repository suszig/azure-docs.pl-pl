<properties
    pageTitle="Google 認証用の登録 | Microsoft Azure"
    description="Google を使用して Azure Mobile Services で認証できるようにアプリケーションを登録する方法について説明します。"
    services="mobile-services"
    documentationCenter="android"
    authors="ggailey777"
    manager="dwrede"
    editor=""/>


<tags 
    ms.service="mobile-services" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-android" 
    ms.devlang="multiple" 
    ms.topic="article" 
    ms.date="11/30/2015" 
    ms.author="glenga"/>


# Mobile Services での Google ログイン用のアプリケーションの登録

[AZURE.INCLUDE [mobile-service-note-mobile-apps](../../includes/mobile-services-note-mobile-apps.md)]

&nbsp;


[AZURE.INCLUDE [mobile-services-selector-register-identity-provider](../../includes/mobile-services-selector-register-identity-provider.md)]

このトピックでは、Google を使用して Azure Mobile Services で認証できるようにアプリケーションを登録する方法について説明します。
>[AZURE.NOTE] このチュートリアルはについて [Azure Mobile Services](http://azure.microsoft.com/services/mobile-services/), 、任意のプラットフォームの拡張性の高いモバイル アプリケーションを構築するためのソリューションです。 Mobile Services によって簡単にデータの同期化を行い、ユーザーを認証して、プッシュ通知を送信できます。 このページをサポートしています、 [認証を使ってみる](mobile-services-ios-get-started-users.md) チュートリアルでは、ユーザーがアプリにサインインする方法を示しています。
<br/>初めてのモバイル サービスを使用する場合は、このチュートリアルを完了してください [モバイル サービスを使ってみる](mobile-services-ios-get-started.md)します。

このトピックの手順を完了するには、検証済みの電子メール アドレスを持つ Google アカウントが必要になります。 新しい Google アカウントを作成するには <a href="http://go.microsoft.com/fwlink/p/?LinkId=268302" target="_blank">accounts.google.com</a>します。

3. 移動、 [Google api](http://go.microsoft.com/fwlink/p/?LinkId=268303) 、Google アカウント資格情報でサインインの web サイト] をクリックして **プロジェクトの作成**, 、提供、 **プロジェクト名**, 、] をクリックし、 **作成**します。

4. **[Products & services]** ドロップ ダウン メニューで **[API Manager]** をクリックし、**[Social APIs]** で **[Google+ API]**、**[Enable API]** の順にクリックします。

5. をクリックして **資格情報** > **OAuth 同意画面**, 選択してから、 **電子メール アドレス**,  、入力、 **製品名**, 、] をクリック **保存**します。

6. **[Credentials]** タブで、**[Add credentials]**、**[OAuth 2.0 client ID]** の順にクリックした後、**[Web application]** を選択します。

7. **[Authorized JavaScript Origins]** にモバイル サービスの URL を入力し、**[Authorized Redirect URI]** で生成された URL を次のいずれかの URL 形式に置き換えて、**[Create]** をクリックします。

    + **.NET バックエンド**: `https://<mobile_service>.azure-mobile.net/signin-google`
    + **JavaScript バックエンド**: `https://<mobile_service>.azure-mobile.net/login/google`
     >[AZURE.NOTE]Mobile Services バックエンドの種類として、正しいリダイレクト URL パスの形式を使用してください。 これが正しくない場合、認証は失敗します。

8. 次の画面で、表示されているクライアント ID とクライアント シークレットの値を書き留めます。
    > [AZURE.IMPORTANT] クライアント シークレットは、重要なセキュリティ資格情報です。 このシークレットを他のユーザーと共有したり、クライアント アプリケーション内で配信したりしないでください。

アプリ内の認証で Google サインインを使用するようにモバイル サービスを構成する準備が整いました。








[google apis]: http://go.microsoft.com/fwlink/p/?LinkId=268303 
[get started with authentication]: /develop/mobile/tutorials/get-started-with-users-dotnet/ 

