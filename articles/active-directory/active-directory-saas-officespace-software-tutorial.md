<properties 
    pageTitle="チュートリアル: Azure Active Directory と OfficeSpace Software の統合 | Microsoft Azure" 
    description="Azure Active Directory で OfficeSpace Software を使用して、シングル サインオンや自動プロビジョニングなどを有効にする方法について説明します。" 
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

#チュートリアル: Azure Active Directory と OfficeSpace Software の統合
  
このチュートリアルの目的は、Azure と OfficeSpace Software の統合を示すことです。  
このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   シングル サインオン対応の OfficeSpace Software サブスクリプション
  
このチュートリアルを完了すると、OfficeSpace Software に割り当てた Azure AD ユーザーは、OfficeSpace Software 会社サイト (サービス プロバイダーが開始したサインオン) で、アプリケーションにシングル サインオンまたはできるを使用して、 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。
  
このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  OfficeSpace Software のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-officespace-software-tutorial/IC777764.png "Scenario")
##OfficeSpace Software のアプリケーション統合の有効化
  
このセクションでは、OfficeSpace Software のアプリケーション統合を有効にする方法を説明します。

###OfficeSpace Software のアプリケーション統合を有効にするには、次の手順に従います。

1.  左側のナビゲーション ウィンドウで、Azure 管理ポータルで、クリックして **Active Directory**します。

    ![Active Directory](./media/active-directory-saas-officespace-software-tutorial/IC700993.png "Active Directory")

2.   **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3.  ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。

    ![アプリケーション](./media/active-directory-saas-officespace-software-tutorial/IC700994.png "Applications")

4.  クリックして **追加** ページの下部にあります。

    ![アプリケーションの追加](./media/active-directory-saas-officespace-software-tutorial/IC749321.png "Add application")

5.   **実行する操作** ダイアログ ボックスで、をクリックして **ギャラリーからアプリケーションを追加**します。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-officespace-software-tutorial/IC749322.png "Add an application from gallerry")

6.   **検索ボックス**, 、型 **OfficeSpace Software**します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-officespace-software-tutorial/IC777765.png "Application gallery")

7.  結果ウィンドウで [ **OfficeSpace Software**, 、クリックして **完了** アプリケーションを追加します。

    ![OfficeSpace Software](./media/active-directory-saas-officespace-software-tutorial/IC781007.png "OfficeSpace Software")
##シングル サインオンの構成
  
このセクションでは、ユーザーが SAML プロトコルに基づくフェデレーションを使用して、Azure AD でのユーザーのアカウントで OfficeSpace Software に対する認証を行えるようにする方法を説明します。  
OfficeSpace Software にシングル サインオンを構成するには、証明書の拇印の値を取得する必要があります。  
この手順に慣れていない場合は、次を参照してください。 [証明書の拇印の値を取得する方法](http://youtu.be/YKQF266SAxI)します。

###シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **OfficeSpace Software** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、* * [シングル サインオン * * ダイアログ。

    ![シングル サインオンの構成](./media/active-directory-saas-officespace-software-tutorial/IC777766.png "Configure single sign=on")

2.   **どのようなユーザーを OfficeSpace Software にサインオンする** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、] をクリックし、 **次**します。

    ![Configure single sign-on](./media/active-directory-saas-officespace-software-tutorial/IC777767.png "Configure single sign-on")

3.   **アプリケーション URL の構成** ] ページの [、 **OfficeSpace Software サインオン URL** URL は、ユーザーが OfficeSpace Software アプリケーションへのサインオンに使用] ボックスに、型 (例::"*https://company.officespacesoftware.com*")、順にクリック **次**します。

    ![Configure App URL](./media/active-directory-saas-officespace-software-tutorial/IC775556.png "Configure App URL")

4.   **OfficeSpace Software でのシングル サインオンの構成** ] ページで、証明書をダウンロードする] をクリックして **証明書のダウンロード**, 、お使いのコンピューターにローカルで証明書ファイルを保存します。

    ![シングル サインオンの構成](./media/active-directory-saas-officespace-software-tutorial/IC793769.png "Configure single sign-on")

5.  別の Web ブラウザーのウィンドウで、管理者として OfficeSpace Software 企業サイトにログインします。

6.  移動して **Admin \ > コネクタ**します。

    ![管理者](./media/active-directory-saas-officespace-software-tutorial/IC777769.png "Admin")

7.  クリックして **SAML 認証**します。

    ![コネクタ](./media/active-directory-saas-officespace-software-tutorial/IC777770.png "Connectors")

8.   **SAML 認証** セクションで、次の手順に従います。

    ![SAML の構成](./media/active-directory-saas-officespace-software-tutorial/IC777771.png "SAML configuration")

    1.  Azure ポータルで、 **OfficeSpace Software でのシングル サインオンの構成** ] ダイアログ ページでコピー、 **リモート ログイン URL** 値に設定して、貼り付けます、 **ログアウト プロバイダー url** ] ボックスに貼り付けます。
    2.  Azure ポータルで、 **OfficeSpace Software でのシングル サインオンの構成** ] ダイアログ ページでコピー、 **リモート ログアウト URL** 値に設定して、貼り付けます、 **クライアント idp ターゲット url** ] ボックスに貼り付けます。
    3.  コピー、 **拇印** エクスポートした証明書からの値し、貼り付けます、 **クライアント idp cert fingerprint** ] ボックスに貼り付けます。  

        >[AZURE.TIP]
        詳細については、次を参照してください [証明書の拇印の値を取得する方法。](http://youtu.be/YKQF266SAxI)

    4.  クリックして **設定を保存する**です。

9.  Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![シングル サインオンの構成](./media/active-directory-saas-officespace-software-tutorial/IC777772.png "Configure single sign-on")
##ユーザー プロビジョニングの構成
  
Azure AD ユーザーの OfficeSpace Software へのログインを有効にするためには、ユーザーを OfficeSpace Software にプロビジョニングする必要があります。 OfficeSpace Software の場合は、プロビジョニングは自動化されています。  
ユーザー側で必要な操作はありません。  
最初のシングル サインオンの試行中に、必要に応じてユーザーが自動的に作成されます。

>[AZURE.NOTE]他の OfficeSpace Software ユーザー アカウント作成ツールを使用することができますか、AAD ユーザー アカウントをプロビジョニング、OfficeSpace Software 提供の Api です。

##ユーザーの割り当て
  
構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

###ユーザーを OfficeSpace Software に割り当てるには、次の手順に従います。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  * * OfficeSpace Software * * アプリケーション統合ページで、をクリックして **ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-officespace-software-tutorial/IC777773.png "Assign users")

3.  テスト ユーザーを選択して [ **割り当てる**, 、順にクリック **はい** して割り当てを確認します。

    ![あり](./media/active-directory-saas-officespace-software-tutorial/IC767830.png "Yes")
  
シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。

