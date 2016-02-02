<properties
    pageTitle="チュートリアル: フラット ファイルと Azure Active Directory の統合 |Microsoft Azure"
    description="Azure Active Directory とフラット ファイルの間でのシングル サインオンを構成する方法について説明します。"
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



# チュートリアル: フラット ファイルと Azure Active Directory の統合

このチュートリアルでは、フラット ファイルを Azure Active Directory (Azure AD) に統合する方法について説明します。<br>平坦化のファイルを Azure AD と統合することを使うと、次の利点があります。

- フラット ファイルへのアクセス権を持つ Azure AD で制御できます。
- Azure AD のアカウントで自動的に署名のフラット ファイル (シングル サインオン) するユーザーを有効にすることができます。
- 1 つの中央サイト (Azure Active Directory ポータル) でアカウントを管理できます。

Azure AD と SaaS アプリの統合に関する詳細を確認するを参照して [アプリケーションへのアクセスと Azure Active Directory でのシングル サインオン](active-directory-appssoaccess-whatis.md)します。

## 前提条件

フラット ファイルを使用して Azure AD の統合を構成するには、次の項目が必要です。

- Azure AD サブスクリプション
- 有効なサブスクリプションで、フラット ファイル シングル サインオン


> [AZURE.NOTE] このチュートリアルの手順をテストする場合、運用環境を使用しないことをお勧めします。


このチュートリアルの手順をテストするには、次の推奨事項に従ってください。

