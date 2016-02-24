<properties 
    pageTitle="チュートリアル: Azure Active Directory と Mimecast Admin Console の統合 | Microsoft Azure" 
    description="Azure Active Directory で Mimecast Admin Console を使用して、シングル サインオンや自動プロビジョニングなどを有効にする方法について説明します。" 
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

#チュートリアル: Azure Active Directory と Mimecast Admin Console の統合
  
このチュートリアルでは、Azure と Mimecast Admin Console の統合について説明します。  
このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   Mimecast Admin Console でのシングル サインオンが有効なサブスクリプション
  
このチュートリアルを完了すると、Mimecast Admin Console に割り当てた Azure AD ユーザーは、Mimecast Admin Console 会社サイト (サービス プロバイダーが開始したサインオン) で、アプリケーションにシングル サインオンまたはできるを使用して、 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。
  
このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  Mimecast Admin Console のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-mimecast-admin-console-tutorial/IC795008.png "Scenario")
##Mimecast Admin Console のアプリケーション統合の有効化
  
このセクションでは、Mimecast Admin Console のアプリケーション統合を有効にする方法を説明します。

###Mimecast Admin Console のアプリケーション統合を有効にするには、次の手順に従います。

1.  左側のナビゲーション ウィンドウで、Azure 管理ポータルで、クリックして **Active Directory**します。

    ![Active Directory](./media/active-directory-saas-mimecast-admin-console-tutorial/IC700993.png "Active Directory")

2.   **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3.  ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。

    ![アプリケーション](./media/active-directory-saas-mimecast-admin-console-tutorial/IC700994.png "Applications")

4.  クリックして **追加** ページの下部にあります。

    ![アプリケーションの追加](./media/active-directory-saas-mimecast-admin-console-tutorial/IC749321.png "Add application")

5.   **実行する操作** ダイアログ ボックスで、をクリックして **ギャラリーからアプリケーションを追加**します。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-mimecast-admin-console-tutorial/IC749322.png "Add an application from gallerry")

6.   **検索ボックス**, 、型 **Mimecast Admin Console**します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-mimecast-admin-console-tutorial/IC795009.png "Application Gallery")

7.  結果ウィンドウで [ **Mimecast Admin Console**, 、クリックして **完了** アプリケーションを追加します。

    ![Mimecast Admin Console](./media/active-directory-saas-mimecast-admin-console-tutorial/IC795010.png "Mimecast Admin Console")
##シングル サインオンの構成
  
このセクションでは、ユーザーが SAML プロトコルに基づくフェデレーションを使用して、Azure AD でのユーザーのアカウントで Mimecast Admin Console に対する認証を行うことができるようにする方法を説明します。  
この手順の途中で、base-64 でエンコードされた証明書ファイルを作成する必要があります。  
この手順に慣れていない場合は、次を参照してください。 [バイナリ証明書をテキスト ファイルに変換する方法](http://youtu.be/PlgrzUZ-Y1o)します。

###シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **Mimecast Admin Console** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、* * [シングル サインオン * * ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-mimecast-admin-console-tutorial/IC795011.png "Configure Single Sign-On")

2.   **どのようなユーザーが Mimecast Admin Console へのサインオン** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、] をクリックし、 **次**します。

    ![Configure Single Sign-On](./media/active-directory-saas-mimecast-admin-console-tutorial/IC795012.png "Configure Single Sign-On")

3.   **アプリケーション URL の構成** ] ページの [、 **Mimecast Admin Console サインオン URL** URL は、ユーザーが Mimecast Admin Console アプリケーションにサインオンする使用] ボックスに、型 (例::"https://webmail-uk.mimecast.com"や"https://webmail-us.mimecast.com")、順にクリック **次**します。

    >[AZURE.NOTE] サインオン URL は、リージョンに固有です。

    ![アプリケーション URL の構成](./media/active-directory-saas-mimecast-admin-console-tutorial/IC795013.png "Configure App URL")

4.   **Mimecast Admin Console でのシングル サインオンの構成** ] ページで、証明書をダウンロードする] をクリックして **証明書のダウンロード**, 、お使いのコンピューターにローカルで証明書ファイルを保存します。

    ![Configure Single Sign-On](./media/active-directory-saas-mimecast-admin-console-tutorial/IC795014.png "Configure Single Sign-On")

5.  別の Web ブラウザー ウィンドウで、Mimecast Admin Console の企業サイトに管理者としてログインします。

6.  移動して **サービス \ > アプリケーション**します。

    ![サービス](./media/active-directory-saas-mimecast-admin-console-tutorial/IC794998.png "Services")

