<properties 
    pageTitle="チュートリアル: Azure Active Directory と 15Five の統合 | Microsoft Azure" 
    description="Azure Active Directory と 15 five を使用して、シングル サインオン、自動化されたプロビジョニング、および詳細を有効にする方法を説明します。" 
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


# チュートリアル: Azure Active Directory と 15Five の統合

このチュートリアルの目的は、Azure と 15Five の統合を紹介することです。 このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   15Five でのシングル サインオンが有効なサブスクリプション

このチュートリアルを完了すると、15 five に割り当てた Azure AD ユーザーは、15 five 会社サイト (サービス プロバイダーが開始したサインオン) で、アプリケーションにシングル サインオンまたはできるを使用して、 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。

このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  15Five のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-15five-tutorial/IC784667.png "Scenario")
## 15Five のアプリケーション統合の有効化

このセクションでは、15Five のアプリケーション統合を有効にする方法について説明します。

### 15Five のアプリケーション統合を有効にするには、次の手順に従います。

1.  Azure 管理ポータルの左側のナビゲーション ウィンドウで、**[Active Directory]** をクリックします。

    ![Active Directory](./media/active-directory-saas-15five-tutorial/IC700993.png "Active Directory")

2.  **[ディレクトリ]** の一覧から、ディレクトリ統合を有効にするディレクトリを選択します。

3.  アプリケーション ビューを開くには、ディレクトリ ビューでトップ メニューの **[アプリケーション]** をクリックします。

    ![アプリケーション](./media/active-directory-saas-15five-tutorial/IC700994.png "Applications")

4.  ページの下部にある **[追加]** をクリックします。

    ![アプリケーションの追加](./media/active-directory-saas-15five-tutorial/IC749321.png "Add application")

5.  **[実行する内容]** ダイアログで、**[ギャラリーからアプリケーションを追加します]** をクリックします。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-15five-tutorial/IC749322.png "Add an application from gallerry")

6.  [**検索**] ボックスに、「**15Five**」と入力します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-15five-tutorial/IC784668.png "Application Gallery")

7.  結果ウィンドウで [**15Five**] を選択し、[**完了**] をクリックしてアプリケーションを追加します。

    ![15Five](./media/active-directory-saas-15five-tutorial/IC784669.png "15Five")
## シングル サインオンの構成

このセクションでは、ユーザーが SAML プロトコルに基づくフェデレーションを使用して、Azure AD でのユーザーのアカウントで 15Five に対する認証を行えるようにする方法を説明します。

### シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **15 five** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、 ** [シングル サインオン ** ダイアログ。

    ![シングル サインオンの構成](./media/active-directory-saas-15five-tutorial/IC784670.png "Configure single sign-on")

2.  **どのようなユーザーが 15 five へのサインオンを** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、順にクリック **次**します。

    ![シングル サインオンの構成](./media/active-directory-saas-15five-tutorial/IC784671.png "Configure single sign-on")

3.  **アプリケーション URL の構成** ] ページの [、 **15 five サインイン URL** ] ボックスに、次のパターンを使用して URL を入力"*https://company.15Five.com*"、] をクリックし、 **次**します。

    ![アプリケーション URL の構成](./media/active-directory-saas-15five-tutorial/IC784672.png "Configure App URL")

4.  **15 five でのシングル サインオンの構成** ] ページで [ **メタデータのダウンロード**, 、15 five サポート チームにメタデータ ファイルを転送します。

    ![シングル サインオンの構成](./media/active-directory-saas-15five-tutorial/IC784673.png "Configure single sign-on")
    >[AZURE.NOTE] 15Five サポート チームがシングル サインオンを有効にする必要があります。

5.  Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![シングル サインオンの構成](./media/active-directory-saas-15five-tutorial/IC784674.png "Configure single sign-on")
## ユーザー プロビジョニングの構成

Azure AD ユーザーが 15Five にログインできるようにするには、そのユーザーを 15Five にプロビジョニングする必要があります。  
15Five の場合、プロビジョニングは手動で行います。

### ユーザー プロビジョニングを構成するには、次の手順に従います。

1.  **15Five** 企業サイトに管理者としてログインします。

2.  [**会社の管理**] に移動します。

    ![会社の管理](./media/active-directory-saas-15five-tutorial/IC784675.png "Manage Company")

3.  移動して **人 \ > ユーザーを追加する**します。

    ![ユーザー](./media/active-directory-saas-15five-tutorial/IC784676.png "People")

4.  [新しいユーザーの追加] セクションで、次の手順に従います。

    ![新しいユーザーの追加](./media/active-directory-saas-15five-tutorial/IC784677.png "Add New Person")

    1.  関連するテキスト ボックスに、プロビジョニングする有効な Azure Active Directory アカウントの**名**、**姓**、**役職**、**電子メール アドレス**を入力します。
    2.  **[Done]** をクリックします。

    >[AZURE.NOTE] アカウントがアクティブになる前に、Azure AD アカウント所有者に、アカウント確認用のリンクを含む電子メールが送信されます。

>[AZURE.NOTE] 他の 15Five ユーザー アカウントの作成ツールまたは 15Five から提供されている API を使用して、AAD ユーザー アカウントをプロビジョニングできます。

## ユーザーの割り当て

構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

### ユーザーを 15Five に割り当てるには、次の手順に従います。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  **15 five * * アプリケーション統合ページで、をクリックして * * ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-15five-tutorial/IC784678.png "Assign users")

3.  テスト ユーザーを選択し、**[割り当て]**、**[はい]** の順にクリックして、割り当てを確定します。

    ![はい](./media/active-directory-saas-15five-tutorial/IC767830.png "Yes")

シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。





