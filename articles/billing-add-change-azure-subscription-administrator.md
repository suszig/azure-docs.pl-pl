<properties
    pageTitle="追加または Azure 共同管理者、サービス管理者アカウントの管理者を変更する方法 |Microsoft Azure"
    description="Azure 共同管理者、サービス管理者、アカウント管理者を追加または変更する方法について説明します"
    services="billing"
    documentationCenter=""
    authors="genlin"
    manager="jarrettr"
    editor="meerak"
    tags="billing"
    />

<tags
    ms.service="billing"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="11/11/2015"
    ms.author="genli"/>

# Azure 共同管理者、サービス管理者、アカウント管理者を追加または変更する方法
## 管理者ロール

Microsoft Azure には、3 種類の管理者ロールがあります。

| 管理ロール   | 制限  | 説明
| ------------- | ------------- |---------------|
|アカウント管理者  | 1 Azure アカウントに 1 人  |Azure サブスクリプションをサインアップまたは購入し、アカウント センターにアクセスし、多様な管理タスクを実行する権限を持っている個人。 管理タスクには、サブスクリプションの作成、サブスクリプションの取り消し、サブスクリプションの料金の変更、サービス管理者の変更などがあります。
| サービス管理者 | 1 Azure サブスクリプションに 1 人  |サブスクリプションに割り当てられたアカウントで、サブスクリプションの Microsoft Azure 管理ポータルにアクセスする権限を持っている個人。 既定で、新しいサブスクリプションのアカウント管理者はサービス管理者でもあります。|
|共同管理者|1 サブスクリプションに (サービス管理者に加えて) 200 人|サービス管理者と同じアクセス権を持っていますが、サブスクリプションと Azure ディレクトリとの関連付けを変更することはできない個人。|

## サブスクリプションの共同管理者を追加する
1. サインイン、 [Azure 管理ポータル](https://manage.windowsazure.com/)します。
2. ナビゲーション ウィンドウで [ **設定**> **管理者**> **追加**します。 </br>![addcodmin](./media/billing-add-change-azure-subscription-administrator/addcoadmin.png)
3. 共同管理者として追加し、共同管理者にアクセスするサブスクリプションを選択し人の電子メール アドレスを入力します。</br> ![addcoadmin2](./media/billing-add-change-azure-subscription-administrator/addcoadmin2.png)</br>

次の電子メール アドレスを共同管理者として追加できます。

* **Microsoft アカウント** (旧 Windows Live ID) </br>
 すべてのコンシューマー向け Microsoft 製品へのサインインし、Outlook (Hotmail)、Skype (MSN)、OneDrive、Windows Phone、Xbox LIVE などのサービスをクラウドに Microsoft アカウントを使用することができます。
* **組織のアカウント**</br>
 組織のアカウントは、Azure Active Directory で作成されるアカウントです。 組織のアカウントのアドレスには、次のようになります。
    @ ユーザー<your domain>。 onmicrosoft.com

**注**

 * Microsoft アカウントでログインしている場合は、共同管理者として他の Microsoft アカウントのみ追加できます。 これは、組織ではないアカウントが特定のアカウント (例: janedoe@contoso.com) が有効なアカウントである場合を検出しないように、セキュリティの考慮事項です。
 * 組織アカウントを使用してログインした場合、組織内の他の組織アカウントを共同管理者として追加することができます。 たとえば、abby@contoso.com では、サービス管理者または共同管理者として bob@contoso.com を追加することができますが、john@notcontoso.com を追加することはできません。 組織アカウントでログインしているユーザーは、Microsoft アカウント ユーザーをサービス管理者または共同管理者として引き続き追加できます。
 * 組織アカウントで Azure にログインできるようになると、サービス管理者と共同管理者アカウントの要件は次のように変わります。

| ログイン方法| Microsoft アカウントを共同管理者またはサービス管理者として追加できる  |同じ組織内の組織アカウントを共同管理者またはサービス管理者として追加できる |異なる組織の組織アカウントを共同管理者またはサービス管理者として追加できる
| ------------- | ------------- |---------------|---------------|
|Microsoft アカウント |はい|なし|いいえ|
|組織アカウント|はい|はい|いいえ|

## サブスクリプションのサービス管理者を変更する
サブスクリプションのサービス管理者を変更できるのは、アカウント管理者のみです。

1. ログオン [アカウント管理ポータル](https://account.windowsazure.com/subscriptions) アカウントの管理者を使用しています。
2. 変更するサブスクリプションを選択します。
3. 右側にある、次のようにクリックします。 **サブスクリプションの編集** 詳細です。 </br>
![editsub](./media/billing-add-change-azure-subscription-administrator/editsub.png)

4.  **サービス管理者** ボックスで、新しいサービス管理者の電子メール アドレスを入力します。 ![changeSA](./media/billing-add-change-azure-subscription-administrator/changeSA.png)

## アカウント管理者を変更します。

Azure アカウントの所有権を譲渡する別のアカウントに、次を参照してください。 [Azure サブスクリプションを転送する](../billing-subscription-transfer.md)です。


