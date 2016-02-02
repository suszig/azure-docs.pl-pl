<properties 
    pageTitle="チュートリアル: Azure Active Directory と AirWatch の統合 | Microsoft Azure" 
    description="Azure Active Directory と AirWatch を使用して、シングル サインオン、自動化されたプロビジョニング、および詳細を有効にする方法を説明します。" 
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


# チュートリアル: Azure Active Directory と AirWatch の統合

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
## AirWatch のアプリケーション統合の有効化

このセクションでは、AirWatch のアプリケーション統合を有効にする方法について説明します。

### AirWatch のアプリケーション統合を有効にするには、次の手順に従います。

1.  Azure 管理ポータルの左側のナビゲーション ウィンドウで、**[Active Directory]** をクリックします。

    ![Active Directory](./media/active-directory-saas-airwatch-tutorial/IC700993.png "Active Directory")

2.  **[ディレクトリ]** の一覧から、ディレクトリ統合を有効にするディレクトリを選択します。

3.  アプリケーション ビューを開くには、ディレクトリ ビューでトップ メニューの **[アプリケーション]** をクリックします。

    ![アプリケーション](./media/active-directory-saas-airwatch-tutorial/IC700994.png "Applications")

4.  ページの下部にある **[追加]** をクリックします。

    ![アプリケーションの追加](./media/active-directory-saas-airwatch-tutorial/IC749321.png "Add application")

5.  **[実行する内容]** ダイアログで、**[ギャラリーからアプリケーションを追加します]** をクリックします。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-airwatch-tutorial/IC749322.png "Add an application from gallerry")

6.  [**検索**] ボックスに、「**AirWatch**」と入力します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-airwatch-tutorial/IC791914.png "Application Gallery")

7.  結果ウィンドウで [**AirWatch**] を選択し、[**完了**] をクリックしてアプリケーションを追加します。

    ![AirWatch](./media/active-directory-saas-airwatch-tutorial/IC791915.png "AirWatch")
## シングル サインオンの構成

このセクションでは、ユーザーが SAML プロトコルに基づくフェデレーションを使用して、Azure AD でのユーザーのアカウントで AirWatch に対する認証を行えるようにする方法を説明します。  
この手順の途中で、base-64 でエンコードされた証明書ファイルを作成する必要があります。  
この手順に慣れていない場合は、次を参照してください。 [バイナリ証明書をテキスト ファイルに変換する方法](http://youtu.be/PlgrzUZ-Y1o)します。

### シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **AirWatch** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、 ** [シングル サインオン ** ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-airwatch-tutorial/IC791916.png "Configure Single Sign-On")

2.  **どのようなユーザーの AirWatch へのアクセス** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、] をクリックし、 **次**します。

    ![Configure Single Sign-On](./media/active-directory-saas-airwatch-tutorial/IC791917.png "Configure Single Sign-On")

3.  **アプリケーション URL の構成** ] ページで、 **AirWatch サインオン URL** ] ボックスに、型に使用する URL、ユーザーが AirWatch アプリケーションにサインイン (例::"*https://companycode.awmdm.com/AirWatch/Login?gid=companycode*")、順にクリック **次**します。

    ![アプリケーション URL の構成](./media/active-directory-saas-airwatch-tutorial/IC791918.png "Configure App URL")

