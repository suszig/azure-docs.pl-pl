<properties 
    pageTitle="チュートリアル: Azure Active Directory と EmpCenter の統合 | Microsoft Azure" 
    description="Azure Active Directory で EmpCenter を使用して、シングル サインオンや自動プロビジョニングなどを有効にする方法について説明します。" 
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
    ms.date="10/01/2015" 
    ms.author="markvi" />

#チュートリアル: Azure Active Directory と EmpCenter の統合
  
このチュートリアルでは、Azure と EmpCenter の統合について説明します。  
このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   EmpCenter でのシングル サインオンが有効なサブスクリプション
  
このチュートリアルを完了すると、EmpCenter に割り当てた Azure AD ユーザーは、EmpCenter 会社サイト (サービス プロバイダーが開始したサインオン) で、アプリケーションにシングル サインオンまたはできるを使用して、 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。
  
このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  EmpCenter のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-empcenter-tutorial/IC802916.png "Scenario")
##EmpCenter のアプリケーション統合の有効化
  
このセクションでは、EmpCenter のアプリケーション統合を有効にする方法について説明します。

###EmpCenter のアプリケーション統合を有効にするには、次の手順を実行します。

1.  左側のナビゲーション ウィンドウで、Azure 管理ポータルで、クリックして **Active Directory**します。

    ![Active Directory](./media/active-directory-saas-empcenter-tutorial/IC700993.png "Active Directory")

2.   **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3.  ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。

    ![アプリケーション](./media/active-directory-saas-empcenter-tutorial/IC700994.png "Applications")

4.  クリックして **追加** ページの下部にあります。

    ![アプリケーションの追加](./media/active-directory-saas-empcenter-tutorial/IC749321.png "Add application")

5.   **実行する操作** ダイアログ ボックスで、をクリックして **ギャラリーからアプリケーションを追加**します。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-empcenter-tutorial/IC749322.png "Add an application from gallerry")

6.   **検索ボックス**, 、型 **EmpCenter**します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-empcenter-tutorial/IC802917.png "Application Gallery")

7.  結果ウィンドウで [ **EmpCenter**, 、クリックして **完了** アプリケーションを追加します。

    ![EmpCentral](./media/active-directory-saas-empcenter-tutorial/IC802918.png "EmpCentral")
##シングル サインオンの構成
  
このセクションでは、ユーザーが SAML プロトコルに基づくフェデレーションを使用して、Azure AD でのユーザーのアカウントで EmpCenter に対する認証を行うことができるようにする方法を説明します。

###シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **EmpCenter** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、* * [シングル サインオン * * ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-empcenter-tutorial/IC802919.png "Configure Single Sign-On")

2.   **どのような EmpCenter にサインオン** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、順にクリック **次**します。

    ![Configure Single Sign-On](./media/active-directory-saas-empcenter-tutorial/IC802920.png "Configure Single Sign-On")

3.   **アプリケーション設定の構成** ] ページで、次の手順を実行します。

    ![Configure App Settings](./media/active-directory-saas-empcenter-tutorial/IC802921.png "Configure App Settings")

    1.   **サインオン URL** ユーザー EmpCenter アプリケーションにサインオンする URL を使用] ボックスに、種類 (例:: *https://partner-authenticati.empcenter.com/workforce/SSO.do*)。
    2.  クリックして **次へ]**

4.   **EmpCenter でのシングル サインオンの構成** ] ページで、メタデータをダウンロードする] をクリックして **メタデータのダウンロード**, 、お使いのコンピューターにメタデータ ファイルを保存します。

    ![Configure Single Sign-On](./media/active-directory-saas-empcenter-tutorial/IC802922.png "Configure Single Sign-On")

5.  ダウンロードしたメタデータ ファイルを EmpCenter サポート チームに送信します。

    >[AZURE.NOTE] EmpCenter サポート チームは、実際の SSO 構成を行うことにします。
    ご使用のサブスクリプションで SSO が有効になると通知されます。

6.  Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-empcenter-tutorial/IC802923.png "Configure Single Sign-On")
##ユーザー プロビジョニングの構成
  
Azure AD ユーザーが EmpCenter にログインできるようにするには、そのユーザーを EmpCenter にプロビジョニングする必要があります。  
EmpCenter の場合、ユーザー アカウントは、EmpCenter サポート チームが作成する必要があります。

>[AZURE.NOTE] 他の EmpCenter ユーザー アカウント作成ツールを使用するまたは Api に用意されている EmpCenter プロビジョニング Azure Active Directory ユーザー アカウントです。

##ユーザーの割り当て
  
構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

###ユーザーを EmpCenter に割り当てるには、次の手順を実行します。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  * * EmpCenter * * アプリケーション統合ページで、をクリックして **ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-empcenter-tutorial/IC802924.png "Assign Users")

3.  テスト ユーザーを選択して [ **割り当てる**, 、順にクリック **はい** して割り当てを確認します。

    ![あり](./media/active-directory-saas-empcenter-tutorial/IC767830.png "Yes")
  
シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。
