<properties 
    pageTitle="チュートリアル: Azure Active Directory と NetDocuments の統合 | Microsoft Azure" 
    description="Azure Active Directory で NetDocuments を使用して、シングル サインオンや自動プロビジョニングなどを有効にする方法について説明します。" 
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

#チュートリアル: Azure Active Directory と NetDocuments の統合
  
このチュートリアルの目的は、Azure と NetDocuments の統合を紹介することです。  
このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   NetDocuments テナント
  
このチュートリアルを完了すると、NetDocuments に割り当てた Azure AD ユーザーは、NetDocuments 会社サイト (サービス プロバイダーが開始したサインオン) で、アプリケーションにシングル サインオンまたはできるを使用して、 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。
  
このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  NetDocuments のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-netdocuments-tutorial/IC795040.png "Scenario")
##NetDocuments のアプリケーション統合の有効化
  
このセクションでは、NetDocuments のアプリケーション統合を有効にする方法を説明します。

###NetDocuments のアプリケーション統合を有効にするには、次の手順に従います。

1.  左側のナビゲーション ウィンドウで、Azure 管理ポータルで、クリックして **Active Directory**します。

    ![Active Directory](./media/active-directory-saas-netdocuments-tutorial/IC700993.png "Active Directory")

2.   **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3.  ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。

    ![アプリケーション](./media/active-directory-saas-netdocuments-tutorial/IC700994.png "Applications")

4.  クリックして **追加** ページの下部にあります。

    ![アプリケーションの追加](./media/active-directory-saas-netdocuments-tutorial/IC749321.png "Add application")

5.   **実行する操作** ダイアログ ボックスで、をクリックして **ギャラリーからアプリケーションを追加**します。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-netdocuments-tutorial/IC749322.png "Add an application from gallerry")

6.   **検索ボックス**, 、型 **NetDocuments**します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-netdocuments-tutorial/IC795041.png "Application Gallery")

7.  結果ウィンドウで [ **NetDocuments**, 、クリックして **完了** アプリケーションを追加します。

    ![NetDocuments](./media/active-directory-saas-netdocuments-tutorial/IC795042.png "NetDocuments")
##シングル サインオンの構成
  
このセクションでは、ユーザーが SAML プロトコルに基づくフェデレーションを使用して、Azure AD でのユーザーのアカウントで NetDocuments に対する認証を行えるようにする方法を説明します。  
NetDocuments にシングル サインオンを構成するには、証明書の拇印の値を取得する必要があります。  
この手順に慣れていない場合は、次を参照してください。 [証明書の拇印の値を取得する方法](http://youtu.be/YKQF266SAxI)します。

###シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **NetDocuments** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、* * [シングル サインオン * * ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-netdocuments-tutorial/IC795043.png "Configure Single Sign-On")

2.   **どのようなユーザーが NetDocuments へのサインオン** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、順にクリック **次**します。

    ![Configure Single Sign-On](./media/active-directory-saas-netdocuments-tutorial/IC795044.png "Configure Single Sign-On")

3.   **アプリケーション URL の構成** ] ページで、次の手順を実行します。

    ![アプリケーション URL の構成](./media/active-directory-saas-netdocuments-tutorial/IC795045.png "Configure App URL")

    1.   **サインオン URL** ] ボックスに、型に使用する URL、ユーザーが、NetDocuments アプリケーションへのサインオン (例::"*https://vault.netvoyage.com/neWeb2/docCent.aspx?whr=CA-JI1BG3H1*") です。
    2.   **NetDocuments 応答 URL** ] テキスト ボックスに入力した同じ値を入力、 **サインオン URL** ] ボックスに貼り付けます。  

        >[AZURE.NOTE]最後に、適切な値を見つけることができます、 **Federated Identity** ダイアログ (手順 9. のスクリーン ショットを参照してください)。

    3.  クリックして **次へ]**

4.   **NetDocuments でのシングル サインオンの構成** ] ページで、証明書をダウンロードする] をクリックして **証明書のダウンロード**, 、お使いのコンピューターにローカルで証明書ファイルを保存します。

    ![Configure Single Sign-On](./media/active-directory-saas-netdocuments-tutorial/IC795046.png "Configure Single Sign-On")

5.  別の Web ブラウザーのウィンドウで、管理者として NetDocuments 企業サイトにログインします。

6.  移動して **Admin**します。

7.  クリックして **の追加と削除] のユーザーとグループ**します。

    ![リポジトリ](./media/active-directory-saas-netdocuments-tutorial/IC795047.png "Repository")

8.  クリックして **認証オプションの詳細な構成する**です。

    ![認証オプションの詳細な構成](./media/active-directory-saas-netdocuments-tutorial/IC795048.png "Configure advanced authentication options")

9.  [ **Federated Identity** ] ダイアログ ボックスで、次の手順を実行します。

    ![フェデレーション ID](./media/active-directory-saas-netdocuments-tutorial/IC795049.png "Federated Identitty")

    1.  として **id サーバーの種類をフェデレーション**, [ **Active Directory フェデレーション サービス**します。
    2.  クリックして **ファイルの選択**, 、ダウンロードしたメタデータ ファイルをアップロードします。
    3.  Click **OK**.

10. Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-netdocuments-tutorial/IC795050.png "Configure Single Sign-On")
##ユーザー プロビジョニングの構成
  
Azure AD ユーザーが NetDocuments にログインできるようにするには、そのユーザーを NetDocuments にプロビジョニングする必要があります。  
NetDocuments の場合、プロビジョニングは手動で行います。

###ユーザー プロビジョニングを構成するには、次の手順に従います。

1.  サインオン、 **NetDocuments** 会社サイトに管理者として。

2.  上部にあるメニュー [ **Admin**します。

    ![管理者](./media/active-directory-saas-netdocuments-tutorial/IC795051.png "Admin")

3.  クリックして **の追加と削除] のユーザーとグループ**します。

    ![リポジトリ](./media/active-directory-saas-netdocuments-tutorial/IC795047.png "Repository")

4.   **電子メール アドレス** ] ボックスに、をクリックして、プロビジョニングする有効な Azure Active Directory アカウントの電子メール アドレスを入力 **ユーザーの追加**します。

    ![電子メール アドレス](./media/active-directory-saas-netdocuments-tutorial/IC795053.png "Email Address")

    >[AZURE.NOTE]Azure Active Directory アカウント所有者では、アクティブ化する前に、アカウントの確認へのリンクが記載された電子メールを取得します。

>[AZURE.NOTE]他の NetDocuments ユーザー アカウント作成ツールを使用することができますか、AAD ユーザー アカウントをプロビジョニング、NetDocuments から提供されている Api です。

##ユーザーの割り当て
  
構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

###ユーザーを NetDocuments に割り当てるには、次の手順に従います。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  * * NetDocuments * * アプリケーション統合ページで、をクリックして **ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-netdocuments-tutorial/IC795054.png "Assign Users")

3.  テスト ユーザーを選択して [ **割り当てる**, 、順にクリック **はい** して割り当てを確認します。

    ![あり](./media/active-directory-saas-netdocuments-tutorial/IC767830.png "Yes")
  
シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。
