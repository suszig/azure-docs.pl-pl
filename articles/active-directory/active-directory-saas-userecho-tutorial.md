<properties
    pageTitle="チュートリアル: Azure Active Directory と UserEcho の統合 | Microsoft Azure"
    description="Azure Active Directory と UserEcho の間でシングル サインオンを構成する方法について説明します。"
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
    ms.date="12/04/2015"
    ms.author="jeedes"/>


# チュートリアル: Azure Active Directory と UserEcho の統合

このチュートリアルでは、Azure Active Directory (Azure AD) と UserEcho を統合する方法を説明します。<br>UserEcho を Azure AD と統合するには、次のメリットがあります。 

- UserEcho にアクセスする Azure AD ユーザーを制御できます。 
- ユーザーが自分の Azure AD アカウントで自動的に UserEcho にサインオン (シングル サインオン) できるようにします。
- 1 つの中央サイト (Azure クラシック ポータル) でアカウントを管理できます。

Azure AD と SaaS アプリの統合に関する詳細を確認するを参照して [アプリケーションへのアクセスと Azure Active Directory でのシングル サインオン](active-directory-appssoaccess-whatis.md)します。

## 前提条件 

Azure AD と UserEcho の統合を構成するには、次のものが必要です。

- Azure AD サブスクリプション
- UserEcho でのシングル サインオンが有効なサブスクリプション


> [AZURE.NOTE] このチュートリアルでは、手順をテストするには、実稼働環境の使用をおしないでいます。


このチュートリアルの手順をテストするには、次の推奨事項に従ってください。

