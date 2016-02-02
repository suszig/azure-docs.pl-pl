<properties
    pageTitle="チュートリアル: Azure Active Directory と Small Improvements の統合 | Microsoft Azure"
    description="Azure Active Directory と Small Improvements の間でシングル サインオンを構成する方法について説明します。"
    services="active-directory"
    documentationCenter=""
    authors="jeevansd"
    manager="stevenpo"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/14/2015"
    ms.author="jeedes"/>



# チュートリアル: Azure Active Directory と Small Improvements の統合

このチュートリアルでは、Azure Active Directory (Azure AD) の小さな改良を統合する方法を説明します。<br>小さな改良が Azure AD と統合することを使うと、次の利点があります。

- Small Improvements にアクセスする Azure AD ユーザーを制御できます。
- ユーザーが自分の Azure AD アカウントで自動的に Small Improvements にサインオン (シングル サインオン) できるようにします。
- 1 つの中央サイト (Azure クラシック ポータル) でアカウントを管理できます。

Azure AD と SaaS アプリの統合に関する詳細を確認するを参照して [アプリケーションへのアクセスと Azure Active Directory でのシングル サインオン](active-directory-appssoaccess-whatis.md)します。

## 前提条件

Azure AD と Small Improvements の統合を構成するには、次のものが必要です。

- Azure AD サブスクリプション
- Small Improvements でのシングル サインオンが有効なサブスクリプション


> [AZURE.NOTE] このチュートリアルの手順をテストする場合、運用環境を使用しないことをお勧めします。


このチュートリアルの手順をテストするには、次の推奨事項に従ってください。

