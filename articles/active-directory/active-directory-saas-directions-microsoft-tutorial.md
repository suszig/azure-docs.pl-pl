<properties 
    pageTitle="チュートリアル: Azure Active Directory と Directions on Microsoft の統合 | Microsoft Azure" 
    description="Azure Active Directory と Microsoft の方向を使用して、シングル サインオン、自動化されたプロビジョニング、および詳細を有効にする方法を説明します。" 
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


# チュートリアル: Azure Active Directory と Directions on Microsoft の統合

このチュートリアルでは、Azure と Directions on Microsoft の統合について説明します。  
このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   Directions on Microsoft サブスクリプション

フェデレーション Directions on Microsoft サブスクリプションをまだを取得していない場合に電子メールで要求"*service@DirectionsOnMicrosoft.com*"です。

このチュートリアルを完了すると、Directions on Microsoft に割り当てた Azure Active Directory ユーザーは、シングル サインオンを使用して、アプリケーションにシングル サインインできるようになります。

このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  Directions on Microsoft のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-directions-microsoft-tutorial/IC786877.png "Scenario")
## Directions on Microsoft のアプリケーション統合の有効化

このセクションでは、Directions on Microsoft のアプリケーション統合を有効にする方法について説明します。

### Directions on Microsoft のアプリケーション統合を有効にするには、次の手順を実行します。

1.  Azure 管理ポータルの左側のナビゲーション ウィンドウで、**[Active Directory]** をクリックします。

    ![Active Directory](./media/active-directory-saas-directions-microsoft-tutorial/IC700993.png "Active Directory")

2.  **[ディレクトリ]** の一覧から、ディレクトリ統合を有効にするディレクトリを選択します。

3.  アプリケーション ビューを開くには、ディレクトリ ビューでトップ メニューの **[アプリケーション]** をクリックします。

    ![アプリケーション](./media/active-directory-saas-directions-microsoft-tutorial/IC700994.png "Applications")

4.  ページの下部にある **[追加]** をクリックします。

    ![アプリケーションの追加](./media/active-directory-saas-directions-microsoft-tutorial/IC749321.png "Add application")

5.  **[実行する内容]** ダイアログで、**[ギャラリーからアプリケーションを追加します]** をクリックします。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-directions-microsoft-tutorial/IC749322.png "Add an application from gallerry")

6.  **検索ボックス**に、「**Directions on Microsoft**」と入力します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-directions-microsoft-tutorial/IC786878.png "Application Gallery")

7.  結果ウィンドウで **[Directions on Microsoft]** を選択し、**[完了]** をクリックしてアプリケーションを追加します。

    ![シナリオ](./media/active-directory-saas-directions-microsoft-tutorial/IC793922.png "Scenario")
## シングル サインオンの構成

このセクションでは、ユーザーが SAML プロトコルに基づくフェデレーションを使用して、Azure AD でのユーザーのアカウントで Directions on Microsoft に対する認証を行うことができるようにする方法を説明します。

### シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **Directions on Microsoft** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、 ** [シングル サインオン ** ダイアログ。

    ![シングル サインオンを有効にする](./media/active-directory-saas-directions-microsoft-tutorial/IC786879.png "Enable Single Sign-On")

2.  **どのようなユーザーを Directions on Microsoft へのサインオン** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、] をクリックし、 **次**します。

    ![Microsoft Azure AD シングル サインオン](./media/active-directory-saas-directions-microsoft-tutorial/IC786880.png "Microsoft Azure AD Singel Sign-On")

3.  **アプリケーション URL の構成** サインオン URL] ボックスで、型のページ **https://www.directionsonmicrosoft.com/user/login**, 、] をクリックし、 **次**します。

    ![アプリケーション URL の構成](./media/active-directory-saas-directions-microsoft-tutorial/IC786881.png "Configure App URL")

4.  **Directions on Microsoft でのシングル サインオンの構成** ] ページで [ **メタデータのダウンロード**, 、コンピューターのローカルにメタデータ ファイルを保存します。

    ![Configure Single Sign-On](./media/active-directory-saas-directions-microsoft-tutorial/IC786882.png "Configure Single Sign-On")

5.  Directions on Microsoft サポート チームにメタデータ ファイルを送信 (*service@DirectionsOnMicrosoft.com*)。 Directions on Microsoft サポート チームがフェデレーション サイトのメンバーシップを見つけられるように、電子メールには会社の情報を含めます。
    >[AZURE.NOTE] Directions on Microsoft のシングル サインオンは、Directions on Microsoft サポート チームが有効にする必要があります。
    シングル サインオンが有効になると通知が届きます。

6.  Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-directions-microsoft-tutorial/IC786883.png "Configure Single Sign-On")
## ユーザー プロビジョニングの構成

Directions on Microsoft へのユーザー プロビジョニングの構成にあたって必要な操作はありません。  
割り当てられたユーザーがアクセス パネルを使用して Directions on Microsoft にログインしようとすると、そのユーザーが存在するかどうかが Directions on Microsoft によって確認されます。 使用可能なユーザー アカウントがない場合、ユーザー アカウントは自動的に作成されます。
## ユーザーの割り当て

構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

### ユーザーを Directions on Microsoft に割り当てるには、次の手順を実行します。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  **Directions on Microsoft * * アプリケーション統合ページで、をクリックして * * ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-directions-microsoft-tutorial/IC786884.png "Assign users")

3.  テスト ユーザーを選択し、**[割り当て]**、**[はい]** の順にクリックして、割り当てを確定します。

    ![はい](./media/active-directory-saas-directions-microsoft-tutorial/IC767830.png "Yes")





