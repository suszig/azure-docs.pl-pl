<properties 
    pageTitle="チュートリアル: Azure Active Directory と Huddle の統合 | Microsoft Azure" 
    description="Azure Active Directory で Huddle を使用して、シングル サインオンや自動プロビジョニングなどを有効にする方法について説明します。" 
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

#チュートリアル: Azure Active Directory と Huddle の統合
  
このチュートリアルでは、Azure と Huddle の統合について説明します。  
このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   Huddle でのシングル サインオンが有効なサブスクリプション
  
このチュートリアルを完了すると、Huddle に割り当てた Azure AD ユーザーは、Huddle 会社サイト (サービス プロバイダーが開始したサインオン) で、アプリケーションにシングル サインオンまたはできるを使用して、 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。
  
このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  Huddle のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![Configure Single Sign-On](./media/active-directory-saas-huddle-tutorial/IC787830.png "Configure Single Sign-On")
##Huddle のアプリケーション統合の有効化
  
このセクションでは、Huddle のアプリケーション統合を有効にする方法について説明します。

###Huddle のアプリケーション統合を有効にするには、次の手順を実行します。

1.  左側のナビゲーション ウィンドウで、Azure 管理ポータルで、クリックして **Active Directory**します。

    ![Active Directory](./media/active-directory-saas-huddle-tutorial/IC700993.png "Active Directory")

2.   **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3.  ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。

    ![アプリケーション](./media/active-directory-saas-huddle-tutorial/IC700994.png "Applications")

4.  クリックして **追加** ページの下部にあります。

    ![アプリケーションの追加](./media/active-directory-saas-huddle-tutorial/IC749321.png "Add application")

5.   **実行する操作** ダイアログ ボックスで、をクリックして **ギャラリーからアプリケーションを追加**します。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-huddle-tutorial/IC749322.png "Add an application from gallerry")

6.   **検索ボックス**, 、型 **Huddle**します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-huddle-tutorial/IC787831.png "Application Gallery")

7.  結果ウィンドウで [ **Huddle**, 、クリックして **完了** アプリケーションを追加します。

    ![Huddle](./media/active-directory-saas-huddle-tutorial/IC787832.png "Huddle")
##シングル サインオンの構成
  
このセクションでは、SAML プロトコルに基づくフェデレーションを使用して、ユーザーが Azure AD のアカウントで Huddle に対する認証を行うことができるようにする方法を説明します。

###シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **Huddle** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、* * [シングル サインオン * * ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-huddle-tutorial/IC787833.png "Configure Single Sign-On")

2.   **どのようなユーザーが Huddle へのサインオン** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、順にクリック **次**します。

    ![Configure Single Sign-On](./media/active-directory-saas-huddle-tutorial/IC787834.png "Configure Single Sign-On")

3.   **アプリケーション URL の構成** ] ページで、 **Huddle サインオン URL** ] ボックスに、次のパターンを使用して Huddle の URL のテナントの種類"*http://company.huddle.com*"、順にクリック **次**します。

    ![Configure App URL](./media/active-directory-saas-huddle-tutorial/IC787835.png "Configure App URL")

4.   **Huddle でのシングル サインオンの構成** ] ページで、次の手順を実行します。

    ![Configure Single Sign-On](./media/active-directory-saas-huddle-tutorial/IC787836.png "Configure Single Sign-On")

    1.  クリックして **証明書のダウンロード**, 、お使いのコンピューターに証明書ファイルを保存します。
    2.  コピー、 **発行者 URL** 値、 **SAML SSO URL** 値、およびダウンロードした証明書、Huddle サポート チームに送っています。

    >[AZURE.NOTE] シングル サインオンは、Huddle サポート チームを有効にする必要があります。
    構成が完了すると、通知が届きます。

5.  Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-huddle-tutorial/IC787837.png "Configure Single Sign-On")
##ユーザー プロビジョニングの構成
  
Azure AD ユーザーが Huddle にログインできるようにするには、そのユーザーを Huddle にプロビジョニングする必要があります。  
Huddle の場合、プロビジョニングは手動で行います。

###ユーザー プロビジョニングを構成するには、次の手順に従います。

1.  ログインして **Huddle** 会社サイトに管理者として。

2.  クリックして **ワークスペース**します。

3.  クリックして **人 \ > 招待**します。

    ![People](./media/active-directory-saas-huddle-tutorial/IC787838.png "People")

4.   **新しい招待の作成** セクションで、次の手順に従います。

    ![新しい招待](./media/active-directory-saas-huddle-tutorial/IC787839.png "New Invitation")

    1.   **チームに参加するユーザーを招待を選択して** 一覧で、[ **チーム**します。
    2.  型、 **電子メール アドレス** 関連テキスト ボックスに、プロビジョニングする有効な AAD アカウントのです。
    3.  クリックして **招待**します。

    >[AZURE.NOTE] Azure AD のアカウント所有者がアクティブ化する前に、アカウントの確認へのリンクが記載された電子メールを受け取ります。

>[AZURE.NOTE] 他の Huddle ユーザー アカウント作成ツールを使用するまたはユーザー アカウントのプロビジョニング AAD Huddle によって提供される Api。

##ユーザーの割り当て
  
構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

###ユーザーを Huddle に割り当てるには、次の手順を実行します。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  * * Huddle * * アプリケーション統合ページで、をクリックして **ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-huddle-tutorial/IC787840.png "Assign Users")

3.  テスト ユーザーを選択して [ **割り当てる**, 、順にクリック **はい** して割り当てを確認します。

    ![あり](./media/active-directory-saas-huddle-tutorial/IC767830.png "Yes")
  
シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。
