<properties 
    pageTitle="チュートリアル: Azure Active Directory と TimeOffManager の統合 | Microsoft Azure" 
    description="Azure Active Directory で TimeOffManager を使用して、シングル サインオンや自動プロビジョニングなどを有効にする方法を説明します。" 
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

#チュートリアル: Azure Active Directory と TimeOffManager の統合
  
このチュートリアルでは、Azure と TimeOffManager の統合について説明します。  
このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   TimeOffManager でのシングル サインオンが有効なサブスクリプション
  
このチュートリアルを完了すると、TimeOffManager に割り当てた Azure AD ユーザーは、TimeOffManager 会社サイト (サービス プロバイダーが開始したサインオン) で、アプリケーションにシングル サインオンまたはできるを使用して、 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。
  
このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  TimeOffManager のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-timeoffmanager-tutorial/IC795909.png "Scenario")

##TimeOffManager のアプリケーション統合の有効化
  
このセクションでは、TimeOffManager のアプリケーション統合を有効にする方法を説明します。

###TimeOffManager のアプリケーション統合を有効にするには、次の手順を実行します。

1.  左側のナビゲーション ウィンドウで、Azure 管理ポータルで、クリックして **Active Directory**します。

    ![Active Directory](./media/active-directory-saas-timeoffmanager-tutorial/IC700993.png "Active Directory")

2.   **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3.  ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。

    ![アプリケーション](./media/active-directory-saas-timeoffmanager-tutorial/IC700994.png "Applications")

4.  クリックして **追加** ページの下部にあります。

    ![アプリケーションの追加](./media/active-directory-saas-timeoffmanager-tutorial/IC749321.png "Add application")

5.   **実行する操作** ダイアログ ボックスで、をクリックして **ギャラリーからアプリケーションを追加**します。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-timeoffmanager-tutorial/IC749322.png "Add an application from gallerry")

6.   **検索ボックス**, 、型 **TimeOffManager**します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-timeoffmanager-tutorial/IC795910.png "Application Gallery")

7.  結果ウィンドウで [ **TimeOffManager**, 、クリックして **完了** アプリケーションを追加します。

    ![TimeOffManager](./media/active-directory-saas-timeoffmanager-tutorial/IC795911.png "TimeOffManager")

##シングル サインオンの構成
  
このセクションでは、ユーザーが SAML プロトコルに基づくフェデレーションを使用して、Azure AD でのユーザーのアカウントで TimeOffManager に対する認証を行うことができるようにする方法を説明します。  
この手順の途中で、base-64 でエンコードされた証明書を TimeOffManager テナントにアップロードする必要があります。  
この手順に慣れていない場合は、次を参照してください [バイナリ証明書をテキスト ファイルに変換する方法。](http://youtu.be/PlgrzUZ-Y1o)

###シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **TimeOffManager** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、* * [シングル サインオン * * ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-timeoffmanager-tutorial/IC795912.png "Configure Single Sign-On")

2.   **どのようなユーザーの TimeOffManager へのアクセス** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、] をクリックし、 **次**します。

    ![Configure Single Sign-On](./media/active-directory-saas-timeoffmanager-tutorial/IC795913.png "Configure Single Sign-On")

3.   **アプリケーション URL の構成** ] ページの [、 **TimeOffManager 応答 URL** ] ボックスに、TimeOffManager AssertionConsumerService URL を入力 (例::"*例: https://www.timeoffmanager.com/cpanel/sso/consume.aspx?company \_id=IC34216*"、順にクリック **次へ]**します。

    ![Configure App URL](./media/active-directory-saas-timeoffmanager-tutorial/IC795914.png "Configure App URL")

    応答 URL は、設定ページの [TimeOffManager シングル サインオン] から入手できます。

    ![シングル サインオンの設定](./media/active-directory-saas-timeoffmanager-tutorial/IC795915.png "Single Sign-On Settings")

4.   **TimeOffManager でのシングル サインオン構成** ] ページで、証明書をダウンロードする] をクリックして **証明書のダウンロード**, 、お使いのコンピューターに証明書ファイルを保存します。

    ![Configure Single Sign-On](./media/active-directory-saas-timeoffmanager-tutorial/IC795916.png "Configure Single Sign-On")

5.  別の Web ブラウザーのウィンドウで、管理者として TimeOffManager 企業サイトにログインします。

