<properties 
    pageTitle="チュートリアル: Azure Active Directory と ShiftPlanning の統合 | Microsoft Azure" 
    description="Azure Active Directory で ShiftPlanning を使用して、シングル サインオンや自動プロビジョニングなどを有効にする方法について説明します。" 
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


# チュートリアル: Azure Active Directory と ShiftPlanning の統合

このチュートリアルの目的は、Azure と ShiftPlanning の統合を示すことです。  
このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   ShiftPlanning でのシングル サインオンが有効なサブスクリプション

このチュートリアルを完了すると、ShiftPlanning に割り当てた Azure AD ユーザーは、ShiftPlanning 会社サイト (サービス プロバイダーが開始したサインオン) で、アプリケーションにシングル サインオンまたはできるを使用して、 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。

このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  ShiftPlanning のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-shiftplanning-tutorial/IC786612.png "Scenario")
## ShiftPlanning のアプリケーション統合の有効化

このセクションでは、ShiftPlanning のアプリケーション統合を有効にする方法を説明します。

### ShiftPlanning のアプリケーション統合を有効にするには、次の手順に従います。

1.  Azure 管理ポータルの左側のナビゲーション ウィンドウで、**[Active Directory]** をクリックします。

    ![Active Directory](./media/active-directory-saas-shiftplanning-tutorial/IC700993.png "Active Directory")

2.  **[ディレクトリ]** の一覧から、ディレクトリ統合を有効にするディレクトリを選択します。

3.  アプリケーション ビューを開くには、ディレクトリ ビューでトップ メニューの **[アプリケーション]** をクリックします。

    ![アプリケーション](./media/active-directory-saas-shiftplanning-tutorial/IC700994.png "Applications")

4.  ページの下部にある **[追加]** をクリックします。

    ![アプリケーションの追加](./media/active-directory-saas-shiftplanning-tutorial/IC749321.png "Add application")

5.  **[実行する内容]** ダイアログで、**[ギャラリーからアプリケーションを追加します]** をクリックします。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-shiftplanning-tutorial/IC749322.png "Add an application from gallerry")

6.  **検索ボックス**に、「**ShiftPlanning**」と入力します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-shiftplanning-tutorial/IC786613.png "Application Gallery")

7.  結果ウィンドウで **[ShiftPlanning]** を選択し、**[完了]** をクリックしてアプリケーションを追加します。

    ![ShiftPlanning](./media/active-directory-saas-shiftplanning-tutorial/IC786614.png "ShiftPlanning")
## シングル サインオンの構成

このセクションでは、SAML プロトコルに基づくフェデレーションを使用して、ShiftPlanning でAzure AD のユーザー アカウントを使用してユーザーを認証できるようにする方法を説明します。  
この手順の途中で、base-64 でエンコードされた証明書ファイルを作成する必要があります。  
この手順に慣れていない場合は、を参照してください [バイナリ証明書をテキスト ファイルに変換する方法](http://youtu.be/PlgrzUZ-Y1o)

### シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **ShiftPlanning** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、 ** [シングル サインオン ** ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-shiftplanning-tutorial/IC786615.png "Configure Single Sign-On")

2.  **どのようなユーザーが ShiftPlanning へのサインオン** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、順にクリック **次**します。

    ![Configure Single Sign-On](./media/active-directory-saas-shiftplanning-tutorial/IC786616.png "Configure Single Sign-On")

3.  **アプリケーション URL の構成** ] ページの [、 **ShiftPlanning サインイン URL** ] ボックスに、次のパターンを使用して URL を入力"*https://company.shiftplanning.com/includes/saml/*"、] をクリックし、 **次**します。

    ![アプリケーション URL の構成](./media/active-directory-saas-shiftplanning-tutorial/IC786617.png "Configure App URL")

4.  **ShiftPlanning でのシングル サインオン構成** ] ページで、証明書をダウンロードする] をクリックして **証明書のダウンロード**, 、お使いのコンピューターに証明書ファイルを保存します。

    ![Configure Single Sign-On](./media/active-directory-saas-shiftplanning-tutorial/IC786618.png "Configure Single Sign-On")

