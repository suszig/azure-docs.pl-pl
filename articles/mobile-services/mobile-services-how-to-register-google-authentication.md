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
<br/>Mobile Services を初めて使用する場合は、チュートリアル「[Mobile Services の使用](mobile-services-ios-get-started.md)」を完了することをお勧めします。

このトピックの手順を完了するには、検証済みの電子メール アドレスを持つ Google アカウントが必要になります。 新しい Google アカウントを作成するには、<a href="http://go.microsoft.com/fwlink/p/?LinkId=268302" target="_blank">accounts.google.com</a> にアクセスしてください。

3. 移動、 [Google api](http://go.microsoft.com/fwlink/p/?LinkId=268303) 、Google アカウント資格情報でサインインの web サイト] をクリックして **プロジェクトの作成**, 、提供、 **プロジェクト名**, 、] をクリックし、 **作成**します。

4.  **製品 & サービス** ] ドロップ ダウン メニューをクリックして **API Manager**, 、[ **ソーシャル Api** ] をクリックして **Google + API** > **を有効にする API**します。

5. をクリックして **資格情報** > **OAuth 同意画面**, 選択してから、 **電子メール アドレス**,  、入力、 **製品名**, 、] をクリック **保存**します。

6.  **資格情報** ] タブ、[ **資格情報を追加** > **OAuth 2.0 クライアント ID**, 選択してから、 **Web アプリケーション**します。


7. モバイル サービスの URL を入力 **Authorized JavaScript Origins**, 、生成された URL を置き換える **Authorized Redirect URI** クリックして次の URL の形式のいずれかで **作成**:
 

    + **.NET backend**: `https://<mobile_service>.azure-mobile.net/signin-google`
    + **JavaScript backend**: `https://<mobile_service>.azure-mobile.net/login/google`

     >[AZURE.NOTE]Make sure that you use the correct redirect URL path format for your type of Mobile Services backend. When this is incorrect, authentication will not succeed.

8. 次の画面で、表示されているクライアント ID とクライアント シークレットの値を書き留めます。

    > [AZURE.IMPORTANT] クライアント シークレットは、重要なセキュリティ資格情報です。 このシークレットを他のユーザーと共有したり、クライアント アプリケーション内で配信したりしないでください。

アプリ内の認証で Google サインインを使用するようにモバイル サービスを構成する準備が整いました。

<!-- Anchors. -->

<!-- Images. -->

<!-- URLs. -->

[Google apis]: http://go.microsoft.com/fwlink/p/?LinkId=268303
[Get started with authentication]: /develop/mobile/tutorials/get-started-with-users-dotnet/


