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


# チュートリアル: Azure Active Directory と Zscaler の統合

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

## Zscaler のアプリケーション統合の有効化

このセクションでは、Zscaler のアプリケーション統合を有効にする方法を説明します。

### Zscaler のアプリケーション統合を有効にするには、次の手順を実行します。

1.  Azure 管理ポータルの左側のナビゲーション ウィンドウで、**[Active Directory]** をクリックします。

    ![Active Directory](./media/active-directory-saas-zscaler-tutorial/IC700993.png "Active Directory")

2.  **[ディレクトリ]** の一覧から、ディレクトリ統合を有効にするディレクトリを選択します。

3.  アプリケーション ビューを開くには、ディレクトリ ビューでトップ メニューの **[アプリケーション]** をクリックします。

    ![アプリケーション](./media/active-directory-saas-zscaler-tutorial/IC700994.png "Applications")

4.  ページの下部にある **[追加]** をクリックします。

    ![アプリケーションの追加](./media/active-directory-saas-zscaler-tutorial/IC749321.png "Add application")

5.  **[実行する内容]** ダイアログで、**[ギャラリーからアプリケーションを追加します]** をクリックします。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-zscaler-tutorial/IC749322.png "Add an application from gallerry")

6.  **検索ボックス**に、「**Zscaler**」と入力します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-zscaler-tutorial/IC769227.png "Application gallery")

7.  結果ウィンドウで **[Zscaler]** を選択し、**[完了]** をクリックしてアプリケーションを追加します。

    ![Zscaler](./media/active-directory-saas-zscaler-tutorial/IC769228.png "Zscaler")

## シングル サインオンの構成

このセクションでは、ユーザーが SAML プロトコルに基づくフェデレーションを使用して、Azure AD でのユーザーのアカウントで Zscaler に対する認証を行うことができるようにする方法を説明します。  
この手順の途中で、証明書を Zscaler にアップロードする必要があります。

### シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **Zscaler** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、 ** [シングル サインオン ** ダイアログ。

    ![シングル サインオンの有効化](./media/active-directory-saas-zscaler-tutorial/IC769229.png "Enable single sign-on")

2.  **どのようなユーザーが Zscaler へのアクセスを** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、] をクリックし、 **次**します。

    ![シングル サインオンの有効化](./media/active-directory-saas-zscaler-tutorial/IC769230.png "Configure single sign on")

3.  **アプリケーション URL の構成** ] ページで、 **Zscaler サインイン URL** ] ボックスは、Zscaler から取得した URL に、記号を入力し、順にクリックして **次**:

    >[AZURE.NOTE] サインインの URL がわからない場合は、Zscaler サポート チームに問い合わせてください。

    ![アプリケーション URL の構成](./media/active-directory-saas-zscaler-tutorial/IC769231.png "Configure app URL")

4.  **Zscaler でのシングル サインオンの構成** ] ページで、次の手順を実行します。

    ![シングル サインオンの構成](./media/active-directory-saas-zscaler-tutorial/IC769232.png "Configure single sign-on")

    1.  クリックして **証明書のダウンロード**, 、としては、証明書ファイルをローカルに保存および **c:\\Zscaler.cer**します。
    2.  **[認証要求 URL]** をクリップボードにコピーします。

5.  Zscaler テナントにログインします。

6.  上部のメニューで **[管理]** をクリックします。

    ![管理](./media/active-directory-saas-zscaler-tutorial/IC769486.png "Administration")

7.  **[管理者とロールの管理]** をクリックし、**[ユーザーと認証の管理]** をクリックします。

    ![管理者とロールの管理](./media/active-directory-saas-zscaler-tutorial/IC769487.png "Manage Administrators & Roles")

8.  **[組織の認証オプションの選択]** セクションで、次の手順を実行します。

    ![認証オプションの選択](./media/active-directory-saas-zscaler-tutorial/IC769488.png "Choose Authentication Options")

    1.  **[SAML シングル サインオンを使用した認証]** を選択します。
    2.  **[SAML シングル サインオン パラメーターの構成]** をクリックします。

