<properties
    pageTitle="チュートリアル: Azure Active Directory と Alcumus Info Exchange の統合 | Microsoft Azure"
    description="Azure Active Directory と Alcumus Info Exchange の間でシングル サインオンを構成する方法について説明します。"
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
    ms.date="10/06/2015"
    ms.author="markusvi"/>


# チュートリアル: Azure Active Directory と Alcumus Info Exchange の統合

このチュートリアルでは、Azure Active Directory (Azure AD) と Alcumus 情報 Exchange を統合する方法を説明します。<br>Alcumus 情報交換を Azure AD と統合するには、次のメリットがあります。 

- Alcumus Info Exchange にアクセスする Azure AD ユーザーを制御できます。 
- ユーザーが自分の Azure AD アカウントで自動的に Alcumus Info Exchange にサインオン (シングル サインオン) できるようにします。
- 1 つの中央サイト (Azure Active Directory ポータル) でアカウントを管理できます。

Azure AD と SaaS アプリの統合に関する詳細を確認するを参照して [アプリケーションへのアクセスと Azure Active Directory でのシングル サインオン](active-directory-appssoaccess-whatis.md)します。

## 前提条件 

Alcumus Info Exchange と Azure AD の統合を構成するには、次のものが必要です。

-  [Azure AD](http://azure.microsoft.com/) サブスクリプション
-  [Alcumus 情報 Exchange](http://www.alcumusgroup.com/) 有効なサブスクリプションでのシングル サインオン


> [AZURE.NOTE] このチュートリアルでは、手順をテストするには、実稼働環境の使用をおしないでいます。


このチュートリアルの手順をテストするには、次の推奨事項に従ってください。

- 必要な場合を除き、運用環境は使用しないでください。
- Azure AD の評価環境を取得していない場合は、1 か月の試用版を入手できます [ここ](https://azure.microsoft.com/pricing/free-trial/)します。 

 
## シナリオの説明
このチュートリアルの目的は、テスト環境で Azure AD のシングル サインオンをテストできるようにすることです。 <br>
このチュートリアルで紹介するシナリオは、次の 3 つの主な構成要素で構成されます。

1. ギャラリーからの Alcumus Info Exchange の追加 
2. Azure AD シングル サインオンの構成とテスト


## ギャラリーからの Alcumus Info Exchange の追加
Azure AD への Alcumus Info Exchange の統合を構成するには、ギャラリーから管理対象 SaaS アプリの一覧に Alcumus Info Exchange を追加する必要があります。

**ギャラリーから Alcumus Info Exchange を追加するには、次の手順に従います。**

1.  **Azure 管理ポータル**, 、左側のナビゲーション ウィンドウで、次のようにクリックします。 **Active Directory**します。 
<br><br>![Active Directory][1]<br>

2.  **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3. ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。
<br><br>![アプリケーション][2]<br>

4. クリックして **追加** ページの下部にあります。
<br><br>![アプリケーション][3]<br>

5.  **実行する操作** ダイアログ ボックスで、をクリックして **ギャラリーからアプリケーションを追加**します。
<br><br>![アプリケーション][4]<br>

6. 検索ボックスに入力 **Alcumus 情報 Exchange**します。
<br><br>![アプリケーション][5]<br>

7. 結果ウィンドウで [ **Alcumus 情報 Exchange**, 、] をクリックし、 **完了** アプリケーションを追加します。
<br><br>![アプリケーション][400]<br>



##  Azure AD シングル サインオンの構成とテスト
このセクションの目的は、"Britta Simon" というテスト ユーザーに基づいて、Alcumus Info Exchange で Azure AD のシングル サインオンを構成し、テストする方法について説明することです。

シングル サインオンを機能させるには、Azure AD ユーザーに対応する Alcumus Info Exchange ユーザーが Azure AD で認識される必要があります。 言い換えると、Azure AD ユーザーと Alcumus Info Exchange の関連ユーザーの間で、リンク関係が確立されている必要があります。<br>
値を割り当てることでこのリンクの関係が確立される、 **ユーザー名** の値として Azure AD で、 **Username** Alcumus 情報交換にします。
 
Alcumus Info Exchange で Azure AD のシングル サインオンを構成してテストするには、次の構成要素を完了する必要があります。

1. **[Azure AD シングル サインオンを構成する](#configuring-azure-ad-single-single-sign-on)** - この機能を使用するユーザーをできるようにします。
2. **[Azure AD テスト ユーザーを作成する](#creating-an-azure-ad-test-user)** - を Azure AD シングル サインオン Britta Simon でテストします。
4. **[Alcumus 情報 Exchange テスト ユーザーを作成する](#creating-a-alcumus-info-exchange-test-user)** - Britta Simon に対応する、自分の Azure AD の表現にリンクされている Alcumus 情報交換できます。
5. **[Azure AD テスト ユーザーの割り当て](#assigning-the-azure-ad-test-user)** - Azure AD シングル サインオンを使用する Britta Simon をできるようにします。
5. **[シングル サインオン テスト](#testing-single-sign-on)** - 構成が機能するかどうかを確認します。

### Azure AD シングル サインオンの構成

このセクションの目的は、Azure AD シングル サインオン Azure AD ポータルで有効にして、Alcumus 情報 Exchange アプリケーションでシングル サインオンを構成するには。<br>

**Alcumus Info Exchange で Azure AD のシングル サインオンを構成するには、次の手順に従います。**

1. Azure AD ポータルでの **Alcumus 情報 Exchange** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、 **シングル サインオンを構成する**  ダイアログ。<br><br>
![シングル サインオンを構成する][6]

2.  **どのようなユーザー Alcumus 情報 Exchange へのアクセス** ] ページで、[ **Azure AD シングル サインオン**, 、順にクリック **次**します。<br><br>
![Azure AD シングル サインオン][7]

3.  **アプリケーション設定の構成** ] ダイアログ ページで、次の手順を実行します。 
<br><br>![Azure AD シングル サインオン][8]<br>
 
     3.1 で、 **応答 URL** ] ボックスに、チームをサポートする、Alcumus 情報交換では、セットアップがコンシューマーの URL を入力します。

     > [AZURE.NOTE] 適切な値は、何がわからない場合、Alcumus 情報 Exchange サポート チームに連絡を使用して [helpdesk@alcumusgroup.com](mailto:helpdesk@alcumusgroup.com)します。

     3.2. クリックして **次**します。
 
4.  **Alcumus 情報 Exchange でのシングル サインオンの構成** ] ページで [ **メタデータのダウンロード**, 、コンピューターのローカルにメタデータ ファイルを保存します<br><br>!。[Azure AD Connect は][9]

5. 使用して Alcumus 情報 Exchange サポート チームに連絡 [helpdesk@alcumusgroup.com](mailto:helpdesk@alcumusgroup.com), に連絡し、SSO がするを有効にする必要があること、メタデータ ファイルを使用してそれらを使用します。


6. Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **次**します。 <br><br>![Azure AD Connect は][10]

7.  **シングル サインオンによる確認** ] ページで [ **完了**します。  <br><br>![Azure AD Connect は][11]




### Azure AD のテスト ユーザーの作成
このセクションの目的は、Azure ポータルで Britta Simon というテスト ユーザーを作成することです。<br>
ユーザーの一覧で選択 **Britta Simon**.<br><br>![Azure AD ユーザーの作成][20]<br>

**Azure AD でテスト ユーザーを作成するには、次の手順に従います。**

1.  **Azure 管理ポータル**, 、左側のナビゲーション ウィンドウで、次のようにクリックします。 **Active Directory**します。<br>
![Azure AD テスト ユーザーを作成します。](./media/active-directory-saas-alcumus-info-tutorial/create_aaduser_02.png) 

2.  **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3. 上部のメニューで、ユーザーの一覧を表示する] をクリックして **ユーザー**.<br>![Azure AD テスト ユーザーを作成します。](./media/active-directory-saas-alcumus-info-tutorial/create_aaduser_03.png) 
 
4. 開くには、 **ユーザーの追加** ] ダイアログ ボックスで、下部にあるツールバーをクリックして **ユーザーの追加**します。 <br>![Azure AD テスト ユーザーを作成します。](./media/active-directory-saas-alcumus-info-tutorial/create_aaduser_04.png) 

5.  **このユーザーについてお聞かせ** ] ダイアログ ページで、次の手順を実行します <br>![Azure AD テスト ユーザーを作成する。](./media/active-directory-saas-alcumus-info-tutorial/create_aaduser_05.png) 
  1. [ユーザーの種類] として [組織内の新しいユーザー] を選択します。
  2. [ユーザー名 **textbox**, 、型 **BrittaSimon**します。
  3. [次へ] をクリックします。

6.   **ユーザー プロファイル** ] ダイアログ ページで、次の手順を実行します <br>![Azure AD テスト ユーザーを作成する。](./media/active-directory-saas-alcumus-info-tutorial/create_aaduser_06.png) 
  1.  **名** ] ボックスに「 **Britta**します。  
  2.  **姓** ボックス型 **Simon**します。
  3.  **表示名** ] ボックスに「 **Britta Simon**します。
  4.  **ロール** 一覧で、[ **ユーザー**します。
  5. クリックして **次**します。

7.  **一時パスワードの取得** ダイアログ ページで、をクリックして **作成**.<br>![Azure AD テスト ユーザーを作成します。](./media/active-directory-saas-alcumus-info-tutorial/create_aaduser_07.png) 
 
8.  **一時パスワードの取得** ] ダイアログ ページで、次の手順を実行します<br>![Azure AD テスト ユーザーを作成する。](./media/active-directory-saas-alcumus-info-tutorial/create_aaduser_08.png) 
  1. 値を書き留めて、 **新しいパスワード**します。
  2. クリックして **完了**します。   

  
 
### Alcumus Info Exchange テスト ユーザーの作成

このセクションの目的は、Alcumus Info Exchange で Britta Simon というユーザーを作成することです。

**Alcumus Info Exchange で Britta Simon というユーザーを作成するには、次の手順に従います。**

1. 使用して Alcumus 情報 Exchange サポート チームに連絡 [helpdesk@alcumusgroup.com](mailto:helpdesk@alcumusgroup.com),、


### Azure AD テスト ユーザーの割り当て

このセクションでは、Britta Simon Alcumus 情報 Exchange へのアクセスを付与することで Azure シングル サインオンの使用を有効にするとします。
<br><br>![ユーザーの割り当て][200]

**Alcumus Info Exchange に Britta Simon を割り当てるには、次の手順に従います。**

1. Azure ポータルで、ディレクトリ ビューでアプリケーション ビューを開く] をクリックして **アプリケーション** 上部のメニュー。<br>
<br><br>![ユーザーを割り当てる][201]
2. アプリケーションの一覧で選択 **Alcumus 情報 Exchange**します。
<br><br>![ユーザーを割り当てる][202]
1. 上部にあるメニュー [ **ユーザー**します。<br>
<br><br>![ユーザーを割り当てる][203]
1. ユーザーの一覧で選択 **Britta Simon**します。

2. 下部にあるツールバー [ **割り当てる**します。
<br><br>![ユーザーを割り当てる][205]



### シングル サインオンのテスト

このセクションでは、構成のテスト Azure AD シングル サインオン アクセス パネルを使用します。<br>
アクセス パネルで [Alcumus Info Exchange] タイルをクリックすると、自動的に Alcumus Info Exchange アプリケーションにサインオンします。


## その他のリソース

* [SaaS アプリと Azure Active Directory を統合する方法に関するチュートリアルの一覧](active-directory-saas-tutorial-list.md)
* [Azure Active Directory のアプリケーション アクセスとシングル サインオンとは](active-directory-appssoaccess-whatis.md)

<!--Image references-->
[1]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumus_01.png
[6]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumus_02.png
[7]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumus_03.png
[8]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumus_04.png
[9]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumus_05.png
[10]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumus_06.png
[11]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumus_07.png
[20]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumus_08.png
[203]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_205.png
[400]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumus_402.png
