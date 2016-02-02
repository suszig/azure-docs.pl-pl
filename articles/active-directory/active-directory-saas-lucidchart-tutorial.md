<properties 
    pageTitle="チュートリアル: Azure Active Directory と Lucidchart の統合 | Microsoft Azure" 
    description="Azure Active Directory と Lucidchart を使用して、シングル サインオン、自動化されたプロビジョニング、および詳細を有効にする方法を説明します。" 
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


# チュートリアル: Azure Active Directory と Lucidchart の統合

このチュートリアルでは、Azure と Lucidchart の統合について説明します。  
このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   Lucidchart でのシングル サインオンが有効なサブスクリプション

このチュートリアルを終了すた後には、Lucidchart に割り当てた Azure AD ユーザーは、Lucidchart 会社サイト (サービス プロバイダーが開始したサインオン) で、アプリケーションにシングル サインオンまたはできるを使用して、 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。

このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  Lucidchart のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-lucidchart-tutorial/IC791183.png "Scenario")
## Lucidchart のアプリケーション統合の有効化

このセクションでは、Lucidchart のアプリケーション統合を有効にする方法を説明します。

### Lucidchart のアプリケーション統合を有効にするには、次の手順に従います。

1.  Azure 管理ポータルの左側のナビゲーション ウィンドウで、**[Active Directory]** をクリックします。

    ![Active Directory](./media/active-directory-saas-lucidchart-tutorial/IC700993.png "Active Directory")

2.  **[ディレクトリ]** の一覧から、ディレクトリ統合を有効にするディレクトリを選択します。

3.  アプリケーション ビューを開くには、ディレクトリ ビューでトップ メニューの **[アプリケーション]** をクリックします。

    ![アプリケーション](./media/active-directory-saas-lucidchart-tutorial/IC700994.png "Applications")

4.  ページの下部にある **[追加]** をクリックします。

    ![アプリケーションの追加](./media/active-directory-saas-lucidchart-tutorial/IC749321.png "Add application")

5.  **[実行する内容]** ダイアログで、**[ギャラリーからアプリケーションを追加します]** をクリックします。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-lucidchart-tutorial/IC749322.png "Add an application from gallerry")

6.  **検索ボックス**に、「**Lucidchart**」と入力します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-lucidchart-tutorial/IC791184.png "Application Gallery")

7.  結果ウィンドウで **[Lucidchart]** を選択し、**[完了]** をクリックしてアプリケーションを追加します。

    ![Lucidchart](./media/active-directory-saas-lucidchart-tutorial/IC791185.png "Lucidchart")
## シングル サインオンの構成

このセクションでは、SAML プロトコルに基づくフェデレーションを使用して、ユーザーが Azure AD のアカウントで Lucidchart に対する認証を行うことができるようにする方法を説明します。

### シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **Lucidchart** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、 ** [シングル サインオン ** ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-lucidchart-tutorial/IC791186.png "Configure Single Sign-On")

2.  **どのようなユーザーが Lucidchart へのサインオン** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、] をクリックし、 **次**します。

    ![Configure Single Sign-On](./media/active-directory-saas-lucidchart-tutorial/IC791187.png "Configure Single Sign-On")

3.  **アプリケーション URL の構成** ] ページの [、 **Lucidchart サインオン URL** URL は、ユーザーが Lucidchart アプリケーションにサインオンする使用] ボックスに、型 (例::"*https://chart2.office.lucidchart.com/saml/sso/azure*")、順にクリック **次**します。

    ![アプリケーション URL の構成](./media/active-directory-saas-lucidchart-tutorial/IC791188.png "Configure App URL")

4.  **Lucidchart でのシングル サインオンの構成** ] ページで、メタデータをダウンロードする] をクリックして **メタデータのダウンロード**, 、コンピューターのローカルにデータ ファイルを保存します。

    ![Configure Single Sign-On](./media/active-directory-saas-lucidchart-tutorial/IC791189.png "Configure Single Sign-On")

5.  別の Web ブラウザー ウィンドウで、Lucidchart の企業サイトに管理者としてログインします。

6.  上部のメニューで **[チーム]** をクリックします。

    ![チーム](./media/active-directory-saas-lucidchart-tutorial/IC791190.png "Team")

7.  クリックして **アプリケーション \ > SAML の管理**します。

    ![SAML の管理](./media/active-directory-saas-lucidchart-tutorial/IC791191.png "Manage SAML")

8.  **[SAML Authentication Settings]** ダイアログ ページで、次の手順に従います。

    1.  選択 **SAML 認証を有効にする**, 、] をクリックし、 **オプション**します。
        ![SAML 認証の設定](./media/active-directory-saas-lucidchart-tutorial/IC791192.png "SAML Authentication Settings")
    2.  **ドメイン** ] ボックスが、ドメインを入力し、クリックして **証明書の変更**します。
        ![証明書の変更](./media/active-directory-saas-lucidchart-tutorial/IC791193.png "Change Certificate")
    3.  ダウンロードしたメタデータ ファイルを開いて、コンテンツをコピーして貼り付けます、 **アップロード メタデータ** ] ボックスに貼り付けます。
        ![メタデータのアップロード](./media/active-directory-saas-lucidchart-tutorial/IC791194.png "Upload Metadata")
    4.  選択 **チームに新しいユーザーを自動的に追加**, 、クリックして **変更を保存**します。
        ![変更を保存](./media/active-directory-saas-lucidchart-tutorial/IC791195.png "Save Changes")

9.  シングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-lucidchart-tutorial/IC791196.png "Configure Single Sign-On")
## ユーザー プロビジョニングの構成

Lucidchart へのユーザー プロビジョニングの構成にあたって必要な操作はありません。  
割り当て済みユーザーがアクセス パネルを使用して Lucidchart にログインしようとすると、そのユーザーが存在するかどうかが Lucidchart によって確認されます。  
使用可能なユーザー アカウントがない場合、ユーザー アカウントは Lucidchart により自動的に作成されます。
## ユーザーを割り当てる

構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

### ユーザーを Lucidchart に割り当てるには、次の手順に従います。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  **Lucidchart * * アプリケーション統合ページで、をクリックして * * ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-lucidchart-tutorial/IC791197.png "Assign Users")

3.  テスト ユーザーを選択し、**[割り当て]**、**[はい]** の順にクリックして、割り当てを確定します。

    ![はい](./media/active-directory-saas-lucidchart-tutorial/IC767830.png "Yes")

シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。




