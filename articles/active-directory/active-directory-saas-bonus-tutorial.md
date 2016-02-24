<properties 
    pageTitle="チュートリアル: Azure Active Directory と Bonus.ly の統合 | Microsoft Azure" 
    description="Azure Active Directory で Bonus.ly を使用して、シングル サインオンや自動プロビジョニングなどを有効にする方法について説明します。" 
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

#チュートリアル: Azure Active Directory と Bonus.ly の統合

このチュートリアルでは、Azure と Bonus.ly の統合について説明します。 このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   Bonus.ly のテスト テナント

このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  Bonus.ly のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-bonus-tutorial/IC773679.png "Scenario")
##Bonus.ly のアプリケーション統合の有効化

このセクションでは、Bonus.ly のアプリケーション統合を有効にする方法について説明します。

###Bonus.ly のアプリケーション統合を有効にするには、次の手順を実行します。

1.  左側のナビゲーション ウィンドウで、Azure 管理ポータルで、クリックして **Active Directory**します。

    ![Enable single sign-on](./media/active-directory-saas-bonus-tutorial/IC773680.png "Enable single sign-on")

2.   **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3.  ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。

    ![アプリケーション](./media/active-directory-saas-bonus-tutorial/IC700994.png "Applications")

4.  クリックして **追加** ページの下部にあります。

    ![アプリケーションの追加](./media/active-directory-saas-bonus-tutorial/IC749321.png "Add application")

5.   **実行する操作** ダイアログ ボックスで、をクリックして **ギャラリーからアプリケーションを追加**します。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-bonus-tutorial/IC749322.png "Add an application from gallerry")

6.   **検索ボックス**, 、型 **Bonus.ly**します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-bonus-tutorial/IC773681.png "Application gallery")

7.  結果ウィンドウで [ **Bonus.ly**, 、クリックして **完了** アプリケーションを追加します。

    ![Bonusly](./media/active-directory-saas-bonus-tutorial/IC773682.png "Bonusly")
##シングル サインオンの構成

このセクションでは、SAML プロトコルに基づくフェデレーションを使用して、ユーザーが Azure AD のアカウントで Bonus.ly に対する認証を行えるようにする方法を説明します。  
Bonus.ly のシングル サインオンを構成するには、証明書からサムプリント値を取得する必要があります。  
この手順に慣れていない場合は、次を参照してください。 [証明書の拇印の値を取得する方法](http://youtu.be/YKQF266SAxI)します。

###シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **Bonus.ly** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、* * [シングル サインオン * * ダイアログ。

    ![Configure single sign-on](./media/active-directory-saas-bonus-tutorial/IC749323.png "Configure single sign-on")

2.   **どのようなユーザーを Bonus.ly にサインオンする** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、] をクリックし、 **次**します。

    ![Configure single sign-on](./media/active-directory-saas-bonus-tutorial/IC773683.png "Configure single sign-on")

3.   **アプリケーション URL の構成** ] ページで、 **Bonus.ly テナント URL** ] ボックスに、次のパターンを使用して URL を入力"*https://\<tenant-name\>。Bonus.ly*"をクリックして **次**: 

    ![アプリケーション URL の構成](./media/active-directory-saas-bonus-tutorial/IC773684.png "Configure app URL")

4.   **Bonus.ly でのシングル サインオンの構成** ] ページで [ **証明書のダウンロード**, 、としては、証明書ファイルをローカルに保存および **c:\\Bonusly.cer**します。

    ![Configure single sign-on](./media/active-directory-saas-bonus-tutorial/IC773685.png "Configure single sign-on")

5.  別のブラウザー ウィンドウ内にログインして **Bonus.ly** テナントです。

6.  上部にあるツールバーで、クリックして **設定**, 、し、[ **Integrations and apps**します。

    ![Bonusly](./media/active-directory-saas-bonus-tutorial/IC773686.png "Bonusly")

7.   **でのシングル サインオン**, [ **SAML**します。

8.   **SAML** ] ダイアログ ページで、次の手順を実行します。

    ![Bonusly](./media/active-directory-saas-bonus-tutorial/IC773687.png "Bonusly")

    1.  Azure ポータルで、 **Bonus.ly でのシングル サインオンの構成** ] ダイアログ ページでコピー、 **リモート ログイン URL** 値に設定して、貼り付けます、 **IdP SSO target URL** ] ボックスに貼り付けます。
    2.  Azure ポータルで、 **Bonus.ly でのシングル サインオンの構成** ] ダイアログ ページでコピー、 **発行者 ID** 値に設定して、貼り付けます、 **IdP 発行者** ] ボックスに貼り付けます。
    3.  Azure ポータルで、 **Bonus.ly でのシングル サインオンの構成** ] ダイアログ ページでコピー、 **リモート ログイン URL** 値に設定して、貼り付けます、 **IdP ログイン URL** ] ボックスに貼り付けます。
    4.  コピー、 **拇印** エクスポートした証明書からの値し、貼り付けます、 **Cert Fingerprint** ] ボックスに貼り付けます。

        >[AZURE.TIP] 詳細については、次を参照してください [証明書の拇印の値を取得する方法。](http://youtu.be/YKQF266SAxI)

9.  クリックして **保存**します。

10. Microsoft Azure AD ポータルの構成情報を選択し、をクリックして **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![シングル サインオンの構成](./media/active-directory-saas-bonus-tutorial/IC773689.png "Configure single sign-on")
##ユーザー プロビジョニングの構成

Azure AD ユーザーが Bonus.ly にログインできるようにするには、ユーザーを Bonus.ly にプロビジョニングする必要があります。  
Bonus.ly の場合、プロビジョニングは手動で行います。

###ユーザー プロビジョニングを構成するには、次の手順に従います。

1.  Web ブラウザー ウィンドウで、Bonus.ly テナントにログインします。

2.  クリックして **設定**

    ![設定](./media/active-directory-saas-bonus-tutorial/IC781041.png "Settings")

3.  クリックして、 **ユーザーおよびボーナス** ] タブをクリックします。

    ![Users and bonuses](./media/active-directory-saas-bonus-tutorial/IC781042.png "Users and bonuses")

4.  クリックして **ユーザーを管理する**です。

    ![Manage Users](./media/active-directory-saas-bonus-tutorial/IC781043.png "Manage Users")

5.  クリックして **ユーザーを追加**します。

    ![ユーザーの追加](./media/active-directory-saas-bonus-tutorial/IC781044.png "Add User")

6.   **ユーザーの追加** ] ダイアログ ボックスで、次の手順を実行します。

    ![ユーザーの追加](./media/active-directory-saas-bonus-tutorial/IC781045.png "Add User")

    1.  型、"**電子メール**, 、**名**, 、**姓、名**"関連テキスト ボックスに、プロビジョニングする有効な AAD アカウントのです。
    2.  クリックして **保存**します。

    >[AZURE.NOTE] AAD アカウントの所有者には、アクティブ化する前に、アカウントの確認へのリンクを含むメールを受け取ります。

>[AZURE.NOTE] 他の Bonus.ly ユーザー アカウント作成ツールを使用することができますか、AAD ユーザー アカウントをプロビジョニング、Bonus.ly から提供されている Api です。

##ユーザーの割り当て

構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

###ユーザーを Bonus.ly に割り当てるには、次の手順を実行します。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  Bonus.ly アプリケーション統合ページでクリックして **ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-bonus-tutorial/IC773690.png "Assign users")

3.  テスト ユーザーを選択して [ **割り当てる**, 、順にクリック **はい** して割り当てを確認します。

    ![あり](./media/active-directory-saas-bonus-tutorial/IC767830.png "Yes")

シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。

