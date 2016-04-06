<properties
    pageTitle="チュートリアル: Azure Active Directory と SciQuest Spend Director の統合 | Microsoft Azure"
    description="Azure Active Directory と SciQuest Spend Director の間でシングル サインオンを構成する方法について説明します。"
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
    ms.date="10/20/2015"
    ms.author="markusvi"/>


# チュートリアル: Azure Active Directory と SciQuest Spend Director の統合

このチュートリアルでは、Azure Active Directory (Azure AD) と SciQuest Spend Director を統合する方法を説明します。<br>Azure AD と SciQuest Spend Director の統合は、次の利点があります。 

- SciQuest Spend Director にアクセスする Azure AD ユーザーを制御できます。 
- ユーザーが自分の Azure AD アカウントで自動的に SciQuest Spend Director にサインオン (シングル サインオン) できるようにします。
- 1 つの中央サイト (Azure Active Directory ポータル) でアカウントを管理できます。

Azure AD と SaaS アプリの統合に関する詳細を確認するを参照して [アプリケーションへのアクセスと Azure Active Directory でのシングル サインオン](active-directory-appssoaccess-whatis.md)します。

## 前提条件 

SciQuest Spend Director と Azure AD の統合を構成するには、次のものが必要です。

- Azure AD サブスクリプション
- シングル サインオン対応の SciQuest Spend Director サブスクリプション


> [AZURE.NOTE] このチュートリアルでは、手順をテストするには、実稼働環境の使用をおしないでいます。


このチュートリアルの手順をテストするには、次の推奨事項に従ってください。

