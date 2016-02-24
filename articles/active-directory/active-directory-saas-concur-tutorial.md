<properties 
    pageTitle="チュートリアル: Azure Active Directory と Concur の統合 | Microsoft Azure" 
    description="Azure Active Directory で Concur を使用して、シングル サインオンや自動プロビジョニングなどを有効にする方法について説明します。" 
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

#チュートリアル: Azure Active Directory と Concur の統合  


このチュートリアルでは、Azure と Concur の統合について説明します。  
このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   Concur のテナント

このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  Concur のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-concur-tutorial/IC769766.png "Scenario")

>[AZURE.NOTE] SAML を使用したフェデレーション SSO の Concur サブスクリプションの構成では、実行する Concur を問い合わせる必要がありますが、個別のタスクです。

##Concur のアプリケーション統合の有効化

このセクションでは、Concur のアプリケーション統合を有効にする方法について説明します。

###Concur のアプリケーション統合を有効にするには、次の手順を実行します。

1.  左側のナビゲーション ウィンドウで、Azure 管理ポータルで、クリックして **Active Directory**します。

    ![Active Directory](./media/active-directory-saas-concur-tutorial/IC700993.png "Active Directory")

2.   **ディレクトリ** 一覧で、ディレクトリ統合を有効にすることを指定するディレクトリを選択します。

3.  ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。

    ![アプリケーション](./media/active-directory-saas-concur-tutorial/IC700994.png "Applications")

4.  開くには、 **アプリケーション ギャラリー**, 、] をクリックして **アプリの追加**, 、クリックして **組織で使用するためのアプリケーションの追加**します。

    ![どの操作を行いますか。](./media/active-directory-saas-concur-tutorial/IC700995.png "What do you want to do?")

5.   **検索ボックス**, 、型 **Concur**します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-concur-tutorial/IC721727.png "Application Gallery")

6.  結果ウィンドウで [ **Concur**, 、クリックして **完了** アプリケーションを追加します。

    ![Concur](./media/active-directory-saas-concur-tutorial/IC721728.png "Concur")
##シングル サインオンの構成

このセクションでは、ユーザーが SAML プロトコルに基づくフェデレーションを使用して、Azure AD でのユーザーのアカウントで Concur に対する認証を行うことができるようにする方法を説明します。

>[AZURE.NOTE] SAML を使用したフェデレーション SSO の Concur サブスクリプションの構成では、実行する Concur を問い合わせる必要がありますが、個別のタスクです。

###シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの * * Concur * * アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、* * [シングル サインオン * * ダイアログ。

    ![シングル サインオンの構成](./media/active-directory-saas-concur-tutorial/IC769767.png "Configure single sign-on")

2.   **どのようなユーザーが Concur にサインオン** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、] をクリックし、 **次**します。

    ![シングル サインオンの構成](./media/active-directory-saas-concur-tutorial/IC769768.png "Configure single sign-on")

3.   **アプリケーション URL の構成** ] ページで、 **Concur のサインイン URL** ] ボックスに、URL を入力、concur のテナントにサインインし、順にクリックして **次**: 

    ![サインイン URL の構成](./media/active-directory-saas-concur-tutorial/IC769769.png "Configure sign in URL")

4.   **Concur でのシングル サインオンの構成** ] ページで、次の手順を実行します。

    ![サインイン URL の構成](./media/active-directory-saas-concur-tutorial/IC769770.png "Configure sign in URL")

    1.  [メタデータのダウンロード] をクリックし、データ ファイルをコンピューターに保存します。
    2.  Concur サポート チームに問い合わせて、テナントの SSO を構成します。
    3.  シングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。  

    >[AZURE.NOTE] SAML を使用したフェデレーション SSO の Concur サブスクリプションの構成では、実行する Concur を問い合わせる必要がありますが、個別のタスクです。

##ユーザー プロビジョニングの構成

このセクションでは、Active Directory のユーザー アカウントのプロビジョニングを Concur に対して有効にする方法について説明します。

Expense Service でアプリケーションを有効にするには、正しい設定と共に、Web サービス管理者のプロファイルを使用する必要があります。 T&E 管理機能で使用する既存の管理者プロファイルに WS 管理者のロールを追加するだけでは十分ではありません。

