<properties 
    pageTitle="チュートリアル: Azure Active Directory と New Relic の統合 | Microsoft Azure" 
    description="Azure Active Directory でシングル サインオン、自動化されたプロビジョニング、および詳細を有効にする New Relic を使用する方法を説明します。" 
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


# チュートリアル: Azure Active Directory と New Relic の統合

このチュートリアルでは、Azure Active Directory と New Relic の間でのシングル サインオンを設定する方法を説明します。

このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   New Relic でのシングル サインオンが有効なサブスクリプション

このチュートリアルを完了すると、New Relic に割り当てられている Azure Active Directory ユーザーは、AAD アクセス パネルを使用してシングル サインオンが可能になります。

1.  New Relic のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-new-relic-tutorial/IC797030.png "Scenario")
## New Relic のアプリケーション統合の有効化

このセクションでは、New Relic のアプリケーション統合を有効にする方法を説明します。

### New Relic のアプリケーション統合を有効にするには、次の手順に従います。

1.  Azure 管理ポータルの左側のナビゲーション ウィンドウで、**[Active Directory]** をクリックします。

    ![Active Directory](./media/active-directory-saas-new-relic-tutorial/IC700993.png "Active Directory")

2.  **[ディレクトリ]** の一覧から、ディレクトリ統合を有効にするディレクトリを選択します。

3.  アプリケーション ビューを開くには、ディレクトリ ビューでトップ メニューの **[アプリケーション]** をクリックします。

    ![アプリケーション](./media/active-directory-saas-new-relic-tutorial/IC700994.png "Applications")

4.  ページの下部にある **[追加]** をクリックします。

    ![アプリケーションの追加](./media/active-directory-saas-new-relic-tutorial/IC749321.png "Add application")

5.  **[実行する内容]** ダイアログで、**[ギャラリーからアプリケーションを追加します]** をクリックします。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-new-relic-tutorial/IC749322.png "Add an application from gallerry")

6.  **検索ボックス**に、「**New Relic**」と入力します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-new-relic-tutorial/IC797031.png "Application Gallery")

7.  結果ウィンドウで **[New Relic]** を選び、**[完了]** をクリックしてアプリケーションを追加します。

    ![New Relic](./media/active-directory-saas-new-relic-tutorial/IC797032.png "New Relic")
## シングル サインオンの構成

このセクションでは、ユーザーが SAML プロトコルに基づくフェデレーションを使用して、Azure Active Directory でのユーザーのアカウントで New Relic に対する認証を行えるようにする方法を説明します。

### シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **New Relic** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、 ** [シングル サインオン ** ダイアログ。

    ![シングル サインオンの構成](./media/active-directory-saas-new-relic-tutorial/IC769534.png "Configure single sign-on")

2.  **どのようなユーザーが New Relic へのサインオン** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、] をクリックし、 **次**します。

    ![Configure Single Sign-On](./media/active-directory-saas-new-relic-tutorial/IC797033.png "Configure Single Sign-On")

3.  **アプリケーション URL の構成** ] ページで、 **新しい Relic サインオン URL** URL は、New Relic のアプリケーションにサインオンし、ユーザーが使用] ボックスに、型 **次**します。

    アプリケーション URL は、New Relic テナントの URL (例:: *https://rpm.newrelic.com*)。

    ![アプリケーション URL の構成](./media/active-directory-saas-new-relic-tutorial/IC797034.png "Configure App URL")

4.  **New Relic でのシングル サインオンの構成** ] ページで、証明書をダウンロードする] をクリックして **証明書のダウンロード**, 、し、コンピューターに証明書ファイルをローカルに保存します。

    ![Configure Single Sign-On](./media/active-directory-saas-new-relic-tutorial/IC797035.png "Configure Single Sign-On")

5.  別の Web ブラウザーのウィンドウで、管理者として **New Relic** 企業サイトにサインオンします。

6.  上部のメニューで **[アカウント設定]** をクリックします。

    ![アカウント設定](./media/active-directory-saas-new-relic-tutorial/IC797036.png "Account Settings")

7.  **[セキュリティと認証]** タブをクリックし、**[シングル サインオン]** タブをクリックします。

    ![シングル サインオン](./media/active-directory-saas-new-relic-tutorial/IC797037.png "Single Sign-On")

8.  SAML ダイアログ ページで、次の手順に従います。

    ![SAML](./media/active-directory-saas-new-relic-tutorial/IC797038.png "SAML")

    1.  **[ファイルの選択]** をクリックして、ダウンロードした Azure Active Directory 証明書をアップロードします。
    2.  Microsoft Azure ポータルで、 **New Relic でのシングル サインオンの構成** ] ページで、コピー、 **リモート ログイン URL** 値に設定して、貼り付けます、 **リモート ログイン URL** ] ボックスに貼り付けます。
    3.  Microsoft Azure ポータルで、 **New Relic でのシングル サインオンの構成** ] ページで、コピー、 **リモート ログアウト URL** 値に設定して、貼り付けます、 **ログアウト ランディング URL** ] ボックスに貼り付けます。
    4.  **[変更を保存する]** をクリックします。

9.  Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-new-relic-tutorial/IC797039.png "Configure Single Sign-On")
## ユーザー プロビジョニングの構成

Azure Active Directory ユーザーが New Relic にログインできるようにするには、そのユーザーを New Relic にプロビジョニングする必要があります。  
New Relic の場合、プロビジョニングは手動で行います。

### ユーザー アカウントを New Relic にプロビジョニングするには、次の手順に従います。

1.  **New Relic** 企業サイトに管理者としてログインします。

2.  上部のメニューで **[アカウント設定]** をクリックします。

    ![アカウント設定](./media/active-directory-saas-new-relic-tutorial/IC797040.png "Account Settings")

3.  **[アカウント]** ペインの左側にある **[概要]** をクリックし、次に **[ユーザーの追加]** をクリックします。

    ![アカウント設定](./media/active-directory-saas-new-relic-tutorial/IC797041.png "Account Settings")

4.  **[アクティブ ユーザー]** ダイアログで、次の手順に従います。

    ![アクティブ ユーザー](./media/active-directory-saas-new-relic-tutorial/IC797042.png "Active Users")

    1.  **[電子メール]** テキストボックスに、プロビジョニングする有効な Azure Active Directory ユーザーの電子メール アドレスを入力します。
    2.  **[ロール]** として **[ユーザー]** を選びます。
    3.  **[このユーザーを追加]** をクリックします。

>[AZURE.NOTE]他の New Relic ユーザー アカウントの作成ツールまたは New Relic から提供されている API を使用して、AAD ユーザー アカウントをプロビジョニングできます。

## ユーザーを割り当てる

構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

### ユーザーを New Relic に割り当てるには、次の手順に従います。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  **New Relic** アプリケーション統合ページで、をクリックして **ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-new-relic-tutorial/IC797043.png "Assign Users")

3.  テスト ユーザーを選択し、**[割り当て]**、**[はい]** の順にクリックして、割り当てを確定します。

    ![はい](./media/active-directory-saas-new-relic-tutorial/IC767830.png "Yes")

シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。









