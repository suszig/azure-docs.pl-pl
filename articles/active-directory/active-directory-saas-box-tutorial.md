<properties 
    pageTitle="チュートリアル: Azure Active Directory と Box の統合 | Microsoft Azure" 
    description="Azure Active Directory で Box を使用して、シングル サインオンや自動プロビジョニングなどを有効にする方法について説明します。" 
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




#チュートリアル: Azure Active Directory と Box の統合


  
このチュートリアルでは、Azure と Box の統合について説明します。  
このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   Box のテスト テナント
  
このチュートリアルを完了すると、ボックスに割り当てた Azure AD ユーザーは、ボックスの会社サイト (サービス プロバイダーが開始したサインオン) で、アプリケーションにシングル サインオンまたはできるを使用して、 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)
  
このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  Box のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-box-tutorial/IC769537.png "Scenario")



##Box のアプリケーション統合の有効化
  
このセクションでは、Box のアプリケーション統合を有効にする方法について説明します。

###Box のアプリケーション統合を有効にするには、次の手順を実行します。

1.  左側のナビゲーション ウィンドウで、Azure 管理ポータルで、クリックして **Active Directory**します。

    ![Active Directory](./media/active-directory-saas-box-tutorial/IC700993.png "Active Directory")

2.   **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3.  ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。

    ![アプリケーション](./media/active-directory-saas-box-tutorial/IC700994.png "Applications")

4.  クリックして **追加** ページの下部にあります。

    ![アプリケーションの追加](./media/active-directory-saas-box-tutorial/IC749321.png "Add application")

5.   **実行する操作** ダイアログ ボックスで、をクリックして **ギャラリーからアプリケーションを追加**します。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-box-tutorial/IC749322.png "Add an application from gallerry")

6.   **検索ボックス**, 、型 **ボックス**します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-box-tutorial/IC701023.png "Application gallery")

7.  結果ウィンドウで [ **ボックス**, 、] をクリックし、 **完了** アプリケーションを追加します。

    ![Box](./media/active-directory-saas-box-tutorial/IC701024.png "Box")



##シングル サインオンの構成
  
このセクションでは、SAML プロトコルに基づくフェデレーションを使用して、ユーザーが Azure AD のアカウントで Box に対する認証を行えるようにする方法を説明します。 <br>
この手順の一環としては、メタデータを Box.com にアップロードする必要です。

###シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **ボックス** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、* * [シングル サインオン * * ダイアログ。

    ![Configure single sign-on](./media/active-directory-saas-box-tutorial/IC769538.png "Configure single sign-on")

2.   **どのようなボックスにサインオン** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、] をクリックし、 **次**します。

    ![Configure single sign-on](./media/active-directory-saas-box-tutorial/IC769539.png "Configure single sign-on")

3.   **アプリケーション URL の構成** ] ページの [、 **Box テナント URL** ] ボックスに、ボックス テナント URL を入力 (例:: https://<mydomainname>. box.com)、順にクリック **次へ]**します。

    ![アプリケーション URL の構成](./media/active-directory-saas-box-tutorial/IC669826.png "Configure app URL")

4.   **Box でのシングル サインオン構成** ] ページで、メタデータをダウンロードする] をクリックして **メタデータのダウンロード**, 、データ ファイルをコンピューターのローカルにします。

    ![Configure single sign-on](./media/active-directory-saas-box-tutorial/IC669824.png "Configure single sign-on")

5.  メタデータ ファイルを Box サポート チームに転送します。 サポート チームは、シングル サインオンを構成する必要があります。

6.  シングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![Configure single sign-on](./media/active-directory-saas-box-tutorial/IC769540.png "Configure single sign-on")
##ユーザー プロビジョニングの構成
  
このセクションでは、Box への Active Directory ユーザー アカウントのプロビジョニングを有効にする方法を説明します。

###シングル サインオンを構成するには、次の手順に従います。

1. Azure の管理ポータルでの **ボックス** アプリケーション統合ページで、] をクリックして **ユーザー プロビジョニングの構成** を開くには、 **ユーザー プロビジョニングの構成** ダイアログ。 <br> <br> ![自動ユーザー プロビジョニングを有効にします。](./media/active-directory-saas-box-tutorial/IC769541.png "Enable automatic user provisioning")

2.  **ボックスへのユーザー プロビジョニングを有効にする** ダイアログ ページで、をクリックして **ユーザー プロビジョニングを有効にする**です。 <br><br>  ![自動ユーザー プロビジョニングを有効にします。](./media/active-directory-saas-box-tutorial/IC769544.png "Enable automatic user provisioning")

3.  **ボックスへのアクセス許可にログイン** ] ページで必要な資格情報を提供してクリックして **Authorize**します。 <br><br> ![自動ユーザー プロビジョニングを有効にします。](./media/active-directory-saas-box-tutorial/IC769546.png "Enable automatic user provisioning")


4. クリックして **ボックスへのアクセス許可** この操作を承認し、Azure 管理ポータルに戻ります。 <br><br> ![自動ユーザー プロビジョニングを有効にします。](./media/active-directory-saas-box-tutorial/IC769549.png "Enable automatic user provisioning")

5. 構成を完了するには、完了ボタンをクリックします。 <br><br> ![自動ユーザー プロビジョニングの有効化](./media/active-directory-saas-box-tutorial/IC769551.png "Enable automatic user provisioning")



##ユーザーの割り当て
  
構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

###ユーザーを Box に割り当てるには、次の手順を実行します。

1. Azure AD ポータルで、テスト アカウントを作成します。

2. * * ボックス * * アプリケーション統合ページで、をクリックして **ユーザーの割り当て**します。 <br><br> ![ユーザーの割り当て](./media/active-directory-saas-box-tutorial/IC769552.png "Assign users")

3.  テスト ユーザーを選択して [ **割り当てる**, 、順にクリック **はい** して割り当てを確認します。 <br><br> ![[はい]](./media/active-directory-saas-box-tutorial/IC767830.png "Yes")
  

10 分間待機し、アカウントが Box に同期されたことを確認します。

最初の検証手順として、Azure 管理ポータルの Box アプリケーション統合ページの D でダッシュボードをクリックして、プロビジョニングの状態を確認できます。

<br><br> ![ダッシュボード](./media/active-directory-saas-box-tutorial/IC769553.png "Dashboard")

正常に完了したユーザー プロビジョニング サイクルは、関連する状態で示されます。

<br><br> ![統合状態](./media/active-directory-saas-box-tutorial/IC769555.png "Integration status")


Box テナントでは同期済みユーザーは、「 **マネージ ユーザー** で、 **Admin Console**します。

<br><br> ![統合状態](./media/active-directory-saas-box-tutorial/IC769556.png "Integration status")


## その他のリソース

* [SaaS アプリと Azure Active Directory を統合する方法に関するチュートリアルの一覧](active-directory-saas-tutorial-list.md)
* [Azure Active Directory のアプリケーション アクセスとシングル サインオンとは](active-directory-appssoaccess-whatis.md)
