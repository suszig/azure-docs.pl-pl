<properties
    pageTitle="チュートリアル: Azure Active Directory と 23 Video の統合 | Microsoft Azure"
    description="Azure Active Directory と 23 Video の間でシングル サインオンを構成する方法について説明します。"
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


# チュートリアル: Azure Active Directory と 23 Video の統合

このチュートリアルでは、Azure Active Directory (Azure AD) と 23 のビデオを統合する方法を説明します。<br>23 を統合する Azure AD でのビデオは、次の利点があります。 

- 23 Video にアクセスする Azure AD ユーザーを制御できます。 
- ユーザーが自分の Azure AD アカウントで自動的に 23 Video にサインオン (シングル サインオン) できるようにします。

Azure AD と SaaS アプリの統合に関する詳細を確認するを参照して [アプリケーションへのアクセスと Azure Active Directory でのシングル サインオン](active-directory-appssoaccess-whatis.md)します。

## 前提条件 

23 Video と Azure AD の統合を構成するには、次のものが必要です。

- Azure AD サブスクリプション
- 23 Video でのシングル サインオンが有効なサブスクリプション


> [AZURE.NOTE] このチュートリアルでは、手順をテストするには、実稼働環境の使用をおしないでいます。


このチュートリアルの手順をテストするには、次の推奨事項に従ってください。

