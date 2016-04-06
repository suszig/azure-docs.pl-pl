<properties 
    pageTitle="チュートリアル: Azure Active Directory と Envoy の統合 | Microsoft Azure" 
    description="Azure Active Directory で Envoy を使用して、シングル サインオンや自動プロビジョニングなどを有効にする方法について説明します。" 
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

#チュートリアル: Azure Active Directory と Envoy の統合
  
このチュートリアルでは、Azure と Envoy の統合について説明します。  
このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   Envoy テナント
  
このチュートリアルを完了すると、Envoy に割り当てた Azure AD ユーザーは、Envoy 会社サイト (サービス プロバイダーが開始したサインオン) で、アプリケーションにシングル サインオンまたはできるを使用して、 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。
  
このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  Envoy のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-envoy-tutorial/IC776759.png "Scenario")
##Envoy のアプリケーション統合の有効化
  
このセクションでは、Envoy のアプリケーション統合を有効にする方法について説明します。

###Envoy のアプリケーション統合を有効にするには、次の手順を実行します。

1.  左側のナビゲーション ウィンドウで、Azure 管理ポータルで、クリックして **Active Directory**します。

    ![Active Directory](./media/active-directory-saas-envoy-tutorial/IC700993.png "Active Directory")

2.   **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3.  ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。

    ![アプリケーション](./media/active-directory-saas-envoy-tutorial/IC700994.png "Applications")

4.  クリックして **追加** ページの下部にあります。

    ![アプリケーションの追加](./media/active-directory-saas-envoy-tutorial/IC749321.png "Add application")

5.   **実行する操作** ダイアログ ボックスで、をクリックして **ギャラリーからアプリケーションを追加**します。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-envoy-tutorial/IC749322.png "Add an application from gallerry")

6.   **検索ボックス**, 、型 **Envoy**します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-envoy-tutorial/IC776760.png "Application gallery")

7.  結果ウィンドウで [ **Envoy**, 、クリックして **完了** アプリケーションを追加します。

    ![Envoy](./media/active-directory-saas-envoy-tutorial/IC776777.png "Envoy")
##シングル サインオンの構成
  
このセクションでは、ユーザーが SAML プロトコルに基づくフェデレーションを使用して、Azure AD でのユーザーのアカウントで Envoy に対する認証を行うことができるようにする方法を説明します。  
Envoy のシングル サインオンを構成するには、証明書から拇印の値を取得する必要があります。  
この手順に慣れていない場合は、次を参照してください [証明書の拇印の値を取得する方法。](http://youtu.be/YKQF266SAxI)

###シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **Envoy** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、* * [シングル サインオン * * ダイアログ。

    ![シングル サインオンの有効化](./media/active-directory-saas-envoy-tutorial/IC776778.png "Enable single sign-on")

2.   **どのようなユーザーを Envoy へのサインオン** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、] をクリックし、 **次**します。

    ![Configure single sign-on](./media/active-directory-saas-envoy-tutorial/IC776779.png "Configure single sign-on")

3.   **アプリケーション URL の構成** ] ページで、 **Envoy サインイン URL** ] ボックスに、次のパターンを使用して URL を入力"*https://\<tenant-name\>。Envoy.com*"、クリックして **次**します。

    ![アプリケーション URL の構成](./media/active-directory-saas-envoy-tutorial/IC776780.png "Configure app URL")

4.   **Envoy でのシングル サインオンの構成** ] ページで、証明書をダウンロードする] をクリックして **証明書のダウンロード**, 、としては、証明書ファイルをローカルに保存および **c:\\Envoy.cer**します。

    ![Configure single sign-on](./media/active-directory-saas-envoy-tutorial/IC776781.png "Configure single sign-on")

5.  別の Web ブラウザー ウィンドウで、Envoy 企業サイトに管理者としてログインします。

6.  上部にあるツールバーで、クリックして **設定**します。

    ![Envoy](./media/active-directory-saas-envoy-tutorial/IC776782.png "Envoy")

7.  クリックして **会社**します。

    ![会社](./media/active-directory-saas-envoy-tutorial/IC776783.png "Company")

8.  クリックして **SAML**します。

    ![SAML](./media/active-directory-saas-envoy-tutorial/IC776784.png "SAML")

9.   **SAML 認証** 構成セクションで、次の手順に従います。

    ![SAML 認証](./media/active-directory-saas-envoy-tutorial/IC776785.png "SAML authentication")

    >[AZURE.NOTE] [HQ location ID の値は、アプリケーションによって自動的に生成します。

    1.  コピー、 **拇印** エクスポートした証明書からの値し、貼り付けます、 **指紋** ] ボックスに貼り付けます。  

        >[AZURE.TIP] 詳細については、次を参照してください [証明書の拇印の値を取得する方法。](http://youtu.be/YKQF266SAxI)

    2.  Azure ポータルで、 **Envoy でのシングル サインオンの構成** ] ダイアログ ページでコピー、 **SAML SSO URL** 値に設定して、貼り付けます、 **Identity Provider HTTP SAML URL** ] ボックスに貼り付けます。
    3.  クリックして **変更を保存**します。

10. Azure AD ポータルでのシングル サインオンの構成情報を選択し、 **完了** を閉じる、 **[シングル サインオン** ダイアログ。

    ![シングル サインオンの構成](./media/active-directory-saas-envoy-tutorial/IC776786.png "Configure single sign-on")
##ユーザー プロビジョニングの構成
  
Envoy へのユーザー プロビジョニングの構成にあたって必要な操作はありません。  
割り当てられたユーザーがアクセス パネルを使用して Envoy にログインしようとすると、そのユーザーが存在するかどうかが Envoy によって確認されます。  
使用可能なユーザー アカウントがない場合、ユーザー アカウントは自動的に作成されます。
##ユーザーの割り当て
  
構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

###ユーザーを Envoy に割り当てるには、次の手順を実行します。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  * * Envoy * * アプリケーション統合ページで、をクリックして **ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-envoy-tutorial/IC776787.png "Assign users")

3.  テスト ユーザーを選択して [ **割り当てる**, 、順にクリック **はい** して割り当てを確認します。

    ![あり](./media/active-directory-saas-envoy-tutorial/IC767830.png "Yes")
  
シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。

