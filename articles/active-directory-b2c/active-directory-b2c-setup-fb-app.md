<properties
    pageTitle="Azure Active Directory B2C プレビュー: Facebook の構成 | Microsoft Azure"
    description="Azure Active Directory B2C によってセキュリティ保護されたアプリケーションで、Facebook アカウントを使用するコンシューマーにサインアップとサインインを提供します"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="msmbaldwin"
    editor="curtand"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/08/2015"
    ms.author="swkrish"/>

# Azure Active Directory B2C プレビュー: Facebook アカウントでコンシューマーにサインアップおよびサインインを提供する

[AZURE.INCLUDE [active-directory-b2c-preview-note](../../includes/active-directory-b2c-preview-note.md)]

## Facebook アプリケーションを作成する

Azure Active Directory (AD) B2C で ID プロバイダーとして Facebook を使用するには、最初に Facebook アプリケーションを作成し、適切なパラメーターを提供する必要があります。 これには、Facebook アカウントは必要があります。ない場合にそれを取得できます [https://www.facebook.com/](https://www.facebook.com/)します。

1. 移動して、 [Facebook Developers の web サイト](https://developers.facebook.com/) 、Facebook アカウントの資格情報を使用してサインインします。
2. されていない場合はクリックして **アプリ** クリックして **開発者として登録する**, で、ポリシーに同意し、登録手順に従います。
3. クリックして **アプリ** し **新しいアプリの追加**します。 選択し、 **web サイト** クリックして、プラットフォームとして **Skip と Create App ID**します。

    ![FB - 新規アプリの追加](./media/active-directory-b2c-setup-fb-app/fb-add-new-app.png)

    ![FB - 新規アプリの追加 - Web サイト](./media/active-directory-b2c-setup-fb-app/fb-add-new-app-website.png)

    ![FB - アプリ ID の作成](./media/active-directory-b2c-setup-fb-app/fb-new-app-skip.png)

4. フォームで、提供、 **表示名**, 、適切な **カテゴリ** ] をクリック **Create App ID**します。 注: Facebook プラットフォームのポリシーを受け入れ、オンライン セキュリティ チェックを完了する必要があります。

    ![FB - アプリ ID の作成](./media/active-directory-b2c-setup-fb-app/fb-create-app-id.png)

5. クリックして **設定** 左側のナビゲーションにします。 有効な入力 **Contact Email**します。
6. をクリックして **プラットフォームの追加 +** し、[ **web サイト**します。

    ![FB - 設定](./media/active-directory-b2c-setup-fb-app/fb-settings.png)

    ![FB - 設定](./media/active-directory-b2c-setup-fb-app/fb-website.png)

7. 入力 [https://login.microsoftonline.com/](https://login.microsoftonline.com/) で、 **サイトの URL** フィールドをクリックして **Save Changes**します。
8. 値をコピー **App ID**します。 クリックして **表示** の値をコピーし、 **App Secret**します。 ディレクトリで ID プロバイダーとして Facebook を構成するには、両方の値が必要です。

    > [AZURE.NOTE]
    **アプリケーション シークレット** は、重要なセキュリティ資格情報。

    ![FB - サイト URL](./media/active-directory-b2c-setup-fb-app/fb-site-url.png)

9. をクリックして、 **詳細** ] タブの上部にあると入力し、 [https://login.microsoftonline.com/te/{ディレクトリ}/oauth2/authresp](https://login.microsoftonline.com/te/{directory}/oauth2/authresp) で、 **Valid OAuth redirect URIs** フィールド (で、 **セキュリティ** セクション) ここで、 **{ディレクトリ}** は、ディレクトリの名前 (たとえば、contosob2c.onmicrosoft.com) で置き換えられます。 クリックして **変更の保存** ページの下部にあります。

    ![FB - OAuth リダイレクト URI](./media/active-directory-b2c-setup-fb-app/fb-oauth-redirect-uri.png)

10. Facebook アプリケーションを Azure AD B2C で使用できるようにするには、アプリケーションをパブリックに利用できるようにする必要があります。 をクリックしてこれを行う **Status & Review** 、左側のナビゲーションと、ページの上部にあるスイッチをオンに **はい**します。 をクリックして **Confirm**します。

    ![FB - OAuth リダイレクト URI](./media/active-directory-b2c-setup-fb-app/fb-app-public.png)

## ディレクトリで ID プロバイダーとして Facebook を構成する

1. [Azure プレビュー ポータルで B2C 機能ブレードに移動](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade)します。
2. B2C 機能ブレードでクリックして **Id プロバイダー**します。
3. クリックして **+ 追加** ブレードの上部にあります。
4. 提供のわかりやすい **名前** id プロバイダーの構成にします。 たとえば、「FB」などと入力します。
5. をクリックして **Id プロバイダーの種類**, [ **Facebook** ] をクリック **OK**します。
6. をクリックして **この id プロバイダーを設定** を入力し、 **App ID** と **アプリケーション シークレット** 内に作成して Facebook アプリケーションの **クライアント ID** と **クライアント シークレット** フィールドそれぞれします。
7. をクリックして **[ok]** し **作成** Facebook 構成を保存します。

