<properties
    pageTitle="Azure Active Directory B2C プレビュー: LinkedIn の構成 | Microsoft Azure"
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

# Azure Active Directory (AD) B2C プレビュー: LinkedIn アカウントでコンシューマーにサインアップおよびサインインを提供する

[AZURE.INCLUDE [active-directory-b2c-preview-note](../../includes/active-directory-b2c-preview-note.md)]

## LinkedIn アプリケーションを作成する

Azure Active Directory (AD) B2C で ID プロバイダーとして LinkedIn を使用するには、最初に LinkedIn アプリケーションを作成し、適切なパラメーターを提供する必要があります。 これには (linkedin) アカウントが必要ない場合にそれを取得できます [https://www.linkedin.com/](https://www.linkedin.com/)します。

1. 移動して、 [LinkedIn デベロッパーの web サイト](https://www.developer.linkedin.com/) LinkedIn アカウントの資格情報を使用してサインインします。
2. クリックして **My Apps** 上部のメニュー バーでし **アプリケーションの作成**します。

    ![LinkedIn - 新しいアプリ](./media/active-directory-b2c-setup-li-app/linkedin-new-app.png)

3.  **新しいアプリケーションを作成** フォームで、関連する情報を入力 (**会社名**, 、**名前**, 、**説明**, 、**アプリケーション ロゴの URL**, 、**アプリケーションの使用**, 、**web サイトの URL**, 、**ビジネス電子メール** と **勤務先電話番号**)。
4. LinkedIn API の使用条件に同意し、クリックして **送信**します。

    ![LinkedIn - アプリの登録](./media/active-directory-b2c-setup-li-app/linkedin-register-app.png)

5. 値をコピー **クライアント ID** と **クライアント シークレット** (下に検索できる、 **認証キー** セクション)。 ディレクトリで ID プロバイダーとして LinkedIn を構成するには、両方の値が必要です。

    > [AZURE.NOTE]
    **クライアント シークレット** は、重要なセキュリティ資格情報。

6. 入力 [https://login.microsoftonline.com/te/{ディレクトリ}/oauth2/authresp](https://login.microsoftonline.com/te/{directory}/oauth2/authresp) で、 **リダイレクト Url の承認** フィールド (下にある、 **OAuth 2.0** セクション) ここで、 **{ディレクトリ}** をディレクトリ名 (たとえば、contoso.onmicrosoft.com) に置き換える] をクリックして **追加**します。 クリックし、 **更新**します。

    ![LinkedIn - アプリの設定](./media/active-directory-b2c-setup-li-app/linkedin-setup.png)

## ディレクトリで ID プロバイダーとして LinkedIn を構成する

1. [Azure プレビュー ポータルで B2C 機能ブレードに移動](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade)します。
2. B2C 機能ブレードでクリックして **Id プロバイダー**します。
3. クリックして **+ 追加** ブレードの上部にあります。
4. 提供のわかりやすい **名前** id プロバイダーの構成にします。 たとえば、「LI」などと入力します。
5. をクリックして **Id プロバイダーの種類**, [ **LinkedIn** ] をクリック **OK**します。
6. をクリックして **この id プロバイダーを設定** を入力し、 **クライアント ID** と **クライアント シークレット** の前に作成した LinkedIn アプリケーションです。
7. をクリックして **[ok]** し **作成** LinkedIn 構成を保存します。

