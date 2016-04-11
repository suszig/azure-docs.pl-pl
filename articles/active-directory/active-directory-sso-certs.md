<properties
    pageTitle="Azure AD でフェデレーション証明書を管理する方法 |Microsoft Azure"
    description="フェデレーション証明書の有効期限をカスタマイズする方法と、有効期限が近づいている証明書を更新する方法について説明します。"
    services="active-directory"
    documentationCenter=""
    authors="liviodlc"
    manager="terrylan"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/29/2015"
    ms.author="liviodlc"/>

#Azure Active Directory でのフェデレーション シングル サインオンの証明書の管理

この記事では、SaaS アプリケーションにフェデレーション シングル サインオン (SSO) を確立するために Azure Active Directory で作成される証明書に関連する一般的な質問について説明します。

この記事はだけを使用する構成されているアプリに関連 **Azure AD シングル サインオン**, の次の例に示すようにします。

![Azure AD Single Sign-On](./media/active-directory-sso-certs/fed-sso.PNG)

##フェデレーション証明書の有効期限をカスタマイズする方法

既定では、証明書は 2 年後に期限切れになるように設定されています。 次の手順に従って、証明書の別の有効期限を選択できます。 ここに用意したスクリーンショットでは、例として Salesforce を使用していますが、これらの手順は、他のフェデレーション SaaS アプリにも適用できます。

1. Azure Active Directory で、アプリケーションの [クイック スタート] ページをクリックします **でのシングル サインオンを構成する**です。

    ![SSO 構成ウィザードを開く](./media/active-directory-sso-certs/config-sso.png)

2. 選択 **Azure AD シングル サインオン**, 、] をクリックし、 **次**します。

3. 入力、 **サインオン URL** 、アプリケーションのチェック ボックスをオンおよび **フェデレーション シングル サインオンを使用する証明書を構成する**です。 クリックして **次**します。

    ![Azure AD Single Sign-On](./media/active-directory-sso-certs/new-app-config-sso.PNG)

4. 次のページで次のように選択します。 **新しい証明書を生成**, 、し、有効にする証明書をとなる期間を選択します。 クリックして **次**します。

    ![新しい証明書の生成](./media/active-directory-sso-certs/new-app-config-cert.PNG)

5. 次に、[上] をクリックして **証明書のダウンロード**します。 特定の SaaS アプリに証明書をアップロードする方法については、次のようにクリックします。 **構成手順を表示**します。

    ![証明書をダウンロードした後アップロードする](./media/active-directory-sso-certs/new-app-config-app.PNG)

6. ダイアログの下部にある確認のチェック ボックスをオンにして [送信] をクリックするまで、証明書は有効になりません。

##有効期限が近づいている証明書を更新する方法

次に示す更新手順を実行すると、ユーザーに対する大幅なダウンタイムがなくなり理想的です。 このセクションで使用するスクリーンショットでは、例として Salesforce を取り上げていますが、これらの手順は他のフェデレーション SaaS アプリにも適用できます。

1. Azure Active Directory で、アプリケーションの [クイック スタート] ページをクリックします **シングル サインオンを構成する**です。

    ![SSO 構成ウィザードを開く](./media/active-directory-sso-certs/renew-sso-button.PNG)

2. ダイアログの最初のページで **Azure AD シングル サインオン** 済みでなければなりませんのでをクリックして選択すると、 **次**します。

3. 2 番目のページで、チェック ボックスをオン **フェデレーション シングル サインオンを使用する証明書を構成する**です。 クリックして **次**します。

    ![Azure AD Single Sign-On](./media/active-directory-sso-certs/renew-config-sso.PNG)

4. 次のページで次のように選択します。 **新しい証明書を生成**, 、どのくらいの期間を選択、新しい証明書の有効であるとします。 クリックして **次**します。

    ![新しい証明書の生成](./media/active-directory-sso-certs/new-app-config-cert.PNG)

5. をクリックして **証明書のダウンロード**します。 正常に証明書を更新するには、次の 2 つの手順を実行する必要があります。

    - SaaS アプリのシングル サインオンの構成画面に新しい証明書をアップロードします。 この特定の SaaS アプリに対して行う方法については、クリックして **構成手順を表示**します。

    - Azure AD で新しい証明書を有効にする] ダイアログ ボックスの下部にある確認のチェック ボックスを選択し、クリックして **次** を送信します。

    > [AZURE.IMPORTANT] アプリケーションにシングル サインオンが無効にする 2 つの手順のいずれか時点が完了しましたが、それが再度有効にする 2 番目の手順が完了するとします。 そのため、ダウンタイムを最小限に抑えるために、2 つの手順を間をおかずに短時間で完了してください。

    ![証明書をダウンロードした後アップロードする](./media/active-directory-sso-certs/renew-config-app.PNG)

[AZURE.INCLUDE [saas-toc](../../includes/active-directory-saas-toc.md)]

