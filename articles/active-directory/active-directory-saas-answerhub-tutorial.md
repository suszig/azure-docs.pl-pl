<properties 
    pageTitle="チュートリアル: Azure Active Directory と AnswerHub の統合 | Microsoft Azure" 
    description="Azure Active Directory で AnswerHub を使用して、シングル サインオンや自動プロビジョニングなどを有効にする方法について説明します。" 
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

#チュートリアル: Azure Active Directory と AnswerHub の統合

このチュートリアルでは、Azure と AnswerHub の統合について説明します。  
このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   有効なサブスクリプションでの AnswerHub のシングル サインオン

このチュートリアルを完了すると、AnswerHub に割り当てた Azure AD ユーザーは、AnswerHub 会社サイト (サービス プロバイダーが開始したサインオン) で、アプリケーションにシングル サインオンまたはできるを使用して、 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。

このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  AnswerHub のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-answerhub-tutorial/IC785165.png "Scenario")
##AnswerHub のアプリケーション統合の有効化

このセクションでは、AnswerHub のアプリケーション統合を有効にする方法を説明します。

###AnswerHub のアプリケーション統合を有効にするには、次の手順に従います。

1.  左側のナビゲーション ウィンドウで、Azure 管理ポータルで、クリックして **Active Directory**します。

    ![Active Directory](./media/active-directory-saas-answerhub-tutorial/IC700993.png "Active Directory")

2.   **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3.  ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。

    ![アプリケーション](./media/active-directory-saas-answerhub-tutorial/IC700994.png "Applications")

4.  クリックして **追加** ページの下部にあります。

    ![アプリケーションの追加](./media/active-directory-saas-answerhub-tutorial/IC749321.png "Add application")

5.   **実行する操作** ダイアログ ボックスで、をクリックして **ギャラリーからアプリケーションを追加**します。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-answerhub-tutorial/IC749322.png "Add an application from gallerry")

6.   **検索ボックス**, 、型 **AnswerHub**します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-answerhub-tutorial/IC785166.png "Application Gallery")

7.  結果ウィンドウで [ **AnswerHub**, 、クリックして **完了** アプリケーションを追加します。

    ![AnswerHub](./media/active-directory-saas-answerhub-tutorial/IC785167.png "AnswerHub")
##シングル サインオンの構成

このセクションでは、ユーザーが SAML プロトコルに基づくフェデレーションを使用して、Azure AD でのユーザーのアカウントで AnswerHub に対する認証を行えるようにする方法を説明します。  
この手順の途中で、base-64 でエンコードされた証明書ファイルを作成する必要があります。  
この手順に慣れていない場合は、次を参照してください [バイナリ証明書をテキスト ファイルに変換する方法。](http://youtu.be/PlgrzUZ-Y1o)

###シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **AnswerHub** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、* * [シングル サインオン * * ダイアログ。

    ![Configure single sign-on](./media/active-directory-saas-answerhub-tutorial/IC785168.png "Configure single sign-on")

2.   **どのようなユーザーの AnswerHub に** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、順にクリック **次**します。

    ![シングル サインオンの構成](./media/active-directory-saas-answerhub-tutorial/IC785169.png "Configure single sign-on")

3.   **アプリケーション URL の構成** ] ページの [、 **AnswerHub サインイン URL** ] ボックスに、次のパターンを使用して URL を入力"*https://company.answerhub.com*"、] をクリックし、 **次**します。

    ![Configure App URL](./media/active-directory-saas-answerhub-tutorial/IC785170.png "Configure App URL")

4.   **AnswerHub でのシングル サインオンの構成** ] ページで、証明書をダウンロードする] をクリックして **証明書のダウンロード**, 、お使いのコンピューターにローカルで証明書ファイルを保存します。

    ![シングル サインオンの構成](./media/active-directory-saas-answerhub-tutorial/IC785171.png "Configure single sign-on")

5.  別の Web ブラウザーのウィンドウで、管理者として AnswerHub 企業サイトにログインします。

