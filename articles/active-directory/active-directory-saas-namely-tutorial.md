<properties
    pageTitle="チュートリアル: Azure Active Directory と Namely の統合 | Microsoft Azure"
    description="Azure Active Directory と Namely の間でシングル サインオンを構成する方法について確認します。"
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


# チュートリアル: Azure Active Directory と Namely の統合

このチュートリアルでは、つまり Azure Active Directory (Azure AD) と統合する方法について説明します。<br>つまり Azure AD と統合は、次の利点があります。 

- Namely にアクセスするユーザーを Azure AD で管理できます。 
- ユーザーが各自の Azure AD アカウントで Namely に自動的にサインオン (シングル サインオン) するように、設定が可能です。
- 1 つの中央サイト (Azure Active Directory ポータル) でアカウントを管理できます。

Azure AD と SaaS アプリの統合に関する詳細を確認するを参照して [アプリケーションへのアクセスと Azure Active Directory でのシングル サインオン](active-directory-appssoaccess-whatis.md)します。

## 前提条件 

Azure AD と Namely の統合を構成するには、次のものが必要です。

- Azure AD サブスクリプション
- Namely でのシングル サインオンが有効なサブスクリプション


> [AZURE.NOTE] このチュートリアルでは、手順をテストするには、実稼働環境の使用をおしないでいます。


このチュートリアルの手順をテストするには、次の推奨事項に従ってください。

