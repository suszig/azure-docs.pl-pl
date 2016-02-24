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

このチュートリアルでは、Amazon Web サービス (AWS) を Azure Active Directory (Azure AD) に統合する方法を説明します。<br>Amazon Web サービス (AWS) を Azure AD と統合するには、次のメリットがあります。 

- Amazon Web Service (AWS) にアクセスする Azure AD ユーザーを制御できます。 
- ユーザーが自分の Azure AD アカウントで自動的に Amazon Web Service (AWS) にサインオンできるようにします (シングル サインオン)。
- 1 つの中央サイト (Azure Active Directory ポータル) でアカウントを管理できます。

Azure AD と SaaS アプリの統合に関する詳細を確認するを参照して [アプリケーションへのアクセスと Azure Active Directory でのシングル サインオン](active-directory-appssoaccess-whatis.md)します。

## 前提条件 

Amazon Web Service (AWS) と Azure AD の統合を構成するには、次のものが必要です。

- Azure AD サブスクリプション
- Amazon Web Service (AWS) でのシングル サインオンが有効なサブスクリプション


> [AZURE.NOTE] このチュートリアルでは、手順をテストするには、実稼働環境の使用をおしないでいます。


このチュートリアルの手順をテストするには、次の推奨事項に従ってください。

- 必要な場合を除き、運用環境は使用しないでください。
- Azure AD の評価環境を取得していない場合は、1 か月の試用版を入手できます [ここ](https://azure.microsoft.com/pricing/free-trial/)します。 

 
## シナリオの説明
このチュートリアルの目的は、テスト環境で Azure AD のシングル サインオンをテストできるようにすることです。 <br>
このチュートリアルで紹介するシナリオは、次の 3 つの主な構成要素で構成されます。

1. ギャラリーからの Amazon Web Service (AWS) の追加 
2. Azure AD シングル サインオンの構成とテスト


## ギャラリーからの Amazon Web Service (AWS) の追加
Azure AD への Amazon Web Service (AWS) の統合を構成するには、ギャラリーから管理対象 SaaS アプリの一覧に Amazon Web Service (AWS) を追加する必要があります。

### ギャラリーから Amazon Web Service (AWS) を追加するには、次の手順を実行します。

1.  **Azure 管理ポータル**, 、左側のナビゲーション ウィンドウで、次のようにクリックします。 **Active Directory**します。 
   <br><br>![Active Directory][1]<br> 

2.  **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3. ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。 
   <br><br>![アプリケーション][2]<br>

4. クリックして **追加** ページの下部にあります。 
   <br><br>![アプリケーション][3]<br>

5.  **実行する操作** ダイアログ ボックスで、をクリックして **ギャラリーからアプリケーションを追加**します。 
   <br><br>![アプリケーション][4]<br>

6. 検索ボックスに入力 **Amazon Web サービス (AWS)**します。
   <br><br>![アプリケーション][5]<br>

7. 結果ウィンドウで [ **Amazon Web サービス (AWS)**, 、クリックして **完了** アプリケーションを追加します。
   <br><br>![アプリケーション][6]<br>



##  Azure AD シングル サインオンの構成とテスト
このセクションの目的は、"Britta Simon" というテスト ユーザーを使用して、Amazon Web Service (AWS) とのAzure AD シングル サインオンを構成してテストする方法を示すことです。

シングル サインオンを機能させるには、Azure AD ユーザーに対応する Amazon Web Service (AWS) ユーザーが Azure AD で認識される必要があります。 言い換えると、Azure AD ユーザーと Amazon Web Service (AWS) の関連ユーザーの間で、リンク関係が確立されている必要があります。<br>
値を割り当てることでこのリンクの関係が確立される、 **ユーザー名** の値として Azure AD で、 **Username** Amazon Web サービス (AWS)。
 
Amazon Web Service (AWS) との Azure AD シングル サインオンを構成してテストするには、次の構成要素を完了する必要があります。

1. **[Azure AD シングル シングル サインオンを構成する](#configuring-azure-ad-single-single-sign-on)** - この機能を使用するユーザーをできるようにします。
2. **[Azure AD テスト ユーザーを作成する](#creating-an-azure-ad-test-user)** - を Azure AD シングル サインオン Britta Simon でテストします。
4. **[Amazon Web サービス (AWS) のテスト ユーザーを作成する](#creating-a-halogen-software-test-user)** - Britta Simon に対応する Amazon Web サービス (AWS) 自分の Azure AD の表現にリンクされています。
5. **[Azure AD テスト ユーザーの割り当て](#assigning-the-azure-ad-test-user)** - Azure AD シングル サインオンを使用する Britta Simon をできるようにします。
5. **[シングル サインオン テスト](#testing-single-sign-on)** - 構成が機能するかどうかを確認します。

### 単一の Azure AD シングル サインオンの構成

このセクションの目的は、Azure AD ポータルで Azure AD シングル サインオンを有効にし、Amazon Web Service (AWS) アプリケーションでのシングル サインオンを構成することです。<br>
Amazon Web サービス (AWS) アプリケーションにはカスタム属性マッピングを追加する必要がある特定の形式での SAML アサーションが必要ですが、 **saml トークン属性** 構成します。 
次のスクリーンショットはその例です。


<br><br>![シングル サインオンを構成する][27]<br>

**Amazon Web Service (AWS) との Azure AD シングル サインオンを構成するには、次の手順を実行します。**

1. Azure AD ポータルでの **Amazon Web サービス (AWS)** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、 **シングル サインオンを構成する**  ダイアログ。
<br><br>![シングル サインオンを構成する][7]<br>

2.  **どのような Amazon Web サービス (AWS) にサインオン** ] ページで、[ **Azure AD シングル サインオン**, 、] をクリックし、 **次**します。
<br><br>![シングル サインオンを構成する][8]<br>

3.  **アプリケーション設定の構成** ] ダイアログ ページで、[次へ] をクリックします。 
<br><br>![アプリ設定の構成][9]<br>
 
4.  **Amazon Web サービス (AWS) でのシングル サインオン構成** ] ページで [ **メタデータのダウンロード**, 、コンピューターのローカルにメタデータ ファイルを保存します。
<br><br>![シングル サインオンを構成する][10]<br>

5. 別の Web ブラウザーのウィンドウで、管理者として Amazon Web Service (AWS) 企業サイトにサインオンします。

6. クリックして **コンソール ホーム**します。
<br><br>![シングル サインオンを構成する][11]<br>

7. クリックして **Id およびアクセス管理**します。 
<br><br>![シングル サインオンを構成する][12]<br>

8. をクリックして **Id プロバイダー**, 、] をクリックし、 **Create Provider**します。 
<br><br>![シングル サインオンを構成する][13]<br>

9.  **プロバイダーの構成** ] ダイアログ ページで、次の手順を実行します。 
<br><br>![シングル サインオンを構成する][14]<br>

     a. として **プロバイダーの種類**, [ **SAML**します。

     b.  **プロバイダー名** ] ボックスに、プロバイダーの名前を入力 (例:: *WAAD*)。

     c. ダウンロードしたメタデータ ファイルをアップロードする] をクリックして **Choose File**します。

     d. クリックして **次のステップ**します。


10.  **プロバイダー情報のことを確認** ダイアログ ページで、をクリックして **作成**します。 
<br><br>![シングル サインオンを構成する][15]<br>

11. クリックして **ロール**, 、] をクリックし、 **新しいロールの作成**します。 
<br><br>![シングル サインオンを構成する][16]<br>

12.  **ロール名の設定** ] ダイアログ ボックスで、次の手順を実行します。 
<br><br>![シングル サインオンを構成する][17]<br>

     a.  **ロール名** ] ボックスに、ロールの名前を入力 (例:: *TestUser*)。

     b. クリックして **次のステップ**します。

