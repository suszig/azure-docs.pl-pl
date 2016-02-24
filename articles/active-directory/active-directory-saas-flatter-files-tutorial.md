<properties
    pageTitle="チュートリアル: Azure Active Directory と Flatter Files の統合 | Microsoft Azure"
    description="Azure Active Directory と Flatter Files の間でシングル サインオンを構成する方法について説明します。"
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


# チュートリアル: Azure Active Directory と Flatter Files の統合

このチュートリアルでは、フラット ファイルを Azure Active Directory (Azure AD) に統合する方法について説明します。<br>フラット ファイルを Azure AD と統合するには、次のメリットがあります。 

- Flatter Files にアクセスする Azure AD ユーザーを制御できます。 
- ユーザーが自分の Azure AD アカウントで自動的に Flatter Files にサインオン (シングル サインオン) できるようにします。
- 1 つの中央サイト (Azure Active Directory ポータル) でアカウントを管理できます。

Azure AD と SaaS アプリの統合に関する詳細を確認するを参照して [アプリケーションへのアクセスと Azure Active Directory でのシングル サインオン](active-directory-appssoaccess-whatis.md)します。

## 前提条件 

Flatter Files と Azure AD の統合を構成するには、次のものが必要です。

- Azure AD サブスクリプション
- Flatter Files でのシングル サインオンが有効なサブスクリプション


> [AZURE.NOTE] このチュートリアルでは、手順をテストするには、実稼働環境の使用をおしないでいます。


このチュートリアルの手順をテストするには、次の推奨事項に従ってください。

