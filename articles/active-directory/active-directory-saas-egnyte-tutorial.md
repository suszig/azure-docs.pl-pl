<properties 
    pageTitle="チュートリアル: Azure Active Directory と Egnyte の統合 | Microsoft Azure" 
    description="Azure Active Directory で Egnyte を使用して、シングル サインオンや自動プロビジョニングなどを有効にする方法について説明します。" 
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

#チュートリアル: Azure Active Directory と Egnyte の統合
  
このチュートリアルでは、Azure と Egnyte の統合について説明します。  
このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   Egnyte でのシングル サインオンが有効なサブスクリプション
  
このチュートリアルを完了すると、Egnyte に割り当てた Azure AD ユーザーは、Egnyte 会社サイト (サービス プロバイダーが開始したサインオン) で、アプリケーションにシングル サインオンまたはできるを使用して、 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)
  
このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  Egnyte のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-egnyte-tutorial/IC787812.png "Scenario")
##Egnyte のアプリケーション統合の有効化
  
このセクションでは、Egnyte のアプリケーション統合を有効にする方法について説明します。

###Egnyte のアプリケーション統合を有効にするには、次の手順を実行します。

1.  左側のナビゲーション ウィンドウで、Azure 管理ポータルで、クリックして **Active Directory**します。

    ![Active Directory](./media/active-directory-saas-egnyte-tutorial/IC700993.png "Active Directory")

2.   **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3.  ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。

    ![アプリケーション](./media/active-directory-saas-egnyte-tutorial/IC700994.png "Applications")

4.  クリックして **追加** ページの下部にあります。

    ![アプリケーションの追加](./media/active-directory-saas-egnyte-tutorial/IC749321.png "Add application")

5.   **実行する操作** ダイアログ ボックスで、をクリックして **ギャラリーからアプリケーションを追加**します。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-egnyte-tutorial/IC749322.png "Add an application from gallerry")

6.   **検索ボックス**, 、型 **egnyte**します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-egnyte-tutorial/IC787813.png "Application Gallery")

7.  結果ウィンドウで [ **Egnyte**, 、クリックして **完了** アプリケーションを追加します。

    ![Egnyte](./media/active-directory-saas-egnyte-tutorial/IC787814.png "Egnyte")
##シングル サインオンの構成
  
このセクションでは、ユーザーが SAML プロトコルに基づくフェデレーションを使用して、Azure AD でのユーザーのアカウントで Egnyte に対する認証を行うことができるようにする方法を説明します。  
この手順の途中で、base-64 でエンコードされた証明書ファイルを作成する必要があります。  
この手順に慣れていない場合は、次を参照してください。 [バイナリ証明書をテキスト ファイルに変換する方法](http://youtu.be/PlgrzUZ-Y1o)します。

###シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **Egnyte** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、* * [シングル サインオン * * ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-egnyte-tutorial/IC787815.png "Configure Single Sign-On")

2.   **どのようなユーザーが Egnyte へのサインオン** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、順にクリック **次**します。

    ![Configure Single Sign-On](./media/active-directory-saas-egnyte-tutorial/IC787816.png "Configure Single Sign-On")

3.   **アプリケーション URL の構成** ] ページの [、 **Egnyte サインイン URL** ] ボックスに、次のパターンを使用して URL を入力"*https://company.egnyte.com*"、] をクリックし、 **次**します。

    ![Configure App URL](./media/active-directory-saas-egnyte-tutorial/IC787817.png "Configure App URL")

