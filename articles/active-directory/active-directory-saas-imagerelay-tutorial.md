<properties
    pageTitle="チュートリアル: Azure Active Directory と ImageRelay の統合 | Microsoft Azure"
    description="Azure Active Directory と ImageRelay の間でシングル サインオンを構成する方法について説明します。"
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
    ms.date="11/16/2015"
    ms.author="markusvi"/>



# チュートリアル: Azure Active Directory と ImageRelay の統合

このチュートリアルでは、Azure Active Directory (Azure AD) と ImageRelay を統合する方法を説明します。<br>Azure AD と統合する ImageRelay を使うと、次の利点があります。

- ImageRelay にアクセスする Azure AD ユーザーを制御できます。
- ユーザーが自分の Azure AD アカウントで自動的に ImageRelay にサインオン (シングル サインオン) できるようにします。
- 1 つの中央サイト (Azure Active Directory ポータル) でアカウントを管理できます。

Azure AD と SaaS アプリの統合に関する詳細を確認するを参照して [アプリケーションへのアクセスと Azure Active Directory でのシングル サインオン](active-directory-appssoaccess-whatis.md)します。

## 前提条件

Azure AD と ImageRelay の統合を構成するには、次のものが必要です。

- Azure AD サブスクリプション
- ImageRelay でのシングル サインオンが有効なサブスクリプション


> [AZURE.NOTE] このチュートリアルの手順をテストする場合、運用環境を使用しないことをお勧めします。


このチュートリアルの手順をテストするには、次の推奨事項に従ってください。

