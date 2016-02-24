<properties
   pageTitle="チュートリアル: Azure Active Directory と NetSuite の統合 | Microsoft Azure"
   description="Azure Active Directory で NetSuite を使用して、シングル サインオンを有効にする方法、プロビジョニングを自動化する方法などについて説明します。"
   services="active-directory"
   documentationCenter=""
   authors="liviodlc"
   manager="TerryLanfear"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="10/20/2015"
   ms.author="liviodlc"/>

#チュートリアル: Azure Active Directory と NetSuite を統合する方法

このチュートリアルでは、Azure Active Directory (Azure AD) に NetSuite 環境を接続する方法を説明します。 NetSuite へのシングル サインオンを構成する方法、ユーザー プロビジョニングの自動化を有効にする方法、NetSuite へのアクセス権をユーザーに割り当てる方法について説明します。 

##前提条件

1. Azure Active Directory にアクセスする、 [Azure 管理ポータル](https://manage.windowsazure.com), 、有効な Azure サブスクリプションが必要です。

2. に対する管理アクセス権が必要な [NetSuite](http://www.netsuite.com/portal/home.shtml) サブスクリプションです。 どちらのサービスにも無料試用版のアカウントを使用できます。

##手順 1. NetSuite をディレクトリに追加する

1.  [Azure 管理ポータル](https://manage.windowsazure.com), 、左側のナビゲーション ウィンドウで、次のようにクリックします。 **Active Directory**します。

    ![Select Active Directory from the left navigation pane.][0]

2.  **ディレクトリ** 一覧で、NetSuite を追加するには、ディレクトリを選択します。

3. をクリックして **アプリケーション** 上部のメニュー。

    ![Click on Applications.][1]

4. クリックして **追加** ページの下部にあります。

    ![Click Add to add a new application.][2]

5.  **実行する操作** ダイアログ ボックスで、をクリックして **ギャラリーからアプリケーションを追加**します。

    ![Click Add an application from the gallery.][3]

6.  **検索ボックス**, 、型 **NetSuite**します。 選択し、 **NetSuite** 結果、およびクリックから **完了** アプリケーションを追加します。

    ![Add NetSuite.][4]

7. これで、NetSuite の [クイック スタート] ページが表示されます。

    ![NetSuite's Quick Start page in Azure AD][5]

##手順 2. シングル サインオンを有効にする

1. Azure ad で、NetSuite の [クイック スタート] ページをクリックして、 **でのシングル サインオンを構成する** ] ボタンをクリックします。

    ![The configure single sign-on button][6]

2. ダイアログが開き、ユーザーが NetSuite にサインオンする方法についてたずねる画面が表示されます。 選択 **Azure AD シングル サインオン**, 、] をクリックし、 **次**します。

    ![Select Azure AD Single Sign-On][7]

    > [AZURE.NOTE] 詳細については、さまざまなシングル サインオン オプション、 [ここをクリックして](../active-directory-appssoaccess-whatis/#how-does-single-sign-on-with-azure-active-directory-work)

3.  **アプリケーション設定の構成** ] ページの **応答 URL** フィールドに、次の形式のいずれかを使用して、NetSuite のテナント URL を入力します。
    - `https://<tenant-name>.netsuite.com/saml2/acs`
    - `https://<tenant-name>.na1.netsuite.com/saml2/acs`
    - `https://<tenant-name>.na2.netsuite.com/saml2/acs`
    - `https://<tenant-name>.sandbox.netsuite.com/saml2/acs`
    - `https://<tenant-name>.na1.sandbox.netsuite.com/saml2/acs`
    - `https://<tenant-name>.na2.sandbox.netsuite.com/saml2/acs`

    ![Type in your tenant URL][8]

4.  **NetSuite でのシングル サインオンの構成** ] ページで、をクリックして **メタデータのダウンロード**, 、お使いのコンピューターにローカルで証明書ファイルを保存します。

    ![Download the metadata.][9]

5. ブラウザーで新しいタブを開き、Netsuite の会社のサイトに管理者としてサインインします。

6. ページの上部にあるツールバー [ **セットアップ**, 、クリックして **セットアップ マネージャー**します。

    ![Go to Setup Manager][10]

7.  **セットアップ タスク** 一覧で、[ **統合**します。

    ![Go to Integration][11]

8.  **Manage Authentication** ] をクリックして **SAML シングル サインオン**します。

    ![Go to SAML Single Sign-on][12]

9.  **SAML Setup** ] ページで、次の手順を実行します。

    - Azure Active Directory にコピー、 **リモート ログイン URL** 値し、全体をコピーして、 **Identity Provider Login Page** フィールド NetSuite にします。

        ![The SAML Setup page.][13]

    - NetSuite で、次のように選択します。 **Primary Authentication Method**します。

    - フィールド **SAMLV2 Identity Provider Metadata**, [ **Upload IDP Metadata File**します。 クリックし、 **参照** 手順 4. でダウンロードしたメタデータ ファイルをアップロードします。

        ![Upload the metadata][16]

    - クリックして **送信**します。

9. Azure AD でシングル サインオンの構成確認のチェック ボックスをオンにして、NetSuite にアップロードした証明書を有効にします。 クリックして **次**します。

    ![Check the confirmation checkbox][14]

10. ダイアログの最後のページでは、このシングル サインオン構成のメンテナンスに関連するエラーと警告の電子メール通知を受信する場合、電子メール アドレスを入力します。 

    ![Type in your email address.][15]

11. クリックして **完了** ダイアログを閉じます。 次に、[上] をクリックして **属性** 、ページの上部にあります。

    ![Click on Attributes.][17]

12. をクリックして **ユーザー属性の追加**します。

    ![Click on Add User Attribute.][18]

13.  **属性名** フィールドを入力 `account`します。  **属性値** フィールドを NetSuite アカウント ID を入力 アカウント ID を確認する方法を次に示します。

    ![Add your NetSuite Account ID.][19]

    - NetSuite で、をクリックして **セットアップ** 上部のナビゲーション メニューからです。
    - をクリックして、 **セットアップ タスク** 左側のナビゲーション] メニューのセクション、 **統合** セクションし、をクリックして **Web Services preferences]**します。
    - NetSuite アカウント ID をコピーして貼り付けます、 **属性値** フィールドに貼り付けます。

        ![Get your account ID][20]

