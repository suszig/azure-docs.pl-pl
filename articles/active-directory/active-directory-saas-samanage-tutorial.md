<properties 
    pageTitle="チュートリアル: Azure Active Directory と Samanage の統合 | Microsoft Azure" 
    description="Azure Active Directory で Samanage を使用して、シングル サインオンや自動プロビジョニングなどを有効にする方法について説明します。" 
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


# チュートリアル: Azure Active Directory と Samanage の統合

このチュートリアルでは、Azure と Samanage の統合について説明します。  
このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   Samanage テナント

このチュートリアルを完了すると、Samanage に割り当てた Azure AD ユーザーは、Samanage の会社サイト (サービス プロバイダーが開始したサインオン) で、アプリケーションにシングル サインオンまたはできるを使用して、 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。

このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  Samanage のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-samanage-tutorial/IC771705.png "Scenario")
## Samanage のアプリケーション統合の有効化

このセクションでは、Samanage のアプリケーション統合を有効にする方法について説明します。

### Samanage のアプリケーション統合を有効にするには、次の手順に従います。

1.  Azure 管理ポータルの左側のナビゲーション ウィンドウで、**[Active Directory]** をクリックします。

    ![Active Directory](./media/active-directory-saas-samanage-tutorial/IC700993.png "Active Directory")

2.  **[ディレクトリ]** の一覧から、ディレクトリ統合を有効にするディレクトリを選択します。

3.  アプリケーション ビューを開くには、ディレクトリ ビューでトップ メニューの **[アプリケーション]** をクリックします。

    ![アプリケーション](./media/active-directory-saas-samanage-tutorial/IC700994.png "Applications")

4.  ページの下部にある **[追加]** をクリックします。

    ![アプリケーションの追加](./media/active-directory-saas-samanage-tutorial/IC749321.png "Add application")

5.  **[実行する内容]** ダイアログで、**[ギャラリーからアプリケーションを追加します]** をクリックします。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-samanage-tutorial/IC749322.png "Add an application from gallerry")

6.  **検索ボックス**に、「**Samanage**」と入力します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-samanage-tutorial/IC771707.png "Application gallery")

7.  結果ウィンドウで **[Samanage]** を選択し、**[完了]** をクリックしてアプリケーションを追加します。

    ![Samanage](./media/active-directory-saas-samanage-tutorial/IC771708.png "Samanage")
## シングル サインオンの構成

このセクションでは、ユーザーが SAML プロトコルに基づくフェデレーションを使用して、Azure AD でのユーザーのアカウントで Samanage に対する認証を行えるようにする方法について説明します。  
この手順の途中で、base-64 でエンコードされた証明書ファイルを作成する必要があります。  
この手順に慣れていない場合は、次を参照してください。 [バイナリ証明書をテキスト ファイルに変換する方法](http://youtu.be/PlgrzUZ-Y1o)します。

### シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **Samanage** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、 ** [シングル サインオン ** ダイアログ。

    ![シングル サインオンの構成](./media/active-directory-saas-samanage-tutorial/IC771709.png "Configure single sign-on")

2.  **どのようなユーザーの Samanage に** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、] をクリックし、 **次**します。

    ![Microsoft Azure AD シングル サインオン](./media/active-directory-saas-samanage-tutorial/IC771710.png "Microsoft Azure AD Single Sign-On")

3.  **アプリケーション URL の構成** ] ページの [、 **Samanage サインイン URL** ] ボックスに、次のパターンを使用して URL を入力"* https://\<tenant-name\>. samanage.com*"、順にクリック **次**します。

    ![アプリケーション URL の構成](./media/active-directory-saas-samanage-tutorial/IC771711.png "Configure App URL")

