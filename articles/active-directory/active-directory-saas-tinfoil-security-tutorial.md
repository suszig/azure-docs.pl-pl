<properties 
    pageTitle="チュートリアル: Azure Active Directory と Tinfoil Security の統合 | Microsoft Azure"
    description="Azure Active Directory で Tinfoil Security を使用して、シングル サインオンや自動プロビジョニングなどを有効にする方法を説明します。" 
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

#チュートリアル: Azure Active Directory と Tinfoil Security の統合
  
このチュートリアルでは、Azure と Tinfoil Security の統合について説明します。  
このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   Tinfoil Security でのシングル サインオンが有効なサブスクリプション
  
このチュートリアルを完了すると、Tinfoil Security に割り当てた Azure AD ユーザーは、Tinfoil Security の会社サイト (id プロバイダーが開始したサインオン)、アプリケーションにシングル サインオンまたはできるを使用して、 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。
  
このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  Tinfoil Security のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![Configure Single Sign-On](./media/active-directory-saas-tinfoil-security-tutorial/IC798965.png "Configure Single Sign-On")

##Tinfoil Security のアプリケーション統合の有効化
  
このセクションでは、Tinfoil Security のアプリケーション統合を有効にする方法を説明します。

###Tinfoil Security のアプリケーション統合を有効にするには、次の手順を実行します。

1.  左側のナビゲーション ウィンドウで、Azure 管理ポータルで、クリックして **Active Directory**します。

    ![Active Directory](./media/active-directory-saas-tinfoil-security-tutorial/IC700993.png "Active Directory")

2.   **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3.  ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。

    ![アプリケーション](./media/active-directory-saas-tinfoil-security-tutorial/IC700994.png "Applications")

4.  クリックして **追加** ページの下部にあります。

    ![アプリケーションの追加](./media/active-directory-saas-tinfoil-security-tutorial/IC749321.png "Add application")

5.   **実行する操作** ダイアログ ボックスで、をクリックして **ギャラリーからアプリケーションを追加**します。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-tinfoil-security-tutorial/IC749322.png "Add an application from gallerry")

6.   **検索ボックス**, 、型 **Tinfoil Security**します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-tinfoil-security-tutorial/IC798966.png "Application Gallery")

7.  結果ウィンドウで [ **Tinfoil Security**, 、クリックして **完了** アプリケーションを追加します。

    ![Tinfoil Security](./media/active-directory-saas-tinfoil-security-tutorial/IC802771.png "Tinfoil Security")

##シングル サインオンの構成
  
このセクションでは、ユーザーが SAML プロトコルに基づくフェデレーションを使用して、Azure AD でのユーザーのアカウントで Tinfoil Security に対する認証を行うことができるようにする方法を説明します。  
Tinfoil Security のシングル サインオンを構成するには、証明書からサムプリント値を取得する必要があります。  
この手順に慣れていない場合は、次を参照してください。 [証明書の拇印の値を取得する方法](http://youtu.be/YKQF266SAxI)します。

###シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **Tinfoil Security** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、* * [シングル サインオン * * ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-tinfoil-security-tutorial/IC798967.png "Configure Single Sign-On")

2.   **どのようなユーザーを Tinfoil Security へのサインオン** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、] をクリックし、 **次**します。

    ![Configure Single Sign-On](./media/active-directory-saas-tinfoil-security-tutorial/IC798968.png "Configure Single Sign-On")

3.   **アプリケーション URL の構成** ] ページで、 **Tinfoil Security 応答 URL** ] ボックスに、Tinfoil Security Assertion Consumer Service (ACS) の URL を入力 (例::"*https://www.tinfoilsecurity.com/saml/consume*"、順にクリック **次**します。

    >[AZURE.NOTE] Tinfoil Security Metadata (https://www.tinfoilsecurity.com/saml/metadata) から ACS URL を取得することができます。

    ![アプリケーション URL の構成](./media/active-directory-saas-tinfoil-security-tutorial/IC798969.png "Configure App URL")

4.   **Tinfoil Security でのシングル サインオンの構成** ] ページで、証明書をダウンロードする] をクリックして **証明書のダウンロード**, 、としては、証明書ファイルをローカルに保存および **c:\\Tinfoil Security.cer**します。

    ![Configure Single Sign-On](./media/active-directory-saas-tinfoil-security-tutorial/IC798970.png "Configure Single Sign-On")

