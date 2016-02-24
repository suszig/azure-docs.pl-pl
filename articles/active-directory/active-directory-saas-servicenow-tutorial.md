<properties 
    pageTitle="チュートリアル: Azure Active Directory と ServiceNow の統合 | Microsoft Azure" 
    description="Azure Active Directory で ServiceNow を使用して、シングル サインオンや自動プロビジョニングなどを有効にする方法を説明します。" 
    services="active-directory" 
    authors="MarkusVi"  
    documentationCenter="na" 
    manager="stevenpo"/>
<tags 
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="11/02/2015" 
    ms.author="markvi" />

#チュートリアル: Azure Active Directory と ServiceNow の統合
  
このチュートリアルでは、Azure と ServiceNow の統合について説明します。  
このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   ServiceNow のテナント
  
このチュートリアルを完了すると、ServiceNow に割り当てた Azure AD ユーザーは、ServiceNow 会社サイト (サービス プロバイダーが開始したサインオン) で、アプリケーションにシングル サインオンまたはできるを使用して、 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)
  
このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  ServiceNow のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-servicenow-tutorial/IC769496.png "Scenario")
##ServiceNow のアプリケーション統合の有効化
  
このセクションでは、ServiceNow のアプリケーション統合を有効にする方法について説明します。

###ServiceNow のアプリケーション統合を有効にするには、次の手順に従います。

1.  左側のナビゲーション ウィンドウで、Azure 管理ポータルで、クリックして **Active Directory**します。

    ![Active Directory](./media/active-directory-saas-servicenow-tutorial/IC700993.png "Active Directory")

2.   **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3.  ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。

    ![アプリケーション](./media/active-directory-saas-servicenow-tutorial/IC700994.png "Applications")

4.  クリックして **追加** ページの下部にあります。

    ![アプリケーションの追加](./media/active-directory-saas-servicenow-tutorial/IC749321.png "Add application")

5.   **実行する操作** ダイアログ ボックスで、をクリックして **ギャラリーからアプリケーションを追加**します。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-servicenow-tutorial/IC749322.png "Add an application from gallerry")

6.   **検索ボックス**, 、型 **ServiceNow**します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-servicenow-tutorial/IC701016.png "Application gallery")

7.  結果ウィンドウで [ **ServiceNow**, 、クリックして **完了** アプリケーションを追加します。

    ![ServiceNow](./media/active-directory-saas-servicenow-tutorial/IC701017.png "ServiceNow")
##シングル サインオンの構成
  
このセクションでは、SAML プロトコルに基づくフェデレーションを使用して、ユーザーが Azure AD のアカウントで ServiceNow に対する認証を行えるようにする方法を説明します。

この手順の途中で、base-64 でエンコードされた証明書を Dropbox for Business テナントにアップロードする必要があります。 この手順に慣れていない場合は、次を参照してください。 [バイナリ証明書をテキスト ファイルに変換する方法](http://youtu.be/PlgrzUZ-Y1o)します。

###シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **ServiceNow** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、* * [シングル サインオン * * ダイアログ。

    ![Configure single sign-on](./media/active-directory-saas-servicenow-tutorial/IC749323.png "Configure single sign-on")

2.   **方法を選択して ServiceNow へのサインオン** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、順にクリック **次**します。

    ![Configure single sign-on](./media/active-directory-saas-servicenow-tutorial/IC749324.png "Configure single sign-on")

3.   **アプリケーション設定の構成** ] ページで、次の手順を実行します。

    ![アプリケーション URL の構成](./media/active-directory-saas-servicenow-tutorial/IC769497.png "Configure app URL")

    a.  **ServiceNow サインオン URL** ] ボックスに、型、ユーザーを ServiceNow アプリケーションにサインオンする使用する URL (例:: *https://\<InstanceName\>.service-now.com*)。

    b.  **発行者 URL** ] ボックスに、型に使用する URL、ユーザーを ServiceNow アプリケーションにサインオン (例:: *https://\<InstanceName\>.service-now.com*)。

    c. クリックして **次へ]**

