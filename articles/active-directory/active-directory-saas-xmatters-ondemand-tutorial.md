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

#チュートリアル: Azure Active Directory と xMatters OnDemand の統合
  
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

##xMatters OnDemand のアプリケーション統合の有効化
  
このセクションでは、xMatters OnDemand のアプリケーション統合を有効にする方法を説明します。

###xMatters OnDemand のアプリケーション統合を有効にするには、次の手順を実行します。

1.  左側のナビゲーション ウィンドウで、Azure 管理ポータルで、クリックして **Active Directory**します。

    ![Active Directory](./media/active-directory-saas-xmatters-ondemand-tutorial/IC700993.png "Active Directory")

2.   **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3.  ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。

    ![アプリケーション](./media/active-directory-saas-xmatters-ondemand-tutorial/IC700994.png "Applications")

4.  クリックして **追加** ページの下部にあります。

    ![アプリケーションの追加](./media/active-directory-saas-xmatters-ondemand-tutorial/IC749321.png "Add application")

5.   **実行する操作** ダイアログ ボックスで、をクリックして **ギャラリーからアプリケーションを追加**します。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-xmatters-ondemand-tutorial/IC749322.png "Add an application from gallerry")

6.   **検索ボックス**, 、型 **xMatters OnDemand**します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-xmatters-ondemand-tutorial/IC776789.png "Application gallery")

7.  結果ウィンドウで [ **XMatters OnDemand**, 、クリックして **完了** アプリケーションを追加します。

    ![xMatters OnDemand](./media/active-directory-saas-xmatters-ondemand-tutorial/IC776790.png "xMatters OnDemand")

##シングル サインオンの構成
  
このセクションでは、ユーザーが SAML プロトコルに基づくフェデレーションを使用して、Azure AD でのユーザーのアカウントで xMatters OnDemand に対する認証を行うことができるようにする方法を説明します。

###シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **XMatters OnDemand** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、* * [シングル サインオン * * ダイアログ。

    ![Configure single sign-on](./media/active-directory-saas-xmatters-ondemand-tutorial/IC776791.png "Configure single sign-on")

2.   **どのようなユーザーを XMatters OnDemand へのサインオン** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、] をクリックし、 **次**します。

    ![シングル サインオンの構成](./media/active-directory-saas-xmatters-ondemand-tutorial/IC776792.png "Configure single sign-on")

3.   **アプリケーション URL の構成** ] ページで、 **XMatters OnDemand サインイン URL** ] ボックスに、次のパターンを使用して URL を入力"*https://\<tenant-name\>。XMattersOnDemandapp.com*"、クリックして **次**します。

    ![アプリケーション URL の構成](./media/active-directory-saas-xmatters-ondemand-tutorial/IC776793.png "Configure app URL")

4.   **XMatters OnDemand でのシングル サインオンの構成** ] ページで、証明書をダウンロードする] をクリックして **証明書のダウンロード**, 、としては、証明書ファイルをローカルに保存および **c:\\XMatters OnDemand.cer**します。

    >[AZURE.IMPORTANT] 証明書は xMatters サポート チームに転送する必要があります。 シングル サインオンの構成を確定するには、その前に、xMatters サポート チームによって証明書がアップロードされる必要があります。

    ![シングル サインオンの構成](./media/active-directory-saas-xmatters-ondemand-tutorial/IC776794.png "Configure single sign on")

5.  別の Web ブラウザーのウィンドウで、XMatters OnDemand の企業サイトに管理者としてログインします。

6.  上部にあるツールバーで、クリックして **Admin**, 、] をクリックし、 **Company Details** 左側にあるナビゲーション バーでします。

    ![管理者](./media/active-directory-saas-xmatters-ondemand-tutorial/IC776795.png "Admin")

7.   **SAML Configuration** ] ページで、次の手順を実行します。

    ![SAML の構成](./media/active-directory-saas-xmatters-ondemand-tutorial/IC776796.png "SAML configuration")

    1.  選択 **Enable SAML**します。
    2.  Azure ポータルで、 **XMatters OnDemand でのシングル サインオンの構成** ] ダイアログ ページでコピー、 **Id プロバイダーの ID** 値に設定して、貼り付けます、 **Id プロバイダーの ID** ] ボックスに貼り付けます。
    3.  Azure ポータルで、 **XMatters OnDemand でのシングル サインオンの構成** ] ダイアログ ページでコピー、 **シングル サインオン サービス URL** 値に設定して、貼り付けます、 **Single Sign On URL** ] ボックスに貼り付けます。
    4.  Azure ポータルで、 **XMatters OnDemand でのシングル サインオンの構成** ] ダイアログ ページでコピー、 **シングル サインアウト サービス URL** 値に設定して、貼り付けます、 **Single Logout URL** ] ボックスに貼り付けます。
    5.  [会社詳細] ページの上部にあるをクリックして **Save Changes**します。
        ![会社の詳細情報](./media/active-directory-saas-xmatters-ondemand-tutorial/IC776797.png "Company details")

8.  Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![シングル サインオンの構成](./media/active-directory-saas-xmatters-ondemand-tutorial/IC776798.png "Configure single sign on")

##ユーザー プロビジョニングの構成
  
Azure AD ユーザーが XMatters OnDemand にログインできるようにするには、そのユーザーを XMatters OnDemand にプロビジョニングする必要があります。  
XMatters OnDemand の場合、プロビジョニングは手動で行います。

###ユーザー アカウントをプロビジョニングするには、次の手順を実行します。

1.  ログインして **XMatters OnDemand** テナントです。

2.  クリックして、 **ユーザー** ] タブをクリックします。

3.  クリックして **ユーザーを追加**します。

    ![ユーザー](./media/active-directory-saas-xmatters-ondemand-tutorial/IC781048.png "Users")

4.  選択 **Active**します。

5.   **ユーザーを追加** セクションで、次の手順に従います。

    ![ユーザーの追加](./media/active-directory-saas-xmatters-ondemand-tutorial/IC781049.png "Add a User")

    1.  入力、 **UserID**, 、**名**, 、**姓、名**, 、**サイト** 、プロビジョニングする有効な AAD アカウントのです。
    2.  クリックして **保存**します。

>[AZURE.NOTE] 他の XMatters OnDemand ユーザー アカウント作成ツールを使用することができますか、AAD ユーザー アカウントをプロビジョニング、XMatters OnDemand 提供の Api です。

##ユーザーの割り当て
  
構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

###ユーザーを XMatters OnDemand に割り当てるには、次の手順を実行します。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  * * XMatters OnDemand * * アプリケーション統合ページで、をクリックして **ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-xmatters-ondemand-tutorial/IC776799.png "Assign users")

3.  テスト ユーザーを選択して [ **割り当てる**, 、順にクリック **はい** して割り当てを確認します。

    ![あり](./media/active-directory-saas-xmatters-ondemand-tutorial/IC767830.png "Yes")
  
シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。

