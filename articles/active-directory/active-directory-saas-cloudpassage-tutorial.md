<properties
    pageTitle="チュートリアル: Azure Active Directory と CloudPassage の統合 | Microsoft Azure"
    description="Azure Active Directory と CloudPassage の間でシングル サインオンを構成する方法について説明します。"
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
    ms.author="markvi"/>



# チュートリアル: Azure Active Directory と CloudPassage の統合

このチュートリアルでは、Azure Active Directory (Azure AD) と CloudPassage を統合する方法を説明します。<br>Azure AD と統合する CloudPassage を使うと、次の利点があります。

- CloudPassage にアクセスする Azure AD ユーザーを制御できます。
- ユーザーが自分の Azure AD アカウントで自動的に CloudPassage にサインオンできるようにします (シングル サインオン)。
- 1 つの中央サイト (Azure Active Directory ポータル) でアカウントを管理できます。

Azure AD と SaaS アプリの統合に関する詳細を確認するを参照して [アプリケーションへのアクセスと Azure Active Directory でのシングル サインオン](active-directory-appssoaccess-whatis.md)します。

## 前提条件

CloudPassage と Azure AD の統合を構成するには、次のものが必要です。

- Azure AD サブスクリプション
- CloudPassage でのシングル サインオンが有効なサブスクリプション


> [AZURE.NOTE] このチュートリアルの手順をテストする場合、運用環境を使用しないことをお勧めします。


このチュートリアルの手順をテストするには、次の推奨事項に従ってください。