4.   **でのシングル サインオンを自動構成する** ] ページで [ **でのシングル サインオン用のアプリケーションを手動で構成**, 、順にクリック **次**します。

    ![アプリケーション URL の構成](./media/active-directory-saas-servicenow-tutorial/IC7694971.png "Configure app URL")



4.   **ServiceNow でのシングル サインオンの構成** ] ページで [ **証明書のダウンロード**, 、コンピューターにローカルで証明書ファイルを保存し、[クリックして **次**します。

    ![シングル サインオンの構成](./media/active-directory-saas-servicenow-tutorial/IC749325.png "Configure single sign-on")

1. ServiceNow アプリケーションに管理者としてサインオンします。

2. 左側のナビゲーション ウィンドウで **プロパティ**します。  

    ![アプリケーション URL の構成](./media/active-directory-saas-servicenow-tutorial/IC7694980.png "Configure app URL")


3.  **複数プロバイダーの SSO プロパティ** ] ダイアログ ボックスで、次の手順を実行します。

    ![アプリケーション URL の構成](./media/active-directory-saas-servicenow-tutorial/IC7694981.png "Configure app URL")

    a. として **複数プロバイダー SSO を有効にする**, [ **はい**します。

    b. として **デバッグ ログが、複数のプロバイダー SSO の統合を有効にする**, [ **はい**します。

    c.  **ユーザーに対するフィールドをテーブル...** ] ボックスに「 **user_name**します。

    d. クリックして **保存**します。



1. 左側のナビゲーション ウィンドウでをクリックして **の x509 証明書**します。

    ![Configure single sign-on](./media/active-directory-saas-servicenow-tutorial/IC7694973.png "Configure single sign-on")


1.  **X.509 証明書** ダイアログ ボックスで、をクリックして **新規**します。

    ![Configure single sign-on](./media/active-directory-saas-servicenow-tutorial/IC7694974.png "Configure single sign-on")


1.  **X.509 証明書** ] ダイアログ ボックスで、次の手順を実行します。

    ![Configure single sign-on](./media/active-directory-saas-servicenow-tutorial/IC7694975.png "Configure single sign-on")

    a. クリックして **新しい**します。

    b.  **名前** ] ボックスに、構成の名前を入力 (例:: **TestSAML2.0**)。

    c. 選択 **Active**します。

    d. として **形式**, [ **PEM**します。

    e. として **型**, [ **ストアの証明書を信頼する**です。

    f. ダウンロードした証明書から Base-64 でエンコードされたファイルを作成します。
    > [AZURE.NOTE] 詳細については、次を参照してください。 [バイナリ証明書をテキスト ファイルに変換する方法](http://youtu.be/PlgrzUZ-Y1o)します。
    
    g. Base-64 でエンコードされた証明書をメモ帳で開き、という内容がクリップボードにコピーおよびに貼り付け、 **PEM 証明書** ] ボックスに貼り付けます。

    h. クリックして **更新**します。


1. 左側のナビゲーション ウィンドウでをクリックして **Id プロバイダー**します。

    ![Configure single sign-on](./media/active-directory-saas-servicenow-tutorial/IC7694976.png "Configure single sign-on")

1.  **Id プロバイダー** ダイアログ ボックスで、をクリックして **新規**:

    ![シングル サインオンの構成](./media/active-directory-saas-servicenow-tutorial/IC7694977.png "Configure single sign-on")


1.  **Id プロバイダー** ダイアログ ボックスで、をクリックして **SAML2 更新プログラム 1?**:

    ![Configure single sign-on](./media/active-directory-saas-servicenow-tutorial/IC7694978.png "Configure single sign-on")


1. [SAML2 Update1 Properties] ダイアログで、次の手順を実行します。

    ![Configure single sign-on](./media/active-directory-saas-servicenow-tutorial/IC7694982.png "Configure single sign-on")


    a. in the **Name** textbox, type a name for your configuration (e.g.: **SAML 2.0**).

    b. In the **User Field** textbox, type **email**. 

    c. In the Azure AD portal, copy the **Identity Provider ID** value, and then paste it into the **Identity Provider URL** textbox.

    d. In the Azure AD portal, copy the **Authentication Request URL** value, and then paste it into the **Identity Provider's AuthnRequest** textbox.

    e. In the Azure AD portal, copy the **Single Sign-Out Service URL** value, and then paste it into the **Identity Provider's SingleLogoutRequest** textbox.

    f. In the **ServiceNow Homepage** textbox, type the URL of your ServiceNow instance homepage.

    > [AZURE.NOTE] The ServiceNow instance homepage is a concatenation of your **ServieNow tenant URL** and **/navpage.do** (e.g.: *https://fabrikam.service-now.com/navpage.do*).
 

    g. In the **Entity ID / Issuer** textbox, type the URL of your ServiceNow tenant.

    h. In the **Audience URL** textbox, type the URL of your ServiceNow tenant. 

    i. In the **Protocol Binding for the IDP's SingleLogoutRequest** textbox, type **urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect**.

    j. In the NameID Policy textbox, type **urn:oasis:names:tc:SAML:1.1:nameid-format:unspecified**.

    k. Deselect **Create an AuthnContextClass**.

    l. In the **AuthnContextClassRef Method**, type **http://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password**.

    m. In **Clock Skew** textbox, type **60**.

    n. As **Single Sign On Script**, select **MultiSSO_SAML2_Update1**.

    o. As **x509 Certificate**, select the certificate you have created in the previous step.

    p. Click **Submit**. 



6. Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **次**します。 

    ![シングル サインオンの構成](./media/active-directory-saas-servicenow-tutorial/IC7694990.png "Configure single sign-on")

7.  **シングル サインオンによる確認** ] ページで [ **完了**します。
 
    ![Configure single sign-on](./media/active-directory-saas-servicenow-tutorial/IC7694991.png "Configure single sign-on")



##ユーザー プロビジョニングの構成
  
このセクションでは、Active Directory のユーザー アカウントのプロビジョニングを ServiceNow に対して有効にする方法について説明します。


### ユーザー プロビジョニングを構成するには、次の手順に従います。

1. Azure の管理ポータルでの **ServiceNow** アプリケーション統合ページで、をクリックして **ユーザー プロビジョニングの構成**します。 <br><br> ![ユーザー プロビジョニングの構成](./media/active-directory-saas-servicenow-tutorial/IC769498.png "User provisioning")


2.  **自動ユーザー プロビジョニングを有効にする ServiceNow 資格情報の入力** ] ページで、次の構成設定を提供します。
ユーザー プロビジョニングの構成 

     2.1.  **ServiceNow インスタンス名** ] ボックスに、ServiceNow インスタンス名を入力します。

     2.2.  **ServiceNow 管理者ユーザー名** ] ボックスに、ServiceNow 管理者アカウントの名前を入力します。

     2.3.  **ServiceNow 管理者パスワード** ] ボックスに、このアカウントのパスワードを入力します。

     2.4. クリックして **検証** 構成を確認します。

     2.5. をクリックして、 **次** を開く] ボタン、 **次のステップ** ページです。

     2.6. このアプリケーションへのすべてのユーザーをプロビジョニングする場合は、選択"**このアプリケーション ディレクトリ内のすべてのユーザー アカウントを自動的にプロビジョニング**"です。 <br><br> ![次のステップ](./media/active-directory-saas-servicenow-tutorial/IC698804.png "Next Steps")

     2.7.  **次のステップ** ] ページで [ **完了** して構成を保存します。











##ユーザーの割り当て
  
構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

###ユーザーを ServiceNow に割り当てるには、次の手順を実行します。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  * * ServiceNow * * アプリケーション統合ページで、をクリックして **ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-servicenow-tutorial/IC769499.png "Assign users")

3.  テスト ユーザーを選択して [ **割り当てる**, 、順にクリック **はい** して割り当てを確認します。

    ![あり](./media/active-directory-saas-servicenow-tutorial/IC767830.png "Yes")
  
シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。


## その他のリソース

* [SaaS アプリと Azure Active Directory を統合する方法に関するチュートリアルの一覧](active-directory-saas-tutorial-list.md)
* [Azure Active Directory のアプリケーション アクセスとシングル サインオンとは](active-directory-appssoaccess-whatis.md)
