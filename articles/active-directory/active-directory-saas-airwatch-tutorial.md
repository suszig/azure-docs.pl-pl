<properties 
    pageTitle="チュートリアル: Azure Active Directory と AirWatch の統合 | Microsoft Azure" 
    description="Azure Active Directory で AirWatch を使用して、シングル サインオンや自動プロビジョニングなどを有効にする方法について説明します。" 
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

#チュートリアル: Azure Active Directory と AirWatch の統合

このチュートリアルでは、Azure と AirWatch の統合について説明します。  
このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   AirWatch でのシングル サインオンが有効なサブスクリプション

このチュートリアルを完了すると、AirWatch に割り当てた Azure AD ユーザーは、AirWatch 会社サイト (サービス プロバイダーが開始したサインオン) で、アプリケーションにシングル サインオンまたはできるを使用して、 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。

このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  AirWatch のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![AirWatch](./media/active-directory-saas-airwatch-tutorial/IC791913.png "AirWatch")
##AirWatch のアプリケーション統合の有効化

このセクションでは、AirWatch のアプリケーション統合を有効にする方法について説明します。

###AirWatch のアプリケーション統合を有効にするには、次の手順に従います。

1.  左側のナビゲーション ウィンドウで、Azure 管理ポータルで、クリックして **Active Directory**します。

    ![Active Directory](./media/active-directory-saas-airwatch-tutorial/IC700993.png "Active Directory")

2.   **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3.  ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。

    ![アプリケーション](./media/active-directory-saas-airwatch-tutorial/IC700994.png "Applications")

4.  クリックして **追加** ページの下部にあります。

    ![アプリケーションの追加](./media/active-directory-saas-airwatch-tutorial/IC749321.png "Add application")

5.   **実行する操作** ダイアログ ボックスで、をクリックして **ギャラリーからアプリケーションを追加**します。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-airwatch-tutorial/IC749322.png "Add an application from gallerry")

6.   **検索ボックス**, 、型 **AirWatch**します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-airwatch-tutorial/IC791914.png "Application Gallery")

7.  結果ウィンドウで [ **AirWatch**, 、クリックして **完了** アプリケーションを追加します。

    ![AirWatch](./media/active-directory-saas-airwatch-tutorial/IC791915.png "AirWatch")
##シングル サインオンの構成

このセクションでは、ユーザーが SAML プロトコルに基づくフェデレーションを使用して、Azure AD でのユーザーのアカウントで AirWatch に対する認証を行えるようにする方法を説明します。  
この手順の途中で、base-64 でエンコードされた証明書ファイルを作成する必要があります。  
この手順に慣れていない場合は、次を参照してください。 [バイナリ証明書をテキスト ファイルに変換する方法](http://youtu.be/PlgrzUZ-Y1o)します。

###シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **AirWatch** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、* * [シングル サインオン * * ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-airwatch-tutorial/IC791916.png "Configure Single Sign-On")

2.   **どのようなユーザーの AirWatch へのアクセス** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、] をクリックし、 **次**します。

    ![Configure Single Sign-On](./media/active-directory-saas-airwatch-tutorial/IC791917.png "Configure Single Sign-On")

3.   **アプリケーション URL の構成** ] ページで、 **AirWatch サインオン URL** ] ボックスに、型に使用する URL、ユーザーが AirWatch アプリケーションにサインイン (例::"*https:// companycode.awmdm.com/AirWatch/Login?gid=companycode*")、順にクリック **次**します。

    ![Configure App URL](./media/active-directory-saas-airwatch-tutorial/IC791918.png "Configure App URL")

4.   **AirWatch でのシングル サインオンの構成** ] ページで [ **証明書のダウンロード**, 、お使いのコンピューターに証明書ファイルを保存します。

    ![Configure Single Sign-On](./media/active-directory-saas-airwatch-tutorial/IC791919.png "Configure Single Sign-On")

5.  別の Web ブラウザーのウィンドウで、管理者として AirWatch 企業サイトにログインします。

6.  左側のナビゲーション ウィンドウで、クリックして **アカウント**, 、クリックして **管理者**します。

    ![管理者](./media/active-directory-saas-airwatch-tutorial/IC791920.png "Administrators")

7.  展開、 **設定** ] メニューの [クリックして **ディレクトリ サービス**します。

    ![Settings](./media/active-directory-saas-airwatch-tutorial/IC791921.png "Settings")

8.  クリックして、 **ユーザー** ] タブの [、 **ベース DN** テキスト フィールドが、ドメイン名を入力し、クリックして **保存**します。

    ![ユーザー](./media/active-directory-saas-airwatch-tutorial/IC791922.png "User")

9.  クリックして、 **Server** ] タブをクリックします。

    ![サーバー](./media/active-directory-saas-airwatch-tutorial/IC791923.png "Server")

