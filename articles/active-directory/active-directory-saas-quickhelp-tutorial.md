<properties
    pageTitle="チュートリアル: Azure Active Directory と QuickHelp の統合 | Microsoft Azure"
    description="Azure Active Directory と QuickHelp の間でシングル サインオンを構成する方法について説明します。"
    services="active-directory"
    documentationCenter=""
    authors="jeevansd"
    manager="prasannas"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/01/2015"
    ms.author="jeedes"/>



# チュートリアル: Azure Active Directory と QuickHelp の統合

このチュートリアルでは、Azure Active Directory (Azure AD) とクイック ヘルプを統合する方法を説明します。<br>Azure AD との統合のクイック ヘルプを使うと、次の利点があります。

- QuickHelp にアクセスする Azure AD ユーザーを制御できます。
- ユーザーが自分の Azure AD アカウントで自動的に QuickHelp にサインオン (シングル サインオン) できるようにします。
- 1 つの中央サイト (Azure Active Directory ポータル) でアカウントを管理できます。

Azure AD と SaaS アプリの統合に関する詳細を確認するを参照して [アプリケーションへのアクセスと Azure Active Directory でのシングル サインオン](active-directory-appssoaccess-whatis.md)します。

## 前提条件

QuickHelp と Azure AD の統合を構成するには、次のものが必要です。

- Azure AD サブスクリプション
- QuickHelp でのシングル サインオンが有効なサブスクリプション


> [AZURE.NOTE] このチュートリアルの手順をテストする場合、運用環境を使用しないことをお勧めします。


このチュートリアルの手順をテストするには、次の推奨事項に従ってください。

