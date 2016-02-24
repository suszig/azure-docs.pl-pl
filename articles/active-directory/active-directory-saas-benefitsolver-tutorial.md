<properties 
    pageTitle="チュートリアル: Azure Active Directory と Benefitsolver の統合 | Microsoft Azure"
    description="Azure Active Directory で Benefitsolver を使用して、シングル サインオンや自動プロビジョニングなどを有効にする方法について説明します。" 
    services="active-directory" 
    authors="MarkusVi"  
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

#チュートリアル: Azure Active Directory と Benefitsolver の統合

このチュートリアルでは、Azure と Benefitsolver の統合について説明します。  
このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   Benefitsolver でのシングル サインオンが有効なサブスクリプション

このチュートリアルを完了すると、Benefitsolver に割り当てた Azure AD ユーザーができるようにアプリケーションを使用して、シングル サインオン、 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。

このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  Benefitsolver のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-benefitsolver-tutorial/IC804820.png "Scenario")
##Benefitsolver のアプリケーション統合の有効化

このセクションでは、Benefitsolver のアプリケーション統合を有効にする方法を説明します。

###Benefitsolver のアプリケーション統合を有効にするには、次の手順を実行します。

1.  左側のナビゲーション ウィンドウで、Azure 管理ポータルで、クリックして **Active Directory**します。

    ![Active Directory](./media/active-directory-saas-benefitsolver-tutorial/IC700993.png "Active Directory")

2.   **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3.  ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。

    ![アプリケーション](./media/active-directory-saas-benefitsolver-tutorial/IC700994.png "Applications")

4.  クリックして **追加** ページの下部にあります。

    ![アプリケーションの追加](./media/active-directory-saas-benefitsolver-tutorial/IC749321.png "Add application")

5.   **実行する操作** ダイアログ ボックスで、をクリックして **ギャラリーからアプリケーションを追加**します。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-benefitsolver-tutorial/IC749322.png "Add an application from gallerry")

6.   **検索ボックス**, 、型 **Benefitsolver**します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-benefitsolver-tutorial/IC804821.png "Application Gallery")

7.  結果ウィンドウで [ **Benefitsolver**, 、クリックして **完了** アプリケーションを追加します。

    ![Benefitssolver](./media/active-directory-saas-benefitsolver-tutorial/IC804822.png "Benefitssolver")
##シングル サインオンの構成

このセクションでは、SAML プロトコルに基づくフェデレーションを使用して、ユーザーが Azure AD のアカウントで Benefitsolver に対する認証を行えるようにする方法を説明します。  
Benefitsolver アプリケーションにはカスタム属性マッピングを追加する必要がある特定の形式での SAML アサーションが必要ですが、 **saml トークン属性** 構成します。  
次のスクリーンショットはその例です。

![属性](./media/active-directory-saas-benefitsolver-tutorial/IC804823.png "Attributes")

###シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **Benefitsolver** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、* * [シングル サインオン * * ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-benefitsolver-tutorial/IC804824.png "Configure Single Sign-On")

2.   **どのような Benefitsolver にサインオン** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、順にクリック **次**します。

    ![Configure Single Sign-On](./media/active-directory-saas-benefitsolver-tutorial/IC804825.png "Configure Single Sign-On")

3.   **アプリケーション設定の構成** ] ページで、次の手順を実行します。

    ![Configure App Settings](./media/active-directory-saas-benefitsolver-tutorial/IC804826.png "Configure App Settings")

    1.   **サインオン URL** ユーザー Benefitsolver アプリケーションにサインオンする URL を使用] ボックスに、種類 (例::"*http://azure-dev.benefitsolver.com*
    2.   **応答 URL** ] ボックスに、Benefitsolver AssertionConsumerService URL を入力 (例::"*https://dev.benefitsolver.com/benefits/BenefitSolverView?page \_name=single\_signon\_saml*") です。  

        >[AZURE.NOTE] 環境内の Benefitsolver サポート チームから実際の値を取得できます。

    3.  クリックして **次**します。

4.   **Benefitsolver でのシングル サインオンの構成** ] ページで、メタデータをダウンロードする] をクリックして **メタデータのダウンロード**, 、コンピューターのローカルにメタデータ ファイルを保存します。

    ![Configure Single Sign-On](./media/active-directory-saas-benefitsolver-tutorial/IC804827.png "Configure Single Sign-On")

5.  ダウンロードしたメタデータ ファイルを Benefitsolver サポート チームに送信します。

    >[AZURE.NOTE] Benefitsolver サポート チームは、実際の SSO 構成を行うことにします。
     ご使用のサブスクリプションで SSO が有効になると通知されます。

6.  Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-benefitsolver-tutorial/IC804828.png "Configure Single Sign-On")

7.  上部にあるメニュー [ **属性** を開くには、 **SAML トークン属性** ダイアログ。

    ![属性](./media/active-directory-saas-benefitsolver-tutorial/IC795920.png "Attributes")

8.  必要な属性のマッピングを追加するには、次の手順を実行します。

    ![属性](./media/active-directory-saas-benefitsolver-tutorial/IC804823.png "Attributes")

    |属性名|属性値|
    |---|---|
    |ClientID|この値は、Benefitsolver サポート チームから入手する必要があります。|
    |ClientKey|この値は、Benefitsolver サポート チームから入手する必要があります。|
    |LogoutURL|この値は、Benefitsolver サポート チームから入手する必要があります。|
    |EmployeeID|この値は、Benefitsolver サポート チームから入手する必要があります。|

    1.  上記の表の各データ行をクリックして **ユーザー属性の追加**します。
    2.   **属性名** ] ボックスに、その行に対して表示される属性名を入力します。
    3.   **属性値** ] ボックスに、その行に対して表示される属性の値を選択します。
    4.  クリックして **完了**します。

9.  クリックして **の変更を適用**します。
##ユーザー プロビジョニングの構成

Azure AD ユーザーが Benefitsolver にログインできるようにするには、ユーザーを Benefitsolver にプロビジョニングする必要があります。  
Benefitsolver の場合、Benefitsolver サポート チームがユーザーを手動で作成する必要があります。

>[AZURE.NOTE] 他の Benefitsolver ユーザー アカウント作成ツールを使用することができます。 または Api に用意されている Benefitsolver AAD ユーザー アカウントのプロビジョニングします。

##ユーザーの割り当て

構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

###ユーザーを Benefitsolver に割り当てるには、次の手順を実行します。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  * * Benefitsolver * * アプリケーション統合ページで、をクリックして **ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-benefitsolver-tutorial/IC804829.png "Assign Users")

3.  テスト ユーザーを選択して [ **割り当てる**, 、順にクリック **はい** して割り当てを確認します。

    ![あり](./media/active-directory-saas-benefitsolver-tutorial/IC767830.png "Yes")

シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。

