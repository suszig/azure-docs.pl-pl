<properties
    pageTitle="チュートリアル: Azure Active Directory と Facebook at Work の統合 | Microsoft Azure"
    description="Azure Active Directory と Facebook at Work の間でシングル サインオンを構成する方法について説明します。"
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


# チュートリアル: Azure Active Directory と Facebook at Work の統合

このチュートリアルでは、Azure Active Directory (Azure AD) を職場での Facebook を統合する方法を説明します。<br>職場での Facebook を Azure AD と統合するには、次のメリットがあります。 

- Facebook at Work にアクセスする Azure AD ユーザーを制御できます 
- Facebook at Work へのアクセス権が付与されているユーザーには、アカウントを自動的にプロビジョニングできます
- ユーザーが自分の Azure AD アカウントで自動的に Facebook at Work にサインオン (シングル サインオン) する機能を有効にすることができます
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
Azure AD への Facebook at Work の統合を構成するには、ギャラリーから管理対象 SaaS アプリの一覧に Facebook at Work を追加する必要があります。

**ギャラリーから Facebook at Work を追加するには、次の手順に従います。**

1.  **Azure 管理ポータル**, 、左側のナビゲーション ウィンドウで、次のようにクリックします。 **Active Directory**します。 
<br><br>![Active Directory][1]<br>

2.  **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3. ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。
<br><br>![アプリケーション][2]<br>

4. クリックして **追加** ページの下部にあります。
<br><br>![アプリケーション][3]<br>

5.  **実行する操作** ダイアログ ボックスで、をクリックして **ギャラリーからアプリケーションを追加**します。
<br><br>![アプリケーション][4]<br>

6. 検索ボックスに入力 **職場での Facebook**します。

7. 結果ウィンドウで [ **職場での Facebook**, 、] をクリックし、 **完了** アプリケーションを追加します。


### Azure AD シングル サインオンの構成

このセクションでは、SAML プロトコルに基づくフェデレーションを使用して、ユーザーが Azure Active Directory のアカウントで Facebook at Work に対する認証を行えるようにする方法を説明します。

**Facebook at Work で Azure AD シングル サインオンを構成するには、次の手順に従います。**

1.  Azure 管理ポータルで、職場で Facebook を追加したら、クリックして **シングル サインオンを構成する**です。

2.   **アプリケーション URL の構成** 画面で、ユーザーが作業のアプリケーションで、Facebook にサインインする場所の URL を入力します。 これは、Facebook の作業のテナント url 
(例: https://example.facebook.com/)。 完了すると、クリックして **次**します。

3.  別の Web ブラウザーのウィンドウで、Facebook at Work 企業サイトに管理者としてログインします。

4. Facebook を id プロバイダーとして Azure AD を使用する作業で構成する次の URL での指示に従います: [https://developers.facebook.com/docs/facebook-at-work/authentication/cloud-providers](https://developers.facebook.com/docs/facebook-at-work/authentication/cloud-providers)

5.  完了すると、Azure 管理ポータルを表示するブラウザー ウィンドウに戻るを手順を完了したことを確認するチェック ボックスをクリックしてクリックして **次** と **完了**します。


## ユーザーを Facebook at Work に自動的にプロビジョニングする

Azure AD は、割り当てられたユーザーのアカウントの詳細を Facebook at Work と自動的に同期する機能をサポートしています。 この自動同期機能によって、ユーザーが初めてサインインする前に、Facebook at Work がユーザーのアクセスを承認するために必要なデータを取得することができます。 また、Azure AD のアクセス権が取り消された場合、Facebook at Work からユーザーのプロビジョニングを解除することができます。

クリックするを自動プロビジョニングを設定できます **アカウント プロビジョニングの構成** 、Azure 管理ポータル ウィンドウでします。

自動プロビジョニングを構成する方法の詳細については、次を参照してください [https://developers.facebook.com/docs/facebook-at-work/provisioning/cloud-providers。](https://developers.facebook.com/docs/facebook-at-work/provisioning/cloud-providers)


## ユーザーを Facebook at Work に割り当てる

プロビジョニングされた AAD ユーザーがアクセス パネルに Facebook at Work を表示できるようにするには、Microsoft Azure 管理ポータル内でアクセス権を割り当てる必要があります。

**ユーザーを Facebook at Work に割り当てるには:**

1.  Azure 管理ポータルでの職場での Facebook の開始ページでクリックして **アカウントを割り当てて、**です。

2.   **表示** ] メニューの [ユーザーまたはグループを職場での Facebook に割り当てるし、チェック マークをクリックするかどうかを選択します。

3.  結果の一覧で、Facebook at Work を割り当てるユーザーまたはグループを選択します。

4.  ページ フッター内をクリックして、 **割り当てる** ] ボタンをクリックします。


## その他のリソース

* [SaaS アプリと Azure Active Directory を統合する方法に関するチュートリアルの一覧](active-directory-saas-tutorial-list.md)
* [Azure Active Directory のアプリケーション アクセスとシングル サインオンとは](active-directory-appssoaccess-whatis.md)

<!--Image references-->
[1]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_general_04.png





