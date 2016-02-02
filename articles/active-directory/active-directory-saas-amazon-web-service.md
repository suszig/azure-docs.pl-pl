<properties
    pageTitle="チュートリアル: Azure Active Directory と Amazon Web Service (AWS) の統合 | Microsoft Azure"
    description="Azure Active Directory で Amazon Web Services (AWS) を使用して、シングル サインオンや自動プロビジョニングなどを有効にする方法について説明します。"
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
    ms.date="09/30/2015"
    ms.author="markvi"/>



# チュートリアル: Azure Active Directory と Amazon Web Service (AWS) の統合

このチュートリアルでは、Amazon Web サービス (AWS) を Azure Active Directory (Azure AD) に統合する方法を説明します。<br>統合 Amazon Web サービス (AWS) と Azure AD を使うと、次の利点があります。

- Amazon Web Service (AWS) にアクセスする Azure AD ユーザーを制御できます。
- ユーザーが自分の Azure AD アカウントで自動的に Amazon Web Service (AWS) にサインオンできるようにします (シングル サインオン)。
- 1 つの中央サイト (Azure Active Directory ポータル) でアカウントを管理できます。

Azure AD と SaaS アプリの統合に関する詳細を確認するを参照して [アプリケーションへのアクセスと Azure Active Directory でのシングル サインオン](active-directory-appssoaccess-whatis.md)します。

## 前提条件

Amazon Web Service (AWS) と Azure AD の統合を構成するには、次のものが必要です。

- Azure AD サブスクリプション
- Amazon Web Service (AWS) でのシングル サインオンが有効なサブスクリプション


> [AZURE.NOTE] このチュートリアルの手順をテストする場合、運用環境を使用しないことをお勧めします。


このチュートリアルの手順をテストするには、次の推奨事項に従ってください。

