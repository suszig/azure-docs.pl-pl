<properties 
    pageTitle="チュートリアル: Azure Active Directory と e-Builder の統合 | Microsoft Azure" 
    description="Azure Active Directory で E-builder を使用して、シングル サインオン、自動化されたプロビジョニング、および詳細を有効にする方法を説明します。" 
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


# チュートリアル: Azure Active Directory と e-Builder の統合

このチュートリアルでは、Azure と e-Builder の統合について説明します。  
このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   e-Builder テナント

このチュートリアルを完了すると、E-builder に割り当てた Azure AD ユーザーは、E-builder 会社サイト (サービス プロバイダーが開始したサインオン) で、アプリケーションにシングル サインオンまたはできるを使用して、 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。

このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  e-Builder のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-e-builder-tutorial/IC777378.png "Scenario")
## e-Builder のアプリケーション統合の有効化

このセクションでは、e-Builder のアプリケーション統合を有効にする方法について説明します。

### e-Builder のアプリケーション統合を有効にするには、次の手順を実行します。

1.  Azure 管理ポータルの左側のナビゲーション ウィンドウで、**[Active Directory]** をクリックします。

    ![Active Directory](./media/active-directory-saas-e-builder-tutorial/IC700993.png "Active Directory")

2.  **[ディレクトリ]** の一覧から、ディレクトリ統合を有効にするディレクトリを選択します。

3.  アプリケーション ビューを開くには、ディレクトリ ビューでトップ メニューの **[アプリケーション]** をクリックします。

    ![アプリケーション](./media/active-directory-saas-e-builder-tutorial/IC700994.png "Applications")

4.  ページの下部にある **[追加]** をクリックします。

    ![アプリケーションの追加](./media/active-directory-saas-e-builder-tutorial/IC749321.png "Add application")

5.  **[実行する内容]** ダイアログで、**[ギャラリーからアプリケーションを追加します]** をクリックします。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-e-builder-tutorial/IC749322.png "Add an application from gallerry")

6.  **検索ボックス**に、「**e-Builder**」と入力します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-e-builder-tutorial/IC777379.png "Application gallery")

7.  結果ウィンドウで **[e-Builder]** を選択し、**[完了]** をクリックしてアプリケーションを追加します。

    ![e-Builder](./media/active-directory-saas-e-builder-tutorial/IC777380.png "e-Builder")
## シングル サインオンの構成

このセクションでは、ユーザーが SAML プロトコルに基づくフェデレーションを使用して、Azure AD でのユーザーのアカウントで e-Builder に対する認証を行うことができるようにする方法を説明します。

### シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **E-builder** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、 ** [シングル サインオン ** ダイアログ。

    ![シングル サインオンの構成](./media/active-directory-saas-e-builder-tutorial/IC777381.png "Configure single sign-on")

2.  **どのようなユーザーを E-builder へのサインオン** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、順にクリック **次**します。

    ![シングル サインオンの構成](./media/active-directory-saas-e-builder-tutorial/IC777382.png "Configure single sign-on")

3.  **アプリケーション URL の構成** ] ページの [、 **E-builder サインイン URL** ] ボックスに、次のパターンを使用して URL を入力"* https://\<tenant-name\>.e Builder.com*"、] をクリックし、 **次**します。

    ![アプリケーション URL の構成](./media/active-directory-saas-e-builder-tutorial/IC777383.png "Configure app URL")

4.  **E-builder でのシングル サインオンの構成** ] ページで、メタデータをダウンロードする] をクリックして **メタデータのダウンロード**, 、し、データ ファイルをローカルでとして **c:\\E-BuilderMetaData.xml**します。

    ![シングル サインオンの構成](./media/active-directory-saas-e-builder-tutorial/IC777384.png "Configure single sign-on")

5.  メタデータ ファイルを e-Builder サポート チームに転送します。 サポート チームは、シングル サインオンを構成する必要があります。

6.  シングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![シングル サインオンの構成](./media/active-directory-saas-e-builder-tutorial/IC777385.png "Configure single sign-on")
## ユーザー プロビジョニングの構成

e-Builder へのユーザー プロビジョニングの構成にあたって必要な操作はありません。  
割り当てられたユーザーがアクセス パネルを使用して e-Builder にログインしようとすると、そのユーザーが存在するかどうかが e-Builder によって確認されます。  
使用可能なユーザー アカウントがない場合、ユーザー アカウントは自動的に作成されます。
## ユーザーの割り当て

構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

### ユーザーを e-Builder に割り当てるには、次の手順を実行します。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  **E-builder * * アプリケーション統合ページで、をクリックして * * ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-e-builder-tutorial/IC777386.png "Assign users")

3.  テスト ユーザーを選択し、**[割り当て]**、**[はい]** の順にクリックして、割り当てを確定します。

    ![はい](./media/active-directory-saas-e-builder-tutorial/IC767830.png "Yes")

シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。