4.   **Egnyte でのシングル サインオンの構成** ] ページで [ **証明書のダウンロード**, 、お使いのコンピューターに証明書ファイルを保存します。

    ![Configure Single Sign-On](./media/active-directory-saas-egnyte-tutorial/IC787818.png "Configure Single Sign-On")

5.  別の Web ブラウザー ウィンドウで、Egnyte 企業サイトに管理者としてログインします。

6.  クリックして **設定**します。

    ![Settings](./media/active-directory-saas-egnyte-tutorial/IC787819.png "Settings")

7.  メニュー、[ **設定**します。

    ![Settings](./media/active-directory-saas-egnyte-tutorial/IC787820.png "Settings")

8.  クリックして、 **構成** タブをクリックし、をクリックし、 **セキュリティ**します。

    ![セキュリティ](./media/active-directory-saas-egnyte-tutorial/IC787821.png "Security")

9.   **シングル サインオン認証** セクションで、次の手順に従います。

    ![シングル サインオン認証](./media/active-directory-saas-egnyte-tutorial/IC787822.png "Single Sign On Authentication")

    1.  として **シングル サインオン認証**, [ **SAML 2.0**します。
    2.  として **Id プロバイダー**, [ **AzureAD**します。
    3.  Azure ポータルで、 **Egnyte でのシングル サインオンの構成** ] ダイアログ ページでコピー、 **リモート ログイン URL** 値に設定して、貼り付けます、* * Id プロバイダー ログイン URL * *] ボックスに貼り付けます。
    4.  Azure ポータルで、 **Egnyte でのシングル サインオンの構成** ] ダイアログ ページでコピー、 **エンティティ ID** 値に設定して、貼り付けます、 **Id プロバイダー エンティティの ID** ] ボックスに貼り付けます。
    5.  作成、 **base-64 でエンコードされた** からダウンロードした証明書ファイル。  

        >[AZURE.TIP]詳細については、次を参照してください [バイナリ証明書をテキスト ファイルに変換する方法。](http://youtu.be/PlgrzUZ-Y1o)

    6.  Base-64 でエンコードされた証明書をメモ帳で開き、という内容がクリップボードにコピーおよびに貼り付け、 **Id プロバイダー証明書** ] ボックスに貼り付けます。
    7.  として **既定のユーザー マッピング**, [ **電子メール アドレス**です。
    8.  として **ドメイン固有の発行者の値を使用して**, [ **無効になっている**します。
    9.  クリックして **保存**します。

10. Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-egnyte-tutorial/IC787823.png "Configure Single Sign-On")
##ユーザー プロビジョニングの構成
  
Azure AD ユーザーが Egnyte にログインできるようにするには、そのユーザーを Egnyte にプロビジョニングする必要があります。  
Egnyte の場合、プロビジョニングは手動で行います。

###ユーザー アカウントをプロビジョニングするには、次の手順を実行します。

1.  ログインして **Egnyte** Egnyte 会社サイトに管理者として。

2.  移動して **設定 \ > ユーザーおよびグループ**します。

3.  クリックして **Add New User**, 、しを追加するユーザーの種類を選択します。

    ![ユーザー](./media/active-directory-saas-egnyte-tutorial/IC787824.png "Users")

4.   **新しい標準ユーザー** セクションで、次の手順に従います。

    ![新しい標準ユーザー](./media/active-directory-saas-egnyte-tutorial/IC787825.png "New Standard User")

    1.  型、 **電子メール**, 、**Username** など、プロビジョニングする有効な Azure Active Directory アカウントの情報です。
    2.  クリックして **保存**します。

    >[AZURE.NOTE] Azure Active Directory アカウント所有者には、通知電子メールが送信されます。

>[AZURE.NOTE] 他の Egnyte ユーザー アカウント作成ツールを使用することができますか、AAD ユーザー アカウントをプロビジョニング、Egnyte から提供されている Api です。

##ユーザーの割り当て
  
構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

###ユーザーを Egnyte に割り当てるには、次の手順を実行します。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  * * Egnyte * * アプリケーション統合ページで、をクリックして **ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-egnyte-tutorial/IC787826.png "Assign Users")

3.  テスト ユーザーを選択して [ **割り当てる**, 、順にクリック **はい** して割り当てを確認します。

    ![あり](./media/active-directory-saas-egnyte-tutorial/IC767830.png "Yes")
  
シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。