- 必要な場合を除き、運用環境は使用しないでください。
- Azure AD の評価環境を取得していない場合は、1 か月の試用版を入手できます [ここ](https://azure.microsoft.com/pricing/free-trial/)します。 

 
## シナリオの説明
このチュートリアルの目的は、テスト環境で Azure AD のシングル サインオンをテストできるようにすることです。 <br>
このチュートリアルで紹介するシナリオは、2 つの主な構成要素で構成されます。

1. ギャラリーからの UserEcho の追加 
2. Azure AD シングル サインオンの構成とテスト


## ギャラリーからの UserEcho の追加
Azure AD への UserEcho の統合を構成するには、ギャラリーから管理対象 SaaS アプリの一覧に UserEcho を追加する必要があります。

**ギャラリーから UserEcho を追加するには、次の手順に従います。**

1.  **Azure クラシック ポータル**, 、左側のナビゲーション ウィンドウで、次のようにクリックします。 **Active Directory**します。 <br><br>
![Active Directory][1]<br>

2.  **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3. ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。<br><br>
![アプリケーション][2]<br>
4. クリックして **追加** ページの下部にあります。<br><br>
![アプリケーション][3]<br>
5.  **実行する操作** ダイアログ ボックスで、をクリックして **ギャラリーからアプリケーションを追加**します。<br><br>
![アプリケーション][4]<br>
6. 検索ボックスに入力 **UserEcho**します。<br><br>
![Azure AD テスト ユーザーを作成します。](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_01.png)<br>
7. 結果ウィンドウで [ **UserEcho**, 、クリックして **完了** アプリケーションを追加します。
<br><br>
![Azure AD テスト ユーザーを作成します。](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_02.png)<br>

##  Azure AD シングル サインオンの構成とテスト
このセクションの目的は、"Britta Simon" というテスト ユーザーに基づいて、UserEcho で Azure AD のシングル サインオンを構成し、テストする方法について説明することです。

シングル サインオンを機能させるには、Azure AD ユーザーに対応する UserEcho ユーザーが Azure AD で認識されている必要があります。 つまり、Azure AD のユーザーと UserEcho に関連するユーザーの間のリンクの関係を確立する必要があります。<br>
値を割り当てることでこのリンクの関係が確立される、 **ユーザー名** の値として Azure AD で、 **Username** UserEcho にします。
 
UserEcho で Azure AD のシングル サインオンを構成してテストするには、次の構成要素を完了する必要があります。

1. **[Azure AD シングル サインオンを構成する](#configuring-azure-ad-single-single-sign-on)** - この機能を使用するユーザーをできるようにします。
2. **[Azure AD テスト ユーザーを作成する](#creating-an-azure-ad-test-user)** - を Azure AD シングル サインオン Britta Simon でテストします。
4. **[UserEcho テスト ユーザーを作成する](#creating-a-userecho-test-user)** - 自分の Azure AD の表現にリンクされている UserEcho に Britta Simon に対応します。
5. **[Azure AD テスト ユーザーの割り当て](#assigning-the-azure-ad-test-user)** - Azure AD シングル サインオンを使用する Britta Simon をできるようにします。
5. **[シングル サインオン テスト](#testing-single-sign-on)** - 構成が機能するかどうかを確認します。

### Azure AD シングル サインオンの構成

このセクションの目的は、Azure クラシック ポータルで Azure AD のシングル サインオンを有効にすることと、UserEcho アプリケーションでシングル サインオンを構成することです。 




**UserEcho で Azure AD シングル サインオンを構成するには、次の手順に従います。**

1. Azure クラシック ポータルでの **UserEcho** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、 **シングル サインオンを構成する**  ダイアログ。
<br><br> ![シングル サインオンを構成します。][6] <br>

2.  **どのような UserEcho にサインオン** ] ページで、[ **Azure AD シングル サインオン**, 、順にクリック **次**します。
<br><br> ![シングル サインオンを構成します。](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_03.png) <br>

3.  **アプリケーション設定の構成** ] ダイアログ ページで、次の手順を実行します。 します。
<br><br>![シングル サインオンを構成します。](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_04.png) <br>

    a.  **サインオン URL** UserEcho アプリケーションにサインオンする URL は、ユーザーが使用] ボックスに、種類 (例:: *https://fabrikam.UserEcho.com/*)。

    b. クリックして **次**します。
 
 
4.  **UserEcho でのシングル サインオンの構成** ] ページで、次の手順を実行します。
<br><br>![シングル サインオンを構成します。](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_05.png) <br>

    a. クリックして **証明書のダウンロード**, 、お使いのコンピューター上のファイルを保存します。

    b. クリックして **次**します。


1. 別の Web ブラウザー ウィンドウで、管理者として UserEcho 企業サイトにサインオンします。

1. 上部にあるツールバーで、メニューを展開するユーザー名をクリックし、クリックして **セットアップ**します。
<br><br>![シングル サインオンを構成します。](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_06.png) <br>

1. クリックして **統合**します。
<br><br>![シングル サインオンを構成します。](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_07.png) <br>


1. クリックして **web サイト**, 、] をクリックし、 **シングル サインオン (SAML2)**します。
<br><br>![シングル サインオンを構成します。](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_08.png) <br>


1.  **シングル サインオン (SAML)** ] ページで、次の手順を実行します。
<br><br>![シングル サインオンを構成します。](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_09.png) <br>

    a. として **SAML が有効な**, [ **はい**します。 

    b. Azure クラシック ポータルでの **UserEcho でのシングル サインオンの構成** ] ダイアログ ページでコピー、 **シングル サインオン サービス URL** 値に設定して、メモ帳で、itOpen キーをダウンロードした証明書貼り付け、コンテンツをコピーおよび貼り付けます、 **SAML SSO URL** ] ボックスに貼り付けます。 

    c. Azure クラシック ポータルでの **UserEcho でのシングル サインオンの構成** ] ダイアログ ページで、コピー、 **リモート ログアウト URL** 値に設定して、貼り付けます、 **リモート logoout URL** ] ボックスに貼り付けます。 

    d. ダウンロードした証明書をメモ帳で開き、コンテンツをコピーして貼り付けます、 **X.509 証明書** ] ボックスに貼り付けます。    

    e. クリックして **保存**します。


6. Azure クラシック ポータルで、シングル サインオンの構成情報を選択し、クリックして **次**します。 
<br><br>![Azure AD シングル サインオン][10]<br>

7.  **シングル サインオンによる確認** ] ページで [ **完了**します。  
  <br><br>![Azure AD シングル サインオン][11]




### Azure AD のテスト ユーザーの作成
このセクションの目的は、Azure クラシック ポータルで Britta Simon というテスト ユーザーを作成することです。<br>
ユーザーの一覧で選択 **Britta Simon**します。<br><br>![Azure AD ユーザーを作成します。][20]<br>

**Azure AD でテスト ユーザーを作成するには、次の手順に従います。**

1.  **Azure クラシック ポータル**, 、左側のナビゲーション ウィンドウで、次のようにクリックします。 **Active Directory**します。
<br><br>![Azure AD テスト ユーザーを作成します。](./media/active-directory-saas-userecho-tutorial/create_aaduser_09.png) <br> 

2.  **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3. 上部のメニューで、ユーザーの一覧を表示する] をクリックして **ユーザー**します。
<br><br> ![Azure AD テスト ユーザーを作成します。](./media/active-directory-saas-userecho-tutorial/create_aaduser_03.png) <br>
 
