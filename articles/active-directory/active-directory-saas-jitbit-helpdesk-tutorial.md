<properties 
    pageTitle="チュートリアル: Azure Active Directory と Jitbit Helpdesk の統合 | Microsoft Azure" 
    description="Azure Active Directory と Jitbit Helpdesk を使用して、シングル サインオン、自動化されたプロビジョニング、および詳細を有効にする方法を説明します。" 
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


# チュートリアル: Azure Active Directory と Jitbit Helpdesk の統合

このチュートリアルでは、Azure と Jitbit Helpdesk の統合について説明します。  
このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   Jitbit Helpdesk テナント

このチュートリアルを完了すると、Jitbit Helpdesk に割り当てた Azure AD ユーザーは、Jitbit Helpdesk 会社サイト (サービス プロバイダーが開始したサインオン) で、アプリケーションにシングル サインオンまたはできるを使用して、 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。

このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  Jitbit Helpdesk のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777676.png "Scenario")
## Jitbit Helpdesk のアプリケーション統合の有効化

このセクションでは、Jitbit Helpdesk のアプリケーション統合を有効にする方法を説明します。

### Jitbit Helpdesk のアプリケーション統合を有効にするには、次の手順を実行します。

1.  Azure 管理ポータルの左側のナビゲーション ウィンドウで、**[Active Directory]** をクリックします。

    ![Active Directory](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC700993.png "Active Directory")

2.  **[ディレクトリ]** の一覧から、ディレクトリ統合を有効にするディレクトリを選択します。

3.  アプリケーション ビューを開くには、ディレクトリ ビューでトップ メニューの **[アプリケーション]** をクリックします。

    ![アプリケーション](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC700994.png "Applications")

4.  ページの下部にある **[追加]** をクリックします。

    ![アプリケーションの追加](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC749321.png "Add application")

5.  **[実行する内容]** ダイアログで、**[ギャラリーからアプリケーションを追加します]** をクリックします。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC749322.png "Add an application from gallerry")

6.  **検索ボックス**に、「**Jitbit Helpdesk**」と入力します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777677.png "Application Gallery")

7.  結果ウィンドウで **[Jitbit Helpdesk]** を選択し、**[完了]** をクリックしてアプリケーションを追加します。

    ![JitBit](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC781008.png "JitBit")
## シングル サインオンの構成

このセクションでは、SAML プロトコルに基づくフェデレーションを使用して Azure AD のアカウントを Jitbit Helpdesk に認証するユーザーを有効にする方法を説明します。 .  
この手順の途中で、base-64 でエンコードされた証明書ファイルを作成する必要があります。  
この手順に慣れていない場合は、次を参照してください。 [バイナリ証明書をテキスト ファイルに変換する方法](http://youtu.be/PlgrzUZ-Y1o)します。
>[AZURE.IMPORTANT] Jitbit Helpdesk テナントでシングル サインオンを構成できるようにするには、まず Jitbit Helpdesk テクニカル サポートに連絡してこの機能を有効にする必要があります。

### シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **Jitbit Helpdesk** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、 ** [シングル サインオン ** ダイアログ。

    ![シングル サインオンの構成](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777678.png "Configure single sign-on")

2.  **どのようなユーザーを Jitbit Helpdesk へのサインオン** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、] をクリックし、 **次**します。

    ![シングル サインオンの構成](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777679.png "Configure single sign-on")

3.  **アプリケーション URL の構成** ] ページで、 **Jitbit Helpdesk サインイン URL** ] ボックスに、次のパターンを使用して URL を入力"* https://\<tenant-name\>します。Jitbit.com*"、クリックして **次**します。

    ![アプリケーション URL の構成](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777528.png "Configure app URL")

4.  **Jitbit Helpdesk でのシングル サインオンの構成** ] ページで、証明書をダウンロードする] をクリックして **証明書のダウンロード**, 、としては、証明書ファイルをローカルに保存および **c:\\Jitbit Helpdesk.cer**します。

    ![シングル サインオンの構成](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777680.png "Configure single sign-on")

5.  別の Web ブラウザーのウィンドウで、Jitbit Helpdesk の企業サイトに管理者としてログインします。

6.  上部のツールバーで **[管理]** をクリックします。

    ![管理](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777681.png "Administration")

7.  **[全般設定]** をクリックします。

    ![ユーザー、会社、およびアクセス許可](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777682.png "Users, companies and permissions")

8.  **[認証設定]** 構成セクションで、次の手順を実行します。

    ![認証設定](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777683.png "Authentication settings")

    1.  **OneLogin** を指定してシングル サインオン (SSO) を使用し、**[SAML 2.0 シングル サインオンの有効化]** サインインを選択します。
    2.  Azure ポータルの **[Jitbit Helpdesk でのシングル サインオンの構成]** ダイアログ ページで **[サービス プロバイダー (SP) によって開始されたエンドポイント]** の値をコピーし、**[エンドポイントの URL]** テキストボックスに貼り付けます。
    3.  ダウンロードした証明書から **base-64 でエンコードされた**ファイルを作成します。
        >[AZURE.TIP]詳細については、「 [バイナリ証明書をテキスト ファイルに変換する方法](http://youtu.be/PlgrzUZ-Y1o)

    4.  base-64 でエンコードされた証明書を開き、その内容をクリップボードにコピーして、**[X.509 証明書]** テキスト ボックスに貼り付けます。
    5.  **[変更を保存]** をクリックします。

9.  Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![シングル サインオンの構成](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777684.png "Configure single sign-on")
## ユーザー プロビジョニングの構成

Azure AD ユーザーが Jitbit Helpdesk にログインできるようにするには、そのユーザーを Jitbit Helpdesk にプロビジョニングする必要があります。  
Jitbit Helpdesk の場合、プロビジョニングは手動で行います。

### ユーザー アカウントをプロビジョニングするには、次の手順を実行します。

1.  **Jitbit Helpdesk** テナントにログインします。

2.  上部のメニューで **[管理]** をクリックします。

    ![管理](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777681.png "Administration")

3.  **[ユーザー、会社、およびアクセス許可]** をクリックします。

    ![ユーザー、会社、およびアクセス許可](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777682.png "Users, companies and permissions")

4.  **[ユーザーの追加]** をクリックします。

    ![ユーザーの追加](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777685.png "Add user")

5.  [作成] セクションで、プロビジョニングする Azure AD アカウントのデータを **[ユーザー名]**、**[電子メール]**、**[名]**、**[姓]** の各テキストボックスに入力します。

    ![作成](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777686.png "Create")

6.  **[作成]** をクリックします。

>[AZURE.NOTE] Helpdesk から提供されている他の Helpdesk ユーザー アカウント作成ツールまたは API を使用して、AAD ユーザー アカウントをプロビジョニングできます。

## ユーザーの割り当て

構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

### ユーザーを Jitbit Helpdesk に割り当てるには、次の手順を実行します。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  **Jitbit Helpdesk * * アプリケーション統合ページで、をクリックして * * ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777687.png "Assign users")

3.  テスト ユーザーを選択し、**[割り当て]**、**[はい]** の順にクリックして、割り当てを確定します。

    ![はい](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC767830.png "Yes")

シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。




