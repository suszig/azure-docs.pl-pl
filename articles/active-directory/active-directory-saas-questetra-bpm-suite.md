<properties
    pageTitle="チュートリアル: Azure Active Directory と Questetra BPM Suite の統合 | Microsoft Azure"
    description="Azure Active Directory と Questetra BPM Suite の間でシングル サインオンを構成する方法について説明します。"
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="msStevenPo"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/02/2015"
    ms.author="markusvi"/>


# チュートリアル: Azure Active Directory と Questetra BPM Suite の統合

このチュートリアルでは、Azure Active Directory (Azure AD) と Questetra BPM スイートを統合する方法を説明します。<br>Questetra BPM スイートを Azure AD と統合するには、次のメリットがあります。 

- Questetra BPM Suite にアクセスする Azure AD ユーザーを制御できます。 
- ユーザーが自分の Azure AD アカウントで自動的に Questetra BPM Suite にサインオン (シングル サインオン) できるようにします。
- 1 つの中央サイト (Azure Active Directory ポータル) でアカウントを管理できます。

Azure AD と SaaS アプリの統合に関する詳細を確認するを参照して [アプリケーションへのアクセスと Azure Active Directory でのシングル サインオン](active-directory-appssoaccess-whatis.md)します。

## 前提条件 

Questetra BPM Suite と Azure AD の統合を構成するには、次のものが必要です。

