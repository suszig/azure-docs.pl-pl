<properties 
    pageTitle="チュートリアル: Azure Active Directory と ScreenSteps の統合 | Microsoft Azure" 
    description="Azure Active Directory で ScreenSteps を使用して、シングル サインオンや自動プロビジョニングなどを有効にする方法について説明します。" 
    services="active-directory" 
    authors="markusvi"  
    documentationCenter="na" manager="stevenpo"/>
<tags 
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="10/22/2015" 
    ms.author="markvi" />

#チュートリアル: Azure Active Directory と ScreenSteps の統合
  
このチュートリアルの目的は、Azure と ScreenSteps の統合を示すことです。  
このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   ScreenSteps テナント
  
このチュートリアルを完了すると、ScreenSteps に割り当てた Azure AD ユーザーは、ScreenSteps の会社サイト (サービス プロバイダーが開始したサインオン) で、アプリケーションにシングル サインオンまたはできるを使用して、 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。
  
このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  ScreenSteps のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-screensteps-tutorial/IC778516.png "Scenario")
##ScreenSteps のアプリケーション統合の有効化
  
このセクションでは、ScreenSteps のアプリケーション統合を有効にする方法を説明します。

###ScreenSteps のアプリケーション統合を有効にするには、次の手順に従います。

1.  左側のナビゲーション ウィンドウで、Azure 管理ポータルで、クリックして **Active Directory**します。

    ![Active Directory](./media/active-directory-saas-screensteps-tutorial/IC700993.png "Active Directory")

2.   **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3.  ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。

    ![アプリケーション](./media/active-directory-saas-screensteps-tutorial/IC700994.png "Applications")

4.  クリックして **追加** ページの下部にあります。

    ![アプリケーションの追加](./media/active-directory-saas-screensteps-tutorial/IC749321.png "Add application")

5.   **実行する操作** ダイアログ ボックスで、をクリックして **ギャラリーからアプリケーションを追加**します。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-screensteps-tutorial/IC749322.png "Add an application from gallerry")

6.   **検索ボックス**, 、型 **ScreenSteps**します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-screensteps-tutorial/IC778517.png "Application gallery")

7.  結果ウィンドウで [ **ScreenSteps**, 、クリックして **完了** アプリケーションを追加します。

    ![ScreenSteps](./media/active-directory-saas-screensteps-tutorial/IC778518.png "ScreenSteps")
##シングル サインオンの構成
  
このセクションでは、SAML プロトコルに基づくフェデレーションを使用して、ScreenSteps で Azure AD のユーザー アカウントを使用してユーザーを認証できるようにする方法を説明します。

###シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **ScreenSteps** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、* * [シングル サインオン * * ダイアログ。

    ![シングル サインオンの構成](./media/active-directory-saas-screensteps-tutorial/IC778519.png "Configure single sign-on")

2.   **どのようなユーザーを ScreenSteps へのサインオン** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、] をクリックし、 **次**します。

    ![シングル サインオンの構成](./media/active-directory-saas-screensteps-tutorial/IC778520.png "Configure single sign-on")

3.   **アプリケーション URL の構成** ] ページで、 **ScreenSteps サインイン URL** ] ボックスに、次のパターンを使用して URL を入力"*https://\<tenant-name\>。ScreenSteps.com*"、クリックして **次**します。

    ![アプリケーション URL の構成](./media/active-directory-saas-screensteps-tutorial/IC778521.png "Configure app URL")

4.   **ScreenSteps でのシングル サインオンの構成** ] ページで、証明書をダウンロードする] をクリックして **証明書のダウンロード**, 、お使いのコンピューターに証明書ファイルを保存します。

    ![シングル サインオンの構成](./media/active-directory-saas-screensteps-tutorial/IC778522.png "Configure single sign-on")

5.  別の Web ブラウザー ウィンドウで、ScreenSteps 企業サイトに管理者としてログインします。

6.  クリックして **アカウント管理**します。

    ![アカウント管理](./media/active-directory-saas-screensteps-tutorial/IC778523.png "Account management")

