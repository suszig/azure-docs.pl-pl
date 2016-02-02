<properties 
    pageTitle="チュートリアル: 受信同期のための Workday の構成 | Microsoft Azure" 
    description="Azure Active Directory でシングル サインオン、自動化されたプロビジョニング、および詳細を有効にする入力方向の同期を使用する方法を説明します。" 
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


# チュートリアル: 受信同期のための Workday の構成

>[AZURE.NOTE]Azure AD のワールドワイド インスタンスを使用している中国のお客様は、Azure Active Directory (AD) Premium を利用できます。    
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

## Workday のアプリケーション統合の有効化

このセクションでは、Workday のアプリケーション統合を有効にする方法について説明します。

### Workday のアプリケーション統合を有効にするには、次の手順を実行します。

1.  Azure 管理ポータルの左側のナビゲーション ウィンドウで、**[Active Directory]** をクリックします。

    ![Active Directory](./media/active-directory-saas-inbound-synchronization-tutorial/IC700993.png "Active Directory")

2.  **[ディレクトリ]** の一覧から、ディレクトリ統合を有効にするディレクトリを選択します。

3.  アプリケーション ビューを開くには、ディレクトリ ビューでトップ メニューの **[アプリケーション]** をクリックします。

    ![アプリケーション](./media/active-directory-saas-inbound-synchronization-tutorial/IC700994.png "Applications")

4.  **アプリケーション ギャラリー**を開くには、**[アプリケーションの追加]**、**[組織で使用するアプリケーションを追加]** の順にクリックします。

    ![どの操作を行いますか。](./media/active-directory-saas-inbound-synchronization-tutorial/IC700995.png "What do you want to do?")

5.  **検索ボックス**に、「**Workday**」と入力します。

    ![Workday](./media/active-directory-saas-inbound-synchronization-tutorial/IC701021.png "Workday")

6.  結果ウィンドウで **[Workday]** を選択し、**[完了]** をクリックしてアプリケーションを追加します。

    ![Workday](./media/active-directory-saas-inbound-synchronization-tutorial/IC701022.png "Workday")

## 統合システム ユーザーの作成

1.  **Workday Workbench**, 、入力 **ユーザーを作成** ] リンクをクリックして [検索] ボックスで **Create Integration System User**します。

    ![ユーザーの作成](./media/active-directory-saas-inbound-synchronization-tutorial/IC750979.png "create user")

2.  新しい統合システム ユーザーのユーザー名とパスワードを指定して、統合システム ユーザーを作成する作業を完了します。 このユーザーがプログラムを使用してログオンするためは、Require New Password at Next Sign In] はオフのままにします。    
    セッション タイムアウトの分数を既定値は、ユーザーのセッション タイムアウトにならないように途中で 0 のままにします。

    ![統合システム ユーザーの作成](./media/active-directory-saas-inbound-synchronization-tutorial/IC750980.png "Create Integration System User")

## セキュリティ グループの作成

このチュートリアルで概説したシナリオの場合、制約のない統合システム セキュリティ グループを作成し、それにユーザーを割り当てる必要があります。

1.  入力して検索ボックスにセキュリティ グループを作成し、セキュリティ グループの作成] リンクをクリックします。

    ![CreateSecurity グループ](./media/active-directory-saas-inbound-synchronization-tutorial/IC750981.png "CreateSecurity Group")

2.  セキュリティ グループの作成タスクを完了します。 統合システム セキュリティ グループの選択-Tenanted Security Group の種類] ドロップダウンから無制限のメンバーが明示的に追加するセキュリティ グループを作成します。

    ![CreateSecurity グループ](./media/active-directory-saas-inbound-synchronization-tutorial/IC750982.png "CreateSecurity Group")

## セキュリティ グループへの統合システム ユーザーの割り当て

1.  検索ボックスにセキュリティ グループの編集を入力し、リンクをクリックして  **Edit Security Group**します。

    ![セキュリティ グループの編集](./media/active-directory-saas-inbound-synchronization-tutorial/IC750983.png "Edit Security Group")

2.  新しい統合セキュリティ グループを名前で検索し、選択します。

    ![セキュリティ グループの編集](./media/active-directory-saas-inbound-synchronization-tutorial/IC750984.png "Edit Security Group")

3.  新しいセキュリティ グループに新しい統合システム ユーザーを追加します。
 

![システム セキュリティ グループ](./media/active-directory-saas-inbound-synchronization-tutorial/IC750985.png "System Security Group")

## セキュリティ グループ オプションの構成

この手順では、次のドメイン セキュリティ ポリシーによって保護されているオブジェクトに対する Get および Put 操作の新しいセキュリティ グループの権限を付与します。

-   外部アカウントのプロビジョニング
-   Worker Data: Public Worker Reports
-   Worker Data: All Positions
-   Worker Data: Current Staffing Information
-   Worker Data: Business Title on Worker Profile

&nbsp;

