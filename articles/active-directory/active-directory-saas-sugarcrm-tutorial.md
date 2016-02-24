<properties 
    pageTitle="チュートリアル: Azure Active Directory と Sugar CRM の統合 | Microsoft Azure" 
    description="Azure Active Directory で Sugar CRM を使用して、シングル サインオンや自動プロビジョニングなどを有効にする方法について説明します。" 
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

#チュートリアル: Azure Active Directory と Sugar CRM の統合
  
このチュートリアルでは、Azure と Sugar CRM の統合について説明します。  
このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   Sugar CRM でのシングル サインオンが有効なサブスクリプション
  
このチュートリアルを完了すると、Sugar CRM に割り当てた Azure AD ユーザーは、Sugar CRM 会社サイト (サービス プロバイダーが開始したサインオン) で、アプリケーションにシングル サインオンまたはできるを使用して、 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。
  
このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  Sugar CRM のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-sugarcrm-tutorial/IC795881.png "Scenario")

##Sugar CRM のアプリケーション統合の有効化
  
このセクションでは、Sugar CRM のアプリケーション統合を有効にする方法を説明します。

###Sugar CRM のアプリケーション統合を有効にするには、次の手順に従います。

1.  左側のナビゲーション ウィンドウで、Azure 管理ポータルで、クリックして **Active Directory**します。

    ![Active Directory](./media/active-directory-saas-sugarcrm-tutorial/IC700993.png "Active Directory")

2.   **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3.  ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。

    ![アプリケーション](./media/active-directory-saas-sugarcrm-tutorial/IC700994.png "Applications")

4.  クリックして **追加** ページの下部にあります。

    ![アプリケーションの追加](./media/active-directory-saas-sugarcrm-tutorial/IC749321.png "Add application")

5.   **実行する操作** ダイアログ ボックスで、をクリックして **ギャラリーからアプリケーションを追加**します。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-sugarcrm-tutorial/IC749322.png "Add an application from gallerry")

6.   **検索ボックス**, 、型 **Sugar CRM**します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-sugarcrm-tutorial/IC795882.png "Application Gallery")

7.  結果ウィンドウで [ **Sugar CRM**, 、クリックして **完了** アプリケーションを追加します。

    ![Sugar CRM](./media/active-directory-saas-sugarcrm-tutorial/IC795883.png "Sugar CRM")

##シングル サインオンの構成
  
このセクションでは、SAML プロトコルに基づくフェデレーションを使用して、Sugar CRM で Azure AD のユーザー アカウントを使用してユーザーを認証できるようにする方法を説明します。  
この手順の途中で、base-64 でエンコードされた証明書を Sugar CRM テナントにアップロードする必要があります。  
この手順に慣れていない場合は、次を参照してください [バイナリ証明書をテキスト ファイルに変換する方法。](http://youtu.be/PlgrzUZ-Y1o)

###シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **Sugar CRM** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、* * [シングル サインオン * * ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-sugarcrm-tutorial/IC795884.png "Configure Single Sign-On")

2.   **どのようなユーザーが Sugar CRM へのサインオン** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、] をクリックし、 **次**します。

    ![Configure Single Sign-On](./media/active-directory-saas-sugarcrm-tutorial/IC795885.png "Configure Single Sign-On")

3.   **アプリケーション URL の構成** ] ページの [、 **Sugar CRM サインオン URL** ユーザーが Sugar CRM アプリケーションにサインオンする URL を使用] ボックスに、型 (例::"*http://company.sugarondemand.com*"、] をクリックし、 **次**します。

    ![アプリケーション URL の構成](./media/active-directory-saas-sugarcrm-tutorial/IC795886.png "Configure App URL")

4.   **Sugar CRM でのシングル サインオンの構成** ] ページで、証明書をダウンロードする] をクリックして **証明書のダウンロード**, 、お使いのコンピューターに証明書ファイルを保存します。

    ![Configure Single Sign-On](./media/active-directory-saas-sugarcrm-tutorial/IC796918.png "Configure Single Sign-On")

5.  別の Web ブラウザー ウィンドウで、Sugar CRM 企業サイトに管理者としてログインします。

6.  移動して **Admin**します。

    ![管理者](./media/active-directory-saas-sugarcrm-tutorial/IC795888.png "Admin")

