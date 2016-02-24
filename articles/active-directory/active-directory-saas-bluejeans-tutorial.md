<properties 
    pageTitle="チュートリアル: Azure AD と BlueJeans の統合 | Microsoft Azure" 
    description="Azure Active Directory で BlueJeans を使用して、シングル サインオンや自動プロビジョニングなどを有効にする方法について説明します。" 
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

#チュートリアル: Azure AD と BlueJeans の統合

このチュートリアルでは、Azure と BlueJeans の統合について説明します。  
このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   BlueJeans でのシングル サインオンが有効なサブスクリプション

このチュートリアルを完了すると、BlueJeans に割り当てた Azure AD ユーザーは、BlueJeans 会社サイト (サービス プロバイダーが開始したサインオン) で、アプリケーションにシングル サインオンまたはできるを使用して、 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。

このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  BlueJeans のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-bluejeans-tutorial/IC785860.png "Scenario")
##BlueJeans のアプリケーション統合の有効化

このセクションでは、BlueJeans のアプリケーション統合を有効にする方法について説明します。

###BlueJeans のアプリケーション統合を有効にするには、次の手順を実行します。

1.  左側のナビゲーション ウィンドウで、Azure 管理ポータルで、クリックして **Active Directory**します。

    ![Active Directory](./media/active-directory-saas-bluejeans-tutorial/IC700993.png "Active Directory")

2.   **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3.  ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。

    ![アプリケーション](./media/active-directory-saas-bluejeans-tutorial/IC700994.png "Applications")

4.  クリックして **追加** ページの下部にあります。

    ![アプリケーションの追加](./media/active-directory-saas-bluejeans-tutorial/IC749321.png "Add application")

5.   **実行する操作** ダイアログ ボックスで、をクリックして **ギャラリーからアプリケーションを追加**します。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-bluejeans-tutorial/IC749322.png "Add an application from gallerry")

6.   **検索ボックス**, 、型 **BlueJeans**します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-bluejeans-tutorial/IC785861.png "Application Gallery")

7.  結果ウィンドウで [ **BlueJeans**, 、クリックして **完了** アプリケーションを追加します。

    ![BlueJeans](./media/active-directory-saas-bluejeans-tutorial/IC785862.png "BlueJeans")
##シングル サインオンの構成

このセクションでは、SAML プロトコルに基づくフェデレーションを使用して、ユーザーが Azure AD のアカウントで BlueJeans に対する認証を行えるようにする方法を説明します。

###シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **BlueJeans** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、* * [シングル サインオン * * ダイアログ。

    ![Configure single sign-on](./media/active-directory-saas-bluejeans-tutorial/IC785863.png "Configure single sign-on")

2.   **どのようなユーザーが BlueJeans へのアクセスを** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、] をクリックし、 **次**します。

    ![Configure Single Sign-On](./media/active-directory-saas-bluejeans-tutorial/IC785864.png "Configure Single Sign-On")

3.   **アプリケーション URL の構成** ] ページの [、 **BlueJeans サインオン URL** ] ボックスに、次のパターンを使用して URL を入力"*https://company.BlueJeans.com*"、] をクリックし、 **次**します。

    ![Configure App URL](./media/active-directory-saas-bluejeans-tutorial/IC785865.png "Configure App URL")

4.   **BlueJeans でのシングル サインオンの構成** ] ページで、証明書をダウンロードする] をクリックして **証明書のダウンロード**, 、お使いのコンピューターに証明書ファイルを保存します。

    ![Configure Single Sign-On](./media/active-directory-saas-bluejeans-tutorial/IC785866.png "Configure Single Sign-On")

5.  ログインする別の web ブラウザーのウィンドウで、 **BlueJeans** 会社サイトに管理者として。

6.  移動して **ADMIN \ > グループの設定 \ > セキュリティ**します。

    ![管理者](./media/active-directory-saas-bluejeans-tutorial/IC785868.png "Admin")

7.   **セキュリティ** セクションで、次の手順に従います。

    ![SAML シングル サインオン](./media/active-directory-saas-bluejeans-tutorial/IC785869.png "SAML Single Sign On")

    1.  選択 **SAML シングル サインオン**します。
    2.  選択 **自動プロビジョニングを有効にする**です。

8.  次の手順を実行します。

    ![証明書パス](./media/active-directory-saas-bluejeans-tutorial/IC785870.png "Certificate Path")

    1.  クリックして **Choose File**, 、し、ダウンロードした証明書をアップロードします。
    2.  Azure ポータルで、 **BlueJeans でのシングル サインオンの構成** ] ダイアログ ページでコピー、 **リモート ログイン URL** 値に設定して、貼り付けます、 **ログイン URL** ] ボックスに貼り付けます。
    3.  Azure ポータルで、 **BlueJeans でのシングル サインオンの構成** ] ダイアログ ページでコピー、 **パスワード変更 URL** 値に設定して、貼り付けます、 **パスワード変更 URL** ] ボックスに貼り付けます。
    4.  Azure ポータルで、 **BlueJeans でのシングル サインオンの構成** ] ダイアログ ページでコピー、 **リモート ログアウト URL** 値に設定して、貼り付けます、 **ログアウト URL** ] ボックスに貼り付けます。

9.  次の手順を実行します。

    ![変更を保存](./media/active-directory-saas-bluejeans-tutorial/IC785874.png "Save Changes")

    1.   **ユーザー id** ] ボックスに「 **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name**します。
    2.   **電子メール** ] ボックスに「 **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name**します。
    3.  クリックして **変更を保存**します。

10. Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-bluejeans-tutorial/IC785876.png "Configure Single Sign-On")
##ユーザー プロビジョニングの構成

Azure AD ユーザーが BlueJeans にログインできるようにするには、ユーザーを BlueJeans にプロビジョニングする必要があります。  
BlueJeans の場合、プロビジョニングは手動で行います。

###ユーザー アカウントをプロビジョニングするには、次の手順を実行します。

1.  ログインして **BlueJeans** 会社サイトに管理者として。

2.  移動 **ADMIN \ > ユーザーの管理 \ > ユーザーの追加**します。

    ![管理者](./media/active-directory-saas-bluejeans-tutorial/IC785877.png "Admin")

    >[AZURE.IMPORTANT]  **ユーザーの追加** ] タブにある矢印にいる場合で、 **セキュリティ] タブ**, 、**自動プロビジョニングを有効にする** チェック ボックスがオフです。

3.   **ユーザーの追加** セクションで、次の手順に従います。

    ![ユーザーの追加](./media/active-directory-saas-bluejeans-tutorial/IC785886.png "Add User")

    1.  型 a **BlueJeans ユーザー名**, 、 **電子メール アドレス**, 、 **BlueJeans ミーティング ID**, 、 **モデレーター パスコード**, 、 **フル_ネーム**, 、 **会社** 関連テキスト ボックスに、プロビジョニングする有効な AAD アカウントのです。
    2.  クリックして **ユーザーを追加**します。

>[AZURE.NOTE] 他の BlueJeans ユーザー アカウント作成ツールを使用することができますか、AAD ユーザー アカウントをプロビジョニング、BlueJeans から提供されている Api です。

##ユーザーの割り当て

構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

###ユーザーを BlueJeans に割り当てるには、次の手順を実行します。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  * * BlueJeans * * アプリケーション統合ページで、をクリックして **ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-bluejeans-tutorial/IC785887.png "Assign Users")

3.  テスト ユーザーを選択して [ **割り当てる**, 、順にクリック **はい** して割り当てを確認します。

    ![あり](./media/active-directory-saas-bluejeans-tutorial/IC767830.png "Yes")

シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。

