<properties 
    pageTitle="チュートリアル: Azure Active Directory と Workday の統合 | Microsoft Azure" 
    description="Azure Active Directory で Workday を使用して、シングル サインオンや自動プロビジョニングなどを有効にする方法について説明します。" 
    services="active-directory" 
    authors="markusvi"  
    documentationCenter="na" 
    manager="stevenpo"/>
<tags 
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="10/22/2015" 
    ms.author="markvi" />

#チュートリアル: Azure Active Directory と Workday の統合
  
このチュートリアルでは、Azure と Workday の統合について説明します。 このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   Workday のテナント
  
このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  Workday のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザー プロビジョニングの構成

![シナリオ](./media/active-directory-saas-workday-tutorial/IC782919.png "Scenario")

##Workday のアプリケーション統合の有効化
  
このセクションでは、Workday のアプリケーション統合を有効にする方法について説明します。

###Workday のアプリケーション統合を有効にするには、次の手順を実行します。

1.  左側のナビゲーション ウィンドウで、Azure 管理ポータルで、クリックして **Active Directory**します。

    ![Active Directory](./media/active-directory-saas-workday-tutorial/IC700993.png "Active Directory")

2.   **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3.  ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。

    ![アプリケーション](./media/active-directory-saas-workday-tutorial/IC700994.png "Applications")

4.  開くには、 **アプリケーション ギャラリー**, 、] をクリックして **アプリの追加**, 、クリックして **組織で使用するためのアプリケーションの追加**します。

    ![どの操作を行いますか。](./media/active-directory-saas-workday-tutorial/IC700995.png "What do you want to do?")

5.   **検索ボックス**, 、型 **Workday**します。

    ![Workday](./media/active-directory-saas-workday-tutorial/IC701021.png "Workday")

6.  結果ウィンドウで [ **Workday**, 、クリックして **完了** アプリケーションを追加します。

    ![Workday](./media/active-directory-saas-workday-tutorial/IC701022.png "Workday")

##シングル サインオンの構成
  
このセクションでは、SAML プロトコルに基づくフェデレーションを使用して、ユーザーが Azure AD のアカウントで Workday に対する認証を行うことができるようにする方法を説明します。  
この手順の途中で、Base-64 でエンコードされた証明書を作成する必要があります。  
この手順に慣れていない場合は、次を参照してください。 [バイナリ証明書をテキスト ファイルに変換する方法](http://youtu.be/PlgrzUZ-Y1o)します。

###シングル サインオンを構成するには、次の手順に従います。

1.   **Workday** アプリケーション統合ページでは、をクリックして **でのシングル サインオンを構成する** を開くには、* * [シングル サインオン * *] ダイアログ。

    ![Configure single sign-on](./media/active-directory-saas-workday-tutorial/IC782920.png "Configure single sign-on")

2.   **どのようなユーザーの Workday へ** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、] をクリックし、 **次**します。

    ![シングル サインオンの構成](./media/active-directory-saas-workday-tutorial/IC782921.png "Configure single sign-on")

3.   **アプリケーション URL の構成** ] ページで、次の手順を実行して順にクリックして **次**します。

    ![アプリケーション URL の構成](./media/active-directory-saas-workday-tutorial/IC782957.png "Configure App URL")

    1.   **サインオン URL** Workday へのサインイン URL は、ユーザーが使用] ボックスに、種類 (例:: *https://impl.workday.com/\<tenant\>/login-saml2.htmld*)
    2.   **Workday 応答 URL** ] ボックスに、Workday 応答 url (例:: *https://impl.workday.com/\<tenant\>/login-saml.htmld*)。

        >[AZURE.NOTE] 応答 URL はサブドメインである必要があります (例:: www wd2、wd3、wd3 impl、wd5、wd5 impl)。 
        >ようなものを使用して"*http://www.myworkday.com*"動作が、"*http://myworkday.com*"しません。 
 
4.   **Workday でのシングル サインオンの構成** ] ページで、証明書をダウンロードする] をクリックして **証明書のダウンロード**, 、お使いのコンピューターに証明書ファイルを保存します。

    ![Configure single sign-on](./media/active-directory-saas-workday-tutorial/IC782922.png "Configure single sign-on")

5.  別の Web ブラウザー ウィンドウで、Workday 企業サイトに管理者としてログインします。

6.  移動して **メニュー \ > Workbench**します。

    ![ワークベンチ](./media/active-directory-saas-workday-tutorial/IC782923.png "Workbench")

7.  移動して **アカウント管理**します。

    ![アカウント管理](./media/active-directory-saas-workday-tutorial/IC782924.png "Account Administration")

8.  移動して **Edit Tenant Setup – セキュリティ**します。

    ![テナントのセキュリティの編集](./media/active-directory-saas-workday-tutorial/IC782925.png "Edit Tenant Security")

