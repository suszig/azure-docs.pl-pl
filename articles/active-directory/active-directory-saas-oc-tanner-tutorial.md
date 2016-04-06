<properties
    pageTitle="チュートリアル: Azure Active Directory と O.C.  Tanner - AppreciateHub の統合 | Microsoft Azure"
    description="Azure Active Directory と O.C.  Tanner - AppreciateHub の間でシングル サインオンを構成する方法について説明します。"
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
    ms.date="10/09/2015"
    ms.author="markusvi"/>


# チュートリアル: Azure Active Directory と O.C.  Tanner - AppreciateHub

このチュートリアルの目的は O.C. を統合する方法を説明するには Tanner - AppreciateHub Azure で Active Directory (Azure AD)。<br>O.C. を統合します。 Tanner - Azure AD と AppreciateHub は、次の利点があります。 

- Azure AD ユーザー O.C. へのアクセスを制御できます。 Tanner - AppreciateHub 
- ユーザーが自分の Azure AD アカウントで自動的に O.C.  Tanner にサインオン (シングル サインオン) できるようにします。
- 1 つの中央サイト (Azure Active Directory ポータル) でアカウントを管理できます。

Azure AD と SaaS アプリの統合に関する詳細を確認するを参照して [アプリケーションへのアクセスと Azure Active Directory でのシングル サインオン](active-directory-appssoaccess-whatis.md)します。

## 前提条件 

Azure AD 統合 O.C. を構成するには Tanner - AppreciateHub、次の項目で済みます。

- Azure AD サブスクリプション
- O.C. Tanner - 有効なサブスクリプションで AppreciateHub シングル サインオン


> [AZURE.NOTE] このチュートリアルでは、手順をテストするには、実稼働環境の使用をおしないでいます。


このチュートリアルの手順をテストするには、次の推奨事項に従ってください。

