<properties 
    pageTitle="チュートリアル: Azure Active Directory と TOPdesk - Public の統合 | Microsoft Azure" 
    description="Azure Active Directory で TOPdesk - Public を使用して、シングル サインオンを有効にする方法、プロビジョニングを自動化する方法などについて説明します。" 
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

#チュートリアル: Azure Active Directory と TOPdesk - Public の統合

このチュートリアルでは、Azure と TOPdesk - Public の統合について説明します。  
このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   TOPdesk - Public でのシングル サインオンが有効なサブスクリプション
  
このチュートリアルを完了すると、Azure AD ユーザーに割り当てた TOPdesk - TOPdesk - Public 会社サイト (サービス プロバイダーが開始したサインオン)、またはを使用して、アプリケーションにシングル サインオンできるパブリック、 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。
  
このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  TOPdesk - Public のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-topdesk-public-tutorial/IC790613.png "Scenario")

##TOPdesk - Public のアプリケーション統合の有効化
  
このセクションでは、TOPdesk - Public のアプリケーション統合を有効にする方法を説明します。

###TOPdesk - Public のアプリケーション統合を有効にするには、次の手順を実行します。

1.  左側のナビゲーション ウィンドウで、Azure 管理ポータルで、クリックして **Active Directory**します。

    ![Active Directory](./media/active-directory-saas-topdesk-public-tutorial/IC700993.png "Active Directory")

2.   **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3.  ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。

    ![アプリケーション](./media/active-directory-saas-topdesk-public-tutorial/IC700994.png "Applications")

4.  クリックして **追加** ページの下部にあります。

    ![アプリケーションの追加](./media/active-directory-saas-topdesk-public-tutorial/IC749321.png "Add application")

5.   **実行する操作** ダイアログ ボックスで、をクリックして **ギャラリーからアプリケーションを追加**します。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-topdesk-public-tutorial/IC749322.png "Add an application from gallerry")

6.   **検索ボックス**, 、型 **TOPdesk - Public**します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-topdesk-public-tutorial/IC790614.png "Application Gallery")

7.  結果ウィンドウで [ **TOPdesk - Public**, 、クリックして **完了** アプリケーションを追加します。

    ![TOPdesk Public](./media/active-directory-saas-topdesk-public-tutorial/IC791317.png "TOPdesk Public")

##シングル サインオンの構成
  
このセクションでは、ユーザーが SAML プロトコルに基づくフェデレーションを使用して、Azure AD でのユーザーのアカウントで TOPdesk - Public に対する認証を行うことができるようにする方法を説明します。  
TOPdesk - Public のシングル サインオンを構成するには、ロゴのアイコン ファイルをアップロードする必要があります。 アイコン ファイルを取得するには、TOPdesk サポート チームにお問い合わせください。

###シングル サインオンを構成するには、次の手順に従います。

1.  サインイン、 **TOPdesk - Public** 会社サイトに管理者として。

2.   **TOPdesk** ] メニューのをクリックして **設定**します。

    ![設定](./media/active-directory-saas-topdesk-public-tutorial/IC790598.png "Settings")

3.  クリックして **ログイン設定**します。

    ![ログイン設定](./media/active-directory-saas-topdesk-public-tutorial/IC790599.png "Login Settings")

4.  展開、 **ログイン設定** ] メニューの [クリックして **全般**します。

    ![全般](./media/active-directory-saas-topdesk-public-tutorial/IC790600.png "General")

5.   **パブリック** のセクションで、 **SAML ログイン** 構成セクションで、次の手順に従います。

    ![技術設定](./media/active-directory-saas-topdesk-public-tutorial/IC790601.png "Technical Settings")

    1.  クリックして **ダウンロード** 公開メタデータ ファイルをダウンロードしてローカル コンピューターに保存します。
    2.  メタデータ ファイルを開き、検索、 **AssertionConsumerService** ノードです。
        ![AssertionConsumerService](./media/active-directory-saas-topdesk-public-tutorial/IC790619.png "AssertionConsumerService")
    3.  コピー、 **AssertionConsumerService** 値。  

        >[AZURE.NOTE] 値は必要があります、 **アプリケーション URL の構成** このチュートリアルの後半のセクションです。

6.  ログインする別の web ブラウザーのウィンドウで、 **Azure Active Directory** 管理者としてポータルです。

7.   **TOPdesk - Public** アプリケーション統合ページでは、をクリックして **でのシングル サインオンを構成する** を開くには、* * [シングル サインオン * *] ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-topdesk-public-tutorial/IC790620.png "Configure Single Sign-On")

8.   **どのようなユーザーが TOPdesk - Public へのサインオン** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、] をクリックし、 **次**します。

    ![Configure Single Sign-On](./media/active-directory-saas-topdesk-public-tutorial/IC790621.png "Configure Single Sign-On")

