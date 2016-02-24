<properties 
    pageTitle="チュートリアル: Azure Active Directory と SAP HANA Cloud Platform の統合 | Microsoft Azure" 
    description="Azure Active Directory で SAP HANA Cloud Platform を使用して、シングル サインオンや自動プロビジョニングなどを有効にする方法について説明します。" 
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

#チュートリアル: Azure Active Directory と SAP HANA Cloud Platform の統合
  
このチュートリアルでは、Azure と SAP HANA Cloud Platform の統合について説明します。  
このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   SAP HANA Cloud Platform アカウント
  
このチュートリアルを完了すると、SAP HANA Cloud Platform に割り当てた Azure AD ユーザーができるようにアプリケーションを使用して、シングル サインオン、 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。

>[AZURE.IMPORTANT]独自のアプリケーションを展開するか、シングル サインオンをテストするには、SAP HANA Cloud Platform アカウントにアプリケーションをサブスクライブする必要があります。 このチュートリアルでは、アプリケーションはアカウントにデプロイされます。
  
このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  SAP HANA Cloud Platform でアプリケーション統合を有効にする
2.  シングル サインオンの構成
3.  ユーザーにロールを割り当てる
4.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790795.png "Scenario")
##SAP HANA Cloud Platform でアプリケーション統合を有効にする
  
このセクションでは、SAP HANA Cloud Platform のアプリケーション統合を有効にする方法について説明します。

###SAP HANA Cloud Platform のアプリケーション統合を有効にするには、次の手順に従います。

1.  左側のナビゲーション ウィンドウで、Azure 管理ポータルで、クリックして **Active Directory**します。

    ![Active Directory](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC700993.png "Active Directory")

2.   **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3.  ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。

    ![アプリケーション](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC700994.png "Applications")

4.  クリックして **追加** ページの下部にあります。

    ![アプリケーションの追加](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC749321.png "Add application")

5.   **実行する操作** ダイアログ ボックスで、をクリックして **ギャラリーからアプリケーションを追加**します。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC749322.png "Add an application from gallerry")

6.   **検索ボックス**, 、型 **SAP HANA Cloud Platform**します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790796.png "Application Gallery")

7.  結果ウィンドウで [ **SAP HANA Cloud Platform**, 、クリックして **完了** アプリケーションを追加します。

    ![SAP Hana](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793929.png "SAP Hana")
##シングル サインオンの構成
  
このセクションでは、ユーザーが SAML プロトコルに基づくフェデレーションを使用して、Azure AD でのユーザーのアカウントで SAP HANA Cloud Platform に対する認証を行えるようにする方法を説明します。  
この手順の途中で、base-64 でエンコードされた証明書を SAP HANA Cloud Platform テナントにアップロードする必要があります。  
この手順に慣れていない場合は、次を参照してください [バイナリ証明書をテキスト ファイルに変換する方法。](http://youtu.be/PlgrzUZ-Y1o)

###シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **SAP HANA Cloud Platform** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、* * [シングル サインオン * * ダイアログ。

    ![Configure single sign-on](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC778552.png "Configure single sign-on")

2.   **どのようなユーザーを SAP HANA Cloud Platform へのサインオン** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、] をクリックし、 **次**します。

    ![Configure Single Sign-On](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790797.png "Configure Single Sign-On")

3.  別の web ブラウザーのウィンドウでへのサインオン https://account.\<landscape host\ で SAP HANA Cloud Platform コックピット >.ondemand.com/cockpit (例:: *https://account.hanatrial.ondemand.com/cockpit*)。

4.  クリックして、 **信頼** ] タブをクリックします。

    ![信頼](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790800.png "Trust")

5.  信頼管理セクションで、次の手順に従います。

    ![メタデータの取得](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793930.png "Get Metadata")

    1.  クリックして、 **Local Service Provider** ] タブをクリックします。
    2.  SAP HANA Cloud Platform のメタデータ ファイルをダウンロードするにはクリックして **Get Metadata**します。

6.  Azure Active Directory ポータルでの **アプリケーション URL の構成** ] ページで、次の手順を実行して順にクリックして **次**します。

    ![Configure App URL](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790798.png "Configure App URL")

    1.   **サインオン URL** URL は、ユーザーへのサインインに使用] ボックスに、型、 **SAP HANA Cloud Platform** アプリケーションです。 これは、SAP HANA Cloud Platform アプリケーションで保護されたリソースのアカウント固有の URL です。 URL は次のパターンに基づいている: *https://\<applicationName\>\<accountName\>.\<landscape host \ >.ondemand.com/\ < path\_to\_protected\_resource\ >* (例:: *https://xleavep1941203872trial.hanatrial.ondemand.com/xleave*)

        >[AZURE.NOTE]これは、ユーザーの認証を必要とする SAP HANA Cloud Platform アプリケーションの URL です。

    2.  ダウンロードした SAP HANA Cloud Platform のメタデータ ファイルを開いてを見つけて、 **ns 3: assertionconsumerservice** タグ。
    3.  値をコピー、 **場所** 属性があり、貼り付けます、 **SAP HANA Cloud Platform 応答 URL** ] ボックスに貼り付けます。

7.   **SAP HANA Cloud Platform でのシングル サインオンの構成** ] ページで、メタデータをダウンロードする] をクリックして **メタデータのダウンロード**, 、お使いのコンピューター上のファイルを保存します。

    ![Configure Single Sign-On](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790799.png "Configure Single Sign-On")