4.  **AirWatch でのシングル サインオンの構成** ] ページで [ **証明書のダウンロード**, 、お使いのコンピューターに証明書ファイルを保存します。

    ![Configure Single Sign-On](./media/active-directory-saas-airwatch-tutorial/IC791919.png "Configure Single Sign-On")

5.  別の Web ブラウザーのウィンドウで、管理者として AirWatch 企業サイトにログインします。

6.  左側のナビゲーション ウィンドウで、[**Accounts**]、[**Administrators**] の順にクリックします。

    ![管理者](./media/active-directory-saas-airwatch-tutorial/IC791920.png "Administrators")

7.  [**Settings**] メニューを展開し、[**Directory Services**] をクリックします。

    ![設定](./media/active-directory-saas-airwatch-tutorial/IC791921.png "Settings")

8.  [**User**] タブをクリックし、[**Base DN**] テキスト フィールドにドメイン名を入力してから [**Save**] をクリックします。

    ![ユーザー](./media/active-directory-saas-airwatch-tutorial/IC791922.png "User")

9.  [**Server**] タブをクリックします。

    ![サーバー](./media/active-directory-saas-airwatch-tutorial/IC791923.png "Server")

10. 次の手順に従います。

    ![アップロード](./media/active-directory-saas-airwatch-tutorial/IC791924.png "Upload")

    1.  [**Directory Type**] として [**None**] を選択します。
    2.  [**Use SAML For Authentication**] を選択します。
    3.  ダウンロードした証明書をアップロードするには、[**Upload**] をクリックします。

11. [**Request**] セクションで、次の手順に従います。

    ![要求](./media/active-directory-saas-airwatch-tutorial/IC791925.png "Request")

    1.  [**Request Binding Type**] として [**POST**] を選択します。
    2.  Azure ポータルの [**Airwatch でのシングル サインオンの構成**] ダイアログ ページで、[**シングル サインオン サービス URL**] の値をコピーし、[**ID プロバイダー シングル サインオン URL**] テキスト ボックスに貼り付けます。
    3.  [**NameID Format**] として [**Email Address**] を選択します。
    4.  [**Save**] をクリックします。

12. [**User**] タブをもう一度クリックします。

    ![ユーザー](./media/active-directory-saas-airwatch-tutorial/IC791926.png "User")

13. [**Attribute**] セクションで、次の手順に従います。

    ![属性](./media/active-directory-saas-airwatch-tutorial/IC791927.png "Attribute")

    1.  **オブジェクト識別子** ] ボックスに「 **http://schemas.microsoft.com/identity/claims/objectidentifier**します。
    2.  **Username** ] ボックスに「 **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**します。
    3.  **表示名** ] ボックスに「 **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**します。
    4.  **名** ] ボックスに「 **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**します。
    5.  **姓** ] ボックスに「 **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**します。
    6.  **電子メール** ] ボックスに「 **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**します。
    7.  **[保存]** をクリックします。

14. Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-airwatch-tutorial/IC791928.png "Configure Single Sign-On")
## ユーザー プロビジョニングの構成

Azure AD ユーザーが AirWatch にログインできるようにするには、そのユーザーを AirWatch にプロビジョニングする必要があります。  
AirWatch の場合、プロビジョニングは手動で行います。

### ユーザー アカウントをプロビジョニングするには、次の手順に従います。

1.  **AirWatch** 企業サイトに管理者としてログインします。

2.  左側のナビゲーション ウィンドウで、[**Accounts**]、[**Users**] の順にクリックします。

    ![ユーザー](./media/active-directory-saas-airwatch-tutorial/IC791929.png "Users")

3.  **ユーザー** ] メニューのをクリックして **リスト ビュー**, 、クリックして **追加 \ > ユーザーの追加**します。

    ![ユーザーの追加](./media/active-directory-saas-airwatch-tutorial/IC791930.png "Add User")

4.  [**Add / Edit User**] ダイアログで、次の手順を実行します。

    ![ユーザーの追加](./media/active-directory-saas-airwatch-tutorial/IC791931.png "Add User")

    1.  関連するテキスト ボックスに、プロビジョニングする有効な Azure Active Directory アカウントの [**Username**]、[**Password**]、[**Confirm Password**]、[**First Name**]、[**Last Name**]、[**Email Address**] を入力します。
    2.  [**Save**] をクリックします。

>[AZURE.NOTE] 他の AirWatch ユーザー アカウントの作成ツールまたは AirWatch から提供されている API を使用して、AAD ユーザー アカウントをプロビジョニングできます。

## ユーザーの割り当て

構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

### ユーザーを AirWatch に割り当てるには、次の手順に従います。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  **AirWatch * * アプリケーション統合ページで、をクリックして * * ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-airwatch-tutorial/IC791932.png "Assign Users")

3.  テスト ユーザーを選択し、**[割り当て]**、**[はい]** の順にクリックして、割り当てを確定します。

    ![はい](./media/active-directory-saas-airwatch-tutorial/IC767830.png "Yes")

シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。