- 必要な場合を除き、運用環境は使用しないでください。
- Azure AD の評価環境を取得していない場合は、1 か月の試用版を入手できます [ここ](https://azure.microsoft.com/pricing/free-trial/)します。


## シナリオの説明

このチュートリアルでは、使用すると、テスト環境で Azure AD シングル サインオンをテストします。 <br>
このチュートリアルで紹介するシナリオは、2 つの主な構成要素で構成されます。

1. ギャラリーから Small Improvements を追加する
2. Azure AD シングル サインオンの構成とテスト


## ギャラリーから Small Improvements を追加する

Azure AD への Small Improvements の統合を構成するには、ギャラリーから管理対象 SaaS アプリの一覧に Small Improvements を追加する必要があります。

**ギャラリーから Small Improvements を追加するには、次の手順に従います。**

1. **Azure クラシック ポータル**, 、左側のナビゲーション ウィンドウで、次のようにクリックします。 **Active Directory**します。 <br><br>
![Active Directory][1]<br>

2. **[ディレクトリ]** の一覧から、ディレクトリ統合を有効にするディレクトリを選択します。

3. ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。<br><br>
![アプリケーション][2]<br>
4. クリックして **追加** ページの下部にあります。<br><br>
![アプリケーション][3]<br>
5. **実行する操作** ダイアログ ボックスで、をクリックして **ギャラリーからアプリケーションを追加**します。<br><br>
![アプリケーション][4]<br>
6. 検索ボックスに入力 **小さな改良が**します。<br><br>
![Azure AD テスト ユーザーを作成します。](./media/active-directory-saas-smallimprovements-tutorial/tutorial_smallimprovements_01.png)<br>
7. 結果ウィンドウで [ **小さな改良が**, 、クリックして **完了** アプリケーションを追加します。
<br><br>

## Azure AD シングル サインオンの構成とテスト

このセクションの目的は、"Britta Simon" というテスト ユーザーに基づいて、Small Improvements で Azure AD のシングル サインオンを構成し、テストする方法について説明することです。

シングル サインオンを機能させるには、Azure AD ユーザーに対応する Small Improvements ユーザーが Azure AD で認識されている必要があります。つまり、Azure AD のユーザーと小さな機能強化に関連するユーザーの間のリンクの関係を確立する必要があります。<br>
このリンク関係は、Azure AD の **[ユーザー名]** の値を、Small Improvements の **[Username]** の値として割り当てることで確立されます。

Small Improvements で Azure AD のシングル サインオンを構成してテストするには、次の構成要素を完了する必要があります。

1. * *[構成する Azure AD シングル サインオン](#configuring-azure-ad-single-single-sign-on)* * - この機能を使用するユーザーをできるようにします。
2. * *[Azure AD テスト ユーザーを作成する](#creating-an-azure-ad-test-user)* * - を Azure AD シングル サインオン Britta Simon でテストします。
4. * *[小さな改良がテスト ユーザーを作成する](#creating-a-small-improvements-test-user)* * - Britta Simon に対応する小さな改良が自分の Azure AD の表現にリンクされているのです。
5. * *[Azure AD テスト ユーザーの割り当て](#assigning-the-azure-ad-test-user)* * - Azure AD シングル サインオンを使用する Britta Simon をできるようにします。
5. * *[シングル サインオンをテスト](#testing-single-sign-on)* * - 構成が機能するかどうかを確認します。

### Azure AD シングル サインオンの構成

このセクションの目的は、Azure クラシック ポータルで Azure AD のシングル サインオンを有効にすることと、Small Improvements アプリケーションでシングル サインオンを構成することです。



**Small Improvements で Azure AD シングル サインオンを構成するには、次の手順に従います。**

1. Azure クラシック ポータルでの **小さな改良が** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、 **シングル サインオンを構成する**  ダイアログ。
<br><br> ![でのシングル サインオンを構成する][6] <br>

2. **どのようなユーザーへの小さな改良が** ] ページで、[ **Azure AD シングル サインオン**, 、] をクリックし、 **次**します。
<br><br> ![でのシングル サインオンを構成する](./media/active-directory-saas-smallimprovements-tutorial/tutorial_smallimprovements_03.png) <br>

3. **[アプリケーション設定の構成]** ダイアログ ページで、次の手順に従います。
<br><br>![でのシングル サインオンを構成する](./media/active-directory-saas-smallimprovements-tutorial/tutorial_smallimprovements_04.png) <br>
    > [AZURE.NOTE] 小さな改良が、サポート チームの連絡先 [Support@small-improvements.com](mailto:support@small-improvements.com) ドメイン名、アカウントを構成します。 これは、シングル サインオンが動作するために必要です。

    a.  **サインオン URL** ユーザーが、小さな改良がアプリケーションにサインオンする URL を使用] ボックスに、型です。  
    b. **[次へ]** をクリックします。

4. **小さな改良がでのシングル サインオンの構成** ] ページで、次の手順を実行します。
<br><br>![でのシングル サインオンを構成する](./media/active-directory-saas-smallimprovements-tutorial/tutorial_smallimprovements_05.png) <br>

    a. **[証明書のダウンロード]** をクリックし、コンピューターにファイルを保存します。

    b. **[次へ]** をクリックします。

1. 別の Web ブラウザー ウィンドウで、管理者として Small Improvements 企業サイトにサインオンします。

1. メイン ダッシュ ボード] ページで、クリックして **管理** 左側のボタンをクリックします。
<br><br> ![でのシングル サインオンを構成する](./media/active-directory-saas-smallimprovements-tutorial/tutorial_smallimprovements_06.png) <br>

1. クリックして、 **SAML SSO** ボタンから **統合** セクションです。
<br><br> ![でのシングル サインオンを構成する](./media/active-directory-saas-smallimprovements-tutorial/tutorial_smallimprovements_07.png) <br>

1. SSO のセットアップ] ページで、次の手順に従います。
<br><br>![でのシングル サインオンを構成する](./media/active-directory-saas-smallimprovements-tutorial/tutorial_smallimprovements_08.png) <br>

    a. Azure クラシック ポータルで、**[Small Improvements でのシングル サインオンの構成]** ダイアログ ページの **[SAML SSO URL]** の値をコピーし、**[HTTP エンドポイント]** テキスト ボックスに貼り付けます。

    b. ダウンロードした証明書をメモ帳で開き、その内容をコピーして、**[x509 証明書]** ボックスに貼り付けます。

    c. SSO およびログイン フォーム認証オプションをユーザーが使用できるようにするには、**[ログイン/パスワードによるアクセスも有効にする]** オプションをオンにします。

    d. **[SAML プロンプト]** ボックスに、SSO ログイン ボタンの名前として適切な値を入力します。

    e. **[保存]** をクリックします。

6. Azure クラシック ポータルで、シングル サインオンの構成情報を選択し、クリックして **次**します。
<br><br>![Azure AD シングル サインオン][10]<br>

7. **[シングル サインオンの確認]** ページで **[完了]** をクリックします。  
  <br><br>![Azure AD シングル サインオン][11]




### Azure AD のテスト ユーザーの作成

このセクションでは、Britta Simon と呼ばれる Azure クラシック ポータルでテスト ユーザーを作成します。<br>
ユーザーの一覧で選択 **Britta Simon**.<br><br>![Azure AD ユーザーを作成][20]<br>

**Azure AD でテスト ユーザーを作成するには、次の手順に従います。**

1. **Azure クラシック ポータル**の左側のナビゲーション ウィンドウで、**[Active Directory]** をクリックします。
<br><br>![Azure AD テスト ユーザーを作成する](./media/active-directory-saas-smallimprovements-tutorial/create_aaduser_09.png) <br>

2. **[ディレクトリ]** の一覧から、ディレクトリ統合を有効にするディレクトリを選択します。

3. 上部のメニューで、ユーザーの一覧を表示する] をクリックして **ユーザー**します。
<br><br> ![Azure AD テスト ユーザーを作成する](./media/active-directory-saas-smallimprovements-tutorial/create_aaduser_03.png) <br>

4. 開くには、 **ユーザーの追加** ] ダイアログ ボックスで、下部にあるツールバーをクリックして **ユーザーの追加**します。
<br><br> ![Azure AD テスト ユーザーを作成する](./media/active-directory-saas-smallimprovements-tutorial/create_aaduser_04.png) <br>