- 必要な場合を除き、運用環境は使用しないでください。
- Azure AD の評価環境を取得していない場合は、1 か月の試用版を入手できます [ここ](https://azure.microsoft.com/pricing/free-trial/)します。


## シナリオの説明

このチュートリアルでは、使用すると、テスト環境で Azure AD シングル サインオンをテストします。 <br>
このチュートリアルで紹介するシナリオは、次の 3 つの主な構成要素で構成されます。

1. ギャラリーからの Amazon Web Service (AWS) の追加
2. Azure AD シングル サインオンの構成とテスト


## ギャラリーからの Amazon Web Service (AWS) の追加

Azure AD への Amazon Web Service (AWS) の統合を構成するには、ギャラリーから管理対象 SaaS アプリの一覧に Amazon Web Service (AWS) を追加する必要があります。

### ギャラリーから Amazon Web Service (AWS) を追加するには、次の手順を実行します。

1. **Microsoft Azure 管理ポータル**の左側のナビゲーション ウィンドウで、**[Active Directory]** をクリックします。 
   <br><br>![Active Directory][1]<br>

2. **[ディレクトリ]** の一覧から、ディレクトリ統合を有効にするディレクトリを選択します。

3. アプリケーション ビューを開くには、ディレクトリ ビューでトップ メニューの **[アプリケーション]** をクリックします。 
   <br><br>![アプリケーション][2]<br>

4. ページの下部にある **[追加]** をクリックします。 
   <br><br>![アプリケーション][3]<br>

5. **[実行する内容]** ダイアログで、**[ギャラリーからアプリケーションを追加します]** をクリックします。 
   <br><br>![アプリケーション][4]<br>

6. 検索ボックスに入力 **Amazon Web サービス (AWS)**します。
   <br><br>![アプリケーション][5]<br>

7. 結果ウィンドウで [ **Amazon Web サービス (AWS)**, 、クリックして **完了** アプリケーションを追加します。
   <br><br>![アプリケーション][6]<br>



## Azure AD シングル サインオンの構成とテスト

このセクションの目的は、"Britta Simon" というテスト ユーザーを使用して、Amazon Web Service (AWS) とのAzure AD シングル サインオンを構成してテストする方法を示すことです。

シングル サインオンを機能させるには、Azure AD ユーザーに対応する Amazon Web Service (AWS) ユーザーが Azure AD で認識される必要があります。つまり、Azure AD のユーザーと関連するユーザー Amazon Web サービス (AWS) の間のリンクの関係を確立する必要があります。<br>
このリンク関係は、Azure AD の **[ユーザー名]** の値を、Amazon Web Service (AWS) の **[Username]** の値として割り当てることで確立されます。

Amazon Web Service (AWS) との Azure AD シングル サインオンを構成してテストするには、次の構成要素を完了する必要があります。

1. * *[Azure AD を構成する 1 つシングル サインオン](#configuring-azure-ad-single-single-sign-on)* * - この機能を使用するユーザーをできるようにします。
2. * *[Azure AD テスト ユーザーを作成する](#creating-an-azure-ad-test-user)* * - を Azure AD シングル サインオン Britta Simon でテストします。
4. * *[Amazon Web サービス (AWS) のテスト ユーザーを作成する](#creating-a-halogen-software-test-user)* * - Britta Simon に対応する Amazon Web サービス (AWS) 自分の Azure AD の表現にリンクされています。
5. * *[Azure AD テスト ユーザーの割り当て](#assigning-the-azure-ad-test-user)* * - Azure AD シングル サインオンを使用する Britta Simon をできるようにします。
5. * *[シングル サインオンをテスト](#testing-single-sign-on)* * - 構成が機能するかどうかを確認します。

### 単一の Azure AD シングル サインオンの構成

このセクションでは、Azure AD シングル サインオン Azure AD ポータルで有効にし、Amazon Web サービス (AWS) アプリケーションでシングル サインオンを構成するにです。<br>
Amazon Web Service (AWS) アプリケーションは、特定の形式の SAML アサーションを使用するため、カスタム属性のマッピングを **SAML トークン属性**の構成に追加する必要があります。 
次のスクリーンショットはその例です。


<br><br>![シングル サインオンを構成します。][27]<br>

**Amazon Web Service (AWS) との Azure AD シングル サインオンを構成するには、次の手順を実行します。**

1. Azure AD ポータルでの **Amazon Web サービス (AWS)** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、 **シングル サインオンを構成する**  ダイアログ。
<br><br>![シングル サインオンを構成します。][7]<br>

2. **どのような Amazon Web サービス (AWS) にサインオン** ] ページで、[ **Azure AD シングル サインオン**, 、] をクリックし、 **次**します。
<br><br>![シングル サインオンを構成します。][8]<br>

3. **アプリケーション設定の構成** ] ダイアログ ページで、[次へ] をクリックします。 
<br><br>![アプリ設定を構成します。][9]<br>

4. **Amazon Web サービス (AWS) でのシングル サインオン構成** ] ページで [ **メタデータのダウンロード**, 、コンピューターのローカルにメタデータ ファイルを保存します。
<br><br>![シングル サインオンを構成します。][10]<br>

5. 別の Web ブラウザーのウィンドウで、管理者として Amazon Web Service (AWS) 企業サイトにサインオンします。

6. クリックして **コンソール ホーム**します。
<br><br>![シングル サインオンを構成します。][11]<br>

7. クリックして **Id およびアクセス管理**します。 
<br><br>![シングル サインオンを構成します。][12]<br>

8. をクリックして **Id プロバイダー**, 、] をクリックし、 **Create Provider**します。 
<br><br>![シングル サインオンを構成します。][13]<br>

9. **プロバイダーの構成** ] ダイアログ ページで、次の手順を実行します。 
<br><br>![シングル サインオンを構成します。][14]<br>

     a. **[Provider Type]** として **[SAML]** を選択します。

     b. **[Provider Name]** ボックスにプロバイダー名を入力します (例: *WAAD*)。

     c. **[Choose File]** をクリックして、ダウンロードしたメタデータ ファイルをアップロードします。

     d. **[Next Step]** をクリックします。

10. **プロバイダー情報のことを確認** ダイアログ ページで、をクリックして **作成**します。 
<br><br>![シングル サインオンを構成します。][15]<br>

11. クリックして **ロール**, 、] をクリックし、 **新しいロールの作成**します。 
<br><br>![シングル サインオンを構成します。][16]<br>

12. **ロール名の設定** ] ダイアログ ボックスで、次の手順を実行します。 
<br><br>![シングル サインオンを構成します。][17]<br>

     a. **[Role Name]** ボックスにロール名を入力します (例: *TestUser*)。

     b. **[Next Step]** をクリックします。

13. **ロールの種類の選択** ] ダイアログ ボックスで、次の手順を実行します。 
<br><br>![シングル サインオンを構成します。][18]<br>

     a. **[Role For Identity Provider Access]** を選択します。

     b. **[Grant Web Single Sign-On (WebSSO) access to SAML providers]** セクションで、**[Select]** をクリックします。

