<properties 
    pageTitle="チュートリアル: Azure Active Directory と Workday の統合 | Microsoft Azure" 
    description="Azure Active Directory で Workday を使用して、シングル サインオンや自動プロビジョニングなどを有効にする方法について説明します。" 
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


# チュートリアル: Azure Active Directory と Workday の統合

このチュートリアルでは、Azure と Workday の統合について説明します。 このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   Workday のテナント

このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  Workday のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザー プロビジョニングの構成

![シナリオ](./media/active-directory-saas-workday-tutorial/IC782919.png "Scenario")

## Workday のアプリケーション統合の有効化

このセクションでは、Workday のアプリケーション統合を有効にする方法について説明します。

### Workday のアプリケーション統合を有効にするには、次の手順を実行します。

1.  Azure 管理ポータルの左側のナビゲーション ウィンドウで、**[Active Directory]** をクリックします。

    ![Active Directory](./media/active-directory-saas-workday-tutorial/IC700993.png "Active Directory")

2.  **[ディレクトリ]** の一覧から、ディレクトリ統合を有効にするディレクトリを選択します。

3.  アプリケーション ビューを開くには、ディレクトリ ビューでトップ メニューの **[アプリケーション]** をクリックします。

    ![アプリケーション](./media/active-directory-saas-workday-tutorial/IC700994.png "Applications")

4.  **アプリケーション ギャラリー**を開くには、**[アプリケーションの追加]**、**[組織で使用するアプリケーションを追加]** の順にクリックします。

    ![どの操作を行いますか。](./media/active-directory-saas-workday-tutorial/IC700995.png "What do you want to do?")

5.  **検索ボックス**に、「**Workday**」と入力します。

    ![Workday](./media/active-directory-saas-workday-tutorial/IC701021.png "Workday")

6.  結果ウィンドウで **[Workday]** を選択し、**[完了]** をクリックしてアプリケーションを追加します。

    ![Workday](./media/active-directory-saas-workday-tutorial/IC701022.png "Workday")

## シングル サインオンの構成

このセクションでは、SAML プロトコルに基づくフェデレーションを使用して、ユーザーが Azure AD のアカウントで Workday に対する認証を行うことができるようにする方法を説明します。  
この手順の途中で、Base-64 でエンコードされた証明書を作成する必要があります。  
この手順に慣れていない場合は、次を参照してください。 [バイナリ証明書をテキスト ファイルに変換する方法](http://youtu.be/PlgrzUZ-Y1o)します。

### シングル サインオンを構成するには、次の手順に従います。

1.  **Workday** アプリケーション統合ページでは、をクリックして **でのシングル サインオンを構成する** を開くには、 ** [シングル サインオン ** ] ダイアログ。

    ![シングル サインオンの構成](./media/active-directory-saas-workday-tutorial/IC782920.png "Configure single sign-on")

2.  **どのようなユーザーの Workday へ** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、] をクリックし、 **次**します。

    ![シングル サインオンの構成](./media/active-directory-saas-workday-tutorial/IC782921.png "Configure single sign-on")

3.  **[アプリケーション URL の構成]** ページで、次の手順を実行し、**[次へ]** をクリックします。

    ![アプリケーション URL の構成](./media/active-directory-saas-workday-tutorial/IC782957.png "Configure App URL")

    1.  **サインオン URL** Workday へのサインイン URL は、ユーザーが使用] ボックスに、種類 (例:: * https://impl.workday.com/\<tenant\>/login-saml2.htmld*)
    2.  **Workday 応答 URL** ] ボックスに、Workday 応答 url (例:: * https://impl.workday.com/\<tenant\>/login-saml.htmld*)。
        >[AZURE.NOTE] 応答 URL には必ずサブドメインを入れます (例: www、wd2、wd3、wd3-impl、wd5、wd5-impl)。 
        >ようなものを使用して"*http://www.myworkday.com*"動作が、"*http://myworkday.com*"しません。 

4.  **Workday でのシングル サインオンの構成** ] ページで、証明書をダウンロードする] をクリックして **証明書のダウンロード**, 、お使いのコンピューターに証明書ファイルを保存します。

    ![シングル サインオンの構成](./media/active-directory-saas-workday-tutorial/IC782922.png "Configure single sign-on")

5.  別の Web ブラウザー ウィンドウで、Workday 企業サイトに管理者としてログインします。

6.  移動して **メニュー \ > Workbench**します。

    ![ワークベンチ](./media/active-directory-saas-workday-tutorial/IC782923.png "Workbench")

7.  **[アカウント管理]** に移動します。

    ![アカウント管理](./media/active-directory-saas-workday-tutorial/IC782924.png "Account Administration")

8.  **[テナントのセットアップの編集 – セキュリティ]** に移動します。

    ![テナントのセキュリティの編集](./media/active-directory-saas-workday-tutorial/IC782925.png "Edit Tenant Security")

