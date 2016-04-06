<properties 
    pageTitle="チュートリアル: Azure Active Directory と OverDrive Books の統合 | Microsoft Azure" 
    description="Azure Active Directory で Overdrive Books を使用して、シングル サインオンや自動プロビジョニングなどを有効にする方法について説明します。" 
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

#チュートリアル: Azure Active Directory と OverDrive Books の統合
  
このチュートリアルの目的は、Azure と OverDrive の統合を紹介することです。  
このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   OverDrive でのシングル サインオンが有効なサブスクリプション
  
このチュートリアルを完了すると、OverDrive に割り当てた Azure AD ユーザーは、OverDrive 会社サイト (サービス プロバイダーが開始したサインオン) で、アプリケーションにシングル サインオンまたはできるを使用して、 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。
  
このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  OverDrive のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-overdrive-books-tutorial/IC784462.png "Scenario")
##OverDrive のアプリケーション統合の有効化
  
このセクションでは、OverDrive のアプリケーション統合を有効にする方法について説明します。

###OverDrive のアプリケーション統合を有効にするには、次の手順に従います。

1.  左側のナビゲーション ウィンドウで、Azure 管理ポータルで、クリックして **Active Directory**します。

    ![Active Directory](./media/active-directory-saas-overdrive-books-tutorial/IC700993.png "Active Directory")

2.   **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3.  ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。

    ![アプリケーション](./media/active-directory-saas-overdrive-books-tutorial/IC700994.png "Applications")

4.  クリックして **追加** ページの下部にあります。

    ![アプリケーションの追加](./media/active-directory-saas-overdrive-books-tutorial/IC749321.png "Add application")

5.   **実行する操作** ダイアログ ボックスで、をクリックして **ギャラリーからアプリケーションを追加**します。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-overdrive-books-tutorial/IC749322.png "Add an application from gallerry")

6.   **検索ボックス**, 、型 **OverDrive**します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-overdrive-books-tutorial/IC784463.png "Application Gallery")

7.  結果ウィンドウで [ **OverDrive**, 、] をクリックし、 **完了** アプリケーションを追加します。

    ![OverDrive](./media/active-directory-saas-overdrive-books-tutorial/IC799950.png "OverDrive")
##シングル サインオンの構成
  
このセクションでは、ユーザーが SAML プロトコルに基づくフェデレーションを使用して、Azure AD でのユーザーのアカウントで OverDrive に対する認証を行えるようにする方法を説明します。

###シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **オーバー ドライブ** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、* * [シングル サインオン * * ダイアログ。

    ![シングル サインオンの有効化](./media/active-directory-saas-overdrive-books-tutorial/IC784465.png "Enable single sign-on")

2.   **どのようなユーザーが OverDrive へのサインオン** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、順にクリック **次**します。

    ![Configure single sign-on](./media/active-directory-saas-overdrive-books-tutorial/IC784466.png "Configure single sign-on")

3.   **アプリケーション URL の構成** ] ページの [、 **OverDrive サインイン URL** ] ボックスに、次のパターンを使用して URL を入力"*http://mslibrarytest.libraryreserve.com*"、] をクリックし、 **次**します。

    ![アプリケーション URL の構成](./media/active-directory-saas-overdrive-books-tutorial/IC784467.png "Configure App URL")

4.   **OverDrive でのシングル サインオンの構成** ] ページでメタデータ ファイルをダウンロードし、OverDrive のサポート チームに送信します。

    ![シングル サインオンの構成](./media/active-directory-saas-overdrive-books-tutorial/IC784468.png "Configure single sign-on")

    >[AZURE.NOTE]OverDrive のサポート チームのシングル サインオンを構成して、構成が完了したときに通知が送信します。

5.  Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![Configure single sign-on](./media/active-directory-saas-overdrive-books-tutorial/IC784469.png "Configure single sign-on")
##ユーザー プロビジョニングの構成
  
OverDrive へのユーザー プロビジョニングの構成にあたって必要な操作はありません。  
割り当てられているユーザーが OverDrive にログインしようとすると、必要に応じて OverDrive アカウントが自動的に作成されます。

>[AZURE.NOTE]他の OverDrive ユーザー アカウント作成ツールを使用することができますか、AAD ユーザー アカウントをプロビジョニング、OverDrive から提供されている Api です。

##ユーザーの割り当て
  
構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

###ユーザーを OverDrive に割り当てるには、次の手順に従います。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  * * OverDrive * * アプリケーション統合ページで、をクリックして **ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-overdrive-books-tutorial/IC784470.png "Assign Users")

3.  テスト ユーザーを選択して [ **割り当てる**, 、順にクリック **はい** して割り当てを確認します。

    ![あり](./media/active-directory-saas-overdrive-books-tutorial/IC767830.png "Yes")
  
シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。

