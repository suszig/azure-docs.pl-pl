<properties
    pageTitle="チュートリアル: Azure Active Directory と Mixpanel の統合 | Microsoft Azure"
    description="Azure Active Directory と Mixpanel の間でシングル サインオンを構成する方法について説明します。"
    services="active-directory"
    documentationCenter=""
    authors="ashimaabrol"
    manager="stevenpo"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/14/2015"
    ms.author="v-aabrol"/>


# チュートリアル: Azure Active Directory と Mixpanel の統合

このチュートリアルでは、Azure Active Directory (Azure AD) と Mixpanel を統合する方法を説明します。<br>Mixpanel を Azure AD と統合するには、次のメリットがあります。

- Mixpanel にアクセスする Azure AD ユーザーを制御できます。
- ユーザーが自分の Azure AD アカウントで自動的に Mixpanel にサインオン (シングル サインオン) できるようにします。
- 1 つの中央サイト (Azure クラシック ポータル) でアカウントを管理できます。


Azure AD と SaaS アプリの統合に関する詳細を確認するを参照して [アプリケーションへのアクセスと Azure Active Directory でのシングル サインオン](active-directory-appssoaccess-whatis.md)します。

## 前提条件

Mixpanel と Azure AD の統合を構成するには、次のものが必要です。

- Azure AD サブスクリプション
- Mixpanel でのシングル サインオンが有効なサブスクリプション


> [AZURE.NOTE] このチュートリアルでは、手順をテストするには、実稼働環境の使用をおしないでいます。


このチュートリアルの手順をテストするには、次の推奨事項に従ってください。

