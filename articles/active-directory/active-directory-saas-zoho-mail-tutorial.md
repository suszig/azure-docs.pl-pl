<properties 
    pageTitle="チュートリアル: Azure Active Directory と Zoho Mail の統合 | Microsoft Azure" 
    description="Azure Active Directory で Zoho Mail を使用して、シングル サインオンや自動プロビジョニングなどを有効にする方法について説明します。" 
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

#チュートリアル: Azure Active Directory と Zoho Mail の統合
  
このチュートリアルでは、Azure と Zoho Mail の統合について説明します。  
このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   Zoho Mail テナント
  
このチュートリアルを完了すると、Zoho メールに割り当てた Azure AD ユーザーは、Zoho メールの会社サイト (サービス プロバイダーが開始したサインオン) で、アプリケーションにシングル サインオンまたはできるを使用して、 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。
  
このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  Zoho Mail のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-zoho-mail-tutorial/IC789600.png "Scenario")

##Zoho Mail のアプリケーション統合の有効化
  
このセクションでは、Zoho Mail のアプリケーション統合を有効にする方法を説明します。

###Zoho Mail のアプリケーション統合を有効にするには、次の手順を実行します。

1.  左側のナビゲーション ウィンドウで、Azure 管理ポータルで、クリックして **Active Directory**します。

    ![Active Directory](./media/active-directory-saas-zoho-mail-tutorial/IC700993.png "Active Directory")

2.   **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3.  ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。

    ![アプリケーション](./media/active-directory-saas-zoho-mail-tutorial/IC700994.png "Applications")

4.  クリックして **追加** ページの下部にあります。

    ![アプリケーションの追加](./media/active-directory-saas-zoho-mail-tutorial/IC749321.png "Add application")

5.   **実行する操作** ダイアログ ボックスで、をクリックして **ギャラリーからアプリケーションを追加**します。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-zoho-mail-tutorial/IC749322.png "Add an application from gallerry")

6.   **検索ボックス**, 、型 **Zoho メール**します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-zoho-mail-tutorial/IC789601.png "Application Gallery")

7.  結果ウィンドウで [ **Zoho メール**, 、クリックして **完了** アプリケーションを追加します。

    ![Zoho Mail](./media/active-directory-saas-zoho-mail-tutorial/IC789602.png "Zoho Mail")

##シングル サインオンの構成
  
このセクションでは、SAML プロトコルに基づくフェデレーションを使用して、ユーザーが Azure AD のアカウントで Zoho Mail に対する認証を行うことができるようにする方法を説明します。  
この手順の途中で、base-64 でエンコードされた証明書ファイルを作成する必要があります。  
この手順に慣れていない場合は、次を参照してください。 [バイナリ証明書をテキスト ファイルに変換する方法](http://youtu.be/PlgrzUZ-Y1o)します。

###シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **Zoho メール** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、* * [シングル サインオン * * ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-zoho-mail-tutorial/IC789603.png "Configure Single Sign-On")

2.   **どのようなユーザーが Zoho メールへのサインオン** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、] をクリックし、 **次**します。

    ![Configure Single Sign-On](./media/active-directory-saas-zoho-mail-tutorial/IC789604.png "Configure Single Sign-On")

3.   **アプリケーション URL の構成** ] ページの [、 **Zoho メール サインオン URL** ] ボックスに、次のパターンを使用して URL を入力"*http://company.ZohoMail.com*"、] をクリックし、 **次**します。

    ![Configure App URL](./media/active-directory-saas-zoho-mail-tutorial/IC789605.png "Configure App URL")

