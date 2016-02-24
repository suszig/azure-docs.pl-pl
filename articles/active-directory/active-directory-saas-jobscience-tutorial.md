<properties 
    pageTitle="チュートリアル: Azure Active Directory と Jobscience の統合 | Microsoft Azure" 
    description="Azure Active Directory で Jobscience を使用して、シングル サインオンや自動プロビジョニングなどを有効にする方法について説明します。" 
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

#チュートリアル: Azure Active Directory と Jobscience の統合
  
このチュートリアルの目的は、Azure と Jobscience の統合を示すことです。  
このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   Jobscience でのシングル サインオンが有効なサブスクリプション
  
このチュートリアルを完了すると、Jobscience に割り当てた Azure AD ユーザーは、Jobscience の会社サイト (サービス プロバイダーが開始したサインオン) で、アプリケーションにシングル サインオンまたはできるを使用して、 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。
  
このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  Jobscience のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-jobscience-tutorial/IC784341.png "Scenario")
##Jobscience のアプリケーション統合の有効化
  
このセクションでは、Jobscience のアプリケーション統合を有効にする方法を説明します。

###Jobscience のアプリケーション統合を有効にするには、次の手順に従います。

1.  左側のナビゲーション ウィンドウで、Azure 管理ポータルで、クリックして **Active Directory**します。

    ![Active Directory](./media/active-directory-saas-jobscience-tutorial/IC700993.png "Active Directory")

2.   **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3.  ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。

    ![アプリケーション](./media/active-directory-saas-jobscience-tutorial/IC700994.png "Applications")

4.  クリックして **追加** ページの下部にあります。

    ![アプリケーションの追加](./media/active-directory-saas-jobscience-tutorial/IC749321.png "Add application")

5.   **実行する操作** ダイアログ ボックスで、をクリックして **ギャラリーからアプリケーションを追加**します。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-jobscience-tutorial/IC749322.png "Add an application from gallerry")

6.   **検索ボックス**, 、型 **jobscience**します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-jobscience-tutorial/IC784342.png "Application Gallery")

7.  結果ウィンドウで [ **Jobscience**, 、クリックして **完了** アプリケーションを追加します。

    ![Jobscience](./media/active-directory-saas-jobscience-tutorial/IC784357.png "Jobscience")
##シングル サインオンの構成
  
このセクションでは、ユーザーが SAML プロトコルに基づくフェデレーションを使用して、Azure AD でのユーザーのアカウントで Jobscience に対する認証を行えるようにする方法を説明します。  
Jobscience のシングル サインオンを構成するには、証明書から拇印の値を取得する必要があります。  
この手順に慣れていない場合は、次を参照してください。 [証明書の拇印の値を取得する方法](http://youtu.be/YKQF266SAxI)します。

###シングル サインオンを構成するには、次の手順に従います。

1.  Jobscience の企業サイトに管理者としてログインします。

2.  移動して **セットアップ**します。

    ![セットアップ](./media/active-directory-saas-jobscience-tutorial/IC784358.png "Setup")

3.  左側のナビゲーション ウィンドウで、 **管理** ] をクリックして **ドメイン管理** に関連するセクションを展開し、クリックして **My Domain** を開くには、 **My Domain** ページです。 

    ![マイ ドメイン](./media/active-directory-saas-jobscience-tutorial/IC767825.png "My Domain")

4.  ドメインが正しくセットアップされたことを確認するに内にあることを確認します"**ステップ 4 は、ユーザーに対して展開された**"を確認し、"**マイ ドメイン設定**"です。

    ![ユーザーにデプロイされたドメイン](./media/active-directory-saas-jobscience-tutorial/IC784377.png "Doman Deployed to User")

5.  別の Web ブラウザー ウィンドウで、Azure ポータルにログインします。

6.   **Jobscience** アプリケーション統合ページでは、をクリックして **でのシングル サインオンを構成する** を開くには、* * [シングル サインオン * *] ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-jobscience-tutorial/IC784360.png "Configure Single Sign-On")

7.   **どのようなユーザーが Jobscience へのサインオン** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、順にクリック **次**します。

    ![Configure Single Sign-On](./media/active-directory-saas-jobscience-tutorial/IC784361.png "Configure Single Sign-On")

8.   **アプリケーション URL の構成** ] ページの [、 **Jobscience サインイン URL** ] ボックスに、次のパターンを使用して URL を入力"*http://company.my.salesforce.com*"、] をクリックし、 **次**します。

    ![アプリケーション URL の構成](./media/active-directory-saas-jobscience-tutorial/IC784362.png "Configure App URL")

9.   **Jobscience でのシングル サインオンの構成** ] ページで、証明書をダウンロードする] をクリックして **証明書のダウンロード**, 、お使いのコンピューターにローカルで証明書ファイルを保存します。

    ![Configure Single Sign-On](./media/active-directory-saas-jobscience-tutorial/IC784363.png "Configure Single Sign-On")

10. Jobscience の会社サイトに次のようにクリックします。 **セキュリティ制御**, 、] をクリックし、 **シングル サインオン設定**します。

    ![セキュリティ コントロール](./media/active-directory-saas-jobscience-tutorial/IC784364.png "Security Controls")