7.  クリックして **リモート認証**します。

    ![リモート認証](./media/active-directory-saas-screensteps-tutorial/IC778524.png "Remote authentication")

8.  クリックして **認証エンドポイントを作成する**です。

    ![リモート認証](./media/active-directory-saas-screensteps-tutorial/IC778525.png "Remote authentication")

9.   **認証エンドポイントを作成する** セクションで、次の手順に従います。

    ![認証エンドポイントの作成](./media/active-directory-saas-screensteps-tutorial/IC778526.png "Create an authentication endpoint")

    1.   **タイトル** ] ボックスのタイトルを入力します。
    2.   **モード** 一覧で、[ **SAML**します。
    3.  クリックして **作成**します。

10.  **Remote Authentication Endpoint** セクションで、次の手順に従います。

    ![リモート認証エンドポイント](./media/active-directory-saas-screensteps-tutorial/IC778527.png "Remote authentication endpoint")

    1.  Azure AD ポータルでの **ScreenSteps でのシングル サインオンの構成** ] ページで、コピー、 **リモート ログイン URL** 値に設定して、貼り付けます、 **リモート ログイン URL** ] ボックスに貼り付けます。
    2.  Azure AD ポータルでの **ScreenSteps でのシングル サインオンの構成** ] ページで、コピー、 **リモート ログアウト URL** 値に設定して、貼り付けます、 **ログアウト URL** ] ボックスに貼り付けます。
    3.  クリックして **ファイルを選択して**, 、し、ダウンロードした証明書をアップロードします。
    4.  クリックして **更新**します。

11. Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![Configure single sign-on](./media/active-directory-saas-screensteps-tutorial/IC778542.png "Configure single sign-on")
##ユーザー プロビジョニングの構成
  
Azure AD ユーザーにログインを有効にするために **ScreenSteps**, にプロビジョニングする必要がありますが **ScreenSteps**します。  
場合に **ScreenSteps**, 、プロビジョニングは手動作業です。

###ユーザー アカウントを ScreenSteps にプロビジョニングするには、次の手順に従います。

1.  ログインして **ScreenSteps** テナントです。

2.  クリックして **アカウント管理**します。

    ![アカウント管理](./media/active-directory-saas-screensteps-tutorial/IC778523.png "Account management")

3.  クリックして **ユーザー**します。

    ![ユーザー](./media/active-directory-saas-screensteps-tutorial/IC778544.png "Users")

4.  クリックして **ユーザーを作成**します。

    ![すべてのユーザー](./media/active-directory-saas-screensteps-tutorial/IC778545.png "All Users")

5.   **ユーザー ロール** 一覧で、ユーザーのロールを選択します。

6.  ユーザー ロール] セクションで、入力、"**名**, 、**姓、名**, 、**電子メール**, 、**ログイン**, 、**パスワード** と **パスワードの確認入力**"関連テキスト ボックスに、プロビジョニングする有効な AAD アカウントのです。

    ![新しいユーザー](./media/active-directory-saas-screensteps-tutorial/IC778546.png "New user")

7.  Groups] セクションで、選択 **Authentication Group (SAML)**, 、クリックして **Create User**します。

    ![グループ](./media/active-directory-saas-screensteps-tutorial/IC778547.png "Groups")

>[AZURE.NOTE]他の ScreenSteps ユーザー アカウント作成ツールを使用することができますか、AAD ユーザー アカウントをプロビジョニング、ScreenSteps から提供されている Api です。

##ユーザーの割り当て
  
構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

###ユーザーを ScreenSteps に割り当てるには、次の手順に従います。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  * * ScreenSteps * * アプリケーション統合ページで、をクリックして **ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-screensteps-tutorial/IC773094.png "Assign users")

3.  テスト ユーザーを選択して [ **割り当てる**, 、順にクリック **はい** して割り当てを確認します。

    ![ユーザーの割り当て](./media/active-directory-saas-screensteps-tutorial/IC778548.png "Assign users")
  
シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。
