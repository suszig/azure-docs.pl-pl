<properties 
    pageTitle="チュートリアル: Azure Active Directory と Bonus.ly の統合 | Microsoft Azure" 
    description="Azure Active Directory と Bonus.ly を使用して、シングル サインオン、自動化されたプロビジョニング、および詳細を有効にする方法を説明します。" 
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


# チュートリアル: Azure Active Directory と Bonus.ly の統合

このチュートリアルでは、Azure と Bonus.ly の統合について説明します。 このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   Bonus.ly のテスト テナント

このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  Bonus.ly のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-bonus-tutorial/IC773679.png "Scenario")
## Bonus.ly のアプリケーション統合の有効化

このセクションでは、Bonus.ly のアプリケーション統合を有効にする方法について説明します。

### Bonus.ly のアプリケーション統合を有効にするには、次の手順を実行します。

1.  Azure 管理ポータルの左側のナビゲーション ウィンドウで、**[Active Directory]** をクリックします。

    ![シングル サインオンの有効化](./media/active-directory-saas-bonus-tutorial/IC773680.png "Enable single sign-on")

2.  **[ディレクトリ]** の一覧から、ディレクトリ統合を有効にするディレクトリを選択します。

3.  アプリケーション ビューを開くには、ディレクトリ ビューでトップ メニューの **[アプリケーション]** をクリックします。

    ![アプリケーション](./media/active-directory-saas-bonus-tutorial/IC700994.png "Applications")

4.  ページの下部にある **[追加]** をクリックします。

    ![アプリケーションの追加](./media/active-directory-saas-bonus-tutorial/IC749321.png "Add application")

5.  **[実行する内容]** ダイアログで、**[ギャラリーからアプリケーションを追加します]** をクリックします。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-bonus-tutorial/IC749322.png "Add an application from gallerry")

6.  **検索ボックス**に、「**Bonus.ly**」と入力します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-bonus-tutorial/IC773681.png "Application gallery")

7.  結果ウィンドウで **[Bonus.ly]** を選択し、**[完了]** をクリックしてアプリケーションを追加します。

    ![Bonusly](./media/active-directory-saas-bonus-tutorial/IC773682.png "Bonusly")
## シングル サインオンの構成

このセクションでは、SAML プロトコルに基づくフェデレーションを使用して、ユーザーが Azure AD のアカウントで Bonus.ly に対する認証を行えるようにする方法を説明します。  
Bonus.ly のシングル サインオンを構成するには、証明書からサムプリント値を取得する必要があります。  
この手順に慣れていない場合は、次を参照してください。 [証明書の拇印の値を取得する方法](http://youtu.be/YKQF266SAxI)します。

### シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **Bonus.ly** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、 ** [シングル サインオン ** ダイアログ。

    ![シングル サインオンの構成](./media/active-directory-saas-bonus-tutorial/IC749323.png "Configure single sign-on")

2.  **どのようなユーザーを Bonus.ly にサインオンする** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、] をクリックし、 **次**します。

    ![シングル サインオンの構成](./media/active-directory-saas-bonus-tutorial/IC773683.png "Configure single sign-on")

3.  **アプリケーション URL の構成** ] ページで、 **Bonus.ly テナント URL** ] ボックスに、次のパターンを使用して URL を入力"* https://\<tenant-name\>します。Bonus.ly*"をクリックして **次**:

    ![アプリケーション URL の構成](./media/active-directory-saas-bonus-tutorial/IC773684.png "Configure app URL")

