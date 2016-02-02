<properties 
    pageTitle="チュートリアル: Azure Active Directory と BambooHR の統合 | Microsoft Azure" 
    description="Azure Active Directory と Bamboo HR を使用して、シングル サインオン、自動化されたプロビジョニング、および詳細を有効にする方法を説明します。" 
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


# チュートリアル: Azure Active Directory と Bamboo HR の統合

このチュートリアルでは、Azure と Bamboo HR の統合について説明します。  
このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   Bamboo HR でのシングル サインオンが有効なサブスクリプション

このチュートリアルを完了すると、BambooHR に割り当てた Azure AD ユーザーは、BambooHR 会社サイト (サービス プロバイダーが開始したサインオン) で、アプリケーションにシングル サインオンまたはできるを使用して、 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。

このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  Bamboo HR のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-bamboo-hr-tutorial/IC796685.png "Scenario")
## Bamboo HR のアプリケーション統合の有効化

このセクションでは、Bamboo HR のアプリケーション統合を有効にする方法について説明します。

### Bamboo HR のアプリケーション統合を有効にするには、次の手順に従います。

1.  Azure 管理ポータルの左側のナビゲーション ウィンドウで、**[Active Directory]** をクリックします。

    ![Active Directory](./media/active-directory-saas-bamboo-hr-tutorial/IC700993.png "Active Directory")

2.  **[ディレクトリ]** の一覧から、ディレクトリ統合を有効にするディレクトリを選択します。

3.  アプリケーション ビューを開くには、ディレクトリ ビューでトップ メニューの **[アプリケーション]** をクリックします。

    ![アプリケーション](./media/active-directory-saas-bamboo-hr-tutorial/IC700994.png "Applications")

4.  ページの下部にある **[追加]** をクリックします。

    ![アプリケーションの追加](./media/active-directory-saas-bamboo-hr-tutorial/IC749321.png "Add application")

5.  **[実行する内容]** ダイアログで、**[ギャラリーからアプリケーションを追加します]** をクリックします。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-bamboo-hr-tutorial/IC749322.png "Add an application from gallerry")

6.  **[検索]** ボックスに、「**BambooHR**」と入力します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-bamboo-hr-tutorial/IC796686.png "Application gallery")

7.  結果ウィンドウで **[BambooHR]** を選択し、**[完了]** をクリックしてアプリケーションを追加します。

    ![BambooHR](./media/active-directory-saas-bamboo-hr-tutorial/IC796687.png "BambooHR")
## シングル サインオンの構成

このセクションでは、ユーザーが SAML プロトコルに基づくフェデレーションを使用して、Azure AD でのユーザーのアカウントで BambooHR に対する認証を行えるようにする方法を説明します。  
この手順の途中で、base-64 でエンコードされた証明書ファイルを作成する必要があります。  
この手順に慣れていない場合は、次を参照してください。 [バイナリ証明書をテキスト ファイルに変換する方法](http://youtu.be/PlgrzUZ-Y1o)します。

### シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **BambooHR** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、 ** [シングル サインオン ** ダイアログ。

    ![シナリオ](./media/active-directory-saas-bamboo-hr-tutorial/IC796685.png "Scenario")

2.  **どのようなユーザーの BambooHR へのアクセス** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、] をクリックし、 **次**します。

    ![シングル サインオンの構成](./media/active-directory-saas-bamboo-hr-tutorial/IC796688.png "Configure single sign-on")

3.  **アプリケーション URL の構成** ] ページで、 **BambooHR サインオン URL** ] ボックスに、型に使用する URL、ユーザーが BambooHR アプリケーションにサインオン (例:: https://company.bamboohr.com)、順にクリック **次**します。

    ![アプリケーション URL の構成](./media/active-directory-saas-bamboo-hr-tutorial/IC796689.png "Configure app URL")

4.  **BambooHR でのシングル サインオンの構成** ] ページで [ **証明書のダウンロード**, 、お使いのコンピューターに証明書ファイルを保存します。

    ![シングル サインオンの構成](./media/active-directory-saas-bamboo-hr-tutorial/IC796690.png "Configure single sign-on")

5.  別の Web ブラウザーのウィンドウで、管理者として BambooHR 企業サイトにログインします。

6.  このホーム ページで、次の手順を実行します。

    ![シングル サインオン](./media/active-directory-saas-bamboo-hr-tutorial/IC796691.png "Single Sign-On")

    1.  **[Apps]** をクリックします。
    2.  左側の [Apps] メニューで、**[Single Sign-On]** をクリックします。
    3.  **[SAML Single Sign-On]** をクリックします。

7.  **[SAML Single Sign-On]** セクションで、次の手順に従います。

    ![SAML シングル サインオン](./media/active-directory-saas-bamboo-hr-tutorial/IC796692.png "SAML Single Sign-On")

    1.  Azure ポータルで、 **BambooHR でのシングル サインオンの構成** ] ダイアログ ページでコピー、 **シングル サインオン サービス URL** 値に設定して、貼り付けます、 ** SSO ログイン URL ** ] ボックスに貼り付けます。
    2.  ダウンロードした証明書から **base-64 でエンコードされた**ファイルを作成します。
        >[AZURE.TIP] 詳細については、「 [バイナリ証明書をテキスト ファイルに変換する方法](http://youtu.be/PlgrzUZ-Y1o)

    3.  base-64 でエンコードされた証明書をメモ帳で開き、その内容をクリップボードにコピーして、**[X.509 Certificate]** テキスト ボックスに貼り付けます。
    4.  **[保存]** をクリックします。

8.  Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![シングル サインオンの構成](./media/active-directory-saas-bamboo-hr-tutorial/IC796693.png "Configure single sign-on")
## ユーザー プロビジョニングの構成

Azure AD ユーザーが BambooHR にログインできるようにするには、そのユーザーを BambooHR にプロビジョニングする必要があります。  
BambooHR の場合、プロビジョニングは手動で行います。

### ユーザー アカウントをプロビジョニングするには、次の手順に従います。

1.  **BambooHR** 企業サイトに管理者としてログインします。

2.  上部のツールバーで **[設定]** をクリックします。

    ![設定](./media/active-directory-saas-bamboo-hr-tutorial/IC796694.png "Setting")

3.  **[Overview]** をクリックします。

4.  左側のナビゲーション ウィンドウで [ **セキュリティ \ > ユーザー**します。

5.  関連するテキスト ボックスに、プロビジョニングする有効な AAD アカウントのユーザー名、パスワード、および電子メール アドレスを入力します。

6.  **[保存]** をクリックします。

>[AZURE.NOTE] 他の BambooHR ユーザー アカウントの作成ツールまたは BambooHR から提供されている API を使用して、AAD ユーザー アカウントをプロビジョニングできます。

## ユーザーの割り当て

構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

### ユーザーを BambooHR に割り当てるには、次の手順に従います。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  **BambooHR * * アプリケーション統合ページで、をクリックして * * ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-bamboo-hr-tutorial/IC796695.png "Assign users")

3.  テスト ユーザーを選択し、**[割り当て]**、**[はい]** の順にクリックして、割り当てを確定します。

    ![はい](./media/active-directory-saas-bamboo-hr-tutorial/IC767830.png "Yes")

シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。