- 必要な場合を除き、運用環境は使用しないでください。
- Azure AD の評価環境を取得していない場合は、1 か月の試用版を入手できます [ここ](https://azure.microsoft.com/pricing/free-trial/)します。


## シナリオの説明

このチュートリアルでは、使用すると、テスト環境で Azure AD シングル サインオンをテストします。 <br>
このチュートリアルで紹介するシナリオは、2 つの主な構成要素で構成されます。

1. ギャラリーからの QuickHelp の追加
2. Azure AD シングル サインオンの構成とテスト


## ギャラリーからの QuickHelp の追加

Azure AD への QuickHelp の統合を構成するには、ギャラリーから管理対象 SaaS アプリの一覧に QuickHelp を追加する必要があります。

**ギャラリーから QuickHelp を追加するには、次の手順に従います。**

1. **Microsoft Azure 管理ポータル**の左側のナビゲーション ウィンドウで、**[Active Directory]** をクリックします。 
<br><br>![Active Directory][1]<br>

2. **[ディレクトリ]** の一覧から、ディレクトリ統合を有効にするディレクトリを選択します。

3. ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。<br><br>
![アプリケーション][2]<br>
4. クリックして **追加** ページの下部にあります。<br><br>
![アプリケーション][3]<br>
5. **実行する操作** ダイアログ ボックスで、をクリックして **ギャラリーからアプリケーションを追加**します。<br><br>
![アプリケーション][4]<br>
6. 検索ボックスに入力 **クイック ヘルプ**します。<br><br>
![アプリケーション][5]<br>
7. 結果ウィンドウで [ **クイック ヘルプ**, 、クリックして **完了** アプリケーションを追加します。
<br><br>![アプリケーション][500]<br>


## Azure AD シングル サインオンの構成とテスト

このセクションの目的は、"Britta Simon" というテスト ユーザーに基づいて、QuickHelp で Azure AD のシングル サインオンを構成し、テストする方法について説明することです。


QuickHelp で Azure AD のシングル サインオンを構成してテストするには、次の構成要素を完了する必要があります。

1. * *[構成する Azure AD シングル サインオン](#configuring-azure-ad-single-single-sign-on)* * - この機能を使用するユーザーをできるようにします。
2. * *[Azure AD テスト ユーザーを作成する](#creating-an-azure-ad-test-user)* * - を Azure AD シングル サインオン Britta Simon でテストします。
4. * *[クイック ヘルプ テスト ユーザーを作成する](#creating-a-quickhelp-test-user)* * - 自分の Azure AD の表現にリンクされるクイック ヘルプに Britta Simon に対応します。
5. * *[Azure AD テスト ユーザーの割り当て](#assigning-the-azure-ad-test-user)* * - Azure AD シングル サインオンを使用する Britta Simon をできるようにします。
5. * *[シングル サインオンをテスト](#testing-single-sign-on)* * - 構成が機能するかどうかを確認します。

### Azure AD シングル サインオンの構成

このセクションでは、Azure AD シングル サインオン Azure AD ポータルで有効にして、クイック ヘルプ アプリケーションでシングル サインオンを構成するにです。<br>

**QuickHelp で Azure AD シングル サインオンを構成するには、次の手順に従います。**

1. Azure AD ポータルでの **クイック ヘルプ** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、 **シングル サインオンを構成する**  ダイアログ。
<br><br> ![でのシングル サインオンを構成する][6] <br>

2. **どのようなユーザーへのクイック ヘルプにアクセス** ] ページで、[ **Azure AD シングル サインオン**, 、] をクリックし、 **次**します。
<br><br> ![Azure AD シングル サインオン][7] <br>

3. **[アプリケーション設定の構成]** ダイアログ ページで、次の手順に従います。
<br><br>![アプリ設定の構成][8] <br>

     a.  **サインオン URL** ユーザーがクイック ヘルプ サイト (e.g.:* https://quickhelp.com/bsiazure/*) にサインオンする URL を使用] ボックスに、型です。
     > [AZURE.NOTE] サインオン URL の値がわからない場合は、QuickHelp サポート チームに問い合わせてください。

     b. **[次へ]** をクリックします。

4. **クイック ヘルプでのシングル サインオンの構成** ] ページで、次の手順:「実行 **メタデータのダウンロード**, しコンピューター ローカルにメタデータ ファイルを保存します。
<br><br>![は Azure AD Connect][9] <br>

1. QuickHelp 企業サイトに管理者としてサインオンします。

2. 上部のメニューで **[Admin]** をクリックします。
<br><br>![シングル サインオンを構成します。][21]<br>

1. **クイック ヘルプ管理** ] メニューのをクリックして **設定**します。
<br><br>![シングル サインオンを構成します。][22]<br>

1. **[認証設定]** をクリックします。

1. **認証設定** ] ページで、次の手順を実行
<br><br>![シングル サインオンを構成します。][23]<br>

    a. **SSO 型** として **WSFederation** を選びます。

    b. ダウンロードした Azure メタデータ ファイルをアップロードするには、**[参照]** をクリックし、目的のファイルに移動し、**[メタデータのアップロード]** をクリックします。

    d.  **電子メール** ] ボックスに「 **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**します。

    e.  **名** ] ボックスに、 **入力 http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**します。

    f.  **姓** ] ボックスに、 **入力 http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**

    g. **[   操作バー ]** で、**[保存]** をクリックします。

6. Azure AD ポータルで、シングル サインオンの構成確認を選択し、**[次へ]** をクリックします。 
<br><br>![Azure AD Connect とは][10]<br>

7. **[シングル サインオンの確認]** ページで **[完了]** をクリックします。  
  <br><br>![Azure AD Connect とは][11]




### Azure AD のテスト ユーザーの作成

このセクションでは、Britta Simon と呼ばれる Azure ポータルでテスト ユーザーを作成します。<br>
ユーザーの一覧で選択 **Britta Simon**.<br><br>![Azure AD ユーザーを作成][20]<br>

**Azure AD でテスト ユーザーを作成するには、次の手順に従います。**

1. **Azure 管理ポータル**, 、左側のナビゲーション ウィンドウで、次のようにクリックします。 **Active Directory**します。<br><br>
![Azure AD テスト ユーザーを作成します。](./media/active-directory-saas-quickhelp-tutorial/create_aaduser_02.png)<br>

2. **[ディレクトリ]** の一覧から、ディレクトリ統合を有効にするディレクトリを選択します。

3. 上部のメニューで、ユーザーの一覧を表示する] をクリックして **ユーザー**.<br><br>![Azure AD テスト ユーザーを作成する](./media/active-directory-saas-quickhelp-tutorial/create_aaduser_03.png) <br>

4. 開くには、 **ユーザーの追加** ] ダイアログ ボックスで、下部にあるツールバーをクリックして **ユーザーの追加**します。<br><br>![Azure AD テスト ユーザーを作成します。](./media/active-directory-saas-quickhelp-tutorial/create_aaduser_04.png)<br>

5. **このユーザーについてお聞かせ** ] ダイアログ ページで、次の手順を実行します <br><br>![Azure AD テスト ユーザーを作成する](./media/active-directory-saas-quickhelp-tutorial/create_aaduser_05.png) 。<br>

    a. [ユーザーの種類] として [組織内の新しいユーザー] を選択します。

    b. **[ユーザー名]** ボックスに「**BrittaSimon**」と入力します。

    c. **[次へ]** をクリックします。

