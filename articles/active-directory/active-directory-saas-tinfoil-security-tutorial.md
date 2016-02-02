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


# チュートリアル: Azure Active Directory と Tinfoil Security の統合

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

## Tinfoil Security のアプリケーション統合の有効化

このセクションでは、Tinfoil Security のアプリケーション統合を有効にする方法を説明します。

### Tinfoil Security のアプリケーション統合を有効にするには、次の手順を実行します。

1.  Azure 管理ポータルの左側のナビゲーション ウィンドウで、**[Active Directory]** をクリックします。

    ![Active Directory](./media/active-directory-saas-tinfoil-security-tutorial/IC700993.png "Active Directory")

2.  **[ディレクトリ]** の一覧から、ディレクトリ統合を有効にするディレクトリを選択します。

3.  アプリケーション ビューを開くには、ディレクトリ ビューでトップ メニューの **[アプリケーション]** をクリックします。

    ![アプリケーション](./media/active-directory-saas-tinfoil-security-tutorial/IC700994.png "Applications")

4.  ページの下部にある **[追加]** をクリックします。

    ![アプリケーションの追加](./media/active-directory-saas-tinfoil-security-tutorial/IC749321.png "Add application")

5.  **[実行する内容]** ダイアログで、**[ギャラリーからアプリケーションを追加します]** をクリックします。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-tinfoil-security-tutorial/IC749322.png "Add an application from gallerry")

6.  **検索ボックス**に、「**Tinfoil Security**」と入力します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-tinfoil-security-tutorial/IC798966.png "Application Gallery")

7.  結果ウィンドウで **[Tinfoil Security]** を選択し、**[完了]** をクリックしてアプリケーションを追加します。

    ![Tinfoil Security](./media/active-directory-saas-tinfoil-security-tutorial/IC802771.png "Tinfoil Security")

## シングル サインオンの構成

このセクションでは、ユーザーが SAML プロトコルに基づくフェデレーションを使用して、Azure AD でのユーザーのアカウントで Tinfoil Security に対する認証を行うことができるようにする方法を説明します。  
Tinfoil Security のシングル サインオンを構成するには、証明書からサムプリント値を取得する必要があります。  
この手順に慣れていない場合は、次を参照してください。 [証明書の拇印の値を取得する方法](http://youtu.be/YKQF266SAxI)します。

### シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **Tinfoil Security** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、 ** [シングル サインオン ** ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-tinfoil-security-tutorial/IC798967.png "Configure Single Sign-On")

2.  **どのようなユーザーを Tinfoil Security へのサインオン** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、] をクリックし、 **次**します。

    ![Configure Single Sign-On](./media/active-directory-saas-tinfoil-security-tutorial/IC798968.png "Configure Single Sign-On")

3.  **アプリケーション URL の構成** ] ページで、 **Tinfoil Security 応答 URL** ] ボックスに、Tinfoil Security Assertion Consumer Service (ACS) の URL を入力 (例::"*https://www.tinfoilsecurity.com/saml/consume*"、順にクリック **次**します。
    >[AZURE.NOTE] Tinfoil Security Metadata (https://www.tinfoilsecurity.com/saml/metadata) から ACS URL を取得することができます。

    ![アプリケーション URL の構成](./media/active-directory-saas-tinfoil-security-tutorial/IC798969.png "Configure App URL")

4.  **Tinfoil Security でのシングル サインオンの構成** ] ページで、証明書をダウンロードする] をクリックして **証明書のダウンロード**, 、としては、証明書ファイルをローカルに保存および **c:\\Tinfoil Security.cer**します。

    ![Configure Single Sign-On](./media/active-directory-saas-tinfoil-security-tutorial/IC798970.png "Configure Single Sign-On")

5.  別の Web ブラウザーのウィンドウで、Tinfoil Security の企業サイトに管理者としてログインします。

6.  上部のツール バーの **[My Account]** をクリックします。

    ![ダッシュボード](./media/active-directory-saas-tinfoil-security-tutorial/IC798971.png "Dashboard")

7.  **[Security]** をクリックします。

    ![セキュリティ](./media/active-directory-saas-tinfoil-security-tutorial/IC798972.png "Security")

8.  [**シングル サインオン**] 構成ページで、次の手順を実行します。

    ![シングル サインオン](./media/active-directory-saas-tinfoil-security-tutorial/IC798973.png "Single Sign-On")

    1.  **[SAML の有効化]** を選択します。
    2.  **[Manual Configuration]** をクリックします。
    3.  Azure ポータルで、**[Tinfoil Security でのシングル サインオンの構成]** ダイアログ ページの **[SAML SSO URL]** の値をコピーし、**[SAML Post URL]** テキストボックスに貼り付けます。
    4.  エクスポートした証明書から **[サムプリント]** の値をコピーし、**[SAML 証明書フィンガープリント]** テキストボックスに貼り付けます。
        >[AZURE.TIP] 詳細については、「 [証明書拇印値を取得する方法](http://youtu.be/YKQF266SAxI)

    5.  **お使いのアカウント ID** をコピーします。
    6.  **[保存]** をクリックします。

9.  Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-tinfoil-security-tutorial/IC798974.png "Configure Single Sign-On")

10. 上部にあるメニュー [ **属性** を開くには、 **SAML トークン属性** ダイアログ。

    ![属性](./media/active-directory-saas-tinfoil-security-tutorial/IC795920.png "Attributes")

11. 必要な属性のマッピングを追加するには、次の手順を実行します。

    ![属性](./media/active-directory-saas-tinfoil-security-tutorial/IC798975.png "Attributes")

    1.  **[ユーザー属性の追加]** をクリックします。
    2.  **[属性名]** テキストボックスに、「**accountid**」と入力します。
    3.  **[属性地]** テキストボックスに、前のセクションでコピーしたアカウント ID 値を貼り付けます。
    4.  **[完了]** をクリックします。

12. **[変更の適用]** をクリックします。

## ユーザー プロビジョニングの構成

Azure AD ユーザーが Tinfoil Security にログインできるようにするには、そのユーザーを Tinfoil Security にプロビジョニングする必要があります。  
Tinfoil Security の場合、プロビジョニングは手動で行います。

### ユーザーをプロビジョニングするには、次の手順に従います。

1.  ユーザーがエンタープライズ アカウントに参加している場合は、Tinfoil Security サポート チームに連絡して、ユーザー アカウントを作成する必要があります。

2.  通常の Tinfoil Security SaaS ユーザーは、グループ作業者をユーザーの任意のサイトに追加できます。 ここでは、新しい Tinfoil Security ユーザー アカウントを作成するための招待状を指定した電子メール アドレスに送信するプロセスが起動します。

>[AZURE.NOTE] Tinfoil Security から提供されている他の Tinfoil Security ユーザー アカウント作成ツールまたは API を使用して、AAD ユーザー アカウントをプロビジョニングできます。

## ユーザーの割り当て

構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

### ユーザーを Tinfoil Security に割り当てるには、次の手順を実行します。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  **Tinfoil Security * * アプリケーション統合ページで、をクリックして * * ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-tinfoil-security-tutorial/IC798976.png "Assign Users")

3.  テスト ユーザーを選択し、**[割り当て]**、**[はい]** の順にクリックして、割り当てを確定します。

    ![はい](./media/active-directory-saas-tinfoil-security-tutorial/IC767830.png "Yes")

シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。




