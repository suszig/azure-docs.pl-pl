<properties pageTitle="チュートリアル: Azure Active Directory と Salesforce Sandbox の統合 | Microsoft Azure" description="Azure Active Directory で Salesforce Sandbox を使用してシングル サインオンや自動化されたプロビジョニングなどを有効にする方法について説明します。" services="active-directory" authors="MarkusVi"  documentationCenter="na" manager="stevenpo"/>
<tags ms.service="active-directory" ms.devlang="na" ms.topic="article" ms.tgt_pltfrm="na" ms.workload="identity" ms.date="10/07/2015" ms.author="markvi" />
#チュートリアル: Azure Active Directory と Salesforce Sandbox の統合
>[AZURE.TIP]クリックして、フィードバック [ここ](http://go.microsoft.com/fwlink/?LinkId=521878)します。
  
このチュートリアルでは、Azure と Salesforce Sandbox の統合について説明します。  
Sandbox を使用すると、Salesforce 運用組織内のデータとアプリケーションを侵害することなく、多様な用途 (開発、テスト、トレーニングなど) ごとに別環境に組織の複数コピーを作成できます。  
詳細については、次を参照してください [サンド ボックスの概要。](https://help.salesforce.com/HTViewHelpDoc?id=create_test_instance.htm&language=en_US)
  
このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   Salesforce.com のサンドボックス
  
まだ Salesforce.com 内に有効なサンドボックスがない場合、Salesforce に連絡する必要があります。
  
このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  Salesforce Sandbox のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ドメインの有効化
4.  ユーザー プロビジョニングの構成
5.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-salesforce-sandbox-tutorial/IC769571.png "Scenario")
##Salesforce Sandbox のアプリケーション統合の有効化
  
このセクションでは、Salesforce Sandbox のアプリケーション統合を有効にする方法について説明します。

###Salesforce Sandbox のアプリケーション統合を有効にするには、次の手順に従います。

1.  左側のナビゲーション ウィンドウで、Azure 管理ポータルで、クリックして **Active Directory**します。

    ![Active Directory](./media/active-directory-saas-salesforce-sandbox-tutorial/IC700993.png "Active Directory")

2.   **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3.  ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。

    ![アプリケーション](./media/active-directory-saas-salesforce-sandbox-tutorial/IC700994.png "Applications")

4.  開くには、 **アプリケーション ギャラリー**, 、] をクリックして **アプリの追加**, 、クリックして **組織で使用するためのアプリケーションの追加**します。

    ![どの操作を行いますか。](./media/active-directory-saas-salesforce-sandbox-tutorial/IC700995.png "What do you want to do?")

5.   **検索ボックス**, 、型 **Salesforce Sandbox**します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-salesforce-sandbox-tutorial/IC710978.png "Application Gallery")

6.  結果ウィンドウで [ **Salesforce Sandbox**, 、クリックして **完了** アプリケーションを追加します。

    ![Salesforce Sandbox](./media/active-directory-saas-salesforce-sandbox-tutorial/IC746474.png "Salesforce Sandbox")
##シングル サインオンの構成
  
このセクションでは、ユーザーが SAML プロトコルに基づくフェデレーションを使用して、Azure AD でのユーザーのアカウントで Salesforce に対する認証を行えるようにする方法について説明します。

###シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **Salesforce Sandbox** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、 **[シングル サインオン** ダイアログ。

    ![Configure single sign-on](./media/active-directory-saas-salesforce-sandbox-tutorial/IC749323.png "Configure single sign-on")

2.   **どのようなユーザーが Salesforce Sandbox にサインイン** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、] をクリックし、 **次**します。

    ![Salesforce Sandbox](./media/active-directory-saas-salesforce-sandbox-tutorial/IC746479.png "Salesforce Sandbox")

3.   **アプリケーション URL の構成** ] ページの [、 **サインオン URL** ] ボックスに、次のパターンを使用して URL を入力 `http://company.my.salesforce.com`, 、] をクリックし、 **次**します。

    ![Configure App URL](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781022.png "Configure App URL")

