<properties
    pageTitle="Azure Active Directory B2C プレビュー: Amazon の構成 | Microsoft Azure"
    description="Azure Active Directory B2C によってセキュリティ保護されたアプリケーションで、LinkedIn アカウントを使用するコンシューマーにサインアップとサインインを提供します"
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

# Azure Active Directory B2C プレビュー: Amazon アカウントでコンシューマーにサインアップおよびサインインを提供する

[AZURE.INCLUDE [active-directory-b2c-preview-note](../../includes/active-directory-b2c-preview-note.md)]

## Amazon アプリケーションを作成する

Azure Active Directory (AD) B2C で ID プロバイダーとして Amazon を使用するには、最初に Amazon アプリケーションを作成し、適切なパラメーターを提供する必要があります。 これには Amazon アカウントが必要ない場合にそれを取得できます [http://www.amazon.com/](http://www.amazon.com/)します。

1. 移動して、 [Amazon 開発者センター](https://login.amazon.com/) Amazon アカウントの資格情報を使用してサインインします。
2. されていない場合はクリックして **サインアップ**, 開発者登録手順に従います、および、ポリシーに同意します。
3. クリックして **新しいアプリケーションを登録**します。

    ![Amazon - 新しいアプリ](./media/active-directory-b2c-setup-amzn-app/amzn-new-app.png)

4. アプリケーションの情報を提供 (**名前**, 、**説明** と **プライバシーに関する声明の URL**)] をクリック **保存**します。

    ![Amazon - アプリの登録](./media/active-directory-b2c-setup-amzn-app/amzn-register-app.png)

5.  **Web 設定** セクションの値をコピー **クライアント ID** と **クライアント シークレット** (] をクリックする必要があります、 **を表示するシークレット** ] ボタンをクリックすると、これを参照してください)。 ディレクトリで ID プロバイダーとして Amazon を構成するには、両方の値が必要です。 クリックして **編集** セクションの下部にあります。

    > [AZURE.NOTE]
    **クライアント シークレット** は、重要なセキュリティ資格情報。

    ![Amazon - クライアント シークレット](./media/active-directory-b2c-setup-amzn-app/amzn-client-secret.png)

6. 入力 [https://login.microsoftonline.com](https://login.microsoftonline.com) で、 **の許可の JavaScript 生成元** フィールドと [https://login.microsoftonline.com/te/{ディレクトリ}/oauth2/authresp](https://login.microsoftonline.com/te/{directory}/oauth2/authresp) で、 **リターン Url を許可** フィールドに、場所 **{ディレクトリ}** は、ディレクトリの名前 (たとえば、contoso.onmicrosoft.com) で置き換えられます。 クリックして **保存**します。

    > [AZURE.NOTE]
     **{ディレクトリ}** 値は大文字小文字を区別します。

    ![Amazon - URL](./media/active-directory-b2c-setup-amzn-app/amzn-urls.png)

## ディレクトリで ID プロバイダーとして Amazon を構成する

1. [Azure プレビュー ポータルで B2C 機能ブレードに移動](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade)します。
2. B2C 機能ブレードでクリックして **Id プロバイダー**します。
3. クリックして **+ 追加** ブレードの上部にあります。
4. 提供のわかりやすい **名前** id プロバイダーの構成にします。 たとえば、「Amzn」などと入力します。
5. をクリックして **Id プロバイダーの種類**, [ **Amazon** ] をクリック **OK**します。
6. をクリックして **この id プロバイダーを設定** を入力し、 **クライアント ID** と **クライアント シークレット** の前に作成した Amazon アプリケーションです。
7. をクリックして **[ok]** し **作成** Amazon 構成を保存します。

