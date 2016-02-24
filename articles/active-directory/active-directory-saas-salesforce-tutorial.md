<properties
   pageTitle="チュートリアル: Azure Active Directory と Salesforce の統合 | Microsoft Azure"
   description="Azure Active Directory で Salesforce を使用してシングル サインオンや自動化されたプロビジョニングなどを有効にする方法について説明します。"
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

#チュートリアル: Azure Active Directory と Salesforce を統合する方法

このチュートリアルでは、Azure Active Directory に Salesforce 環境を接続する方法を説明します。 Salesforce へのシングル サインオンを構成する方法、自動化されたユーザー プロビジョニングを有効にする方法、Salesforce へのアクセス権をユーザーに割り当てる方法などについて説明します。

##前提条件

1. Azure Active Directory にアクセスする、 [Azure 管理ポータル](https://manage.windowsazure.com), 、有効な Azure サブスクリプションが必要です。

2. 有効なテナントが必要 [Salesforce.com](https://www.salesforce.com/)します。

> [AZURE.IMPORTANT] Salesforce.com を使用している場合 **試用** アカウント、自動化されたユーザー プロビジョニングを構成することはできません。 試用アカウントの場合、アカウントを購入するまでは、必要な API にアクセスできません。
> 
> この制限を回避するを使用して、 [無料の開発者アカウント](https://developer.salesforce.com/signup) をこのチュートリアルを完了します。

Salesforce Sandbox 環境を使用している場合を参照してください、 [Salesforce Sandbox の統合に関するチュートリアル](https://go.microsoft.com/fwLink/?LinkID=521879)します。

##ビデオ チュートリアル

以下のビデオを使用して、このチュートリアルの手順に従います。

**ビデオ チュートリアル第 1 部: シングル サインオンを有効にする方法**

> [AZURE.VIDEO integrating-salesforce-with-azure-ad-how-to-enable-single-sign-on]

**ビデオ チュートリアル第 2 部: ユーザー プロビジョニングを自動化する方法**

> [AZURE.VIDEO integrating-salesforce-with-azure-ad-how-to-automate-user-provisioning]

##手順 1. Salesforce をディレクトリに追加する

1.  [Azure 管理ポータル](https://manage.windowsazure.com), 、左側のナビゲーション ウィンドウで、次のようにクリックします。 **Active Directory**します。

    ![Select Active Directory from the left navigation pane.][0]

2.  **ディレクトリ** 一覧で、Salesforce を追加するには、ディレクトリを選択します。

3. をクリックして **アプリケーション** 上部のメニュー。

    ![Click on Applications.][1]

4. クリックして **追加** ページの下部にあります。

    ![Click Add to add a new application.][2]

5.  **実行する操作** ダイアログ ボックスで、をクリックして **ギャラリーからアプリケーションを追加**します。

    ![Click Add an application from the gallery.][3]

6.  **検索ボックス**, 、型 **Salesforce**します。 選択し、 **Salesforce** 結果、およびクリックから **完了** アプリケーションを追加します。

    ![Salesforce の追加][4]

7. これで、Salesforce の [クイック スタート] ページが表示されます。

    ![Azure AD の Salesforce の [クイック スタート] ページ][5]

##手順 2. シングル サインオンを有効にする

1. シングル サインオンを構成する前に、Salesforce 環境用のカスタム ドメインを設定し、デプロイする必要があります。 手順を実行する方法については、次を参照してください。 [ドメイン名の設定](https://help.salesforce.com/HTViewHelpDoc?id=domain_name_setup.htm&language=en_US)します。

2. Azure AD の Salesforce のクイック スタートのページ] をクリックして、 **でのシングル サインオンを構成する** ] ボタンをクリックします。

    ![The configure single sign-on button][6]

3. ダイアログが開き、ユーザーが Salesforce にサインオンする方法について質問する画面が表示されます。 選択 **Azure AD シングル サインオン**, 、] をクリックし、 **次**します。

    ![Select Azure AD Single Sign-On][7]

    > [AZURE.NOTE] 詳細については、さまざまなシングル サインオン オプション、 [ここをクリックして](../active-directory-appssoaccess-whatis/#how-does-single-sign-on-with-azure-active-directory-work)

4.  **アプリケーション設定の構成** ] ページで、記入、 **サインオン URL** の次の形式を使用して、Salesforce ドメインの URL を入力します。
 - エンタープライズ アカウント: `https://<domain>.my.salesforce.com`
 - 開発者アカウント: `https://<domain>-dev-ed.my.salesforce.com` 

    ![自分のサインオン URL を入力][8]

5.  **Salesforce でのシングル サインオンの構成** ] ページで、をクリックして **証明書のダウンロード**, 、お使いのコンピューターにローカルで証明書ファイルを保存します。

    ![証明書のダウンロード][9]

6. ブラウザーで新しいタブを開き、Salesforce の管理者アカウントにログインします。

7. 下にある、 **管理者** ナビゲーション ウィンドウで、をクリックして **セキュリティ制御** 関連セクションを展開します。 [ **シングル サインオン設定**します。

    ![[セキュリティ コントロール] から [シングル サインオンの設定] をクリック][10]

8.  **シングル サインオン設定** ] ページで、をクリックして、 **編集** ] ボタンをクリックします。

    ![[編集] ボタンをクリック][11]

    > [AZURE.NOTE] Salesforce アカウント用のシングル サインオン設定を有効にする場合は、Salesforce のサポートに連絡して、この機能を有効にする必要があります。

9. 選択 **SAML の有効化**, 、クリックして **保存**します。

    ![[SAML 有効] を選択][12]

10. SAML シングル サインオンの設定を構成するのには、クリックして **新規**します。

    ![[SAML 有効] を選択][13]

11.  **SAML シングル サインオン設定の編集** ] ページで、次の構成を行います。

    ![作成すべき構成のスクリーンショット][14]

 -  **名前** フィールドに、この構成のフレンドリ名を入力します。 値を提供する **名前** 自動的に入力、 **API 名** ] ボックスに貼り付けます。

 - Azure AD では、コピー、 **発行者 URL** 値に設定して、貼り付けます、 **発行者** フィールド Salesforce にします。

 -  **エンティティ Id] ボックスに貼り付けます**, 、次のパターンを使用して、Salesforce のドメイン名を入力します。
     - エンタープライズ アカウント: `https://<domain>.my.salesforce.com`
     - 開発者アカウント: `https://<domain>-dev-ed.my.salesforce.com`

 - をクリックして **参照** または **Choose File** を開くには、 **Choose File to Upload** ] ダイアログ ボックスで、Salesforce の証明書を選択し、クリックして **を開く** 、証明書をアップロードします。

 -  **SAML Identity Type**, [ **アサーションには、ユーザーの salesforce.com ユーザー名が含まれています。**します。

 -  **SAML Identity Location**, [ **Id は Subject ステートメントの NameIdentifier 要素**

 - Azure AD では、コピー、 **リモート ログイン URL** 値に設定して、貼り付けます、 **Identity Provider Login URL** フィールド Salesforce にします。

 -  **Service Provider Initiated Request Binding**, [ **HTTP リダイレクト**します。

 - 最後に、クリックして **保存** SAML シングル サインオンの設定を適用します。

12. Salesforce の左側のナビゲーション ウィンドウで、次のようにクリックします。 **ドメイン管理** に関連するセクションを展開し、クリック **My Domain**します。

    ![[マイ ドメイン] をクリック][15]

13. 下へスクロールして、 **認証の構成** セクションし、をクリックして、 **編集** ] ボタンをクリックします。

    ![[編集] ボタンをクリック][16]

14.  **認証サービス** セクションで SAML SSO 構成の表示名を選択し、をクリックし、 **保存**します。

    ![自分の SSO 構成を選択][17]

    > [AZURE.NOTE] 1 つ以上の認証サービスが選択されている場合、Salesforce 環境へのシングル サインオンを開始しようとしているユーザー、適宜扱おうとすると、サインインに使用する認証サービスを選択します。 かどうかは、発生すると、このされないようにしておいてください **他のすべての認証サービスをオフのまま**します。

15. Salesforce にアップロードした証明書を有効にするには、Azure AD でシングル サインオンの構成確認のチェック ボックスをオンにします。 クリックして **次**します。

    ![Check the confirmation checkbox][18]

16. ダイアログの最後のページでは、このシングル サインオン構成のメンテナンスに関連するエラーと警告の電子メール通知を受信する場合、電子メール アドレスを入力します。 

    ![Type in your email address.][19]

17. クリックして **完了** ダイアログを閉じます。 構成をテストするには、以下の「セクションを参照してください。 [Salesforce ユーザーを割り当てる](#step-4-assign-users-to-salesforce)します。

##手順 3. 自動化されたユーザー プロビジョニングを有効にする

1. Salesforce の [Azure AD のクイック スタート] ページで] をクリックして、 **ユーザー プロビジョニングの構成** ] ボタンをクリックします。

    ![[ユーザー プロビジョニングの構成] ボタンをクリック][20]

2.  **ユーザー プロビジョニングの構成** ダイアログで、Salesforce の管理者ユーザー名とパスワードを入力します。

    ![管理者のユーザー名またはパスワードを入力][21]

    > [AZURE.NOTE] 実稼働環境を構成する場合は、この手順に合わせて、Salesforce で新しい管理者アカウントを作成することをお勧めします。 このアカウントが必要、 **システム管理者** プロファイルが Salesforce に割り当てられます。

3. Salesforce のセキュリティ トークンを取得するには、新しいタブを開き、同じ Salesforce の管理者アカウントにサインインします。 ページの右上隅に、名前をクリックし、をクリックして **個人の設定**します。

    ![自分の名前をクリックして、[個人の設定] をクリック][22]

4. 左側のナビゲーション ウィンドウでのクリックして **個人** 、関連するセクションを展開し、をクリックする **Reset My Security Token**します。

    ![自分の名前をクリックして、[個人の設定] をクリック][23]

5.  **Reset My Security Token** ] ページで、をクリックして、 **セキュリティ トークンのリセット** ] ボタンをクリックします。

    ![警告文を確認][24]

