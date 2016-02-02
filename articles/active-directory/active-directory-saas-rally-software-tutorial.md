<properties 
    pageTitle="チュートリアル: Azure Active Directory と Rally Software の統合 | Microsoft Azure" 
    description="Azure Active Directory で Rally Software を使用して、シングル サインオンや自動プロビジョニングなどを有効にする方法について説明します。" 
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


# チュートリアル: Azure Active Directory と Rally Software の統合

このチュートリアルでは、Azure と Rally Software の統合について説明します。  
このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   Rally Software テナント

このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  Rally Software のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-rally-software-tutorial/IC769525.png "Scenario")
## Rally Software のアプリケーション統合の有効化

このセクションでは、Rally Software のアプリケーション統合を有効にする方法を説明します。

### Rally Software のアプリケーション統合を有効にするには、次の手順に従います。

1.  Azure 管理ポータルの左側のナビゲーション ウィンドウで、**[Active Directory]** をクリックします。

    ![Active Directory](./media/active-directory-saas-rally-software-tutorial/IC700993.png "Active Directory")

2.  **[ディレクトリ]** の一覧から、ディレクトリ統合を有効にするディレクトリを選択します。

3.  アプリケーション ビューを開くには、ディレクトリ ビューでトップ メニューの **[アプリケーション]** をクリックします。

    ![アプリケーション](./media/active-directory-saas-rally-software-tutorial/IC700994.png "Applications")

4.  ページの下部にある **[追加]** をクリックします。

    ![アプリケーションの追加](./media/active-directory-saas-rally-software-tutorial/IC749321.png "Add application")

5.  **[実行する内容]** ダイアログで、**[ギャラリーからアプリケーションを追加します]** をクリックします。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-rally-software-tutorial/IC749322.png "Add an application from gallerry")

6.  **検索ボックス**に、「**rally**」と入力します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-rally-software-tutorial/IC769526.png "Application gallery")

7.  結果ウィンドウで **[Rally Software]** を選び、**[完了]** をクリックしてアプリケーションを追加します。

    ![Rally Software](./media/active-directory-saas-rally-software-tutorial/IC769527.png "Rally Software")
## シングル サインオンの構成

このセクションでは、ユーザーが SAML プロトコルに基づくフェデレーションを使用して、Azure AD でのユーザーのアカウントで Rally Software に対する認証を行うことができるようにする方法を説明します。  
この手順の途中で、証明書を Rally Software にアップロードする必要があります。

### シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **Rally Software * * アプリケーション統合ページで、] をクリックして * * でのシングル サインオンを構成する** を開くには、 ** [シングル サインオン ** ダイアログ。

    ![シングル サインオンの構成](./media/active-directory-saas-rally-software-tutorial/IC749323.png "Configure single sign-on")

2.  **どのようなユーザーの Rally へ** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、] をクリックし、 **次**します。

    ![Microsoft Azure AD シングル サインオン](./media/active-directory-saas-rally-software-tutorial/IC769528.png "Microsoft Azure AD Single Sign-On")

3.  **アプリケーション URL の構成** ] ページの [、 **Rally Software テナント URL** ] ボックスに、次のパターンを使用して URL を入力"* https://\<tenant-name\>. rally.com*"、順にクリック **次**します。

    ![アプリケーション URL の構成](./media/active-directory-saas-rally-software-tutorial/IC769529.png "Configure App URL")

4.  **Rally でのシングル サインオンの構成** ] ページで [メタデータのダウンロード] をクリックして、コンピューターに保存します。

    ![シングル サインオンの構成](./media/active-directory-saas-rally-software-tutorial/IC769530.png "Configure single sign-on")

5.  **Rally Software** テナントにログインします。

6.  上部にあるツールバーの **[Setup]** をクリックして、**[Subscription]** を選びます。

    ![サブスクリプション](./media/active-directory-saas-rally-software-tutorial/IC769531.png "Subscription")

7.  上部にあるツールバー右側の **[Action]** ボタンをクリックして、**[Edit Subscription]** を選びます。

8.  **[Subscription]** ダイアログ ページで、次の手順に従い、その後 **[Save & Close]** をクリックします。

    ![認証](./media/active-directory-saas-rally-software-tutorial/IC769542.png "Authentication")

    1.  Authentication のドロップダウン リストから、**[Rally or SSO authentication]** を選びます。
    2.  Azure ポータルの **[Rally Software でのシングル サインオンの構成]** ダイアログ ページで、**[ID プロバイダーの ID]** 値をコピーして、**[ID プロバイダーの URL]** テキストボックスに貼り付けます。
    3.  Azure ポータルで、**[Rally Software でのシングル サインオンの構成]** ダイアログ ページの **[リモート ログアウト URL]** の値をコピーします。

9.  Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![シングル サインオンの構成](./media/active-directory-saas-rally-software-tutorial/IC769547.png "Configure single sign-on")
## ユーザー プロビジョニングの構成

AAD ユーザーがサインインできるように、Azure Active Directory ユーザー名を使用して、Rally Software アプリケーションにユーザーをプロビジョニングする必要があります。

### ユーザー プロビジョニングを構成するには、次の手順に従います。

1.  Rally Software テナントにログインします。

2.  移動して **セットアップ \ > ユーザー**, 、クリックして **+ Add New**します。

    ![ユーザー](./media/active-directory-saas-rally-software-tutorial/IC781039.png "Users")

3.  New User テキストボックスに名前を入力し、**[Add with Details]** をクリックします。

4.  **[Create User]** セクションで、次の手順に従います。

    ![Create User](./media/active-directory-saas-rally-software-tutorial/IC781040.png "Create User")

    1.  **[User Name]** ボックスに、プロビジョニングする Azure AD ユーザーの名前を入力します。
    2.  **[Email Address]** ボックスに、プロビジョニングする Azure AD ユーザーのメール アドレスを入力します。
    3.  **[Save & Close]** をクリックします。

>[AZURE.NOTE]他の Rally Software ユーザー アカウントの作成ツールまたは Rally Software から提供されている API を使用して、AAD ユーザー アカウントをプロビジョニングできます。

## ユーザーを割り当てる

構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

### ユーザーを Rally Software に割り当てるには、次の手順に従います。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  **Rally Software** アプリケーション統合ページで、をクリックして **ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-rally-software-tutorial/IC769548.png "Assign users")

3.  テスト ユーザーを選択し、**[割り当て]**、**[はい]** の順にクリックして、割り当てを確定します。

    ![はい](./media/active-directory-saas-rally-software-tutorial/IC767830.png "Yes")

シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。









