<properties
    pageTitle="Azure Active Directory B2C プレビュー: Google+ の構成 | Microsoft Azure"
    description="Azure Active Directory B2C によってセキュリティ保護されたアプリケーションで、Google+ アカウントを使用するコンシューマーにサインアップとサインインを提供します"
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

# Azure Active Directory B2C プレビュー: Google+ アカウントでコンシューマーにサインアップおよびサインインを提供する

[AZURE.INCLUDE [active-directory-b2c-preview-note](../../includes/active-directory-b2c-preview-note.md)]

## Google+ アプリケーションを作成する

Azure Active Directory (AD) B2C で ID プロバイダーとして Google+ を使用するには、最初に Google+ アプリケーションを作成し、適切なパラメーターを提供する必要があります。 これには Google + アカウントが必要ない場合にそれを取得できます [https://accounts.google.com/SignUp](https://accounts.google.com/SignUp)します。

1. 移動して、 [Google Developers Console](https://console.developers.google.com/) Google + アカウントの資格情報を使用してサインインします。
2. をクリックして **プロジェクトの作成**, 、入力、 **プロジェクト名**, サービス利用規約に同意し、[クリックして **作成**します。

    ![G+ - 作業開始](./media/active-directory-b2c-setup-goog-app/google-get-started.png)

    ![G+ - 新しいプロジェクト](./media/active-directory-b2c-setup-goog-app/google-new-project.png)

3. をクリックして **APIs & Auth** し **資格情報** 左側のナビゲーションにします。
4. クリックして、 **OAuth 同意画面** 上部にあるタブをクリックします。

    ![G+ - 資格情報](./media/active-directory-b2c-setup-goog-app/google-add-cred.png)

5. 選択するか、有効な指定 **電子メール アドレス**, 、提供、 **製品名** ] をクリック **保存**します。

    ![G+ - OAuth 同意画面](./media/active-directory-b2c-setup-goog-app/google-consent-screen.png)

6. をクリックして **資格情報を追加** にして **OAuth 2.0 クライアント ID**します。

    ![G+ - OAuth 同意画面](./media/active-directory-b2c-setup-goog-app/google-add-oauth2-client-id.png)

7.  **アプリケーションの種類**, [ **Web アプリケーション**します。

    ![G+ - OAuth 同意画面](./media/active-directory-b2c-setup-goog-app/google-web-app.png)

8. 提供、 **名前** 、アプリケーションでは、次を入力してください。 [https://login.microsoftonline.com](https://login.microsoftonline.com) で、 **Authorized redirect URIs** フィールドと [https://login.microsoftonline.com/te/{tenant}/oauth2/authresp](https://login.microsoftonline.com/te/{tenant}/oauth2/authresp) で、 **Authorized redirect URIs** フィールドに、ここで **{tenant}** はテナントの名前 (たとえば、contosob2c.onmicrosoft.com) で置き換えられます。 クリックして **作成**します。

    > [AZURE.NOTE]
     **{Tenant}** 値は大文字小文字を区別します。

    ![G+ - クライアント ID の作成](./media/active-directory-b2c-setup-goog-app/google-create-client-id.png)

9. 値をコピー **クライアント ID** と **クライアント シークレット**します。 テナントで ID プロバイダーとして Google+ を構成するには、両方の値が必要です。

    > [AZURE.NOTE]
    **クライアント シークレット** は、重要なセキュリティ資格情報。

    ![G+ - クライアント シークレット](./media/active-directory-b2c-setup-goog-app/google-client-secret.png)

## テナントで ID プロバイダーとして Google+ を構成する

1. [Azure プレビュー ポータルで B2C 機能ブレードに移動](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade)します。
2. B2C 機能ブレードでクリックして **Id プロバイダー**します。
3. クリックして **+ 追加** ブレードの上部にあります。
4. 提供のわかりやすい **名前** id プロバイダーの構成にします。 たとえば、「G+」などと入力します。
5. をクリックして **Id プロバイダーの種類**, [ **Google** ] をクリック **OK**します。
6. クリックして **この id プロバイダーを設定** を入力し、 **クライアント ID** と **クライアント シークレット** 前に作成した Google + アプリケーションのです。
7. をクリックして **OK** し **作成** Google + 構成を保存します。