- 必要な場合を除き、運用環境は使用しないでください。
- Azure AD の評価環境を取得していない場合は、1 か月の試用版を入手できます [ここ](https://azure.microsoft.com/pricing/free-trial/)します。 

 
## シナリオの説明
このチュートリアルの目的は、テスト環境で Azure AD のシングル サインオンをテストできるようにすることです。 <br>
このチュートリアルで紹介するシナリオは、2 つの主な構成要素で構成されます。

1. ギャラリーからの 23 Video の追加 
2. Azure AD シングル サインオンの構成とテスト


## ギャラリーからの 23 Video の追加
Azure AD への 23 Video の統合を構成するには、ギャラリーから管理対象 SaaS アプリの一覧に 23 Video を追加する必要があります。

**ギャラリーから 23 Video を追加するには、次の手順に従います。**

1.  **Azure クラシック ポータル**, 、左側のナビゲーション ウィンドウで、次のようにクリックします。 **Active Directory**します。 <br><br>
![Active Directory][1]<br>

2.  **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3. ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。<br><br>
![アプリケーション][2]<br>
4. クリックして **追加** ページの下部にあります。<br><br>
![アプリケーション][3]<br>
5.  **実行する操作** ダイアログ ボックスで、をクリックして **ギャラリーからアプリケーションを追加**します。<br><br>
![アプリケーション][4]<br>
6. 検索ボックスに入力 **23 ビデオ**します。<br><br>
![アプリケーション][5]<br>
7. 結果ウィンドウで [ **23 ビデオ**, 、] をクリックし、 **完了** アプリケーションを追加します。
<br><br>![アプリケーション][25]<br>

##  Azure AD シングル サインオンの構成とテスト
このセクションの目的は、"Britta Simon" というテスト ユーザーに基づいて、23 Video で Azure AD のシングル サインオンを構成し、テストする方法について説明することです。

シングル サインオンを機能させるには、Azure AD ユーザーに対応する 23 Video ユーザーが Azure AD で認識されている必要があります。 つまり、Azure AD のユーザーと関連ユーザー 23 の間でリンクの関係がビデオ必要がありますを確立します。<br>
値を割り当てることでこのリンクの関係が確立される、 **ユーザー名** の値として Azure AD で、 **ユーザー名** 23 ビデオです。
 
23 Video で Azure AD のシングル サインオンを構成してテストするには、次の構成要素を完了する必要があります。

1. **[Azure AD シングル サインオンを構成する](#configuring-azure-ad-single-single-sign-on)** - この機能を使用するユーザーをできるようにします。
2. **[Azure AD テスト ユーザーを作成する](#creating-an-azure-ad-test-user)** - を Azure AD シングル サインオン Britta Simon でテストします。
4. **[23 ビデオ テスト ユーザーを作成する](#creating-a-23-video-test-user)** - して、自分の Azure AD の表現にリンクされているビデオが 23 で Britta Simon に対応します。
5. **[Azure AD テスト ユーザーの割り当て](#assigning-the-azure-ad-test-user)** - Azure AD シングル サインオンを使用する Britta Simon をできるようにします。
5. **[シングル サインオン テスト](#testing-single-sign-on)** - 構成が機能するかどうかを確認します。

### Azure AD シングル サインオンの構成

このセクションでは、Azure AD シングル サインオン Azure クラシック ポータルで有効にし、23 のビデオ アプリケーションでシングル サインオンを構成するにです。<br>

**23 Video で Azure AD シングル サインオンを構成するには、次の手順に従います。**

1. Azure クラシック ポータルでの **23 ビデオ** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、 **シングル サインオンを構成する**  ダイアログ。
<br><br> ![シングル サインオンを構成します。][6] <br>

2.  **どのようなユーザーの 23 ビデオに** ] ページで、[ **Azure AD シングル サインオン**, 、順にクリック **次**します。
<br><br> ![Azure AD シングル サインオン][7] <br>

3.  **アプリケーション設定の構成** ] ダイアログ ページで、次の手順を実行します。
<br><br>![Azure AD シングル サインオン][8] <br>
 
     a.  **応答 URL** 23 ビデオ サイトにサインオンする URL は、ユーザーが使用] ボックスに、種類 (例:: *https://britta-simon.23Video.com/saml/login*)。

     > [AZURE.NOTE] SAML 2.0 を使用して active Directory 統合は、23 のビデオ ユーザーをすべて使用できます。 サポートに問い合わせてください [support@23company.com](mailto:support@23company.com) 場合は、関連するメタデータを作成する必要があります。

     b. クリックして **次**します。
 
4.  **23 ビデオでのシングル サインオンの構成** ] ページで、次の手順を実行します。
<br><br>![Azure AD シングル サインオン][9] <br>

    a. [証明書のダウンロード] をクリックし、コンピューターにファイルを保存します。

    b. 使用して、23 のビデオ サポート チームにお問い合わせください [support@23company.com](mailto:support@23company.com), 、ダウンロードした証明書を説明する、 **発行者 URL**, 、 **シングル サインオン サービス URL**, 、 **シングル サインアウト URL**, 、23 ビデオ アプリケーションが SSO をセットアップすることを確認してください。 

    c. クリックして **次**します。


6. Azure クラシック ポータルで、シングル サインオンの構成情報を選択し、をクリックして **次**します。 
<br><br>![Azure AD シングル サインオン][10]<br>

7.  **シングル サインオンによる確認** ] ページで [ **完了**します。  
  <br><br>![Azure AD シングル サインオン][11]




### Azure AD のテスト ユーザーの作成
このセクションの目的は、Azure クラシック ポータルで Britta Simon というテスト ユーザーを作成することです。<br>
ユーザーの一覧で選択 **Britta Simon**します。<br><br>![Azure AD ユーザーを作成します。][20]<br>

**Azure AD でテスト ユーザーを作成するには、次の手順に従います。**

1.  **Azure クラシック ポータル**, 、左側のナビゲーション ウィンドウで、次のようにクリックします。 **Active Directory**します。
<br><br>![Azure AD テスト ユーザーを作成します。](./media/active-directory-saas-23video-tutorial/create_aaduser_09.png) <br> 

2.  **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3. 上部のメニューで、ユーザーの一覧を表示する] をクリックして **ユーザー**します。
<br><br> ![Azure AD テスト ユーザーを作成します。](./media/active-directory-saas-23video-tutorial/create_aaduser_03.png) <br>
 
4. 開くには、 **ユーザーの追加** ] ダイアログ ボックスで、下部にあるツールバーをクリックして **ユーザーの追加**します。 
<br><br> ![Azure AD テスト ユーザーを作成します。](./media/active-directory-saas-23video-tutorial/create_aaduser_04.png) <br>

5.  **このユーザーについてお聞かせ** ] ダイアログ ページで、次の手順を実行します。 
<br><br> ![Azure AD テスト ユーザーを作成します。](./media/active-directory-saas-23video-tutorial/create_aaduser_05.png) <br> 

    a. [ユーザーの種類] として [組織内の新しいユーザー] を選択します。

    b. [ユーザー名 **textbox**, 、型 **BrittaSimon**します。

    c. クリックして **次**します。

6.   **ユーザー プロファイル** ] ダイアログ ページで、次の手順を実行します。 
<br><br>![Azure AD テスト ユーザーを作成します。](./media/active-directory-saas-23video-tutorial/create_aaduser_06.png) <br>
 
    a.  **名** ] ボックスに「 **Britta**します。  

    b.  **姓** ] ボックスに、型、 **Simon**します。

    c.  **表示名** ] ボックスに「 **Britta Simon**します。

    d.  **ロール** 一覧で、[ **ユーザー**します。
    e. クリックして **次**します。

7.  **一時パスワードの取得** ダイアログ ページで、をクリックして **作成**します。
<br><br> ![Azure AD テスト ユーザーを作成します。](./media/active-directory-saas-23video-tutorial/create_aaduser_07.png) <br>
 
8.  **一時パスワードの取得** ] ダイアログ ページで、次の手順を実行します。
<br><br>![Azure AD テスト ユーザーを作成します。](./media/active-directory-saas-23video-tutorial/create_aaduser_08.png) <br>
  
    a. 値を書き留めて、 **新しいパスワード**します。

    b. クリックして **完了**します。   

  
 
