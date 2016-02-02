<properties 
    pageTitle="チュートリアル: Azure Active Directory と PolicyStat の統合 | Microsoft Azure" 
    description="Azure Active Directory で PolicyStat を使用して、シングル サインオンや自動プロビジョニングなどを有効にする方法について説明します。" 
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


# チュートリアル: Azure Active Directory と PolicyStat の統合

このチュートリアルでは、Azure と PolicyStat の統合について説明します。  
このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   PolicyStat テナント

このチュートリアルを完了すると、PolicyStat に割り当てた Azure AD ユーザーは、PolicyStat 会社サイト (サービス プロバイダーが開始したサインオン) で、アプリケーションにシングル サインオンまたはできるを使用して、 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。

このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  PolicyStat のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-policystat-tutorial/IC808662.png "Scenario")
## PolicyStat のアプリケーション統合の有効化

このセクションでは、PolicyStat のアプリケーション統合を有効にする方法を説明します。

### PolicyStat のアプリケーション統合を有効にするには、次の手順に従います。

1.  Azure 管理ポータルの左側のナビゲーション ウィンドウで、**[Active Directory]** をクリックします。

    ![Active Directory](./media/active-directory-saas-policystat-tutorial/IC700993.png "Active Directory")

2.  **[ディレクトリ]** の一覧から、ディレクトリ統合を有効にするディレクトリを選択します。

3.  アプリケーション ビューを開くには、ディレクトリ ビューでトップ メニューの **[アプリケーション]** をクリックします。

    ![アプリケーション](./media/active-directory-saas-policystat-tutorial/IC700994.png "Applications")

4.  ページの下部にある **[追加]** をクリックします。

    ![アプリケーションの追加](./media/active-directory-saas-policystat-tutorial/IC749321.png "Add application")

5.  **[実行する内容]** ダイアログで、**[ギャラリーからアプリケーションを追加します]** をクリックします。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-policystat-tutorial/IC749322.png "Add an application from gallerry")

6.  **検索ボックス**に、「**PolicyStat**」と入力します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-policystat-tutorial/IC808627.png "Application Gallery")

7.  結果ウィンドウで **[PolicyStat]** を選び、**[完了]** をクリックしてアプリケーションを追加します。

    ![PolicyStat](./media/active-directory-saas-policystat-tutorial/IC810430.png "PolicyStat")
## シングル サインオンの構成

このセクションでは、ユーザーが SAML プロトコルに基づくフェデレーションを使用して、Azure AD でのユーザーのアカウントで PolicyStat に対する認証を行えるようにする方法を説明します。  
PolicyStat アプリケーションは、特定の形式の SAML アサーションを使用するため、カスタム属性のマッピングを **SAML トークンの属性**の構成に追加する必要があります。  
次のスクリーンショットはその例です。

![属性](./media/active-directory-saas-policystat-tutorial/IC808628.png "Attributes")

### シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **PolicyStat** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、 ** [シングル サインオン ** ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-policystat-tutorial/IC808629.png "Configure Single Sign-On")

2.  **どのような PolicyStat にサインオン** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、順にクリック **次**します。

    ![Configure Single Sign-On](./media/active-directory-saas-policystat-tutorial/IC808630.png "Configure Single Sign-On")

3.  **アプリケーション設定の構成** ] ページの [、 **サインオン URL** ユーザーが URL PolicyStat アプリケーションにサインオンする URL を使用] ボックスに、型 (例:: *"https://demo-azure.policystat.com"*)、順にクリック **次**します。

    ![Configure App Settings](./media/active-directory-saas-policystat-tutorial/IC808631.png "Configure App Settings")

4.  **PolicyStat でのシングル サインオンの構成** ] ページで [ **メタデータのダウンロード**, 、お使いのコンピューターにメタデータ ファイルを保存します。

    ![Configure Single Sign-On](./media/active-directory-saas-policystat-tutorial/IC808632.png "Configure Single Sign-On")

5.  別の Web ブラウザーのウィンドウで、PolicyStat 企業サイトに管理者としてログインします。

6.  **[Admin]** タブをクリックしてから、左側のナビゲーション ウィンドウの **[Single Sign-On Configuration]** をクリックします。

    ![管理者メニュー](./media/active-directory-saas-policystat-tutorial/IC808633.png "Administrator Menu")

7.  **[Setup]** セクションで、**[Enable Single Sign-on Integration]** を選びます。

    ![シングル サインオンの構成](./media/active-directory-saas-policystat-tutorial/IC808634.png "Single Sign-On Configuration")

8.  **[Configure Attributes]** をクリックして 、**[Configure Attributes]** セクションで次の手順に従います。

    ![シングル サインオンの構成](./media/active-directory-saas-policystat-tutorial/IC808635.png "Single Sign-On Configuration")

    1.  **[Username Attribute]** テキストボックスに、「**uid**」と入力します。
    2.  **[First Name Attribute]** テキストボックスに、「**firstname**」と入力します。
    3.  **[Last Name Attribute]** テキストボックスに、「**lastname**」と入力します。
    4.  **[Email Attribute]** テキストボックスに、「**emailaddress**」と入力します。
    5.  **[変更を保存]** をクリックします。

9.  **[Your IDP Metadata]** をクリックして、**[Your IDP Metadata]** セクションで次の手順に従います。

    ![シングル サインオンの構成](./media/active-directory-saas-policystat-tutorial/IC808635.png "Single Sign-On Configuration")

    1.  ダウンロードしたメタデータ ファイルの内容をコピーし、**[Your Identity Provider Metadata]** テキストボックスに貼り付けます。
    2.  **[変更を保存]** をクリックします。

10. Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-policystat-tutorial/IC771723.png "Configure Single Sign-On")

11. 12. 上部にあるメニュー [ **属性** を開くには、 **SAML トークン属性** ダイアログ。

    ![属性](./media/active-directory-saas-policystat-tutorial/IC795920.png "Attributes")

13. 必要な属性のマッピングを追加するには、次の手順を実行します。

    ![属性](./media/active-directory-saas-policystat-tutorial/IC804823.png "Attributes")

    1.  **[ユーザー属性の追加]** をクリックします。
    2.  **[属性名]** テキストボックスに、「**uid**」と入力します。
    3.  **[属性値]** テキストボックスで、「**ExtractMailPrefix()**」を選びます。
    4.  **[メール]** 一覧で、「**User.mail**」を選びます。
    5.  **[完了]** をクリックします。
## ユーザー プロビジョニングの構成

Azure AD ユーザーが PolicyStat にログインできるようにするには、ユーザーを PolicyStat にプロビジョニングする必要があります。  
PolicyStat は、ジャストインタイム ユーザー プロビジョニングをサポートしています。 つまり、PolicyStat にユーザーを手動で追加する必要はありません。  
ユーザーはシングル サインオンの最初のログイン時に自動的に追加されます。
>[AZURE.NOTE]PolicyStat から提供されている他の PolicyStat ユーザー アカウント作成ツールまたは API を使用して、AAD ユーザー アカウントをプロビジョニングできます。

## ユーザーを割り当てる

構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

### ユーザーを PolicyStat に割り当てるには、次の手順に従います。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  **PolicyStat * * アプリケーション統合ページで、をクリックして * * ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-policystat-tutorial/IC808636.png "Assign Users")

3.  テスト ユーザーを選択し、**[割り当て]**、**[はい]** の順にクリックして、割り当てを確定します。

    ![はい](./media/active-directory-saas-policystat-tutorial/IC767830.png "Yes")

シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。




