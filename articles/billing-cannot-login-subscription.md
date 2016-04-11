<properties
    pageTitle="Azure サブスクリプションにログインして管理することができない | Microsoft Azure"
    description="Azure サブスクリプションのログインに関するよくあるいくつかの問題のトラブルシューティング情報を掲載しています。"
    services="billing"
    documentationCenter=""
    authors="genlin"
    manager="jarrettr"
    editor="na"
    tags="billing"
    />

<tags
    ms.service="billing"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="11/30/2015"
    ms.author="genli"/>

# Azure サブスクリプションにログインして管理することができない

この記事は、ログインの問題のよくあるいくつかの原因のトラブルシューティングに役立ちます。

## どのポータルにアクセスしようとしていますか?

アカウント所有者にのみアクセスできます、 [アカウント センター](https://account.windowsazure.com/) サービス管理者 (SA) との共同管理者 (CA) へのアクセスには [Azure ポータル](https://manage.windowsazure.com/)します。

管理者ロールの更新方法については、以下のリンクを参照してください。

- [アカウントの SA を更新するには](./billing-add-change-azure-subscription-administrator.md#change-service-administrator-for-a-subscription)

- [管理ポータルで CA を新規に追加するには](./billing-add-change-azure-subscription-administrator.md#add-a-co-administrator-for-a-subscription)

## サブスクリプションは Microsoft アカウントまたは組織アカウントに関連付けられていますか?

Microsoft アカウントとはお使いの電子メール アドレスとそのパスワードです。これを使用して、Windows Live プログラムまたはサービス (Outlook、Hotmail、MSN、OneDrive など) にサインインします。 Microsoft アカウントを設定するには、会社の電子メールなど、所有する任意の電子メール アドレスを使用します。 参照してください [www.microsoft.com/account](http://www.microsoft.com/account) 詳細です。

アカウントを組織アカウントに関連付けた場合、適切なログイン オプション次のように選択してください。 組織アカウントの使用の詳細については、次を参照してください [組織として Azure にサインアップ](./active-directory/sign-up-organization.md):。

![サインイン ページ](./media/billing-cannot-login-subscription/signin.png)

## 共同管理者: 他のアカウントを管理するのに適切な種類のアカウントを使用していますか?

- Microsoft アカウントを使用してログインした場合、他の Microsoft アカウントを共同管理者として追加することしかできません。 これは、セキュリティの要件がない組織アカウントが特定のアカウント (例: janedoe@contoso.com) が有効なアカウントである場合を検出しないようにします。
- 組織アカウントを使用してログインした場合、組織内の他の組織アカウントを共同管理者として追加することができます。 たとえば、abby@contoso.com bob@contoso.com をサービス管理者または共同管理者として追加できますが、john@notcontoso.com を追加することはできません。 組織アカウントでログインしているユーザーは、Microsoft アカウント ユーザーをサービス管理者または共同管理者として追加することもできます。

組織アカウントで Azure にログインできるようになると、サービス管理者 (SA) と共同管理者 (CA) アカウントの要件は次のように変わります。

| ログイン方法| Microsoft アカウントを共同管理者またはサービス管理者として追加できる  |同じ組織内の組織アカウントを共同管理者またはサービス管理者として追加できる |異なる組織の組織アカウントを共同管理者またはサービス管理者として追加できる
| ------------- | ------------- |---------------|---------------|
|Microsoft アカウント |はい|なし|いいえ|
|組織アカウント|はい|はい|いいえ|

## キャッシュ/Cookie の削除、IE InPrivate ブラウズ モードの使用、および他のブラウザーの使用

この記事のどの時点でもその他のヘルプを必要がある場合で Azure のエキスパートに連絡することができます、 [MSDN Azure とスタック オーバーフロー フォーラム](http://azure.microsoft.com/support/forums/)します。 または、Azure サポート インシデントを送信できます。 移動して、 [Azure サポート サイト](http://azure.microsoft.com/support/options/) のサポートにお問い合わせ] をクリックします。 Azure のサポートを使用する方法の詳細については、読み取り、 [Microsoft Azure サポートに関する FAQ](http://azure.microsoft.com/support/faq/)します。