- Azure AD サブスクリプション
-  [Questetra BPM スイート](https://senbon-imadegawa-988.questetra.net/) 有効なサブスクリプションでのシングル サインオン


> [AZURE.NOTE] このチュートリアルでは、手順をテストするには、実稼働環境の使用をおしないでいます。


このチュートリアルの手順をテストするには、次の推奨事項に従ってください。

- 必要な場合を除き、運用環境は使用しないでください。
- Azure AD の評価環境を取得していない場合は、1 か月の試用版を入手できます [ここ](https://azure.microsoft.com/pricing/free-trial/)します。 

 
## シナリオの説明
このチュートリアルの目的は、テスト環境で Azure AD のシングル サインオンをテストできるようにすることです。 <br>
このチュートリアルで紹介するシナリオは、次の 3 つの主な構成要素で構成されます。

1. ギャラリーからの Questetra BPM Suite の追加 
2. Azure AD シングル サインオンの構成とテスト


## ギャラリーからの Questetra BPM Suite の追加
Azure AD への Questetra BPM Suite の統合を構成するには、ギャラリーから管理対象 SaaS アプリの一覧に Questetra BPM Suite を追加する必要があります。

**ギャラリーから Questetra BPM Suite を追加するには、次の手順に従います。**

1.  **Azure 管理ポータル**, 、左側のナビゲーション ウィンドウで、次のようにクリックします。 **Active Directory**します。 <br><br>
![Active Directory][1]

2.  **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3. ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。<br><br>
![アプリケーション][2]
4. クリックして **追加** ページの下部にあります。<br><br>
![アプリケーション][3]
5.  **実行する操作** ダイアログ ボックスで、をクリックして **ギャラリーからアプリケーションを追加**します。<br><br>
![アプリケーション][4]
6. 検索ボックスに入力 **Questetra BPM スイート**します。<br>
![アプリケーション][5]
7. 結果ウィンドウで [ **Questetra BPM スイート**, 、クリックして **完了** アプリケーションを追加します。<br>



##  Azure AD シングル サインオンの構成とテスト
このセクションの目的は、"Britta Simon" というテスト ユーザーに基づいて、Questetra BPM Suite で Azure AD のシングル サインオンを構成し、テストする方法について説明することです。

シングル サインオンを機能させるには、Azure AD ユーザーに対応する Questetra BPM Suite ユーザーが Azure AD で認識されている必要があります。 言い換えると、Azure AD ユーザーと Questetra BPM Suite の関連ユーザーの間で、リンクの関係が確立されている必要があります。<br>
値を割り当てることでこのリンクの関係が確立される、 **ユーザー名** の値として Azure AD で、 **ユーザー名** Questetra BPM スイートです。
 
Questetra BPM Suite で Azure AD のシングル サインオンを構成し、テストするには、次の要素を完了する必要があります。

1. **[Azure AD シングル サインオンを構成する](#configuring-azure-ad-single-single-sign-on)** - この機能を使用するユーザーをできるようにします。
2. **[Azure AD テスト ユーザーを作成する](#creating-an-azure-ad-test-user)** - を Azure AD シングル サインオン Britta Simon でテストします。
4. **[Questetra BPM スイートのテスト ユーザーを作成する](#creating-a-questetra-bpm-suite-test-user)** - 自分の Azure AD の表現にリンクされている Questetra BPM スイートに Britta Simon に対応します。
5. **[Azure AD テスト ユーザーの割り当て](#assigning-the-azure-ad-test-user)** - Azure AD シングル サインオンを使用する Britta Simon をできるようにします。
5. **[シングル サインオン テスト](#testing-single-sign-on)** - 構成が機能するかどうかを確認します。

### Azure AD シングル サインオンの構成

このセクションの目的は、Azure AD シングル サインオン Azure AD ポータルで有効にして、Questetra BPM Suite アプリケーションでシングル サインオンを構成するには。<br>

**Questetra BPM Suite で Azure AD シングル サインオンを構成するには、次の手順に従います。**

1. Azure AD ポータルでの **Questetra BPM スイート** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、 **シングル サインオンを構成する**  ダイアログ。<br><br>
![シングル サインオンを構成します。][8]

2.  **どのような Questetra BPM スイートにサインオン** ] ページで、[ **Azure AD シングル サインオン**, 、順にクリック **次**します。<br><br>
![Azure AD シングル サインオン][9]


3. ログインする別の web ブラウザーのウィンドウで、 **Questetra BPM スイート** 会社サイトに管理者として。

4. 上部にあるメニュー [ **システム設定**します。 <br><br> ![Azure AD シングル サインオン][10]

5. 開くには、 **SingleSignOnSAML** ] ページで [ **SSO (SAML)**します。 <br><br> ![Azure AD シングル サインオン][11]


6. Azure ポータルでの **アプリケーション設定の構成** ] ダイアログ ページで、次の手順を実行します <br><br>![アプリケーション設定の構成。][13]
 
    a.  **Questetra BPM スイート** 会社サイトでは、「SP 情報」のコピーで、 **ACS URL**, 、貼り付けると、 **サインオン URL** ] ボックスに貼り付けます。

    b.  **Questetra BPM スイート** 会社サイトでは、「SP 情報」のコピーで、 **エンティティ ID**, 、貼り付けると、 **発行者 URL** ] ボックスに貼り付けます。

    c.  **Questetra BPM スイート** 会社サイトでは、「SP 情報」のコピーで、 **ACS URL**, 、貼り付けると、 **応答 URL** ] ボックスに貼り付けます。

    d. クリックして **次**します。

 
7.  **Questetra BPM スイートでのシングル サインオンの構成** ] ページで [ **証明書のダウンロード**, 、お使いのコンピューターにローカルで証明書ファイルを保存します<br><br>![。シングル サインオンを構成します。][14]


8.  **Questetra BPM スイート** 会社のサイトは、次の手順に従います: <br><br>![シングル サインオンを構成します。][15]

    a. 選択 **でのシングル サインオンを有効にする**です。
     
    b. Azure のポータルでは、コピー、 **発行者 URL** 値に設定して、貼り付けます、 **エンティティ ID** ] ボックスに貼り付けます。

    c. Azure のポータルでは、コピー、 **シングル サインオン サービス URL** 値に設定して、貼り付けます、 **サインイン ページ URL** ] ボックスに貼り付けます。

    d. Azure のポータルでは、コピー、 **シングル サインアウト サービス URL** 値に設定して、貼り付けます、 **サインアウト ページ URL** ] ボックスに貼り付けます。

    e.  **NameID format** ] ボックスに「 **urn oasis: 名: 対象の形式: emailAddress**します。


    f. Create a base-64 encoded file from your downloaded certificate. 

    >[AZURE.TIP] For more details, see [How to convert a binary certificate into a text file](http://youtu.be/PlgrzUZ-Y1o).

    g. Open your base-64 encoded certificate in notepad, copy the content of it into your clipboard, and then paste it into the **Validation certificate** textbox. 

    h. Click **Save**.


9. Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **次**します。 <br><br>![Azure AD Connect とは][17]


10.  **シングル サインオンによる確認** ] ページで [ **完了**します。  <br><br>![Azure AD Connect とは][18]




### Azure AD のテスト ユーザーの作成
このセクションの目的は、Azure ポータルで Britta Simon というテスト ユーザーを作成することです。

**Azure AD でテスト ユーザーを作成するには、次の手順に従います。**

1.  **Azure 管理ポータル**, 、左側のナビゲーション ウィンドウで、次のようにクリックします。 **Active Directory**します。
<br><br>![Azure AD テスト ユーザーを作成します。][100] 

2.  **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3. 上部のメニューで、ユーザーの一覧を表示する] をクリックして **ユーザー**します。
<br><br>![Azure AD テスト ユーザーを作成します。][101] 

4. 開くには、 **ユーザーの追加** ] ダイアログ ボックスで、下部にあるツールバーをクリックして **ユーザーの追加**します。 
<br><br>![Azure AD テスト ユーザーを作成します。][102] 

