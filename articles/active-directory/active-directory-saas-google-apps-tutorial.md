<properties
   pageTitle="チュートリアル: Azure Active Directory と Google Apps の統合 | Microsoft Azure"
   description="Azure Active Directory で Google Apps を使用して、シングル サインオンを有効にする方法、プロビジョニングを自動化する方法などについて説明します。"
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
   ms.date="11/01/2015"
   ms.author="liviodlc"/>

#チュートリアル: Azure Active Directory と Google Apps を統合する方法

このチュートリアルでは、Azure Active Directory (Azure AD) に Google Apps 環境を接続する方法を説明します。 Google Apps へのシングル サインオンを構成する方法、ユーザー プロビジョニングの自動化を有効にする方法、Google Apps へのアクセス権をユーザーに割り当てる方法について説明します。 

##前提条件

1. Azure Active Directory にアクセスする、 [Azure 管理ポータル](https://manage.windowsazure.com), 、有効な Azure サブスクリプションが必要です。

2. 有効なテナントが必要 [Google Apps for Work](https://www.google.com/work/apps/) または [Google Apps for Education](https://www.google.com/edu/products/productivity-tools/)します。 どちらのサービスにも無料試用版のアカウントを使用できます。

##ビデオ チュートリアル

2 分間で Google Apps へのシングル サインオンを有効にする方法:

> [AZURE.VIDEO enable-single-sign-on-to-google-apps-in-2-minutes-with-azure-ad]

##手順 1. Google Apps をディレクトリに追加する

1.  [Azure 管理ポータル](https://manage.windowsazure.com), 、左側のナビゲーション ウィンドウで、次のようにクリックします。 **Active Directory**します。

    ![Select Active Directory from the left navigation pane.][0]

2.  **ディレクトリ** 一覧で、Google Apps を追加するには、ディレクトリを選択します。

3. をクリックして **アプリケーション** 上部のメニュー。

    ![Click on Applications.][1]

4. クリックして **追加** ページの下部にあります。

    ![Click Add to add a new application.][2]

5.  **実行する操作** ダイアログ ボックスで、をクリックして **ギャラリーからアプリケーションを追加**します。

    ![Click Add an application from the gallery.][3]

6.  **検索ボックス**, 、型 **Google Apps**します。 選択し、 **Google Apps** 結果、およびクリックから **完了** アプリケーションを追加します。

    ![Add Google Apps.][4]

7. これで、Google Apps の [クイック スタート] ページが表示されます。

    ![Google Apps' Quick Start page in Azure AD][5]

##手順 2. シングル サインオンを有効にする

1. Azure ad では、Google Apps の [クイック スタート] ページで、クリックして、 **でのシングル サインオンを構成する** ] ボタンをクリックします。

    ![The configure single sign-on button][6]

2. ダイアログが開き、ユーザーが Google Apps にサインオンする方法についてたずねる画面が表示されます。 選択 **Azure AD シングル サインオン**, 、] をクリックし、 **次**します。

    ![Select Azure AD Single Sign-On][7]

    > [AZURE.NOTE] 詳細については、さまざまなシングル サインオン オプション、 [ここをクリックして](../active-directory-appssoaccess-whatis/#how-does-single-sign-on-with-azure-active-directory-work)

3.  **アプリケーション設定の構成** ] ページでの **サインオン URL** フィールドに、次の形式を使用して Google Apps テナントの url を入力します。 `https://mail.google.com/a/<yourdomain>`

    ![Type in your tenant URL][8]

4.  **でのシングル サインオンを自動構成する** ] ページで、Google Apps テナントのドメインで入力します。 キーを押します、 **構成** ] ボタンをクリックします。

    ![ドメイン名を入力し、[構成] をクリックします。](./media/active-directory-saas-google-apps-tutorial/ga-auto-config.png)

    > [AZURE.NOTE] シングル サインオンを手動で構成する場合を参照してください [省略可能な手順: 手動で構成するシングル サインオン。](#optional-step-manually-configure-single-sign-on)

5. Google Apps 管理者アカウントにサインインします。 クリックして **許可** Azure Active Directory を Google Apps サブスクリプション内の構成の変更を許可するためにします。

    ![ドメイン名を入力し、[構成] をクリックします。](./media/active-directory-saas-google-apps-tutorial/ga-consent.PNG)

6. Azure Active Directory が Google Apps テナントを構成する間、数秒待ちます。 これが完了すると、クリックして **次**します。

10. ダイアログの最後のページでは、このシングル サインオン構成のメンテナンスに関連するエラーと警告の電子メール通知を受信する場合、電子メール アドレスを入力します。

    ![Type in your email address.][14]

11. クリックして **完了** ダイアログを閉じます。 構成をテストするには、以下の「セクションを参照してください。 [Google Apps にユーザーを割り当てる](#step-4-assign-users-to-google-apps)します。

##オプションの手順: シングル サインオンを手動で構成する

シングル サインオンを手動で設定する場合は、次の手順を実行します。

1. Azure ad では、Google Apps の [クイック スタート] ページで、クリックして、 **でのシングル サインオンを構成する** ] ボタンをクリックします。

    ![The configure single sign-on button][6]

2. ダイアログが開き、ユーザーが Google Apps にサインオンする方法についてたずねる画面が表示されます。 選択 **Azure AD シングル サインオン**, 、] をクリックし、 **次**します。

    ![Select Azure AD Single Sign-On][7]

    > [AZURE.NOTE] 詳細については、さまざまなシングル サインオン オプション、 [ここをクリックして](../active-directory-appssoaccess-whatis/#how-does-single-sign-on-with-azure-active-directory-work)

3.  **アプリケーション設定の構成** ] ページでの **サインオン URL** フィールドに、次の形式を使用して Google Apps テナントの url を入力します。 `https://mail.google.com/a/<yourdomain>`

    ![Type in your tenant URL][8]

