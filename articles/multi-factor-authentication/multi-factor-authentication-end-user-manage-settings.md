<properties 
    pageTitle="Azure Multi-Factor Authentication での問題 | Microsoft Azure" 
    description="このドキュメントでは、Azure Multi-factor Authentication で問題が発生した場合の処置に関する情報をユーザーに提供します。" 
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
    ms.date="12/10/2015" 
    ms.author="billmath"/>

# Azure Multi-Factor Authentication での問題
>[AZURE.IMPORTANT]
>このページの改善にご協力ください。  このページで回答が見つからない場合、マイクロソフトが追加できるように詳しいフィードバックをご提供ください。

ここでは、ユーザーが遭遇することがある一般的な問題に対処するときに役立つ情報を紹介します。


- [関連付け ID のエラー](#correlation-id-errors)
- [電話を紛失した場合や電話が盗まれた場合](#i-have-lost-my-phone-or-it-was-stolen?)
- [電話番号を変更する必要がある場合](#i-want-to-change-my-phone-number)
- [電話を新しくしたので、電話番号を変更する必要がある場合](#i-have-a-new-phone-and-need-to-change-my-phone-number)
- [電話でコードを受け取ることができない場合](#i-am-not-receiving-a-code-on-my-phone)
- [アプリ パスワードが機能しない場合](#app-passwords-are-not-working)
- [古いデバイスから Azure Authenticator をクリーンアップして新しいデバイスに移行する場合](#how-do-i-clean-up-azure-authenticator-from-my-old-device-and-move-to-a-new-one)
- [問題に対する回答が見つからなかった場合](#i-didn't-find-an-answer-to-my-problem)

##関連付け ID のエラー
トラブルシューティングの手順を試みて、実行中に問題が発生する場合に質問を投稿することができます、 [Azure AD フォーラム](https://social.msdn.microsoft.com/forums/azure/home?forum=WindowsAzureAD), 、[Microsoft サポート技術情報 (KB) を検索](https://www.microsoft.com/en-us/Search/result.aspx?q=azure%20active%20directory%20connect&form=mssupport) または [サポートにお問い合わせ](https://support.microsoft.com/en-us) およびが迅速に問題を見ていきましょう。

サポートへのお問い合わせの際は、次の情報を含めることをお勧めします。

 - **エラーの一般的な説明** – どのような正確なエラー メッセージが表示されましたか。  エラー メッセージが表示されなかった場合は、気が付いた予期しない動作について詳しく説明してください。
 - **ページ** – どのようなページは、エラーを確認したときにいました (URL を含む)?
 - **ErrorCode** -を受け取っている特定のエラー コード。
 - **SessionId** -特定のセッション id が表示されます。
 - **関連付け ID** – エラー表示されたときに生成された相関 id コードは何でした。
 - **タイムスタンプ** – された正確な日付と時刻が、エラーを説明しました (タイム ゾーンを含む)?
 
![関連付け ID](./media/multi-factor-authentication-end-user-manage/correlation.png) 

 - **ユーザー ID** – エラー (例: user@contoso.com) を確認したユーザーの ID は何でしたか。
 - **ユーザーの情報** – フェデレーション ユーザー、パスワード ハッシュ同期、クラウドのみか。  そのユーザーには、Azure AD Premium、Enterprise Mobility、または Azure AD Basic のライセンスが割り当てられていましたか。  Office 365 を使用して、ユーザーか。 など

この情報を含めることで、迅速に問題を解決するのに役立ちます。

## 電話を紛失した場合や電話が盗まれた場合
電話機が紛失または盗難にあった場合は、管理者があることをお勧めのリセット、 [アプリ パスワード](multi-factor-authentication-manage-users-and-devices.md#delete-users-existing-app-passwords) オフ [記憶されたデバイス](multi-factor-authentication-manage-users-and-devices.md#restore-mfa-on-all-suspended-devices-for-a-user)します。

アカウントに再度サインインする方法は 2 種類あります。  1 つ目は、代替の認証用電話番号をセットアップしている場合、その番号を使用してアカウントにサインインし、セキュリティ設定を変更します。

2 つ目の認証用電話番号を指定していた場合は、その番号を使用してサインインします。 
![セットアップ](./media/multi-factor-authentication-end-user-manage/altphone.png)
スクリーン ショットで、上記 2 つの電話番号はセットアップされているに注意してください。  1 つ目は末尾が 67 の番号で、2 つ目は末尾が 30 の番号です。
  
予備の電話番号を使用してサインインすると、サインインに選択すると、通常であれば、し、単に **別の確認オプションを使用して**します。
![別の確認](./media/multi-factor-authentication-end-user-manage/differentverification.png)

それから 2 つ目の電話番号を選択します。  この場合は、選択は **電話 + X XXXXXXXX30**

![Alternate phone](./media/multi-factor-authentication-end-user-manage/altphone2.png)

>[AZURE.IMPORTANT]
>2 つ目の認証用電話番号を構成することは重要です。  1 つ目の電話番号の携帯電話にモバイル アプリがインストールされている場合が多いため、電話の紛失や盗難が起きた場合にアカウントに再度サインインできる唯一の手段は、2 つ目の電話番号を使用する方法になります。

管理者に問い合わせていて、そのチェック ボックスをオフにする必要がセカンダリ認証の電話番号を構成していない場合、次のように設定するサインイン、するように求められます [多要素認証のセットアップ](multi-factor-authentication-manage-users-and-devices.md#require-selected-users-to-provide-contact-methods-again) 再度します。

## 電話番号を変更する必要がある場合
多要素認証の使用方法に応じて、電話番号などの設定を変更できる場所がいくつかあります。   下の表からご使用の状況に最も近いものを選択します。

多要素認証の使用方法|説明
:------------- | :------------- | 
[Office 365 で使用する](#changing-your-settings-with-office-365)|  この場合には、Office 365 ポータルを使用して設定を変更します。
[わからない](#changing-your-settings-with-the-myapps-portal)|つまりにサインインする必要が [http://myapps.microsoft.com](http://myapps.microsoft.com) し、設定を変更します。
[Microsoft Azure で使用する](#changing-your-settings-with-microsoft-azure)| この場合には、Azure ポータルを使用して設定を変更します。


 
### Office 365 で設定を変更する


Office 365 で多要素認証を使用する場合は、Office 365 ポータルで、追加のセキュリティ確認の設定を管理できます。

#### Office 365 ポータルで設定を変更するには

1. ログオン、 [Office 365 ポータル](https://login.microsoftonline.com/)します。
2. 右上隅でウィジェットを選択し、Office 365 設定を選択します。
3. [追加のセキュリティ確認] をクリックします。
4. 右クリックというリンクを **アカウントのセキュリティに使用される電話番号を更新します。**
![O365](./media/multi-factor-authentication-end-user-manage/o365a.png)
5. 設定を変更できるページに移動します。
![O365](./media/multi-factor-authentication-end-user-manage/o365b.png)


### Myapps ポータルで設定を変更する

多要素認証を使用する方法がわからない場合、いつでも Myapps ポータルで設定を変更できます。

#### Myapps ポータルで設定を変更するには

1. サインインする [https://myapps.microsoft.com](https://myapps.microsoft.com)  
2. 上部にある [プロファイル] を選択します。
3. 追加のセキュリティ確認を選択します。
![Myapps](./media/multi-factor-authentication-end-user-manage/myapps1.png)
4. 設定を変更できるページに移動します。

![追加のセキュリティ確認](./media/multi-factor-authentication-end-user-manage-myapps/proofup.png)

### Microsoft Azure で設定を変更する

Azure で多要素認証を使用している場合は、Azure ポータルで設定を変更することになります。

#### Azure ポータルで追加のセキュリティ確認を設定する方法


1. Azure ポータルにログオンします。
2. Azure ポータルの上部に示されているユーザー名をクリックします。 これにより、ドロップ ダウン ボックスが表示されます。
3. ドロップダウン ボックスから、追加のセキュリティ確認を選択します。
![Azure](./media/multi-factor-authentication-end-user-manage/azure1.png)
4. 設定を変更できるページに移動します。
![追加のセキュリティ確認](./media/multi-factor-authentication-end-user-manage-azure/proofup.png)

##電話を新しくしたので、電話番号を変更する必要がある場合

新しい電話を用意し、mfa が使用する 1 つ目の連絡先番号を変更する必要がある場合、2 つの方法のいずれかでそれを実行できます。

最初の方法は、2 つ目の認証方法を使用することです。  2 つ目の認証用電話番号を指定していた場合は、その番号を使用してサインインします。 
![セットアップ](./media/multi-factor-authentication-end-user-manage/altphone.png)
スクリーン ショットで、上記 2 つの電話番号はセットアップされているに注意してください。  1 つ目は末尾が 67 の番号で、2 つ目は末尾が 30 の番号です。
  
予備の電話番号を使用してサインインすると、サインインに選択すると、通常であれば、し、単に **別の確認オプションを使用して**します。
![別の確認](./media/multi-factor-authentication-end-user-manage/differentverification.png)

それから 2 つ目の電話番号を選択します。  この場合は、選択は **電話 + X XXXXXXXX30**

![Alternate phone](./media/multi-factor-authentication-end-user-manage/altphone2.png)

>[AZURE.IMPORTANT]
>2 つ目の認証用電話番号を構成することは重要です。  1 つ目の電話番号の携帯電話にモバイル アプリがインストールされている場合が多いため、電話の紛失や盗難が起きた場合にアカウントに再度サインインできる唯一の手段は、2 つ目の電話番号を使用する方法になります。

2 つ目の方法は、管理者または代わりに mfa を設定した人に連絡することです。  これは、2 つ目の認証電話番号を構成していない場合にのみ必要となります。  ように求められますが、管理者または mfa をセットアップした担当者にお問い合わせに与えられ、次回サインインするための設定をオフにすることがある、それ以外の場合 [多要素認証のセットアップ](multi-factor-authentication-manage-users-and-devices.md#require-selected-users-to-provide-contact-methods-again) 再度します。

##電話でコードまたは呼び出しを受け取ることができない場合

最初に次のことを確認する必要があります。



- 電話の呼び出しを携帯電話で受けるように選択している場合、呼び出し信号が適切であることを確認します。  配信の速度や利用可能性は、場所やサービス プロバイダーによって異なります。
- 確認コードをテキスト メッセージで携帯電話に送信するように選択している場合は、ご利用のサービス プランと、デバイスがテキスト メッセージの配信をサポートしていることを確認します。 配信の速度や利用可能性は、場所やサービス プロバイダーによって異なります。 また、これらのコードの受信を試みているときに、十分な電波が届いていることを確認します。
- モバイル アプリを使って確認コードを受信するように選択している場合は、電波状態が良いことを確認します。  配信の速度や利用可能性は、場所とサービス プロバイダーによって異なる場合があることも忘れないでください。 

使用する場合は、スマート フォンがある場合は、お勧めします [Azure Authenticator アプリ](multi-factor-authentication-azure-authenticator)します。

モバイル アプリを使用してテキスト メッセージによる確認コードを選択して受信の間で切り替えることができます **別の確認オプションを使用して** ときにサインインします。 

![Different Verification](./media/multi-factor-authentication-end-user-manage/differentverification.png) 

これらのいずれかのサービスの配信方法が、他の方法よりも確実な場合があります。

複数の確認コードを受信した場合は、最新のコードだけが機能することに注意してください。 

予備の電話番号を構成している場合は、サインイン ページでプロンプトが表示されたらその電話番号を選択して再試行することをお勧めします。 持っていない別の方法が構成されている、管理者に連絡してとを依頼の設定をオフにするため、次回にサインイン、するように求められます [多要素認証のセットアップ](multi-factor-authentication-manage-users-and-devices.md#require-selected-users-to-provide-contact-methods-again) 再度します。

##アプリ パスワードが機能しない場合
最初に、アプリ パスワードが正しく入力されていることを確認します。  これがまだ動作しない場合に、サインインしてくださいと [新しいアプリ パスワードを作成する](multi-factor-authentication-end-user-app-passwords.md)です。  これでうまくいかない場合は、管理者に連絡してもらう [、既存のアプリ パスワードを削除](multi-factor-authentication-manage-users-and-devices.md#delete-users-existing-app-passwords) 新しいものを作成して、それを使用します。

##古いデバイスから Azure Authenticator をクリーンアップして新しいデバイスに移行する場合
デバイスからアプリをアンインストールしても、デバイスを再フラッシュしても、アクティブ化はバック エンドで削除されません。 説明する手順を使用する必要があります [を新しいデバイスに移動します。](multi-factor-authentication-azure-authenticator.md#how-to-move-to-the-new-azure-authenticator-app)します。

##問題に対する回答が見つからなかった場合
場合はこのページで、問題の解決が見つかりませんでしたに質問を投稿することができます、 [Azure AD フォーラム](https://social.msdn.microsoft.com/forums/azure/home?forum=WindowsAzureAD), 、[Microsoft サポート技術情報 (KB) 検索](https://www.microsoft.com/en-us/Search/result.aspx?q=azure%20active%20directory%20connect&form=mssupport) または [サポートにお問い合わせ](https://support.microsoft.com/en-us)します。

また、管理者または代わりに多要素認証を設定した人に問い合わせ、サポートを要請できます。

最後に、このページに関する詳しいフィードバックを残してください。それにより、このページを更新し、情報を追加して継続的に改善できます。