5. **このユーザーについてお聞かせ** ] ダイアログ ページで、次の手順を実行します。
<br><br> ![Azure AD テスト ユーザーを作成する](./media/active-directory-saas-smallimprovements-tutorial/create_aaduser_05.png) <br>

    a. [ユーザーの種類] として [組織内の新しいユーザー] を選択します。

    b. **[ユーザー名]** ボックスに「**BrittaSimon**」と入力します。

    c. **[次へ]** をクリックします。

6.  **ユーザー プロファイル** ] ダイアログ ページで、次の手順を実行します。
<br><br>![Azure AD テスト ユーザーを作成する](./media/active-directory-saas-smallimprovements-tutorial/create_aaduser_06.png) <br>

    a. **[名]** ボックスに「**Britta**」と入力します。

    b. **[姓]** ボックスに「**Simon**」と入力します。

    c. **[表示名]** ボックスに「**Britta Simon**」と入力します。

    d. **[ロール]** 一覧で **[ユーザー]** を選択します。
    e. **[次へ]** をクリックします。

7. **一時パスワードの取得** ダイアログ ページで、をクリックして **作成**します。
<br><br> ![Azure AD テスト ユーザーを作成する](./media/active-directory-saas-smallimprovements-tutorial/create_aaduser_07.png) <br>

8. **一時パスワードの取得** ] ダイアログ ページで、次の手順を実行します。
<br><br>![Azure AD テスト ユーザーを作成する](./media/active-directory-saas-smallimprovements-tutorial/create_aaduser_08.png) <br>

    a. **[新しいパスワード]** の値を書き留めます。

    b. **[完了]** をクリックします。



### Small Improvements テスト ユーザーの作成

このセクションの目的は、Small Improvements で Britta Simon というユーザーを作成することです。
既に有効にしているで [構成する Azure AD シングル サインオン](#configuring-azure-ad-single-single-sign-on)します。


**Small Improvements で Britta Simon というユーザーを作成するには、次の手順に従います。**

1. 管理者として Small Improvements 企業サイトにサインオンします。

1. ホーム ページの左側のメニューに移動し、**[管理]** をクリックします。

1. クリックして、 **ユーザー ディレクトリ** ユーザー管理] セクションからボタンをクリックします。<br><br>![Azure AD テスト ユーザーを作成する](./media/active-directory-saas-smallimprovements-tutorial/tutorial_smallimprovements_10.png) <br>

