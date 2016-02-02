<properties 
    pageTitle="チュートリアル: Azure Active Directory と Greenhouse の統合 | Microsoft Azure" 
    description="Azure Active Directory と Greenhouse を使用して、シングル サインオン、自動化されたプロビジョニング、および詳細を有効にする方法を説明します。" 
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


# チュートリアル: Azure Active Directory と Greenhouse の統合

このチュートリアルでは、Azure と Greenhouse の統合について説明します。  
このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   Greenhouse シングル サインオンでのサブスクリプション

このチュートリアルを完了すると、Greenhouse に割り当てた Azure AD ユーザーは、Greenhouse 会社サイト (サービス プロバイダーが開始したサインオン) で、アプリケーションにシングル サインオンまたはできるを使用して、 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。

このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  Greenhouse のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-greenhouse-tutorial/IC790783.png "Scenario")
## Greenhouse のアプリケーション統合の有効化

このセクションでは、Greenhouse のアプリケーション統合を有効にする方法を説明します。

### Greenhouse のアプリケーション統合を有効にするには、次の手順を実行します。

1.  Azure 管理ポータルの左側のナビゲーション ウィンドウで、**[Active Directory]** をクリックします。

    ![Active Directory](./media/active-directory-saas-greenhouse-tutorial/IC700993.png "Active Directory")

2.  **[ディレクトリ]** の一覧から、ディレクトリ統合を有効にするディレクトリを選択します。

3.  アプリケーション ビューを開くには、ディレクトリ ビューでトップ メニューの **[アプリケーション]** をクリックします。

    ![アプリケーション](./media/active-directory-saas-greenhouse-tutorial/IC700994.png "Applications")

4.  ページの下部にある **[追加]** をクリックします。

    ![アプリケーションの追加](./media/active-directory-saas-greenhouse-tutorial/IC749321.png "Add application")

5.  **[実行する内容]** ダイアログで、**[ギャラリーからアプリケーションを追加します]** をクリックします。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-greenhouse-tutorial/IC749322.png "Add an application from gallerry")

6.  **検索ボックス**に「**greenhouse**」と入力します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-greenhouse-tutorial/IC790784.png "Application Gallery")

7.  結果ウィンドウで **[Greenhouse]** を選択し、**[完了]** をクリックしてアプリケーションを追加します。

    ![Greenhouse](./media/active-directory-saas-greenhouse-tutorial/IC790785.png "Greenhouse")
## シングル サインオンの構成

このセクションでは、ユーザーが SAML プロトコルに基づくフェデレーションを使用して、Azure AD でのユーザーのアカウントで Greenhouse に対する認証を行うことができるようにする方法を説明します。

### シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **Greenhouse** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、 ** [シングル サインオン ** ダイアログ。

    ![シングル サインオンの構成](./media/active-directory-saas-greenhouse-tutorial/IC790786.png "Configure single sign-on")

2.  **どのようなユーザーの Greenhouse に** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、] をクリックし、 **次**します。

    ![シングル サインオンの構成](./media/active-directory-saas-greenhouse-tutorial/IC790787.png "Configure single sign-on")

3.  **アプリケーション URL の構成** ] ページの [、 **サインオン URL** ] ボックスに、次のパターンを使用して URL を入力"*https://company.greenhouse.io*"、] をクリックし、 **次**します。

    ![アプリケーション URL の構成](./media/active-directory-saas-greenhouse-tutorial/IC790788.png "Configure App URL")

4.  **Greenhouse でのシングル サインオンの構成** ] ページで [ **メタデータのダウンロード**, 、コンピューターのローカルにメタデータ ファイルを保存します。

    ![シングル サインオンの構成](./media/active-directory-saas-greenhouse-tutorial/IC790789.png "Configure single sign-on")

5.  このメタデータ ファイルを Greenhous サポート チームに転送します。
    >[AZURE.NOTE] Greenhouse サポート チームがシングル サインオンを有効にする必要があります。

6.  Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![シングル サインオンの構成](./media/active-directory-saas-greenhouse-tutorial/IC790790.png "Configure single sign-on")
## ユーザー プロビジョニングの構成

Azure AD ユーザーが Greenhouse にログインできるようにするには、ユーザーを Greenhouse にプロビジョニングする必要があります。  
Greenhouse の場合、プロビジョニングは手動で行います。

### ユーザー アカウントをプロビジョニングするには、次の手順を実行します。

1.  **Greenhouse** 企業サイトに管理者としてログインします。

2.  上部のメニューで、**[構成]**、**[ユーザー]** の順にクリックします。

    ![ユーザー](./media/active-directory-saas-greenhouse-tutorial/IC790791.png "Users")

3.  **[新しいユーザー]** をクリックします。

    ![新しいユーザー](./media/active-directory-saas-greenhouse-tutorial/IC790792.png "New User")

4.  **[新しいユーザーの追加]** セクションで、次の手順を実行します。

    ![新しいユーザー名の追加](./media/active-directory-saas-greenhouse-tutorial/IC790793.png "Add New User")

    1.  **[ユーザー電子メールの入力]** テキスト ボックスに、プロビジョニングする有効な Azure Active Directory アカウントの電子メール アドレスを入力します。
    2.  **[保存]** をクリックします。
        >[AZURE.NOTE] Azure Active Directory のアカウント所有者には、アカウントがアクティブになる前に、アカウント確認用のリンクを含む電子メールが送信されます。

>[AZURE.NOTE] Greenhouse から提供されている他の Greenhouse ユーザー アカウント作成ツールまたは API を使用して、AAD ユーザー アカウントをプロビジョニングできます。

## ユーザーの割り当て

構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

### ユーザーを Greenhouse に割り当てるには、次の手順を実行します。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  **Greenhouse * * アプリケーション統合ページで、をクリックして * * ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-greenhouse-tutorial/IC790794.png "Assign users")

3.  テスト ユーザーを選択し、**[割り当て]**、**[はい]** の順にクリックして、割り当てを確定します。

    ![はい](./media/active-directory-saas-greenhouse-tutorial/IC767830.png "Yes")

シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。