14. **信頼を確立** ] ダイアログ ボックスで、次の手順を実行します。  
<br><br>![シングル サインオンを構成します。][19]<br>

     a. SAML プロバイダーとして、前に作成した SAML プロバイダーを選択します (例: *WAAD*)。

     b. **[Next Step]** をクリックします。

15. **ロールの信頼の確認** ダイアログ ボックスで、をクリックして **次の手順**します。 
<br><br>![シングル サインオンを構成します。][32]<br>

16. **アタッチ ポリシー** ダイアログ ボックスで、をクリックして **次の手順**します。  
<br><br>![シングル サインオンを構成します。][33]<br>

17. **レビュー** ] ダイアログ ボックスで、次の手順を実行します。   
<br><br>![シングル サインオンを構成します。][34]<br>

     a. **Role の ARN 値** をコピーします。

     b. **Trusted Entities の ARN 値**をコピーします。

     c. **[Create Role]** をクリックします。

18. Azure AD ポータルで、シングル サインオンの構成確認を選択し、**[次へ]** をクリックします。
<br><br>![Azure AD Connect とは][20]<br>

19. **シングル サインオンの確認** ] ページで [ **完了** を閉じる、 **でのシングル サインオンを構成する** ] ダイアログ。
<br><br>![Azure AD Connect とは][22]<br>

20. 上部のメニューで、**[属性]** をクリックして、**[SAML トークン属性]** ダイアログを開きます。 
<br><br>![シングル サインオンを構成します。][21]<br>

21. **[ユーザー属性の追加]** をクリックします。 
<br><br>![シングル サインオンを構成します。][23]<br>

22. ユーザー属性の追加] ダイアログ ボックスでは、次の手順を実行します。 
<br><br>![シングル サインオンを構成します。][24]<br>

     a.  **属性名** ] ボックスに「 **https://aws.amazon.com/SAML/Attributes/Role**します。

     b. **[属性値]** ボックスに、**[Role の ARN 値] と [Trusted Entity の ARN 値]** を入力します。
     >[AZURE.TIP] これらは、ロールの作成時に [Review] ダイアログでコピーした値です。 

     c. **[完了]** をクリックして **[ユーザー属性の追加]** ダイアログボックスを閉じます。

23. **[ユーザー属性の追加]** をクリックします。 
<br><br>![シングル サインオンを構成します。][23]<br>

24. ユーザー属性の追加] ダイアログ ボックスでは、次の手順を実行します。 
<br><br>![シングル サインオンを構成します。][25]<br>

     a.  **属性名** ] ボックスに「 **https://aws.amazon.com/SAML/Attributes/RoleSessionName**します。

     b. **[属性名]** ボックスに、「**userprincipalname**」と入力します。

     c. **[完了]** をクリックして **[ユーザー属性の追加]** ダイアログボックスを閉じます。

25. **[変更の適用]** をクリックします。 
<br><br>![シングル サインオンを構成します。][26]<br>





### Azure AD のテスト ユーザーの作成

このセクションでは、Britta Simon と呼ばれる Azure ポータルでテスト ユーザーを作成します。<br>
ユーザーの一覧で選択 **Britta Simon**.<br><br>![Azure AD テスト ユーザーを作成する](./media/active-directory-saas-amazon-web-service/create_aaduser_01.png)<br>

