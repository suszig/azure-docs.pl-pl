<properties 
    pageTitle="チュートリアル: Azure Active Directory と Bime の統合 | Microsoft Azure" 
    description="Azure Active Directory で Bime を使用して、シングル サインオンや自動プロビジョニングなどを有効にする方法について説明します。" 
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

#チュートリアル: Azure Active Directory と Bime の統合

このチュートリアルでは、Azure と Bime の統合について説明します。  
このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   Bime テナント

このチュートリアルを完了すると、Bime に割り当てた Azure AD ユーザーは、Bime 会社サイト (サービス プロバイダーが開始したサインオン) で、アプリケーションにシングル サインオンまたはできるを使用して、 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。

このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  Bime のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-bime-tutorial/IC775552.png "Scenario")
##Bime のアプリケーション統合の有効化

このセクションでは、Bime のアプリケーション統合を有効にする方法について説明します。

###Bime のアプリケーション統合を有効にするには、次の手順を実行します。

1.  左側のナビゲーション ウィンドウで、Azure 管理ポータルで、クリックして **Active Directory**します。

    ![Active Directory](./media/active-directory-saas-bime-tutorial/IC700993.png "Active Directory")

2.   **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3.  ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。

    ![アプリケーション](./media/active-directory-saas-bime-tutorial/IC700994.png "Applications")

4.  クリックして **追加** ページの下部にあります。

    ![アプリケーションの追加](./media/active-directory-saas-bime-tutorial/IC749321.png "Add application")

5.   **実行する操作** ダイアログ ボックスで、をクリックして **ギャラリーからアプリケーションを追加**します。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-bime-tutorial/IC749322.png "Add an application from gallerry")

6.   **検索ボックス**, 、型 **Bime**します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-bime-tutorial/IC775553.png "Application Gallery")

7.  結果ウィンドウで [ **Bime**, 、クリックして **完了** アプリケーションを追加します。

    ![Bime](./media/active-directory-saas-bime-tutorial/IC775554.png "Bime")
##シングル サインオンの構成

このセクションでは、SAML プロトコルに基づくフェデレーションを使用して、ユーザーが Azure AD のアカウントで Bime に対する認証を行えるようにする方法を説明します。  
Bime のシングル サインオンを構成するには、証明書からサムプリント値を取得する必要があります。  
この手順に慣れていない場合は、次を参照してください。 [証明書の拇印の値を取得する方法](http://youtu.be/YKQF266SAxI)します。

###シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **Bime** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、* * [シングル サインオン * * ダイアログ。

    ![Configure single sign-on](./media/active-directory-saas-bime-tutorial/IC771709.png "Configure single sign-on")

2.   **どのようなユーザー Bime にサインオンする** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、] をクリックし、 **次**します。

    ![Configure Single Sign-On](./media/active-directory-saas-bime-tutorial/IC775555.png "Configure Single Sign-On")

3.   **アプリケーション URL の構成** ] ページで、 **Bime サインイン URL** ] ボックスに、次のパターンを使用して URL を入力"*https://\<tenant-name\>。Bimeapp.com*"、クリックして **次**します。

    ![Configure App URL](./media/active-directory-saas-bime-tutorial/IC775556.png "Configure App URL")

4.   **Bime でのシングル サインオンの構成** ] ページで、証明書をダウンロードする] をクリックして **証明書のダウンロード**, 、としては、証明書ファイルをローカルに保存および **c:\\Bime.cer**します。

    ![Configure Single Sign-On](./media/active-directory-saas-bime-tutorial/IC775557.png "Configure Single Sign-On")

5.  別の Web ブラウザー ウィンドウで、Bime 企業サイトに管理者としてログインします。

6.  ツールバーで、次のようにクリックします。 **Admin**, 、し **アカウント**します。

    ![管理者](./media/active-directory-saas-bime-tutorial/IC775558.png "Admin")

7.  アカウント構成ページで、次の手順に従います。

    ![Configure Single Sign-On](./media/active-directory-saas-bime-tutorial/IC775559.png "Configure Single Sign-On")

    1.  選択 **Enable SAML authentication**します。
    2.  Azure ポータルで、 **Bime でのシングル サインオンの構成** ] ダイアログ ページでコピー、 **リモート ログイン URL** 値に設定して、貼り付けます、 **リモート ログイン URL** ] ボックスに貼り付けます。
    3.  コピー、 **拇印** エクスポートした証明書からの値し、貼り付けます、 **Certificate Fingerprint** ] ボックスに貼り付けます。  

        >[AZURE.TIP] 詳細については、次を参照してください [証明書の拇印の値を取得する方法。](http://youtu.be/YKQF266SAxI)

    4.  クリックして **保存**します。

8.  Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-bime-tutorial/IC775560.png "Configure Single Sign-On")
##ユーザー プロビジョニングの構成

Azure AD ユーザーが Bime にログインできるようにするには、ユーザーを Bime にプロビジョニングする必要があります。  
Bime の場合、プロビジョニングは手動で行います。

###ユーザー プロビジョニングを構成するには、次の手順に従います。

1.  ログインして **Bime** テナントです。

2.  ツールバーで、次のようにクリックします。 **Admin**, 、し **ユーザー**します。

    ![管理者](./media/active-directory-saas-bime-tutorial/IC775561.png "Admin")

3.   **Users List**, 、] をクリックして **Add New User** (「+」) です。

    ![ユーザー](./media/active-directory-saas-bime-tutorial/IC775562.png "Users")

4.   **ユーザー詳細** ] ダイアログ ページで、次の手順を実行します。

    ![ユーザーの詳細](./media/active-directory-saas-bime-tutorial/IC775563.png "User Details")

    1.  プロビジョニングする有効な AAD アカウントの名、姓、ログイン、電子メールを入力します。
    2.  [保存] をクリックします。

>[AZURE.NOTE] 他の Bime ユーザー アカウント作成ツールを使用するや、AAD ユーザー アカウントをプロビジョニング、Bime 提供の Api です。

##ユーザーの割り当て

構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

###ユーザーを Bime に割り当てるには、次の手順を実行します。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  * * Bime * * アプリケーション統合ページで、をクリックして **ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-bime-tutorial/IC775564.png "Assign users")

3.  テスト ユーザーを選択して [ **割り当てる**, 、順にクリック **はい** して割り当てを確認します。

    ![あり](./media/active-directory-saas-bime-tutorial/IC767830.png "Yes")

シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。

