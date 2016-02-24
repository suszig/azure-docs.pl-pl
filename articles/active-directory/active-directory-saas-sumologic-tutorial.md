<properties 
    pageTitle="チュートリアル: Azure Active Directory と SumoLogic の統合 | Microsoft Azure" 
    description="Azure Active Directory で SumoLogic を使用して、シングル サインオンや自動プロビジョニングなどを有効にする方法を説明します。" 
    services="active-directory" 
    authors="MarkusVi"  
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

#チュートリアル: Azure Active Directory と SumoLogic の統合
  
このチュートリアルでは、Azure と SumoLogic の統合について説明します。  
このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   SumoLogic テナント
  
このチュートリアルを完了すると、sumologic のアプリケーションにシングル サインオンできるをするように割り当てられた Azure AD ユーザーの会社サイト (サービス プロバイダーが開始したサインオン)、またはを使用して、 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。
  
このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  SumoLogic のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-sumologic-tutorial/IC778549.png "Scenario")

##SumoLogic のアプリケーション統合の有効化
  
このセクションでは、SumoLogic のアプリケーション統合を有効にする方法を説明します。

###SumoLogic のアプリ統合を有効にするには、次の手順に従います。

1.  左側のナビゲーション ウィンドウで、Azure 管理ポータルで、クリックして **Active Directory**します。

    ![Active Directory](./media/active-directory-saas-sumologic-tutorial/IC700993.png "Active Directory")

2.   **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3.  ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。

    ![アプリケーション](./media/active-directory-saas-sumologic-tutorial/IC700994.png "Applications")

4.  クリックして **追加** ページの下部にあります。

    ![アプリケーションの追加](./media/active-directory-saas-sumologic-tutorial/IC749321.png "Add application")

5.   **実行する操作** ダイアログ ボックスで、をクリックして **ギャラリーからアプリケーションを追加**します。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-sumologic-tutorial/IC749322.png "Add an application from gallerry")

6.   **検索ボックス**, 、型 **sumologic**します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-sumologic-tutorial/IC778550.png "Application gallery")

7.  結果ウィンドウで [ **SumoLogic**, 、クリックして **完了** アプリケーションを追加します。

    ![SumoLogic](./media/active-directory-saas-sumologic-tutorial/IC778551.png "SumoLogic")

##シングル サインオンの構成
  
このセクションでは、SAML プロトコルに基づくフェデレーションを使用して、SumoLogic で Azure AD のユーザー アカウントを使用してユーザーを認証できるようにする方法を説明します。  
この手順の途中で、base-64 でエンコードされた証明書を SumoLogic テナントにアップロードする必要があります。  
この手順に慣れていない場合は、次を参照してください [バイナリ証明書をテキスト ファイルに変換する方法。](http://youtu.be/PlgrzUZ-Y1o)

###シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **SumoLogic** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、* * [シングル サインオン * * ダイアログ。

    ![Configure single sign-on](./media/active-directory-saas-sumologic-tutorial/IC778552.png "Configure single sign-on")

2.   **どのようなユーザーの SumoLogic へのアクセス** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、] をクリックし、 **次**します。

    ![Configure single sign-on](./media/active-directory-saas-sumologic-tutorial/IC778553.png "Configure single sign-on")

3.   **アプリケーション URL の構成** ] ページで、 **SumoLogic サインイン URL** ] ボックスに、次のパターンを使用して URL を入力"*https://\<tenant-name\>。SumoLogic.com*"、クリックして **次**します。

    ![アプリケーション URL の構成](./media/active-directory-saas-sumologic-tutorial/IC778554.png "Configure aoo URL")

4.   **SumoLogic でのシングル サインオンの構成** ] ページで、証明書をダウンロードする] をクリックして **証明書のダウンロード**, 、お使いのコンピューターに証明書ファイルを保存します。

    ![シングル サインオンの構成](./media/active-directory-saas-sumologic-tutorial/IC778555.png "Configure single sign-on")

5.  別の Web ブラウザーのウィンドウで、SumoLogic 企業サイトに管理者としてログインします。

6.  移動して **管理 \ > セキュリティ**します。

    ![管理](./media/active-directory-saas-sumologic-tutorial/IC778556.png "Manage")