6. この管理アカウントに関連付けられている電子メールの受信トレイを確認します。 新しいセキュリティ トークンが記載された Salesforce.com からの電子メールを探します。

7. トークンをコピーし、Azure AD のウィンドウに移動し、貼り付けます、 **ユーザーのセキュリティ トークン** フィールドです。 クリックして **次**します。

    ![セキュリティ トークンへの貼り付け][25]

8. 確認ページで、プロビジョニング エラー発生時の電子メール通知の受信を選択できます。 クリックして **完了** ダイアログを閉じます。

    ![お知らせを受け取るメール アドレスを入力][26]

##手順 4. Salesforce にユーザーを割り当てる

1. 構成をテストするために、まずディレクトリに新しいテスト アカウントを作成します。

2. Salesforce の [クイック スタート] ページで、をクリックして、 **ユーザーを割り当てる** ] ボタンをクリックします。

    ![Click on Assign Users][27]

3. テスト ユーザーを選択し、クリックして、 **を割り当てる** 、画面の下部にあるボタンをクリックします。

 - 自動化されたユーザー プロビジョニングを有効にしていない場合は、次のような確認メッセージが表示されます。

        ![Confirm the assignment.][28]

 - 自動化されたユーザー プロビジョニングを有効にしている場合は、ユーザーに必要な Salesforce プロファイルの種類の定義を求めるメッセージが表示されます。 新しくプロビジョニングされたユーザーは、数分後に Salesforce 環境に表示されます。

        ![Confirm the assignment.][29]

        > [AZURE.IMPORTANT] If you are provisioning to a Salesforce **developer** environment, you will have a very limited number of licenses available for each profile. Therefore, it's best to provision users to the **Chatter Free User** profile, which has 4,999 licenses available.