- 必要な場合を除き、運用環境は使用しないでください。
- Azure AD の評価環境を取得していない場合は、1 か月の試用版を入手できます [ここ](https://azure.microsoft.com/pricing/free-trial/)します。


## シナリオの説明

このチュートリアルでは、使用すると、テスト環境で Azure AD シングル サインオンをテストします。 <br>
このチュートリアルで紹介するシナリオは、2 つの主な構成要素で構成されます。

1. ギャラリーからの ImageRelay の追加

2. Azure AD シングル サインオンの構成とテスト


## ギャラリーからの ImageRelay の追加

Azure AD への ImageRelay の統合を構成するには、ギャラリーから管理対象 SaaS アプリの一覧に ImageRelay を追加する必要があります。

**ギャラリーから ImageRelay を追加するには、次の手順に従います。**

1. Azure ポータルの左側のナビゲーション ウィンドウで、[クリックして **Active Directory**します。 <br><br>
![Active Directory][1]<br>

2. **[ディレクトリ]** の一覧から、ディレクトリ統合を有効にするディレクトリを選択します。

3. ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。<br><br>
![アプリケーション][2]<br>
4. クリックして **追加** ページの下部にあります。<br><br>
![アプリケーション][3]<br>
5. **実行する操作** ダイアログ ボックスで、をクリックして **ギャラリーからアプリケーションを追加**します。<br><br>
![アプリケーション][4]<br>
6. 検索ボックスに入力 **ImageRelay**します。<br><br>
![Azure AD テスト ユーザーを作成します。](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_01.png)<br>
7. 結果ウィンドウで [ **ImageRelay**, 、クリックして **完了** アプリケーションを追加します。
<br><br>

## Azure AD シングル サインオンの構成とテスト

このセクションの目的は、"Britta Simon" というテスト ユーザーに基づいて、ImageRelay で Azure AD のシングル サインオンを構成し、テストする方法について説明することです。

シングル サインオンを機能させるには、Azure AD で ImageRelay の関連ユーザーを表すユーザー アカウントが必要です。つまり、Azure AD のユーザーと ImageRelay に関連するユーザーの間のリンクの関係を確立する必要があります。<br>
このリンク関係は、Azure AD の **[ユーザー名]** の値を、ImageRelay の **[Username]** の値として割り当てることで確立されます。

ImageRelay で Azure AD のシングル サインオンを構成してテストするには、次の構成要素を完了する必要があります。

1. * *[構成する Azure AD シングル サインオン](#configuring-azure-ad-single-single-sign-on)* * - この機能を使用するユーザーをできるようにします。
2. * *[Azure AD テスト ユーザーを作成する](#creating-an-azure-ad-test-user)* * - を Azure AD シングル サインオン Britta Simon でテストします。
4. * *[ImageRelay テスト ユーザーを作成する](#creating-a-userecho-test-user)* * - 自分の Azure AD の表現にリンクされている ImageRelay に Britta Simon に対応します。
5. * *[Azure AD テスト ユーザーの割り当て](#assigning-the-azure-ad-test-user)* * - Azure AD シングル サインオンを使用する Britta Simon をできるようにします。
5. * *[シングル サインオンをテスト](#testing-single-sign-on)* * - 構成が機能するかどうかを確認します。

### Azure AD シングル サインオンの構成

このセクションの目的は、Azure AD ポータルで Azure AD のシングル サインオンを有効にすることと、ImageRelay アプリケーションでシングル サインオンを構成することです。


**ImageRelay で Azure AD シングル サインオンを構成するには、次の手順に従います。**

1. Azure AD ポータルの **ImageRelay** アプリケーション統合ページで **[シングル サインオンの構成]** をクリックし、**[シングル サインオンの構成]** ダイアログを開きます。

     ![でのシングル サインオンを構成する][6] <br>

2. **[ユーザーの ImageRelay へのアクセスを設定してください]** ページで、**[Microsoft Azure AD シングル サインオン]** を選択し、**[次へ]** をクリックします。

    ![でのシングル サインオンを構成する](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_03.png) <br>

3. **[アプリケーション設定の構成]** ダイアログ ページで、次の手順に従います。

     ![でのシングル サインオンを構成する](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_04.png) <br>

    a.  **サインオン URL** ImageRelay アプリケーションにサインオンする URL は、ユーザーが使用] ボックスに、種類 (例: *https://fabrikam.ImageRelay.com/*)。

    b. **[次へ]** をクリックします。

4. **[ImageRelay でのシングル サインオンの構成]** ページで、次の手順を実行します。

    ![でのシングル サインオンを構成する](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_05.png) <br>

    a. **[証明書のダウンロード]** をクリックし、コンピューターにファイルを保存します。

    b. **[次へ]** をクリックします。

5. 別のブラウザー ウィンドウで、管理者として ImageRelay 企業サイトにサインインします。

    a.上部にあるツールバーで、クリックして、 **ユーザーおよびアクセス許可** ワークロード<br>。<br>![でのシングル サインオンを構成する](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_06.png) <br>

    b.クリックして **新しいアクセス許可を作成**.<br><br>![でのシングル サインオンを構成する](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_08.png) <br>

    c. **Single Sign On Settings** ワークロードで、 **のみサインインでシングル サインオンを使用してこのグループができる** チェック ボックスをオンにし **保存**.<br><br>![でのシングル サインオンを構成する](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_09.png) <br>

    d.移動して **アカウント設定**.<br><br>![でのシングル サインオンを構成する](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_10.png) <br>

    e.移動して、 **Single Sign On Settings** ワークロード<br>。<br>![でのシングル サインオンを構成する](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_11.png)<br>

    f.示されるように、フォームに記入し、 **保存**.<br><br>![でのシングル サインオンを構成する](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_12.png)<br>

    - **ログイン URL (SSO)**: Azure Active Directory からシングル サインオン サービス url<br>。<br>![でのシングル サインオンを構成する](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_13.png)<br>

    - **ログアウト サービス URL**: Azure Active Directory からシングル サインアウト サービスである<br>。<br>![でのシングル サインオンを構成する](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_14.png)<br>

    - **名前 Id 形式**, [ **urn oasis: 名: 対象の形式: emailAddress**.<br><br>![でのシングル サインオンを構成する](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_15.png)<br>

    - **サービス プロバイダー (イメージ リレー) からの要求のオプションのバインド**, [ **POST バインディング**.<br><br>![でのシングル サインオンを構成する](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_16.png)<br>

    - **X.509 証明書**, 、クリックして **証明書の更新**.<br><br>![でのシングル サインオンを構成する](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_17.png)<br>

    - メモ帳で、Azure Active Directory から、手順 4. でダウンロードした証明書を開き、コピーして、ここで、証明書の内容を貼り付けます。<br><br>![でのシングル サインオンを構成する](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_18.png)<br>

    - **Just-In-Time ユーザー プロビジョニング**, を選択、 **Just-In-Time ユーザー プロビジョニングを有効にする** チェック ボックスをオンします<br>。<br>![でのシングル サインオンを構成する](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_19.png)<br>

    - アクセス許可グループを選択します (たとえば、 **SSO 基本的な**) でのシングル サインオンを介してのみサインインが許可されます<br>。<br>![でのシングル サインオンを構成する](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_20.png)<br>

6. Azure AD ポータルで、シングル サインオンの構成確認を選択し、**[次へ]** をクリックします。

    ![Azure AD シングル サインオン][10]<br>

7. **[シングル サインオンの確認]** ページで **[完了]** をクリックします。

    ![Azure AD のシングル サインオン][11]


### Azure AD のテスト ユーザーの作成

このセクションでは、Britta Simon と呼ばれる Azure ポータルでテスト ユーザーを作成します。<br>
ユーザーの一覧で選択 **Britta Simon**.<br><br>![Azure AD ユーザーを作成][20]<br>

**Azure AD でテスト ユーザーを作成するには、次の手順に従います。**

1. **Azure ポータル**, 、左側のナビゲーション ウィンドウで、次のようにクリックします **Active Directory**。<br>。<br>![Azure AD テスト ユーザーを作成する](./media/active-directory-saas-imagerelay-tutorial/create_aaduser_09.png) <br>

2. **[ディレクトリ]** の一覧から、ディレクトリ統合を有効にするディレクトリを選択します。

3. 上部のメニューで、ユーザーの一覧を表示する] をクリックして **ユーザー**.<br><br>![Azure AD テスト ユーザーを作成する](./media/active-directory-saas-imagerelay-tutorial/create_aaduser_03.png) <br>

4. 開くには、 **ユーザーの追加** ] ダイアログ ボックスで、下部にあるツールバーをクリックして **ユーザーの追加**.<br><br>![Azure AD テスト ユーザーを作成する](./media/active-directory-saas-imagerelay-tutorial/create_aaduser_04.png) <br>

5. **このユーザーについてお聞かせ** ] ダイアログ ページで、次の手順を実行します<br><br>![Azure AD テスト ユーザーを作成する](./media/active-directory-saas-imagerelay-tutorial/create_aaduser_05.png) 。<br>

    a. [ユーザーの種類] として [組織内の新しいユーザー] を選択します。

    b. **[ユーザー名]** ボックスに「**BrittaSimon**」と入力します。

    c. **[次へ]** をクリックします。

6.  **ユーザー プロファイル** ] ダイアログ ページで、次の手順を実行します<br><br>![Azure AD テスト ユーザーを作成する](./media/active-directory-saas-imagerelay-tutorial/create_aaduser_06.png) 。<br>

    a. **[名]** ボックスに「**Britta**」と入力します。

    b. **[姓]** ボックスに「**Simon**」と入力します。

    c. **[表示名]** ボックスに「**Britta Simon**」と入力します。

    d. **[ロール]** 一覧で **[ユーザー]** を選択します。

    e. **[次へ]** をクリックします。

7. **一時パスワードの取得** ダイアログ ページで、をクリックして **作成**.<br><br>![Azure AD テスト ユーザーを作成する](./media/active-directory-saas-imagerelay-tutorial/create_aaduser_07.png) <br>

8. **一時パスワードの取得** ] ダイアログ ページで、次の手順を実行します<br><br>![Azure AD テスト ユーザーを作成する](./media/active-directory-saas-imagerelay-tutorial/create_aaduser_08.png) 。<br>

    a. **[新しいパスワード]** の値を書き留めます。

    b. **[完了]** をクリックします。



### ImageRelay テスト ユーザーの作成

このセクションの目的は、ImageRelay で Britta Simon というユーザーを作成することです。

**ImageRelay で Britta Simon というユーザーを作成するには、次の手順に従います。**

1. ImageRelay 企業サイトに管理者としてログインします。

1. 移動 **ユーザーおよびアクセス許可**    選択 **SSO ユーザーの作成**.<br><br>![でのシングル サインオンを構成する](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_21.png) <br>

1. 入力、 **電子メール**, 、**名**, 、**姓** と **会社** プロビジョニングし、アクセス許可グループ (たとえば、SSO 基本) でのシングル サインオンを介してのみサインインできるグループを選択するユーザー<br>。<br>![でのシングル サインオンを構成する](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_22.png) <br>

1. **[作成]** をクリックします。

### Azure AD テスト ユーザーの割り当て

このセクションでは、Britta Simon ImageRelay に自分のアクセスを与えることで Azure シングル サインオンの使用を有効にするとします。
<br><br>![ユーザーを割り当てる][200] <br>

**ImageRelay に Britta Simon を割り当てるには、次の手順に従います。**

1. Azure ポータルで、ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー<br>。<br>![Assign User][201] <br>

2. アプリケーションの一覧で選択 **ImageRelay**.<br><br>![でのシングル サインオンを構成する](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_23.png) <br>

1. 上部にあるメニュー [ **ユーザー**.<br><br>![Assign User][203] <br>

1. ユーザーの一覧で **[Britta Simon]** を選択します。

2. 下部にあるツールバー [ **割り当てる**.<br><br>![Assign User][205]


### シングル サインオンのテスト

このセクションでは、構成のテスト Azure AD シングル サインオン アクセス パネルを使用します。<br>
アクセス パネルで [ImageRelay] タイルをクリックすると、自動的に ImageRelay アプリケーションにサインオンします。


## その他のリソース

* [Azure Active Directory による SaaS アプリを統合する方法に関するチュートリアルの一覧](active-directory-saas-tutorial-list.md)
* [アプリケーション アクセスと Azure Active Directory でのシングル サインオンとは](active-directory-appssoaccess-whatis.md)





[1]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_01.png 
[2]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_02.png 
[3]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_03.png 
[4]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_04.png 
[6]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_05.png 
[10]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_06.png 
[11]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_07.png 
[20]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_100.png 
[200]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_200.png 
[201]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_201.png 
[203]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_203.png 
[204]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_204.png 
[205]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_205.png 