7.  クリックして **SAML**します。

    ![グローバル セキュリティ設定](./media/active-directory-saas-sumologic-tutorial/IC778557.png "Global security settings")

8.   **構成を選択するか、新規に作成** 一覧で、選択 **Azure AD**, 、クリックして **構成**します。

    ![SAML 2.0 の構成](./media/active-directory-saas-sumologic-tutorial/IC778558.png "Configure SAML 2.0")

9.   **SAML 2.0 の構成** ] ダイアログ ボックスで、次の手順を実行します。

    ![SAML 2.0 の構成](./media/active-directory-saas-sumologic-tutorial/IC778559.png "Configure SAML 2.0")

    1.   **構成名** ] ボックスに「 **Azure AD**します。
    2.  選択 **デバッグ モード**します。
    3.  Azure ポータルで、 **SumoLogic でのシングル サインオンの構成** ] ダイアログ ページでコピー、 **発行者 URL** 値に設定して、貼り付けます、 **発行者** ] ボックスに貼り付けます。
    4.  Azure ポータルで、 **SumoLogic でのシングル サインオンの構成** ] ダイアログ ページでコピー、 **認証要求 URL** 値に設定して、貼り付けます、 **認証要求 URL** ] ボックスに貼り付けます。
    5.  作成、 **base-64 でエンコードされた** からダウンロードした証明書ファイル。  

        >[AZURE.TIP] 詳細については、次を参照してください [バイナリ証明書をテキスト ファイルに変換する方法。](http://youtu.be/PlgrzUZ-Y1o)

    6.  Base-64 でエンコードされた証明書をメモ帳で開き、内容をクリップボードにコピーおよび貼り付けて、全体の証明書を **X.509 証明書** ] ボックスに貼り付けます。
    7.  として **電子メール属性**, [ **使用 SAML サブジェクト**します。
    8.  選択 **SP に開始したログイン構成**します。
    9.   **ログイン パス** ] ボックスに「 **Azure**します。
    10. クリックして **保存**します。

10. Azure ポータルで、 **SumoLogic でのシングル サインオンの構成** ] ダイアログ ページで、シングル サインオンの構成情報を選択してクリックして **完了**します。

    ![Configure single sign-on](./media/active-directory-saas-sumologic-tutorial/IC778560.png "Configure single sign-on")

##ユーザー プロビジョニングの構成
  
Azure AD ユーザーが SumoLogic にログインできるようにするには、そのユーザーを SumoLogic にプロビジョニングする必要があります。  
SumoLogic の場合、プロビジョニングは手動で行います。

###ユーザー アカウントをプロビジョニングするには、次の手順を実行します。

1.  ログインして **SumoLogic** テナントです。

2.  移動して **管理 \ > ユーザー**します。

    ![ユーザー](./media/active-directory-saas-sumologic-tutorial/IC778561.png "Users")

3.  クリックして **追加**します。

    ![ユーザー](./media/active-directory-saas-sumologic-tutorial/IC778562.png "Users")

4.   **新しいユーザー** ] ダイアログ ボックスで、次の手順を実行します。

    ![新しいユーザー](./media/active-directory-saas-sumologic-tutorial/IC778563.png "New User")

    1.  プロビジョニングする Azure AD アカウントの関連情報を入力、 **名**, 、**姓** と **電子メール** テキスト ボックスです。
    2.  ロールを選択します。
    3.  として **ステータス**, [ **Active**します。
    4.  クリックして **保存**します。

>[AZURE.NOTE] 他の SumoLogic ユーザー アカウント作成ツールを使用することができますか、AAD ユーザー アカウントをプロビジョニング、SumoLogic から提供されている Api です。

##ユーザーの割り当て
  
構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

###ユーザーを SumoLogic に割り当てるには、次の手順に従います。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.   **SumoLogic** アプリケーション統合ページで、をクリックして **ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-sumologic-tutorial/IC778564.png "Assign users")

3.  テスト ユーザーを選択して [ **割り当てる**, 、順にクリック **はい** して割り当てを確認します。

    ![あり](./media/active-directory-saas-sumologic-tutorial/IC767830.png "Yes")
  
シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。
