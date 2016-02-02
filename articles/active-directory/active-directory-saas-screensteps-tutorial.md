<properties 
    pageTitle="チュートリアル: Azure Active Directory と ScreenSteps の統合 | Microsoft Azure" 
    description="Azure Active Directory で ScreenSteps を使用して、シングル サインオンや自動プロビジョニングなどを有効にする方法について説明します。" 
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


# チュートリアル: Azure Active Directory と ScreenSteps の統合

このチュートリアルの目的は、Azure と ScreenSteps の統合を示すことです。  
このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   ScreenSteps テナント

このチュートリアルを完了すると、ScreenSteps に割り当てた Azure AD ユーザーは、ScreenSteps の会社サイト (サービス プロバイダーが開始したサインオン) で、アプリケーションにシングル サインオンまたはできるを使用して、 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。

このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  ScreenSteps のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-screensteps-tutorial/IC778516.png "Scenario")
## ScreenSteps のアプリケーション統合の有効化

このセクションでは、ScreenSteps のアプリケーション統合を有効にする方法を説明します。

### ScreenSteps のアプリケーション統合を有効にするには、次の手順に従います。

1.  Azure 管理ポータルの左側のナビゲーション ウィンドウで、**[Active Directory]** をクリックします。

    ![Active Directory](./media/active-directory-saas-screensteps-tutorial/IC700993.png "Active Directory")

2.  **[ディレクトリ]** の一覧から、ディレクトリ統合を有効にするディレクトリを選択します。

3.  アプリケーション ビューを開くには、ディレクトリ ビューでトップ メニューの **[アプリケーション]** をクリックします。

    ![アプリケーション](./media/active-directory-saas-screensteps-tutorial/IC700994.png "Applications")

4.  ページの下部にある **[追加]** をクリックします。

    ![アプリケーションの追加](./media/active-directory-saas-screensteps-tutorial/IC749321.png "Add application")

5.  **[実行する内容]** ダイアログで、**[ギャラリーからアプリケーションを追加します]** をクリックします。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-screensteps-tutorial/IC749322.png "Add an application from gallerry")

6.  **検索ボックス**に、「**ScreenSteps**」と入力します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-screensteps-tutorial/IC778517.png "Application gallery")

7.  結果ウィンドウで **[ScreenSteps]** を選択し、**[完了]** をクリックしてアプリケーションを追加します。

    ![ScreenSteps](./media/active-directory-saas-screensteps-tutorial/IC778518.png "ScreenSteps")
## シングル サインオンの構成

このセクションでは、SAML プロトコルに基づくフェデレーションを使用して、ScreenSteps で Azure AD のユーザー アカウントを使用してユーザーを認証できるようにする方法を説明します。

### シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **ScreenSteps** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、 ** [シングル サインオン ** ダイアログ。

    ![シングル サインオンの構成](./media/active-directory-saas-screensteps-tutorial/IC778519.png "Configure single sign-on")

2.  **どのようなユーザーを ScreenSteps へのサインオン** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、] をクリックし、 **次**します。

    ![シングル サインオンの構成](./media/active-directory-saas-screensteps-tutorial/IC778520.png "Configure single sign-on")

3.  **アプリケーション URL の構成** ] ページで、 **ScreenSteps サインイン URL** ] ボックスに、次のパターンを使用して URL を入力"* https://\<tenant-name\>します。ScreenSteps.com*"、クリックして **次**します。

    ![アプリケーション URL の構成](./media/active-directory-saas-screensteps-tutorial/IC778521.png "Configure app URL")

4.  **ScreenSteps でのシングル サインオンの構成** ] ページで、証明書をダウンロードする] をクリックして **証明書のダウンロード**, 、お使いのコンピューターに証明書ファイルを保存します。

    ![シングル サインオンの構成](./media/active-directory-saas-screensteps-tutorial/IC778522.png "Configure single sign-on")

5.  別の Web ブラウザー ウィンドウで、ScreenSteps 企業サイトに管理者としてログインします。

6.  **[Account Management]** をクリックします。

    ![アカウント管理](./media/active-directory-saas-screensteps-tutorial/IC778523.png "Account management")

