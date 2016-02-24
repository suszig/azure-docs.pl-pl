<properties 
    pageTitle="チュートリアル: Azure Active Directory と Qualtrics の統合 | Microsoft Azure" 
    description="Azure Active Directory で Qualtrics を使用して、シングル サインオンや自動プロビジョニングなどを有効にする方法について説明します。" 
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

#チュートリアル: Azure Active Directory と Qualtrics の統合
  
このチュートリアルでは、Azure と Qualtrics の統合について説明します。  
このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   Qualtrics でのシングル サインオンが有効なサブスクリプション
  
このチュートリアルを完了すると、Qualtrics に割り当てた Azure AD ユーザーは、Qualtrics 会社サイト (サービス プロバイダーが開始したサインオン) で、アプリケーションにシングル サインオンまたはできるを使用して、 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。
  
このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  Qualtrics のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-qualtrics-tutorial/IC789542.png "Scenario")
##Qualtrics のアプリケーション統合の有効化
  
このセクションでは、Qualtrics のアプリケーション統合を有効にする方法を説明します。

###Qualtrics のアプリケーション統合を有効にするには、次の手順に従います。

1.  左側のナビゲーション ウィンドウで、Azure 管理ポータルで、クリックして **Active Directory**します。

    ![Active Directory](./media/active-directory-saas-qualtrics-tutorial/IC700993.png "Active Directory")

2.   **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3.  ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。

    ![アプリケーション](./media/active-directory-saas-qualtrics-tutorial/IC700994.png "Applications")

4.  クリックして **追加** ページの下部にあります。

    ![アプリケーションの追加](./media/active-directory-saas-qualtrics-tutorial/IC749321.png "Add application")

5.   **実行する操作** ダイアログ ボックスで、をクリックして **ギャラリーからアプリケーションを追加**します。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-qualtrics-tutorial/IC749322.png "Add an application from gallerry")

6.   **検索ボックス**, 、型 **Qualtrics**します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-qualtrics-tutorial/IC789543.png "Application Gallery")

7.  結果ウィンドウで [ **Qualtrics**, 、クリックして **完了** アプリケーションを追加します。

    ![Qualtrics](./media/active-directory-saas-qualtrics-tutorial/IC789544.png "Qualtrics")
##シングル サインオンの構成
  
このセクションでは、ユーザーが SAML プロトコルに基づくフェデレーションを使用して、Azure AD でのユーザーのアカウントで Qualtrics に対する認証を行えるようにする方法を説明します。

###シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **Qualtrics** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、* * [シングル サインオン * * ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-qualtrics-tutorial/IC789545.png "Configure Single Sign-On")

2.   **どのようなユーザーの Qualtrics へのアクセス** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、] をクリックし、 **次**します。

    ![Configure Single Sign-On](./media/active-directory-saas-qualtrics-tutorial/IC789546.png "Configure Single Sign-On")

3.   **アプリケーション URL の構成** ] ページの [、 **Qualtrics サインオン URL** ] ボックスに、URL を入力 (例::"*https://ssotest2ut1.qualtrics.com*")、順にクリック **次**します。

    ![Configure App URL](./media/active-directory-saas-qualtrics-tutorial/IC789547.png "Configure App URL")

4.   **Qualtrics でのシングル サインオンの構成** ] ページで [ **メタデータのダウンロード**, 、お使いのコンピューターにメタデータ ファイルを保存します。

    ![Configure Single Sign-On](./media/active-directory-saas-qualtrics-tutorial/IC789548.png "Configure Single Sign-On")

5.  Qualtrics サポート チームに、メタデータ ファイルを送信します。

    >[AZURE.NOTE]シングル サインオンの構成では、Qualtrics サポート チームによって実行されます。 構成が完了すると、サポート チームから通知が届きます。

6.  Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-qualtrics-tutorial/IC789549.png "Configure Single Sign-On")
##ユーザー プロビジョニングの構成
  
Qualtrics へのユーザー プロビジョニングの構成にあたって必要な操作はありません。  
割り当てられたユーザーがアクセス パネルを使用して Qualtrics にログインしようとすると、そのユーザーが存在するかどうかが Qualtrics によって確認されます。  
使用可能なユーザー アカウントがない場合、ユーザー アカウントは Qualtrics により自動的に作成されます。
##ユーザーを割り当てる
  
構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

###ユーザーを Qualtrics に割り当てるには、次の手順に従います。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  * * Qualtrics * * アプリケーション統合ページで、をクリックして **ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-qualtrics-tutorial/IC789550.png "Assign Users")

3.  テスト ユーザーを選択して [ **割り当てる**, 、順にクリック **はい** して割り当てを確認します。

    ![あり](./media/active-directory-saas-qualtrics-tutorial/IC767830.png "Yes")
  
シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。