9.   **アプリケーション URL の構成** ] ページで、次の手順を実行します。

    ![アプリケーション URL の構成](./media/active-directory-saas-topdesk-public-tutorial/IC790622.png "Configure App URL")

    1.   **TOPdesk - Public サインオン URL** URL は、ユーザーが TOPdesk - Public アプリケーションへのサインインに使用] ボックスに、型 (例::"*https://qssolutions.topdesk.net*") です。
    2.   **TOPdesk – Public 応答 URL** ] ボックスに、貼り付け、 **Topdesk-public AssertionConsumerService URL** (例::"*https://qssolutions.topdesk.net/tas/public/login/saml*")
    3.  クリックして **次**します。

10.  **TOPdesk - Public でのシングル サインオンの構成** ] ページで、メタデータ ファイルをダウンロードする] をクリックして **メタデータのダウンロード**, 、コンピューターのローカルに保存します。

    ![Configure Single Sign-On](./media/active-directory-saas-topdesk-public-tutorial/IC790623.png "Configure Single Sign-On")

11. 証明書ファイルを作成するには、次の手順を実行します。

    ![証明書](./media/active-directory-saas-topdesk-public-tutorial/IC790606.png "Certificate")

    1.  ダウンロードしたメタデータ ファイルを開きます。
    2.  展開、 **RoleDescriptor** を持つノード、 **xsi:type** の **給紙: デプロイ**します。
    3.  値をコピー、 **X509Certificate** ノードです。
    4.  保存、コピーした **X509Certificate** 値をファイルにコンピューターのローカルにします。

12. Topdesk - Public 会社サイトの [、 **TOPdesk** ] メニューのをクリックして **設定**します。

    ![設定](./media/active-directory-saas-topdesk-public-tutorial/IC790598.png "Settings")

13. クリックして **ログイン設定**します。

    ![ログイン設定](./media/active-directory-saas-topdesk-public-tutorial/IC790599.png "Login Settings")

14. 展開、 **ログイン設定** ] メニューの [クリックして **全般**します。

    ![全般](./media/active-directory-saas-topdesk-public-tutorial/IC790600.png "General")

15.  **パブリック** ] をクリックして **追加**します。

    ![SAML のログイン](./media/active-directory-saas-topdesk-public-tutorial/IC790625.png "SAML Login")

16.  **SAML 構成アシスタント** ] ダイアログ ページで、次の手順を実行します。

    ![SAML Configuration Assistant](./media/active-directory-saas-topdesk-public-tutorial/IC790608.png "SAML Configuration Assistant")

    1.  ダウンロードしたメタデータ ファイルをアップロードする **フェデレーション メタデータ**, 、クリックして **参照**します。
    2.  下にある、証明書ファイルをアップロードする **証明書 (RSA)**, 、クリックして **参照**します。
    3.  下にある、TOPdesk サポート チームから取得したロゴ ファイルをアップロードする **ロゴ アイコン**, 、クリックして **参照**します。
    4.   **ユーザー名属性** ] ボックスに「 **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**します。
    5.   **表示名** ] ボックスに、構成の名前を入力します。
    6.  クリックして **保存**します。

17. Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-topdesk-public-tutorial/IC790627.png "Configure Single Sign-On")

##ユーザー プロビジョニングの構成
  
Azure AD ユーザーが TOPdesk - Public にログインできるようにするには、そのユーザーを TOPdesk - Public にプロビジョニングする必要があります。  
TOPdesk - Public の場合、プロビジョニングは手動で行います。

###ユーザー プロビジョニングを構成するには、次の手順に従います。

1.  サインイン、 **TOPdesk - Public** 会社サイトに管理者として。

2.  上部にあるメニュー [ **TOPdesk \ > 新規 \ > サポート ファイル \ > ユーザー**します。

    ![Person](./media/active-directory-saas-topdesk-public-tutorial/IC790628.png "Person")

3.  [New Person] ダイアログで、次の手順を実行します。

    ![新しいユーザー](./media/active-directory-saas-topdesk-public-tutorial/IC790629.png "New Person")

    1.  [General] タブをクリックします。
    2.  [Surname] テキスト ボックスに、プロビジョニングする有効な Azure Active Directory アカウントの姓を入力します。
    3.  選択、 **サイト** アカウントにします。
    4.  クリックして **保存**します。

>[AZURE.NOTE] TOPdesk - Public AAD ユーザー アカウントをプロビジョニング、提供の Api や、他の TOPdesk - Public のユーザー アカウント作成ツールを使用できます。

##ユーザーの割り当て
  
構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

###ユーザーを TOPdesk - Public に割り当てるには、次の手順に従います。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  * * TOPdesk - Public * * アプリケーション統合ページで、をクリックして **ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-topdesk-public-tutorial/IC790630.png "Assign Users")

3.  テスト ユーザーを選択して [ **割り当てる**, 、順にクリック **はい** して割り当てを確認します。

    ![あり](./media/active-directory-saas-topdesk-public-tutorial/IC767830.png "Yes")
  
シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。
