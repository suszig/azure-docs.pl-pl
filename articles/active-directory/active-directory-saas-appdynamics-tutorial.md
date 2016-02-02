<properties 
    pageTitle="チュートリアル: Azure Active Directory と AppDynamics の統合 | Microsoft Azure" 
    description="Azure Active Directory と AppDynamics を使用して、シングル サインオン、自動化されたプロビジョニング、および詳細を有効にする方法を説明します。" 
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


# チュートリアル: Azure Active Directory と AppDynamics の統合

このチュートリアルでは、Azure と AppDynamics の統合について説明します。 このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   AppDynamics でのシングル サインオンが有効なサブスクリプション

このチュートリアルを完了すると、AppDynamics に割り当てた Azure AD ユーザーは、AppDynamics 会社サイト (サービス プロバイダーが開始したサインオン) で、アプリケーションにシングル サインオンまたはできるを使用して、 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。

このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  AppDynamics のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-appdynamics-tutorial/IC790209.png "Scenario")
## AppDynamics のアプリケーション統合の有効化

このセクションでは、AppDynamics のアプリケーション統合を有効にする方法を説明します。

### AppDynamics のアプリケーション統合を有効にするには、次の手順に従います。

1.  Azure 管理ポータルの左側のナビゲーション ウィンドウで、**[Active Directory]** をクリックします。

    ![Active Directory](./media/active-directory-saas-appdynamics-tutorial/IC700993.png "Active Directory")

2.  **[ディレクトリ]** の一覧から、ディレクトリ統合を有効にするディレクトリを選択します。

3.  アプリケーション ビューを開くには、ディレクトリ ビューでトップ メニューの **[アプリケーション]** をクリックします。

    ![アプリケーション](./media/active-directory-saas-appdynamics-tutorial/IC700994.png "Applications")

4.  ページの下部にある **[追加]** をクリックします。

    ![アプリケーションの追加](./media/active-directory-saas-appdynamics-tutorial/IC749321.png "Add application")

5.  **[実行する内容]** ダイアログで、**[ギャラリーからアプリケーションを追加します]** をクリックします。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-appdynamics-tutorial/IC749322.png "Add an application from gallerry")

6.  **[検索]** ボックスに、**「AppDynamics」**と入力します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-appdynamics-tutorial/IC790210.png "Application Gallery")

7.  結果ウィンドウで **[AppDynamics]** を選択し、**[完了]** をクリックしてアプリケーションを追加します。

    ![AppDynamics](./media/active-directory-saas-appdynamics-tutorial/IC790211.png "AppDynamics")
## シングル サインオンの構成

このセクションでは、ユーザーが SAML プロトコルに基づくフェデレーションを使用して、Azure AD でのユーザーのアカウントで AppDynamics に対する認証を行えるようにする方法を説明します。  
この手順の途中で、base-64 でエンコードされた証明書ファイルを作成する必要があります。  
この手順に慣れていない場合は、次を参照してください。 [バイナリ証明書をテキスト ファイルに変換する方法](http://youtu.be/PlgrzUZ-Y1o)します。

### シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **AppDynamics** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、 ** [シングル サインオン ** ダイアログ。

    ![シングル サインオンの構成](./media/active-directory-saas-appdynamics-tutorial/IC790212.png "Configure Single SignOn")

2.  **方法を選択して AppDynamics にサインオン** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、] をクリックし、 **次**します。

    ![シングル サインオンの構成](./media/active-directory-saas-appdynamics-tutorial/IC790213.png "Configure Single SignOn")

3.  **アプリケーション URL の構成** ] ページで、 **AppDynamics サインオン URL** 使用する URL にユーザーが AppDynamics にサインオンする] ボックスに、型 ("*https://companyname.saas.appdynamics.com*")、順にクリック **次**します。

    ![アプリケーション URL の構成](./media/active-directory-saas-appdynamics-tutorial/IC790214.png "Configure App URL")

