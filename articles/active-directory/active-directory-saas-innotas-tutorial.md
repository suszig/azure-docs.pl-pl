<properties 
    pageTitle="チュートリアル: Azure Active Directory と Innotas の統合 | Microsoft Azure"
    description="Azure Active Directory で Innotas を使用して、シングル サインオンや自動プロビジョニングなどを有効にする方法について説明します。" 
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

#チュートリアル: Azure Active Directory と Innotas の統合
  
このチュートリアルでは、Azure と Innotas の統合について説明します。  
このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   Innotas テナント
  
このチュートリアルを完了すると、Innotas に割り当てた Azure AD ユーザーは、Innotas 会社サイト (サービス プロバイダーが開始したサインオン) で、アプリケーションにシングル サインオンまたはできるを使用して、 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。
  
このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  Innotas のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-innotas-tutorial/IC777331.png "Scenario")
##Innotas のアプリケーション統合の有効化
  
このセクションでは、Innotas のアプリケーション統合を有効にする方法について説明します。

###Innotas のアプリケーション統合を有効にするには、次の手順を実行します。

1.  左側のナビゲーション ウィンドウで、Azure 管理ポータルで、クリックして **Active Directory**します。

    ![Active Directory](./media/active-directory-saas-innotas-tutorial/IC700993.png "Active Directory")

2.   **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3.  ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。

    ![アプリケーション](./media/active-directory-saas-innotas-tutorial/IC700994.png "Applications")

4.  クリックして **追加** ページの下部にあります。

    ![アプリケーションの追加](./media/active-directory-saas-innotas-tutorial/IC749321.png "Add application")

5.   **実行する操作** ダイアログ ボックスで、をクリックして **ギャラリーからアプリケーションを追加**します。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-innotas-tutorial/IC749322.png "Add an application from gallerry")

6.   **検索ボックス**, 、型 **Innotas**します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-innotas-tutorial/IC777332.png "Application gallery")

7.  結果ウィンドウで [ **Innotas**, 、クリックして **完了** アプリケーションを追加します。

    ![Innotas](./media/active-directory-saas-innotas-tutorial/IC777333.png "Innotas")
##シングル サインオンの構成
  
このセクションでは、SAML プロトコルに基づくフェデレーションを使用して、ユーザーが Azure AD のアカウントで Innotas に対する認証を行うことができるようにする方法を説明します。

###シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **Innotas** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、* * [シングル サインオン * * ダイアログ。

    ![シングル サインオンの構成](./media/active-directory-saas-innotas-tutorial/IC777334.png "Configure single sign-on")

2.   **どのようなユーザーの Innotas へのアクセス** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、] をクリックし、 **次**します。

    ![Configure single sign-on](./media/active-directory-saas-innotas-tutorial/IC777335.png "Configure single sign-on")

3.   **アプリケーション URL の構成** ] ページで、 **Innotas サインイン URL** ] ボックスに、次のパターンを使用して URL を入力"*https://\<tenant-name\>。Innotas.com*"、クリックして **次**します。

    ![アプリケーション URL の構成](./media/active-directory-saas-innotas-tutorial/IC777336.png "Configure app URL")

4.   **Innotas でのシングル サインオンの構成** ] ページで、メタデータをダウンロードする] をクリックして **メタデータのダウンロード**, 、し、データ ファイルをローカルでとして **c:\\InnotasMetaData.xml**します。

    ![シングル サインオンの構成](./media/active-directory-saas-innotas-tutorial/IC777337.png "Configure single sign-on")

5.  メタデータ ファイルを Innotas サポート チームに転送します。 サポート チームは、シングル サインオンを構成する必要があります。

6.  シングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![シングル サインオンの構成](./media/active-directory-saas-innotas-tutorial/IC777338.png "Configure single sign-on")
##ユーザー プロビジョニングの構成
  
Innotas へのユーザー プロビジョニングの構成にあたって必要な操作はありません。  
割り当てられたユーザーがアクセス パネルを使用して Innotas にログインしようとすると、そのユーザーが存在するかどうかが Innotas によって確認されます。  
使用可能なユーザー アカウントがない場合、ユーザー アカウントは Innotas によって自動的に作成されます。
##ユーザーの割り当て
  
構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

###ユーザーを Innotas に割り当てるには、次の手順を実行します。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  * * Innotas * * アプリケーション統合ページで、をクリックして **ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-innotas-tutorial/IC777339.png "Assign users")

3.  テスト ユーザーを選択して [ **割り当てる**, 、順にクリック **はい** して割り当てを確認します。

    ![あり](./media/active-directory-saas-innotas-tutorial/IC767830.png "Yes")
  
シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。

