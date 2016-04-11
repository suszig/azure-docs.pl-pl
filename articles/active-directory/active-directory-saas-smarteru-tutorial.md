<properties 
    pageTitle="チュートリアル: Azure Active Directory と SmarterU の統合 | Microsoft Azure" 
    description="Azure Active Directory で SmarterU を使用して、シングル サインオンや自動プロビジョニングなどを有効にする方法について説明します。" 
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

#チュートリアル: Azure Active Directory と SmarterU の統合
  
このチュートリアルでは、Azure と SmarterU の統合について説明します。  
このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   SmarterU テナント
  
このチュートリアルを完了すると、SmarterU に割り当てた Azure AD ユーザーは、SmarterU 会社サイト (サービス プロバイダーが開始したサインオン) で、アプリケーションにシングル サインオンまたはできるを使用して、 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。
  
このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  SmarterU のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-smarteru-tutorial/IC777320.png "Scenario")

##SmarterU のアプリケーション統合の有効化
  
このセクションでは、SmarterU のアプリケーション統合を有効にする方法について説明します。

###SmarterU のアプリケーション統合を有効にするには、次の手順に従います。

1.  左側のナビゲーション ウィンドウで、Azure 管理ポータルで、クリックして **Active Directory**します。

    ![Active Directory](./media/active-directory-saas-smarteru-tutorial/IC700993.png "Active Directory")

2.   **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3.  ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。

    ![アプリケーション](./media/active-directory-saas-smarteru-tutorial/IC700994.png "Applications")

4.  クリックして **追加** ページの下部にあります。

    ![アプリケーションの追加](./media/active-directory-saas-smarteru-tutorial/IC749321.png "Add application")

5.   **実行する操作** ダイアログ ボックスで、をクリックして **ギャラリーからアプリケーションを追加**します。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-smarteru-tutorial/IC749322.png "Add an application from gallerry")

6.   **検索ボックス**, 、型 **SmarterU**します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-smarteru-tutorial/IC777321.png "Application fallery")

7.  結果ウィンドウで [ **SmarterU**, 、クリックして **完了** アプリケーションを追加します。

    ![SmarterU](./media/active-directory-saas-smarteru-tutorial/IC777322.png "SmarterU")

##シングル サインオンの構成
  
このセクションでは、SAML プロトコルに基づくフェデレーションを使用して、SmarterU で Azure AD のユーザー アカウントを使用してユーザーを認証できるようにする方法を説明します。

###シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **SmarterU** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、* * [シングル サインオン * * ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-smarteru-tutorial/IC777323.png "Configure Single Sign-On")

2.   **どのようなユーザーの SmarterU へのアクセス** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、] をクリックし、 **次**します。

    ![Configure Single Sign-On](./media/active-directory-saas-smarteru-tutorial/IC777324.png "Configure Single Sign-On")

3.   **SmarterU でのシングル サインオンの構成** ] ページで、メタデータをダウンロードする] をクリックして **メタデータのダウンロード**, 、し、データ ファイルをローカルでとして **c:\\SmarterUMetaData.cer**します。

    ![Configure Single Sign-On](./media/active-directory-saas-smarteru-tutorial/IC777325.png "Configure Single Sign-On")

4.  別の Web ブラウザー ウィンドウで、SmarterU 企業サイトに管理者としてログインします。

5.  上部にあるツールバーで、クリックして **アカウント設定**します。

    ![Account Settings](./media/active-directory-saas-smarteru-tutorial/IC777326.png "Account Settings")

6.  アカウント構成ページで、次の手順に従います。

    ![外部認証](./media/active-directory-saas-smarteru-tutorial/IC777327.png "External Authorization")

    1.  選択 **Enable External Authorization**します。
    2.   **Master Login Control** ] セクションの [、 **SmarterU** ] タブをクリックします。
    3.   **User Default Login** ] セクションの [、 **SmarterU** ] タブをクリックします。
    4.  選択 **Enable Okta**します。
    5.  ダウンロードしたメタデータ ファイルの内容をコピーして貼り付けます、 **Okta Metadata** ] ボックスに貼り付けます。
    6.  クリックして **保存**します。

7.  Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-smarteru-tutorial/IC777328.png "Configure Single Sign-On")

##ユーザー プロビジョニングの構成
  
Azure AD ユーザーが SmarterU にログインできるようにするには、そのユーザーを SmarterU にプロビジョニングする必要があります。  
SmarterU の場合、プロビジョニングは手動で行います。

###ユーザー アカウントをプロビジョニングするには、次の手順を実行します。

1.  ログインして **SmarterU** テナントです。

2.  移動して **ユーザー**します。

3.  ユーザー セクションで、次の手順に従います。

    ![新しいユーザー](./media/active-directory-saas-smarteru-tutorial/IC777329.png "New User")

    1.  クリックして **+ ユーザー**します。
    2.  次のテキスト ボックスに、Azure AD ユーザー アカウントの関連する属性の値を入力: **プライマリ電子メール**, 、**従業員 ID**, 、**パスワード**, 、**パスワードの確認**, 、**指定された名前**, 、**Surname**します。
    3.  クリックして **Active**します。
    4.  クリックして **保存**します。

>[AZURE.NOTE] 他の SmarterU ユーザー アカウント作成ツールを使用するや、AAD ユーザー アカウントをプロビジョニング、SmarterU 提供の Api です。

##ユーザーの割り当て
  
構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

###ユーザーを SmarterU に割り当てるには、次の手順に従います。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  * * SmarterU * * アプリケーション統合ページで、をクリックして **ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-smarteru-tutorial/IC777330.png "Assign Users")

3.  テスト ユーザーを選択して [ **割り当てる**, 、順にクリック **はい** して割り当てを確認します。

    ![あり](./media/active-directory-saas-smarteru-tutorial/IC767830.png "Yes")
  
シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。
