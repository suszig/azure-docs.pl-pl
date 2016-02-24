<properties 
    pageTitle="Azure Multi-Factor Authentication レポート" 
    description="ここでは、追加のセキュリティ確認を強制するなど、ユーザー設定を変更する方法について説明します。" 
    documentationCenter="" 
    services="multi-factor-authentication" 
    authors="billmath" 
    manager="stevenpo" 
    editor="curtand"/>

<tags 
    ms.service="multi-factor-authentication" 
    ms.workload="identity" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="11/19/2015" 
    ms.author="billmath"/>

# クラウドでの Azure Multi-factor Authentication によるユーザー設定の管理

管理者は、ユーザーとデバイスに関する次の設定を管理できます。  

- [選択したユーザーについて連絡方法の再指定を必須にする](#require-selected-users-to-provide-contact-methods-again)
- [ユーザーの既存のアプリ パスワードを削除する](#delete-users-existing-app-passwords)
- [Restore MFA on all suspended devices for a user (パブリック プレビュー)](#restore-mfa-on-all-suspended-devices-for-a-user)






これは、コンピューターまたはデバイスが紛失または盗難にあった場合や、ユーザーのアクセスを削除する必要がある場合に便利です。


## 選択したユーザーについて連絡方法の再指定を必須にする

この設定は、ユーザーのサインイン時に、もう一度登録プロセスを完了することを強制します。 ブラウザー以外のアプリは、ユーザーがアプリ パスワードを持っている場合に引き続き動作しますので注意してください。  ユーザー アプリ パスワードを削除するも選択して、 **選択したユーザーが生成した既存のすべてのアプリ パスワードを削除**します。

### 連絡方法をもう一度提供するようユーザーに要求する方法

<ol>
<li>Azure 管理ポータルにサインインします</li>
<li>。左側で、[Active Directory] をクリックします</li>
<li>。ディレクトリ、連絡方法の再指定を必須にユーザーのディレクトリをクリックします</li>
<li>。上部にある [ユーザー] をクリックします</li>
<li>。ページの下部にある、多要素認証の管理] をクリックして. これにより、Multi-Factor Authentication ページが開きます。
<li>管理するユーザーを見つけ、その名前の横にあるチェック ボックスをオンにします。 上部でビューを変更する必要があります</li>
<li>。これが表示されます、 **ユーザー設定の管理** 右にリンクします。 それをクリックして</li> 
<li>チェック ボックスをオンに **選択したユーザーについて連絡方法を再度指定する必要があります**します。</li>

![連絡方法を提供する](./media/multi-factor-authentication-manage-users-and-devices/reproofup.png)

<li>[保存] をクリックします。</li>
<li>[閉じる] をクリックします。</li>

## ユーザーの既存のアプリ パスワードを削除する

ユーザーが作成したすべてのアプリ パスワードを削除します。 これらのアプリ パスワードに関連付けられているブラウザー以外のアプリは、新しいアプリ パスワードが作成されるまで機能しなくなります。

### ユーザーの既存のアプリ パスワードを削除する方法

<ol>
<li>Azure 管理ポータルにサインインします</li>
<li>。左側で、[Active Directory] をクリックします</li>
<li>。ディレクトリをクリックしてアプリ パスワードのデータ型を削除するユーザーのディレクトリに</li>
<li>上部にある [ユーザー] をクリックします</li>
<li>。ページの下部にある、多要素認証の管理] をクリックして. これにより、Multi-Factor Authentication ページが開きます。
<li>管理するユーザーを見つけ、その名前の横にあるチェック ボックスをオンにします。 上部でビューを変更する必要があります</li>
<li>。これが表示されます、 **ユーザー設定の管理** 右にリンクします。 それをクリックして</li> 
<li>チェック ボックスをオンに **選択したユーザーが生成した既存のすべてのアプリ パスワードを削除**.</li>
![Delete app passwords](./media/multi-factor-authentication-manage-users-and-devices/deleteapppasswords.png)
<li>保存] をクリックします</li>
<li>。[閉じる] をクリックします。</li>





## ユーザーのすべての停止中のデバイスで MFA を復元する

管理者は、デバイスやブラウザーで Multi-Factor Authentication をリセットすることができます。 これを行うには、ユーザーのデバイスやブラウザーについて Multi-Factor Authentication を復元します。 このとき、ユーザーのすべてのデバイスやブラウザーから中断が解除されます。 

### ユーザーの中断されたすべてのデバイスで MFA を復元する方法

<ol>
<li>Azure 管理ポータルにサインインします</li>
<li>。左側で、[Active Directory] をクリックします</li>
<li>。Mfa を復元するユーザーのディレクトリにディレクトリをクリックします</li>
<li>。上部にある [ユーザー] をクリックします</li>
<li>。ページの下部にある、多要素認証の管理] をクリックして. これにより、Multi-Factor Authentication ページが開きます。
<li>管理するユーザーを見つけ、その名前の横にあるチェック ボックスをオンにします。 上部でビューを変更する必要があります</li>
<li>。これが表示されます、 **ユーザー設定の管理** 右にリンクします。 それをクリックして</li> 
<li>中断されたすべてのデバイスに Multi-factor Authentication を復元チェック マークを入れます</li>
![Delete app passwords](./media/multi-factor-authentication-manage-users-and-devices/rememberdevices.png)
<li>。保存] をクリックします</li>
<li>。[閉じる] をクリックします。</li>