5.  **このユーザーについてお聞かせ** ] ダイアログ ページで、次の手順を実行します。
<br><br>![Azure AD テスト ユーザーを作成します。][103]
 
    a. として **Type Of User**, [ **、組織内の新しいユーザー**します。
  
    b. [ユーザー名 **textbox**, 、型 **BrittaSimon**します。

    c. [次へ] をクリックします。
6.   **ユーザー プロファイル** ] ダイアログ ページで、次の手順を実行します。 
<br><br>![Azure AD テスト ユーザーを作成します。][104] 
  
    a.  **名** ] ボックスに「 **Britta**します。 
 
    b.  **姓** ] ボックスに、型、 **Simon**します。

    c.  **表示名** ] ボックスに「 **Britta Simon**します。

    d.  **ロール** 一覧で、[ **ユーザー**します。

    e. クリックして **次**します。

7.  **一時パスワードの取得** ダイアログ ページで、をクリックして **作成**します。
<br><br>![Azure AD テスト ユーザーを作成します。][105]  

8.  **一時パスワードの取得** ] ダイアログ ページで、次の手順を実行します。
<br><br>![Azure AD テスト ユーザーを作成します。][106]   
  1. 値を書き留めて、 **新しいパスワード**します。
  2. クリックして **完了**します。   
  
 
### Questetra BPM Suite のテスト ユーザーの作成

このセクションの目的は、Questetra BPM Suite で Britta Simon というユーザーを作成することです。

**Questetra BPM Suite で Britta Simon というユーザーを作成するには、次の手順に従います。**

1.  Questetra BPM Suite 企業サイトに管理者としてサインオンします。
2.  移動して **システムの設定 > ユーザーの一覧 > 新しいユーザー**します。 
3.  新しいユーザー] ダイアログ ボックスでは、次の手順を実行します。 <br><br>![テスト ユーザーの作成][300] 

    a.  **名前** ] ボックスに、Azure AD の Britta のユーザー名を入力します。

    b.  **電子メール** ] ボックスに、Azure AD の Britta のユーザー名を入力します。

    c.  **パスワード** ] ボックスに、パスワードを入力します。

4.  クリックして **新しいユーザーの追加**します。



### Azure AD テスト ユーザーの割り当て

このセクションでは、Britta Simon Questetra BPM スイートにアクセスを付与することによって Azure シングル サインオンの使用を有効にするとします。
<br><br>![What is Azure AD Connect][200]

**Questetra BPM Suite に Britta Simon を割り当てるには、次の手順に従います。**

1. Azure ポータルで、ディレクトリ ビューでアプリケーション ビューを開く] をクリックして **アプリケーション** 上部のメニュー<br><br>![。Azure AD Connect とは][201]
2. アプリケーションの一覧で選択 **Questetra BPM スイート**します。
<br><br>![Azure AD Connect とは][205]
1. 上部にあるメニュー [ **ユーザー**します。
<br><br>![Azure AD Connect とは][202]
1. ユーザーの一覧で選択 **Britta Simon**します。
<br><br>![Azure AD Connect とは][203]
2. 下部にあるツールバー [ **割り当てる**します。
<br><br>![Azure AD Connect とは][204]



### シングル サインオンのテスト

このセクションでは、構成のテスト Azure AD シングル サインオン アクセス パネルを使用します。<br>
アクセス パネルで Questetra BPM Suite のタイルをクリックすると、自動的に Questetra BPM Suite アプリケーションにサインオンします。


## その他のリソース

* [SaaS アプリと Azure Active Directory を統合する方法に関するチュートリアルの一覧](active-directory-saas-tutorial-list.md)
* [Azure Active Directory のアプリケーション アクセスとシングル サインオンとは](active-directory-appssoaccess-whatis.md)

<!--Image references-->
[1]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_01.png
[2]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_02.png
[3]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_03.png
[4]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_04.png
[5]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_01.png


[8]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_06.png
[9]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_02.png
[10]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_03.png
[11]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_04.png
[12]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_05.png
[13]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_06.png
[14]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_07.png
[15]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_08.png
[16]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_09.png
[17]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_10.png
[18]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_08.png


[100]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_09.png 
[101]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_10.png 
[102]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_11.png 
[103]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_12.png 
[104]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_13.png 
[105]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_14.png 
[106]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_15.png 


[200]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_16.png 
[201]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_17.png 
[202]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_18.png
[203]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_19.png
[204]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_20.png
[205]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_12.png

[300]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_11.png 
