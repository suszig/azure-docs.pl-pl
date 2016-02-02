<properties 
    pageTitle="チュートリアル: Azure Active Directory と Picturepark の統合 | Microsoft Azure" 
    description="Azure Active Directory で Picturepark を使用して、シングル サインオンや自動プロビジョニングなどを有効にする方法について説明します。" 
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


# チュートリアル: Azure Active Directory と Picturepark の統合

このチュートリアルでは、Azure と Picturepark の統合について説明します。  
このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   Picturepark テナント

このチュートリアルを完了すると、Picturepark に割り当てた Azure AD ユーザーは、Picturepark 会社サイト (サービス プロバイダーが開始したサインオン) で、アプリケーションにシングル サインオンまたはできるを使用して、 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。

このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  Picturepark のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-picturepark-tutorial/IC795055.png "Scenario")
## Picturepark のアプリケーション統合の有効化

このセクションでは、Picturepark のアプリケーション統合を有効にする方法を説明します。

### Picturepark のアプリケーション統合を有効にするには、次の手順に従います。

1.  Azure 管理ポータルの左側のナビゲーション ウィンドウで、**[Active Directory]** をクリックします。

    ![Active Directory](./media/active-directory-saas-picturepark-tutorial/IC700993.png "Active Directory")

2.  **[ディレクトリ]** の一覧から、ディレクトリ統合を有効にするディレクトリを選択します。

3.  アプリケーション ビューを開くには、ディレクトリ ビューでトップ メニューの **[アプリケーション]** をクリックします。

    ![アプリケーション](./media/active-directory-saas-picturepark-tutorial/IC700994.png "Applications")

4.  ページの下部にある **[追加]** をクリックします。

    ![アプリケーションの追加](./media/active-directory-saas-picturepark-tutorial/IC749321.png "Add application")

5.  **[実行する内容]** ダイアログで、**[ギャラリーからアプリケーションを追加します]** をクリックします。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-picturepark-tutorial/IC749322.png "Add an application from gallerry")

6.  **検索ボックス**に「**Picturepark**」と入力します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-picturepark-tutorial/IC795056.png "Application Gallery")

7.  結果ウィンドウで **[Picturepark]** を選び、**[完了]** をクリックしてアプリケーションを追加します。

    ![Picturepark](./media/active-directory-saas-picturepark-tutorial/IC795057.png "Picturepark")
## シングル サインオンの構成

このセクションでは、ユーザーが SAML プロトコルに基づくフェデレーションを使用して、Azure AD でのユーザーのアカウントで Picturepark に対する認証を行えるようにする方法を説明します。  
Picturepark にシングル サインオンを構成するには、証明書の拇印の値を取得する必要があります。  
この手順に慣れていない場合は、次を参照してください [証明書の拇印の値を取得する方法](http://youtu.be/YKQF266SAxI).。

### シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **Picturepark** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、 ** [シングル サインオン ** ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-picturepark-tutorial/IC795058.png "Configure Single Sign-On")

2.  **どのようなユーザーの Picturepark へのアクセス** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、] をクリックし、 **次**します。

    ![Configure Single Sign-On](./media/active-directory-saas-picturepark-tutorial/IC795059.png "Configure Single Sign-On")

3.  **アプリケーション URL の構成** ] ページの [、 **Picturepark サインオン URL** ] ボックスに、次のパターンを使用して URL を入力"*http://company.picturepark.com*"、] をクリックし、 **次**します。

    ![アプリケーション URL の構成](./media/active-directory-saas-picturepark-tutorial/IC795060.png "Configure App URL")

4.  **Picturepark でのシングル サインオンの構成** ] ページで、証明書をダウンロードする] をクリックして **証明書のダウンロード**, 、お使いのコンピューターにローカルで証明書ファイルを保存します。

    ![Configure Single Sign-On](./media/active-directory-saas-picturepark-tutorial/IC795061.png "Configure Single Sign-On")

5.  別の Web ブラウザーのウィンドウで、Picturepark 企業サイトに管理者としてログインします。

6.  上部にあるツールバーで **[Administrative tools]**、**[Management Console]** の順にクリックします。

    ![管理コンソール](./media/active-directory-saas-picturepark-tutorial/IC795062.png "Management Console")

7.  **[Authentication]**、**[Identity providers]** の順にクリックします。

    ![認証](./media/active-directory-saas-picturepark-tutorial/IC795063.png "Authentication")

8.  **[Identity provider configuration]** セクションで、次の手順に従います。

    ![ID プロバイダーの構成](./media/active-directory-saas-picturepark-tutorial/IC795064.png "Identity provider configuration")

    1.  **[追加]** をクリックします。
    2.  構成の名前を入力します。
    3.  **[Set as default]** を選びます。
    4.  Azure ポータルの **[Picturepark でのシングル サインオンの構成]** ダイアログ ページで **[SAML SSO URL]** の値をコピーし、それを **[Issuer URI]** テキストボックスに貼り付けます。
    5.  エクスポートした証明書から **[拇印]** の値をコピーし、**[Trusted Issuer Thumb Print]** ボックスに貼り付けます。
        >[AZURE.TIP]詳細については、「 [証明書拇印値を取得する方法](http://youtu.be/YKQF266SAxI)

    6.  **[JoinDefaultUsersGroup]** をクリックします。
    7.  設定する、 **Emailaddress** 属性、 **クレーム** ] ボックスに「 **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**します。
        ![構成](./media/active-directory-saas-picturepark-tutorial/IC795065.png "Configuration")
    8.  **[保存]** をクリックします。

9.  Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-picturepark-tutorial/IC795066.png "Configure Single Sign-On")

## ユーザー プロビジョニングの構成

Azure AD ユーザーが Picturepark にログインできるようにするには、ユーザーを Picturepark にプロビジョニングする必要があります。  
Picturepark の場合、プロビジョニングは手動で行います。

### ユーザー アカウントをプロビジョニングするには、次の手順に従います。

1.  **Picturepark** テナントにログインします。

2.  上部にあるツールバーで **[Administrative tools]**、**[Users]** の順にクリックします。

    ![ユーザー](./media/active-directory-saas-picturepark-tutorial/IC795067.png "Users")

3.  **[Users overview]** タブで、**[New]** をクリックします。

    ![ユーザー管理](./media/active-directory-saas-picturepark-tutorial/IC795068.png "User management")

4.  **[Create User]** ダイアログで、次の手順に従います。

    ![Create User](./media/active-directory-saas-picturepark-tutorial/IC795069.png "Create User")

    1.  プロビジョニングする有効な Azure Active Directory ユーザーの次の情報を、関連するテキストボックスに入力します。 **[Email Address]**、**[Password]**、**[Confirm Password]**、**[First Name]**、**[Last Name]**、**[Company]**、**[Country]**、**[ZIP]**、**[City]**
    2.  **[Language]** を選びます。
    3.  **[作成]** をクリックします。

>[AZURE.NOTE]Picturepark から提供されている他の Picturepark ユーザー アカウント作成ツールまたは API を使用して、AAD ユーザー アカウントをプロビジョニングできます。

## ユーザーを割り当てる

構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

### ユーザーを Picturepark に割り当てるには、次の手順に従います。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  **Picturepark * * アプリケーション統合ページで、をクリックして * * ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-picturepark-tutorial/IC795070.png "Assign Users")

3.  テスト ユーザーを選択し、**[割り当て]**、**[はい]** の順にクリックして、割り当てを確定します。

    ![はい](./media/active-directory-saas-picturepark-tutorial/IC767830.png "Yes")

シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。