6.  移動して **アカウント \ > アカウント オプション \ > Single Sign On Settings**します。

    ![シングル サインオンの設定](./media/active-directory-saas-timeoffmanager-tutorial/IC795917.png "Single Sign-On Settings")

7.   **シングル サインオン設定** セクションで、次の手順に従います。

    ![シングル サインオンの設定](./media/active-directory-saas-timeoffmanager-tutorial/IC795918.png "Single Sign-On Settings")

    1.  作成、 **base-64 でエンコードされた** からダウンロードした証明書ファイル。  

        >[AZURE.TIP] 詳細については、次を参照してください [バイナリ証明書をテキスト ファイルに変換する方法。](http://youtu.be/PlgrzUZ-Y1o)

    2.  Base-64 でエンコードされた証明書をメモ帳で開き、内容をクリップボードにコピーおよび貼り付けて、全体の証明書を **X.509 証明書** ] ボックスに貼り付けます。
    3.  Azure ポータルで、 **TimeOffManager でのシングル サインオンの構成** ] ダイアログ ページでコピー、 **発行者 URL** 値に設定して、貼り付けます、 **Idp 発行者** ] ボックスに貼り付けます。
    4.  Azure ポータルで、 **TimeOffManager でのシングル サインオンの構成** ] ダイアログ ページでコピー、 **リモート ログイン URL** 値に設定して、貼り付けます、 **IdP エンドポイント URL** ] ボックスに貼り付けます。
    5.  として **SAML の適用**, [ **いいえ**します。
    6.  として **ユーザーの自動作成**, [ **はい**します。
    7.  Azure ポータルで、 **TimeOffManager でのシングル サインオンの構成** ] ダイアログ ページでコピー、 **リモート ログアウト URL** 値に設定して、貼り付けます、 **ログアウト URL** ] ボックスに貼り付けます。
    8.  クリックして **変更を保存**します。

8.  Azure ポータルで、 **TimeOffManager でのシングル サインオンの構成** ] ダイアログ ページで、シングル サインオンの構成情報を選択し、をクリックして **完了**します。

    ![Configure Single Sign-On](./media/active-directory-saas-timeoffmanager-tutorial/IC795919.png "Configure Single Sign-On")

9.  上部にあるメニュー [ **属性** を開くには、 **SAML トークン属性** ダイアログ。

    ![属性](./media/active-directory-saas-timeoffmanager-tutorial/IC795920.png "Attributes")

10. 必要な属性のマッピングを追加するには、次の手順を実行します。

    ![SAML トークンの属性](./media/active-directory-saas-timeoffmanager-tutorial/IC795921.png "saml token attributes")

  	|属性名|属性値|
  	|---|---|
  	|Firstname|User.givenname|
  	|Lastname|User.surname|

    1.  上記の表の各データ行をクリックして **ユーザー属性の追加**します。
    2.   **属性名** ] ボックスに、その行に対して表示される属性名を入力します。
    3.   **属性値** ] ボックスに、その行に対して表示される属性の値を選択します。
    4.  クリックして **完了**します。

11. クリックして **の変更を適用**します。

##ユーザー プロビジョニングの構成
  
Azure AD ユーザーが TimeOffManager にログインできるようにするには、そのユーザーを TimeOffManager にプロビジョニングする必要があります。  
TimeOffManager は、ジャストインタイム ユーザー プロビジョニングをサポートしています。 ユーザー側で必要な操作はありません。  
シングル サインオンを使用して初めてログインするときに、ユーザーが自動的に追加されます。

>[AZURE.NOTE] 他の TimeOffManager ユーザー アカウント作成ツールを使用することができますか、AAD ユーザー アカウントをプロビジョニング、TimeOffManager から提供されている Api です。

##ユーザーの割り当て
  
構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

###ユーザーを TimeOffManager に割り当てるには、次の手順を実行します。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.   **TimeOffManager** アプリケーション統合ページで、をクリックして **ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-timeoffmanager-tutorial/IC795922.png "Assign Users")

3.  テスト ユーザーを選択して [ **割り当てる**, 、順にクリック **はい** して割り当てを確認します。

    ![あり](./media/active-directory-saas-timeoffmanager-tutorial/IC767830.png "Yes")
  
シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。


