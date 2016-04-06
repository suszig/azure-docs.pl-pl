<properties 
    pageTitle="チュートリアル: Azure Active Directory と Clarizen の統合 | Microsoft Azure" 
    description="Azure Active Directory で Clarizen を使用して、シングル サインオンや自動プロビジョニングなどを有効にする方法について説明します。" 
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

#チュートリアル: Azure Active Directory と Clarizen の統合

このチュートリアルでは、Azure と Clarizen の統合について説明します。  
このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   Clarizen でのシングル サインオンが有効なサブスクリプション

このチュートリアルを完了すると、Clarizen に割り当てた Azure AD ユーザーは、Clarizen 会社サイト (サービス プロバイダーが開始したサインオン) で、アプリケーションにシングル サインオンまたはできるを使用して、 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。

このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  Clarizen のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-clarizen-tutorial/IC784679.png "Scenario")
##Clarizen のアプリケーション統合の有効化

このセクションでは、Clarizen のアプリケーション統合を有効にする方法について説明します。

###Clarizen のアプリケーション統合を有効にするには、次の手順を実行します。

1.  左側のナビゲーション ウィンドウで、Azure 管理ポータルで、クリックして **Active Directory**します。

    ![Active Directory](./media/active-directory-saas-clarizen-tutorial/IC700993.png "Active Directory")

2.   **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3.  ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。

    ![アプリケーション](./media/active-directory-saas-clarizen-tutorial/IC700994.png "Applications")

4.  クリックして **追加** ページの下部にあります。

    ![アプリケーションの追加](./media/active-directory-saas-clarizen-tutorial/IC749321.png "Add application")

5.   **実行する操作** ダイアログ ボックスで、をクリックして **ギャラリーからアプリケーションを追加**します。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-clarizen-tutorial/IC749322.png "Add an application from gallerry")

6.   **検索ボックス**, 、型 **Clarizen**します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-clarizen-tutorial/IC784680.png "Application Gallery")

7.  結果ウィンドウで [ **Clarizen**, 、クリックして **完了** アプリケーションを追加します。

    ![Clarizen](./media/active-directory-saas-clarizen-tutorial/IC784681.png "Clarizen")
##シングル サインオンの構成

このセクションでは、SAML プロトコルに基づくフェデレーションを使用して、ユーザーが Azure AD のアカウントで Clarizen に対する認証を行うことができるようにする方法を説明します。

###シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **Clarizen** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、* * [シングル サインオン * * ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-clarizen-tutorial/IC784682.png "Configure Single Sign-On")

2.   **どのようなユーザーが Clarizen へのサインオン** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、順にクリック **次**します。

    ![Configure Single Sign-On](./media/active-directory-saas-clarizen-tutorial/IC784683.png "Configure Single Sign-On")

3.   **Clarizen でのシングル サインオンの構成** ] ページで、証明書をダウンロードする] をクリックして **証明書のダウンロード**, 、お使いのコンピューターに証明書ファイルを保存します。

    ![Configure Single Sign-On](./media/active-directory-saas-clarizen-tutorial/IC784684.png "Configure Single Sign-On")

4.  ログインする別の web ブラウザーのウィンドウで、 **Clarizen** 会社サイトに管理者として (例:: *https://app2.clarizen.com/Clarizen/Pages/Service/Login.aspx*)。

5.  ユーザー名をクリックし、 **設定**します。

    ![[設定]](./media/active-directory-saas-clarizen-tutorial/IC784685.png "Settings")

6.  をクリックして、 **グローバル設定** ] タブで、し、 **Federated Authentication**, 、] をクリックして **編集**します。

    ![グローバル設定](./media/active-directory-saas-clarizen-tutorial/IC786906.png "Global Settings")

7.   **Federated Authentication** ] ダイアログ ボックスで、次の手順を実行します。

    ![フェデレーション認証](./media/active-directory-saas-clarizen-tutorial/IC785892.png "Federated Authentication")

    1.  クリックして **アップロード** をダウンロードした証明書をアップロードします。
    2.  Azure ポータルで、 **Clarizen でのシングル サインオンの構成** ] ダイアログ ページでコピー、 **シングル サインオン サービス URL** 値に設定して、貼り付けます、 **サインイン URL** ] ボックスに貼り付けます。
    3.  Azure ポータルで、 **シングル サインアウト Clarizen での構成** ] ダイアログ ページでコピー、 **シングル サインアウト サービス URL** 値に設定して、貼り付けます、 **サインアウト URL** ] ボックスに貼り付けます。
    4.  選択 **POST を使用して**します。
    5.  クリックして **保存**します。

8.  Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-clarizen-tutorial/IC784688.png "Configure Single Sign-On")
##ユーザー プロビジョニングの構成

Azure AD ユーザーが Clarizen にログインできるようにするには、そのユーザーを Clarizen にプロビジョニングする必要があります。  
Clarizen の場合、プロビジョニングは手動で行います。

###ユーザー アカウントをプロビジョニングするには、次の手順を実行します。

1.  ログインして **Clarizen** 会社サイトに管理者として。

2.  クリックして **人**します。

    ![People](./media/active-directory-saas-clarizen-tutorial/IC784689.png "People")

3.  クリックして **ユーザーの招待**します。

    ![ユーザーの招待](./media/active-directory-saas-clarizen-tutorial/IC784690.png "Invite Users")

4.  [ユーザーの招待] ダイアログ ページで、次の手順を実行します。

    ![ユーザーの招待](./media/active-directory-saas-clarizen-tutorial/IC784691.png "Invite People")

    1.   **電子メール** ] ボックスに、プロビジョニングする有効な Azure Active Directory アカウントの電子メール アドレスを入力します。
    2.  クリックして **招待**します。

    >[AZURE.NOTE] Azure Active Directory アカウント所有者は電子メールを受信し、アクティブ化する前に、自分のアカウントを確認するリンクを参照します。

##ユーザーの割り当て

構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

###ユーザーを Clarizen に割り当てるには、次の手順を実行します。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  * * Clarizen * * アプリケーション統合ページで、をクリックして **ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-clarizen-tutorial/IC784692.png "Assign Users")

3.  テスト ユーザーを選択して [ **割り当てる**, 、順にクリック **はい** して割り当てを確認します。

    ![あり](./media/active-directory-saas-clarizen-tutorial/IC767830.png "Yes")

シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。


