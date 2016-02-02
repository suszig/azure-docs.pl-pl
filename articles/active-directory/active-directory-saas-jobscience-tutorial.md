<properties 
    pageTitle="チュートリアル: Azure Active Directory と Jobscience の統合 | Microsoft Azure" 
    description="Azure Active Directory と Jobscience を使用して、シングル サインオン、自動化されたプロビジョニング、および詳細を有効にする方法を説明します。" 
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


# チュートリアル: Azure Active Directory と Jobscience の統合

このチュートリアルの目的は、Azure と Jobscience の統合を示すことです。  
このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   Jobscience でのシングル サインオンが有効なサブスクリプション

このチュートリアルを完了すると、Jobscience に割り当てた Azure AD ユーザーは、Jobscience の会社サイト (サービス プロバイダーが開始したサインオン) で、アプリケーションにシングル サインオンまたはできるを使用して、 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。

このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  Jobscience のアプリ統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-jobscience-tutorial/IC784341.png "Scenario")
## Jobscience のアプリ統合の有効化

このセクションでは、Jobscience のアプリ統合を有効にする方法を説明します。

### Jobscience のアプリ統合を有効にするには、次の手順に従います。

1.  Azure 管理ポータルの左側のナビゲーション ウィンドウで、**[Active Directory]** をクリックします。

    ![Active Directory](./media/active-directory-saas-jobscience-tutorial/IC700993.png "Active Directory")

2.  **[ディレクトリ]** の一覧から、ディレクトリ統合を有効にするディレクトリを選択します。

3.  アプリケーション ビューを開くには、ディレクトリ ビューでトップ メニューの **[アプリケーション]** をクリックします。

    ![アプリケーション](./media/active-directory-saas-jobscience-tutorial/IC700994.png "Applications")

4.  ページの下部にある **[追加]** をクリックします。

    ![アプリケーションの追加](./media/active-directory-saas-jobscience-tutorial/IC749321.png "Add application")

5.  **[実行する内容]** ダイアログで、**[ギャラリーからアプリケーションを追加します]** をクリックします。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-jobscience-tutorial/IC749322.png "Add an application from gallerry")

6.  **検索ボックス**に、「**jobscience**」と入力します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-jobscience-tutorial/IC784342.png "Application Gallery")

7.  結果ウィンドウで **[Jobscience]** を選択し、**[完了]** をクリックしてアプリを追加します。

    ![Jobscience](./media/active-directory-saas-jobscience-tutorial/IC784357.png "Jobscience")
## シングル サインオンの構成

このセクションでは、ユーザーが SAML プロトコルに基づくフェデレーションを使用して、Azure AD でのユーザーのアカウントで Jobscience に対する認証を行えるようにする方法を説明します。  
Jobscience のシングル サインオンを構成するには、証明書から拇印の値を取得する必要があります。  
この手順に慣れていない場合は、次を参照してください。 [証明書の拇印の値を取得する方法](http://youtu.be/YKQF266SAxI)します。

### シングル サインオンを構成するには、次の手順に従います。

1.  Jobscience の企業サイトに管理者としてログインします。

2.  **[セットアップ]** に移動します。

    ![セットアップ](./media/active-directory-saas-jobscience-tutorial/IC784358.png "Setup")

3.  左側のナビゲーション ウィンドウで、 **管理** ] をクリックして **ドメイン管理** に関連するセクションを展開し、クリックして **My Domain** を開くには、 **My Domain** ページです。

    ![マイ ドメイン](./media/active-directory-saas-jobscience-tutorial/IC767825.png "My Domain")

4.  ドメインが正しくセットアップされたことを確認するに内にあることを確認します"**ステップ 4 は、ユーザーに対して展開された**"を確認し、"**マイ ドメイン設定**"です。

    ![ユーザーにデプロイされたドメイン](./media/active-directory-saas-jobscience-tutorial/IC784377.png "Doman Deployed to User")

5.  別の Web ブラウザー ウィンドウで、Azure ポータルにログインします。

6.  **Jobscience** アプリケーション統合ページでは、をクリックして **でのシングル サインオンを構成する** を開くには、 ** [シングル サインオン ** ] ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-jobscience-tutorial/IC784360.png "Configure Single Sign-On")

7.  **どのようなユーザーが Jobscience へのサインオン** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、順にクリック **次**します。

    ![Configure Single Sign-On](./media/active-directory-saas-jobscience-tutorial/IC784361.png "Configure Single Sign-On")

8.  **アプリケーション URL の構成** ] ページの [、 **Jobscience サインイン URL** ] ボックスに、次のパターンを使用して URL を入力"*http://company.my.salesforce.com*"、] をクリックし、 **次**します。

    ![アプリケーション URL の構成](./media/active-directory-saas-jobscience-tutorial/IC784362.png "Configure App URL")

9.  **Jobscience でのシングル サインオンの構成** ] ページで、証明書をダウンロードする] をクリックして **証明書のダウンロード**, 、お使いのコンピューターにローカルで証明書ファイルを保存します。

    ![Configure Single Sign-On](./media/active-directory-saas-jobscience-tutorial/IC784363.png "Configure Single Sign-On")

10. Jobscience の企業サイトで、**[セキュリティ コントロール]** をクリックしてから、**[シングル サインオンの設定]** をクリックします。

    ![セキュリティ コントロール](./media/active-directory-saas-jobscience-tutorial/IC784364.png "Security Controls")

