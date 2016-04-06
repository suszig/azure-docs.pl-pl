<properties 
    pageTitle="チュートリアル: Azure Active Directory と Aha! の統合 | Microsoft Azure" 
    description="Aha を使用する方法を説明します。 シングル サインオン、自動化されたプロビジョニング、および詳細を有効にする Azure Active Directory です。" 
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

#チュートリアル: Azure Active Directory と Aha! の統合

このチュートリアルでは、Azure と Aha! の統合について説明します。  
このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   疑問が出てくる! シングル サインオン サブスクリプション有効になっています。

このチュートリアルを完了すると、Azure AD ユーザーに割り当てた Aha! aha アプリケーションにシングル サインオンすることになります。 会社サイト (サービス プロバイダーが開始したサインオン)、またはを使用して、 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。

このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  Aha! のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-aha-tutorial/IC798944.png "Scenario")
##Aha! のアプリケーション統合の有効化

このセクションでは、Aha! のアプリケーション統合を有効にする方法について説明します。

###Aha! のアプリケーション統合を有効にするには、次の手順に従います。

1.  左側のナビゲーション ウィンドウで、Azure 管理ポータルで、クリックして **Active Directory**します。

    ![Active Directory](./media/active-directory-saas-aha-tutorial/IC700993.png "Active Directory")

2.   **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3.  ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。

    ![アプリケーション](./media/active-directory-saas-aha-tutorial/IC700994.png "Applications")

4.  クリックして **追加** ページの下部にあります。

    ![アプリケーションの追加](./media/active-directory-saas-aha-tutorial/IC749321.png "Add application")

5.   **実行する操作** ダイアログ ボックスで、をクリックして **ギャラリーからアプリケーションを追加**します。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-aha-tutorial/IC749322.png "Add an application from gallerry")

6.   **検索ボックス**, 、型 **Aha!**します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-aha-tutorial/IC798945.png "Application Gallery")

7.  結果ウィンドウで [ **Aha!**, 、クリックして **完了** アプリケーションを追加します。

    ![Aha!](./media/active-directory-saas-aha-tutorial/IC802746.png "Aha!")
##シングル サインオンの構成

このセクションの目標は、ユーザーが Aha への認証を有効にする方法を説明するです。 Azure AD アカウントで SAML プロトコルに基づくフェデレーションを使用します。

###シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **Aha!** アプリケーション統合ページで、をクリックして **でのシングル サインオンを構成する** を開くには、* * [シングル サインオン * * ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-aha-tutorial/IC798946.png "Configure Single Sign-On")

2.   **どのようなユーザーの aha!** ページで、[ **Microsoft Azure AD シングル サインオン**, 、クリックして **次**します。

    ![Configure Single Sign-On](./media/active-directory-saas-aha-tutorial/IC798947.png "Configure Single Sign-On")

3.   **アプリケーション URL の構成** ] ページで、 **Aha!Aha! サインオン URL**] テキスト ボックスに、Aha! アプリケーションへのサインオンにユーザーが使用する URL アプリケーション (例::"*https://company.aha.io/session/new*") を順にクリック **次**します。

    ![アプリケーション URL の構成](./media/active-directory-saas-aha-tutorial/IC798948.png "Configure App URL")

4.   **Aha でのシングル サインオンの構成です。** ページで、メタデータ ファイルをダウンロードする] をクリックして **メタデータのダウンロード**, 、コンピューターのローカルにメタデータ ファイルを保存します。

    ![Configure Single Sign-On](./media/active-directory-saas-aha-tutorial/IC798949.png "Configure Single Sign-On")

5.  別の web ブラウザーのウィンドウでログイン、Aha! 会社サイトに管理者として。

6.  上部にあるメニュー [ **設定**します。

    ![Settings](./media/active-directory-saas-aha-tutorial/IC798950.png "Settings")

7.  クリックして **アカウント**します。

    ![プロファイル](./media/active-directory-saas-aha-tutorial/IC798951.png "Profile")

8.  クリックして **セキュリティとシングル サインオン**します。

    ![セキュリティとシングル サインオン](./media/active-directory-saas-aha-tutorial/IC798952.png "Security and single sign-on")

9.   **でのシングル サインオン** ] セクションで、として **Id プロバイダー**, [ **SAML2.0**します。

    ![セキュリティとシングル サインオン](./media/active-directory-saas-aha-tutorial/IC798953.png "Security and single sign-on")

10.  **でのシングル サインオン** 構成] ページで、次の手順を実行します。

    ![シングル サインオン](./media/active-directory-saas-aha-tutorial/IC798954.png "Single Sign-On")

    1.   **名前** ] ボックスに、構成の名前を入力します。
    2.   **を使用して構成**, [ **メタデータ ファイル**します。
    3.  ダウンロードしたメタデータ ファイルをアップロードする] をクリックして **参照**します。
    4.  クリックして **更新**します。

11. Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![Configure Single Sign-On](./media/active-directory-saas-aha-tutorial/IC798955.png "Configure Single Sign-On")
##ユーザー プロビジョニングの構成

Azure AD ユーザーが Aha! にログインできるようにするには、そのユーザーを Aha! にプロビジョニングする必要があります。  
Aha! の場合、プロビジョニングは自動化されています。  
ユーザー側で必要な操作はありません。
  
最初のシングル サインオンの試行中に、必要に応じてユーザーが自動的に作成されます。

>[AZURE.NOTE] その他の Aha を使用することができます。 ユーザー アカウント作成ツールまたは Aha で提供される Api です。 AAD ユーザー アカウントをプロビジョニングします。

##ユーザーの割り当て

構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

###ユーザーを Aha! に割り当てるには、次の手順に従います。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  * * 疑問が出てくる! * * アプリケーション統合ページで、をクリックして **ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-aha-tutorial/IC798956.png "Assign Users")

3.  テスト ユーザーを選択して [ **割り当てる**, 、順にクリック **はい** して割り当てを確認します。

    ![あり](./media/active-directory-saas-aha-tutorial/IC767830.png "Yes")

シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。


