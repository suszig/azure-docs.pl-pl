<properties 
    pageTitle="チュートリアル: Azure Active Directory と Boomi の統合 | Microsoft Azure" 
    description="Azure Active Directory と Boomi を使用して、シングル サインオン、自動化されたプロビジョニング、および詳細を有効にする方法を説明します。" 
    services="active-directory" 
    authors="MarkusVi"  
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


# チュートリアル: Azure Active Directory と Boomi の統合

このチュートリアルでは、Azure と Boomi の統合について説明します。  
このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   Boomi でのシングル サインオンが有効なサブスクリプション

このチュートリアルを完了すると、Boomi に割り当てた Azure AD ユーザーは、Boomi 会社サイト (サービス プロバイダーが開始したサインオン) で、アプリケーションにシングル サインオンまたはできるを使用して、 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。

このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  Boomi のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-boomi-tutorial/IC791134.png "Scenario")
## Boomi のアプリケーション統合の有効化

このセクションでは、Boomi のアプリケーション統合を有効にする方法について説明します。

### Boomi のアプリケーション統合を有効にするには、次の手順を実行します。

1.  Azure 管理ポータルの左側のナビゲーション ウィンドウで、**[Active Directory]** をクリックします。

    ![Active Directory](./media/active-directory-saas-boomi-tutorial/IC700993.png "Active Directory")

2.  **[ディレクトリ]** の一覧から、ディレクトリ統合を有効にするディレクトリを選択します。

3.  アプリケーション ビューを開くには、ディレクトリ ビューでトップ メニューの **[アプリケーション]** をクリックします。

    ![アプリケーション](./media/active-directory-saas-boomi-tutorial/IC700994.png "Applications")

4.  ページの下部にある **[追加]** をクリックします。

    ![アプリケーションの追加](./media/active-directory-saas-boomi-tutorial/IC749321.png "Add application")

5.  **[実行する内容]** ダイアログで、**[ギャラリーからアプリケーションを追加します]** をクリックします。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-boomi-tutorial/IC749322.png "Add an application from gallerry")

6.  **検索ボックス**に、「**Boomi**」と入力します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-boomi-tutorial/IC790822.png "Application Gallery")

7.  結果ウィンドウで **[Boomi]** を選択し、**[完了]** をクリックしてアプリケーションを追加します。

    ![Boomi](./media/active-directory-saas-boomi-tutorial/IC790823.png "Boomi")
## シングル サインオンの構成

このセクションでは、SAML プロトコルに基づくフェデレーションを使用して、ユーザーが Azure AD のアカウントで Boomi に対する認証を行えるようにする方法を説明します。

### シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **Boomi** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、 ** [シングル サインオン ** ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-boomi-tutorial/IC790824.png "Configure Single Sign-On")

2.  **どのようなユーザーが Boomi へのサインオン** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、] をクリックし、 **次**します。

    ![Configure Single Sign-On](./media/active-directory-saas-boomi-tutorial/IC790825.png "Configure Single Sign-On")

3.  **アプリケーション URL の構成** ] ページの [、 **Boomi 応答 URL** ] ボックスに「、 **Boomi AtomSphere ログイン URL** (例::"*https://platform.boomi.com/sso/AccountName/saml*")、順にクリック **次**します。

    ![アプリケーション URL の構成](./media/active-directory-saas-boomi-tutorial/IC790826.png "Configure App URL")

4.  **Boomi でのシングル サインオンの構成** ] ページで、証明書をダウンロードする] をクリックして **証明書のダウンロード**, 、お使いのコンピューターにローカルで証明書ファイルを保存します。

    ![Configure Single Sign-On](./media/active-directory-saas-boomi-tutorial/IC790827.png "Configure Single Sign-On")

5.  別の Web ブラウザー ウィンドウで、Boomi 企業サイトに管理者としてログインします。

6.  上部のツール バーで会社名をクリックし、**[Setup]** をクリックします。

    ![セットアップ](./media/active-directory-saas-boomi-tutorial/IC790828.png "Setup")

7.  **[SSO Options]** をクリックします。

    ![SSO Options](./media/active-directory-saas-boomi-tutorial/IC790829.png "SSO Options")

8.  **[Single Sign-On Options]** セクションで、次の手順を実行します。

    ![Single Sign-On Options](./media/active-directory-saas-boomi-tutorial/IC790830.png "Single Sign-On Options")

    1.  **[Enable SAML Single Sign-On]** を選択します。
    2.  **[Import]** をクリックして、ダウンロードした証明書をアップロードします。
    3.  Azure ポータルで、**[Boomi でのシングル サインオンの構成]** ダイアログ ページの **[リモート ログイン URL]** の値をコピーし、**[Identity Provider Login URL]** ボックスに貼り付けます。
    4.  **[Federation Id Location]** で、**[Federation Id is in NameID element of the Subject]** を選択します。
    5.  **[保存]** をクリックします。

9.  Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-boomi-tutorial/IC775560.png "Configure Single Sign-On")
## ユーザー プロビジョニングの構成

Azure AD ユーザーが Boomi にログインできるようにするには、ユーザーを Boomi にプロビジョニングする必要があります。  
Boomi の場合、プロビジョニングは手動で行います。

### ユーザー プロビジョニングを構成するには、次の手順に従います。

1.  **Boomi** 企業サイトに管理者としてログインします。

2.  移動して **ユーザー管理 \ > ユーザー**します。

    ![ユーザー](./media/active-directory-saas-boomi-tutorial/IC790831.png "Users")

3.  **[ユーザーの追加]** をクリックします。

    ![ユーザーの追加](./media/active-directory-saas-boomi-tutorial/IC790832.png "Add User")

4.  **[Add User Roles]** ダイアログ ページで、次の手順を実行します。

    ![ユーザーの追加](./media/active-directory-saas-boomi-tutorial/IC790833.png "Add User")

    1.  対応するテキスト ボックスに、プロビジョニングする有効な AAD アカウントの**姓**、**名**、**電子メール アドレス**を入力します。
    2.  [OK] をクリックします。

>[AZURE.NOTE] Boomi から提供されている他の Boomi ユーザー アカウント作成ツールまたは API を使用して、AAD ユーザー アカウントをプロビジョニングできます。

## ユーザーの割り当て

構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、ユーザーにアプリケーションへのアクセス権を付与する必要があります。

### ユーザーを Boomi に割り当てるには、次の手順を実行します。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  **Boomi * * アプリケーション統合ページで、をクリックして * * ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-boomi-tutorial/IC790834.png "Assign Users")

3.  テスト ユーザーを選択し、**[割り当て]**、**[はい]** の順にクリックして、割り当てを確定します。

    ![はい](./media/active-directory-saas-boomi-tutorial/IC767830.png "Yes")

シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。





