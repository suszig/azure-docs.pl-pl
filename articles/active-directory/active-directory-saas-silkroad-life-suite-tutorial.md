<properties
    pageTitle="チュートリアル: Azure Active Directory と SilkRoad Life Suite の統合 | Microsoft Azure"
    description="Azure Active Directory と SilkRoad Life Suite の間でシングル サインオンを構成する方法について説明します。"
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
    ms.date="10/23/2015"
    ms.author="markusvi"/>


# チュートリアル: Azure Active Directory と SilkRoad Life Suite の統合

このチュートリアルでは、Azure Active Directory (Azure AD) と SilkRoad ライフ スイートを統合する方法を説明します。<br>SilkRoad ライフ スイートを Azure AD と統合するには、次のメリットがあります。 

- SilkRoad Life Suite にアクセスする Azure AD ユーザーを制御できます。 
- ユーザーが自分の Azure AD アカウントで自動的に SilkRoad Life Suite にサインオン (シングル サインオン) できるようにします。

Azure AD と SaaS アプリの統合に関する詳細を確認するを参照して [アプリケーションへのアクセスと Azure Active Directory でのシングル サインオン](active-directory-appssoaccess-whatis.md)します。

## 前提条件 

SilkRoad Life Suite と Azure AD の統合を構成するには、次のものが必要です。

- Azure AD サブスクリプション
- SilkRoad Life Suite でのシングル サインオンが有効なサブスクリプション


> [AZURE.NOTE] このチュートリアルでは、手順をテストするには、実稼働環境の使用をおしないでいます。


このチュートリアルの手順をテストするには、次の推奨事項に従ってください。

