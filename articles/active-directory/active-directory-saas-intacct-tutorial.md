<properties 
    pageTitle="チュートリアル: Azure Active Directory と Intacct の統合 | Microsoft Azure" 
    description="Azure Active Directory で Intacct を使用して、シングル サインオンや自動プロビジョニングなどを有効にする方法について説明します。" 
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

#チュートリアル: Azure Active Directory と Intacct の統合
  
このチュートリアルでは、Azure と Intacct の統合について説明します。  
このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   Intacct テナント
  
このチュートリアルを完了すると、Intacct に割り当てた Azure AD ユーザーは、Intacct 会社サイト (サービス プロバイダーが開始したサインオン) で、アプリケーションにシングル サインオンまたはできるを使用して、 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。
  
このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  Intacct のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-intacct-tutorial/IC790030.png "Scenario")
##Intacct のアプリケーション統合の有効化
  
このセクションでは、Intacct のアプリケーション統合を有効にする方法について説明します。

###Intacct のアプリケーション統合を有効にするには、次の手順を実行します。

1.  左側のナビゲーション ウィンドウで、Azure 管理ポータルで、クリックして **Active Directory**します。

    ![Active Directory](./media/active-directory-saas-intacct-tutorial/IC700993.png "Active Directory")

2.   **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3.  ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。

    ![アプリケーション](./media/active-directory-saas-intacct-tutorial/IC700994.png "Applications")

4.  クリックして **追加** ページの下部にあります。

    ![アプリケーションの追加](./media/active-directory-saas-intacct-tutorial/IC749321.png "Add application")

5.   **実行する操作** ダイアログ ボックスで、をクリックして **ギャラリーからアプリケーションを追加**します。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-intacct-tutorial/IC749322.png "Add an application from gallerry")

6.   **検索ボックス**, 、型 **Intacct**します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-intacct-tutorial/IC790031.png "Application Gallery")

7.  結果ウィンドウで [ **Intacct**, 、クリックして **完了** アプリケーションを追加します。

    ![Intacct](./media/active-directory-saas-intacct-tutorial/IC790032.png "Intacct")
##シングル サインオンの構成
  
このセクションでは、SAML プロトコルに基づくフェデレーションを使用して、ユーザーが Azure AD のアカウントで Intacct に対する認証を行うことができるようにする方法を説明します。  
この手順の途中で、base-64 でエンコードされた証明書ファイルを作成する必要があります。  
この手順に慣れていない場合は、次を参照してください。 [バイナリ証明書をテキスト ファイルに変換する方法](http://youtu.be/PlgrzUZ-Y1o)します。

###シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **Intacct** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、* * [シングル サインオン * * ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-intacct-tutorial/IC790033.png "Configure Single Sign-On")

2.   **どのようなユーザーの Intacct に** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、] をクリックし、 **次**します。

    ![Configure Single Sign-On](./media/active-directory-saas-intacct-tutorial/IC790034.png "Configure Single Sign-On")

3.   **アプリケーション URL の構成** ] ページの [、 **Intacct サインオン URL** ] ボックスに、次のパターンを使用して URL を入力"*https://Intacct.com/company*"、] をクリックし、 **次**します。

    ![Configure App URL](./media/active-directory-saas-intacct-tutorial/IC790035.png "Configure App URL")

4.   **Intacct でのシングル サインオンの構成** ] ページで [ **証明書のダウンロード**, 、お使いのコンピューターに証明書ファイルを保存します。

    ![Configure Single Sign-On](./media/active-directory-saas-intacct-tutorial/IC790036.png "Configure Single Sign-On")

5.  別の Web ブラウザー ウィンドウで、Intacct 企業サイトに管理者としてログインします。

6.  クリックして、 **会社** タブをクリックし、をクリックし、 **会社情報**します。

    ![会社](./media/active-directory-saas-intacct-tutorial/IC790037.png "Company")

7.  クリックして、 **セキュリティ** タブをクリックし、をクリックし、 **編集**します。

    ![セキュリティ](./media/active-directory-saas-intacct-tutorial/IC790038.png "Security")

8.   **シングル サインオン (SSO)** セクションで、次の手順に従います。

    ![シングル サインオン](./media/active-directory-saas-intacct-tutorial/IC790039.png "Single Sign On")

    1.  選択 **でシングル サインオンを有効にする**です。
    2.  として **Id プロバイダーの種類**, [ **SAML 2.0**します。
    3.  Azure ポータルで、 **Intacct でのシングル サインオンの構成** ] ダイアログ ページでコピー、 **発行者 URL** 値に設定して、貼り付けます、 **発行者 URL** ] ボックスに貼り付けます。
    4.  Azure ポータルで、 **Intacct でのシングル サインオンの構成** ] ダイアログ ページでコピー、 **リモート ログイン URL** 値に設定して、貼り付けます、 **ログイン URL** ] ボックスに貼り付けます。
    5.  作成、 **base-64 でエンコードされた** からダウンロードした証明書ファイル。
        
        >[AZURE.TIP]詳細については、次を参照してください [バイナリ証明書をテキスト ファイルに変換する方法。](http://youtu.be/PlgrzUZ-Y1o)

    6.  Base-64 でエンコードされた証明書をメモ帳で開き、という内容がクリップボードにコピーおよびに貼り付け、 **証明書** ] テキスト ボックス
    7.  クリックして **保存**します。

9.  Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-intacct-tutorial/IC790040.png "Configure Single Sign-On")
##ユーザー プロビジョニングの構成
  
Azure AD ユーザーが Intacct にログインできるようにするには、そのユーザーを Intacct にプロビジョニングする必要があります。  
Intacct の場合、プロビジョニングは手動で行います。

###ユーザー アカウントをプロビジョニングするには、次の手順を実行します。

1.  ログインして **Intacct** テナントです。

2.  クリックして、 **会社** タブをクリックし、をクリックし、 **ユーザー**します。

    ![ユーザー](./media/active-directory-saas-intacct-tutorial/IC790041.png "Users")

3.  クリックして、 **追加** ] タブ

    ![[追加] のいずれかを](./media/active-directory-saas-intacct-tutorial/IC790042.png "Add")

4.   **ユーザー情報** セクションで、次の手順に従います。

    ![ユーザー情報](./media/active-directory-saas-intacct-tutorial/IC790043.png "User Information")

    1.  型、 **ユーザー ID**, 、 **姓、名**, 、**名**, 、 **電子メール アドレス**, 、 **タイトル** と **Phone** 関連テキスト ボックスに、プロビジョニングする Azure AD アカウントのです。
    2.  選択、 **Admin 特権** 、プロビジョニングする Azure AD アカウントのです。
    3.  クリックして **保存**します。
        
        >[AZURE.NOTE] AAD アカウントの所有者は電子メールを受信し、アクティブ化する前に、自分のアカウントを確認するリンクを参照します。

>[AZURE.NOTE] 他の Intacct ユーザー アカウント作成ツールを使用することができますか、AAD ユーザー アカウントをプロビジョニング、Intacct から提供されている Api です。

##ユーザーの割り当て
  
構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

###ユーザーを Intacct に割り当てるには、次の手順を実行します。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  * * Intacct * * アプリケーション統合ページで、をクリックして **ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-intacct-tutorial/IC790044.png "Assign Users")

3.  テスト ユーザーを選択して [ **割り当てる**, 、順にクリック **はい** して割り当てを確認します。

    ![あり](./media/active-directory-saas-intacct-tutorial/IC767830.png "Yes")
  
シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。
