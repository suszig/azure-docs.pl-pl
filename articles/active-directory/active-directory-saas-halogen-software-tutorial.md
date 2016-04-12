<properties
    pageTitle="チュートリアル: Azure Active Directory と Halogen Software の統合"
    description="Azure Active Directory と Halogen Software の間でシングル サインオンを構成する方法について説明します。"
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="stevenpo"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/01/2015"
    ms.author="markusvi"/>


# チュートリアル: Azure Active Directory と Halogen Software の統合

このチュートリアルでは、Azure Active Directory (Azure AD) と Halogen Software を統合する方法を説明します。<br>Azure AD と Halogen Software の統合は、次の利点があります。 

- Halogen Software にアクセスする Azure AD ユーザーを制御できます。 
- ユーザーが自分の Azure AD アカウントで自動的に Halogen Software にサインオン (シングル サインオン) できるようにします。
- 1 つの中央サイト (Azure Active Directory ポータル) でアカウントを管理できます。

Azure AD と SaaS アプリの統合に関する詳細を確認するを参照して [アプリケーションへのアクセスと Azure Active Directory でのシングル サインオン](active-directory-appssoaccess-whatis.md)します。

## 前提条件 

Halogen Software と Azure AD の統合を構成するには、次のものが必要です。

- Azure AD サブスクリプション
- シングル サインオン対応の Halogen Software サブスクリプション


> [AZURE.NOTE] このチュートリアルでは、手順をテストするには、実稼働環境の使用をおしないでいます。


このチュートリアルの手順をテストするには、次の推奨事項に従ってください。