4.  **でのシングル サインオンを自動構成する** というラベルが付いたチェック ボックスを選択] ページで、 **手動でこのアプリケーションでのシングル サインオンを構成する**です。 クリックして **次**します。

    ![手動構成を選択します。](./media/active-directory-saas-google-apps-tutorial/ga-auto-skip.PNG)

4.  **Google Apps でのシングル サインオンの構成** ] ページで、をクリックして **証明書のダウンロード**, 、お使いのコンピューターにローカルで証明書ファイルを保存します。

    ![Download the certificate.][9]

5. ブラウザーで新しいタブを開き、サインイン、 [Google Apps の管理コンソール](http://admin.google.com/) 管理者アカウントを使用します。

6. クリックして **セキュリティ**します。 リンクが表示されない場合は、下にある隠れることがあります、 **コントロール** 画面の下部にあるメニュー。

    ![Click Security.][10]

7.  **セキュリティ** ] ページで [ **シングル サインオン (SSO) を設定します。**

    ![Click SSO.][11]

8. 次の構成の変更を実行します。

    ![Configure SSO][12]

    - 選択 **サード パーティ製 id プロバイダーで SSO をセットアップ**します。

    - Azure AD では、コピー、 **シングル サインオン サービス URL**, 、貼り付けます、 **サインイン ページ URL** フィールド Google Apps にします。

    - Azure AD では、コピー、 **シングル サインアウト サービス URL**, 、貼り付けます、 **サインアウト ページ URL** フィールド Google Apps にします。

    - Azure AD では、コピー、 **パスワード変更 URL**, 、貼り付けます、 **パスワード変更 URL** フィールド Google Apps にします。

    - Google Apps での **検証証明書**, 、手順 4. でダウンロードした証明書をアップロードします。

    - クリックして **変更を保存**します。

9. Azure AD でシングル サインオンの構成確認のチェック ボックスをオンにして、Google Apps にアップロードした証明書を有効にします。 クリックして **次**します。

    ![Check the confirmation checkbox][13]

10. ダイアログの最後のページでは、このシングル サインオン構成のメンテナンスに関連するエラーと警告の電子メール通知を受信する場合、電子メール アドレスを入力します。 

    ![Type in your email address.][14]

11. クリックして **完了** ダイアログを閉じます。 構成をテストするには、以下の「セクションを参照してください。 [Google Apps にユーザーを割り当てる](#step-4-assign-users-to-google-apps)します。

##手順 3. 自動化されたユーザー プロビジョニングを有効にする

> [AZURE.NOTE] Google Apps へのユーザー プロビジョニングを自動化するためのもう 1 つの実行可能なオプションは、使用する [Google Apps Directory Sync (GADS)](https://support.google.com/a/answer/106368?hl=en) Google Apps に内部設置型 Active Directory id。 このチュートリアルでは Azure Active Directory (クラウド) のユーザーとメールが有効なグループを Google Apps にプロビジョニングします。

1. サインイン、 [Google Apps の管理コンソール](http://admin.google.com/) 、管理者アカウントを使用し、クリックして **セキュリティ**します。 リンクが表示されない場合は、下にある隠れることがあります、 **コントロール** 画面の下部にあるメニュー。

    ![Click Security.][10]

2.  **セキュリティ** ] ページで [ **API リファレンス**します。

    ![Click API Reference.][15]

3. 選択 **Enable API access**します。

    ![Click API Reference.][16]

    > [AZURE.IMPORTANT] Google Apps では、Azure Active Directory でのユーザー名にプロビジョニングするすべてのユーザーに対して *必要があります* カスタム ドメインに関連付けられています。 たとえば、ユーザー名をよう bob@contoso.onmicrosoft.com は Google Apps で受け付けられません bob@contoso.com が受け入れられますができます。 Azure AD でプロパティを編集することによって、既存のユーザーのドメインを変更できます。 Azure Active Directory と Google Apps の両方でカスタム ドメインを設定する方法を次に示します。

4. まだ Azure Active Directory にカスタム ドメイン名を追加していない場合は、次の手順に従います。

    -  [Azure 管理ポータル](https://manage.windowsazure.com), 、左側のナビゲーション ウィンドウで、次のようにクリックします。 **Active Directory**します。 ディレクトリの一覧で、ディレクトリを選択します。 

    - をクリックして **ドメイン** クリックしてトップレベルのメニューから **カスタム ドメインの追加**します。

        ![カスタム ドメインの追加][17]

    - ドメイン名を入力、 **ドメイン名** フィールドです。 Google Apps で使用するのと同じドメイン名にする必要があります。 準備ができたら、次のようをクリックして、 **追加** ] ボタンをクリックします。

        ![Type in your domain name.][18]

    - をクリックして **次** の確認] ページに移動します。 このドメインを所有していることを確認するために、このページに表示されている値に従ってドメインの DNS レコードを編集する必要があります。 いずれかを使用することを確認することもできます **MX レコード** または **TXT レコード**, 、選択した項目に応じて、 **レコードの種類** オプション。 Azure AD でドメイン名を確認する方法のより包括的な手順については、次を参照してください。 [を Azure AD ドメイン名を追加](https://go.microsoft.com/fwLink/?LinkID=278919&clcid=0x409)します。

        ![Verify your domain name.][19]

    - ディレクトリに追加するすべてのドメインに対して、上記の手順を繰り返します。

5. すべてのドメインを Azure AD で確認したので、今度は Google Apps で確認する必要があります。 Google Apps にまだ登録されていない各ドメインに対して、次の手順を実行します。

    -  [Google Apps の管理コンソール](http://admin.google.com/), 、] をクリックして **ドメイン**します。

        ![Click on Domains][20]

    - クリックして **ドメインやドメイン エイリアスを追加**します。

        ![Add a new domain][21]

    - 選択 **別のドメインを追加**, 、しを追加するには、ドメインの名前を入力します。

        ![Type in your domain name][22]

    - をクリックして **続行してドメインの所有権を確認**します。 次に、手順に従って、ドメイン名を所有していることを確認します。 Google Apps でドメインを確認する包括的な手順については、次を参照してください。 [Google Apps でサイトの所有者を確認](https://support.google.com/webmasters/answer/35179)します。

    - Google Apps に追加するすべての追加ドメインに対して、上記の手順を繰り返します。

    > [AZURE.WARNING] かどうかは、Google Apps テナントのプライマリ ドメインを変更済みであるかどうかと、Azure ad でシングル サインオンを構成しを下にある手順 3. を繰り返す必要が [手順 2: シングル サインオンを有効にする](#step-two-enable-single-sign-on)です。

6.  [Google Apps の管理コンソール](http://admin.google.com/), 、] をクリックして **管理者の役割**します。

    ![Click on Google Apps][26]

7. ユーザー プロビジョニングの管理にどの管理者アカウントを使用するかを決定します。  **Admin ロール** 、そのアカウントの編集、 **特権** の役割にします。 すべてのことを確認、 **管理者 API 特権** を有効にすると、このアカウントをプロビジョニングに使用することができます。

    ![Click on Google Apps][27]

    > [AZURE.NOTE] 実稼働環境を構成する場合は、この手順に合わせて、Google Apps で新しい管理者アカウントを作成することをお勧めします。 管理者アカウントには、必要な API 特権を持つ管理者の役割が関連付けられている必要があります。

8. Azure の Active Directory で] をクリックして **アプリケーション** クリックしてトップレベルのメニューで **Google Apps**します。

    ![Click on Google Apps][23]

9. Google Apps の [クイック スタート] ページでのクリックして **ユーザー プロビジョニングの構成**します。

    ![Configure user provisioning][24]

10. 表示されるダイアログ ボックスでクリックして **ユーザー プロビジョニングを有効にする** プロビジョニングの管理に使用するには Google Apps の管理者アカウントを認証します。

    ![Enable provisioning][25]

11. Google Apps のテナントに対して変更を行うためのアクセス許可を Azure Active Directory に付与することを確認します。

    ![Confirm permissions.][28]

12. クリックして **完了** ダイアログを閉じます。

##手順 4. Google Apps にユーザーを割り当てる

1. 構成をテストするために、ディレクトリに新しいテスト アカウントを作成します。

2. Google Apps のクイック スタート] ページで、[上] をクリックして、 **ユーザーを割り当てる** ] ボタンをクリックします。

    ![Click on Assign Users][29]

3. テスト ユーザーを選択し、クリックして、 **を割り当てる** 、画面の下部にあるボタンをクリックします。

 - 自動化されたユーザー プロビジョニングを有効にしていない場合は、次のような確認メッセージが表示されます。

        ![Confirm the assignment.][30]

 - ユーザー プロビジョニングの自動化を有効にしている場合は、ユーザーに必要な Google Apps での役割の種類の定義を要求するプロンプトが表示されます。 新しくプロビジョニングされたユーザーは、数分後に、Google Apps の環境に表示されます。

4. シングル サインオン設定をテストするには、アクセス パネルを開きます [https://myapps.microsoft.com](https://myapps.microsoft.com/), テスト アカウントにサインインしをクリックして **Google Apps**します。

[AZURE.INCLUDE [saas-toc](../../includes/active-directory-saas-toc.md)]

[0]: ./media/active-directory-saas-google-apps-tutorial/azure-active-directory.png
[1]: ./media/active-directory-saas-google-apps-tutorial/applications-tab.png
[2]: ./media/active-directory-saas-google-apps-tutorial/add-app.png
[3]: ./media/active-directory-saas-google-apps-tutorial/add-app-gallery.png
[4]: ./media/active-directory-saas-google-apps-tutorial/add-gapps.png
[5]: ./media/active-directory-saas-google-apps-tutorial/gapps-added.png
[6]: ./media/active-directory-saas-google-apps-tutorial/config-sso.png
[7]: ./media/active-directory-saas-google-apps-tutorial/sso-gapps.png
[8]: ./media/active-directory-saas-google-apps-tutorial/sso-url.png
[9]: ./media/active-directory-saas-google-apps-tutorial/download-cert.png
[10]: ./media/active-directory-saas-google-apps-tutorial/gapps-security.png
[11]: ./media/active-directory-saas-google-apps-tutorial/security-gapps.png
[12]: ./media/active-directory-saas-google-apps-tutorial/gapps-sso-config.png
[13]: ./media/active-directory-saas-google-apps-tutorial/gapps-sso-confirm.png
[14]: ./media/active-directory-saas-google-apps-tutorial/gapps-sso-email.png
[15]: ./media/active-directory-saas-google-apps-tutorial/gapps-api.png
[16]: ./media/active-directory-saas-google-apps-tutorial/gapps-api-enabled.png
[17]: ./media/active-directory-saas-google-apps-tutorial/add-custom-domain.png
[18]: ./media/active-directory-saas-google-apps-tutorial/specify-domain.png
[19]: ./media/active-directory-saas-google-apps-tutorial/verify-domain.png
[20]: ./media/active-directory-saas-google-apps-tutorial/gapps-domains.png
[21]: ./media/active-directory-saas-google-apps-tutorial/gapps-add-domain.png
[22]: ./media/active-directory-saas-google-apps-tutorial/gapps-add-another.png
[23]: ./media/active-directory-saas-google-apps-tutorial/apps-gapps.png
[24]: ./media/active-directory-saas-google-apps-tutorial/gapps-provisioning.png
[25]: ./media/active-directory-saas-google-apps-tutorial/gapps-provisioning-auth.png
[26]: ./media/active-directory-saas-google-apps-tutorial/gapps-admin.png
[27]: ./media/active-directory-saas-google-apps-tutorial/gapps-admin-privileges.png
[28]: ./media/active-directory-saas-google-apps-tutorial/gapps-auth.png
[29]: ./media/active-directory-saas-google-apps-tutorial/assign-users.png
[30]: ./media/active-directory-saas-google-apps-tutorial/assign-confirm.png

