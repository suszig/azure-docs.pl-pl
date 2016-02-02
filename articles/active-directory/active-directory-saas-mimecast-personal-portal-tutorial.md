<properties 
    pageTitle="チュートリアル: Azure Active Directory と Mimecast Personal Portal の統合 | Microsoft Azure" 
    description="Azure Active Directory で Mimecast Personal Portal を使用して、シングル サインオン、自動化されたプロビジョニング、および詳細を有効にする方法を説明します。" 
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


# チュートリアル: Azure Active Directory と Mimecast Personal Portal の統合

このチュートリアルでは、Azure と Mimecast Personal Portal の統合について説明します。  
このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   Mimecast Personal Portal でのシングル サインオンが有効なサブスクリプション

このチュートリアルを完了すると、Mimecast Personal Portal に割り当てた Azure AD ユーザーは、Mimecast Personal Portal 会社サイト (サービス プロバイダーが開始したサインオン) で、アプリケーションにシングル サインオンまたはできるを使用して、 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。

このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  Mimecast Personal Portal のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-mimecast-personal-portal-tutorial/IC794991.png "Scenario")
## Mimecast Personal Portal のアプリケーション統合の有効化

このセクションでは、Mimecast Personal Portal のアプリケーション統合を有効にする方法を説明します。

### Mimecast Personal Portal のアプリケーション統合を有効にするには、次の手順に従います。

1.  Azure 管理ポータルの左側のナビゲーション ウィンドウで、**[Active Directory]** をクリックします。

    ![Active Directory](./media/active-directory-saas-mimecast-personal-portal-tutorial/IC700993.png "Active Directory")

2.  **[ディレクトリ]** の一覧から、ディレクトリ統合を有効にするディレクトリを選択します。

3.  アプリケーション ビューを開くには、ディレクトリ ビューでトップ メニューの **[アプリケーション]** をクリックします。

    ![アプリケーション](./media/active-directory-saas-mimecast-personal-portal-tutorial/IC700994.png "Applications")

4.  ページの下部にある **[追加]** をクリックします。

    ![アプリケーションの追加](./media/active-directory-saas-mimecast-personal-portal-tutorial/IC749321.png "Add application")

5.  **[実行する内容]** ダイアログで、**[ギャラリーからアプリケーションを追加します]** をクリックします。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-mimecast-personal-portal-tutorial/IC749322.png "Add an application from gallerry")

6.  **[検索ボックス]** に、**[Mimecast Personal Portal]** と入力します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-mimecast-personal-portal-tutorial/IC794992.png "Application Gallery")

7.  結果ウィンドウで **[Mimecast Personal Portal]** を選択し、**[完了]** をクリックしてアプリケーションを追加します。

    ![Mimecast Personal Portal](./media/active-directory-saas-mimecast-personal-portal-tutorial/IC794993.png "Mimecast Personal Portal")
## シングル サインオンの構成

このセクションでは、ユーザーが SAML プロトコルに基づくフェデレーションを使用して、Azure AD でのユーザーのアカウントで Mimecast Personal Portal に対する認証を行うことができるようにする方法を説明します。  
この手順の途中で、base-64 でエンコードされた証明書ファイルを作成する必要があります。  
この手順に慣れていない場合は、次を参照してください。 [バイナリ証明書をテキスト ファイルに変換する方法](http://youtu.be/PlgrzUZ-Y1o)します。

### シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **Mimecast Personal Portal** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、 ** [シングル サインオン ** ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-mimecast-personal-portal-tutorial/IC794994.png "Configure Single Sign-On")

2.  **どのようなユーザーが Mimecast Personal Portal へのサインオン** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、] をクリックし、 **次**します。

    ![Configure Single Sign-On](./media/active-directory-saas-mimecast-personal-portal-tutorial/IC794995.png "Configure Single Sign-On")

3.  **アプリケーション URL の構成** ] ページの [、 **Mimecast Personal Portal サインオン URL** Mimecast Personal Portal アプリケーションにサインオンする URL は、ユーザーが使用] ボックスに、型 (例::"https://webmail-uk.mimecast.com"や"https://webmail-us.mimecast.com")、順にクリック **次**します。
    >[AZURE.NOTE] サインオン URL は、リージョン固有のものになります。

    ![アプリケーション URL の構成](./media/active-directory-saas-mimecast-personal-portal-tutorial/IC794996.png "Configure App URL")

4.  **Mimecast Personal Portal でのシングル サインオンの構成** ] ページで、証明書をダウンロードする] をクリックして **証明書のダウンロード**, 、お使いのコンピューターにローカルで証明書ファイルを保存します。

    ![Configure Single Sign-On](./media/active-directory-saas-mimecast-personal-portal-tutorial/IC794997.png "Configure Single Sign-On")

5.  別の Web ブラウザー ウィンドウで、Mimecast Personal Portal の企業サイトに管理者としてログインします。

