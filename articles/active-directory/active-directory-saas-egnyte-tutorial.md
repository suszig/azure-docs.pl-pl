<properties 
    pageTitle="チュートリアル: Azure Active Directory と Egnyte の統合 | Microsoft Azure" 
    description="Azure Active Directory と Egnyte を使用して、シングル サインオン、自動化されたプロビジョニング、および詳細を有効にする方法を説明します。" 
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


# チュートリアル: Azure Active Directory と Egnyte の統合

このチュートリアルでは、Azure と Egnyte の統合について説明します。  
このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   Egnyte でのシングル サインオンが有効なサブスクリプション

このチュートリアルを完了すると、Egnyte に割り当てた Azure AD ユーザーができるよう、Egnyte 会社サイト (サービス プロバイダーが開始したサインオン)、または、を使用して、アプリケーションにシングル サインオン [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)

このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  Egnyte のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-egnyte-tutorial/IC787812.png "Scenario")
## Egnyte のアプリケーション統合の有効化

このセクションでは、Egnyte のアプリケーション統合を有効にする方法について説明します。

### Egnyte のアプリケーション統合を有効にするには、次の手順を実行します。

1.  Azure 管理ポータルの左側のナビゲーション ウィンドウで、**[Active Directory]** をクリックします。

    ![Active Directory](./media/active-directory-saas-egnyte-tutorial/IC700993.png "Active Directory")

2.  **[ディレクトリ]** の一覧から、ディレクトリ統合を有効にするディレクトリを選択します。

3.  アプリケーション ビューを開くには、ディレクトリ ビューでトップ メニューの **[アプリケーション]** をクリックします。

    ![アプリケーション](./media/active-directory-saas-egnyte-tutorial/IC700994.png "Applications")

4.  ページの下部にある **[追加]** をクリックします。

    ![アプリケーションの追加](./media/active-directory-saas-egnyte-tutorial/IC749321.png "Add application")

5.  **[実行する内容]** ダイアログで、**[ギャラリーからアプリケーションを追加します]** をクリックします。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-egnyte-tutorial/IC749322.png "Add an application from gallerry")

6.  **検索ボックス**に、「**Egnyte**」と入力します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-egnyte-tutorial/IC787813.png "Application Gallery")

7.  結果ウィンドウで **[Egnyte]** を選択し、**[完了]** をクリックしてアプリケーションを追加します。

    ![Egnyte](./media/active-directory-saas-egnyte-tutorial/IC787814.png "Egnyte")
## シングル サインオンの構成

このセクションでは、ユーザーが SAML プロトコルに基づくフェデレーションを使用して、Azure AD でのユーザーのアカウントで Egnyte に対する認証を行うことができるようにする方法を説明します。  
この手順の途中で、base-64 でエンコードされた証明書ファイルを作成する必要があります。  
この手順に慣れていない場合は、次を参照してください。 [バイナリ証明書をテキスト ファイルに変換する方法](http://youtu.be/PlgrzUZ-Y1o)します。

### シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **Egnyte** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、 ** [シングル サインオン ** ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-egnyte-tutorial/IC787815.png "Configure Single Sign-On")

2.  **どのようなユーザーが Egnyte へのサインオン** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、順にクリック **次**します。

    ![Configure Single Sign-On](./media/active-directory-saas-egnyte-tutorial/IC787816.png "Configure Single Sign-On")

3.  **アプリケーション URL の構成** ] ページの [、 **Egnyte サインイン URL** ] ボックスに、次のパターンを使用して URL を入力"*https://company.egnyte.com*"、] をクリックし、 **次**します。

    ![アプリケーション URL の構成](./media/active-directory-saas-egnyte-tutorial/IC787817.png "Configure App URL")