7.  クリックして **認証プロファイル**します。

    ![認証プロファイル](./media/active-directory-saas-mimecast-admin-console-tutorial/IC794999.png "Authentication Profiles")

8.  クリックして **新しい認証プロファイル**します。

    ![新しい認証プロファイル](./media/active-directory-saas-mimecast-admin-console-tutorial/IC795000.png "New Authentication Profiles")

9.   **認証プロファイル** セクションで、次の手順に従います。

    ![認証プロファイル](./media/active-directory-saas-mimecast-admin-console-tutorial/IC795015.png "Authentication Profile")

    1.   **説明** ] ボックスに、構成の名前を入力します。
    2.  選択 **Mimecast Admin Console に SAML 認証を強制**します。
    3.  として **プロバイダー**, [ **Azure Active Directory**します。
    4.  Azure ポータルで、 **Mimecast Admin Console でのシングル サインオンの構成** ] ダイアログ ページでコピー、 **発行者 URL** 値に設定して、貼り付けます、 **発行者 URL** ] ボックスに貼り付けます。
    5.  Azure ポータルで、 **Mimecast Admin Console でのシングル サインオンの構成** ] ダイアログ ページでコピー、 **リモート ログイン URL** 値に設定して、貼り付けます、 **ログイン URL** ] ボックスに貼り付けます。
    6.  Azure ポータルで、 **Mimecast Admin Console でのシングル サインオンの構成** ] ダイアログ ページでコピー、 **リモート ログイン URL** 値に設定して、貼り付けます、 **ログアウト URL** ] ボックスに貼り付けます。  

        >[AZURE.NOTE]ログイン URL 値ログアウト URL 値と、Mimecast Admin Console に同じです。

    7.  作成、 **base-64 でエンコードされた** からダウンロードした証明書ファイル。  

        >[AZURE.TIP]詳細については、次を参照してください。 [バイナリ証明書をテキスト ファイルに変換する方法](http://youtu.be/PlgrzUZ-Y1o)します。

    8.  開いているメモ帳で、base-64 でエンコードされた証明書、最初の行を削除 ("*--*") と最後の行 ("*--*")、その残りのコンテンツをクリップボードにコピーおよびに貼り付け、 **Id プロバイダー証明書 (メタデータ)** ] ボックスに貼り付けます。
    9.  選択 **シングル サインオンの許可**します。
    10. クリックして **保存**します。

10. Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-mimecast-admin-console-tutorial/IC795016.png "Configure Single Sign-On")
##ユーザー プロビジョニングの構成
  
Azure AD ユーザーが Mimecast Admin Console にログインできるようにするには、そのユーザーを Mimecast Admin Console にプロビジョニングする必要があります。  
Mimecast Admin Console の場合、プロビジョニングは手動で行います。
  
ユーザーを作成する前に、ドメインを登録する必要があります。

###ユーザー プロビジョニングを構成するには、次の手順に従います。

1.  サインイン、 **Mimecast Admin Console** 管理者として。

2.  移動して **ディレクトリ \ > 内部**します。

    ![ディレクトリ](./media/active-directory-saas-mimecast-admin-console-tutorial/IC795003.png "Directories")

3.  クリックして **新規ドメインの登録**します。

    ![新規ドメインの登録](./media/active-directory-saas-mimecast-admin-console-tutorial/IC795004.png "Register New Domain")

4.  新しいドメインが作成されたら、クリックして **新しいアドレス**です。

    ![新規アドレス](./media/active-directory-saas-mimecast-admin-console-tutorial/IC795005.png "New Address")

5.  新しいアドレスのダイアログで、次の手順に従います。

    ![Save](./media/active-directory-saas-mimecast-admin-console-tutorial/IC795006.png "Save")

    1.  型、 **電子メール アドレス**, 、**グローバル名**, 、**パスワード** と **パスワードの確認** 関連テキスト ボックスに、プロビジョニングする有効な AAD アカウントの属性です。
    2.  クリックして **保存**します。

>[AZURE.NOTE]その他の Mimecast Admin Console ユーザー アカウント作成ツールまたは Mimecast Admin Console で AAD ユーザー アカウントのプロビジョニングに提供される Api を使用することができます。

##ユーザーの割り当て
  
構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

###ユーザーを Mimecast Admin Console に割り当てるには、次の手順に従います。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  * * Mimecast Admin Console * * アプリケーション統合ページで、をクリックして **ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-mimecast-admin-console-tutorial/IC795017.png "Assign Users")

3.  テスト ユーザーを選択して [ **割り当てる**, 、順にクリック **はい** して割り当てを確認します。

    ![あり](./media/active-directory-saas-mimecast-admin-console-tutorial/IC767830.png "Yes")
  
シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。