10. 次の手順に従います。

    ![アップロード](./media/active-directory-saas-airwatch-tutorial/IC791924.png "Upload")

    1.  として **ディレクトリ タイプ**, [ **None**します。
    2.  選択 **SAML を使用して認証**します。
    3.  ダウンロードした証明書をアップロードするにはクリックして **アップロード**します。

11.  **要求** セクションで、次の手順に従います。

    ![Request (要求)](./media/active-directory-saas-airwatch-tutorial/IC791925.png "Request")

    1.  として **Request Binding Type**, [ **POST**します。
    2.  Azure ポータルで、 **Airwatch でのシングル サインオンの構成** ] ダイアログ ページでコピー、 **シングル サインオン サービス URL** 値に設定して、貼り付けます、 **Identity Provider Single Sign On URL** ] ボックスに貼り付けます。
    3.  として **NameID Format**, [ **電子メール アドレス**です。
    4.  クリックして **保存**します。

12. クリックして、 **ユーザー** タブを再度クリックします。

    ![ユーザー](./media/active-directory-saas-airwatch-tutorial/IC791926.png "User")

13.  **属性** セクションで、次の手順に従います。

    ![属性](./media/active-directory-saas-airwatch-tutorial/IC791927.png "Attribute")

    1.   **オブジェクト識別子** ] ボックスに「 **http://schemas.microsoft.com/identity/claims/objectidentifier**します。
    2.   **Username** ] ボックスに「 **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**します。
    3.   **表示名** ] ボックスに「 **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**します。
    4.   **名** ] ボックスに「 **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**します。
    5.   **姓** ] ボックスに「 **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**します。
    6.   **電子メール** ] ボックスに「 **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**します。
    7.  クリックして **保存**します。

14. Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-airwatch-tutorial/IC791928.png "Configure Single Sign-On")
##ユーザー プロビジョニングの構成

Azure AD ユーザーが AirWatch にログインできるようにするには、そのユーザーを AirWatch にプロビジョニングする必要があります。  
AirWatch の場合、プロビジョニングは手動で行います。

###ユーザー アカウントをプロビジョニングするには、次の手順を実行します。

1.  ログインして **AirWatch** 会社サイトに管理者として。

2.  左側のナビゲーション ウィンドウでをクリックして **アカウント**, 、クリックして **ユーザー**します。

    ![ユーザー](./media/active-directory-saas-airwatch-tutorial/IC791929.png "Users")

3.   **ユーザー** ] メニューのをクリックして **リスト ビュー**, 、クリックして **追加 \ > ユーザーの追加**します。

    ![ユーザーの追加](./media/active-directory-saas-airwatch-tutorial/IC791930.png "Add User")

4.   **Add/edit User** ] ダイアログ ボックスで、次の手順を実行します。

    ![ユーザーの追加](./media/active-directory-saas-airwatch-tutorial/IC791931.png "Add User")

    1.  型、 **Username**, 、**パスワード**, 、**パスワードの確認**, 、**名**, 、**Last Name**, 、**電子メール アドレス** 関連テキスト ボックスに、プロビジョニングする有効な Azure Active Directory アカウントのです。
    2.  クリックして **保存**します。

>[AZURE.NOTE] 他の AirWatch ユーザー アカウント作成ツールを使用することができますか、AAD ユーザー アカウントをプロビジョニング、AirWatch から提供されている Api です。

##ユーザーの割り当て

構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

###ユーザーを AirWatch に割り当てるには、次の手順に従います。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  * * AirWatch * * アプリケーション統合ページで、をクリックして **ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-airwatch-tutorial/IC791932.png "Assign Users")

3.  テスト ユーザーを選択して [ **割り当てる**, 、順にクリック **はい** して割り当てを確認します。

    ![あり](./media/active-directory-saas-airwatch-tutorial/IC767830.png "Yes")

シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。