1.  検索ボックスにドメイン セキュリティ ポリシーを入力し、機能領域のドメイン セキュリティ ポリシー] リンクをクリックします。

    ![ドメイン セキュリティ ポリシー](./media/active-directory-saas-inbound-synchronization-tutorial/IC750986.png "Domain Security Policies")

2.  システムを検索し、システムの機能領域を選択します。 ラベル、ボタン、[ok] をクリックします。

    ![ドメイン セキュリティ ポリシー](./media/active-directory-saas-inbound-synchronization-tutorial/IC750987.png "Domain Security Policies")

3.  システムの機能領域のセキュリティ ポリシーの一覧では、セキュリティの管理] を展開し、外部アカウントの準備、ドメイン セキュリティ ポリシーを選択します。

    ![ドメイン セキュリティ ポリシー](./media/active-directory-saas-inbound-synchronization-tutorial/IC750988.png "Domain Security Policies")

4.  アクセス許可の編集] ボタンをクリックし、次に、アクセス許可の編集画面で、Get および Put の統合アクセス許可を持つセキュリティ グループの一覧に新しいセキュリティ グループを追加します。

    ![アクセス許可の編集](./media/active-directory-saas-inbound-synchronization-tutorial/IC750989.png "Edit Permission")

5.  機能領域を選択するための画面に戻るには、上、手順 1、および今回は、staffing を検索が Staffing] 機能領域を選択し、[ok] というラベルのボタンをクリックして繰り返します。

    ![ドメイン セキュリティ ポリシー](./media/active-directory-saas-inbound-synchronization-tutorial/IC750990.png "Domain Security Policies")

6.  Staffing] 機能領域のセキュリティ ポリシーの一覧で展開 Worker Data: スタッフ、および以下のセキュリティ ポリシーの残りの各手順 4 を繰り返します。

    -   Worker Data: Public Worker Reports
    -   Worker Data: All Positions
    -   Worker Data: Current Staffing Information
    -   Worker Data: Business Title on Worker Profile

    ![ドメイン セキュリティ ポリシー](./media/active-directory-saas-inbound-synchronization-tutorial/IC750991.png "Domain Security Policies")

## セキュリティ ポリシーの変更のアクティブ化

1.  入力して検索] ボックスをアクティブ化し、[Activate Pending Security Policy Changes] リンクをクリックします。

    ![アクティブ化](./media/active-directory-saas-inbound-synchronization-tutorial/IC750992.png "Activate")

2.   監査目的でコメントを入力して、ラベル、ボタン、[ok] をクリックすると、セキュリティ ポリシーの保留中の変更のアクティブ化タスクを開始します。


    ![保留中のセキュリティのアクティブ化](./media/active-directory-saas-inbound-synchronization-tutorial/IC750993.png "Activate Pending Security")

3.  確認して、というラベルのボタン、[ok] をクリックするとというラベルが付いたチェック ボックスをオンには、次の画面でタスクを完了します。

    ![保留中のセキュリティのアクティブ化](./media/active-directory-saas-inbound-synchronization-tutorial/IC750994.png "Activate Pending Security")

## Microsoft Azure AD でのユーザー インポートの構成

このセクションでは、Microsoft Azure AD を構成して Workday からユーザーをインポートする方法を概説します。

### Microsoft Azure AD でユーザーのインポートを構成するには、次の手順を実行します。

1.  **Workday** アプリケーション統合ページで、**[ユーザー インポートの構成]** をクリックして、**[プロビジョニングの構成]** ダイアログを開きます。

2.  **[設定と管理者資格情報]** ページで次の手順を実行し、[次へ] をクリックします。

    ![設定と管理者の資格情報](./media/active-directory-saas-inbound-synchronization-tutorial/IC750995.png "Settings and admin credentials")

    1.  **Workday 管理者ユーザー名** ] ボックスで作成したユーザーの名前を入力、 [統合システム ユーザーを作成する](https://msdn.microsoft.com/library/azure/Dn762434.aspx#BKMK_CreateUser) セクションです。
    2.  **Workday 管理者のパスワード** ] ボックスで作成したユーザーのパスワードを入力、 [統合システム ユーザーを作成する](https://msdn.microsoft.com/library/azure/Dn762434.aspx#BKMK_CreateUser) セクションです。
    3.  **[WORKDAY テナント URL]** テキストボックスに、URL または Workday テナントを入力します。

3.  **[接続テスト]** ページで、**[テスト開始]** をクリックして接続を確認し、**[次へ]** をクリックします。

    ![接続テスト](./media/active-directory-saas-inbound-synchronization-tutorial/IC750996.png "Test connection")

4.  **[プロビジョニング オプション]** ページで、**[次へ]** をクリックします。

    ![プロビジョニングのオプション](./media/active-directory-saas-inbound-synchronization-tutorial/IC750997.png "Provisioning options")

5.  **[プロビジョニングの開始]** ダイアログで、**[完了]** をクリックします。

    ![プロビジョニングの開始](./media/active-directory-saas-inbound-synchronization-tutorial/IC750998.png "Start provisioning")

これで、**[ユーザー]** セクションに移動して、Workday ユーザーがインポートされたかどうかを確認できます。





