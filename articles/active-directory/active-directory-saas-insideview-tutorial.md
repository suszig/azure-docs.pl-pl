<properties 
    pageTitle="チュートリアル: Azure Active Directory と InsideView の統合 | Microsoft Azure" 
    description="Azure Active Directory と InsideView を使用して、シングル サインオン、自動化されたプロビジョニング、および詳細を有効にする方法を説明します。" 
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


# チュートリアル: Azure Active Directory と InsideView の統合

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
## InsideView のアプリケーション統合の有効化

このセクションでは、InsideView のアプリケーション統合を有効にする方法を説明します。

### InsideView のアプリケーション統合を有効にするには、次の手順を実行します。

1.  Azure 管理ポータルの左側のナビゲーション ウィンドウで、**[Active Directory]** をクリックします。

    ![Active Directory](./media/active-directory-saas-insideview-tutorial/IC700993.png "Active Directory")

2.  **[ディレクトリ]** の一覧から、ディレクトリ統合を有効にするディレクトリを選択します。

3.  アプリケーション ビューを開くには、ディレクトリ ビューでトップ メニューの **[アプリケーション]** をクリックします。

    ![アプリケーション](./media/active-directory-saas-insideview-tutorial/IC700994.png "Applications")

4.  ページの下部にある **[追加]** をクリックします。

    ![アプリケーションの追加](./media/active-directory-saas-insideview-tutorial/IC749321.png "Add application")

5.  **[実行する内容]** ダイアログで、**[ギャラリーからアプリケーションを追加します]** をクリックします。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-insideview-tutorial/IC749322.png "Add an application from gallerry")

6.  **検索ボックス**に「**InsideView**」と入力します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-insideview-tutorial/IC794129.png "Application Gallery")

7.  結果ウィンドウで **[InsideView]** を選択し、**[完了]** をクリックしてアプリケーションを追加します。

    ![InsideView](./media/active-directory-saas-insideview-tutorial/IC794130.png "InsideView")
## シングル サインオンの構成

このセクションでは、ユーザーが SAML プロトコルに基づくフェデレーションを使用して、Azure AD でのユーザーのアカウントで InsideView に対する認証を行うことができるようにする方法を説明します。  
この手順の途中で、base-64 でエンコードされた証明書ファイルを作成する必要があります。  
この手順に慣れていない場合は、次を参照してください。 [バイナリ証明書をテキスト ファイルに変換する方法](http://youtu.be/PlgrzUZ-Y1o)します。

### シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **InsideView** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、 ** [シングル サインオン ** ダイアログ。

    ![シングル サインオンの構成](./media/active-directory-saas-insideview-tutorial/IC794131.png "Configure Single SignOn")

2.  **どのようなユーザーが InsideView へのサインオン** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、] をクリックし、 **次**します。

    ![シングル サインオンの構成](./media/active-directory-saas-insideview-tutorial/IC794132.png "Configure Single SignOn")

3.  **アプリケーション URL の構成** ] ページの [、 **InsideView 応答 URL** ] ボックスに、該当する InsideView SSO の URL (例:: `https://my.insideview.com/iv/<STS >/login.iv`)、順にクリック **次**します。

    ![アプリケーション URL の構成](./media/active-directory-saas-insideview-tutorial/IC794133.png "Configure App URL")

4.  **InsideView でのシングル サインオンの構成** ] ページで、証明書をダウンロードする] をクリックして **証明書のダウンロード**, 、お使いのコンピューターに証明書ファイルを保存します。

    ![シングル サインオンの構成](./media/active-directory-saas-insideview-tutorial/IC794134.png "Configure Single SignOn")

5.  別の Web ブラウザー ウィンドウで、InsideView 企業サイトに管理者としてログインします。

6.  上部のツールバーで、**[管理者]**、**[シングル サインオン設定]** をクリックし、**[SAML の追加]** をクリックします。

    ![SAML シングル サインオンの設定](./media/active-directory-saas-insideview-tutorial/IC794135.png "SAML Single Sign On Settings")

7.  **[新しい SAML の追加]** セクションで、次の手順を実行します。

    ![新しい SAML の追加](./media/active-directory-saas-insideview-tutorial/IC794136.png "Add a New SAML")

    1.  **[STS 名]** テキストボックスに、構成の名前を入力します。
    2.  Azure ポータルの **[InsideView でのシングル サインオンの構成]** ダイアログ ページで、**[サービス プロバイダー (SP) が開始したエンドポイント]** の値をコピーし、**[SamlP/WS-Fed 未承諾エンドポイント]** テキストボックスに貼り付けます。
    3.  ダウンロードした証明書から **base-64 でエンコードされた**ファイルを作成します。
        >[AZURE.TIP]詳細については、「 [バイナリ証明書をテキスト ファイルに変換する方法](http://youtu.be/PlgrzUZ-Y1o)

    4.  base-64 でエンコードされた証明書をメモ帳で開き、その内容をクリップボードにコピーして、**[STS 証明書]** テキストボックスに貼り付けます。
    5.  **Crm ユーザー Id マッピング** ] ボックスに「 **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**します。
    6.  **Crm 電子メール マッピング** ] ボックスに「 **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**します。
    7.  **Crm ファースト ネーム マッピング** ] ボックスに「 **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**します。
    8.  **Crm lastName マッピング** ] ボックスに「 **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**します。
    9.  **[保存]** をクリックします。

8.  Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![シングル サインオンの構成](./media/active-directory-saas-insideview-tutorial/IC794137.png "Configure Single SignOn")
## ユーザー プロビジョニングの構成

Azure AD ユーザーが InsideView にログインできるようにするには、そのユーザーを InsideView にプロビジョニングする必要があります。  
InsideView の場合、プロビジョニングは手動で行います。

ユーザーまたは連絡先を InsideView に作成する、カスタマー サクセス マネージャーにお問い合わせや電子メールを送信 **support@insideview.com**
>[AZURE.NOTE] InsideView から提供されている他の InsideView ユーザー アカウント作成ツールまたは API を使用して、Azure AD ユーザー アカウントをプロビジョニングできます。

## ユーザーの割り当て

構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

### ユーザーを InsideView に割り当てるには、次の手順を実行します。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  **InsideView * * アプリケーション統合ページで、をクリックして * * ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-insideview-tutorial/IC794138.png "Assign Users")

3.  テスト ユーザーを選択し、**[割り当て]**、**[はい]** の順にクリックして、割り当てを確定します。

    ![はい](./media/active-directory-saas-insideview-tutorial/IC767830.png "Yes")

シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。




