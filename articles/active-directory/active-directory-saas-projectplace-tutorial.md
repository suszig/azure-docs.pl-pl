<properties 
    pageTitle="チュートリアル: Azure Active Directory と Projectplace の統合 | Microsoft Azure" 
    description="Azure Active Directory で Projectplace を使用して、シングル サインオンや自動プロビジョニングなどを有効にする方法について説明します。" 
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

#チュートリアル: Azure Active Directory と Projectplace の統合
  
このチュートリアルでは、Azure と Projectplace の統合について説明します。  
このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   Projectplace でのシングル サインオンが有効なサブスクリプション
  
このチュートリアルを完了すると、Projectplace に割り当てた Azure AD ユーザーは、Projectplace 会社サイト (サービス プロバイダーが開始したサインオン) で、アプリケーションにシングル サインオンまたはできるを使用して、 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。
  
このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  Projectplace のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-projectplace-tutorial/IC790217.png "Scenario")
##Projectplace のアプリケーション統合の有効化
  
このセクションでは、Projectplace のアプリケーション統合を有効にする方法を説明します。

###Projectplace のアプリケーション統合を有効にするには、次の手順に従います。

1.  左側のナビゲーション ウィンドウで、Azure 管理ポータルで、クリックして **Active Directory**します。

    ![Active Directory](./media/active-directory-saas-projectplace-tutorial/IC700993.png "Active Directory")

2.   **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3.  ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。

    ![アプリケーション](./media/active-directory-saas-projectplace-tutorial/IC700994.png "Applications")

4.  クリックして **追加** ページの下部にあります。

    ![アプリケーションの追加](./media/active-directory-saas-projectplace-tutorial/IC749321.png "Add application")

5.   **実行する操作** ダイアログ ボックスで、をクリックして **ギャラリーからアプリケーションを追加**します。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-projectplace-tutorial/IC749322.png "Add an application from gallerry")

6.   **検索ボックス**, 、型 **Projectplace**します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-projectplace-tutorial/IC790218.png "Application Gallery")

7.  結果ウィンドウで [ **Projectplace**, 、クリックして **完了** アプリケーションを追加します。

    ![Projectplace](./media/active-directory-saas-projectplace-tutorial/IC790219.png "ProjectPlace")
##シングル サインオンの構成
  
このセクションでは、ユーザーが SAML プロトコルに基づくフェデレーションを使用して、Azure AD でのユーザーのアカウントで Projectplace に対する認証を行えるようにする方法を説明します。

###シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **Projectplace** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、* * [シングル サインオン * * ダイアログ。

    ![シングル サインオンの構成](./media/active-directory-saas-projectplace-tutorial/IC790220.png "Configure Single SignOn")

2.   **どのようなユーザーの Projectplace へのアクセス** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、] をクリックし、 **次**します。

    ![シングル サインオンの構成](./media/active-directory-saas-projectplace-tutorial/IC790221.png "Configure Single SignOn")

3.   **アプリケーション URL の構成** ] ページの [、 **Projectplace サインオン URL** ] ボックスに、ProjectPlace テナントの URL を入力 (例::"*http://company.projectplace.com*")、順にクリック **次**します。

    ![アプリケーション URL の構成](./media/active-directory-saas-projectplace-tutorial/IC790222.png "Configure App URL")

4.   **Projectplace でのシングル サインオンの構成** ] ページで [ **メタデータのダウンロード**, 、お使いのコンピューターにメタデータ ファイルを保存します。

    ![シングル サインオンの構成](./media/active-directory-saas-projectplace-tutorial/IC790223.png "Configure Single SignOn")

5.  Projectplace サポート チームにメタデータ ファイルを送信します。

    >[AZURE.NOTE] シングル サインオンの構成では、Projectplace サポート チームによって実行されます。 構成が完了すると、サポート チームから通知が届きます。

6.  Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![シングル サインオンの構成](./media/active-directory-saas-projectplace-tutorial/IC790227.png "Configure Single SignOn")
##ユーザー プロビジョニングの構成
  
Azure AD ユーザーが Projectplace にログインできるようにするには、ユーザーを Projectplace にプロビジョニングする必要があります。  
Projectplace の場合、プロビジョニングは手動で行います。

###ユーザー アカウントをプロビジョニングするには、次の手順を実行します。

1.  ログインして **Projectplace** 会社サイトに管理者として。

2.  移動して **人**, 、クリックして **メンバー**します。

    ![People](./media/active-directory-saas-projectplace-tutorial/IC790228.png "People")

3.  クリックして **メンバーを追加**します。

    ![メンバーの追加](./media/active-directory-saas-projectplace-tutorial/IC790232.png "Add Members")

4.   **メンバーの追加** セクションで、次の手順に従います。

    ![新しいメンバー](./media/active-directory-saas-projectplace-tutorial/IC790233.png "New Members")

    1.   **新しいメンバー** ] ボックスに、関連するテキスト ボックスに、プロビジョニングする有効な AAD アカウントの電子メール アドレスを入力します。
    2.  クリックして **送信**

        >[AZURE.NOTE] アクティブ化する前に、アカウントの確認へのリンクが記載された電子メールは、Azure Active Directory アカウント所有者に送信されます。
    
>[AZURE.NOTE]他の Projectplace ユーザー アカウント作成ツールを使用することができますか、AAD ユーザー アカウントをプロビジョニング、Projectplace から提供されている Api です。

##ユーザーの割り当て
  
構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

###ユーザーを Projectplace に割り当てるには、次の手順に従います。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  * * Projectplace * * アプリケーション統合ページで、をクリックして **ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-projectplace-tutorial/IC790234.png "Assign Users")

3.  テスト ユーザーを選択して [ **割り当てる**, 、順にクリック **はい** して割り当てを確認します。

    ![あり](./media/active-directory-saas-projectplace-tutorial/IC767830.png "Yes")
  
シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。
