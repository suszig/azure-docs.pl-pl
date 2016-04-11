<properties 
    pageTitle="チュートリアル: Azure Active Directory と ShiftPlanning の統合 | Microsoft Azure" 
    description="Azure Active Directory で ShiftPlanning を使用して、シングル サインオンや自動プロビジョニングなどを有効にする方法について説明します。" 
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

#チュートリアル: Azure Active Directory と ShiftPlanning の統合
  
このチュートリアルの目的は、Azure と ShiftPlanning の統合を示すことです。  
このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   ShiftPlanning でのシングル サインオンが有効なサブスクリプション
  
このチュートリアルを完了すると、ShiftPlanning に割り当てた Azure AD ユーザーは、ShiftPlanning 会社サイト (サービス プロバイダーが開始したサインオン) で、アプリケーションにシングル サインオンまたはできるを使用して、 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。
  
このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  ShiftPlanning のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-shiftplanning-tutorial/IC786612.png "Scenario")
##ShiftPlanning のアプリケーション統合の有効化
  
このセクションでは、ShiftPlanning のアプリケーション統合を有効にする方法を説明します。

###ShiftPlanning のアプリケーション統合を有効にするには、次の手順に従います。

1.  左側のナビゲーション ウィンドウで、Azure 管理ポータルで、クリックして **Active Directory**します。

    ![Active Directory](./media/active-directory-saas-shiftplanning-tutorial/IC700993.png "Active Directory")

2.   **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3.  ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。

    ![アプリケーション](./media/active-directory-saas-shiftplanning-tutorial/IC700994.png "Applications")

4.  クリックして **追加** ページの下部にあります。

    ![アプリケーションの追加](./media/active-directory-saas-shiftplanning-tutorial/IC749321.png "Add application")

5.   **実行する操作** ダイアログ ボックスで、をクリックして **ギャラリーからアプリケーションを追加**します。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-shiftplanning-tutorial/IC749322.png "Add an application from gallerry")

6.   **検索ボックス**, 、型 **ShiftPlanning**します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-shiftplanning-tutorial/IC786613.png "Application Gallery")

7.  結果ウィンドウで [ **ShiftPlanning**, 、クリックして **完了** アプリケーションを追加します。

    ![ShiftPlanning](./media/active-directory-saas-shiftplanning-tutorial/IC786614.png "ShiftPlanning")
##シングル サインオンの構成
  
このセクションでは、SAML プロトコルに基づくフェデレーションを使用して、ShiftPlanning でAzure AD のユーザー アカウントを使用してユーザーを認証できるようにする方法を説明します。  
この手順の途中で、base-64 でエンコードされた証明書ファイルを作成する必要があります。  
この手順に慣れていない場合は、次を参照してください [バイナリ証明書をテキスト ファイルに変換する方法。](http://youtu.be/PlgrzUZ-Y1o)

###シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **ShiftPlanning** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、* * [シングル サインオン * * ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-shiftplanning-tutorial/IC786615.png "Configure Single Sign-On")

2.   **どのようなユーザーが ShiftPlanning へのサインオン** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、順にクリック **次**します。

    ![Configure Single Sign-On](./media/active-directory-saas-shiftplanning-tutorial/IC786616.png "Configure Single Sign-On")

3.   **アプリケーション URL の構成** ] ページの [、 **ShiftPlanning サインイン URL** ] ボックスに、次のパターンを使用して URL を入力"*https://company.shiftplanning.com/includes/saml/*"、] をクリックし、 **次**します。

    ![Configure App URL](./media/active-directory-saas-shiftplanning-tutorial/IC786617.png "Configure App URL")

4.   **ShiftPlanning でのシングル サインオン構成** ] ページで、証明書をダウンロードする] をクリックして **証明書のダウンロード**, 、お使いのコンピューターに証明書ファイルを保存します。

    ![Configure Single Sign-On](./media/active-directory-saas-shiftplanning-tutorial/IC786618.png "Configure Single Sign-On")

