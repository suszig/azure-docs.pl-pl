<properties
    pageTitle="チュートリアル: Azure Active Directory の職場での Facebook と統合 |Microsoft Azure"
    description="職場で Azure Active Directory と Facebook の間でのシングル サインオンを構成する方法について説明します。"
    services="active-directory"
    documentationCenter=""
    authors="asmalser-msft"
    manager="stevenpo"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="11/04/2015"
    ms.author="asmalser"/>



# チュートリアル: Azure Active Directory の職場での Facebook と統合

このチュートリアルでは、Azure Active Directory (Azure AD) を職場での Facebook を統合する方法を説明します。<br>職場を Azure AD と統合する Facebook を使うと、次の利点があります。

- 作業の Facebook へのアクセスを持っている Azure AD で制御できます。
- Facebook at Work へのアクセス権が付与されているユーザーには、アカウントを自動的にプロビジョニングできます
- 自動的に署名済みの作業 (シングル サインオン) での Facebook には Azure AD アカウントでユーザーを有効にすることができます。
- 1 つの場所でアカウントを管理できます

Azure AD と SaaS アプリの統合に関する詳細を確認するを参照して [アプリケーションへのアクセスと Azure Active Directory でのシングル サインオン](active-directory-appssoaccess-whatis.md)します。


## 前提条件

CS Stars と Azure AD の統合を構成するには、次のものが必要です。

- Azure AD サブスクリプション
- シングル サインオンを有効にした Facebook at Work

このチュートリアルの手順をテストするには、次の推奨事項に従ってください。

- 必要な場合を除き、運用環境は使用しないでください。
- Azure AD の評価環境を取得していない場合は、1 か月の試用版を入手できます [ここ](https://azure.microsoft.com/pricing/free-trial/)します。


## ギャラリーから Facebook at Work を追加する

Facebook の統合を構成すると、Azure AD に作業を管理対象 SaaS アプリの一覧に、ギャラリーからの職場で Facebook を追加する必要があります。

**ギャラリーから、職場で Facebook を追加するには、次の手順を実行します。**

1. **Microsoft Azure 管理ポータル**の左側のナビゲーション ウィンドウで、**[Active Directory]** をクリックします。 
<br><br>![Active Directory][1]<br>

2. **[ディレクトリ]** の一覧から、ディレクトリ統合を有効にするディレクトリを選択します。

3. アプリケーション ビューを開くには、ディレクトリ ビューでトップ メニューの **[アプリケーション]** をクリックします。
<br><br>![アプリケーション][2]<br>

4. ページの下部にある **[追加]** をクリックします。
<br><br>![アプリケーション][3]<br>

5. **[実行する内容]** ダイアログで、**[ギャラリーからアプリケーションを追加します]** をクリックします。
<br><br>![アプリケーション][4]<br>

6. 検索ボックスに「**Facebook at Work**」と入力します。

7. 結果ウィンドウで [ **職場での Facebook**, 、] をクリックし、 **完了** アプリケーションを追加します。


### Azure AD シングル サインオンの構成

このセクションでは、ユーザーに職場での Facebook のアカウントで SAML プロトコルに基づくフェデレーションを使用して、Azure Active Directory 認証を有効にする方法について説明します。

**職場での Facebook と Azure AD シングル サインオンを構成するには、次の手順を実行します。**

1.  Microsoft Azure 管理ポータルで Facebook at Work を追加したら、**[シングル サインオンの構成]** をクリックします。

2.  **[アプリケーション URL の構成]** 画面に、ユーザーが Facebook at Work にサインインする URL を入力します。 これは、Facebook の作業のテナント url 
(例: https://example.facebook.com/)。 完了すると、クリックして **次**します。

3.  別の web ブラウザーのウィンドウで、管理者として、Facebook の作業の会社サイトにログインします。

4. Facebook を id プロバイダーとして Azure AD を使用する作業で構成する次の URL での指示に従います: [https://developers.facebook.com/docs/facebook-at-work/authentication/cloud-providers](https://developers.facebook.com/docs/facebook-at-work/authentication/cloud-providers)

5.  完了したら、Microsoft Azure 管理ポータルが表示されているブラウザー ウィンドウに戻り、手順を完了したことを確認するチェックボックスをオンにして、**[次へ]** と **[完了]** をクリックします。


## ユーザーを Facebook at Work に自動的にプロビジョニングする

Azure AD は、割り当てられたユーザーのアカウントの詳細を Facebook at Work と自動的に同期する機能をサポートしています。 この自動同期機能によって、ユーザーが初めてサインインする前に、Facebook at Work がユーザーのアクセスを承認するために必要なデータを取得することができます。 また、Azure AD のアクセス権が取り消された場合、Facebook at Work からユーザーのプロビジョニングを解除することができます。

自動プロビジョニングを設定するには、Microsoft Azure 管理ポータル ウィンドウで **[アカウント プロビジョニングの構成]** をクリックします。

自動プロビジョニングを構成する方法に関する追加詳細については、「 [https://developers.facebook.com/docs/facebook-at-work/provisioning/cloud-providers](https://developers.facebook.com/docs/facebook-at-work/provisioning/cloud-providers)


## ユーザーを Facebook at Work に割り当てる

プロビジョニングされた AAD ユーザーがアクセス パネルに Facebook at Work を表示できるようにするには、Microsoft Azure 管理ポータル内でアクセス権を割り当てる必要があります。

**ユーザーを Facebook at Work に割り当てるには:**

1.  Microsoft Azure 管理ポータルの Facebook at Work のスタート ページで、**[アカウントの割り当て]** をクリックします。

2.  **[表示]** メニューで、Facebook at Work にユーザーとグループのどちらを割り当てるかを選択し、チェックマーク ボタンをクリックします。

3.  結果の一覧で、Facebook at Work を割り当てるユーザーまたはグループを選択します。

4.  ページ フッターにある **[割り当て]** をクリックします。


## その他のリソース

* [Azure Active Directory による SaaS アプリを統合する方法に関するチュートリアルの一覧](active-directory-saas-tutorial-list.md)
* [アプリケーション アクセスと Azure Active Directory でのシングル サインオンとは](active-directory-appssoaccess-whatis.md)



[1]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_general_01.png 
[2]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_general_02.png 
[3]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_general_03.png 
[4]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_general_04.png 

