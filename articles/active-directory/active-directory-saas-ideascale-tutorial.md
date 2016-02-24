<properties 
    pageTitle="チュートリアル: Azure Active Directory と IdeaScale の統合 | Microsoft Azure" 
    description="Azure Active Directory で IdeaScale を使用して、シングル サインオンや自動プロビジョニングなどを有効にする方法について説明します。" 
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

#チュートリアル: Azure Active Directory と IdeaScale の統合
  
このチュートリアルでは、Azure と IdeaScale の統合について説明します。  
このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   IdeaScale でのシングル サインオンが有効なサブスクリプション
  
このチュートリアルを完了すると、IdeaScale に割り当てた Azure AD ユーザーができるようにアプリケーションを使用して、シングル サインオン、 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。
  
このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  IdeaScale のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-ideascale-tutorial/IC790838.png "Scenario")
##IdeaScale のアプリケーション統合の有効化
  
このセクションでは、IdeaScale のアプリケーション統合を有効にする方法を説明します。

###IdeaScale のアプリケーション統合を有効にするには、次の手順を実行します。

1.  左側のナビゲーション ウィンドウで、Azure 管理ポータルで、クリックして **Active Directory**します。

    ![Active Directory](./media/active-directory-saas-ideascale-tutorial/IC700993.png "Active Directory")

2.   **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3.  ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。

    ![アプリケーション](./media/active-directory-saas-ideascale-tutorial/IC700994.png "Applications")

4.  クリックして **追加** ページの下部にあります。

    ![アプリケーションの追加](./media/active-directory-saas-ideascale-tutorial/IC749321.png "Add application")

5.   **実行する操作** ダイアログ ボックスで、をクリックして **ギャラリーからアプリケーションを追加**します。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-ideascale-tutorial/IC749322.png "Add an application from gallerry")

6.   **検索ボックス**, 、型 **IdeaScale**します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-ideascale-tutorial/IC790841.png "Application Gallery")

7.  結果ウィンドウで [ **IdeaScale**, 、クリックして **完了** アプリケーションを追加します。

    ![IdeaScale](./media/active-directory-saas-ideascale-tutorial/IC790842.png "IdeaScale")
##シングル サインオンの構成
  
このセクションでは、ユーザーが SAML プロトコルに基づくフェデレーションを使用して、Azure AD でのユーザーのアカウントで IdeaScale に対する認証を行うことができるようにする方法を説明します。  
IdeaScale のシングル サインオンを構成するには、証明書からサムプリント値を取得する必要があります。  
この手順に慣れていない場合は、次を参照してください。 [証明書の拇印の値を取得する方法](http://youtu.be/YKQF266SAxI)します。

###シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **IdeaScale** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、* * [シングル サインオン * * ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-ideascale-tutorial/IC790843.png "Configure Single Sign-On")

2.   **どのようなユーザーが IdeaScale へのサインオン** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、] をクリックし、 **次**します。

    ![Configure Single Sign-On](./media/active-directory-saas-ideascale-tutorial/IC790844.png "Configure Single Sign-On")

3.   **アプリケーション URL の構成** ] ページの [、 **IdeaScale サインオン URL** URL は、ユーザーが IdeaScale アプリケーションにサインオンする使用] ボックスに、型 (例::"*https://company.IdeaScale.com*")、順にクリック **次**します。

    ![アプリケーション URL の構成](./media/active-directory-saas-ideascale-tutorial/IC790845.png "Configure App URL")

4.   **IdeaScale でのシングル サインオンの構成** ] ページで、メタデータをダウンロードする] をクリックして **メタデータのダウンロード**, 、コンピューターのローカルにメタデータ ファイルを保存します。

    ![Configure Single Sign-On](./media/active-directory-saas-ideascale-tutorial/IC790846.png "Configure Single Sign-On")

5.  別の Web ブラウザーのウィンドウで、IdeaScale 企業サイトに管理者としてログインします。

6.  移動して **コミュニティ設定**します。

    ![コミュニティの設定](./media/active-directory-saas-ideascale-tutorial/IC790847.png "Community Settings")

7.  移動して **セキュリティ \ > シングル サインオンの設定**します。

    ![シングル サインオンの設定](./media/active-directory-saas-ideascale-tutorial/IC790848.png "Single Signon Settings")

8.  として **シングル サインオンの種類**, [ **SAML 2.0**します。

    ![シングル サインオンのタイプ](./media/active-directory-saas-ideascale-tutorial/IC790849.png "Single Signon Type")

9.   **シングル サインオンの設定** ] ダイアログ ボックスで、次の手順を実行します。

    ![シングル サインオンの設定](./media/active-directory-saas-ideascale-tutorial/IC790850.png "Single Signon Settings")

    1.  Azure ポータルで、 **IdeaScale でのシングル サインオンの構成** ] ダイアログ ページでコピー、 **エンティティ ID** 値に設定して、貼り付けます、 **SAML IdP エンティティ ID** ] ボックスに貼り付けます。
    2.  ダウンロードしたメタデータ ファイルの内容をコピーして貼り付けます、 **SAML IdP Metadata** ] ボックスに貼り付けます。
    3.  Azure ポータルで、 **IdeaScale でのシングル サインオンの構成** ] ダイアログ ページでコピー、 **リモート ログアウト URL** 値に設定して、貼り付けます、 **Logout Success URL** ] ボックスに貼り付けます。
    4.  クリックして **変更を保存**します。

10. Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-ideascale-tutorial/IC790851.png "Configure Single Sign-On")
##ユーザー プロビジョニングの構成
  
Azure AD ユーザーが IdeaScale にログインできるようにするには、そのユーザーを IdeaScale にプロビジョニングする必要があります。  
IdeaScale の場合、プロビジョニングは手動で行います。

###ユーザー プロビジョニングを構成するには、次の手順に従います。

1.  ログインして **IdeaScale** 会社サイトに管理者として。

2.  移動して **コミュニティ設定**します。

    ![コミュニティの設定](./media/active-directory-saas-ideascale-tutorial/IC790847.png "Community Settings")

3.  移動して **基本設定 \ > メンバーの管理**します。

4.  クリックして **メンバーを追加**します。

    ![メンバ管理](./media/active-directory-saas-ideascale-tutorial/IC790852.png "Member Management")

5.  [新しいメンバーの追加] セクションで、次の手順を実行します。

    ![新しいメンバーの追加](./media/active-directory-saas-ideascale-tutorial/IC790853.png "Add New Member")

    1.   **電子メール アドレス** ] ボックスに、プロビジョニングする有効な AAD アカウントの電子メール アドレスを入力します。
    2.  クリックして **変更を保存**します。

    >[AZURE.NOTE] Azure Active Directory アカウント所有者では、電子メールをアクティブ化する前に、アカウントの確認へのリンクを取得します。

>[AZURE.NOTE] 他の IdeaScale ユーザー アカウント作成ツールを使用することができますか、AAD ユーザー アカウントをプロビジョニング、IdeaScale から提供されている Api です。

##ユーザーの割り当て
  
構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

###ユーザーを IdeaScale に割り当てるには、次の手順を実行します。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  * * IdeaScale * * アプリケーション統合ページで、をクリックして **ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-ideascale-tutorial/IC790854.png "Assign Users")

3.  テスト ユーザーを選択して [ **割り当てる**, 、順にクリック **はい** して割り当てを確認します。

    ![あり](./media/active-directory-saas-ideascale-tutorial/IC767830.png "Yes")
  
シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。
