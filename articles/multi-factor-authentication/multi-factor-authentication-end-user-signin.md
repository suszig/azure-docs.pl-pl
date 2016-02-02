<properties 
    pageTitle="Azure Multi-Factor Authentication を使用した Azure MFA サインイン エクスペリエンス" 
    description="このページでは、Azure MFA で利用可能な各種サインイン方法を確認できる参照先情報を示します。" 
    services="multi-factor-authentication" 
    documentationCenter="" 
    authors="billmath" 
    manager="stevenpo" 
    editor="curtland"/>

<tags 
    ms.service="multi-factor-authentication" 
    ms.workload="identity" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="11/19/2015" 
    ms.author="billmath"/>


# Azure Multi-Factor Authentication を使用したサインイン エクスペリエンス

> [AZURE.NOTE]  このページで提供する次のドキュメントでは、一般的なサインイン エクスペリエンスを示します。 ヘルプを参照してくださいサインインを [Azure 多要素認証で問題が発生](multi-factor-authentication-end-user-manage-settings.md)



## サインインのときにどんなことが生じますか。

サインインの方法、および多要素認証の使用法によって、エクスペリエンスは異なります。 このセクションでは、サインインするときに想定される事柄について記します。 お客さまの操作内容に最も近いものを選択してください。


 操作内容| 説明
:------------- | :------------- |
 [携帯また職場の電話を使用してサインイン](#signing-in-with-mobile-or-office-phone)| 携帯電話または会社電話を使用してサインインする際の操作について説明します。
 [通知を使用してモバイル アプリによってサインインします。](#signing-in-with-the-mobile-app-using-notification)| 通知を使用してモバイル アプリでサインインする際の操作について説明します。
 [確認コードを使用したモバイル アプリを使用してサインイン](#signing-in-with-the-mobile-app-using-verification-code)| 確認コードを使用してモバイル アプリでサインインする際の操作について説明します。
 [別の方法を使用してサインイン](#signing-in-with-an-alternate-method)| 別の方法を使用する場合の操作について説明します。

## 携帯電話または会社電話を使用してサインインする

多要素認証で携帯電話または会社電話を使用した場合のエクスペリエンスについて以下に記します。

### 会社の電話または携帯電話への呼び出しを使用してサインインする方法

- Office 365 などのアプリケーションまたはサービスにユーザー名とパスワードを使用してサインインします。
- Microsoft からの呼び出しがあります。

![Microsoft が呼び出す](./media/multi-factor-authentication-end-user-signin-phone/call.png)

- 電話に出て、# キーを押します。

![リダイレクト先](./media/multi-factor-authentication-end-user-signin-phone/phone.png)

- これ署名された必要があります。</li>

## 通知を使用してモバイル アプリによってサインインする

多要素認証でモバイル アプリを使用した場合、通知が送信されたときのエクスペリエンスについて以下に記します。

### モバイル アプリに送信された通知を使用してサインインする方法

- Office 365 などのアプリケーションまたはサービスにユーザー名とパスワードを使用してサインインします。
- Microsoft から通知が送信されます

![Microsoft が通知を送信する](./media/multi-factor-authentication-end-user-signin-app-notify/notify.png)


- 電話に出て、確認キーを押します。

![確認](./media/multi-factor-authentication-end-user-signin-app-notify/phone.png)


- これでサインインできます。


## 確認コードを使用してモバイル アプリでサインインする

多要素認証でモバイル アプリを使用した場合、確認コードを使用するときのエクスペリエンスについて以下に記します。

### モバイル アプリで確認コードを使用してサインインする方法

- Office 365 などのアプリケーションまたはサービスにユーザー名とパスワードを使用してサインインします。
- Microsoft によって確認コードを求めるプロンプトが表示されます。

![検証コードを入力する](./media/multi-factor-authentication-end-user-signin-app-verify/verify.png)

- お客様の電話で Azure Authenticatior アプリを開き、サインインのためのボックスにコードを入力します。

![コードを取得する](./media/multi-factor-authentication-end-user-signin-app-verify/phone.png)

- これでサインインできます。


## 別の方法を使用してサインインする

次のセクションでは、主要な方法が使用できないときに、別の方法でサインインする方法を示します。

### 別の方法でサインインする方法

- Office 365 などのアプリケーションまたはサービスにユーザー名とパスワードを使用してサインインします。
- [別の確認オプションを使用する] を選択します。 各種オプションが選択項目として示されます。 オプションの数は、セットアップ済みの数によって異なります。

![別の方法を使用する](./media/multi-factor-authentication-end-user-signin-alt/alt.png)

- 代替方法を選択し、サインインします。






