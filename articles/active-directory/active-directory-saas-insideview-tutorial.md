<properties 
    pageTitle="チュートリアル: Azure Active Directory と InsideView の統合 | Microsoft Azure" 
    description="Azure Active Directory で InsideView を使用して、シングル サインオンや自動プロビジョニングなどを有効にする方法を説明します。" 
    services="active-directory" 
    authors="MarkusVi"  
    documentationCenter="na" manager="stevenpo"/>
<tags 
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="10/22/2015" 
    ms.author="markvi" />

#チュートリアル: Azure Active Directory と InsideView の統合
  
このチュートリアルでは、Azure と InsideView の統合について説明します。  
このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   InsideView テナント
  
このチュートリアルを完了すると、InsideView に割り当てた Azure AD ユーザーは、InsideView 会社サイト (サービス プロバイダーが開始したサインオン) で、アプリケーションにシングル サインオンまたはできるを使用して、 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。
  
このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  InsideView のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-insideview-tutorial/IC794128.png "Scenario")
##InsideView のアプリケーション統合の有効化
  
このセクションでは、InsideView のアプリケーション統合を有効にする方法を説明します。

###InsideView のアプリケーション統合を有効にするには、次の手順を実行します。

1.  左側のナビゲーション ウィンドウで、Azure 管理ポータルで、クリックして **Active Directory**します。

    ![Active Directory](./media/active-directory-saas-insideview-tutorial/IC700993.png "Active Directory")

2.   **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3.  ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。

    ![アプリケーション](./media/active-directory-saas-insideview-tutorial/IC700994.png "Applications")

4.  クリックして **追加** ページの下部にあります。

    ![アプリケーションの追加](./media/active-directory-saas-insideview-tutorial/IC749321.png "Add application")

5.   **実行する操作** ダイアログ ボックスで、をクリックして **ギャラリーからアプリケーションを追加**します。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-insideview-tutorial/IC749322.png "Add an application from gallerry")

6.   **検索ボックス**, 、型 **InsideView**します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-insideview-tutorial/IC794129.png "Application Gallery")

7.  結果ウィンドウで [ **InsideView**, 、クリックして **完了** アプリケーションを追加します。

    ![InsideView](./media/active-directory-saas-insideview-tutorial/IC794130.png "InsideView")
##シングル サインオンの構成
  
このセクションでは、ユーザーが SAML プロトコルに基づくフェデレーションを使用して、Azure AD でのユーザーのアカウントで InsideView に対する認証を行うことができるようにする方法を説明します。  
この手順の途中で、base-64 でエンコードされた証明書ファイルを作成する必要があります。  
この手順に慣れていない場合は、次を参照してください。 [バイナリ証明書をテキスト ファイルに変換する方法](http://youtu.be/PlgrzUZ-Y1o)します。

###シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **InsideView** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、* * [シングル サインオン * * ダイアログ。

    ![シングル サインオンの構成](./media/active-directory-saas-insideview-tutorial/IC794131.png "Configure Single SignOn")

2.   **どのようなユーザーが InsideView へのサインオン** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、] をクリックし、 **次**します。

    ![シングル サインオンの構成](./media/active-directory-saas-insideview-tutorial/IC794132.png "Configure Single SignOn")

3.   **アプリケーション URL の構成** ] ページの [、 **InsideView 応答 URL** ] ボックスに、該当する InsideView SSO の URL (例:: `https://my.insideview.com/iv/<STS Name>/login.iv`)、順にクリック **次**します。

    ![アプリケーション URL の構成](./media/active-directory-saas-insideview-tutorial/IC794133.png "Configure App URL")

4.   **InsideView でのシングル サインオンの構成** ] ページで、証明書をダウンロードする] をクリックして **証明書のダウンロード**, 、お使いのコンピューターに証明書ファイルを保存します。

    ![シングル サインオンの構成](./media/active-directory-saas-insideview-tutorial/IC794134.png "Configure Single SignOn")

5.  別の Web ブラウザー ウィンドウで、InsideView 企業サイトに管理者としてログインします。

6.  上部にあるツールバーで、クリックして **Admin**, 、**SingleSignOn の設定**, 、] をクリックし、 **SAML の追加**します。

    ![SAML シングル サインオンの設定](./media/active-directory-saas-insideview-tutorial/IC794135.png "SAML Single Sign On Settings")

7.   **新しい SAML の追加** セクションで、次の手順に従います。

    ![新しい SAML の追加](./media/active-directory-saas-insideview-tutorial/IC794136.png "Add a New SAML")

    1.   **STS 名** ] ボックスに、構成の名前を入力します。
    2.  Azure ポータルで、 **InsideView でのシングル サインオンの構成** ] ダイアログ ページでコピー、 **サービス プロバイダー (SP) が開始したエンドポイント** 値に設定して、貼り付けます、 **SamlP/Ws-fed Unsolicated エンドポイント** ] ボックスに貼り付けます。
    3.  作成、 **base-64 でエンコードされた** からダウンロードした証明書ファイル。
        
        >[AZURE.TIP]詳細については、次を参照してください [バイナリ証明書をテキスト ファイルに変換する方法。](http://youtu.be/PlgrzUZ-Y1o)

    4.  Base-64 でエンコードされた証明書をメモ帳で開き、という内容がクリップボードにコピーおよびに貼り付け、 **STS 証明書** ] テキスト ボックス
    5.   **Crm ユーザー Id マッピング** ] ボックスに「 **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**します。
    6.   **Crm 電子メール マッピング** ] ボックスに「 **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**します。
    7.   **Crm ファースト ネーム マッピング** ] ボックスに「 **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**します。
    8.   **Crm lastName マッピング** ] ボックスに「 **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**します。
    9.  クリックして **保存**します。

8.  Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![シングル サインオンの構成](./media/active-directory-saas-insideview-tutorial/IC794137.png "Configure Single SignOn")
##ユーザー プロビジョニングの構成
  
Azure AD ユーザーが InsideView にログインできるようにするには、そのユーザーを InsideView にプロビジョニングする必要があります。  
InsideView の場合、プロビジョニングは手動で行います。
  
ユーザーまたは連絡先を InsideView に作成する、カスタマー サクセス マネージャーにお問い合わせや電子メールを送信 **support@insideview.com**

>[AZURE.NOTE] 他の InsideView ユーザー アカウント作成ツールを使用するまたは Azure AD ユーザー アカウントをプロビジョニングする InsideView 提供の Api です。

##ユーザーの割り当て
  
構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

###ユーザーを InsideView に割り当てるには、次の手順を実行します。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  * * InsideView * * アプリケーション統合ページで、をクリックして **ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-insideview-tutorial/IC794138.png "Assign Users")

3.  テスト ユーザーを選択して [ **割り当てる**, 、順にクリック **はい** して割り当てを確認します。

    ![あり](./media/active-directory-saas-insideview-tutorial/IC767830.png "Yes")
  
シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。

