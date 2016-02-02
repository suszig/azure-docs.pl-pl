<properties 
    pageTitle="チュートリアル: Azure Active Directory と RightAnswers の統合 | Microsoft Azure" 
    description="Azure Active Directory で RightAnswers を使用して、シングル サインオンや自動プロビジョニングなどを有効にする方法について説明します。" 
    services="active-directory" 
    authors="markusvi"  
    documentationCenter="na" manager="stevenpo"/>
<tags 
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="10/22/2015" 
    ms.author="markvi" />


# チュートリアル: Azure Active Directory と RightAnswers の統合

このチュートリアルでは、Azure と RightAnswers の統合について説明します。 このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   RightAnswers でのシングル サインオンが有効なサブスクリプション

このチュートリアルを完了すると、RightAnswers に割り当てた Azure AD ユーザーができるようにアプリケーションを使用して、シングル サインオン、 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。

このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  RightAnswers のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-rightanswers-tutorial/IC802925.png "Scenario")
## RightAnswers のアプリケーション統合の有効化

このセクションでは、RightAnswers のアプリケーション統合を有効にする方法を説明します。

### RightAnswers のアプリケーション統合を有効にするには、次の手順に従います。

1.  Azure 管理ポータルの左側のナビゲーション ウィンドウで、**[Active Directory]** をクリックします。

    ![Active Directory](./media/active-directory-saas-rightanswers-tutorial/IC700993.png "Active Directory")

2.  **[ディレクトリ]** の一覧から、ディレクトリ統合を有効にするディレクトリを選択します。

3.  アプリケーション ビューを開くには、ディレクトリ ビューでトップ メニューの **[アプリケーション]** をクリックします。

    ![アプリケーション](./media/active-directory-saas-rightanswers-tutorial/IC700994.png "Applications")

4.  ページの下部にある **[追加]** をクリックします。

    ![アプリケーションの追加](./media/active-directory-saas-rightanswers-tutorial/IC749321.png "Add application")

5.  **[実行する内容]** ダイアログで、**[ギャラリーからアプリケーションを追加します]** をクリックします。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-rightanswers-tutorial/IC749322.png "Add an application from gallerry")

6.  **検索ボックス**に、「**RightAnswers**」と入力します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-rightanswers-tutorial/IC802926.png "Application Gallery")

7.  結果ウィンドウで **[RightAnswers]** を選び、**[完了]** をクリックしてアプリケーションを追加します。
## シングル サインオンの構成

このセクションでは、ユーザーが SAML プロトコルに基づくフェデレーションを使用して、Azure AD でのユーザーのアカウントで RightAnswers に対する認証を行えるようにする方法を説明します。

### シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **RightAnswers** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、 ** [シングル サインオン ** ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-rightanswers-tutorial/IC802927.png "Configure Single Sign-On")

2.  **どのような RightAnswers にサインオン** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、順にクリック **次**します。

    ![Configure Single Sign-On](./media/active-directory-saas-rightanswers-tutorial/IC802928.png "Configure Single Sign-On")

3.  **アプリケーション設定の構成** ] ページの [、 **サインオン URL** ユーザー RightAnswers アプリケーションにサインオンする URL を使用] ボックスに、型 (例:: *https://fortify.rightanswers.com/portal/ss/*)、順にクリック **次**します。

    ![Configure App Settings](./media/active-directory-saas-rightanswers-tutorial/IC802929.png "Configure App Settings")

4.  **RightAnswers でのシングル サインオンの構成** ] ページで、メタデータをダウンロードする] をクリックして **メタデータのダウンロード**, 、コンピューターのローカルにメタデータ ファイルを保存します。

    ![Configure Single Sign-On](./media/active-directory-saas-rightanswers-tutorial/IC802930.png "Configure Single Sign-On")

5.  ダウンロードしたメタデータ ファイルを RightAnswers サポート チームに送信します。
    >[AZURE.NOTE] RightAnswers サポート チームが、実際に SSO を構成する必要があります。
    ご使用のサブスクリプションで SSO が有効になると通知されます。

6.  Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-rightanswers-tutorial/IC802931.png "Configure Single Sign-On")
## ユーザー プロビジョニングの構成

Azure AD ユーザーが RightAnswers にログインできるようにするには、ユーザーを RightAnswers にプロビジョニングする必要があります。  
RightAnswers の場合、プロビジョニングは自動化されています。  
アイテムを操作することはありません。

最初のシングル サインオンの試行中に、必要に応じてユーザーが自動的に作成されます。
>[AZURE.NOTE]RightAnswers から提供されている他の RightAnswers ユーザー アカウント作成ツールまたは API を使用して、AAD ユーザー アカウントをプロビジョニングできます。

## ユーザーを割り当てる

構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

### ユーザーを RightAnswers に割り当てるには、次の手順に従います。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  **RightAnswers * * アプリケーション統合ページで、をクリックして * * ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-rightanswers-tutorial/IC802932.png "Assign Users")

3.  テスト ユーザーを選択し、**[割り当て]**、**[はい]** の順にクリックして、割り当てを確定します。

    ![はい](./media/active-directory-saas-rightanswers-tutorial/IC767830.png "Yes")

シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。