- 必要な場合を除き、運用環境は使用しないでください。
- Azure AD の評価環境を取得していない場合は、1 か月の試用版を入手できます [ここ](https://azure.microsoft.com/pricing/free-trial/)します。 

 
## シナリオの説明
このチュートリアルの目的は、テスト環境で Azure AD のシングル サインオンをテストできるようにすることです。 <br>
このチュートリアルで紹介するシナリオは、次の 3 つの主な構成要素で構成されます。

1. ギャラリーからの Flatter Files の追加 
2. Azure AD シングル サインオンの構成とテスト


## ギャラリーからの Flatter Files の追加
Azure AD への Flatter Files の統合を構成するには、ギャラリーから管理対象 SaaS アプリの一覧に Flatter Files を追加する必要があります。

**ギャラリーから Flatter Files を追加するには、次の手順に従います。**

1.  **Azure 管理ポータル**, 、左側のナビゲーション ウィンドウで、次のようにクリックします。 **Active Directory**します。 <br><br>
![Active Directory][1]<br>

2.  **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3. ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。<br><br>
![アプリケーション][2]<br>
4. クリックして **追加** ページの下部にあります。<br><br>
![アプリケーション][3]<br>
5.  **実行する操作** ダイアログ ボックスで、をクリックして **ギャラリーからアプリケーションを追加**します。<br><br>
![アプリケーション][4]<br>
6. 検索ボックスに入力 **フラット ファイル**します。<br><br>
![Azure AD テスト ユーザーを作成します。](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_01.png)<br>
7. 結果ウィンドウで [ **フラット ファイル**, 、クリックして **完了** アプリケーションを追加します。<br><br>
![Azure AD テスト ユーザーを作成します。](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_500.png)<br>

##  Azure AD シングル サインオンの構成とテスト
このセクションの目的は、"Britta Simon" というテスト ユーザーに基づいて、Flatter Files で Azure AD のシングル サインオンを構成し、テストする方法について説明することです。

シングル サインオンを機能させるには、Azure AD ユーザーに対応する Flatter Files ユーザーが Azure AD で認識されている必要があります。 つまり、Azure AD のユーザーとフラット ファイルに関連するユーザーの間のリンクの関係を確立する必要があります。<br>
値を割り当てることでこのリンクの関係が確立される、 **ユーザー名** の値として Azure AD で、 **Username** フラット ファイルにします。
 
Flatter Files で Azure AD のシングル サインオンを構成してテストするには、次の構成要素を完了する必要があります。

1. **[Azure AD シングル サインオンを構成する](#configuring-azure-ad-single-single-sign-on)** - この機能を使用するユーザーをできるようにします。
2. **[Azure AD テスト ユーザーを作成する](#creating-an-azure-ad-test-user)** - を Azure AD シングル サインオン Britta Simon でテストします。
4. **[フラット ファイルのテスト ユーザーを作成する](#creating-a-halogen-software-test-user)** - 自分の Azure AD の表現にリンクされているフラット ファイルに Britta Simon に対応します。
5. **[Azure AD テスト ユーザーの割り当て](#assigning-the-azure-ad-test-user)** - Azure AD シングル サインオンを使用する Britta Simon をできるようにします。
5. **[シングル サインオン テスト](#testing-single-sign-on)** - 構成が機能するかどうかを確認します。

### Azure AD シングル サインオンの構成

このセクションでは、Azure AD シングル サインオン Azure AD ポータルで有効にし、フラット ファイルのアプリケーションでシングル サインオンを構成するにです。 この手順の途中で、base-64 でエンコードされた証明書ファイルを作成する必要があります。 この手順に慣れていない場合は、次を参照してください。 [バイナリ証明書をテキスト ファイルに変換する方法](http://youtu.be/PlgrzUZ-Y1o)します。

Flatter Files のシングル サインオンを構成するには、登録済みのドメインが必要です。 フラット ファイルにサポートを使用してチームが、登録済みドメインまだ、連絡先を持っていない場合 [support@flatterfiles.com](mailto:support@flatterfiles.com)します。  



**Flatter Files で Azure AD シングル サインオンを構成するには、次の手順に従います。**

1. Azure AD ポータルでの **フラット ファイル** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、 **シングル サインオンを構成する**  ダイアログ。
<br><br> ![でのシングル サインオンを構成する][6] <br>

2.  **どのようなフラット ファイルへのサインオン** ] ページで、[ **Azure AD シングル サインオン**, 、] をクリックし、 **次**します。
<br><br> ![でのシングル サインオンを構成する](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_02.png) <br>

3.  **アプリケーション設定の構成** ダイアログ ページで、をクリックして **次**します。
<br><br>![シングル サインオンを構成する](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_03.png) <br>

    > [AZURE.NOTE] フラット ファイルは、すべての顧客に対して同じ SSO ログイン URL を使用して: [https://www.flatterfiles.com/site/login/sso/](https://www.flatterfiles.com/site/login/sso/)します。
.
 
 
4.  **フラット ファイルでのシングル サインオンの構成** ] ページで、次の手順を実行します。
<br><br>![シングル サインオンを構成します。](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_04.png)  <br>

    a. クリックして **証明書のダウンロード**, 、お使いのコンピューター上のファイルを保存します。

    b. クリックして **次**します。


1. 管理者として Flatter Files アプリケーションにサインオンします。

2. [ダッシュボード] をクリックします。 
<br><br>![Configure Single Sign-On](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_05.png)  <br>



2. をクリックして **設定**, で次の手順を実行し、 **会社** ] タブをクリックします。 
<br><br>![シングル サインオンを構成します。](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_06.png)  <br>

    a. 選択 **SAML 2.0 を使用して認証**します。

    b. クリックして **SAML 構成**します。



2.  **SAML Configuration** ] ダイアログ ボックスで、次の手順を実行します。 
<br><br>![シングル サインオンを構成します。](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_08.png)  <br>

    a. [Domain] ボックスに、登録済みのドメインを入力します。

    > [AZURE.NOTE] フラット ファイルにサポートを使用してチームが、登録済みドメインまだ、連絡先を持っていない場合 [support@flatterfiles.com](mailto:support@flatterfiles.com)します。
    
    b. Azure ポータルでの構成で単一のフラット ファイル] ダイアログ ボックス、copt でサインオン、シングル サインオン サービス URL、および Id プロバイダーの URL] ボックスに貼り付けます。

    c.  作成、 **base-64 でエンコードされた** からダウンロードした証明書ファイル。  

    >[AZURE.TIP] 詳細については、次を参照してください [バイナリ証明書をテキスト ファイルに変換する方法。](http://youtu.be/PlgrzUZ-Y1o)

    d.  Base-64 でエンコードされた証明書をメモ帳で開き、という内容がクリップボードにコピーおよびに貼り付け、 **FlatterFiles Identity Provider Certificate** ] ボックスに貼り付けます。

    e. クリックして **更新**します。

6. Azure AD ポータルで、シングル サインオンの構成情報を選択し、をクリックし、 **次**します。 
<br><br>![Azure AD シングル サインオン][10]<br>

7.  **シングル サインオンによる確認** ] ページで [ **完了**します。  
  <br><br>![Azure AD シングル サインオン][11]




### Azure AD のテスト ユーザーの作成
このセクションの目的は、Azure ポータルで Britta Simon というテスト ユーザーを作成することです。<br>
ユーザーの一覧で選択 **Britta Simon**.<br><br>![Azure AD ユーザーを作成します。][20]<br>

**Azure AD でテスト ユーザーを作成するには、次の手順に従います。**

1.  **Azure 管理ポータル**, 、左側のナビゲーション ウィンドウで、次のようにクリックします。 **Active Directory**します。
<br><br>![Azure AD テスト ユーザーを作成する](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_09.png) <br> 

2.  **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3. 上部のメニューで、ユーザーの一覧を表示する] をクリックして **ユーザー**します。
<br><br> ![Azure AD テスト ユーザーを作成する](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_03.png) <br>
 
4. 開くには、 **ユーザーの追加** ] ダイアログ ボックスで、下部にあるツールバーをクリックして **ユーザーの追加**します。 
<br><br> ![Azure AD テスト ユーザーを作成する](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_04.png) <br>

5.  **このユーザーについてお聞かせ** ] ダイアログ ページで、次の手順を実行します。 
<br><br> ![Azure AD テスト ユーザーを作成する](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_05.png) <br> 

    a. [ユーザーの種類] として [組織内の新しいユーザー] を選択します。

    b. [ユーザー名 **textbox**, 、型 **BrittaSimon**します。

    c. クリックして **次**します。

6.   **ユーザー プロファイル** ] ダイアログ ページで、次の手順を実行します。 
<br><br>![Azure AD テスト ユーザーを作成する](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_06.png) <br>
 
    a.  **名** ] ボックスに「 **Britta**します。  

    b.  **姓** ] ボックスに、型、 **Simon**します。

    c.  **表示名** ] ボックスに「 **Britta Simon**します。

    d.  **ロール** 一覧で、[ **ユーザー**します。
    e. クリックして **次**します。

7.  **一時パスワードの取得** ダイアログ ページで、をクリックして **作成**します。
<br><br> ![Azure AD テスト ユーザーを作成する](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_07.png) <br>
 
8.  **一時パスワードの取得** ] ダイアログ ページで、次の手順を実行します。
<br><br>![Azure AD テスト ユーザーを作成する](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_08.png) <br>
  
    a. 値を書き留めて、 **新しいパスワード**します。

    b. クリックして **完了**します。   

  
 