4.  **AppDynamics でのシングル サインオンの構成** ] ページで、証明書をダウンロードする] をクリックして **証明書のダウンロード**, 、お使いのコンピューターに証明書ファイルを保存します。

    ![シングル サインオンの構成](./media/active-directory-saas-appdynamics-tutorial/IC790215.png "Configure Single SignOn")

5.  別の Web ブラウザーのウィンドウで、管理者として AppDynamics 企業サイトにログインします。

6.  上部にあるツールバーで **[Settings]**、**[Administration]** の順にクリックします。

    ![管理](./media/active-directory-saas-appdynamics-tutorial/IC790216.png "Administration")

7.  **[Authentication Provider]** タブをクリックします。

    ![Authentication Provider](./media/active-directory-saas-appdynamics-tutorial/IC790224.png "Authentication Provider")

8.  **[Authentication Provider]** セクションで、次の手順を実行します。

    ![SAML の構成](./media/active-directory-saas-appdynamics-tutorial/IC790225.png "SAML Configuration")

    1.  **[Authentication Provider]** として、**[SAML]** を選択します。
    2.  Azure AD ポータルの **[AppDynamics でのシングル サインオンの構成]** ページで、**[リモート ログイン URL]** 値をコピーして、**[Login URL]** テキスト ボックスに貼り付けます。
    3.  Azure ポータルで、**[AppDynamics でのシングル サインオンの構成]** ダイアログ ページの **[リモート ログアウト URL]** の値をコピーし、**[Logout URL]** ボックスに貼り付けます。
    4.  ダウンロードした証明書から **base-64 でエンコードされた**ファイルを作成します。
        >[AZURE.TIP] 詳細については、「 [バイナリ証明書をテキスト ファイルに変換する方法](http://youtu.be/PlgrzUZ-Y1o)

    5.  base-64 でエンコードされた証明書をメモ帳で開き、その内容をクリップボードにコピーして、**[証明書]** テキストボックスに貼り付けます。
    6.  **[保存]** をクリックします。
        ![保存](./media/active-directory-saas-appdynamics-tutorial/IC777673.png "Save")

9.  Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![シングル サインオンの構成](./media/active-directory-saas-appdynamics-tutorial/IC790226.png "Configure Single SignOn")
## ユーザー プロビジョニングの構成

Azure AD ユーザーが AppDynamics にログインできるようにするには、そのユーザーを AppDynamics にプロビジョニングする必要があります。  
AppDynamics の場合、プロビジョニングは手動で行います。

### ユーザー プロビジョニングを構成するには、次の手順に従います。

1.  AppDynamics 企業サイトに管理者としてログインします。

2.  **[Users]** に移動して、**[+]** をクリックして **[Create User]** ダイアログを開きます。

    ![ユーザー](./media/active-directory-saas-appdynamics-tutorial/IC790229.png "Users")

3.  **[Create User]** セクションで、次の手順に従います。

    ![Create User](./media/active-directory-saas-appdynamics-tutorial/IC790230.png "Create User")

    1.  関連するテキスト ボックスに、プロビジョニングする有効な AAD アカウントの **[Username]**、**[Name]**、**[Email]**、**[New Password]**、**[Repeat New Password]** を入力します。
    2.  **[保存]** をクリックします。

>[AZURE.NOTE] 他の AppDynamics ユーザー アカウント作成ツールまたは AppDynamics から提供されている API を使用して、Azure AD ユーザー アカウントをプロビジョニングできます。

## ユーザーの割り当て

構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

### ユーザーを AppDynamics に割り当てるには、次の手順に従います。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  **AppDynamics * * アプリケーション統合ページで、をクリックして * * ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-appdynamics-tutorial/IC790231.png "Assign Users")

3.  テスト ユーザーを選択し、**[割り当て]**、**[はい]** の順にクリックして、割り当てを確定します。

    ![はい](./media/active-directory-saas-appdynamics-tutorial/IC767830.png "Yes")

シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。





