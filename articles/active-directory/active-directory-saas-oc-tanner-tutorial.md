<properties
    pageTitle="チュートリアル: O.C. と Azure Active Directory の統合Tanner - AppreciateHub |Microsoft Azure"
    description="Azure Active Directory と O.C. の間でのシングル サインオンを構成する方法について説明します。Tanner - AppreciateHub です。"
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



# チュートリアル: O.C. と Azure Active Directory の統合Tanner - AppreciateHub

このチュートリアルの目的は O.C. を統合する方法を説明するにはTanner - AppreciateHub Azure で Active Directory (Azure AD)。<br>O.C. を統合します。Tanner - Azure AD と AppreciateHub は、次の利点があります。

- Azure AD ユーザー O.C. へのアクセスを制御できます。Tanner - AppreciateHub
- 自動的に署名 - O.C. にユーザーを有効にすることができます。Tanner - が、Azure AD アカウント (シングル サインオン) AppreciateHub
- 1 つの中央サイト (Azure Active Directory ポータル) でアカウントを管理できます。

Azure AD と SaaS アプリの統合に関する詳細を確認するを参照して [アプリケーションへのアクセスと Azure Active Directory でのシングル サインオン](active-directory-appssoaccess-whatis.md)します。

## 前提条件

Azure AD 統合 O.C. を構成するにはTanner - AppreciateHub、次の項目で済みます。

- Azure AD サブスクリプション
- O.C.Tanner - 有効なサブスクリプションで AppreciateHub シングル サインオン


> [AZURE.NOTE] このチュートリアルの手順をテストする場合、運用環境を使用しないことをお勧めします。


このチュートリアルの手順をテストするには、次の推奨事項に従ってください。

