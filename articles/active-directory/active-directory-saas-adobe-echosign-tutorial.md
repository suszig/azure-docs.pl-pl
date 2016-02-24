<properties 
    pageTitle="チュートリアル: Azure Active Directory と  Adobe EchoSign の統合 | Microsoft Azure" 
    description="Azure Active Directory で Adobe EchoSign を使用して、シングル サインオンや自動プロビジョニングなどを有効にする方法について説明します。" 
    services="active-directory" authors="markusvi"  
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

#チュートリアル: Azure Active Directory と Adobe EchoSign の統合

このチュートリアルでは、Azure と Adobe EchoSign の統合について説明します。  
このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   Adobe EchoSign でのシングル サインオンが有効なサブスクリプション

このチュートリアルを完了すると、Adobe EchoSign に割り当てた Azure AD ユーザーは、Adobe EchoSign 会社サイト (サービス プロバイダーが開始したサインオン) で、アプリケーションにシングル サインオンまたはできるを使用して、 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。

このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  Adobe EchoSign のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-adobe-echosign-tutorial/IC789511.png "Scenario")
##Adobe EchoSign のアプリケーション統合の有効化

このセクションでは、Adobe EchoSign のアプリケーション統合を有効にする方法を説明します。

###Adobe EchoSign のアプリケーション統合を有効にするには、次の手順に従います。

1.  左側のナビゲーション ウィンドウで、Azure 管理ポータルで、クリックして **Active Directory**します。

    ![Active Directory](./media/active-directory-saas-adobe-echosign-tutorial/IC700993.png "Active Directory")

2.   **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3.  ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。

    ![アプリケーション](./media/active-directory-saas-adobe-echosign-tutorial/IC700994.png "Applications")

4.  クリックして **追加** ページの下部にあります。

    ![アプリケーションの追加](./media/active-directory-saas-adobe-echosign-tutorial/IC749321.png "Add application")

5.   **実行する操作** ダイアログ ボックスで、をクリックして **ギャラリーからアプリケーションを追加**します。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-adobe-echosign-tutorial/IC749322.png "Add an application from gallerry")

6.   **検索ボックス**, 、型 **Adobe EchoSign**します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-adobe-echosign-tutorial/IC789514.png "Application Gallery")

7.  結果ウィンドウで [ **Adobe EchoSign**, 、クリックして **完了** アプリケーションを追加します。

    ![Adobe EchoSign](./media/active-directory-saas-adobe-echosign-tutorial/IC789515.png "Adobe EchoSign")
##シングル サインオンの構成

このセクションでは、ユーザーが SAML プロトコルに基づくフェデレーションを使用して、Azure AD でのユーザーのアカウントで Adobe EchoSign に対する認証を行えるようにする方法を説明します。  
この手順の途中で、base-64 でエンコードされた証明書ファイルを作成する必要があります。  
この手順に慣れていない場合は、次を参照してください。 [バイナリ証明書をテキスト ファイルに変換する方法](http://youtu.be/PlgrzUZ-Y1o)します。

###シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **Adobe EchoSign** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、* * [シングル サインオン * * ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-adobe-echosign-tutorial/IC789516.png "Configure Single Sign-On")

2.   **どのようなユーザーの Adobe EchoSign に** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、] をクリックし、 **次**します。

    ![Configure Single Sign-On](./media/active-directory-saas-adobe-echosign-tutorial/IC789517.png "Configure Single Sign-On")

3.   **アプリケーション URL の構成** ] ページの [、 **Adobe EchoSign サインオン URL** ] ボックスに、次のパターンを使用して URL を入力"*https://company.echosign.com/*"、] をクリックし、 **次**します。

    ![Configure App URL](./media/active-directory-saas-adobe-echosign-tutorial/IC789518.png "Configure App URL")