7.  **[Remote Authentication]** をクリックします。

    ![リモート認証](./media/active-directory-saas-screensteps-tutorial/IC778524.png "Remote authentication")

8.  **[Create authentication endpoint]** をクリックします。

    ![リモート認証](./media/active-directory-saas-screensteps-tutorial/IC778525.png "Remote authentication")

9.  **[Create an Authentication Endpoint]** セクションで、次の手順に従います。

    ![認証エンドポイントの作成](./media/active-directory-saas-screensteps-tutorial/IC778526.png "Create an authentication endpoint")

    1.  **[Title]** テキスト ボックスに、タイトルを入力します。
    2.  **[Mode]** の一覧から **[SAML]** を選択します。
    3.  **[作成]** をクリックします。

10. **[Remote Authentication Endpoint]** セクションで、次の手順に従います。

    ![リモート認証エンドポイント](./media/active-directory-saas-screensteps-tutorial/IC778527.png "Remote authentication endpoint")

    1.  Azure AD ポータルでの **ScreenSteps でのシングル サインオンの構成** ] ページで、コピー、 **リモート ログイン URL** 値に設定して、貼り付けます、 **リモート ログイン URL** ] ボックスに貼り付けます。
    2.  Azure AD ポータルでの **ScreenSteps でのシングル サインオンの構成** ] ページで、コピー、 **リモート ログアウト URL** 値に設定して、貼り付けます、 **ログアウト URL** ] ボックスに貼り付けます。
    3.  **[Choose a file]** をクリックし、ダウンロードした証明書をアップロードします。
    4.  **[Update]** をクリックします。

11. Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![シングル サインオンの構成](./media/active-directory-saas-screensteps-tutorial/IC778542.png "Configure single sign-on")
## ユーザー プロビジョニングの構成

Azure AD ユーザーが **ScreenSteps** にログインできるようにするには、ユーザーを **ScreenSteps** にプロビジョニングする必要があります。  
**ScreenSteps** の場合、プロビジョニングは手動で行います。

### ユーザー アカウントを ScreenSteps にプロビジョニングするには、次の手順に従います。

1.  **ScreenSteps** テナントにログインします。

2.  **[Account Management]** をクリックします。

    ![アカウント管理](./media/active-directory-saas-screensteps-tutorial/IC778523.png "Account management")

3.  **[ユーザー]** をクリックします。

    ![ユーザー](./media/active-directory-saas-screensteps-tutorial/IC778544.png "Users")

4.  **[Create a user]** をクリックします。

    ![すべてのユーザー](./media/active-directory-saas-screensteps-tutorial/IC778545.png "All Users")

5.  **[User Role]** の一覧からユーザーのロールを選択します。

6.  ユーザー ロール] セクションで、入力、"**名**, 、**姓、名**, 、**電子メール**, 、**ログイン**, 、**パスワード** と **パスワードの確認入力**"関連テキスト ボックスに、プロビジョニングする有効な AAD アカウントのです。

    ![新しいユーザー](./media/active-directory-saas-screensteps-tutorial/IC778546.png "New user")

7.  [Groups] セクションで、**[Authentication Group (SAML)]** を選択し、**[Create User]** をクリックします。

    ![グループ](./media/active-directory-saas-screensteps-tutorial/IC778547.png "Groups")

>[AZURE.NOTE]ScreenSteps から提供されている他の ScreenSteps ユーザー アカウント作成ツールまたは  API を使用して、AAD ユーザー アカウントをプロビジョニングできます。

## ユーザーの割り当て

構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

### ユーザーを ScreenSteps に割り当てるには、次の手順に従います。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  **ScreenSteps * * アプリケーション統合ページで、をクリックして * * ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-screensteps-tutorial/IC773094.png "Assign users")

3.  テスト ユーザーを選択し、**[割り当て]**、**[はい]** の順にクリックして、割り当てを確定します。

    ![ユーザーの割り当て](./media/active-directory-saas-screensteps-tutorial/IC778548.png "Assign users")

シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。




