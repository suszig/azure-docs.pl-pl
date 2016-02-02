<properties 
    pageTitle="チュートリアル: Azure Active Directory と Kudos の統合 | Microsoft Azure" 
    description="Azure Active Directory と Kudos を使用して、シングル サインオン、自動化されたプロビジョニング、および詳細を有効にする方法を説明します。" 
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


# チュートリアル: Azure Active Directory と Kudos の統合

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
## Kudos のアプリケーション統合の有効化

このセクションでは、Kudos のアプリケーション統合を有効にする方法について説明します。

### Kudos のアプリケーション統合を有効にするには、次の手順に従います。

1.  Azure 管理ポータルの左側のナビゲーション ウィンドウで、**[Active Directory]** をクリックします。

    ![Active Directory](./media/active-directory-saas-kudos-tutorial/IC700993.png "Active Directory")

2.  **[ディレクトリ]** の一覧から、ディレクトリ統合を有効にするディレクトリを選択します。

3.  アプリケーション ビューを開くには、ディレクトリ ビューでトップ メニューの **[アプリケーション]** をクリックします。

    ![アプリケーション](./media/active-directory-saas-kudos-tutorial/IC700994.png "Applications")

4.  ページの下部にある **[追加]** をクリックします。

    ![アプリケーションの追加](./media/active-directory-saas-kudos-tutorial/IC749321.png "Add application")

5.  **[実行する内容]** ダイアログで、**[ギャラリーからアプリケーションを追加します]** をクリックします。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-kudos-tutorial/IC749322.png "Add an application from gallerry")

6.  **検索ボックス**に、「**Kudos**」と入力します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-kudos-tutorial/IC787800.png "Application Gallery")

7.  結果ウィンドウで **[Kudos]** を選択し、**[完了]** をクリックしてアプリケーションを追加します。

    ![Kudos](./media/active-directory-saas-kudos-tutorial/IC787801.png "Kudos")
## シングル サインオンの構成

このセクションでは、ユーザーが SAML プロトコルに基づくフェデレーションを使用して、Azure AD でのユーザーのアカウントで Kudos に対する認証を行えるようにする方法を説明します。  
この手順の途中で、base-64 でエンコードされた証明書ファイルを作成する必要があります。  
この手順に慣れていない場合は、次を参照してください。 [バイナリ証明書をテキスト ファイルに変換する方法](http://youtu.be/PlgrzUZ-Y1o)します。

### シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **Kudos** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、 ** [シングル サインオン ** ダイアログ。

    ![シングル サインオンの構成](./media/active-directory-saas-kudos-tutorial/IC787802.png "Configure single sign-on")

2.  **どのようなユーザーの Kudos に** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、] をクリックし、 **次**します。

    ![シングル サインオンの構成](./media/active-directory-saas-kudos-tutorial/IC787803.png "Configure single sign-on")

3.  **アプリケーション URL の構成** ] ページの [、 **Kudos サインオン URL** ] ボックスに、次のパターンを使用して URL を入力"*https://company.kudosnow.com*"、] をクリックし、 **次**します。

    ![アプリケーション URL の構成](./media/active-directory-saas-kudos-tutorial/IC787804.png "Configure App URL")

4.  **Kudos でのシングル サインオンの構成** ] ページで [ **証明書のダウンロード**, 、お使いのコンピューターに証明書ファイルを保存します。

    ![シングル サインオンの構成](./media/active-directory-saas-kudos-tutorial/IC787805.png "Configure single sign-on")

5.  別の Web ブラウザーのウィンドウで、Kudos の企業サイトに管理者としてログインします。

6.  上部のメニューで **[Settings]** をクリックします。

    ![設定](./media/active-directory-saas-kudos-tutorial/IC787806.png "Settings")

7.  クリックして **インテグレーション \ > SSO**します。

8.  **[SSO]** セクションで、次の手順に従います。

    ![SSO](./media/active-directory-saas-kudos-tutorial/IC787807.png "SSO")

    1.  Azure ポータルで、 **Kudos でのシングル サインオンの構成** ] ダイアログ ページでコピー、 **シングル サインオン サービス URL** 値に設定して、貼り付けます、 ** サインオン URL ** ] ボックスに貼り付けます。
    2.  ダウンロードした証明書から **base-64 でエンコードされた**ファイルを作成します。
        >[AZURE.TIP]
        詳細については、「 [バイナリ証明書をテキスト ファイルに変換する方法](http://youtu.be/PlgrzUZ-Y1o)

    3.  base-64 でエンコードされた証明書をメモ帳で開き、その内容をクリップボードにコピーして、**[X.509 証明書]** テキストボックスに貼り付けます。
    4.  Azure ポータルで、 **Kudos でのシングル サインオンの構成** ] ダイアログ ページでコピー、 **シングル サインアウト サービス URL** 値に設定して、貼り付けます、 ** Logout To URL ** ] ボックスに貼り付けます。
    5.  **[Your Kudos URL (Kudos の URL)]** テキストボックスに、企業名を入力します。
    6.  **[保存]** をクリックします。

9.  Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![シングル サインオンの構成](./media/active-directory-saas-kudos-tutorial/IC787808.png "Configure single sign-on")
## ユーザー プロビジョニングの構成

Azure AD ユーザーが Kudos にログインできるようにするには、そのユーザーを Kudos にプロビジョニングする必要があります。  
Kudos の場合、プロビジョニングは手動で行います。

### ユーザー アカウントをプロビジョニングするには、次の手順に従います。

1.  **Kudos** の企業サイトに管理者としてログインします。

2.  上部のメニューで **[Settings]** をクリックします。

    ![設定](./media/active-directory-saas-kudos-tutorial/IC787806.png "Settings")

3.  **[ユーザー管理者]** をクリックします。

4.  **[ユーザー]** タブをクリックして、**[ユーザーの追加]** をクリックします。

    ![ユーザー管理者](./media/active-directory-saas-kudos-tutorial/IC787809.png "User Admin")

5.  **[ユーザーの追加]** セクションで、次の手順に従います。

    ![ユーザーの追加](./media/active-directory-saas-kudos-tutorial/IC787810.png "Add a User")

    1.  プロビジョニングする有効な Azure Active Directory アカウントの**[名]**、**[姓]**、**[メール]**、その他の詳細を該当するボックスに入力します。
    2.  **[ユーザーの作成]** をクリックします。

>[AZURE.NOTE]Kudos から提供されている他の Canvas ユーザー アカウント作成ツールや API を使用して、AAD ユーザー アカウントをプロビジョニングできます。

## ユーザーを割り当てる

構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

### ユーザーを Kudos に割り当てるには、次の手順に従います。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  **Kudos * * アプリケーション統合ページで、をクリックして * * ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-kudos-tutorial/IC787811.png "Assign users")

3.  テスト ユーザーを選択し、**[割り当て]**、**[はい]** の順にクリックして、割り当てを確定します。

    ![はい](./media/active-directory-saas-kudos-tutorial/IC767830.png "Yes")

シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。