- 必要な場合を除き、運用環境は使用しないでください。
- Azure AD の評価環境を取得していない場合は、1 か月の試用版を入手できます [ここ](https://azure.microsoft.com/pricing/free-trial/)します。


## シナリオの説明

このチュートリアルでは、使用すると、テスト環境で Azure AD シングル サインオンをテストします。 <br>
このチュートリアルで紹介するシナリオは、次の 3 つの主な構成要素で構成されます。

1. O.C. を追加します。Tanner - ギャラリーから AppreciateHub
2. Azure AD シングル サインオンの構成とテスト


## O.C. を追加します。Tanner - ギャラリーから AppreciateHub

O.C. の統合を構成するにはTanner - Azure AD に AppreciateHub、O.C. を追加する必要がありますTanner - 管理対象 SaaS アプリの一覧に、ギャラリーから AppreciateHub します。

**O.C. を追加するにはTanner - AppreciateHub ギャラリーから、[が次の手順を実行します。**

1. **Microsoft Azure 管理ポータル**の左側のナビゲーション ウィンドウで、**[Active Directory]** をクリックします。 
<br><br> ![Active Directory][1] <br>

2. **[ディレクトリ]** の一覧から、ディレクトリ統合を有効にするディレクトリを選択します。

3. アプリケーション ビューを開くには、ディレクトリ ビューでトップ メニューの **[アプリケーション]** をクリックします。
<br><br> ![アプリケーション][2] <br>

4. ページの下部にある **[追加]** をクリックします。
<br><br> ![アプリケーション][3] <br>

5. **[実行する内容]** ダイアログで、**[ギャラリーからアプリケーションを追加します]** をクリックします。
<br><br> ![アプリケーション][4] <br>

6. 検索ボックスに入力 **O.C.Tanner - AppreciateHub**します。
<br><br> ![アプリケーション][5] <br>

7. 結果ウィンドウで [ **O.C.Tanner - AppreciateHub**, 、クリックして **完了** アプリケーションを追加します。
<br><br> ![アプリケーション][25] <br>




## Azure AD シングル サインオンの構成とテスト

このセクションの目的は構成し、Azure AD シングル サインオン O.C. でテストする方法を説明するにはTanner - AppreciateHub は、"Britta Simon"というテスト ユーザーに基づいています。

どのようなユーザーに対応する O.C. を知る必要がある Azure AD のシングル サインオンが動作する、Tanner - ユーザーが Azure AD で AppreciateHub です。つまり、リンクの関係が、Azure AD ユーザーと O.C. に関連するユーザーTanner - AppreciateHub を確立する必要があります。<br>
値を割り当てることでこのリンクの関係が確立される、 **ユーザー名** の値として Azure AD で、 **Username** O.C. でTanner - AppreciateHub です。

構成および Azure AD シングル サインオン O.C. でテストするにはTanner - AppreciateHub を次の要素を完了する必要があります。

1. * *[構成する Azure AD シングル サインオン](#configuring-azure-ad-single-single-sign-on)* * - この機能を使用するユーザーをできるようにします。
2. * *[Azure AD テスト ユーザーを作成する](#creating-an-azure-ad-test-user)* * - を Azure AD シングル サインオン Britta Simon でテストします。
4. * *[、O.C. を作成するTanner - AppreciateHub テスト ユーザー](#creating-a-halogen-software-test-user)* * - O.C. に Britta Simon に対応します。Tanner - AppreciateHub 自分の Azure AD の表現にリンクされています。
5. * *[Azure AD テスト ユーザーの割り当て](#assigning-the-azure-ad-test-user)* * - Azure AD シングル サインオンを使用する Britta Simon をできるようにします。
5. * *[シングル サインオンをテスト](#testing-single-sign-on)* * - 構成が機能するかどうかを確認します。

### Azure AD シングル サインオンの構成

このセクションの目的は Azure AD シングル サインオン Azure AD ポータルで有効にして、O.C. でシングル サインオンを構成するにはTanner - AppreciateHub アプリケーションです。<br>

**O.C. と Azure AD シングル サインオンを構成するにはTanner - AppreciateHub、次の手順を実行します。**

1. Azure AD ポータルでの **O.C.Tanner - AppreciateHub** アプリケーション統合ページでは、をクリックして **でのシングル サインオンを構成する** を開くには、 **シングル サインオンを構成する**  ダイアログ。<br><br>
![Configure Single Sign-On][6]

2. **どのような O.C. にサインオンTanner - AppreciateHub** ] ページで、[ **Azure AD シングル サインオン**, 、] をクリックし、 **次**します。<br><br>
![Azure AD のシングル サインオン][7]

3. **[アプリケーション設定の構成]** ダイアログ ページで、次の手順に従います。
<br><br>![アプリ設定を構成します。][8]

     a. 次のリンクを使用してメタデータ ファイルを開く: [https://fed.appreciatehub.com/fed/sp/metadata](https://fed.appreciatehub.com/fed/sp/metadata)します。

     b. **md:AssertionConsumerService** ノードを探します。

     c. 値をコピー、 **場所** 属性です。 
        <br><br>![アプリ設定を構成します。][12]

     d. **[サインオン URL]** テキスト ボックスに、前の手順で取得した値を貼り付けます。
     > [AZURE.NOTE] メタデータ ファイルからの応答 URL の取得 2/17/2014 問題の場合は、O.C. にお問い合わせください。Tanner - AppreciateHub サポート チームを使用して [sso@octanner.com](mailto:sso@octanner.com)します。

     e. **[次へ]** をクリックします。

4. **O.C. でのシングル サインオンの構成Tanner - AppreciateHub** ] ページで [ **メタデータのダウンロード**, 、コンピューターのローカルにメタデータ ファイルを保存します<br>。<br>![は Azure AD Connect][9]

5. 連絡先、O.C.Tanner - xyz, による AppreciateHub サポート チームがそれらをメタデータ ファイルを提供しに連絡し、SSO がするを有効にする必要があります、します。

6. Azure AD ポータルで、シングル サインオンの構成確認を選択し、**[次へ]** をクリックします。<br><br>![Azure AD Connect とは][10]

7. **[シングル サインオンの確認]** ページで **[完了]** をクリックします。<br><br>![Azure AD Connect とは][11]




### Azure AD のテスト ユーザーの作成

このセクションでは、Britta Simon と呼ばれる Azure ポータルでテスト ユーザーを作成します。<br>
ユーザーの一覧で選択 **Britta Simon**.<br><br>![Azure AD ユーザーを作成][20]<br>

**Azure AD でテスト ユーザーを作成するには、次の手順に従います。**

1. **Azure 管理ポータル**, 、左側のナビゲーション ウィンドウで、次のようにクリックします。 **Active Directory**します。<br>
![Azure AD のテスト ユーザーの作成](./media/active-directory-saas-oc-tanner-tutorial/create_aaduser_02.png)

2. **[ディレクトリ]** の一覧から、ディレクトリ統合を有効にするディレクトリを選択します。

3. 上部のメニューで、ユーザーの一覧を表示する] をクリックして **ユーザー**.<br>![Azure AD テスト ユーザーを作成する](./media/active-directory-saas-oc-tanner-tutorial/create_aaduser_03.png)

4. 開くには、 **ユーザーの追加** ] ダイアログ ボックスで、下部にあるツールバーをクリックして **ユーザーの追加**します。<br>![Azure AD テスト ユーザーを作成します。](./media/active-directory-saas-oc-tanner-tutorial/create_aaduser_04.png)

5. **このユーザーについてお聞かせ** ] ダイアログ ページで、次の手順を実行します <br>![Azure AD テスト ユーザーを作成する](./media/active-directory-saas-oc-tanner-tutorial/create_aaduser_05.png)。

    a. [ユーザーの種類] として [組織内の新しいユーザー] を選択します。

    b. **[ユーザー名]** ボックスに「**BrittaSimon**」と入力します。

    c. **[次へ]** をクリックします。

6.  **ユーザー プロファイル** ] ダイアログ ページで、次の手順を実行します <br>![Azure AD テスト ユーザーを作成する](./media/active-directory-saas-oc-tanner-tutorial/create_aaduser_06.png)。

    a. **[名]** ボックスに「**Britta**」と入力します。

    b. **[姓]** ボックスに「**Simon**」と入力します。

    c. **[表示名]** ボックスに「**Britta Simon**」と入力します。

    d. **[ロール]** 一覧で **[ユーザー]** を選択します。
    e. **[次へ]** をクリックします。

7. **一時パスワードの取得** ダイアログ ページで、をクリックして **作成**.<br>![Azure AD テスト ユーザーを作成する](./media/active-directory-saas-oc-tanner-tutorial/create_aaduser_07.png)

8. **一時パスワードの取得** ] ダイアログ ページで、次の手順を実行します<br>![Azure AD テスト ユーザーを作成する](./media/active-directory-saas-oc-tanner-tutorial/create_aaduser_08.png)。

    a. **[新しいパスワード]** の値を書き留めます。

    b. **[完了]** をクリックします。



### O.C. を作成します。Tanner - AppreciateHub テスト ユーザー

このセクションの目的は、O.C. で Britta Simon をというユーザーを作成するにはTanner - AppreciateHub です。

**O.C. で Britta Simon をというユーザーを作成するにはTanner - AppreciateHub、次の手順を実行します。**

1. OC Tanner サポート チームに、Azure AD 内のユーザー名 Britta Simon と同じ値の nameID 属性を持つユーザーを作成することを依頼します。


### Azure AD テスト ユーザーの割り当て

このセクションの目的が O.C. に自分のアクセスを付与することによって Azure シングル サインオンを使用する Britta Simon を有効にするのにはTanner - AppreciateHub です。
<br><br>![ユーザーを割り当てる][200]

**O.C. に Britta Simon を割り当てるTanner - AppreciateHub、次の手順を実行します。**

1. Azure ポータルで、ディレクトリ ビューでアプリケーション ビューを開く] をクリックして **アプリケーション** 上部のメニュー。<br>
<br><br>![ユーザーを割り当てる][201]
2. アプリケーションの一覧で選択 **O.C.Tanner - AppreciateHub**します。
<br><br>![ユーザーを割り当てる][202]
1. 上部にあるメニュー [ **ユーザー**します。<br>
<br><br>![ユーザーを割り当てる][203]
1. ユーザーの一覧で **[Britta Simon]** を選択します。

2. 下部にあるツールバー [ **割り当てる**します。
<br><br>![ユーザーを割り当てる][205]



### シングル サインオンのテスト

このセクションでは、構成のテスト Azure AD シングル サインオン アクセス パネルを使用します。<br>
O.C. をクリックすると、Tanner - アクセス パネルに AppreciateHub 並べて表示する必要があります取得自動的にサインオンして O.C. にTanner - AppreciateHub アプリケーションです。


## その他のリソース

* [Azure Active Directory による SaaS アプリを統合する方法に関するチュートリアルの一覧](active-directory-saas-tutorial-list.md)
* [アプリケーション アクセスと Azure Active Directory でのシングル サインオンとは](active-directory-appssoaccess-whatis.md)




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