**Azure AD でテスト ユーザーを作成するには、次の手順に従います。**

1. **Microsoft Azure 管理ポータル**の左側のナビゲーション ウィンドウで、**[Active Directory]** をクリックします。
<br><br>![Azure AD テスト ユーザーを作成します。](./media/active-directory-saas-amazon-web-service/create_aaduser_02.png)<br>

2. **[ディレクトリ]** の一覧から、ディレクトリ統合を有効にするディレクトリを選択します。

3. 上部のメニューで、ユーザーの一覧を表示する] をクリックして **ユーザー**します。
<br><br>![Azure AD テスト ユーザーを作成します。](./media/active-directory-saas-amazon-web-service/create_aaduser_03.png)<br>

4. 開くには、 **ユーザーの追加** ] ダイアログ ボックスで、下部にあるツールバーをクリックして **ユーザーの追加**します。 
<br><br>![Azure AD テスト ユーザーを作成します。](./media/active-directory-saas-amazon-web-service/create_aaduser_04.png)<br>

5. **このユーザーについてお聞かせ** ] ダイアログ ページで、次の手順を実行します。 
<br><br>![Azure AD テスト ユーザーを作成します。](./media/active-directory-saas-amazon-web-service/create_aaduser_05.png)<br>

  1. [ユーザーの種類] として [組織内の新しいユーザー] を選択します。
  2. **[ユーザー名]** テキスト ボックスに「**BrittaSimon**」と入力します。
  3. [次へ] をクリックします。

6.  **ユーザー プロファイル** ] ダイアログ ページで、次の手順を実行します。 
<br><br>![Azure AD テスト ユーザーを作成します。](./media/active-directory-saas-amazon-web-service/create_aaduser_06.png)<br>

  1. **[名]** ボックスに「**Britta**」と入力します。
  2. **[姓]** ボックスに「**Simon**」と入力します。
  3. **[表示名]** ボックスに「**Britta Simon**」と入力します。
  4. **[ロール]** 一覧で **[ユーザー]** を選択します。
  5. **[次へ]** をクリックします。

7. **一時パスワードの取得** ダイアログ ページで、をクリックして **作成**します。
<br><br>![Azure AD テスト ユーザーを作成します。](./media/active-directory-saas-amazon-web-service/create_aaduser_07.png)<br>

8. **一時パスワードの取得** ] ダイアログ ページで、次の手順を実行します。
<br><br>![Azure AD テスト ユーザーを作成します。](./media/active-directory-saas-amazon-web-service/create_aaduser_08.png)<br>

  1. **[新しいパスワード]** の値を書き留めます。
  2. **[完了]** をクリックします。


### Amazon Web Service (AWS) テスト ユーザーの作成

このセクションの目的は、Amazon Web Service (AWS) で Britta Simon というユーザーを作成することです。

### Amazon Web Service (AWS) で Britta Simon というユーザーを作成するには、次の手順に従います。

1. 管理者として **Amazon Web Service (AWS)** 企業サイトにログインします。

2. クリックして、 **コンソール ホーム** アイコン。 
<br><br>![シングル サインオンを構成します。][11]<br>

3. ユーザー] をクリックし、アクセス管理します。 
<br><br>![シングル サインオンを構成します。][28]<br>

4. ダッシュ ボードで、ユーザーをクリックし、新しいユーザーの作成] をクリックします。 
<br><br>![シングル サインオンを構成します。][29]<br>

5. ユーザーの作成] ダイアログ ボックスでは、次の手順を実行します。 
<br><br>![シングル サインオンを構成します。][30]<br>

     a. **[Enter User Names]** ボックスに、Brita Simon の Azure AD でのユーザー名 (userprincipalname) を入力します。

     b. **[作成]** をクリックします。




### Azure AD テスト ユーザーの割り当て

このセクションの目的は、Britta Simon に Amazon Web Service (AWS) へのアクセスを許可することで、このユーザーが Azure シングル サインオンを使用できるようにすることです。

![ユーザーの割り当て][31]

**Britta Simon を CloudPassage に割り当てるには、次の手順を実行します。**

