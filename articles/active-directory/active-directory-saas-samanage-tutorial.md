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

#チュートリアル: Azure Active Directory と Samanage の統合
  
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
##Samanage のアプリケーション統合の有効化
  
このセクションでは、Samanage のアプリケーション統合を有効にする方法について説明します。

###Samanage のアプリケーション統合を有効にするには、次の手順に従います。

1.  左側のナビゲーション ウィンドウで、Azure 管理ポータルで、クリックして **Active Directory**します。

    ![Active Directory](./media/active-directory-saas-samanage-tutorial/IC700993.png "Active Directory")

2.   **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3.  ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。

    ![アプリケーション](./media/active-directory-saas-samanage-tutorial/IC700994.png "Applications")

4.  クリックして **追加** ページの下部にあります。

    ![アプリケーションの追加](./media/active-directory-saas-samanage-tutorial/IC749321.png "Add application")

5.   **実行する操作** ダイアログ ボックスで、をクリックして **ギャラリーからアプリケーションを追加**します。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-samanage-tutorial/IC749322.png "Add an application from gallerry")

6.   **検索ボックス**, 、型 **Samanage**します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-samanage-tutorial/IC771707.png "Application gallery")

7.  結果ウィンドウで [ **Samanage**, 、クリックして **完了** アプリケーションを追加します。

    ![Samanage](./media/active-directory-saas-samanage-tutorial/IC771708.png "Samanage")
##シングル サインオンの構成
  
このセクションでは、ユーザーが SAML プロトコルに基づくフェデレーションを使用して、Azure AD でのユーザーのアカウントで Samanage に対する認証を行えるようにする方法について説明します。  
この手順の途中で、base-64 でエンコードされた証明書ファイルを作成する必要があります。  
この手順に慣れていない場合は、次を参照してください。 [バイナリ証明書をテキスト ファイルに変換する方法](http://youtu.be/PlgrzUZ-Y1o)します。

###シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **Samanage** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、* * [シングル サインオン * * ダイアログ。

    ![シングル サインオンの構成](./media/active-directory-saas-samanage-tutorial/IC771709.png "Configure single sign-on")

2.   **どのようなユーザーの Samanage に** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、] をクリックし、 **次**します。

    ![Microsoft Azure AD シングル サインオン](./media/active-directory-saas-samanage-tutorial/IC771710.png "Microsoft Azure AD Single Sign-On")

3.   **アプリケーション URL の構成** ] ページの [、 **Samanage サインイン URL** ] ボックスに、次のパターンを使用して URL を入力"*https://\<tenant-name\>.samanage.com*"、] をクリックし、 **次**します。

    ![Configure App URL](./media/active-directory-saas-samanage-tutorial/IC771711.png "Configure App URL")

4.   **Samanage でのシングル サインオンの構成** ] ページで [ **証明書のダウンロード**, 、お使いのコンピューターに証明書ファイルを保存します。

    ![Configure Single Sign-On](./media/active-directory-saas-samanage-tutorial/IC777613.png "Configure Single Sign-On")

5.  別の Web ブラウザーのウィンドウで、Samanage 企業サイトに管理者としてログインします。

6.  をクリックして **ダッシュ ボード** 選択 **セットアップ** 左側のナビゲーション ウィンドウでします。

    ![ダッシュボード](./media/active-directory-saas-samanage-tutorial/IC771712.png "Dashboard")

7.  クリックして **シングル サインオン**します。

    ![シングル サインオン](./media/active-directory-saas-samanage-tutorial/IC771713.png "Single Sign-On")

8.   **Login using SAML** ] ダイアログ ページで、次の手順を実行し、順にクリックして **Save Changes**:

    1.  クリックして **SAML によるシングル サインオンを有効にする**です。
        ![SAML を使用してログインします。](./media/active-directory-saas-samanage-tutorial/IC771719.png "Login using SAML")
    2.  Azure ポータルで、 **Samanage でのシングル サインオンの構成** ] ダイアログ ページでコピー、 **Id プロバイダーの ID** 値に設定して、貼り付けます、* * Id プロバイダーの URL * *] ボックスに貼り付けます。
        ![シングル サインオンを構成します。](./media/active-directory-saas-samanage-tutorial/IC771720.png "Configure Single Sign-On")
    3.  Azure ポータルで、 **Samanage でのシングル サインオンの構成** ] ダイアログ ページでコピー、 **リモート ログイン URL** 値に設定して、貼り付けます、 **ログイン URL** ] ボックスに貼り付けます。
    4.  Azure ポータルで、 **Samanage でのシングル サインオンの構成** ] ダイアログ ページでコピー、 **リモート ログアウト URL** 値に設定して、貼り付けます、 **ログアウト URL** ] ボックスに貼り付けます。
    5.  作成、 **base-64 でエンコードされた** からダウンロードした証明書ファイル。  

        >[AZURE.TIP] 詳細については、次を参照してください [バイナリ証明書をテキスト ファイルに変換する方法。](http://youtu.be/PlgrzUZ-Y1o)

    6.  Base-64 でエンコードされた証明書をメモ帳で開き、という内容がクリップボードにコピーおよびに貼り付け、 **X.509 証明書** ] テキスト ボックス
    7.  クリックして **Samanage に存在しない場合は、ユーザーを作成**します。
        ![更新プログラム](./media/active-directory-saas-samanage-tutorial/IC771722.png "Update")
    8.  クリックして **更新**します。

9.  Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-samanage-tutorial/IC771723.png "Configure Single Sign-On")
##ユーザー プロビジョニングの構成
  
Azure AD ユーザーが Samanage にログインできるようにするには、そのユーザーを Samanage にプロビジョニングする必要があります。  
Samanage の場合、プロビジョニングは手動で行います。

###ユーザー アカウントをプロビジョニングするには、次の手順を実行します。

1.  ログインして **Samanage** テナントです。

2.  移動して **ダッシュ ボード \ > セットアップ**します。

    ![セットアップ](./media/active-directory-saas-samanage-tutorial/IC771724.png "Setup")

3.  クリックして、 **ユーザー** ] タブ

    ![ユーザー](./media/active-directory-saas-samanage-tutorial/IC771725.png "Users")

4.  クリックして **新しいユーザー**します。

    ![新しいユーザー](./media/active-directory-saas-samanage-tutorial/IC771726.png "New User")

5.  型、 **電子メール アドレス** と **名** 、Azure AD のアカウントをプロビジョニングし、をクリックする **ユーザーの作成**します。

    >[AZURE.NOTE]AAD アカウントの所有者は電子メールを受信し、アクティブ化する前に、自分のアカウントを確認するリンクを参照します。

    ![ユーザーの作成](./media/active-directory-saas-samanage-tutorial/IC771727.png "Creat User")

>[AZURE.NOTE]他の Samanage ユーザー アカウント作成ツールを使用することができますか、AAD ユーザー アカウントをプロビジョニング、Samanage から提供されている Api です。

##ユーザーの割り当て
  
構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

###ユーザーを Samanage に割り当てるには、次の手順に従います。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  * * Samanage * * アプリケーション統合ページで、をクリックして **ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-samanage-tutorial/IC771728.png "Assign users")

3.  テスト ユーザーを選択して [ **割り当てる**, 、順にクリック **はい** して割り当てを確認します。

    ![あり](./media/active-directory-saas-samanage-tutorial/IC767830.png "Yes")
  
シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。