6.  **ユーザー プロファイル** ] ダイアログ ページで、次の手順を実行します。 
<br><br>![Azure AD テスト ユーザーを作成する](./media/active-directory-saas-quickhelp-tutorial/create_aaduser_06.png) <br>

    a. **[名]** ボックスに「**Britta**」と入力します。

    b. **[姓]** ボックスに「**Simon**」と入力します。

    c. **[表示名]** ボックスに「**Britta Simon**」と入力します。

    d. **[ロール]** 一覧で **[ユーザー]** を選択します。
    e. **[次へ]** をクリックします。

7. **一時パスワードの取得** ダイアログ ページで、をクリックして **作成**します。
<br><br> ![Azure AD テスト ユーザーを作成する](./media/active-directory-saas-quickhelp-tutorial/create_aaduser_07.png) <br>

8. **一時パスワードの取得** ] ダイアログ ページで、次の手順を実行します。
<br><br>![Azure AD テスト ユーザーを作成する](./media/active-directory-saas-quickhelp-tutorial/create_aaduser_08.png) <br>

    a. **[新しいパスワード]** の値を書き留めます。

    b. **[完了]** をクリックします。



### QuickHelp テスト ユーザーの作成

このセクションの目的は、QuickHelp で Britta Simon というユーザーを作成することです。
シングル サインオンを機能させるには、Azure AD ユーザーに対応する QuickHelp ユーザーが Azure AD で認識されている必要があります。 言い換えると、Azure AD ユーザーと QuickHelp の関連ユーザーの間で、リンク関係が確立されている必要があります。

QuickHelp では、ジャストインタイム プロビジョニングがサポートされています。 つまり、必要に応じて、ユーザー アカウントは、QuickHelp で自動的に作成され、Azure AD アカウントにリンクされます。

このセクションでは、ユーザー側で必要な操作はありません。


### Azure AD テスト ユーザーの割り当て

このセクションでは、クイック ヘルプにアクセスを付与することによって Azure シングル サインオンを使用する Britta Simon を有効にするとします。
<br><br>![ユーザーを割り当てる][200] <br>

**QuickHelp に Britta Simon を割り当てるには、次の手順に従います。**

1. Azure ポータルで、ディレクトリ ビューでアプリケーション ビューを開く] をクリックして **アプリケーション** 上部のメニュー。
<br><br>![ユーザーを割り当てる][201] <br>

2. アプリケーションの一覧で選択 **クイック ヘルプ**します。
<br><br>![ユーザーを割り当てる][202] <br>

1. 上部のメニューで **[ユーザー]** をクリックします。
<br><br>![ユーザーを割り当てる][203] <br>

1. ユーザーの一覧で **[Britta Simon]** を選択します。

2. 下部にあるツールバー [ **割り当てる**します。
<br><br>![ユーザーを割り当てる][205]



### シングル サインオンのテスト

このセクションでは、構成のテスト Azure AD シングル サインオン アクセス パネルを使用します。<br>
アクセス パネルで [QuickHelp] タイルをクリックすると、自動的に QuickHelp アプリケーションにサインオンします。


## その他のリソース

* [Azure Active Directory による SaaS アプリを統合する方法に関するチュートリアルの一覧](active-directory-saas-tutorial-list.md)
* [アプリケーション アクセスと Azure Active Directory でのシングル サインオンとは](active-directory-appssoaccess-whatis.md)





[1]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_01.png 
[2]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_02.png 
[3]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_03.png 
[4]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_04.png 
[5]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_01.png 
[500]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_14.png 
[6]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_05.png 
[7]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_02.png 
[8]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_03.png 
[9]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_04.png 
[10]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_06.png 
[11]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_07.png 
[20]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_100.png 
[21]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_05.png 
[22]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_06.png 
[23]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_07.png 
[24]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_08.png 
[25]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_09.png 
[26]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_10.png 
[27]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_11.png 
[28]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_12.png 
[200]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_200.png 
[201]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_201.png 
[202]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_13.png 
[203]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_203.png 
[204]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_204.png 
[205]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_205.png 
[400]: ./media/active-directory-saas-QuickHelp-tutorial/tutorial_QuickHelp_400.png 
[401]: ./media/active-directory-saas-QuickHelp-tutorial/tutorial_QuickHelp_401.png 
[402]: ./media/active-directory-saas-QuickHelp-tutorial/tutorial_QuickHelp_402.png 