- 必要な場合を除き、運用環境は使用しないでください。
- Azure AD の評価環境を取得していない場合は、1 か月の試用版を入手できます [ここ](https://azure.microsoft.com/pricing/free-trial/)します。


## シナリオの説明

このチュートリアルでは、使用すると、テスト環境で Azure AD シングル サインオンをテストします。 <br>
このチュートリアルで紹介するシナリオは、次の 3 つの主な構成要素で構成されます。

1. ギャラリーからの Flatter Files の追加
2. Azure AD シングル サインオンの構成とテスト


## ギャラリーからの Flatter Files の追加

フラット ファイルの Azure AD への統合を構成するのには、管理対象 SaaS アプリの一覧に、ギャラリーからフラット ファイルを追加する必要があります。

**ギャラリーからフラット ファイルを追加するには、次の手順を実行します。**

1. **Azure 管理ポータル**, 、左側のナビゲーション ウィンドウで、次のようにクリックします。 **Active Directory**します。 <br><br>
![Active Directory][1]<br>

2. **[ディレクトリ]** の一覧から、ディレクトリ統合を有効にするディレクトリを選択します。

3. ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。<br><br>
![アプリケーション][2]<br>
4. クリックして **追加** ページの下部にあります。<br><br>
![アプリケーション][3]<br>
5. **実行する操作** ダイアログ ボックスで、をクリックして **ギャラリーからアプリケーションを追加**します。<br><br>
![アプリケーション][4]<br>
6. 検索ボックスに入力 **フラット ファイル**します。<br><br>
![Azure AD テスト ユーザーを作成します。](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_01.png)<br>
7. 結果ウィンドウで [ **フラット ファイル**, 、クリックして **完了** アプリケーションを追加します。<br><br>
![Azure AD テスト ユーザーを作成します。](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_500.png)<br>

## Azure AD シングル サインオンの構成とテスト

このセクションでは、構成し、"Britta Simon"というテスト ユーザーに基づいて、フラット ファイルを Azure AD シングル サインオンをテストする方法を説明します。

シングル サインオンの動作する、Azure AD はユーザーは、ユーザーが Azure AD でフラット ファイルの対応するを把握する必要があります。つまり、Azure AD のユーザーとフラット ファイルに関連するユーザーの間のリンクの関係を確立する必要があります。<br>
値を割り当てることでこのリンクの関係が確立される、 **ユーザー名** の値として Azure AD で、 **Username** フラット ファイルにします。

構成して、フラット ファイルを Azure AD シングル サインオン テストを次の要素を完了する必要があります。

1. * *[構成する Azure AD シングル サインオン](#configuring-azure-ad-single-single-sign-on)* * - この機能を使用するユーザーをできるようにします。
2. * *[Azure AD テスト ユーザーを作成する](#creating-an-azure-ad-test-user)* * - を Azure AD シングル サインオン Britta Simon でテストします。
4. * *[フラット ファイルのテスト ユーザーを作成する](#creating-a-halogen-software-test-user)* * - 自分の Azure AD の表現にリンクされているフラット ファイルに Britta Simon に対応します。
5. * *[Azure AD テスト ユーザーの割り当て](#assigning-the-azure-ad-test-user)* * - Azure AD シングル サインオンを使用する Britta Simon をできるようにします。
5. * *[シングル サインオンをテスト](#testing-single-sign-on)* * - 構成が機能するかどうかを確認します。

### Azure AD シングル サインオンの構成

このセクションでは、Azure AD シングル サインオン Azure AD ポータルで有効にし、フラット ファイルのアプリケーションでシングル サインオンを構成するにです。 この手順の途中で、base-64 でエンコードされた証明書ファイルを作成する必要があります。 この手順に慣れていない場合は、次を参照してください。 [バイナリ証明書をテキスト ファイルに変換する方法](http://youtu.be/PlgrzUZ-Y1o)します。

Flatter Files のシングル サインオンを構成するには、登録済みのドメインが必要です。 フラット ファイルにサポートを使用してチームが、登録済みドメインまだ、連絡先を持っていない場合 [support@flatterfiles.com](mailto:support@flatterfiles.com)します。



**フラット ファイルを Azure AD シングル サインオンを構成するには、次の手順を実行します。**

1. Azure AD ポータルでの **フラット ファイル** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、 **シングル サインオンを構成する**  ダイアログ。
<br><br> ![でのシングル サインオンを構成する][6] <br>

2. **どのようなフラット ファイルへのサインオン** ] ページで、[ **Azure AD シングル サインオン**, 、] をクリックし、 **次**します。
<br><br> ![でのシングル サインオンを構成する](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_02.png) <br>

3. **アプリケーション設定の構成** ダイアログ ページで、をクリックして **次**します。
<br><br>![でのシングル サインオンを構成する](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_03.png) <br>
    > [AZURE.NOTE] フラット ファイルは、すべての顧客に対して同じ SSO ログイン URL を使用して: [https://www.flatterfiles.com/site/login/sso/](https://www.flatterfiles.com/site/login/sso/)します。
.

4. **フラット ファイルでのシングル サインオンの構成** ] ページで、次の手順を実行します。
<br><br>![シングル サインオンを構成します。](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_04.png)  <br>

    a. **[証明書のダウンロード]** をクリックし、コンピューターにファイルを保存します。

    b. **[次へ]** をクリックします。

1. サインオン管理者として、フラット ファイルのアプリケーションにします。

2. [ダッシュボード] をクリックします。 
<br><br>![シングル サインオンを構成します。](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_05.png)  <br>

2. をクリックして **設定**, で次の手順を実行し、 **会社** ] タブをクリックします。 
<br><br>![シングル サインオンを構成します。](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_06.png)  <br>

    a. 選択 **SAML 2.0 を使用して認証**します。

    b. クリックして **SAML 構成**します。

2. **SAML Configuration** ] ダイアログ ボックスで、次の手順を実行します。 
<br><br>![シングル サインオンを構成します。](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_08.png)  <br>

    a. [Domain] ボックスに、登録済みのドメインを入力します。
    > [AZURE.NOTE] フラット ファイルにサポートを使用してチームが、登録済みドメインまだ、連絡先を持っていない場合 [support@flatterfiles.com](mailto:support@flatterfiles.com)します。

    b. Azure ポータルでの構成で単一のフラット ファイル] ダイアログ ボックス、copt でサインオン、シングル サインオン サービス URL、および Id プロバイダーの URL] ボックスに貼り付けます。

    c. ダウンロードした証明書から **base-64 でエンコードされた**ファイルを作成します。
    >[AZURE.TIP] 詳細については、「 [バイナリ証明書をテキスト ファイルに変換する方法](http://youtu.be/PlgrzUZ-Y1o)

    d. Base-64 でエンコードされた証明書をメモ帳で開き、という内容がクリップボードにコピーおよびに貼り付け、 **FlatterFiles Identity Provider Certificate** ] ボックスに貼り付けます。

    e. **[Update]** をクリックします。

6. Azure AD ポータルで、シングル サインオンの構成確認を選択し、**[次へ]** をクリックします。 
<br><br>![Azure AD シングル サインオン][10]<br>

7. **[シングル サインオンの確認]** ページで **[完了]** をクリックします。  
  <br><br>![Azure AD シングル サインオン][11]




### Azure AD のテスト ユーザーの作成

このセクションでは、Britta Simon と呼ばれる Azure ポータルでテスト ユーザーを作成します。<br>
ユーザーの一覧で選択 **Britta Simon**.<br><br>![Azure AD ユーザーを作成][20]<br>

**Azure AD でテスト ユーザーを作成するには、次の手順に従います。**

1. **Microsoft Azure 管理ポータル**の左側のナビゲーション ウィンドウで、**[Active Directory]** をクリックします。
<br><br>![Azure AD テスト ユーザーを作成する](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_09.png) <br>

2. **[ディレクトリ]** の一覧から、ディレクトリ統合を有効にするディレクトリを選択します。

3. 上部のメニューで、ユーザーの一覧を表示する] をクリックして **ユーザー**します。
<br><br> ![Azure AD テスト ユーザーを作成する](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_03.png) <br>

4. 開くには、 **ユーザーの追加** ] ダイアログ ボックスで、下部にあるツールバーをクリックして **ユーザーの追加**します。 
<br><br> ![Azure AD テスト ユーザーを作成する](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_04.png) <br>

5. **このユーザーについてお聞かせ** ] ダイアログ ページで、次の手順を実行します。 
<br><br> ![Azure AD テスト ユーザーを作成する](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_05.png) <br>

    a. [ユーザーの種類] として [組織内の新しいユーザー] を選択します。

    b. **[ユーザー名]** ボックスに「**BrittaSimon**」と入力します。

    c. **[次へ]** をクリックします。

