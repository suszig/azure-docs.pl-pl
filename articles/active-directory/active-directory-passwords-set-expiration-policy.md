<properties
    pageTitle="Azure Active Directory でパスワード有効期限ポリシーを設定する | Microsoft Azure"
    description="個別または一括で Azure Active Directory のパスワードの有効期限ポリシーを確認する方法、およびパスワードの有効期限を変更する方法を説明します。"
    services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="msStevenPo"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="11/16/2015"
    ms.author="curtand"/>



# Azure Active Directory でパスワード有効期限ポリシーを設定する

> [AZURE.NOTE] このトピックは、ID およびディレクトリ サービスを Microsoft Azure Active Directory に依存する Microsoft Intune や Office 365 などのクラウド サービスのオンライン ヘルプ コンテンツです。

Microsoft クラウド サービスのグローバル管理者は、Windows PowerShell 用 Microsoft Azure Active Directory モジュールを使用して、ユーザーのパスワードの有効期限が切れないように設定できます。 また、Windows PowerShell コマンドレットを使用すると、期限が切れない構成を削除したり、期限が切れないように設定されているユーザー パスワードを確認したりすることもできます。
  > [AZURE.NOTE] 有効期限が切れないように構成できるのは、ディレクトリ同期によって同期されていないユーザー アカウントのパスワードだけです。 ディレクトリ同期の詳細については、[トピックの一覧を参照してください。 [ディレクトリ同期のロードマップ](https://msdn.microsoft.com/library/azure/hh967642.aspx)します。

Windows PowerShell コマンドレットを使用するには、最初に Windows PowerShell をインストールする必要があります。

## どの操作を行いますか。

- [パスワードの有効期限ポリシーを確認する方法](#how-to-check-expiration-policy-for-a-password)

- [パスワードの有効期限の設定します。](#set-a-password-to-expire)

- [パスワードを設定できるように、有効期限はありません。](#set-a-password-not-to-expire)

## パスワードの有効期限ポリシーを確認する方法

1.  会社の管理者の資格情報を使用して Windows PowerShell に接続します。

2.  次のいずれかを実行します。

    - 1 人のユーザーのパスワードが無期限に設定されているかどうかを表示するには、ユーザー プリンシパル名 (UPN) (aprilr@contoso.onmicrosoft.com など) を使用して、次のコマンドレットを実行するかをチェックする、ユーザーのユーザー ID: `Get-msoluser-UserPrincipalName < ユーザー ID > |選択 PasswordNeverExpires`

    - すべてのユーザーの [パスワードを無期限にする"設定を表示するには、次のコマンドレットを実行します `Get-msoluser |。選択 UserPrincipalName、PasswordNeverExpires`

## パスワードを期限付きに設定する

1.  会社の管理者の資格情報を使用して Windows PowerShell に接続します。

2.  次のいずれかを実行します。

    - 1 人のユーザーのパスワードを設定して、パスワードの有効期限が切れるように、ユーザー プリンシパル名 (UPN) を使用して、次のコマンドレットを実行しますまたは、ユーザーのユーザー ID: `Set-msoluser-UserPrincipalName < ユーザー ID > - PasswordNeverExpires \$false`。

    - 期限が切れるように、組織内のすべてのユーザーのパスワードを設定するには、次のコマンドレットを使用して: `Get-msoluser |Set-msoluser-PasswordNeverExpires \$false`

## パスワードを無期限に設定する

1. 会社の管理者の資格情報を使用して Windows PowerShell に接続します。

2.  次のいずれかを実行します。

    - 期限切れに 1 人のユーザーのパスワードを設定して、ユーザー プリンシパル名 (UPN) を使用して、次のコマンドレットを実行しますまたは、ユーザーのユーザー ID: `Set-msoluser-UserPrincipalName < ユーザー ID > - PasswordNeverExpires \$true`。

    - 期限切れに組織内のすべてのユーザーのパスワードを設定するには、次のコマンドレットを実行します `Get-msoluser |。Set-msoluser-PasswordNeverExpires \$true`





