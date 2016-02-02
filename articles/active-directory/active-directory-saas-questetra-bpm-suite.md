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

このチュートリアルでは、Azure Active Directory (Azure AD) と Questetra BPM スイートを統合する方法を説明します。<br>Azure AD と Questetra BPM スイートを統合することを使うと、次の利点があります。

- Questetra BPM Suite にアクセスする Azure AD ユーザーを制御できます。
- ユーザーが自分の Azure AD アカウントで自動的に Questetra BPM Suite にサインオン (シングル サインオン) できるようにします。
- 1 つの中央サイト (Azure Active Directory ポータル) でアカウントを管理できます。

Azure AD と SaaS アプリの統合に関する詳細を確認するを参照して [アプリケーションへのアクセスと Azure Active Directory でのシングル サインオン](active-directory-appssoaccess-whatis.md)します。

## 前提条件

Questetra BPM Suite と Azure AD の統合を構成するには、次のものが必要です。

- Azure AD サブスクリプション
- [Questetra BPM スイート](https://senbon-imadegawa-988.questetra.net/) 有効なサブスクリプションでのシングル サインオン


> [AZURE.NOTE] このチュートリアルの手順をテストする場合、運用環境を使用しないことをお勧めします。


このチュートリアルの手順をテストするには、次の推奨事項に従ってください。

- 必要な場合を除き、運用環境は使用しないでください。
- Azure AD の評価環境を取得していない場合は、1 か月の試用版を入手できます [ここ](https://azure.microsoft.com/pricing/free-trial/)します。


## シナリオの説明

このチュートリアルでは、使用すると、テスト環境で Azure AD シングル サインオンをテストします。 <br>
このチュートリアルで紹介するシナリオは、次の 3 つの主な構成要素で構成されます。

1. ギャラリーからの Questetra BPM Suite の追加
2. Azure AD シングル サインオンの構成とテスト


## ギャラリーからの Questetra BPM Suite の追加

Azure AD への Questetra BPM Suite の統合を構成するには、ギャラリーから管理対象 SaaS アプリの一覧に Questetra BPM Suite を追加する必要があります。

**ギャラリーから Questetra BPM Suite を追加するには、次の手順に従います。**

1. **Azure 管理ポータル**, 、左側のナビゲーション ウィンドウで、次のようにクリックします。 **Active Directory**します。 <br><br>
![Active Directory][1]

2. **[ディレクトリ]** の一覧から、ディレクトリ統合を有効にするディレクトリを選択します。

3. ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。<br><br>
![アプリケーション][2]
4. クリックして **追加** ページの下部にあります。<br><br>
![アプリケーション][3]
5. **実行する操作** ダイアログ ボックスで、をクリックして **ギャラリーからアプリケーションを追加**します。<br><br>
![アプリケーション][4]
6. 検索ボックスに入力 **Questetra BPM スイート**します。<br>
![アプリケーション][5]
7. 結果ウィンドウで [ **Questetra BPM スイート**, 、クリックして **完了** アプリケーションを追加します。<br>



## Azure AD シングル サインオンの構成とテスト

このセクションの目的は、"Britta Simon" というテスト ユーザーに基づいて、Questetra BPM Suite で Azure AD のシングル サインオンを構成し、テストする方法について説明することです。

シングル サインオンを機能させるには、Azure AD ユーザーに対応する Questetra BPM Suite ユーザーが Azure AD で認識されている必要があります。つまり、Azure AD のユーザーと Questetra BPM スイート内の関連ユーザーの間のリンクの関係を確立する必要があります。<br>
このリンクの関係を確立するには、Azure AD の **[ユーザー名]** の値を、Questetra BPM Suite の **[Username]** の値として割り当てます。

Questetra BPM Suite で Azure AD のシングル サインオンを構成し、テストするには、次の要素を完了する必要があります。

1. * *[構成する Azure AD シングル サインオン](#configuring-azure-ad-single-single-sign-on)* * - この機能を使用するユーザーをできるようにします。
2. * *[Azure AD テスト ユーザーを作成する](#creating-an-azure-ad-test-user)* * - を Azure AD シングル サインオン Britta Simon でテストします。
4. * *[Questetra BPM スイートのテスト ユーザーを作成する](#creating-a-questetra-bpm-suite-test-user)* * - 自分の Azure AD の表現にリンクされている Questetra BPM スイートに Britta Simon に対応します。
5. * *[Azure AD テスト ユーザーの割り当て](#assigning-the-azure-ad-test-user)* * - Azure AD シングル サインオンを使用する Britta Simon をできるようにします。
5. * *[シングル サインオンをテスト](#testing-single-sign-on)* * - 構成が機能するかどうかを確認します。

### Azure AD シングル サインオンの構成

このセクションの目的は、Azure AD シングル サインオン Azure AD ポータルで有効にして、Questetra BPM Suite アプリケーションでシングル サインオンを構成するには。<br>

**Questetra BPM Suite で Azure AD シングル サインオンを構成するには、次の手順に従います。**

1. Azure AD ポータルでの **Questetra BPM スイート** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、 **シングル サインオンを構成する**  ダイアログ。<br><br>
![Configure Single Sign-On][8]

2. **どのような Questetra BPM スイートにサインオン** ] ページで、[ **Azure AD シングル サインオン**, 、順にクリック **次**します。<br><br>
![Azure AD のシングル サインオン][9]

3. 別の Web ブラウザーのウィンドウで、管理者として **Questetra BPM Suite** 企業サイトにログインします。

4. 上部にあるメニュー [ **システム設定**します。<br><br> ![Azure AD シングル サインオン][10]

5. 開くには、 **SingleSignOnSAML** ] ページで [ **SSO (SAML)**します。<br><br> ![Azure AD シングル サインオン][11]

6. Azure ポータルでの **アプリケーション設定の構成** ] ダイアログ ページで、次の手順を実行します <br><br>![アプリケーション設定の構成][13]。

    a. **Questetra BPM Suite** 企業サイトの [SP 情報] セクションで **[ACS URL]** をコピーし、**[サインオン URL]** テキスト ボックスに貼り付けます。

    b. **Questetra BPM Suite** 企業サイトの [SP 情報] セクションで **[エンティティ ID]** をコピーし、**[発行者の URL]** ボックスに貼り付けます。

    c. **Questetra BPM Suite** 企業サイトの [SP 情報] セクションで **[ACS URL]** をコピーし、**[応答 URL]** ボックスに貼り付けます。

    d. **[次へ]** をクリックします。

7. **Questetra BPM スイートでのシングル サインオンの構成** ] ページで [ **証明書のダウンロード**, 、お使いのコンピューターにローカルで証明書ファイルを保存します<br>。<br>![でのシングル サインオンを構成する][14]

8. **Questetra BPM スイート** 会社のサイトは、次の手順に従います: <br><br>![シングル サインオンを構成する][15]

    a. **[シングル サインオンを有効にする]** を選択します。

    b. Azure ポータルで、**[発行者の URL]** の値をコピーし、**[エンティティ ID]** ボックスに貼り付けます。

    c. Azure ポータルで、**[シングル サインオン サービス URL]** の値をコピーし、**[サインイン ページの URL]** ボックスに貼り付けます。

    d. Azure ポータルで、**[シングル サインアウト サービス URL]** の値をコピーし、**[サインアウト ページの URL]** ボックスに貼り付けます。

    e. **[NameID 形式]** ボックスに「**urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress**」と入力します。

    f. ダウンロードした証明書から base-64 でエンコードされたファイルを作成します。
    >[AZURE.TIP] 詳細については、次を参照してください。 [バイナリ証明書をテキスト ファイルに変換する方法](http://youtu.be/PlgrzUZ-Y1o)します。

    g. base-64 でエンコードされた証明書をメモ帳で開き、その内容をクリップボードにコピーし、**[検証証明書]** ボックスに貼り付けます。

    h. **[保存]** をクリックします。

9. Azure AD ポータルで、シングル サインオンの構成確認を選択し、**[次へ]** をクリックします。<br><br>![Azure AD Connect とは][17]

10. **[シングル サインオンの確認]** ページで **[完了]** をクリックします。<br><br>![Azure AD Connect とは][18]




### Azure AD のテスト ユーザーの作成

このセクションの目的は、Azure ポータルで Britta Simon というテスト ユーザーを作成することです。

**Azure AD でテスト ユーザーを作成するには、次の手順に従います。**

1. **Microsoft Azure 管理ポータル**の左側のナビゲーション ウィンドウで、**[Active Directory]** をクリックします。
<br><br>![Azure AD テスト ユーザーを作成します。][100]

2. **[ディレクトリ]** の一覧から、ディレクトリ統合を有効にするディレクトリを選択します。

3. 上部のメニューで、ユーザーの一覧を表示する] をクリックして **ユーザー**します。
<br><br>![Azure AD テスト ユーザーを作成します。][101]

4. 開くには、 **ユーザーの追加** ] ダイアログ ボックスで、下部にあるツールバーをクリックして **ユーザーの追加**します。 
<br><br>![Azure AD テスト ユーザーを作成します。][102]

5. **このユーザーについてお聞かせ** ] ダイアログ ページで、次の手順を実行します。
<br><br>![Azure AD テスト ユーザーを作成します。][103]

    a. **[ユーザーの種類]** として **[組織内の新しいユーザー]** を選択します。

    b. **[ユーザー名]** ボックスに「**BrittaSimon**」と入力します。

    c. [次へ] をクリックします。
6.  **ユーザー プロファイル** ] ダイアログ ページで、次の手順を実行します。 
<br><br>![Azure AD テスト ユーザーを作成します。][104]

    a. **[名]** ボックスに「**Britta**」と入力します。

    b. **[姓]** ボックスに「**Simon**」と入力します。

    c. **[表示名]** ボックスに「**Britta Simon**」と入力します。

    d. **[ロール]** 一覧で **[ユーザー]** を選択します。

    e. **[次へ]** をクリックします。

7. **一時パスワードの取得** ダイアログ ページで、をクリックして **作成**します。
<br><br>![Azure AD テスト ユーザーを作成します。][105]

8. **一時パスワードの取得** ] ダイアログ ページで、次の手順を実行します。
<br><br>![Azure AD テスト ユーザーを作成します。][106]
  1. **[新しいパスワード]** の値を書き留めます。
  2. **[完了]** をクリックします。


### Questetra BPM Suite のテスト ユーザーの作成

このセクションの目的は、Questetra BPM Suite で Britta Simon というユーザーを作成することです。

**Questetra BPM Suite で Britta Simon というユーザーを作成するには、次の手順に従います。**

1.  Questetra BPM Suite 企業サイトに管理者としてサインオンします。
2.  **[システム設定]、[ユーザー一覧]、[新規ユーザー]** の順に移動します。
3.  新しいユーザー] ダイアログ ボックスで、次の手順を実行します: <br><br>![テスト ユーザーの作成][300]

    a. **[名前]** ボックスに、Britta の Azure AD でのユーザー名を入力します。

    b. **[メール]** ボックスに、Britta の Azure AD でのユーザー名を入力します。

    c. **[パスワード]** ボックスに、パスワードを入力します。

4.  **[新しいユーザーの追加]** をクリックします。



### Azure AD テスト ユーザーの割り当て

このセクションでは、Britta Simon Questetra BPM スイートにアクセスを付与することによって Azure シングル サインオンの使用を有効にするとします。
<br><br>![Azure AD Connect とは][200]

**Questetra BPM Suite に Britta Simon を割り当てるには、次の手順に従います。**

1. Azure ポータルで、ディレクトリ ビューでアプリケーション ビューを開く] をクリックして **アプリケーション** 上部のメニュー<br>。<br>![は Azure AD Connect][201]
2. アプリケーションの一覧で選択 **Questetra BPM スイート**します。
<br><br>![Azure AD Connect とは][205]
1. 上部のメニューで **[ユーザー]** をクリックします。
<br><br>![Azure AD Connect とは][202]
1. ユーザーの一覧で **[Britta Simon]** を選択します。
<br><br>![Azure AD Connect とは][203]
2. 下部にあるツールバー [ **割り当てる**します。
<br><br>![Azure AD Connect とは][204]



### シングル サインオンのテスト

このセクションでは、構成のテスト Azure AD シングル サインオン アクセス パネルを使用します。<br>
アクセス パネルで Questetra BPM Suite のタイルをクリックすると、自動的に Questetra BPM Suite アプリケーションにサインオンします。


## その他のリソース

* [Azure Active Directory による SaaS アプリを統合する方法に関するチュートリアルの一覧](active-directory-saas-tutorial-list.md)
* [アプリケーション アクセスと Azure Active Directory でのシングル サインオンとは](active-directory-appssoaccess-whatis.md)



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

