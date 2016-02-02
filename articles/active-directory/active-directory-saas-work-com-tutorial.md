<properties 
    pageTitle="チュートリアル: Azure Active Directory と Work.com の統合 | Microsoft Azure" 
    description="Azure Active Directory で Work.com を使用して、シングル サインオンや自動プロビジョニングなどを有効にする方法について説明します。" 
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


# チュートリアル: Azure Active Directory と Work.com の統合

このチュートリアルでは、Azure と Work.com の統合について説明します。  
このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   Work.com でのシングル サインオンが有効なサブスクリプション

シングル サインオンできる Work.com 会社サイト (サービス プロバイダーが開始したサインオン) で、アプリケーションにまたはを使用してこのチュートリアルを完了すると、先がある AAD ユーザーが Work.com のアクセスを割り当てる、 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。

このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  Work.com のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-work-com-tutorial/IC794105.png "Scenario")

## Work.com のアプリケーション統合の有効化

このセクションでは、Work.com のアプリケーション統合を有効にする方法について説明します。

### Work.com のアプリケーション統合を有効にするには、次の手順に従います。

1.  Azure 管理ポータルの左側のナビゲーション ウィンドウで、**[Active Directory]** をクリックします。

    ![Active Directory](./media/active-directory-saas-work-com-tutorial/IC700993.png "Active Directory")

2.  **[ディレクトリ]** の一覧から、ディレクトリ統合を有効にするディレクトリを選択します。

3.  アプリケーション ビューを開くには、ディレクトリ ビューでトップ メニューの **[アプリケーション]** をクリックします。

    ![アプリケーション](./media/active-directory-saas-work-com-tutorial/IC700994.png "Applications")

4.  ページの下部にある **[追加]** をクリックします。

    ![アプリケーションの追加](./media/active-directory-saas-work-com-tutorial/IC749321.png "Add application")

5.  **[実行する内容]** ダイアログで、**[ギャラリーからアプリケーションを追加します]** をクリックします。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-work-com-tutorial/IC749322.png "Add an application from gallerry")

6.  **検索ボックス**に、「**Work.com**」と入力します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-work-com-tutorial/IC794106.png "Application Gallery")

7.  結果ウィンドウで **[Work.com]** を選択し、**[完了]** をクリックしてアプリケーションを追加します。

    ![Work.com](./media/active-directory-saas-work-com-tutorial/IC794107.png "Work.com")

## シングル サインオンの構成

このセクションでは、SAML プロトコルに基づくフェデレーションを使用して、ユーザーが Azure AD のアカウントで Work.com に対する認証を行うことができるようにする方法を説明します。  
この手順の途中で、証明書を Work.com.com にアップロードする必要があります。
>[AZURE.NOTE] シングル サインオンを構成する場合でも、カスタムの Work.com ドメイン名をセットアップする必要があります。 少なくともドメイン名を定義し、ドメイン名をテストしてから、組織全体にデプロイする必要があります。

### シングル サインオンを構成するには、次の手順を実行します。

1.  管理者として Work.com テナントにログインします。

2.  **[セットアップ]** に移動します。

    ![セットアップ](./media/active-directory-saas-work-com-tutorial/IC794108.png "Setup")

3.  左側のナビゲーション ウィンドウで、 **管理** ] をクリックして **ドメイン管理** に関連するセクションを展開し、クリックして **My Domain** を開くには、 **My Domain** ページです。

    ![マイ ドメイン](./media/active-directory-saas-work-com-tutorial/IC767825.png "My Domain")

4.  ドメインが正しくセットアップされたことを確認するに内にあることを確認します"**ステップ 4 は、ユーザーに対して展開された**"を確認し、"**マイ ドメイン設定**"です。

    ![ユーザーにデプロイされたドメイン](./media/active-directory-saas-work-com-tutorial/IC784377.png "Doman Deployed to User")

5.  別の Web ブラウザー ウィンドウで、Azure ポータルにログインします。

6.  **Work.com * * アプリケーション統合ページでは、をクリックして * * でのシングル サインオンを構成する** を開くには、 ** [シングル サインオン ** ] ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-work-com-tutorial/IC794109.png "Configure Single Sign-On")

7.  **どのようなユーザーが Work.com へのサインオン** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、] をクリックし、 **次**します。

    ![Configure Single Sign-On](./media/active-directory-saas-work-com-tutorial/IC794110.png "Configure Single Sign-On")

8.  **アプリケーション URL の構成** ] ページで、 **Work.com サインオン URL** URL は、ユーザーが Work.com アプリケーションにサインオンする使用] ボックスに、型 (例::" *http://company.my.salesforce.com*")、順にクリック **次**:

    ![アプリケーション URL の構成](./media/active-directory-saas-work-com-tutorial/IC794111.png "Configure App URL")

9.  **Work.com でのシングル サインオンの構成** ] ページで、証明書をダウンロードする] をクリックして **証明書のダウンロード**, 、お使いのコンピューターにローカルで証明書ファイルを保存します。

    ![Configure Single Sign-On](./media/active-directory-saas-work-com-tutorial/IC794112.png "Configure Single Sign-On")

10. Work.com テナントにログインします。

11. **[セットアップ]** に移動します。

    ![セットアップ](./media/active-directory-saas-work-com-tutorial/IC794108.png "Setup")

12. **[セキュリティ制御]** メニューを展開表示し、**[シングル サインオンの設定]** をクリックします。

    ![シングル サインオンの設定](./media/active-directory-saas-work-com-tutorial/IC794113.png "Single Sign-On Settings")