9.   **リダイレクト Url** セクションで、次の手順に従います。

    ![リダイレクト URL](./media/active-directory-saas-workday-tutorial/IC7829581.png "Redirection URLs")

     9.1. クリックして **行を追加する**です。

     9.2.  **Login Redirect URL** ] テキスト ボックスおよび **Mobile Redirect URL** ] ボックスに、型、 **Workday テナント URL** で入力した、 **アプリケーション URL の構成** Azure ポータルのページです。
    
     9.3. Azure ポータルで、 **Workday でのシングル サインオンの構成** ] ダイアログ ページでコピー、 **シングル サインアウト サービス URL**, に貼り付けると、 **ログアウト リダイレクト URL** ] ボックスに貼り付けます。

     9.4.   **環境** ] ボックスに、環境の名前を入力します。  


       >[AZURE.NOTE] The value of the Environment attribute is tied to the value of the tenant URL:
        >
        >-   If the domain name of the Workday tenant URL starts with impl (e.g.: *https://impl.workday.com/\<tenant\>/login-saml2.htmld*), the **Environment** attribute must be set to Implementation.
        >-   If the domain name starts with something else, you need to contact Workday to get the matching **Environment** value.

10.  **SAML Setup** セクションで、次の手順に従います。

    ![SAML Setup](./media/active-directory-saas-workday-tutorial/IC782926.png "SAML Setup")

     10.1.  選択 **SAML 認証を有効にする**です。

     10.2.  クリックして **行を追加する**です。

11. [SAML ID プロバイダー] セクションで、次の手順に従います。

    ![SAML ID プロバイダー](./media/active-directory-saas-workday-tutorial/IC7829271.png "SAML Identity Providers")

     11.1.  Id プロバイダー名] ボックスで、プロバイダー名を入力します (例:: *SPInitiatedSSO*)。

     11.2.  Azure ポータルで、 **Workday でのシングル サインオンの構成** ] ダイアログ ページでコピー、 **Id プロバイダーの ID** 値に設定して、貼り付けます、 **発行者** ] ボックスに貼り付けます。

     11.3.  選択 **Workday Initialted ログアウトを有効にする**です。

     11.4. Azure ポータルで、 **Workday でのシングル サインオンの構成** ] ダイアログ ページでコピー、 **シングル サインアウト サービス URL** 値に設定して、貼り付けます、 **ログアウト要求の URL** ] ボックスに貼り付けます。


     11.3.  Click **Identity Provider Public Key Certificate**, and then click **Create**. <br><br>
        ![Create](./media/active-directory-saas-workday-tutorial/IC782928.png "Create")

     11.4.  Click **Create x509 Public Key**. <br><br>
        ![Create](./media/active-directory-saas-workday-tutorial/IC782929.png "Create")

     11.5.  In the **View x509 Public Key** section, perform the following steps: <br><br>
        ![View x509 Public Key](./media/active-directory-saas-workday-tutorial/IC782930.png "View x509 Public Key") <br>

      1.  In the **Name** textbox, type a name for your certificate (e.g.: *PPE\_SP*).
      2.  In the **Valid From** textbox, type the valid from attribute value of your certificate.
      3.  In the **Valid To** textbox, type the valid to attribute value of your certificate.
        
           >[AZURE.NOTE] You can get the valid from date and the valid to date from the downloaded certificate by double-clicking it. The dates are listed under the **Details** tab.

      4.  Create a **Base-64 encoded** file from your downloaded certificate.  

        >[AZURE.TIP] For more details, see [How to convert a binary certificate into a text file](http://youtu.be/PlgrzUZ-Y1o)

      5.  Open your base-64 encoded certificate in notepad, and then copy the content of it.
      6.  In the **Certificate** textbox, paste the content of your clipboard.
      7.  Click **OK**.

12.  次の手順に従います。 <br><br>
        ![SSO 構成](./media/active-directory-saas-workday-tutorial/IC7829351111.png "SSO configuration")

     12.1.  有効にする、 **x509 秘密キーの組**します。

     12.2.   **Service Provider ID** ] ボックスに「 **http://www.workday.com**します。

     12.3.  選択 **Enable SP Initiated SAML Authentication**します。

     12.4.  Azure ポータルで、 **Workday でのシングル サインオンの構成** ] ダイアログ ページでコピー、 **シングル サインオン サービス URL** 値に設定して、貼り付けます、 **IdP SSO サービス URL** ] ボックスに貼り付けます。
     
     12.5 select **SP が開始した認証要求 Do Not Deflate**します。

     12.6. として **Authentication Request Signature Method**, [ **SHA256**します。 <br><br>
        ![Authentication Request Signature Method](./media/active-directory-saas-workday-tutorial/IC782932.png "Authentication Request Signature Method") <br><br>
 
     12.7 [ **OK**します。 <br><br>
        ![[OK]](./media/active-directory-saas-workday-tutorial/IC782933.png "OK")

12. Azure AD ポータルでの **Workday でのシングル サインオンの構成** ] ページで [ **次**します。 <br><br>

    ![Configure single sign-on](./media/active-directory-saas-workday-tutorial/IC782934.png "Configure single sign-on")

13.  **シングル サインオンによる確認** ] ページで [ **完了**します。 <br><br>

    ![Configure single sign-on](./media/active-directory-saas-workday-tutorial/IC782935111.png "Configure single sign-on")



##ユーザー プロビジョニングの構成
  
Workday にテスト ユーザーをプロビジョニングするには、Workday のサポート チームに連絡する必要があります。  
Workday のサポート チームにユーザーを作成してもらいます。

##ユーザーの割り当て
  
構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

###ユーザーを Workday に割り当てるには、次の手順を実行します。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  * * Workday * * アプリケーション統合ページで、をクリックして **ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-workday-tutorial/IC782935.png "Assign Users")

3.  テスト ユーザーを選択して [ **割り当てる**, 、順にクリック **はい** して割り当てを確認します。

    ![あり](./media/active-directory-saas-workday-tutorial/IC767830.png "Yes")
  
シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。
