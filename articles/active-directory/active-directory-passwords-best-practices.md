<properties 
    pageTitle="ベスト プラクティス: Azure AD Password Management | Microsoft Azure" 
    description="Azure Active Directory での Password Management のデプロイと使用のベスト プラクティス、サンプル エンド ユーザー マニュアル、およびトレーニング ガイドです。" 
    services="active-directory" 
    documentationCenter="" 
    authors="asteen" 
    manager="kbrint" 
    editor="billmath"/>

<tags 
    ms.service="active-directory" 
    ms.workload="identity" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="11/16/2015" 
    ms.author="asteen"/>


# Password Management のデプロイとユーザー トレーニング

パスワードのリセットを有効にした後、次に実行する必要がある手順は、組織内のユーザーにサービスを使用してもらうことです。 そのためには、ユーザーがサービスを使用できるように適切に構成されていることを確認すると共に、ユーザーが自分のパスワードを問題なく管理するために必要なトレーニングを受けてもらう必要があります。 この記事では、次の概念を説明します。

* [* * ユーザーのパスワード管理 * に設定する方法](#how-to-get-users-configured-for-password-reset)
  * [パスワードのリセット用に構成されたアカウントは、します。](#what-makes-an-account-configured)
  * [認証データを設定する方法](#ways-to-populate-authentication-data)
* [* * パスワードのリセット、組織 * * をロールアウトする最善の方法](#what-is-the-best-way-to-roll-out-password-reset-for-users)
  * [電子メール ベースの展開 + サンプル メール通信](#email-based-rollout)
  * [ユーザー用の独自のカスタム パスワード管理ポータルを作成します。](#creating-your-own-password-portal)
  * [強制登録を使用して、強制的にユーザーのサインイン時に登録する方法](#using-enforced-registration)
  * [ユーザー アカウントの認証データをアップロードする方法](#uploading-data-yourself)
* [* * サンプル ユーザーとサポート トレーニング資料 (準備中)**](#sample-training-materials)

## ユーザーにパスワード リセットを構成してもらう方法

このセクションでは、組織内のすべてのユーザーが、自分のパスワードを忘れた場合にセルフ サービス パスワード リセットを効果的に使用できるようにするさまざまなメソッドについて説明します。

### アカウントが構成された状態とは

ユーザーは、パスワード リセットを使用する前に、次の条件を**すべて**満たす必要があります。

1.  ディレクトリでパスワードのリセットが有効であること。 読み取りによるパスワード リセットを有効にする方法を学習 [ユーザーによる Azure AD パスワードのリセットを有効にする](active-directory-passwords-getting-started.md#enable-users-to-reset-their-azure-ad-passwords) または [ユーザーによるリセットまたは AD パスワードの変更を有効にする](active-directory-passwords-getting-started.md#enable-users-to-reset-or-change-their-ad-passwords)
2.  ユーザーがライセンスを取得していること。
 - クラウド ユーザーの場合は、**有料の Office 365 ライセンス**を持っているか、**AAD Basic** または **AAD Premium** ライセンスが割り当てられていること。
 - オンプレミスのユーザー (フェデレーション ユーザーまたはハッシュ同期ユーザー) の場合は、**AAD Premium ライセンスが割り当てられていること**。
3.  ユーザーが現在のパスワード リセット ポリシーに従って、**認証データの最小セットを定義していること**。
 - 認証データは、ディレクトリ内の対応するフィールドに適切な形式のデータが含まれている場合に定義されたとみなされます。
 - 認証データの最小セットは、ワン ゲート ポリシーが構成される場合は有効な認証オプションの**少なくとも 1 つ**が、ツー ゲート ポリシーが構成される場合は有効な認証オプションの**少なくとも 2 つ**が構成されたときに定義されます。
4.  かどうか、ユーザーがアカウントを使用して、内部設置型、 [パスワード ライトバック](active-directory-passwords-getting-started.md#enable-users-to-reset-or-change-their-ad-passwords) 有効化し、有効にする必要があります

### 認証データを設定する方法

組織内のユーザーがパスワードのリセットに使用するデータを指定する方法はいくつかあります。

- ユーザーを編集、 [Azure 管理ポータル](https://manage.windowsazure.com) または [Office 365 管理ポータル](https://portal.microsoftonline.com)
- Azure AD Sync を使用して、ユーザー プロパティをオンプレミスの Active Directory ドメインから Azure AD に同期する
- Windows PowerShell を使用して、ユーザーのプロパティを編集する [次の手順に従って](active-directory-passwords-learn-more.md#how-to-access-password-reset-data-for-your-users)します。
- で登録ポータルで自分のデータを登録できるように [http://aka.ms/ssprsetup](http://aka.ms/ssprsetup)
- 設定して、Azure AD のアカウントにサインインしたときにパスワード リセットの登録を必要と、  [* * ユーザー サインインの際に登録する必要がありますか。 * *](active-directory-passwords-customize.md#require-users-to-register-when-signing-in) 構成オプションを **[はい]**します。

システムでパスワード リセットを機能させるためにユーザーが登録を行う必要はありません。 たとえば、ローカル ディレクトリに既存の携帯電話または会社の電話番号がある場合は、Azure AD に同期することで、パスワード リセットで自動的に使用できます。

参照することも複数の [パスワードのリセットによるデータの使用方法](active-directory-passwords-learn-more.md#what-data-is-used-by-password-reset) と [PowerShell を使用した個別の認証のフィールドを挿入する方法](active-directory-passwords-learn-more.md#how-to-access-password-reset-data-for-your-users)します。

## ユーザーにパスワード リセットを展開する最善の方法

パスワード リセットの一般的な展開手順を次に示します。

1.  ことで、ディレクトリでパスワード リセットを有効にする、 **構成** ] タブで、 [Azure 管理ポータル](https://manage.windowsazure.com) を選択すると、 **[はい]** の **パスワード リセットの有効なユーザー** オプション。
2.  先にするにはパスワードのリセットを提供する各ユーザーに適切なライセンスを割り当てるに移動して、 **ライセンス** ] タブで、 [Azure 管理ポータル](https://manage.windowsazure.com)します。
3.  必要に応じて、**[パスワード リセットへのアクセスの制限]** オプションを **[はい]** に設定し、パスワード リセットを有効にするセキュリティ グループを選択することで、パスワード リセットをユーザー グループに制限して、この機能を時間をかけて展開します (これらのユーザーにはライセンスが割り当てられている必要があります)。
4.  ユーザーがアクセス パネルまたは dirsync、PowerShell、自分でこれらのユーザーの適切な認証データをアップロードして強制登録を有効にすると、パスワード リセットに送信するか、電子メールを登録する手順の説明を使用するように指示する、または [Azure 管理ポータル](https://manage.windowsazure.com)します。 詳細は後述します。
5.  時間の経過と共にユーザーの [レポート] タブに移動し、表示して登録することを確認、 [* * パスワード リセット登録アクティビティ * *](active-directory-passwords-get-insights.md#view-password-reset-registration-activity) レポートします。
6.  十分な数のユーザーが登録されると、監視レポート] タブに移動し、表示によるパスワード リセットを使用して、 [* * パスワード リセット アクティビティ * *](active-directory-passwords-get-insights.md#view-password-reset-activity) レポートします。

組織内のユーザーにパスワード リセットの登録とパスワード リセットの使用を実行できることを通知する方法はいくつかあります。 これらを次に説明します。

### 電子メール ベースの展開

パスワード リセットの登録とパスワード リセットの使用についてユーザーに通知する最も簡単な方法は、手順の説明を記載した電子メールを送信することです。 この方法で使用できるテンプレートを次に示します。 色やロゴを自由に置き換えて、要件に合わせてカスタマイズしてください。

  ![][001]

実行できます [こちらの電子メール テンプレートをダウンロード](http://1drv.ms/1xWFtQM)します。

### 独自のパスワードのポータルを作成する

パスワード管理機能をデプロイする多くの顧客にとって有効な 1 つの手法は、1 つの場所でパスワードに関係するすべてのことを管理するのに使用できる単一の「パスワード ポータル」を作成することです。

多くの最大のお客様は、Azure AD のパスワードへのリンクを https://passwords.contoso.com リセット ポータルとパスワード リセット登録ポータル、パスワードの変更ページなどのルート DNS エントリを作成するを選択します。 この方法では、ユーザーがサービスの使用を開始するまでに少し時間がある場合にアクセス可能な覚えやすい URL を 1 つ、送信する任意の電子メールまたは広告に含めることができます。

ここでは、最新の応答性の高い UI 設計パラダイムを使用する単純なページを作成してあります。これは、すべてのブラウザーおよびモバイル デバイスで動作します。

  ![][007]

実行できます [こちらの web サイト テンプレートをダウンロード](https://github.com/kenhoff/password-reset-page)します。 組織のニーズに合わせてロゴおよび色をカスタマイズすることをお勧めします。

### 強制登録の使用

ユーザー自身にパスワード リセットの登録を行う場合は、強制することできますもに、アクセス パネルにサインインした際に登録する [http://myapps.microsoft.com](http://myapps.microsoft.com)します。 このオプションは、ディレクトリの **[構成]** タブで **[ユーザーが初めてアクセス パネルにサインインするときに登録を要求しますか?]** オプションを有効にすることで、有効にできます。

必要に応じて、**[ユーザーによる連絡先データの確認が必要になるまでの日数]** オプションを 0 以外の値に変更することで、構成可能な期間の後でユーザーに再登録を求めるかどうかを定義することもできます。 参照してください [ユーザーのパスワード管理の動作のカスタマイズ](active-directory-passwords-customize.md#password-management-behavior) の詳細。

  ![][002]

このオプションを有効にした後、ユーザーがアクセス パネルにサインインすると、管理者がユーザーの連絡先情報を確認することを求めていることを通知するポップアップが表示されます。 ユーザーが自分のアカウントにアクセスできなくなっている場合は、ポップアップからパスワードをリセットできます。

  ![][003]

クリックすると **今すぐ確認する** する、 **パスワード リセット登録ポータル** で [http://aka.ms/ssprsetup](http://aka.ms/ssprsetup) し、登録する必要があります。 この方法による登録は、**[キャンセル]** ボタンをクリックするかウィンドウを閉じることでキャンセルできますが、ユーザーが登録を行っていない場合は、サインインするたびに登録を求められます。

  ![][004]

### 管理者によるデータのアップロード

管理者が認証データをアップロードする場合、パスワードをリセットする前にユーザーがパスワード リセットの登録を行う必要はありません。 ユーザーは、定義されたパスワード リセット ポリシーに従って認証データをアカウントに定義している限り、自分のパスワードをリセットできます。

AAD Connect または Windows PowerShell を使用して設定できるプロパティについては、次を参照してください。 [パスワードのリセットで使用されるデータ](active-directory-passwords-learn-more.md#what-data-is-used-by-password-reset)します。

使用して認証データをアップロードする、 [Azure 管理ポータル](https://manage.windowsazure.com) 次の手順で。

1.  内のディレクトリに移動し、 **Active Directory 拡張機能** で、 [Azure 管理ポータル](https://manage.windowsazure.com)します。
2.  **[ユーザー]** タブをクリックします。
3.  対象のユーザーを一覧から選択します。
4.  最初のタブに、パスワードのリセットを有効にするためのプロパティとして使用できる **[連絡用メール アドレス]** が表示されます。

    ![][005]

5.  **[勤務先の情報]** タブをクリックします。
6.  ページに、**[会社電話]**、**[携帯電話]**、**[認証用電話]**、および **[認証用メール]** が表示されます。 これらのプロパティを設定して、ユーザーがパスワードをリセットできるようにすることも可能です。

    ![][006]

参照してください [パスワードのリセットで使用されるデータ](active-directory-passwords-learn-more.md#what-data-is-used-by-password-reset) にこれらのプロパティの使用方法を参照してください。

参照してください [パスワードにアクセスする方法は、PowerShell から、ユーザーにとってのデータをリセット](active-directory-passwords-learn-more.md#how-to-access-password-reset-data-for-your-users) を読み取るし、PowerShell では、このデータを設定する方法を確認します。

## サンプル トレーニング資料

パスワード リセットのデプロイと使用を IT 組織とユーザーに短時間で浸透させるサンプル トレーニング資料を準備しています。 しばらくお待ちください。


<br/>
<br/>
<br/>

## パスワードのリセットに関するドキュメントへのリンク

Azure AD のパスワードのリセットに関するすべてのドキュメント ページへのリンクを以下に示します。

* [* * リセットするには、独自のパスワード * *](active-directory-passwords-update-your-own-password.md) -リセットまたはシステムのユーザーとして、自分のパスワードを変更する方法について説明
* [* * 方法には * *](active-directory-passwords-how-it-works.md) -サービスとの 6 つの異なるコンポーネントについて学習それぞれの機能
* [* * 概要 * *](active-directory-passwords-getting-started.md) -ユーザーをリセットしたり、クラウドまたはオンプレミスのパスワード変更を許可する方法について
* [* * * * カスタマイズ](active-directory-passwords-customize.md) -、外観と、組織のニーズに合わせてサービスの動作をカスタマイズする方法について
* [* * Get * * insights](active-directory-passwords-get-insights.md) -統合レポート機能について
* [* * * * FAQ](active-directory-passwords-faq.md) -よく寄せられる質問に対する回答を得る
* [* * * * トラブルシューティング](active-directory-passwords-troubleshoot.md) -サービスに問題を迅速にトラブルシューティングする方法について説明
* [* * より * * について](active-directory-passwords-learn-more.md) - 掘り下げますサービスの機能の技術的な詳細




[001]: ./media/active-directory-passwords-best-practices/001.jpg "Image_001.jpg"
[002]: ./media/active-directory-passwords-best-practices/002.jpg "Image_002.jpg"
[003]: ./media/active-directory-passwords-best-practices/003.jpg "Image_003.jpg"
[004]: ./media/active-directory-passwords-best-practices/004.jpg "Image_004.jpg"
[005]: ./media/active-directory-passwords-best-practices/005.jpg "Image_005.jpg"
[006]: ./media/active-directory-passwords-best-practices/006.jpg "Image_006.jpg"
[007]: ./media/active-directory-passwords-best-practices/007.jpg "Image_007.jpg"

