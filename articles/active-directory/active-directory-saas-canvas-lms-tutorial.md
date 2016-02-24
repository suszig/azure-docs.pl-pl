<properties
    pageTitle="チュートリアル: Azure Active Directory と Canvas LMS の統合 | Microsoft Azure" 
    description="Azure Active Directory で Canvas LMS を使用して、シングル サインオンや自動プロビジョニングなどを有効にする方法について説明します。" 
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

#チュートリアル: Azure Active Directory と Canvas LMS の統合

このチュートリアルでは、Azure と Canvas の統合について説明します。  
このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   Canvas テナント

このチュートリアルを完了すると、Canvas に割り当てた Azure AD ユーザーは、Canvas 会社サイト (サービス プロバイダーが開始したサインオン) で、アプリケーションにシングル サインオンまたはできるを使用して、 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。

このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  Canvas のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-canvas-lms-tutorial/IC775984.png "Scenario")
##Canvas のアプリケーション統合の有効化

このセクションでは、Canvas のアプリケーション統合を有効にする方法について説明します。

###Canvas のアプリケーション統合を有効にするには、次の手順を実行します。

1.  左側のナビゲーション ウィンドウで、Azure 管理ポータルで、クリックして **Active Directory**します。

    ![Active Directory](./media/active-directory-saas-canvas-lms-tutorial/IC700993.png "Active Directory")

2.   **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3.  ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。

    ![アプリケーション](./media/active-directory-saas-canvas-lms-tutorial/IC700994.png "Applications")

4.  クリックして **追加** ページの下部にあります。

    ![アプリケーションの追加](./media/active-directory-saas-canvas-lms-tutorial/IC749321.png "Add application")

5.   **実行する操作** ダイアログ ボックスで、をクリックして **ギャラリーからアプリケーションを追加**します。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-canvas-lms-tutorial/IC749322.png "Add an application from gallerry")

6.   **検索ボックス**, 、型 **キャンバス**します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-canvas-lms-tutorial/IC775985.png "Application Gallery")

7.  結果ウィンドウで [ **キャンバス**, 、クリックして **完了** アプリケーションを追加します。

    ![キャンバス](./media/active-directory-saas-canvas-lms-tutorial/IC775986.png "Canvas")
##シングル サインオンの構成

このセクションでは、SAML プロトコルに基づくフェデレーションを使用して、ユーザーが Azure AD のアカウントで Canvas に対する認証を行えるようにする方法を説明します。  
Canvas のシングル サインオンを構成するには、証明書からサムプリント値を取得する必要があります。  
この手順に慣れていない場合は、次を参照してください [証明書の拇印の値を取得する方法。](http://youtu.be/YKQF266SAxI)

###シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **キャンバス** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、* * [シングル サインオン * * ダイアログ。

    ![シングル サインオンの構成](./media/active-directory-saas-canvas-lms-tutorial/IC771709.png "Configure single sign-on")

2.   **方法を選択してキャンバスにサインオン** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、] をクリックし、 **次**します。

    ![Configure Single Sign-On](./media/active-directory-saas-canvas-lms-tutorial/IC775987.png "Configure Single Sign-On")

3.   **アプリケーション URL の構成** ] ページの [、 **Canvas Sign In URL** ] ボックスに、次のパターンを使用して URL を入力 `https://<tenant-name>.instructure.com`, 、] をクリックし、 **次**します。

    ![アプリケーション URL の構成](./media/active-directory-saas-canvas-lms-tutorial/IC775988.png "Configure App URL")

4.   **Canvas でのシングル サインオン構成** ] ページで、証明書をダウンロードする] をクリックして **証明書のダウンロード**, 、お使いのコンピューターにローカルで証明書ファイルを保存します。

    ![Configure Single Sign-On](./media/active-directory-saas-canvas-lms-tutorial/IC775989.png "Configure Single Sign-On")

5.  別の Web ブラウザー ウィンドウで、Canvas 企業サイトに管理者としてログインします。

6.  移動して **コース \ > アカウントの管理 \ > Microsoft**します。

    ![キャンバス](./media/active-directory-saas-canvas-lms-tutorial/IC775990.png "Canvas")

