<properties 
    pageTitle="チュートリアル: Azure Active Directory と FreshService の統合 | Microsoft Azure" 
    description="Azure Active Directory で FreshService を使用して、シングル サインオンや自動プロビジョニングなどを有効にする方法について説明します。" 
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

#チュートリアル: Azure Active Directory と FreshService の統合
  
このチュートリアルでは、Azure と FreshService の統合について説明します。  
このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   FreshService でのシングル サインオンが有効なサブスクリプション
  
このチュートリアルを完了すると、FreshService に割り当てた Azure AD ユーザーができるようにアプリケーションを使用して、シングル サインオン、 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。
  
このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  FreshService のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-freshservice-tutorial/IC790807.png "Scenario")
##FreshService のアプリケーション統合の有効化
  
このセクションでは、FreshService のアプリケーション統合を有効にする方法について説明します。

###FreshService のアプリケーション統合を有効にするには、次の手順を実行します。

1.  左側のナビゲーション ウィンドウで、Azure 管理ポータルで、クリックして **Active Directory**します。

    ![Active Directory](./media/active-directory-saas-freshservice-tutorial/IC700993.png "Active Directory")

2.   **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3.  ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。

    ![アプリケーション](./media/active-directory-saas-freshservice-tutorial/IC700994.png "Applications")

4.  クリックして **追加** ページの下部にあります。

    ![アプリケーションの追加](./media/active-directory-saas-freshservice-tutorial/IC749321.png "Add application")

5.   **実行する操作** ダイアログ ボックスで、をクリックして **ギャラリーからアプリケーションを追加**します。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-freshservice-tutorial/IC749322.png "Add an application from gallerry")

6.   **検索ボックス**, 、型 **FreshService**します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-freshservice-tutorial/IC790808.png "Application Gallery")

7.  結果ウィンドウで [ **FreshService**, 、クリックして **完了** アプリケーションを追加します。

    ![FreshService](./media/active-directory-saas-freshservice-tutorial/IC790809.png "Freshservice")
##シングル サインオンの構成
  
このセクションでは、ユーザーが SAML プロトコルに基づくフェデレーションを使用して、Azure AD でのユーザーのアカウントで FreshService に対する認証を行うことができるようにする方法を説明します。  
FreshService にシングル サインオンを構成するには、証明書から拇印の値を取得する必要があります。  
この手順に慣れていない場合は、次を参照してください。 [証明書の拇印の値を取得する方法](http://youtu.be/YKQF266SAxI)します。

###シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **FreshService** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、* * [シングル サインオン * * ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-freshservice-tutorial/IC790810.png "Configure Single Sign-On")

2.   **どのようなユーザーが FreshService へのサインオン** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、順にクリック **次**します。

    ![Configure Single Sign-On](./media/active-directory-saas-freshservice-tutorial/IC790811.png "Configure Single Sign-On")

3.   **アプリケーション URL の構成** ] ページで、 **FreshService サインオン URL** ] ボックスに、型に使用する URL、ユーザーが Freshdesk アプリケーションにサインオン (例::"*http://democompany.freshservice.com/*")、順にクリック **次**します。

    ![Configure App URL](./media/active-directory-saas-freshservice-tutorial/IC790812.png "Configure App URL")

4.   **FreshService でのシングル サインオンの構成** ] ページで、証明書をダウンロードする] をクリックして **証明書のダウンロード**, 、お使いのコンピューターにローカルで証明書ファイルを保存します。

    ![Configure Single Sign-On](./media/active-directory-saas-freshservice-tutorial/IC790813.png "Configure Single Sign-On")

5.  別の Web ブラウザー ウィンドウで、FreshService 企業サイトに管理者としてログインします。

6.  上部にあるメニュー [ **Admin**します。

    ![管理者](./media/active-directory-saas-freshservice-tutorial/IC790814.png "Admin")

7.   **カスタマー ポータル**, 、クリックして **セキュリティ**します。

    ![セキュリティ](./media/active-directory-saas-freshservice-tutorial/IC790815.png "Security")

8.   **セキュリティ** セクションで、次の手順に従います。

    ![シングル サインオン](./media/active-directory-saas-freshservice-tutorial/IC790816.png "Single Sign On")

    1.  スイッチ **シングル サインオン部族**します。
    2.  選択 **SAML SSO**します。
    3.  Azure ポータルで、 **FreshService でのシングル サインオンの構成** ] ダイアログ ページでコピー、 **リモート ログイン URL** 値に設定して、貼り付けます、 **SAML Login URL** ] ボックスに貼り付けます。
    4.  Azure ポータルで、 **FreshService でのシングル サインオンの構成** ] ダイアログ ページでコピー、 **リモート ログアウト URL** 値に設定して、貼り付けます、 **ログアウト URL** ] ボックスに貼り付けます。
    5.  コピー、 **拇印** エクスポートした証明書からの値し、貼り付けます、 **Security Certificate Fingerprint** ] ボックスに貼り付けます。
    
        >[AZURE.TIP]詳細については、次を参照してください [証明書の拇印の値を取得する方法。](http://youtu.be/YKQF266SAxI)

9.  Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-freshservice-tutorial/IC790817.png "Configure Single Sign-On")
##ユーザー プロビジョニングの構成
  
Azure AD ユーザーが FreshService にログインできるようにするには、そのユーザーを FreshService にプロビジョニングする必要があります。  
FreshService の場合、プロビジョニングは手動で行います。

###ユーザー アカウントをプロビジョニングするには、次の手順を実行します。

1.  ログインして **FreshService** 会社サイトに管理者として。

2.  上部にあるメニュー [ **Admin**します。

    ![管理者](./media/active-directory-saas-freshservice-tutorial/IC790814.png "Admin")

3.   **ユーザー管理** ] をクリックして **要求者**します。

    ![要求者](./media/active-directory-saas-freshservice-tutorial/IC790818.png "Requesters")

4.  クリックして **New Requester**します。

    ![新しい要求者](./media/active-directory-saas-freshservice-tutorial/IC790819.png "New Requesters")

5.   **New Requester** セクションで、次の手順に従います。

    ![新しい要求者](./media/active-directory-saas-freshservice-tutorial/IC790820.png "New Requester")

    1.  入力、 **名** と **電子メール** 関連テキスト ボックスに、プロビジョニングする有効な Azure Active Directory アカウントの属性です。
    2.  クリックして **保存**します。

    >[AZURE.NOTE] Azure Active Directory アカウント所有者がアクティブ化する前に、アカウントの確認へのリンクが記載された電子メールを取得します。

>[AZURE.NOTE] 他の FreshService ユーザー アカウント作成ツールを使用することができますか、AAD ユーザー アカウントをプロビジョニング、FreshService から提供されている Api です。

##ユーザーの割り当て
  
構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

###ユーザーを FreshService に割り当てるには、次の手順を実行します。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  * * FreshService * * アプリケーション統合ページで、をクリックして **ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-freshservice-tutorial/IC790821.png "Assign Users")

3.  テスト ユーザーを選択して [ **割り当てる**, 、順にクリック **はい** して割り当てを確認します。

    ![あり](./media/active-directory-saas-freshservice-tutorial/IC767830.png "Yes")
  
シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。

