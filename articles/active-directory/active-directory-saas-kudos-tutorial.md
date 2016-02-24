<properties 
    pageTitle="チュートリアル: Azure Active Directory と Kudos の統合 | Microsoft Azure" 
    description="Azure Active Directory で Kudos を使用して、シングル サインオンや自動プロビジョニングなどを有効にする方法について説明します。" 
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

#チュートリアル: Azure Active Directory と Kudos の統合
  
このチュートリアルの目的は、Azure と Kudos の統合を紹介することです。  
このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   Kudos テナント
  
このチュートリアルを完了すると、Kudos に割り当てた Azure AD ユーザーは、Kudos 会社サイト (サービス プロバイダーが開始したサインオン) で、アプリケーションにシングル サインオンまたはできるを使用して、 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。
  
このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  Kudos のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-kudos-tutorial/IC787799.png "Scenario")
##Kudos のアプリケーション統合の有効化
  
このセクションでは、Kudos のアプリケーション統合を有効にする方法について説明します。

###Kudos のアプリケーション統合を有効にするには、次の手順に従います。

1.  左側のナビゲーション ウィンドウで、Azure 管理ポータルで、クリックして **Active Directory**します。

    ![Active Directory](./media/active-directory-saas-kudos-tutorial/IC700993.png "Active Directory")

2.   **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3.  ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。

    ![アプリケーション](./media/active-directory-saas-kudos-tutorial/IC700994.png "Applications")

4.  クリックして **追加** ページの下部にあります。

    ![アプリケーションの追加](./media/active-directory-saas-kudos-tutorial/IC749321.png "Add application")

5.   **実行する操作** ダイアログ ボックスで、をクリックして **ギャラリーからアプリケーションを追加**します。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-kudos-tutorial/IC749322.png "Add an application from gallerry")

6.   **検索ボックス**, 、型 **Kudos**します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-kudos-tutorial/IC787800.png "Application Gallery")

7.  結果ウィンドウで [ **Kudos**, 、クリックして **完了** アプリケーションを追加します。

    ![Kudos](./media/active-directory-saas-kudos-tutorial/IC787801.png "Kudos")
##シングル サインオンの構成
  
このセクションでは、ユーザーが SAML プロトコルに基づくフェデレーションを使用して、Azure AD でのユーザーのアカウントで Kudos に対する認証を行えるようにする方法を説明します。  
この手順の途中で、base-64 でエンコードされた証明書ファイルを作成する必要があります。  
この手順に慣れていない場合は、次を参照してください。 [バイナリ証明書をテキスト ファイルに変換する方法](http://youtu.be/PlgrzUZ-Y1o)します。

###シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **Kudos** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、* * [シングル サインオン * * ダイアログ。

    ![Configure single sign-on](./media/active-directory-saas-kudos-tutorial/IC787802.png "Configure single sign-on")

2.   **どのようなユーザーの Kudos に** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、] をクリックし、 **次**します。

    ![Configure single sign-on](./media/active-directory-saas-kudos-tutorial/IC787803.png "Configure single sign-on")

3.   **アプリケーション URL の構成** ] ページの [、 **Kudos サインオン URL** ] ボックスに、次のパターンを使用して URL を入力"*https://company.kudosnow.com*"、] をクリックし、 **次**します。

    ![アプリケーション URL の構成](./media/active-directory-saas-kudos-tutorial/IC787804.png "Configure App URL")

4.   **Kudos でのシングル サインオンの構成** ] ページで [ **証明書のダウンロード**, 、お使いのコンピューターに証明書ファイルを保存します。

    ![Configure single sign-on](./media/active-directory-saas-kudos-tutorial/IC787805.png "Configure single sign-on")

5.  別の Web ブラウザーのウィンドウで、Kudos の企業サイトに管理者としてログインします。

6.  上部にあるメニュー [ **設定**します。

    ![設定](./media/active-directory-saas-kudos-tutorial/IC787806.png "Settings")

7.  クリックして **インテグレーション \ > SSO**します。

8.   **SSO** セクションで、次の手順に従います。

    ![SSO](./media/active-directory-saas-kudos-tutorial/IC787807.png "SSO")

    1.  Azure ポータルで、 **Kudos でのシングル サインオンの構成** ] ダイアログ ページでコピー、 **シングル サインオン サービス URL** 値に設定して、貼り付けます、* * サインオン URL * *] ボックスに貼り付けます。
    2.  作成、 **base-64 でエンコードされた** からダウンロードした証明書ファイル。  

        >[AZURE.TIP]
        詳細については、次を参照してください [バイナリ証明書をテキスト ファイルに変換する方法。](http://youtu.be/PlgrzUZ-Y1o)

    3.  Base-64 でエンコードされた証明書をメモ帳で開き、という内容がクリップボードにコピーおよびに貼り付け、 **X.509 証明書** ] テキスト ボックス
    4.  Azure ポータルで、 **Kudos でのシングル サインオンの構成** ] ダイアログ ページでコピー、 **シングル サインアウト サービス URL** 値に設定して、貼り付けます、* * Logout To URL * *] ボックスに貼り付けます。
    5.   **Your Kudos URL** ] ボックスに、会社名を入力します。
    6.  クリックして **保存**します。

9.  Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![Configure single sign-on](./media/active-directory-saas-kudos-tutorial/IC787808.png "Configure single sign-on")
##ユーザー プロビジョニングの構成
  
Azure AD ユーザーが Kudos にログインできるようにするには、そのユーザーを Kudos にプロビジョニングする必要があります。  
Kudos の場合、プロビジョニングは手動で行います。

###ユーザー アカウントをプロビジョニングするには、次の手順を実行します。

1.  ログインして **Kudos** 会社サイトに管理者として。

2.  上部にあるメニュー [ **設定**します。

    ![設定](./media/active-directory-saas-kudos-tutorial/IC787806.png "Settings")

3.  クリックして **ユーザー Admin**します。

4.  クリックして、 **ユーザー** タブをクリックし、をクリックし、 **ユーザーを追加**します。

    ![ユーザー管理者](./media/active-directory-saas-kudos-tutorial/IC787809.png "User Admin")

5.   **ユーザーを追加** セクションで、次の手順に従います。

    ![ユーザーの追加](./media/active-directory-saas-kudos-tutorial/IC787810.png "Add a User")

    1.  型、 **名**, 、**姓**, 、**電子メール** など、関連するテキスト ボックスに、プロビジョニングする有効な Azure Active Directory アカウントの情報です。
    2.  クリックして **ユーザー作成**します。

>[AZURE.NOTE]他の Kudos ユーザー アカウント作成ツールを使用することができますか、AAD ユーザー アカウントをプロビジョニング、Kudos から提供されている Api です。

##ユーザーの割り当て
  
構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

###ユーザーを Kudos に割り当てるには、次の手順に従います。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  * * Kudos * * アプリケーション統合ページで、をクリックして **ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-kudos-tutorial/IC787811.png "Assign users")

3.  テスト ユーザーを選択して [ **割り当てる**, 、順にクリック **はい** して割り当てを確認します。

    ![あり](./media/active-directory-saas-kudos-tutorial/IC767830.png "Yes")
  
シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。
