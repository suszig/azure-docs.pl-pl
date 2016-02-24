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
> [AZURE.NOTE] このトピックでは、Microsoft Azure Active Directory の id およびディレクトリ サービスを使用する Microsoft Intune や Office 365 など、クラウド サービスのオンライン ヘルプ コンテンツを提供します。

Microsoft クラウド サービスのグローバル管理者は、Windows PowerShell 用 Microsoft Azure Active Directory モジュールを使用して、ユーザーのパスワードの有効期限が切れないように設定できます。 また、Windows PowerShell コマンドレットを使用すると、期限が切れない構成を削除したり、期限が切れないように設定されているユーザー パスワードを確認したりすることもできます。

  > [AZURE.NOTE] 期限切れを防ぐディレクトリ同期によって同期されていないユーザー アカウントのパスワードのみを構成することができます。 ディレクトリ同期の詳細については、[トピックの一覧を参照してください。 [ディレクトリ同期のロードマップ](https://msdn.microsoft.com/library/azure/hh967642.aspx)します。

Windows PowerShell コマンドレットを使用するには、最初に Windows PowerShell をインストールする必要があります。

## どの操作を行いますか。

- [パスワードの有効期限ポリシーを確認する方法](#how-to-check-expiration-policy-for-a-password)

- [パスワードを期限付きに設定する](#set-a-password-to-expire)

- [パスワードを無期限に設定する](#set-a-password-not-to-expire)

## パスワードの有効期限ポリシーを確認する方法

1.  会社の管理者の資格情報を使用して Windows PowerShell に接続します。

2.  次のいずれかを実行します。

    - 1 人のユーザーのパスワードを設定を無期限にすると、ユーザー プリンシパル名 (UPN) (aprilr@contoso.onmicrosoft.com など) またはユーザーのユーザー ID を使用して、次のコマンドレットを実行するには、をチェックします。 `Get-MSOLUser -UserPrincipalName <user ID> | Select PasswordNeverExpires`

    - すべてのユーザーについて「パスワードを無期限にする」設定を表示するには、次のコマンドレットを実行します。 `Get-MSOLUser | Select UserPrincipalName, PasswordNeverExpires`

## パスワードを期限付きに設定する

1.  会社の管理者の資格情報を使用して Windows PowerShell に接続します。

2.  次のいずれかを実行します。

    - 特定のユーザーのパスワードを期限付きに設定するには、ユーザーのユーザー プリンシパル名 (UPN) またはユーザー ID を使用して、次のコマンドレットを実行します。 `Set-MsolUser -UserPrincipalName <user ID> -PasswordNeverExpires \$false`

    - 組織内のすべてのユーザーのパスワードを期限付きに設定するには、次のコマンドレットを使用します。 `Get-MSOLUser | Set-MsolUser -PasswordNeverExpires \$false`

## パスワードを無期限に設定する

1. 会社の管理者の資格情報を使用して Windows PowerShell に接続します。

2.  次のいずれかを実行します。

    - 特定のユーザーのパスワードを無期限に設定するには、ユーザーのユーザー プリンシパル名 (UPN) またはユーザー ID を使用して、次のコマンドレットを実行します。 `Set-MsolUser -UserPrincipalName <user ID> -PasswordNeverExpires \$true`

    - 組織内のすべてのユーザーのパスワードを無期限に設定するには、次のコマンドレットを実行します。 `Get-MSOLUser | Set-MsolUser -PasswordNeverExpires \$true`

