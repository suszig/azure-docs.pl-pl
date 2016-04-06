<properties 
    pageTitle="チュートリアル: Azure Active Directory と Wikispaces の統合 | Microsoft Azure" 
    description="Azure Active Directory で Wikispaces を使用してシングル サインオンや自動プロビジョニングなどを有効にする方法について説明します。" 
    services="active-directory" 
    authors="MarkusVi"  
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

#チュートリアル: Azure Active Directory と Wikispaces の統合
  
このチュートリアルでは、Azure と Wikispaces の統合について説明します。  
このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   Wikispaces でのシングル サインオンが有効なサブスクリプション
  
このチュートリアルを完了すると、Wikispaces に割り当てた Azure AD ユーザーは、Wikispaces 会社サイト (サービス プロバイダーが開始したサインオン) で、アプリケーションにシングル サインオンまたはできるを使用して、 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。
  
このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  Wikispaces のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-wikispaces-tutorial/IC787182.png "Sceanrio")

##Wikispaces のアプリケーション統合の有効化
  
このセクションでは、Wikispaces のアプリケーション統合を有効にする方法を説明します。

###Wikispaces のアプリケーション統合を有効にするには、次の手順に従います。

1.  左側のナビゲーション ウィンドウで、Azure 管理ポータルで、クリックして **Active Directory**します。

    ![Active Directory](./media/active-directory-saas-wikispaces-tutorial/IC700993.png "Active Directory")

2.   **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3.  ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。

    ![アプリケーション](./media/active-directory-saas-wikispaces-tutorial/IC700994.png "Applications")

4.  クリックして **追加** ページの下部にあります。

    ![アプリケーションの追加](./media/active-directory-saas-wikispaces-tutorial/IC749321.png "Add application")

5.   **実行する操作** ダイアログ ボックスで、をクリックして **ギャラリーからアプリケーションを追加**します。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-wikispaces-tutorial/IC749322.png "Add an application from gallerry")

6.   **検索ボックス**, 、型 **Wikispaces**します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-wikispaces-tutorial/IC787186.png "Application Gallery")

7.  結果ウィンドウで [ **Wikispaces**, 、クリックして **完了** アプリケーションを追加します。

    ![Wikispaces](./media/active-directory-saas-wikispaces-tutorial/IC787187.png "Wikispaces")

##シングル サインオンの構成
  
このセクションでは、ユーザーが SAML プロトコルに基づくフェデレーションを使用して、Azure AD でのユーザーのアカウントで Wikispaces に対する認証を行うことができるようにする方法を説明します。

###シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **Wikispaces** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、* * [シングル サインオン * * ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-wikispaces-tutorial/IC787188.png "Configure Single Sign-On")

2.   **どのようなユーザーが Wikispaces へのサインオン** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、順にクリック **次**します。

    ![Configure Single Sign-On](./media/active-directory-saas-wikispaces-tutorial/IC787189.png "Configure Single Sign-On")

3.   **アプリケーション URL の構成** ] ページの [、 **Wikispaces サインオン URL** ] ボックスに、次のパターンを使用して URL を入力"*http://company.wikispaces.net*"、] をクリックし、 **次**します。

    ![アプリケーション URL の構成](./media/active-directory-saas-wikispaces-tutorial/IC787190.png "Configure App URL")

4.   **Wikispaces でのシングル サインオンの構成** ] ページで [ **メタデータのダウンロード**, 、お使いのコンピューターにメタデータ ファイルを保存します。

    ![Configure Single Sign-On](./media/active-directory-saas-wikispaces-tutorial/IC787191.png "Configure Single Sign-On")

5.  Wikispaces サポート チームに、メタデータ ファイルを送信します。

    >[AZURE.NOTE] シングル サインオンの構成では、Wikispaces サポート チームによって実行されます。 構成が完了すると、サポート チームから通知が届きます。

6.  Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-wikispaces-tutorial/IC787192.png "Configure Single Sign-On")

##ユーザー プロビジョニングの構成
  
Azure AD ユーザーが Wikispaces にログインできるようにするには、そのユーザーを Wikispaces にプロビジョニングする必要があります。  
Wikispaces の場合、プロビジョニングは手動で行います。

###ユーザー アカウントをプロビジョニングするには、次の手順を実行します。

1.  ログインして **Wikispaces** 会社サイトに管理者として。

2.  移動して **メンバー**します。

    ![メンバー](./media/active-directory-saas-wikispaces-tutorial/IC787193.png "Members")

3.  クリックして、 **招待**します。

    ![ユーザーの招待](./media/active-directory-saas-wikispaces-tutorial/IC787194.png "Invite People")

4.   **Invite People** セクションで、次の手順に従います。

    ![ユーザーの招待](./media/active-directory-saas-wikispaces-tutorial/IC787208.png "Invite People")

    1.  型、 **ユーザー名または電子メール アドレス** 関連テキスト ボックスに、プロビジョニングする有効な AAD アカウントのです。
    2.  クリックして **送信**します。  

        >[AZURE.NOTE] Azure Active Directory アカウント所有者は、アクティブ化する前に、アカウントの確認へのリンクが記載された電子メールを受信します。

>[AZURE.NOTE] 他の Wikispaces ユーザー アカウント作成ツールを使用することができますか、AAD ユーザー アカウントをプロビジョニング、Wikispaces から提供されている Api です。

##ユーザーの割り当て
  
構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

###ユーザーを Wikispaces に割り当てるには、次の手順に従います。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  * * Wikispaces * * アプリケーション統合ページで、をクリックして **ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-wikispaces-tutorial/IC787195.png "Assign Users")

3.  テスト ユーザーを選択して [ **割り当てる**, 、順にクリック **はい** して割り当てを確認します。

    ![あり](./media/active-directory-saas-wikispaces-tutorial/IC767830.png "Yes")
  
シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。