Concur コンサルタントまたはクライアント管理者は、Web サービス管理者プロファイルを別に作成する必要があります。クライアント管理者は、Web サービス管理者の機能 (アプリケーションの有効化など) に対してこのプロファイルを使用します。 このプロファイルは、クライアント管理者が日々使用する T&E 管理者プロファイルとは別にする必要があります (T&E 管理者プロファイルには、WS 管理者のロールを割り当てないでください)。

アプリケーションの有効化に使用するプロファイルを作成するときは、ユーザー プロファイルのフィールドにクライアント管理者の名前を入力します。 これにより、プロファイルに所有権を割り当てできます。クライアントはこのプロファイルをクリックして使用してログイン プロファイルを作成すると、"*を有効にする*"Web Services] メニューにあるパートナー アプリケーションのボタンをクリックします。

次の理由から、通常の T&E の管理に使用するプロファイルではこのアクションを実行しないでください。

1.  クライアントがクリックした人である、"*はい*"、アプリケーションが有効にした後に表示されるダイアログ ウィンドウでします。 このクリックは、クライアントがパートナー アプリケーションに対して自分のデータにアクセスすることを許可する意思を示すものです。したがって、パートナーなど、クライアント以外の人物が [はい] をクリックすることはできません。
2.  クライアント管理者が T&E 管理者プロファイルを使用してアプリケーションを有効にした後で、会社を退職した場合 (その結果、プロファイルが非アクティブになった場合)、そのプロファイルを使用して有効にしたアプリケーションは、別のアクティブな WS 管理者プロファイルで有効にするまで機能しなくなります。 このため、WS 管理者プロファイルを別途作成する必要があります。
3.  管理者が会社を退職した場合、その WS 管理者プロファイルに関連付けられている名前を、必要に応じて別の管理者に変更することができます。この方法では、プロファイルを非アクティブ化する必要がないため、有効にしたアプリケーションに影響が及ぶことはありません。

###ユーザー プロビジョニングを構成するには、次の手順に従います。

1.  ログオン、 **Concur** テナントです。

2.   **管理** メニューの [ **Web サービス**します。

    ![Concur テナント](./media/active-directory-saas-concur-tutorial/IC721729.png "Concur tenant")

3.  左側にあるから、 **Web サービス** ] ウィンドウの [ **Enable Partner Application**します。

    ![パートナー アプリケーションの有効化](./media/active-directory-saas-concur-tutorial/IC721730.png "Enable Partner Application")

4.   **Enable Application** 一覧で、[ **Azure Active Directory**, 、] をクリックし、 **を有効にする**です。

    ![Microsoft Azure Active Directory](./media/active-directory-saas-concur-tutorial/IC721731.png "Microsoft Azure Active Directory")

5.  をクリックして **はい** を閉じる、 **アクションの確認** ダイアログ。

    ![アクションの確認](./media/active-directory-saas-concur-tutorial/IC721732.png "Confirm Action")

6.  Azure の管理ポータルで [ **Concur** を開くには、アプリケーションの一覧から、 **Concur** ] ダイアログ ページです。

7.  開くには、 **ユーザー プロビジョニングの構成** ダイアログ ページで、をクリックして **ユーザー プロビジョニングの構成**します。

8.  ユーザー名と Concur 管理者のパスワードを入力し、クリックして **次**します。

9.  構成を終了する、 **確認** ページで、クリックして、 **完了** ] ボタンをクリックします。

ここで、テスト アカウントを作成して 10 分間待機し、アカウントが Concur に同期されていることを確認します。
##ユーザーの割り当て

構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

###ユーザーを Concur に割り当てるには、次の手順を実行します。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  * * Concur * * アプリケーション統合ページで、をクリックして **ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-concur-tutorial/IC769771.png "Assign users")

3.  テスト ユーザーを選択して [ **割り当てる**, 、順にクリック **はい** して割り当てを確認します。

    ![あり](./media/active-directory-saas-concur-tutorial/IC767830.png "Yes")

ここで 10 分間待機し、アカウントが Concur に同期されたことを確認します。

シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。

