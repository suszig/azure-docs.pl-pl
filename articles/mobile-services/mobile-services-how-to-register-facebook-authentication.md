<properties
    pageTitle="Facebook 認証用に登録する | Azure Mobile Services"
    description="Azure Mobile Services アプリケーションで Facebook 認証を使用する方法について説明します。"
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
    ms.date="11/15/2015"
    ms.author="glenga"/>

# Mobile Services での Facebook 認証用のアプリケーションの登録

[AZURE.INCLUDE [mobile-service-note-mobile-apps](../../includes/mobile-services-note-mobile-apps.md)]

&nbsp;


[AZURE.INCLUDE [mobile-services-selector-register-identity-provider](../../includes/mobile-services-selector-register-identity-provider.md)]

このトピックでは、Facebook を使用して Azure Mobile Services で認証できるようにアプリケーションを登録する方法について説明します。

>[AZURE.NOTE] このチュートリアルでは、[Azure Mobile Services], はあらゆるプラットフォームの拡張性の高いモバイル アプリケーションを構築するためのソリューションです。 Mobile Services によって簡単にデータの同期化を行い、ユーザーを認証して、プッシュ通知を送信できます。 このページはアプリケーションへユーザーをログインさせる方法を説明する「[認証の使用](mobile-services-ios-get-started-users.md)」チュートリアルをサポートしています。 Mobile Services を初めて使用する場合は、チュートリアル「[Mobile Services の使用](mobile-services-ios-get-started.md)」を完了することをお勧めします。

このトピックの手順を完了するには、検証済みの電子メール アドレスを持つ Facebook アカウントおよび携帯電話番号が必要になります。 新しい Facebook アカウントを作成するには、[facebook.com](http://go.microsoft.com/fwlink/p/?LinkId=268285) にアクセスしてください。

1. [Facebook Developers](http://go.microsoft.com/fwlink/p/?LinkId=268285) の Web サイトに移動し、Facebook アカウントの資格情報でサインインします。

2. (省略可能)既に登録していない場合、クリックして **My Apps** ] をクリックし、 **開発者として登録する**, で、ポリシーに同意し、登録手順に従います。

3. クリックして **My Apps** > **新しいアプリの追加** > **セットアップの高度な**です。

4. 一意な入力 **表示名** アプリでは、選択 **Apps for Pages** [ **カテゴリ**, 、] をクリックし、 **Create App ID** してセキュリティの手順を完了します。

    これにより、新しい Facebook アプリ ID が作成されます。

5. をクリックして **設定**, にモバイル サービルのドメインを入力 **App Domains**, 、省略可能な入力 **Contact Email**, 、] をクリックして **Add Platform** を選択し、 **web サイト**します。

    ![][3]

6. モバイル サービスの URL を入力 **サイトの URL**, 、クリックして **Save Changes**します。

7. をクリックして **表示**, 、パスワードの入力要求された場合の値をメモしておいてください **App ID** と **App Secret**します。

    ![][5]
    &nbsp;

    >[AZURE.IMPORTANT] アプリケーション シークレットは、重要なセキュリティ資格情報です。 このシークレットは、他のユーザーと共有したり、アプリケーションと共に配布したりしないでください。
    &nbsp;

8. をクリックして、 **詳細** ] タブの [種類で、次の URL のいずれかの書式 **Valid OAuth redirect URIs**, 、] をクリックし、 **変更の保存**:

    + **.NET バックエンド**: `https://<mobile_service>.azure-mobile.net/signin-facebook`
    + **JavaScript バックエンド**: `https://<mobile_service>.azure-mobile.net/login/facebook`

     >[AZURE.NOTE]モバイル サービス バックエンドの種類には、正しいリダイレクト URL パスの形式を使用することを確認します。 これが正しくない場合、認証は失敗します。


9. クリックして **Status & Review** > **はい** アプリへの汎用パブリック アクセスを有効にします。

    新しいアプリを登録するために使用した Facebook アカウントは、アプリの管理者であり、アプリへの管理者としてのアクセス権があります。 この手順で汎用パブリック アクセスが与えられるため、アプリはその他の Facebook アカウントを使用して認証できるようになります。


これで、App ID と App Secret の値を Mobile Services に渡すことにより、Facebook ログインを使用してアプリケーションで認証を使用する準備ができました。

<!-- Anchors. -->

<!-- Images. -->
[3]: ./media/mobile-services-how-to-register-facebook-authentication/mobile-services-facebook-configure-app.png
[5]: ./media/mobile-services-how-to-register-facebook-authentication/mobile-services-facebook-completed.png

<!-- URLs. -->
[Facebook Developers]: http://go.microsoft.com/fwlink/p/?LinkId=268286
[Get started with authentication]: /develop/mobile/tutorials/get-started-with-users-dotnet/
[Azure Mobile Services]: http://azure.microsoft.com/services/mobile-services/