11.  **シングル サインオン設定** セクションで、次の手順に従います。

    ![シングル サインオンの設定](./media/active-directory-saas-jobscience-tutorial/IC781026.png "Single Sign-On Settings")

    1.  選択 **SAML の有効化**します。
    2.  クリックして **新しい**します。

12.  **SAML シングル サインオン設定の編集** ] ダイアログ ボックスで、次の手順を実行します。

    ![SAML シングル サインオンの設定](./media/active-directory-saas-jobscience-tutorial/IC784365.png "SAML Single Sign-On Setting")

    1.   **名前** ] ボックスに、構成の名前を入力します。
    2.  Azure ポータルで、 **Jobscience でのシングル サインオンの構成** ] ダイアログ ページでコピー、 **発行者 URL** 値に設定して、貼り付けます、 **発行者** ] テキスト ボックス
    3.   **エンティティ Id** ] ボックスに「 **https://salesforce-jobscience.com**
    4.  クリックして **参照** 、Azure AD 証明書をアップロードします。
    5.  として **SAML Identity Type**, [ **アサーションには、ユーザー オブジェクトからのフェデレーション ID が含まれています。**します。
    6.  として **SAML Identity Location**, [ **Id は Subject ステートメントの NameIdentfier 要素**します。
    7.  Azure ポータルで、 **Jobscience でのシングル サインオンの構成** ] ダイアログ ページで、コピー、 **リモート ログイン URL** 値に設定して、貼り付けます、 **Identity Provider Login URL** ] ボックスに貼り付けます
    8.  Azure ポータルで、 **Jobscience でのシングル サインオンの構成** ] ダイアログ ページで、コピー、 **リモート ログアウト URL** 値に設定して、貼り付けます、 **Identity Provider Logout URL** ] ボックスに貼り付けます
    9.  クリックして **保存**します。

13. 左側のナビゲーション ウィンドウで、 **管理** ] をクリックして **ドメイン管理** に関連するセクションを展開し、クリックして **My Domain** を開くには、 **My Domain** ページです。 

    ![マイ ドメイン](./media/active-directory-saas-jobscience-tutorial/IC767825.png "My Domain")

14.  **My Domain** ] ページで、 **Login Page Branding** ] をクリックして **編集**します。

    ![ログイン ページのブランド](./media/active-directory-saas-jobscience-tutorial/IC767826.png "Login Page Branding")

15.  **Login Page Branding** ] ページで、 **認証サービス** セクションの名前、 **SAML SSO 設定** が表示されます。 クリックして選択し、 **保存**します。

    ![ログイン ページのブランド](./media/active-directory-saas-jobscience-tutorial/IC784366.png "Login Page Branding")

16. Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-jobscience-tutorial/IC784367.png "Configure Single Sign-On")
  
SP を取得するには、ログイン URL] をクリックでシングル サインオンを開始します。、 **Single Sign On settings** で、 **セキュリティ制御** メニュー セクションです。

![セキュリティ コントロール](./media/active-directory-saas-jobscience-tutorial/IC784368.png "Security Controls")
  
前の手順で作成した SSO プロファイルをクリックします。  
このページの URL で、会社のシングル サインオンを表示する (例: *https://companyname.my.salesforce.com?so=companyid*)。
##ユーザー プロビジョニングの構成
  
Azure AD ユーザーが Jobscience にログインできるようにするには、そのユーザーを Jobscience にプロビジョニングする必要があります。  
Jobscience の場合、プロビジョニングは手動で行います。

###ユーザー プロビジョニングを構成するには、次の手順に従います。

1.  ログインして **Jobscience** 会社サイトに管理者として。

2.  [セットアップ] に移動します。

    ![セットアップ](./media/active-directory-saas-jobscience-tutorial/IC784358.png "Setup")

3.  移動して **ユーザーの管理 \ > ユーザー**します。

    ![ユーザー](./media/active-directory-saas-jobscience-tutorial/IC784369.png "Users")

4.  クリックして **新しいユーザー**します。

    ![すべてのユーザー](./media/active-directory-saas-jobscience-tutorial/IC784370.png "All Users")

5.   **ユーザーの編集** ] ダイアログ ボックスで、次の手順を実行します。

    ![ユーザーの編集](./media/active-directory-saas-jobscience-tutorial/IC784371.png "User Edit")

    1.  名、姓、名、エイリアス、メール、プロビジョニングする Azure AD ユーザーのユーザー名とニックネームのプロパティを該当するボックスに入力します。
    2.  クリックして **保存**します。

    >[AZURE.NOTE] Azure AD アカウントの所有者では、継続は開始する前に、アカウントの確認へのリンクが記載された電子メールを取得します。

>[AZURE.NOTE] 他の Jobscience ユーザー アカウント作成ツールを使用するや、AAD ユーザー アカウントをプロビジョニング、Jobscience 提供の Api です。

##ユーザーの割り当て
  
構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

###ユーザーを Jobscience に割り当てるには、次の手順に従います。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  * * Jobscience * * アプリケーション統合ページで、をクリックして **ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-jobscience-tutorial/IC784372.png "Assign Users")

3.  テスト ユーザーを選択して [ **割り当てる**, 、順にクリック **はい** して割り当てを確認します。

    ![あり](./media/active-directory-saas-jobscience-tutorial/IC767830.png "Yes")
  
シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。
