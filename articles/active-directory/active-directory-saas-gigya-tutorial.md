<properties 
    pageTitle="チュートリアル: Azure Active Directory と Gigya の統合 | Microsoft Azure" 
    description="Azure Active Directory で Gigya を使用して、シングル サインオンや自動プロビジョニングなどを有効にする方法について説明します。" 
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

#チュートリアル: Azure Active Directory と Gigya の統合
  
このチュートリアルでは、Azure と Gigya の統合について説明します。  
このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   Gigya でのシングル サインオンが有効なサブスクリプション
  
このチュートリアルを完了すると、Gigya に割り当てた Azure AD ユーザーは、Gigya 会社サイト (サービス プロバイダーが開始したサインオン) で、アプリケーションにシングル サインオンまたはできるを使用して、 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。
  
このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  Gigya のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![Configure Single Sign-On](./media/active-directory-saas-gigya-tutorial/IC789512.png "Configure Single Sign-On")
##Gigya のアプリケーション統合の有効化
  
このセクションでは、Gigya のアプリケーション統合を有効にする方法について説明します。

###Gigya のアプリケーション統合を有効にするには、次の手順を実行します。

1.  左側のナビゲーション ウィンドウで、Azure 管理ポータルで、クリックして **Active Directory**します。

    ![Active Directory](./media/active-directory-saas-gigya-tutorial/IC700993.png "Active Directory")

2.   **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3.  ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。

    ![アプリケーション](./media/active-directory-saas-gigya-tutorial/IC700994.png "Applications")

4.  クリックして **追加** ページの下部にあります。

    ![アプリケーションの追加](./media/active-directory-saas-gigya-tutorial/IC749321.png "Add application")

5.   **実行する操作** ダイアログ ボックスで、をクリックして **ギャラリーからアプリケーションを追加**します。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-gigya-tutorial/IC749322.png "Add an application from gallerry")

6.   **検索ボックス**, 、型 **Gigya**します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-gigya-tutorial/IC789513.png "Application Gallery")

7.  結果ウィンドウで [ **Gigya**, 、クリックして **完了** アプリケーションを追加します。

    ![Gigya](./media/active-directory-saas-gigya-tutorial/IC789527.png "Gigya")
##シングル サインオンの構成
  
このセクションでは、ユーザーが SAML プロトコルに基づくフェデレーションを使用して、Azure AD でのユーザーのアカウントで Gigya に対する認証を行うことができるようにする方法を説明します。  
この手順の途中で、base-64 でエンコードされた証明書ファイルを作成する必要があります。  
この手順に慣れていない場合は、次を参照してください。 [バイナリ証明書をテキスト ファイルに変換する方法](http://youtu.be/PlgrzUZ-Y1o)します。

###シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **Gigya** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、* * [シングル サインオン * * ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-gigya-tutorial/IC789528.png "Configure Single Sign-On")

2.   **どのようなユーザーが Gigya へのサインオン** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、順にクリック **次**します。

    ![Configure Single Sign-On](./media/active-directory-saas-gigya-tutorial/IC789529.png "Configure Single Sign-On")

3.   **アプリケーション URL の構成** ] ページの [、 **Gigya サインオン URL** ] ボックスに、次のパターンを使用して URL を入力"*http://company.gigya.com*"、] をクリックし、 **次**します。

    ![アプリケーション URL の構成](./media/active-directory-saas-gigya-tutorial/IC789530.png "Configure App URL")

4.   **Gigya でのシングル サインオンの構成** ] ページで [ **証明書のダウンロード**, 、お使いのコンピューターに証明書ファイルを保存します。

    ![Configure Single Sign-On](./media/active-directory-saas-gigya-tutorial/IC789531.png "Configure Single Sign-On")

5.  別の Web ブラウザー ウィンドウで、Gigya 企業サイトに管理者としてログインします。

6.  移動して **設定 \ > SAML ログイン**, 、順にクリックし、 **追加** ] ボタンをクリックします。

    ![SAML のログイン](./media/active-directory-saas-gigya-tutorial/IC789532.png "SAML Login")

7.   **SAML ログイン** セクションで、次の手順に従います。

    ![SAML の構成](./media/active-directory-saas-gigya-tutorial/IC789533.png "SAML Configuration")

    1.   **名前** ] ボックスに、構成の名前を入力します。
    2.  Azure ポータルで、 **Gigya でのシングル サインオンの構成** ] ダイアログ ページでコピー、 **発行者 URL** 値に設定して、貼り付けます、 **発行者** ] ボックスに貼り付けます。
    3.  Azure ポータルで、 **Gigya でのシングル サインオンの構成** ] ダイアログ ページでコピー、 **シングル サインオン サービス URL** 値に設定して、貼り付けます、 **シングル サインオン サービス URL** ] ボックスに貼り付けます。
    4.  Azure ポータルで、 **Gigya でのシングル サインオンの構成** ] ダイアログ ページでコピー、 **名前 Id 形式** 値に設定して、貼り付けます、 **名前 ID 形式** ] ボックスに貼り付けます。
    5.  作成、 **base-64 でエンコードされた** からダウンロードした証明書ファイル。
        
        >[AZURE.TIP]詳細については、次を参照してください [バイナリ証明書をテキスト ファイルに変換する方法。](http://youtu.be/PlgrzUZ-Y1o)

    6.  Base-64 でエンコードされた証明書をメモ帳で開き、という内容がクリップボードにコピーおよびに貼り付け、 **X.509 証明書** ] ボックスに貼り付けます。
    7.  クリックして **設定を保存する**です。

8.  Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-gigya-tutorial/IC789534.png "Configure Single Sign-On")
##ユーザー プロビジョニングの構成
  
Azure AD ユーザーが Gigya にログインできるようにするには、ユーザーを Gigya にプロビジョニングする必要があります。  
Gigya の場合、プロビジョニングは手動で行います。

###ユーザー アカウントをプロビジョニングするには、次の手順を実行します。

1.  ログインして **Gigya** 会社サイトに管理者として。

2.  移動して **Admin \ > ユーザーの管理**, 、] をクリックし、 **ユーザーの招待**します。

    ![Manage Users](./media/active-directory-saas-gigya-tutorial/IC789535.png "Manage Users")

3.  [ユーザーの招待] ダイアログで、次の手順を実行します。

    ![ユーザーの招待](./media/active-directory-saas-gigya-tutorial/IC789536.png "Invite Users")

    1.   **電子メール** ] ボックスに、プロビジョニングする有効な Azure Active Directory アカウントの電子メール エイリアスを入力します。
    2.  クリックして **ユーザーの招待**します。
    
        >[AZURE.NOTE] Azure Active Directory アカウント所有者には、アクティブ化する前に、アカウントの確認へのリンクを含むメールを受け取ります。

>[AZURE.NOTE] 他の Gigya ユーザー アカウント作成ツールを使用することができますか、AAD ユーザー アカウントをプロビジョニング、Gigya から提供されている Api です。

##ユーザーの割り当て
  
構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

###ユーザーを Gigya に割り当てるには、次の手順を実行します。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  * * Gigya * * アプリケーション統合ページで、をクリックして **ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-gigya-tutorial/IC789537.png "Assign Users")

3.  テスト ユーザーを選択して [ **割り当てる**, 、順にクリック **はい** して割り当てを確認します。

    ![あり](./media/active-directory-saas-gigya-tutorial/IC767830.png "Yes")
  
シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。
