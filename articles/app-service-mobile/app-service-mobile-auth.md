<properties
    pageTitle="Azure Mobile Apps での認証および承認 | Microsoft Azure"
    description="Azure Mobile Apps の認証および承認機能の概念リファレンスと概要について説明します"
    services="app-service\mobile"
    documentationCenter=""
    authors="mattchenderson" 
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="11/30/2015"
    ms.author="mahender"/>

# Azure Mobile Apps での認証および承認

[AZURE.INCLUDE [app-service-mobile-note-mobile-services](../../includes/app-service-mobile-note-mobile-services.md)]

## App Service の認証および承認とは

App Service の認証および承認とは、アプリのバックエンドのコードを変更せずに、アプリケーションがユーザーのログインを許可する機能です。 これにより、アプリケーションの保護が容易になり、またユーザーごとのデータにも対応できるようになります。

App Service では、フェデレートされた識別情報を使用するサード パーティ製 **id プロバイダー** ("IDP") は、アカウントを格納し、ユーザーの認証、およびアプリケーションは独自の代わりにこの id を使用します。 App Service は、特別な 5 つの id プロバイダーをサポートしています: _Azure Active Directory_, 、_Facebook_, 、_Google_, 、_Microsoft アカウント_, 、および _Twitter_します。 他の ID プロバイダーや独自のカスタム ID ソリューションを統合して、アプリでのこのサポートを拡張することもできます。

アプリで利用できる ID プロバイダーの数には制限がないため、エンド ユーザーにログイン方法のオプションを提供できます。

すぐに開始したい場合は、以下のチュートリアルのいずれかを参照してください。

- [iOS アプリに認証を追加する]
- [Xamarin.iOS アプリに認証を追加する]
- [Xamarin.Android アプリに認証を追加する]
- [Windows アプリに認証を追加する]

## 認証のしくみ

いずれかの ID プロバイダーで認証を行うには、最初にその ID プロバイダーを構成してアプリケーションを把握させる必要があります。 すると、ID プロバイダーが、ID とシークレットを求めてくるので、ユーザーはそれをアプリケーションに戻します。 これにより信頼関係が確立し、App Service は提供された ID を検証できるようになります。

これらの手順の詳細については、以下のトピックを参照してください。

- [Azure Active Directory ログインを使用するようにアプリを構成する方法]
- [Facebook ログインを使用するようにアプリを構成する方法]
- [Google ログインを使用するようにアプリを構成する方法]
- [Microsoft アカウント ログインを使用するようにアプリを構成する方法]
- [Twitter ログインを使用するようにアプリを構成する方法]

バックエンドですべてを構成したら、クライアントを変更してログインできるようにできます。 これには 2 つの方法はあります。

- 1 行のコードを使用して、Mobile Apps クライアント SDK でユーザーがサインインできるようにします。
- 特定の ID プロバイダーが発行している SDK を利用し、ID を確立し、App Service にアクセスできるようにします。

>[AZURE.TIP] よりネイティブ感情ログイン エクスペリエンスを取得し、更新のサポートやその他のプロバイダーに固有のメリットを活用して、ほとんどのアプリケーションはプロバイダー SDK を使用する必要があります。

### プロバイダー SDK の動作のしくみ

プロバイダー SDK の設定を希望しない場合は、Mobile Apps でのログインを許可できます。 Mobile Apps クライアント SDK が、選択したプロバイダーの Web ビューを開き、サインインを完了させます。 サーバーがログイン管理を行い、クライアント SDK がプロバイダーのトークンを受け取ることはないため、これは "サーバー フロー" または "サーバー主導のフロー" と、ブログやフォーラムではときどき呼ばれています。

このフローを開始するために必要なコードは、各プラットフォームの認証のチュートリアルで説明されています。 フローの最後では、クライアント SDK が App Service トークンを取得し、そのトークンはバックエンドへのすべての要求に自動的に添付されます。

### プロバイダー SDK での認証のしくみ