1. Azure ポータルで、ディレクトリ ビューでアプリケーション ビューを開く] をクリックして **アプリケーション** 上部のメニュー。
<br><br>![ユーザーを割り当てる][26]<br>

2. アプリケーションの一覧で選択 **Amazon Web サービス (AWS)**します。
<br><br>![ユーザーを割り当てる][27]<br>

1. 上部のメニューで **[ユーザー]** をクリックします。
<br><br>![ユーザーを割り当てる][25]<br>

1. ユーザーの一覧で **[Britta Simon]** を選択します。

2. 下部にあるツールバー [ **割り当てる**します。
<br><br>![ユーザーを割り当てる][29]<br>

### シングル サインオンのテスト

このセクションでは、構成のテスト Azure AD シングル サインオン アクセス パネルを使用します。<br>
アクセス パネルで [Amazon Web Service (AWS)] タイルをクリックすると、自動的に Amazon Web Service (AWS) アプリケーションにサインオンします。


## その他のリソース

* [Azure Active Directory による SaaS アプリを統合する方法に関するチュートリアルの一覧](active-directory-saas-tutorial-list.md)
* [アプリケーション アクセスと Azure Active Directory でのシングル サインオンとは](active-directory-appssoaccess-whatis.md)



[1]: ./media/active-directory-saas-amazon-web-service/tutorial_general_01.png 
[2]: ./media/active-directory-saas-amazon-web-service/tutorial_general_02.png 
[3]: ./media/active-directory-saas-amazon-web-service/tutorial_general_03.png 
[4]: ./media/active-directory-saas-amazon-web-service/tutorial_general_04.png 
[5]: ./media/active-directory-saas-amazon-web-service/ic795019.png 
[6]: ./media/active-directory-saas-amazon-web-service/ic795020.png 
[7]: ./media/active-directory-saas-amazon-web-service/ic795027.png 
[8]: ./media/active-directory-saas-amazon-web-service/ic795028.png 
[9]: ./media/active-directory-saas-amazon-web-service/capture23.png 
[10]: ./media/active-directory-saas-amazon-web-service/capture24.png 
[11]: ./media/active-directory-saas-amazon-web-service/ic795031.png 
[12]: ./media/active-directory-saas-amazon-web-service/ic795032.png 
[13]: ./media/active-directory-saas-amazon-web-service/ic795033.png 
[14]: ./media/active-directory-saas-amazon-web-service/ic795034.png 
[15]: ./media/active-directory-saas-amazon-web-service/ic795035.png 
[16]: ./media/active-directory-saas-amazon-web-service/ic795022.png 
[17]: ./media/active-directory-saas-amazon-web-service/ic795023.png 
[18]: ./media/active-directory-saas-amazon-web-service/ic795024.png 
[19]: ./media/active-directory-saas-amazon-web-service/ic795025.png 
[20]: ./media/active-directory-saas-amazon-web-service/ic7950351.png 
[21]: ./media/active-directory-saas-amazon-web-service/tutorial_general_80.png 
[22]: ./media/active-directory-saas-amazon-web-service/ic7950352.png 
[23]: ./media/active-directory-saas-amazon-web-service/tutorial_general_81.png 
[24]: ./media/active-directory-saas-amazon-web-service/ic7950353.png 
[25]: ./media/active-directory-saas-amazon-web-service/tutorial_general_15.png 
[26]: ./media/active-directory-saas-amazon-web-service/tutorial_general_18.png 
[27]: ./media/active-directory-saas-amazon-web-service/ic7950357.png 
[28]: ./media/active-directory-saas-amazon-web-service/ic7950321.png 
[29]: ./media/active-directory-saas-amazon-web-service/tutorial_general_16.png 
[30]: ./media/active-directory-saas-amazon-web-service/ic795038.png 
[31]: ./media/active-directory-saas-amazon-web-service/tutorial_general_17.png 
[32]: ./media/active-directory-saas-amazon-web-service/ic7950251.png 
[33]: ./media/active-directory-saas-amazon-web-service/ic7950252.png 
[34]: ./media/active-directory-saas-amazon-web-service/ic7950253.png 

