<properties 
    pageTitle="チュートリアル: Azure Active Directory と Zscaler One の統合 | Microsoft Azure" 
    description="Azure Active Directory で Zscaler One を使用して、シングル サインオンや自動プロビジョニングなどを有効にする方法について説明します。" 
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

#チュートリアル: Azure Active Directory と Zscaler One の統合

このチュートリアルでは、Azure と ZScaler One の統合について説明します。  
 このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。  

-   有効な Azure サブスクリプション
-   ZScaler One でのシングル サインオンが有効なサブスクリプション  

このチュートリアルを完了すると、ZScaler One に割り当てた Azure AD ユーザーは、ZScaler One 会社サイト (サービス プロバイダーが開始したサインオン) で、アプリケーションにシングル サインオンまたはできるを使用して、 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。  

このチュートリアルで説明するシナリオは、次の要素で構成されています。  

1.  ZScaler One のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  プロキシ設定の構成
4.  ユーザー プロビジョニングの構成
5.  ユーザーの割り当て  

![シナリオ](./media/active-directory-saas-zscaler-one-tutorial/IC800214.png "Scenario")  

##ZScaler One のアプリケーション統合の有効化

このセクションでは、ZScaler One のアプリケーション統合を有効にする方法を説明します。  

###ZScaler One のアプリケーション統合を有効にするには、次の手順を実行します。

1.  左側のナビゲーション ウィンドウで、Azure 管理ポータルで、クリックして **Active Directory**します。  

    ![Active Directory](./media/active-directory-saas-zscaler-one-tutorial/IC700993.png "Active Directory")  

2.   **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。  

3.  ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。  

    ![アプリケーション](./media/active-directory-saas-zscaler-one-tutorial/IC700994.png "Applications")  

4.  クリックして **追加** ページの下部にあります。  

    ![アプリケーションの追加](./media/active-directory-saas-zscaler-one-tutorial/IC749321.png "Add application")  

5.   **実行する操作** ダイアログ ボックスで、をクリックして **ギャラリーからアプリケーションを追加**します。  

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-zscaler-one-tutorial/IC749322.png "Add an application from gallerry")  

6.   **検索ボックス**, 、型 **ZScaler One**します。  

    ![アプリケーション ギャラリー](./media/active-directory-saas-zscaler-one-tutorial/IC800215.png "Application Gallery")  

7.  結果ウィンドウで [ **ZScaler One**, 、クリックして **完了** アプリケーションを追加します。  

    ![ZScaler One](./media/active-directory-saas-zscaler-one-tutorial/IC800216.png "ZScaler One")  

##シングル サインオンの構成

このセクションでは、ユーザーが SAML プロトコルに基づくフェデレーションを使用して、Azure AD でのユーザーのアカウントで ZScaler One に対する認証を行うことができるようにする方法を説明します。  
この手順の途中で、base-64 でエンコードされた証明書を ZScaler One テナントにアップロードする必要があります。  
この手順に慣れていない場合は、次を参照してください [バイナリ証明書をテキスト ファイルに変換する方法。](http://youtu.be/PlgrzUZ-Y1o)  

###シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **ZScaler One** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、* * [シングル サインオン * * ダイアログ。  

    ![Configure Single Sign-On](./media/active-directory-saas-zscaler-one-tutorial/IC800217.png "Configure Single Sign-On")  

2.   **どのようなユーザーが ZScaler One へのサインオン** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、] をクリックし、 **次**します。  

    ![Configure Single Sign-On](./media/active-directory-saas-zscaler-one-tutorial/IC800218.png "Configure Single Sign-On")  

3.   **アプリケーション URL の構成** ] ページで、 **ZScaler One サインオン URL** ] ボックスに、ユーザーが ZScaler One アプリケーションにサインオンする際に使用する URL を入力し、順にクリックして **次**します。  

    ![Configure App URL](./media/active-directory-saas-zscaler-one-tutorial/IC800219.png "Configure App URL")  

    >[AZURE.NOTE]必要がある場合、ZScaler One サポート チームから、環境の実際の値を取得できます。  

4.   **ZScaler One でのシングル サインオンの構成** ] ページで、証明書をダウンロードする] をクリックして **証明書のダウンロード**, 、お使いのコンピューターに証明書ファイルを保存します。  

    ![Configure Single Sign-On](./media/active-directory-saas-zscaler-one-tutorial/IC800220.png "Configure Single Sign-On")  

5.  別の Web ブラウザー ウィンドウで、ZScaler One 企業サイトに管理者としてログインします。  

