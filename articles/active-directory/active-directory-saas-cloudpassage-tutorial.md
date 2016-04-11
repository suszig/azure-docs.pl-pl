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

このチュートリアルでは、Azure Active Directory (Azure AD) と CloudPassage を統合する方法を説明します。<br>CloudPassage を Azure AD と統合するには、次のメリットがあります。 

- CloudPassage にアクセスする Azure AD ユーザーを制御できます。 
- ユーザーが自分の Azure AD アカウントで自動的に CloudPassage にサインオンできるようにします (シングル サインオン)。
- 1 つの中央サイト (Azure Active Directory ポータル) でアカウントを管理できます。

Azure AD と SaaS アプリの統合に関する詳細を確認するを参照して [アプリケーションへのアクセスと Azure Active Directory でのシングル サインオン](active-directory-appssoaccess-whatis.md)します。

## 前提条件 

CloudPassage と Azure AD の統合を構成するには、次のものが必要です。

- Azure AD サブスクリプション
- CloudPassage でのシングル サインオンが有効なサブスクリプション


> [AZURE.NOTE] このチュートリアルでは、手順をテストするには、実稼働環境の使用をおしないでいます。


このチュートリアルの手順をテストするには、次の推奨事項に従ってください。

- 必要な場合を除き、運用環境は使用しないでください。
- Azure AD のテスト環境を取得していない場合は、無料の 1 か月 Azure 試用版サブスクリプションを取得できます [ここ](https://azure.microsoft.com/pricing/free-trial/)します。 

 
## シナリオの説明
このチュートリアルの目的は、テスト環境で Azure AD のシングル サインオンをテストできるようにすることです。 <br>
このチュートリアルで紹介するシナリオは、2 つの主な構成要素で構成されます。

1. ギャラリーからの CloudPassage の追加 
2. Azure AD シングル サインオンの構成とテスト


## ギャラリーからの CloudPassage の追加
Azure AD への CloudPassage の統合を構成するには、ギャラリーから管理対象 SaaS アプリの一覧に CloudPassage を追加する必要があります。

### ギャラリーから CloudPassage を追加するには、次の手順に従います。

1.  **Azure 管理ポータル**, 、左側のナビゲーション ウィンドウで、次のようにクリックします。 **Active Directory**します。 <br><br>
![Active Directory][1]

2.  **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3. ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。<br><br>
![アプリケーション][2]
4. クリックして **追加** ページの下部にあります。<br><br>
![アプリケーション][3]
5.  **実行する操作** ダイアログ ボックスで、をクリックして **ギャラリーからアプリケーションを追加**します。<br><br>
![アプリケーション][4]
6. 検索ボックスに入力 **CloudPassage**します。<br><br>
![アプリケーション][5]
7. 結果ウィンドウで [ **CloudPassage**, 、クリックして **完了** アプリケーションを追加します。<br><br>
![アプリケーション][6]



##  Azure AD シングル サインオンの構成とテスト
このセクションの目的は、"Britta Simon" というテスト ユーザーを使用して、CloudPassage との Azure AD シングル サインオンを構成してテストする方法を示すことです。

シングル サインオンを機能させるには、Azure AD ユーザーに対応する CloudPassage ユーザーが Azure AD で認識される必要があります。 つまり、Azure AD のユーザーと CloudPassage に関連するユーザーの間のリンクの関係を確立する必要があります。<br>
値を割り当てることでこのリンクの関係が確立される、 **ユーザー名** の値として Azure AD で、 **Username** CloudPassage にします。
 
CloudPassage での Azure AD シングル サインオンを構成してテストするには、次の構成要素を完了する必要があります。

1. **[Azure AD シングル シングル サインオンを構成する](#configuring-azure-ad-single-single-sign-on)** - この機能を使用するユーザーをできるようにします。
2. **[Azure AD テスト ユーザーを作成する](#creating-an-azure-ad-test-user)** - を Azure AD シングル サインオン Britta Simon でテストします。
4. **[CloudPassage テスト ユーザーを作成する](#creating-a-halogen-software-test-user)** - 自分の Azure AD の表現にリンクされている CloudPassage に Britta Simon に対応します。
5. **[Azure AD テスト ユーザーの割り当て](#assigning-the-azure-ad-test-user)** - Azure AD シングル サインオンを使用する Britta Simon をできるようにします。
5. **[シングル サインオン テスト](#testing-single-sign-on)** - 構成が機能するかどうかを確認します。

### Azure AD シングル サインオンの構成

このセクションの目的は、Azure AD シングル サインオン Azure AD ポータルで有効にして、CloudPassage アプリケーションでシングル サインオンを構成するには。<br>
CloudPassage アプリケーションでは、特定の形式の SAML アサーションを使用するため、カスタム属性マッピングを SAML トークン属性の構成に追加する必要があります。 
次のスクリーンショットはその例です。
<br><br> ![Configure Single Sign-On][21]

**CloudPassage との Azure AD シングル サインオンを構成するには、次の手順を実行します。**

1. Azure AD ポータルでの **CloudPassage** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、 **シングル サインオンを構成する**  ダイアログ。<br><br>
![シングル サインオンを構成します。][7]

2.  **どのような CloudPassage にサインオン** ] ページで、[ **Azure AD シングル サインオン**, 、順にクリック **次**します。<br><br>
![シングル サインオンを構成します。][8]

3.  **アプリケーション設定の構成** ] ダイアログ ページで、次の手順を実行します。 <br><br>![アプリ設定を構成します。][9]
 
     3.1.  **サインオン URL** CloudPassage アプリへのサインオンに、ユーザーが URL を使用] ボックスに、種類 (例:: *https://portal.cloudpassage.com/saml/init/accountid*)。 

     3.2. 応答 URL] ボックスで、入力、AssertionConsumerService URL (例:: *https://portal.cloudpassage.com/saml/consume/accountid*)。 <br> クリックしてこの属性の値を取得する **SSO セットアップ ドキュメント** で、 **シングル サインオン設定** CloudPassage ポータルのセクションです。 <br><br>![シングル サインオンを構成します。][10]

     3.3 クリックして **次**します。



4.  **CloudPassage でのシングル サインオンの構成** ] ページで [ **証明書のダウンロード**, 、お使いのコンピューターにローカルで証明書ファイルを保存します。 <br><br>![シングル サインオンを構成します。][11]

5. 別の Web ブラウザーのウィンドウで、管理者として CloudPassage 企業サイトにサインオンします。

6. 上部にあるメニュー、[ **設定**, 、クリックして **サイトの管理**します。 <br><br> ![シングル サインオンを構成します。][12]

7. クリックして、 **認証設定** ] タブをクリックします。 <br><br> ![シングル サインオンを構成します。][13]


8.  **シングル サインオン設定** セクションで、次の手順に従います。 <br><br> ![シングル サインオンを構成します。][14]


     8.1. In the Azure portal, on the **Configure single sign-on at CloudPassage** dialog page, copy the **Issuer URL** value, and then paste it into the **SAML issuer URL** textbox.

     8.2. In the Azure portal, on the **Configure single sign-on at CloudPassage** dialog page, copy the **Service Provider (SP) initiated endpoint** value, and then paste it into the **SAML endpoint URL** textbox.

     8.3. In the Azure portal, on the **Configure single sign-on at CloudPassage** dialog page, copy the **Logout URL** value, and then paste it into the **Logout landing page** textbox.

     8.4. Create a **base-64** encoded file from your downloaded certificate. 
          
      >[AZURE.TIP] For more details, see [How to convert a binary certificate into a text file](http://youtu.be/PlgrzUZ-Y1o).

     8.5. Open your base-64 encoded certificate in notepad, copy the content of it into your clipboard, and then paste it into the **x 509 certificate** textbox.

     8.6. Click **Save**.


9. Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **次**します。 <br><br> ![シングル サインオンを構成します。][15]


10.  **シングル サインオンによる確認** ] ページで [ **完了**します。 <br><br> ![シングル サインオンを構成します。][16]



11. 上部にある nu、クリックして **属性** を開くには、 **SAML トークン属性** ダイアログ。 <br><br> ![シングル サインオンを構成します。][17]

12. 次の表の行ごとに、必要なユーザー属性を追加するには、次の手順を実行します。 <br>

| 属性名 | 属性値 |
| --- | --- |
| firstname | User.givenname |
| lastname | User.surname |
| email | User.mail |  

     12.1. Click **add user attribute**. <br><br> ![Configure Single Sign-On][18]

     12.2. In the **Attribute Name** textbox, type the attribute name shown for that row and as **Attribure Value**, select the attribute value shown for that row . <br><br> ![Configure Single Sign-On][19]
     
     12.2.3 Click **Complete**.


13. 下部にある tollbar、クリックして **変更を適用**します。 <br><br> ![シングル サインオンを構成します。][20]



### Azure AD のテスト ユーザーの作成

このセクションの目的は、Azure ポータルで Britta Simon というテスト ユーザーを作成することです。<br><br>
ユーザーの一覧で選択 **Britta Simon**します。<br>![Azure AD テスト ユーザーを作成します。](./media/active-directory-saas-cloudpassage-tutorial/create_aaduser_01.png)

**Azure AD でテスト ユーザーを作成するには、次の手順に従います。**

1.  **Azure 管理ポータル**, 、左側のナビゲーション ウィンドウで、次のようにクリックします。 **Active Directory**します。<br>
![Azure AD テスト ユーザーを作成します。](./media/active-directory-saas-cloudpassage-tutorial/create_aaduser_02.png) 

2.  **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3. 上部のメニューで、ユーザーの一覧を表示する] をクリックして **ユーザー**します。<br>![Azure AD テスト ユーザーを作成します。](./media/active-directory-saas-cloudpassage-tutorial/create_aaduser_03.png) 
 
4. 開くには、 **ユーザーの追加** ] ダイアログ ボックスで、下部にあるツールバーをクリックして **ユーザーの追加**します。 <br>![Azure AD テスト ユーザーを作成します。](./media/active-directory-saas-cloudpassage-tutorial/create_aaduser_04.png) 

5.  **このユーザーについてお聞かせ** ] ダイアログ ページで、次の手順を実行します。 <br>![Azure AD テスト ユーザーを作成します。](./media/active-directory-saas-cloudpassage-tutorial/create_aaduser_05.png) 
  1. [ユーザーの種類] として [組織内の新しいユーザー] を選択します。
  2. [ユーザー名 **textbox**, 、型 **BrittaSimon**します。
  3. [次へ] をクリックします。

6.   **ユーザー プロファイル** ] ダイアログ ページで、次の手順を実行します。 <br>![Azure AD テスト ユーザーを作成します。](./media/active-directory-saas-cloudpassage-tutorial/create_aaduser_06.png) 
  1.  **名** ] ボックスに「 **Britta**します。  
  2.  **姓** ] ボックスに、型、 **Simon**します。
  3.  **表示名** ] ボックスに「 **Britta Simon**します。
  4.  **ロール** 一覧で、[ **ユーザー**します。
  5. クリックして **次**します。

7.  **一時パスワードの取得** ダイアログ ページで、をクリックして **作成**します。<br>![Azure AD テスト ユーザーを作成します。](./media/active-directory-saas-cloudpassage-tutorial/create_aaduser_07.png) 
 
8.  **一時パスワードの取得** ] ダイアログ ページで、次の手順を実行します。<br>![Azure AD テスト ユーザーを作成します。](./media/active-directory-saas-cloudpassage-tutorial/create_aaduser_08.png) 
  1. 値を書き留めて、 **新しいパスワード**します。
  2. クリックして **完了**します。   


  
 
### CloudPassage テスト ユーザーの作成

このセクションの目的は、CloudPassage で Britta Simon というユーザーを作成することです。

#### CloudPassage で Britta Simon というユーザーを作成するには、次の手順に従います。

1.  サインオン、 **CloudPassage** 会社サイトに管理者として。 

2.  上部にあるツールバーで、クリックして **設定**, 、クリックして **サイトの管理**します。 <br>![CloudPassage テスト ユーザーの作成][22] 

3.  クリックして、 **ユーザー** タブをクリックし、をクリックして **Add New User**します。 <br>![CloudPassage テスト ユーザーの作成][23]
    
4.   **Add New User** セクションで、次の手順に従います。 <br>![CloudPassage テスト ユーザーの作成][24]

     4.1.  **名** ] ボックスに、Britta を入力します。

     4.2.  **姓** ] ボックスに、Simon を入力します。

     4.3.  **ユーザー名** ] ボックスに、 **電子メール** ] テキスト ボックスおよび **電子メールの再入力** ] ボックスに、Azure AD の Britta のユーザー名を入力します。

     4.4. として **アクセスの種類**, [ **Halo ポータル アクセスの有効化**します。

     4.5. クリックして **追加**します。










### Azure AD テスト ユーザーの割り当て

このセクションでは、CloudPassage に自分のアクセスを付与することによって Azure シングル サインオンを使用する Britta Simon を有効にします。
<br><br>![ユーザーの割り当て][30]

**Britta Simon を CloudPassage に割り当てるには、次の手順を実行します。**

1. Azure ポータルで、ディレクトリ ビューでアプリケーション ビューを開く] をクリックして **アプリケーション** 上部のメニュー。<br>
<br><br>![ユーザーを割り当てる][26]
2. アプリケーションの一覧で選択 **CloudPassage**します。
<br><br>![ユーザーを割り当てる][27]
1. 上部にあるメニュー [ **ユーザー**します。<br>
<br><br>![ユーザーを割り当てる][25]
1. ユーザーの一覧で選択 **Britta Simon**します。

2. 下部にあるツールバー [ **割り当てる**します。
<br><br>![ユーザーを割り当てる][29]



### シングル サインオンのテスト

このセクションでは、構成のテスト Azure AD シングル サインオン アクセス パネルを使用します。<br>
アクセス パネルで [CloudPassage] タイルをクリックすると、自動的に CloudPassage アプリケーションにサインオンします。


## その他のリソース

* [SaaS アプリと Azure Active Directory を統合する方法に関するチュートリアルの一覧](active-directory-saas-tutorial-list.md)
* [Azure Active Directory のアプリケーション アクセスとシングル サインオンとは](active-directory-appssoaccess-whatis.md)

<!--Image references-->
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