4. 開くには、 **ユーザーの追加** ] ダイアログ ボックスで、下部にあるツールバーをクリックして **ユーザーの追加**します。 
<br><br> ![Azure AD テスト ユーザーを作成します。](./media/active-directory-saas-userecho-tutorial/create_aaduser_04.png) <br>

5.  **このユーザーについてお聞かせ** ] ダイアログ ページで、次の手順を実行します。 
<br><br> ![Azure AD テスト ユーザーを作成します。](./media/active-directory-saas-userecho-tutorial/create_aaduser_05.png) <br> 

    a. [ユーザーの種類] として [組織内の新しいユーザー] を選択します。

    b. [ユーザー名 **textbox**, 、型 **BrittaSimon**します。

    c. クリックして **次**します。

6.   **ユーザー プロファイル** ] ダイアログ ページで、次の手順を実行します。 
<br><br>![Azure AD テスト ユーザーを作成します。](./media/active-directory-saas-userecho-tutorial/create_aaduser_06.png) <br>
 
    a.  **名** ] ボックスに「 **Britta**します。  

    b.  **姓** ] ボックスに、型、 **Simon**します。

    c.  **表示名** ] ボックスに「 **Britta Simon**します。

    d.  **ロール** 一覧で、[ **ユーザー**します。
    e. クリックして **次**します。

7.  **一時パスワードの取得** ダイアログ ページで、をクリックして **作成**します。
<br><br> ![Azure AD テスト ユーザーを作成します。](./media/active-directory-saas-userecho-tutorial/create_aaduser_07.png) <br>
 
8.  **一時パスワードの取得** ] ダイアログ ページで、次の手順を実行します。
<br><br>![Azure AD テスト ユーザーを作成します。](./media/active-directory-saas-userecho-tutorial/create_aaduser_08.png) <br>
  
    a. 値を書き留めて、 **新しいパスワード**します。

    b. クリックして **完了**します。   

  
 
### UserEcho のテスト ユーザーの作成

このセクションの目的は、UserEcho で Britta Simon というユーザーを作成することです。

**UserEcho で Britta Simon というユーザーを作成するには、次の手順に従います。**

1. UserEcho 企業サイトに管理者としてサインオンします。

1. 上部にあるツールバーで、メニューを展開するユーザー名をクリックし、クリックして **セットアップ**します。
<br><br>![シングル サインオンを構成します。](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_06.png) <br>

1. クリックして **ユーザー**, を拡大する、 **ユーザー** セクションです。
<br><br>![シングル サインオンを構成します。](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_10.png) <br>

1. クリックして **ユーザー**します。
<br><br>![シングル サインオンを構成します。](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_11.png) <br>

1. クリックして **新しいユーザーを招待**します。
<br><br>![シングル サインオンを構成します。](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_12.png) <br>


1.  **新しいユーザーを招待** ] ダイアログ ボックスで、次の手順を実行します。
<br><br>![シングル サインオンを構成します。](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_13.png) <br>

    a.  **名前** ] ボックスに「 **Britta Simon**します。

    b.  **電子メール** ] ボックスに、Azure 旧ポータルで Britta の電子メール アドレスを入力します。

    c. クリックして **招待**します。

Britta に招待状が送信され、UserEcho の使用を開始できるようになります。 



### Azure AD テスト ユーザーの割り当て

このセクションでは、Britta Simon UserEcho に自分のアクセスを与えることで Azure シングル サインオンの使用を有効にするとします。
<br><br>![ユーザーの割り当て][200] <br>

**UserEcho に Britta Simon を割り当てるには、次の手順に従います。**

1. Azure クラシック ポータルでは、ビューを開くアプリケーション、ディレクトリ ビューで、クリックして **アプリケーション** 上部のメニュー。
<br><br>![ユーザーを割り当てる][201] <br>

2. アプリケーションの一覧で選択 **UserEcho**します。
<br><br>![シングル サインオンを構成します。](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_50.png) <br>

1. 上部にあるメニュー [ **ユーザー**します。
<br><br>![ユーザーを割り当てる][203] <br>

1. ユーザーの一覧で選択 **Britta Simon**します。

2. 下部にあるツールバー [ **割り当てる**します。
<br><br>![ユーザーを割り当てる][205]



### シングル サインオンのテスト

このセクションでは、構成のテスト Azure AD シングル サインオン アクセス パネルを使用します。<br>
アクセス パネルで UserEcho のタイルをクリックすると、自動的に UserEcho アプリケーションにサインオンします。


## その他のリソース

* [SaaS アプリと Azure Active Directory を統合する方法に関するチュートリアルの一覧](active-directory-saas-tutorial-list.md)
* [Azure Active Directory のアプリケーション アクセスとシングル サインオンとは](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_205.png








