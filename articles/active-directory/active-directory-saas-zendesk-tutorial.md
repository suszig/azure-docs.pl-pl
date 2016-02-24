<properties 
    pageTitle="チュートリアル: Azure Active Directory と Zendesk の統合 | Microsoft Azure" 
    description="Azure Active Directory で Zendesk を使用して、シングル サインオンや自動プロビジョニングなどを有効にする方法について説明します。" 
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

#チュートリアル: Azure Active Directory と Zendesk の統合
  
このチュートリアルでは、Azure と Zendesk の統合について説明します。  
このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   Zendesk テナント
  
このチュートリアルを完了すると、Zendesk に割り当てた Azure AD ユーザーは、Zendesk 会社サイト (サービス プロバイダーが開始したサインオン) で、アプリケーションにシングル サインオンまたはできるを使用して、 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。
  
このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  Zendesk のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-zendesk-tutorial/IC773083.png "Scenario")

##Zendesk のアプリケーション統合の有効化
  
このセクションでは、Zendesk のアプリケーション統合を有効にする方法について説明します。

###Zendesk のアプリケーション統合を有効にするには、次の手順を実行します。

1.  左側のナビゲーション ウィンドウで、Azure 管理ポータルで、クリックして **Active Directory**します。

    ![Active Directory](./media/active-directory-saas-zendesk-tutorial/IC700993.png "Active Directory")

2.   **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3.  ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。

    ![アプリケーション](./media/active-directory-saas-zendesk-tutorial/IC700994.png "Applications")

4.  クリックして **追加** ページの下部にあります。

    ![アプリケーションの追加](./media/active-directory-saas-zendesk-tutorial/IC749321.png "Add application")

5.   **実行する操作** ダイアログ ボックスで、をクリックして **ギャラリーからアプリケーションを追加**します。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-zendesk-tutorial/IC749322.png "Add an application from gallerry")

6.   **検索ボックス**, 、型 **Zendesk**します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-zendesk-tutorial/IC773084.png "Application Gallery")

7.  結果ウィンドウで [ **Zendesk**, 、クリックして **完了** アプリケーションを追加します。

    ![Zendesk](./media/active-directory-saas-zendesk-tutorial/IC773085.png "Zendesk")

##シングル サインオンの構成
  
このセクションでは、SAML プロトコルに基づくフェデレーションを使用して、ユーザーが Azure AD のアカウントで Zendesk に対する認証を行うことができるようにする方法を説明します。  
Zendesk にシングル サインオンを構成するには、証明書からサムプリント値を取得する必要があります。  
この手順に慣れていない場合は、次を参照してください。 [証明書の拇印の値を取得する方法](http://youtu.be/YKQF266SAxI)します。

###シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **Zendesk** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、* * [シングル サインオン * * ダイアログ。

    ![シングル サインオン](./media/active-directory-saas-zendesk-tutorial/IC773086.png "Single sign-on")

2.   **どのようなユーザーの Zendesk に** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、] をクリックし、 **次**します。

    ![Configure single sign-on](./media/active-directory-saas-zendesk-tutorial/IC773087.png "Configure single sign-on")

3.   **アプリケーション URL の構成** ] ページの [、 **Zendesk サインイン URL** ] ボックスに、次のパターンを使用して URL を入力"*https://\<tenant-name\>.zendesk.com*"、] をクリックし、 **次**します。

    ![アプリケーション URL の構成](./media/active-directory-saas-zendesk-tutorial/IC773088.png "Configure app URL")

4.   **Zendesk でのシングル サインオンの構成** ] ページで、証明書をダウンロードする] をクリックして **証明書のダウンロード**, 、としては、証明書ファイルをローカルに保存および **c:\\zendesk.cer**します。

    ![Configure single sign-on](./media/active-directory-saas-zendesk-tutorial/IC777534.png "Configure single sign-on")

5.  別の Web ブラウザー ウィンドウで、Zendesk 企業サイトに管理者としてログインします。

6.  クリックして **Admin**します。

7.  左側のナビゲーション ウィンドウで、クリックして **設定**, 、クリックして **セキュリティ**します。

    ![セキュリティ](./media/active-directory-saas-zendesk-tutorial/IC773089.png "Security")

8.   **セキュリティ** ] ページで、[、 **管理とエージェント** ] タブをクリックします。

9.  選択 **シングル サインオン (SSO) and SAML**, 、し、[ **SAML**します。

10. Azure AD ポータルでの **Zendesk でのシングル サインオンの構成** ] ページで、コピー、 **SAML SSO URL** 値に設定して、貼り付けます、 **SAML SSO URL** ] ボックスに貼り付けます。

11. Azure AD ポータルでの **Zendesk でのシングル サインオンの構成** ] ページで、コピー、 **リモート ログアウト URL** 値に設定して、貼り付けます、 **リモート ログアウト URL** ] ボックスに貼り付けます。

    ![シングル サインオン](./media/active-directory-saas-zendesk-tutorial/IC773090.png "Single sign-on")

12. コピー、 **拇印** エクスポートした証明書からの値し、貼り付けます、 **Certificate Fingerprint** ] ボックスに貼り付けます。

    >[AZURE.TIP] 詳細については、次を参照してください [証明書の拇印の値を取得する方法。](http://youtu.be/YKQF266SAxI)

13. クリックして **保存**します。

14. Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![Configure single sign-on](./media/active-directory-saas-zendesk-tutorial/IC773093.png "Configure single sign-on")

##ユーザー プロビジョニングの構成
  
Azure AD ユーザーにログインを有効にするために **Zendesk**, にプロビジョニングする必要がありますが **Zendesk**します。  
場合に **Zendesk**, 、プロビジョニングは手動作業です。

###ユーザー アカウントを Zendesk にプロビジョニングするには、次の手順に従います。

1.  ログインして **Zendesk** テナントです。

2.  選択、 **顧客リスト** ] タブをクリックします。

3.  選択、 **ユーザー** タブをクリックし、クリックして **追加**します。

    ![ユーザーの追加](./media/active-directory-saas-zendesk-tutorial/IC773632.png "Add user")

4.  をクリックして、プロビジョニングする既存の Azure AD アカウントの電子メール アドレスを入力 **保存**します。

    ![新しいユーザー](./media/active-directory-saas-zendesk-tutorial/IC773633.png "New user")

>[AZURE.NOTE] 他の Zendesk ユーザー アカウント作成ツールを使用することができますか、AAD ユーザー アカウントをプロビジョニング、Zendesk から提供されている Api です。

##ユーザーの割り当て
  
構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

###ユーザーを Zendesk に割り当てるには、次の手順を実行します。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  * * Zendesk * * アプリケーション統合ページで、をクリックして **ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-zendesk-tutorial/IC773094.png "Assign users")

3.  テスト ユーザーを選択して [ **割り当てる**, 、順にクリック **はい** して割り当てを確認します。

    ![あり](./media/active-directory-saas-zendesk-tutorial/IC767830.png "Yes")
  
シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。