4.  **Samanage でのシングル サインオンの構成** ] ページで [ **証明書のダウンロード**, 、お使いのコンピューターに証明書ファイルを保存します。

    ![Configure Single Sign-On](./media/active-directory-saas-samanage-tutorial/IC777613.png "Configure Single Sign-On")

5.  別の Web ブラウザーのウィンドウで、Samanage 企業サイトに管理者としてログインします。

6.  **[Dashboard]** をクリックして、左のナビゲーション ウィンドウで **[Setup]** を選択します。

    ![ダッシュボード](./media/active-directory-saas-samanage-tutorial/IC771712.png "Dashboard")

7.  **[シングル サインオン]** をクリックします。

    ![シングル サインオン](./media/active-directory-saas-samanage-tutorial/IC771713.png "Single Sign-On")

8.  **[Login using SAML]** ダイアログ ページで、次の手順に従ってから、**[Save Changes]** をクリックします。

    1.  クリックして **SAML によるシングル サインオンを有効にする**します。
        ![SAML を使用してログイン](./media/active-directory-saas-samanage-tutorial/IC771719.png "Login using SAML")
    2.  Azure ポータルで、 **Samanage でのシングル サインオンの構成** ] ダイアログ ページでコピー、 **Id プロバイダーの ID** 値に設定して、貼り付けます、 ** Id プロバイダーの URL ** ] ボックスに貼り付けます。
        ![Configure Single Sign-On](./media/active-directory-saas-samanage-tutorial/IC771720.png "Configure Single Sign-On")
    3.  Azure ポータルの **[Samanage でのシングル サインオンの構成]** ダイアログ ページで **[リモート ログイン URL]** の値をコピーして、**[ログイン URL]** テキストボックスに貼り付けます。
    4.  Azure ポータルの **[Samanage でのシングル サインオンの構成]** ダイアログ ページで **[リモート ログアウト URL]** の値をコピーして、**[ログアウト URL]** テキストボックスに貼り付けます。
    5.  ダウンロードした証明書から **base-64 でエンコードされた**ファイルを作成します。
        >[AZURE.TIP] 詳細については、「 [バイナリ証明書をテキスト ファイルに変換する方法](http://youtu.be/PlgrzUZ-Y1o)

    6.  base-64 でエンコードされた証明書をメモ帳で開き、その内容をクリップボードにコピーして、**[X.509 Certificate]** テキスト ボックスに貼り付けます。
    7.  クリックして **Samanage に存在しない場合は、ユーザーを作成する**します。
        ![更新](./media/active-directory-saas-samanage-tutorial/IC771722.png "Update")
    8.  **[Update]** をクリックします。

9.  Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-samanage-tutorial/IC771723.png "Configure Single Sign-On")
## ユーザー プロビジョニングの構成

Azure AD ユーザーが Samanage にログインできるようにするには、そのユーザーを Samanage にプロビジョニングする必要があります。  
Samanage の場合、プロビジョニングは手動で行います。

### ユーザー アカウントをプロビジョニングするには、次の手順に従います。

1.  **Samanage** テナントにログインします。

2.  移動して **ダッシュ ボード \ > セットアップ**します。

    ![セットアップ](./media/active-directory-saas-samanage-tutorial/IC771724.png "Setup")

3.  **[ユーザー]** タブをクリックします。

    ![ユーザー](./media/active-directory-saas-samanage-tutorial/IC771725.png "Users")

4.  **[新しいユーザー]** をクリックします。

    ![新しいユーザー](./media/active-directory-saas-samanage-tutorial/IC771726.png "New User")

5.  プロビジョニングする Azure AD アカウントの**[電子メール]** と **[名前]** を入力し、**[ユーザーの作成]** をクリックします。
    >[AZURE.NOTE]AAD アカウント所有者がメールを受信し、リンクに従ってアカウントを確認するとそのアカウントがアクティブになります。

    ![ユーザーの作成](./media/active-directory-saas-samanage-tutorial/IC771727.png "Creat User")

>[AZURE.NOTE]他の Samanage ユーザー アカウントの作成ツールまたは Samanage から提供されている API を使用して、AAD ユーザー アカウントをプロビジョニングできます。

## ユーザーを割り当てる

構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

### ユーザーを Samanage に割り当てるには、次の手順に従います。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  **Samanage * * アプリケーション統合ページで、をクリックして * * ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-samanage-tutorial/IC771728.png "Assign users")

3.  テスト ユーザーを選択し、**[割り当て]**、**[はい]** の順にクリックして、割り当てを確定します。

    ![はい](./media/active-directory-saas-samanage-tutorial/IC767830.png "Yes")

シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。




