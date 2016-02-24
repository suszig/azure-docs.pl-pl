<properties 
    pageTitle="チュートリアル: Azure Active Directory と Brightspace by Desire2Learn の統合 | Microsoft Azure" 
    description="Azure Active Directory で Brightspace by Desire2Learn を使用して、シングル サインオンや自動プロビジョニングなどを有効にする方法について説明します。" 
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

#チュートリアル: Azure Active Directory と Brightspace by Desire2Learn の統合

このチュートリアルでは、Azure と Brightspace by Desire2Learn の統合について説明します。  
このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   Brightspace by Desire2Learn でのシングル サインオンが有効なサブスクリプション

このチュートリアルを完了すると、Brightspace by Desire2Learn に割り当てた Azure AD ユーザーができるよう、brightspace by Desire2Learn 会社サイト (サービス プロバイダーが開始したサインオン)、またはを使用してアプリケーションにシングル サインオン、 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。

このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  Brightspace by Desire2Learn のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-brightspace-desire2learn-tutorial/IC798957.png "Scenario")
##Brightspace by Desire2Learn のアプリケーション統合の有効化

このセクションでは、Brightspace by Desire2Learn のアプリケーション統合を有効にする方法を説明します。

###Brightspace by Desire2Learn のアプリケーション統合を有効にするには、次の手順に従います。

1.  左側のナビゲーション ウィンドウで、Azure 管理ポータルで、クリックして **Active Directory**します。

    ![Active Directory](./media/active-directory-saas-brightspace-desire2learn-tutorial/IC700993.png "Active Directory")

2.   **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3.  ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。

    ![アプリケーション](./media/active-directory-saas-brightspace-desire2learn-tutorial/IC700994.png "Applications")

4.  クリックして **追加** ページの下部にあります。

    ![アプリケーションの追加](./media/active-directory-saas-brightspace-desire2learn-tutorial/IC749321.png "Add application")

5.   **実行する操作** ダイアログ ボックスで、をクリックして **ギャラリーからアプリケーションを追加**します。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-brightspace-desire2learn-tutorial/IC749322.png "Add an application from gallerry")

6.   **検索ボックス**, 、型 **Brightspace by Desire2Learn**します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-brightspace-desire2learn-tutorial/IC798958.png "Apllication Gallery")

7.  結果ウィンドウで [ **Brightspace by Desire2Learn**, 、クリックして **完了** アプリケーションを追加します。

    ![Brightspace by Desire2Learn](./media/active-directory-saas-brightspace-desire2learn-tutorial/IC799321.png "Brightspace by Desire2Learn")
##シングル サインオンの構成

このセクションでは、SAML プロトコルに基づくフェデレーションを使用して、ユーザーが Azure AD のアカウントで Brightspace by Desire2Learn に対する認証を行えるようにする方法を説明します。

###シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **Brightspace by Desire2Learn** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、* * [シングル サインオン * * ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-brightspace-desire2learn-tutorial/IC798959.png "Configure Single Sign-On")

2.   **どのようなユーザーの Brightspace by Desire2Learn に** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、順にクリック **次**します。

    ![Configure Single Sign-On](./media/active-directory-saas-brightspace-desire2learn-tutorial/IC798960.png "Configure Single Sign-On")

3.   **アプリケーション URL の構成** ] ページで、次の手順を実行します。

    ![Configure App URL](./media/active-directory-saas-brightspace-desire2learn-tutorial/IC798961.png "Configure App URL")

    1.   **サインオン URL** URL は、ユーザーへのサインインに使用] ボックスに、型、 **Brightspace by Desire2Learn** (例:: *https://partnershowcase.desire2learn.com/Shibboleth.sso/Login?entityID=https://sts.windows-ppe.net/5caf9349-fd93-4a74-b064-0070f65bfb49/&target=https%3A%2F%2Fpartnershowcase.desire2learn.com%2Fd2l%2FshibbolethSSO%2Faspinfo.asp*)。
    2.  クリックして **次へ]**

4.   **Brightspace by Desire2Learn でのシングル サインオンの構成** ] ページで、メタデータをダウンロードする] をクリックして **メタデータのダウンロード**, 、コンピューターにメタデータを保存します。

    ![Configure Single Sign-On](./media/active-directory-saas-brightspace-desire2learn-tutorial/IC798962.png "Configure Single Sign-On")

5.  ダウンロードしたメタデータ ファイルを Brightspace by Desire2Learn サポート チームに送信します。

    >[AZURE.NOTE] Brightspace by Desire2Learn サポート チームは、実際の SSO 構成を行うことができます。
    ご使用のサブスクリプションで SSO が有効になると通知されます。

6.  Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-brightspace-desire2learn-tutorial/IC798963.png "Configure Single Sign-On")
##ユーザー プロビジョニングの構成

Azure AD ユーザーが Brightspace by Desire2Learn にログインできるようにするには、ユーザーを Brightspace by Desire2Learn にプロビジョニングする必要があります。  
Brightspace by Desire2Learn の場合、Brightspace by Desire2Learn サポート チームがユーザー アカウントを作成する必要があります。

>[AZURE.NOTE] Api によって提供される Brightspace by Desire2Learn にプロビジョニング Azure Active Directory ユーザー アカウントや Desire2Learn ユーザー アカウント作成ツールで他の Brightspace を使用できます。

##ユーザーの割り当て

構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

###ユーザーを Brightspace by Desire2Learn に割り当てるには、次の手順を実行します。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  * * Brightspace by Desire2Learn * * アプリケーション統合ページで、をクリックして **ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-brightspace-desire2learn-tutorial/IC798964.png "Assign Users")

3.  テスト ユーザーを選択して [ **割り当てる**, 、順にクリック **はい** して割り当てを確認します。

    ![あり](./media/active-directory-saas-brightspace-desire2learn-tutorial/IC767830.png "Yes")

シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。

