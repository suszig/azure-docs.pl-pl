<properties 
    pageTitle="チュートリアル: Azure Active Directory と Mozy Enterprise の統合 | Microsoft Azure" 
    description="Azure Active Directory で Mozy Enterprise を使用して、シングル サインオンや自動プロビジョニングなどを有効にする方法について説明します。" 
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

#チュートリアル: Azure Active Directory と Mozy Enterprise の統合
  
このチュートリアルでは、Azure と Mozy Enterprise の統合について説明します。  
このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   Mozy Enterprise テナント
  
このチュートリアルを完了すると、Mozy Enterprise に割り当てた Azure AD ユーザーは、Mozy Enterprise 会社サイト (サービス プロバイダーが開始したサインオン) で、アプリケーションにシングル サインオンまたはできるを使用して、 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。
  
このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  Mozy Enterprise のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-mozy-enterprise-tutorial/IC777308.png "Scenario")
##Mozy Enterprise のアプリケーション統合の有効化
  
このセクションでは、Mozy Enterprise のアプリケーション統合を有効にする方法を説明します。

###Mozy Enterprise のアプリケーション統合を有効にするには、次の手順に従います。

1.  左側のナビゲーション ウィンドウで、Azure 管理ポータルで、クリックして **Active Directory**します。

    ![Active Directory](./media/active-directory-saas-mozy-enterprise-tutorial/IC700993.png "Active Directory")

2.   **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3.  ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。

    ![アプリケーション](./media/active-directory-saas-mozy-enterprise-tutorial/IC700994.png "Applications")

4.  クリックして **追加** ページの下部にあります。

    ![アプリケーションの追加](./media/active-directory-saas-mozy-enterprise-tutorial/IC749321.png "Add application")

5.   **実行する操作** ダイアログ ボックスで、をクリックして **ギャラリーからアプリケーションを追加**します。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-mozy-enterprise-tutorial/IC749322.png "Add an application from gallerry")

6.   **検索ボックス**, 、型 **mozy enterprise**します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-mozy-enterprise-tutorial/IC777309.png "Application Gallery")

7.  結果ウィンドウで [ **Mozy Enterprise**, 、クリックして **完了** アプリケーションを追加します。

    ![Mozy Enterprise](./media/active-directory-saas-mozy-enterprise-tutorial/IC777310.png "Mozy Enterprise")
##シングル サインオンの構成
  
このセクションでは、ユーザーが SAML プロトコルに基づくフェデレーションを使用して、Azure AD でのユーザーのアカウントで Mozy Enterprise に対する認証を行うことができるようにする方法を説明します。  
この手順の途中で、base-64 でエンコードされた証明書を Mozy Enterprise テナントにアップロードする必要があります。  
この手順に慣れていない場合は、次を参照してください [バイナリ証明書をテキスト ファイルに変換する方法。](http://youtu.be/PlgrzUZ-Y1o)

###シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **Mozy Enterprise** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、* * [シングル サインオン * * ダイアログ。

    ![Configure single sign-on](./media/active-directory-saas-mozy-enterprise-tutorial/IC771709.png "Configure single sign-on")

2.   **どのようなユーザーを Mozy Enterprise へのサインオン** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、] をクリックし、 **次**します。

    ![Configure single sign-on](./media/active-directory-saas-mozy-enterprise-tutorial/IC777311.png "Configure single sign-on")

3.   **アプリケーション URL の構成** ] ページで、 **Mozy Enterprise サインイン URL** ] ボックスに、次のパターンを使用して URL を入力"*https://\<tenant-name\>。Mozyenterprise.com*"、クリックして **次**します。

    ![アプリケーション URL の構成](./media/active-directory-saas-mozy-enterprise-tutorial/IC777312.png "Configure app URL")

4.   **Mozy Enterprise でのシングル サインオンの構成** ] ページで、証明書をダウンロードする] をクリックして **証明書のダウンロード**, 、お使いのコンピューターに証明書ファイルを保存します。

    ![シングル サインオンの構成](./media/active-directory-saas-mozy-enterprise-tutorial/IC777313.png "Configure single sign-on")

5.  別の Web ブラウザーのウィンドウで、Mozy Enterprise の企業サイトに管理者としてログインします。

6.   **構成** ] をクリックして **認証ポリシー**します。

    ![認証ポリシー](./media/active-directory-saas-mozy-enterprise-tutorial/IC777314.png "Authentication policy")

