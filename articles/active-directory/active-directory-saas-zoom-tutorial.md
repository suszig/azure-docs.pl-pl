<properties 
    pageTitle="チュートリアル: Azure Active Directory と Zoom の統合 | Microsoft Azure" 
    description="Azure Active Directory で Zoom を使用して、シングル サインオンや自動プロビジョニングなどを有効にする方法について説明します。" 
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


# チュートリアル: Azure Active Directory と Zoom の統合

このチュートリアルでは、Azure と Zoom の統合について説明します。  
このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   Zoom のテナント

このチュートリアルを完了すると、Zoom に割り当てた Azure AD ユーザーができるよう、Zoom 会社サイト (サービス プロバイダーが開始したサインオン)、または、を使用して、アプリケーションにシングル サインオン [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)

このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  Zoom のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-zoom-tutorial/IC784693.png "Scenario")

## Zoom のアプリケーション統合の有効化

このセクションでは、Zoom のアプリケーション統合を有効にする方法について説明します。

### Zoom のアプリケーション統合を有効にするには、次の手順を実行します。

1.  Azure 管理ポータルの左側のナビゲーション ウィンドウで、**[Active Directory]** をクリックします。

    ![Active Directory](./media/active-directory-saas-zoom-tutorial/IC700993.png "Active Directory")

2.  **[ディレクトリ]** の一覧から、ディレクトリ統合を有効にするディレクトリを選択します。

3.  アプリケーション ビューを開くには、ディレクトリ ビューでトップ メニューの **[アプリケーション]** をクリックします。

    ![アプリケーション](./media/active-directory-saas-zoom-tutorial/IC700994.png "Applications")

4.  ページの下部にある **[追加]** をクリックします。

    ![アプリケーションの追加](./media/active-directory-saas-zoom-tutorial/IC749321.png "Add application")

5.  **[実行する内容]** ダイアログで、**[ギャラリーからアプリケーションを追加します]** をクリックします。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-zoom-tutorial/IC749322.png "Add an application from gallerry")

6.  **検索ボックス**に、「**Zoom**」と入力します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-zoom-tutorial/IC784694.png "Application Gallery")

7.  結果ウィンドウで **[Zoom]** を選択し、**[完了]** をクリックしてアプリケーションを追加します。

    ![Zoom](./media/active-directory-saas-zoom-tutorial/IC784695.png "Zoom")

## シングル サインオンの構成

このセクションでは、ユーザーが SAML プロトコルに基づくフェデレーションを使用して、Azure AD でのユーザーのアカウントで Zoom に対する認証を行うことができるようにする方法を説明します。  
この手順の途中で、base-64 でエンコードされた証明書ファイルを作成する必要があります。  
この手順に慣れていない場合は、次を参照してください。 [バイナリ証明書をテキスト ファイルに変換する方法](http://youtu.be/PlgrzUZ-Y1o)します。

### シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **ズーム** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、 ** [シングル サインオン ** ダイアログ。

    ![シングル サインオンの構成](./media/active-directory-saas-zoom-tutorial/IC784696.png "Configure single sign-on")

2.  **どのようなズームをサインオン** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、順にクリック **次**します。

    ![シングル サインオンの構成](./media/active-directory-saas-zoom-tutorial/IC784697.png "Configure single sign-on")

3.  **アプリケーション URL の構成** ] ページの [、 **Zoom サインイン URL** ] ボックスに、次のパターンを使用して URL を入力"*http://company.zoom.us*"、] をクリックし、 **次**します。

    ![アプリケーション URL の構成](./media/active-directory-saas-zoom-tutorial/IC784698.png "Configure App URL")

4.  **Zoom でのシングル サインオン構成** ] ページで [ **証明書のダウンロード**, 、コンピューターに証明書ファイルを保存します。

    ![シングル サインオンの構成](./media/active-directory-saas-zoom-tutorial/IC784699.png "Configure single sign-on")

5.  別の Web ブラウザー ウィンドウで、Zoom 企業サイトに管理者としてログインします。

6.  **[シングル サインオン]** タブをクリックします。

    ![シングル サインオン](./media/active-directory-saas-zoom-tutorial/IC784700.png "Single sign-on")

7.  **[セキュリティ制御]** タブをクリックし、**[シングル サインオン]** に移動します。

8.  [Single Sign-On] セクションで、次の手順に従います。

    ![シングル サインオン](./media/active-directory-saas-zoom-tutorial/IC784701.png "Single sign-on")

    1.  Azure ポータルの **[Zoom でのシングル サインオンの構成]** ダイアログ ページで、**[シングル サインオン サービス URL]** の値をコピーし、**[サインイン ページ URL]** テキストボックスに貼り付けます。
    2.  Azure ポータルの **[Zoom でのシングル サインオンの構成]** ダイアログ ページで、**[シングル サインアウト サービス URL]** の値をコピーし、**[サインアウト ページ URL]** テキストボックスに貼り付けます。
    3.  ダウンロードした証明書から **base-64 でエンコードされた**ファイルを作成します。
        >[AZURE.TIP] 詳細については、「 [バイナリ証明書をテキスト ファイルに変換する方法](http://youtu.be/PlgrzUZ-Y1o)

    4.  base-64 でエンコードされた証明書をメモ帳で開き、その内容をクリップボードにコピーして、**[ID プロバイダー証明書]** テキスト ボックスに貼り付けます。
    5.  Azure ポータルの **[Zoom でのシングル サインオンの構成]** ダイアログ ページで **[発行者の URL]** の値をコピーし、それを **[発行者]** テキストボックスに貼り付けます。
    6.  **[保存]** をクリックします。

9.  Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![シングル サインオンの構成](./media/active-directory-saas-zoom-tutorial/IC784702.png "Configure single sign-on")

## ユーザー プロビジョニングの構成

Azure AD ユーザーが Zoom にログインできるようにするには、ユーザーを Zoom にプロビジョニングする必要があります。  
Zoom の場合、プロビジョニングは手動で行います。

### ユーザー アカウントをプロビジョニングするには、次の手順を実行します。

1.  **Zoom** 企業サイトに管理者としてログインします。

2.  **[アカウント管理]** タブをクリックし、**[ユーザー管理]** をクリックします。

3.  [ユーザー管理] セクションで、**[ユーザーの追加]** をクリックします。

    ![ユーザー管理](./media/active-directory-saas-zoom-tutorial/IC784703.png "User management")

4.  **[ユーザーの追加]** ページで、次の手順を実行します。

    ![ユーザーの追加](./media/active-directory-saas-zoom-tutorial/IC784704.png "Add users")

    1.  **[ユーザー タイプ]** として、**[基本]** を選択します。
    2.  **[電子メール]** テキストボックスに、プロビジョニングする有効な AAD アカウントの電子メール アドレスを入力します。
    3.  **[追加]** をクリックします。

>[AZURE.NOTE] Zoom から提供されている他の Zoom ユーザー アカウント作成ツールまたは API を使用して、AAD ユーザー アカウントをプロビジョニングできます。

## ユーザーの割り当て

構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

### ユーザーを Zoomｖに割り当てるには、次の手順を実行します。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  **ズーム * * アプリケーション統合ページで、をクリックして * * ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-zoom-tutorial/IC784705.png "Assign users")

3.  テスト ユーザーを選択し、**[割り当て]**、**[はい]** の順にクリックして、割り当てを確定します。

    ![はい](./media/active-directory-saas-zoom-tutorial/IC767830.png "Yes")

シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。




