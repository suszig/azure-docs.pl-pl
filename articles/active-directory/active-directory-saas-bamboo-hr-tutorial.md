<properties 
    pageTitle="チュートリアル: Azure Active Directory と BambooHR の統合 | Microsoft Azure" 
    description="Azure Active Directory で BambooHR を使用して、シングル サインオンや自動プロビジョニングなどを有効にする方法について説明します。" 
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

#チュートリアル: Azure Active Directory と Bamboo HR の統合

このチュートリアルでは、Azure と Bamboo HR の統合について説明します。  
このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   Bamboo HR でのシングル サインオンが有効なサブスクリプション

このチュートリアルを完了すると、BambooHR に割り当てた Azure AD ユーザーは、BambooHR 会社サイト (サービス プロバイダーが開始したサインオン) で、アプリケーションにシングル サインオンまたはできるを使用して、 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。

このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  Bamboo HR のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-bamboo-hr-tutorial/IC796685.png "Scenario")
##Bamboo HR のアプリケーション統合の有効化

このセクションでは、Bamboo HR のアプリケーション統合を有効にする方法について説明します。

###Bamboo HR のアプリケーション統合を有効にするには、次の手順に従います。

1.  左側のナビゲーション ウィンドウで、Azure 管理ポータルで、クリックして **Active Directory**します。

    ![Active Directory](./media/active-directory-saas-bamboo-hr-tutorial/IC700993.png "Active Directory")

2.   **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3.  ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。

    ![アプリケーション](./media/active-directory-saas-bamboo-hr-tutorial/IC700994.png "Applications")

4.  クリックして **追加** ページの下部にあります。

    ![アプリケーションの追加](./media/active-directory-saas-bamboo-hr-tutorial/IC749321.png "Add application")

5.   **実行する操作** ダイアログ ボックスで、をクリックして **ギャラリーからアプリケーションを追加**します。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-bamboo-hr-tutorial/IC749322.png "Add an application from gallerry")

6.   **検索ボックス**, 、型 **BambooHR**します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-bamboo-hr-tutorial/IC796686.png "Application gallery")

7.  結果ウィンドウで [ **BambooHR**, 、クリックして **完了** アプリケーションを追加します。

    ![BambooHR](./media/active-directory-saas-bamboo-hr-tutorial/IC796687.png "BambooHR")
##シングル サインオンの構成

このセクションでは、ユーザーが SAML プロトコルに基づくフェデレーションを使用して、Azure AD でのユーザーのアカウントで BambooHR に対する認証を行えるようにする方法を説明します。  
この手順の途中で、base-64 でエンコードされた証明書ファイルを作成する必要があります。  
この手順に慣れていない場合は、次を参照してください。 [バイナリ証明書をテキスト ファイルに変換する方法](http://youtu.be/PlgrzUZ-Y1o)します。

###シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **BambooHR** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、* * [シングル サインオン * * ダイアログ。

    ![シナリオ](./media/active-directory-saas-bamboo-hr-tutorial/IC796685.png "Scenario")

2.   **どのようなユーザーの BambooHR へのアクセス** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、] をクリックし、 **次**します。

    ![Configure single sign-on](./media/active-directory-saas-bamboo-hr-tutorial/IC796688.png "Configure single sign-on")

3.   **アプリケーション URL の構成** ] ページで、 **BambooHR サインオン URL** ] ボックスに、型に使用する URL、ユーザーが BambooHR アプリケーションにサインオン (例:: https://company.bamboohr.com)、順にクリック **次**します。

    ![アプリケーション URL の構成](./media/active-directory-saas-bamboo-hr-tutorial/IC796689.png "Configure app URL")

4.   **BambooHR でのシングル サインオンの構成** ] ページで [ **証明書のダウンロード**, 、お使いのコンピューターに証明書ファイルを保存します。

    ![Configure single sign-on](./media/active-directory-saas-bamboo-hr-tutorial/IC796690.png "Configure single sign-on")

5.  別の Web ブラウザーのウィンドウで、管理者として BambooHR 企業サイトにログインします。

6.  このホーム ページで、次の手順を実行します。

    ![シングル サインオン](./media/active-directory-saas-bamboo-hr-tutorial/IC796691.png "Single Sign-On")

    1.  クリックして **アプリ**します。
    2.  左側の [アプリケーション] メニューでクリックして **でのシングル サインオン**します。
    3.  クリックして **SAML シングル サインオン**します。

7.   **SAML シングル サインオン** セクションで、次の手順に従います。

    ![SAML シングル サインオン](./media/active-directory-saas-bamboo-hr-tutorial/IC796692.png "SAML Single Sign-On")

    1.  Azure ポータルで、 **BambooHR でのシングル サインオンの構成** ] ダイアログ ページでコピー、 **シングル サインオン サービス URL** 値に設定して、貼り付けます、* * SSO ログイン URL * *] ボックスに貼り付けます。
    2.  作成、 **base-64 でエンコードされた** からダウンロードした証明書ファイル。  

        >[AZURE.TIP] 詳細については、次を参照してください [バイナリ証明書をテキスト ファイルに変換する方法。](http://youtu.be/PlgrzUZ-Y1o)

    3.  Base-64 でエンコードされた証明書をメモ帳で開き、という内容がクリップボードにコピーおよびに貼り付け、 **X.509 証明書** ] テキスト ボックス
    4.  クリックして **保存**します。

8.  Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![Configure single sign-on](./media/active-directory-saas-bamboo-hr-tutorial/IC796693.png "Configure single sign-on")
##ユーザー プロビジョニングの構成

Azure AD ユーザーが BambooHR にログインできるようにするには、そのユーザーを BambooHR にプロビジョニングする必要があります。  
BambooHR の場合、プロビジョニングは手動で行います。

###ユーザー アカウントをプロビジョニングするには、次の手順を実行します。

1.  ログインして **BambooHR** サイト管理者として。

2.  上部にあるツールバーで、クリックして **設定**します。

    ![設定](./media/active-directory-saas-bamboo-hr-tutorial/IC796694.png "Setting")

3.  クリックして **概要**します。

4.  左側のナビゲーション ウィンドウで [ **セキュリティ \ > ユーザー**します。

5.  関連するテキスト ボックスに、プロビジョニングする有効な AAD アカウントのユーザー名、パスワード、および電子メール アドレスを入力します。

6.  クリックして **保存**します。

>[AZURE.NOTE] 他の BambooHR ユーザー アカウント作成ツールを使用することができますか、AAD ユーザー アカウントをプロビジョニング、BambooHR から提供されている Api です。

##ユーザーの割り当て

構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

###ユーザーを BambooHR に割り当てるには、次の手順に従います。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  * * BambooHR * * アプリケーション統合ページで、をクリックして **ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-bamboo-hr-tutorial/IC796695.png "Assign users")

3.  テスト ユーザーを選択して [ **割り当てる**, 、順にクリック **はい** して割り当てを確認します。

    ![あり](./media/active-directory-saas-bamboo-hr-tutorial/IC767830.png "Yes")

シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。