9.  **[リダイレクト URL]** セクションで、次の手順を実行します。

    ![リダイレクト URL](./media/active-directory-saas-workday-tutorial/IC7829581.png "Redirection URLs")

     9.1. **[行の追加]** をクリックします。

     9.2. **[ログイン リダイレクトの URL]** ボックスと **[モバイル リダイレクトの URL]** ボックスに、Azure ポータルの **[アプリケーション URL の構成]** で **[Workday テナント URL]** に入力した URL を入力します。

     9.3. Azure ポータルで、**[Workday でのシングル サインオンの構成]** ダイアログ ページの **[シングル サインアウト サービス URL]** をコピーし、**[ログアウト リダイレクト URL]** ボックスに貼り付けます。

     9.4.  **[環境]** テキスト ボックスに、環境の名前を入力します。

       >[AZURE.NOTE] [環境] 属性の値が、テナント URL の値に関連付けられます。
        >
        >-Workday テナントのドメイン名の URL が impl で始まる場合 (例:: * https://impl.workday.com/\<tenant\>/login-saml2.htmld*)、 **環境** 実装に属性を設定する必要があります。
        >-ドメイン名は、別のもので起動する場合は、Workday に問い合わせの一致する必要があります。 **環境** 値。

10. **[SAML 設定]** セクションで、次の手順を実行します。

    ![SAML のセットアップ](./media/active-directory-saas-workday-tutorial/IC782926.png "SAML Setup")

     10.1.  **[Enable SAML Authentication]** を選択します。

     10.2.  **[行の追加]** をクリックします。

11. [SAML ID プロバイダー] セクションで、次の手順に従います。

    ![SAML ID プロバイダー](./media/active-directory-saas-workday-tutorial/IC7829271.png "SAML Identity Providers")

     11.1.  [ID プロバイダー名] テキスト ボックスに、プロバイダー名を入力します (例: *SPInitiatedSSO*)。

     11.2.  Azure ポータルで、**[Workday でのシングル サインオンの構成]** ダイアログ ページの **[ID プロバイダーの ID]** の値をコピーし、**[発行者]** テキスト ボックスに貼り付けます。

     11.3.  **[Workday 始動ログアウトを有効にする]** を選択します。

     11.4. Azure ポータルで、**[Workday でのシングル サインオンの構成]** ダイアログ ページの **[シングル サインアウト サービス URL]** 値をコピーし、**[ログアウト要求 URL]** ボックスに貼り付けます。

     11.3.  クリックして **Identity Provider Public Key Certificate**, 、] をクリックし、 **作成**します。 <br><br>
        ![作成](./media/active-directory-saas-workday-tutorial/IC782928.png "Create")

     11.4.  をクリックして **作成 x509 公開キー**します。 <br><br>
        ![作成](./media/active-directory-saas-workday-tutorial/IC782929.png "Create")

     11.5.  **View x509 公開キー** セクションで、次の手順に従います。 <br><br>
        ![View x509 公開キー](./media/active-directory-saas-workday-tutorial/IC782930.png "View x509 Public Key") <br>

      1.  **名** ] ボックスに、証明書の名前を入力 (例:: *PPE\_SP*)。
      2.  **[有効期間の開始日]** テキスト ボックスに、証明書の有効期間の開始日を示す属性の値を入力します。
      3.  **[有効期間の終了日]** テキスト ボックスに、証明書の有効期間の終了日を示す属性の値を入力します。

           >[AZURE.NOTE] 有効期間の開始日と終了日は、ダウンロードした証明書をダブルクリックして確認できます。 日付は **[詳細]** タブに表示されます。

      4.  ダウンロードした証明書から **Base-64 でエンコードされた**ファイルを作成します。
        >[AZURE.TIP] 詳細については、「 [バイナリ証明書をテキスト ファイルに変換する方法](http://youtu.be/PlgrzUZ-Y1o)

      5.  Base 64 でエンコードされた証明書をメモ帳で開き、その内容をコピーします。
      6.  **[証明書]** テキスト ボックスに、クリップボードの内容を貼り付けます。
      7.  **[OK]** をクリックします。

12.  次の手順に従います。 <br><br>
        ![SSO 構成](./media/active-directory-saas-workday-tutorial/IC7829351111.png "SSO configuration")

     12.1.  **[x509 秘密鍵のペア]** を有効にします。

     12.2.  **Service Provider ID** ] ボックスに「 **http://www.workday.com**します。

     12.3.  **[SP によって開始された SAML 認証を有効にする]** を選択します。

     12.4.  Azure ポータルの **[Workday でのシングル サインオンの構成]** ダイアログ ページで、**[シングル サインオン サービス URL]** の値をコピーし、**[IdP SSO サービス URL]** テキスト ボックスに貼り付けます。

     12.5 選択 **SP が開始した認証要求 Do Not Deflate**します。

     12.6. として **Authentication Request Signature Method**, [ **SHA256**します。 <br><br>
        ![Authentication Request Signature Method](./media/active-directory-saas-workday-tutorial/IC782932.png "Authentication Request Signature Method") <br><br>

     12.7 クリック **OK**します。 <br><br>
        ![OK](./media/active-directory-saas-workday-tutorial/IC782933.png "OK")

12. Azure AD ポータルでの **Workday でのシングル サインオンの構成** ] ページで [ **次**します。 <br><br>

    ![シングル サインオンの構成](./media/active-directory-saas-workday-tutorial/IC782934.png "Configure single sign-on")

13. **シングル サインオンによる確認** ] ページで [ **完了**します。 <br><br>

    ![シングル サインオンの構成](./media/active-directory-saas-workday-tutorial/IC782935111.png "Configure single sign-on")



## ユーザー プロビジョニングの構成

Workday にテスト ユーザーをプロビジョニングするには、Workday のサポート チームに連絡する必要があります。  
Workday のサポート チームにユーザーを作成してもらいます。

## ユーザーの割り当て

構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

### ユーザーを Workday に割り当てるには、次の手順を実行します。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  **Workday * * アプリケーション統合ページで、をクリックして * * ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-workday-tutorial/IC782935.png "Assign Users")

3.  テスト ユーザーを選択し、**[割り当て]**、**[はい]** の順にクリックして、割り当てを確定します。

    ![はい](./media/active-directory-saas-workday-tutorial/IC767830.png "Yes")

シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。




