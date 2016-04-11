<properties 
    pageTitle="チュートリアル: Azure Active Directory と Zoom の統合 | Microsoft Azure" 
    description="Azure Active Directory で Zoom を使用して、シングル サインオンや自動プロビジョニングなどを有効にする方法について説明します。" 
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

#チュートリアル: Azure Active Directory と Zoom の統合
  
このチュートリアルでは、Azure と Zoom の統合について説明します。  
このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   Zoom のテナント
  
このチュートリアルを完了すると、Zoom に割り当てた Azure AD ユーザーは、Zoom 会社サイト (サービス プロバイダーが開始したサインオン) で、アプリケーションにシングル サインオンまたはできるを使用して、 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)
  
このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  Zoom のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-zoom-tutorial/IC784693.png "Scenario")

##Zoom のアプリケーション統合の有効化
  
このセクションでは、Zoom のアプリケーション統合を有効にする方法について説明します。

###Zoom のアプリケーション統合を有効にするには、次の手順を実行します。

1.  左側のナビゲーション ウィンドウで、Azure 管理ポータルで、クリックして **Active Directory**します。

    ![Active Directory](./media/active-directory-saas-zoom-tutorial/IC700993.png "Active Directory")

2.   **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3.  ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。

    ![アプリケーション](./media/active-directory-saas-zoom-tutorial/IC700994.png "Applications")

4.  クリックして **追加** ページの下部にあります。

    ![アプリケーションの追加](./media/active-directory-saas-zoom-tutorial/IC749321.png "Add application")

5.   **実行する操作** ダイアログ ボックスで、をクリックして **ギャラリーからアプリケーションを追加**します。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-zoom-tutorial/IC749322.png "Add an application from gallerry")

6.   **検索ボックス**, 、型 **ズーム**します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-zoom-tutorial/IC784694.png "Application Gallery")

7.  結果ウィンドウで [ **ズーム**, 、] をクリックし、 **完了** アプリケーションを追加します。

    ![Zoom](./media/active-directory-saas-zoom-tutorial/IC784695.png "Zoom")

##シングル サインオンの構成
  
このセクションでは、ユーザーが SAML プロトコルに基づくフェデレーションを使用して、Azure AD でのユーザーのアカウントで Zoom に対する認証を行うことができるようにする方法を説明します。  
この手順の途中で、base-64 でエンコードされた証明書ファイルを作成する必要があります。  
この手順に慣れていない場合は、次を参照してください。 [バイナリ証明書をテキスト ファイルに変換する方法](http://youtu.be/PlgrzUZ-Y1o)します。

###シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **ズーム** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、* * [シングル サインオン * * ダイアログ。

    ![Configure single sign-on](./media/active-directory-saas-zoom-tutorial/IC784696.png "Configure single sign-on")

2.   **どのようなズームをサインオン** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、順にクリック **次**します。

    ![Configure single sign-on](./media/active-directory-saas-zoom-tutorial/IC784697.png "Configure single sign-on")

3.   **アプリケーション URL の構成** ] ページの [、 **Zoom サインイン URL** ] ボックスに、次のパターンを使用して URL を入力"*http://company.zoom.us*"、] をクリックし、 **次**します。

    ![Configure App URL](./media/active-directory-saas-zoom-tutorial/IC784698.png "Configure App URL")

4.   **Zoom でのシングル サインオン構成** ] ページで [ **証明書のダウンロード**, 、コンピューターに証明書ファイルを保存します。

    ![Configure single sign-on](./media/active-directory-saas-zoom-tutorial/IC784699.png "Configure single sign-on")

5.  別の Web ブラウザー ウィンドウで、Zoom 企業サイトに管理者としてログインします。

6.  クリックして、 **でのシングル サインオン** ] タブをクリックします。

    ![シングル サインオン](./media/active-directory-saas-zoom-tutorial/IC784700.png "Single sign-on")

7.  をクリックして、 **セキュリティ制御** タブをクリックしに移動し、 **でのシングル サインオン** 設定します。

8.  [シングル サインオン] セクションで、次の手順に従います。

    ![シングル サインオン](./media/active-directory-saas-zoom-tutorial/IC784701.png "Single sign-on")

    1.  Azure ポータルでの **Zoom でのシングル サインオン構成** ] ダイアログ ページでコピー、 **シングル サインオン サービス URL** 値に設定して、貼り付けます、 **サインイン ページ URL** ] ボックスに貼り付けます。
    2.  Azure ポータルでの **Zoom でのシングル サインオン構成** ] ダイアログ ページでコピー、 **シングル サインアウト サービス URL** 値に設定して、貼り付けます、 **サインアウト ページ URL** ] ボックスに貼り付けます。
    3.  作成、 **base-64 でエンコードされた** からダウンロードした証明書ファイル。  

        >[AZURE.TIP] 詳細については、次を参照してください [バイナリ証明書をテキスト ファイルに変換する方法。](http://youtu.be/PlgrzUZ-Y1o)

    4.  Base-64 でエンコードされた証明書をメモ帳で開き、という内容がクリップボードにコピーおよびに貼り付け、 **Id プロバイダー証明書** ] テキスト ボックス
    5.  Azure ポータルでの **Zoom でのシングル サインオン構成** ] ダイアログ ページでコピー、 **発行者 URL** 値に設定して、貼り付けます、 **発行者** ] ボックスに貼り付けます。
    6.  クリックして **保存**します。

9.  Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![シングル サインオンの構成](./media/active-directory-saas-zoom-tutorial/IC784702.png "Configure single sign-on")

##ユーザー プロビジョニングの構成
  
Azure AD ユーザーが Zoom にログインできるようにするには、ユーザーを Zoom にプロビジョニングする必要があります。  
Zoom の場合、プロビジョニングは手動で行います。

###ユーザー アカウントをプロビジョニングするには、次の手順を実行します。

1.  ログインして **ズーム** 会社サイトに管理者として。

2.  クリックして、 **アカウント管理** タブをクリックし、をクリックして **ユーザー管理**します。

3.  [ユーザーの管理] の [ **ユーザーを追加する**です。

    ![ユーザー管理](./media/active-directory-saas-zoom-tutorial/IC784703.png "User management")

4.   **ユーザーを追加する** ] ページで、次の手順を実行します。

    ![Add users (ユーザーを追加する)](./media/active-directory-saas-zoom-tutorial/IC784704.png "Add users")

    1.  として **ユーザー タイプ**, [ **基本的な**です。
    2.   **電子メール** ] ボックスに、プロビジョニングする有効な AAD アカウントの電子メール アドレスを入力します。
    3.  クリックして **追加**します。

>[AZURE.NOTE] 他の Zoom ユーザー アカウント作成ツールを使ったり、提供されている Api Zoom にプロビジョニング AAD からユーザー アカウント。

##ユーザーの割り当て
  
構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

###ユーザーを Zoomｖに割り当てるには、次の手順を実行します。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  * * ズーム * * アプリケーション統合ページで、をクリックして **ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-zoom-tutorial/IC784705.png "Assign users")

3.  テスト ユーザーを選択して [ **割り当てる**, 、順にクリック **はい** して割り当てを確認します。

    ![あり](./media/active-directory-saas-zoom-tutorial/IC767830.png "Yes")
  
シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。

