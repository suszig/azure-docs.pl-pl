<properties 
    pageTitle="チュートリアル: Azure Active Directory と Sprinklr の統合 | Microsoft Azure" 
    description="Azure Active Directory で Sprinklr を使用して、シングル サインオンや自動プロビジョニングなどを有効にする方法について説明します。" 
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

#チュートリアル: Azure Active Directory と Sprinklr の統合
  
このチュートリアルでは、Azure と Sprinklr の統合について説明します。  
このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   Sprinklr テナント
  
このチュートリアルを完了すると、Sprinklr に割り当てた Azure AD ユーザーは、Sprinklr の会社サイト (サービス プロバイダーが開始したサインオン) で、アプリケーションにシングル サインオンまたはできるを使用して、 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。
  
このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  Sprinklr のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-sprinklr-tutorial/IC782900.png "Scenario")

##Sprinklr のアプリケーション統合の有効化
  
このセクションでは、Sprinklr のアプリケーション統合を有効にする方法について説明します。

###Sprinklr のアプリケーション統合を有効にするには、次の手順に従います。

1.  左側のナビゲーション ウィンドウで、Azure 管理ポータルで、クリックして **Active Directory**します。

    ![Active Directory](./media/active-directory-saas-sprinklr-tutorial/IC700993.png "Active Directory")

2.   **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3.  ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。

    ![アプリケーション](./media/active-directory-saas-sprinklr-tutorial/IC700994.png "Applications")

4.  クリックして **追加** ページの下部にあります。

    ![アプリケーションの追加](./media/active-directory-saas-sprinklr-tutorial/IC749321.png "Add application")

5.   **実行する操作** ダイアログ ボックスで、をクリックして **ギャラリーからアプリケーションを追加**します。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-sprinklr-tutorial/IC749322.png "Add an application from gallerry")

6.   **検索ボックス**, 、型 **Sprinklr**します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-sprinklr-tutorial/IC782901.png "Application Gallery")

7.  結果ウィンドウで [ **Sprinklr**, 、クリックして **完了** アプリケーションを追加します。

    ![Sprinklr](./media/active-directory-saas-sprinklr-tutorial/IC782902.png "Sprinklr")

##シングル サインオンの構成
  
このセクションでは、SAML プロトコルに基づくフェデレーションを使用して、Sprinklr で Azure AD のユーザー アカウントを使用してユーザーを認証できるようにする方法を説明します。  
この手順の途中で、base-64 でエンコードされた証明書ファイルを作成する必要があります。  
この手順に慣れていない場合は、次を参照してください。 [バイナリ証明書をテキスト ファイルに変換する方法](http://youtu.be/PlgrzUZ-Y1o)します。

###シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **Sprinklr** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、* * [シングル サインオン * * ダイアログ。

    ![シングル サインオンの構成](./media/active-directory-saas-sprinklr-tutorial/IC782903.png "Configure single sign-on")

2.   **どのようなユーザーの Sprinklr へのアクセス** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、] をクリックし、 **次**します。

    ![シングル サインオンの構成](./media/active-directory-saas-sprinklr-tutorial/IC782904.png "Configure single sign-on")

3.   **アプリケーション URL の構成** ] ページの [、 **Sprinklr Sign In URL** ] ボックスに、次のパターンを使用して URL を入力"*https://\<tenant-name\>.sprinklr.com*"、] をクリックし、 **次**します。

    ![アプリケーション URL の構成](./media/active-directory-saas-sprinklr-tutorial/IC782905.png "Configure App URL")

4.   **Sprinklr でのシングル サインオンの構成** ] ページで、証明書をダウンロードする] をクリックして **証明書のダウンロード**, 、お使いのコンピューターに証明書ファイルを保存します。

    ![Configure single sign-on](./media/active-directory-saas-sprinklr-tutorial/IC782906.png "Configure single sign-on")

5.  別の Web ブラウザー ウィンドウで、Sprinklr 企業サイトに管理者としてログインします。

6.  移動して **管理 \ > 設定**します。

    ![管理](./media/active-directory-saas-sprinklr-tutorial/IC782907.png "Administration")

7.  移動して **パートナーの管理 \ > シングル サインオン** の左側のペインからです。

    ![パートナーの管理](./media/active-directory-saas-sprinklr-tutorial/IC782908.png "Manage Partner")

8.  クリックして **+ Add Single Sign Ons**します。

    ![シングル サインオン](./media/active-directory-saas-sprinklr-tutorial/IC782909.png "Single Sign-Ons")