9.  **[SAML シングル サインオン パラメーターの構成]** ダイアログ ページで、次の手順に従い、**[完了]** をクリックします。

    ![証明書のアップロード](./media/active-directory-saas-zscaler-tutorial/IC769489.png "Upload certificate")

    1.  **[認証のためにユーザーに表示される SAML ポータルの URL]** テキスト ボックスに、Azure ポータルの **[認証要求 URL]** フィールドの値を貼り付けます。
    2.  **[ログイン名を含む属性]** テキスト ボックスに「**NameID**」と入力します。
    3.  **[SSL パブリック証明書のアップロード]** フィールドで、Azure ポータルからダウンロードした証明書をアップロードします。
    4.  **[SAML 自動プロビジョニングを有効にする]** を選択します。

10. **[ユーザー認証の構成]** ダイアログ ページで、次の手順に従います。

    ![ユーザー認証の構成](./media/active-directory-saas-zscaler-tutorial/IC769490.png "Configure User Authentication")

    1.  **[保存]** をクリックします。
    2.  **[今すぐ認証する]** をクリックします。

11. Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![シングル サインオンの構成](./media/active-directory-saas-zscaler-tutorial/IC769491.png "Configure single sign-on")

## プロキシ設定の構成

### Internet Explorer でプロキシ設定を構成するには

1.  **Internet Explorer** を開始します。

2.  **[ツール]** メニューの **[インターネット オプション]** を選択し、**[インターネット オプション]** ダイアログを開きます。

    ![を開き](./media/active-directory-saas-zscaler-tutorial/IC769492.png "Internet Options")

3.  **[接続]** タブをクリックします。

    ![接続](./media/active-directory-saas-zscaler-tutorial/IC769493.png "Connections")

4.  **[LAN の設定]** をクリックして **[LAN の設定]** ダイアログを開きます。

5.  [プロキシ サーバー] セクションで、次の手順を実行します。

    ![プロキシ サーバー](./media/active-directory-saas-zscaler-tutorial/IC769494.png "Proxy server")

    1.  [LAN にプロキシ サーバーを使用する] をオンにします。
    2.  [アドレス] ボックスに「**gateway.zscalertwo.net**」と入力します。
    3.  [ポート] ボックスに「**80**」と入力します。
    4.  **[ローカル アドレスにはプロキシ サーバーを使用しない]** を選択します。
    5.  **[OK]** をクリックして **[ローカル エリア ネットワーク (LAN) の設定]** ダイアログを閉じます。

6.  **[OK]** をクリックして **[インターネット オプション]** ダイアログを閉じます。

## ユーザー プロビジョニングの構成

Azure AD ユーザーが Zscaler にログインできるようにするには、そのユーザーを Zscaler にプロビジョニングする必要があります。  
Zscaler の場合、プロビジョニングは手動で行います。

### ユーザー プロビジョニングを構成するには、次の手順に従います。

1.  **Zscaler** テナントにログインします。

2.  **[管理]** をクリックします。

    ![管理](./media/active-directory-saas-zscaler-tutorial/IC781035.png "Administration")

3.  **[ユーザー管理]** をクリックします。

    ![ユーザー管理](./media/active-directory-saas-zscaler-tutorial/IC781036.png "User Management")

4.  **[ユーザー]** タブで、**[追加]**.をクリックします。

    ![[追加] のいずれかを](./media/active-directory-saas-zscaler-tutorial/IC781037.png "Add")

5.  [ユーザーの追加] セクションで、次の手順を実行します。

    ![ユーザーの追加](./media/active-directory-saas-zscaler-tutorial/IC781038.png "Add User")

    1.  プロビジョニングする有効な AAD アカウントの **[ユーザー ID]**、**[ユーザー表示名]**、**[パスワード]**、**[パスワードの確認]** に入力し、**[グループ]** と **[部署]** を選択します。
    2.  **[保存]** をクリックします。

>[AZURE.NOTE] Zscaler から提供されている他の Zscaler ユーザー アカウント作成ツールまたは API を使用して、AAD ユーザー アカウントをプロビジョニングできます。

## ユーザーの割り当て

構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

### ユーザーを Zscaler に割り当てるには、次の手順を実行します。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  **Zscaler** アプリケーション統合ページで、をクリックして **ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-zscaler-tutorial/IC769495.png "Assign users")

3.  テスト ユーザーを選択し、**[割り当て]**、**[はい]** の順にクリックして、割り当てを確定します。

    ![はい](./media/active-directory-saas-zscaler-tutorial/IC767830.png "Yes")

シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。





