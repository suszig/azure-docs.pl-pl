<properties 
    pageTitle="チュートリアル: Azure Active Directory と FM:Systems の統合 | Microsoft Azure" 
    description="FM を使用する方法について説明します。 シングル サインオン、自動化されたプロビジョニング、および詳細を有効にする Azure Active Directory でシステムです。" 
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


# チュートリアル: Azure Active Directory と FM: Systems の統合

このチュートリアルでは、Azure と FM: Systems の統合について説明します。  
このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   FM:Systems でのシングル サインオンが有効なサブスクリプション

このチュートリアルを完了すると、FM:Systems に割り当てた Azure AD ユーザーは、FM:Systems 会社サイト (サービス プロバイダーが開始したサインオン) で、アプリケーションにシングル サインオンまたはできるを使用して、 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。

このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  FM:Systems のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-fm-systems-tutorial/IC795899.png "Scenario")
## FM:Systems のアプリケーション統合の有効化

このセクションでは、FM:Systems のアプリケーション統合を有効にする方法について説明します。

### FM:Systems のアプリケーション統合を有効にするには、次の手順を実行します。

1.  Azure 管理ポータルの左側のナビゲーション ウィンドウで、**[Active Directory]** をクリックします。

    ![Active Directory](./media/active-directory-saas-fm-systems-tutorial/IC700993.png "Active Directory")

2.  **[ディレクトリ]** の一覧から、ディレクトリ統合を有効にするディレクトリを選択します。

3.  アプリケーション ビューを開くには、ディレクトリ ビューでトップ メニューの **[アプリケーション]** をクリックします。

    ![アプリケーション](./media/active-directory-saas-fm-systems-tutorial/IC700994.png "Applications")

4.  ページの下部にある **[追加]** をクリックします。

    ![アプリケーションの追加](./media/active-directory-saas-fm-systems-tutorial/IC749321.png "Add application")

5.  **[実行する内容]** ダイアログで、**[ギャラリーからアプリケーションを追加します]** をクリックします。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-fm-systems-tutorial/IC749322.png "Add an application from gallerry")

6.  **検索ボックス**に、「**FM:Systems**」と入力します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-fm-systems-tutorial/IC795900.png "Application Gallery")

7.  結果ウィンドウで **[FM:Systems]** を選択し、**[完了]** をクリックしてアプリケーションを追加します。

    ![FM: Systems](./media/active-directory-saas-fm-systems-tutorial/IC800213.png "FM: Systems")
## シングル サインオンの構成

このセクションでは、ユーザーが SAML プロトコルに基づくフェデレーションを使用して、Azure AD でのユーザーのアカウントで FM:Systems に対する認証を行うことができるようにする方法を説明します。

### シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **FM:Systems** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、 ** [シングル サインオン ** ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-fm-systems-tutorial/IC790810.png "Configure Single Sign-On")

2.  **どのようなユーザーの FM:Systems へのアクセス** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、] をクリックし、 **次**します。

    ![Configure Single Sign-On](./media/active-directory-saas-fm-systems-tutorial/IC795901.png "Configure Single Sign-On")

3.  **アプリケーション URL の構成** ] ページで、次の手順を実行します。

    ![アプリケーション URL の構成](./media/active-directory-saas-fm-systems-tutorial/IC795902.png "Configure App URL")

    1.  **FM:Systems サインオン URL** ] ボックスに、fm:systems **応答 URL** (例:: *https://dpr.fmshosted.com/fminteract/ConsumerService2.aspx*)。
        >[AZURE.WARNING] この値は、FM: Systems サポート チームから入手できます。

    2.  **[次へ]** をクリックします。

4.  **FM:Systems でのシングル サインオンの構成** ] ページで、メタデータをダウンロードする] をクリックして **メタデータのダウンロード**, 、コンピューターにメタデータを保存します。

    ![Configure Single Sign-On](./media/active-directory-saas-fm-systems-tutorial/IC795903.png "Configure Single Sign-On")

5.  ダウンロードしたメタデータ ファイルを FM: Systems サポート チームに送信します。
    >[AZURE.NOTE] FM: Systems サポート チームが、実際に SSO を構成する必要があります。
    ご使用のサブスクリプションで SSO が有効になると通知されます。

6.  Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-fm-systems-tutorial/IC795904.png "Configure Single Sign-On")
## ユーザー プロビジョニングの構成

Azure AD ユーザーが FM:Systems にログインできるようにするには、そのユーザーを FM:Systems にプロビジョニングする必要があります。  
FM:Systems の場合、プロビジョニングは手動で行います。

### ユーザー プロビジョニングを構成するには、次の手順に従います。

1.  Web ブラウザー ウィンドウで、FM:Systems 企業サイトに管理者としてログインします。

2.  移動して **システム管理 \ > [セキュリティの管理 \ > ユーザー \ > ユーザーの一覧**します。

    ![システム管理](./media/active-directory-saas-fm-systems-tutorial/IC795905.png "System Administration")

3.  **[新しいユーザーの作成]** をクリックします。

    ![新しいユーザーの作成](./media/active-directory-saas-fm-systems-tutorial/IC795906.png "Create New User")

4.  **[Create User]** セクションで、次の手順に従います。

    ![ユーザーの作成](./media/active-directory-saas-fm-systems-tutorial/IC795907.png "Create User")

    1.  関連するテキスト ボックスに、プロビジョニングする有効な Azure Active Directory アカウントのユーザー名、パスワード、確認パスワード、電子メール アドレス、および従業員 ID を入力します。
    2.  **[次へ]** をクリックします。

>[AZURE.NOTE] FM:Systems から提供されている他の FM:Systems ユーザー アカウント作成ツールまたは API を使用して、AAD ユーザー アカウントをプロビジョニングできます。

## ユーザーの割り当て

構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

### ユーザーを FM:Systems に割り当てるには、次の手順を実行します。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  **FM:Systems * * アプリケーション統合ページで、をクリックして * * ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-fm-systems-tutorial/IC795908.png "Assign Users")

3.  テスト ユーザーを選択し、**[割り当て]**、**[はい]** の順にクリックして、割り当てを確定します。

    ![はい](./media/active-directory-saas-fm-systems-tutorial/IC767830.png "Yes")

シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。