13.  **ロールの種類の選択** ] ダイアログ ボックスで、次の手順を実行します。 
<br><br>![シングル サインオンを構成する][18]<br>

     a. 選択 **の Id プロバイダーのアクセスのロール**します。

     b.  **SAML プロバイダーへのアクセスの使用許可 Web シングル サインオン (WebSSO)** ] をクリックして **選択**します。


14.  **信頼を確立** ] ダイアログ ボックスで、次の手順を実行します。  
<br><br>![シングル サインオンを構成する][19]<br>
     
     a. SAML プロバイダーの場合は、プロバイダーを選択して、SAML previousley を作成した (例:: *WAAD*) 

     b. クリックして **次のステップ**します。


15.  **ロールの信頼の確認** ダイアログ ボックスで、をクリックして **次の手順**します。 
<br><br>![シングル サインオンを構成する][32]<br>


16.  **アタッチ ポリシー** ダイアログ ボックスで、をクリックして **次の手順**します。  
<br><br>![シングル サインオンを構成する][33]<br>


17.  **レビュー** ] ダイアログ ボックスで、次の手順を実行します。   
<br><br>![シングル サインオンを構成する][34]<br>

     a. コピー、 **ロール ARN** 値。

     b. コピー、 **信頼されたエンティティ** ARN 値。

     c. クリックして **ロールを作成する**です。 

18. Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **次**します。
<br><br>![Azure AD Connect は][20]<br>

19.  **シングル サインオンの確認** ] ページで [ **完了** を閉じる、 **でのシングル サインオンを構成する** ] ダイアログ。
<br><br>![Azure AD Connect は][22]<br>


20. 上部にあるメニュー [ **属性** を開くには、 **SAML トークン属性** ダイアログ。 
<br><br>![シングル サインオンを構成する][21]<br>

21. クリックして **ユーザー属性の追加**します。 
<br><br>![シングル サインオンを構成する][23]<br>

22. ユーザー属性の追加] ダイアログ ボックスでは、次の手順を実行します。 
<br><br>![シングル サインオンを構成する][24]<br> 

     a.  **属性名** ] ボックスに「 **https://aws.amazon.com/SAML/Attributes/Role**します。

     b.  **属性値** ] ボックスに「 **[ロール ARN value] [信頼されたエンティティ ARN value]**します。

     >[AZURE.TIP] これらは、自分のロールを作成したときに、確認ダイアログからコピーした値です。 

     c. クリックして **完了** を閉じる、 **ユーザー属性を追加** ダイアログ。

23. クリックして **ユーザー属性の追加**します。 
<br><br>![シングル サインオンを構成する][23]<br>


24. ユーザー属性の追加] ダイアログ ボックスでは、次の手順を実行します。 
<br><br>![シングル サインオンを構成する][25]<br>


     a. In the **Attribute Name** textbox, type **https://aws.amazon.com/SAML/Attributes/RoleSessionName**.

     b. In the **Attribute Value** textbox, type **userprincipalname**.

     c. Click **Complete** to close the **Add User Attribute** dialog.


25. クリックして **の変更を適用**します。 
<br><br>![シングル サインオンを構成する][26]<br>





### Azure AD のテスト ユーザーの作成

このセクションの目的は、Azure ポータルで Britta Simon というテスト ユーザーを作成することです。<br>
ユーザーの一覧で選択 **Britta Simon**.<br><br>![Azure AD テスト ユーザーを作成します。](./media/active-directory-saas-amazon-web-service/create_aaduser_01.png)<br>

**Azure AD でテスト ユーザーを作成するには、次の手順に従います。**

1.  **Azure 管理ポータル**, 、左側のナビゲーション ウィンドウで、次のようにクリックします。 **Active Directory**します。
<br><br>![Azure AD テスト ユーザーを作成します。](./media/active-directory-saas-amazon-web-service/create_aaduser_02.png)<br> 

2.  **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3. 上部のメニューで、ユーザーの一覧を表示する] をクリックして **ユーザー**します。
<br><br>![Azure AD テスト ユーザーを作成します。](./media/active-directory-saas-amazon-web-service/create_aaduser_03.png)<br> 
 
4. 開くには、 **ユーザーの追加** ] ダイアログ ボックスで、下部にあるツールバーをクリックして **ユーザーの追加**します。 
<br><br>![Azure AD テスト ユーザーを作成します。](./media/active-directory-saas-amazon-web-service/create_aaduser_04.png)<br> 