4. した場合、既に別の Salesforce Sandbox インスタンスに対するシングル サインオン、ディレクトリのかどうかは、構成する必要もあります、 **識別子** として同じ値を持つこと、 **サインオン URL**します。  **識別子** チェックでフィールドを検索できる、 **詳細設定の表示** のチェック ボックス、 **アプリケーション URL の構成** 、ダイアログ ボックスのページです。

4.   **Salesforce Sandbox でのシングル サインオンの構成** ] ページで [ **証明書のダウンロード**, 、お使いのコンピューターに証明書ファイルを保存します。

    ![Configure Single Sign-On](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781023.png "Configure Single Sign-On")

5.  別の Web ブラウザーのウィンドウで、Salesforce Sandbox 企業サイトに管理者としてログインします。

6.  上部にあるメニュー [ **セットアップ**します。

    ![セットアップ](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781024.png "Setup")

7.  左側のナビゲーション ウィンドウで、クリックして **セキュリティ制御**, 、順にクリック **シングル サインオンの設定**します。

    ![シングル サインオンの設定](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781025.png "Single Sign-On Settings")

8.  [シングル サインオンの設定] セクションで、次の手順に従います。

    ![シングル サインオンの設定](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781026.png "Single Sign-On Settings")

    1.  選択 **SAML の有効化**します。
    2.  クリックして **新しい**します。

9.  [SAML シングル サインオンの設定] セクションで、次の手順に従います。

    ![SAML シングル サインオンの設定](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781027.png "SAML Single Sign-On Settings")

    1.  名] ボックスで、構成の名前を入力します (例:: *SPSSOWAAD\_Test*)。
    2.  Azure ポータルで、 **Salesforce Sandbox でのシングル サインオンの構成** ] ダイアログ ページでコピー、 **発行者 URL** 値に設定して、貼り付けます、 **発行者** ] ボックスに貼り付けます。
    3.   **エンティティ Id** ] ボックスに「 **https://test.salesforce.com** これが、ディレクトリに追加する最初の Salesforce Sandbox インスタンスである場合。 追加した Salesforce Sandbox のインスタンスからの場合、 **エンティティ ID** に入力、 **サインオン URL**, 、この形式にする必要があります。 `http://company.my.salesforce.com`
    4.  クリックして **参照** をダウンロードした証明書をアップロードします。
    5.  として **SAML Identity Type**, [ **アサーションには、ユーザー オブジェクトからのフェデレーション ID が含まれています。**します。
    6.  として **SAML Identity Location**, [ **Id を Subject ステートメントの NameIdentifier 要素に**します。
    7.  Azure ポータルで、 **Salesforce Sandbox でのシングル サインオンの構成** ] ダイアログ ページでコピー、 **リモート ログイン URL** 値に設定して、貼り付けます、 **Identity Provider Login URL** ] ボックスに貼り付けます。
    8.  Azure ポータルで、 **Salesforce Sandbox でのシングル サインオンの構成** ] ダイアログ ページでコピー、 **リモート ログアウト URL** 値に設定して、貼り付けます、 **Identity Provider Logout URL** ] ボックスに貼り付けます。
    9.  として **Service Provider Initiated Request Binding**, [ **HTTP POST**します。
    10. クリックして **保存**します。

10. Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781028.png "Configure Single Sign-On")

##ドメインの有効化
  
このセクションでは、ドメインが既に作成されていることを前提としています。  
詳細については、次を参照してください。 [ドメイン名の定義](https://help.salesforce.com/HTViewHelpDoc?id=domain_name_define.htm&language=en_US)します。

###ドメインを有効にするには、次の手順に従います。

1.  左側のナビゲーション ウィンドウで、クリックして **ドメイン管理**, 、クリックして **My Domain です。**

    ![マイ ドメイン](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781029.png "My Domain")

    >[AZURE.NOTE]自分のドメインが正しく構成されていることを確認してください。

2.   **Login Page Settings** ] をクリックして **編集**, 、その後、として **認証サービス**, 、前のセクションから SAML シングル サインオン設定の名前を選択し、最後にクリックして、 **保存**します。

    ![マイ ドメイン](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781030.png "My Domain")
  
