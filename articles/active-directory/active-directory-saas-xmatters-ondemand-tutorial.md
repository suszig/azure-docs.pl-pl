<properties 
    pageTitle="チュートリアル: Azure Active Directory と xMatters OnDemand の統合 | Microsoft Azure" description="Azure Active Directory で xMatters OnDemand を使用して、シングル サインオンや自動プロビジョニングなどを有効にする方法について説明します。" 
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


# チュートリアル: Azure Active Directory と xMatters OnDemand の統合

このチュートリアルでは、Azure と xMatters OnDemand の統合について説明します。 このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   xMatters OnDemand テナント

このチュートリアルを完了すると、xMatters OnDemand に割り当てた Azure AD ユーザーが xMatters OnDemand 会社サイト (サービス プロバイダーが開始したサインオン) で、アプリケーションにシングル サインオンまたはできるを使用して、 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。

このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  xMatters OnDemand のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-xmatters-ondemand-tutorial/IC776788.png "Scenario")

## xMatters OnDemand のアプリケーション統合の有効化

このセクションでは、xMatters OnDemand のアプリケーション統合を有効にする方法を説明します。

### xMatters OnDemand のアプリケーション統合を有効にするには、次の手順を実行します。

1.  Azure 管理ポータルの左側のナビゲーション ウィンドウで、**[Active Directory]** をクリックします。

    ![Active Directory](./media/active-directory-saas-xmatters-ondemand-tutorial/IC700993.png "Active Directory")

2.  **[ディレクトリ]** の一覧から、ディレクトリ統合を有効にするディレクトリを選択します。

3.  アプリケーション ビューを開くには、ディレクトリ ビューでトップ メニューの **[アプリケーション]** をクリックします。

    ![アプリケーション](./media/active-directory-saas-xmatters-ondemand-tutorial/IC700994.png "Applications")

4.  ページの下部にある **[追加]** をクリックします。

    ![アプリケーションの追加](./media/active-directory-saas-xmatters-ondemand-tutorial/IC749321.png "Add application")

5.  **[実行する内容]** ダイアログで、**[ギャラリーからアプリケーションを追加します]** をクリックします。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-xmatters-ondemand-tutorial/IC749322.png "Add an application from gallerry")

6.  **検索ボックス**に、「**xMatters OnDemand**」と入力します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-xmatters-ondemand-tutorial/IC776789.png "Application gallery")

7.  結果ウィンドウで **[xMatters OnDemand]** を選択し、**[完了]** をクリックしてアプリケーションを追加します。

    ![xMatters OnDemand](./media/active-directory-saas-xmatters-ondemand-tutorial/IC776790.png "xMatters OnDemand")

## シングル サインオンの構成

このセクションでは、ユーザーが SAML プロトコルに基づくフェデレーションを使用して、Azure AD でのユーザーのアカウントで xMatters OnDemand に対する認証を行うことができるようにする方法を説明します。

### シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **XMatters OnDemand** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、 ** [シングル サインオン ** ダイアログ。

    ![シングル サインオンの構成](./media/active-directory-saas-xmatters-ondemand-tutorial/IC776791.png "Configure single sign-on")

2.  **どのようなユーザーを XMatters OnDemand へのサインオン** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、] をクリックし、 **次**します。

    ![シングル サインオンの構成](./media/active-directory-saas-xmatters-ondemand-tutorial/IC776792.png "Configure single sign-on")

3.  **アプリケーション URL の構成** ] ページで、 **XMatters OnDemand サインイン URL** ] ボックスに、次のパターンを使用して URL を入力"* https://\<tenant-name\>します。XMattersOnDemandapp.com*"、クリックして **次**します。

    ![アプリケーション URL の構成](./media/active-directory-saas-xmatters-ondemand-tutorial/IC776793.png "Configure app URL")

