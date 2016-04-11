<properties 
    pageTitle="チュートリアル: Azure Active Directory と ArcGIS の統合 | Microsoft Azure" 
    description="Azure Active Directory で ArcGIS を使用して、シングル サインオンや自動プロビジョニングなどを有効にする方法について説明します。" 
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

#チュートリアル: Azure Active Directory と ArcGIS の統合

このチュートリアルでは、Azure と ArcGIS の統合について説明します。 このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   ArcGIS でのシングル サインオンが有効なサブスクリプション

このチュートリアルを完了すると、ArcGIS に割り当てた Azure AD ユーザーは、ArcGIS 会社サイト (サービス プロバイダーが開始したサインオン) で、アプリケーションにシングル サインオンまたはできるを使用して、 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。

このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  ArcGIS のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-arcgis-tutorial/IC784735.png "Scenario")
##ArcGIS のアプリケーション統合の有効化

このセクションでは、ArcGIS のアプリケーション統合を有効にする方法について説明します。

###ArcGIS のアプリケーション統合を有効にするには、次の手順に従います。

1.  左側のナビゲーション ウィンドウで、Azure 管理ポータルで、クリックして **Active Directory**します。

    ![Active Directory](./media/active-directory-saas-arcgis-tutorial/IC700993.png "Active Directory")

2.   **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3.  ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。

    ![アプリケーション](./media/active-directory-saas-arcgis-tutorial/IC700994.png "Applications")

4.  クリックして **追加** ページの下部にあります。

    ![アプリケーションの追加](./media/active-directory-saas-arcgis-tutorial/IC749321.png "Add application")

5.   **実行する操作** ダイアログ ボックスで、をクリックして **ギャラリーからアプリケーションを追加**します。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-arcgis-tutorial/IC749322.png "Add an application from gallerry")

6.   **検索ボックス**, 、型 **ArcGIS**します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-arcgis-tutorial/IC784736.png "Applcation Gallery")

7.  結果ウィンドウで [ **ArcGIS**, 、クリックして **完了** アプリケーションを追加します。

    ![ArcGIS](./media/active-directory-saas-arcgis-tutorial/IC784737.png "ArcGIS")
##シングル サインオンの構成

このセクションでは、ユーザーが SAML プロトコルに基づくフェデレーションを使用して、Azure AD でのユーザーのアカウントで ArcGIS に対する認証を行えるようにする方法を説明します。

###シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **ArcGIS** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、* * [シングル サインオン * * ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-arcgis-tutorial/IC784738.png "Configure Single Sign-On")

2.   **どのようなユーザーが ArcGIS へのサインオン** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、順にクリック **次**します。

    ![Configure Single Sign-On](./media/active-directory-saas-arcgis-tutorial/IC784739.png "Configure Single Sign-On")

3.   **アプリケーション URL の構成** ] ページで、 **ArcGIS サインイン URL** URL は、次のパターンを使ってサインインして、ユーザーが使用] ボックスに、型"*https://company.maps.arcgis.com*"、順にクリック **次**します。

    ![Configure App URL](./media/active-directory-saas-arcgis-tutorial/IC784740.png "Configure App URL")

4.   **ArcGIS でのシングル サインオンの構成** ] ページで [ **メタデータのダウンロード**, 、コンピューターのローカルにメタデータ ファイルを保存します。

    ![Configure Single Sign-On](./media/active-directory-saas-arcgis-tutorial/IC784741.png "Configure Single Sign-On")

5.  別の Web ブラウザーのウィンドウで、管理者として ArcGIS 企業サイトにログインします。

6.  クリックして **設定を編集する**です。

    ![Edit Settings](./media/active-directory-saas-arcgis-tutorial/IC784742.png "Edit Settings")

7.  クリックして **セキュリティ**します。

    ![セキュリティ](./media/active-directory-saas-arcgis-tutorial/IC784743.png "Security")

8.   **企業ログイン**, 、クリックして **Id プロバイダーの設定**します。

    ![Enterprise Logins](./media/active-directory-saas-arcgis-tutorial/IC784744.png "Enterprise Logins")

9.   **Id プロバイダーの設定** 構成] ページで、次の手順を実行します。

    ![Set Identity Provider](./media/active-directory-saas-arcgis-tutorial/IC784745.png "Set Identity Provider")

    1.  [Name] テキスト ボックスに、組織の名前を入力します。
    2.   **を使用してエンタープライズ Id プロバイダーのメタデータが提供される**, [ **ファイル**します。
    3.  ダウンロードしたメタデータ ファイルをアップロードする] をクリックして **ファイルの選択**します。
    4.  クリックして **Id プロバイダーの設定**します。

10. Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-arcgis-tutorial/IC784746.png "Configure Single Sign-On")
##ユーザー プロビジョニングの構成

Azure AD ユーザーが ArcGIS にログインできるようにするには、そのユーザーを ArcGIS にプロビジョニングする必要があります。  
ArcGIS の場合、プロビジョニングは手動で行います。

###ユーザー プロビジョニングを構成するには、次の手順に従います。

1.  ログインして **ArcGIS** テナントです。

2.  クリックして **メンバーの招待**します。

    ![Invite Members](./media/active-directory-saas-arcgis-tutorial/IC784747.png "Invite Members")

3.  選択 **電子メールを送信せずにメンバーを自動的に追加**, 、] をクリックし、 **次**します。

    ![Add Members Automatically](./media/active-directory-saas-arcgis-tutorial/IC784748.png "Add Members Automatically")

4.   **メンバー** ] ダイアログ ページで、次の手順を実行します。

    ![Add and review](./media/active-directory-saas-arcgis-tutorial/IC784749.png "Add and review")

    1.  入力、 **名**, 、**姓** と **電子メール** 、プロビジョニングする有効な AAD アカウントのです。
    2.  クリックして **の追加および確認**します。

5.  をクリックして入力したら、データを確認 **メンバーの追加**します。

    ![Add member](./media/active-directory-saas-arcgis-tutorial/IC784750.png "Add member")

>[AZURE.NOTE] 他の ArcGIS ユーザー アカウント作成ツールを使用することができますか、AAD ユーザー アカウントをプロビジョニング、ArcGIS から提供されている Api です。

##ユーザーの割り当て

構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

###ユーザーを ArcGIS に割り当てるには、次の手順に従います。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  * * ArcGIS * * アプリケーション統合ページで、をクリックして **ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-arcgis-tutorial/IC784751.png "Assign Users")

3.  テスト ユーザーを選択して [ **割り当てる**, 、順にクリック **はい** して割り当てを確認します。

    ![あり](./media/active-directory-saas-arcgis-tutorial/IC767830.png "Yes")

シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。

