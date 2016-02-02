<properties 
    pageTitle="チュートリアル: Azure Active Directory と Igloo Software の統合 | Microsoft Azure" 
    description="Azure Active Directory で Igloo Software を使用して、シングル サインオンや自動プロビジョニングなどを有効にする方法を説明します。" 
    services="active-directory" 
    authors="jeevansd"  
    documentationCenter="na" 
    manager="prasannas"/>
<tags 
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="12/04/2015" 
    ms.author="jeedes" />


# チュートリアル: Azure Active Directory と Igloo Software の統合

このチュートリアルでは、Azure と Igloo Software の統合について説明します。  
このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   [Igloo Software](http://www.igloosoftware.com/) シングル サインオンが有効なサブスクリプションで

このチュートリアルを完了すると、Igloo Software に割り当てた Azure AD ユーザーは、Igloo Software 会社サイト (サービス プロバイダーが開始したサインオン) で、アプリケーションにシングル サインオンまたはできるを使用して、 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。

このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  Igloo Software のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-igloo-software-tutorial/IC783961.png "Scenario")
## Igloo Software のアプリケーション統合の有効化

このセクションでは、Igloo Software のアプリケーション統合を有効にする方法を説明します。

### Igloo Software のアプリケーション統合を有効にするには、次の手順を実行します。

1.  Azure クラシック ポータルの左側のナビゲーション ウィンドウで、**[Active Directory]** をクリックします。

    ![Active Directory](./media/active-directory-saas-igloo-software-tutorial/IC700993.png "Active Directory")

2.  **[ディレクトリ]** の一覧から、ディレクトリ統合を有効にするディレクトリを選択します。

3.  アプリケーション ビューを開くには、ディレクトリ ビューでトップ メニューの **[アプリケーション]** をクリックします。

    ![アプリケーション](./media/active-directory-saas-igloo-software-tutorial/IC700994.png "Applications")

4.  ページの下部にある **[追加]** をクリックします。

    ![アプリケーションの追加](./media/active-directory-saas-igloo-software-tutorial/IC749321.png "Add application")

5.  **[実行する内容]** ダイアログで、**[ギャラリーからアプリケーションを追加します]** をクリックします。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-igloo-software-tutorial/IC749322.png "Add an application from gallerry")

6.  **検索ボックス**に「**Igloo Software**」と入力します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-igloo-software-tutorial/IC783962.png "Application Gallery")

7.  結果ウィンドウで **[Igloo Software]** を選択し、**[完了]** をクリックしてアプリケーションを追加します。

    ![Igloo](./media/active-directory-saas-igloo-software-tutorial/IC783963.png "Igloo")
## シングル サインオンの構成

このセクションでは、ユーザーが SAML プロトコルに基づくフェデレーションを使用して、Azure AD でのユーザーのアカウントで Igloo Software に対する認証を行うことができるようにする方法を説明します。  
この手順の途中で、base-64 でエンコードされた証明書を Central Desktop テナントにアップロードする必要があります。  
この手順に慣れていない場合は、次を参照してください。 [バイナリ証明書をテキスト ファイルに変換する方法](http://youtu.be/PlgrzUZ-Y1o)します。

### シングル サインオンを構成するには、次の手順に従います。

1.  Azure クラシック ポータルでの **Igloo Software** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、 ** [シングル サインオン ** ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-igloo-software-tutorial/IC783964.png "Configure Single Sign-On")

2.  **どのようなユーザーを Igloo Software にサインオンする** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、] をクリックし、 **次**します。

    ![Microsoft Azure AD シングル サインオン](./media/active-directory-saas-igloo-software-tutorial/IC783965.png "Microsoft Azure AD Single Sign-On")

3.  **アプリケーション URL の構成** ] ページの [、 **Igloo Software サインイン URL** ] ボックスに、次のパターンを使用して URL を入力"*https://company.igloocommunities.com/?signin*"、] をクリックし、 **次**します。

    ![アプリケーション URL の構成](./media/active-directory-saas-igloo-software-tutorial/IC773625.png "Configure App URL")

4.  **Igloo Software でのシングル サインオンの構成** ] ページで、証明書をダウンロードする] をクリックして **証明書のダウンロード**, 、お使いのコンピューターにローカルで証明書ファイルを保存します。

    ![Configure Single Sign-On](./media/active-directory-saas-igloo-software-tutorial/IC783966.png "Configure Single Sign-On")