4.   **Adobe EchoSign でのシングル サインオンの構成** ] ページで [ **証明書のダウンロード**, 、お使いのコンピューターに証明書ファイルを保存します。

    ![Configure Single Sign-On](./media/active-directory-saas-adobe-echosign-tutorial/IC789519.png "Configure Single Sign-On")

5.  別の Web ブラウザーのウィンドウで、管理者として Adobe EchoSign 企業サイトにログインします。

6.  上部にあるメニュー、[ **アカウント**, 、順を左側のナビゲーション ウィンドウで次のようにクリックします。 **SAML 設定** [ **アカウントの設定**します。

    ![アカウント](./media/active-directory-saas-adobe-echosign-tutorial/IC789520.png "Account")

7.  [SAML 設定] セクションで、次の手順に従います。

    ![SAML 設定](./media/active-directory-saas-adobe-echosign-tutorial/IC789521.png "SAML Settings")

    1.  として **SAML モード**, [ **必須の SAML**します。
    2.  選択 **EchoSign 資格情報を使用してログインを EchoSign アカウント管理者を許可する**です。
    3.  として **ユーザーの作成**, [ **SAML を通して認証されたユーザーを自動的に追加**します。

8.  次に、以下の手順に進みます。

    ![SAML 設定](./media/active-directory-saas-adobe-echosign-tutorial/IC789522.png "SAML Settings")

    1.  Azure ポータルで、 **Adobe EchoSign でのシングル サインオンの構成** ] ダイアログ ページでコピー、 **エンティティ ID** 値に設定して、貼り付けます、 **IdP エンティティ ID** ] ボックスに貼り付けます。
    2.  Azure ポータルで、 **Adobe EchoSign でのシングル サインオンの構成** ] ダイアログ ページでコピー、 **リモート ログイン URL** 値に設定して、貼り付けます、 **IdP ログイン URL** ] ボックスに貼り付けます。
    3.  Azure ポータルで、 **Adobe EchoSign でのシングル サインオンの構成** ] ダイアログ ページでコピー、 **リモート ログアウト URL** 値に設定して、貼り付けます、 **IdP ログアウト URL** ] ボックスに貼り付けます。
    4.  作成、 **base-64 でエンコードされた** からダウンロードした証明書ファイル。  

        >[AZURE.TIP] 詳細については、次を参照してください [バイナリ証明書をテキスト ファイルに変換する方法。](http://youtu.be/PlgrzUZ-Y1o)
    5.Base-64 でエンコードされた証明書をメモ帳で開き、という内容がクリップボードにコピーおよびに貼り付け、 **IdP 証明書** ] テキスト ボックス
    6.クリックして **変更を保存**します。

9.  Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-adobe-echosign-tutorial/IC789523.png "Configure Single Sign-On")
##ユーザー プロビジョニングの構成

Azure AD ユーザーが Adobe EchoSign にログインできるようにするには、そのユーザーを Adobe EchoSign にプロビジョニングする必要があります。  
Adobe EchoSign の場合、プロビジョニングは手動で実行します。

###ユーザー アカウントをプロビジョニングするには、次の手順を実行します。

1.  ログインして **Adobe EchoSign** 会社サイトに管理者として。

2.  上部にあるメニュー、[ **アカウント**, 、順を左側のナビゲーション ウィンドウで次のようにクリックします。 **ユーザーおよびグループ**, 、] をクリック **新しいユーザーを作成**します。

    ![アカウント](./media/active-directory-saas-adobe-echosign-tutorial/IC789524.png "Account")

3.   **新規ユーザーを作成** セクションで、次の手順に従います。

    ![Create User](./media/active-directory-saas-adobe-echosign-tutorial/IC789525.png "Create User")

    1.  型、 **電子メール アドレス**, 、**名** と **姓** 関連テキスト ボックスに、プロビジョニングする有効な AAD アカウントのです。
    2.  クリックして **ユーザー作成**します。

        >[AZURE.NOTE] Azure Active Directory アカウント所有者には、アクティブ化する前に、アカウントの確認へのリンクを含むメールを受け取ります。

>[AZURE.NOTE] 他の Adobe EchoSign ユーザー アカウント作成ツールを使用することができますか、AAD ユーザー アカウントをプロビジョニング、Adobe EchoSign から提供されている Api です。

##ユーザーの割り当て

構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

###Adobe EchoSign にユーザーを割り当てるには、次の手順を実行します。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  * * Adobe EchoSign * * アプリケーション統合ページで、をクリックして **ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-adobe-echosign-tutorial/IC789526.png "Assign Users")

3.  テスト ユーザーを選択して [ **割り当てる**, 、順にクリック **はい** して割り当てを確認します。

    ![あり](./media/active-directory-saas-adobe-echosign-tutorial/IC767830.png "Yes")

シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。