7.   **管理** ] をクリックして **パスワード管理**します。

    ![管理](./media/active-directory-saas-sugarcrm-tutorial/IC795889.png "Administration")

8.  選択 **SAML 認証を有効にする**です。

    ![管理](./media/active-directory-saas-sugarcrm-tutorial/IC795890.png "Administration")

9.   **SAML 認証** セクションで、次の手順に従います。

    ![SAML 認証](./media/active-directory-saas-sugarcrm-tutorial/IC795891.png "SAML Authentication")

    1.  Azure ポータルで、 **Sugar CRM でのシングル サインオンの構成** ] ダイアログ ページでコピー、 **リモート ログイン URL** 値に設定して、貼り付けます、 **ログイン URL** ] ボックスに貼り付けます。
    2.  Azure ポータルで、 **Sugar CRM でのシングル サインオンの構成** ] ダイアログ ページでコピー、 **リモート ログイン URL** 値に設定して、貼り付けます、 **SLO URL** ] ボックスに貼り付けます。
    3.  作成、 **base-64 でエンコードされた** からダウンロードした証明書ファイル。

        >[AZURE.TIP] 詳細については、次を参照してください [バイナリ証明書をテキスト ファイルに変換する方法。](http://youtu.be/PlgrzUZ-Y1o)

    4.  Base-64 でエンコードされた証明書をメモ帳で開き、内容をクリップボードにコピーおよび貼り付けて、全体の証明書を **X.509 証明書** ] ボックスに貼り付けます。
    5.  クリックして **保存**します。

10. Azure ポータルで、 **Sugar CRM でのシングル サインオンの構成** ] ダイアログ ページで、シングル サインオンの構成情報を選択し、 **完了**します。

    ![Configure Single Sign-On](./media/active-directory-saas-sugarcrm-tutorial/IC796919.png "Configure Single Sign-On")

##ユーザー プロビジョニングの構成
  
Azure AD ユーザーが Sugar CRM にログインできるようにするには、そのユーザーを Sugar CRM にプロビジョニングする必要があります。  
Sugar CRM の場合、プロビジョニングは手動で行います。

###ユーザー アカウントをプロビジョニングするには、次の手順を実行します。

1.  ログインして **Sugar CRM** 会社サイトに管理者として。

2.  移動して **Admin**します。

    ![管理者](./media/active-directory-saas-sugarcrm-tutorial/IC795888.png "Admin")

3.   **管理** ] をクリックして **ユーザー管理**します。

    ![管理](./media/active-directory-saas-sugarcrm-tutorial/IC795893.png "Administration")

4.  移動して **ユーザー \ > ユーザーの新規作成**します。

    ![新しいユーザーの作成](./media/active-directory-saas-sugarcrm-tutorial/IC795894.png "Create New User")

5.   **ユーザー プロファイル** ] タブで、次の手順を実行します。

    ![新しいユーザー](./media/active-directory-saas-sugarcrm-tutorial/IC795895.png "New User")

    1.  関連するテキスト ボックスに、プロビジョニングする有効な Azure Active Directory アカウントのユーザー名、姓、および電子メール アドレスを入力します。

6.  として **ステータス**, [ **Active**します。

7.  [Password] タブで、次の手順に従います。

    ![新しいユーザー](./media/active-directory-saas-sugarcrm-tutorial/IC795896.png "New User")

    1.  該当するテキスト ボックスにパスワードを入力します。
    2.  クリックして **保存**します。

>[AZURE.NOTE] 他の Sugar CRM ユーザー アカウント作成ツールを使用することができますか、AAD ユーザー アカウントをプロビジョニング、Sugar CRM から提供されている Api です。

##ユーザーの割り当て
  
構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

###ユーザーを Sugar CRM に割り当てるには、次の手順に従います。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.   **Sugar CRM** アプリケーション統合ページで、をクリックして **ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-sugarcrm-tutorial/IC795897.png "Assign Users")

3.  テスト ユーザーを選択して [ **割り当てる**, 、順にクリック **はい** して割り当てを確認します。

    ![あり](./media/active-directory-saas-sugarcrm-tutorial/IC767830.png "Yes")
  
シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。