### 23 Video テスト ユーザーの作成

このセクションの目的は、23 Video で Britta Simon というユーザーを作成することです。

**23 Video で Britta Simon というユーザーを作成するには、次の手順に従います。**

1. 23 Video 企業サイトに管理者としてサインオンします。

1. 移動して **設定**します。


2.  **ユーザー** ] をクリックして **構成**します。
<br><br>![ユーザーを割り当てる][400]<br>

3. クリックして **新しいユーザーを追加**します。 
<br><br>![ユーザーを割り当てる][401]<br>

4.  **このサイトへの参加を招待** セクションで、次の手順に従います。
<br><br>![ユーザーを割り当てる][402]<br>

    a.  **電子メール アドレス** ] ボックスに、Azure AD の Britta Simon の電子メール アドレスを入力します。

    b. クリックして **ユーザーを追加**します。   


### Azure AD テスト ユーザーの割り当て

このセクションでは、23 ビデオへのアクセスを付与することで Azure シングル サインオンを使用する Britta Simon を有効にするとします。
<br><br>![ユーザーの割り当て][200] <br>

**23 Video に Britta Simon を割り当てるには、次の手順に従います。**

1. Azure クラシック ポータルでは、ビューを開くアプリケーション、ディレクトリ ビューで、クリックして **アプリケーション** 上部のメニュー。
<br><br>![ユーザーを割り当てる][201] <br>
2. アプリケーションの一覧で選択 **23 ビデオ**します。
<br><br>![ユーザーを割り当てる][202] <br>
1. 上部にあるメニュー [ **ユーザー**します。
<br><br>![ユーザーを割り当てる][203] <br>
1. ユーザーの一覧で選択 **Britta Simon**します。

2. 下部にあるツールバー [ **割り当てる**します。
<br><br>![ユーザーを割り当てる][205]



### シングル サインオンのテスト

このセクションでは、構成のテスト Azure AD シングル サインオン アクセス パネルを使用します。<br>
アクセス パネルで [23 Video] タイルをクリックすると、自動的に 23 Video アプリケーションにサインオンします。


## その他のリソース

* [SaaS アプリと Azure Active Directory を統合する方法に関するチュートリアルの一覧](active-directory-saas-tutorial-list.md)
* [Azure Active Directory のアプリケーション アクセスとシングル サインオンとは](active-directory-appssoaccess-whatis.md)


<!--Image references-->
[1]: ./media/active-directory-saas-23video-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-23video-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-23video-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-23video-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-23video-tutorial/tutorial_23video_01.png
[25]: ./media/active-directory-saas-23video-tutorial/tutorial_23video_02.png

[6]: ./media/active-directory-saas-23video-tutorial/tutorial_general_05.png
[7]: ./media/active-directory-saas-23video-tutorial/tutorial_23video_03.png
[8]: ./media/active-directory-saas-23video-tutorial/tutorial_23video_04.png
[9]: ./media/active-directory-saas-23video-tutorial/tutorial_23video_05.png
[10]: ./media/active-directory-saas-23video-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-23video-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-23video-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-23video-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-23video-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-23video-tutorial/tutorial_23video_07.png
[203]: ./media/active-directory-saas-23video-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-23video-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-23video-tutorial/tutorial_general_205.png

[400]: ./media/active-directory-saas-23video-tutorial/tutorial_23video_10.png
[401]: ./media/active-directory-saas-23video-tutorial/tutorial_23video_11.png
[402]: ./media/active-directory-saas-23video-tutorial/tutorial_23video_12.png






