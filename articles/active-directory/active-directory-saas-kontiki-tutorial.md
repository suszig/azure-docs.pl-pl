<properties 
    pageTitle="チュートリアル: Azure Active Directory と Kontiki の統合 | Microsoft Azure" 
    description="Azure Active Directory で Kontiki を使用して、シングル サインオンや自動プロビジョニングなどを有効にする方法について説明します。" 
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

#チュートリアル: Azure Active Directory と Kontiki の統合
  
このチュートリアルの目的は、Azure と Kontiki の統合を紹介することです。  
このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   Kontiki でのシングル サインオンが有効なサブスクリプション
  
このチュートリアルを完了すると、Kontiki に割り当てた Azure AD ユーザーは、Kontiki 会社サイト (サービス プロバイダーが開始したサインオン) で、アプリケーションにシングル サインオンまたはできるを使用して、 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。
  
このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  Kontiki のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-kontiki-tutorial/IC790235.png "Scenario")
##Kontiki のアプリケーション統合の有効化
  
このセクションでは、Kontiki のアプリケーション統合を有効にする方法について説明します。

###Kontiki のアプリケーション統合を有効にするには、次の手順に従います。

1.  左側のナビゲーション ウィンドウで、Azure 管理ポータルで、クリックして **Active Directory**します。

    ![Active Directory](./media/active-directory-saas-kontiki-tutorial/IC700993.png "Active Directory")

2.   **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3.  ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。

    ![アプリケーション](./media/active-directory-saas-kontiki-tutorial/IC700994.png "Applications")

4.  クリックして **追加** ページの下部にあります。

    ![アプリケーションの追加](./media/active-directory-saas-kontiki-tutorial/IC749321.png "Add application")

5.   **実行する操作** ダイアログ ボックスで、をクリックして **ギャラリーからアプリケーションを追加**します。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-kontiki-tutorial/IC749322.png "Add an application from gallerry")

6.   **検索ボックス**, 、型 **Kontiki**します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-kontiki-tutorial/IC790236.png "Application Gallery")

7.  結果ウィンドウで [ **Kontiki**, 、クリックして **完了** アプリケーションを追加します。

    ![Kontiki](./media/active-directory-saas-kontiki-tutorial/IC790237.png "Kontiki")
##シングル サインオンの構成
  
このセクションでは、SAML プロトコルに基づくフェデレーションを使用して、ユーザーが Azure AD のアカウントで Kontiki に対する認証を行えるようにする方法を説明します。

###シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **Kontiki** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、* * [シングル サインオン * * ダイアログ。

    ![シングル サインオンの構成](./media/active-directory-saas-kontiki-tutorial/IC790238.png "Configure Single SignOn")

2.   **どのようなユーザーが Kontiki にサインオン** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、] をクリックし、 **次**します。

    ![シングル サインオンの構成](./media/active-directory-saas-kontiki-tutorial/IC790239.png "Configure Single SignOn")

3.   **アプリケーション URL の構成** ] ページの [、 **Kontiki サインオン URL** ] ボックスに、型 URL を使って、ユーザーを Kontiki にサインオン (例::"*https://company.mc.eval.kontiki.com/*")、順にクリック **次**します。

    ![Configure App URL](./media/active-directory-saas-kontiki-tutorial/IC790240.png "Configure App URL")

4.   **Kontiki でのシングル サインオンの構成** ] ページで [ **メタデータのダウンロード**, 、お使いのコンピューターにメタデータ ファイルを保存します。

    ![シングル サインオンの構成](./media/active-directory-saas-kontiki-tutorial/IC790241.png "Configure Single SignOn")

5.  Kontiki サポート チームに、メタデータ ファイルを送信します。

    >[AZURE.NOTE] シングル サインオンの構成では、Kontiki サポート チームによって実行されます。 構成が完了すると、サポート チームから通知が届きます。

6.  Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![シングル サインオンの構成](./media/active-directory-saas-kontiki-tutorial/IC790242.png "Configure Single SignOn")
##ユーザー プロビジョニングの構成
  
Kontiki へのユーザー プロビジョニングの構成にあたって必要な操作はありません。  
割り当てられたユーザーがアクセス パネルを使用して Kontiki にログインしようとすると、そのユーザーが存在するかどうかが Kontiki によって確認されます。  
使用可能なユーザー アカウントがない場合、ユーザー アカウントは Kontiki により自動的に作成されます。
##ユーザーの割り当て
  
構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

###ユーザーを Kontiki に割り当てるには、次の手順に従います。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  * * Kontiki * * アプリケーション統合ページで、をクリックして **ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-kontiki-tutorial/IC790243.png "Assign Users")

3.  テスト ユーザーを選択して [ **割り当てる**, 、順にクリック **はい** して割り当てを確認します。

    ![あり](./media/active-directory-saas-kontiki-tutorial/IC767830.png "Yes")
  
シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。