6.  移動して **サービス \ > アプリケーション**します。

    ![アプリケーション](./media/active-directory-saas-mimecast-personal-portal-tutorial/IC794998.png "Applications")

7.  **[認証プロファイル]** をクリックします。

    ![認証プロファイル](./media/active-directory-saas-mimecast-personal-portal-tutorial/IC794999.png "Authentication Profiles")

8.  **[新しい認証プロファイル]** をクリックします。

    ![新しい認証プロファイル](./media/active-directory-saas-mimecast-personal-portal-tutorial/IC795000.png "New Authentication Profil")

9.  [**認証プロバイダー**] セクションで、次の手順に従います。

    ![認証プロファイル](./media/active-directory-saas-mimecast-personal-portal-tutorial/IC795001.png "Authentication Profil")

    1.  **[説明]** テキストボックスに、構成の名前を入力します。
    2.  **[Mimecast Personal Portal に SAML 認証を適用]** を選択します。
    3.  **[プロバイダー]** で **[Azure Active Directory]** を選択します。
    4.  Azure ポータルの **[Mimecast Personal Portal でのシングル サインオンの構成]** ダイアログ ページで **[発行者 URL]** の値をコピーし、それを **[発行者 URL]** テキストボックスに貼り付けます。
    5.  Azure ポータルの **[Mimecast Personal Portal でのシングル サインオンの構成]** ダイアログ ページで **[リモート ログイン URL]** の値をコピーし、それを **[ログイン URL]** テキストボックスに貼り付けます。
    6.  Azure ポータルの **[Mimecast Personal Portal でのシングル サインオンの構成]** ダイアログ ページで **[リモート ログイン URL]** の値をコピーし、それを **[ログアウト URL]** テキストボックスに貼り付けます。
        >[AZURE.NOTE] Mimecast Personal Portal では、[ログイン URL] と [ログアウト URL] の値は同じです。

    7.  ダウンロードした証明書から **base-64 でエンコードされた**ファイルを作成します。
        >[AZURE.TIP]詳細については、次を参照してください。 [バイナリ証明書をテキスト ファイルに変換する方法](http://youtu.be/PlgrzUZ-Y1o)します。

    8.  開いているメモ帳で、base-64 でエンコードされた証明書、最初の行を削除 ("*--*") と最後の行 ("*--*")、その残りのコンテンツをクリップボードにコピーおよびに貼り付け、 **Id プロバイダー証明書 (メタデータ)** ] ボックスに貼り付けます。
    9.  **[シングル サインオンの許可]** を選択します。
    10. **[保存]** をクリックします。

10. Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-mimecast-personal-portal-tutorial/IC795002.png "Configure Single Sign-On")
## ユーザー プロビジョニングの構成

Azure AD ユーザーが Mimecast Personal Portal にログインできるようにするには、そのユーザーを Mimecast Personal Portal にプロビジョニングする必要があります。  
Mimecast Personal Portal の場合、プロビジョニングは手動で行います。

ユーザーを作成する前に、ドメインを登録する必要があります。

### ユーザー プロビジョニングを構成するには、次の手順に従います。

1.  管理者として **[Mimecast Personal Portal]** にサインオンします。

2.  移動して **ディレクトリ \ > 内部**します。

    ![ディレクトリ](./media/active-directory-saas-mimecast-personal-portal-tutorial/IC795003.png "Directories")

3.  **[新規ドメインの登録]** をクリックします。

    ![新規ドメインの登録](./media/active-directory-saas-mimecast-personal-portal-tutorial/IC795004.png "Register New Domain")

4.  新しいドメインを作成したら、**[新規アドレス]** をクリックします。

    ![新規アドレス](./media/active-directory-saas-mimecast-personal-portal-tutorial/IC795005.png "New Address")

5.  新しいアドレスのダイアログで、次の手順に従います。

    ![保存](./media/active-directory-saas-mimecast-personal-portal-tutorial/IC795006.png "Save")

    1.  該当するテキストボックスに、プロビジョニングする有効な AAD アカウントの **[メール アドレス]**、**[グローバル名]**、**[パスワード]**、**[パスワードの確認]** 属性を入力します。
    2.  **[保存]** をクリックします。

>[AZURE.NOTE]Mimecast Personal Portal から提供されている他の IdeaScale ユーザー アカウント作成ツールや API を使用して、AAD ユーザー アカウントをプロビジョニングできます。

## ユーザーを割り当てる

構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

### ユーザーを Mimecast Personal Portal に割り当てるには、次の手順に従います。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  **Mimecast Personal Portal * * アプリケーション統合ページで、をクリックして * * ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-mimecast-personal-portal-tutorial/IC795007.png "Assign Users")

3.  テスト ユーザーを選択し、**[割り当て]**、**[はい]** の順にクリックして、割り当てを確定します。

    ![はい](./media/active-directory-saas-mimecast-personal-portal-tutorial/IC767830.png "Yes")

シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。