6.  移動して **管理**します。

7.  クリックして、 **ユーザーおよびグループ** ] タブをクリックします。

8.  左側のナビゲーション ウィンドウで、 **ソーシャル設定** ] をクリックして **SAML のセットアップ**します。

9.  クリックして **IDP 構成** ] タブをクリックします。

10.  **IDP 構成** ] タブで、次の手順を実行します。

    ![SAML Setup](./media/active-directory-saas-answerhub-tutorial/IC785172.png "SAML Setup")

    1.  Azure ポータルで、 **AnswerHub でのシングル サインオンの構成** ] ダイアログ ページでコピー、 **リモート ログイン URL** 値に設定して、貼り付けます、 **IDP ログイン URL** ] ボックスに貼り付けます。
    2.  Azure ポータルで、 **AnswerHub でのシングル サインオンの構成** ] ダイアログ ページでコピー、 **リモート ログアウト URL** 値に設定して、貼り付けます、 **IDP ログアウト URL** ] ボックスに貼り付けます。
    3.  Azure ポータルで、 **AnswerHub でのシングル サインオンの構成** ] ダイアログ ページでコピー、 **名前 Id 形式** 値に設定して、貼り付けます、 **IDP 名前 Id 形式** ] ボックスに貼り付けます。
    4.  クリックして **キーおよび証明書**します。

11. [Keys and Certificates] タブでは、次の手順を実行します。

    ![Keys and Certificates](./media/active-directory-saas-answerhub-tutorial/IC785173.png "Keys and Certificates")

    1.  作成、 **base-64 でエンコードされた** からダウンロードした証明書ファイル。  

        >[AZURE.TIP] 詳細については、次を参照してください [バイナリ証明書をテキスト ファイルに変換する方法。](http://youtu.be/PlgrzUZ-Y1o)

    2.  Base-64 でエンコードされた証明書をメモ帳で開き、という内容がクリップボードにコピーおよびに貼り付け、 **IDP 公開キー (x509 形式)** ] ボックスに貼り付けます。
    3.  クリックして **保存**します。

12.  **IDP 構成** ] タブ、[ **保存**します。

13. Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![シングル サインオンの構成](./media/active-directory-saas-answerhub-tutorial/IC785174.png "Configure single sign-on")
##ユーザー プロビジョニングの構成

Azure AD ユーザーが AnswerHub にログインできるようにするには、そのユーザーを AnswerHub にプロビジョニングする必要があります。  
AnswerHub の場合、プロビジョニングは手動で行います。

###ユーザー プロビジョニングを構成するには、次の手順に従います。

1.  ログインして **AnswerHub** 会社サイトに管理者として。

2.  移動して **管理**します。

3.  クリックして、 **ユーザーおよびグループ** ] タブをクリックします。

4.  左側にある、ナビゲーション ウィンドウでの **ユーザーの管理** ] をクリックして **ユーザーを作成またはインポート**します。

    ![Users & Groups](./media/active-directory-saas-answerhub-tutorial/IC785175.png "Users & Groups")

5.  型、 **電子メール アドレス**, 、**Username** と **パスワード** クリックして、関連するテキスト ボックスにプロビジョニングする有効な Azure Active Directory アカウントの **保存**します。

>[AZURE.NOTE] 他の AnswerHub ユーザー アカウント作成ツールを使用することができますか、AAD ユーザー アカウントをプロビジョニング、AnswerHub から提供されている Api です。

##ユーザーの割り当て

構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

###ユーザーを AnswerHub に割り当てるには、次の手順に従います。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  * * AnswerHub * * アプリケーション統合ページで、をクリックして **ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-answerhub-tutorial/IC785176.png "Assign users")

3.  テスト ユーザーを選択して [ **割り当てる**, 、順にクリック **はい** して割り当てを確認します。

    ![あり](./media/active-directory-saas-answerhub-tutorial/IC767830.png "Yes")

シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。


