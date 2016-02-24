<properties 
    pageTitle="チュートリアル: Azure Active Directory と Zscaler の統合 | Microsoft Azure" 
    description="Azure Active Directory で Zscaler を使用して、シングル サインオンや自動プロビジョニングなどを有効にする方法について説明します。" 
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

#チュートリアル: Azure Active Directory と Zscaler の統合
  
このチュートリアルでは、Azure と Zscaler の統合について説明します。 このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   Zscaler のテナント
  
このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  Zscaler のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  プロキシ設定の構成
4.  ユーザー プロビジョニングの構成
5.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-zscaler-tutorial/IC769226.png "Scenario")

##Zscaler のアプリケーション統合の有効化
  
このセクションでは、Zscaler のアプリケーション統合を有効にする方法を説明します。

###Zscaler のアプリケーション統合を有効にするには、次の手順を実行します。

1.  左側のナビゲーション ウィンドウで、Azure 管理ポータルで、クリックして **Active Directory**します。

    ![Active Directory](./media/active-directory-saas-zscaler-tutorial/IC700993.png "Active Directory")

2.   **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3.  ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。

    ![アプリケーション](./media/active-directory-saas-zscaler-tutorial/IC700994.png "Applications")

4.  クリックして **追加** ページの下部にあります。

    ![アプリケーションの追加](./media/active-directory-saas-zscaler-tutorial/IC749321.png "Add application")

5.   **実行する操作** ダイアログ ボックスで、をクリックして **ギャラリーからアプリケーションを追加**します。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-zscaler-tutorial/IC749322.png "Add an application from gallerry")

6.   **検索ボックス**, 、型 **Zscaler**します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-zscaler-tutorial/IC769227.png "Application gallery")

7.  結果ウィンドウで [ **Zscaler**, 、クリックして **完了** アプリケーションを追加します。

    ![Zscaler](./media/active-directory-saas-zscaler-tutorial/IC769228.png "Zscaler")

##シングル サインオンの構成
  
このセクションでは、ユーザーが SAML プロトコルに基づくフェデレーションを使用して、Azure AD でのユーザーのアカウントで Zscaler に対する認証を行うことができるようにする方法を説明します。  
この手順の途中で、証明書を Zscaler にアップロードする必要があります。

###シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **Zscaler** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、* * [シングル サインオン * * ダイアログ。

    ![Enable single sign-on](./media/active-directory-saas-zscaler-tutorial/IC769229.png "Enable single sign-on")

2.   **どのようなユーザーが Zscaler へのアクセスを** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、] をクリックし、 **次**します。

    ![シングル サインオンの有効化](./media/active-directory-saas-zscaler-tutorial/IC769230.png "Configure single sign on")

3.   **アプリケーション URL の構成** ] ページで、 **Zscaler サインイン URL** ] ボックスは、Zscaler から取得した URL に、記号を入力し、順にクリックして **次**: 

    >[AZURE.NOTE] サインイン URL とは何かわからない場合は、Zscaler サポート チームに問い合わせてください。

    ![アプリケーション URL の構成](./media/active-directory-saas-zscaler-tutorial/IC769231.png "Configure app URL")

4.   **Zscaler でのシングル サインオンの構成** ] ページで、次の手順を実行します。

    ![Configure single sign-on](./media/active-directory-saas-zscaler-tutorial/IC769232.png "Configure single sign-on")

    1.  クリックして **証明書のダウンロード**, 、としては、証明書ファイルをローカルに保存および **c:\\Zscaler.cer**します。
    2.  コピー、 **認証要求 URL** をクリップボードにします。

5.  Zscaler テナントにログインします。

6.  上部にあるメニュー [ **管理**します。

    ![管理](./media/active-directory-saas-zscaler-tutorial/IC769486.png "Administration")

7.   **管理の管理者とロール**, 、クリックして **管理ユーザーと認証**します。

    ![管理者とロールの管理](./media/active-directory-saas-zscaler-tutorial/IC769487.png "Manage Administrators & Roles")

8.   **、組織の認証オプションを選択して** セクションで、次の手順に従います。

    ![認証オプションの選択](./media/active-directory-saas-zscaler-tutorial/IC769488.png "Choose Authentication Options")

    1.  選択 **SAML シングル サインオンを使用して認証**します。
    2.  クリックして **SAML シングル サインオン パラメーターの構成**します。

9.   **SAML シングル サインオン パラメーターの構成** ] ダイアログ ページで、次の手順を実行し、順にクリックして **実行**:

    ![証明書のアップロード](./media/active-directory-saas-zscaler-tutorial/IC769489.png "Upload certificate")

    1.   **認証のためにユーザーが送られる SAML ポータルの URL** ] ボックスの値を貼り付けます、 **認証要求 URL** フィールドを Azure ポータルからです。
    2.   **属性のログイン名を含む** ] ボックスに「 **NameID**します。
    3.   **SSL 公開証明書のアップロード** フィールドに、Azure ポータルからダウンロードした証明書をアップロードします。
    4.  選択 **SAML 自動プロビジョニングを有効にする**です。