- 必要な場合を除き、運用環境は使用しないでください。
- Azure AD の評価環境を取得していない場合は、1 か月の試用版を入手できます [ここ](https://azure.microsoft.com/pricing/free-trial/)します。


## シナリオの説明
このチュートリアルの目的は、テスト環境で Azure AD のシングル サインオンをテストできるようにすることです。 <br>
このチュートリアルで紹介するシナリオは、2 つの主な構成要素で構成されます。

1. ギャラリーから Mixpanel を追加する
2. Azure AD シングル サインオンの構成とテスト


## ギャラリーから Mixpanel を追加する
Azure AD への Mixpanel の統合を構成するには、ギャラリーから管理対象 SaaS アプリの一覧に Mixpanel を追加する必要があります。

**ギャラリーから Mixpanel を追加するには、次の手順を実行します。**

1.  **Azure クラシック ポータル**, 、左側のナビゲーション ウィンドウで、次のようにクリックします。 **Active Directory**します。 <br><br>
![Active Directory][1]<br>

2.  **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3. ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。<br><br>
![アプリケーション][2]<br>
4. クリックして **追加** ページの下部にあります。<br><br>
![アプリケーション][3]<br>
5.  **実行する操作** ダイアログ ボックスで、をクリックして **ギャラリーからアプリケーションを追加**します。<br><br>
![アプリケーション][4]<br>
6. 検索ボックスに入力 **Mixpanel**します。<br><br>
![Azure AD テスト ユーザーを作成します。](./media/active-directory-saas-mixpanel-tutorial/tutorial_mixpanel_01.png)<br>
7. 結果ウィンドウで [ **Mixpanel**, 、クリックして **完了** アプリケーションを追加します。
<br><br>

##  Azure AD シングル サインオンの構成とテスト
このセクションの目的は、"Britta Simon" というテスト ユーザーに基づいて、Mixpanel で Azure AD のシングル サインオンを構成し、テストする方法について説明することです。

シングル サインオンを機能させるには、Azure AD ユーザーに対応する Mixpanel ユーザーが Azure AD で認識されている必要があります。 言い換えると、Azure AD ユーザーと Mixpanel の関連ユーザーの間で、リンク関係が確立されている必要があります。<br>
値を割り当てることでこのリンクの関係が確立される、 **ユーザー名** の値として Azure AD で、 **Username** Mixpanel にします。

Mixpanel で Azure AD のシングル サインオンを構成してテストするには、次の構成要素を完了する必要があります。

1. **[Azure AD シングル サインオンを構成する](#configuring-azure-ad-single-single-sign-on)** - この機能を使用するユーザーをできるようにします。
2. **[Azure AD テスト ユーザーを作成する](#creating-an-azure-ad-test-user)** - を Azure AD シングル サインオン Britta Simon でテストします。
4. **[Mixpanel テスト ユーザーを作成する](#creating-a-mixpanel-test-user)** - 自分の Azure AD の表現にリンクされている Mixpanel に Britta Simon に対応します。
5. **[Azure AD テスト ユーザーの割り当て](#assigning-the-azure-ad-test-user)** - Azure AD シングル サインオンを使用する Britta Simon をできるようにします。
5. **[シングル サインオン テスト](#testing-single-sign-on)** - 構成が機能するかどうかを確認します。

### Azure AD シングル サインオンの構成

このセクションの目的は、Azure クラシック ポータルで Azure AD のシングル サインオンを有効にすることと、Mixpanel アプリケーションでシングル サインオンを構成することです。



**Mixpanel で Azure AD シングル サインオンを構成するには、次の手順に従います。**

1. Azure クラシック ポータルでの **Mixpanel** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、 **シングル サインオンを構成する**  ダイアログ。
<br><br> ![でのシングル サインオンを構成する][6] <br>

2.  **どのような Mixpanel にサインオン** ] ページで、[ **Azure AD シングル サインオン**, 、順にクリック **次**します。
<br><br> ![でのシングル サインオンを構成する](./media/active-directory-saas-mixpanel-tutorial/tutorial_mixpanel_03.png) <br>

3.  **アプリケーション設定の構成** ] ダイアログ ページで、次の手順を実行します。
<br><br>![シングル サインオンを構成する](./media/active-directory-saas-mixpanel-tutorial/tutorial_mixpanel_04.png) <br>


    a. In the **Sign On URL** textbox, type the URL used by your users to sign-on to your Mixpanel application using the following pattern: **“https://mixpanel.com/login/”**.

    > [AZURE.NOTE] Please register at [https://mixpanel.com/register/](https://mixpanel.com/register/) to set up your login credentials and  contact the [Mixpanel support team](mailto:support@Mixpanel.com) to enable SSO settings for you tenant. <br>You can also get your Sign On URL value if necessary from your Mixpanel support team.

    b. Click **Next**,



4.  **Mixpanel でのシングル サインオンの構成** ] ページで、次の手順を実行します。
<br><br>![シングル サインオンを構成する](./media/active-directory-saas-mixpanel-tutorial/tutorial_mixpanel_05.png) <br>

    a. クリックして **証明書のダウンロード**, 、お使いのコンピューター上のファイルを保存します。 

    b. クリックして **次**します。


5. 別のブラウザー ウィンドウで、管理者として Mixpanel アプリケーションにサインオンします。
   
6. ページ下部にある小さい] をクリックして **歯車の形** 左上隅のアイコン。 
   <br><br>![Mixpanel シングル サイン オン](./media/active-directory-saas-mixpanel-tutorial/tutorial_mixpanel_06.png) <br>

7. クリックして、 **アクセス セキュリティ** タブをクリックし、をクリックして **の設定を変更**します。
   <br><br>![Mixpanel 設定](./media/active-directory-saas-mixpanel-tutorial/tutorial_mixpanel_08.png) <br>
    
8.  **、証明書を変更** ダイアログ ページで、] をクリックして **ファイルの選択** 、ダウンロードした証明書をアップロードし **次**します。
  <br><br>![Mixpanel 設定](./media/active-directory-saas-mixpanel-tutorial/tutorial_mixpanel_09.png) <br>

9. Azure クラシック ポータルでの **Mixpanel でのシングル サインオンの構成** ] ダイアログ ページでコピー、 **シングル サインオン サービス URL** 値には、上の認証の [URL] テキスト ボックスに貼り付ける、 **認証 URL を変更** ダイアログ ページで、クリックして **次へ]**します。
   <br><br>![Mixpanel 設定](./media/active-directory-saas-mixpanel-tutorial/tutorial_mixpanel_10.png) <br>
    
1. クリックして **実行**します。


6. Azure クラシック ポータルで、シングル サインオンの構成情報を選択し、クリックして **次**します。
<br><br>![Azure AD シングル サインオン][10]<br>

7.  **シングル サインオンによる確認** ] ページで [ **完了**します。  
  <br><br>![Azure AD シングル サインオン][11]




### Azure AD のテスト ユーザーの作成
このセクションの目的は、Azure クラシック ポータルで Britta Simon というテスト ユーザーを作成することです。<br>
ユーザーの一覧で選択 **Britta Simon**.<br><br>![Azure AD ユーザーを作成します。][20]<br>

**Azure AD でテスト ユーザーを作成するには、次の手順に従います。**

1.  **Azure クラシック ポータル**, 、左側のナビゲーション ウィンドウで、次のようにクリックします。 **Active Directory**します。
<br><br>![Azure AD テスト ユーザーを作成する](./media/active-directory-saas-mixpanel-tutorial/create_aaduser_09.png) <br>

2.  **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3. 上部のメニューで、ユーザーの一覧を表示する] をクリックして **ユーザー**します。
<br><br> ![Azure AD テスト ユーザーを作成する](./media/active-directory-saas-mixpanel-tutorial/create_aaduser_03.png) <br>

4. 開くには、 **ユーザーの追加** ] ダイアログ ボックスで、下部にあるツールバーをクリックして **ユーザーの追加**します。
<br><br> ![Azure AD テスト ユーザーを作成する](./media/active-directory-saas-mixpanel-tutorial/create_aaduser_04.png) <br>