- 必要な場合を除き、運用環境は使用しないでください。
- Azure AD のテスト環境を取得していない場合は、無料の 1 か月 Azure 試用版サブスクリプションを取得できます [ここ](https://azure.microsoft.com/pricing/free-trial/)します。


## シナリオの説明

このチュートリアルでは、使用すると、テスト環境で Azure AD シングル サインオンをテストします。 <br>
このチュートリアルで紹介するシナリオは、2 つの主な構成要素で構成されます。

1. ギャラリーからの CloudPassage の追加
2. Azure AD シングル サインオンの構成とテスト


## ギャラリーからの CloudPassage の追加

Azure AD への CloudPassage の統合を構成するには、ギャラリーから管理対象 SaaS アプリの一覧に CloudPassage を追加する必要があります。

### ギャラリーから CloudPassage を追加するには、次の手順に従います。

1. **Azure 管理ポータル**, 、左側のナビゲーション ウィンドウで、次のようにクリックします。 **Active Directory**します。 <br><br>
![Active Directory][1]

2. **[ディレクトリ]** の一覧から、ディレクトリ統合を有効にするディレクトリを選択します。

3. ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。<br><br>
![アプリケーション][2]
4. クリックして **追加** ページの下部にあります。<br><br>
![アプリケーション][3]
5. **実行する操作** ダイアログ ボックスで、をクリックして **ギャラリーからアプリケーションを追加**します。<br><br>
![アプリケーション][4]
6. 検索ボックスに入力 **CloudPassage**します。<br><br>
![アプリケーション][5]
7. 結果ウィンドウで [ **CloudPassage**, 、クリックして **完了** アプリケーションを追加します。<br><br>
![アプリケーション][6]



## Azure AD シングル サインオンの構成とテスト

このセクションの目的は、"Britta Simon" というテスト ユーザーを使用して、CloudPassage との Azure AD シングル サインオンを構成してテストする方法を示すことです。

シングル サインオンを機能させるには、Azure AD ユーザーに対応する CloudPassage ユーザーが Azure AD で認識される必要があります。つまり、Azure AD のユーザーと CloudPassage に関連するユーザーの間のリンクの関係を確立する必要があります。<br>
このリンク関係は、Azure AD の **[ユーザー名]** の値を、CloudPassage の **[Username]** の値として割り当てることで確立されます。

CloudPassage での Azure AD シングル サインオンを構成してテストするには、次の構成要素を完了する必要があります。

1. * *[Azure AD を構成する 1 つシングル サインオン](#configuring-azure-ad-single-single-sign-on)* * - この機能を使用するユーザーをできるようにします。
2. * *[Azure AD テスト ユーザーを作成する](#creating-an-azure-ad-test-user)* * - を Azure AD シングル サインオン Britta Simon でテストします。
4. * *[CloudPassage テスト ユーザーを作成する](#creating-a-halogen-software-test-user)* * - 自分の Azure AD の表現にリンクされている CloudPassage に Britta Simon に対応します。
5. * *[Azure AD テスト ユーザーの割り当て](#assigning-the-azure-ad-test-user)* * - Azure AD シングル サインオンを使用する Britta Simon をできるようにします。
5. * *[シングル サインオンをテスト](#testing-single-sign-on)* * - 構成が機能するかどうかを確認します。

### Azure AD シングル サインオンの構成

このセクションの目的は、Azure AD シングル サインオン Azure AD ポータルで有効にして、CloudPassage アプリケーションでシングル サインオンを構成するには。<br>
CloudPassage アプリケーションでは、特定の形式の SAML アサーションを使用するため、カスタム属性マッピングを SAML トークン属性の構成に追加する必要があります。 
次のスクリーンショットはその例です。
<br><br> ![でのシングル サインオンを構成する][21]

**CloudPassage との Azure AD シングル サインオンを構成するには、次の手順を実行します。**

1. Azure AD ポータルでの **CloudPassage** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、 **シングル サインオンを構成する**  ダイアログ。<br><br>
![Configure Single Sign-On][7]

2. **どのような CloudPassage にサインオン** ] ページで、[ **Azure AD シングル サインオン**, 、順にクリック **次**します。<br><br>
![Configure Single Sign-On][8]

3. **アプリケーション設定の構成** ] ダイアログ ページで、次の手順を実行します <br><br>![アプリケーション設定の構成][9]。

     3.1. **サインオン URL** CloudPassage アプリへのサインオンに、ユーザーが URL を使用] ボックスに、種類 (例:: *https://portal.cloudpassage.com/saml/init/accountid*)。

     3.2. 応答 URL] ボックスで、入力、AssertionConsumerService URL (例:: *https://portal.cloudpassage.com/saml/consume/accountid*)。<br> クリックしてこの属性の値を取得する **SSO セットアップ ドキュメント** で、 **シングル サインオン設定** CloudPassage ポータルのセクションです。<br><br>![シングル サインオンを構成します。][10]

     3.3. **[次へ]** をクリックします。

4. **CloudPassage でのシングル サインオンの構成** ] ページで [ **証明書のダウンロード**, 、お使いのコンピューターにローカルで証明書ファイルを保存します。<br><br>![シングル サインオンを構成します。][11]

5. 別の Web ブラウザーのウィンドウで、管理者として CloudPassage 企業サイトにサインオンします。

6. 上部にあるメニュー、[ **設定**, 、クリックして **サイトの管理**します。<br><br> ![でのシングル サインオンを構成する][12]

7. クリックして、 **認証設定** ] タブをクリックします。<br><br> ![でのシングル サインオンを構成する][13]

8. **シングル サインオン設定** セクションで、次の手順に従います: <br><br> ![シングル サインオンを構成する][14]

     8.1. Azure ポータルの **[CloudPassage でのシングル サインオンの構成]** ダイアログ ページで **[発行者の URL]** の値をコピーし、それを **[SAML issuer URL]** テキスト ボックスに貼り付けます。

     8.2. Azure ポータルの **[CloudPassage でのシングル サインオンの構成]** ダイアログ ページで **[サービス プロバイダー (SP) によって開始されたエンドポイント]** の値をコピーし、**[SAML endpoint URL]** テキスト ボックスに貼り付けます。

     8.3. Azure ポータルの **[CloudPassage でのシングル サインオンの構成]** ダイアログ ページで **[ログアウト URL]** の値をコピーし、**[Logout landing page]** テキスト ボックスに貼り付けます。

     8.4. ダウンロードした証明書から **Base-64** でエンコードされたファイルを作成します。
      >[AZURE.TIP] 詳細については、次を参照してください。 [バイナリ証明書をテキスト ファイルに変換する方法](http://youtu.be/PlgrzUZ-Y1o)します。

     8.5. base-64 でエンコードされた証明書をメモ帳で開き、その内容をクリップボードにコピーし、**[X.509 Certificate]** テキスト ボックスに貼り付けます。

     8.6. **[保存]** をクリックします。

9. Azure AD ポータルで、シングル サインオンの構成確認を選択し、**[次へ]** をクリックします。<br><br> ![でのシングル サインオンを構成する][15]

10. **[シングル サインオンの確認]** ページで **[完了]** をクリックします。<br><br> ![でのシングル サインオンを構成する][16]

11. 上部にある nu、クリックして **属性** を開くには、 **SAML トークン属性** ダイアログ。<br><br> ![でのシングル サインオンを構成する][17]

12. 次の表の行ごとに、必要なユーザー属性を追加するには、次の手順を実行します。 <br>

| 属性名| 属性値|
| --- | --- |
| firstname| User.givenname|
| lastname| User.surname|
| email| User.mail|

     12.1. Click **add user attribute**. <br><br> ![Configure Single Sign-On][18]
    
     12.2. In the **Attribute Name** textbox, type the attribute name shown for that row and as **Attribure Value**, select the attribute value shown for that row . <br><br> ![Configure Single Sign-On][19]
    
     12.2.3 Click **Complete**.

13. 下部にある tollbar、クリックして **変更を適用**します。<br><br> ![でのシングル サインオンを構成する][20]



### Azure AD のテスト ユーザーの作成

このセクションでは、Britta Simon と呼ばれる Azure ポータルでテスト ユーザーを作成します。<br><br>
ユーザーの一覧で選択 **Britta Simon**.<br>![Azure AD テスト ユーザーを作成する](./media/active-directory-saas-cloudpassage-tutorial/create_aaduser_01.png)

**Azure AD でテスト ユーザーを作成するには、次の手順に従います。**

1. **Azure 管理ポータル**, 、左側のナビゲーション ウィンドウで、次のようにクリックします。 **Active Directory**します。<br>
![Azure AD のテスト ユーザーの作成](./media/active-directory-saas-cloudpassage-tutorial/create_aaduser_02.png)

2. **[ディレクトリ]** の一覧から、ディレクトリ統合を有効にするディレクトリを選択します。

3. 上部のメニューで、ユーザーの一覧を表示する] をクリックして **ユーザー**.<br>![Azure AD テスト ユーザーを作成する](./media/active-directory-saas-cloudpassage-tutorial/create_aaduser_03.png)

4. 開くには、 **ユーザーの追加** ] ダイアログ ボックスで、下部にあるツールバーをクリックして **ユーザーの追加**します。<br>![Azure AD テスト ユーザーを作成します。](./media/active-directory-saas-cloudpassage-tutorial/create_aaduser_04.png)

5. **このユーザーについてお聞かせ** ] ダイアログ ページで、次の手順を実行します <br>![Azure AD テスト ユーザーを作成する](./media/active-directory-saas-cloudpassage-tutorial/create_aaduser_05.png)。
  1. [ユーザーの種類] として [組織内の新しいユーザー] を選択します。
  2. **[ユーザー名]** テキスト ボックスに「**BrittaSimon**」と入力します。
  3. [次へ] をクリックします。

6.  **ユーザー プロファイル** ] ダイアログ ページで、次の手順を実行します <br>![Azure AD テスト ユーザーを作成する](./media/active-directory-saas-cloudpassage-tutorial/create_aaduser_06.png)。
  1. **[名]** ボックスに「**Britta**」と入力します。
  2. **[姓]** ボックスに「**Simon**」と入力します。
  3. **[表示名]** ボックスに「**Britta Simon**」と入力します。
  4. **[ロール]** 一覧で **[ユーザー]** を選択します。
  5. **[次へ]** をクリックします。

7. **一時パスワードの取得** ダイアログ ページで、をクリックして **作成**.<br>![Azure AD テスト ユーザーを作成する](./media/active-directory-saas-cloudpassage-tutorial/create_aaduser_07.png)

8. **一時パスワードの取得** ] ダイアログ ページで、次の手順を実行します<br>![Azure AD テスト ユーザーを作成する](./media/active-directory-saas-cloudpassage-tutorial/create_aaduser_08.png)。
  1. **[新しいパスワード]** の値を書き留めます。
  2. **[完了]** をクリックします。




### CloudPassage テスト ユーザーの作成

このセクションの目的は、CloudPassage で Britta Simon というユーザーを作成することです。

#### CloudPassage で Britta Simon というユーザーを作成するには、次の手順に従います。

1.  **CloudPassage** 企業サイトに管理者としてサインオンします。

2.  上部にあるツールバーで、クリックして **設定**, 、クリックして **サイトの管理**します。<br>![CloudPassage テスト ユーザーの作成][22]

3.  クリックして、 **ユーザー** タブをクリックし、をクリックして **Add New User**します。<br>![CloudPassage テスト ユーザーの作成][23]

4.  **Add New User** セクションで、次の手順に従います: <br>![CloudPassage テスト ユーザーを作成する][24]

     4.1. **[First Name]** テキスト ボックスに「Britta」と入力します。

     4.2. **[Last Name]** テキスト ボックスに「Simon」と入力します。

     4.3. **[Username]** テキスト ボックス、**[Email]** テキスト ボックス、**[Retype Email]** テキスト ボックスに、Britta の Azure AD でのユーザー名を入力します。

     4.4. **[Access Type]** として、**[Enable Halo Portal Access]** を選択します。

     4.5. **[追加]** をクリックします。










### Azure AD テスト ユーザーの割り当て

このセクションでは、CloudPassage に自分のアクセスを付与することによって Azure シングル サインオンを使用する Britta Simon を有効にします。
<br><br>![ユーザーを割り当てる][30]

**Britta Simon を CloudPassage に割り当てるには、次の手順を実行します。**

1. Azure ポータルで、ディレクトリ ビューでアプリケーション ビューを開く] をクリックして **アプリケーション** 上部のメニュー。<br>
<br><br>![ユーザーを割り当てる][26]
2. アプリケーションの一覧で選択 **CloudPassage**します。
<br><br>![ユーザーを割り当てる][27]
1. 上部にあるメニュー [ **ユーザー**します。<br>
<br><br>![ユーザーを割り当てる][25]
1. ユーザーの一覧で **[Britta Simon]** を選択します。

2. 下部にあるツールバー [ **割り当てる**します。
<br><br>![ユーザーを割り当てる][29]



### シングル サインオンのテスト

このセクションでは、構成のテスト Azure AD シングル サインオン アクセス パネルを使用します。<br>
アクセス パネルで [CloudPassage] タイルをクリックすると、自動的に CloudPassage アプリケーションにサインオンします。


## その他のリソース

* [Azure Active Directory による SaaS アプリを統合する方法に関するチュートリアルの一覧](active-directory-saas-tutorial-list.md)
* [アプリケーション アクセスと Azure Active Directory でのシングル サインオンとは](active-directory-appssoaccess-whatis.md)



[1]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_01.png 
[2]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_02.png 
[3]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_03.png 
[4]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_04.png 
[5]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_01.png 
[6]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_02.png 
[7]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_05.png 
[8]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_03.png 
[9]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_04.png 
[10]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_05.png 
[11]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_06.png 
[12]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_07.png 
[13]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_08.png 
[14]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_09.png 
[15]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_10.png 
[16]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_11.png 
[17]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_10.png 
[18]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_11.png 
[19]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_12.png 
[20]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_14.png 
[21]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_14.png 
[22]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_15.png 
[23]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_16.png 
[24]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_17.png 
[25]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_15.png 
[26]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_12.png 
[27]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_13.png 
[28]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_15.png 
[29]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_16.png 
[30]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_17.png 