11. **[シングル サインオンの設定]** セクションで、次の手順に従います。

    ![シングル サインオンの設定](./media/active-directory-saas-jobscience-tutorial/IC781026.png "Single Sign-On Settings")

    1.  **[SAML Enabled]** を選択します。
    2.  **[新規]** をクリックします。

12. **[SAML シングル サインオン設定の編集]** ページで、次の手順に従います。

    ![SAML シングル サインオンの設定](./media/active-directory-saas-jobscience-tutorial/IC784365.png "SAML Single Sign-On Setting")

    1.  **[名前]** テキスト ボックスに、構成の名前を入力します。
    2.  Azure ポータルの **[Jobscience でのシングル サインオンの構成]** ダイアログ ページで **[発行者 URL]** の値をコピーし、それを **[発行者]** テキストボックスに貼り付けます。
    3.  **エンティティ Id** ] ボックスに「 **https://salesforce-jobscience.com**
    4.  **[参照]** をクリックして、Azure AD の証明書をアップロードします。
    5.  **[SAML ID の種類]** として、**[アサーション には、ユーザー オブジェクトからのフェデレーション ID が含まれます]** を選択します。
    6.  **[SAML ID の場所]** として、**[ID を Subject ステートメントの NameIdentifier 要素にする]** チェックボックスをオンにします。
    7.  Azure ポータルの **[Jobscience でのシングル サインオンの構成]** ダイアログ ページで、**[リモート ログイン URL]** の値をコピーし、****[ID プロバイダー ログイン URL]**** ボックスに貼り付けます。
    8.  Azure ポータルの **[Jobscience でのシングル サインオンの構成]** ダイアログ ページで、**[リモート ログアウト URL]** の値をコピーし、****[ID プロバイダー ログアウト URL]**** ボックスに貼り付けます。
    9.  **[保存]** をクリックします。

13. 左側のナビゲーション ウィンドウで、 **管理** ] をクリックして **ドメイン管理** に関連するセクションを展開し、クリックして **My Domain** を開くには、 **My Domain** ページです。

    ![マイ ドメイン](./media/active-directory-saas-jobscience-tutorial/IC767825.png "My Domain")

14. **My Domain** ] ページで、 **Login Page Branding** ] をクリックして **編集**します。

    ![ログイン ページのブランド](./media/active-directory-saas-jobscience-tutorial/IC767826.png "Login Page Branding")

15. **Login Page Branding** ] ページで、 **認証サービス** セクションの名前、 **SAML SSO 設定** が表示されます。 クリックして選択し、 **保存**します。

    ![ログイン ページのブランド](./media/active-directory-saas-jobscience-tutorial/IC784366.png "Login Page Branding")

16. Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-jobscience-tutorial/IC784367.png "Configure Single Sign-On")

SP によって開始されるシングル サインオンのログイン URL を取得するには、**[Security Controls (セキュリティ コントロール)**] メニュー セクションの **[シングル サインオンの設定]** をクリックします。

![セキュリティ コントロール](./media/active-directory-saas-jobscience-tutorial/IC784368.png "Security Controls")

前の手順で作成した SSO プロファイルをクリックします。  
このページの URL で、会社のシングル サインオンを表示する (例: *https://companyname.my.salesforce.com?so=companyid*)。
## ユーザー プロビジョニングの構成

Azure AD ユーザーが Jobscience にログインできるようにするには、そのユーザーを Jobscience にプロビジョニングする必要があります。  
Jobscience の場合、プロビジョニングは手動で行います。

### ユーザー プロビジョニングを構成するには、次の手順に従います。

1.  **Jobscience** の企業サイトに管理者としてログインします。

2.  [セットアップ] に移動します。

    ![セットアップ](./media/active-directory-saas-jobscience-tutorial/IC784358.png "Setup")

3.  移動して **ユーザーの管理 \ > ユーザー**します。

    ![ユーザー](./media/active-directory-saas-jobscience-tutorial/IC784369.png "Users")

4.  **[新しいユーザー]** をクリックします。

    ![すべてのユーザー](./media/active-directory-saas-jobscience-tutorial/IC784370.png "All Users")

5.  **[ユーザーの編集]** ダイアログで、次の手順に従います。

    ![ユーザーの編集](./media/active-directory-saas-jobscience-tutorial/IC784371.png "User Edit")

    1.  名、姓、名、エイリアス、メール、プロビジョニングする Azure AD ユーザーのユーザー名とニックネームのプロパティを該当するボックスに入力します。
    2.  **[保存]** をクリックします。

    >[AZURE.NOTE] Azure AD のアカウント所有者には、アカウントがアクティブになる前に、アカウント確認用のリンクを含むメールが送信されます。

>[AZURE.NOTE] 他の Jobscience ユーザー アカウント作成ツールや Jobscience から提供されている API を使用して、AAD ユーザー アカウントをプロビジョニングできます。

## ユーザーを割り当てる

構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

### ユーザーを Jobscience に割り当てるには、次の手順に従います。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  **Jobscience * * アプリケーション統合ページで、をクリックして * * ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-jobscience-tutorial/IC784372.png "Assign Users")

3.  テスト ユーザーを選択し、**[割り当て]**、**[はい]** の順にクリックして、割り当てを確定します。

    ![はい](./media/active-directory-saas-jobscience-tutorial/IC767830.png "Yes")

シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。




