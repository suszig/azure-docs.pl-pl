<properties 
    pageTitle="Azure AD でのパスワード リセット | Microsoft Azure"
    description="パスワードのリセット、変更、パスワード管理のレポート、ローカルのオンプレミスの Active Directory へのライトバックなど、Azure AD のパスワード管理機能の説明。" 
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



# ユーザーおよび管理者の Azure AD のパスワード リセット

  >[AZURE.IMPORTANT] Azure または O365 のパスワードをリセットをしたいとお考えですか。 必要な場合は、次のようにしてください。 [このセクションに進んで](#users-how-to-manage-your-own-password)します。

セルフ サービスの実現は、コストを削減し労力を節約する手段として、世界中の IT 部門が長年目指してきた大きな目標です。 実際、市場はクラウドまたはオンプレミスから、オンプレミスのグループや、パスワード、ユーザー プロファイルを管理できるようにする製品であふれています。 Azure AD は、現在利用可能な機能の中でも、最も使いやすく強力なセルフ サービス機能を提供することで、これらの製品とは一線を画しています。

**Azure AD パスワード管理**は、定義するセキュリティ ポリシーに準拠したままで、ユーザーが任意のデバイスで、時間と場所に関係なくパスワードを管理できる一連の機能です。

## ユーザー: 自分のパスワードを管理する方法

職場のリソースへのアクセスに Office 365 または Microsoft アカウントを使用している (管理者ではない) ユーザーは、以下のリンクをクリックして、パスワードでよく発生する問題を解決する方法を学習してください。

| トピック| |
| --------- | --------- |
| パスワードのリセット登録を行う方法| [パスワードのリセットを登録する方法](active-directory-passwords-update-your-own-password.md#how-to-register-for-password-reset)|
| O365 の自分のパスワードを変更する方法| [Office 365 でパスワードを変更する方法](active-directory-passwords-update-your-own-password.md#how-to-change-your-password-from-o365)|
| Myapps.microsoft.com の自分のパスワードを変更する方法| [アクセス パネルからパスワードを変更する方法](active-directory-passwords-update-your-own-password.md#how-to-change-your-password-from-the-access-panel)|
| 忘れたパスワードをリセットする方法| [パスワードをリセットする方法](active-directory-passwords-update-your-own-password.md#how-to-reset-your-password)|
| サインインができない自分のアカウントをロック解除する方法| [内部設置型アカウントのロックを解除する方法](active-directory-passwords-update-your-own-password.md#how-to-unlock-your-account)|
| パスワードのリセットの失敗のトラブルシューティングを支援する方法| [一般的な問題とその解決方法](active-directory-passwords-update-your-own-password.md#common-problems-and-their-solutions)|

## 管理者: Azure AD のパスワード リセットの概要について学習する

Azure AD のパスワード リセットを行いたい、またはそれについてさらに学習したい管理者は、以下のリンクの関心のある項目をお読みください。

| トピック| |
| --------- | --------- |
| サポートされるシナリオ| [Azure AD のパスワードのリセットで使用できるとは](#what-is-possible-with-azure-ad-password-reset)|
| 使用する理由| [Azure AD のパスワードのリセットを使用する理由](#why-use-azure-ad-password-reset)|
| 価格と可用性| [価格と可用性](#pricing-and-availability)|
| パスワード リセットを有効にする| [ユーザーのパスワードのリセットを有効にします。](#enable-password-reset-for-your-users)|
| 動作をカスタマイズする| [パスワード リセットの動作をカスタマイズします。](#customize-password-reset-behavior)|
| ユーザーに展開する| [パスワードのリセットを使用するユーザーを構成します。](#configure-your-users-to-use-password-reset)|
| レポートを表示する| [ビューのパスワード リセット アクティビティの統合レポート](#view-password-reset-activity-with-integrated-reports)|
| ユーザーのパスワードのリセット| [ユーザーのパスワードを管理します。](#manage-your-users-passwords)|
| 自分の組織のパスワード ポリシーを設定する| [パスワード ポリシーを設定](#set-password-policies)|
| 問題をトラブルシューティングする| [問題をトラブルシューティングします。](#troubleshoot-a-problem)|
| FAQ| [読み取り、よく寄せられる質問](#read-a-faq)|
| 技術的な詳細| [技術的な詳細を理解します。](#understand-the-technical-details)|
| 新たにリリースされた機能| [最近のサービスの更新](#recent-service-updates)|
| その他のドキュメントへのリンク| [パスワードへのリンクのマニュアルをリセットします。](#links-to-password-reset-documentation)|

### Azure AD のパスワード リセットでできること

ここでは、Azure AD のパスワード管理機能を使用して実行できることをいくつか示します。

- **セルフ サービスによるパスワードの変更**では、エンド ユーザーまたは管理者は、管理者またはヘルプ デスクにサポートを依頼することなく、有効期限が切れた、または有効期限が切れていないパスワードを変更できます。
- **セルフ サービスによるパスワードのリセット**では、エンド ユーザーまたは管理者は、管理者またはヘルプ デスクにサポートを依頼することなく、パスワードを自動的にリセットできます。 セルフ サービスのパスワード リセットには、Azure AD Premium または Basic が必要です。 詳細については、「Azure Active Directory のエディション」をご覧ください。
- **管理者によるパスワードのリセット** 内からエンドユーザーまたは別の管理者のパスワードをリセットすることができます、 [Azure 管理ポータル](https://manage.windowsazure.com)します。
- **パスワード管理アクティビティ レポート**では、組織内で発生したパスワードのリセットおよび登録アクティビティの詳細が管理者に提供されます。
- **パスワード ライトバック**では、クラウドからオンプレミスのパスワードを管理できるので、フェデレーション ユーザーまたはパスワード同期済みユーザーは、またはこれらのユーザーに代わって、上記のシナリオをすべて実行できます。 パスワード ライトバックには Azure AD Premium が必要です。 詳細については、「Azure AD Premium の概要」を参照してください。

### Azure AD のパスワード リセットを使用する理由

ここでは、Azure AD のパスワード管理機能を使用すべき理由をいくつか示します。

- **コストを削減する** -サポート部門によるパスワードのリセットは、通常、組織の IT 支出の 20% を占めます。
- **ユーザー エクスペリエンスを向上させる** -ユーザーは、パスワードを忘れるたびにヘルプデスクに電話して、時間を費やしたくはありません。
- **ヘルプデスクの負荷を低減する** -パスワード管理は、ほとんどの組織のヘルプデスクにおいて、それひとつに最も負担のかかる仕事です。
- **モビリティを高める** -ユーザーが任意の場所からパスワードをリセットできます。

### 価格と可用性

Azure AD のパスワード リセットは所持しているサブスクリプションに応じて、3 つのレベルで利用できます。

- **Azure AD Free** - クラウドのみの管理者が、自分のパスワードをリセットできます
- **Azure AD Basic または O365 のすべての有料サブスクリプション** - クラウドのみのユーザーおよびクラウドのみの管理者が、自分のパスワードをリセットできます
- **Azure AD Premium** - すべてのユーザーまたは管理者は、クラウドのみを含むフェデレーション、またはパスワード同期ユーザーは、自分のパスワードをリセットできます (を必要と [パスワード ライトバックを有効にする](active-directory-passwords-getting-started.md#enable-users-to-reset-or-change-their-ad-passwords))

Azure AD Premium または基本料金の詳細については、次を参照してください。、 [Active Directory の料金詳細](https://azure.microsoft.com/pricing/details/active-directory/) ページです。

## ユーザーのパスワード リセットの有効化

| トピック| |
| --------- | --------- |
| クラウド ユーザーのパスワードをリセットする方法| [ユーザーにクラウドの Azure Active Directory パスワードのリセットを有効にします。](active-directory-passwords-getting-started.md#enable-users-to-reset-their-azure-ad-passwords)|
| オンプレミスのユーザーのパスワード リセットおよび変更を有効にする方法| [ユーザーによるリセットまたは内部設置型 Active Directory パスワードの変更を有効にします。](active-directory-passwords-getting-started.md#enable-users-to-reset-or-change-their-ad-passwords)|
| パスワードのリセットを一連の特定のユーザーに適用する方法| [特定のユーザーにパスワード リセットを制限します。](active-directory-passwords-customize.md#restrict-access-to-password-reset)|
| クラウドのパスワードのリセットをテストする方法| [ユーザーとして、Azure AD のパスワードをリセットします。](active-directory-passwords-getting-started.md#step-3-reset-your-azure-ad-password-as-a-user)|
| オンプレミスのパスワードのリセットをテストする方法| [リセット、社内ユーザーとして AD パスワード](active-directory-passwords-getting-started.md#step-5-reset-your-ad-password-as-a-user)|
| 後でパスワード リセットを無効化する方法| [パスワードのリセットが有効なユーザーの設定:](active-directory-passwords-customize.md#users-enabled-for-password-reset)|


## パスワードのリセット動作のカスタマイズ

| トピック| |
| --------- | --------- |
| サポートされる認証方法を変更する方法| [ユーザーに使用できる設定: 認証の方法](active-directory-passwords-customize.md#authentication-methods-available-to-users)|
| 必要な認証方法の数を変更する方法| [必要な認証方法の数を設定します。](active-directory-passwords-customize.md#number-of-authentication-methods-required)|
| カスタムの秘密の質問を設定する方法| [カスタム セキュリティの質問を設定します。](active-directory-passwords-customize.md#custom-security-questions)|
| 既定のローカライズされた秘密の質問を設定する方法| [ナレッジ ベースのセキュリティの質問を設定します。](active-directory-passwords-customize.md#knowledge-based-security-questions)|
| 必要な秘密の質問の数を変更する方法| [登録後、またはリセットのセキュリティの質問の数を設定します。](active-directory-passwords-customize.md#number-of-questions-required-to-register)|
| ユーザーが管理者に連絡する方法をカスタマイズする方法| [「管理者に連絡」リンクをカスタマイズする設定。](active-directory-passwords-customize.md#customize-the-contact-your-administrator-link)|
| パスワードをリセットせずに AD アカウントのロックを解除することをユーザーに許可する方法| [パスワードをリセットすることがなく自分の AD アカウントのロックを解除できるようにする設定。](active-directory-passwords-customize.md#allow-users-to-unlock-accounts-without-resetting-their-password)|
| ユーザーのパスワード リセット通知を有効にする方法| [自分のパスワードがリセットされたときにユーザーに通知の設定。](active-directory-passwords-customize.md#notify-users-and-admins-when-their-own-password-has-been-reset)|
| 管理者のパスワード リセット通知を有効にする方法| [設定: 管理者が自分のパスワードをリセットすると、その他の管理者に通知します。](active-directory-passwords-customize.md#notify-admins-when-other-admins-reset-their-own-passwords)|
| パスワード リセットの外観をカスタマイズする方法| [会社名、ブランド、およびロゴの設定。 ](active-directory-passwords-customize.md#password-managment-look-and-feel)|


## パスワードのリセットを使用できるようにするためのユーザーの構成

| トピック| |
| --------- | --------- |
| アカウントがパスワード リセット用に構成されているか確認する方法| [パスワードのリセット用に構成されたアカウントを作成方法](active-directory-passwords-best-practices.md#what-makes-an-account-configured)|
| パスワード リセットをユーザーに構成する方法| [ユーザーのパスワード リセット認証データを追加する方法](active-directory-passwords-best-practices.md#ways-to-populate-authentication-data)|
| ユーザーのデータを手動でアップロードする方法| [パスワードをアップロードするデータをリセットする自分で](active-directory-passwords-best-practices.md#uploading-data-yourself)|
| PowerShell を使用してユーザーのデータを読み取ったり設定したりする方法| [パスワードにアクセスする方法、ユーザーのデータをリセットします。](active-directory-passwords-learn-more.md#how-to-access-password-reset-data-for-your-users)|
| オンプレミスでパスワード リセット データを同期する方法| [パスワードのリセットで使用されるデータ](active-directory-passwords-learn-more.md#what-data-is-used-by-password-reset)|
| ユーザーにパスワードのリセットを登録し実行してもらうための電子メール キャンペーンの使用方法| [パスワードのリセットの電子メール ベースの展開](active-directory-passwords-best-practices.md#email-based-rollout)|
| サインイン時にユーザーに登録を強制する方法| [パスワードのリセットの登録ベースの展開の強制](active-directory-passwords-customize.md#require-users-to-register-when-signing-in)|
| 登録内容の定期的な再確認をユーザーに強制する方法| [設定: 数日前に、ユーザーは、認証データを再確認する必要があります。](active-directory-passwords-customize.md#number-of-days-before-users-must-confirm-their-contact-data)|
| パスワード リセットをエンドユーザーに連絡する際のベスト プラクティス| [ユーザーが使用するための独自のパスワード ポータルを作成します。](active-directory-passwords-best-practices.md#creating-your-own-password-portal)|


## パスワード リセット アクティビティの統合レポートでの表示

| トピック| |
| --------- | --------- |
| パスワード リセット レポートを確認できる場所へのアクセス方法| [パスワード管理レポートの概要](active-directory-passwords-get-insights.md#overview-of-password-management-reports)|
| 組織でのユーザーのパスワード リセットの使用の確認方法| [ビューのパスワード リセット アクティビティ](active-directory-passwords-get-insights.md#view-password-reset-activity)|
| 登録ユーザー数、および登録対象を確認できる場所へのアクセス方法| [ビューのパスワード リセット登録アクティビティ](active-directory-passwords-get-insights.md#view-password-reset-registration-activity)|
| API を使用したパスワード リセット レポートの取得方法| [レポートの API にアクセスする azure ad アプリケーションを作成します。](active-directory-reporting-api-getting-started.md#creating-an-azure-ad-application-to-access-the-api)|
| API で入手できるパスワード リセット レポート情報の種類| [レポートの API で使用できるパスワード リセットおよび登録イベント](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-reports-and-events-preview#SsprActivityEvent)|


## ユーザーのパスワードの管理

| トピック| |
| --------- | --------- |
| O365 管理ポータルからユーザーのパスワードをリセットする方法| [Office 365 のユーザーのパスワードをリセットします。](https://support.office.com/article/Reset-a-user-s-password-7A5D073B-7FAE-4AA5-8F96-9ECD041ABA9C)|
| PowerShell を使用してユーザーのパスワードをリセットする方法| [Set-msoluserpassword とユーザーのパスワードをリセットします。](https://msdn.microsoft.com/library/azure/dn194140.aspx)|


## パスワード ポリシーの設定

| トピック| |
| --------- | --------- |
| Office 365 から組織のパスワードの有効期限ポリシーを設定する方法| [パスワードの有効期限ポリシーを設定します。](https://support.office.com/article/Set-a-user-s-password-expiration-policy-0f54736f-eb22-414c-8273-498a0918678f)|
| PowerShell で特定のユーザーのパスワードが期限切れにならないように設定する方法| [期限切れに PowerShell を使用して個々 のユーザーのパスワードを設定します。](https://support.office.com/article/Set-an-individual-user-s-password-to-never-expire-f493e3af-e1d8-4668-9211-230c245a0466)|
| ユーザーのパスワードが期限切れにならないように設定されていることを PowerShell で確認する方法| [PowerShell を使用して個々 のユーザーのパスワード有効期限の状態を確認します。](https://support.office.com/article/Set-an-individual-user-s-password-to-never-expire-f493e3af-e1d8-4668-9211-230c245a0466#__toc378845827)|


## 問題をトラブルシューティングする

| トピック| |
| --------- | --------- |
| ヘルプが必要である場合にサポートに提供する情報| [ヘルプが必要な場合に含める情報](active-directory-passwords-troubleshoot.md#information-to-include-when-you-need-help)|
| パスワードのリセットの問題を解決する方法| [パスワード リセット ポータルをトラブルシューティングします。](active-directory-passwords-troubleshoot.md#troubleshoot-the-password-reset-portal)|
| パスワード ライトバックの問題を解決する方法| [パスワード ライトバックをトラブルシューティングします。](active-directory-passwords-troubleshoot.md#troubleshoot-password-writeback)|
| パスワード ライトバックの接続の問題を解決する方法| [パスワード ライトバックの接続のトラブルシューティングを行う](active-directory-passwords-troubleshoot.md#troubleshoot-password-writeback-connectivity)|
| パスワードのリセット構成の問題を解決する方法| [Azure 管理ポータルでパスワード リセットの構成をトラブルシューティングします。](active-directory-passwords-troubleshoot.md#troubleshoot-password-reset-configuration-in-the-azure-management-portal)|
| パスワード リセット レポートの問題を解決する方法| [Azure 管理ポータルでのパスワード管理レポートをトラブルシューティングします。](active-directory-passwords-troubleshoot.md#troubleshoot-password-management-reports-in-the-azure-management-portal)|
| パスワード リセット登録の問題を解決する方法| [パスワード リセット登録ポータルをトラブルシューティングします。](active-directory-passwords-troubleshoot.md#troubleshoot-the-password-reset-registration-portal)|
| パスワード ライトバックのイベント ログのエラー コード| [パスワード ライトバックのイベント ログのエラー コード](active-directory-passwords-troubleshoot.md#password-writeback-event-log-error-codes)|


## FAQ を読む

| トピック| |
| --------- | --------- |
| パスワード リセット登録に関する FAQ を参照する| [パスワード リセット登録に関する FAQ](active-directory-passwords-faq.md#password-reset-registration)|
| パスワード リセットに関する FAQ を参照する| [パスワード リセットに関する FAQ](active-directory-passwords-faq.md#password-reset)|
| パスワード リセット レポートに関する FAQ を参照する| [パスワード管理レポートに関する FAQ](active-directory-passwords-faq.md#password-management-reports)|
| パスワード ライトバックに関する FAQ を参照する| [パスワード ライトバックに関する FAQ](active-directory-passwords-faq.md#password-writeback)|


## 技術的な詳細の理解

| トピック| |
| --------- | --------- |
| パスワード ライトバックについて学習する| [パスワード ライトバックの概要](active-directory-passwords-learn-more.md#password-writeback-overview)|
| パスワード ライトバックのしくみについて学習する| [パスワード ライトバックはどのように動作するのでしょうか。](active-directory-passwords-learn-more.md#how-password-writeback-works)|
| パスワード ライトバックがサポートするシナリオを学習する| [パスワード ライトバックでサポートされるシナリオ](active-directory-passwords-learn-more.md#scenarios-supported-for-password-writeback)|
| パスワード ライトバックのセキュリティ保護のしくみについて学習する| [パスワード ライトバックのセキュリティ モデル](active-directory-passwords-learn-more.md#password-writeback-security-model)|
| パスワード リセット ポータルのしくみを学習する| [パスワードが、ポータルのしくみをリセットする方法](active-directory-passwords-learn-more.md#how-does-the-password-reset-portal-work)|
| データの種類については、パスワードのリセットで使用されることを学習します。| [どのようなデータは、パスワードのリセットで使用されますか。](active-directory-passwords-learn-more.md#what-data-is-used-by-password-reset)|

## 最新のサービス更新内容

#### Office 365 アプリへのサインイン時にパスワード リセット登録を実施する - 2015 年 11 月

- 有効にした後に、  [登録を強制](active-directory-passwords-customize.md#require-users-to-register-when-signing-in) 機能は、ユーザーが職場または学校のアカウントでサインインする任意の場所から登録する必要があります。 これにより、多くの組織がパスワード リセットを使用開始する速度が大幅に上昇します。 この新しい機能により、大規模な組織がわずか 2 週間で利用開始したこともありました。

#### パスワードをリセットすることなく Active Directory アカウントのロックを解除する機能をサポート - 2015 年 11 月

- (リセットせずに) ロックのみを解除できることは、今日のヘルプデスクにとっては大きな推進力となります。 実際、多くの組織で、アカウントのロック解除にパスワードのリセット予算を最大 70% 使用しています。 Azure AD のパスワード リセットでは、この需要に対応するために、ユーザーが AD アカウントのロックを解除できる (パスワード リセットからは独立した) 機能を有効にできるようになりました。 ここで、オンにする方法を確認: [設定: ユーザーがパスワードをリセットすることがなく自分の AD アカウントのロックを解除できるように](active-directory-passwords-customize.md#allow-users-to-unlock-accounts-without-resetting-their-password)します。

#### 登録ページでの使いやすさの更新 - 2015 年 10 月

- ユーザーは既に登録されたデータがある場合、電子メールを送信したり電話をかけたりしなくても、[問題なし] をクリックするだけでデータを更新できるようになりました。

#### パスワード ライトバックの信頼性が向上 - 2015 年 9 月

- Azure AD Connect の 9 月リリースの時点で、パスワード ライトバック エージェントは接続と、より堅牢な追加のフェールオーバー機能とを積極的に再試行するようになります。

#### パスワード リセット レポート データを取得するための API - 2015 年 8 月

- パスワード リセット レポートの背後にあるデータを取得してから直接、 [Azure AD Reports and Events API](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprActivityEvent)します。

#### クラウド ドメインへの参加中に Azure AD のパスワード リセットをサポート - 2015 年 8 月

- クラウド ユーザーは、クラウド ドメインへの参加のためのオンボード作業中に Windows 10 サインイン画面から自身のパスワードをリセットできるようになりました。 この機能は Windows 10 のサインイン画面ではまだ公開されていません。

#### Azure およびフェデレーション アプリへのサインイン時にパスワード リセット登録を強制 - 2015 年 7 月

- Azure 管理ポータルと、フェデレーション シングル サインオン アプリケーションに対するサポートの強制登録中のサインイン myapps.microsoft.com にサインインしたときに、登録を適用するだけでなく

#### セキュリティに関する質問のローカライズ サポート - 2015 年 5 月

- パスワード リセットに対してセキュリティの質問を構成する場合に、完全な O365 言語セットでローカライズされた定義済みのセキュリティの質問を選択できるようになりました。

#### パスワード リセット中のアカウントのロック解除をサポート - 2015 年 6 月

- パスワード ライトバックを使用している場合、アカウントがロックされている状態でパスワードをリセットすると、Active Directory アカウントが自動的にロック解除されます。

#### SSPR 登録のブランド化 - 2015 年 4 月

- パスワードのリセットの登録ページは、会社のロゴでブランド化されるようになりました。

#### セキュリティの質問 - 2015 年 3 月

- セキュリティの質問を、一般に公開しました。

#### アカウントのロックの解除 - 2015 年 3 月

- パスワードのリセットが発生した場合に、ユーザーがロックを解除できるようになりました。

## 近日対応予定

現在作業中の優れた機能の一部を次に示します。

**サインイン時に登録済みデータを更新するようユーザーに喚起する機能** - 開発中

- 現在、myapps.microsoft.com へのアクセス時に登録済みデータを更新するようユーザーに喚起する機能をサポートしていますが、すべてのサインインでこれを行う機能を開発中です。

## パスワードのリセットに関するドキュメントへのリンク

Azure AD のパスワードのリセットに関するすべてのドキュメント ページへのリンクを以下に示します。

* [* * リセットするには、独自のパスワード * *](active-directory-passwords-update-your-own-password.md) -リセットまたはシステムのユーザーとして、自分のパスワードを変更する方法について説明
* [* * 方法には * *](active-directory-passwords-how-it-works.md) -サービスとの 6 つの異なるコンポーネントについて学習それぞれの機能
* [* * 概要 * *](active-directory-passwords-getting-started.md) -ユーザーをリセットしたり、クラウドまたはオンプレミスのパスワード変更を許可する方法について
* [* * * * カスタマイズ](active-directory-passwords-customize.md) -、外観と、組織のニーズに合わせてサービスの動作をカスタマイズする方法について
* [* * ベスト プラクティス * *](active-directory-passwords-best-practices.md) -を迅速に展開し、組織内のパスワードを効果的に管理する方法について説明
* [* * Get * * insights](active-directory-passwords-get-insights.md) -統合レポート機能について
* [* * * * FAQ](active-directory-passwords-faq.md) -よく寄せられる質問に対する回答を得る
* [* * * * トラブルシューティング](active-directory-passwords-troubleshoot.md) -サービスに問題を迅速にトラブルシューティングする方法について説明
* [* * より * * について](active-directory-passwords-learn-more.md) - 掘り下げますサービスの機能の技術的な詳細




