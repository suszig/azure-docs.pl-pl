<properties 
    pageTitle="チュートリアル: Azure Active Directory と 15Five の統合 | Microsoft Azure" 
    description="Azure Active Directory で 15Five を使用して、シングル サインオンや自動プロビジョニングなどを有効にする方法について説明します。" 
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

#チュートリアル: Azure Active Directory と 15Five の統合

このチュートリアルの目的は、Azure と 15Five の統合を紹介することです。 このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   15Five でのシングル サインオンが有効なサブスクリプション

このチュートリアルを完了すると、15 five に割り当てた Azure AD ユーザーは、15 five 会社サイト (サービス プロバイダーが開始したサインオン) で、アプリケーションにシングル サインオンまたはできるを使用して、 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。

このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  15Five のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-15five-tutorial/IC784667.png "Scenario")
##15Five のアプリケーション統合の有効化

このセクションでは、15Five のアプリケーション統合を有効にする方法について説明します。

###15Five のアプリケーション統合を有効にするには、次の手順に従います。

1.  左側のナビゲーション ウィンドウで、Azure 管理ポータルで、クリックして **Active Directory**します。

    ![Active Directory](./media/active-directory-saas-15five-tutorial/IC700993.png "Active Directory")

2.   **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3.  ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。

    ![アプリケーション](./media/active-directory-saas-15five-tutorial/IC700994.png "Applications")

4.  クリックして **追加** ページの下部にあります。

    ![アプリケーションの追加](./media/active-directory-saas-15five-tutorial/IC749321.png "Add application")

5.   **実行する操作** ダイアログ ボックスで、をクリックして **ギャラリーからアプリケーションを追加**します。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-15five-tutorial/IC749322.png "Add an application from gallerry")

6.   **検索ボックス**, 、型 **15 five**します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-15five-tutorial/IC784668.png "Application Gallery")

7.  結果ウィンドウで [ **15 five**, 、クリックして **完了** アプリケーションを追加します。

    ![15Five](./media/active-directory-saas-15five-tutorial/IC784669.png "15Five")
##シングル サインオンの構成

このセクションでは、ユーザーが SAML プロトコルに基づくフェデレーションを使用して、Azure AD でのユーザーのアカウントで 15Five に対する認証を行えるようにする方法を説明します。

###シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **15 five** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、* * [シングル サインオン * * ダイアログ。

    ![Configure single sign-on](./media/active-directory-saas-15five-tutorial/IC784670.png "Configure single sign-on")

2.   **どのようなユーザーが 15 five へのサインオンを** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、順にクリック **次**します。

    ![Configure single sign-on](./media/active-directory-saas-15five-tutorial/IC784671.png "Configure single sign-on")

3.   **アプリケーション URL の構成** ] ページの [、 **15 five サインイン URL** ] ボックスに、次のパターンを使用して URL を入力"*https://company.15Five.com*"、] をクリックし、 **次**します。

    ![アプリケーション URL の構成](./media/active-directory-saas-15five-tutorial/IC784672.png "Configure App URL")

4.   **15 five でのシングル サインオンの構成** ] ページで [ **メタデータのダウンロード**, 、15 five サポート チームにメタデータ ファイルを転送します。

    ![シングル サインオンの構成](./media/active-directory-saas-15five-tutorial/IC784673.png "Configure single sign-on")

    >[AZURE.NOTE] シングル サインオンは、15 five サポート チームを有効にする必要があります。

5.  Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![シングル サインオンの構成](./media/active-directory-saas-15five-tutorial/IC784674.png "Configure single sign-on")
##ユーザー プロビジョニングの構成

Azure AD ユーザーが 15Five にログインできるようにするには、そのユーザーを 15Five にプロビジョニングする必要があります。  
15Five の場合、プロビジョニングは手動で行います。

###ユーザー プロビジョニングを構成するには、次の手順に従います。

1.  ログインして **15 five** 会社サイトに管理者として。

2.  移動して **会社の管理**します。

    ![会社の管理](./media/active-directory-saas-15five-tutorial/IC784675.png "Manage Company")

3.  移動して **人 \ > ユーザーを追加する**です。

    ![People](./media/active-directory-saas-15five-tutorial/IC784676.png "People")

4.  [新しいユーザーの追加] セクションで、次の手順に従います。

    ![新しいユーザーの追加](./media/active-directory-saas-15five-tutorial/IC784677.png "Add New Person")

    1.  型、 **名**, 、**姓**, 、**タイトル**, 、**電子メール アドレス** 関連テキスト ボックスに、プロビジョニングする有効な Azure Active Directory アカウントのです。
    2.  クリックして **実行**します。

    >[AZURE.NOTE] Azure AD のアカウント所有者がアクティブ化する前に、アカウントの確認へのリンクが記載された電子メールを受け取ります。

>[AZURE.NOTE] 他の 15 five ユーザー アカウント作成ツールを使ったり、AAD ユーザー アカウントのプロビジョニングを 15 five から提供されている Api。

##ユーザーの割り当て

構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

###ユーザーを 15Five に割り当てるには、次の手順に従います。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  * * 15 five * * アプリケーション統合ページで、をクリックして **ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-15five-tutorial/IC784678.png "Assign users")

3.  テスト ユーザーを選択して [ **割り当てる**, 、順にクリック **はい** して割り当てを確認します。

    ![あり](./media/active-directory-saas-15five-tutorial/IC767830.png "Yes")

シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。

