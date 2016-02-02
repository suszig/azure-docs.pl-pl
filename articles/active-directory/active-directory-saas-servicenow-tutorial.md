<properties 
    pageTitle="チュートリアル: Azure Active Directory と ServiceNow の統合 | Microsoft Azure" 
    description="Azure Active Directory と ServiceNow を使用して、シングル サインオン、自動化されたプロビジョニング、および詳細を有効にする方法を説明します。" 
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


# チュートリアル: Azure Active Directory と ServiceNow の統合

このチュートリアルでは、Azure と ServiceNow の統合について説明します。  
このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   ServiceNow のテナント

このチュートリアルを完了すると、ServiceNow に割り当てた Azure AD ユーザーができるよう、ServiceNow 会社サイト (サービス プロバイダーが開始したサインオン)、または、を使用して、アプリケーションにシングル サインオン [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)

このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  ServiceNow のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-servicenow-tutorial/IC769496.png "Scenario")
## ServiceNow のアプリケーション統合の有効化

このセクションでは、ServiceNow のアプリケーション統合を有効にする方法について説明します。

### ServiceNow のアプリケーション統合を有効にするには、次の手順に従います。

1.  Azure 管理ポータルの左側のナビゲーション ウィンドウで、**[Active Directory]** をクリックします。

    ![Active Directory](./media/active-directory-saas-servicenow-tutorial/IC700993.png "Active Directory")

2.  **[ディレクトリ]** の一覧から、ディレクトリ統合を有効にするディレクトリを選択します。

3.  アプリケーション ビューを開くには、ディレクトリ ビューでトップ メニューの **[アプリケーション]** をクリックします。

    ![アプリケーション](./media/active-directory-saas-servicenow-tutorial/IC700994.png "Applications")

4.  ページの下部にある **[追加]** をクリックします。

    ![アプリケーションの追加](./media/active-directory-saas-servicenow-tutorial/IC749321.png "Add application")

5.  **[実行する内容]** ダイアログで、**[ギャラリーからアプリケーションを追加します]** をクリックします。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-servicenow-tutorial/IC749322.png "Add an application from gallerry")

6.  **検索ボックス**に、「**ServiceNow**」と入力します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-servicenow-tutorial/IC701016.png "Application gallery")

7.  結果ウィンドウで **[ServiceNow]** を選択し、**[完了]** をクリックしてアプリケーションを追加します。

    ![ServiceNow](./media/active-directory-saas-servicenow-tutorial/IC701017.png "ServiceNow")
## シングル サインオンの構成

このセクションでは、SAML プロトコルに基づくフェデレーションを使用して、ユーザーが Azure AD のアカウントで ServiceNow に対する認証を行えるようにする方法を説明します。

