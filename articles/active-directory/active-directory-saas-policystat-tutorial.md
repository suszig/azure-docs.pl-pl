<properties 
    pageTitle="チュートリアル: Azure Active Directory と PolicyStat の統合 | Microsoft Azure" 
    description="Azure Active Directory で PolicyStat を使用して、シングル サインオンや自動プロビジョニングなどを有効にする方法について説明します。" 
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

#チュートリアル: Azure Active Directory と PolicyStat の統合
  
このチュートリアルでは、Azure と PolicyStat の統合について説明します。  
このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   PolicyStat テナント
  
このチュートリアルを完了すると、PolicyStat に割り当てた Azure AD ユーザーは、PolicyStat 会社サイト (サービス プロバイダーが開始したサインオン) で、アプリケーションにシングル サインオンまたはできるを使用して、 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。
  
このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  PolicyStat のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-policystat-tutorial/IC808662.png "Scenario")
##PolicyStat のアプリケーション統合の有効化
  
このセクションでは、PolicyStat のアプリケーション統合を有効にする方法を説明します。

###PolicyStat のアプリケーション統合を有効にするには、次の手順に従います。

1.  左側のナビゲーション ウィンドウで、Azure 管理ポータルで、クリックして **Active Directory**します。

    ![Active Directory](./media/active-directory-saas-policystat-tutorial/IC700993.png "Active Directory")

2.   **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3.  ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。

    ![アプリケーション](./media/active-directory-saas-policystat-tutorial/IC700994.png "Applications")

4.  クリックして **追加** ページの下部にあります。

    ![アプリケーションの追加](./media/active-directory-saas-policystat-tutorial/IC749321.png "Add application")

5.   **実行する操作** ダイアログ ボックスで、をクリックして **ギャラリーからアプリケーションを追加**します。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-policystat-tutorial/IC749322.png "Add an application from gallerry")

6.   **検索ボックス**, 、型 **PolicyStat**します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-policystat-tutorial/IC808627.png "Application Gallery")

7.  結果ウィンドウで [ **PolicyStat**, 、クリックして **完了** アプリケーションを追加します。

    ![PolicyStat](./media/active-directory-saas-policystat-tutorial/IC810430.png "PolicyStat")
##シングル サインオンの構成
  
このセクションでは、ユーザーが SAML プロトコルに基づくフェデレーションを使用して、Azure AD でのユーザーのアカウントで PolicyStat に対する認証を行えるようにする方法を説明します。  
PolicyStat アプリケーションにはカスタム属性マッピングを追加する必要がある特定の形式での SAML アサーションが必要ですが、 **saml トークン属性** 構成します。  
次のスクリーンショットはその例です。

![属性](./media/active-directory-saas-policystat-tutorial/IC808628.png "Attributes")

###シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **PolicyStat** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、* * [シングル サインオン * * ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-policystat-tutorial/IC808629.png "Configure Single Sign-On")

2.   **どのような PolicyStat にサインオン** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、順にクリック **次**します。

    ![Configure Single Sign-On](./media/active-directory-saas-policystat-tutorial/IC808630.png "Configure Single Sign-On")

3.   **アプリケーション設定の構成** ] ページの [、 **サインオン URL** ユーザーが URL PolicyStat アプリケーションにサインオンする URL を使用] ボックスに、型 (例:: *"https://demo-azure.policystat.com"*)、順にクリック **次**します。

    ![Configure App Settings](./media/active-directory-saas-policystat-tutorial/IC808631.png "Configure App Settings")

4.   **PolicyStat でのシングル サインオンの構成** ] ページで [ **メタデータのダウンロード**, 、お使いのコンピューターにメタデータ ファイルを保存します。

    ![Configure Single Sign-On](./media/active-directory-saas-policystat-tutorial/IC808632.png "Configure Single Sign-On")

5.  別の Web ブラウザーのウィンドウで、PolicyStat 企業サイトに管理者としてログインします。

6.  クリックして、 **Admin** タブをクリックし、をクリックして **シングル サインオンの構成** 左側のナビゲーション ウィンドウでします。

    ![管理者メニュー](./media/active-directory-saas-policystat-tutorial/IC808633.png "Administrator Menu")

7.   **セットアップ** ] セクションの [ **を有効にするシングル サインオン統合**します。

    ![シングル サインオンの構成](./media/active-directory-saas-policystat-tutorial/IC808634.png "Single Sign-On Configuration")

8.  クリックして **属性を構成する**, 、[、 **属性を構成する** セクションで、次の手順に従います。

    ![シングル サインオンの構成](./media/active-directory-saas-policystat-tutorial/IC808635.png "Single Sign-On Configuration")

    1.   **ユーザー名属性** ] ボックスに「 **uid**します。
    2.   **名属性** ] ボックスに「 **firstname**します。
    3.   **姓属性** ] ボックスに「 **lastname**します。
    4.   **電子メール属性** ] ボックスに「 **emailaddress**します。
    5.  クリックして **変更を保存**します。

9.  をクリックして **Your IDP Metadata**, 、[、 **Your IDP Metadata** セクションで、次の手順に従います。

    ![シングル サインオンの構成](./media/active-directory-saas-policystat-tutorial/IC808635.png "Single Sign-On Configuration")

    1.  ダウンロードしたメタデータ ファイルを開いて、コンテンツをコピーして貼り付けます、 **、Id プロバイダー メタデータ** ] テキスト ボックス
    2.  クリックして **変更を保存**します。

10. Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-policystat-tutorial/IC771723.png "Configure Single Sign-On")

11. 12. 上部にあるメニュー [ **属性** を開くには、 **SAML トークン属性** ダイアログ。

    ![属性](./media/active-directory-saas-policystat-tutorial/IC795920.png "Attributes")

13. 必要な属性のマッピングを追加するには、次の手順を実行します。

    ![属性](./media/active-directory-saas-policystat-tutorial/IC804823.png "Attributes")

    1.  クリックして **ユーザー属性の追加**します。
    2.   **属性名** ] ボックスに「 **uid**します。
    3.   **属性値** ] ボックスに、select **ExtractMailPrefix()**します。
    4.   **メール** 一覧で、[ **User.mail**します。
    5.  クリックして **完了**します。
##ユーザー プロビジョニングの構成
  
Azure AD ユーザーが PolicyStat にログインできるようにするには、ユーザーを PolicyStat にプロビジョニングする必要があります。  
PolicyStat は、ジャストインタイム ユーザー プロビジョニングをサポートしています。 つまり、PolicyStat にユーザーを手動で追加する必要はありません。  
ユーザーはシングル サインオンの最初のログイン時に自動的に追加されます。

>[AZURE.NOTE]他の PolicyStat ユーザー アカウント作成ツールを使用することができます。 または Api に用意されている PolicyStat AAD ユーザー アカウントのプロビジョニングします。

##ユーザーの割り当て
  
構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

###ユーザーを PolicyStat に割り当てるには、次の手順に従います。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  * * PolicyStat * * アプリケーション統合ページで、をクリックして **ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-policystat-tutorial/IC808636.png "Assign Users")

3.  テスト ユーザーを選択して [ **割り当てる**, 、順にクリック **はい** して割り当てを確認します。

    ![あり](./media/active-directory-saas-policystat-tutorial/IC767830.png "Yes")
  
シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。
