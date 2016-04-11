<properties 
    pageTitle="チュートリアル: Azure Active Directory と Veracode の統合 | Microsoft Azure" 
    description="Azure Active Directory で Veracode を使用して、シングル サインオンや自動プロビジョニングなどを有効にする方法について説明します。" 
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

#チュートリアル: Azure Active Directory と Veracode の統合
  
このチュートリアルでは、Azure と Veracode の統合について説明します。 このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   Veracode でのシングル サインオンが有効なサブスクリプション
  
このチュートリアルを完了すると Veracode に割り当てた Azure AD ユーザーができるようにアプリケーションを使用して、シングル サインオン、 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。
  
このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  Veracode のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-veracode-tutorial/IC802903.png "Scenario")

##Veracode のアプリケーション統合の有効化
  
このセクションでは、Veracode のアプリケーション統合を有効にする方法について説明します。

###Veracode のアプリケーション統合を有効にするには、次の手順を実行します。

1.  左側のナビゲーション ウィンドウで、Azure 管理ポータルで、クリックして **Active Directory**します。

    ![Active Directory](./media/active-directory-saas-veracode-tutorial/IC700993.png "Active Directory")

2.   **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3.  ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。

    ![アプリケーション](./media/active-directory-saas-veracode-tutorial/IC700994.png "Applications")

4.  クリックして **追加** ページの下部にあります。

    ![アプリケーションの追加](./media/active-directory-saas-veracode-tutorial/IC749321.png "Add application")

5.   **実行する操作** ダイアログ ボックスで、をクリックして **ギャラリーからアプリケーションを追加**します。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-veracode-tutorial/IC749322.png "Add an application from gallerry")

6.   **検索ボックス**, 、型 **と Veracode**します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-veracode-tutorial/IC802904.png "Application Gallery")

7.  結果ウィンドウで [ **と Veracode**, 、クリックして **完了** アプリケーションを追加します。

    ![Veracode](./media/active-directory-saas-veracode-tutorial/IC802905.png "Veracode")

##シングル サインオンの構成
  
このセクションでは、SAML プロトコルに基づくフェデレーションを使用して、ユーザーが Azure AD のアカウントで Veracode に対する認証を行うことができるようにする方法を説明します。  
Veracode アプリケーションにはカスタム属性マッピングを追加する必要がある特定の形式での SAML アサーションが必要ですが、 **saml トークン属性** 構成します。  
次のスクリーンショットはその例です。

![属性](./media/active-directory-saas-veracode-tutorial/IC802906.png "Attributes")

###シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **と Veracode** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、* * [シングル サインオン * * ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-veracode-tutorial/IC802907.png "Configure Single Sign-On")

2.   **どのようなユーザーと Veracode サインオンする** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、] をクリックし、 **次**します。

    ![Configure Single Sign-On](./media/active-directory-saas-veracode-tutorial/IC802908.png "Configure Single Sign-On")

3.   **アプリケーション設定の構成** ] ページで [ **次**します。

    ![Configure App Settings](./media/active-directory-saas-veracode-tutorial/IC802909.png "Configure App Settings")

4.   **と Veracode でのシングル サインオンの構成** ] ページで、証明書をダウンロードする] をクリックして **証明書のダウンロード**, 、お使いのコンピューターにローカルで証明書ファイルを保存します。

    ![Configure Single Sign-On](./media/active-directory-saas-veracode-tutorial/IC802910.png "Configure Single Sign-On")

5.  別の Web ブラウザーのウィンドウで、Veracode 企業サイトに管理者としてログインします。

6.  上部にあるメニュー [ **設定**, 、] をクリックし、 **Admin**します。

    ![管理](./media/active-directory-saas-veracode-tutorial/IC802911.png "Administration")

7.  クリックして、 **SAML** ] タブをクリックします。

8.   **組織 SAML 設定** セクションで、次の手順に従います。

    ![管理](./media/active-directory-saas-veracode-tutorial/IC802912.png "Administration")

    1.  Azure ポータルで、 **と Veracode でのシングル サインオンの構成** ] ダイアログ ページで、コピー、 **発行者 URL** 値に設定して、貼り付けます、 **発行者** ] テキスト ボックス
    2.  ダウンロードした証明書をアップロードするにはクリックして **Choose File**します。
    3.  選択 **自己登録を有効にする**です。

9.   **自己登録設定** セクションで、次の手順を実行し、をクリックし、 **保存**:

    ![管理](./media/active-directory-saas-veracode-tutorial/IC802913.png "Administration")

    1.  として **新しいユーザーのアクティブ化**, [ **ライセンス認証を必要としない**します。
    2.  として **ユーザー データの更新**, [ **] 基本設定と Veracode ユーザー データ**します。
    3.   **SAML 属性の詳細**, 、次を選択します。
        -   **ユーザー ロール**
        -   **ポリシー管理者**
        -   **レビュー担当者**
        -   **セキュリティ リーダー**
        -   **Executive**
        -   **申請者**
        -   **作成者**
        -   **すべてのスキャンの種類**
        -   **チームのメンバーシップ**
        -   **既定のチーム**

10. Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-veracode-tutorial/IC802914.png "Configure Single Sign-On")

11. 上部にあるメニュー [ **属性** を開くには、 **SAML トークン属性** ダイアログ。

    ![属性](./media/active-directory-saas-veracode-tutorial/IC795920.png "Attributes")

12. 必要な属性のマッピングを追加するには、次の手順を実行します。

    ![属性](./media/active-directory-saas-veracode-tutorial/IC802906.png "Attributes")

    | 属性名 | 属性値 |
    |:---------------|:----------------|
    | firstname      | User.givenname  |
    | lastname       | User.surname    |
    | email          | User.mail       |

    1.  上記の表の各データ行をクリックして **ユーザー属性の追加**します。
    
    2.   **属性名** ] ボックスに、その行に対して表示される属性名を入力します。

    3.   **属性値** ] ボックスに、その行に対して表示される属性の値を選択します。

    4.  クリックして **完了**します。

13. クリックして **の変更を適用**します。

##ユーザー プロビジョニングの構成
  
Azure AD ユーザーが Veracode にログインできるようにするには、そのユーザーを Veracode にプロビジョニングする必要があります。  
Veracode の場合、プロビジョニングは自動化されています。  
ユーザー側で必要な操作はありません。
  
最初のシングル サインオンの試行中に、必要に応じてユーザーが自動的に作成されます。

>[AZURE.NOTE] 他のと Veracode ユーザー アカウント作成ツールを使用するまたは Api に用意されていると Veracode AAD ユーザー アカウントのプロビジョニングします。

##ユーザーの割り当て
  
構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

###ユーザーを Veracode に割り当てるには、次の手順を実行します。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  * * と Veracode * * アプリケーション統合ページで、をクリックして **ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-veracode-tutorial/IC802915.png "Assign Users")

3.  テスト ユーザーを選択して [ **割り当てる**, 、順にクリック **はい** して割り当てを確認します。

    ![あり](./media/active-directory-saas-veracode-tutorial/IC767830.png "Yes")
  
シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。