5.  別の Web ブラウザーのウィンドウで、管理者として Igloo Software 企業サイトにログインします。

6.  **コントロール パネル**に移動します。

    ![コントロール パネル](./media/active-directory-saas-igloo-software-tutorial/IC799949.png "Control Panel")

7.  **[メンバーシップ]** タブで **[サインインの設定]** をクリックします。

    ![サインインの設定](./media/active-directory-saas-igloo-software-tutorial/IC783968.png "Sign in Settings")

8.  SAML の構成のセクションで、**[SAML 認証の構成]** をクリックします。

    ![SAML の構成](./media/active-directory-saas-igloo-software-tutorial/IC783969.png "SAML Configuration")

9.  **[全般構成]** セクションで、次の手順を実行します。

    ![全般構成](./media/active-directory-saas-igloo-software-tutorial/IC783970.png "General Configuration")

    1.  **[接続名]** テキストボックスに、構成のカスタム名を入力します。
    2.  Azure クラシック ポータルで、**[Igloo Software でのシングル サインオンの構成]** ダイアログ ページの **[リモート ログイン URL]** の値をコピーし、**[IdP ログイン URL]** テキストボックスに貼り付けます。
    3.  Azure クラシック ポータルで、**[Igloo Software でのシングル サインオンの構成]** ダイアログ ページの **[リモート ログアウト URL]** の値をコピーし、**[IdP Logout URL]** ボックスに貼り付けます。
    4.  **[ログアウト応答と要求 HTTP のタイプ]** として、**[POST]** を選択します。
    5.  ダウンロードした証明書からテキスト ファイルを作成します。
        >[AZURE.TIP]詳細については、「 [バイナリ証明書をテキスト ファイルに変換する方法](http://youtu.be/PlgrzUZ-Y1o)

    6.  最初の行と最後の行を証明書のテキスト ファイル バージョンから削除し、残りの証明書テキストをコピーして、それを **[公開証明書]** ボックスに貼り付けます。

10. **[応答および認証の構成]** で、次の手順を実行します。

    ![応答および認証の構成](./media/active-directory-saas-igloo-software-tutorial/IC783971.png "Response and Authentication Configuration")

    1.  **[ID プロバイダー]** として **[Microsoft ADFS]** を選択します。
    2.  **[識別子の種類]** として **[電子メール アドレス]** を選択します。
    3.  **[電子メール属性]** テキストボックスに、「**emailaddress**」と入力します。
    4.  **[名属性]** テキストボックスに、「**givenname**」と入力します。
    5.  **[姓属性]** テキストボックスに、「**surname**」と入力します。

11. 次の手順を実行して、構成を完成させます。

    ![サインインでユーザー作成](./media/active-directory-saas-igloo-software-tutorial/IC783972.png "User creation on Sign in")

    1.  **[サインインでユーザー作成]** で **[サインイン時に新しいユーザーをサイトに作成]** を選択します。
    2.  **[サインインの設定]** で、**[[サインイン] 画面で SAML ボタンを使用する]** を選択します。
    3.  **[保存]** をクリックします。

12. Azure クラシック ポータルで、シングル サインオンの構成情報を選択し、をクリックして **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-igloo-software-tutorial/IC783973.png "Configure Single Sign-On")
## ユーザー プロビジョニングの構成

Igloo Software へのユーザー プロビジョニングを構成するときに必要な操作はありません。  
割り当てられたユーザーがアクセス パネルを使用して Igloo Software にログインしようとすると、そのユーザーが存在するかどうかが Igloo Software によって確認されます。  
使用可能なユーザー アカウントがない場合、ユーザー アカウントは Igloo Software によって自動的に作成されます。
## ユーザーの割り当て

構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

### ユーザーを Igloo Software に割り当てるには、次の手順に従います。

1.  Azure クラシック ポータルで、テスト アカウントを作成します。

2.  **Igloo Software * * アプリケーション統合ページで、をクリックして * * ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-igloo-software-tutorial/IC783974.png "Assign Users")

3.  テスト ユーザーを選択し、**[割り当て]**、**[はい]** の順にクリックして、割り当てを確定します。

    ![はい](./media/active-directory-saas-igloo-software-tutorial/IC767830.png "Yes")

シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。