ドメインを構成するとすぐに、ユーザーはドメインの URL を使用して Salesforce Sandbox にログインできるようになります。  
URL の値を取得するには、前のセクションで作成した SSO プロファイルをクリックします。
##ユーザー プロビジョニングの構成
  
このセクションでは、Active Directory のユーザー アカウントのユーザー プロビジョニングを Salesforce Sandbox に対して有効にする方法について説明します。

###ユーザー プロビジョニングを構成するには、次の手順に従います。

1.  Salesforce のポータルの上部にあるナビゲーション バーで、自分の名前を選択し、ユーザー メニューを展開します。

    ![個人の設定](./media/active-directory-saas-salesforce-sandbox-tutorial/IC698773.png "My Settings")

2.  ユーザー メニューから選択 **個人の設定** を開くには、 **個人の設定** ページです。

3.  左のウィンドウで **個人** Personal] セクションを展開し、クリックを **Reset My Security Token**:

    ![個人の設定](./media/active-directory-saas-salesforce-sandbox-tutorial/IC698774.png "My Settings")

4.   **Reset My Security Token** ] ページで [ **セキュリティ トークンのリセット** Salesforce.com のセキュリティ トークンを含む電子メールを要求します。

    ![新しいトークン](./media/active-directory-saas-salesforce-sandbox-tutorial/IC698776.png "New Token")

5.  電子メールに Salesforce.com からの電子メールの受信トレイを確認"**salesforce.com.com のセキュリティの確認**"という件名です。

6.  そのメールを確認し、セキュリティ トークンの値をコピーします。

7.  Azure の管理ポータルでの **salesforce Sandbox** アプリケーション統合ページでは、をクリックして **ユーザー プロビジョニングの構成** を開くには、 **ユーザー プロビジョニングの構成** ] ダイアログ。

    ![Configure user provisioning](./media/active-directory-saas-salesforce-sandbox-tutorial/IC769573.png "Configure user provisioning")

8.   **自動ユーザー プロビジョニングを有効にする Salesforce Sandbox 資格情報の入力** ] ページで、次の構成設定を提供します。

    ![Salesforce Sandbox](./media/active-directory-saas-salesforce-sandbox-tutorial/IC746476.png "Salesforce Sandbox")

    1.   **Salesforce Sandbox 管理ユーザー名** ] ボックスに、Salesforce sandbox アカウントの名前を持つ入力、 **システム管理者** 割り当てられている salesforce プロファイル。

    2.   **Salesforce Sandbox 管理パスワード** ] ボックスに、このアカウントのパスワードを入力します。

    3.   **ユーザーのセキュリティ トークン** ] ボックスに、セキュリティ トークンの値を貼り付けます。

    4.  クリックして **検証** 構成を確認します。

    5.  をクリックして、 **次** を開く] ボタン、 **確認** ページです。

9.   **確認** ] ページで [ **完了** して構成を保存します。
##ユーザーの割り当て
  
構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

###ユーザーを Salesforce Sandbox に割り当てるには、次の手順に従います。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  * * Salesforce Sandbox * * アプリケーション統合ページで、をクリックして **ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-salesforce-sandbox-tutorial/IC769574.png "Assign users")

3.  テスト ユーザーを選択して [ **割り当てる**, 、順にクリック **はい** して割り当てを確認します。

    ![あり](./media/active-directory-saas-salesforce-sandbox-tutorial/IC767830.png "Yes")
  
10 分待ってから、アカウントが Salesforce Sandbox と同期できたことをご確認ください。
  
シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](https://msdn.microsoft.com/library/dn308586)します。

