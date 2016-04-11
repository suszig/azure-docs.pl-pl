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

#チュートリアル: Azure Active Directory と ThousandEyes の統合
  
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

##ThousandEyes のアプリケーション統合の有効化
  
このセクションでは、ThousandEyes のアプリケーション統合を有効にする方法を説明します。

###ThousandEyes のアプリケーション統合を有効にするには、次の手順に従います。

1.  左側のナビゲーション ウィンドウで、Azure 管理ポータルで、クリックして **Active Directory**します。

    ![Active Directory](./media/active-directory-saas-thousandeyes-tutorial/IC700993.png "Active Directory")

2.   **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3.  ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。

    ![アプリケーション](./media/active-directory-saas-thousandeyes-tutorial/IC700994.png "Applications")

4.  クリックして **追加** ページの下部にあります。

    ![アプリケーションの追加](./media/active-directory-saas-thousandeyes-tutorial/IC749321.png "Add application")

5.   **実行する操作** ダイアログ ボックスで、をクリックして **ギャラリーからアプリケーションを追加**します。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-thousandeyes-tutorial/IC749322.png "Add an application from gallerry")

6.   **検索ボックス**, 、型 **ThousandEyes**します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-thousandeyes-tutorial/IC790060.png "Application Gallery")

7.  結果ウィンドウで [ **ThousandEyes**, 、クリックして **完了** アプリケーションを追加します。

    ![ThousandEyes](./media/active-directory-saas-thousandeyes-tutorial/IC790061.png "ThousandEyes")

##シングル サインオンの構成
  
このセクションでは、ユーザーが SAML プロトコルに基づくフェデレーションを使用して、Azure Active Directory でのユーザーのアカウントで ThousandEyes に対する認証を行えるようにする方法を説明します。

###シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **ThousandEyes** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、* * [シングル サインオン * * ダイアログ。

    ![シングル サインオンの構成](./media/active-directory-saas-thousandeyes-tutorial/IC790062.png "Configure Single SignOn")

2.   **どのようなユーザーを ThousandEyes にサインオンする** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、] をクリックし、 **次**します。

    ![シングル サインオンの構成](./media/active-directory-saas-thousandeyes-tutorial/IC790063.png "Configure Single SignOn")

3.   **アプリケーション URL の構成** ] ページの [、 **ThousandEyes サインイン URL** ] ボックスに、型、ユーザーが使用する URL、ThousandEyes アプリケーションにサインインする (例::"*https://app.thousandeyes.com/login/sso*")、順にクリック **次**します。 

    ![Configure App URL](./media/active-directory-saas-thousandeyes-tutorial/IC790064.png "Configure App URL")

4.   **ThousandEyes でのシングル サインオンの構成** ] ページで、証明書をダウンロードする] をクリックして **証明書のダウンロード**, 、し、コンピューターに証明書ファイルをローカルに保存します。

    ![シングル サインオンの構成](./media/active-directory-saas-thousandeyes-tutorial/IC790065.png "Configure Single SignOn")

5.  サインインする別の web ブラウザーのウィンドウで、 **ThousandEyes** 会社サイトに管理者として。

6.  上部にあるメニュー [ **設定**します。

    ![[設定]](./media/active-directory-saas-thousandeyes-tutorial/IC790066.png "Settings")

7.  クリックして **アカウント**

    ![アカウント](./media/active-directory-saas-thousandeyes-tutorial/IC790067.png "Account")

8.  クリックして、 **セキュリティと認証** ] タブをクリックします。

    ![セキュリティと認証](./media/active-directory-saas-thousandeyes-tutorial/IC790068.png "Security & Authentication")

9.   **セットアップでのシングル サインオン** セクションで、次の手順に従います。

    ![シングル サインオンの設定](./media/active-directory-saas-thousandeyes-tutorial/IC790069.png "Setup Single Sign-On")

    1.  選択 **でのシングル サインオンを有効にする**です。
    2.  Microsoft Azure ポータルで、 **ThousandEyes でのシングル サインオンの構成** ] ページで、コピー、 **リモート ログイン URL** 値に設定して、貼り付けます、 **ログイン ページの URL** ] ボックスに貼り付けます。
    3.  Microsoft Azure ポータルで、 **ThousandEyes でのシングル サインオンの構成** ] ページで、コピー、 **リモート ログアウト URL** 値に設定して、貼り付けます、 **ログアウト ページ URL** ] ボックスに貼り付けます。
    4.  Microsoft Azure ポータルでの **ThousandEyes でのシングル サインオンの構成** ] ページで、コピー、 **発行者 URL** 値に設定して、貼り付けます、 **Id プロバイダー発行者** ] ボックスに貼り付けます。
    5.   **Identity Provider Certificate**, 、] をクリックして **ファイルの選択**, 、および Microsoft Azure ポータルからダウンロードした証明書をアップロードします。
    6.  クリックして **保存**します。

10. Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![シングル サインオンの構成](./media/active-directory-saas-thousandeyes-tutorial/IC790070.png "Configure Single SignOn")

##ユーザー プロビジョニングの構成
  
Azure AD ユーザーが ThousandEyes にログインできるようにするには、そのユーザーを ThousandEyes にプロビジョニングする必要があります。  
ThousandEyes の場合、プロビジョニングは手動で行います。

###ユーザー アカウントを ThousandEyes にプロビジョニングするには、次の手順に従います。

1.  ThousandEyes 企業サイトに管理者としてログインします。

2.  クリックして **設定**します。

    ![[設定]](./media/active-directory-saas-thousandeyes-tutorial/IC790066.png "Settings")

3.  クリックして **アカウント**します。

    ![アカウント](./media/active-directory-saas-thousandeyes-tutorial/IC790067.png "Account")

4.  クリックして、 **アカウントとユーザー** ] タブをクリックします。

    ![アカウントとユーザー](./media/active-directory-saas-thousandeyes-tutorial/IC790073.png "Accounts & Users")

5.   **ユーザーの追加およびアカウント** セクションで、次の手順に従います。

    ![ユーザー アカウントの追加](./media/active-directory-saas-thousandeyes-tutorial/IC790074.png "Add User Accounts")

    1.  型、 **名前**, 、**電子メール** など、関連するテキスト ボックスに、プロビジョニングする有効な Azure Active Directory アカウントの情報です。
    2.  クリックして **新しいユーザー アカウントを追加**します。

        >[AZURE.NOTE] AAD アカウントの所有者を確認し、アカウントをアクティブ化リンクが記載された電子メールが表示されます。

>[AZURE.NOTE] 他の ThousandEyes ユーザー アカウント作成ツールを使用することができますか、AAD ユーザー アカウントをプロビジョニング、ThousandEyes から提供されている Api です。

##ユーザーの割り当て
  
構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

###ユーザーを ThousandEyes に割り当てるには、次の手順を実行します。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.   **ThousandEyes** アプリケーション統合ページで、をクリックして **ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-thousandeyes-tutorial/IC790075.png "Assign Users")

3.  テスト ユーザーを選択して [ **割り当てる**, 、順にクリック **はい** して割り当てを確認します。

    ![あり](./media/active-directory-saas-thousandeyes-tutorial/IC767830.png "Yes")
  
シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。


