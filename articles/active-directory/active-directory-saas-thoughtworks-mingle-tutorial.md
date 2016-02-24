<properties 
    pageTitle="チュートリアル: Azure Active Directory と Thoughtworks Mingle の統合 | Microsoft Azure" 
    description="Azure Active Directory で Thoughtworks Mingle を使用して、シングル サインオンや自動プロビジョニングなどを有効にする方法を説明します。" 
    services="active-directory" 
    authors="markusvi"  
    documentationCenter="na" manager="stevenpo"/>
<tags 
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="10/22/2015" 
    ms.author="markvi" />

#チュートリアル: Azure Active Directory と Thoughtworks Mingle の統合
  
このチュートリアルでは、Azure と Thoughtworks Mingle の統合について説明します。  
このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   Thoughtworks Mingle テナント
  
このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  Thoughtworks Mingle のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785150.png "Scenario")

##Thoughtworks Mingle のアプリケーション統合の有効化
  
このセクションでは、Thoughtworks Mingle のアプリケーション統合を有効にする方法を説明します。

###Thoughtworks Mingle のアプリケーション統合を有効にするには、次の手順を実行します。

1.  左側のナビゲーション ウィンドウで、Azure 管理ポータルで、クリックして **Active Directory**します。

    ![Active Directory](./media/active-directory-saas-thoughtworks-mingle-tutorial/IC700993.png "Active Directory")

2.   **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3.  ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。

    ![アプリケーション](./media/active-directory-saas-thoughtworks-mingle-tutorial/IC700994.png "Applications")

4.  クリックして **追加** ページの下部にあります。

    ![アプリケーションの追加](./media/active-directory-saas-thoughtworks-mingle-tutorial/IC749321.png "Add application")

5.   **実行する操作** ダイアログ ボックスで、をクリックして **ギャラリーからアプリケーションを追加**します。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-thoughtworks-mingle-tutorial/IC749322.png "Add an application from gallerry")

6.   **検索ボックス**, 、型 **thoughtworks mingle**します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785151.png "Application Gallery")

7.  結果ウィンドウで [ **Thoughtworks Mingle**, 、クリックして **完了** アプリケーションを追加します。

    ![Thoughtworks Mingle](./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785152.png "Thoughtworks Mingle")

##シングル サインオンの構成
  
このセクションでは、ユーザーが SAML プロトコルに基づくフェデレーションを使用して、Azure AD でのユーザーのアカウントで Thoughtworks Mingle に対する認証を行うことができるようにする方法を説明します。  
この手順の途中で、証明書を Thoughtworks Mingle にアップロードする必要があります。

###シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの * * Thoughtworks Mingle * * アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、* * [シングル サインオン * * ダイアログ。

    ![シングル サインオンの構成](./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785153.png "Configure single sign-on")

2.   **どのようなユーザーの Thoughtworks Mingle へのアクセス** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、] をクリックし、 **次**します。

    ![シングル サインオンの構成](./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785154.png "Configure single sign-on")

3.   **アプリケーション URL の構成** ] ページの [、 **Thoughtworks Mingle テナント URL** ] ボックスに、次のパターンを使用して URL を入力"*http://company.mingle.thoughtworks.com*"、] をクリックし、 **次**します。

    ![Configure App URL](./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785155.png "Configure App URL")

4.   **Thoughtworks Mingle でのシングル サインオンの構成** ] ページで [メタデータのダウンロード] をクリックして、コンピューターに保存します。

    ![Configure single sign-on](./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785156.png "Configure single sign-on")

5.  ログインして **Thoughtworks Mingle** 会社サイトに管理者として。

6.  クリックして、 **Admin** タブをクリックし、[、] をクリックして **SSO 構成**します。

    ![SSO 構成](./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785157.png "SSO Config")

7.   **SSO 構成** セクションで、次の手順に従います。

    ![SSO 構成](./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785158.png "SSO Config")

    1.  メタデータ ファイルをアップロードするにはクリックして **ファイルの選択**します。
    2.  クリックして **変更を保存**します。

8.  Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![シングル サインオンの構成](./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785159.png "Configure single sign-on")

##ユーザー プロビジョニングの構成
  
AAD ユーザーがサインインできるように、Azure Active Directory ユーザー名を使用して、Thoughtworks Mingle アプリケーションにユーザーをプロビジョニングする必要があります。  
Thoughtworks Mingle の場合、プロビジョニングは手動で行います。

###ユーザー プロビジョニングを構成するには、次の手順に従います。

1.  Thoughtworks Mingle 企業サイトに管理者としてログインします。

2.  クリックして **プロファイル**します。

    ![最初のプロジェクト](./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785160.png "Your First Project")

3.  クリックして、 **管理者** タブをクリックし、をクリックし、 **ユーザー**します。

    ![ユーザー](./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785161.png "Users")

4.  クリックして **新しいユーザー**します。

    ![新しいユーザー](./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785162.png "New User")

5.   **新しいユーザー** ] ダイアログ ページで、次の手順を実行します。

    ![新しいユーザー](./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785163.png "New User")

    1.  型、 **サインイン名**, 、**表示名**, 、**パスワードの選択**, 、**パスワードの確認入力** 関連テキスト ボックスに、プロビジョニングする有効な AAD アカウントのです。
    2.  として **ユーザー タイプ**, [ **完全なユーザー**します。
    3.  クリックして **このプロファイルを作成する**です。

>[AZURE.NOTE] 他の Thoughtworks Mingle ユーザー アカウント作成ツールを使用する、または AAD ユーザー アカウントをプロビジョニング、Thoughtworks Mingle から提供されている Api です。

##ユーザーの割り当て
  
構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

###ユーザーを Thoughtworks Mingle に割り当てるには、次の手順を実行します。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.   **Thoughtworks Mingle** アプリケーション統合ページで、をクリックして **ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785164.png "Assign Users")

3.  テスト ユーザーを選択して [ **割り当てる**, 、順にクリック **はい** して割り当てを確認します。

    ![あり](./media/active-directory-saas-thoughtworks-mingle-tutorial/IC767830.png "Yes")
  
シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。

