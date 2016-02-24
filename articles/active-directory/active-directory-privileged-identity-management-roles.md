<properties
   pageTitle="Azure Privileged Identity Management: ロール"
   description="Azure Privileged Identity Management 拡張機能で特権 ID に使用されるロールについて説明します。"
   services="active-directory"
   documentationCenter=""
   authors="IHenkel"
   manager="stevenpo"
   editor=""/>

<tags
   ms.service="na"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/21/2015"
   ms.author="inhenk"/>

# Azure Privileged Identity Management: ロール

<!-- **PLACEHOLDER: Need description of how this works. Azure PIM uses roles from MSODS objects.**-->

## Azure Active Directory、Office 365、その他のソースからのロール

Azure PIM は、既定の管理者ロールとして次のロールを使用します。

- グローバル管理者
- 課金管理者
- サービス管理者
- ユーザー管理者
- パスワード管理者

Office 365 からの役割に関する詳細については、Exchange Online、Sharepoint Online および Skype for Business ここをクリックします[。Office 365 の管理ロールの割り当てください。](https://support.office.com/en-us/article/Assigning-admin-roles-in-Office-365-eac4d046-1afd-4f1a-85fc-8219c79e1504?ui=en-US&rs=en-US&ad=US)

<!--**PLACEHOLDER: The above article may not be the one we want since PIM gets roles from places other that Office 365**-->


<!-- ## The PIM Security Administrator Role **PLACEHOLDER: Need description of the Security Administrator role.**-->

## ユーザーのロールとログイン
> [AZURE.NOTE]Azure PIM にログインできるユーザーのためには、Azure のライセンスが必要です。

## Azure AD でのユーザーへのライセンスの割り当て

> [AZURE.NOTE] ライセンス オプションのみが表示されますライセンスが実際にこのサブスクリプションの終了します。

1. グローバル管理者アカウントまたは共同管理者アカウントで、[http://manage.windowsazure.com] (http://manage.windowsazure.com) にログインします。
2. をクリックして **すべてのアイテム** のメイン メニュー。
3. ライセンスが関連付けられていて使用するディレクトリを選択します。
4. をクリックして **ライセンス**します。 利用可能なライセンスの一覧が表示されます。
5. 配布するライセンスが含まれているライセンス プランをクリックします。
6. クリックして **ユーザーの割り当て**します。
7. ライセンスを割り当てるユーザーを選択します。
8. クリックして、 **割り当てる** ] ボタンをクリックします。  ユーザーは Azure にログインできるようになります。

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
##次のステップ
[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

