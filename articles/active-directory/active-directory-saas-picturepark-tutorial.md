<properties 
    pageTitle="チュートリアル: Azure Active Directory と Picturepark の統合 | Microsoft Azure" 
    description="Azure Active Directory で Picturepark を使用して、シングル サインオンや自動プロビジョニングなどを有効にする方法について説明します。" 
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

#チュートリアル: Azure Active Directory と Picturepark の統合
  
このチュートリアルでは、Azure と Picturepark の統合について説明します。  
このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   Picturepark テナント
  
このチュートリアルを完了すると、Picturepark に割り当てた Azure AD ユーザーは、Picturepark 会社サイト (サービス プロバイダーが開始したサインオン) で、アプリケーションにシングル サインオンまたはできるを使用して、 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。
  
このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  Picturepark のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-picturepark-tutorial/IC795055.png "Scenario")
##Picturepark のアプリケーション統合の有効化
  
このセクションでは、Picturepark のアプリケーション統合を有効にする方法を説明します。

###Picturepark のアプリケーション統合を有効にするには、次の手順に従います。

1.  左側のナビゲーション ウィンドウで、Azure 管理ポータルで、クリックして **Active Directory**します。

    ![Active Directory](./media/active-directory-saas-picturepark-tutorial/IC700993.png "Active Directory")

2.   **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3.  ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。

    ![アプリケーション](./media/active-directory-saas-picturepark-tutorial/IC700994.png "Applications")

4.  クリックして **追加** ページの下部にあります。

    ![アプリケーションの追加](./media/active-directory-saas-picturepark-tutorial/IC749321.png "Add application")

5.   **実行する操作** ダイアログ ボックスで、をクリックして **ギャラリーからアプリケーションを追加**します。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-picturepark-tutorial/IC749322.png "Add an application from gallerry")

6.   **検索ボックス**, 、型 **Picturepark**します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-picturepark-tutorial/IC795056.png "Application Gallery")

7.  結果ウィンドウで [ **Picturepark**, 、クリックして **完了** アプリケーションを追加します。

    ![Picturepark](./media/active-directory-saas-picturepark-tutorial/IC795057.png "Picturepark")
##シングル サインオンの構成
  
このセクションでは、ユーザーが SAML プロトコルに基づくフェデレーションを使用して、Azure AD でのユーザーのアカウントで Picturepark に対する認証を行えるようにする方法を説明します。  
Picturepark にシングル サインオンを構成するには、証明書の拇印の値を取得する必要があります。  
この手順に慣れていない場合は、次を参照してください [証明書の拇印の値を取得する方法](http://youtu.be/YKQF266SAxI).。

###シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **Picturepark** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、* * [シングル サインオン * * ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-picturepark-tutorial/IC795058.png "Configure Single Sign-On")

2.   **どのようなユーザーの Picturepark へのアクセス** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、] をクリックし、 **次**します。

    ![Configure Single Sign-On](./media/active-directory-saas-picturepark-tutorial/IC795059.png "Configure Single Sign-On")

3.   **アプリケーション URL の構成** ] ページの [、 **Picturepark サインオン URL** ] ボックスに、次のパターンを使用して URL を入力"*http://company.picturepark.com*"、] をクリックし、 **次**します。

    ![Configure App URL](./media/active-directory-saas-picturepark-tutorial/IC795060.png "Configure App URL")

4.   **Picturepark でのシングル サインオンの構成** ] ページで、証明書をダウンロードする] をクリックして **証明書のダウンロード**, 、お使いのコンピューターにローカルで証明書ファイルを保存します。

    ![Configure Single Sign-On](./media/active-directory-saas-picturepark-tutorial/IC795061.png "Configure Single Sign-On")

5.  別の Web ブラウザーのウィンドウで、Picturepark 企業サイトに管理者としてログインします。

6.  上部にあるツールバーで、クリックして **管理ツール**, 、クリックして **管理コンソール**します。

    ![管理コンソール](./media/active-directory-saas-picturepark-tutorial/IC795062.png "Management Console")

7.  クリックして **認証**, 、] をクリックし、 **Id プロバイダー**します。

    ![認証](./media/active-directory-saas-picturepark-tutorial/IC795063.png "Authentication")

8.   **Id プロバイダーの構成** セクションで、次の手順に従います。

    ![ID プロバイダーの構成](./media/active-directory-saas-picturepark-tutorial/IC795064.png "Identity provider configuration")

    1.  クリックして **追加**します。
    2.  構成の名前を入力します。
    3.  選択 **既定として設定**します。
    4.  Azure ポータルで、 **Picturepark でのシングル サインオンの構成** ] ダイアログ ページでコピー、 **SAML SSO URL** 値に設定して、貼り付けます、 **発行者 URI** ] ボックスに貼り付けます。
    5.  コピー、 **拇印** エクスポートした証明書からの値し、貼り付けます、 **発行者のサムプリントを信頼できる** ] ボックスに貼り付けます。  

        >[AZURE.TIP]詳細については、次を参照してください [証明書の拇印の値を取得する方法。](http://youtu.be/YKQF266SAxI)

    6.  クリックして **JoinDefaultUsersGroup**します。
    7.  設定する、 **Emailaddress** 属性、 **クレーム** ] ボックスに「 **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**します。
        ![構成](./media/active-directory-saas-picturepark-tutorial/IC795065.png "Configuration")
    8.  クリックして **保存**します。

9.  Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-picturepark-tutorial/IC795066.png "Configure Single Sign-On")

##ユーザー プロビジョニングの構成
  
Azure AD ユーザーが Picturepark にログインできるようにするには、ユーザーを Picturepark にプロビジョニングする必要があります。  
Picturepark の場合、プロビジョニングは手動で行います。

###ユーザー アカウントをプロビジョニングするには、次の手順を実行します。

1.  ログインして **Picturepark** テナントです。

2.  上部にあるツールバーで、クリックして **管理ツール**, 、クリックして **ユーザー**します。

    ![ユーザー](./media/active-directory-saas-picturepark-tutorial/IC795067.png "Users")

3.   **ユーザーの概要** ] タブ、[ **新規**します。

    ![ユーザー管理](./media/active-directory-saas-picturepark-tutorial/IC795068.png "User management")

4.   **Create User** ] ダイアログ ボックスで、次の手順を実行します。

    ![Create User](./media/active-directory-saas-picturepark-tutorial/IC795069.png "Create User")

    1.  型、: **電子メール アドレス**, 、**パスワード**, 、**パスワードの確認**, 、**名**, 、**姓**, 、**会社**, 、**国**, 、**ZIP**, 、**City** ot、関連するテキスト ボックスにプロビジョニングする有効な Azure Active Directory ユーザーのです。
    2.  選択、 **言語**です。
    3.  クリックして **作成**します。

>[AZURE.NOTE]他の Picturepark ユーザー アカウント作成ツールを使用することができますか、AAD ユーザー アカウントをプロビジョニング、Picturepark から提供されている Api です。

##ユーザーの割り当て
  
構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

###ユーザーを Picturepark に割り当てるには、次の手順に従います。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  * * Picturepark * * アプリケーション統合ページで、をクリックして **ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-picturepark-tutorial/IC795070.png "Assign Users")

3.  テスト ユーザーを選択して [ **割り当てる**, 、順にクリック **はい** して割り当てを確認します。

    ![あり](./media/active-directory-saas-picturepark-tutorial/IC767830.png "Yes")
  
シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。