8.  SAP HANA Cloud Platform コックピットに、 **Local Service Provider** セクションで、次の手順に従います。

    ![信頼管理](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793931.png "Trust Management")

    1.  クリックして **編集**します。
    2.  として **構成の種類**, [ **カスタム**します。
    3.  として **Local Provider Name**, 、既定値のままにします。
    4.  生成する、 **署名キー** と **署名証明書** キーのペアをクリックして **Generate Key Pair**します。
    5.  として **Principal Propagation**, [ **無効になっている**します。
    6.  として **Force Authentication**, [ **無効になっている**します。
    7.  クリックして **保存**します。

9.  クリックして、 **Trusted Identity Provider** タブをクリックし、をクリックし、 **Add Trusted Identity Provider**します。

    ![信頼管理](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790802.png "Trust Management")

    >[AZURE.NOTE]信頼できる id プロバイダーの一覧を管理するには、Local Service Provider] セクションで、カスタム構成の種類を選択する必要があります。 既定の構成タイプでは、SAP ID サービスに対する編集不可能で暗黙的な信頼があります。 [なし] では、いずれの信頼設定もありません。

10. をクリックして、 **全般** タブをクリックし、をクリックして **参照** ダウンロードしたメタデータ ファイルをアップロードします。

    ![信頼管理](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793932.png "Trust Management")

    >[AZURE.NOTE] 値であるメタデータ ファイルをアップロードしたら **シングル サイン オン URL**, 、**Single Logout URL** と **署名証明書** は自動的に設定されます。

11. クリックして、 **属性** ] タブをクリックします。

12.  **属性** ] タブで、次の手順を実行します。

    ![属性](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790804.png "Attributes")

    1.  クリックして **add assertion-based Attribute**, 、次のアサーション ベースの属性を追加します。

        |アサーション属性| プリンシパル属性|
        |-------------------|--------------------|
        |http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname|   firstname|--------------------|--------------------|
        |http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname|        lastname|-----------|
        |http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress|email|

    >[AZURE.NOTE]属性の構成は、HCP 上のアプリケーションは開発方法、つまり、SAML 応答で必要になると、コード内でこの属性にアクセスされる名前 (プリンシパル属性)、属性に依存します。
    >  
    >a.   **属性の既定の** のスクリーン ショットがわかりやすくするためだけです。 このシナリオでは必要ありません。  
    >
    >b.  名前と値 **プリンシパル属性** スクリーン ショットに示すように、アプリケーションの開発方法に依存します。 使用するアプリケーションによって、異なるマッピングが必要になる場合があります。

13. Azure ポータルで、 **SAP HANA Cloud Platform でのシングル サインオンの構成** ] ダイアログ ページで、シングル サインオンの構成情報を選択してクリックして **完了**します。

    ![Configure Single Sign-On](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC796933.png "Configure Single Sign-On")
  
オプションの手順として、Azure Active Directory ID プロバイダーのアサーション ベースのグループを構成できます。

>[AZURE.NOTE]SAP HANA Cloud Platform でグループを使用するには、SAML 2.0 アサーションの属性の値によって決定される、SAP HANA Cloud Platform アプリケーションで 1 つ以上のロールに 1 つまたは複数のユーザーを動的に割り当てることができます。 たとえば、アサーションに属性が含まれている場合"*コントラクト = temporary*「場合は、影響を受けるすべてのユーザー、グループに追加する」*一時*"です。 グループ"*一時*"SAP HANA Cloud Platform アカウントにデプロイされた 1 つまたは複数のアプリケーションから 1 つまたは複数の役割が含まれます。
>  
>SAP HANA Cloud Platform アカウントでアプリケーションの 1 つ以上のロールに多くのユーザーを一括で割り当てる場合は、アサーション ベースのグループを使用します。 直接割り当てることをお勧め (a) 特定の役割に割り当てるユーザーの 1 人または少数の数だけの場合、"**承認**"SAP HANA Cloud Platform コックピットのタブをクリックします。

##ユーザーにロールを割り当てる
  
Azure AD ユーザーが SAP HANA Cloud Platform にログインできるようにするには、SAP HANA Cloud Platform のロールをそれらのユーザーに割り当てる必要があります。

###ロールをユーザーに割り当てるには、次の手順に従います。

1.  ログインして **SAP HANA Cloud Platform** コックピットします。

2.  次の手順に従います。

    ![承認](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790805.png "Authorizations")

    1.  クリックして **承認**します。
    2.  クリックして、 **ユーザー** ] タブをクリックします。
    3.   **ユーザー** ] ボックスに、ユーザーの電子メール アドレスを入力します。
    4.  をクリックして **割り当てる** ロールにユーザーに割り当てる。
    5.  クリックして **保存**します。

##ユーザーの割り当て
  
構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

###ユーザーを SAP HANA Cloud Platform に割り当てるには、次の手順に従います。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.   **SAP HANA Cloud Platform** アプリケーション統合ページで、をクリックして **ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790806.png "Assign Users")

3.  テスト ユーザーを選択して [ **割り当てる**, 、順にクリック **はい** して割り当てを確認します。

    ![あり](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC767830.png "Yes")
  
シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。
