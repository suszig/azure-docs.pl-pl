<properties 
    pageTitle="チュートリアル: Azure Active Directory と Chromeriver の統合 | Microsoft Azure" 
    description="Azure Active Directory で Chromeriver を使用して、シングル サインオンや自動プロビジョニングなどを有効にする方法について説明します。" 
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


#チュートリアル: Azure Active Directory と Chromeriver の統合

このチュートリアルでは、Azure と Chromeriver の統合について説明します。  
このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   Chromeriver でのシングル サインオンが有効なサブスクリプション

このチュートリアルを完了すると、chromeriver との統合に割り当てた Azure AD ユーザーは、chromeriver との統合の会社サイト (サービス プロバイダーが開始したサインオン) で、アプリケーションにシングル サインオンまたはできるを使用して、 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。

このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  Chromeriver のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-chromeriver-tutorial/IC802755.png "Scenario")
##Chromeriver のアプリケーション統合の有効化

このセクションでは、Chromeriver のアプリケーション統合を有効にする方法を説明します。

###Chromeriver のアプリケーション統合を有効にするには、次の手順を実行します。

1.  左側のナビゲーション ウィンドウで、Azure 管理ポータルで、クリックして **Active Directory**します。

    ![Active Directory](./media/active-directory-saas-chromeriver-tutorial/IC700993.png "Active Directory")

2.   **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3.  ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。

    ![アプリケーション](./media/active-directory-saas-chromeriver-tutorial/IC700994.png "Applications")

4.  クリックして **追加** ページの下部にあります。

    ![アプリケーションの追加](./media/active-directory-saas-chromeriver-tutorial/IC749321.png "Add application")

5.   **実行する操作** ダイアログ ボックスで、をクリックして **ギャラリーからアプリケーションを追加**します。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-chromeriver-tutorial/IC749322.png "Add an application from gallerry")

6.   **検索ボックス**, 、型 **chromeriver との統合**します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-chromeriver-tutorial/IC802756.png "Application Gallery")

7.  結果ウィンドウで [ **chromeriver との統合**, 、クリックして **完了** アプリケーションを追加します。
##シングル サインオンの構成

このセクションでは、SAML プロトコルに基づくフェデレーションを使用して、ユーザーが Azure AD のアカウントで Chromeriver に対する認証を行えるようにする方法を説明します。

###シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **chromeriver との統合** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、* * [シングル サインオン * * ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-chromeriver-tutorial/IC802757.png "Configure Single Sign-On")

2.   **どのような chromeriver との統合にサインオン** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、] をクリックし、 **次**します。

    ![Configure Single Sign-On](./media/active-directory-saas-chromeriver-tutorial/IC802758.png "Configure Single Sign-On")

3.   **アプリケーション設定の構成** ] ページで、次の手順を実行します。

    ![Configure App Settings](./media/active-directory-saas-chromeriver-tutorial/IC802759.png "Configure App Settings")

    1.   **応答 URL** ] ボックスに、入力、chromeriver との統合 **AssertionConsumerService URL** (例:: *https://qa-app.chromeriver.com/login/sso/saml/consume?customerId=911*)。  

        >[AZURE.NOTE] この値は、chromeriver との統合サポート チームから入手できます。

    2.  クリックして **次へ]**

4.   **Chromeriver との統合でのシングル サインオンの構成** ] ページで、メタデータをダウンロードする] をクリックして **メタデータのダウンロード**, 、お使いのコンピューターにメタデータ ファイルを保存します。

    ![Configure Single Sign-On](./media/active-directory-saas-chromeriver-tutorial/IC802760.png "Configure Single Sign-On")

5.  ダウンロードしたメタデータ ファイルを Chromeriver サポート チームに送信します。

    >[AZURE.NOTE] Chromeriver との統合サポート チームは、実際の SSO 構成を行うことにします。  
    ご使用のサブスクリプションで SSO が有効になると通知されます。

6.  Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-chromeriver-tutorial/IC802761.png "Configure Single Sign-On")
##ユーザー プロビジョニングの構成

Azure AD ユーザーが Chromeriver にログインできるようにするには、ユーザーを Chromeriver にプロビジョニングする必要があります。  
Chromeriver の場合、Chromeriver サポート チームがユーザー アカウントを作成する必要があります。

>[AZURE.NOTE] 他の chromeriver との統合ユーザー アカウント作成ツールを使用するまたは Api によって提供される chromeriver との統合にプロビジョニング Azure Active Directory ユーザー アカウントです。

##ユーザーの割り当て

構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

###ユーザーを Chromeriver に割り当てるには、次の手順を実行します。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  * * Chromeriver との統合 * * アプリケーション統合ページで、をクリックして **ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-chromeriver-tutorial/IC802762.png "Assign Users")

3.  テスト ユーザーを選択して [ **割り当てる**, 、順にクリック **はい** して割り当てを確認します。

    ![あり](./media/active-directory-saas-chromeriver-tutorial/IC767830.png "Yes")

シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。