13. **[シングル サインオンの設定]** ダイアログ ページで、次の手順を実行します。

    ![SAML 有効](./media/active-directory-saas-work-com-tutorial/IC781026.png "SAML Enabled")

    1.  **[SAML Enabled]** を選択します。
    2.  **[新規]** をクリックします。

14. **[SAML シングル サインオンの設定]** セクションで、次の手順に従います。

    ![SAML シングル サインオンの設定](./media/active-directory-saas-work-com-tutorial/IC794114.png "SAML Single Sign-On Setting")

    1.  **[名前]** テキスト ボックスに、構成の名前を入力します。
        >[AZURE.NOTE] 値を提供する **名** では自動的に入力、 **API 名** ] ボックスに貼り付けます。

    2.  Azure ポータルの **[Work.com でのシングル サインオンの構成]** ダイアログ ページで **[発行者の URL]** の値をコピーし、それを **[発行者]** テキストボックスに貼り付けます。
    3.  **[参照]** をクリックして、ダウンロードした証明書をアップロードします。
    4.  **エンティティ Id** ] ボックスに「 **https://salesforce-work.com**します。
    5.  **[SAML ID の種類]** として、**[アサーションにはユーザー オブジェクトからのフェデレーション ID が含まれます]** を選択します。
    6.  **[SAML ID の場所]** で、**[ID を Subject ステートメントの NameIdentifier 要素にする]** チェックボックスをオンにします。
    7.  Azure ポータルで、**[Work.com でのシングル サインオンの構成]** ダイアログ ページの **[リモート ログイン URL]** の値をコピーし、**[ID プロバイダー ログイン URL]** ボックスに貼り付けます。
    8.  Azure ポータルで、**[Work.com でのシングル サインオンの構成]** ダイアログ ページの **[リモート ログアウト URL]** の値をコピーし、**[ID プロバイダー ログアウト URL]** ボックスに貼り付けます。
    9.  **[サービス プロバイダーが開始した要求のバインド]** で **[HTTP POST]** を選択します。
    10. **[保存]** をクリックします。

15. Work.com のポータルの左側のナビゲーション ウィンドウで、次のようにクリックします。 **ドメイン管理** に関連するセクションを展開し、クリック **My Domain** を開くには、 **My Domain** ページです。

    ![マイ ドメイン](./media/active-directory-saas-work-com-tutorial/IC794115.png "My Domain")

16. **My Domain** ] ページで、 **Login Page Branding** ] をクリックして **編集**します。

    ![ログイン ページのブランド](./media/active-directory-saas-work-com-tutorial/IC767826.png "Login Page Branding")

17. **Login Page Branding** ] ページで、 **認証サービス** セクションの名前、 **SAML SSO 設定** が表示されます。 クリックして選択し、 **保存**します。

    ![ログイン ページのブランド](./media/active-directory-saas-work-com-tutorial/IC784366.png "Login Page Branding")

18. Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-work-com-tutorial/IC794116.png "Configure Single Sign-On")

## ユーザー プロビジョニングの構成

Azure Active Directory ユーザーがサインインできるように、Azure Active Directory ユーザーを Work.com に対してプロビジョニングする必要があります。  
Work.com の場合、プロビジョニングは手動で行います。

### ユーザー プロビジョニングを構成するには、次の手順に従います。

1.  Work.com 企業サイトに管理者としてサインオンします。

2.  **[セットアップ]** に移動します。

    ![セットアップ](./media/active-directory-saas-work-com-tutorial/IC794108.png "Setup")

3.  移動して **ユーザーの管理 \ > ユーザー**します。

    ![Manage Users](./media/active-directory-saas-work-com-tutorial/IC784369.png "Manage Users")

4.  **[新しいユーザー]** をクリックします。

    ![すべてのユーザー](./media/active-directory-saas-work-com-tutorial/IC794117.png "All Users")

5.  [ユーザーの編集] セクションで、次の手順を実行します。

    ![ユーザーの編集](./media/active-directory-saas-work-com-tutorial/IC794118.png "User Edit")

    1.  関連するテキストボックスにプロビジョニングする有効な Azure Active Directory アカウントの **[姓]**、**[エイリアス]**、**[電子メール]**、**[ユーザー名]**、**[ニックネーム]** の属性を入力します。
    2.  **[ロール]**、**[ユーザー ライセンス]**、**[プロファイル]** を選択します。
    3.  **[保存]** をクリックします。
        >[AZURE.NOTE] Azure Active Directory のアカウント所有者には、そのアカウントがアクティブになる前に、アカウント確認用のリンクを含む電子メールが送信されます。

>[AZURE.NOTE] Work.com から提供されている他の Work.com ユーザー アカウント作成ツールまたは API を使用して、AAD ユーザー アカウントをプロビジョニングできます。

## ユーザーの割り当て

構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

### ユーザーを Work.com に割り当てるには、次の手順を実行します。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  Work.com アプリケーション統合ページでクリックして **ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-work-com-tutorial/IC794119.png "Assign Users")

3.  テスト ユーザーを選択し、**[割り当て]**、**[はい]** の順にクリックして、割り当てを確定します。

    ![はい](./media/active-directory-saas-work-com-tutorial/IC767830.png "Yes")

ここで 10 分間待機し、アカウントが Work.com に同期されたことを確認します。

シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。




