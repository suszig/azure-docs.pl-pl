<properties 
    pageTitle="チュートリアル: Azure Active Directory と Syncplicity の統合 | Microsoft Azure" 
    description="Azure Active Directory で Syncplicity を使用して、シングル サインオンや自動プロビジョニングなどを有効にする方法について説明します。" 
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

#チュートリアル: Azure Active Directory と Syncplicity の統合
  
このチュートリアルでは、Azure Active Directory (Azure AD) と Syncplicity の間でのシングル サインオンを設定する方法を説明します。
  
このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   Syncplicity テナント
  
このチュートリアルを完了すると、Syncplicity に割り当てた Azure AD ユーザーは、Syncplicity 企業サイト (サービス プロバイダーが開始したサインオン) で、または Azure AD アクセス パネルを使用して、アプリケーションにシングル サインオンできるようになります。

1.  Syncplicity のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-syncplicity-tutorial/IC769524.png "Scenario")

##Syncplicity のアプリケーション統合の有効化
  
このセクションでは、Syncplicity のアプリケーション統合を有効にする方法を説明します。

###Syncplicity のアプリ統合を有効にするには、次の手順に従います。

1.  左側のナビゲーション ウィンドウで、Azure 管理ポータルで、クリックして **Active Directory**します。

    ![Active Directory](./media/active-directory-saas-syncplicity-tutorial/IC700993.png "Active Directory")

2.   **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3.  ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。

    ![アプリケーション](./media/active-directory-saas-syncplicity-tutorial/IC700994.png "Applications")

4.  クリックして **追加** ページの下部にあります。

    ![アプリケーションの追加](./media/active-directory-saas-syncplicity-tutorial/IC749321.png "Add application")

5.   **実行する操作** ダイアログ ボックスで、をクリックして **ギャラリーからアプリケーションを追加**します。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-syncplicity-tutorial/IC749322.png "Add an application from gallerry")

6.   **検索ボックス**, 、型 **Syncplicity**します。

    ![Syncplicity アプリケーション ギャラリー](./media/active-directory-saas-syncplicity-tutorial/IC769532.png "Syncplicity application gallery")

7.  結果ウィンドウで [ **Syncplicity**, 、クリックして **完了** アプリケーションを追加します。

    ![Syncplicity](./media/active-directory-saas-syncplicity-tutorial/IC769533.png "Syncplicity")

##シングル サインオンの構成
  
このセクションでは、SAML プロトコルに基づくフェデレーションを使用して、Syncplicity で Azure AD のユーザー アカウントを使用してユーザーを認証できるようにする方法を説明します。

###シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **Syncplicity** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、* * [シングル サインオン * * ダイアログ。

    ![シングル サインオンの構成](./media/active-directory-saas-syncplicity-tutorial/IC769534.png "Configure single sign-on")

2.   **どのようなユーザー Syncplicity にサインオンする** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、] をクリックし、 **次**します。

    ![Microsoft Azure AD シングル サインオン](./media/active-directory-saas-syncplicity-tutorial/IC769535.png "Microsoft Azure AD Single Sign-On")

3.   **アプリケーション URL の構成** ] ページで、 **Syncplicity サインイン URL** ] ボックスに、URL のユーザーが Syncplicity アプリケーションへのサインインを使用して型をクリックして **次**します。 

    アプリケーション URL は、Syncplicity テナント URL (例:: *http://company.Syncplicity.com*)。

    ![アプリケーション URL の構成](./media/active-directory-saas-syncplicity-tutorial/IC769536.png "Configure app URL")

4.   **Syncplicity でのシングル サインオンの構成** ] ページで、証明書をダウンロードする] をクリックして **証明書のダウンロード**, 、し、コンピューターに証明書ファイルをローカルに保存します。

    ![シングル サインオンの構成](./media/active-directory-saas-syncplicity-tutorial/IC769543.png "Configure single sign-on")

5.  サインイン、 **Syncplicity** テナントです。

6.  上部にあるメニュー、[ **admin**, を選択 **設定**, 、] をクリックし、 **カスタム ドメインの管理とシングル サインオン**します。

    ![Syncplicity](./media/active-directory-saas-syncplicity-tutorial/IC769545.png "Syncplicity")

