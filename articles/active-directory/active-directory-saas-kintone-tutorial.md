<properties 
    pageTitle="チュートリアル: Azure Active Directory と Kintone の統合 | Microsoft Azure" 
    description="Azure Active Directory で Kintone を使用して、シングル サインオンや自動プロビジョニングなどを有効にする方法について説明します。" 
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
    ms.date="10/20/2015" 
    ms.author="markvi" />

#チュートリアル: Azure Active Directory と Kintone の統合
  
このチュートリアルの目的は、Azure と Kintone の統合を紹介することです。  
このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   Kintone でのシングル サインオンが有効なサブスクリプション
  
このチュートリアルを完了すると、Kintone に割り当てた Azure AD ユーザーは、Kintone 会社サイト (サービス プロバイダーが開始したサインオン) で、アプリケーションにシングル サインオンまたはできるを使用して、 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。
  
このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  Kintone のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-kintone-tutorial/IC785859.png "Scenario")
##Kintone のアプリケーション統合の有効化
  
このセクションでは、Kintone のアプリケーション統合を有効にする方法について説明します。

###Kintone のアプリケーション統合を有効にするには、次の手順に従います。

1.  左側のナビゲーション ウィンドウで、Azure 管理ポータルで、クリックして **Active Directory**します。

    ![Active Directory](./media/active-directory-saas-kintone-tutorial/IC700993.png "Active Directory")

2.   **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3.  ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。

    ![アプリケーション](./media/active-directory-saas-kintone-tutorial/IC700994.png "Applications")

4.  クリックして **追加** ページの下部にあります。

    ![アプリケーションの追加](./media/active-directory-saas-kintone-tutorial/IC749321.png "Add application")

5.   **実行する操作** ダイアログ ボックスで、をクリックして **ギャラリーからアプリケーションを追加**します。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-kintone-tutorial/IC749322.png "Add an application from gallerry")

6.   **検索ボックス**, 、型 **Kintone**します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-kintone-tutorial/IC785867.png "Application Gallery")

7.  結果ウィンドウで [ **Kintone**, 、クリックして **完了** アプリケーションを追加します。

    ![Kintone](./media/active-directory-saas-kintone-tutorial/IC785871.png "Kintone")
##シングル サインオンの構成
  
このセクションでは、SAML プロトコルに基づくフェデレーションを使用して、ユーザーが Azure AD のアカウントで Kintone に対する認証を行えるようにする方法を説明します。

###シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **Kintone** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、* * [シングル サインオン * * ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-kintone-tutorial/IC785872.png "Configure Single Sign-On")

2.   **どのようなユーザーが Kintone へのサインオン** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、順にクリック **次**します。

    ![Configure Single Sign-On](./media/active-directory-saas-kintone-tutorial/IC785873.png "Configure Single Sign-On")

3.   **アプリケーション URL の構成** ] ページの [、 **Kintone サインイン URL** ] ボックスに、次のパターンを使用して URL を入力"*https://company.kintone.com*"、] をクリックし、 **次**します。

    ![アプリケーション URL の構成](./media/active-directory-saas-kintone-tutorial/IC785875.png "Configure App URL")

4.   **Kintone でのシングル サインオン構成** ] ページで、証明書をダウンロードする] をクリックして **証明書のダウンロード**, 、お使いのコンピューターに証明書ファイルを保存します。

    ![Configure Single Sign-On](./media/active-directory-saas-kintone-tutorial/IC785878.png "Configure Single Sign-On")

5.  ログインする別の web ブラウザーのウィンドウで、 **Kintone** 会社サイトに管理者として。

6.  クリックして **設定**します。

    ![設定](./media/active-directory-saas-kintone-tutorial/IC785879.png "Settings")

7.  クリックして **ユーザーとシステム管理**します。

    ![ユーザーとシステム管理](./media/active-directory-saas-kintone-tutorial/IC785880.png "Users & System Administration")

8.   **システム管理 \ > セキュリティ** クリックして **ログイン**します。

    ![ログイン](./media/active-directory-saas-kintone-tutorial/IC785881.png "Login")

9.  クリックして **Enable SAML authentication**します。

    ![SAML 認証](./media/active-directory-saas-kintone-tutorial/IC785882.png "SAML Authentication")

10. [SAML 承認] セクションで、次の手順に従います。

    ![SAML 認証](./media/active-directory-saas-kintone-tutorial/IC785883.png "SAML Authentication")

    1.  Azure ポータルで、 **Kintone でのシングル サインオンの構成** ] ダイアログ ページでコピー、 **リモート ログイン URL** 値に設定して、貼り付けます、 **ログイン URL** ] ボックスに貼り付けます。
    2.  Azure ポータルで、 **Kintone でのシングル サインオンの構成** ] ダイアログ ページでコピー、 **リモート ログアウト URL** 値に設定して、貼り付けます、 **ログアウト URL** ] ボックスに貼り付けます。
    3.  クリックして **参照** をダウンロードした証明書をアップロードします。
    4.  クリックして **保存**します。

11. Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-kintone-tutorial/IC785884.png "Configure Single Sign-On")
##ユーザー プロビジョニングの構成
  
Azure AD ユーザーが Kintone にログインできるようにするには、そのユーザーを Kintone にプロビジョニングする必要があります。  
Kintone の場合、プロビジョニングは手動で行います。

###ユーザー アカウントをプロビジョニングするには、次の手順を実行します。

1.  ログインして **Kintone** 会社サイトに管理者として。

2.  クリックして **設定**します。

    ![設定](./media/active-directory-saas-kintone-tutorial/IC785879.png "Settings")

3.  クリックして **ユーザーとシステム管理**します。

    ![ユーザーとシステム管理](./media/active-directory-saas-kintone-tutorial/IC785880.png "User & System Administration")

4.   **ユーザーの管理**, をクリックして **部門とユーザー**します。

    ![部門とユーザー](./media/active-directory-saas-kintone-tutorial/IC785888.png "Department & Users")

5.  クリックして **新しいユーザー**します。

    ![新しいユーザー](./media/active-directory-saas-kintone-tutorial/IC785889.png "New Users")

6.   **新しいユーザー** セクションで、次の手順に従います。

    ![新しいユーザー](./media/active-directory-saas-kintone-tutorial/IC785890.png "New Users")

    1.  型 a **表示名**, 、**ログイン名**, 、**新しいパスワード**, 、**パスワードの確認**, 、**電子メール アドレス** など関連するテキストに、プロビジョニングする有効な AAD アカウントの情報です。
    2.  クリックして **保存**します。

>[AZURE.NOTE] 他の Kintone ユーザー アカウント作成ツールを使用することができますか、AAD ユーザー アカウントをプロビジョニング、Kintone から提供されている Api です。

##ユーザーの割り当て
  
構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

###ユーザーを Kintone に割り当てるには、次の手順に従います。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  * * Kintone * * アプリケーション統合ページで、をクリックして **ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-kintone-tutorial/IC785891.png "Assign Users")

3.  テスト ユーザーを選択して [ **割り当てる**, 、順にクリック **はい** して割り当てを確認します。

    ![あり](./media/active-directory-saas-kintone-tutorial/IC767830.png "Yes")
  
シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。