5.  **このユーザーについてお聞かせ** ] ダイアログ ページで、次の手順を実行します。 
<br><br>![Azure AD テスト ユーザーを作成します。](./media/active-directory-saas-amazon-web-service/create_aaduser_05.png)<br> 

  1. [ユーザーの種類] として [組織内の新しいユーザー] を選択します。
  2. [ユーザー名 **textbox**, 、型 **BrittaSimon**します。
  3. [次へ] をクリックします。

6.   **ユーザー プロファイル** ] ダイアログ ページで、次の手順を実行します。 
<br><br>![Azure AD テスト ユーザーを作成します。](./media/active-directory-saas-amazon-web-service/create_aaduser_06.png)<br> 

  1.  **名** ] ボックスに「 **Britta**します。  
  2.  **姓** ボックス型 **Simon**します。
  3.  **表示名** ] ボックスに「 **Britta Simon**します。
  4.  **ロール** 一覧で、[ **ユーザー**します。
  5. クリックして **次**します。

7.  **一時パスワードの取得** ダイアログ ページで、をクリックして **作成**します。
<br><br>![Azure AD テスト ユーザーを作成します。](./media/active-directory-saas-amazon-web-service/create_aaduser_07.png)<br> 
 
8.  **一時パスワードの取得** ] ダイアログ ページで、次の手順を実行します。
<br><br>![Azure AD テスト ユーザーを作成します。](./media/active-directory-saas-amazon-web-service/create_aaduser_08.png)<br> 

  1. 値を書き留めて、 **新しいパスワード**します。
  2. クリックして **完了**します。   
  
 
### Amazon Web Service (AWS) テスト ユーザーの作成

このセクションの目的は、Amazon Web Service (AWS) で Britta Simon というユーザーを作成することです。

### Amazon Web Service (AWS) で Britta Simon というユーザーを作成するには、次の手順に従います。

1. ログインして **Amazon Web サービス (AWS)** 会社サイトに管理者として。

2. クリックして、 **コンソール ホーム** アイコン。 
<br><br>![シングル サインオンを構成する][11]<br>

3. ユーザー] をクリックし、アクセス管理します。 
<br><br>![シングル サインオンを構成する][28]<br>

4. ダッシュ ボードで、ユーザーをクリックし、新しいユーザーの作成] をクリックします。 
<br><br>![シングル サインオンを構成する][29]<br>

5. ユーザーの作成] ダイアログ ボックスでは、次の手順を実行します。 
<br><br>![シングル サインオンを構成する][30]<br>

     a.  **ユーザー名の入力** テキスト ボックスでは、Azure AD で Brita Simon のユーザー名 (userprincipalname) を入力します。

     b. クリックして **作成**します。




### Azure AD テスト ユーザーの割り当て

このセクションの目的は、Britta Simon に Amazon Web Service (AWS) へのアクセスを許可することで、このユーザーが Azure シングル サインオンを使用できるようにすることです。

![ユーザーを割り当てる][31]

**Britta Simon を CloudPassage に割り当てるには、次の手順を実行します。**

1. Azure ポータルで、ディレクトリ ビューでアプリケーション ビューを開く] をクリックして **アプリケーション** 上部のメニュー。
<br><br>![ユーザーを割り当てる][26]<br>

2. アプリケーションの一覧で選択 **Amazon Web サービス (AWS)**します。
<br><br>![ユーザーを割り当てる][27]<br>

1. 上部にあるメニュー [ **ユーザー**します。
<br><br>![ユーザーを割り当てる][25]<br>

1. ユーザーの一覧で選択 **Britta Simon**します。

2. 下部にあるツールバー [ **割り当てる**します。
<br><br>![ユーザーを割り当てる][29]<br>

### シングル サインオンのテスト

このセクションでは、構成のテスト Azure AD シングル サインオン アクセス パネルを使用します。<br>
アクセス パネルで [Amazon Web Service (AWS)] タイルをクリックすると、自動的に Amazon Web Service (AWS) アプリケーションにサインオンします。


## その他のリソース

* [SaaS アプリと Azure Active Directory を統合する方法に関するチュートリアルの一覧](active-directory-saas-tutorial-list.md)
* [Azure Active Directory のアプリケーション アクセスとシングル サインオンとは](active-directory-appssoaccess-whatis.md)

<!--Image references-->
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
























