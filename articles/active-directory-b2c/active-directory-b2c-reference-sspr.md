<properties
    pageTitle="Azure Active Directory B2C プレビュー: セルフサービス パスワード リセット | Microsoft Azure"
    description="Azure Active Directory B2C でコンシューマー向けにセルフサービスによるパスワードのリセットをセットアップする方法を示すトピック"
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
    ms.date="09/22/2015"
    ms.author="swkrish"/>

# Azure Active Directory B2C プレビュー: コンシューマー向けにセルフサービス パスワードのリセットをセットアップする

[AZURE.INCLUDE [active-directory-b2c-preview-note](../../includes/active-directory-b2c-preview-note.md)]

この機能を使用すると、コンシューマー (ローカル アカウントにサインアップしたユーザー) はパスワードを自分でリセットできます。 これによりサポート スタッフの負担を大幅に軽減でき、アプリケーションを定期的に使用するコンシューマーが何百万人もいるような場合に特に効果的です。 現在、回復の手段としては検証済みの電子メール アドレスのみがサポートされています。 将来的には、他の回復方法 (検証済みの電話番号、セキュリティの質問など) が追加される予定です。 既定のディレクトリでは、セルフサービスによるパスワードのリセットは無効になっています。 この機能を有効にするには次のようにします。

1. サインイン、 [Azure ポータル](https://manage.windowsazure.com/) サブスクリプション管理者とします。 これは、ディレクトリを作成するときに使用したものと同じ職場または学校アカウント、または同じ Microsoft アカウントです。
2. 左側のナビゲーション バーで Active Directory 拡張機能に移動します。
3. [ディレクトリの検索、 **ディレクトリ** タブし、それをクリックします。
4. をクリックして、 **構成** ] タブをクリックします。
5. 下へスクロールして、 **ユーザー パスワードのリセット ポリシー** セクションや切り替え、 **パスワードのリセットが有効なユーザー** オプションを **[はい]**します。 注意して、 **連絡用電子メール アドレス** オプションがオンになってはそのまま使用-です。

    ![セルフサービスのパスワード リセット](./media/active-directory-b2c-reference-sspr/sspr.png)
 
6. クリックして **保存** ページの下部にあります。 以上で終わりです。

テストするには、任意のサインイン ポリシー (ID プロバイダーがローカル アカウントのもの) で [今すぐ実行] 機能を使用します。 ローカル アカウントでサインインのページ (ここで入力する電子メール アドレスとパスワードまたはユーザー名とパスワード)、をクリックして **アカウントにアクセスできない?** コンシューマー エクスペリエンスを確認します。

> [AZURE.NOTE]
セルフ サービス パスワード リセット ページはカスタマイズ可能なを使用して、 [会社のブランド機能](active-directory-add-company-branding.md)です。