7.   **認証ポリシー** セクションで、次の手順に従います。

    ![認証ポリシー](./media/active-directory-saas-mozy-enterprise-tutorial/IC777315.png "Authentication policy")

    1.  選択 **ディレクトリ サービス** として **プロバイダー**します。
    2.  選択 **Use LDAP Push**します。
    3.  クリックして、 **SAML 認証** ] タブをクリックします。
    4.  Azure ポータルで、 **Mozy Enterprise でのシングル サインオンの構成** ] ダイアログ ページでコピー、 **認証要求 URL** 値に設定して、貼り付けます、 **認証 URL** ] ボックスに貼り付けます。
    5.  Azure ポータルで、 **Mozy Enterprise でのシングル サインオンの構成** ] ダイアログ ページでコピー、 **Id プロバイダーの ID** 値に設定して、貼り付けます、 **SAML エンドポイント** ] ボックスに貼り付けます。
    6.  作成、 **base-64 でエンコードされた** からダウンロードした証明書ファイル。  

        >[AZURE.TIP]詳細については、次を参照してください [バイナリ証明書をテキスト ファイルに変換する方法。](http://youtu.be/PlgrzUZ-Y1o)

    7.  Base-64 でエンコードされた証明書をメモ帳で開き、内容をクリップボードにコピーおよび貼り付けて、全体の証明書を **SAML 証明** ] ボックスに貼り付けます。
    8.  選択 **Enable SSO for Admins、ネットワーク資格情報を使用してログインする**です。
    9.  クリックして **変更を保存**します。

8.  Azure ポータルで、 **Mozy Enterprise でのシングル サインオンの構成** ] ダイアログ ページで、シングル サインオンの構成情報を選択し、 **完了**します。

    ![シングル サインオンの構成](./media/active-directory-saas-mozy-enterprise-tutorial/IC777316.png "Configure single sign-on")
##ユーザー プロビジョニングの構成
  
Azure AD ユーザーが Mozy Enterprise にログインできるようにするには、ユーザーを Mozy Enterprise にプロビジョニングする必要があります。  
Mozy Enterprise の場合、プロビジョニングは手動で行います。

###ユーザー アカウントをプロビジョニングするには、次の手順を実行します。

1.  ログインして **Mozy Enterprise** テナントです。

2.  クリックして **ユーザー**, 、] をクリックし、 **Add New User**します。

    ![ユーザー](./media/active-directory-saas-mozy-enterprise-tutorial/IC777317.png "Users")

    >[AZURE.NOTE] **Add New User** オプションが表示される場合にのみのみ **Mozy** が下にあるプロバイダーとして選択されている **認証ポリシー**します。 SAML 認証が構成されている場合、ユーザーはシングル サインオンでの初回ログイン時に自動的に追加されます。 

3.  新しいユーザーのダイアログで、次の手順に従います。

    ![ユーザーの追加](./media/active-directory-saas-mozy-enterprise-tutorial/IC777318.png "Add Users")

    1.   **グループを選択** 一覧で、グループを選択します。
    2.   **ユーザーの種類** ボックスの一覧で、種類を選択します。
    3.   **Username** ] ボックスに、Azure AD ユーザーの名前を入力します。
    4.   **電子メール** ] ボックスに、Azure AD ユーザーの電子メール アドレスを入力します。
    5.  選択 **命令電子メールでユーザー**します。
    6.  クリックして **人のユーザーを追加**します。

    >[AZURE.NOTE]ユーザーを作成すると、アクティブ化する前に、アカウントの確認へのリンクを含む Azure AD のユーザーに電子メールが送信されます。

>[AZURE.NOTE]他の Mozy Enterprise ユーザー アカウント作成ツールを使用するや、AAD ユーザー アカウントのプロビジョニングを Mozy enterprise 提供の Api です。

##ユーザーの割り当て
 
構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

###ユーザーを Mozy Enterprise に割り当てるには、次の手順に従います。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  * * Mozy Enterprise * * アプリケーション統合ページで、をクリックして **ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-mozy-enterprise-tutorial/IC777319.png "Assign users")

3.  テスト ユーザーを選択して [ **割り当てる**, 、順にクリック **はい** して割り当てを確認します。

    ![あり](./media/active-directory-saas-mozy-enterprise-tutorial/IC767830.png "Yes")
  
シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。

