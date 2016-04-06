<properties
    pageTitle="チュートリアル: Azure Active Directory と CS Stars の統合 | Microsoft Azure"
    description="Azure Active Directory と CS Stars の間でシングル サインオンを構成する方法について説明します。"
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
    ms.date="10/06/2015"
    ms.author="markusvi"/>


# チュートリアル: Azure Active Directory と CS Stars の統合

このチュートリアルでは、Azure Active Directory (Azure AD) と CS 星を統合する方法を説明します。<br>Azure AD CS の星の統合は、次の利点があります。 

- CS Stars にアクセスする Azure AD ユーザーを制御できます。 
- ユーザーが自分の Azure AD アカウントで自動的に CS Stars にサインオン (シングル サインオン) できるようにします。
- 1 つの中央サイト (Azure Active Directory ポータル) でアカウントを管理できます。

Azure AD と SaaS アプリの統合に関する詳細を確認するを参照して [アプリケーションへのアクセスと Azure Active Directory でのシングル サインオン](active-directory-appssoaccess-whatis.md)します。

## 前提条件 

CS Stars と Azure AD の統合を構成するには、次のものが必要です。

- Azure AD サブスクリプション
- CS Stars でのシングル サインオンが有効なサブスクリプション


> [AZURE.NOTE] このチュートリアルでは、手順をテストするには、実稼働環境の使用をおしないでいます。


このチュートリアルの手順をテストするには、次の推奨事項に従ってください。

- 必要な場合を除き、運用環境は使用しないでください。
- Azure AD の評価環境を取得していない場合は、1 か月の試用版を入手できます [ここ](https://azure.microsoft.com/pricing/free-trial/)します。 

 
## シナリオの説明
このチュートリアルの目的は、テスト環境で Azure AD のシングル サインオンをテストできるようにすることです。 <br>
このチュートリアルで紹介するシナリオは、次の 3 つの主な構成要素で構成されます。

1. ギャラリーからの CS Stars の追加 
2. Azure AD シングル サインオンの構成とテスト


## ギャラリーからの CS Stars の追加
Azure AD への CS Stars の統合を構成するには、ギャラリーから管理対象 SaaS アプリの一覧に CS Stars を追加する必要があります。

**ギャラリーから CS Stars を追加するには、次の手順に従います。**

1.  **Azure 管理ポータル**, 、左側のナビゲーション ウィンドウで、次のようにクリックします。 **Active Directory**します。 
<br><br>![Active Directory][1]<br>

2.  **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3. ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。
<br><br>![アプリケーション][2]<br>

4. クリックして **追加** ページの下部にあります。
<br><br>![アプリケーション][3]<br>

5.  **実行する操作** ダイアログ ボックスで、をクリックして **ギャラリーからアプリケーションを追加**します。
<br><br>![アプリケーション][4]<br>

6. 検索ボックスに入力 **CS 星**します。
<br><br>![アプリケーション][5]<br>

7. 結果ウィンドウで [ **CS 星**, 、] をクリックし、 **完了** アプリケーションを追加します。
<br><br>![アプリケーション][400]<br>



##  Azure AD シングル サインオンの構成とテスト
このセクションの目的は、"Britta Simon" というテスト ユーザーに基づいて、CS Stars で Azure AD のシングル サインオンを構成し、テストする方法について説明することです。

シングル サインオンを機能させるには、Azure AD ユーザーに対応する CS Stars ユーザーが Azure AD で認識されている必要があります。 つまり、Azure AD のユーザーと関連ユーザー CS 星の間のリンクの関係を確立する必要があります。<br>
値を割り当てることでこのリンクの関係が確立される、 **ユーザー名** の値として Azure AD で、 **Username** CS 個の星でします。
 
CS Stars で Azure AD のシングル サインオンを構成してテストするには、次の構成要素を完了する必要があります。

1. **[Azure AD シングル サインオンを構成する](#configuring-azure-ad-single-single-sign-on)** - この機能を使用するユーザーをできるようにします。
2. **[Azure AD テスト ユーザーを作成する](#creating-an-azure-ad-test-user)** - を Azure AD シングル サインオン Britta Simon でテストします。
4. **[CS 星テスト ユーザーを作成する](#creating-a-cs-stars-test-user)** - 自分の Azure AD の表現にリンクされている CS 星に Britta Simon に対応します。
5. **[Azure AD テスト ユーザーの割り当て](#assigning-the-azure-ad-test-user)** - Azure AD シングル サインオンを使用する Britta Simon をできるようにします。
5. **[シングル サインオン テスト](#testing-single-sign-on)** - 構成が機能するかどうかを確認します。

### Azure AD シングル サインオンの構成

このセクションでは、Azure AD シングル サインオン Azure AD ポータルで有効にし、CS 星アプリケーションでシングル サインオンを構成するにです。<br>

**で Azure AD シングル サインオンを構成するには、次の手順に従います。**

1. Azure AD ポータルでの **CS 星** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、 **シングル サインオンを構成する**  ダイアログ。
<br><br>![シングル サインオンを構成します。][6]<br>

2.  **どのような CS 星サインオン** ] ページで、[ **Azure AD シングル サインオン**, 、順にクリック **次**します。
<br><br>![Azure AD シングル サインオン][7]<br>

3.  **アプリケーション設定の構成** ] ダイアログ ページで、次の手順を実行します。 
<br><br>![アプリ設定を構成します。][8]<br>
 
     3.1 で、 **サインオン URL** ] ボックスに、型に使用する URL、ユーザーが CS 星アプリケーションにサインオン (例:: *https://uat.csstars.com/enterprise/default.cmdx?ssoclient=C234UAT2*)。

     > [AZURE.NOTE] 適切な値は、何がわからない場合は、沼 ClearSight 担当者に問い合わせてください。

     3.2. クリックして **次**します。
 
4.  **CS 個の星でのシングル サインオンの構成** ] ページで [ **メタデータのダウンロード**, 、コンピューターのローカルにメタデータ ファイルを保存します。
<br><br>![Azure AD Connect とは][9]<br>

5. CS Stars に対してシングル サインオンを有効にするには、Marsh ClearSight 担当者に問い合わせて、メタデータ ファイルを渡してください。


6. Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **次**します。 
<br><br>![Azure AD Connect とは][10]<br>

7.  **シングル サインオンによる確認** ] ページで [ **完了**します。  
<br><br>![Azure AD Connect とは][11]<br>




### Azure AD のテスト ユーザーの作成
このセクションの目的は、Azure ポータルで Britta Simon というテスト ユーザーを作成することです。<br>
ユーザーの一覧で選択 **Britta Simon**します。
<br><br>![Azure AD ユーザーを作成します。][20]<br>

**Azure AD でテスト ユーザーを作成するには、次の手順に従います。**

1.  **Azure 管理ポータル**, 、左側のナビゲーション ウィンドウで、次のようにクリックします。 **Active Directory**します。
<br><br>![Azure AD テスト ユーザーを作成します。](./media/active-directory-saas-amazon-web-service/create_aaduser_02.png)<br> 

2.  **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3. 上部のメニューで、ユーザーの一覧を表示する] をクリックして **ユーザー**します。
<br><br>![Azure AD テスト ユーザーを作成します。](./media/active-directory-saas-amazon-web-service/create_aaduser_03.png)<br> 
 
4. 開くには、 **ユーザーの追加** ] ダイアログ ボックスで、下部にあるツールバーをクリックして **ユーザーの追加**します。 
<br><br>![Azure AD テスト ユーザーを作成します。](./media/active-directory-saas-amazon-web-service/create_aaduser_04.png)<br> 

5.  **このユーザーについてお聞かせ** ] ダイアログ ページで、次の手順を実行します。 
<br><br>![Azure AD テスト ユーザーを作成します。](./media/active-directory-saas-amazon-web-service/create_aaduser_05.png)<br> 
  1. [ユーザーの種類] として [組織内の新しいユーザー] を選択します。
  2. [ユーザー名 **textbox**, 、型 **BrittaSimon**します。
  3. [次へ] をクリックします。

6.   **ユーザー プロファイル** ] ダイアログ ページで、次の手順を実行します。 
<br><br>![Azure AD テスト ユーザーを作成します。](./media/active-directory-saas-amazon-web-service/create_aaduser_06.png)<br> 
  1.  **名** ] ボックスに「 **Britta**します。  
  2.  **姓** ボックス型 **Simon**します。
  3.  **表示名** ] ボックスに「 **Britta Simon**します。
  4.  **ロール** 一覧で、[ **ユーザー**します。
  5. クリックして **次**します。

7.  **一時パスワードの取得** ダイアログ ページで、をクリックして **作成**します。
<br><br>![Azure AD テスト ユーザーを作成します。](./media/active-directory-saas-amazon-web-service/create_aaduser_07.png)<br> 
 
8.  **一時パスワードの取得** ] ダイアログ ページで、次の手順を実行します。
<br><br>![Azure AD テスト ユーザーを作成します。](./media/active-directory-saas-amazon-web-service/create_aaduser_08.png)<br> 
  1. 値を書き留めて、 **新しいパスワード**します。
  2. クリックして **完了**します。   

  
 