4.  **Egnyte でのシングル サインオンの構成** ] ページで [ **証明書のダウンロード**, 、お使いのコンピューターに証明書ファイルを保存します。

    ![Configure Single Sign-On](./media/active-directory-saas-egnyte-tutorial/IC787818.png "Configure Single Sign-On")

5.  別の Web ブラウザー ウィンドウで、Egnyte 企業サイトに管理者としてログインします。

6.  **[Settings]** をクリックします。

    ![設定](./media/active-directory-saas-egnyte-tutorial/IC787819.png "Settings")

7.  メニューで **[設定]** をクリックします。

    ![設定](./media/active-directory-saas-egnyte-tutorial/IC787820.png "Settings")

8.  **[構成]** タブをクリックし、**[セキュリティ]** をクリックします。

    ![セキュリティ](./media/active-directory-saas-egnyte-tutorial/IC787821.png "Security")

9.  **[シングル サインオン認証]** セクションで、次の手順を実行します。

    ![シングル サインオン認証](./media/active-directory-saas-egnyte-tutorial/IC787822.png "Single Sign On Authentication")

    1.  **[シングル サインオン認証]** として **[SAML 2.0]** を選択します。
    2.  **[ID プロバイダー]** として **[AzureAD]** を選択します。
    3.  Azure ポータルで、 **Egnyte でのシングル サインオンの構成** ] ダイアログ ページでコピー、 **リモート ログイン URL** 値に設定して、貼り付けます、 ** Id プロバイダー ログイン URL ** ] ボックスに貼り付けます。
    4.  Azure ポータルで、**[Egnyte でのシングル サインオンの構成]** ダイアログ ページの **[エンティティ ID]** の値をコピーし、**[ID プロバイダー エンティティ ID]** ボックスに貼り付けます。
    5.  ダウンロードした証明書から **base-64 でエンコードされた**ファイルを作成します。
        >[AZURE.TIP]詳細については、「 [バイナリ証明書をテキスト ファイルに変換する方法](http://youtu.be/PlgrzUZ-Y1o)

    6.  base-64 でエンコードされた証明書をメモ帳で開き、その内容をクリップボードにコピーして、**[ID プロバイダー証明書]** ボックスに貼り付けます。
    7.  **[既定のユーザー マッピング]** として **[電子メール アドレス]** を選択します。
    8.  **[ドメイン固有の発行者の値を使用]** として **[無効]** を選択します。
    9.  **[保存]** をクリックします。

10. Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-egnyte-tutorial/IC787823.png "Configure Single Sign-On")
## ユーザー プロビジョニングの構成

Azure AD ユーザーが Egnyte にログインできるようにするには、そのユーザーを Egnyte にプロビジョニングする必要があります。  
Egnyte の場合、プロビジョニングは手動で行います。

### ユーザー アカウントをプロビジョニングするには、次の手順を実行します。

1.  **Egnyte** 企業サイトに管理者としてログインします。

2.  移動して **設定 \ > ユーザーおよびグループ**します。

3.  **[新しいユーザーの追加]** をクリックし、追加するユーザーの種類を選択します。

    ![ユーザー](./media/active-directory-saas-egnyte-tutorial/IC787824.png "Users")

4.  **[新しい標準ユーザー]** セクションで、次の手順を実行します。

    ![新しい標準ユーザー](./media/active-directory-saas-egnyte-tutorial/IC787825.png "New Standard User")

    1.  プロビジョニングする有効な Azure Active Directory アカウントの、**電子メール**、**ユーザー名**などの詳細を入力します。
    2.  **[保存]** をクリックします。

    >[AZURE.NOTE] Azure Active Directory アカウント保有者に通知メールが届きます。

>[AZURE.NOTE] Egnyte から提供されている他の Egnyte ユーザー アカウント作成ツールまたは API を使用して、AAD ユーザー アカウントをプロビジョニングできます。

## ユーザーの割り当て

構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

### ユーザーを Egnyte に割り当てるには、次の手順を実行します。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  **Egnyte * * アプリケーション統合ページで、をクリックして * * ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-egnyte-tutorial/IC787826.png "Assign Users")

3.  テスト ユーザーを選択し、**[割り当て]**、**[はい]** の順にクリックして、割り当てを確定します。

    ![はい](./media/active-directory-saas-egnyte-tutorial/IC767830.png "Yes")

シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。




