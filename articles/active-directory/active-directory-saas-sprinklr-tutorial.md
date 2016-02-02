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


# チュートリアル: Azure Active Directory と Sprinklr の統合

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

## Sprinklr のアプリケーション統合の有効化

このセクションでは、Sprinklr のアプリケーション統合を有効にする方法について説明します。

### Sprinklr のアプリケーション統合を有効にするには、次の手順に従います。

1.  Azure 管理ポータルの左側のナビゲーション ウィンドウで、**[Active Directory]** をクリックします。

    ![Active Directory](./media/active-directory-saas-sprinklr-tutorial/IC700993.png "Active Directory")

2.  **[ディレクトリ]** の一覧から、ディレクトリ統合を有効にするディレクトリを選択します。

3.  アプリケーション ビューを開くには、ディレクトリ ビューでトップ メニューの **[アプリケーション]** をクリックします。

    ![アプリケーション](./media/active-directory-saas-sprinklr-tutorial/IC700994.png "Applications")

4.  ページの下部にある **[追加]** をクリックします。

    ![アプリケーションの追加](./media/active-directory-saas-sprinklr-tutorial/IC749321.png "Add application")

5.  **[実行する内容]** ダイアログで、**[ギャラリーからアプリケーションを追加します]** をクリックします。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-sprinklr-tutorial/IC749322.png "Add an application from gallerry")

6.  **検索ボックス**に、「**Sprinklr**」と入力します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-sprinklr-tutorial/IC782901.png "Application Gallery")

7.  結果ウィンドウで **[Sprinklr]** を選択し、**[完了]** をクリックしてアプリケーションを追加します。

    ![Sprinklr](./media/active-directory-saas-sprinklr-tutorial/IC782902.png "Sprinklr")

## シングル サインオンの構成

このセクションでは、SAML プロトコルに基づくフェデレーションを使用して、Sprinklr で Azure AD のユーザー アカウントを使用してユーザーを認証できるようにする方法を説明します。  
この手順の途中で、base-64 でエンコードされた証明書ファイルを作成する必要があります。  
この手順に慣れていない場合は、次を参照してください。 [バイナリ証明書をテキスト ファイルに変換する方法](http://youtu.be/PlgrzUZ-Y1o)します。

### シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **Sprinklr** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、 ** [シングル サインオン ** ダイアログ。

    ![シングル サインオンの構成](./media/active-directory-saas-sprinklr-tutorial/IC782903.png "Configure single sign-on")

2.  **どのようなユーザーの Sprinklr へのアクセス** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、] をクリックし、 **次**します。

    ![シングル サインオンの構成](./media/active-directory-saas-sprinklr-tutorial/IC782904.png "Configure single sign-on")

3.  **アプリケーション URL の構成** ] ページの [、 **Sprinklr Sign In URL** ] ボックスに、次のパターンを使用して URL を入力"* https://\<tenant-name\>. sprinklr.com*"、順にクリック **次**します。

    ![アプリケーション URL の構成](./media/active-directory-saas-sprinklr-tutorial/IC782905.png "Configure App URL")

4.  **Sprinklr でのシングル サインオンの構成** ] ページで、証明書をダウンロードする] をクリックして **証明書のダウンロード**, 、お使いのコンピューターに証明書ファイルを保存します。

    ![シングル サインオンの構成](./media/active-directory-saas-sprinklr-tutorial/IC782906.png "Configure single sign-on")

5.  別の Web ブラウザー ウィンドウで、Sprinklr 企業サイトに管理者としてログインします。

6.  移動して **管理 \ > 設定**します。

    ![管理](./media/active-directory-saas-sprinklr-tutorial/IC782907.png "Administration")

7.  移動して **パートナーの管理 \ > シングル サインオン** の左側のペインからです。

    ![パートナーの管理](./media/active-directory-saas-sprinklr-tutorial/IC782908.png "Manage Partner")

8.  **[+Add Single Sign Ons]** をクリックします。

    ![シングル サインオン](./media/active-directory-saas-sprinklr-tutorial/IC782909.png "Single Sign-Ons")

