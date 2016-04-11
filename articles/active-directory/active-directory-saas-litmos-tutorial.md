<properties
    pageTitle="チュートリアル: Azure Active Directory と Litmos の統合 | Microsoft Azure"
    description="Azure Active Directory と Litmos の間でシングル サインオンを構成する方法について説明します。"
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
    ms.date="10/30/2015"
    ms.author="markusvi"/>


# チュートリアル: Azure Active Directory と Litmos の統合

このチュートリアルでは、Azure Active Directory (Azure AD) と Litmos を統合する方法を説明します。<br>Litmos を Azure AD と統合するには、次のメリットがあります。 

- Litmos にアクセスする Azure AD ユーザーを制御できます。 
- ユーザーが自分の Azure AD アカウントで自動的に Litmos にサインオン (シングル サインオン) できるようにします。
- 1 つの中央サイト (Azure Active Directory ポータル) でアカウントを管理できます。

Azure AD と SaaS アプリの統合に関する詳細を確認するを参照して [アプリケーションへのアクセスと Azure Active Directory でのシングル サインオン](active-directory-appssoaccess-whatis.md)します。

## 前提条件 

Litmos と Azure AD の統合を構成するには、次のものが必要です。

- Azure AD サブスクリプション
- Litmos でのシングル サインオンが有効なサブスクリプション


> [AZURE.NOTE] このチュートリアルでは、手順をテストするには、実稼働環境の使用をおしないでいます。


このチュートリアルの手順をテストするには、次の推奨事項に従ってください。

