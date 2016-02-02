<properties 
    pageTitle="チュートリアル: Azure Active Directory と ThousandEyes の統合 | Microsoft Azure" 
    description="Azure Active Directory で ThousandEyes を使用してシングル サインオンや自動プロビジョニングなどを有効にする方法について説明します。" 
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


# チュートリアル: Azure Active Directory と ThousandEyes の統合

このチュートリアルでは、Azure Active Directory (Azure AD) と ThousandEyes の間でのシングル サインオンを設定する方法を説明します。

このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   ThousandEyes でのシングル サインオンが有効なサブスクリプション

このチュートリアルを完了すると、ThousandEyes に割り当てた AAD ユーザーは、ThousandEyes 企業サイト (サービス プロバイダーが開始したサインオン) で、または AAD アクセス パネルを使用して、アプリケーションにシングル サインオンできるようになります。

1.  ThousandEyes のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-thousandeyes-tutorial/IC790059.png "Scenario")

## ThousandEyes のアプリケーション統合の有効化

このセクションでは、ThousandEyes のアプリケーション統合を有効にする方法を説明します。

### ThousandEyes のアプリケーション統合を有効にするには、次の手順に従います。

1.  Azure 管理ポータルの左側のナビゲーション ウィンドウで、**[Active Directory]** をクリックします。

    ![Active Directory](./media/active-directory-saas-thousandeyes-tutorial/IC700993.png "Active Directory")

2.  **[ディレクトリ]** の一覧から、ディレクトリ統合を有効にするディレクトリを選択します。

3.  アプリケーション ビューを開くには、ディレクトリ ビューでトップ メニューの **[アプリケーション]** をクリックします。

    ![アプリケーション](./media/active-directory-saas-thousandeyes-tutorial/IC700994.png "Applications")

4.  ページの下部にある **[追加]** をクリックします。

    ![アプリケーションの追加](./media/active-directory-saas-thousandeyes-tutorial/IC749321.png "Add application")

5.  **[実行する内容]** ダイアログで、**[ギャラリーからアプリケーションを追加します]** をクリックします。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-thousandeyes-tutorial/IC749322.png "Add an application from gallerry")

6.  **検索ボックス**に、「**ThousandEyes**」と入力します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-thousandeyes-tutorial/IC790060.png "Application Gallery")

7.  結果ウィンドウで **[ThousandEyes]** を選択し、**[完了]** をクリックしてアプリケーションを追加します。

    ![ThousandEyes](./media/active-directory-saas-thousandeyes-tutorial/IC790061.png "ThousandEyes")

## シングル サインオンの構成

このセクションでは、ユーザーが SAML プロトコルに基づくフェデレーションを使用して、Azure Active Directory でのユーザーのアカウントで ThousandEyes に対する認証を行えるようにする方法を説明します。

### シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **ThousandEyes** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、 ** [シングル サインオン ** ダイアログ。

    ![シングル サインオンの構成](./media/active-directory-saas-thousandeyes-tutorial/IC790062.png "Configure Single SignOn")

2.  **どのようなユーザーを ThousandEyes にサインオンする** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、] をクリックし、 **次**します。

    ![シングル サインオンの構成](./media/active-directory-saas-thousandeyes-tutorial/IC790063.png "Configure Single SignOn")

3.  **アプリケーション URL の構成** ] ページの [、 **ThousandEyes サインイン URL** ] ボックスに、型、ユーザーが使用する URL、ThousandEyes アプリケーションにサインインする (例::"*https://app.thousandeyes.com/login/sso*")、順にクリック **次**します。

    ![アプリケーション URL の構成](./media/active-directory-saas-thousandeyes-tutorial/IC790064.png "Configure App URL")

4.  **ThousandEyes でのシングル サインオンの構成** ] ページで、証明書をダウンロードする] をクリックして **証明書のダウンロード**, 、し、コンピューターに証明書ファイルをローカルに保存します。

    ![シングル サインオンの構成](./media/active-directory-saas-thousandeyes-tutorial/IC790065.png "Configure Single SignOn")

5.  別の Web ブラウザーのウィンドウで、管理者として **ThousandEyes** 企業サイトにサインオンします。