6.  上部にあるメニュー [ **管理**します。  

    ![管理](./media/active-directory-saas-zscaler-one-tutorial/IC800206.png "Administration")  

7.   **管理の管理者とロール**, 、クリックして **ユーザーの管理と認証**します。  

    ![ユーザーと認証の管理](./media/active-directory-saas-zscaler-one-tutorial/IC800207.png "Manage Users & Authentication")  

8.   **、組織の認証オプションを選択して** セクションで、次の手順に従います。  

    ![認証](./media/active-directory-saas-zscaler-one-tutorial/IC800208.png "Authentication")  

    1.  選択 **SAML シングル サインオンを使用して認証**します。  
    2.  クリックして **SAML シングル サインオン パラメーターの構成**します。  

9.   **SAML シングル サインオン パラメーターの構成** ] ダイアログ ページで、次の手順を実行し、順にクリックして **実行**:  

    ![シングル サインオン](./media/active-directory-saas-zscaler-one-tutorial/IC800209.png "Single Sign-On")  

    1.  Azure ポータルでの **ZScaler One でのシングル サインオンの構成** ] ダイアログ ページでコピー、 **認証要求 URL** 値に設定して、貼り付けます、 **認証のためにユーザーが送られる SAML ポータルの URL** ] ボックスに貼り付けます。  
    2.   **属性のログイン名を含む** ] ボックスに「 **NameID**します。  
    3.  ダウンロードした証明書をアップロードするにはクリックして **Zscaler pem**します。  
    4.  選択 **SAML 自動プロビジョニングを有効にする**です。  

10.  **ユーザー認証を構成する** ] ダイアログ ページで、次の手順を実行します。  

    ![管理](./media/active-directory-saas-zscaler-one-tutorial/IC800210.png "Administration")  

    1.  クリックして **保存**します。  
    2.  クリックして **を今すぐアクティブ**します。  

11. Azure ポータルで、 **ZScaler One でのシングル サインオンの構成** ] ダイアログ ページで、シングル サインオンの構成情報を選択し、 **完了**します。  

    ![Configure Single Sign-On](./media/active-directory-saas-zscaler-one-tutorial/IC800221.png "Configure Single Sign-On")  

##プロキシ設定の構成

###Internet Explorer でプロキシ設定を構成するには

1.  開始 **Internet Explorer**します。  

2.  選択 **インターネット オプション** から、 **ツール** メニューを開くには、 **インターネット オプション** ダイアログ。  

    ![を開き](./media/active-directory-saas-zscaler-one-tutorial/IC769492.png "Internet Options")  

3.  クリックして、 **接続** ] タブをクリックします。  

    ![接続](./media/active-directory-saas-zscaler-one-tutorial/IC769493.png "Connections")  

4.  クリックして **LAN の設定** を開くには、 **LAN の設定** ダイアログ。  

5.  [プロキシ サーバー] セクションで、次の手順を実行します。  

    ![プロキシ サーバー](./media/active-directory-saas-zscaler-one-tutorial/IC769494.png "Proxy server")  

    1.  [LAN にプロキシ サーバーを使用する] をオンにします。  
    2.  [アドレス] ボックスに入力 **gateway.zscalerone.net**します。  
    3.  [ポート] ボックスに入力 **80**します。  
    4.  選択 **ローカル アドレスに対してプロキシ サーバーを使用しない**します。  
    5.  をクリックして **OK** を閉じる、 **ローカル エリア ネットワーク (LAN) 設定** ダイアログ。  

6.  をクリックして **OK** を閉じる、 **インターネット オプション** ダイアログ。  

##ユーザー プロビジョニングの構成

Azure AD ユーザーが ZScaler One にログインできるようにするには、そのユーザーを ZScaler One にプロビジョニングする必要があります。  
 ZScaler One の場合、プロビジョニングは手動で行います。  

###ユーザー プロビジョニングを構成するには、次の手順に従います。

1.  ログインして **Zscaler One** テナントです。  

2.  クリックして **管理**します。  

    ![管理](./media/active-directory-saas-zscaler-one-tutorial/IC781035.png "Administration")  

3.  クリックして **ユーザー管理**します。  

    ![[追加] のいずれかを](./media/active-directory-saas-zscaler-one-tutorial/IC781037.png "Add")  

4.   **ユーザー** ] タブ、[ **追加**します。  

    ![[追加] のいずれかを](./media/active-directory-saas-zscaler-one-tutorial/IC781037.png "Add")  

5.  [ユーザーの追加] セクションで、次の手順を実行します。  

    ![ユーザーの追加](./media/active-directory-saas-zscaler-one-tutorial/IC781038.png "Add User")  

    1.  型、 **UserID**, 、**ユーザー表示名**, 、**パスワード**, 、**パスワードの確認**, 、し、[ **グループ** と **部門** 、プロビジョニングする有効な AAD アカウントのです。  
    2.  クリックして **保存**します。  

>[AZURE.NOTE]他の ZScaler One ユーザー アカウント作成ツールを使用することができますか、AAD ユーザー アカウントをプロビジョニング、ZScaler One 提供の Api です。  

##ユーザーの割り当て

構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。  

###ユーザーを ZScaler One に割り当てるには、次の手順を実行します。

1.  Azure AD ポータルで、テスト アカウントを作成します。  

2.   **ZScaler One** アプリケーション統合ページで、をクリックして **ユーザーの割り当て**します。  

    ![ユーザーの割り当て](./media/active-directory-saas-zscaler-one-tutorial/IC800222.png "Assign Users")  

3.  テスト ユーザーを選択して [ **割り当てる**, 、順にクリック **はい** して割り当てを確認します。  

    ![あり](./media/active-directory-saas-zscaler-one-tutorial/IC767830.png "Yes")  

シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。  

