<properties 
    pageTitle="チュートリアル: Azure Active Directory と RunMyProcess の統合 | Microsoft Azure" 
    description="Azure Active Directory で RunMyProcess を使用して、シングル サインオンや自動プロビジョニングなどを有効にする方法について説明します。" 
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

#チュートリアル: Azure Active Directory と RunMyProcess の統合
  
このチュートリアルでは、Azure と RunMyProcess の統合について説明します。  
このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   RunMyProcess テナント
  
このチュートリアルを完了すると、RunMyProcess に割り当てた Azure AD ユーザーは、RunMyProcess 会社サイト (サービス プロバイダーが開始したサインオン) で、アプリケーションにシングル サインオンまたはできるを使用して、 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。
  
このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  RunMyProcess のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-runmyprocess-tutorial/IC789614.png "Scenario")
##RunMyProcess のアプリケーション統合の有効化
  
このセクションでは、RunMyProcess のアプリケーション統合を有効にする方法について説明します。

###RunMyProcess のアプリケーション統合を有効にするには、次の手順に従います。

1.  左側のナビゲーション ウィンドウで、Azure 管理ポータルで、クリックして **Active Directory**します。

    ![Active Directory](./media/active-directory-saas-runmyprocess-tutorial/IC700993.png "Active Directory")

2.   **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3.  ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。

    ![アプリケーション](./media/active-directory-saas-runmyprocess-tutorial/IC700994.png "Applications")

4.  クリックして **追加** ページの下部にあります。

    ![アプリケーションの追加](./media/active-directory-saas-runmyprocess-tutorial/IC749321.png "Add application")

5.   **実行する操作** ダイアログ ボックスで、をクリックして **ギャラリーからアプリケーションを追加**します。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-runmyprocess-tutorial/IC749322.png "Add an application from gallerry")

6.   **検索ボックス**, 、型 **RunMyProcess**します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-runmyprocess-tutorial/IC789615.png "Application Gallery")

7.  結果ウィンドウで [ **RunMyProcess**, 、クリックして **完了** アプリケーションを追加します。

    ![RunMyProcess](./media/active-directory-saas-runmyprocess-tutorial/IC789616.png "RunMyProcess")
##シングル サインオンの構成
  
このセクションでは、ユーザーが SAML プロトコルに基づくフェデレーションを使用して、Azure AD でのユーザーのアカウントで RunMyProcess に対する認証を行えるようにする方法について説明します。  
この手順の途中で、base-64 でエンコードされた証明書ファイルを作成する必要があります。  
この手順に慣れていない場合は、次を参照してください。 [バイナリ証明書をテキスト ファイルに変換する方法](http://youtu.be/PlgrzUZ-Y1o)します。

###シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **RunMyProcess** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、* * [シングル サインオン * * ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-runmyprocess-tutorial/IC789617.png "Configure Single Sign-On")

2.   **どのようなユーザーが RunMyProcess へのサインオン** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、順にクリック **次**します。

    ![Configure Single Sign-On](./media/active-directory-saas-runmyprocess-tutorial/IC789622.png "Configure Single Sign-On")

3.   **アプリケーション URL の構成** ] ページの [、 **RunMyProcess サインオン URL** ] ボックスに、次のパターンを使用して URL を入力"*http://company.runmyprocess.com*"、] をクリックし、 **次**します。

    ![アプリケーション URL の構成](./media/active-directory-saas-runmyprocess-tutorial/IC789623.png "Configure App URL")

4.   **RunMyProcess でのシングル サインオンの構成** ] ページで [ **証明書のダウンロード**, 、お使いのコンピューターに証明書ファイルを保存します。

    ![Configure Single Sign-On](./media/active-directory-saas-runmyprocess-tutorial/IC789624.png "Configure Single Sign-On")

5.  別の Web ブラウザーのウィンドウで、RunMyProcess 企業サイトに管理者としてログインします。

6.  移動して **アカウント \ > 構成**します。

    ![アカウント](./media/active-directory-saas-runmyprocess-tutorial/IC789625.png "Account")

7.  クリックして、 **認証方法** ] タブをクリックします。

8.   **認証方法** セクションで、次の手順に従います。

    ![SSO](./media/active-directory-saas-runmyprocess-tutorial/IC789626.png "SSO")

    1.  として **メソッド**, [ **SSO with Samlv2**します。
    2.  Azure ポータルで、 **RunMyProcess でのシングル サインオンの構成** ] ダイアログ ページでコピー、 **SAML SSO URL** 値に設定して、貼り付けます、 **SSO リダイレクト** ] ボックスに貼り付けます。
    3.  Azure ポータルで、 **RunMyProcess でのシングル サインオンの構成** ] ダイアログ ページでコピー、 **シングル サインアウト サービス URL** 値に設定して、貼り付けます、 **ログアウト リダイレクト** ] ボックスに貼り付けます。
    4.   **名前 id 形式** ] ボックスに「 **urn oasis: 名: 対象の形式: emailAddress**します。
    5.  作成、 **base-64 でエンコードされた** からダウンロードした証明書ファイル。  

        >[AZURE.TIP] 詳細については、次を参照してください [バイナリ証明書をテキスト ファイルに変換する方法。](http://youtu.be/PlgrzUZ-Y1o)

    6.  Base-64 でエンコードされた証明書をメモ帳で開き、という内容がクリップボードにコピーおよびに貼り付け、 **証明書** ] テキスト ボックス
    7.  クリックして **保存**します。

9.  Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-runmyprocess-tutorial/IC789627.png "Configure Single Sign-On")
##ユーザー プロビジョニングの構成
  
Azure AD ユーザーが RunMyProcess にログインできるようにするには、ユーザーを RunMyProcess にプロビジョニングする必要があります。  
RunMyProcess の場合、プロビジョニングは手動で行います。

###ユーザー アカウントをプロビジョニングするには、次の手順を実行します。

1.  ログインして **RunMyProcess** 会社サイトに管理者として。

2.  移動して **アカウント \ > ユーザー**, 、クリックして **新しいユーザー**します。

    ![新しいユーザー](./media/active-directory-saas-runmyprocess-tutorial/IC789631.png "New User")

3.   **ユーザー設定** セクションで、次の手順に従います。

    ![プロファイル](./media/active-directory-saas-runmyprocess-tutorial/IC789632.png "Profile")

    1.  型、 **名前** と **電子メール** 関連テキスト ボックスに、プロビジョニングする有効な AAD アカウントのです。
    2.  選択、 **IDE 言語**, 、 **言語** と **プロファイル**します。
    3.  選択 **自分のアカウント作成の電子メールの送信**します。
    4.  クリックして **保存**します。

>[AZURE.NOTE]他の RunMyProcess ユーザー アカウント作成ツールを使用する、または Api によって提供される RunMyProcess にプロビジョニング Azure Active Directory ユーザー アカウントです。

##ユーザーの割り当て
  
構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

###ユーザーを RunMyProcess に割り当てるには、次の手順に従います。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  * * RunMyProcess * * アプリケーション統合ページで、をクリックして **ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-runmyprocess-tutorial/IC789633.png "Assign Users")

3.  テスト ユーザーを選択して [ **割り当てる**, 、順にクリック **はい** して割り当てを確認します。

    ![あり](./media/active-directory-saas-runmyprocess-tutorial/IC767830.png "Yes")
  
シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。
