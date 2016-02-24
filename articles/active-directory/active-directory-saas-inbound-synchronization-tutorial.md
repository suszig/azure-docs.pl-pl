<properties 
    pageTitle="チュートリアル: 受信同期のための Workday の構成 | Microsoft Azure" 
    description="Azure Active Directory で受信同期を使用して、シングル サインオンや自動プロビジョニングなどを有効にする方法について説明します。" 
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

#チュートリアル: 受信同期のための Workday の構成
>[AZURE.NOTE]Azure Active Directory (AD) Premium は、Azure AD のワールドワイド インスタンスを使用している中国のお客様に使用できます。    
中国の 21Vianet が運営する Microsoft Azure サービスでは、Azure AD Premium は現在サポートされていません。    

このチュートリアルでは、Workday から Microsoft Azure AD にユーザーをインポートするために、Workday と Microsoft Azure AD で実行する必要のある手順について説明します。    
 このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。  

-   有効な Azure サブスクリプション  
-   Workday のテナント  

このチュートリアルで説明するシナリオは、次の要素で構成されています。  

1.  Workday のアプリケーション統合の有効化  
2.  統合システム ユーザーの作成  
3.  セキュリティ グループの作成  
4.  セキュリティ グループへの統合システム ユーザーの割り当て  
5.  セキュリティ グループ オプションの構成  
6.  セキュリティ ポリシーの変更のアクティブ化  
7.  Microsoft Azure AD でのユーザー インポートの構成  

##Workday のアプリケーション統合の有効化

このセクションでは、Workday のアプリケーション統合を有効にする方法について説明します。    

###Workday のアプリケーション統合を有効にするには、次の手順を実行します。

1.  左側のナビゲーション ウィンドウで、Azure 管理ポータルで、クリックして **Active Directory**します。    

    ![Active Directory](./media/active-directory-saas-inbound-synchronization-tutorial/IC700993.png "Active Directory")  

2.   **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。    

3.  ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。    

    ![アプリケーション](./media/active-directory-saas-inbound-synchronization-tutorial/IC700994.png "Applications")  

4.  開くには、 **アプリケーション ギャラリー**, 、] をクリックして **アプリの追加**, 、クリックして **組織で使用するためのアプリケーションの追加**します。    

    ![どの操作を行いますか。](./media/active-directory-saas-inbound-synchronization-tutorial/IC700995.png "What do you want to do?")  

5.   **検索ボックス**, 、型 **Workday**します。    

    ![Workday](./media/active-directory-saas-inbound-synchronization-tutorial/IC701021.png "Workday")  

6.  結果ウィンドウで [ **Workday**, 、クリックして **完了** アプリケーションを追加します。    

    ![Workday](./media/active-directory-saas-inbound-synchronization-tutorial/IC701022.png "Workday")  

##統合システム ユーザーの作成

1.   **Workday Workbench**, 、入力 **ユーザーを作成** ] リンクをクリックして [検索] ボックスで **Create Integration System User**します。     

    ![create user](./media/active-directory-saas-inbound-synchronization-tutorial/IC750979.png "create user")  

2.  新しい統合システム ユーザーのユーザー名とパスワードを指定して、統合システム ユーザーを作成する作業を完了します。  このユーザーはプログラムを使用してログオンするため、[次回のサインイン時に新しいパスワードを要求する] オプションはオフのままにしておきます。    
    [セッション タイムアウト (分)] は既定値の 0 のままにしておきます。これにより、ユーザーのセッションが有効期限前にタイムアウトするのを防ぎます。    

    ![統合システム ユーザーの作成](./media/active-directory-saas-inbound-synchronization-tutorial/IC750980.png "Create Integration System User")  

##セキュリティ グループの作成

このチュートリアルで概説したシナリオの場合、制約のない統合システム セキュリティ グループを作成し、それにユーザーを割り当てる必要があります。    

1.  検索ボックスに「セキュリティ グループの作成」と入力し、[セキュリティ グループの作成] リンクをクリックします。     

    ![CreateSecurity グループ](./media/active-directory-saas-inbound-synchronization-tutorial/IC750981.png "CreateSecurity Group")  

2.  セキュリティ グループの作成タスクを完了します。  [テナント セキュリティ グループの種類] ドロップダウンから [統合システム セキュリティ グループ - 制約なし] を選択し、メンバーが明示的に追加されるセキュリティ グループを作成します。     

    ![CreateSecurity グループ](./media/active-directory-saas-inbound-synchronization-tutorial/IC750982.png "CreateSecurity Group")  

##セキュリティ グループへの統合システム ユーザーの割り当て

1.  検索ボックスにセキュリティ グループの編集を入力し、リンクをクリックして  **Edit Security Group**します。     

    ![セキュリティ グループの編集](./media/active-directory-saas-inbound-synchronization-tutorial/IC750983.png "Edit Security Group")  

2.  新しい統合セキュリティ グループを名前で検索し、選択します。    

    ![セキュリティ グループの編集](./media/active-directory-saas-inbound-synchronization-tutorial/IC750984.png "Edit Security Group")  

3.  新しいセキュリティ グループに新しい統合システム ユーザーを追加します。       

    ![システム セキュリティ グループ](./media/active-directory-saas-inbound-synchronization-tutorial/IC750985.png "System Security Group")  

##セキュリティ グループ オプションの構成

この手順では、新しいセキュリティ グループに対して、次のドメイン セキュリティ ポリシーによってセキュリティ保護されるオブジェクトへの Get および Put 操作の権限を付与します。  

