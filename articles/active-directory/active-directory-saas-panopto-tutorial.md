<properties 
    pageTitle="チュートリアル: Azure Active Directory と Panopto の統合 | Microsoft Azure" 
    description="Azure Active Directory で Panopto を使用して、シングル サインオンや自動プロビジョニングなどを有効にする方法について説明します。" 
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

#チュートリアル: Azure Active Directory と Panopto の統合
  
このチュートリアルでは、Azure と Panopto の統合について説明します。  
このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   Panopto テナント
  
このチュートリアルを完了すると、Panopto に割り当てた Azure AD ユーザーは、Panopto 会社サイト (サービス プロバイダーが開始したサインオン) で、アプリケーションにシングル サインオンまたはできるを使用して、 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。
  
このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  Panopto のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-panopto-tutorial/IC777665.png "Scenario")
##Panopto のアプリケーション統合の有効化
  
このセクションでは、Panopto のアプリケーション統合を有効にする方法について説明します。

###Panopto のアプリケーション統合を有効にするには、次の手順に従います。

1.  左側のナビゲーション ウィンドウで、Azure 管理ポータルで、クリックして **Active Directory**します。

    ![Active Directory](./media/active-directory-saas-panopto-tutorial/IC700993.png "Active Directory")

2.   **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3.  ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。

    ![アプリケーション](./media/active-directory-saas-panopto-tutorial/IC700994.png "Applications")

4.  クリックして **追加** ページの下部にあります。

    ![アプリケーションの追加](./media/active-directory-saas-panopto-tutorial/IC749321.png "Add application")

5.   **実行する操作** ダイアログ ボックスで、をクリックして **ギャラリーからアプリケーションを追加**します。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-panopto-tutorial/IC749322.png "Add an application from gallerry")

6.   **検索ボックス**, 、型 **Panopto**します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-panopto-tutorial/IC777666.png "Appkication Gallery")

7.  結果ウィンドウで [ **Panopto**, 、クリックして **完了** アプリケーションを追加します。

    ![Panopto](./media/active-directory-saas-panopto-tutorial/IC782936.png "Panopto")
##シングル サインオンの構成
  
このセクションでは、ユーザーが SAML プロトコルに基づくフェデレーションを使用して、Azure AD のアカウントで Panopto に対する認証を行えるようにする方法を説明します。  
この手順の途中で、base-64 でエンコードされた証明書ファイルを作成する必要があります。  
この手順に慣れていない場合は、次を参照してください。 [バイナリ証明書をテキスト ファイルに変換する方法](http://youtu.be/PlgrzUZ-Y1o)します。

###シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **Panopto** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、* * [シングル サインオン * * ダイアログ。

    ![シングル サインオンの構成](./media/active-directory-saas-panopto-tutorial/IC777667.png "Configure single sign-on")

2.   **どのようなユーザーの Panopto へのアクセス** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、] をクリックし、 **次**します。

    ![シングル サインオンの構成](./media/active-directory-saas-panopto-tutorial/IC777668.png "Configure single sign-on")

3.   **アプリケーション URL の構成** ] ページで、 **Panopto のサインイン URL** ] ボックスに、次のパターンを使用して URL を入力"*https://\<tenant-name\>。Panopto.com*"、クリックして **次**します。

    ![アプリケーション URL の構成](./media/active-directory-saas-panopto-tutorial/IC777528.png "Configure app URL")

4.   **Panopto でのシングル サインオンの構成** ] ページで、証明書をダウンロードする] をクリックして **証明書のダウンロード**, 、お使いのコンピューターに証明書ファイルを保存します。

    ![シングル サインオンの構成](./media/active-directory-saas-panopto-tutorial/IC777669.png "Configure single sign-on")

5.  別の Web ブラウザーのウィンドウで、Panopto 企業サイトに管理者としてログインします。

6.  ツールバーの左側でクリックして **システム**, 、クリックして **Id プロバイダー**します。

    ![System](./media/active-directory-saas-panopto-tutorial/IC777670.png "System")

7.  クリックして **プロバイダーを追加する**です。

    ![ID プロバイダー](./media/active-directory-saas-panopto-tutorial/IC777671.png "Identity Providers")

8.  [SAML プロバイダー] セクションで、次の手順に従います。

    ![SaaS の構成](./media/active-directory-saas-panopto-tutorial/IC777672.png "SaaS configuration")

    1.   **プロバイダーの種類** 一覧で、[ **SAML20**
    2.   **インスタンス名** ] ボックスに、インスタンスの名前を入力します。
    3.   **わかりやすい説明** ] ボックスに、わかりやすい説明を入力します。
    4.  Azure ポータルで、 **Panopto でのシングル サインオンの構成** ] ダイアログ ページでコピー、 **発行者 URL** 値に設定して、貼り付けます、 **発行者** ] ボックスに貼り付けます。
    5.  Azure ポータルで、 **Panopto でのシングル サインオンの構成** ] ダイアログ ページでコピー、 **SAML SSO URL** 値に設定して、貼り付けます、 **Bounce Page Url** ] ボックスに貼り付けます。
    6.  作成、 **base-64 でエンコードされた** からダウンロードした証明書ファイル。  

        >[AZURE.TIP] 詳細については、次を参照してください [バイナリ証明書をテキスト ファイルに変換する方法。](http://youtu.be/PlgrzUZ-Y1o)

    7.  Base-64 でエンコードされた証明書をメモ帳で開き、という内容がクリップボードにコピーおよびに貼り付け、 **公開キー** ] テキスト ボックス
    8.  クリックして **保存**します。
        ![保存](./media/active-directory-saas-panopto-tutorial/IC777673.png "Save")

9.  Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![シングル サインオンの構成](./media/active-directory-saas-panopto-tutorial/IC777674.png "Configure single sign-on")
##ユーザー プロビジョニングの構成
  
Panopto へのユーザー プロビジョニングの構成にあたって必要な操作はありません。  
割り当てられたユーザーがアクセス パネルを使用して Panopto にログインしようとすると、そのユーザーが存在するかどうかが Panopto によって確認されます。  
使用可能なユーザー アカウントがない場合、ユーザー アカウントは Panopto により自動的に作成されます。

>[AZURE.NOTE]他の Panopto ユーザー アカウント作成ツールを使用するまたは Azure AD ユーザー アカウントをプロビジョニングする Panopto から提供されている Api です。

##ユーザーの割り当て
  
構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

###ユーザーを Panopto に割り当てるには、次の手順に従います。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  * * Panopto * * アプリケーション統合ページで、をクリックして **ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-panopto-tutorial/IC777675.png "Assign users")

3.  テスト ユーザーを選択して [ **割り当てる**, 、順にクリック **はい** して割り当てを確認します。

    ![あり](./media/active-directory-saas-panopto-tutorial/IC767830.png "Yes")
  
シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。