プロバイダー SDK を使用すると、アプリが実行されているプラットフォーム OS とログイン エクスペリエンスがより緊密に通信するようになります。 これにより、プロバイダーのトークンとクライアント上のユーザー情報の一部が得られるので、グラフ API が使用しやすくなったり、ユーザー エクスペリエンスをカスタマイズしやすくなったりします。 クライアント コードがログインを処理し、クライアント コードがプロバイダーのトークンにアクセスするため、これは "クライアント フロー" または "クライアント主導のフロー" と、ブログやフォーラムではときどき呼ばれています。

プロバイダーのトークンが取得されたら、App Service に送信して検証する必要があります。 フローの最後では、クライアント SDK が App Service トークンを取得し、そのトークンはバックエンドへのすべての要求に自動的に添付されます。 希望する場合、開発者はプロバイダー トークンへの参照を保持できます。

## 承認のしくみ

アプリ サービスの認証/承認の方法はいくつかの公開 **要求が認証されていない場合に実行するアクション**します。 コードが特定の要求を受け取る前に、App Service に要求が認証されているかを確認できます。認証されていない場合、拒否し、再試行する前にユーザー ログインを試行させます。

1 つの選択肢として、認証されていない要求を ID プロバイダーのいずれかにリダイレクトさせることができます。 これでは実際、ユーザーは Web ブラウザーで新しいページに移動させられます。 ただし、この方法で、モバイル クライアントをリダイレクトできませんし、認証されていない応答は HTTP を受け取ります。 _401 Unauthorized_ 応答します。 そのため、クライアントが行う最初の要求は必ずログイン エンドポイントに対してである必要があり、それからその他の任意の API を呼び出す必要があります。 ログイン前に別の API を呼び出そうとすると、クライアントにはエラーが表示されます。

認証が必要なエンドポイントをより細かく制御するには、認証されていない要求に対しては "アクションなし (要求を許可)" を選択することも可能です。 この場合、すべての認証の決定は、アプリケーションのコードに委ねられます。 これにより、カスタム承認規則に基づき特定のユーザーにアクセスを許可できます。

## ドキュメント

以下のチュートリアルは、App Service を使用し、モバイル クライアントに認証を追加する方法について説明しています。

- [iOS アプリに認証を追加する]
- [Xamarin.iOS アプリに認証を追加する]
- [Xamarin.Android アプリに認証を追加する]
- [Windows アプリに認証を追加する]

以下のチュートリアルは、さまざまな認証プロバイダーを使用する、App Service の構成方法について説明しています。

- [Azure Active Directory ログインを使用するようにアプリを構成する方法]
- [Facebook ログインを使用するようにアプリを構成する方法]
- [Google ログインを使用するようにアプリを構成する方法]
- [Microsoft アカウント ログインを使用するようにアプリを構成する方法]
- [Twitter ログインを使用するようにアプリを構成する方法]

ここでは、活用することも、以外の id システムを使用する場合、 [.NET server SDK のカスタム認証のサポートをプレビュー](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#custom-auth)します。

上記のフローをいくつかの詳細を検索することもできる、 [App Service の認証の概要](app-service-authentication-overview.md)します。 このトピックには、Mobile Apps では使用されなくなった、しかし概念的な内容は引き続き利用できる App Service ゲートウェイに関する情報も含まれています。

[Add authentication to your iOS app]: app-service-mobile-ios-get-started-users.md
[Add authentication to your Xamarin.iOS app]: app-service-mobile-xamarin-ios-get-started-users.md
[Add authentication to your Xamarin.Android app]: app-service-mobile-xamarin-android-get-started-users.md
[Add Authentication to your Windows app]: app-service-mobile-windows-store-dotnet-get-started-users.md

[How to configure your app to use Azure Active Directory login]: app-service-mobile-how-to-configure-active-directory-authentication.md
[How to configure your app to use Facebook login]: app-service-mobile-how-to-configure-facebook-authentication.md
[How to configure your app to use Google login]: app-service-mobile-how-to-configure-google-authentication.md
[How to configure your app to use Microsoft Account login]: app-service-mobile-how-to-configure-microsoft-authentication.md
[How to configure your app to use Twitter login]: app-service-mobile-how-to-configure-twitter-authentication.md
