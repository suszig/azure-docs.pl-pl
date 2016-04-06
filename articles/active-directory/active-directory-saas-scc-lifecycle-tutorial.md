<properties 
    pageTitle="チュートリアル: Azure Active Directory と SCC LifeCycle の統合 | Microsoft Azure" 
    description="Azure Active Directory で SCC LifeCycle を使用して、シングル サインオンや自動プロビジョニングなどを有効にする方法について説明します。" 
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

#チュートリアル: Azure Active Directory と SCC LifeCycle の統合
  
このチュートリアルでは、Azure と SCC LifeCycle の統合について説明します。  
このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   SCC LifeCycle でのシングル サインオンが有効なサブスクリプション
  
このチュートリアルを完了すると、SCC LifeCycle に割り当てた Azure AD ユーザーは、SCC LifeCycle 会社サイト (サービス プロバイダーが開始したサインオン) で、アプリケーションにシングル サインオンまたはできるを使用して、 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。
  
このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  SCC LifeCycle のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-scc-lifecycle-tutorial/IC794120.png "Scenario")
##SCC LifeCycle のアプリケーション統合の有効化
  
このセクションでは、SCC LifeCycle のアプリケーション統合を有効にする方法を説明します。

###SCC LifeCycle のアプリケーション統合を有効にするには、次の手順に従います。

1.  左側のナビゲーション ウィンドウで、Azure 管理ポータルで、クリックして **Active Directory**します。

    ![Active Directory](./media/active-directory-saas-scc-lifecycle-tutorial/IC700993.png "Active Directory")

2.   **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3.  ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。

    ![アプリケーション](./media/active-directory-saas-scc-lifecycle-tutorial/IC700994.png "Applications")

4.  クリックして **追加** ページの下部にあります。

    ![アプリケーションの追加](./media/active-directory-saas-scc-lifecycle-tutorial/IC749321.png "Add application")

5.   **実行する操作** ダイアログ ボックスで、をクリックして **ギャラリーからアプリケーションを追加**します。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-scc-lifecycle-tutorial/IC749322.png "Add an application from gallerry")

6.   **検索ボックス**, 、型 **SCC LifeCycle**します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-scc-lifecycle-tutorial/IC794121.png "Application Gallery")

7.  結果ウィンドウで [ **SCC LifeCycle**, 、クリックして **完了** アプリケーションを追加します。

    ![SCC LifeCycle](./media/active-directory-saas-scc-lifecycle-tutorial/IC795082.png "SCC LifeCycle")
##シングル サインオンの構成
  
このセクションでは、ユーザーが SAML プロトコルに基づくフェデレーションを使用して、Azure AD でのユーザーのアカウントで SCC LifeCycle に対する認証を行えるようにする方法について説明します。

###シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **SCC LifeCycle** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、* * [シングル サインオン * * ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-scc-lifecycle-tutorial/IC794122.png "Configure Single Sign-On")

2.   **どのようなユーザー SCC LifeCycle にサインオンする** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、] をクリックし、 **次**します。

    ![Configure Single Sign-On](./media/active-directory-saas-scc-lifecycle-tutorial/IC794123.png "Configure Single Sign-On")

3.   **アプリケーション URL の構成** ] ページで、 **サインオン URL** URL は、次のパターンを使用して、SCC LifeCycle のアプリケーションにサインオンする、ユーザーが使用] ボックスに、型"*https://bs1.scc.com/lc7/welcome/customer/PICTtest.aspx*"、順にクリック **次**します。

    ![アプリケーション URL の構成](./media/active-directory-saas-scc-lifecycle-tutorial/IC794124.png "Configure App URL")

4.   **SCC LifeCycle でのシングル サインオンの構成** ] ページで [ **メタデータのダウンロード**, 、コンピューターのローカルにメタデータ ファイルを保存します。

    ![Configure Single Sign-On](./media/active-directory-saas-scc-lifecycle-tutorial/IC795083.png "Configure Single Sign-On")

5.  このメタデータ ファイルを SCC LifeCycle サポート チームに転送します。

    >[AZURE.NOTE]シングル サインオンの SCC LifeCycle サポート チームを有効にするには

6.  Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-scc-lifecycle-tutorial/IC794125.png "Configure Single Sign-On")
##ユーザー プロビジョニングの構成
  
Azure AD ユーザーが SCC LifeCycle にログインできるようにするには、ユーザーを SCC LifeCycle にプロビジョニングする必要があります。
  
SCC LifeCycle へのユーザー プロビジョニングの構成にあたって必要な操作はありません。  
割り当て済みユーザーが SCC LifeCycle にログインしようとすると、必要に応じて SCC LifeCycle アカウントが自動的に作成されます。

>[AZURE.NOTE]他の SCC LifeCycle ユーザー アカウント作成ツールを使用することができますか、AAD ユーザー アカウントのプロビジョニングに、SCC LifeCycle から提供されている Api です。

##ユーザーの割り当て
  
構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

###ユーザーを SCC LifeCycle に割り当てるには、次の手順に従います。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  * * SCC LifeCycle * * アプリケーション統合ページで、をクリックして **ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-scc-lifecycle-tutorial/IC794126.png "Assign Users")

3.  テスト ユーザーを選択して [ **割り当てる**, 、順にクリック **はい** して割り当てを確認します。

    ![あり](./media/active-directory-saas-scc-lifecycle-tutorial/IC767830.png "Yes")
  
シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。