- 必要な場合を除き、運用環境は使用しないでください。
- Azure AD の評価環境を取得していない場合は、1 か月の試用版を入手できます [ここ](https://azure.microsoft.com/pricing/free-trial/)します。 

 
## シナリオの説明
このチュートリアルの目的は、テスト環境で Azure AD のシングル サインオンをテストできるようにすることです。 <br>
このチュートリアルで紹介するシナリオは、2 つの主な構成要素で構成されます。

1. ギャラリーからの SilkRoad Life Suite の追加 
2. Azure AD シングル サインオンの構成とテスト


## ギャラリーからの SilkRoad Life Suite の追加
Azure AD への SilkRoad Life Suite の統合を構成するには、ギャラリーから管理対象 SaaS アプリの一覧に SilkRoad Life Suite を追加する必要があります。

**ギャラリーから SilkRoad Life Suite を追加するには、次の手順に従います。**

1.  **Azure 管理ポータル**, 、左側のナビゲーション ウィンドウで、次のようにクリックします。 **Active Directory**します。 <br><br>
![Active Directory][1]<br>

2.  **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3. ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。<br><br>
![アプリケーション][2]<br>
4. クリックして **追加** ページの下部にあります。<br><br>
![アプリケーション][3]<br>
5.  **実行する操作** ダイアログ ボックスで、をクリックして **ギャラリーからアプリケーションを追加**します。<br><br>
![アプリケーション][4]<br>
6. 検索ボックスに入力 **SilkRoad ライフ スイート**します。<br><br>
![アプリケーション][5]<br>
7. 結果ウィンドウで [ **SilkRoad ライフ スイート**, 、] をクリックし、 **完了** アプリケーションを追加します。
<br><br>![アプリケーション][50]<br>


##  Azure AD シングル サインオンの構成とテスト
このセクションの目的は、"Britta Simon" というテスト ユーザーに基づいて、SilkRoad Life Suite で Azure AD のシングル サインオンを構成し、テストする方法について説明することです。

シングル サインオンを機能させるには、Azure AD ユーザーに対応する SilkRoad Life Suite ユーザーが Azure AD で認識されている必要があります。 つまり、Azure AD のユーザーと SilkRoad ライフ スイート内の関連ユーザーの間のリンクの関係を確立する必要があります。<br>
値を割り当てることでこのリンクの関係が確立される、 **ユーザー名** の値として Azure AD で、 **ユーザー名** SilkRoad ライフ スイートです。
 
SilkRoad Life Suite で Azure AD のシングル サインオンを構成してテストするには、次の構成要素を完了する必要があります。

1. **[Azure AD シングル サインオンを構成する](#configuring-azure-ad-single-single-sign-on)** - この機能を使用するユーザーをできるようにします。
2. **[Azure AD テスト ユーザーを作成する](#creating-an-azure-ad-test-user)** - を Azure AD シングル サインオン Britta Simon でテストします。
4. **[SilkRoad ライフ スイートのテスト ユーザーを作成する](#creating-a-silkroad-life-suite-test-user)** - 自分の Azure AD の表現にリンクされている SilkRoad ライフ スイートに Britta Simon に対応します。
5. **[Azure AD テスト ユーザーの割り当て](#assigning-the-azure-ad-test-user)** - Azure AD シングル サインオンを使用する Britta Simon をできるようにします。
5. **[シングル サインオン テスト](#testing-single-sign-on)** - 構成が機能するかどうかを確認します。

### Azure AD シングル サインオンの構成

このセクションの目的は、Azure AD シングル サインオン Azure AD ポータルで有効にして、SilkRoad ライフ Suite アプリケーションでシングル サインオンを構成するには。<br>

**SilkRoad Life Suite で Azure AD シングル サインオンを構成するには、次の手順に従います。**

5. SilkRoad 企業サイトに管理者としてサインオンします。 


    > [AZURE.NOTE] Microsoft Azure AD とフェデレーションを構成するための SilkRoad ライフ スイート認証アプリケーションにアクセスするには、SilkRoad サポートまたは SilkRoad サービス担当者に問い合わせてください。


6. 移動して **サービス プロバイダー**, とクリックし、 **フェデレーション詳細**します。 
<br><br>![Azure AD シングル サインオン][10] <br>


1. クリックして **フェデレーション メタデータのダウンロード**, 、お使いのコンピューターにメタデータ ファイルを保存します。
<br><br>![Azure AD シングル サインオン][11] <br>

3. Azure AD ポータルでの **SilkRoad ライフ スイート** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、 **シングル サインオンを構成する**  ダイアログ。
<br><br> ![シングル サインオンを構成します。][6] <br>

2.  **どのような SilkRoad ライフ スイートにサインオン** ] ページで、[ **Azure AD シングル サインオン**, 、] をクリックし、 **次**します。
<br><br> ![Azure AD シングル サインオン][7] <br>

3.  **アプリケーション設定の構成** ] ダイアログ ページで、次の手順を実行します。
<br><br>![Azure AD シングル サインオン][8] <br>
 
    a.  **サインオン URL** SilkRoad ライフ スイート サイトへのサインオンに、ユーザーが URL を使用] ボックスに、種類 (例:: *https://defcompanytest-test-redcarpet.silkroad-eng.com/Authentication/*)。

    b. 開く、ダウンロードした **Silkroad** メタデータ ファイルです。

    c. 検索、 **AssertionConsumerService** タグをし、コピー、 **場所** 属性です。         
    <br><br>![Azure AD シングル サインオン][21] <br>
   
    d. 値を貼り付けて、 **応答 URL** ] ボックスに貼り付けます。
 
    e. クリックして **次**します。
 
4.  **SilkRoad ライフ スイートでのシングル サインオンの構成** ] ページで、次の手順を実行します。
<br><br>![Azure AD シングル サインオン][9] <br>

    a. [証明書のダウンロード] をクリックし、コンピューターにファイルを保存します。

    b. クリックして **次**します。




1.  **SilkRoad** 、アプリケーションをクリックして **認証ソース**します。
<br><br>![Azure AD シングル サインオン][12] <br>



1. クリックして **認証ソースを追加する**です。 
<br><br>![Azure AD シングル サインオン][13] <br>



1.  **認証ソースの追加** セクションで、次の手順に従います。 
<br><br>![Azure AD シングル サインオン][14] <br>

    a. [ **オプション 2 - メタデータ ファイル**, 、] をクリックして **参照** ダウンロードしたメタデータ ファイルをアップロードします。

    b. クリックして **Id プロバイダーを作成するファイルのデータを使用して**します。



1.  **認証ソース** ] をクリックして **編集**します。 
<br><br>![Azure AD シングル サインオン][15] <br>


1.  **認証ソースの編集** ] ダイアログ ボックスで、次の手順を実行します。 
<br><br>![Azure AD シングル サインオン][16] <br>

    a. として **有効**, [ **はい**します。

    b.  **IdP 説明** ] ボックスには、構成の説明を入力 (例:: *Azure AD の SSO*)。

    c.  **IdP 名前** ] ボックスに、特定の構成には、名前を入力 (例:: *Azure SP*)。

    d. クリックして **保存**します。


6. その他のすべての認証ソースを無効にします。 
<br><br>![Azure AD Single Sign-On][17]<br>