- 必要な場合を除き、運用環境は使用しないでください。
- Azure AD の評価環境を取得していない場合は、1 か月の試用版を入手できます [ここ](https://azure.microsoft.com/pricing/free-trial/)します。 

 
## シナリオの説明
このチュートリアルの目的は、テスト環境で Azure AD のシングル サインオンをテストできるようにすることです。 <br>
このチュートリアルで紹介するシナリオは、2 つの主な構成要素で構成されます。

1. ギャラリーからの Halogen Software の追加 
2. Azure AD シングル サインオンの構成とテスト


## ギャラリーからの Halogen Software の追加
Azure AD への Halogen Software の統合を構成するには、ギャラリーから管理対象 SaaS アプリの一覧に Halogen Software を追加する必要があります。

**ギャラリーから Halogen Software を追加するには、次の手順に従います。**

1.  **Azure 管理ポータル**, 、左側のナビゲーション ウィンドウで、次のようにクリックします。 **Active Directory**します。 <br><br>
![Active Directory][1]

2.  **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3. ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。<br><br>
![アプリケーション][2]
4. クリックして **追加** ページの下部にあります。<br><br>
![アプリケーション][3]
5.  **実行する操作** ダイアログ ボックスで、をクリックして **ギャラリーからアプリケーションを追加**します。<br><br>
![アプリケーション][4]
6. 検索ボックスに入力 **halogen ソフトウェア**します。<br>
![アプリケーション][5]
7. 結果ウィンドウで [ **Halogen Software**, 、クリックして **完了** アプリケーションを追加します。<br>



##  Azure AD シングル サインオンの構成とテスト
このセクションの目的は、"Britta Simon" というテスト ユーザーに基づいて、Halogen Software で Azure AD のシングル サインオンを構成し、テストする方法について説明することです。

シングル サインオンを機能させるには、Azure AD ユーザーに対応する Halogen Software ユーザーが Azure AD で認識されている必要があります。 つまり、Azure AD のユーザーと Halogen Software で関連するユーザーの間のリンクの関係を確立する必要があります。<br>
値を割り当てることでこのリンクの関係が確立される、 **ユーザー名** の値として Azure AD で、 **Username** Halogen Software でします。
 
Halogen Software で Azure AD のシングル サインオンを構成し、テストするには、次の要素を完了する必要があります。

1. **[Azure AD シングル シングル サインオンを構成する](#configuring-azure-ad-single-single-sign-on)** - この機能を使用するユーザーをできるようにします。
2. **[Azure AD テスト ユーザーを作成する](#creating-an-azure-ad-test-user)** - を Azure AD シングル サインオン Britta Simon でテストします。
4. **[Halogen Software のテスト ユーザーを作成する](#creating-a-halogen-software-test-user)** - して、自分の Azure AD の表現にリンクされている Halogen Software で Britta Simon に対応します。
5. **[Azure AD テスト ユーザーの割り当て](#assigning-the-azure-ad-test-user)** - Azure AD シングル サインオンを使用する Britta Simon をできるようにします。
5. **[シングル サインオン テスト](#testing-single-sign-on)** - 構成が機能するかどうかを確認します。

### 単一の Azure AD シングル サインオンの構成

このセクションでは、Azure AD シングル サインオン Azure AD ポータルで有効にして、Halogen Software アプリケーションにシングル サインオンを構成するにです。<br>

**Halogen Software で Azure AD シングル サインオンを構成するには、次の手順に従います。**

1. Azure AD ポータルでの **Halogen Software** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、 **シングル サインオンを構成する**  ダイアログ。<br><br>
![シングル サインオンを構成します。][8]

2.  **どのようなユーザー Halogen Software にサインオンする** ] ページで、[ **Azure AD シングル サインオン**, 、] をクリックし、 **次**します。<br><br>
![Azure AD シングル サインオン][9]

3.  **アプリケーション設定の構成** ] ダイアログ ページで、次の手順を実行します。 <br><br>![アプリ設定を構成します。][10]
 
     3.1 で、 **サインオン URL** ] ボックスに、型に使用する URL、ユーザーが次のパターンを使用して、Halogen Software アプリケーションにサインオン: *https://global.hgncloud.com/fabrikam/welcome.jsp*

     3.2. クリックして **次**します。
 
4.  **Halogen Software でのシングル サインオンの構成** ] ページで [ **メタデータのダウンロード**, 、コンピューターのローカルにメタデータ ファイルを保存します。<br><br>![Azure AD Connect とは][11]

5. 別のブラウザーのウィンドウでへサインオン、 **Halogen Software** 管理者としてアプリケーションです。
6. クリックして、 **オプション** ] タブをクリックします。 <br><br>![Azure AD Connect とは][12]
7. 左側のナビゲーション ウィンドウで、クリックして **SAML Configuration**します。 <br><br>![Azure AD Connect とは][13]
8.  **SAML Configuration** ] ページで、次の手順を実行します。 <br><br>![Azure AD Connect とは][14]

     8.1. として **一意識別子**, [ **NameID**します。

     8.2. として **Unique Identifier Maps To**, [ **Username**します。

     8.3. ダウンロードしたメタデータ ファイルをアップロードする] をクリックして **参照** 、ファイルを選択し、 **ファイルのアップロード**します。

     8.4. 構成をテストするにはクリックして **テストの実行**します。 
     
     > [AZURE.NOTE] メッセージを待機する必要があります"*SAML テストが完了します。Please close this window*" というメッセージが表示されるまで待機する必要があります。 次に、開いているブラウザー ウィンドウを閉じます。 <br>  **Enable SAML** ] チェック ボックスは、テストが完了している場合のみ有効です。

     8.5. 選択 **Enable SAML**します。
    
     8.6 クリックして **変更を保存**します。 


9. Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。 <br><br>![Azure AD Connect とは][15]
10.  **シングル サインオンによる確認** ] ページで [ **完了**します。  <br><br>![Azure AD Connect とは][16]




### Azure AD のテスト ユーザーの作成
このセクションの目的は、Azure ポータルで Britta Simon というテスト ユーザーを作成することです。

**Azure AD でテスト ユーザーを作成するには、次の手順に従います。**

1.  **Azure 管理ポータル**, 、左側のナビゲーション ウィンドウで、次のようにクリックします。 **Active Directory**します。
<br><br>![Azure AD Connect とは][100] 
2.  **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。
3. 上部のメニューで、ユーザーの一覧を表示する] をクリックして **ユーザー**します。
<br><br>![Azure AD Connect とは][101] 
4. 開くには、 **ユーザーの追加** ] ダイアログ ボックスで、下部にあるツールバーをクリックして **ユーザーの追加**します。 
<br><br>![Azure AD Connect とは][102] 
5.  **このユーザーについてお聞かせ** ] ダイアログ ページで、次の手順を実行します。
<br><br>![Azure AD Connect とは][103] 
  1. として **Type Of User**, [ **、組織内の新しいユーザー**します。
  2. [ユーザー名 **textbox**, 、型 **BrittaSimon**します。
  3. [次へ] をクリックします。
6.   **ユーザー プロファイル** ] ダイアログ ページで、次の手順を実行します。 
<br><br>![Azure AD Connect とは][104] 
  1.  **名** ] ボックスに「 **Britta**します。  
  2.  **姓** ボックス型 **Simon**します。
  3.  **表示名** ] ボックスに「 **Britta Simon**します。
  4.  **ロール** 一覧で、[ **ユーザー**します。
  5. クリックして **次**します。
7.  **一時パスワードの取得** ダイアログ ページで、をクリックして **作成**します。
<br><br>![Azure AD Connect とは][105]  
8.  **一時パスワードの取得** ] ダイアログ ページで、次の手順を実行します。
<br><br>![Azure AD Connect とは][106]   
  1. 値を書き留めて、 **新しいパスワード**します。
  2. クリックして **完了**します。   
  
 
