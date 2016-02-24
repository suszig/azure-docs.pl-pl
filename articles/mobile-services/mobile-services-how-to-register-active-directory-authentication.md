<properties
    pageTitle="Azure Active Directory 認証用の登録 | Microsoft Azure"
    description="Mobile Services アプリケーションで Azure Active Directory 認証用に登録する方法について説明します。"
    authors="wesmc7777"
    services="mobile-services"
    documentationCenter=""
    manager="dwrede"
    editor=""/>

<tags
    ms.service="mobile-services"
    ms.workload="mobile"
    ms.tgt_pltfrm="multiple"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="11/15/2015"
    ms.author="ricksal"/>

# アプリケーションを登録して Azure Active Directory アカウント ログインを使用する

[AZURE.INCLUDE [mobile-service-note-mobile-apps](../../includes/mobile-services-note-mobile-apps.md)]

&nbsp;


[AZURE.INCLUDE [mobile-services-selector-register-identity-provider](../../includes/mobile-services-selector-register-identity-provider.md)]

##概要

このトピックでは、モバイル サービスの認証プロバイダーとして Azure Active Directory を使用できるようにするためにアプリを登録する方法を説明します。

##アプリを登録する

>[AZURE.NOTE] 使用するこのトピックで説明する手順は、 [Mobile Services アプリへの認証の追加](../mobile-services-dotnet-backend-windows-store-dotnet-get-started-users.md) を使用する場合にチュートリアル「 [サービス主導型ログイン操作](http://msdn.microsoft.com/library/azure/dn283952.aspx) アプリの使用。 また、アプリの要件がある場合 [クライアント主導型ログイン操作](http://msdn.microsoft.com/library/azure/jj710106.aspx) Azure Active Directory と .NET バックエンド モバイル サービスを開始する必要があります、 [Active Directory 認証ライブラリ シングル サインオンによるアプリケーションの認証](mobile-services-windows-store-dotnet-adal-sso-authentication.md) チュートリアルです。

1. [Azure クラシック ポータル] にログオンをクリックして、モバイル サービスに移動、 **Identity** ] タブの [下へスクロールして、 **Azure active directory** id プロバイダーのセクションとコピー、 **アプリケーション URL の** 表示されます。

    ![AAD 用のモバイル サービス アプリ URL](./media/mobile-services-how-to-register-active-directory-authentication/mobile-services-copy-app-url-waad-auth.png)

2. 移動 **Active Directory** [Azure クラシック ポータル] のディレクトリをクリックし、 **ドメイン** ディレクトリの既定のドメインのメモに記録します。

3. クリックして **アプリケーション** > **追加** > **[組織が開発中のアプリケーションを追加**します。

4. アプリケーションの追加ウィザードで入力、 **名前** ] をクリックしてアプリケーションの  **Web アプリケーションや Web API** 型です。

    ![AAD アプリに名前を付ける](./media/mobile-services-how-to-register-active-directory-authentication/mobile-services-add-app-wizard-1-waad-auth.png)

5.  **サインオン URL** ボックスで、モバイル サービスからコピーしたアプリケーションの URL の値を貼り付けます。 同じ一意の値を入力して、 **App ID URI** ボックスで、[続行] をクリックします。

    ![AAD アプリのプロパティを設定する](./media/mobile-services-how-to-register-active-directory-authentication/mobile-services-add-app-wizard-2-waad-auth.png)

6. クリックして、アプリケーションを追加した後、 **構成** ] タブをクリックし、コピー、 **クライアント ID** アプリのです。

    >[AZURE.NOTE].NET バックエンド モバイル サービスを編集する必要も、 **応答 URL** ] の [値 **でのシングル サインオン** 末尾のパス、モバイル サービスの URL である _/signin-aad_します。 たとえば、  `https://todolist.azure-mobile.net/signin-aad`

7. モバイル サービスに返す **Identity** タブし、コピーした **クライアント ID** 、azure active directory id プロバイダーの値。

    ![](./media/mobile-services-how-to-register-active-directory-authentication/mobile-services-clientid-pasted-waad-auth.png)

8.   **許可されたテナント** ボックスの一覧で、アプリケーションを登録するディレクトリのドメインを入力 (など `contoso.onmicrosoft.com`)、をクリックし、 **保存**します。

これで、アプリケーションで認証に Azure Active Directory を使用する準備ができました。

<!-- Anchors. -->

<!-- Images. -->


<!-- URLs. -->
[Azure classic portal]: https://manage.windowsazure.com/