6.  上部のメニューで **[Settings]** をクリックします。

    ![設定](./media/active-directory-saas-thousandeyes-tutorial/IC790066.png "Settings")

7.  **[Account]** をクリックします。

    ![アカウント](./media/active-directory-saas-thousandeyes-tutorial/IC790067.png "Account")

8.  **[Security & Authentication]** タブをクリックします。

    ![セキュリティと認証](./media/active-directory-saas-thousandeyes-tutorial/IC790068.png "Security & Authentication")

9.  **[Setup Single Sign-On]** セクションで、次の手順を実行します。

    ![シングル サインオンの設定](./media/active-directory-saas-thousandeyes-tutorial/IC790069.png "Setup Single Sign-On")

    1.  **[Enable Single Sign-On]** を選択します。
    2.  Microsoft Azure ポータルで、 **ThousandEyes でのシングル サインオンの構成** ] ページで、コピー、 **リモート ログイン URL** 値に設定して、貼り付けます、 **ログイン ページの URL** ] ボックスに貼り付けます。
    3.  Microsoft Azure ポータルで、 **ThousandEyes でのシングル サインオンの構成** ] ページで、コピー、 **リモート ログアウト URL** 値に設定して、貼り付けます、 **ログアウト ページ URL** ] ボックスに貼り付けます。
    4.  Microsoft Azure ポータルでの **ThousandEyes でのシングル サインオンの構成** ] ページで、コピー、 **発行者 URL** 値に設定して、貼り付けます、 **Id プロバイダー発行者** ] ボックスに貼り付けます。
    5.  **[ID プロバイダー証明書]** で **[ファイルの選択]** をクリックし、Microsoft Azure ポータルからダウンロードした証明書をアップロードします。
    6.  **[保存]** をクリックします。

10. Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![シングル サインオンの構成](./media/active-directory-saas-thousandeyes-tutorial/IC790070.png "Configure Single SignOn")

## ユーザー プロビジョニングの構成

Azure AD ユーザーが ThousandEyes にログインできるようにするには、そのユーザーを ThousandEyes にプロビジョニングする必要があります。  
ThousandEyes の場合、プロビジョニングは手動で行います。

### ユーザー アカウントを ThousandEyes にプロビジョニングするには、次の手順に従います。

1.  ThousandEyes 企業サイトに管理者としてログインします。

2.  **[Settings]** をクリックします。

    ![設定](./media/active-directory-saas-thousandeyes-tutorial/IC790066.png "Settings")

3.  [**アカウント**] クリックします。

    ![アカウント](./media/active-directory-saas-thousandeyes-tutorial/IC790067.png "Account")

4.  **[Accounts & Users]** タブをクリックします。

    ![アカウントとユーザー](./media/active-directory-saas-thousandeyes-tutorial/IC790073.png "Accounts & Users")

5.  **[Add Users & Accounts]** セクションで、次の手順を実行します。

    ![ユーザー アカウントの追加](./media/active-directory-saas-thousandeyes-tutorial/IC790074.png "Add User Accounts")

    1.  プロビジョニングする有効な Azure Active Directory アカウントの**[Name]**、**[Email]**、その他の詳細を該当するボックスに入力します。
    2.  **[Add New User to Account]** をクリックします。
        >[AZURE.NOTE] AAD アカウントの所有者にアカウントの確認およびアクティブ化用のリンクを含む電子メールが送信されます。

>[AZURE.NOTE] ThousandEyes から提供されている他の ThousandEyes ユーザー アカウント作成ツールまたは API を使用して、AAD ユーザー アカウントをプロビジョニングできます。

## ユーザーの割り当て

構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

### ユーザーを ThousandEyes に割り当てるには、次の手順を実行します。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  **ThousandEyes** アプリケーション統合ページで、をクリックして **ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-thousandeyes-tutorial/IC790075.png "Assign Users")

3.  テスト ユーザーを選択し、**[割り当て]**、**[はい]** の順にクリックして、割り当てを確定します。

    ![はい](./media/active-directory-saas-thousandeyes-tutorial/IC767830.png "Yes")

シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。





