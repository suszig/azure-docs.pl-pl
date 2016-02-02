<properties 
    pageTitle="チュートリアル: 受信同期のための Workday の構成 | Microsoft Azure" 
    description="Workday を Azure Active Directory の ID データのソースとして使用する方法について説明します。" 
    services="active-directory" 
    authors="MarkusVi"  
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

このチュートリアルでは、Workday から Azure AD にユーザーをインポートするために、Workday と Azure AD で実行する必要のある手順について説明します。

このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure AD Premium サブスクリプション
-   Workday のテナント

このチュートリアルで説明するシナリオは、次の要素で構成されています。

1. Workday のアプリケーション統合の有効化

2. 統合システム ユーザーの作成

3. セキュリティ グループの作成

4. セキュリティ グループへの統合システム ユーザーの割り当て

5. セキュリティ グループ オプションの構成

6. セキュリティ ポリシーの変更のアクティブ化

7. Azure AD でのユーザー インポートの構成



## Workday のアプリケーション統合の有効化

このセクションでは、Workday のアプリケーション統合を有効にする方法について説明します。

### Workday のアプリケーション統合を有効にするには、次の手順を実行します。

1.  Azure 管理ポータルの左側のナビゲーション ウィンドウで、**[Active Directory]** をクリックします。

    ![Active Directory](./media/active-directory-saas-workday-inbound-tutorial/IC700993.png "Active Directory")

2.  **[ディレクトリ]** の一覧から、ディレクトリ統合を有効にするディレクトリを選択します。

3.  アプリケーション ビューを開くには、ディレクトリ ビューでトップ メニューの **[アプリケーション]** をクリックします。

    ![アプリケーション](./media/active-directory-saas-workday-inbound-tutorial/IC700994.png "Applications")

4.  ページの下部にある **[追加]** をクリックします。

    ![アプリケーションの追加](./media/active-directory-saas-workday-inbound-tutorial/IC749321.png "Add application")

5. 検索ボックスに、「**Workday**」と入力します。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-workday-inbound-tutorial/IC701021.png "Add an application from gallerry")

6. 結果ウィンドウで [Workday] を選択し、[完了] をクリックしてアプリケーションを追加します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-workday-inbound-tutorial/IC701022.png "Application gallery")





## 統合システム ユーザーの作成

1. **Workday Workbench**, 、入力して検索ボックスにユーザーを作成し、クリックして **Create Integration System User**します。<br><br>   ![ユーザーを作成します。](./media/active-directory-saas-workday-inbound-tutorial/IC750979.png "Create user")

2. 新しい統合システム ユーザーのユーザー名とパスワードを指定して、**統合システム ユーザーを作成**する作業を完了します。このユーザーがプログラムを使用してログオンするためは、Require New Password at Next Sign In] はオフのままにします。 <br>
[セッション タイムアウト (分)] は既定値の 0 のままにしておきます。これにより、ユーザーのセッションが有効期限前にタイムアウトするのを防ぎます。<br><br>   ![統合システム ユーザーを作成します。](./media/active-directory-saas-workday-inbound-tutorial/IC750980.png "Create Integration System User")





## セキュリティ グループの作成

このチュートリアルで概説したシナリオの場合、制約のない統合システム セキュリティ グループを作成し、それにユーザーを割り当てる必要があります。


1. 入力検索ボックスにセキュリティ グループを作成し、クリックして **Create Security Group**します。<br><br>   ![CreateSecurity グループ](./media/active-directory-saas-workday-inbound-tutorial/IC750981.png "CreateSecurity Group")

2. セキュリティ グループの作成タスクを完了します。[テナント セキュリティ グループの種類] ドロップダウンから [統合システム セキュリティ グループ - 制約なし] を選択し、メンバーが明示的に追加されるセキュリティ グループを作成します。<br><br>   ![CreateSecurity グループ](./media/active-directory-saas-workday-inbound-tutorial/IC750982.png "CreateSecurity Group")




## セキュリティ グループへの統合システム ユーザーの割り当て

1. 検索ボックスにセキュリティ グループの編集を入力し、クリックして **Edit Security Group**します。<br><br>   ![セキュリティ グループの編集](./media/active-directory-saas-workday-inbound-tutorial/IC750983.png "Edit Security Group")

2. 新しい統合セキュリティ グループを名前で検索し、選択します。<br><br> ![セキュリティ グループの編集](./media/active-directory-saas-workday-inbound-tutorial/IC750984.png "Edit Security Group")
セキュリティ グループの編集

3. 新しいセキュリティ グループに新しい統合システム ユーザーを追加します。<br><br>   ![システム セキュリティ グループ](./media/active-directory-saas-workday-inbound-tutorial/IC750985.png "System Security Group")




## セキュリティ グループ オプションの構成

この手順では、新しいセキュリティ グループに対して、次のドメイン セキュリティ ポリシーによってセキュリティ保護されるオブジェクトへの **Get** および **Put** 操作の権限を付与します。

- 外部アカウントのプロビジョニング

- Worker Data: Public Worker Reports

- Worker Data: All Positions

- Worker Data: Current Staffing Information

- Worker Data: Business Title on Worker Profile


