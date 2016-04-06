<properties 
    pageTitle="チュートリアル: Azure Active Directory と TalentLMS の統合 | Microsoft Azure" 
    description="Azure Active Directory で TalentLMS を使用して、シングル サインオンや自動プロビジョニングなどを有効にする方法について説明します。" 
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

#チュートリアル: Azure Active Directory と TalentLMS の統合
  
このチュートリアルでは、Azure と TalentLMS の統合について説明します。  
このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   TalentLMS テナント
  
このチュートリアルを完了すると、TalentLMS に割り当てた Azure AD ユーザーは、TalentLMS 会社サイト (サービス プロバイダーが開始したサインオン) で、アプリケーションにシングル サインオンまたはできるを使用して、 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。
  
このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  TalentLMS のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-talentlms-tutorial/IC777289.png "Scenario")

##TalentLMS のアプリケーション統合の有効化
  
このセクションでは、TalentLMS のアプリケーション統合を有効にする方法について説明します。

###TalentLMS のアプリケーション統合を有効にするには、次の手順に従います。

1.  左側のナビゲーション ウィンドウで、Azure 管理ポータルで、クリックして **Active Directory**します。

    ![Active Directory](./media/active-directory-saas-talentlms-tutorial/IC700993.png "Active Directory")

2.   **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3.  ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。

    ![アプリケーション](./media/active-directory-saas-talentlms-tutorial/IC700994.png "Applications")

4.  クリックして **追加** ページの下部にあります。

    ![アプリケーションの追加](./media/active-directory-saas-talentlms-tutorial/IC749321.png "Add application")

5.   **実行する操作** ダイアログ ボックスで、をクリックして **ギャラリーからアプリケーションを追加**します。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-talentlms-tutorial/IC749322.png "Add an application from gallerry")

6.   **検索ボックス**, 、型 **TalentLMS**します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-talentlms-tutorial/IC777290.png "Application gallery")

7.  結果ウィンドウで [ **TalentLMS**, 、クリックして **完了** アプリケーションを追加します。

    ![TalentLMS](./media/active-directory-saas-talentlms-tutorial/IC777291.png "TalentLMS")

##シングル サインオンの構成
  
このセクションでは、SAML プロトコルに基づくフェデレーションを使用して Azure AD のアカウントを TalentLMS に認証するユーザーを有効にする方法を説明します。 .  
TalentLMS のシングル サインオンを構成するには、証明書からサムプリント値を取得する必要があります。  
この手順に慣れていない場合は、次を参照してください。 [証明書の拇印の値を取得する方法](http://youtu.be/YKQF266SAxI)します。

###シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **TalentLMS** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、* * [シングル サインオン * * ダイアログ。

    ![シングル サインオンの構成](./media/active-directory-saas-talentlms-tutorial/IC777292.png "Configure single sign-on")

2.   **どのようなユーザーが TalentLMS へのサインオン** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、] をクリックし、 **次**します。

    ![Configure single sign-on](./media/active-directory-saas-talentlms-tutorial/IC777293.png "Configure single sign-on")

3.   **アプリケーション URL の構成** ] ページで、 **TalentLMS サインイン URL** ] ボックスに、次のパターンを使用して URL を入力"*https://\<tenant-name\>。TalentLMSapp.com*"、クリックして **次**します。

    ![サインオン URL](./media/active-directory-saas-talentlms-tutorial/IC777294.png "Sign on URL")

4.   **TalentLMS でのシングル サインオンの構成** ] ページで、証明書をダウンロードする] をクリックして **証明書のダウンロード**, 、としては、証明書ファイルをローカルに保存および **c:\\TalentLMS.cer**します。

    ![Configure Single Sign-On](./media/active-directory-saas-talentlms-tutorial/IC777295.png "Configure Single Sign-On")

5.  別の Web ブラウザー ウィンドウで、TalentLMS 企業サイトに管理者としてログインします。

6.   **アカウントと設定** ] をクリックして、 **ユーザー** ] タブをクリックします。

    ![アカウントと設定](./media/active-directory-saas-talentlms-tutorial/IC777296.png "Account & Settings")

7.  クリックして **シングル サインオン (SSO)**,、

8.  [シングル サインオン] セクションで、次の手順に従います。

    ![シングル サインオン](./media/active-directory-saas-talentlms-tutorial/IC777297.png "Single Sign-On")

    1.   **SSO integration type** 一覧で、[ **SAML 2.0**します。
    2.  Azure ポータルで、 **TalentLMS でのシングル サインオンの構成** ] ダイアログ ページでコピー、 **Id プロバイダーの ID** 値に設定して、貼り付けます、 **Id プロバイダー (IdP)** ] ボックスに貼り付けます。
    3.  コピー、 **拇印** エクスポートした証明書からの値し、貼り付けます、 **Certificate Fingerprint** ] ボックスに貼り付けます。

        >[AZURE.TIP] 詳細については、次を参照してください [証明書の拇印の値を取得する方法。](http://youtu.be/YKQF266SAxI)

    4.  Azure ポータルで、 **TalentLMS でのシングル サインオンの構成** ] ダイアログ ページでコピー、 **リモート ログイン URL** 値に設定して、貼り付けます、 **リモート サインイン URL** ] ボックスに貼り付けます。
    5.  Azure ポータルで、 **TalentLMS でのシングル サインオンの構成** ] ダイアログ ページでコピー、 **リモート ログアウト URL** 値に設定して、貼り付けます、 **リモート サインアウト URL** ] ボックスに貼り付けます。
    6.   **[Targetedid]** ] ボックスに「 **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name**
    7.   **名** ] ボックスに「 **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**
    8.   **姓、名** ] ボックスに「 **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**
    9.   **電子メール** ] ボックスに「 **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**
    10. クリックして **保存**します。

9.  Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-talentlms-tutorial/IC777298.png "Configure Single Sign-On")

##ユーザー プロビジョニングの構成
  
Azure AD ユーザーが TalentLMS にログインできるようにするには、そのユーザーを TalentLMS にプロビジョニングする必要があります。  
TalentLMS の場合、プロビジョニングは手動で行います。

###ユーザー アカウントをプロビジョニングするには、次の手順を実行します。

1.  ログインして **TalentLMS** テナントです。

2.  クリックして **ユーザー**, 、] をクリックし、 **ユーザーの追加**します。

3.   **ユーザーの追加** ] ダイアログ ページで、次の手順を実行します。

    ![ユーザーの追加](./media/active-directory-saas-talentlms-tutorial/IC777299.png "Add User")

    1.  次のテキスト ボックスに、Azure AD ユーザー アカウントの関連する属性の値を入力: **名**, 、**姓、名**, 、**電子メール アドレス**です。
    2.  クリックして **ユーザーを追加**します。

>[AZURE.NOTE] 他の TalentLMS ユーザー アカウント作成ツールを使用するや、AAD ユーザー アカウントをプロビジョニング、TalentLMS 提供の Api です。

##ユーザーの割り当て
  
構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

###ユーザーを TalentLMS に割り当てるには、次の手順に従います。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  * * TalentLMS * * アプリケーション統合ページで、をクリックして **ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-talentlms-tutorial/IC777300.png "Assign users")

3.  テスト ユーザーを選択して [ **割り当てる**, 、順にクリック **はい** して割り当てを確認します。

    ![あり](./media/active-directory-saas-talentlms-tutorial/IC767830.png "Yes")
  
シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。