### CS Stars テスト ユーザーの作成

このセクションの目的は、CS Stars で Britta Simon というユーザーを作成することです。

**CS Stars で Britta Simon というユーザーを作成するには、次の手順に従います。**

1. Marsh ClearSight 担当者にお問い合わせください。


### Azure AD テスト ユーザーの割り当て

このセクションでは、CS つ星のアクセスを付与することによって Azure シングル サインオンを使用する Britta Simon を有効にするとします。
<br><br>![ユーザーの割り当て][200]<br>

**CS Stars に Britta Simon を割り当てるには、次の手順に従います。**

1. Azure ポータルで、ディレクトリ ビューでアプリケーション ビューを開く] をクリックして **アプリケーション** 上部のメニュー。
<br><br>![ユーザーを割り当てる][201]<br>
2. アプリケーションの一覧で選択 **CS 星**します。
<br><br>![ユーザーを割り当てる][202]<br>
1. 上部にあるメニュー [ **ユーザー**します。<br>
<br><br>![ユーザーを割り当てる][203]<br>
1. ユーザーの一覧で選択 **Britta Simon**します。

2. 下部にあるツールバー [ **割り当てる**します。
<br><br>![ユーザーを割り当てる][205]<br>



### シングル サインオンのテスト

このセクションでは、構成のテスト Azure AD シングル サインオン アクセス パネルを使用します。<br>
アクセス パネルで [CS Stars] タイルをクリックすると、自動的に CS Stars アプリケーションにサインオンします。


## その他のリソース

* [SaaS アプリと Azure Active Directory を統合する方法に関するチュートリアルの一覧](active-directory-saas-tutorial-list.md)
* [Azure Active Directory のアプリケーション アクセスとシングル サインオンとは](active-directory-appssoaccess-whatis.md)

<!--Image references-->
[1]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_csstars_01.png
[6]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_csstars_02.png
[7]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_csstars_03.png
[8]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_csstars_04.png
[9]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_csstars_05.png
[10]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_csstars_06.png
[11]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_csstars_07.png
[20]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_csstars_202.png
[203]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_general_205.png

[400]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_csstars_403.png




