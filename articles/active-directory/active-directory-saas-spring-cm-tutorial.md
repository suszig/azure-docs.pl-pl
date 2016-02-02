<properties 
    pageTitle="チュートリアル: Azure Active Directory と SpringCM の統合 | Microsoft Azure" 
    description="Azure Active Directory で SpringCM を使用して、シングル サインオンや自動プロビジョニングなどを有効にする方法について説明します。" 
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


# チュートリアル: Azure Active Directory と SpringCM の統合

このチュートリアルでは、Azure Active Directory と SpringCM の間でのシングル サインオンを設定する方法を説明します。

このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   SpringCM でのシングル サインオンが有効なサブスクリプション

このチュートリアルを完了すると、SpringCM に割り当てられている Azure Active Directory ユーザーは、AAD アクセス パネルを使用してシングル サインオンできます。

1.  SpringCM のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-spring-cm-tutorial/IC797044.png "Scenario")

## SpringCM のアプリケーション統合の有効化

このセクションでは、SpringCM のアプリケーション統合を有効にする方法について説明します。

### SpringCM のアプリケーション統合を有効にするには、次の手順に従います。

1.  Azure 管理ポータルの左側のナビゲーション ウィンドウで、**[Active Directory]** をクリックします。

    ![Active Directory](./media/active-directory-saas-spring-cm-tutorial/IC700993.png "Active Directory")

2.  **[ディレクトリ]** の一覧から、ディレクトリ統合を有効にするディレクトリを選択します。

3.  アプリケーション ビューを開くには、ディレクトリ ビューでトップ メニューの **[アプリケーション]** をクリックします。

    ![アプリケーション](./media/active-directory-saas-spring-cm-tutorial/IC700994.png "Applications")

4.  ページの下部にある **[追加]** をクリックします。

    ![アプリケーションの追加](./media/active-directory-saas-spring-cm-tutorial/IC749321.png "Add application")

5.  **[実行する内容]** ダイアログで、**[ギャラリーからアプリケーションを追加します]** をクリックします。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-spring-cm-tutorial/IC749322.png "Add an application from gallerry")

6.  **検索ボックス**に、「**SpringCM**」と入力します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-spring-cm-tutorial/IC797045.png "Application Gallery")

7.  結果ウィンドウで **[SpringCM]** を選択し、**[完了]** をクリックしてアプリケーションを追加します。

    ![SpringCM](./media/active-directory-saas-spring-cm-tutorial/IC797046.png "SpringCM")

## シングル サインオンの構成

このセクションでは、SAML プロトコルに基づくフェデレーションを使用して、SpringCM で Azure AD のユーザー アカウントを使用してユーザーを認証できるようにする方法を説明します。

### シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **SpringCM** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、 ** [シングル サインオン ** ダイアログ。

    ![シングル サインオンの構成](./media/active-directory-saas-spring-cm-tutorial/IC797047.png "Configure single Sign-On")

2.  **どのようなユーザーの SpringCM へのアクセス** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、] をクリックし、 **次**します。

    ![シングル サインオンの構成](./media/active-directory-saas-spring-cm-tutorial/IC797048.png "Configure single Sign-On")

3.  **アプリケーション URL の構成** ] ページで、 **SpringCM サインオン URL** 、SpringCM のアプリケーションにサインオンし、URL は、ユーザーが使用] ボックスに、型 **次**します。

    アプリケーション URL は、SpringCM テナントの URL (例:: *https://na11.springcm.com/atlas/SSO/SSOEndpoint.ashx?aid=16826*)。

    ![アプリケーション URL の構成](./media/active-directory-saas-spring-cm-tutorial/IC797049.png "Configure App URL")

4.  **SpringCM でのシングル サインオンの構成** ] ページで、証明書をダウンロードする] をクリックして **証明書のダウンロード**, 、し、コンピューターに証明書ファイルをローカルに保存します。

    ![シングル サインオンの構成](./media/active-directory-saas-spring-cm-tutorial/IC797050.png "Configure Single SignOn")

5.  別の Web ブラウザーのウィンドウで、管理者として **SpringCM** 企業サイトにサインオンします。

6.  上部にあるメニューの **[GO TO]** をクリックし、**[Preferences]** をクリックします。次に、**[Account Preferences]** セクションで、**[SAML SSO]** をクリックします。

    ![SAML SSO](./media/active-directory-saas-spring-cm-tutorial/IC797051.png "SAML SSO")

7.  [Identity Provider Configuration] セクションで、次の手順に従います。

    ![ID プロバイダー構成](./media/active-directory-saas-spring-cm-tutorial/IC797052.png "Identity Provider Configuration")

    1.  ダウンロードした Azure Active Directory 証明書をアップロードするために、**[Select Issuer Certificate]** または **[Change Issuer Certificate]** をクリックします。
    2.  Microsoft Azure ポータルで、 **SpringCM でのシングル サインオンの構成** ] ページで、コピー、 **発行者 URL** 値に設定して、貼り付けます、 **発行者** ] ボックスに貼り付けます。
    3.  Microsoft Azure ポータルで、 **SpringCM でのシングル サインオンの構成** ] ページで、コピー、 **シングル サインオン サービス URL** 値に設定して、貼り付けます、 **サービス プロバイダー (SP) が開始したエンドポイント** ] ボックスに貼り付けます。
    4.  **[SAML Enabled]** として、**[Enable]** を選択します。
    5.  **[保存]** をクリックします。

8.  Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![シングル サインオンの構成](./media/active-directory-saas-spring-cm-tutorial/IC797053.png "Configure Single SignOn")

## ユーザー プロビジョニングの構成

Azure Active Directory ユーザーが SpringCM にログインできるようにするには、そのユーザーを SpringCM にプロビジョニングする必要があります。  
SpringCM の場合、プロビジョニングは手動で行います。
>[AZURE.NOTE] 詳細については、「 [を作成し、、ユーザーを SpringCM 編集](http://knowledge.springcm.com/create-and-edit-a-springcm-user)

### ユーザー アカウントを SpringCM にプロビジョニングするには、次の手順に従います。

1.  **SpringCM** 企業サイトに管理者としてログインします。

2.  **[GOTO]**、**[Address Book]** の順にクリックします。

    ![Create User](./media/active-directory-saas-spring-cm-tutorial/IC797054.png "Create User")

3.  **[Create User]** をクリックします。

4.  **[User Role]** を選択します。

5.  **[Send Activation Email]** を選択します。

6.  関連するテキスト ボックスに、プロビジョニングする有効な Azure Active Directory アカウントの姓名と電子メール アドレスを入力します。

7.  ユーザーを **[Security group]** に追加します。

8.  **[保存]** をクリックします。

>[AZURE.NOTE] SpringCM から提供されている他の SpringCM ユーザー アカウント作成ツールまたは API を使用して、AAD ユーザー アカウントをプロビジョニングできます。

## ユーザーの割り当て

構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

### ユーザーを SpringCM に割り当てるには、次の手順に従います。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  **SpringCM** アプリケーション統合ページで、をクリックして **ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-spring-cm-tutorial/IC797055.png "Assign Users")

3.  テスト ユーザーを選択し、**[割り当て]**、**[はい]** の順にクリックして、割り当てを確定します。

    ![はい](./media/active-directory-saas-spring-cm-tutorial/IC767830.png "Yes")

シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。