- 必要な場合を除き、運用環境は使用しないでください。
- Azure AD の評価環境を取得していない場合は、1 か月の試用版を入手できます [ここ](https://azure.microsoft.com/pricing/free-trial/)します。 

 
## シナリオの説明
このチュートリアルの目的は、テスト環境で Azure AD のシングル サインオンをテストできるようにすることです。 <br>
このチュートリアルで紹介するシナリオは、次の 3 つの主な構成要素で構成されます。

1. ギャラリーからの Litmos の追加 
2. Azure AD シングル サインオンの構成とテスト


## ギャラリーからの Litmos の追加
Azure AD への Litmos の統合を構成するには、ギャラリーから管理対象 SaaS アプリの一覧に Litmos を追加する必要があります。

**ギャラリーから Litmos を追加するには、次の手順に従います。**

1.  **Azure ポータル**, 、左側のナビゲーション ウィンドウで、次のようにクリックします。 **Active Directory**します。 <br><br>
![Active Directory][1]<br>

2.  **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3. ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。<br><br>
![アプリケーション][2]<br>
4. クリックして **追加** ページの下部にあります。<br><br>
![アプリケーション][3]<br>
5.  **実行する操作** ダイアログ ボックスで、をクリックして **ギャラリーからアプリケーションを追加**します。<br><br>
![アプリケーション][4]<br>
6. 検索ボックスに入力 **Litmos**します。<br><br>
![アプリケーション][5]<br>
7. 結果ウィンドウで [ **Litmos**, 、クリックして **完了** アプリケーションを追加します。
<br><br>![アプリケーション][500]<br>


##  Azure AD シングル サインオンの構成とテスト
このセクションの目的は、"Britta Simon" というテスト ユーザーに基づいて、Litmos で Azure AD のシングル サインオンを構成し、テストする方法について説明することです。

シングル サインオンを機能させるには、Azure AD ユーザーに対応する Litmos ユーザーが Azure AD で認識されている必要があります。 つまり、Azure AD のユーザーと Litmos に関連するユーザーの間のリンクの関係を確立する必要があります。<br>
値を割り当てることでこのリンクの関係が確立される、 **ユーザー名** の値として Azure AD で、 **Username** Litmos にします。
 
Litmos で Azure AD のシングル サインオンを構成してテストするには、次の構成要素を完了する必要があります。

1. **[Azure AD シングル サインオンを構成する](#configuring-azure-ad-single-single-sign-on)** - この機能を使用するユーザーをできるようにします。
2. **[Azure AD テスト ユーザーを作成する](#creating-an-azure-ad-test-user)** - を Azure AD シングル サインオン Britta Simon でテストします。
4. **[Litmos テスト ユーザーを作成する](#creating-a-halogen-software-test-user)** - 自分の Azure AD の表現にリンクされている Litmos に Britta Simon に対応します。
5. **[Azure AD テスト ユーザーの割り当て](#assigning-the-azure-ad-test-user)** - Azure AD シングル サインオンを使用する Britta Simon をできるようにします。
5. **[シングル サインオン テスト](#testing-single-sign-on)** - 構成が機能するかどうかを確認します。

### Azure AD シングル サインオンの構成

このセクションの目的は、Azure AD シングル サインオン Azure AD ポータルで有効にして、Litmos アプリケーションでシングル サインオンを構成するには。<br>
この手順の途中で、base-64 でエンコードされた証明書ファイルを作成する必要があります。  
この手順に慣れていない場合は、次を参照してください。 [バイナリ証明書をテキスト ファイルに変換する方法](http://youtu.be/PlgrzUZ-Y1o)します。

構成の一環として、カスタマイズする必要があります。、 **SAML トークン属性** Litmos アプリケーションにします。  
<br><br> ![Azure AD シングル サインオン][17] <br>

**Litmos で Azure AD シングル サインオンを構成するには、次の手順に従います。**

1. Azure AD ポータルでの **Litmos** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、 **シングル サインオンを構成する**  ダイアログ。
<br><br> ![シングル サインオンを構成します。][6] <br>

2.  **どのような Litmos にサインオン** ] ページで、[ **Azure AD シングル サインオン**, 、順にクリック **次**します。
<br><br> ![Azure AD シングル サインオン][7] <br>


1. サインオン Litmos 会社サイト (例:: *https://azureapptest.litmos.com/account/Login*)、管理者として。
<br><br> ![Azure AD シングル サインオン][21] <br>


1. 左側にあるナビゲーション バーで、クリックして **アカウント**します。
<br><br> ![Azure AD シングル サインオン][22] <br>


1. クリックして、 **統合** ] タブをクリックします。
<br><br> ![Azure AD シングル サインオン][23] <br>


1.  **統合** ] タブで、下へスクロールして **サード パーティ製の統合**, 、] をクリックし、 **SAML 2.0** ] タブをクリックします。
<br><br> ![Azure AD シングル サインオン][24] <br>

1. 下の値をコピー **litmos の「SAML endoiint が:**です。
<br><br> ![Azure AD シングル サインオン][26] <br>


3. Azure ポータルでの **アプリケーション設定の構成** ] ダイアログ ページで、次の手順を実行します。
<br><br>![Azure AD シングル サインオン][8] <br>
 
    a.  **識別子** Litmos アプリケーションにサインオンする URL は、ユーザーが使用] ボックスに、種類 (例:: *https://azureapptest.litmos.com/account/Login*)。
     
    b.  **応答 URL** ] ボックスに、前の手順で Litmos アプリケーションからコピーした値を貼り付けます。

    c. クリックして **次**します。
 
4.  **Litmos でのシングル サインオンの構成** ] ページで、次の手順を実行します。
<br><br>![Azure AD シングル サインオン][2] <br>

    a. [証明書のダウンロード] をクリックし、コンピューターにファイルを保存します。


1.  **Litmos** アプリケーションでは、次の手順を実行します。
<br><br>![Azure AD シングル サインオン][25] <br>

    a. クリックして **Enable SAML**します。

    b. 作成、 **base-64 でエンコードされた** からダウンロードした証明書ファイル。  

    >[AZURE.TIP] 詳細については、次を参照してください [バイナリ証明書をテキスト ファイルに変換する方法。](http://youtu.be/PlgrzUZ-Y1o)

    c. Base-64 でエンコードされた証明書をメモ帳で開き、という内容がクリップボードにコピーおよびに貼り付け、 **SAML X.509 証明書** ] ボックスに貼り付けます。

    d. クリックして **変更を保存**します。


6. Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **次**します。 
<br><br>![Azure AD シングル サインオン][10]<br>

7.  **シングル サインオンによる確認** ] ページで [ **完了**します。  
  <br><br>![Azure AD シングル サインオン][11]


20. 上部にあるメニュー [ **属性** を開くには、 **SAML トークン属性** ダイアログ。 
<br><br>![シングル サインオンを構成します。][12]<br>


24.  **ユーザー属性を追加** ] ダイアログ ボックスで、次の手順を実行します。 
<br><br>![シングル サインオンを構成します。][14]<br>

  	| 属性名 | 属性値 |
  	| ---            | ---             |
  	| 電子メール          | User.mail       |
  	| FirstName      | User.givenname  |
  	| Lastname       | User.surname    |

    上の表のデータ行ごとに、次の手順を実行します。
   
    a. クリックして **ユーザー属性の追加**します。 <br><br>![シングル サインオンを構成します。][15]<br>


    a. In the **Attribute Name** textbox, type the **Attribute Name** shown for that row.

    b. Select the **Attribute Value** shown for that row.

    c. Click **Complete** to close the **Add User Attribute** dialog.


25. クリックして **の変更を適用**します。 
<br><br>![シングル サインオンを構成します。][16]<br>




### Azure AD のテスト ユーザーの作成
このセクションの目的は、Azure ポータルで Britta Simon というテスト ユーザーを作成することです。<br>
ユーザーの一覧で選択 **Britta Simon**します。<br><br>![Azure AD ユーザーを作成します。][20]<br>

**Azure AD でテスト ユーザーを作成するには、次の手順に従います。**

1.  **Azure ポータル**, 、左側のナビゲーション ウィンドウで、次のようにクリックします。 **Active Directory**します。
<br><br>![Azure AD テスト ユーザーを作成します。](./media/active-directory-saas-litmos-tutorial/create_aaduser_09.png) <br> 

2.  **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3. 上部のメニューで、ユーザーの一覧を表示する] をクリックして **ユーザー**します。
<br><br> ![Azure AD テスト ユーザーを作成します。](./media/active-directory-saas-litmos-tutorial/create_aaduser_03.png) <br>
 
4. 開くには、 **ユーザーの追加** ] ダイアログ ボックスで、下部にあるツールバーをクリックして **ユーザーの追加**します。 
<br><br> ![Azure AD テスト ユーザーを作成します。](./media/active-directory-saas-litmos-tutorial/create_aaduser_04.png) <br>