7.  左側のナビゲーション ウィンドウで [ **認証**, 、] をクリックし、 **新しい SAML 構成の追加**します。

    ![認証](./media/active-directory-saas-canvas-lms-tutorial/IC775991.png "Authentication")

8.  [現在の統合] ページで、次の手順を実行します。

    ![現在の統合](./media/active-directory-saas-canvas-lms-tutorial/IC775992.png "Current Integration")

    1.  Azure ポータルで、 **キャンバスでのシングル サインオンの構成** ] ダイアログ ページでコピー、 **エンティティ ID** 値に設定して、貼り付けます、 **IdP エンティティ ID** ] ボックスに貼り付けます。
    2.  Azure ポータルで、 **キャンバスでのシングル サインオンの構成** ] ダイアログ ページでコピー、 **リモート ログイン URL** 値に設定して、貼り付けます、 **ログオン URL** ] ボックスに貼り付けます。
    3.  Azure ポータルで、 **キャンバスでのシングル サインオンの構成** ] ダイアログ ページでコピー、 **リモート ログイン URL** 値に設定して、貼り付けます、 **ログアウト URL** ] ボックスに貼り付けます。
    4.  Azure ポータルで、 **キャンバスでのシングル サインオンの構成** ] ダイアログ ページでコピー、 **パスワード変更 URL** 値に設定して、貼り付けます、 **パスワードの変更] リンク** ] ボックスに貼り付けます。
    5.  コピー、 **拇印** エクスポートした証明書からの値し、貼り付けます、 **Certificate Fingerprint** ] ボックスに貼り付けます。  

        >[AZURE.TIP] 詳細については、次を参照してください [証明書の拇印の値を取得する方法。](http://youtu.be/YKQF266SAxI)

    6.   **ログイン属性** 一覧で、[ **NameID**します。
    7.   **識別子の形式** 一覧で、[ **emailAddress**します。
    8.  クリックして **認証設定の保存**します。

9.  Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-canvas-lms-tutorial/IC775993.png "Configure Single Sign-On")
##ユーザー プロビジョニングの構成

Azure AD ユーザーが Canvas にログインできるようにするには、ユーザーを Canvas にプロビジョニングする必要があります。  
Canvas の場合、プロビジョニングは手動で行います。

###ユーザー アカウントをプロビジョニングするには、次の手順を実行します。

1.  ログインして **キャンバス** テナントです。

2.  移動して **コース \ > アカウントの管理 \ > Microsoft**します。

    ![キャンバス](./media/active-directory-saas-canvas-lms-tutorial/IC775990.png "Canvas")

3.  クリックして **ユーザー**します。

    ![ユーザー](./media/active-directory-saas-canvas-lms-tutorial/IC775995.png "Users")

4.  クリックして **新しいユーザーを追加**します。

    ![ユーザー](./media/active-directory-saas-canvas-lms-tutorial/IC775996.png "Users")

5.  [新しいユーザーの追加] ダイアログ ページで、次の手順を実行します。

    ![ユーザーの追加](./media/active-directory-saas-canvas-lms-tutorial/IC775997.png "Add User")

    1.   **フル_ネーム** ] ボックスに、ユーザーの名前を入力します。
    2.   **電子メール** ] ボックスに、ユーザーの電子メール アドレスを入力します。
    3.   **ログイン** ] ボックスに、ユーザーの Azure AD 電子メール アドレスを入力します。
    4.  選択 **このアカウント作成についてユーザーを電子メールで送信**します。
    5.  クリックして **ユーザーを追加**します。

>[AZURE.NOTE] 他の Canvas ユーザー アカウント作成ツールを使用するまたは AAD ユーザー アカウントをプロビジョニング、Canvas から提供されている Api です。

##ユーザーの割り当て

構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

###ユーザーを Canvas に割り当てるには、次の手順を実行します。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  * * キャンバス * * アプリケーション統合ページで、をクリックして **ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-canvas-lms-tutorial/IC775998.png "Assigning users")

3.  テスト ユーザーを選択して [ **割り当てる**, 、順にクリック **はい** して割り当てを確認します。

    ![あり](./media/active-directory-saas-canvas-lms-tutorial/IC767830.png "Yes")

シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。