7.   **シングル サインオン (SSO)** ] ダイアログ ページで、次の手順を実行します。

    ![シングル サインオン \(SSO\)](./media/active-directory-saas-syncplicity-tutorial/IC769550.png "Single Sign-On \(SSO\)")

    1.   **カスタム ドメインの** ] ボックスに、自分のドメインの名前を入力します。
    2.  選択 **有効になっている** として **シングル サインオン ステータス**します。
    3.  Microsoft Azure ポータルで、 **Syncplicity でのシングル サインオンの構成** ] ページで、コピー、 **エンティティ ID** 値に設定して、貼り付けます、 **エンティティ Id** ] ボックスに貼り付けます。
    4.  Microsoft Azure ポータルで、 **Syncplicity でのシングル サインオンの構成** ] ページで、コピー、 **シングル サインオン サービス URL** 値に設定して、貼り付けます、 **サインイン ページ URL** ] ボックスに貼り付けます。
    5.  Microsoft Azure ポータルで、 **Syncplicity でのシングル サインオンの構成** ] ページで、コピー、 **リモート ログアウト URL** 値に設定して、貼り付けます、 **ログアウト ページ URL** ] ボックスに貼り付けます。
    6.   **Identity Provider Certificate**, 、] をクリックして **ファイルの選択**, 、および Microsoft Azure ポータルからダウンロードした証明書をアップロードします。
    7.  クリックして **変更を保存**します。

8.  Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![確認](./media/active-directory-saas-syncplicity-tutorial/IC769554.png "Confirmation")

##ユーザー プロビジョニングの構成
  
AAD ユーザーがサインインできるようにするには、ユーザーを Syncplicity アプリケーションにプロビジョニングする必要があります。 このセクションでは、Syncplicity で AAD ユーザー アカウントを作成する方法について説明します。

###ユーザー アカウントを Syncplicity にプロビジョニングするには、次の手順に従います。

1.  ログインして **Syncplicity** テナント (例:: *https://company.Syncplicity.com*)。

2.  をクリックして **管理者** 選択 **ユーザー アカウント**します。

3.  クリックして **ユーザーを追加**します。

    ![Manage Users](./media/active-directory-saas-syncplicity-tutorial/IC769764.png "Manage Users")

4.  型、 **電子メール アドレス** 、プロビジョニングする AAD アカウントの選択 **ユーザー** として **ロール**, 、] をクリックし、 **次**します。

    ![アカウント情報](./media/active-directory-saas-syncplicity-tutorial/IC769765.png "Account Information")

    >[AZURE.NOTE] AAD アカウントの所有者を確認し、アカウントをアクティブ化リンクが記載された電子メールが表示されます。

5.  新しいユーザーのメンバーになる必要がありますし、[会社のグループを選択 **次**します。

    ![グループ メンバーシップ](./media/active-directory-saas-syncplicity-tutorial/IC769772.png "Group Membership")

    >[AZURE.NOTE] だけで、表示されている、グループがない場合はクリックして **次**します。

6.  クリックして、ユーザーのコンピューターで Syncplicity の制御下に配置するフォルダーを選択して **次**します。

    ![Syncplicity フォルダー](./media/active-directory-saas-syncplicity-tutorial/IC769773.png "Syncplicity Folders")

>[AZURE.NOTE] 他の Syncplicity ユーザー アカウント作成ツールを使用するまたは AAD ユーザー アカウントをプロビジョニング、Syncplicity 提供の Api です。

##ユーザーの割り当て
  
構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

###ユーザーを Syncplicity に割り当てるには、次の手順に従います。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.   **Syncplicity** アプリケーション統合ページで、をクリックして **ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-syncplicity-tutorial/IC769557.png "Assign users")

3.  テスト ユーザーを選択して [ **割り当てる**, 、順にクリック **はい** して割り当てを確認します。

    ![あり](./media/active-directory-saas-syncplicity-tutorial/IC767830.png "Yes")
  
シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。



