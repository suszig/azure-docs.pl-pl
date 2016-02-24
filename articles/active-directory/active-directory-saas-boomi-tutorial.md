<properties 
    pageTitle="チュートリアル: Azure Active Directory と Boomi の統合 | Microsoft Azure" 
    description="Azure Active Directory で Boomi を使用して、シングル サインオンや自動プロビジョニングなどを有効にする方法について説明します。" 
    services="active-directory" 
    authors="MarkusVi"  
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

#チュートリアル: Azure Active Directory と Boomi の統合

このチュートリアルでは、Azure と Boomi の統合について説明します。  
このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   Boomi でのシングル サインオンが有効なサブスクリプション

このチュートリアルを完了すると、Boomi に割り当てた Azure AD ユーザーは、Boomi 会社サイト (サービス プロバイダーが開始したサインオン) で、アプリケーションにシングル サインオンまたはできるを使用して、 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。

このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  Boomi のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-boomi-tutorial/IC791134.png "Scenario")
##Boomi のアプリケーション統合の有効化

このセクションでは、Boomi のアプリケーション統合を有効にする方法について説明します。

###Boomi のアプリケーション統合を有効にするには、次の手順を実行します。

1.  左側のナビゲーション ウィンドウで、Azure 管理ポータルで、クリックして **Active Directory**します。

    ![Active Directory](./media/active-directory-saas-boomi-tutorial/IC700993.png "Active Directory")

2.   **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3.  ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。

    ![アプリケーション](./media/active-directory-saas-boomi-tutorial/IC700994.png "Applications")

4.  クリックして **追加** ページの下部にあります。

    ![アプリケーションの追加](./media/active-directory-saas-boomi-tutorial/IC749321.png "Add application")

5.   **実行する操作** ダイアログ ボックスで、をクリックして **ギャラリーからアプリケーションを追加**します。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-boomi-tutorial/IC749322.png "Add an application from gallerry")

6.   **検索ボックス**, 、型 **Boomi**します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-boomi-tutorial/IC790822.png "Application Gallery")

7.  結果ウィンドウで [ **Boomi**, 、クリックして **完了** アプリケーションを追加します。

    ![Boomi](./media/active-directory-saas-boomi-tutorial/IC790823.png "Boomi")
##シングル サインオンの構成

このセクションでは、SAML プロトコルに基づくフェデレーションを使用して、ユーザーが Azure AD のアカウントで Boomi に対する認証を行えるようにする方法を説明します。

###シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **Boomi** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、* * [シングル サインオン * * ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-boomi-tutorial/IC790824.png "Configure Single Sign-On")

2.   **どのようなユーザーが Boomi へのサインオン** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、] をクリックし、 **次**します。

    ![Configure Single Sign-On](./media/active-directory-saas-boomi-tutorial/IC790825.png "Configure Single Sign-On")

3.   **アプリケーション URL の構成** ] ページの [、 **Boomi 応答 URL** ] ボックスに「、 **Boomi AtomSphere ログイン URL** (例::"*https://platform.boomi.com/sso/AccountName/saml*")、順にクリック **次**します。

    ![アプリケーション URL の構成](./media/active-directory-saas-boomi-tutorial/IC790826.png "Configure App URL")

4.   **Boomi でのシングル サインオンの構成** ] ページで、証明書をダウンロードする] をクリックして **証明書のダウンロード**, 、お使いのコンピューターにローカルで証明書ファイルを保存します。

    ![Configure Single Sign-On](./media/active-directory-saas-boomi-tutorial/IC790827.png "Configure Single Sign-On")

5.  別の Web ブラウザー ウィンドウで、Boomi 企業サイトに管理者としてログインします。

6.  上部にあるツールバーで、会社名をクリックし、 **セットアップ**します。

    ![セットアップ](./media/active-directory-saas-boomi-tutorial/IC790828.png "Setup")

7.  クリックして **SSO オプション**します。

    ![SSO Options](./media/active-directory-saas-boomi-tutorial/IC790829.png "SSO Options")

8.   **シングル サインオン オプション** セクションで、次の手順に従います。

    ![Single Sign-On Options](./media/active-directory-saas-boomi-tutorial/IC790830.png "Single Sign-On Options")

    1.  選択 **SAML シングル サインオンを有効にする**です。
    2.  クリックして **インポート**, をダウンロードした証明書をアップロードします。
    3.  Azure ポータルで、 **Boomi でのシングル サインオンの構成** ] ダイアログ ページでコピー、 **リモート ログイン URL** 値に設定して、貼り付けます、 **Identity Provider Login URL** ] ボックスに貼り付けます。
    4.  として **Federation Id Location**, [ **フェデレーション Id はサブジェクトの NameID 要素に**します。
    5.  クリックして **保存**します。

9.  Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-boomi-tutorial/IC775560.png "Configure Single Sign-On")
##ユーザー プロビジョニングの構成

Azure AD ユーザーが Boomi にログインできるようにするには、ユーザーを Boomi にプロビジョニングする必要があります。  
Boomi の場合、プロビジョニングは手動で行います。

###ユーザー プロビジョニングを構成するには、次の手順に従います。

1.  ログインして **Boomi** 会社サイトに管理者として。

2.  移動して **ユーザー管理 \ > ユーザー**します。

    ![ユーザー](./media/active-directory-saas-boomi-tutorial/IC790831.png "Users")

3.  クリックして **ユーザーを追加**します。

    ![ユーザーの追加](./media/active-directory-saas-boomi-tutorial/IC790832.png "Add User")

4.   **ユーザー役割の追加** ] ダイアログ ページで、次の手順を実行します。

    ![ユーザーの追加](./media/active-directory-saas-boomi-tutorial/IC790833.png "Add User")

    1.  型、 **名**, 、**姓** と **電子メール** 関連テキスト ボックスに、プロビジョニングする有効な AAD アカウントのです。
    2.  [OK] をクリックします。

>[AZURE.NOTE] 他の Boomi ユーザー アカウント作成ツールを使用することができますか、AAD ユーザー アカウントをプロビジョニング、Boomi から提供されている Api です。

##ユーザーの割り当て

構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

###ユーザーを Boomi に割り当てるには、次の手順を実行します。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  * * Boomi * * アプリケーション統合ページで、をクリックして **ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-boomi-tutorial/IC790834.png "Assign Users")

3.  テスト ユーザーを選択して [ **割り当てる**, 、順にクリック **はい** して割り当てを確認します。

    ![あり](./media/active-directory-saas-boomi-tutorial/IC767830.png "Yes")

シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。

