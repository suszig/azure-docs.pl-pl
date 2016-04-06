<properties 
    pageTitle="チュートリアル: Azure Active Directory と Citrix ShareFile の統合 | Microsoft Azure" 
    description="Azure Active Directory で Citrix ShareFile を使用して、シングル サインオンや自動プロビジョニングなどを有効にする方法について説明します。" 
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

#チュートリアル: Azure Active Directory と Citrix ShareFile の統合

このチュートリアルでは、Azure と Citrix ShareFile の統合について説明します。  
このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   Citrix ShareFile テナント

このチュートリアルを完了すると、Citrix ShareFile に割り当てた Azure AD ユーザーは、Citrix ShareFile 会社サイト (サービス プロバイダーが開始したサインオン) で、アプリケーションにシングル サインオンまたはできるを使用して、 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。

このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  Citrix ShareFile のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-citrix-sharefile-tutorial/IC773620.png "Scenario")
##Citrix ShareFile のアプリケーション統合の有効化

このセクションでは、Citrix ShareFile のアプリケーション統合を有効にする方法を説明します。

###Citrix ShareFile のアプリケーション統合を有効にするには、次の手順を実行します。

1.  左側のナビゲーション ウィンドウで、Azure 管理ポータルで、クリックして **Active Directory**します。

    ![Active Directory](./media/active-directory-saas-citrix-sharefile-tutorial/IC700993.png "Active Directory")

2.   **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3.  ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。

    ![アプリケーション](./media/active-directory-saas-citrix-sharefile-tutorial/IC700994.png "Applications")

4.  クリックして **追加** ページの下部にあります。

    ![アプリケーションの追加](./media/active-directory-saas-citrix-sharefile-tutorial/IC749321.png "Add application")

5.   **実行する操作** ダイアログ ボックスで、をクリックして **ギャラリーからアプリケーションを追加**します。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-citrix-sharefile-tutorial/IC749322.png "Add an application from gallerry")

6.   **検索ボックス**, 、型 **Citrix ShareFile**します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-citrix-sharefile-tutorial/IC773621.png "Application gallery")

7.  結果ウィンドウで [ **Citrix ShareFile**, 、クリックして **完了** アプリケーションを追加します。

    ![Citrix ShareFile](./media/active-directory-saas-citrix-sharefile-tutorial/IC773622.png "Citrix ShareFile")
##シングル サインオンの構成

このセクションでは、SAML プロトコルに基づくフェデレーションを使用して、ユーザーが Azure AD のアカウントで Citrix ShareFile に対する認証を行えるようにする方法を説明します。

###シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **Citrix ShareFile** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、* * [シングル サインオン * * ダイアログ。

    ![シングル サインオンの有効化](./media/active-directory-saas-citrix-sharefile-tutorial/IC773623.png "Enable single sign-on")

2.   **どのようなユーザーが Citrix ShareFile へのサインオン** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、] をクリックし、 **次**します。

    ![Configure Single Sign-On](./media/active-directory-saas-citrix-sharefile-tutorial/IC773624.png "Configure Single Sign-On")

3.   **アプリケーション URL の構成** ] ページの [、 **Citrix ShareFile サインオン URL** ] ボックスに、次のパターンを使用して URL を入力 `https://<tenant-name>.shareFile.com`, 、] をクリックし、 **次**します。

    ![アプリケーション URL の構成](./media/active-directory-saas-citrix-sharefile-tutorial/IC773625.png "Configure App URL")

4.   **Citrix ShareFile でのシングル サインオンの構成** ] ページで、証明書をダウンロードする] をクリックして **証明書のダウンロード**, 、お使いのコンピューターに証明書ファイルを保存します。

    ![シングル サインオンの構成](./media/active-directory-saas-citrix-sharefile-tutorial/IC773626.png "ConfigureSingle Sign-On")

5.  ログインする別の web ブラウザーのウィンドウで、 **Citrix ShareFile** 会社サイトに管理者として。

6.  上部にあるツールバーで、クリックして **Admin**します。

7.  左側のナビゲーション ウィンドウで [ **シングル サインオンを構成する**です。

    ![アカウント管理](./media/active-directory-saas-citrix-sharefile-tutorial/IC773627.png "Account Administration")

8.   **でのシングル サインオン/SAML 2.0 Configuration** 」の「 **基本設定**, 、、次の手順を実行します。

    ![シングル サインオン](./media/active-directory-saas-citrix-sharefile-tutorial/IC773628.png "Single sign-on")

    1.  クリックして **Enable SAML**します。
    2.  Azure ポータルで、 **Citrix ShareFile でのシングル サインオンの構成** ] ダイアログ ページでコピー、 **エンティティ ID** 値に設定して、貼り付けます、 **Your IDP Issuer/エンティティ ID** ] ボックスに貼り付けます。
    3.  Azure ポータルで、 **Citrix ShareFile でのシングル サインオンの構成** ] ダイアログ ページでコピー、 **リモート ログイン URL** 値に設定して、貼り付けます、 **ログイン URL** ] ボックスに貼り付けます。
    4.  Azure ポータルで [ **、[シングル サインオン Citrix ShareFile で** ] ダイアログ ページでコピー、 **リモート ログアウト URL** 値に設定して、貼り付けます、 **ログアウト URL** ] ボックスに貼り付けます。
    5.  クリックして **変更** ] の横に、 **X.509 証明書** して Azure AD ポータルからダウンロードした証明書をアップロードします。
        ![基本設定](./media/active-directory-saas-citrix-sharefile-tutorial/IC773629.png "Basic Settings")

9.  クリックして **保存** Citrix ShareFile 管理ポータルで。

10. Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![シングル サインオンの構成](./media/active-directory-saas-citrix-sharefile-tutorial/IC773630.png "Configure single sign-on")
##ユーザー プロビジョニングの構成

Azure AD ユーザーが Citrix ShareFile にログインできるようにするには、ユーザーを Citrix ShareFile にプロビジョニングする必要があります。  
Citrix ShareFile の場合、プロビジョニングは手動で行います。

###ユーザー アカウントをプロビジョニングするには、次の手順を実行します。

1.  ログインして **Citrix ShareFile** テナントです。

2.  クリックして **ユーザーの管理 \ > Manage Users Home \ > [+ Create Employee**します。

    ![従業員の作成](./media/active-directory-saas-citrix-sharefile-tutorial/IC781050.png "Create Employee")

3.  入力、 **電子メール**, 、**名** と **姓、名** の有効な Azure AD アカウントをプロビジョニングします。

    ![基本情報](./media/active-directory-saas-citrix-sharefile-tutorial/IC799951.png "Basic Information")

4.  クリックして **ユーザーを追加**します。

    >[AZURE.NOTE] AAD アカウントの所有者は電子メールを受信し、アクティブ化する前に、自分のアカウントを確認するリンクを参照します。

>[AZURE.NOTE] 他の Citrix ShareFile ユーザー アカウント作成ツールを使用することができますか、AAD ユーザー アカウントをプロビジョニング、Citrix ShareFile 提供の Api です。

##ユーザーの割り当て

構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

###ユーザーを Citrix ShareFile に割り当てるには、次の手順を実行します。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  * * Citrix ShareFile * * アプリケーション統合ページで、をクリックして **ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-citrix-sharefile-tutorial/IC773631.png "Assign users")

3.  テスト ユーザーを選択して [ **割り当てる**, 、順にクリック **はい** して割り当てを確認します。

    ![あり](./media/active-directory-saas-citrix-sharefile-tutorial/IC767830.png "Yes")

シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。