5.  別の Web ブラウザーのウィンドウで、Tinfoil Security の企業サイトに管理者としてログインします。

6.  上部にあるツールバーで、クリックして **マイ アカウント**します。

    ![ダッシュボード](./media/active-directory-saas-tinfoil-security-tutorial/IC798971.png "Dashboard")

7.  クリックして **セキュリティ**します。

    ![セキュリティ](./media/active-directory-saas-tinfoil-security-tutorial/IC798972.png "Security")

8.   **でのシングル サインオン** 構成] ページで、次の手順を実行します。

    ![シングル サインオン](./media/active-directory-saas-tinfoil-security-tutorial/IC798973.png "Single Sign-On")

    1.  選択 **Enable SAML**します。
    2.  クリックして **を手動で構成**します。
    3.  Azure ポータルで、 **Tinfoil Security でのシングル サインオンの構成** ] ダイアログ ページでコピー、 **SAML SSO URL** 値に設定して、貼り付けます、 **SAML Post URL** ] ボックスに貼り付けます。
    4.  コピー、 **拇印** エクスポートした証明書からの値し、貼り付けます、 **SAML Certificate Fingerprint** ] ボックスに貼り付けます。  

        >[AZURE.TIP] 詳細については、次を参照してください [証明書の拇印の値を取得する方法。](http://youtu.be/YKQF266SAxI)

    5.  コピー **アカウント ID**します。
    6.  クリックして **保存**します。

9.  Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-tinfoil-security-tutorial/IC798974.png "Configure Single Sign-On")

10. 上部にあるメニュー [ **属性** を開くには、 **SAML トークン属性** ダイアログ。

    ![属性](./media/active-directory-saas-tinfoil-security-tutorial/IC795920.png "Attributes")

11. 必要な属性のマッピングを追加するには、次の手順を実行します。

    ![属性](./media/active-directory-saas-tinfoil-security-tutorial/IC798975.png "Attributes")

    1.  クリックして **ユーザー属性の追加**します。
    2.   **属性名** ] ボックスに「 **accountid**します。
    3.   **属性値** 前のセクションでコピーしたテキスト ボックスに、値を貼り付けますアカウント id を指定します。
    4.  クリックして **完了**します。

12. クリックして **の変更を適用**します。

##ユーザー プロビジョニングの構成
  
Azure AD ユーザーが Tinfoil Security にログインできるようにするには、そのユーザーを Tinfoil Security にプロビジョニングする必要があります。  
Tinfoil Security の場合、プロビジョニングは手動で行います。

###ユーザーをプロビジョニングするには、次の手順に従います。

1.  ユーザーがエンタープライズ アカウントに参加している場合は、Tinfoil Security サポート チームに連絡して、ユーザー アカウントを作成する必要があります。

2.  通常の Tinfoil Security SaaS ユーザーは、グループ作業者をユーザーの任意のサイトに追加できます。 ここでは、新しい Tinfoil Security ユーザー アカウントを作成するための招待状を指定した電子メール アドレスに送信するプロセスが起動します。

>[AZURE.NOTE] 他の Tinfoil Security ユーザー アカウント作成ツールを使用することができますか、AAD ユーザー アカウントをプロビジョニング、Tinfoil Security から提供されている Api です。

##ユーザーの割り当て
  
構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

###ユーザーを Tinfoil Security に割り当てるには、次の手順を実行します。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  * * Tinfoil Security * * アプリケーション統合ページで、をクリックして **ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-tinfoil-security-tutorial/IC798976.png "Assign Users")

3.  テスト ユーザーを選択して [ **割り当てる**, 、順にクリック **はい** して割り当てを確認します。

    ![あり](./media/active-directory-saas-tinfoil-security-tutorial/IC767830.png "Yes")
  
シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。
