<properties 
    pageTitle="チュートリアル: Azure Active Directory と Replicon の統合 | Microsoft Azure" 
    description="Azure Active Directory で Replicon を使用して、シングル サインオンや自動プロビジョニングなどを有効にする方法について説明します。" 
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


# チュートリアル: Azure Active Directory と Replicon の統合

このチュートリアルでは、Azure と Replicon の統合について説明します。 このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   Replicon テナント

このチュートリアルを完了すると、Replicon に割り当てた Azure AD ユーザーは、Replicon の会社サイト (サービス プロバイダーが開始したサインオン) で、アプリケーションにシングル サインオンまたはできるを使用して、 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。

このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  Replicon のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-replicon-tutorial/IC777798.png "Scenario")
## Replicon のアプリケーション統合の有効化

このセクションでは、Replicon のアプリケーション統合を有効にする方法について説明します。

### Replicon のアプリケーション統合を有効にするには、次の手順に従います。

1.  Azure 管理ポータルの左側のナビゲーション ウィンドウで、**[Active Directory]** をクリックします。

    ![Active Directory](./media/active-directory-saas-replicon-tutorial/IC700993.png "Active Directory")

2.  **[ディレクトリ]** の一覧から、ディレクトリ統合を有効にするディレクトリを選択します。

3.  アプリケーション ビューを開くには、ディレクトリ ビューでトップ メニューの **[アプリケーション]** をクリックします。

    ![アプリケーション](./media/active-directory-saas-replicon-tutorial/IC700994.png "Applications")

4.  ページの下部にある **[追加]** をクリックします。

    ![アプリケーションの追加](./media/active-directory-saas-replicon-tutorial/IC749321.png "Add application")

5.  **[実行する内容]** ダイアログで、**[ギャラリーからアプリケーションを追加します]** をクリックします。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-replicon-tutorial/IC749322.png "Add an application from gallerry")

6.  **検索ボックス**に、「**Replicon**」と入力します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-replicon-tutorial/IC777799.png "Application gallery")

7.  結果ウィンドウで **[Replicon]** を選び、**[完了]** をクリックしてアプリケーションを追加します。

    ![Replicon](./media/active-directory-saas-replicon-tutorial/IC777800.png "Replicon")
## シングル サインオンの構成

このセクションでは、SAML プロトコルに基づくフェデレーションを使用して、ユーザーが Azure AD のアカウントで Replicon に対する認証を行うことができるようにする方法を説明します。

### シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **Replicon** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、 ** [シングル サインオン ** ダイアログ。

    ![シングル サインオンの構成](./media/active-directory-saas-replicon-tutorial/IC777801.png "Configure single sign-on")

2.  **どのような replicon サインオン** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、順にクリック **次**します。

    ![シングル サインオンの構成](./media/active-directory-saas-replicon-tutorial/IC777802.png "Configure single sign-on")

3.  **アプリケーション URL の構成** ] ページで、次の手順を実行します。

    ![アプリケーション URL の構成](./media/active-directory-saas-replicon-tutorial/IC777803.png "Configure app URL")

    1.  **Replicon サインオン URL** ] ボックスに、Replicon テナント URL を入力 (例:: *https://na2.replicon.com/company/saml2/sp-sso/post*)。
    2.  **Replicon 応答 URL** ] ボックスに、Replicon の入力 **AssertionConsumerService** URL (例:: *https://global.replicon.com/!/saml2、会社、sso/投稿*)。
        >[AZURE.NOTE] Replicon メタデータから URL を取得できます。
        * * https://global.replicon.com/!/saml2/\<YourCompanyKey\>* *。

    3.  **[次へ]** をクリックします。

4.  **Replicon でのシングル サインオンの構成** ] ページで、メタデータをダウンロードする] をクリックして **メタデータのダウンロード**, 、コンピューターにメタデータを保存します。

    ![シングル サインオンの構成](./media/active-directory-saas-replicon-tutorial/IC777804.png "Configure single sign-on")

5.  別の Web ブラウザー ウィンドウで、 Replicon 企業サイトに管理者としてログインします。

6.  SAML 2.0 を構成するには、次の手順に従います。

    ![SAML 認証の有効化](./media/active-directory-saas-replicon-tutorial/IC777805.png "Enable SAML authentication")

    1.  表示する、 **EnableSAML Authentication2** ] ダイアログ ボックスで、会社のキーの後、URL に次を追加します。  
        **/services/SecurityService1.svc/help/test/EnableSAMLAuthentication2**  
        完全な URL のスキーマを次に示します。  
        * * https://na2.replicon.com/\<YourCompanyKey\>/services/SecurityService1.svc/help/test/EnableSAMLAuthentication2**
    2.  **[+]** をクリックして、**[v20Configuration]** セクションを展開します。
    3.  **[+]** をクリックして、**[metaDataConfiguration]** セクションを展開します。
    4.  **[Choose File]** をクリックして、ID プロバイダー メタデータ XML ファイルを選び、**[送信]** をクリックします。

7.  Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![シングル サインオンの構成](./media/active-directory-saas-replicon-tutorial/IC778418.png "Configure single sign-on")
## ユーザー プロビジョニングの構成

Azure AD ユーザーが Replicon にログインできるようにするには、そのユーザーを Replicon にプロビジョニングする必要があります。  
Replicon の場合、プロビジョニングは手動で行います。

### ユーザー プロビジョニングを構成するには、次の手順に従います。

1.  Web ブラウザー ウィンドウで、 Replicon 企業サイトに管理者としてログインします。

2.  移動して **管理 \ > ユーザー**します。

    ![ユーザー](./media/active-directory-saas-replicon-tutorial/IC777806.png "Users")

3.  **[+Add User]** をクリックします。

    ![ユーザーの追加](./media/active-directory-saas-replicon-tutorial/IC777807.png "Add User")

4.  **[User Profile]** セクションで、次の手順に従います。

    ![ユーザー プロファイル](./media/active-directory-saas-replicon-tutorial/IC777808.png "User profile")

    1.  **[Login Name]** ボックスに、プロビジョニングする Azure AD ユーザーの Azure AD メール アドレスを入力します。
    2.  **[Authentication Type]** は **[SSO]** を選びます。
    3.  **[Department]** テキストボックスに、ユーザーの所属を入力します。
    4.  **[Employee Type]** は **[Administrator]** を選びます。
    5.  **[Save User Profile]** をクリックします。

>[AZURE.NOTE]Replicon から提供されている他の Replicon ユーザー アカウント作成ツールまたは API を使用して、AAD ユーザー アカウントをプロビジョニングできます。

## ユーザーを割り当てる

構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

### ユーザーを Replicon に割り当てるには、次の手順に従います。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  **Replicon * * アプリケーション統合ページで、をクリックして * * ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-replicon-tutorial/IC777809.png "Assign users")

3.  テスト ユーザーを選択し、**[割り当て]**、**[はい]** の順にクリックして、割り当てを確定します。

    ![はい](./media/active-directory-saas-replicon-tutorial/IC767830.png "Yes")

シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。




