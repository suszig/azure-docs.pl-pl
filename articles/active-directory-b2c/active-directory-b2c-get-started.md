<properties
    pageTitle="Azure Active Directory B2C プレビュー: Azure Active Directory B2C テナントの作成 | Microsoft Azure"
    description="Azure Active Directory B2C テナントの作成方法に関するトピック"
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
    ms.date="09/28/2015"
    ms.author="swkrish"/>

# Azure Active Directory B2C プレビュー: Azure AD B2C テナントを作成する方法

Azure Active Directory (AD) B2C を使い始めるには、以下に示す 3 つの手順を実行します。

[AZURE.INCLUDE [active-directory-b2c-preview-note](../../includes/active-directory-b2c-preview-note.md)]

## 手順 1: Azure サブスクリプションにサインアップする

Azure サブスクリプションが既にある場合は、次の手順に進みます。 ない場合は、サインアップ [Azure サブスクリプション](sign-up-organization.md) し、Azure AD B2C へのアクセスを取得します。

> [AZURE.NOTE]
現在、Azure AD B2C プレビューは無料で使用できますが、制限があります (テナントあたり最大 50,000 ユーザー)。 Azure サブスクリプションにアクセスするために必要な [Azure ポータル](http://manage.windowsazure.com/)します。

## 手順 2: Azure AD B2C テナントを作成する

次の手順に従って、新しい Azure AD B2C テナントを作成します。 現在、既存のディレクトリで B2C の機能を有効にすることはできません。

1. サインイン、 [Azure ポータル](https://manage.windowsazure.com/) サブスクリプション管理者とします。 これは、Azure へのサインアップに使用したものと同じ職場または学校アカウント、または同じ Microsoft アカウントです。
2. クリックして **新しい** > **App Services** > **Active Directory** > **ディレクトリ** > **カスタム作成**します。

    ![テナントの作成](./media/active-directory-b2c-get-started/new-directory.png)

3. 選択、 **名前**, 、**ドメイン名** と **国または地域** テナントのです。
4. というオプションをオンに"**これは B2C ディレクトリ**"です。
5. チェック マークをクリックして設定を完了します。

    ![B2C テナントの作成](./media/active-directory-b2c-get-started/create-b2c-directory.png)

6. テナントが作成され、Active Directory 拡張機能に表示されます。 また、現在のユーザーがテナントのグローバル管理者になります。 必要に応じて、他のグローバル管理者を追加できます。

    > [AZURE.IMPORTANT]
    テナントの作成には最大で 2 分かかることがあります。 テナントの作成時に問題が発生する場合は確認して [記事](active-directory-b2c-support-create-directory.md) ガイダンスです。

## 手順 3: Azure プレビュー ポータルで B2C 機能ブレードに移動する

1. 左側のナビゲーション バーで Active Directory 拡張機能に移動します。
2. [テナントの検索、 **ディレクトリ** タブし、それをクリックします。
3. をクリックして、 **構成** ] タブをクリックします。
4. をクリックして、 **B2C の管理設定** 内のリンク、 **B2C 管理** セクションです。

    ![B2C テナントの作成](./media/active-directory-b2c-get-started/b2c-directory-configure-tab.png)

4. 新しいブラウザー タブまたはウィンドウで、B2C 機能ブレードが含まれた Azure プレビュー ポータルが開きます。

    > [AZURE.IMPORTANT]
    (テナント数が少ない場合に) このページが正しく読み込まれないという既知の問題があります。 この問題は、ブラウザーを更新することで解決されます。 解決されない場合は、サポートにご連絡ください。

5. アクセスしやすいように、このブレードをスタート画面にピン留めします (右上隅)。

    ![B2C 機能ブレード](./media/active-directory-b2c-get-started/b2c-features-blade.png)

    > [AZURE.NOTE]
    ユーザーおよびグループを管理できる、セルフ サービス パスワード リセットの構成と会社ので、テナントの機能をブランド化、 [Azure ポータル](https://manage.windowsazure.com/)します。

## 次のステップ

上に移動 [Azure AD B2C アプリケーションを登録して、クイック スタート アプリケーションを構築](active-directory-b2c-app-registration.md)します。

