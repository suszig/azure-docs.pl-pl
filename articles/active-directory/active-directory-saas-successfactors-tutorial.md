<properties 
    pageTitle="チュートリアル: Azure Active Directory と SuccessFactors の統合 | Microsoft Azure"
    description="Azure Active Directory で SuccessFactors を使用してシングル サインオンや自動プロビジョニングなどを有効にする方法について説明します。" 
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

#チュートリアル: Azure Active Directory と SuccessFactors の統合
  
このチュートリアルの目的はにおける Azure と SuccessFactors の統合を紹介する、 **SP がシングル サインオン モードを開始した**します。  
このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   SP によって開始されたモードで SuccessFactors でのシングル サインオンが有効なサブスクリプション
  
このチュートリアルを完了すると、SuccessFactors に割り当てた Azure AD ユーザーは、SuccessFactors 会社サイト (サービス プロバイダーが開始したサインオン) で、アプリケーションにシングル サインオンまたはできるを使用して、 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。
  
このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  SuccessFactors のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-successfactors-tutorial/IC791135.png "Scenario")

##SuccessFactors のアプリケーション統合の有効化
  
このセクションでは、SuccessFactors のアプリケーション統合を有効にする方法を説明します。

###SuccessFactors アプリケーションのアプリケーション統合を有効にするには、次の手順に従います。

1.  左側のナビゲーション ウィンドウで、Azure 管理ポータルで、クリックして **Active Directory**します。

    ![Active Directory](./media/active-directory-saas-successfactors-tutorial/IC700993.png "Active Directory")

2.   **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3.  ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。

    ![アプリケーション](./media/active-directory-saas-successfactors-tutorial/IC700994.png "Applications")

4.  クリックして **追加** ページの下部にあります。

    ![アプリケーションの追加](./media/active-directory-saas-successfactors-tutorial/IC749321.png "Add application")

5.   **実行する操作** ダイアログ ボックスで、をクリックして **ギャラリーからアプリケーションを追加**します。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-successfactors-tutorial/IC749322.png "Add an application from gallerry")

6.   **検索ボックス**, 、型 **SuccessFactors**します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-successfactors-tutorial/IC791136.png "Appliation Gallery")

7.  結果ウィンドウで [ **SuccessFactors**, 、クリックして **完了** アプリケーションを追加します。

    ![SuccessFactors](./media/active-directory-saas-successfactors-tutorial/IC791137.png "SuccessFactors")

##シングル サインオンの構成
  
このセクションでは、SAML プロトコルに基づくフェデレーションを使用して、SuccessFactors で Azure AD のユーザー アカウントを使用してユーザーを認証できるようにする方法を説明します。
  
シングル サインオンを構成するには、SuccessFactors サポート チームに連絡する必要があります。

###シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **SuccessFactors** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、* * [シングル サインオン * * ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-successfactors-tutorial/IC791138.png "Configure Single Sign-On")

2.   **どのようなユーザーが SuccessFactors へのサインオン** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、] をクリックし、 **次**します。

    ![Configure Single Sign-On](./media/active-directory-saas-successfactors-tutorial/IC791139.png "Configure Single Sign-On")

3.   **アプリケーション URL の構成** ] ページで、次の手順を実行して順にクリックして **次**します。

    ![Configure App URL](./media/active-directory-saas-successfactors-tutorial/IC791140.png "Configure App URL")

    1.   **サインオン URL** ] ボックスに、型に使用する URL、ユーザーが、SuccessFactors アプリケーションへのサインオン (例::"*https://performancemanager4.successfactors.com/sf/home?company=CompanyName&loginMethod=SSO*") です。
    2.   **SuccessFactors 応答 URL** ] ボックスに「 **https://performancemanager4.successfactors.com/saml2/SAMLAssertionConsumer?company=CompanyName**します。

        >[AZURE.NOTE] この値は、単なる一時的なプレース ホルダーです。  
        >実際の値は、SuccessFactors サポート チームから取得します。  
        >このチュートリアルの中に、SuccessFactors サポート チームに連絡することを指示する手順があります。  
        >サポート チームから、実際の SuccessFactors 応答 URL を取得できます。

4.   **SuccessFactors でのシングル サインオンの構成** ] ページで、証明書をダウンロードする] をクリックして **証明書のダウンロード**, 、お使いのコンピューターに証明書ファイルを保存します。

    ![Configure Single Sign-On](./media/active-directory-saas-successfactors-tutorial/IC791141.png "Configure Single Sign-On")

5.  SAML ベースのシングル サインオンを構成するために、SuccessFactors サポート チームに連絡し、次の項目をチームに提供します。

    1.  ダウンロードした証明書
    2.  リモート ログイン URL
    3.  リモート ログアウト URL

    >[AZURE.IMPORTANT] SuccessFactors サポート チームに NameId 形式のパラメーターを設定するよう依頼してください"*未指定*"です。

    Successfactors サポート チームが送信する正しい **Successfactors 応答 URL** に必要な **アプリケーション URL の構成** ダイアログ。

6.  Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-successfactors-tutorial/IC791142.png "Configure Single Sign-On")

##ユーザー プロビジョニングの構成
  
Azure AD ユーザーが SuccessFactors にログインできるようにするには、ユーザーを SuccessFactors にプロビジョニングする必要があります。  
SuccessFactors の場合、プロビジョニングは手動で行います。
  
SuccessFactors でユーザーを作成するには、SuccessFactors のサポート チームに連絡する必要があります。

##ユーザーの割り当て
  
構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

###ユーザーを SuccessFactors に割り当てるには、次の手順に従います。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  * * SuccessFactors * * アプリケーション統合ページで、をクリックして **ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-successfactors-tutorial/IC791143.png "Assign Users")

3.  テスト ユーザーを選択して [ **割り当てる**, 、順にクリック **はい** して割り当てを確認します。

    ![あり](./media/active-directory-saas-successfactors-tutorial/IC767830.png "Yes")
  
シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。