9.   **でのシングル サインオン** ] ページで、次の手順を実行します。

    ![シングル サインオン](./media/active-directory-saas-sprinklr-tutorial/IC782910.png "Single Sign-Ons")

    1.   **名前** ] ボックスに、構成の名前を入力 (例:: *WAADSSOTest*)。
    2.  選択 **有効になっている**します。
    3.  選択 **新しい SSO 証明書を使用して**します。
    4.  作成、 **base-64 でエンコードされた** からダウンロードした証明書ファイル。  

        >[AZURE.TIP] 詳細については、次を参照してください [バイナリ証明書をテキスト ファイルに変換する方法。](http://youtu.be/PlgrzUZ-Y1o)

    5.  Base-64 でエンコードされた証明書をメモ帳で開き、という内容がクリップボードにコピーおよびに貼り付け、 **Identity Provider Certificate** ] ボックスに、
    6.  Azure ポータルで、 **Sprinklr でのシングル サインオンの構成** ] ダイアログ ページでコピー、 **Id プロバイダーの ID** 値に設定して、貼り付けます、 **エンティティ Id** ] ボックスに貼り付けます。
    7.  Azure ポータルで、 **Sprinklr でのシングル サインオンの構成** ] ダイアログ ページでコピー、 **リモート ログイン URL** 値に設定して、貼り付けます、 **Identity Provider Login URL** ] ボックスに貼り付けます。
    8.  Azure ポータルで、 **Sprinklr でのシングル サインオンの構成** ] ダイアログ ページでコピー、 **リモート ログアウト URL** 値に設定して、貼り付けます、 **Identity Provider Logout URL** ] ボックスに貼り付けます。
    9.  として **SAML ユーザー ID の種類**, [ **アサーションには、ユーザーが含まれている"sprinklr.com ユーザー名**します。
    10. として **SAML ユーザー ID の所在地**, [ **ユーザー ID は Subject ステートメントの名前 Id 要素に**します。
    11. 閉じる **保存**します。

        ![SAML](./media/active-directory-saas-sprinklr-tutorial/IC782911.png "SAML")

10. Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![Configure single sign-on](./media/active-directory-saas-sprinklr-tutorial/IC782912.png "Configure single sign-on")

##ユーザー プロビジョニングの構成
  
AAD ユーザーがサインインできるようにするには、ユーザーを Sprinklr アプリケーションにプロビジョニングする必要があります。  
このセクションでは、Sprinklr で AAD ユーザー アカウントを作成する方法について説明します。

###ユーザー アカウントを Sprinklr にプロビジョニングするには、次の手順に従います。

1.  Sprinklr 企業サイトに管理者としてログインします。

2.  移動して **管理 \ > 設定**します。

    ![管理](./media/active-directory-saas-sprinklr-tutorial/IC782907.png "Administration")

3.  移動して **クライアントの管理 \ > ユーザー** 左側のペインからです。

    ![[設定]](./media/active-directory-saas-sprinklr-tutorial/IC782914.png "Settings")

4.  クリックして **ユーザーを追加**します。

    ![[設定]](./media/active-directory-saas-sprinklr-tutorial/IC782915.png "Settings")

5.   **編集ユーザー** ] ダイアログ ボックスで、次の手順を実行します。

    ![ユーザーの編集](./media/active-directory-saas-sprinklr-tutorial/IC782916.png "Edit user")

    1.   **電子メール**, 、**名** と **姓** テキスト ボックスは、プロビジョニングする Azure AD ユーザー アカウントの情報を入力します。
    2.  選択 **Password Disabled**します。
    3.  選択、 **言語**です。
    4.  選択、 **ユーザー タイプ**します。
    5.  クリックして **更新**します。

    >[AZURE.IMPORTANT] **パスワードが無効になっている** Id プロバイダーを介してログインするユーザーを選択する必要があります。

6.  移動して **ロール**, 、次の手順を実行します。

    ![パートナーのロール](./media/active-directory-saas-sprinklr-tutorial/IC782917.png "Partner Roles")

    1.   **グローバル** 一覧で、[ **ALL\_Permissions**します。
    2.  クリックして **更新**します。

>[AZURE.NOTE] 他の Sprinklr ユーザー アカウント作成ツールを使用するまたは Azure AD ユーザー アカウントをプロビジョニングする Sprinklr 提供の Api です。

##ユーザーの割り当て
  
構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

###ユーザーを Sprinklr に割り当てるには、次の手順に従います。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  * * Sprinklr * * アプリケーション統合ページで、をクリックして **ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-sprinklr-tutorial/IC782918.png "Assign users")

3.  テスト ユーザーを選択して [ **割り当てる**, 、順にクリック **はい** して割り当てを確認します。

    ![あり](./media/active-directory-saas-sprinklr-tutorial/IC767830.png "Yes")
  
シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。