9.  **[Single Sign on]** ページで、次の手順に従います。

    ![シングル サインオン](./media/active-directory-saas-sprinklr-tutorial/IC782910.png "Single Sign-Ons")

    1.  **[Name]** テキスト ボックスに、構成の名前を入力します (例: *WAADSSOTest*)。
    2.  **[Enabled]** を選択します。
    3.  **[Use new SSO Certificate]** を選択します。
    4.  ダウンロードした証明書から **base-64 でエンコードされた**ファイルを作成します。
        >[AZURE.TIP] 詳細については、「 [バイナリ証明書をテキスト ファイルに変換する方法](http://youtu.be/PlgrzUZ-Y1o)

    5.  base-64 でエンコードされた証明書をノートパッドで開き、その内容をクリップボードにコピーして、**[ID プロバイダー証明書]** テキスト ボックスに貼り付けます。
    6.  Azure ポータルの **[Sprinklr でのシングル サインオンの構成]** ダイアログ ページで、**ID プロバイダーの ID** の値をコピーし、**[Entity Id]** ボックスに貼り付けます。
    7.  Azure ポータルで、**[Sprinklr でのシングル サインオンの構成]** ダイアログ ページの **[リモート ログイン URL]** の値をコピーし、**[Identity Provider Login URL]** テキスト ボックスに貼り付けます。
    8.  Azure ポータルで、**[Sprinklr でのシングル サインオンの構成]** ダイアログ ページの **[リモート ログアウト URL]** の値をコピーし、**[Identity Provider Logout URL]** テキスト ボックスに貼り付けます。
    9.  **[SAML User ID Type]** として**[Assertion contains User”s sprinklr.com username]** を選択します。
    10. **[SAML User ID Location]** として **[User ID is in the Name Identifier element of the Subject statement]** を選択します。
    11. **[Save]** をクリックします。

        ![SAML](./media/active-directory-saas-sprinklr-tutorial/IC782911.png "SAML")

10. Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![シングル サインオンの構成](./media/active-directory-saas-sprinklr-tutorial/IC782912.png "Configure single sign-on")

## ユーザー プロビジョニングの構成

AAD ユーザーがサインインできるようにするには、ユーザーを Sprinklr アプリケーションにプロビジョニングする必要があります。  
このセクションでは、Sprinklr で AAD ユーザー アカウントを作成する方法について説明します。

### ユーザー アカウントを Sprinklr にプロビジョニングするには、次の手順に従います。

1.  Sprinklr 企業サイトに管理者としてログインします。

2.  移動して **管理 \ > 設定**します。

    ![管理](./media/active-directory-saas-sprinklr-tutorial/IC782907.png "Administration")

3.  移動して **クライアントの管理 \ > ユーザー** 左側のペインからです。

    ![設定](./media/active-directory-saas-sprinklr-tutorial/IC782914.png "Settings")

4.  **[ユーザーの追加]** をクリックします。

    ![設定](./media/active-directory-saas-sprinklr-tutorial/IC782915.png "Settings")

5.  **[Edit user]** ダイアログで、次の手順に従います。

    ![ユーザーの編集](./media/active-directory-saas-sprinklr-tutorial/IC782916.png "Edit user")

    1.  **[Email]**、**[First Name]**、および **[Last Name]** テキスト ボックスに、プロビジョニングする Azure AD のユーザー アカウントの情報を入力します。
    2.  **[Password Disabled]** を選択します。
    3.  **[Language]** を選択します。
    4.  **[User Type]** を選択します。
    5.  **[Update]** をクリックします。

    >[AZURE.IMPORTANT] ユーザーが ID プロバイダー経由でログインできるようにするには、**[Password Disabled]** を選択する必要があります。

6.  **[Role]** に移動して、次の手順に従います。

    ![パートナーのロール](./media/active-directory-saas-sprinklr-tutorial/IC782917.png "Partner Roles")

    1.  **グローバル** 一覧で、[ **ALL\_Permissions**します。
    2.  **[Update]** を選択します。

>[AZURE.NOTE] Sprinklr から提供されている他の Sprinklr ユーザー アカウント作成ツールまたは API を使用して、Azure AD ユーザー アカウントをプロビジョニングできます。

## ユーザーの割り当て

構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

### ユーザーを Sprinklr に割り当てるには、次の手順に従います。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  **Sprinklr * * アプリケーション統合ページで、をクリックして * * ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-sprinklr-tutorial/IC782918.png "Assign users")

3.  テスト ユーザーを選択し、**[割り当て]**、**[はい]** の順にクリックして、割り当てを確定します。

    ![はい](./media/active-directory-saas-sprinklr-tutorial/IC767830.png "Yes")

シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。




