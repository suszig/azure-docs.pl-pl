<properties 
    pageTitle="チュートリアル: Azure Active Directory と Jitbit Helpdesk の統合 | Microsoft Azure" 
    description="Azure Active Directory で Jitbit Helpdesk を使用して、シングル サインオンや自動プロビジョニングなどを有効にする方法について説明します。" 
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

#チュートリアル: Azure Active Directory と Jitbit Helpdesk の統合
  
このチュートリアルでは、Azure と Jitbit Helpdesk の統合について説明します。  
このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   Jitbit Helpdesk テナント
  
このチュートリアルを完了すると、Jitbit Helpdesk に割り当てた Azure AD ユーザーは、Jitbit Helpdesk 会社サイト (サービス プロバイダーが開始したサインオン) で、アプリケーションにシングル サインオンまたはできるを使用して、 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。
  
このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  Jitbit Helpdesk のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777676.png "Scenario")
##Jitbit Helpdesk のアプリケーション統合の有効化
  
このセクションでは、Jitbit Helpdesk のアプリケーション統合を有効にする方法を説明します。

###Jitbit Helpdesk のアプリケーション統合を有効にするには、次の手順を実行します。

1.  左側のナビゲーション ウィンドウで、Azure 管理ポータルで、クリックして **Active Directory**します。

    ![Active Directory](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC700993.png "Active Directory")

2.   **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3.  ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。

    ![アプリケーション](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC700994.png "Applications")

4.  クリックして **追加** ページの下部にあります。

    ![アプリケーションの追加](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC749321.png "Add application")

5.   **実行する操作** ダイアログ ボックスで、をクリックして **ギャラリーからアプリケーションを追加**します。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC749322.png "Add an application from gallerry")

6.   **検索ボックス**, 、型 **Jitbit Helpdesk**します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777677.png "Application Gallery")

7.  結果ウィンドウで [ **Jitbit Helpdesk**, 、クリックして **完了** アプリケーションを追加します。

    ![JitBit](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC781008.png "JitBit")
##シングル サインオンの構成
  
このセクションでは、SAML プロトコルに基づくフェデレーションを使用して Azure AD のアカウントを Jitbit Helpdesk に認証するユーザーを有効にする方法を説明します。 .  
この手順の途中で、base-64 でエンコードされた証明書ファイルを作成する必要があります。  
この手順に慣れていない場合は、次を参照してください。 [バイナリ証明書をテキスト ファイルに変換する方法](http://youtu.be/PlgrzUZ-Y1o)します。

>[AZURE.IMPORTANT] Jitbit Helpdesk テナントの [シングル サインオンを構成するためには、まずこの機能を有効にする Jitbit Helpdesk テクニカル サポートに連絡する必要があります。

###シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **Jitbit Helpdesk** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、* * [シングル サインオン * * ダイアログ。

    ![Configure single sign-on](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777678.png "Configure single sign-on")

2.   **どのようなユーザーを Jitbit Helpdesk へのサインオン** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、] をクリックし、 **次**します。

    ![シングル サインオンの構成](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777679.png "Configure single sign-on")

3.   **アプリケーション URL の構成** ] ページで、 **Jitbit Helpdesk サインイン URL** ] ボックスに、次のパターンを使用して URL を入力"*https://\<tenant-name\>。Jitbit.com*"、クリックして **次**します。

    ![アプリケーション URL の構成](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777528.png "Configure app URL")

4.   **Jitbit Helpdesk でのシングル サインオンの構成** ] ページで、証明書をダウンロードする] をクリックして **証明書のダウンロード**, 、としては、証明書ファイルをローカルに保存および **c:\\Jitbit Helpdesk.cer**します。

    ![シングル サインオンの構成](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777680.png "Configure single sign-on")

5.  別の Web ブラウザーのウィンドウで、Jitbit Helpdesk の企業サイトに管理者としてログインします。

6.  上部にあるツールバーで、クリックして **管理**します。

    ![管理](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777681.png "Administration")

7.  クリックして **全般設定**します。

    ![ユーザー、会社、およびアクセス許可](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777682.png "Users, companies and permissions")

8.   **認証設定** 構成セクションで、次の手順に従います。

    ![認証設定](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777683.png "Authentication settings")

    1.  選択 **を有効にする SAML 2.0 のシングル サインオン** シングル サインオン (SSO) を使用してサインイン **OneLogin**します。
    2.  Azure ポータルで、 **Jitbit Helpdesk でのシングル サインオンの構成** ] ダイアログ ページでコピー、 **サービス プロバイダー (SP) が開始したエンドポイント** 値に設定して、貼り付けます、 **エンドポイントの URL** ] ボックスに貼り付けます。
    3.  作成、 **base-64 でエンコードされた** からダウンロードした証明書ファイル。
        
        >[AZURE.TIP]詳細については、次を参照してください [バイナリ証明書をテキスト ファイルに変換する方法。](http://youtu.be/PlgrzUZ-Y1o)

    4.  Base 64 でエンコードされた証明書を開き、という内容がクリップボードにコピーおよびに貼り付け、 **X.509 証明書** ] テキスト ボックス
    5.  クリックして **変更を保存**します。

9.  Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![Configure single sign-on](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777684.png "Configure single sign-on")
##ユーザー プロビジョニングの構成
  
Azure AD ユーザーが Jitbit Helpdesk にログインできるようにするには、そのユーザーを Jitbit Helpdesk にプロビジョニングする必要があります。  
Jitbit Helpdesk の場合、プロビジョニングは手動で行います。

###ユーザー アカウントをプロビジョニングするには、次の手順を実行します。

1.  ログインして **Jitbit Helpdesk** テナントです。

2.  上部にあるメニュー [ **管理**します。

    ![管理](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777681.png "Administration")

3.  クリックして **ユーザー、会社、およびアクセス許可**します。

    ![ユーザー、会社、およびアクセス許可](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777682.png "Users, companies and permissions")

4.  クリックして **ユーザーの追加**します。

    ![ユーザーの追加](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777685.png "Add user")

5.  [作成] セクションには、次のテキスト ボックスに、プロビジョニングする Azure AD アカウントのデータを入力: **Username**, 、**電子メール**, 、**名**, 、**姓**

    ![作成](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777686.png "Create")

6.  クリックして **作成**します。

>[AZURE.NOTE] 他の Jitbit Helpdesk ユーザー アカウント作成ツールを使用するまたは AAD ユーザー アカウントをプロビジョニング、Jitbit Helpdesk 提供の Api です。

##ユーザーの割り当て
  
構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

###ユーザーを Jitbit Helpdesk に割り当てるには、次の手順を実行します。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  * * Jitbit Helpdesk * * アプリケーション統合ページで、をクリックして **ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777687.png "Assign users")

3.  テスト ユーザーを選択して [ **割り当てる**, 、順にクリック **はい** して割り当てを確認します。

    ![あり](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC767830.png "Yes")
  
シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。