5.  **このユーザーについてお聞かせ** ] ダイアログ ページで、次の手順を実行します。 
<br><br> ![Azure AD テスト ユーザーを作成します。](./media/active-directory-saas-litmos-tutorial/create_aaduser_05.png) <br> 

    a. として **Type Of User**, [ **、組織内の新しいユーザー**します。

    b. [ユーザー名 **textbox**, 、型 **BrittaSimon**します。

    c. クリックして **次**します。

6.   **ユーザー プロファイル** ] ダイアログ ページで、次の手順を実行します。 
<br><br>![Azure AD テスト ユーザーを作成します。](./media/active-directory-saas-litmos-tutorial/create_aaduser_06.png) <br>
 
    a.  **名** ] ボックスに「 **Britta**します。  

    b.  **姓** ] ボックスに、型、 **Simon**します。

    c.  **表示名** ] ボックスに「 **Britta Simon**します。

    d.  **ロール** 一覧で、[ **ユーザー**します。
    e. クリックして **次**します。

7.  **一時パスワードの取得** ダイアログ ページで、をクリックして **作成**します。
<br><br> ![Azure AD テスト ユーザーを作成します。](./media/active-directory-saas-litmos-tutorial/create_aaduser_07.png) <br>
 
8.  **一時パスワードの取得** ] ダイアログ ページで、次の手順を実行します。
<br><br>![Azure AD テスト ユーザーを作成します。](./media/active-directory-saas-litmos-tutorial/create_aaduser_08.png) <br>
  
    a. 値を書き留めて、 **新しいパスワード**します。

    b. クリックして **完了**します。   

  
 
### Litmos のテスト ユーザーの作成

このセクションでは、Litmos で Britta Simon をというユーザーを作成します。<br>
Litmos アプリケーションでは、ジャストインタイム プロビジョニングがサポートされています。 そのため、アクセス パネルを使用してアプリケーションにアクセスを試みると、必要に応じてユーザー アカウントが自動的に作成されます。

**Litmos で Britta Simon というユーザーを作成するには、次の手順に従います。**


1. サインオン Litmos 会社サイト (例:: *https://azureapptest.litmos.com/account/Login*)、管理者として。
<br><br> ![Azure AD シングル サインオン][21] <br>


1. 左側にあるナビゲーション バーで、クリックして **アカウント**します。
<br><br> ![Azure AD シングル サインオン][22] <br>


1. クリックして、 **統合** ] タブをクリックします。
<br><br> ![Azure AD シングル サインオン][23] <br>


1.  **統合** ] タブで、下へスクロールして **サード パーティ製の統合**, 、] をクリックし、 **SAML 2.0** ] タブをクリックします。
<br><br> ![Azure AD シングル サインオン][24] <br>

1. 選択 **Autogenerate ユーザー:**です。
<br><br> ![Azure AD シングル サインオン][27] <br>


### Azure AD テスト ユーザーの割り当て

このセクションでは、Britta Simon Litmos に自分のアクセスを与えることで Azure シングル サインオンの使用を有効にするとします。
<br><br>![ユーザーの割り当て][200] <br>

**Litmos に Britta Simon を割り当てるには、次の手順に従います。**

1. Azure ポータルで、ディレクトリ ビューでアプリケーション ビューを開く] をクリックして **アプリケーション** 上部のメニュー。
<br><br>![ユーザーを割り当てる][201] <br>
2. アプリケーションの一覧で選択 **Litmos**します。
<br><br>![ユーザーを割り当てる][202] <br>
1. 上部にあるメニュー [ **ユーザー**します。
<br><br>![ユーザーを割り当てる][203] <br>
1. ユーザーの一覧で選択 **Britta Simon**します。

2. 下部にあるツールバー [ **割り当てる**します。
<br><br>![ユーザーを割り当てる][205]



### シングル サインオンのテスト

このセクションでは、構成のテスト Azure AD シングル サインオン アクセス パネルを使用します。<br>
アクセス パネルで Litmos のタイルをクリックすると、自動的に Litmos アプリケーションにサインオンします。


## その他のリソース

* [SaaS アプリと Azure Active Directory を統合する方法に関するチュートリアルの一覧](active-directory-saas-tutorial-list.md)
* [Azure Active Directory のアプリケーション アクセスとシングル サインオンとは](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_01.png
[500]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_02.png

[6]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_05.png
[7]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_03.png
[8]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_04.png
[9]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_05.png
[10]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_07.png
[12]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_80.png
[13]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_81.png
[14]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_82.png
[15]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_81.png
[16]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_19.png
[17]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_67.png


[20]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_100.png
[21]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_60.png
[22]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_61.png
[23]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_62.png
[24]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_63.png
[25]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_64.png
[26]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_65.png
[27]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_66.png

[200]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_68.png
[203]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_205.png


[400]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_400.png
[401]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_401.png
[402]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_402.png







