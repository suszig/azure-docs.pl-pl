<properties
    pageTitle="Azure Active Directory Premium の概要"
    description="Azure Active Directory Premium エディションにサインアップする方法について説明します。"
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="stevenpo" 
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/30/2015"
    ms.author="markvi"/>

# Azure Active Directory Premium の概要

Azure Active Directory には、Free、Basic、Premium という 3 つのエディションがあります。 Free エディションは、Azure または Office 365 のサブスクリプションに含まれます。 Basic および Premium エディションから入手、 [Microsoft Enterprise Agreement](https://www.microsoft.com/en-us/licensing/licensing-programs/enterprise.aspx) または [オープン ボリューム ライセンス](https://www.microsoft.com/en-us/licensing/licensing-programs/open-license.aspx) プログラムです。 Azure および Office 365 のサブスクリプション会員も、Active Directory Premium をオンラインで購入できます。 [こちらでサインイン](https://portal.office.com/Commerce/Catalog.aspx) 購入します。

> [AZURE.NOTE]
Azure Active Directory のワールドワイド インスタンスを使用している中国のお客様は、Azure Active Directory Premium および Basic エディションを使用できます。 中国の 21Vianet が運営する Microsoft Azure サービスでは、Azure Active Directory Premium および Basic エディションは現在サポートされていません。 詳細については、下記までお問い合わせください、 [Azure Active Directory フォーラム](http://feedback.azure.com/forums/169401-azure-active-directory)します。

Azure Active Directory Premium に記載されても、 **Enterprise Mobility Suite**します。 Enterprise Mobility Suite は、Microsoft Intune、Azure Rights Management、および Active Directory Premium のサービスを 1 つのライセンス プランでまとめて使用できるコスト効果の高い方法です。 詳細については、次を参照してください。、 [Enterprise Mobility Suite](https://www.microsoft.com/en-us/server-cloud/enterprise-mobility/overview.aspx) web サイトです。

Azure Active Directory Premium の機能は、次の手順に従ってすぐに使い始めることができます。 同じ手順が、Azure Active Directory Basic エディションにも適用されます。

## 手順 1: Active Directory Premium にサインアップする

サインアップするを参照してください、 [ボリューム ライセンス](http://www.microsoft.com/en-us/licensing/how-to-buy/how-to-buy.aspx) web サイトです。

## 手順 2: ライセンス プランをアクティブ化する

Microsoft のエンタープライズ ボリューム ライセンス プログラムでライセンス プランを初めて購入する場合は、Azure Active Directory のディレクトリ内からライセンスの割り当てを開始する前にまず、ライセンス プランをアクティブ化する必要があります。 そのためには、初めてライセンス プランを購入した後で受け取る確認電子メール (以下を参照) の [サインイン] または [サインアップ] リンクをクリックする必要があります。 同じディレクトリでの 2 回目以降の購入では、ライセンスは同じディレクトリで自動的にアクティブ化されます。

![][1]

既存のテナントの場合は選択し、 **サイン イン** リンクをクリックして既存の管理者アカウントでサインインします。 ライセンスをアクティブ化する必要があるディレクトリからグローバル管理者の資格情報でサインインすることが重要です。

ライセンス プランで使用する新しい Azure Active Directory テナントを作成するかどうかは、選択する際、 **サインアップ** リンクをクリックすると、次の画面をします。

![][2]

電子メールから開始したサインアップ プロセスまたはサインインプロセスのいずれかが完了すると、テナントのライセンス プランがアクティブ化されていることを確認する次の画面が表示されます。

![][3]

## 手順 3: Azure Active Directory のアクセスをアクティブ化する

ディレクトリのライセンスがプロビジョニングされると、Azure Active Directory Premium または Enterprise Mobility Suite のライセンスと機能の管理を開始できることを確認する歓迎電子メール (下図参照) を受け取ります。 以前に Microsoft Azure を使用している場合 (詳細については、後の手順 4 を参照してください)、新しいライセンスを割り当てる http://manage.windowsazure.com に進むことができます。 Microsoft Azure に慣れていない場合、電子メールの [サインイン] リンクを選択するに移動したり、 [Azure Active Directory アクティブ化ページへのアクセス](https://account.windowsazure.com/signup?offer=MS-AZR-0110P) 一連の Azure 管理ポータルを介してディレクトリにアクセスするための手順を実行します。

![][4]

サインインが正常に済んだ後は、携帯電話の番号を入力して検証を受け、第 2 要素の認証画面 (下図) を完了する必要があります。 その後ことができますを選択して Azure Active Directory へのアクセスをアクティブ化する **サインアップ**します。

![][5]

アクティブ化は、次のように、数分後にかかる場合があります、アクセスがアクティブな茶色のバーが消え、右上隅の [ポータル] リンクをクリックするかに移動することができます、 [Azure 管理ポータル](http://manage.windowsazure.com)します。

![][6]

この例では、Azure へのアクセスは Azure Active Directory に制限されています。

![][7]

以前の使用により、Azure に既にアクセスできる状態である場合があります。さらに、追加の Azure サブスクリプションをアクティブ化することによって、Azure Active Directory へのアクセスを完全な Azure アクセスにアップグレードできます。 このような場合は、管理ポータルに次のような他の機能も表示されます。

![][8]

前に示した歓迎電子メールを受け取る前に Azure Active Directory へのアクセスをアクティブ化しようとすると、次のエラー メッセージが表示される場合があります。 電子メールを受け取った後、少ししてからもう一度やり直してください。

![][9]

サブスクリプションの新しい管理者も、このリンクを介して管理ポータルへのアクセスをアクティブ化できます。

## 手順 4: ユーザー アカウントにライセンスを割り当てる

購入したプランの使用を開始する前に、組織内のユーザーが Premium で提供される豊富な機能を使用できるように、ユーザー アカウントにライセンスを手動で割り当てる必要があります。 以下の手順を使用して、ユーザーにライセンスを割り当て、ユーザーが Azure Active Directory Premium の機能を使用できるようにします。

ライセンスをユーザーに割り当てるには:

1. カスタマイズするディレクトリのグローバル管理者として、管理ポータルにサインインします。
2. クリックして **Active Directory**, 、ライセンスを割り当てるディレクトリを選択します。
3. 選択、 **ライセンス** ] タブで [ **Active Directory Premium** または **Enterprise Mobility Suite**, 、] をクリックし、 **割り当てる**します。

    ![][10]

4. ダイアログ ボックスで、ライセンスを割り当てるユーザーを選択し、チェック マーク アイコンをクリックして変更を保存します。

    ![][11]

## ライセンスの制限

一部のライセンス プランは、他のライセンス プランのサブセットまたはスーパーセットです。 ほとんどの場合、ユーザーに既に割り当てられているライセンス プランをユーザーに割り当てることはできません。 スーパーセットであるライセンス プランを割り当てる場合は、先にサブセットのライセンス プランを削除する必要があります。

## ライセンスの要件

ユーザーにライセンスを割り当てるときは、次に示すように、アカウントのプロパティで主な利用場所を指定できます。 利用場所を指定しないと、テナントの場所が自動的にユーザーに割り当てられます。

![][12]

Microsoft クラウド サービスで利用できるサービスと機能は、国または地域によって異なります。 Voice over Internet Protocol (VoIP) などのサービスは、ある国または地域では利用できても、他の国または地域では利用できないことがあります。 サービスに含まれる機能は、法的な理由から特定の国や地域では制限される場合があります。 サービスまたは機能を利用するときの制限の有無を確認するには、サービスのライセンス制限サイトで国または地域を検索してください。

## 参照トピック

- [サインイン ページとアクセス パネル ページに対する会社のブランドの追加](active-directory-add-company-branding.md)
- [アクセスおよび使用状況レポートを表示します。](active-directory-view-access-usage-reports.md)

<!--Image references-->
[1]: ./media/active-directory-get-started-premium/MOLSEmail.png
[2]: ./media/active-directory-get-started-premium/MOLSAccountProfile.png
[3]: ./media/active-directory-get-started-premium/MOLSThankYou.png
[4]: ./media/active-directory-get-started-premium/AADEmail.png
[5]: ./media/active-directory-get-started-premium/SignUppage.png
[6]: ./media/active-directory-get-started-premium/Subscriptionspage.png
[7]: ./media/active-directory-get-started-premium/Premiuminportal.png
[8]: ./media/active-directory-get-started-premium/Premiuminportal_large.png
[9]: ./media/active-directory-get-started-premium/Signuppage_oops.png
[10]: ./media/active-directory-get-started-premium/contosolicenseplan.png
[11]: ./media/active-directory-get-started-premium/Assignlicensespicker.png
[12]: ./media/active-directory-get-started-premium/Usagelocation.png

