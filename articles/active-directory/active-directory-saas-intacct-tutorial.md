<properties 
    pageTitle="チュートリアル: Azure Active Directory と Intacct の統合 | Microsoft Azure" 
    description="Azure Active Directory と Intacct を使用して、シングル サインオン、自動化されたプロビジョニング、および詳細を有効にする方法を説明します。" 
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


# チュートリアル: Azure Active Directory と Intacct の統合

このチュートリアルでは、Azure と Intacct の統合について説明します。  
このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   Intacct テナント

このチュートリアルを完了すると、Intacct に割り当てた Azure AD ユーザーは、Intacct 会社サイト (サービス プロバイダーが開始したサインオン) で、アプリケーションにシングル サインオンまたはできるを使用して、 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。

このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  Intacct のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-intacct-tutorial/IC790030.png "Scenario")
## Intacct のアプリケーション統合の有効化

このセクションでは、Intacct のアプリケーション統合を有効にする方法について説明します。

### Intacct のアプリケーション統合を有効にするには、次の手順を実行します。

1.  Azure 管理ポータルの左側のナビゲーション ウィンドウで、**[Active Directory]** をクリックします。

    ![Active Directory](./media/active-directory-saas-intacct-tutorial/IC700993.png "Active Directory")

2.  **[ディレクトリ]** の一覧から、ディレクトリ統合を有効にするディレクトリを選択します。

3.  アプリケーション ビューを開くには、ディレクトリ ビューでトップ メニューの **[アプリケーション]** をクリックします。

    ![アプリケーション](./media/active-directory-saas-intacct-tutorial/IC700994.png "Applications")

4.  ページの下部にある **[追加]** をクリックします。

    ![アプリケーションの追加](./media/active-directory-saas-intacct-tutorial/IC749321.png "Add application")

5.  **[実行する内容]** ダイアログで、**[ギャラリーからアプリケーションを追加します]** をクリックします。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-intacct-tutorial/IC749322.png "Add an application from gallerry")

6.  **検索ボックス**に、「**Intacct**」と入力します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-intacct-tutorial/IC790031.png "Application Gallery")

7.  結果ウィンドウで **[Intacct]** を選択し、**[完了]** をクリックしてアプリケーションを追加します。

    ![Intacct](./media/active-directory-saas-intacct-tutorial/IC790032.png "Intacct")
## シングル サインオンの構成

このセクションでは、SAML プロトコルに基づくフェデレーションを使用して、ユーザーが Azure AD のアカウントで Intacct に対する認証を行うことができるようにする方法を説明します。  
この手順の途中で、base-64 でエンコードされた証明書ファイルを作成する必要があります。  
この手順に慣れていない場合は、次を参照してください。 [バイナリ証明書をテキスト ファイルに変換する方法](http://youtu.be/PlgrzUZ-Y1o)します。

### シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **Intacct** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、 ** [シングル サインオン ** ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-intacct-tutorial/IC790033.png "Configure Single Sign-On")

2.  **どのようなユーザーの Intacct に** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、] をクリックし、 **次**します。

    ![Configure Single Sign-On](./media/active-directory-saas-intacct-tutorial/IC790034.png "Configure Single Sign-On")

3.  **アプリケーション URL の構成** ] ページの [、 **Intacct サインオン URL** ] ボックスに、次のパターンを使用して URL を入力"*https://Intacct.com/company*"、] をクリックし、 **次**します。

    ![アプリケーション URL の構成](./media/active-directory-saas-intacct-tutorial/IC790035.png "Configure App URL")

4.  **Intacct でのシングル サインオンの構成** ] ページで [ **証明書のダウンロード**, 、お使いのコンピューターに証明書ファイルを保存します。

    ![Configure Single Sign-On](./media/active-directory-saas-intacct-tutorial/IC790036.png "Configure Single Sign-On")

5.  別の Web ブラウザー ウィンドウで、Intacct 企業サイトに管理者としてログインします。

6.  **[会社]** タブをクリックし、**[会社情報]** をクリックします。

    ![会社](./media/active-directory-saas-intacct-tutorial/IC790037.png "Company")

7.  **[セキュリティ]** タブをクリックし、**[編集]** をクリックします。

    ![セキュリティ](./media/active-directory-saas-intacct-tutorial/IC790038.png "Security")

8.  **[シングル サインオン (SSO)]** セクションで、次の手順を実行します。

    ![シングル サインオン](./media/active-directory-saas-intacct-tutorial/IC790039.png "Single Sign On")

    1.  **[シングル サインオンを有効にする]** を選択します。
    2.  **[ID プロバイダーの種類]** として **[SAML 2.0]** を選択します。
    3.  Azure ポータルの **[Intacct でのシングル サインオンの構成]** ダイアログ ページで **[発行者の URL]** の値をコピーし、それを **[発行者 URL]** テキストボックスに貼り付けます。
    4.  Azure ポータルで、**[Intacct でのシングル サインオンの構成]** ダイアログ ページの **[リモート ログイン URL]** の値をコピーし、**[ログイン URL]** ボックスに貼り付けます。
    5.  ダウンロードした証明書から **base-64 でエンコードされた**ファイルを作成します。
        >[AZURE.TIP]詳細については、「 [バイナリ証明書をテキスト ファイルに変換する方法](http://youtu.be/PlgrzUZ-Y1o)

    6.  base-64 でエンコードされた証明書をメモ帳で開き、その内容をクリップボードにコピーして、**[証明書]** テキストボックスに貼り付けます。
    7.  **[保存]** をクリックします。

9.  Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-intacct-tutorial/IC790040.png "Configure Single Sign-On")
## ユーザー プロビジョニングの構成

Azure AD ユーザーが Intacct にログインできるようにするには、そのユーザーを Intacct にプロビジョニングする必要があります。  
Intacct の場合、プロビジョニングは手動で行います。

### ユーザー アカウントをプロビジョニングするには、次の手順を実行します。

1.  **Intacct** テナントにログインします。

2.  **[会社]** タブをクリックし、**[ユーザー]** をクリックします。

    ![ユーザー](./media/active-directory-saas-intacct-tutorial/IC790041.png "Users")

3.  **[追加]** タブをクリックします。

    ![[追加] のいずれかを](./media/active-directory-saas-intacct-tutorial/IC790042.png "Add")

4.  **[ユーザー情報]** セクションで、次の手順を実行します。

    ![ユーザー情報](./media/active-directory-saas-intacct-tutorial/IC790043.png "User Information")

    1.  プロビジョニングする Azure AD アカウントの**ユーザー ID**、**姓**、**名**、**電子メール アドレス**、**役職**、および**電話番号**を、関連するテキストボックスに入力します。
    2.  プロビジョニングする Azure AD アカウントの**管理者特権**を選択します。
    3.  **[保存]** をクリックします。
        >[AZURE.NOTE] AAD アカウント所有者がメールを受信し、リンクに従ってアカウントを確認するとそのアカウントがアクティブになります。

>[AZURE.NOTE] Intacct から提供されている他の Intacct ユーザー アカウント作成ツールまたは API を使用して、AAD ユーザー アカウントをプロビジョニングできます。

## ユーザーの割り当て

構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

### ユーザーを Intacct に割り当てるには、次の手順を実行します。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  **Intacct * * アプリケーション統合ページで、をクリックして * * ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-intacct-tutorial/IC790044.png "Assign Users")

3.  テスト ユーザーを選択し、**[割り当て]**、**[はい]** の順にクリックして、割り当てを確定します。

    ![はい](./media/active-directory-saas-intacct-tutorial/IC767830.png "Yes")

シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。




