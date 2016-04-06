<properties 
    pageTitle="チュートリアル: Azure Active Directory と UserVoice の統合 | Microsoft Azure" 
    description="Azure Active Directory で UserVoice を使用して、シングル サインオンや自動プロビジョニングなどを有効にする方法を説明します。" 
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

#チュートリアル: Azure Active Directory と UserVoice の統合
  
このチュートリアルでは、Azure と UserVoice の統合について説明します。  
このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   UserVoice のテナント
  
このチュートリアルを完了すると、UserVoice に割り当てた Azure AD ユーザーは、UserVoice 会社サイト (サービス プロバイダーが開始したサインオン) で、アプリケーションにシングル サインオンまたはできるを使用して、 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。
  
このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  UserVoice のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-uservoice-tutorial/IC777514.png "Scenario")

##UserVoice のアプリケーション統合の有効化
  
このセクションでは、UserVoice のアプリケーション統合を有効にする方法を説明します。

###UserVoice のアプリケーション統合を有効にするには、次の手順を実行します。

1.  左側のナビゲーション ウィンドウで、Azure 管理ポータルで、クリックして **Active Directory**します。

    ![Active Directory](./media/active-directory-saas-uservoice-tutorial/IC700993.png "Active Directory")

2.   **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3.  ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。

    ![アプリケーション](./media/active-directory-saas-uservoice-tutorial/IC700994.png "Applications")

4.  クリックして **追加** ページの下部にあります。

    ![アプリケーションの追加](./media/active-directory-saas-uservoice-tutorial/IC749321.png "Add application")

5.   **実行する操作** ダイアログ ボックスで、をクリックして **ギャラリーからアプリケーションを追加**します。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-uservoice-tutorial/IC749322.png "Add an application from gallerry")

6.   **検索ボックス**, 、型 **UserVoice**します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-uservoice-tutorial/IC777513.png "Application gallery")

7.  結果ウィンドウで [ **UserVoice**, 、クリックして **完了** アプリケーションを追加します。

    ![UserVoice](./media/active-directory-saas-uservoice-tutorial/IC777810.png "UserVoice")

##シングル サインオンの構成
  
このセクションでは、ユーザーが SAML プロトコルに基づくフェデレーションを使用して、Azure AD でのユーザーのアカウントで UserVoice に対する認証を行うことができるようにする方法を説明します。  
UserVoice のシングル サインオンを構成するには、証明書からサムプリント値を取得する必要があります。  
この手順に慣れていない場合は、次を参照してください。 [証明書の拇印の値を取得する方法](http://youtu.be/YKQF266SAxI)します。

###シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **UserVoice** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、* * [シングル サインオン * * ダイアログ。

    ![シングル サインオンの構成](./media/active-directory-saas-uservoice-tutorial/IC777515.png "Configure single sign-on")

2.   **どのようなユーザーの UserVoice に** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、順にクリック **次**します。

    ![シングル サインオンの構成](./media/active-directory-saas-uservoice-tutorial/IC777516.png "Configure single sign-on")

3.   **アプリケーション URL の構成** ] ページで、 **UserVoice サインイン URL** ] ボックスに、次のパターンを使用して URL を入力"*https://\<tenant-name\>。UserVoice.com*"、クリックして **次**します。

    ![アプリケーション URL の構成](./media/active-directory-saas-uservoice-tutorial/IC777517.png "Configure App URL")

4.   **UserVoice でのシングル サインオンの構成** ] ページで、証明書をダウンロードする] をクリックして **証明書のダウンロード**, 、としては、証明書ファイルをローカルに保存および **c:\\UserVoice.cer**します。

    ![シングル サインオンの構成](./media/active-directory-saas-uservoice-tutorial/IC777518.png "Configure single sign-on")

5.  別の Web ブラウザーのウィンドウで、UserVoice 企業サイトに管理者としてログインします。

6.  上部のツール バーの [設定] をクリックし、[Web ポータル] を選択します。

    ![Settings](./media/active-directory-saas-uservoice-tutorial/IC777519.png "Settings")

7.   **Web ポータル** ] タブで、 **ユーザー認証** ] をクリックして **編集** を開くには、 **ユーザー認証の編集** ] ダイアログ ページ

    ![Web ポータル](./media/active-directory-saas-uservoice-tutorial/IC777520.png "Web portal")

8.   **ユーザー認証の編集** ] ダイアログ ページで、次の手順を実行します。

    ![ユーザー認証の編集](./media/active-directory-saas-uservoice-tutorial/IC777521.png "Edit user authentication")

    1.  クリックして **シングル サインオン (SSO)**します。
    2.  Azure ポータルで、 **UserVoice でのシングル サインオンの構成** ] ダイアログ ページでコピー、 **リモート ログイン URL** 値に設定して、貼り付けます、 **SSO リモート サインイン** ] ボックスに貼り付けます。
    3.  Azure ポータルでの **UserVoice でのシングル サインオンの構成** ] ダイアログ ページでコピー、 **リモート ログアウト URL** 値に設定して、貼り付けます、 **リモート サインアウト textbox**します。
    4.  コピー、 **拇印** エクスポートした証明書からの値し、貼り付けます、 **現在の証明書の SHA1 フィンガー プリント** ] ボックスに貼り付けます。  

        >[AZURE.TIP] 詳細については、次を参照してください [証明書の拇印の値を取得する方法。](http://youtu.be/YKQF266SAxI)

    5.  クリックして **認証設定の保存**します。

9.  Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![シングル サインオンの構成](./media/active-directory-saas-uservoice-tutorial/IC777522.png "Configure single sign-on")

##ユーザー プロビジョニングの構成
  
Azure AD ユーザーが UserVoice にログインできるようにするには、そのユーザーを UserVoice にプロビジョニングする必要があります。  
UserVoice の場合、プロビジョニングは手動で行います。

###ユーザー アカウントをプロビジョニングするには、次の手順を実行します。

1.  ログインして **UserVoice** テナントです。

2.  移動して **設定**します。

    ![Settings](./media/active-directory-saas-uservoice-tutorial/IC777811.png "Settings")

3.  クリックして **全般**します。

4.  クリックして **エージェントと権限**します。

    ![エージェントとアクセス許可](./media/active-directory-saas-uservoice-tutorial/IC777812.png "Agents and permissions")

5.  クリックして **管理者の追加**します。

    ![管理者の追加](./media/active-directory-saas-uservoice-tutorial/IC777813.png "Add admins")

6.   **管理者を招待** ] ダイアログ ボックスで、次の手順を実行します。

    ![管理者の招待](./media/active-directory-saas-uservoice-tutorial/IC777814.png "Invite admins")

    1.  電子メール] テキストのボックスで、プロビジョニングして、をクリックし、アカウントの電子メール アドレスを入力 **追加**します。
    2.  クリックして **招待**します。

>[AZURE.NOTE] 他の UserVoice ユーザー アカウント作成ツールを使用するや、AAD ユーザー アカウントのプロビジョニングに UserVoice を提供の Api です。

##ユーザーの割り当て
  
構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

###ユーザーを UserVoice に割り当てるには、次の手順を実行します。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  * * UserVoice * * アプリケーション統合ページで、をクリックして **ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-uservoice-tutorial/IC777523.png "Assign users")

3.  テスト ユーザーを選択して [ **割り当てる**, 、順にクリック **はい** して割り当てを確認します。

    ![あり](./media/active-directory-saas-uservoice-tutorial/IC767830.png "Yes")
  
シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。