4.   **Zoho メールでのシングル サインオンの構成** ] ページで [ **証明書のダウンロード**, 、お使いのコンピューターに証明書ファイルを保存します。

    ![Configure Single Sign-On](./media/active-directory-saas-zoho-mail-tutorial/IC789606.png "Configure Single Sign-On")

5.  別の Web ブラウザー ウィンドウで、Zoho Mail 企業サイトに管理者としてログインします。

6.  移動して、 **コントロール パネルの [**します。

    ![コントロール パネル](./media/active-directory-saas-zoho-mail-tutorial/IC789607.png "Control Panel")

7.  クリックして、 **SAML 認証** ] タブをクリックします。

    ![SAML 認証](./media/active-directory-saas-zoho-mail-tutorial/IC789608.png "SAML Authentication")

8.   **SAML 認証の詳細** セクションで、次の手順に従います。

    ![SAML 認証の詳細](./media/active-directory-saas-zoho-mail-tutorial/IC789609.png "SAML Authentication Details")

    1.  Azure ポータルで、 **Zoho メールでのシングル サインオンの構成** ] ダイアログ ページでコピー、 **リモート ログイン URL** 値に設定して、貼り付けます、 **ログイン URL** ] ボックスに貼り付けます。
    2.  Azure ポータルで、 **Zoho メールでのシングル サインオンの構成** ] ダイアログ ページでコピー、 **リモート ログアウト URL** 値に設定して、貼り付けます、 **ログアウト URL** ] ボックスに貼り付けます。
    3.  Azure ポータルで、 **Zoho メールでのシングル サインオンの構成** ] ダイアログ ページでコピー、 **パスワード変更 URL** 値に設定して、貼り付けます、 **パスワード変更 URL** ] ボックスに貼り付けます。
    4.  作成、 **base-64 でエンコードされた** からダウンロードした証明書ファイル。  

        >[AZURE.TIP] 詳細については、次を参照してください [バイナリ証明書をテキスト ファイルに変換する方法。](http://youtu.be/PlgrzUZ-Y1o)

    5.  Base-64 でエンコードされた証明書をメモ帳で開き、という内容がクリップボードにコピーおよびに貼り付け、 **PublicKey** ] ボックスに貼り付けます。
    6.  として **アルゴリズム**, [ **RSA**します。
    7.  Click **OK**.

9.  Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-zoho-mail-tutorial/IC789610.png "Configure Single Sign-On")

##ユーザー プロビジョニングの構成
  
Azure AD ユーザーが Zoho Mail にログインできるようにするには、そのユーザーを Zoho Mail にプロビジョニングする必要があります。  
Zoho Mail の場合、プロビジョニングは手動で行います。

###ユーザー アカウントをプロビジョニングするには、次の手順を実行します。

1.  ログインして **Zoho メール** 会社サイトに管理者として。

2.  移動して **コントロール パネル \ > メールおよび Docs**します。

3.  移動して **ユーザーの詳細 \ > ユーザーの追加**します。

    ![ユーザーの追加](./media/active-directory-saas-zoho-mail-tutorial/IC789611.png "Add User")

4.   **ユーザーを追加する** ] ダイアログ ボックスで、次の手順を実行します。

    ![ユーザーの追加](./media/active-directory-saas-zoho-mail-tutorial/IC789612.png "Add User")

    1.  型、 **名**, 、**姓**, 、**電子メール ID**, 、**パスワード** 関連テキスト ボックスに、プロビジョニングする有効な Azure Active Directory アカウントのです。
    2.  Click **OK**.  

        >[AZURE.NOTE] Azure Active Directory アカウント所有者は、アクティブ化する前に、アカウントを確認するためのリンク電子メールで送信されます。

>[AZURE.NOTE] 他の Zoho メール ユーザー アカウント作成ツールを使用するまたは AAD ユーザー アカウントをプロビジョニング、Zoho メールで提供されている Api です。

##ユーザーの割り当て
  
構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

###ユーザーを Zoho Mail に割り当てるには、次の手順を実行します。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  * * Zoho メール * * アプリケーション統合ページで、をクリックして **ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-zoho-mail-tutorial/IC789613.png "Assign Users")

3.  テスト ユーザーを選択して [ **割り当てる**, 、順にクリック **はい** して割り当てを確認します。

    ![あり](./media/active-directory-saas-zoho-mail-tutorial/IC767830.png "Yes")
  
シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。

