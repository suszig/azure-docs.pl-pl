<properties
    pageTitle="チュートリアル: Azure Active Directory と ABa Sainsburys Connect の統合 | Microsoft Azure" 
    description="Azure Active Directory で ABa Sainsburys Connect を使用して、シングル サインオンや自動プロビジョニングなどを有効にする方法について説明します。" 
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

#チュートリアル: Azure Active Directory と ABa Sainsburys Connect の統合

このチュートリアルでは、Azure と ABa Sainsburys Connect の統合について説明します。  
このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   Aba Sainsburys Connect でのシングル サインオンが有効なサブスクリプション

このチュートリアルを完了すると、Aba Sainsburys 接続に割り当てた Azure AD ユーザーは、の Aba Sainsburys 接続の会社サイト (サービス プロバイダーが開始したサインオン) で、アプリケーションにシングル サインオンまたはできるを使用して、 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。

このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  Aba Sainsburys Connect のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-aba-sainsburys-connect-tutorial/IC807723.png "Scenario")
##Aba Sainsburys Connect のアプリケーション統合の有効化

このセクションでは、Aba Sainsburys Connect のアプリケーション統合を有効にする方法を説明します。

###Aba Sainsburys Connect のアプリケーション統合を有効にするには、次の手順に従います。

1.  左側のナビゲーション ウィンドウで、Azure 管理ポータルで、クリックして **Active Directory**します。

    ![Active Directory](./media/active-directory-saas-aba-sainsburys-connect-tutorial/IC700993.png "Active Directory")

2.   **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3.  ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。

    ![アプリケーション](./media/active-directory-saas-aba-sainsburys-connect-tutorial/IC700994.png "Applications")

4.  クリックして **追加** ページの下部にあります。

    ![アプリケーションの追加](./media/active-directory-saas-aba-sainsburys-connect-tutorial/IC749321.png "Add application")

5.   **実行する操作** ダイアログ ボックスで、をクリックして **ギャラリーからアプリケーションを追加**します。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-aba-sainsburys-connect-tutorial/IC749322.png "Add an application from gallerry")

6.   **検索ボックス**, 、型 **Aba Sainsburys 接続**します。

    ![ABa Sainsburys Connect](./media/active-directory-saas-aba-sainsburys-connect-tutorial/IC807724.png "Aba Sainsburys Connect")

7.  結果ウィンドウで [ **Aba Sainsburys 接続**, 、] をクリックし、 **完了** アプリケーションを追加します。

    ![ABa Sainsburys Connect](./media/active-directory-saas-aba-sainsburys-connect-tutorial/IC807725.png "Aba Sainsburys Connect")
##シングル サインオンの構成

このセクションでは、ユーザーが SAML プロトコルに基づくフェデレーションを使用して、Azure AD でのユーザーのアカウントで Aba Sainsburys Connect に対する認証を行えるようにする方法を説明します。

###シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **Aba Sainsburys 接続** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、* * [シングル サインオン * * ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-aba-sainsburys-connect-tutorial/IC807726.png "Configure Single Sign-On")

2.   **どのようなユーザー Aba Sainsburys 接続へのサインオンに** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、順にクリック **次**します。

    ![Configure Single Sign-On](./media/active-directory-saas-aba-sainsburys-connect-tutorial/IC807727.png "Configure Single Sign-On")

3.   **アプリケーション設定の構成** ] ページで、次の手順を実行します。

    ![Configure App Settings](./media/active-directory-saas-aba-sainsburys-connect-tutorial/IC807728.png "Configure App Settings")

    1.   **サインオン URL** ユーザー Aba Sainsburys 接続アプリケーションにサインオンする URL を使用] ボックスに、種類 (例:: *https://myaba.co.uk/client-access/sainsburys/saml.php*)。
    2.  クリックして **次へ]**

4.   **Aba Sainsburys 接続でのシングル サインオンの構成** ] ページで、メタデータをダウンロードする] をクリックして **メタデータのダウンロード**, 、お使いのコンピューターにメタデータ ファイルを保存します。

    ![Configure Single Sign-On](./media/active-directory-saas-aba-sainsburys-connect-tutorial/IC807729.png "Configure Single Sign-On")

5.  ダウンロードしたメタデータ ファイルを Aba Sainsburys Connect サポート チームに送信します。

    >[AZURE.NOTE] Aba Sainsburys 接続サポート チームは、実際の SSO 構成を行うことにします。
    ご使用のサブスクリプションで SSO が有効になると通知されます。

6.  Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-aba-sainsburys-connect-tutorial/IC807730.png "Configure Single Sign-On")
##ユーザー プロビジョニングの構成

Azure AD ユーザーが Aba Sainsburys Connect にログインできるようにするには、ユーザーを Aba Sainsburys Connect にプロビジョニングする必要があります。  
Aba Sainsburys Connect の場合、ユーザー アカウントは、Aba Sainsburys Connect のサポート チームによって作成される必要があります。

>[AZURE.NOTE] 他の Aba Sainsburys 接続ユーザー アカウント作成ツールを使用するまたは Api Connect によって提供される Aba Sainsburys プロビジョニング Azure Active Directory ユーザー アカウントです。

##ユーザーの割り当て

構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

###Aba Sainsburys Connect にユーザーを割り当てるには、次の手順を実行します。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  * * Aba Sainsburys 接続 * * アプリケーション統合ページで、をクリックして **ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-aba-sainsburys-connect-tutorial/IC807731.png "Assign Users")

3.  テスト ユーザーを選択して [ **割り当てる**, 、順にクリック **はい** して割り当てを確認します。

    ![あり](./media/active-directory-saas-aba-sainsburys-connect-tutorial/IC767830.png "Yes")

シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。

