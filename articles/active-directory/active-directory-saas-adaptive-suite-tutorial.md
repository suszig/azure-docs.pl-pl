<properties 
    pageTitle="チュートリアル: Azure Active Directory と Adaptive Suite の統合 | Microsoft Azure"
    description="Azure Active Directory で Adaptive Suite を使用して、シングル サインオンや自動プロビジョニングなどを有効にする方法について説明します。" 
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

#チュートリアル: Azure Active Directory と Adaptive Suite の統合

このチュートリアルでは、Azure と Adaptive Suite の統合について説明します。  
このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   Adaptive Suite のテナント

このチュートリアルを完了すると、アダプティブ スイートに割り当てた Azure AD ユーザーは、アダプティブ スイート会社サイト (サービス プロバイダーが開始したサインオン) で、アプリケーションにシングル サインオンまたはできるを使用して、 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。

このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  Adaptive Suite のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-adaptive-suite-tutorial/IC805637.png "Scenario")
##Adaptive Suite のアプリケーション統合の有効化

このセクションでは、Adaptive Suite のアプリケーション統合を有効にする方法を説明します。

###Adaptive Suite のアプリケーション統合を有効にするには、次の手順に従います。

1.  左側のナビゲーション ウィンドウで、Azure 管理ポータルで、クリックして **Active Directory**します。

    ![Active Directory](./media/active-directory-saas-adaptive-suite-tutorial/IC700993.png "Active Directory")

2.   **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3.  ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。

    ![アプリケーション](./media/active-directory-saas-adaptive-suite-tutorial/IC700994.png "Applications")

4.  クリックして **追加** ページの下部にあります。

    ![アプリケーションの追加](./media/active-directory-saas-adaptive-suite-tutorial/IC749321.png "Add application")

5.   **実行する操作** ダイアログ ボックスで、をクリックして **ギャラリーからアプリケーションを追加**します。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-adaptive-suite-tutorial/IC749322.png "Add an application from gallerry")

6.   **検索ボックス**, 、型 **アダプティブ スイート**します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-adaptive-suite-tutorial/IC805638.png "Application Gallery")

7.  結果ウィンドウで [ **アダプティブ スイート**, 、] をクリックし、 **完了** アプリケーションを追加します。

    ![Adaptive Suite](./media/active-directory-saas-adaptive-suite-tutorial/IC805639.png "Adaptive Suite")
##シングル サインオンの構成

このセクションでは、ユーザーが SAML プロトコルに基づくフェデレーションを使用して、Azure AD でのユーザーのアカウントで Adaptive Suite に対する認証を行えるようにする方法を説明します。  
Adaptive Suite にシングル サインオンを構成するには、証明書の拇印の値を取得する必要があります。  
この手順に慣れていない場合は、次を参照してください。 [証明書の拇印の値を取得する方法](http://youtu.be/YKQF266SAxI)します。

###シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **アダプティブ スイート** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、* * [シングル サインオン * * ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-adaptive-suite-tutorial/IC805640.png "Configure Single Sign-On")

2.   **どのようなアダプティブ スイートにサインオン** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、] をクリックし、 **次**します。

    ![Configure Single Sign-On](./media/active-directory-saas-adaptive-suite-tutorial/IC805641.png "Configure Single Sign-On")

3.   **アプリケーション設定の構成** ] ページの [、 **応答 URL** ] ボックスに、次のパターンを使用して URL を入力"*https://login.adaptiveinsights.com:443/samlsso/RlJFRVRSSUFMMTI3MTE =*"、] をクリックし、 **次**します。

    >[AZURE.NOTE] この値を取得するには、アダプティブ スイートから **SAML SSO 設定** ページです。

    ![Configure App Settings](./media/active-directory-saas-adaptive-suite-tutorial/IC805642.png "Configure App Settings")

4.   **アダプティブ スイートでのシングル サインオンの構成** ] ページで、証明書をダウンロードする] をクリックして **証明書のダウンロード**, 、お使いのコンピューターにローカルで証明書ファイルを保存します。

    ![Configure Single Sign-On](./media/active-directory-saas-adaptive-suite-tutorial/IC805643.png "Configure Single Sign-On")