この手順の途中で、Base-64 でエンコードされた証明書を Dropbox for Business テナントにアップロードする必要があります。 この手順に慣れていない場合は、次を参照してください。 [バイナリ証明書をテキスト ファイルに変換する方法](http://youtu.be/PlgrzUZ-Y1o)します。

### シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **ServiceNow** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、 ** [シングル サインオン ** ダイアログ。

    ![シングル サインオンの構成](./media/active-directory-saas-servicenow-tutorial/IC749323.png "Configure single sign-on")

2.  **方法を選択して ServiceNow へのサインオン** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、順にクリック **次**します。

    ![シングル サインオンの構成](./media/active-directory-saas-servicenow-tutorial/IC749324.png "Configure single sign-on")

3.  **[アプリケーション設定の構成]** ページで、次の手順を実行します。

    ![アプリケーション URL の構成](./media/active-directory-saas-servicenow-tutorial/IC769497.png "Configure app URL")

    a.、 **ServiceNow サインオン URL** ] ボックスに、型、ユーザーを ServiceNow アプリケーションにサインオンする使用する URL (例:: * https://\<InstanceName\>サービス now.com*)。

    b. **発行者 URL** ] ボックスに、型に使用する URL、ユーザーを ServiceNow アプリケーションにサインオン (例:: * https://\<InstanceName\>サービス now.com*)。

    c. **[次へ]** をクリックします。

4.  **でのシングル サインオンを自動構成する** ] ページで [ **でのシングル サインオン用のアプリケーションを手動で構成**, 、順にクリック **次**します。

    ![アプリケーション URL の構成](./media/active-directory-saas-servicenow-tutorial/IC7694971.png "Configure app URL")

4.  **ServiceNow でのシングル サインオンの構成** ] ページで [ **証明書のダウンロード**, 、コンピューターにローカルで証明書ファイルを保存し、[クリックして **次**します。

    ![シングル サインオンの構成](./media/active-directory-saas-servicenow-tutorial/IC749325.png "Configure single sign-on")

1. ServiceNow アプリケーションに管理者としてサインオンします。

2. 左側のナビゲーション ウィンドウでをクリックして **プロパティ**します。

    ![アプリケーション URL の構成](./media/active-directory-saas-servicenow-tutorial/IC7694980.png "Configure app URL")

3. **[Multiple Provider SSO Properties]** ダイアログで、次の手順を実行します。

    ![アプリケーション URL の構成](./media/active-directory-saas-servicenow-tutorial/IC7694981.png "Configure app URL")

    a. **[Enable multiple provider SSO]** で **[Yes]** を選択します。

    b. **[Enable debug logging got the multiple provider SSO integration]** で **[Yes]** を選択します。

    c. **[The field on the user table that...]** ボックスに「**user_name**」と入力します。

    d. **[保存]** をクリックします。

1. 左側のナビゲーション ウィンドウでをクリックして **の x509 証明書**します。

    ![シングル サインオンの構成](./media/active-directory-saas-servicenow-tutorial/IC7694973.png "Configure single sign-on")

1. **[x509 Certificates]** ダイアログで、**[New]** をクリックします。

    ![シングル サインオンの構成](./media/active-directory-saas-servicenow-tutorial/IC7694974.png "Configure single sign-on")

1. **[X.509 Certificates]** ダイアログで、次の手順を実行します。

    ![シングル サインオンの構成](./media/active-directory-saas-servicenow-tutorial/IC7694975.png "Configure single sign-on")

    a. **[新規]** をクリックします。

    b.  **名** ] ボックスに、構成の名前を入力 (例:: **TestSAML2.0**)。

    c. **[アクティブ]** を選択します。

    d. **[形式]** で **[PEM]** を選択します。

    e. **[Type]** で **[Trust Store Cert]** を選択します。

    f. Base-64 でエンコードされたファイルをダウンロードした証明書から作成します。
    > [AZURE.NOTE] 詳細については、次を参照してください。 [バイナリ証明書をテキスト ファイルに変換する方法](http://youtu.be/PlgrzUZ-Y1o)します。

    g. Base-64 でエンコードされた証明書をメモ帳で開き、という内容がクリップボードにコピーおよびに貼り付け、 **PEM 証明書** ] ボックスに貼り付けます。

    h. **[Update]** を選択します。

1. 左側のナビゲーション ウィンドウでをクリックして **Id プロバイダー**します。

    ![シングル サインオンの構成](./media/active-directory-saas-servicenow-tutorial/IC7694976.png "Configure single sign-on")

1. **[Identity Providers]** で、**[New]** をクリックします。

    ![シングル サインオンの構成](./media/active-directory-saas-servicenow-tutorial/IC7694977.png "Configure single sign-on")

1. **[Identity Providers]** ダイアログで、**[SAML2 Update1?]** をクリックします。

    ![シングル サインオンの構成](./media/active-directory-saas-servicenow-tutorial/IC7694978.png "Configure single sign-on")

1. [SAML2 Update1 Properties] ダイアログで、次の手順を実行します。

    ![シングル サインオンの構成](./media/active-directory-saas-servicenow-tutorial/IC7694982.png "Configure single sign-on")

    a.  **名** ] ボックスに、構成の名前を入力 (例:: **SAML 2.0**)。

    b. **[ユーザー フィールド]** ボックスに「**email**」と入力します。

    c. Azure AD ポータルで、**[Identity Provider ID]** 値をコピーし、**[Identity Provider URL]** ボックスに貼り付けます。

    d. Azure AD ポータルの **[認証要求 URL]** 値をコピーし、**[Identity Provider's AuthnRequest]** ボックスに貼り付けます。

    e. Azure AD ポータルの **[シングル サインオン サービス URL]** 値をコピーし、**[Identity Provider's SingleLogoutRequest]** ボックスに貼り付けます。

    f. **[ServiceNow Homepage]** ボックスに ServiceNow インスタンス ホームページの URL を入力します。
    > [AZURE.NOTE] ServiceNow インスタンス ホームページの連結は、 **ServieNow テナント URL** と **/navpage.do** (例:: *https://fabrikam.service-now.com/navpage.do*)。

    g. **[Entity ID / Issuer]** ボックスに、ServiceNow テナントの URL を入力します。

    h. **[Audience URL]** ボックスに ServiceNow テナントの URL を入力します。

    i.  **IDP の SingleLogoutRequest に割り当てられたバインド プロトコル** ] ボックスに「 **urn: oasis: 名: tc: SAML:2.0:bindings:HTTP-リダイレクト**します。

    j します。 NameID ポリシー] ボックスに入力 **urn oasis: 名: 対象の形式: 未指定**します。

    k です。 **[Create an AuthnContextClass]** をオフにします。

    l です。 **AuthnContextClassRef メソッド**, 、型 **http://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password**します。

    m。 **[Clock Skew]** ボックスに「**60**」と入力します。

    n があります。 **[Single Sign On Script]** で **[MultiSSO_SAML2_Update1]** を選択します。

    です。 **[x509 Certificate]** で、前の手順で作成した証明書を選択します。

    p です。 **[Submit]** をクリックします。

6. Azure AD ポータルで、シングル サインオンの構成確認を選択し、**[次へ]** をクリックします。

    ![シングル サインオンの構成](./media/active-directory-saas-servicenow-tutorial/IC7694990.png "Configure single sign-on")

7. **[シングル サインオンの確認]** ページで **[完了]** をクリックします。

    ![シングル サインオンの構成](./media/active-directory-saas-servicenow-tutorial/IC7694991.png "Configure single sign-on")



## ユーザー プロビジョニングの構成

このセクションでは、Active Directory のユーザー アカウントのプロビジョニングを ServiceNow に対して有効にする方法について説明します。


### ユーザー プロビジョニングを構成するには、次の手順に従います。

1. Azure の管理ポータルでの **ServiceNow** アプリケーション統合ページで、をクリックして **ユーザー プロビジョニングの構成**します。<br><br> ![ユーザー プロビジョニング](./media/active-directory-saas-servicenow-tutorial/IC769498.png "User provisioning")

2. **自動ユーザー プロビジョニングを有効にする ServiceNow 資格情報の入力** ] ページで、次の構成設定を提供します。
ユーザー プロビジョニングの構成

     2.1. **[ServiceNow インスタンス名]** ボックスに、ServiceNow インスタンス名を入力します。

     2.2. **[ServiceNow 管理ユーザー名]** ボックスに、ServiceNow 管理者アカウントの名前を入力します。

     2.3. **[ServiceNow 管理パスワード]** ボックスに、このアカウントのパスワードを入力します。

     2.4. **[検証]** をクリックして構成を確認します。

     2.5. **[次へ]** をクリックして、**[次のステップ]** ページを開きます。

     2.6. このアプリケーションへのすべてのユーザーをプロビジョニングする場合は、選択"**このアプリケーション ディレクトリ内のすべてのユーザー アカウントを自動的にプロビジョニング**"です。<br><br> ![次のステップ](./media/active-directory-saas-servicenow-tutorial/IC698804.png "Next Steps")

     2.7. **[次のステップ]** ページで、**[完了]** をクリックして構成を保存します。











## ユーザーの割り当て

構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

### ユーザーを ServiceNow に割り当てるには、次の手順を実行します。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  **ServiceNow * * アプリケーション統合ページで、をクリックして * * ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-servicenow-tutorial/IC769499.png "Assign users")

3.  テスト ユーザーを選択し、**[割り当て]**、**[はい]** の順にクリックして、割り当てを確定します。

    ![はい](./media/active-directory-saas-servicenow-tutorial/IC767830.png "Yes")

シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。


## その他のリソース

* [Azure Active Directory による SaaS アプリを統合する方法に関するチュートリアルの一覧](active-directory-saas-tutorial-list.md)
* [アプリケーション アクセスと Azure Active Directory でのシングル サインオンとは](active-directory-appssoaccess-whatis.md)




