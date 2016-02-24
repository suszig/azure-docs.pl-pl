<properties 
    pageTitle="チュートリアル: Azure Active Directory と Abintegro の統合 | Microsoft Azure" 
    description="Azure Active Directory で Abintegro を使用して、シングル サインオンや自動プロビジョニングなどを有効にする方法について説明します。" 
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

#チュートリアル: Azure Active Directory と Abintegro の統合

このチュートリアルでは、Azure と Abintegro の統合について説明します。  
このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   Abintegro でのシングル サインオンが有効なサブスクリプション

このチュートリアルを完了すると、Abintegro に割り当てた Azure AD ユーザーは、Abintegro 会社サイト (サービス プロバイダーが開始したサインオン) で、アプリケーションにシングル サインオンまたはできるを使用して、 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。

このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  Abintegro のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-abintegro-tutorial/IC790076.png "Scenario")
##Abintegro のアプリケーション統合の有効化

このセクションでは、Abintegro のアプリケーション統合を有効にする方法を説明します。

###Abintegro のアプリケーション統合を有効にするには、次の手順に従います。

1.  左側のナビゲーション ウィンドウで、Azure 管理ポータルで、クリックして **Active Directory**します。

    ![Active Directory](./media/active-directory-saas-abintegro-tutorial/IC700993.png "Active Directory")

2.   **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3.  ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。

    ![アプリケーション](./media/active-directory-saas-abintegro-tutorial/IC700994.png "Applications")

4.  クリックして **追加** ページの下部にあります。

    ![アプリケーションの追加](./media/active-directory-saas-abintegro-tutorial/IC749321.png "Add application")

5.   **実行する操作** ダイアログ ボックスで、をクリックして **ギャラリーからアプリケーションを追加**します。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-abintegro-tutorial/IC749322.png "Add an application from gallerry")

6.   **検索ボックス**, 、型 **abintegro**します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-abintegro-tutorial/IC790077.png "Application Gallery")

7.  結果ウィンドウで [ **Abintegro**, 、クリックして **完了** アプリケーションを追加します。

    ![Abintegro](./media/active-directory-saas-abintegro-tutorial/IC790078.png "Abintegro")
##シングル サインオンの構成

このセクションでは、ユーザーが SAML プロトコルに基づくフェデレーションを使用して、Azure AD でのユーザーのアカウントで Abintegro に対する認証を行えるようにする方法を説明します。

###シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **Abintegro** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、* * [シングル サインオン * * ダイアログ。

    ![シングル サインオンの構成](./media/active-directory-saas-abintegro-tutorial/IC790079.png "Configure Single SignOn")

2.   **どのようなユーザーの Abintegro へのアクセス** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、] をクリックし、 **次**します。

    ![シングル サインオンの構成](./media/active-directory-saas-abintegro-tutorial/IC790080.png "Configure Single SignOn")

3.   **アプリケーション URL の構成** ] ページで、 **Abintegro サインイン URL** ] ボックスに、型、URL を使って、ユーザーを Abintegro への (例:: `https://dev.abintegro.com/Shibboleth.sso/Login?entityID=<Issuer>&target=https://dev.abintegro.com/secure/`)、順にクリック **次**します。

    ![Configure App URL](./media/active-directory-saas-abintegro-tutorial/IC790081.png "Configure App URL")

4.   **Abintegro でのシングル サインオンの構成** ] ページで [ **メタデータのダウンロード**, 、お使いのコンピューターにメタデータ ファイルを保存します。

    ![シングル サインオンの構成](./media/active-directory-saas-abintegro-tutorial/IC790082.png "Configure Single SignOn")

5.  Abintegro サポート チームに、メタデータ ファイルを送信します。

    >[AZURE.NOTE] シングル サインオンの構成では、Abintegro サポート チームによって実行されます。 構成が完了すると、サポート チームから通知が届きます。

6.  Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![シングル サインオンの構成](./media/active-directory-saas-abintegro-tutorial/IC790083.png "Configure Single SignOn")
##ユーザー プロビジョニングの構成

Abintegro へのユーザー プロビジョニングの構成にあたって必要な操作はありません。  
割り当てられたユーザーがアクセス パネルを使用してAbintegro にログインしようとすると、そのユーザーが存在するかどうかが Abintegro によって確認されます。  
使用可能なユーザー アカウントがない場合、ユーザー アカウントは Abintegro により自動的に作成されます。
##ユーザーの割り当て

構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

###ユーザーを Abintegro に割り当てるには、次の手順に従います。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  * * Abintegro * * アプリケーション統合ページで、をクリックして **ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-abintegro-tutorial/IC790084.png "Assign Users")

3.  テスト ユーザーを選択して [ **割り当てる**, 、順にクリック **はい** して割り当てを確認します。

    ![あり](./media/active-directory-saas-abintegro-tutorial/IC767830.png "Yes")

シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。