### Halogen Software のテスト ユーザーの作成

このセクションの目的は、Halogen Software で Britta Simon というユーザーを作成することです。

**Halogen Software で Britta Simon というユーザーを作成するには、次の手順に従います。**

1. サインイン、 **Halogen Software** 管理者としてアプリケーションです。
2. クリックして、 **ユーザー センター** タブをクリックし、をクリックして **Create User**します。
<br><br>![Azure AD Connect とは][300]  
3.  **新しいユーザー** ] ダイアログ ページで、次の手順を実行します。
<br><br>![Azure AD Connect とは][301]
  1.  **名** ] ボックスに「 **Britta**します。 
  2.  **姓** ] ボックスに「 **Simon**します。
  3.  **Username** ] ボックスに「 **Azure AD ポータルでの Brita Simon のユーザー名**します。
  4.  **パスワード** ] ボックスに Britta のパスワードを入力します。
  5. クリックして **保存**します。


### Azure AD テスト ユーザーの割り当て

このセクションでは、Britta Simon に Halogen Software へのアクセスを付与することで Azure シングル サインオンを使用して有効にするとします。
<br><br>![Azure AD Connect の概要][200]

**Halogen Software に Britta Simon を割り当てるには、次の手順に従います。**

1. Azure ポータルで、ディレクトリ ビューでアプリケーション ビューを開く] をクリックして **アプリケーション** 上部のメニュー。<br>
<br><br>![Azure AD Connect とは][201]
2. アプリケーションの一覧で選択 **Halogen Software**します。
<br><br>![Azure AD Connect とは][202]
1. 上部にあるメニュー [ **ユーザー**します。<br>
<br><br>![Azure AD Connect とは][203]
1. ユーザーの一覧で選択 **Britta Simon**します。
<br><br>![Azure AD Connect とは][204]
2. 下部にあるツールバー [ **割り当てる**します。
<br><br>![Azure AD Connect とは][205]



### シングル サインオンのテスト

このセクションでは、構成のテスト Azure AD シングル サインオン アクセス パネルを使用します。<br>
アクセス パネルで Halogen Software のタイルをクリックすると、自動的に Halogen Software アプリケーションにサインオンします。


## その他のリソース

* [SaaS アプリと Azure Active Directory を統合する方法に関するチュートリアルの一覧](active-directory-saas-tutorial-list.md)
* [Azure Active Directory のアプリケーション アクセスとシングル サインオンとは](active-directory-appssoaccess-whatis.md)

<!--Image references-->
[1]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_01.png
[2]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_02.png
[3]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_03.png
[4]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_04.png
[5]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_05.png
[6]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_06.png
[7]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_07.png
[8]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_08.png
[9]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_09.png
[10]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_10.png
[11]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_11.png
[12]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_12.png
[13]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_13.png
[14]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_14.png
[15]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_15.png
[16]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_16.png
[100]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_100.png 
[101]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_101.png 
[102]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_102.png 
[103]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_103.png 
[104]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_104.png 
[105]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_105.png 
[106]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_106.png 
[200]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_200.png 
[201]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_201.png 
[202]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_202.png
[203]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_203.png
[204]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_204.png
[205]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_205.png
[300]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_300.png
[301]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_301.png