7. Azure AD ポータルでの **シングル サインオンによる確認** ] ページで [ **次**します。  
  <br><br>![Azure AD シングル サインオン][18]

1.  **シングル サインオンによる確認** ] ページで [ **完了**します。
  <br><br>![Azure AD シングル サインオン][19]


### Azure AD のテスト ユーザーの作成
このセクションの目的は、Azure ポータルで Britta Simon というテスト ユーザーを作成することです。<br>
ユーザーの一覧で選択 **Britta Simon**します。<br><br>![Azure AD ユーザーを作成します。][20]<br>

**Azure AD でテスト ユーザーを作成するには、次の手順に従います。**

1.  **Azure 管理ポータル**, 、左側のナビゲーション ウィンドウで、次のようにクリックします。 **Active Directory**します。
<br><br>![Azure AD テスト ユーザーを作成します。](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_09.png) <br> 

2.  **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3. 上部のメニューで、ユーザーの一覧を表示する] をクリックして **ユーザー**します。
<br><br> ![Azure AD テスト ユーザーを作成します。](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_03.png) <br>
 
4. 開くには、 **ユーザーの追加** ] ダイアログ ボックスで、下部にあるツールバーをクリックして **ユーザーの追加**します。 
<br><br> ![Azure AD テスト ユーザーを作成します。](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_04.png) <br>

5.  **このユーザーについてお聞かせ** ] ダイアログ ページで、次の手順を実行します。 
<br><br> ![Azure AD テスト ユーザーを作成します。](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_05.png) <br> 

    a. [ユーザーの種類] として [組織内の新しいユーザー] を選択します。

    b. [ユーザー名 **textbox**, 、型 **BrittaSimon**します。

    c. クリックして **次**します。

6.   **ユーザー プロファイル** ] ダイアログ ページで、次の手順を実行します。 
<br><br>![Azure AD テスト ユーザーを作成します。](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_06.png) <br>
 
    a.  **名** ] ボックスに「 **Britta**します。  

    b.  **姓** ] ボックスに、型、 **Simon**します。

    c.  **表示名** ] ボックスに「 **Britta Simon**します。

    d.  **ロール** 一覧で、[ **ユーザー**します。
    e. クリックして **次**します。

7.  **一時パスワードの取得** ダイアログ ページで、をクリックして **作成**します。
<br><br> ![Azure AD テスト ユーザーを作成します。](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_07.png) <br>
 
8.  **一時パスワードの取得** ] ダイアログ ページで、次の手順を実行します。
<br><br>![Azure AD テスト ユーザーを作成します。](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_08.png) <br>
  
    a. 値を書き留めて、 **新しいパスワード**します。

    b. クリックして **完了**します。   

  
 
### SilkRoad Life Suite テスト ユーザーの作成

このセクションの目的は、SilkRoad Life Suite で Britta Simon というユーザーを作成することです。 Britta の SSO ID を持つ必要があります (とも呼ば、 *AuthParam*) Britta に一致する **emailaddress** Azure AD でします。

**SilkRoad Life Suite で Britta Simon というユーザーを作成するには、次の手順に従います。**

1. 持つユーザーを作成する、SilkRoad ライフ Suite サポート チームに質問する **SSO ID** 属性として同じ値、 **emailaddress** Azure AD の Britta Simon のです。



### Azure AD テスト ユーザーの割り当て

このセクションでは、Britta Simon SilkRoad ライフ スイートにアクセスを付与することによって Azure シングル サインオンの使用を有効にするとします。
<br><br>![ユーザーの割り当て][200] <br>

**SilkRoad Life Suite に Britta Simon を割り当てるには、次の手順に従います。**

1. Azure ポータルで、ディレクトリ ビューでアプリケーション ビューを開く] をクリックして **アプリケーション** 上部のメニュー。
<br><br>![ユーザーを割り当てる][201] <br>
2. アプリケーションの一覧で選択 **SilkRoad ライフ スイート**します。
<br><br>![ユーザーを割り当てる][202] <br>
1. 上部にあるメニュー [ **ユーザー**します。
<br><br>![ユーザーを割り当てる][203] <br>
1. ユーザーの一覧で選択 **Britta Simon**します。

2. 下部にあるツールバー [ **割り当てる**します。
<br><br>![ユーザーを割り当てる][205]



### シングル サインオンのテスト

このセクションでは、構成のテスト Azure AD シングル サインオン アクセス パネルを使用します。<br>
アクセス パネルで SilkRoad Life Suite のタイルをクリックすると、自動的に SilkRoad Life Suite アプリケーションにサインオンします。


## その他のリソース

* [SaaS アプリと Azure Active Directory を統合する方法に関するチュートリアルの一覧](active-directory-saas-tutorial-list.md)
* [Azure Active Directory のアプリケーション アクセスとシングル サインオンとは](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_01.png
[50]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_02.png

[6]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_05.png
[7]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_03.png
[8]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_04.png
[9]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_05.png
[10]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_06.png
[11]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_07.png
[12]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_08.png
[13]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_09.png
[14]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_10.png
[15]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_11.png
[16]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_12.png
[17]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_13.png
[18]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_06.png
[19]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_07.png


[20]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_100.png
[21]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_15.png


[200]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_14.png
[203]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_205.png







