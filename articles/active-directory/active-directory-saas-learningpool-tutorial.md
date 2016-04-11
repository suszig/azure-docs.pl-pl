<properties 
    pageTitle="チュートリアル: Azure Active Directory と Learningpool の統合 | Microsoft Azure" 
    description="Azure Active Directory で Learningpool を使用して、シングル サインオンや自動プロビジョニングなどを有効にする方法について説明します。" 
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

#チュートリアル: Azure Active Directory と Learningpool の統合
  
このチュートリアルの目的は、Azure と Learningpool の統合を示すことです。  
このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   Learningpool でのシングル サインオンが有効なサブスクリプション
  
このチュートリアルを完了すると、Learningpool に割り当てた Azure AD ユーザーは、Learningpool 会社サイト (サービス プロバイダーが開始したサインオン) で、アプリケーションにシングル サインオンまたはできるを使用して、 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。
  
このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  Learningpool のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-learningpool-tutorial/IC791166.png "Scenario")
##Learningpool のアプリケーション統合の有効化
  
このセクションでは、Learningpool のアプリケーション統合を有効にする方法を説明します。

###Learningpool のアプリケーション統合を有効にするには、次の手順に従います。

1.  左側のナビゲーション ウィンドウで、Azure 管理ポータルで、クリックして **Active Directory**します。

    ![Active Directory](./media/active-directory-saas-learningpool-tutorial/IC700993.png "Active Directory")

2.   **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3.  ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。

    ![アプリケーション](./media/active-directory-saas-learningpool-tutorial/IC700994.png "Applications")

4.  クリックして **追加** ページの下部にあります。

    ![アプリケーションの追加](./media/active-directory-saas-learningpool-tutorial/IC749321.png "Add application")

5.   **実行する操作** ダイアログ ボックスで、をクリックして **ギャラリーからアプリケーションを追加**します。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-learningpool-tutorial/IC749322.png "Add an application from gallerry")

6.   **検索ボックス**, 、型 **Learningpool**します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-learningpool-tutorial/IC795073.png "Application Gallery")

7.  結果ウィンドウで [ **Learningpool**, 、クリックして **完了** アプリケーションを追加します。

    ![Learningpool](./media/active-directory-saas-learningpool-tutorial/IC809577.png "Learningpool")
##シングル サインオンの構成
  
このセクションでは、ユーザーが SAML プロトコルに基づくフェデレーションを使用して、Azure AD でのユーザーのアカウントで Learningpool に対する認証を行えるようにする方法を説明します。
  
Learningpool アプリケーションにはカスタム属性マッピングを追加する必要がある特定の形式での SAML アサーションが必要ですが、 **saml トークン属性** 構成します。  
次のスクリーンショットはその例です。

![SAML トークンの属性](./media/active-directory-saas-learningpool-tutorial/IC795074.png "SAML Token Attributes")

###シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **Learningpool** アプリケーション統合ページで、上部のメニューをクリックして **属性** を開くには、 **SAML トークン属性** ダイアログ。

    ![属性](./media/active-directory-saas-learningpool-tutorial/IC795075.png "Attributes")

2.  必要な属性のマッピングを追加するには、次の手順を実行します。

    ###  

  	|属性名                |属性値            |
  	|------------------------------|---------------------------|

     urn:oid:1.2.840.113556.1.4.221 | User.userprincipalname
  	|-------------------------------|--------------------------|  
     urn:oid:2.5.4.42|User.givenname   
  	|urn:oid:0.9.2342.19200300.100.1.3|User.mail
  	|urn:oid:2.5.4.4|User.surname

    1.  上記の表の各データ行をクリックして **ユーザー属性の追加**します。
    2.   **属性名** ] ボックスに、その行に対して表示される属性名を入力します。
    3.   **属性値** 一覧で、その行に対して表示される属性の値を選択します。
    4.  クリックして **完了**します。

3.  クリックして **の変更を適用**します。

4.  ブラウザーで、次のようにクリックします。 **戻る** を開くには、 **クイック スタート** ダイアログを再度開きます。

5.  をクリックして **でのシングル サインオンを構成する** を開くには、* * [シングル サインオン * * ダイアログ。

    ![シングル サインオンの構成](./media/active-directory-saas-learningpool-tutorial/IC795076.png "Configure Singel Sign-On")

6.   **どのようなユーザーの Learningpool へのアクセス** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、] をクリックし、 **次**します。

    ![Configure Single Sign-On](./media/active-directory-saas-learningpool-tutorial/IC795077.png "Configure Single Sign-On")

7.   **アプリケーション URL の構成** ] ページで、 **Learningpool サインオン URL** URL は、ユーザーが Learningpool アプリケーションにサインオンする使用] ボックスに、種類 (例:。 
https://parliament.preview.learningpool.com/auth/shibboleth/index.php) を順にクリック **次**します。

    ![アプリケーション URL の構成](./media/active-directory-saas-learningpool-tutorial/IC795078.png "Configure App URL")

8.   **Learningpool でのシングル サインオンの構成** ] ページで、メタデータをダウンロードする] をクリックして **メタデータのダウンロード**, 、お使いのコンピューターにローカルで証明書ファイルを保存します。

    ![Configure Single Sign-On](./media/active-directory-saas-learningpool-tutorial/IC795079.png "Configure Single Sign-On")

9.  メタデータ ファイルを Learningpool サポート チームに転送します。

    >[AZURE.NOTE]シングル サインオンの Learningpool サポート チームを有効にするには

10. Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-learningpool-tutorial/IC795080.png "Configure Single Sign-On")
##ユーザー プロビジョニングの構成
  
Azure AD ユーザーが Learningpool にログインできるようにするには、ユーザーを Learningpool にプロビジョニングする必要があります。
  
Learningpool へのユーザー プロビジョニングの構成にあたって必要な操作はありません。  
ユーザーは、Learningpool サポート チームが作成する必要があります。

>[AZURE.NOTE]他の Learningpool ユーザー アカウント作成ツールを使用することができますか、AAD ユーザー アカウントをプロビジョニング、Learningpool から提供されている Api です。

##ユーザーの割り当て
  
構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

###ユーザーを Learningpool に割り当てるには、次の手順に従います。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  * * Learningpool * * アプリケーション統合ページで、をクリックして **ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-learningpool-tutorial/IC795081.png "Assign Users")

3.  テスト ユーザーを選択して [ **割り当てる**, 、順にクリック **はい** して割り当てを確認します。

    ![あり](./media/active-directory-saas-learningpool-tutorial/IC767830.png "Yes")
  
シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。

