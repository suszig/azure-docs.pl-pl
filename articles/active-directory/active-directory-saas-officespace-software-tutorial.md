<properties 
    pageTitle="チュートリアル: Azure Active Directory と OfficeSpace Software の統合 | Microsoft Azure" 
    description="Azure Active Directory と OfficeSpace Software を使用してでのシングル サインオン、自動化されたプロビジョニング、および詳細を有効にする方法を説明します。" 
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


# チュートリアル: Azure Active Directory と OfficeSpace Software の統合

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
## OfficeSpace Software のアプリケーション統合の有効化

このセクションでは、OfficeSpace Software のアプリケーション統合を有効にする方法を説明します。

### OfficeSpace Software のアプリケーション統合を有効にするには、次の手順に従います。

1.  Azure 管理ポータルの左側のナビゲーション ウィンドウで、**[Active Directory]** をクリックします。

    ![Active Directory](./media/active-directory-saas-officespace-software-tutorial/IC700993.png "Active Directory")

2.  **[ディレクトリ]** の一覧から、ディレクトリ統合を有効にするディレクトリを選択します。

3.  アプリケーション ビューを開くには、ディレクトリ ビューでトップ メニューの **[アプリケーション]** をクリックします。

    ![アプリケーション](./media/active-directory-saas-officespace-software-tutorial/IC700994.png "Applications")

4.  ページの下部にある **[追加]** をクリックします。

    ![アプリケーションの追加](./media/active-directory-saas-officespace-software-tutorial/IC749321.png "Add application")

5.  **[実行する内容]** ダイアログで、**[ギャラリーからアプリケーションを追加します]** をクリックします。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-officespace-software-tutorial/IC749322.png "Add an application from gallerry")

6.  **[検索ボックス]** に、「**OfficeSpace Software**」と入力します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-officespace-software-tutorial/IC777765.png "Application gallery")

7.  結果ウィンドウで **[OfficeSpace Software]** を選び、**[完了]** をクリックしてアプリケーションを追加します。

    ![OfficeSpace Software](./media/active-directory-saas-officespace-software-tutorial/IC781007.png "OfficeSpace Software")
## シングル サインオンの構成

このセクションでは、ユーザーが SAML プロトコルに基づくフェデレーションを使用して、Azure AD でのユーザーのアカウントで OfficeSpace Software に対する認証を行えるようにする方法を説明します。  
OfficeSpace Software にシングル サインオンを構成するには、証明書の拇印の値を取得する必要があります。  
この手順に慣れていない場合は、次を参照してください。 [証明書の拇印の値を取得する方法](http://youtu.be/YKQF266SAxI)します。

### シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **OfficeSpace Software** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、 ** [シングル サインオン ** ダイアログ。

    ![シングル サインオンの構成](./media/active-directory-saas-officespace-software-tutorial/IC777766.png "Configure single sign=on")

2.  **どのようなユーザーを OfficeSpace Software にサインオンする** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、] をクリックし、 **次**します。

    ![シングル サインオンの構成](./media/active-directory-saas-officespace-software-tutorial/IC777767.png "Configure single sign-on")

3.  **アプリケーション URL の構成** ] ページの [、 **OfficeSpace Software サインオン URL** URL は、ユーザーが OfficeSpace Software アプリケーションへのサインオンに使用] ボックスに、型 (例::"*https://company.officespacesoftware.com*")、順にクリック **次**します。

    ![アプリケーション URL の構成](./media/active-directory-saas-officespace-software-tutorial/IC775556.png "Configure App URL")

4.  **OfficeSpace Software でのシングル サインオンの構成** ] ページで、証明書をダウンロードする] をクリックして **証明書のダウンロード**, 、お使いのコンピューターにローカルで証明書ファイルを保存します。

    ![シングル サインオンの構成](./media/active-directory-saas-officespace-software-tutorial/IC793769.png "Configure single sign-on")

5.  別の Web ブラウザーのウィンドウで、管理者として OfficeSpace Software 企業サイトにログインします。

6.  移動して **Admin \ > コネクタ**します。

    ![管理者](./media/active-directory-saas-officespace-software-tutorial/IC777769.png "Admin")

7.  **[SAML 承認]** をクリックします。

    ![コネクタ](./media/active-directory-saas-officespace-software-tutorial/IC777770.png "Connectors")

8.  **[SAML 承認]** セクションで、次の手順に従います。

    ![SAML の構成](./media/active-directory-saas-officespace-software-tutorial/IC777771.png "SAML configuration")

    1.  Azure ポータルの **[OfficeSpace Software でのシングル サインオンの構成]** ダイアログ ページで、**[リモート ログイン URL]** 値をコピーして、**[ログアウト プロバイダー URL]** ボックスに貼り付けます。
    2.  Azure ポータルの **[OfficeSpace Software でのシングル サインオンの構成]** ダイアログ ページで、**[リモート ログアウト URL]** 値をコピーして、**[クライアント IDP ターゲット URL]** ボックスに貼り付けます。
    3.  エクスポートした証明書から **[拇印]** の値をコピーして、**[クライアント IDP 証明書の指紋]** ボックスに貼り付けます。
        >[AZURE.TIP]
        詳細については、「 [証明書拇印値を取得する方法](http://youtu.be/YKQF266SAxI)

    4.  **[Save Settings]** をクリックします。

9.  Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![シングル サインオンの構成](./media/active-directory-saas-officespace-software-tutorial/IC777772.png "Configure single sign-on")
## ユーザー プロビジョニングの構成

Azure AD ユーザーの OfficeSpace Software へのログインを有効にするためには、ユーザーを OfficeSpace Software にプロビジョニングする必要があります。 OfficeSpace Software の場合は、プロビジョニングは自動化されています。  
アイテムを操作することはありません。  
最初のシングル サインオンの試行中に、必要に応じてユーザーが自動的に作成されます。
>[AZURE.NOTE]他の OfficeSpace Software ユーザー アカウントの作成ツールまたは OfficeSpace Software から提供されている API を使用して、AAD ユーザー アカウントをプロビジョニングできます。

## ユーザーを割り当てる

構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

### ユーザーを OfficeSpace Software に割り当てるには、次の手順に従います。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  **OfficeSpace Software * * アプリケーション統合ページで、をクリックして * * ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-officespace-software-tutorial/IC777773.png "Assign users")

3.  テスト ユーザーを選択し、**[割り当て]**、**[はい]** の順にクリックして、割り当てを確定します。

    ![はい](./media/active-directory-saas-officespace-software-tutorial/IC767830.png "Yes")

シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。