- 必要な場合を除き、運用環境は使用しないでください。
- Azure AD の評価環境を取得していない場合は、1 か月の試用版を入手できます [ここ](https://azure.microsoft.com/pricing/free-trial/)します。 

 
## シナリオの説明
このチュートリアルの目的は、テスト環境で Azure AD のシングル サインオンをテストできるようにすることです。 <br>
このチュートリアルで紹介するシナリオは、2 つの主な構成要素で構成されます。

1. ギャラリーから Namely を追加する 
2. Azure AD シングル サインオンの構成とテスト


## ギャラリーから Namely を追加する
Azure AD への Namely の統合を構成するには、ギャラリーから管理対象 SaaS アプリの一覧に Namely を追加する必要があります。

**ギャラリーから Namely を追加するには、次の手順を実行します。**

1.  **Azure 管理ポータル**, 、左側のナビゲーション ウィンドウで、次のようにクリックします。 **Active Directory**します。 <br><br>
![Active Directory][1]<br>

2.  **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3. ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。<br><br>
![アプリケーション][2]<br>
4. クリックして **追加** ページの下部にあります。<br><br>
![アプリケーション][3]<br>
5.  **実行する操作** ダイアログ ボックスで、をクリックして **ギャラリーからアプリケーションを追加**します。<br><br>
![アプリケーション][4]<br>
6. 検索ボックスに入力 **つまり**します。<br><br>
![Azure AD テスト ユーザーを作成します。](./media/active-directory-saas-namely-tutorial/tutorial_namely_01.png)<br>
7. 結果ウィンドウで [ **つまり**, 、クリックして **完了** アプリケーションを追加します。
<br><br>
![Azure AD テスト ユーザーを作成します。](./media/active-directory-saas-namely-tutorial/tutorial_namely_02.png)<br>

##  Azure AD シングル サインオンの構成とテスト
このセクションの目的は、"Britta Simon" というテスト ユーザーをベースに、Namely での Azure AD のシングル サインオンを構成し、テストする方法について説明することです。

シングル サインオンを機能させるには、Azure AD ユーザーに対応する Namely ユーザーが Azure AD で認識されている必要があります。 つまり、Azure AD のユーザーと関連ユーザーの間のリンク関係つまり必要がありますを確立します。<br>
値を割り当てることでこのリンクの関係が確立される、 **ユーザー名** の値として Azure AD で、 **Username** でつまりします。
 
Namely で Azure AD のシングル サインオンを構成してテストするには、次の手順を完了する必要があります。

1. **[Azure AD シングル サインオンを構成する](#configuring-azure-ad-single-single-sign-on)** - この機能を使用するユーザーをできるようにします。
2. **[Azure AD テスト ユーザーを作成する](#creating-an-azure-ad-test-user)** - を Azure AD シングル サインオン Britta Simon でテストします。
4. **[作成する、つまりユーザーをテスト](#creating-a-namely-test-user)** - Britta Simon の対応するで具体的にリンクされている自分の Azure AD の表現です。
5. **[Azure AD テスト ユーザーの割り当て](#assigning-the-azure-ad-test-user)** - Azure AD シングル サインオンを使用する Britta Simon をできるようにします。
5. **[シングル サインオン テスト](#testing-single-sign-on)** - 構成が機能するかどうかを確認します。

### Azure AD シングル サインオンの構成

このセクションの目的は、Azure AD ポータルで Azure AD のシングル サインオンを有効にすることと、Namely アプリケーションでシングル サインオンを構成することです。 




**Namely で Azure AD シングル サインオンを構成するには、次の手順を実行します。**

1. Azure AD ポータルでの **つまり** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、 **シングル サインオンを構成する**  ダイアログ。
<br><br> ![シングル サインオンを構成します。][6] <br>

2.  **どのような具体的にサインインするユーザー** ] ページで、[ **Azure AD シングル サインオン**, 、] をクリックし、 **次**します。
<br><br> ![シングル サインオンを構成します。](./media/active-directory-saas-namely-tutorial/tutorial_namely_03.png) <br>

3.  **アプリケーション設定の構成** ] ダイアログ ページで、次の手順を実行します。 します。
<br><br>![シングル サインオンを構成します。](./media/active-directory-saas-namely-tutorial/tutorial_namely_04.png) <br>

    a.  **サインオン URL** 、つまりアプリケーションにサインオンする URL は、ユーザーが使用] ボックスに、種類 (例:: *https://fabrikam.Namely.com/*)。

    b. クリックして **次**します。
 
 
4.  **つまりでのシングル サインオン構成** ] ページで、次の手順を実行します。
<br><br>![シングル サインオンを構成します。](./media/active-directory-saas-namely-tutorial/tutorial_namely_05.png) <br>

    a. クリックして **証明書のダウンロード**, 、お使いのコンピューター上のファイルを保存します。

    b. クリックして **次**します。


1. Web ブラウザーの別のウィンドウで、Namely の企業サイトに管理者としてサインオンします。

1. 上部にあるツールバーで、クリックして **会社**します。
<br><br>![シングル サインオンを構成します。](./media/active-directory-saas-namely-tutorial/tutorial_namely_06.png) <br>

1. クリックして、 **設定** ] タブをクリックします。
<br><br>![シングル サインオンを構成します。](./media/active-directory-saas-namely-tutorial/tutorial_namely_07.png) <br>


1. クリックして **SAML**します。
<br><br>![シングル サインオンを構成します。](./media/active-directory-saas-namely-tutorial/tutorial_namely_08.png) <br>


1.  **SAML 設定** ] ページで、次の手順を実行します。
<br><br>![シングル サインオンを構成します。](./media/active-directory-saas-namely-tutorial/tutorial_namely_09.png) <br>

    a. クリックして **Enable SAML**します。 

    b. Azure ポータルでの **つまりでのシングル サインオン構成** ] ダイアログ ページでコピー、 **シングル サインオン サービス URL** 値に設定して、貼り付けます、 **Id プロバイダーの DDO url** ] ボックスに貼り付けます。 

    c. ダウンロードした証明書をメモ帳で開き、コンテンツをコピーして貼り付けます、 **Id プロバイダー証明書** ] ボックスに貼り付けます。    

    d. クリックして **保存**します。


6. Azure AD ポータルで、シングル サインオンの構成情報を選択し、をクリックし、 **次**します。 
<br><br>![Azure AD シングル サインオン][10]<br>

7.  **シングル サインオンによる確認** ] ページで [ **完了**します。  
  <br><br>![Azure AD シングル サインオン][11]




### Azure AD のテスト ユーザーの作成
このセクションの目的は、Azure ポータルで Britta Simon というテスト ユーザーを作成することです。<br>
ユーザーの一覧で選択 **Britta Simon**します。<br><br>![Azure AD ユーザーを作成します。][20]<br>

**Azure AD でテスト ユーザーを作成するには、次の手順に従います。**

1.  **Azure 管理ポータル**, 、左側のナビゲーション ウィンドウで、次のようにクリックします。 **Active Directory**します。
<br><br>![Azure AD テスト ユーザーを作成します。](./media/active-directory-saas-namely-tutorial/create_aaduser_09.png) <br> 

2.  **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3. 上部のメニューで、ユーザーの一覧を表示する] をクリックして **ユーザー**します。
<br><br> ![Azure AD テスト ユーザーを作成します。](./media/active-directory-saas-namely-tutorial/create_aaduser_03.png) <br>
 
4. 開くには、 **ユーザーの追加** ] ダイアログ ボックスで、下部にあるツールバーをクリックして **ユーザーの追加**します。 
<br><br> ![Azure AD テスト ユーザーを作成します。](./media/active-directory-saas-namely-tutorial/create_aaduser_04.png) <br>