-   外部アカウントのプロビジョニング  
-   Worker Data: Public Worker Reports  
-   Worker Data: All Positions  
-   Worker Data: Current Staffing Information  
-   Worker Data: Business Title on Worker Profile  

&nbsp;  

1.  検索ボックスにドメインのセキュリティ ポリシーを入力し、[機能領域のドメイン セキュリティ ポリシー] リンクをクリックします。     

    ![ドメイン セキュリティ ポリシー](./media/active-directory-saas-inbound-synchronization-tutorial/IC750986.png "Domain Security Policies")  

2.  システムを検索し、システムの機能領域を選択します。  [OK] ボタンをクリックします。     

    ![ドメイン セキュリティ ポリシー](./media/active-directory-saas-inbound-synchronization-tutorial/IC750987.png "Domain Security Policies")  

3.  [システム] 機能領域のセキュリティ ポリシーの一覧で、[Security Administration] を展開し、ドメイン セキュリティ ポリシーの [外部アカウントのプロビジョニング] を選択します。     

    ![ドメイン セキュリティ ポリシー](./media/active-directory-saas-inbound-synchronization-tutorial/IC750988.png "Domain Security Policies")  

4.  [権限の編集] ボタンをクリックし、次に [権限の編集] 画面で、Get と Put の統合アクセス権限を持つセキュリティ グループの一覧に新しいセキュリティ グループを追加します。     

    ![アクセス許可の編集](./media/active-directory-saas-inbound-synchronization-tutorial/IC750989.png "Edit Permission")  

5.  上記の手順 1. を繰り返して、機能領域を選択する画面に戻ります。今回はスタッフを検索して、[スタッフ] 機能領域を選択し、[OK] ボタンをクリックします。    

    ![ドメイン セキュリティ ポリシー](./media/active-directory-saas-inbound-synchronization-tutorial/IC750990.png "Domain Security Policies")  

6.  スタッフ機能領域のセキュリティ ポリシーの一覧で、[ Worker Data: Staffing] を展開し、残りの各セキュリティ ポリシーに対して上記の手順 4. を繰り返します。    

    -   Worker Data: Public Worker Reports  
    -   Worker Data: All Positions  
    -   Worker Data: Current Staffing Information  
    -   Worker Data: Business Title on Worker Profile    

    ![ドメイン セキュリティ ポリシー](./media/active-directory-saas-inbound-synchronization-tutorial/IC750991.png "Domain Security Policies")  

##セキュリティ ポリシーの変更のアクティブ化

1.  検索ボックスに「アクティブ化」と入力し、[保留中のセキュリティ ポリシーの変更をアクティブ化] をクリックします。    

    ![アクティブ化](./media/active-directory-saas-inbound-synchronization-tutorial/IC750992.png "Activate")  

2.   監査用のコメントを入力し、[OK] ボタンをクリックして、[保留中のセキュリティ ポリシーの変更のアクティブ化] 作業を開始します。      

    ![Activate Pending Security](./media/active-directory-saas-inbound-synchronization-tutorial/IC750993.png "Activate Pending Security")  

3.  [確認] チェックボックスをオンにして、[OK] ボタンをクリックし、次の画面で作業を完了します。     

    ![保留中のセキュリティのアクティブ化](./media/active-directory-saas-inbound-synchronization-tutorial/IC750994.png "Activate Pending Security")  

##Microsoft Azure AD でのユーザー インポートの構成

このセクションでは、Microsoft Azure AD を構成して Workday からユーザーをインポートする方法を概説します。    

###Microsoft Azure AD でユーザーのインポートを構成するには、次の手順を実行します。

1.   **Workday** アプリケーション統合ページでは、をクリックして **ユーザー インポートを構成** を開くには、 **プロビジョニングの構成** ダイアログ。    

2.   **設定と管理者資格情報** ] ページで、次の手順を実行して、[次へ] をクリックします。    

    ![設定と管理者の資格情報](./media/active-directory-saas-inbound-synchronization-tutorial/IC750995.png "Settings and admin credentials")    

    1.   **Workday 管理者ユーザー名** ] ボックスで作成したユーザーの名前を入力、 [統合システム ユーザーを作成する](https://msdn.microsoft.com/library/azure/Dn762434.aspx#BKMK_CreateUser) セクションです。    
    2.   **Workday 管理者のパスワード** ] ボックスで作成したユーザーのパスワードを入力、 [統合システム ユーザーを作成する](https://msdn.microsoft.com/library/azure/Dn762434.aspx#BKMK_CreateUser) セクションです。    
    3.   **Workday テナント URL** ] ボックスに、URL または Workday テナントを入力します。    

3.   **接続をテストする** ] ページで [ **テストの開始** を接続を確認し、をクリックして **次**します。    

    ![接続テスト](./media/active-directory-saas-inbound-synchronization-tutorial/IC750996.png "Test connection")  

4.   **プロビジョニング オプション** ] ページで [ **次**します。    

    ![プロビジョニングのオプション](./media/active-directory-saas-inbound-synchronization-tutorial/IC750997.png "Provisioning options")  

5.   **プロビジョニングを開始** ダイアログ ボックスで、をクリックして **完了**します。    

    ![プロビジョニングの開始](./media/active-directory-saas-inbound-synchronization-tutorial/IC750998.png "Start provisioning")  

今すぐに移動することができます、 **ユーザー** セクションし、Workday ユーザーがインポートされているかどうか確認します。    