1. 検索ボックスにドメインのセキュリティ ポリシーを入力し、[機能領域のドメイン セキュリティ ポリシー] リンクをクリックします。<br><br>   ![ドメイン セキュリティ ポリシー](./media/active-directory-saas-workday-inbound-tutorial/IC750986.png "Domain Security Policies")

2. システムを検索し、**システム**の機能領域を選択します。**[OK]** をクリックします。<br><br>   ![ドメイン セキュリティ ポリシー](./media/active-directory-saas-workday-inbound-tutorial/IC750987.png "Domain Security Policies")

3. [システム] 機能領域のセキュリティ ポリシーの一覧で、[Security Administration] を展開し、ドメイン セキュリティ ポリシーの [外部アカウントのプロビジョニング] を選択します。<br><br>   ![ドメイン セキュリティ ポリシー](./media/active-directory-saas-workday-inbound-tutorial/IC750988.png "Domain Security Policies")

4. をクリックして **アクセス許可の編集**, 、[、 **アクセス許可の編集**ダイアログ ページを持つセキュリティ グループの一覧に新しいセキュリティ グループを追加 **取得** と **配置** 統合のアクセス許可。<br><br>   ![アクセス許可を編集します。](./media/active-directory-saas-workday-inbound-tutorial/IC750989.png "Edit Permission")

5. 機能領域を選択するための画面に戻るには、上、手順 1、および今回は、staffing を検索 Staffing] 機能領域を選択して繰り返しを **OK**します。<br><br> ![ドメイン セキュリティ ポリシー](./media/active-directory-saas-workday-inbound-tutorial/IC750990.png "Domain Security Policies")

6. スタッフ機能領域のセキュリティ ポリシーの一覧で、[ Worker Data: Staffing] を展開し、残りの各セキュリティ ポリシーに対して上記の手順 4. を繰り返します。

     - Worker Data: Public Worker Reports

     - Worker Data: All Positions

     - Worker Data: Current Staffing Information

     - Worker Data: Business Title on Worker Profile


<br><br>   ![ドメイン セキュリティ ポリシー](./media/active-directory-saas-workday-inbound-tutorial/IC750991.png "Domain Security Policies")







## セキュリティ ポリシーの変更のアクティブ化

1. 検索ボックスに「アクティブ化」と入力し、[保留中のセキュリティ ポリシーの変更をアクティブ化] をクリックします。<br><br>   ![アクティブ化します。](./media/active-directory-saas-workday-inbound-tutorial/IC750992.png "Activate")

2. クリックして、監査目的でコメントを入力して、セキュリティ ポリシーの保留中の変更のアクティブ化タスクを開始 **OK**します。<br><br>   ![保留中のセキュリティをアクティブ化します。](./media/active-directory-saas-workday-inbound-tutorial/IC750993.png "Activate Pending Security")

3. 確認] で、ラベル付けされているチェック ボックスをオンに次の画面でタスクを完了し、 **OK**します。<br><br>   ![保留中のセキュリティをアクティブ化します。](./media/active-directory-saas-workday-inbound-tutorial/IC750994.png "Activate Pending Security")





## Azure AD でのユーザー インポートの構成

このセクションでは、Azure AD を構成して Workday からユーザーをインポートする方法について説明します。


### Azure AD でユーザーのインポートを構成するには、次の手順を実行します。

1. **Workday** アプリケーション統合ページで、**[ユーザー インポートの構成]** をクリックして、**[プロビジョニングの構成]** ダイアログを開きます。

2.**設定と管理者資格情報** ] ページで、次の手順を実行して順にクリックして **次**: <br><br>   ![設定と管理者資格情報](./media/active-directory-saas-workday-inbound-tutorial/IC750995.png "Settings and admin credentials")

     2.1. Workday 管理者ユーザー名] ボックスには、作成する方法で作成したユーザーの名前を入力、統合システム ユーザーを参照します。

     2.2. Workday 管理パスワード] ボックスで、作成する方法で作成したユーザーのパスワードを入力、統合システム ユーザーを参照します。

     2.3. Workday テナント URL] ボックスに、URL または Workday テナントを入力します。

3. **[接続テスト]** ページで、**[テスト開始]** をクリックして接続を確認し、**[次へ]** をクリックします。<br><br>   ![接続をテストします。](./media/active-directory-saas-workday-inbound-tutorial/IC750996.png "Test connection")

4. **プロビジョニング** オプション] ページでをクリックして **次**します。<br><br>   ![プロビジョニング オプション](./media/active-directory-saas-workday-inbound-tutorial/IC750997.png "Provisioning options")

5. **[プロビジョニングの開始]** ダイアログで、**[完了]** をクリックします。<br><br>   ![プロビジョニングを開始します。](./media/active-directory-saas-workday-inbound-tutorial/IC750998.png "Start provisioning")


これで、**[ユーザー]** セクションに移動して、Workday ユーザーがインポートされたかどうかを確認できます。



## その他のリソース

* [Azure Active Directory による SaaS アプリを統合する方法に関するチュートリアルの一覧](active-directory-saas-tutorial-list.md)
* [アプリケーション アクセスと Azure Active Directory でのシングル サインオンとは](active-directory-appssoaccess-whatis.md)