10.  **ユーザー認証を構成する** ] ダイアログ ページで、次の手順を実行します。

    ![ユーザー認証の構成](./media/active-directory-saas-zscaler-tutorial/IC769490.png "Configure User Authentication")

    1.  クリックして **保存**します。
    2.  クリックして **を今すぐアクティブ**します。

11. Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![Configure single sign-on](./media/active-directory-saas-zscaler-tutorial/IC769491.png "Configure single sign-on")

##プロキシ設定の構成

###Internet Explorer でプロキシ設定を構成するには

1.  開始 **Internet Explorer**します。

2.  選択 **インターネット オプション** から、 **ツール** メニューを開くには、 **インターネット オプション** ダイアログ。

    ![を開き](./media/active-directory-saas-zscaler-tutorial/IC769492.png "Internet Options")

3.  クリックして、 **接続** ] タブをクリックします。

    ![接続](./media/active-directory-saas-zscaler-tutorial/IC769493.png "Connections")

4.  クリックして **LAN の設定** を開くには、 **LAN の設定** ダイアログ。

5.  [プロキシ サーバー] セクションで、次の手順を実行します。

    ![プロキシ サーバー](./media/active-directory-saas-zscaler-tutorial/IC769494.png "Proxy server")

    1.  [LAN にプロキシ サーバーを使用する] をオンにします。
    2.  [アドレス] ボックスに入力 **gateway.zscalertwo.net**します。
    3.  [ポート] ボックスに入力 **80**します。
    4.  選択 **ローカル アドレスに対してプロキシ サーバーを使用しない**します。
    5.  をクリックして **OK** を閉じる、 **ローカル エリア ネットワーク (LAN) 設定** ダイアログ。

6.  をクリックして **OK** を閉じる、 **インターネット オプション** ダイアログ。

##ユーザー プロビジョニングの構成
  
Azure AD ユーザーが Zscaler にログインできるようにするには、そのユーザーを Zscaler にプロビジョニングする必要があります。  
Zscaler の場合、プロビジョニングは手動で行います。

###ユーザー プロビジョニングを構成するには、次の手順に従います。

1.  ログインして **Zscaler** テナントです。

2.  クリックして **管理**します。

    ![管理](./media/active-directory-saas-zscaler-tutorial/IC781035.png "Administration")

3.  クリックして **ユーザー管理**します。

    ![ユーザー管理](./media/active-directory-saas-zscaler-tutorial/IC781036.png "User Management")

4.   **ユーザー** ] タブ、[ **追加**します。

    ![[追加] のいずれかを](./media/active-directory-saas-zscaler-tutorial/IC781037.png "Add")

5.  [ユーザーの追加] セクションで、次の手順を実行します。

    ![ユーザーの追加](./media/active-directory-saas-zscaler-tutorial/IC781038.png "Add User")

    1.  型、 **UserID**, 、**ユーザー表示名**, 、**パスワード**, 、**パスワードの確認**, 、し、[ **グループ** と **部門** 、プロビジョニングする有効な AAD アカウントのです。
    2.  クリックして **保存**します。

>[AZURE.NOTE] AAD ユーザー アカウントをプロビジョニング、Zscaler 提供の Api やその他の Zscaler ユーザー アカウント作成ツールを使用できます。

##ユーザーの割り当て
  
構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

###ユーザーを Zscaler に割り当てるには、次の手順を実行します。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.   **Zscaler** アプリケーション統合ページで、をクリックして **ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-zscaler-tutorial/IC769495.png "Assign users")

3.  テスト ユーザーを選択して [ **割り当てる**, 、順にクリック **はい** して割り当てを確認します。

    ![あり](./media/active-directory-saas-zscaler-tutorial/IC767830.png "Yes")
  
シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。