5.  別の Web ブラウザーのウィンドウで、管理者として Adaptive Suite 企業サイトにログインします。

6.  移動して **Admin**します。

    ![管理者](./media/active-directory-saas-adaptive-suite-tutorial/IC805644.png "Admin")

7.   **ユーザーおよびロール** ] をクリックして **SAML SSO 設定の管理**します。

    ![SAML SSO 設定の管理](./media/active-directory-saas-adaptive-suite-tutorial/IC805645.png "Manage SAML SSO Settings")

8.   **SAML SSO 設定** ] ページで、次の手順を実行します。

    ![SAML SSO 設定](./media/active-directory-saas-adaptive-suite-tutorial/IC805646.png "SAML SSO Settings")

    1.   **Id プロバイダー名** ] ボックスに、構成の名前を入力します。
    2.  Azure ポータルで、 **アダプティブ スイートでのシングル サインオンの構成** ] ダイアログ ページでコピー、 **エンティティ ID** 値に設定して、貼り付けます、 **Id プロバイダー エンティティの ID** ] ボックスに貼り付けます。
    3.  Azure ポータルで、 **アダプティブ スイートでのシングル サインオンの構成** ] ダイアログ ページでコピー、 **SAML SSO URL** 値に設定して、貼り付けます、 **SSO URL の Id プロバイダー** ] ボックスに貼り付けます。
    4.  Azure ポータルで、 **アダプティブ スイートでのシングル サインオンの構成** ] ダイアログ ページでコピー、 **SAML SSO URL** 値に設定して、貼り付けます、 **カスタム ログアウト URL** ] ボックスに貼り付けます。
    5.  ダウンロードした証明書をアップロードするにはクリックして **ファイルの選択**します。
    6.  として **SAML ユーザー id**, [ **アダプティブ Insights ユーザー名のユーザーの**です。
    7.  として **SAML ユーザー id の所在地**, [ **NameID のサブジェクトのユーザー id**します。
    8.  として **SAML NameID format**, [ **電子メール アドレス**です。
    9.  として **Enable SAML**, [ **SAML SSO を許可してアダプティブ Insights の直接ログイン**します。
    10. クリックして **保存**します。

9.  Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-adaptive-suite-tutorial/IC805647.png "Configure Single Sign-On")
##ユーザー プロビジョニングの構成

Azure AD ユーザーが Adaptive Suite にログインできるようにするには、そのユーザーを Adaptive Suite にプロビジョニングする必要があります。  
Adaptive Suite の場合、プロビジョニングは手動で実行します。

###ユーザー プロビジョニングを構成するには、次の手順に従います。

1.  ログインして **アダプティブ スイート** 会社サイトに管理者として。

2.  移動して **Admin**します。

    ![管理者](./media/active-directory-saas-adaptive-suite-tutorial/IC805644.png "Admin")

3.   **ユーザーおよびロール** ] をクリックして **ユーザーの追加**します。

    ![ユーザーの追加](./media/active-directory-saas-adaptive-suite-tutorial/IC805648.png "Add User")

4.   **新しいユーザー** セクションで、次の手順に従います。

    ![送信](./media/active-directory-saas-adaptive-suite-tutorial/IC805649.png "Submit")

    1.  型、 **名前**, 、**ログイン**, 、**電子メール**, 、**パスワード** 関連テキスト ボックスに、プロビジョニングする有効な Azure Active Directory ユーザーのです。
    2.  選択、 **ロール**します。
    3.  クリックして **送信**します。

>[AZURE.NOTE] 他のアダプティブ スイート ユーザー アカウント作成ツールを使用するや、AAD ユーザー アカウントのプロビジョニングをアダプティブ スイート別提供の Api です。

##ユーザーの割り当て

構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

###ユーザーを Adaptive Suite に割り当てるには、次の手順に従います。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  * * アダプティブ スイート * * アプリケーション統合ページで、をクリックして **ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-adaptive-suite-tutorial/IC805650.png "Assign Users")

3.  テスト ユーザーを選択して [ **割り当てる**, 、順にクリック **はい** して割り当てを確認します。

    ![あり](./media/active-directory-saas-adaptive-suite-tutorial/IC767830.png "Yes")

シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。

