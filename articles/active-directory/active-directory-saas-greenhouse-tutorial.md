<properties 
    pageTitle="チュートリアル: Azure Active Directory と Greenhouse の統合 | Microsoft Azure" 
    description="Azure Active Directory で Greenhouse を使用して、シングル サインオンや自動プロビジョニングなどを有効にする方法について説明します。" 
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

#チュートリアル: Azure Active Directory と Greenhouse の統合
  
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
##Greenhouse のアプリケーション統合の有効化
  
このセクションでは、Greenhouse のアプリケーション統合を有効にする方法を説明します。

###Greenhouse のアプリケーション統合を有効にするには、次の手順を実行します。

1.  左側のナビゲーション ウィンドウで、Azure 管理ポータルで、クリックして **Active Directory**します。

    ![Active Directory](./media/active-directory-saas-greenhouse-tutorial/IC700993.png "Active Directory")

2.   **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3.  ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。

    ![アプリケーション](./media/active-directory-saas-greenhouse-tutorial/IC700994.png "Applications")

4.  クリックして **追加** ページの下部にあります。

    ![アプリケーションの追加](./media/active-directory-saas-greenhouse-tutorial/IC749321.png "Add application")

5.   **実行する操作** ダイアログ ボックスで、をクリックして **ギャラリーからアプリケーションを追加**します。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-greenhouse-tutorial/IC749322.png "Add an application from gallerry")

6.   **検索ボックス**, 、型 **greenhouse**します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-greenhouse-tutorial/IC790784.png "Application Gallery")

7.  結果ウィンドウで [ **Greenhouse**, 、クリックして **完了** アプリケーションを追加します。

    ![Greenhouse](./media/active-directory-saas-greenhouse-tutorial/IC790785.png "Greenhouse")
##シングル サインオンの構成
  
このセクションでは、ユーザーが SAML プロトコルに基づくフェデレーションを使用して、Azure AD でのユーザーのアカウントで Greenhouse に対する認証を行うことができるようにする方法を説明します。

###シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **Greenhouse** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、* * [シングル サインオン * * ダイアログ。

    ![シングル サインオンの構成](./media/active-directory-saas-greenhouse-tutorial/IC790786.png "Configure single sign-on")

2.   **どのようなユーザーの Greenhouse に** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、] をクリックし、 **次**します。

    ![Configure single sign-on](./media/active-directory-saas-greenhouse-tutorial/IC790787.png "Configure single sign-on")

3.   **アプリケーション URL の構成** ] ページの [、 **サインオン URL** ] ボックスに、次のパターンを使用して URL を入力"*https://company.greenhouse.io*"、] をクリックし、 **次**します。

    ![アプリケーション URL の構成](./media/active-directory-saas-greenhouse-tutorial/IC790788.png "Configure App URL")

4.   **Greenhouse でのシングル サインオンの構成** ] ページで [ **メタデータのダウンロード**, 、コンピューターのローカルにメタデータ ファイルを保存します。

    ![Configure single sign-on](./media/active-directory-saas-greenhouse-tutorial/IC790789.png "Configure single sign-on")

5.  このメタデータ ファイルを Greenhous サポート チームに転送します。

    >[AZURE.NOTE] シングル サインオンの Greenhouse サポート チームを有効にするには

6.  Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![Configure single sign-on](./media/active-directory-saas-greenhouse-tutorial/IC790790.png "Configure single sign-on")
##ユーザー プロビジョニングの構成
  
Azure AD ユーザーが Greenhouse にログインできるようにするには、ユーザーを Greenhouse にプロビジョニングする必要があります。  
Greenhouse の場合、プロビジョニングは手動で行います。

###ユーザー アカウントをプロビジョニングするには、次の手順を実行します。

1.  ログインして **Greenhouse** 会社サイトに管理者として。

2.  上部にあるメニュー、[ **構成**, 、クリックして **ユーザー**します。

    ![ユーザー](./media/active-directory-saas-greenhouse-tutorial/IC790791.png "Users")

3.  クリックして **新規ユーザー**します。

    ![新しいユーザー](./media/active-directory-saas-greenhouse-tutorial/IC790792.png "New User")

4.   **Add New User** セクションで、次の手順に従います。

    ![新しいユーザー名の追加](./media/active-directory-saas-greenhouse-tutorial/IC790793.png "Add New User")

    1.   **ユーザーの電子メールを入力して** ] ボックスに、プロビジョニングする有効な Azure Active Directory アカウントの電子メール アドレスを入力します。
    2.  クリックして **保存**します。
        
        >[AZURE.NOTE] Azure Active Directory アカウント保有者がアクティブ化する前に、アカウントの確認へのリンクが記載された電子メールを受け取ります。

>[AZURE.NOTE] 他の Greenhouse ユーザー アカウント作成ツールを使用することができますか、AAD ユーザー アカウントをプロビジョニング、Greenhouse から提供されている Api です。

##ユーザーの割り当て
  
構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

###ユーザーを Greenhouse に割り当てるには、次の手順を実行します。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  * * Greenhouse * * アプリケーション統合ページで、をクリックして **ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-greenhouse-tutorial/IC790794.png "Assign users")

3.  テスト ユーザーを選択して [ **割り当てる**, 、順にクリック **はい** して割り当てを確認します。

    ![あり](./media/active-directory-saas-greenhouse-tutorial/IC767830.png "Yes")
  
シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。

