<properties 
    pageTitle="チュートリアル: Azure Active Directory と LogicMonitor の統合 | Microsoft Azure" 
    description="Azure Active Directory と LogicMonitor を使用して、シングル サインオン、自動化されたプロビジョニング、および詳細を有効にする方法を説明します。" 
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


# チュートリアル: Azure Active Directory と LogicMonitor の統合

このチュートリアルの目的は、Azure と LogicMonitor の統合を示すことです。  
このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   LogicMonitor テナント

このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  LogicMonitor のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-logicmonitor-tutorial/IC790045.png "Scenario")
## LogicMonitor のアプリケーション統合の有効化

このセクションでは、LogicMonitor のアプリケーション統合を有効にする方法を説明します。

### LogicMonitor のアプリケーション統合を有効にするには、次の手順に従います。

1.  Azure 管理ポータルの左側のナビゲーション ウィンドウで、**[Active Directory]** をクリックします。

    ![Active Directory](./media/active-directory-saas-logicmonitor-tutorial/IC700993.png "Active Directory")

2.  **[ディレクトリ]** の一覧から、ディレクトリ統合を有効にするディレクトリを選択します。

3.  アプリケーション ビューを開くには、ディレクトリ ビューでトップ メニューの **[アプリケーション]** をクリックします。

    ![アプリケーション](./media/active-directory-saas-logicmonitor-tutorial/IC700994.png "Applications")

4.  ページの下部にある **[追加]** をクリックします。

    ![アプリケーションの追加](./media/active-directory-saas-logicmonitor-tutorial/IC749321.png "Add application")

5.  **[実行する内容]** ダイアログで、**[ギャラリーからアプリケーションを追加します]** をクリックします。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-logicmonitor-tutorial/IC749322.png "Add an application from gallerry")

6.  **検索ボックス**に「**logicMonitor**」と入力します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-logicmonitor-tutorial/IC790046.png "Application Gallery")

7.  結果ウィンドウで **[LogicMonitor]** を選択し、**[完了]** をクリックしてアプリケーションを追加します。

    ![LogicMonitor](./media/active-directory-saas-logicmonitor-tutorial/IC790047.png "LogicMonitor")
## シングル サインオンの構成

このセクションでは、ユーザーが SAML プロトコルに基づくフェデレーションを使用して、Azure AD でのユーザーのアカウントで LogicMonitor に対する認証を行えるようにする方法を説明します。

### シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **LogicMonitor * * アプリケーション統合ページで、] をクリックして * * でのシングル サインオンを構成する** を開くには、 ** [シングル サインオン ** ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-logicmonitor-tutorial/IC790048.png "Configure Single Sign-On")

2.  **どのようなユーザーの LogicMonitor へのアクセス** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、] をクリックし、 **次**します。

    ![Configure Single Sign-On](./media/active-directory-saas-logicmonitor-tutorial/IC790049.png "Configure Single Sign-On")

3.  **アプリケーション URL の構成** ] ページで、 **サインオン URL** ] ボックスに、型に使用する URL、ユーザーが LogicMonitor への \ (e、g、:"*http://company.logicmonitor.com*"\)、順にクリック **次**します。

    ![アプリケーション URL の構成](./media/active-directory-saas-logicmonitor-tutorial/IC790050.png "Configure App URL")

4.  **LogicMonitor でのシングル サインオンの構成** ] ページで [ **メタデータのダウンロード**, 、お使いのコンピューターに保存します。

    ![Configure Single Sign-On](./media/active-directory-saas-logicmonitor-tutorial/IC790051.png "Configure Single Sign-On")

5.  **LogicMonitor** の企業サイトに管理者としてログインします。

6.  上部のメニューで **[Settings]** をクリックします。

    ![設定](./media/active-directory-saas-logicmonitor-tutorial/IC790052.png "Settings")

7.  左側にあるナビゲーション バーで、**[シングル サインオン]** をクリックします。

    ![シングル サインオン](./media/active-directory-saas-logicmonitor-tutorial/IC790053.png "Single Sign-On")

8.  **[シングル サインオンの設定]** セクションで、次の手順に従います。

    ![シングル サインオンの設定](./media/active-directory-saas-logicmonitor-tutorial/IC790054.png "Single Sign-On Settings")

    1.  **[シングル サインオンを有効にする]** を選択します。
    2.  **[Default Role Assignment (既定のロールの割り当て]** で、**[読み取り専用]** を選択します。
    3.  ダウンロードしたメタデータ ファイルをメモ帳で開き、ファイルの内容を **[Identity Provider Metadata (ID プロバイダーのメタデータ)]** テキストボックスに貼り付けます。
    4.  **[変更を保存]** をクリックします。

9.  Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-logicmonitor-tutorial/IC790055.png "Configure Single Sign-On")
## ユーザー プロビジョニングの構成

AAD ユーザーがサインインできるように、Azure Active Directory ユーザー名を使用して、LogicMonitor アプリケーションにユーザーをプロビジョニングする必要があります。

### ユーザー プロビジョニングを構成するには、次の手順に従います。

1.  LogicMonitor の企業サイトに管理者としてログインします。

2.  上部のメニューで、**[設定]**、**[Roles and Users (ロールとユーザー)]** の順にクリックします。

    ![ロールとユーザー](./media/active-directory-saas-logicmonitor-tutorial/IC790056.png "Roles and Users")

3.  **[追加]** をクリックします。

4.  **[アカウントの追加]** セクションで、次の手順に従います。

    ![アカウントの追加](./media/active-directory-saas-logicmonitor-tutorial/IC790057.png "Add an account")

    1.  関連するテキストボックスにプロビジョニングする Azure Active Directory ユーザーの **[ユーザー名]**、**[電子メール]**、**[パスワード]**、**[パスワードの再入力]** の値を入力します。
    2.  **[ロール]**、**[アクセス許可の表示]**、**[状態]** の順に選択します。
    3.  **[Submit]** をクリックします。

>[AZURE.NOTE]LogicMonitor から提供されている他の LogicMonitor ユーザー アカウント作成ツールや API を使用して、Azure Active Directory ユーザー アカウントをプロビジョニングできます。

## ユーザーを割り当てる

構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

### ユーザーを LogicMonitor に割り当てるには、次の手順に従います。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  **LogicMonitor** アプリケーション統合ページで、をクリックして **ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-logicmonitor-tutorial/IC790058.png "Assign Users")

3.  テスト ユーザーを選択し、**[割り当て]**、**[はい]** の順にクリックして、割り当てを確定します。

    ![はい](./media/active-directory-saas-logicmonitor-tutorial/IC767830.png "Yes")

シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。









