<properties 
    pageTitle="チュートリアル: Azure Active Directory と Cornerstone OnDemand の統合 | Microsoft Azure" 
    description="Azure Active Directory で Cornerstone OnDemand を使用して、シングル サインオンや自動プロビジョニングなどを有効にする方法について説明します。" 
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

#チュートリアル: Azure Active Directory と Cornerstone OnDemand の統合

このチュートリアルでは、Azure と Cornerstone OnDemand の統合について説明します。  
このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   Cornerstone OnDemand テナント

このチュートリアルを完了すると、Cornerstone OnDemand に割り当てた Azure AD ユーザーは、Cornerstone OnDemand 会社サイト (サービス プロバイダーが開始したサインオン) で、アプリケーションにシングル サインオンまたはできるを使用して、 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。

このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  Cornerstone OnDemand のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781593.png "Scenario")
##Cornerstone OnDemand のアプリケーション統合の有効化

このセクションでは、Cornerstone OnDemand のアプリケーション統合を有効にする方法について説明します。

###Cornerstone OnDemand のアプリケーション統合を有効にするには、次の手順を実行します。

1.  左側のナビゲーション ウィンドウで、Azure 管理ポータルで、クリックして **Active Directory**します。

    ![Active Directory](./media/active-directory-saas-cornerstone-ondemand-tutorial/IC700993.png "Active Directory")

2.   **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3.  ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。

    ![アプリケーション](./media/active-directory-saas-cornerstone-ondemand-tutorial/IC700994.png "Applications")

4.  クリックして **追加** ページの下部にあります。

    ![アプリケーションの追加](./media/active-directory-saas-cornerstone-ondemand-tutorial/IC749321.png "Add application")

5.   **実行する操作** ダイアログ ボックスで、をクリックして **ギャラリーからアプリケーションを追加**します。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-cornerstone-ondemand-tutorial/IC749322.png "Add an application from gallerry")

6.   **検索ボックス**, 、型 **cornerstone ondemand**します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781594.png "Application Gallery")

7.  結果ウィンドウで [ **Cornerstone OnDemand**, 、クリックして **完了** アプリケーションを追加します。

    ![Cornerstone OnDemand](./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781595.png "Cornerstone OnDemand")
##シングル サインオンの構成

このセクションでは、ユーザーが SAML プロトコルに基づくフェデレーションを使用して、Azure AD でのユーザーのアカウントで Cornerstone OnDemand に対する認証を行うことができるようにする方法を説明します。

###シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **Cornerstone OnDemand** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、* * [シングル サインオン * * ダイアログ。

    ![Enable Single Sign-On](./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781596.png "Enable Single Sign-On")

2.   **どのようなユーザーを Cornerstone OnDemand へのサインオン** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、] をクリックし、 **次**します。

    ![Microsoft Azure AD シングル サインオン](./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781597.png "Microsoft Azure AD Single Sign-On")

3.   **アプリケーション URL の構成** ] ページの [、 **Cornerstone OnDemand サインイン URL** ] ボックスに、次のパターンを使用して URL を入力"*http://company.csod.com*"、] をクリックし、 **次**します。

    ![Configure App URL](./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781598.png "Configure App URL")

4.   **Cornerstone OnDemand でのシングル サインオンの構成** ] ページで、証明書をダウンロードする] をクリックして **証明書のダウンロード**, 、お使いのコンピューターにローカルで証明書ファイルを保存します。

    ![Configure Single Sign-On](./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781599.png "Configure Single Sign-On")

5.  次のアイテムを Cornerstone OnDemand サポート チームに送信します。

    1.  ダウンロードした証明書
    2.   **リモート ログイン URL** 値
    3.   **リモート ログアウト URL** 値。

    >[AZURE.NOTE] シングル サインオンは、Cornerstone OnDemand サポート チームが構成する必要があります。
    構成が完了すると、サポート チームから通知が届きます。

6.  シングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781600.png "Configure Single Sign-On")
##ユーザー プロビジョニングの構成

Azure AD ユーザーが Cornerstone OnDemand にログインできるようにするには、そのユーザーを Cornerstone OnDemand にプロビジョニングする必要があります。  
Cornerstone OnDemand の場合、プロビジョニングは手動で行います。

###ユーザー プロビジョニングを構成するには、次の手順に従います。

1.  プロビジョニングする Azure AD ユーザーに関する情報 (名前、電子メールなど) を、Cornerstone OnDemand サポート チームに送信します。

>[AZURE.NOTE] 他の Cornerstone OnDemand ユーザー アカウント作成ツールを使用することができますか、AAD ユーザー アカウントをプロビジョニング、Cornerstone OnDemand 提供の Api です。

##ユーザーの割り当て

構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

###ユーザーを Cornerstone OnDemand に割り当てるには、次の手順を実行します。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  * * Cornerstone OnDemand * * アプリケーション統合ページで、をクリックして **ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-cornerstone-ondemand-tutorial/IC775564.png "Assign users")

3.  テスト ユーザーを選択して [ **割り当てる**, 、順にクリック **はい** して割り当てを確認します。

    ![ユーザーの割り当て](./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781601.png "Assign Users")

シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。

