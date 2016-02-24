<properties 
    pageTitle="しくみ: Azure AD でのパスワード管理 | Microsoft Azure" 
    description="ユーザーによるパスワードの登録、リセット、および変更、そして管理者によるオンプレミスの Active Directory パスワードの構成、レポート作成、および管理を含む、Azure AD でのパスワード管理について学習します。" 
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

# パスワード管理のしくみ
Azure Active Directory でのパスワード管理は、以下に説明するいくつかの論理コンポーネントで構成されます。  コンポーネントの詳細については、各リンクをクリックしてください。

- [**パスワード管理の構成ポータル**](#password-management-configuration-portal) – 管理者は自分のディレクトリの構成] タブに移動して、「がテナント内のパスワードの管理方法のさまざまな側面を制御できます、 [Azure 管理ポータル](https://manage.windowsazure.com)します。
- [**ユーザー登録ポータル**](#user-registration-portal) – ユーザーはこの web ポータルからパスワード リセットを自ら登録できます。
- [**ユーザー パスワードのリセット ポータル**](#user-password-reset-portal) – ユーザーが管理者のパスワードのリセット ポリシーに従って、さまざまな課題の番号を使用して、自分のパスワードをリセットできます
- [**ユーザー パスワードの変更ポータル**](#user-password-change-portal) – ユーザーは、古いパスワードを入力し、この web ポータルを使用して新しいパスワードを選択していつでも、自分のパスワードを変更することができます
- [**パスワード管理レポート**](#password-management-reports) – 管理者は表示してで自分のディレクトリの [レポート] タブの [活動レポート] セクションに移動して、テナント内のパスワードのリセットと登録アクティビティの分析、 [Azure 管理ポータル](https://manage.windowsazure.com)
- [**Azure AD Connect のパスワード ライトバック コンポーネント**](#password-writeback-component-of-azure-ad-connect) – 管理者は必要に応じて有効、パスワードの書き戻し機能の管理を有効にする Azure AD Connect をインストールするフェデレーション ユーザーまたはパスワードが、クラウドからのユーザーのパスワードを同期するとき。

## パスワード管理の構成ポータル
特定のディレクトリを使用するためのパスワード管理ポリシーを構成することができます、 [Azure 管理ポータル](https://manage.windowsazure.com) に移動して、 **ユーザー パスワードのリセット ポリシー** セクションで、ディレクトリの **構成** ] タブをクリックします。  この構成ページでは、パスワードが組織内でどのように管理されるかを、以下をはじめとするさまざまな側面から制御できます。

- ディレクトリのユーザー全員に対してパスワードのリセットの有効と無効を切り替える
- パスワードをリセットする前にユーザーがクリアする必要があるチャレンジの数 (1 つまたは 2 つ) を設定する
- 組織内のユーザーに対して有効にするチャレンジを以下のなかから選択して設定する
 - 携帯電話 (テキストによる確認コードまたは音声通話)
 - 会社電話 (音声通話)
 - 代替電子メール (電子メールによる確認コード)
 - セキュリティの質問 (ナレッジ ベースの認証)
- セキュリティの質問の認証方法 (セキュリティの質問が有効な場合のみ表示される) を使用するために、登録する必要がある質問の数を設定する
- セキュリティの質問の認証方法 (セキュリティの質問が有効な場合のみ表示される) を使用するために、リセット時に指定する必要がある質問の数を設定する
- ユーザーがパスワード リセットに登録する際に使用する可能性がある事前に準備されたローカライズ済みのセキュリティに関する質問を使用する (セキュリティの質問が有効になっている場合にのみ表示される)
- ユーザーがパスワード リセットに登録する際に使用する可能性があるカスタムのセキュリティに関する質問を定義する (セキュリティの質問が有効になっている場合にのみ表示される)
- アプリケーションに移動するときにパスワード リセットの登録を必要とするアクセス パネル [http://myapps.microsoft.com](http://myapps.microsoft.com)します。
- 設定可能な日数が経過した後、以前登録したデータを再確認することをユーザーに求める (適用されている登録が有効な場合のみ表示される)
- パスワードのリセットに際して問題が発生した場合に表示される、カスタムのヘルプデスクの電子メール アドレスまたは URL を指定する
- パスワード ライトバック機能を有効化または無効化する (AAD Connect を使用してパスワード ライトバックがデプロイされている場合)
- パスワード ライトバック エージェントの状態を表示する (AAD Connect を使用してパスワード ライトバックがデプロイされている場合)
- 自分のパスワードをリセットしたときに、ユーザーへの電子メール通知を有効にすると (で見つかった、 **通知** のセクションで、 [Azure 管理ポータル](https://manage.windowsazure.com))
- その他の管理者が、自分のパスワードをリセットした場合は、管理者に電子メール通知を有効にする (で見つかった、 **通知** のセクションで、 [Azure 管理ポータル](https://manage.windowsazure.com)
- ブランドのユーザーのパスワード リセット ポータルとパスワードは、テナントのブランド化のカスタマイズ機能を使用して電子メールを組織のロゴおよび名前をリセット (で見つかった、 **ディレクトリのプロパティ** のセクションで、 [Azure 管理ポータル](https://manage.windowsazure.com)

組織内のパスワード管理の構成に関する詳細については、「 [概要: Azure AD パスワード管理](active-directory-passwords-getting-started.md)します。

##ユーザー登録ポータル
ユーザーがパスワードをリセットできるようにするには、パスワードのリセットに対して管理者が設定した数のチャレンジをクリアできるように、クラウドのユーザー アカウントを正しい認証データに更新する必要があります。  管理者は、Azure Web ポータルまたは Office Web ポータル、DirSync または Azure AD Connect、あるいは Windows PowerShell を使用して、ユーザーの代わりにこの認証情報を定義することもできます。

ただし、ユーザーが自分でデータを登録する方が好ましい場合は、ユーザーがアクセスして情報を登録できる Web ページも用意されています。  このページでユーザーは、それぞれの組織で有効になっているパスワード リセット ポリシーに従って認証情報を指定できます。  入力されたデータが確認されると、クラウドのユーザー アカウントに保存され、後にアカウントを復旧するときに使用されます。 登録ポータルの外観を次に示します。

  ![][001]

詳細については、次を参照してください。 [概要: Azure AD パスワード管理](active-directory-passwords-getting-started.md) と [ベスト プラクティス: Azure AD パスワード管理](active-directory-passwords-best-practices.md)します。 

##ユーザー パスワードのリセット ポータル
組織内のユーザーがサインインのための職場または学校のアカウントを使用する任意の web ページから自動的にパスワードをリセットできるセルフ_サービスのパスワードをリセットし、組織のセルフ サービス パスワード リセット ポリシーをセットアップ、ユーザーは、ディレクトリ内の適切な連絡先データであることを確認を有効にすると (たとえば [portal.microsoftonline.com](https://portal.microsoftonline.com))。 ページでユーザーが表示される、 **アカウントにアクセスできない?** リンクします。 

  ![][002]

このリンクをクリックすると、セルフサービスのパスワード リセット ポータルが起動します。

  ![][003]

ユーザーが、自分のパスワードをリセットする方法の詳細については、次を参照してください。 [概要: Azure AD パスワード管理](active-directory-passwords-getting-started.md)します。

##ユーザー パスワードの変更ポータル
ユーザーが自分のパスワードを変更したい場合は、パスワード変更ポータルを使用すればいつでも変更できます。  パスワード変更ポータルへは、アクセス パネルのプロファイル ページからアクセスできるほか、Office 365 アプリケーション内の [パスワードを変更する] リンクをクリックしてもアクセスできます。  パスワードの有効期限が切れた場合は、ユーザーがサインインすると自動的にパスワードの変更を求められます。 

  ![][004]

いずれの場合も、パスワード ライトバックが有効で、ユーザーがフェデレーションまたはパスワード同期であれば、これらの変更されたパスワードはオンプレミスの Active Directory にライトバックされます。 パスワード変更ポータルの外観を次に示します。 

  ![][005]

ユーザーが内部設置型 Active Directory パスワードを変更する方法の詳細については、次を参照してください。 [概要: Azure AD パスワード管理](active-directory-passwords-getting-started.md)します。

##パスワード管理レポート
移動して、 **レポート** ] タブをクリックし、検索、 **アクティビティ ログ** ] セクションで、次の 2 つのパスワード管理レポートが表示されます: **パスワード リセット アクティビティ** と **パスワード リセット登録アクティビティ**します。  これら 2 つのレポートを使用すると、組織内でパスワード リセットに登録しているユーザー、およびパスワード リセットを使用しているユーザーのビューを参照できます。 これらのレポートがどのようにここでは、 [Azure 管理ポータル](https://manage.windowsazure.com):

  ![][006]

詳細については、次を参照してください。 [Get Insights: Azure AD パスワード管理レポート](active-directory-passwords-get-insights.md)します。

##Azure AD Connect のパスワード ライトバック コンポーネント
組織内のユーザーのパスワードがオンプレミスの環境 (フェデレーションまたはパスワード同期のいずれか) から発生している場合、Azure AD Connect の最新バージョンをインストールすると、クラウドから直接パスワードを更新できます。  これは、ユーザーが AD のパスワードを忘れた場合や変更を希望する場合、Web から直接更新可能だということです。  次に、Azure AD Connect インストール ウィザードでパスワード ライトバックを選択できる場所を示します。

  ![][007]

Azure AD Connect の詳細については、次を参照してください。 [概要: Azure AD Connect](active-directory-aadconnect.md)します。 パスワードの書き戻しに関する詳細については、次を参照してください。 [概要: Azure AD パスワード管理](active-directory-passwords-getting-started.md)します。


<br/>
<br/>
<br/>

## パスワードのリセットに関するドキュメントへのリンク
Azure AD のパスワードのリセットに関するすべてのドキュメント ページへのリンクを以下に示します。 

* [**自分のパスワードをリセット**](active-directory-passwords-update-your-own-password.md) -リセットまたはシステムのユーザーとして、自分のパスワードを変更する方法について説明
* [**作業の開始**](active-directory-passwords-getting-started.md) -ユーザーをリセットしたり、クラウドまたはオンプレミスのパスワード変更を許可する方法について
* [**カスタマイズ**](active-directory-passwords-customize.md) -、外観と、組織のニーズに合わせてサービスの動作をカスタマイズする方法について
* [**ベスト プラクティス**](active-directory-passwords-best-practices.md) -を迅速に展開し、組織内のパスワードを効果的に管理する方法について説明
* [**情報を把握**](active-directory-passwords-get-insights.md) -統合レポート機能について
* [**よく寄せられる質問**](active-directory-passwords-faq.md) -よく寄せられる質問に対する回答を得る
* [**トラブルシューティング**](active-directory-passwords-troubleshoot.md) -サービスに問題を迅速にトラブルシューティングする方法について説明
* [**詳細については**](active-directory-passwords-learn-more.md) - 掘り下げますサービスの機能の技術的な詳細



[001]: ./media/active-directory-passwords-how-it-works/001.jpg "Image_001.jpg"
[002]: ./media/active-directory-passwords-how-it-works/002.jpg "Image_002.jpg"
[003]: ./media/active-directory-passwords-how-it-works/003.jpg "Image_003.jpg"
[004]: ./media/active-directory-passwords-how-it-works/004.jpg "Image_004.jpg"
[005]: ./media/active-directory-passwords-how-it-works/005.jpg "Image_005.jpg"
[006]: ./media/active-directory-passwords-how-it-works/006.jpg "Image_006.jpg"
[007]: ./media/active-directory-passwords-how-it-works/007.jpg "Image_007.jpg"