5.  別の Web ブラウザーのウィンドウで、**ShiftPlanning** 企業サイトに管理者としてログインします。

6.  上部のメニューで **[Admin]** をクリックします。

    ![管理者](./media/active-directory-saas-shiftplanning-tutorial/IC786619.png "Admin")

7.  **[Integration]** の **[Single Sign-On]** をクリックします。

    ![シングル サインオン](./media/active-directory-saas-shiftplanning-tutorial/IC786620.png "Single Sign-On")

8.  **[Single Sign-On Options]** セクションで、次の手順に従います。

    ![シングル サインオン](./media/active-directory-saas-shiftplanning-tutorial/IC786905.png "Single Sign-On")

    1.  **[SAML Enabled]** を選択します。
    2.  **[Allow Password Login]** を選択します。
    3.  Azure ポータルで、**[ShiftPlanning でのシングル サインオンの構成]** ダイアログ ページの **[リモート ログイン URL]** の値をコピーし、**[SAML Issuer URL]** ボックスに貼り付けます。
    4.  Azure ポータルで、**[ShiftPlanning でのシングル サインオンの構成]** ダイアログ ページの **[リモート ログアウト URL]** の値をコピーし、**[Remote Logout URL]** ボックスに貼り付けます。
    5.  ダウンロードした証明書から **base-64 でエンコードされた**ファイルを作成します。
        >[AZURE.TIP]詳細については、「 [バイナリ証明書をテキスト ファイルに変換する方法](http://youtu.be/PlgrzUZ-Y1o)

    6.  base-64 でエンコードされた証明書をメモ帳で開き、その内容をクリップボードにコピーして、**[X.509 Certificate]** テキスト ボックスに貼り付けます。
    7.  **[Save Settings]** をクリックします。

9.  Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-shiftplanning-tutorial/IC786621.png "Configure Single Sign-On")
## ユーザー プロビジョニングの構成

Azure AD ユーザーが ShiftPlanning にログインできるようにするには、ユーザーを ShiftPlanning にプロビジョニングする必要があります。  
ShiftPlanning の場合、プロビジョニングは手動で行います。

### ユーザー アカウントをプロビジョニングするには、次の手順を実行します。

1.  **ShiftPlanning** 企業サイトに管理者としてログインします。

2.  **[管理者]** をクリックします。

    ![管理者](./media/active-directory-saas-shiftplanning-tutorial/IC786619.png "Admin")

3.  **[Staff]** をクリックします。

    ![スタッフ](./media/active-directory-saas-shiftplanning-tutorial/IC786623.png "Staff")

4.  **[Actions]** の **[Add Employee]** をクリックします。

    ![従業員の追加](./media/active-directory-saas-shiftplanning-tutorial/IC786624.png "Add Employees")

5.  **[Add Employees]** セクションで、次の手順に従います。

    ![従業員の保存](./media/active-directory-saas-shiftplanning-tutorial/IC786625.png "Save Employees")

    1.  対応するテキスト ボックスに、プロビジョニングする有効な AAD アカウントの**姓**、**名**、**電子メール アドレス**を入力します。
    2.  **[Save Employees]** をクリックします。

>[AZURE.NOTE]ShiftPlanning から提供されている他の ShiftPlanning ユーザー アカウント作成ツールまたは API を使用して、AAD ユーザー アカウントをプロビジョニングできます。

## ユーザーの割り当て

構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

### ユーザーを ShiftPlanning に割り当てるには、次の手順に従います。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  **ShiftPlanning * * アプリケーション統合ページで、をクリックして * * ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-shiftplanning-tutorial/IC786626.png "Assign Users")

3.  テスト ユーザーを選択し、**[割り当て]**、**[はい]** の順にクリックして、割り当てを確定します。

    ![はい](./media/active-directory-saas-shiftplanning-tutorial/IC767830.png "Yes")

シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。