- 必要な場合を除き、運用環境は使用しないでください。
- Azure AD の評価環境を取得していない場合は、1 か月の試用版を入手できます [ここ](https://azure.microsoft.com/pricing/free-trial/)します。 

 
## シナリオの説明
このチュートリアルの目的は、テスト環境で Azure AD のシングル サインオンをテストできるようにすることです。 <br>
このチュートリアルで紹介するシナリオは、次の 3 つの主な構成要素で構成されます。

1. ギャラリーからの O.C.  Tanner - AppreciateHub の追加 
2. Azure AD シングル サインオンの構成とテスト


## ギャラリーからの O.C.  Tanner - AppreciateHub の追加
Azure AD への O.C.  Tanner - AppreciateHub の統合を構成するには、ギャラリーから管理対象 SaaS アプリの一覧に O.C.  Tanner - AppreciateHub を追加する必要があります。

**O.C. を追加するには Tanner - AppreciateHub ギャラリーから、[が次の手順を実行します。**

1.  **Azure 管理ポータル**, 、左側のナビゲーション ウィンドウで、次のようにクリックします。 **Active Directory**します。 
<br><br> ![Active Directory][1] <br>

2.  **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3. ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。
<br><br> ![アプリケーション][2] <br>

4. クリックして **追加** ページの下部にあります。
<br><br> ![アプリケーション][3] <br>

5.  **実行する操作** ダイアログ ボックスで、をクリックして **ギャラリーからアプリケーションを追加**します。
<br><br> ![アプリケーション][4] <br>

6. 検索ボックスに入力 **O.C.Tanner - AppreciateHub**します。
<br><br> ![アプリケーション][5] <br>

7. 結果ウィンドウで [ **O.C.Tanner - AppreciateHub**, 、クリックして **完了** アプリケーションを追加します。
<br><br> ![アプリケーション][25] <br>




##  Azure AD シングル サインオンの構成とテスト
このセクションの目的は、Azure AD と O.C.  Tanner - AppreciateHub のシングル サインオンを構成し、テスト ユーザー "Britta Simon" を使用してテストする方法について説明することです。

シングル サインオンを機能させるには、Azure AD ユーザーに対応する O.C.  Tanner - AppreciateHub ユーザーが Azure AD で認識されている必要があります。 言い換えると、Azure AD ユーザーと O.C.  Tanner - AppreciateHub を確立する必要があります。<br>
値を割り当てることでこのリンクの関係が確立される、 **ユーザー名** の値として Azure AD で、 **Username** O.C. で Tanner - AppreciateHub の間でシングル サインオンを構成する方法について説明します。
 
O.C.  Tanner - AppreciateHub で Azure AD のシングル サインオンを構成してテストするには、次の構成要素を完了する必要があります。

1. **[Azure AD シングル サインオンを構成する](#configuring-azure-ad-single-single-sign-on)** - この機能を使用するユーザーをできるようにします。
2. **[Azure AD テスト ユーザーを作成する](#creating-an-azure-ad-test-user)** - を Azure AD シングル サインオン Britta Simon でテストします。
4. **[O.C. を作成します。Tanner - AppreciateHub テスト ユーザー](#creating-a-halogen-software-test-user)** - O.C. に Britta Simon に対応します。 Tanner - AppreciateHub で Britta Simon に対応するユーザーを作成し、Azure AD の Britta Simon にリンクさせます。
5. **[Azure AD テスト ユーザーの割り当て](#assigning-the-azure-ad-test-user)** - Azure AD シングル サインオンを使用する Britta Simon をできるようにします。
5. **[シングル サインオン テスト](#testing-single-sign-on)** - 構成が機能するかどうかを確認します。

### Azure AD シングル サインオンの構成

このセクションの目的は Azure AD シングル サインオン Azure AD ポータルで有効にして、O.C. でシングル サインオンを構成するには Tanner - AppreciateHub  アプリケーションにサインオンします。<br>

**O.C. と Azure AD シングル サインオンを構成するには Tanner - AppreciateHub、次の手順を実行します。**

1. Azure AD ポータルでの **O.C.Tanner - AppreciateHub** アプリケーション統合ページでは、をクリックして **でのシングル サインオンを構成する** を開くには、 **シングル サインオンを構成する**  ダイアログ。<br><br>
![シングル サインオンを構成します。][6]

2.  **O.C. にサインオンするユーザーを希望する方法Tanner - AppreciateHub** ] ページで、[ **Azure AD シングル サインオン**, 、] をクリックし、 **次**します。<br><br>
![Azure AD シングル サインオン][7]

3.  **アプリケーション設定の構成** ] ダイアログ ページで、次の手順を実行します。
<br><br>![アプリ設定を構成します。][8]
 
     a. 次のリンクを使用してメタデータ ファイルを開く: [https://fed.appreciatehub.com/fed/sp/metadata](https://fed.appreciatehub.com/fed/sp/metadata)します。

     b. 検索、 **md:AssertionConsumerService** ノードです。 

     c. 値をコピー、 **場所** 属性です。 
        <br><br>![アプリ設定を構成します。][12]
     
     d.  **サインオン URL** ] ボックスに、前の手順で取得した値を超えています。

     > [AZURE.NOTE] メタデータ ファイルからの応答 URL の取得 2/17/2014 問題の場合は、O.C. にお問い合わせください。 Tanner - AppreciateHub サポート チームを使用して [sso@octanner.com](mailto:sso@octanner.com)します。

     e. クリックして **次**します。
 
4.  **O.C. でのシングル サインオンの構成Tanner - AppreciateHub** ] ページで [ **メタデータのダウンロード**, 、コンピューターのローカルにメタデータ ファイルを保存します。<br><br>![Azure AD Connect とは][9]

5. O.C.  Tanner - AppreciateHub サポート チーム (xyz) に連絡してメタデータ ファイルを提供し、SSO を有効にする必要があることを伝えます。


6. Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **次**します。 <br><br>![Azure AD Connect とは][10]

7.  **シングル サインオンによる確認** ] ページで [ **完了**します。  <br><br>![Azure AD Connect とは][11]




### Azure AD のテスト ユーザーの作成
このセクションの目的は、Azure ポータルで Britta Simon というテスト ユーザーを作成することです。<br>
ユーザーの一覧で選択 **Britta Simon**します。<br><br>![Azure AD ユーザーを作成します。][20]<br>

**Azure AD でテスト ユーザーを作成するには、次の手順に従います。**

1.  **Azure 管理ポータル**, 、左側のナビゲーション ウィンドウで、次のようにクリックします。 **Active Directory**します。<br>
![Azure AD テスト ユーザーを作成します。](./media/active-directory-saas-oc-tanner-tutorial/create_aaduser_02.png) 

2.  **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3. 上部のメニューで、ユーザーの一覧を表示する] をクリックして **ユーザー**します。<br>![Azure AD テスト ユーザーを作成します。](./media/active-directory-saas-oc-tanner-tutorial/create_aaduser_03.png) 
 
4. 開くには、 **ユーザーの追加** ] ダイアログ ボックスで、下部にあるツールバーをクリックして **ユーザーの追加**します。 <br>![Azure AD テスト ユーザーを作成します。](./media/active-directory-saas-oc-tanner-tutorial/create_aaduser_04.png) 

5.  **このユーザーについてお聞かせ** ] ダイアログ ページで、次の手順を実行します。 <br>![Azure AD テスト ユーザーを作成します。](./media/active-directory-saas-oc-tanner-tutorial/create_aaduser_05.png) 

    a. [ユーザーの種類] として [組織内の新しいユーザー] を選択します。

    b. [ユーザー名 **textbox**, 、型 **BrittaSimon**します。

    c. クリックして **次**します。

6.   **ユーザー プロファイル** ] ダイアログ ページで、次の手順を実行します。 <br>![Azure AD テスト ユーザーを作成します。](./media/active-directory-saas-oc-tanner-tutorial/create_aaduser_06.png)
 
    a.  **名** ] ボックスに「 **Britta**します。  

    b.  **姓** ] ボックスに、型、 **Simon**します。

    c.  **表示名** ] ボックスに「 **Britta Simon**します。

    d.  **ロール** 一覧で、[ **ユーザー**します。
    e. クリックして **次**します。

7.  **一時パスワードの取得** ダイアログ ページで、をクリックして **作成**します。<br>![Azure AD テスト ユーザーを作成します。](./media/active-directory-saas-oc-tanner-tutorial/create_aaduser_07.png) 
 
8.  **一時パスワードの取得** ] ダイアログ ページで、次の手順を実行します。<br>![Azure AD テスト ユーザーを作成します。](./media/active-directory-saas-oc-tanner-tutorial/create_aaduser_08.png) 
  
    a. 値を書き留めて、 **新しいパスワード**します。

    b. クリックして **完了**します。   

  
 