4. シングル サインオン設定をテストするには、アクセス パネルを開きます [https://myapps.microsoft.com](https://myapps.microsoft.com/), テスト アカウントにサインインしをクリックして **Salesforce**します。

[AZURE.INCLUDE [saas-toc](../../includes/active-directory-saas-toc.md)]

[0]: ./media/active-directory-saas-salesforce-tutorial/azure-active-directory.png
[1]: ./media/active-directory-saas-salesforce-tutorial/applications-tab.png
[2]: ./media/active-directory-saas-salesforce-tutorial/add-app.png
[3]: ./media/active-directory-saas-salesforce-tutorial/add-app-gallery.png
[4]: ./media/active-directory-saas-salesforce-tutorial/add-salesforce.png
[5]: ./media/active-directory-saas-salesforce-tutorial/salesforce-added.png
[6]: ./media/active-directory-saas-salesforce-tutorial/config-sso.png
[7]: ./media/active-directory-saas-salesforce-tutorial/select-azure-ad-sso.png
[8]: ./media/active-directory-saas-salesforce-tutorial/config-app-settings.png
[9]: ./media/active-directory-saas-salesforce-tutorial/download-certificate.png
[10]: ./media/active-directory-saas-salesforce-tutorial/sf-admin-sso.png
[11]: ./media/active-directory-saas-salesforce-tutorial/sf-admin-sso-edit.png
[12]: ./media/active-directory-saas-salesforce-tutorial/sf-enable-saml.png
[13]: ./media/active-directory-saas-salesforce-tutorial/sf-admin-sso-new.png
[14]: ./media/active-directory-saas-salesforce-tutorial/sf-saml-config.png
[15]: ./media/active-directory-saas-salesforce-tutorial/sf-my-domain.png
[16]: ./media/active-directory-saas-salesforce-tutorial/sf-edit-auth-config.png
[17]: ./media/active-directory-saas-salesforce-tutorial/sf-auth-config.png
[18]: ./media/active-directory-saas-salesforce-tutorial/sso-confirm.png
[19]: ./media/active-directory-saas-salesforce-tutorial/sso-notification.png
[20]: ./media/active-directory-saas-salesforce-tutorial/config-prov.png
[21]: ./media/active-directory-saas-salesforce-tutorial/config-prov-dialog.png
[22]: ./media/active-directory-saas-salesforce-tutorial/sf-my-settings.png
[23]: ./media/active-directory-saas-salesforce-tutorial/sf-personal-reset.png
[24]: ./media/active-directory-saas-salesforce-tutorial/sf-reset-token.png
[25]: ./media/active-directory-saas-salesforce-tutorial/got-the-token.png
[26]: ./media/active-directory-saas-salesforce-tutorial/prov-confirm.png
[27]: ./media/active-directory-saas-salesforce-tutorial/assign-users.png
[28]: ./media/active-directory-saas-salesforce-tutorial/assign-confirm.png
[29]: ./media/active-directory-saas-salesforce-tutorial/assign-sf-profile.png