- 必要な場合を除き、運用環境は使用しないでください。
- Azure AD の評価環境を取得していない場合は、1 か月の試用版を入手できます [ここ](https://azure.microsoft.com/pricing/free-trial/)します。 

 
## シナリオの説明
このチュートリアルの目的は、テスト環境で Azure AD のシングル サインオンをテストできるようにすることです。 <br>
このチュートリアルで紹介するシナリオは、2 つの主な構成要素で構成されます。

1. ギャラリーからの SciQuest Spend Director の追加 
2. Azure AD シングル サインオンの構成とテスト


## ギャラリーからの SciQuest Spend Director の追加
Azure AD への SciQuest Spend Director の統合を構成するには、ギャラリーから管理対象 SaaS アプリの一覧に SciQuest Spend Director を追加する必要があります。

**ギャラリーから SciQuest Spend Director を追加するには、次の手順に従います。**

1.  **Azure 管理ポータル**, 、左側のナビゲーション ウィンドウで、次のようにクリックします。 **Active Directory**します。 <br><br>
![Active Directory][1]

2.  **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3. ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。<br><br>
![アプリケーション][2]
4. クリックして **追加** ページの下部にあります。<br><br>
![アプリケーション][3]
5.  **実行する操作** ダイアログ ボックスで、をクリックして **ギャラリーからアプリケーションを追加**します。<br><br>
![アプリケーション][4]
6. 検索ボックスに入力 **sciQuest spend director**します。<br>
![アプリケーション][5]
7. 結果ウィンドウで [ **SciQuest Spend Director**, 、クリックして **完了** アプリケーションを追加します。<br>
![アプリケーション][6]


##  Azure AD シングル サインオンの構成とテスト
このセクションの目的は、"Britta Simon" というテスト ユーザーに基づいて、SciQuest Spend Director で Azure AD のシングル サインオンを構成し、テストする方法について説明することです。

シングル サインオンを機能させるには、Azure AD ユーザーに対応する SciQuest Spend Director ユーザーが Azure AD で認識されている必要があります。 つまり、Azure AD のユーザーと SciQuest Spend Director の関連するユーザーの間のリンクの関係を確立する必要があります。<br>
値を割り当てることでこのリンクの関係が確立される、 **ユーザー名** の値として Azure AD で、 **Username** SciQuest Spend Director でします。
 
SciQuest Spend Director で Azure AD のシングル サインオンを構成し、テストするには、次の要素を完了する必要があります。

1. **[Azure AD シングル シングル サインオンを構成する](#configuring-azure-ad-single-single-sign-on)** - この機能を使用するユーザーをできるようにします。
2. **[Azure AD テスト ユーザーを作成する](#creating-an-azure-ad-test-user)** - を Azure AD シングル サインオン Britta Simon でテストします。
4. **[SciQuest Spend Director のテスト ユーザーを作成する](#creating-a-halogen-software-test-user)** - して、自分の Azure AD の表現にリンクされている SciQuest Spend Director で Britta Simon に対応します。
5. **[Azure AD テスト ユーザーの割り当て](#assigning-the-azure-ad-test-user)** - Azure AD シングル サインオンを使用する Britta Simon をできるようにします。
5. **[シングル サインオン テスト](#testing-single-sign-on)** - 構成が機能するかどうかを確認します。

### 単一の Azure AD シングル サインオンの構成

このセクションでは、Azure AD シングル サインオン Azure AD ポータルで有効にして、SciQuest Spend Director アプリケーションでシングル サインオンを構成するにです。<br>

**SciQuest Spend Director で Azure AD シングル サインオンを構成するには、次の手順に従います。**

1. Azure AD ポータルでの **SciQuest Spend Director** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、 **シングル サインオンを構成する**  ダイアログ。<br><br>
![シングル サインオンを構成します。][8]

2.  **どのような SciQuest Spend Director にサインオン** ] ページで、[ **Azure AD シングル サインオン**, 、] をクリックし、 **次**します。<br><br>
![Azure AD シングル サインオン][9]

3.  **アプリケーション設定の構成** ] ダイアログ ページで、次の手順を実行します。 <br><br>![アプリ設定を構成します。][10]
 
     3.1.  **サインオン URL** ] ボックスに、型に使用する URL、ユーザーが、次のパターンを使用して、SciQuest Spend Director アプリケーションへのサインオン: *https://*。sciquest.com/.**

     3.2.  **応答 URL** ] テキスト ボックスに入力した同じ値を入力、 **サインオン URL** ] ボックスに貼り付けます。 

     3.3 クリックして **次**します。
 
4.  **SciQuest Spend Director でのシングル サインオンの構成** ] ページで [ **メタデータのダウンロード**, 、コンピューターのローカルにメタデータ ファイルを保存します。<br><br>![Azure AD Connect とは][11]

5. 上記のダウンロードしたメタデータを使用してこの認証方法を有効にする方法については、SciQuest のサポートにお問い合わせください。

6. Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。 <br><br>![Azure AD Connect とは][15]
10.  **シングル サインオンによる確認** ] ページで [ **完了**します。  <br><br>![Azure AD Connect は][16]




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
  
 
### SciQuest Spend Director のテスト ユーザーの作成

このセクションの目的は、SciQuest Spend Director で Britta Simon というユーザーを作成することです。

テスト アカウントを作成するには、SciQuest Spend Director のサポート チームに連絡し、テスト アカウントの詳細を伝える必要があります。

または、SciQuest Spend Director でサポートされているシングル サインオン機能であるジャストインタイム プロビジョニングを使用することもできます。 <br>
ジャストイン タイム プロビジョニングを有効にすると、ユーザーは自動的に作成 SciQuest Spend Director でシングル サインオンの試行中に存在していない場合。 この機能により、対応するシングル サインオン ユーザーを手動で作成する必要がなくなります。

ジャストインタイム プロビジョニングを有効にするには、SciQuest Spend Director のサポート チームに連絡する必要があります。
  

### Azure AD テスト ユーザーの割り当て

このセクションでは、Britta Simon に SciQuest Spend Director にアクセスを付与することによって Azure シングル サインオンを使用して有効にするとします。
<br><br>![Azure AD Connect の概要][200]

**SciQuest Spend Director に Britta Simon を割り当てるには、次の手順に従います。**

1. Azure ポータルで、ディレクトリ ビューでアプリケーション ビューを開く] をクリックして **アプリケーション** 上部のメニュー。<br>
<br><br>![Azure AD Connect とは][201]
2. アプリケーションの一覧で選択 **SciQuest Spend Director**します。
<br><br>![Azure AD Connect とは][202]
1. 上部にあるメニュー [ **ユーザー**します。<br>
<br><br>![Azure AD Connect とは][203]
1. ユーザーの一覧で選択 **Britta Simon**します。
<br><br>![Azure AD Connect とは][204]
2. 下部にあるツールバー [ **割り当てる**します。
<br><br>![Azure AD Connect とは][205]



### シングル サインオンのテスト

このセクションでは、構成のテスト Azure AD シングル サインオン アクセス パネルを使用します。<br>
アクセス パネルで SciQuest Spend Director のタイルをクリックすると、自動的に SciQuest Spend Director アプリケーションにサインオンします。


## その他のリソース

* [SaaS アプリと Azure Active Directory を統合する方法に関するチュートリアルの一覧](active-directory-saas-tutorial-list.md)
* [Azure Active Directory のアプリケーション アクセスとシングル サインオンとは](active-directory-appssoaccess-whatis.md)

<!--Image references-->
[1]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_01.png
[2]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_02.png
[3]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_03.png
[4]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_04.png
[5]: ./media/active-directory-saas-sciquest-spend-director/tutorial_sciquest_spend_director_01.png
[6]: ./media/active-directory-saas-sciquest-spend-director/tutorial_sciquest_spend_director_05.png
[8]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_06.png
[9]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_07.png
[10]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_08.png
[11]: ./media/active-directory-saas-sciquest-spend-director/tutorial_sciquest_spend_director_03.png
[15]: ./media/active-directory-saas-sciquest-spend-director/tutorial_sciquest_spend_director_04.png

[100]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_09.png 
[101]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_10.png 
[102]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_11.png 
[103]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_12.png 
[104]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_13.png 
[105]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_14.png 
[106]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_15.png 
[200]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_16.png 
[201]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_17.png 
[202]: ./media/active-directory-saas-sciquest-spend-director/tutorial_sciquest_spend_director_06.png
[203]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_18.png
[204]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_19.png
[205]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_20.png



