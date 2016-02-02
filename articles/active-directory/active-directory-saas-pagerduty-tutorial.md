<properties 
    pageTitle="チュートリアル: Azure Active Directory と PagerDuty の統合 | Microsoft Azure" 
    description="Azure Active Directory と Pagerduty を使用して、シングル サインオン、自動化されたプロビジョニング、および詳細を有効にする方法を説明します。" 
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


# チュートリアル: Azure Active Directory と PagerDuty の統合

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
## PagerDuty のアプリケーション統合の有効化

このセクションでは、PagerDuty のアプリケーション統合を有効にする方法を説明します。

### PagerDuty のアプリケーション統合を有効にするには、次の手順に従います。

1.  Azure 管理ポータルの左側のナビゲーション ウィンドウで、**[Active Directory]** をクリックします。

    ![Active Directory](./media/active-directory-saas-pagerduty-tutorial/IC700993.png "Active Directory")

2.  **[ディレクトリ]** の一覧から、ディレクトリ統合を有効にするディレクトリを選択します。

3.  アプリケーション ビューを開くには、ディレクトリ ビューでトップ メニューの **[アプリケーション]** をクリックします。

    ![アプリケーション](./media/active-directory-saas-pagerduty-tutorial/IC700994.png "Applications")

4.  ページの下部にある **[追加]** をクリックします。

    ![アプリケーションの追加](./media/active-directory-saas-pagerduty-tutorial/IC749321.png "Add application")

5.  **[実行する内容]** ダイアログで、**[ギャラリーからアプリケーションを追加します]** をクリックします。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-pagerduty-tutorial/IC749322.png "Add an application from gallerry")

6.  **検索ボックス**に、「**PagerDuty**」と入力します

    ![アプリケーション ギャラリー](./media/active-directory-saas-pagerduty-tutorial/IC778529.png "Application gallery")

7.  結果ウィンドウで **[PagerDuty]** を選び、**[完了]** をクリックしてアプリケーションを追加します。

    ![PagerDuty](./media/active-directory-saas-pagerduty-tutorial/IC778530.png "PagerDuty")
## シングル サインオンの構成

このセクションでは、ユーザーが SAML プロトコルに基づくフェデレーションを使用して、Azure AD でのユーザーのアカウントで PagerDuty に対する認証を行えるようにする方法を説明します。  
この手順の途中で、base-64 でエンコードされた証明書ファイルを作成する必要があります。  
この手順に慣れていない場合は、次を参照してください。 [バイナリ証明書をテキスト ファイルに変換する方法](http://youtu.be/PlgrzUZ-Y1o)します。

### シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **Pagerduty** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、 ** [シングル サインオン ** ダイアログ。

    ![シングル サインオンの構成](./media/active-directory-saas-pagerduty-tutorial/IC778531.png "Configure single sign-on")

2.  **どのようなユーザーの Pagerduty へのアクセス** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、] をクリックし、 **次**します。

    ![シングル サインオンの構成](./media/active-directory-saas-pagerduty-tutorial/IC778532.png "Configure single sign-on")

3.  **アプリケーション URL の構成** ] ページで、 **Pagerduty サインイン URL** ] ボックスに、次のパターンを使用して URL を入力"* https://\<tenant-name\>します。Pagerduty.com*"、クリックして **次**します。

    ![アプリ URL の構成](./media/active-directory-saas-pagerduty-tutorial/IC778533.png "Configure app url")

4.  **Pagerduty でのシングル サインオンの構成** ] ページで [ **証明書のダウンロード**, 、お使いのコンピューターに証明書ファイルを保存します。

    ![シングル サインオンの構成](./media/active-directory-saas-pagerduty-tutorial/IC778534.png "Configure single sign-on")

5.  別の Web ブラウザーのウィンドウで、PagerDuty 企業サイトに管理者としてログインします。

6.  上部のメニューで **[アカウント設定]** をクリックします。

    ![アカウント設定](./media/active-directory-saas-pagerduty-tutorial/IC778535.png "Account Settings")

7.  **[シングル サインオン]** をクリックします。

    ![シングル サインオン](./media/active-directory-saas-pagerduty-tutorial/IC778536.png "Single sign-on")

8.  [シングル サインオンの有効化 (SSO)] ページで、次の手順に従います。

    ![シングル サインオンの有効化](./media/active-directory-saas-pagerduty-tutorial/IC778537.png "Enable single sign-on")

    1.  ダウンロードした証明書から **base-64 でエンコードされた**ファイルを作成します。
        >[AZURE.TIP] 詳細については、「 [バイナリ証明書をテキスト ファイルに変換する方法](http://youtu.be/PlgrzUZ-Y1o)

    2.  base-64 でエンコードされた証明書をメモ帳で開き、その内容をクリップボードにコピーして、**[X.509 Certificate]** テキスト ボックスに貼り付けます。
    3.  Azure ポータルの **[PagerDuty でのシングル サインオンの構成]** ダイアログ ページで、**[リモート ログイン URL]** 値をコピーし、**[ログイン URL]** ボックスに貼り付けます。
    4.  Azure ポータルの **[PagerDuty でのシングル サインオンの構成]** ダイアログ ページで、**[リモート ログアウト URL]** 値をコピーし、**[ログアウト URL]** ボックスに貼り付けます。
    5.  **[シングル サインオンを有効にする]** を選びます。
    6.  **[変更を保存]** をクリックします。

9.  Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![シングル サインオンの構成](./media/active-directory-saas-pagerduty-tutorial/IC778538.png "Configure single sign-on")
## ユーザー プロビジョニングの構成

Azure AD ユーザーが PagerDuty にログインできるようにするには、ユーザーを PagerDuty にプロビジョニングする必要があります。  
PagerDuty の場合、プロビジョニングは手動で行います。

### ユーザー アカウントをプロビジョニングするには、次の手順に従います。

1.  **PagerDuty** テナントにログインします。

2.  上部のメニューで **[ユーザー]** をクリックします。

3.  **[ユーザーの追加]** をクリックします。

    ![ユーザーの追加](./media/active-directory-saas-pagerduty-tutorial/IC778539.png "Add Users")

4.  **[チームを招待]** ダイアログで、プロビジョニングする Azure AD ユーザーの**[氏名]** と **[電子メール]** アドレスを入力し、 **[追加]** 、 **[招待を送信]** の順にクリックします。

    ![チームの招待](./media/active-directory-saas-pagerduty-tutorial/IC778540.png "Invite your team")
    >[AZURE.NOTE] PagerDuty アカウントを作成すると、追加したすべてのユーザーが招待を受信します。

>[AZURE.NOTE] PagerDuty から提供されている他の PagerDuty ユーザー アカウント作成ツールまたは API を使用して、AAD ユーザー アカウントをプロビジョニングできます。

## ユーザーを割り当てる

構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

### ユーザーを PagerDuty に割り当てるには、次の手順に従います。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  **Pagerduty * * アプリケーション統合ページで、をクリックして * * ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-pagerduty-tutorial/IC778541.png "Assign users")

3.  テスト ユーザーを選択し、**[割り当て]**、**[はい]** の順にクリックして、割り当てを確定します。

    ![はい](./media/active-directory-saas-pagerduty-tutorial/IC767830.png "Yes")

シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。




