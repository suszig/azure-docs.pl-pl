<properties 
    pageTitle="チュートリアル: Azure Active Directory と SimpleNexus の統合 | Microsoft Azure" 
    description="Azure Active Directory で SimpleNexus を使用して、シングル サインオンや自動プロビジョニングなどを有効にする方法について説明します。" 
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


# チュートリアル: Azure Active Directory と SimpleNexus の統合

このチュートリアルの目的は、Azure と SimpleNexus の統合を示すことです。  
このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   SimpleNexus でのシングル サインオンが有効なサブスクリプション

このチュートリアルを完了すると、SimpleNexus に割り当てた Azure AD ユーザーは、SimpleNexus 会社サイト (サービス プロバイダーが開始したサインオン) で、アプリケーションにシングル サインオンまたはできるを使用して、 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。

このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  SimpleNexus のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-simplenexus-tutorial/IC785893.png "Scenario")
## SimpleNexus のアプリケーション統合の有効化

このセクションでは、SimpleNexus のアプリケーション統合を有効にする方法を説明します。

### SimpleNexus のアプリケーション統合を有効にするには、次の手順に従います。

1.  Azure 管理ポータルの左側のナビゲーション ウィンドウで、**[Active Directory]** をクリックします。

    ![Active Directory](./media/active-directory-saas-simplenexus-tutorial/IC700993.png "Active Directory")

2.  **[ディレクトリ]** の一覧から、ディレクトリ統合を有効にするディレクトリを選択します。

3.  アプリケーション ビューを開くには、ディレクトリ ビューでトップ メニューの **[アプリケーション]** をクリックします。

    ![アプリケーション](./media/active-directory-saas-simplenexus-tutorial/IC700994.png "Applications")

4.  ページの下部にある **[追加]** をクリックします。

    ![アプリケーションの追加](./media/active-directory-saas-simplenexus-tutorial/IC749321.png "Add application")

5.  **[実行する内容]** ダイアログで、**[ギャラリーからアプリケーションを追加します]** をクリックします。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-simplenexus-tutorial/IC749322.png "Add an application from gallerry")

6.  **検索ボックス**に、「**simple nexus**」と入力します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-simplenexus-tutorial/IC785894.png "Application Gallery")

7.  結果ウィンドウで **[SimpleNexus]** を選択し、**[完了]** をクリックしてアプリケーションを追加します。

    ![Simple Nexus](./media/active-directory-saas-simplenexus-tutorial/IC809578.png "Simple Nexus")
## シングル サインオンの構成

このセクションでは、SAML プロトコルに基づくフェデレーションを使用して、SimpleNexus で Azure AD のユーザー アカウントを使用してユーザーを認証できるようにする方法を説明します。

### シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **SimpleNexus** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、 ** [シングル サインオン ** ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-simplenexus-tutorial/IC785896.png "Configure Single Sign-On")

2.  **どのようなユーザーの SimpleNexus へのアクセス** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、] をクリックし、 **次**します。

    ![Configure Single Sign-On](./media/active-directory-saas-simplenexus-tutorial/IC785897.png "Configure Single Sign-On")

3.  **アプリケーション URL の構成** ] ページの [、 **SimpleNexus サインイン URL** ] ボックスに、次のパターンを使用して URL を入力"*https://simplenexus.com/CompanyName \_login*"、] をクリックし、 **次**します。

    ![アプリケーション URL の構成](./media/active-directory-saas-simplenexus-tutorial/IC786904.png "Configure App URL")

4.  **SimpleNexus でのシングル サインオンの構成** ] ページで [ **メタデータのダウンロード**, とし、メタデータ ファイルを SimpleNexus サポート チームに転送します。

    ![Configure Single Sign-On](./media/active-directory-saas-simplenexus-tutorial/IC785899.png "Configure Single Sign-On")
    >[AZURE.NOTE] シングル サインオンは SimpleNexus サポート チームが有効にする必要があります。

5.  Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-simplenexus-tutorial/IC785900.png "Configure Single Sign-On")
## ユーザー プロビジョニングの構成

Azure AD ユーザーが SimpleNexus にログインできるようにするには、ユーザーを SimpleNexus にプロビジョニングする必要があります。  
SimpleNexus の場合、プロビジョニングは、テナント管理者が手動で実行するタスクです。
>[AZURE.NOTE] SimpleNexus から提供されている他の SimpleNexus ユーザー アカウント作成ツールまたは API を使用して、AAD ユーザー アカウントをプロビジョニングできます。

## ユーザーの割り当て

構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

### ユーザーを SimpleNexus に割り当てるには、次の手順に従います。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  **SimpleNexus * * アプリケーション統合ページで、をクリックして * * ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-simplenexus-tutorial/IC785901.png "Assign Users")

3.  テスト ユーザーを選択し、**[割り当て]**、**[はい]** の順にクリックして、割り当てを確定します。

    ![はい](./media/active-directory-saas-simplenexus-tutorial/IC767830.png "Yes")

シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。




