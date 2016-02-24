<properties 
    pageTitle="チュートリアル: Azure Active Directory と Lynda.com の統合 | Microsoft Azure" 
    description="Azure Active Directory で Lynda.com を使用して、シングル サインオンや自動プロビジョニングなどを有効にする方法について説明します。" 
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

#チュートリアル: Azure Active Directory と Lynda.com の統合
  
このチュートリアルでは、Azure と Lynda.com の統合について説明します。  
このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   Lynda.com テナント
  
このチュートリアルを完了すると、Lynda.com に割り当てた Azure AD ユーザーは、Lynda.com の会社サイト (サービス プロバイダーが開始したサインオン) で、アプリケーションにシングル サインオンまたはできるを使用して、 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。
  
このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  Lynda.com のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-lynda-tutorial/IC781046.png "Scenario")
##Lynda.com のアプリケーション統合の有効化
  
このセクションでは、Lynda.com のアプリケーション統合を有効にする方法について説明します。

###Lynda.com のアプリケーション統合を有効にするには、次の手順に従います。

1.  左側のナビゲーション ウィンドウで、Azure 管理ポータルで、クリックして **Active Directory**します。

    ![Active Directory](./media/active-directory-saas-lynda-tutorial/IC700993.png "Active Directory")

2.   **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3.  ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。

    ![アプリケーション](./media/active-directory-saas-lynda-tutorial/IC700994.png "Applications")

4.  クリックして **追加** ページの下部にあります。

    ![アプリケーションの追加](./media/active-directory-saas-lynda-tutorial/IC749321.png "Add application")

5.   **実行する操作** ダイアログ ボックスで、をクリックして **ギャラリーからアプリケーションを追加**します。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-lynda-tutorial/IC749322.png "Add an application from gallerry")

6.   **検索ボックス**, 、型 **Lynda.com**します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-lynda-tutorial/IC777524.png "Application Gallery")

7.  結果ウィンドウで [ **Lynda.com**, 、クリックして **完了** アプリケーションを追加します。

    ![Lynda.com](./media/active-directory-saas-lynda-tutorial/IC777525.png "Lynda.com")
##シングル サインオンの構成
  
このセクションでは、SAML プロトコルに基づくフェデレーションを使用して、ユーザーが Azure AD のアカウントで Lynda.com に対する認証を行うことができるようにする方法を説明します。

>[AZURE.IMPORTANT]Lynda.com テナントの [シングル サインオンを構成するためには、まずこの機能を有効にする Lynda.com テクニカル サポートに連絡する必要があります。

###シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **Lynda.com** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、* * [シングル サインオン * * ダイアログ。

    ![Configure single sign-on](./media/active-directory-saas-lynda-tutorial/IC777526.png "Configure single sign-on")

2.   **どのようなユーザーを Lynda.com へのサインオン** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、] をクリックし、 **次**します。

    ![Configure single sign-on](./media/active-directory-saas-lynda-tutorial/IC777527.png "Configure single sign-on")

3.   **アプリケーション URL の構成** ] ページの [、 **Lynda.com サインイン URL** ] ボックスに、Lynda.com テナントの URL を入力 (例:: *https://shib.lynda.com/Shibboleth.sso/InCommon?providerId=https://sts.windows-ppe.net/6247032d-9415-403c-b72b-277e3fb6f2c8/&target= https://shib.lynda.com/InCommon*)、順にクリック **次**します。

    ![アプリケーション URL の構成](./media/active-directory-saas-lynda-tutorial/IC781047.png "Configure app URL")

4.   **Lynda.com でのシングル サインオンの構成** ] ページで、メタデータをダウンロードする] をクリックして **メタデータのダウンロード**, 、お使いのコンピューターにローカルで証明書ファイルを保存します。

    ![Configure single sign-on](./media/active-directory-saas-lynda-tutorial/IC777529.png "Configure single sign-on")

5.  ダウンロードしたメタデータ ファイルを Lynda.com サポート チームに送信します。 シングル サインオンの構成は、Lynda.com のサポート チームが行います。

6.  Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![Configure single sign-on](./media/active-directory-saas-lynda-tutorial/IC777530.png "Configure single sign-on")
##ユーザー プロビジョニングの構成
  
Lynda.com へのユーザー プロビジョニングの構成にあたって必要な操作はありません。  
割り当て済みユーザーがアクセス パネルを使用して Lynda.com にログインしようとすると、そのユーザーが存在するかどうかが Lynda.com によって確認されます。  
使用可能なユーザー アカウントがない場合、ユーザー アカウントは Lynda.com により自動的に作成されます。

>[AZURE.NOTE]他の Lynda.com ユーザー アカウント作成ツールを使用するまたは AAD ユーザー アカウントをプロビジョニング、Lynda.com から提供されている Api です。

##ユーザーの割り当て
  
構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

###ユーザーを Lynda.com に割り当てるには、次の手順に従います。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  **Lynda.com で * * アプリケーション統合ページで、をクリックして **ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-lynda-tutorial/IC777531.png "Assign users")

3.  テスト ユーザーを選択して [ **割り当てる**, 、順にクリック **はい** して割り当てを確認します。

    ![あり](./media/active-directory-saas-lynda-tutorial/IC767830.png "Yes")
  
シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。