5.  ログインする別の web ブラウザーのウィンドウで、 **ShiftPlanning** 会社サイトに管理者として。

6.  上部にあるメニュー [ **Admin**します。

    ![管理者](./media/active-directory-saas-shiftplanning-tutorial/IC786619.png "Admin")

7.   **統合**, をクリックして **でのシングル サインオン**します。

    ![シングル サインオン](./media/active-directory-saas-shiftplanning-tutorial/IC786620.png "Single Sign-On")

8.   **でのシングル サインオン** セクションで、次の手順に従います。

    ![シングル サインオン](./media/active-directory-saas-shiftplanning-tutorial/IC786905.png "Single Sign-On")

    1.  選択 **SAML の有効化**します。
    2.  選択 **パスワード ログインを許可**
    3.  Azure ポータルで、 **ShiftPlanning でのシングル サインオンの構成** ] ダイアログ ページでコピー、 **リモート ログイン URL** 値に設定して、貼り付けます、 **SAML 発行者 URL** ] ボックスに貼り付けます。
    4.  Azure ポータルで、 **ShiftPlanning でのシングル サインオンの構成** ] ダイアログ ページでコピー、 **リモート ログアウト URL** 値に設定して、貼り付けます、 **リモート ログアウト URL** ] ボックスに貼り付けます。
    5.  作成、 **base-64 でエンコードされた** からダウンロードした証明書ファイル。  

        >[AZURE.TIP]詳細については、次を参照してください [バイナリ証明書をテキスト ファイルに変換する方法。](http://youtu.be/PlgrzUZ-Y1o)

    6.  Base-64 でエンコードされた証明書をメモ帳で開き、という内容がクリップボードにコピーおよびに貼り付け、 **X.509 証明書** ] テキスト ボックス
    7.  クリックして **設定を保存する**です。

9.  Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-shiftplanning-tutorial/IC786621.png "Configure Single Sign-On")
##ユーザー プロビジョニングの構成
  
Azure AD ユーザーが ShiftPlanning にログインできるようにするには、ユーザーを ShiftPlanning にプロビジョニングする必要があります。  
ShiftPlanning の場合、プロビジョニングは手動で行います。

###ユーザー アカウントをプロビジョニングするには、次の手順を実行します。

1.  ログインして **ShiftPlanning** 会社サイトに管理者として。

2.  クリックして **Admin**します。

    ![管理者](./media/active-directory-saas-shiftplanning-tutorial/IC786619.png "Admin")

3.  クリックして **スタッフ**します。

    ![スタッフ](./media/active-directory-saas-shiftplanning-tutorial/IC786623.png "Staff")

4.   **アクション**, 、クリックして **従業員の追加**します。

    ![従業員の追加](./media/active-directory-saas-shiftplanning-tutorial/IC786624.png "Add Employees")

5.   **従業員を追加** セクションで、次の手順に従います。

    ![従業員の保存](./media/active-directory-saas-shiftplanning-tutorial/IC786625.png "Save Employees")

    1.  型、 **名**, 、**姓** と **電子メール** 関連テキスト ボックスに、プロビジョニングする有効な AAD アカウントのです。
    2.  クリックして **従業員の保存**します。

>[AZURE.NOTE]他の ShiftPlanning ユーザー アカウント作成ツールを使用することができますか、AAD ユーザー アカウントをプロビジョニング、ShiftPlanning から提供されている Api です。

##ユーザーの割り当て
  
構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

###ユーザーを ShiftPlanning に割り当てるには、次の手順に従います。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  * * ShiftPlanning * * アプリケーション統合ページで、をクリックして **ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-shiftplanning-tutorial/IC786626.png "Assign Users")

3.  テスト ユーザーを選択して [ **割り当てる**, 、順にクリック **はい** して割り当てを確認します。

    ![あり](./media/active-directory-saas-shiftplanning-tutorial/IC767830.png "Yes")
  
シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。