5.  **このユーザーについてお聞かせ** ] ダイアログ ページで、次の手順を実行します。
<br><br> ![Azure AD テスト ユーザーを作成する](./media/active-directory-saas-mixpanel-tutorial/create_aaduser_05.png) <br>

    a. [ユーザーの種類] として [組織内の新しいユーザー] を選択します。

    b. [ユーザー名 **textbox**, 、型 **BrittaSimon**します。

    c. クリックして **次**します。

6.   **ユーザー プロファイル** ] ダイアログ ページで、次の手順を実行します。
<br><br>![Azure AD テスト ユーザーを作成する](./media/active-directory-saas-mixpanel-tutorial/create_aaduser_06.png) <br>

    a.  **名** ] ボックスに「 **Britta**します。  

    b.  **姓** ] ボックスに、型、 **Simon**します。

    c.  **表示名** ] ボックスに「 **Britta Simon**します。

    d.  **ロール** 一覧で、[ **ユーザー**します。

    e. クリックして **次**します。

7.  **一時パスワードの取得** ダイアログ ページで、をクリックして **作成**します。
<br><br> ![Azure AD テスト ユーザーを作成する](./media/active-directory-saas-mixpanel-tutorial/create_aaduser_07.png) <br>

8.  **一時パスワードの取得** ] ダイアログ ページで、次の手順を実行します。
<br><br>![Azure AD テスト ユーザーを作成する](./media/active-directory-saas-mixpanel-tutorial/create_aaduser_08.png) <br>

    a. 値を書き留めて、 **新しいパスワード**します。

    b. クリックして **完了**します。   



### Mixpanel テスト ユーザーの作成

このセクションの目的は、Mixpanel で Britta Simon というユーザーを作成することです。 

1.  Mixpanel 企業サイトに管理者としてサインオンします。

2.  ページの下部にあるを開くには左上隅に小さな歯車] ボタンをクリックして、 **設定** ウィンドウです。

3.  クリックして、 **チーム** ] タブをクリックします。

4.  **チーム メンバー** ] ボックスに、Azure に Britta の電子メール アドレスを入力します。
   <br><br>![Mixpanel 設定](./media/active-directory-saas-mixpanel-tutorial/tutorial_mixpanel_11.png) <br>

4.  クリックして **招待**します。 

ユーザーにプロファイルを設定するために電子メールが届きます。


### Azure AD テスト ユーザーの割り当て

このセクションでは、Britta Simon Mixpanel に自分のアクセスを与えることで Azure シングル サインオンの使用を有効にするとします。
<br><br>![ユーザーの割り当て][200] <br>

**Mixpanel に Britta Simon を割り当てるには、次の手順を実行します。**

1. Azure クラシック ポータルでは、ビューを開くアプリケーション、ディレクトリ ビューで、クリックして **アプリケーション** 上部のメニュー。
<br><br>![ユーザーを割り当てる][201] <br>

2. アプリケーションの一覧で選択 **Mixpanel**します。
<br><br>![シングル サインオンを構成する](./media/active-directory-saas-mixpanel-tutorial/tutorial_mixpanel_50.png) <br>

1. 上部にあるメニュー [ **ユーザー**します。
<br><br>![ユーザーを割り当てる][203] <br>

1. ユーザーの一覧で選択 **Britta Simon**します。

2. 下部にあるツールバー [ **割り当てる**します。
<br><br>![ユーザーを割り当てる][205]



### シングル サインオンのテスト

このセクションでは、構成のテスト Azure AD シングル サインオン アクセス パネルを使用します。<br>
アクセス パネルで Mixpanel のタイルをクリックすると、自動的に Mixpanel アプリケーションにサインオンします。


## その他のリソース

* [SaaS アプリと Azure Active Directory を統合する方法に関するチュートリアルの一覧](active-directory-saas-tutorial-list.md)
* [Azure Active Directory のアプリケーション アクセスとシングル サインオンとは](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-mixpanel-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-mixpanel-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-mixpanel-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-mixpanel-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-mixpanel-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-mixpanel-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-mixpanel-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-mixpanel-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-mixpanel-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-mixpanel-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-mixpanel-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-mixpanel-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-mixpanel-tutorial/tutorial_general_205.png