### Flatter Files テスト ユーザーの作成

このセクションの目的は、Flatter Files で Britta Simon というユーザーを作成することです。

**Flatter Files で Britta Simon というユーザーを作成するには、次の手順に従います。**

1. サインイン、 **フラット ファイル** 会社サイトに管理者として。

2. 左側のナビゲーション ウィンドウでをクリックして **設定**, 、ユーザーをクリックして **] タブ**です。
<br><br>![Cfreate フラット ファイルのユーザー](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_09.png)<br>

3. クリックして **ユーザーを追加**します。 

4.  **ユーザーの追加** ] ダイアログ ボックスで、次の手順を実行します。
<br><br>![Cfreate フラット ファイルのユーザー](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_10.png)<br>

    a.  **名** ] ボックスに「 **Britta**します。

    b.  **姓** ] ボックスに「 **Simon**します。 

    c.  **電子メール アドレス** ] ボックスに、Azure ポータルで Britta の電子メール アドレスを入力します。

    c. クリックして **送信**します。   


### Azure AD テスト ユーザーの割り当て

このセクションでは、フラット ファイルへのアクセスを付与することによって Azure シングル サインオンを使用する Britta Simon を有効にするとします。
<br><br>![ユーザーの割り当て][200] <br>

**Flatter Files に Britta Simon を割り当てるには、次の手順に従います。**

1. Azure ポータルで、ディレクトリ ビューでアプリケーション ビューを開く] をクリックして **アプリケーション** 上部のメニュー。
<br><br>![ユーザーを割り当てる][201] <br>
2. アプリケーションの一覧で選択 **フラット ファイル**します。
<br><br>![ユーザーを割り当てる](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_11.png) <br>
1. 上部にあるメニュー [ **ユーザー**します。
<br><br>![ユーザーを割り当てる][203] <br>
1. ユーザーの一覧で選択 **Britta Simon**します。

2. 下部にあるツールバー [ **割り当てる**します。
<br><br>![ユーザーを割り当てる][205]



### シングル サインオンのテスト

このセクションでは、構成のテスト Azure AD シングル サインオン アクセス パネルを使用します。<br>
アクセス パネルで [Flatter Files] タイルをクリックすると、自動的に Flatter Files アプリケーションにサインオンします。


## その他のリソース

* [SaaS アプリと Azure Active Directory を統合する方法に関するチュートリアルの一覧](active-directory-saas-tutorial-list.md)
* [Azure Active Directory のアプリケーション アクセスとシングル サインオンとは](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_205.png







