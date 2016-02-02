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


# チュートリアル: Azure Active Directory と Slack の統合

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

## Slack のアプリケーション統合の有効化

このセクションでは、Slack のアプリケーション統合を有効にする方法について説明します。

### Slack のアプリケーション統合を有効にするには、次の手順を実行します。

1.  Azure 管理ポータルの左側のナビゲーション ウィンドウで、**[Active Directory]** をクリックします。

    ![Active Directory](./media/active-directory-saas-slack-tutorial/IC700993.png "Active Directory")

2.  **[ディレクトリ]** の一覧から、ディレクトリ統合を有効にするディレクトリを選択します。

3.  アプリケーション ビューを開くには、ディレクトリ ビューでトップ メニューの **[アプリケーション]** をクリックします。

    ![アプリケーション](./media/active-directory-saas-slack-tutorial/IC700994.png "Applications")

4.  ページの下部にある **[追加]** をクリックします。

    ![アプリケーションの追加](./media/active-directory-saas-slack-tutorial/IC749321.png "Add application")

5.  **[実行する内容]** ダイアログで、**[ギャラリーからアプリケーションを追加します]** をクリックします。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-slack-tutorial/IC749322.png "Add an application from gallerry")

6.  **検索ボックス**に、「**Slack**」と入力します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-slack-tutorial/IC794981.png "Application Gallery")

7.  結果ウィンドウで **[Slack]** を選択し、**[完了]** をクリックしてアプリケーションを追加します。

    ![シナリオ](./media/active-directory-saas-slack-tutorial/IC796925.png "Scenario")

## シングル サインオンの構成

このセクションでは、SAML プロトコルに基づくフェデレーションを使用して、Slack で Azure AD のユーザー アカウントを使用してユーザーを認証できるようにする方法を説明します。  
この手順の途中で、base-64 でエンコードされた証明書ファイルを作成する必要があります。  
この手順に慣れていない場合は、を参照してください [バイナリ証明書をテキスト ファイルに変換する方法](http://youtu.be/PlgrzUZ-Y1o)

### シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **Slack** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、 ** [シングル サインオン ** ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-slack-tutorial/IC794982.png "Configure Single Sign-On")

2.  **どのようなユーザーが Slack へのサインオン** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、] をクリックし、 **次**します。

    ![Configure Single Sign-On](./media/active-directory-saas-slack-tutorial/IC794983.png "Configure Single Sign-On")

3.  **アプリケーション URL の構成** ] ページで、 **Slack サインイン URL** ] ボックスに、Slack テナントの URL を入力 (例::"*https://azuread.slack.com*")、順にクリック **次**します。

    ![アプリケーション URL の構成](./media/active-directory-saas-slack-tutorial/IC794984.png "Configure App URL")

4.  **Slack でのシングル サインオンの構成** ] ページで、証明書をダウンロードする] をクリックして **証明書のダウンロード**, 、お使いのコンピューターにローカルで証明書ファイルを保存します。

    ![Configure Single Sign-On](./media/active-directory-saas-slack-tutorial/IC794985.png "Configure Single Sign-On")

5.  別の Web ブラウザー ウィンドウで、Slacka 企業サイトに管理者としてログインします。

6.  移動して **Microsoft Azure AD に \ > チームの設定**します。

    ![チーム設定](./media/active-directory-saas-slack-tutorial/IC794986.png "Team Settings")

7.  **[Team Settings]** セクションで、**[Authentication]** タブをクリックし、**[Change Settings]** をクリックします。

    ![チーム設定](./media/active-directory-saas-slack-tutorial/IC794987.png "Team Settings")

8.  **[SAML Authentication Settings]** ダイアログで、次の手順を実行します。

    ![SAML 設定](./media/active-directory-saas-slack-tutorial/IC794988.png "SAML Settings")

    1.  Azure ポータルで、**[Slack でのシングル サインオンの構成]** ダイアログ ページの **[SAML SSO URL]** の値をコピーし、**[SAML 2.0 Endpoint (HTTP)]** テキスト ボックスに貼り付けます。
    2.  Azure ポータルで、**[Slack でのシングル サインオンの構成]** ダイアログ ページの **[発行者の URL]** の値をコピーし、**[Identity Provider Issuer]** テキスト ボックスに貼り付けます。
    3.  ダウンロードした証明書から **base-64 でエンコードされた**ファイルを作成します。
        >[AZURE.TIP] 詳細については、「 [バイナリ証明書をテキスト ファイルに変換する方法](http://youtu.be/PlgrzUZ-Y1o)

    4.  base-64 でエンコードされた証明書をメモ帳で開き、その内容をクリップボードにコピーして、**[Public Certificate]** テキスト ボックスに貼り付けます。
    5.  **[Allow users to change their email address]** を選択解除します。
    6.  **[Allow users to choose their own username]** を選択します。
    7.  **[Authentication for your team must be used by]** で、**[It’s optional]** を選択します。
    8.  **[Save Configuration]** をクリックします。

9.  Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-slack-tutorial/IC794989.png "Configure Single Sign-On")

## ユーザー プロビジョニングの構成

Azure AD ユーザーが Slack にログインできるようにするには、ユーザーを Slack にプロビジョニングする必要があります。

Slack へのユーザー プロビジョニングの構成にあたって必要な操作はありません。  
割り当てられているユーザーが Slack にログインしようとすると、必要に応じて Slack アカウントが自動的に作成されます。

## ユーザーの割り当て

構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

### ユーザーを Slack に割り当てるには、次の手順を実行します。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  **Slack * * アプリケーション統合ページで、をクリックして * * ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-slack-tutorial/IC794990.png "Assign Users")

3.  テスト ユーザーを選択し、**[割り当て]**、**[はい]** の順にクリックして、割り当てを確定します。

    ![はい](./media/active-directory-saas-slack-tutorial/IC767830.png "Yes")

シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。