14. Azure AD でクリックして **完了** SAML 属性の追加を完了します。 クリックして **変更を適用** 下部のメニューでします。

    ![変更を保存します。][21]

15. ユーザーは NetSuite にシングル サインオンする前に、まず、NetSuite で適切なアクセス許可が割り当てられている必要があります。 次の手順に従って、アクセス許可を割り当てます。

    - 上部のナビゲーション メニューをクリックして **セットアップ**, 、クリックして **セットアップ マネージャー**します。

        ![Go to Setup Manager][10]

    - 左側のナビゲーション メニューでクリックして **Users/roles**, 、] をクリックし、 **ロールの管理**します。

        ![Go to Manage Roles][22]

    - クリックして **新しいロール**します。

    - 入力、 **名前** する新しいロールを選択、 **シングル サインオンのみ** チェック ボックスをオンします。

        ![Name the new role.][23]

    - クリックして **保存**します。

    - 上部にあるメニュー [ **権限**します。 クリックし、 **セットアップ**します。

        ![Go to Permissions][24]

    - 選択 **Set Up SAM Single サインオン**, 、クリックして **追加**します。

    - クリックして **保存**します。

    - 上部のナビゲーション メニューをクリックして **セットアップ**, 、クリックして **セットアップ マネージャー**します。

        ![Go to Setup Manager][10]

    - 左側のナビゲーション メニューでクリックして **Users/roles**, 、] をクリックし、 **ユーザーの管理**します。

        ![Go to Manage Users][25]

    - テスト ユーザーを選択します。 クリックし、 **編集**します。

        ![Go to Manage Users][26]

    - Roles] ダイアログ ボックスで、[ロールを作成し、をクリックして選択 **追加**します。

        ![Go to Manage Users][27]

    - クリックして **保存**します。

19. 構成をテストするには、以下の「セクションを参照してください。 [NetSuite ユーザーを割り当てる](#step-4-assign-users-to-netsuite)します。

##手順 3. 自動化されたユーザー プロビジョニングを有効にする

> [AZURE.NOTE] 既定では、プロビジョニングされたユーザーを NetSuite 環境のルートの支社に追加されます。

1. Azure Active Directory で、NetSuite の [クイック スタート] ページをクリックします **ユーザー プロビジョニングの構成**します。

    ![Configure user provisioning][28]

2. 開く] ダイアログで、NetSuite の管理者の資格情報の入力の [ **次**します。

    ![Type in your NetSuite admin credentials.][29]

3. プロビジョニング エラーの通知を受け取る場合は、確認ページで電子メール アドレスを入力します。

    ![Confirm.][30]

4. クリックして **完了** ダイアログを閉じます。

##手順 4. NetSuite にユーザーを割り当てる

1. 構成をテストするために、ディレクトリに新しいテスト アカウントを作成します。

2. NetSuite の [クイック スタート] ページで、をクリックして、 **ユーザーを割り当てる** ] ボタンをクリックします。

    ![Click on Assign Users][31]

3. テスト ユーザーを選択し、クリックして、 **を割り当てる** 、画面の下部にあるボタンをクリックします。

 - 自動化されたユーザー プロビジョニングを有効にしていない場合は、次のような確認メッセージが表示されます。

        ![Confirm the assignment.][32]

 - ユーザー プロビジョニングの自動化を有効にしている場合は、ユーザーに必要な NetSuite でのロールの種類の定義を要求するプロンプトが表示されます。 新しくプロビジョニングされたユーザーは、数分後に NetSuite 環境に表示されます。

4. シングル サインオン設定をテストするには、アクセス パネルを開きます [https://myapps.microsoft.com](https://myapps.microsoft.com/), テスト アカウントにサインインしをクリックして **NetSuite**します。

[AZURE.INCLUDE [saas-toc](../../includes/active-directory-saas-toc.md)]

[0]: ./media/active-directory-saas-netsuite-tutorial/azure-active-directory.png
[1]: ./media/active-directory-saas-netsuite-tutorial/applications-tab.png
[2]: ./media/active-directory-saas-netsuite-tutorial/add-app.png
[3]: ./media/active-directory-saas-netsuite-tutorial/add-app-gallery.png
[4]: ./media/active-directory-saas-netsuite-tutorial/add-netsuite.png
[5]: ./media/active-directory-saas-netsuite-tutorial/quick-start-netsuite.png
[6]: ./media/active-directory-saas-netsuite-tutorial/config-sso.png
[7]: ./media/active-directory-saas-netsuite-tutorial/sso-netsuite.png
[8]: ./media/active-directory-saas-netsuite-tutorial/sso-config-netsuite.png
[9]: ./media/active-directory-saas-netsuite-tutorial/config-sso-netsuite.png
[10]: ./media/active-directory-saas-netsuite-tutorial/ns-setup.png
[11]: ./media/active-directory-saas-netsuite-tutorial/ns-integration.png
[12]: ./media/active-directory-saas-netsuite-tutorial/ns-saml.png
[13]: ./media/active-directory-saas-netsuite-tutorial/ns-saml-setup.png
[14]: ./media/active-directory-saas-netsuite-tutorial/ns-sso-confirm.png
[15]: ./media/active-directory-saas-netsuite-tutorial/sso-email.png
[16]: ./media/active-directory-saas-netsuite-tutorial/ns-sso-setup.png
[17]: ./media/active-directory-saas-netsuite-tutorial/ns-attributes.png
[18]: ./media/active-directory-saas-netsuite-tutorial/ns-add-attribute.png
[19]: ./media/active-directory-saas-netsuite-tutorial/ns-add-account.png
[20]: ./media/active-directory-saas-netsuite-tutorial/ns-account-id.png
[21]: ./media/active-directory-saas-netsuite-tutorial/ns-save-saml.png
[22]: ./media/active-directory-saas-netsuite-tutorial/ns-manage-roles.png
[23]: ./media/active-directory-saas-netsuite-tutorial/ns-new-role.png
[24]: ./media/active-directory-saas-netsuite-tutorial/ns-sso.png
[25]: ./media/active-directory-saas-netsuite-tutorial/ns-manage-users.png
[26]: ./media/active-directory-saas-netsuite-tutorial/ns-edit-user.png
[27]: ./media/active-directory-saas-netsuite-tutorial/ns-add-role.png
[28]: ./media/active-directory-saas-netsuite-tutorial/netsuite-provisioning.png
[29]: ./media/active-directory-saas-netsuite-tutorial/ns-creds.png
[30]: ./media/active-directory-saas-netsuite-tutorial/ns-confirm.png
[31]: ./media/active-directory-saas-netsuite-tutorial/assign-users.png
[32]: ./media/active-directory-saas-netsuite-tutorial/assign-confirm.png