4.  **XMatters OnDemand でのシングル サインオンの構成** ] ページで、証明書をダウンロードする] をクリックして **証明書のダウンロード**, 、としては、証明書ファイルをローカルに保存および **c:\\XMatters OnDemand.cer**します。
    >[AZURE.IMPORTANT] XMatters サポート チームに証明書を転送する必要があります。 シングル サインオンの構成を確定するには、その前に、xMatters サポート チームによって証明書がアップロードされる必要があります。

    ![シングル サインオンの有効化](./media/active-directory-saas-xmatters-ondemand-tutorial/IC776794.png "Configure single sign on")

5.  別の Web ブラウザーのウィンドウで、XMatters OnDemand の企業サイトに管理者としてログインします。

6.  上部のツールバーで、**[管理者]** をクリックし、左側にあるナビゲーション バーで **[会社の詳細]** をクリックします。

    ![管理者](./media/active-directory-saas-xmatters-ondemand-tutorial/IC776795.png "Admin")

7.  **[SAML 構成]** ページで、次の手順を実行します。

    ![SAML の構成](./media/active-directory-saas-xmatters-ondemand-tutorial/IC776796.png "SAML configuration")

    1.  **[SAML の有効化]** を選択します。
    2.  Azure ポータルの **[XMatters OnDemand でのシングル サインオンの構成]** ダイアログ ページで、**[ID プロバイダーの ID]** 値をコピーして、**[ID プロバイダーの URL]** テキストボックスに貼り付けます。
    3.  Azure ポータルの **[XMatters OnDemand でのシングル サインオンの構成]** ダイアログ ページで、**[シングル サインオン サービス URL]** の値をコピーし、**[シングル サインオン サービス URL]** テキストボックスに貼り付けます。
    4.  Azure ポータルの **[XMatters OnDemand でのシングル サインオンの構成]** ダイアログ ページで、**[シングル サインアウト サービス URL]** の値をコピーし、**[シングル ログアウト URL]** テキストボックスに貼り付けます。
    5.  [会社詳細] ページの上部にあるをクリックして **Save Changes**します。
        ![会社の詳細情報](./media/active-directory-saas-xmatters-ondemand-tutorial/IC776797.png "Company details")

8.  Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![シングル サインオンの有効化](./media/active-directory-saas-xmatters-ondemand-tutorial/IC776798.png "Configure single sign on")

## ユーザー プロビジョニングの構成

Azure AD ユーザーが XMatters OnDemand にログインできるようにするには、そのユーザーを XMatters OnDemand にプロビジョニングする必要があります。  
XMatters OnDemand の場合、プロビジョニングは手動で行います。

### ユーザー アカウントをプロビジョニングするには、次の手順を実行します。

1.  **XMatters OnDemand** テナントにログインします。

2.  **[ユーザー]** タブをクリックします。

3.  **[ユーザーの追加]** をクリックします。

    ![ユーザー](./media/active-directory-saas-xmatters-ondemand-tutorial/IC781048.png "Users")

4.  **[アクティブ]** を選択します。

5.  **[ユーザーの追加]** セクションで、次の手順に従います。

    ![ユーザーの追加](./media/active-directory-saas-xmatters-ondemand-tutorial/IC781049.png "Add a User")

    1.  プロビジョニングする有効な AAD アカウントの**ユーザー ID**、**名**、**姓**、**サイト**を入力します。
    2.  **[保存]** をクリックします。

>[AZURE.NOTE] XMatters OnDemand から提供されている他の XMatters OnDemand ユーザー アカウント作成ツールまたは API を使用して、AAD ユーザー アカウントをプロビジョニングできます。

## ユーザーの割り当て

構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

### ユーザーを XMatters OnDemand に割り当てるには、次の手順を実行します。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  **XMatters OnDemand * * アプリケーション統合ページで、をクリックして * * ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-xmatters-ondemand-tutorial/IC776799.png "Assign users")

3.  テスト ユーザーを選択し、**[割り当て]**、**[はい]** の順にクリックして、割り当てを確定します。

    ![はい](./media/active-directory-saas-xmatters-ondemand-tutorial/IC767830.png "Yes")

シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。




