<properties 
    pageTitle="チュートリアル: Azure Active Directory と Gigya の統合 | Microsoft Azure" 
    description="Azure Active Directory で Gigya を使用して、シングル サインオンや自動プロビジョニングなどを有効にする方法について説明します。" 
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
    ms.date="10/20/2015" 
    ms.author="markvi" />


# チュートリアル: Azure Active Directory と Gigya の統合

このチュートリアルでは、Azure と Gigya の統合について説明します。  
このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   Gigya でのシングル サインオンが有効なサブスクリプション

このチュートリアルを完了すると、Gigya に割り当てた Azure AD ユーザーは、Gigya 会社サイト (サービス プロバイダーが開始したサインオン) で、アプリケーションにシングル サインオンまたはできるを使用して、 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。

このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  Gigya のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![Configure Single Sign-On](./media/active-directory-saas-gigya-tutorial/IC789512.png "Configure Single Sign-On")
## Gigya のアプリケーション統合の有効化

このセクションでは、Gigya のアプリケーション統合を有効にする方法について説明します。

### Gigya のアプリケーション統合を有効にするには、次の手順を実行します。

1.  Azure 管理ポータルの左側のナビゲーション ウィンドウで、**[Active Directory]** をクリックします。

    ![Active Directory](./media/active-directory-saas-gigya-tutorial/IC700993.png "Active Directory")

2.  **[ディレクトリ]** の一覧から、ディレクトリ統合を有効にするディレクトリを選択します。

3.  アプリケーション ビューを開くには、ディレクトリ ビューでトップ メニューの **[アプリケーション]** をクリックします。

    ![アプリケーション](./media/active-directory-saas-gigya-tutorial/IC700994.png "Applications")

4.  ページの下部にある **[追加]** をクリックします。

    ![アプリケーションの追加](./media/active-directory-saas-gigya-tutorial/IC749321.png "Add application")

5.  **[実行する内容]** ダイアログで、**[ギャラリーからアプリケーションを追加します]** をクリックします。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-gigya-tutorial/IC749322.png "Add an application from gallerry")

6.  **検索ボックス**に、「**Gigya**」と入力します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-gigya-tutorial/IC789513.png "Application Gallery")

7.  結果ウィンドウで **[Gigya]** を選択し、**[完了]** をクリックしてアプリケーションを追加します。

    ![Gigya](./media/active-directory-saas-gigya-tutorial/IC789527.png "Gigya")
## シングル サインオンの構成

このセクションでは、ユーザーが SAML プロトコルに基づくフェデレーションを使用して、Azure AD でのユーザーのアカウントで Gigya に対する認証を行うことができるようにする方法を説明します。  
この手順の途中で、base-64 でエンコードされた証明書ファイルを作成する必要があります。  
この手順に慣れていない場合は、次を参照してください。 [バイナリ証明書をテキスト ファイルに変換する方法](http://youtu.be/PlgrzUZ-Y1o)します。

### シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **Gigya** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、 ** [シングル サインオン ** ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-gigya-tutorial/IC789528.png "Configure Single Sign-On")

2.  **どのようなユーザーが Gigya へのサインオン** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、順にクリック **次**します。

    ![Configure Single Sign-On](./media/active-directory-saas-gigya-tutorial/IC789529.png "Configure Single Sign-On")

3.  **アプリケーション URL の構成** ] ページの [、 **Gigya サインオン URL** ] ボックスに、次のパターンを使用して URL を入力"*http://company.gigya.com*"、] をクリックし、 **次**します。

    ![アプリケーション URL の構成](./media/active-directory-saas-gigya-tutorial/IC789530.png "Configure App URL")

4.  **Gigya でのシングル サインオンの構成** ] ページで [ **証明書のダウンロード**, 、お使いのコンピューターに証明書ファイルを保存します。

    ![Configure Single Sign-On](./media/active-directory-saas-gigya-tutorial/IC789531.png "Configure Single Sign-On")

5.  別の Web ブラウザー ウィンドウで、Gigya 企業サイトに管理者としてログインします。

6.  移動して **設定 \ > SAML ログイン**, 、順にクリックし、 **追加** ] ボタンをクリックします。

    ![SAML のログイン](./media/active-directory-saas-gigya-tutorial/IC789532.png "SAML Login")

7.  **[SAML ログイン]** セクションで、次の手順を実行します。

    ![SAML の構成](./media/active-directory-saas-gigya-tutorial/IC789533.png "SAML Configuration")

    1.  **[名前]** テキスト ボックスに、構成の名前を入力します。
    2.  Azure ポータルの **[Gigya でのシングル サインオンの構成]** ダイアログ ページで **[発行者の URL]** の値をコピーし、それを **[発行者]** テキストボックスに貼り付けます。
    3.  Azure ポータルの **[Gigya でのシングル サインオンの構成]** ダイアログ ページで、**[シングル サインオン サービス URL]** の値をコピーし、**[シングル サインオン サービス URL]** テキストボックスに貼り付けます。
    4.  Azure ポータルの **[Gigya でのシングル サインオンの構成]** ダイアログ ページで、**[名前識別子形式]** の値をコピーし、**[名前識別子形式]** テキストボックスに貼り付けます。
    5.  ダウンロードした証明書から **base-64 でエンコードされた**ファイルを作成します。
        >[AZURE.TIP]詳細については、「 [バイナリ証明書をテキスト ファイルに変換する方法](http://youtu.be/PlgrzUZ-Y1o)

    6.  base-64 でエンコードされた証明書をメモ帳で開き、その内容をクリップボードにコピーして、**[X.509 証明書]** テキストボックスに貼り付けます。
    7.  **[Save Settings]** をクリックします。

8.  Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-gigya-tutorial/IC789534.png "Configure Single Sign-On")
## ユーザー プロビジョニングの構成

Azure AD ユーザーが Gigya にログインできるようにするには、ユーザーを Gigya にプロビジョニングする必要があります。  
Gigya の場合、プロビジョニングは手動で行います。

### ユーザー アカウントをプロビジョニングするには、次の手順を実行します。

1.  **Gigya** 企業サイトに管理者としてログインします。

2.  移動して **Admin \ > ユーザーの管理**, 、] をクリックし、 **ユーザーの招待**します。

    ![Manage Users](./media/active-directory-saas-gigya-tutorial/IC789535.png "Manage Users")

3.  [ユーザーの招待] ダイアログで、次の手順を実行します。

    ![ユーザーの招待](./media/active-directory-saas-gigya-tutorial/IC789536.png "Invite Users")

    1.  **[電子メール]** テキスト ボックスに、プロビジョニングする有効な Azure Active Directory アカウントの電子メール エイリアスを入力します。
    2.  **[ユーザーの招待]** をクリックします。
        >[AZURE.NOTE] Azure Active Directory のアカウント所有者には、アカウントがアクティブになる前に、アカウント確認用のリンクを含む電子メールが送信されます。

>[AZURE.NOTE] Gigya から提供されている他の Gigya ユーザー アカウント作成ツールまたは API を使用して、AAD ユーザー アカウントをプロビジョニングできます。

## ユーザーの割り当て

構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

### ユーザーを Gigya に割り当てるには、次の手順を実行します。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  **Gigya * * アプリケーション統合ページで、をクリックして * * ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-gigya-tutorial/IC789537.png "Assign Users")

3.  テスト ユーザーを選択し、**[割り当て]**、**[はい]** の順にクリックして、割り当てを確定します。

    ![はい](./media/active-directory-saas-gigya-tutorial/IC767830.png "Yes")

シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。