4.  **Bonus.ly でのシングル サインオンの構成** ] ページで [ **証明書のダウンロード**, 、としては、証明書ファイルをローカルに保存および **c:\\Bonusly.cer**します。

    ![シングル サインオンの構成](./media/active-directory-saas-bonus-tutorial/IC773685.png "Configure single sign-on")

5.  別のブラウザー ウィンドウで、**Bonus.ly** テナントにログインします。

6.  上部のツール バーの **[Settings]** をクリックし、**[Integrations and apps]** を選択します。

    ![Bonusly](./media/active-directory-saas-bonus-tutorial/IC773686.png "Bonusly")

7.  **[Single Sign-On]** の **[SAML]** を選択します。

8.  **[SAML]** ダイアログ ページで、次の手順を実行します。

    ![Bonusly](./media/active-directory-saas-bonus-tutorial/IC773687.png "Bonusly")

    1.  Azure ポータルで、**[Bonus.ly でのシングル サインオンの構成]** ダイアログ ページの **[リモート ログイン URL]** の値をコピーし、**[IdP SSO target URL]** ボックスに貼り付けます。
    2.  Azure ポータルで、**[Bonus.ly でのシングル サインオンの構成]** ダイアログ ページの **[発行者 ID]** の値をコピーし、**[IdP Issuer]** ボックスに貼り付けます。
    3.  Azure ポータルで、**[Bonus.ly でのシングル サインオンの構成]** ダイアログ ページの **[リモート ログイン URL]** の値をコピーし、**[IdP Login URL]** ボックスに貼り付けます。
    4.  エクスポートした証明書から **[サムプリント]** の値をコピーし、**[Cert Fingerprint]** ボックスに貼り付けます。
        >[AZURE.TIP] 詳細については、「 [証明書拇印値を取得する方法](http://youtu.be/YKQF266SAxI)

9.  **[Save]** をクリックします。

10. Microsoft Azure AD ポータルで、構成確認を選択し、**[完了]** をクリックして **[シングル サインオンの構成]** ダイアログを閉じます。

    ![シングル サインオンの構成](./media/active-directory-saas-bonus-tutorial/IC773689.png "Configure single sign-on")
## ユーザー プロビジョニングの構成

Azure AD ユーザーが Bonus.ly にログインできるようにするには、ユーザーを Bonus.ly にプロビジョニングする必要があります。  
Bonus.ly の場合、プロビジョニングは手動で行います。

### ユーザー プロビジョニングを構成するには、次の手順に従います。

1.  Web ブラウザー ウィンドウで、Bonus.ly テナントにログインします。

2.  **[設定]** をクリックします

    ![設定](./media/active-directory-saas-bonus-tutorial/IC781041.png "Settings")

3.  **[Users and bonuses]** タブをクリックします。

    ![Users and bonuses](./media/active-directory-saas-bonus-tutorial/IC781042.png "Users and bonuses")

4.  **[Manage Users]** をクリックします。

    ![Manage Users](./media/active-directory-saas-bonus-tutorial/IC781043.png "Manage Users")

5.  **[ユーザーの追加]** をクリックします。

    ![ユーザーの追加](./media/active-directory-saas-bonus-tutorial/IC781044.png "Add User")

6.  **[Add User]** ダイアログで、次の手順を実行します。

    ![ユーザーの追加](./media/active-directory-saas-bonus-tutorial/IC781045.png "Add User")

    1.  型、"**電子メール**, 、**名**, 、**姓、名**"関連テキスト ボックスに、プロビジョニングする有効な AAD アカウントのです。
    2.  **[保存]** をクリックします。

    >[AZURE.NOTE] アカウントがアクティブになる前に、AAD アカウント所有者に、アカウント確認用のリンクを含む電子メールが送信されます。

>[AZURE.NOTE] Bonus.ly から提供されている他の Bonus.ly ユーザー アカウント作成ツールまたは API を使用して、AAD ユーザー アカウントをプロビジョニングできます。

## ユーザーの割り当て

構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、ユーザーにアプリケーションへのアクセス権を付与する必要があります。

### ユーザーを Bonus.ly に割り当てるには、次の手順を実行します。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  Bonus.ly アプリケーション統合ページでクリックして **ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-bonus-tutorial/IC773690.png "Assign users")

3.  テスト ユーザーを選択し、**[割り当て]**、**[はい]** の順にクリックして、割り当てを確定します。

    ![はい](./media/active-directory-saas-bonus-tutorial/IC767830.png "Yes")

シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。