5.  **このユーザーについてお聞かせ** ] ダイアログ ページで、次の手順を実行します。 
<br><br> ![Azure AD テスト ユーザーを作成します。](./media/active-directory-saas-namely-tutorial/create_aaduser_05.png) <br> 

    a. [ユーザーの種類] として [組織内の新しいユーザー] を選択します。

    b. [ユーザー名 **textbox**, 、型 **BrittaSimon**します。

    c. クリックして **次**します。

6.   **ユーザー プロファイル** ] ダイアログ ページで、次の手順を実行します。 
<br><br>![Azure AD テスト ユーザーを作成します。](./media/active-directory-saas-namely-tutorial/create_aaduser_06.png) <br>
 
    a.  **名** ] ボックスに「 **Britta**します。  

    b.  **姓** ] ボックスに、型、 **Simon**します。

    c.  **表示名** ] ボックスに「 **Britta Simon**します。

    d.  **ロール** 一覧で、[ **ユーザー**します。
    e. クリックして **次**します。

7.  **一時パスワードの取得** ダイアログ ページで、をクリックして **作成**します。
<br><br> ![Azure AD テスト ユーザーを作成します。](./media/active-directory-saas-namely-tutorial/create_aaduser_07.png) <br>
 
8.  **一時パスワードの取得** ] ダイアログ ページで、次の手順を実行します。
<br><br>![Azure AD テスト ユーザーを作成します。](./media/active-directory-saas-namely-tutorial/create_aaduser_08.png) <br>
  
    a. 値を書き留めて、 **新しいパスワード**します。

    b. クリックして **完了**します。   

  
 
### Namely のテスト ユーザーの作成

このセクションの目的は、Namely で Britta Simon というユーザーを作成することです。

**Namely で Britta Simon というユーザーを作成するには、次の手順を実行します。**

1. Namely の企業サイトに管理者としてサインオンします。

1. 上部にあるツールバーで、クリックして **人**します。
<br><br>![シングル サインオンを構成します。](./media/active-directory-saas-namely-tutorial/tutorial_namely_10.png) <br>

1. クリックして、 **ディレクトリ** ] タブをクリックします。
<br><br>![シングル サインオンを構成します。](./media/active-directory-saas-namely-tutorial/tutorial_namely_11.png) <br>

1. クリックして **Add New Person**します。



1.  **Add New Person** ] ダイアログ ボックスで、次の手順を実行します。

    a.  **名** ] ボックスに「 **Britta**します。

    b.  **姓、名** ] ボックスに「 **Simon**します。

    c.  **電子メール** ] ボックスに、Azure ポータルで Britta の電子メール アドレスを入力します。

    d. クリックして **保存**します。





### Azure AD テスト ユーザーの割り当て

このセクションでは、具体的に自分のアクセスを付与することによって Azure シングル サインオンを使用する Britta Simon を有効にするとします。
<br><br>![ユーザーの割り当て][200] <br>

**Britta Simon を Namely に割り当てるには、次の手順を実行します。**

1. Azure ポータルで、ディレクトリ ビューでアプリケーション ビューを開く] をクリックして **アプリケーション** 上部のメニュー。
<br><br>![ユーザーを割り当てる][201] <br>

2. アプリケーションの一覧で選択 **つまり**します。
<br><br>![シングル サインオンを構成します。](./media/active-directory-saas-namely-tutorial/tutorial_namely_50.png) <br>

1. 上部にあるメニュー [ **ユーザー**します。
<br><br>![ユーザーを割り当てる][203] <br>

1. ユーザーの一覧で選択 **Britta Simon**します。

2. 下部にあるツールバー [ **割り当てる**します。
<br><br>![ユーザーを割り当てる][205]



### シングル サインオンのテスト

このセクションでは、構成のテスト Azure AD シングル サインオン アクセス パネルを使用します。<br>
アクセス パネルで [Namely] タイルをクリックすると、Namely アプリケーションに自動的にサインオンします。


## その他のリソース

* [SaaS アプリと Azure Active Directory を統合する方法に関するチュートリアルの一覧](active-directory-saas-tutorial-list.md)
* [Azure Active Directory のアプリケーション アクセスとシングル サインオンとは](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-namely-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-namely-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-namely-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-namely-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-namely-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-namely-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-namely-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-namely-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-namely-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-namely-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-namely-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-namely-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-namely-tutorial/tutorial_general_205.png








