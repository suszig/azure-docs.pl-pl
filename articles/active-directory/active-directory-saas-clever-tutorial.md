<properties 
    pageTitle="チュートリアル: Azure Active Directory と Clever の統合 | Microsoft Azure" 
    description="Azure Active Directory で Clever を使用して、シングル サインオンや自動プロビジョニングなどを有効にする方法について説明します。" 
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

#チュートリアル: Azure Active Directory と Clever の統合

このチュートリアルでは、Azure と Clever の統合について説明します。 このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   Clever テナント

このチュートリアルを完了すると、Clever に割り当てた Azure AD ユーザーは、巧妙な会社サイト (サービス プロバイダーが開始したサインオン) で、アプリケーションにシングル サインオンまたはできるを使用して、 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。

このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  Clever のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-clever-tutorial/IC798977.png "Scenario")
##Clever のアプリケーション統合の有効化

このセクションでは、Clever のアプリケーション統合を有効にする方法について説明します。

###Clever のアプリケーション統合を有効にするには、次の手順を実行します。

1.  左側のナビゲーション ウィンドウで、Azure 管理ポータルで、クリックして **Active Directory**します。

    ![Active Directory](./media/active-directory-saas-clever-tutorial/IC700993.png "Active Directory")

2.   **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3.  ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。

    ![アプリケーション](./media/active-directory-saas-clever-tutorial/IC700994.png "Applications")

4.  クリックして **追加** ページの下部にあります。

    ![アプリケーションの追加](./media/active-directory-saas-clever-tutorial/IC749321.png "Add application")

5.   **実行する操作** ダイアログ ボックスで、をクリックして **ギャラリーからアプリケーションを追加**します。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-clever-tutorial/IC749322.png "Add an application from gallerry")

6.   **検索ボックス**, 、型 **Clever**します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-clever-tutorial/IC798978.png "Application Gallery")

7.  結果ウィンドウで [ **Clever**, 、クリックして **完了** アプリケーションを追加します。

    ![Clever](./media/active-directory-saas-clever-tutorial/IC798979.png "Clever")
##シングル サインオンの構成

このセクションでは、SAML プロトコルに基づくフェデレーションを使用して、ユーザーが Azure AD のアカウントで Clever に対する認証を行えるようにする方法を説明します。  
巧妙なアプリケーションにはカスタム属性マッピングを追加する必要がある特定の形式での SAML アサーションが必要ですが、 **saml トークン属性** 構成します。  
次のスクリーンショットはその例です。

![属性](./media/active-directory-saas-clever-tutorial/IC798980.png "Attributes")

###シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **Clever** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、* * [シングル サインオン * * ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-clever-tutorial/IC784682.png "Configure Single Sign-On")

2.   **どのような Clever にサインオン** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、順にクリック **次**します。

    ![Configure Single Sign-On](./media/active-directory-saas-clever-tutorial/IC798981.png "Configure Single Sign-On")

3.   **アプリケーション URL の構成** ] ページの [、 **巧妙なサインオン URL** ユーザーが巧みなアプリケーションにサインオンする URL を使用] ボックスに、型 (例:: *https://clever.com/in/azsandbox*)、順にクリック **次**します。

    ![アプリケーション URL の構成](./media/active-directory-saas-clever-tutorial/IC798982.png "Configure App URL")

4.   **Clever でのシングル サインオンの構成** ] ページで、メタデータをダウンロードする] をクリックして **メタデータのダウンロード**, 、コンピューターのローカルにメタデータ ファイルを保存します。

    ![Configure Single Sign-On](./media/active-directory-saas-clever-tutorial/IC798983.png "Configure Single Sign-On")

5.  別の Web ブラウザー ウィンドウで、Clever 企業サイトに管理者としてログインします。

6.  ツールバーで、次のようにクリックします。 **インスタント ログイン**します。

    ![インスタント ログイン](./media/active-directory-saas-clever-tutorial/IC798984.png "Instant Login")

7.   **インスタント ログイン** ] ページで、次の手順を実行します。

    ![インスタント ログイン](./media/active-directory-saas-clever-tutorial/IC798985.png "Instant Login")

    1.  型、 **ログイン URL**します。  

        >[AZURE.NOTE]  **ログイン URL** カスタム値です。
         実際の値は、SClever サポート チームから取得できます。

    2.  として **Id システム**, [ **ADFS**します。
    3.  クリックして **保存**します。

8.  Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-clever-tutorial/IC798986.png "Configure Single Sign-On")

9.  上部にあるメニュー [ **属性** を開くには、 **SAML トークン属性** ダイアログ。

    ![属性](./media/active-directory-saas-clever-tutorial/IC795920.png "Attributes")

10. 必要な属性のマッピングを追加するには、次の手順を実行します。

    ![SAML トークンの属性](./media/active-directory-saas-clever-tutorial/IC795921.png "saml token attributes")

    |属性名|属性値|
    |---|---|
    |clever.student.credentials.district\_username|User.userprincipalname|

    1.  上記の表の各データ行をクリックして **ユーザー属性の追加**します。
    2.   **属性名** ] ボックスに、その行に対して表示される属性名を入力します。
    3.   **属性値** ] ボックスに、その行に対して表示される属性の値を選択します。
    4.  クリックして **完了**します。

11. クリックして **の変更を適用**します。

##ユーザー プロビジョニングの構成

Azure AD ユーザーが Clever にログインできるようにするには、そのユーザーを Clever にプロビジョニングする必要があります。  
Clever の場合、プロビジョニングは Clever サポート チームが実行する必要のある手動のタスクです。

>[AZURE.NOTE] 他の巧妙なユーザー アカウント作成ツールを使用するまたは Clever AAD ユーザー アカウントのプロビジョニングに提供の Api です。

##ユーザーの割り当て

構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

###ユーザーを Clever に割り当てるには、次の手順を実行します。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  * * Clever * * アプリケーション統合ページで、をクリックして **ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-clever-tutorial/IC798987.png "Assign Users")

3.  テスト ユーザーを選択して [ **割り当てる**, 、順にクリック **はい** して割り当てを確認します。

    ![あり](./media/active-directory-saas-clever-tutorial/IC767830.png "Yes")

シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。

