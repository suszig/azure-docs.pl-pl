<properties 
    pageTitle="チュートリアル: Azure Active Directory と ClickTime の統合 | Microsoft Azure" 
    description="Azure Active Directory で ClickTime を使用して、シングル サインオンや自動プロビジョニングなどを有効にする方法について説明します。" 
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

#チュートリアル: Azure Active Directory と ClickTime の統合

このチュートリアルでは、Azure と ClickTime の統合について説明します。  
このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   ClickTime テナント

このチュートリアルを完了すると、ClickTime に割り当てた Azure AD ユーザーは、ClickTime 会社サイト (サービス プロバイダーが開始したサインオン) で、アプリケーションにシングル サインオンまたはできるを使用して、 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。

このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  ClickTime のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-clicktime-tutorial/IC777274.png "Scenario")
##ClickTime のアプリケーション統合の有効化

このセクションでは、ClickTime のアプリケーション統合を有効にする方法を説明します。

###ClickTime のアプリケーション統合を有効にするには、次の手順を実行します。

1.  左側のナビゲーション ウィンドウで、Azure 管理ポータルで、クリックして **Active Directory**します。

    ![Active Directory](./media/active-directory-saas-clicktime-tutorial/IC700993.png "Active Directory")

2.   **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3.  ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。

    ![アプリケーション](./media/active-directory-saas-clicktime-tutorial/IC700994.png "Applications")

4.  クリックして **追加** ページの下部にあります。

    ![アプリケーションの追加](./media/active-directory-saas-clicktime-tutorial/IC749321.png "Add application")

5.   **実行する操作** ダイアログ ボックスで、をクリックして **ギャラリーからアプリケーションを追加**します。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-clicktime-tutorial/IC749322.png "Add an application from gallerry")

6.   **検索ボックス**, 、型 **ClickTime**します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-clicktime-tutorial/IC777275.png "Application gallery")

7.  結果ウィンドウで [ **ClickTime**, 、クリックして **完了** アプリケーションを追加します。

    ![ClickTime](./media/active-directory-saas-clicktime-tutorial/IC777276.png "ClickTime")
##シングル サインオンの構成

このセクションでは、ユーザーが SAML プロトコルに基づくフェデレーションを使用して、Azure AD でのユーザーのアカウントで ClickTime に対する認証を行うことができるようにする方法を説明します。  
この手順の途中で、base-64 でエンコードされた証明書を ClickTime テナントにアップロードする必要があります。  
この手順に慣れていない場合は、次を参照してください。 [バイナリ証明書をテキスト ファイルに変換する方法](http://youtu.be/PlgrzUZ-Y1o)します。

>[AZURE.IMPORTANT] ClickTime テナントの [シングル サインオンを構成するためには、まずこの機能を有効にする ClickTime テクニカル サポートに連絡する必要があります。

###シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **ClickTime** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、* * [シングル サインオン * * ダイアログ。

    ![シングル サインオンの有効化](./media/active-directory-saas-clicktime-tutorial/IC777277.png "Enable single sign-on")

2.   **どのようなユーザーが ClickTime へのサインオン** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、] をクリックし、 **次**します。

    ![Configure single sign-on](./media/active-directory-saas-clicktime-tutorial/IC777278.png "Configure single sign-on")

3.   **ClickTime でのシングル サインオンの構成** ] ページで、証明書をダウンロードする] をクリックして **証明書のダウンロード**, 、お使いのコンピューターに証明書ファイルを保存します。

    ![シングル サインオンの構成](./media/active-directory-saas-clicktime-tutorial/IC777279.png "Configure single sign-on")

4.  別の Web ブラウザー ウィンドウで、ClickTime 企業サイトに管理者としてログインします。

5.  上部にあるツールバーで、クリックして **設定**, 、] をクリックし、 **セキュリティ設定**します。

6.   **シングル サインオン設定** 構成セクションで、次の手順に従います。

    ![セキュリティ設定](./media/active-directory-saas-clicktime-tutorial/IC777280.png "Security Settings")

    1.  選択 **許可** シングル サインオン (SSO) を使用してサインイン **OneLogin**します。
    2.  Azure ポータルで、 **ClickTime でのシングル サインオンの構成** ] ダイアログ ページでコピー、 **シングル サインオン サービス URL** 値に設定して、貼り付けます、 **Id プロバイダー エンドポイント** ] ボックスに貼り付けます。
    3.  作成、 **base-64 でエンコードされた** からダウンロードした証明書ファイル。  

        >[AZURE.TIP] 詳細については、次を参照してください [バイナリ証明書をテキスト ファイルに変換する方法。](http://youtu.be/PlgrzUZ-Y1o)

    4.  Base-64 でエンコードされた証明書を開く **メモ帳**, 、コンテンツをコピーして貼り付けます、 **X.509 証明書** ] ボックスに貼り付けます。
    5.  クリックして **保存**します。

7.  Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![シングル サインオンの構成](./media/active-directory-saas-clicktime-tutorial/IC777281.png "Configure single sign-on")
##ユーザー プロビジョニングの構成

Azure AD ユーザーが ClickTime にログインできるようにするには、そのユーザーを ClickTime にプロビジョニングする必要があります。  
ClickTime の場合、プロビジョニングは手動で行います。

###ユーザー アカウントをプロビジョニングするには、次の手順を実行します。

1.  ログインして **ClickTime** テナントです。

2.  上部にあるツールバーで、クリックして **会社**, 、クリックして **人**します。

    ![People](./media/active-directory-saas-clicktime-tutorial/IC777282.png "People")

3.  クリックして **人追加**します。

    ![Add Person](./media/active-directory-saas-clicktime-tutorial/IC777283.png "Add Person")

4.  [New Person] セクションで、次の手順を実行します。

    ![People](./media/active-directory-saas-clicktime-tutorial/IC777284.png "People")

    1.   **電子メール アドレス** ] ボックスに、Azure AD アカウントの電子メール アドレスを入力します。
    2.   **フル_ネーム** ] ボックスに、Azure AD アカウントの名前を入力します。  

        >[AZURE.NOTE] をする場合は、新しい person オブジェクトの追加のプロパティを設定できます。

    3.  クリックして **保存**します。

>[AZURE.NOTE] 他の ClickTime ユーザー アカウント作成ツールを使用するや、AAD ユーザー アカウントをプロビジョニング、ClickTime 提供の Api です。

##ユーザーの割り当て

構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

###ユーザーを ClickTime に割り当てるには、次の手順を実行します。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  * * ClickTime * * アプリケーション統合ページで、をクリックして **ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-clicktime-tutorial/IC777285.png "Assign users")

3.  テスト ユーザーを選択して [ **割り当てる**, 、順にクリック **はい** して割り当てを確認します。

    ![あり](./media/active-directory-saas-clicktime-tutorial/IC767830.png "Yes")

シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。