6.  **ユーザー プロファイル** ] ダイアログ ページで、次の手順を実行します。 
<br><br>![Azure AD テスト ユーザーを作成する](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_06.png) <br>

    a. **[名]** ボックスに「**Britta**」と入力します。

    b. **[姓]** ボックスに「**Simon**」と入力します。

    c. **[表示名]** ボックスに「**Britta Simon**」と入力します。

    d. **[ロール]** 一覧で **[ユーザー]** を選択します。
    e. **[次へ]** をクリックします。

7. **一時パスワードの取得** ダイアログ ページで、をクリックして **作成**します。
<br><br> ![Azure AD テスト ユーザーを作成する](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_07.png) <br>

8. **一時パスワードの取得** ] ダイアログ ページで、次の手順を実行します。
<br><br>![Azure AD テスト ユーザーを作成する](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_08.png) <br>

    a. **[新しいパスワード]** の値を書き留めます。

    b. **[完了]** をクリックします。



### Flatter Files テスト ユーザーの作成

このセクションでは、フラット ファイルで Britta Simon をというユーザーを作成します。

**フラット ファイルで Britta Simon をというユーザーを作成するには、次の手順を実行します。**

1. サインイン、 **フラット ファイル** 会社サイトに管理者として。

2. 左側のナビゲーション ウィンドウでをクリックして **設定**, 、ユーザーをクリックして **] タブ**です。
<br><br>![Cfreate フラット ファイルのユーザー](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_09.png)<br>

3. **[ユーザーの追加]** をクリックします。

4. **[Add User]** ダイアログで、次の手順を実行します。
<br><br>![Cfreate フラット ファイルのユーザー](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_10.png)<br>

    a. **[名]** ボックスに「**Britta**」と入力します。

    b. **[Last Name]** ボックスに「**Simon**」と入力します。

    c.  **電子メール アドレス** ] ボックスに、Azure ポータルで Britta の電子メール アドレスを入力します。

    c. **[Submit]** をクリックします。


### Azure AD テスト ユーザーの割り当て

このセクションでは、フラット ファイルへのアクセスを付与することによって Azure シングル サインオンを使用する Britta Simon を有効にするとします。
<br><br>![ユーザーを割り当てる][200] <br>

**フラット ファイルに Britta Simon を割り当てるには、次の手順を実行します。**

1. Azure ポータルで、ディレクトリ ビューでアプリケーション ビューを開く] をクリックして **アプリケーション** 上部のメニュー。
<br><br>![ユーザーを割り当てる][201] <br>
2. アプリケーションの一覧で選択 **フラット ファイル**します。
<br><br>![ユーザーを割り当てる](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_11.png) <br>
1. 上部のメニューで **[ユーザー]** をクリックします。
<br><br>![ユーザーを割り当てる][203] <br>
1. ユーザーの一覧で **[Britta Simon]** を選択します。

2. 下部にあるツールバー [ **割り当てる**します。
<br><br>![ユーザーを割り当てる][205]



### シングル サインオンのテスト

このセクションでは、構成のテスト Azure AD シングル サインオン アクセス パネルを使用します。<br>
アクセス パネルでフラット ファイルのタイルをクリックするとする必要があります取得自動的にサインオン フラット ファイル、アプリケーションにします。


## その他のリソース

* [Azure Active Directory による SaaS アプリを統合する方法に関するチュートリアルの一覧](active-directory-saas-tutorial-list.md)
* [アプリケーション アクセスと Azure Active Directory でのシングル サインオンとは](active-directory-appssoaccess-whatis.md)





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

