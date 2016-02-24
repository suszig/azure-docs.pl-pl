<properties 
    pageTitle="チュートリアル: Azure Active Directory と Rally Software の統合 | Microsoft Azure" 
    description="Azure Active Directory で Rally Software を使用して、シングル サインオンや自動プロビジョニングなどを有効にする方法について説明します。" 
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

#チュートリアル: Azure Active Directory と Rally Software の統合
  
このチュートリアルでは、Azure と Rally Software の統合について説明します。  
このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   Rally Software テナント
  
このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  Rally Software のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-rally-software-tutorial/IC769525.png "Scenario")
##Rally Software のアプリケーション統合の有効化
  
このセクションでは、Rally Software のアプリケーション統合を有効にする方法を説明します。

###Rally Software のアプリケーション統合を有効にするには、次の手順に従います。

1.  左側のナビゲーション ウィンドウで、Azure 管理ポータルで、クリックして **Active Directory**します。

    ![Active Directory](./media/active-directory-saas-rally-software-tutorial/IC700993.png "Active Directory")

2.   **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3.  ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。

    ![アプリケーション](./media/active-directory-saas-rally-software-tutorial/IC700994.png "Applications")

4.  クリックして **追加** ページの下部にあります。

    ![アプリケーションの追加](./media/active-directory-saas-rally-software-tutorial/IC749321.png "Add application")

5.   **実行する操作** ダイアログ ボックスで、をクリックして **ギャラリーからアプリケーションを追加**します。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-rally-software-tutorial/IC749322.png "Add an application from gallerry")

6.   **検索ボックス**, 、型 **rally**します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-rally-software-tutorial/IC769526.png "Application gallery")

7.  結果ウィンドウで [ **Rally Software**, 、クリックして **完了** アプリケーションを追加します。

    ![Rally Software](./media/active-directory-saas-rally-software-tutorial/IC769527.png "Rally Software")
##シングル サインオンの構成
  
このセクションでは、ユーザーが SAML プロトコルに基づくフェデレーションを使用して、Azure AD でのユーザーのアカウントで Rally Software に対する認証を行うことができるようにする方法を説明します。  
この手順の途中で、証明書を Rally Software にアップロードする必要があります。

###シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの * * Rally Software * * アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、* * [シングル サインオン * * ダイアログ。

    ![シングル サインオンの構成](./media/active-directory-saas-rally-software-tutorial/IC749323.png "Configure single sign-on")

2.   **どのようなユーザーの Rally へ** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、] をクリックし、 **次**します。

    ![Microsoft Azure AD シングル サインオン](./media/active-directory-saas-rally-software-tutorial/IC769528.png "Microsoft Azure AD Single Sign-On")

3.   **アプリケーション URL の構成** ] ページの [、 **Rally Software テナント URL** ] ボックスに、次のパターンを使用して URL を入力"*https://\<tenant-name\>.rally.com*"、] をクリックし、 **次**します。

    ![アプリケーション URL の構成](./media/active-directory-saas-rally-software-tutorial/IC769529.png "Configure App URL")

4.   **Rally でのシングル サインオンの構成** ] ページで [メタデータのダウンロード] をクリックして、コンピューターに保存します。

    ![シングル サインオンの構成](./media/active-directory-saas-rally-software-tutorial/IC769530.png "Configure single sign-on")

5.  ログインして **Rally Software** テナントです。

6.  上部にあるツールバーで、クリックして **セットアップ**, 、し、[ **サブスクリプション**します。

    ![[サブスクリプション]](./media/active-directory-saas-rally-software-tutorial/IC769531.png "Subscription")

7.  クリックして、 **アクション** 上にある右側にあるツール バー ボタンをクリックし、[ **サブスクリプションの編集**します。

8.   **サブスクリプション** ] ダイアログ ページで、次の手順を実行し、順にクリックして **保存して閉じる**:

    ![認証](./media/active-directory-saas-rally-software-tutorial/IC769542.png "Authentication")

    1.  選択 **Rally または SSO 認証** 認証] ドロップダウンから
    2.  Azure ポータルで、 **Rally Software でのシングル サインオンの構成** ] ダイアログ ページでコピー、 **Id プロバイダーの ID** 値に設定して、貼り付けます、 **Identity Provider URL** ] テキスト ボックス
    3.  Azure ポータルで、 **Rally Software でのシングル サインオンの構成** ] ダイアログ ページで、コピー、 **リモート ログアウト URL** 値。

9.  Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![Configure single sign-on](./media/active-directory-saas-rally-software-tutorial/IC769547.png "Configure single sign-on")
##ユーザー プロビジョニングの構成
  
AAD ユーザーがサインインできるように、Azure Active Directory ユーザー名を使用して、Rally Software アプリケーションにユーザーをプロビジョニングする必要があります。

###ユーザー プロビジョニングを構成するには、次の手順に従います。

1.  Rally Software テナントにログインします。

2.  移動して **セットアップ \ > ユーザー**, 、クリックして **+ Add New**します。

    ![ユーザー](./media/active-directory-saas-rally-software-tutorial/IC781039.png "Users")

3.  新しいユーザー] ボックスで、名前を入力し、クリックして **Add with Details**します。

4.   **Create User** セクションで、次の手順に従います。

    ![Create User](./media/active-directory-saas-rally-software-tutorial/IC781040.png "Create User")

    1.   **ユーザー名** ] ボックスに、プロビジョニングする Azure AD ユーザーの名前を入力します。
    2.   **電子メール アドレス** ] ボックスに、プロビジョニングする Azure AD ユーザーの電子メール アドレスを入力します。
    3.  クリックして **保存して閉じる**します。

>[AZURE.NOTE]他の Rally Software ユーザー アカウント作成ツールを使用することができますか、AAD ユーザー アカウントをプロビジョニング、Rally Software 提供の Api です。

##ユーザーの割り当て
  
構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

###ユーザーを Rally Software に割り当てるには、次の手順に従います。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.   **Rally Software** アプリケーション統合ページで、をクリックして **ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-rally-software-tutorial/IC769548.png "Assign users")

3.  テスト ユーザーを選択して [ **割り当てる**, 、順にクリック **はい** して割り当てを確認します。

    ![あり](./media/active-directory-saas-rally-software-tutorial/IC767830.png "Yes")
  
シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。