1. クリックして **チーム メンバーを追加**します。<br><br>![Azure AD テスト ユーザーを作成する](./media/active-directory-saas-smallimprovements-tutorial/tutorial_smallimprovements_11.png) <br>

1. 型、 **名**, 、**姓** と **電子メール アドレス** 関連テキスト ボックスに、プロビジョニングする有効なユーザーのです。<br><br>![Azure AD テスト ユーザーを作成する](./media/active-directory-saas-smallimprovements-tutorial/tutorial_smallimprovements_12.png) <br>

1. **[通知電子メールを送信する]** ボックスに、個人のメッセージを入力することもできます。 通知を送信しない場合は、このチェック ボックスをオフにします。

1. **[ユーザーの作成]** をクリックします。

### Azure AD テスト ユーザーの割り当て

このセクションでは、小規模の向上を自分のアクセスを付与することによって Azure シングル サインオンを使用する Britta Simon を有効にするとします。
<br><br>![ユーザーを割り当てる][200] <br>

**Small Improvements に Britta Simon を割り当てるには、次の手順に従います。**

1. Azure クラシック ポータルでは、ビューを開くアプリケーション、ディレクトリ ビューで、クリックして **アプリケーション** 上部のメニュー。
<br><br>![ユーザーを割り当てる][201] <br>

2. アプリケーションの一覧で選択 **小さな改良が**します。
<br><br>![でのシングル サインオンを構成する](./media/active-directory-saas-smallimprovements-tutorial/tutorial_smallimprovements_50.png) <br>

1. 上部のメニューで **[ユーザー]** をクリックします。
<br><br>![ユーザーを割り当てる][203] <br>

1. ユーザーの一覧で **[Britta Simon]** を選択します。

2. 下部にあるツールバー [ **割り当てる**します。
<br><br>![ユーザーを割り当てる][205]



### シングル サインオンのテスト

このセクションでは、構成のテスト Azure AD シングル サインオン アクセス パネルを使用します。<br>
アクセス パネルで [Small Improvements] タイルをクリックすると、自動的に Small Improvements アプリケーションにサインオンします。


## その他のリソース

* [Azure Active Directory による SaaS アプリを統合する方法に関するチュートリアルの一覧](active-directory-saas-tutorial-list.md)
* [アプリケーション アクセスと Azure Active Directory でのシングル サインオンとは](active-directory-appssoaccess-whatis.md)





[1]: ./media/active-directory-saas-smallimprovements-tutorial/tutorial_general_01.png 
[2]: ./media/active-directory-saas-smallimprovements-tutorial/tutorial_general_02.png 
[3]: ./media/active-directory-saas-smallimprovements-tutorial/tutorial_general_03.png 
[4]: ./media/active-directory-saas-smallimprovements-tutorial/tutorial_general_04.png 
[6]: ./media/active-directory-saas-smallimprovements-tutorial/tutorial_general_05.png 
[10]: ./media/active-directory-saas-smallimprovements-tutorial/tutorial_general_06.png 
[11]: ./media/active-directory-saas-smallimprovements-tutorial/tutorial_general_07.png 
[20]: ./media/active-directory-saas-smallimprovements-tutorial/tutorial_general_100.png 
[200]: ./media/active-directory-saas-smallimprovements-tutorial/tutorial_general_200.png 
[201]: ./media/active-directory-saas-smallimprovements-tutorial/tutorial_general_201.png 
[203]: ./media/active-directory-saas-smallimprovements-tutorial/tutorial_general_203.png 
[204]: ./media/active-directory-saas-smallimprovements-tutorial/tutorial_general_204.png 
[205]: ./media/active-directory-saas-smallimprovements-tutorial/tutorial_general_205.png 

