<properties
    pageTitle="チュートリアル: Azure Active Directory と ITRP の統合 | Microsoft Azure" 
    description="Azure Active Directory で ITRP を使用して、シングル サインオンや自動プロビジョニングなどを有効にする方法について説明します。" 
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
    ms.date="10/20/2015" 
    ms.author="markvi" />

#チュートリアル: Azure Active Directory と ITRP の統合
  
このチュートリアルでは、Azure と ITRP の統合について説明します。  
このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   ITRP テナント
  
このチュートリアルを完了すると、ITRP に割り当てた Azure AD ユーザーは、ITRP 会社サイト (サービス プロバイダーが開始したサインオン) で、アプリケーションにシングル サインオンまたはできるを使用して、 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。
  
このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  ITRP のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-itrp-tutorial/IC775551.png "Scenario")
##ITRP のアプリケーション統合の有効化
  
このセクションでは、ITRP のアプリケーション統合を有効にする方法について説明します。

###ITRP のアプリケーション統合を有効にするには、次の手順を実行します。

1.  左側のナビゲーション ウィンドウで、Azure 管理ポータルで、クリックして **Active Directory**します。

    ![Active Directory](./media/active-directory-saas-itrp-tutorial/IC700993.png "Active Directory")

2.   **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3.  ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。

    ![アプリケーション](./media/active-directory-saas-itrp-tutorial/IC700994.png "Applications")

4.  クリックして **追加** ページの下部にあります。

    ![アプリケーションの追加](./media/active-directory-saas-itrp-tutorial/IC749321.png "Add application")

5.   **実行する操作** ダイアログ ボックスで、をクリックして **ギャラリーからアプリケーションを追加**します。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-itrp-tutorial/IC749322.png "Add an application from gallerry")

6.   **検索ボックス**, 、型 **ITRP**します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-itrp-tutorial/IC775565.png "Application Gallery")

7.  結果ウィンドウで [ **ITRP**, 、クリックして **完了** アプリケーションを追加します。

    ![ITRP](./media/active-directory-saas-itrp-tutorial/IC775566.png "ITRP")
##シングル サインオンの構成
  
このセクションでは、ユーザーが SAML プロトコルに基づくフェデレーションを使用して、Azure AD でのユーザーのアカウントで ITRP に対する認証を行うことができるようにする方法を説明します。  
ITRP のシングル サインオンを構成するには、証明書から拇印の値を取得する必要があります。  
この手順に慣れていない場合は、次を参照してください。 [証明書の拇印の値を取得する方法](http://youtu.be/YKQF266SAxI)します。

###シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **ITRP** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、* * [シングル サインオン * * ダイアログ。

    ![シングル サインオンの構成](./media/active-directory-saas-itrp-tutorial/IC771709.png "Configure single sign-on")

2.   **どのようなユーザー ITRP にサインオンする** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、] をクリックし、 **次**します。

    ![Configure Single Sign-On](./media/active-directory-saas-itrp-tutorial/IC775567.png "Configure Single Sign-On")

3.   **アプリケーション URL の構成** ] ページで、 **ITRP サインイン URL** ] ボックスに、次のパターンを使用して URL を入力"*https://\<tenant-name\>。ITRP.com*"、クリックして **次**します。

    ![アプリケーション URL の構成](./media/active-directory-saas-itrp-tutorial/IC775568.png "Configure App URL")

4.   **ITRP でのシングル サインオンの構成** ] ページで、証明書をダウンロードする] をクリックして **証明書のダウンロード**, 、としては、証明書ファイルをローカルに保存および **c:\\ITRP.cer**します。

    ![Configure Single Sign-On](./media/active-directory-saas-itrp-tutorial/IC775569.png "Configure Single Sign-On")

5.  別の Web ブラウザー ウィンドウで、ITRP 企業サイトに管理者としてログインします。

6.  上部にあるツールバーで、クリックして **設定**します。

    ![ITRP](./media/active-directory-saas-itrp-tutorial/IC775570.png "ITRP")

7.  左側のナビゲーション ウィンドウで [ **でのシングル サインオン**します。

    ![シングル サインオン](./media/active-directory-saas-itrp-tutorial/IC775571.png "Single Sign-On")

8.  [シングル サインオン] 構成セクションで、次の手順を実行します。

    ![シングル サインオン](./media/active-directory-saas-itrp-tutorial/IC775572.png "Single Sign-On")

    ![シングル サインオン](./media/active-directory-saas-itrp-tutorial/IC775573.png "Single Sign-On")

    1.  クリックして **を有効にする**です。
    2.  Azure ポータルで、 **ITRP でのシングル サインオンの構成** ] ダイアログ ページでコピー、 **リモート ログアウト URL** 値に設定して、貼り付けます、 **リモート ログアウト URL** ] ボックスに貼り付けます。
    3.  Azure ポータルで、 **ITRP でのシングル サインオンの構成** ] ダイアログ ページでコピー、 **SAML SSO URL** 値に設定して、貼り付けます、 **SAML SSO URL** ] ボックスに貼り付けます。
    4.  コピー、 **拇印** エクスポートした証明書からの値し、貼り付けます、 **Certificate Fingerprint** ] ボックスに貼り付けます。
        
        >[AZURE.TIP]詳細については、次を参照してください [証明書の拇印の値を取得する方法。](http://youtu.be/YKQF266SAxI)

    5.  クリックして **保存**します。

9.  Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-itrp-tutorial/IC775574.png "Configure Single Sign-On")
##ユーザー プロビジョニングの構成
  
Azure AD ユーザーが ITRP にログインできるようにするには、ユーザーを ITRP にプロビジョニングする必要があります。  
ITRP の場合、プロビジョニングは手動で行います。

###ユーザー アカウントをプロビジョニングするには、次の手順を実行します。

1.  ログインして **ITRP** テナントです。

2.  上部にあるツールバーで、クリックして **レコード**します。

    ![管理者](./media/active-directory-saas-itrp-tutorial/IC775575.png "Admin")

3.  ポップアップ メニューから選択 **人**します。

    ![People](./media/active-directory-saas-itrp-tutorial/IC775587.png "People")

4.  クリックして **Add New Person** (「+」) です。

    ![管理者](./media/active-directory-saas-itrp-tutorial/IC775576.png "Admin")

5.  [新しいユーザーの追加] ダイアログで、次の手順を実行します。

    ![ユーザー](./media/active-directory-saas-itrp-tutorial/IC775577.png "User")

    1.  型、 **名前**, 、**電子メール** 、プロビジョニングする有効な AAD アカウントのです。
    2.  クリックして **保存**します。

>[AZURE.NOTE] 他の ITRP ユーザー アカウント作成ツールを使用することができますか、AAD ユーザー アカウントをプロビジョニング、ITRP 提供の Api です。

##ユーザーの割り当て
  
構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

###ユーザーを ITRP に割り当てるには、次の手順を実行します。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  * * ITRP * * アプリケーション統合ページで、をクリックして **ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-itrp-tutorial/IC775588.png "Assign Users")

3.  テスト ユーザーを選択して [ **割り当てる**, 、順にクリック **はい** して割り当てを確認します。

    ![あり](./media/active-directory-saas-itrp-tutorial/IC767830.png "Yes")
  
シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。
