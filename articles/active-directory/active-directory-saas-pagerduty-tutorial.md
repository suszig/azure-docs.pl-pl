<properties 
    pageTitle="チュートリアル: Azure Active Directory と PagerDuty の統合 | Microsoft Azure" 
    description="Azure Active Directory で Pagerduty を使用して、シングル サインオンや自動プロビジョニングなどを有効にする方法について説明します。" 
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

#チュートリアル: Azure Active Directory と PagerDuty の統合
  
このチュートリアルでは、Azure と PagerDuty の統合について説明します。  
このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   PagerDuty テナント
  
このチュートリアルを完了すると、Pagerduty に割り当てた Azure AD ユーザーは、Pagerduty の会社サイト (サービス プロバイダーが開始したサインオン) で、アプリケーションにシングル サインオンまたはできるを使用して、 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。
  
このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  PagerDuty のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-pagerduty-tutorial/IC778528.png "Scenario")
##PagerDuty のアプリケーション統合の有効化
  
このセクションでは、PagerDuty のアプリケーション統合を有効にする方法を説明します。

###PagerDuty のアプリケーション統合を有効にするには、次の手順に従います。

1.  左側のナビゲーション ウィンドウで、Azure 管理ポータルで、クリックして **Active Directory**します。

    ![Active Directory](./media/active-directory-saas-pagerduty-tutorial/IC700993.png "Active Directory")

2.   **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3.  ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。

    ![アプリケーション](./media/active-directory-saas-pagerduty-tutorial/IC700994.png "Applications")

4.  クリックして **追加** ページの下部にあります。

    ![アプリケーションの追加](./media/active-directory-saas-pagerduty-tutorial/IC749321.png "Add application")

5.   **実行する操作** ダイアログ ボックスで、をクリックして **ギャラリーからアプリケーションを追加**します。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-pagerduty-tutorial/IC749322.png "Add an application from gallerry")

6.   **検索ボックス**, 、型 **Pagerduty**します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-pagerduty-tutorial/IC778529.png "Application gallery")

7.  結果ウィンドウで [ **Pagerduty**, 、クリックして **完了** アプリケーションを追加します。

    ![PagerDuty](./media/active-directory-saas-pagerduty-tutorial/IC778530.png "PagerDuty")
##シングル サインオンの構成
  
このセクションでは、ユーザーが SAML プロトコルに基づくフェデレーションを使用して、Azure AD でのユーザーのアカウントで PagerDuty に対する認証を行えるようにする方法を説明します。  
この手順の途中で、base-64 でエンコードされた証明書ファイルを作成する必要があります。  
この手順に慣れていない場合は、次を参照してください。 [バイナリ証明書をテキスト ファイルに変換する方法](http://youtu.be/PlgrzUZ-Y1o)します。

###シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **Pagerduty** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、* * [シングル サインオン * * ダイアログ。

    ![Configure single sign-on](./media/active-directory-saas-pagerduty-tutorial/IC778531.png "Configure single sign-on")

2.   **どのようなユーザーの Pagerduty へのアクセス** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、] をクリックし、 **次**します。

    ![Configure single sign-on](./media/active-directory-saas-pagerduty-tutorial/IC778532.png "Configure single sign-on")

3.   **アプリケーション URL の構成** ] ページで、 **Pagerduty サインイン URL** ] ボックスに、次のパターンを使用して URL を入力"*https://\<tenant-name\>。Pagerduty.com*"、クリックして **次**します。

    ![アプリケーション url の構成](./media/active-directory-saas-pagerduty-tutorial/IC778533.png "Configure app url")

4.   **Pagerduty でのシングル サインオンの構成** ] ページで [ **証明書のダウンロード**, 、お使いのコンピューターに証明書ファイルを保存します。

    ![Configure single sign-on](./media/active-directory-saas-pagerduty-tutorial/IC778534.png "Configure single sign-on")

5.  別の Web ブラウザーのウィンドウで、PagerDuty 企業サイトに管理者としてログインします。

6.  上部にあるメニュー [ **アカウント設定**します。

    ![Account Settings](./media/active-directory-saas-pagerduty-tutorial/IC778535.png "Account Settings")

7.  クリックして **シングル サインオン**します。

    ![シングル サインオン](./media/active-directory-saas-pagerduty-tutorial/IC778536.png "Single sign-on")

8.  [シングル サインオンの有効化 (SSO)] ページで、次の手順に従います。

    ![Enable single sign-on](./media/active-directory-saas-pagerduty-tutorial/IC778537.png "Enable single sign-on")

    1.  作成、 **base-64 でエンコードされた** からダウンロードした証明書ファイル。  

        >[AZURE.TIP] 詳細については、次を参照してください [バイナリ証明書をテキスト ファイルに変換する方法。](http://youtu.be/PlgrzUZ-Y1o)

    2.  Base-64 でエンコードされた証明書をメモ帳で開き、という内容がクリップボードにコピーおよびに貼り付け、 **X.509 証明書** ] テキスト ボックス
    3.  Azure ポータルで、 **Pagerduty でのシングル サインオンの構成** ] ダイアログ ページでコピー、 **リモート ログイン URL** 値に設定して、貼り付けます、 **ログイン URL** ] ボックスに貼り付けます。
    4.  Azure ポータルで、 **Pagerduty でのシングル サインオンの構成** ] ダイアログ ページでコピー、 **リモート ログアウト URL** 値に設定して、貼り付けます、 **ログアウト URL** ] ボックスに貼り付けます。
    5.  選択 **シングル サインオンが有効に**します。
    6.  クリックして **変更を保存**します。

9.  Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![Configure single sign-on](./media/active-directory-saas-pagerduty-tutorial/IC778538.png "Configure single sign-on")
##ユーザー プロビジョニングの構成
  
Azure AD ユーザーが PagerDuty にログインできるようにするには、ユーザーを PagerDuty にプロビジョニングする必要があります。  
PagerDuty の場合、プロビジョニングは手動で行います。

###ユーザー アカウントをプロビジョニングするには、次の手順を実行します。

1.  ログインして **Pagerduty** テナントです。

2.  上部にあるメニュー [ **ユーザー**します。

3.  クリックして **ユーザーを追加する**です。

    ![ユーザーの追加](./media/active-directory-saas-pagerduty-tutorial/IC778539.png "Add Users")

4.   **チームの招待** ダイアログ ボックスで、型、 **氏名** と **電子メール** プロビジョニング] をクリックする Azure AD ユーザーのアドレス **追加**, 、順にクリック **招待の送信**します。

    ![チームの招待](./media/active-directory-saas-pagerduty-tutorial/IC778540.png "Invite your team")

    >[AZURE.NOTE] すべてのユーザーの PagerDuty アカウントを作成する招待をお届けします。

>[AZURE.NOTE] 他の Pagerduty ユーザー アカウント作成ツールを使用することができますか、AAD ユーザー アカウントをプロビジョニング、Pagerduty から提供されている Api です。

##ユーザーの割り当て
  
構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

###ユーザーを PagerDuty に割り当てるには、次の手順に従います。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  * * Pagerduty * * アプリケーション統合ページで、をクリックして **ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-pagerduty-tutorial/IC778541.png "Assign users")

3.  テスト ユーザーを選択して [ **割り当てる**, 、順にクリック **はい** して割り当てを確認します。

    ![あり](./media/active-directory-saas-pagerduty-tutorial/IC767830.png "Yes")
  
シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。
