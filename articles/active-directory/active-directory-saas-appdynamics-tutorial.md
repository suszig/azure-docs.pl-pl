<properties 
    pageTitle="チュートリアル: Azure Active Directory と AppDynamics の統合 | Microsoft Azure" 
    description="Azure Active Directory で AppDynamics を使用して、シングル サインオンや自動プロビジョニングなどを有効にする方法について説明します。" 
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

#チュートリアル: Azure Active Directory と AppDynamics の統合

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
##AppDynamics のアプリケーション統合の有効化

このセクションでは、AppDynamics のアプリケーション統合を有効にする方法を説明します。

###AppDynamics のアプリケーション統合を有効にするには、次の手順に従います。

1.  左側のナビゲーション ウィンドウで、Azure 管理ポータルで、クリックして **Active Directory**します。

    ![Active Directory](./media/active-directory-saas-appdynamics-tutorial/IC700993.png "Active Directory")

2.   **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3.  ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。

    ![アプリケーション](./media/active-directory-saas-appdynamics-tutorial/IC700994.png "Applications")

4.  クリックして **追加** ページの下部にあります。

    ![アプリケーションの追加](./media/active-directory-saas-appdynamics-tutorial/IC749321.png "Add application")

5.   **実行する操作** ダイアログ ボックスで、をクリックして **ギャラリーからアプリケーションを追加**します。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-appdynamics-tutorial/IC749322.png "Add an application from gallerry")

6.   **検索ボックス**, 、型 **AppDynamics**します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-appdynamics-tutorial/IC790210.png "Application Gallery")

7.  結果ウィンドウで [ **AppDynamics**, 、クリックして **完了** アプリケーションを追加します。

    ![AppDynamics](./media/active-directory-saas-appdynamics-tutorial/IC790211.png "AppDynamics")
##シングル サインオンの構成

このセクションでは、ユーザーが SAML プロトコルに基づくフェデレーションを使用して、Azure AD でのユーザーのアカウントで AppDynamics に対する認証を行えるようにする方法を説明します。  
この手順の途中で、base-64 でエンコードされた証明書ファイルを作成する必要があります。  
この手順に慣れていない場合は、次を参照してください。 [バイナリ証明書をテキスト ファイルに変換する方法](http://youtu.be/PlgrzUZ-Y1o)します。

###シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **AppDynamics** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、* * [シングル サインオン * * ダイアログ。

    ![シングル サインオンの構成](./media/active-directory-saas-appdynamics-tutorial/IC790212.png "Configure Single SignOn")

2.   **方法を選択して AppDynamics にサインオン** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、] をクリックし、 **次**します。

    ![シングル サインオンの構成](./media/active-directory-saas-appdynamics-tutorial/IC790213.png "Configure Single SignOn")

3.   **アプリケーション URL の構成** ] ページで、 **AppDynamics サインオン URL** 使用する URL にユーザーが AppDynamics にサインオンする] ボックスに、型 ("*https://companyname.saas.appdynamics.com*")、順にクリック **次**します。

    ![Configure App URL](./media/active-directory-saas-appdynamics-tutorial/IC790214.png "Configure App URL")

4.   **AppDynamics でのシングル サインオンの構成** ] ページで、証明書をダウンロードする] をクリックして **証明書のダウンロード**, 、お使いのコンピューターに証明書ファイルを保存します。

    ![シングル サインオンの構成](./media/active-directory-saas-appdynamics-tutorial/IC790215.png "Configure Single SignOn")

5.  別の Web ブラウザーのウィンドウで、管理者として AppDynamics 企業サイトにログインします。

6.  上部にあるツールバーで、クリックして **設定**, 、クリックして **管理**します。

    ![管理](./media/active-directory-saas-appdynamics-tutorial/IC790216.png "Administration")

7.  クリックして、 **認証プロバイダー** ] タブをクリックします。

    ![Authentication Provider](./media/active-directory-saas-appdynamics-tutorial/IC790224.png "Authentication Provider")

8.   **認証プロバイダー** セクションで、次の手順に従います。

    ![SAML の構成](./media/active-directory-saas-appdynamics-tutorial/IC790225.png "SAML Configuration")

    1.  として **認証プロバイダー**, [ **SAML**します。
    2.  Azure ポータルで、 **AppDynamics でのシングル サインオンの構成** ] ダイアログ ページでコピー、 **リモート ログイン URL** 値に設定して、貼り付けます、 **ログイン URL** ] ボックスに貼り付けます。
    3.  Azure ポータルで、 **AppDynamics でのシングル サインオンの構成** ] ダイアログ ページでコピー、 **リモート ログアウト URL** 値に設定して、貼り付けます、 **ログアウト URL** ] ボックスに貼り付けます。
    4.  作成、 **base-64 でエンコードされた** からダウンロードした証明書ファイル。  

        >[AZURE.TIP] 詳細については、次を参照してください [バイナリ証明書をテキスト ファイルに変換する方法。](http://youtu.be/PlgrzUZ-Y1o)

    5.  Base-64 でエンコードされた証明書をメモ帳で開き、という内容がクリップボードにコピーおよびに貼り付け、 **証明書** ] テキスト ボックス
    6.  クリックして **保存**します。
        ![保存](./media/active-directory-saas-appdynamics-tutorial/IC777673.png "Save")

9.  Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![シングル サインオンの構成](./media/active-directory-saas-appdynamics-tutorial/IC790226.png "Configure Single SignOn")
##ユーザー プロビジョニングの構成

Azure AD ユーザーが AppDynamics にログインできるようにするには、そのユーザーを AppDynamics にプロビジョニングする必要があります。  
AppDynamics の場合、プロビジョニングは手動で行います。

###ユーザー プロビジョニングを構成するには、次の手順に従います。

1.  AppDynamics 企業サイトに管理者としてログインします。

2.  移動して **ユーザー**, 、] をクリックし、 **+** を開くには、 **Create User** ダイアログ。

    ![ユーザー](./media/active-directory-saas-appdynamics-tutorial/IC790229.png "Users")

3.   **Create User** セクションで、次の手順に従います。

    ![Create User](./media/active-directory-saas-appdynamics-tutorial/IC790230.png "Create User")

    1.  型、 **Username**, 、**名前**, 、**電子メール**, 、**新しいパスワード**, 、**もう一度新しいパスワード** 関連テキスト ボックスに、プロビジョニングする有効な AAD アカウントのです。
    2.  クリックして **保存**します。

>[AZURE.NOTE] 他の AppDynamics ユーザー アカウント作成ツールを使用するまたは Azure AD ユーザー アカウントをプロビジョニングする AppDynamics 提供の Api です。

##ユーザーの割り当て

構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

###ユーザーを AppDynamics に割り当てるには、次の手順に従います。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  * * AppDynamics * * アプリケーション統合ページで、をクリックして **ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-appdynamics-tutorial/IC790231.png "Assign Users")

3.  テスト ユーザーを選択して [ **割り当てる**, 、順にクリック **はい** して割り当てを確認します。

    ![あり](./media/active-directory-saas-appdynamics-tutorial/IC767830.png "Yes")

シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。

