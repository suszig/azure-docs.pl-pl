<properties
    pageTitle="Azure Active Directory のパスワード ポリシーと制限 | Microsoft Azure"
    description="Azure Active Directory のパスワードに適用されるポリシー (使用できる文字、長さ、有効期限など) について説明します"
  services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="stevenpo"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="11/03/2015"
    ms.author="curtand"/>



# Azure Active Directory のパスワード ポリシーと制限

この記事では、Azure AD ディレクトリに格納されているユーザー アカウントと関連付けられたパスワード ポリシーと複雑さの要件について説明します。

## すべてのユーザー アカウントに適用される UserPrincipalName ポリシー

Azure AD 認証システムにサインインする必要があるすべてのユーザー アカウントは、一意のユーザー プリンシパル名 (UPN) 属性値がそのアカウントに関連付けられている必要があります。 次の表にアウトライン、ポリシーをオンプレミス Active Directory 提供ユーザー アカウントに適用 (クラウドに同期)、およびクラウド専用のユーザー アカウントです。

| プロパティ| UserPrincipalName の要件|
|   ----------------------- |   ----------------------- |
| 使用できる文字| <ul> <li>A – Z</li> <li>a -z </li><li>0 – 9</li> <li> . - \_ ! \# ^ \~</li></ul>|
| 使用できない文字| <ul> <li>@</li> <li>ピリオド文字を含めることはできません"です ' の直前に、' @' 記号。</li></ul>|
| 長さの制限| <ul> <li>全体の長さは 113 文字以内で</li><li>64 文字の前に、' @' 記号</li><li>48 文字の後、' @' 記号</li></ul>

## クラウド ユーザー アカウントにのみ適用されるパスワード ポリシー

次の表では、作成され、Azure AD で管理されるユーザー アカウントに適用できる利用可能なパスワード ポリシー設定について説明します。

| プロパティ| 必要条件|
|   ----------------------- |   ----------------------- |
| 使用できる文字| <ul><li>A – Z</li><li>a -z </li><li>0 – 9</li> <li>@ # $ % ^ & * - _ ! + = [ ] { } &#124; \ : ‘ , . ? / ` ~ “ ( ) ;</li></ul>|
| 使用できない文字| <ul><li>Unicode 文字</li><li>スペース</li><li>スペース</li><li> **強力なパスワードのみ**: ドットの文字を含めることはできません '.' の直前に、' @' 記号</li></ul>|
| パスワードの制限| <ul><li>8 文字の最小および最大 16 文字</li><li>**強力なパスワードのみ**: 次の 4 内の 3 列が必要です:<ul><li>小文字</li><li>大文字</li><li>数字 (0 ~ 9)</li><li>記号 (前記のパスワード制限を参照してください)</li></ul></li></ul>|
| パスワードの有効期間| <ul><li>既定値: **90** 日 </li><li>値は、Azure Active Directory Module for Windows PowerShell から Set-msolpasswordpolicy コマンドレットを使用して設定できます。</li></ul>|
| パスワードの期限切れの通知| <ul><li>既定値: **14** 日 (パスワード期限切れ前) に、</li><li>値は Set-msolpasswordpolicy コマンドレットを使用して設定できます。</li></ul>|
| パスワードの期限切れ| <ul><li>既定値: **false** 日 (パスワードの有効期限が有効になっていることを示します) </li><li>Set-msoluser コマンドレットを使用して個々 のユーザー アカウントの値を構成することができます。 </li></ul>|
| パスワードの履歴| 前回のパスワードは再使用できません。|
| パスワード履歴の期間| 無期限|
| アカウントのロックアウト| <ul><li>10 回失敗ログオンの試行 (正しくないパスワード) が、ユーザーがログオンの一部として CAPTCHA ダイアログを解決する必要があります。</li><li>さらに 10 失敗したログオンの試行 (正しくないパスワード) が、正しい解決 CAPTCHA ダイアログ ボックスのユーザーがロックアウトされる期間中にします。さらに正しくないパスワードを急激に増加、ロックアウト期間が発生します。</li></ul>|


## 次のステップ

* [任意の場所からパスワードを管理します。](active-directory-passwords.md)
* [パスワード管理のしくみ](active-directory-passwords-how-it-works.md)
* [パスワード管理の概要概要](active-directory-passwords-getting-started.md)
* [パスワード管理をカスタマイズします。](active-directory-passwords-customize.md)
* [パスワード管理のベスト プラクティス](active-directory-passwords-best-practices.md)
* [パスワード管理レポートとオペレーション インサイトを取得する方法](active-directory-passwords-get-insights.md)
* [パスワード管理に関する FAQ](active-directory-passwords-faq.md)
* [パスワード管理をトラブルシューティングします。](active-directory-passwords-troubleshoot.md)
* [詳細については](active-directory-passwords-learn-more.md)





