<properties 
    pageTitle="チュートリアル: Azure Active Directory と SpringCM の統合 | Microsoft Azure" 
    description="Azure Active Directory で SpringCM を使用して、シングル サインオンや自動プロビジョニングなどを有効にする方法について説明します。" 
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

#チュートリアル: Azure Active Directory と SpringCM の統合
  
このチュートリアルでは、Azure Active Directory と SpringCM の間でのシングル サインオンを設定する方法を説明します。
  
このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   SpringCM でのシングル サインオンが有効なサブスクリプション
  
このチュートリアルを完了すると、SpringCM に割り当てられている Azure Active Directory ユーザーは、AAD アクセス パネルを使用してシングル サインオンできます。

1.  SpringCM のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-spring-cm-tutorial/IC797044.png "Scenario")

##SpringCM のアプリケーション統合の有効化
  
このセクションでは、SpringCM のアプリケーション統合を有効にする方法について説明します。

###SpringCM のアプリケーション統合を有効にするには、次の手順に従います。

1.  左側のナビゲーション ウィンドウで、Azure 管理ポータルで、クリックして **Active Directory**します。

    ![Active Directory](./media/active-directory-saas-spring-cm-tutorial/IC700993.png "Active Directory")

2.   **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3.  ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。

    ![アプリケーション](./media/active-directory-saas-spring-cm-tutorial/IC700994.png "Applications")

4.  クリックして **追加** ページの下部にあります。

    ![アプリケーションの追加](./media/active-directory-saas-spring-cm-tutorial/IC749321.png "Add application")

5.   **実行する操作** ダイアログ ボックスで、をクリックして **ギャラリーからアプリケーションを追加**します。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-spring-cm-tutorial/IC749322.png "Add an application from gallerry")

6.   **検索ボックス**, 、型 **SpringCM**します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-spring-cm-tutorial/IC797045.png "Application Gallery")

7.  結果ウィンドウで [ **SpringCM**, 、クリックして **完了** アプリケーションを追加します。

    ![SpringCM](./media/active-directory-saas-spring-cm-tutorial/IC797046.png "SpringCM")

##シングル サインオンの構成
  
このセクションでは、SAML プロトコルに基づくフェデレーションを使用して、SpringCM で Azure AD のユーザー アカウントを使用してユーザーを認証できるようにする方法を説明します。

###シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **SpringCM** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、* * [シングル サインオン * * ダイアログ。

    ![シングル サインオンの構成](./media/active-directory-saas-spring-cm-tutorial/IC797047.png "Configure single Sign-On")

2.   **どのようなユーザーの SpringCM へのアクセス** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、] をクリックし、 **次**します。

    ![シングル サインオンの構成](./media/active-directory-saas-spring-cm-tutorial/IC797048.png "Configure single Sign-On")

3.   **アプリケーション URL の構成** ] ページで、 **SpringCM サインオン URL** 、SpringCM のアプリケーションにサインオンし、URL は、ユーザーが使用] ボックスに、型 **次**します。 

    アプリケーション URL は、SpringCM テナントの URL (例:: *https://na11.springcm.com/atlas/SSO/SSOEndpoint.ashx?aid=16826*)。

    ![アプリケーション URL の構成](./media/active-directory-saas-spring-cm-tutorial/IC797049.png "Configure App URL")

4.   **SpringCM でのシングル サインオンの構成** ] ページで、証明書をダウンロードする] をクリックして **証明書のダウンロード**, 、し、コンピューターに証明書ファイルをローカルに保存します。

    ![シングル サインオンの構成](./media/active-directory-saas-spring-cm-tutorial/IC797050.png "Configure Single SignOn")

5.  サインインする別の web ブラウザーのウィンドウで、 **SpringCM** 会社サイトに管理者として。

6.  上部にあるメニュー、[ **ジャンプ**, 、] をクリックして **設定**, 、[、 **アカウントの基本設定** ] をクリックして **SAML SSO**します。

    ![SAML SSO](./media/active-directory-saas-spring-cm-tutorial/IC797051.png "SAML SSO")

7.  [Identity Provider Configuration] セクションで、次の手順に従います。

    ![ID プロバイダー構成](./media/active-directory-saas-spring-cm-tutorial/IC797052.png "Identity Provider Configuration")

    1.  ダウンロードした Azure Active Directory 証明書をアップロードする] をクリックして **発行者証明書の選択** または **発行者証明書の変更**します。
    2.  Microsoft Azure ポータルで、 **SpringCM でのシングル サインオンの構成** ] ページで、コピー、 **発行者 URL** 値に設定して、貼り付けます、 **発行者** ] ボックスに貼り付けます。
    3.  Microsoft Azure ポータルで、 **SpringCM でのシングル サインオンの構成** ] ページで、コピー、 **シングル サインオン サービス URL** 値に設定して、貼り付けます、 **サービス プロバイダー (SP) が開始したエンドポイント** ] ボックスに貼り付けます。
    4.  として **SAML の有効化**, [ **を有効にする**です。
    5.  クリックして **保存**します。

8.  Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![シングル サインオンの構成](./media/active-directory-saas-spring-cm-tutorial/IC797053.png "Configure Single SignOn")

##ユーザー プロビジョニングの構成
  
Azure Active Directory ユーザーが SpringCM にログインできるようにするには、そのユーザーを SpringCM にプロビジョニングする必要があります。  
SpringCM の場合、プロビジョニングは手動で行います。

>[AZURE.NOTE] 詳細については、次を参照してください [を作成し、SpringCM ユーザーの編集。](http://knowledge.springcm.com/create-and-edit-a-springcm-user)

###ユーザー アカウントを SpringCM にプロビジョニングするには、次の手順に従います。

1.  ログインして **SpringCM** 会社サイトに管理者として。

2.  クリックして **GOTO**, 、] をクリックし、 **アドレス帳**します。

    ![Create User](./media/active-directory-saas-spring-cm-tutorial/IC797054.png "Create User")

3.  クリックして **ユーザー作成**します。

4.  選択、 **ユーザー ロール**します。

5.  選択 **アクティブ化して電子メールを送信**します。

6.  関連するテキスト ボックスに、プロビジョニングする有効な Azure Active Directory アカウントの姓名と電子メール アドレスを入力します。

7.  ユーザーを追加、 **セキュリティ グループ**します。

8.  クリックして **保存**します。

>[AZURE.NOTE] 他の SpringCM ユーザー アカウント作成ツールを使用することができますか、AAD ユーザー アカウントをプロビジョニング、SpringCM から提供されている Api です。

##ユーザーの割り当て
  
構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

###ユーザーを SpringCM に割り当てるには、次の手順に従います。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.   **SpringCM** アプリケーション統合ページで、をクリックして **ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-spring-cm-tutorial/IC797055.png "Assign Users")

3.  テスト ユーザーを選択して [ **割り当てる**, 、順にクリック **はい** して割り当てを確認します。

    ![あり](./media/active-directory-saas-spring-cm-tutorial/IC767830.png "Yes")
  
シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。





