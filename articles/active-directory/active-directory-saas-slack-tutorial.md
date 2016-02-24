<properties 
    pageTitle="チュートリアル: Azure Active Directory と Slack の統合 | Microsoft Azure" 
    description="Azure Active Directory で Slack を使用して、シングル サインオンや自動プロビジョニングなどを有効にする方法について説明します。" 
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

#チュートリアル: Azure Active Directory と Slack の統合
  
このチュートリアルでは、Azure と Slack の統合について説明します。  
このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   Slack でのシングル サインオンが有効なサブスクリプション
  
このチュートリアルを完了すると、Slack に割り当てた Azure AD ユーザーは、Slack 会社サイト (サービス プロバイダーが開始したサインオン) で、アプリケーションにシングル サインオンまたはできるを使用して、 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。
  
このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  Slack のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-slack-tutorial/IC794980.png "Scenario")

##Slack のアプリケーション統合の有効化
  
このセクションでは、Slack のアプリケーション統合を有効にする方法について説明します。

###Slack のアプリケーション統合を有効にするには、次の手順を実行します。

1.  左側のナビゲーション ウィンドウで、Azure 管理ポータルで、クリックして **Active Directory**します。

    ![Active Directory](./media/active-directory-saas-slack-tutorial/IC700993.png "Active Directory")

2.   **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3.  ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。

    ![アプリケーション](./media/active-directory-saas-slack-tutorial/IC700994.png "Applications")

4.  クリックして **追加** ページの下部にあります。

    ![アプリケーションの追加](./media/active-directory-saas-slack-tutorial/IC749321.png "Add application")

5.   **実行する操作** ダイアログ ボックスで、をクリックして **ギャラリーからアプリケーションを追加**します。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-slack-tutorial/IC749322.png "Add an application from gallerry")

6.   **検索ボックス**, 、型 **Slack**します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-slack-tutorial/IC794981.png "Application Gallery")

7.  結果ウィンドウで [ **Slack**, 、] をクリックし、 **完了** アプリケーションを追加します。

    ![シナリオ](./media/active-directory-saas-slack-tutorial/IC796925.png "Scenario")

##シングル サインオンの構成
  
このセクションでは、SAML プロトコルに基づくフェデレーションを使用して、Slack で Azure AD のユーザー アカウントを使用してユーザーを認証できるようにする方法を説明します。  
この手順の途中で、base-64 でエンコードされた証明書ファイルを作成する必要があります。  
この手順に慣れていない場合は、次を参照してください [バイナリ証明書をテキスト ファイルに変換する方法。](http://youtu.be/PlgrzUZ-Y1o)

###シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **Slack** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、* * [シングル サインオン * * ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-slack-tutorial/IC794982.png "Configure Single Sign-On")

2.   **どのようなユーザーが Slack へのサインオン** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、] をクリックし、 **次**します。

    ![Configure Single Sign-On](./media/active-directory-saas-slack-tutorial/IC794983.png "Configure Single Sign-On")

3.   **アプリケーション URL の構成** ] ページで、 **Slack サインイン URL** ] ボックスに、Slack テナントの URL を入力 (例::"*https://azuread.slack.com*")、順にクリック **次**します。

    ![アプリケーション URL の構成](./media/active-directory-saas-slack-tutorial/IC794984.png "Configure App URL")

4.   **Slack でのシングル サインオンの構成** ] ページで、証明書をダウンロードする] をクリックして **証明書のダウンロード**, 、お使いのコンピューターにローカルで証明書ファイルを保存します。

    ![Configure Single Sign-On](./media/active-directory-saas-slack-tutorial/IC794985.png "Configure Single Sign-On")

5.  別の Web ブラウザー ウィンドウで、Slacka 企業サイトに管理者としてログインします。

6.  移動して **Microsoft Azure AD に \ > チームの設定**します。

    ![チーム設定](./media/active-directory-saas-slack-tutorial/IC794986.png "Team Settings")

7.   **チーム設定** ] をクリックして、 **認証** タブをクリックし、をクリックし、 **設定の変更**します。

    ![チーム設定](./media/active-directory-saas-slack-tutorial/IC794987.png "Team Settings")

8.   **SAML 認証設定** ] ダイアログ ボックスで、次の手順を実行します。

    ![SAML 設定](./media/active-directory-saas-slack-tutorial/IC794988.png "SAML Settings")

    1.  Azure ポータルで、 **Slack でのシングル サインオンの構成** ] ダイアログ ページでコピー、 **SAML SSO URL** 値に設定して、貼り付けます、 **SAML 2.0 エンドポイント (HTTP)** ] ボックスに貼り付けます。
    2.  Azure ポータルでの **Slack でのシングル サインオンの構成** ] ダイアログ ページでコピー、 **発行者 URL** 値に設定して、貼り付けます、 **Id プロバイダー発行者** ] ボックスに貼り付けます。
    3.  作成、 **base-64 でエンコードされた** からダウンロードした証明書ファイル。
    
        >[AZURE.TIP] 詳細については、次を参照してください [バイナリ証明書をテキスト ファイルに変換する方法。](http://youtu.be/PlgrzUZ-Y1o)

    4.  Base-64 でエンコードされた証明書をメモ帳で開き、という内容がクリップボードにコピーおよびに貼り付け、 **公開証明書** ] ボックスに貼り付けます。
    5.  選択を解除 **ユーザーの電子メール アドレスを変更できるように**します。
    6.  選択 **独自のユーザー名を選択できるように**します。
    7.  として **するチーム用の認証を使用する必要があります**, [ **は省略可能**します。
    8.  クリックして **構成を保存**します。

9.  Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-slack-tutorial/IC794989.png "Configure Single Sign-On")

##ユーザー プロビジョニングの構成
  
Azure AD ユーザーが Slack にログインできるようにするには、ユーザーを Slack にプロビジョニングする必要があります。
  
Slack へのユーザー プロビジョニングの構成にあたって必要な操作はありません。  
割り当てられているユーザーが Slack にログインしようとすると、必要に応じて Slack アカウントが自動的に作成されます。

##ユーザーの割り当て
  
構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

###ユーザーを Slack に割り当てるには、次の手順を実行します。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  * * Slack * * アプリケーション統合ページで、をクリックして **ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-slack-tutorial/IC794990.png "Assign Users")

3.  テスト ユーザーを選択して [ **割り当てる**, 、順にクリック **はい** して割り当てを確認します。

    ![あり](./media/active-directory-saas-slack-tutorial/IC767830.png "Yes")
  
シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。