### O.C.  Tanner - AppreciateHub テスト ユーザーの作成

このセクションの目的は、O.C.  Tanner - AppreciateHub の間でシングル サインオンを構成する方法について説明します。

**O.C. で Britta Simon をというユーザーを作成するには Tanner - AppreciateHub、次の手順を実行します。**

1. OC Tanner サポート チームに、Azure AD 内のユーザー名 Britta Simon と同じ値の nameID 属性を持つユーザーを作成することを依頼します。


### Azure AD テスト ユーザーの割り当て

このセクションの目的は、Britta Simon に O.C.  Tanner - AppreciateHub の間でシングル サインオンを構成する方法について説明します。
<br><br>![ユーザーの割り当て][200]

**O.C. に Britta Simon を割り当てる Tanner - AppreciateHub、次の手順を実行します。**

1. Azure ポータルで、ディレクトリ ビューでアプリケーション ビューを開く] をクリックして **アプリケーション** 上部のメニュー。<br>
<br><br>![ユーザーを割り当てる][201]
2. アプリケーションの一覧で選択 **O.C.Tanner - AppreciateHub**します。
<br><br>![ユーザーを割り当てる][202]
1. 上部にあるメニュー [ **ユーザー**します。<br>
<br><br>![ユーザーを割り当てる][203]
1. ユーザーの一覧で選択 **Britta Simon**します。

2. 下部にあるツールバー [ **割り当てる**します。
<br><br>![ユーザーを割り当てる][205]



### シングル サインオンのテスト

このセクションでは、構成のテスト Azure AD シングル サインオン アクセス パネルを使用します。<br>
アクセス パネルで [O.C.  Tanner - AppreciateHub] タイルをクリックすると、自動的に O.C.  Tanner - AppreciateHub  アプリケーションにサインオンします。


## その他のリソース

* [SaaS アプリと Azure Active Directory を統合する方法に関するチュートリアルの一覧](active-directory-saas-tutorial-list.md)
* [Azure Active Directory のアプリケーション アクセスとシングル サインオンとは](active-directory-appssoaccess-whatis.md)


<!--Image references-->
[1]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_octanner_01.png
[25]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_octanner_25.png


[6]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_05.png
[7]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_octanner_02.png
[8]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_octanner_03.png
[9]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_octanner_04.png
[10]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_octanner_05.png
[11]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_octanner_06.png
[12]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_octanner_08.png
[20]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_octanner_07.png
[203]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_205.png








