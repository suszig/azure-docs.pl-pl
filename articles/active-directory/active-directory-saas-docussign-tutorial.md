<properties
    pageTitle="チュートリアル: Azure Active Directory と DocuSign の統合 | Microsoft Azure"
    description="Azure Active Directory と DocuSign の間でシングル サインオンを構成する方法について説明します。"
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="stevenpo"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/06/2015"
    ms.author="markvi"/>


# チュートリアル: Azure Active Directory と DocuSign の統合

このチュートリアルでは、Azure と DocuSign の統合について説明します。
このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

- 有効な Azure サブスクリプション
- DocuSign のテナント



このチュートリアルで説明するシナリオは、次の要素で構成されています。

1. [DocuSign のアプリケーション統合の有効化](#enabling-the-application-integration-for-docusign) 


2. [シングル サインオンの構成](#configuring-single-sign-on) 


3. [アカウント プロビジョニングの構成](#configuring-account-provisioning) 


4. [ユーザーの割り当て](#assigning-users) 




<br><br>![シングル サインオンの構成 ][0]<br>


 

## DocuSign のアプリケーション統合の有効化

このセクションでは、DocuSign のアプリケーション統合を有効にする方法について説明します。

### DocuSign のアプリケーション統合を有効にするには、次の手順を実行します。

1. 左側のナビゲーション ウィンドウで、Azure 管理ポータルで、クリックして **Active Directory**します。
<br><br>![シングル サインオンを構成します。 ][1]<br>

2. [ディレクトリ] の一覧から、ディレクトリ統合を有効にするディレクトリを選択します。

3. ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。
<br><br>![シングル サインオンを構成します。 ][2]<br>

4. クリックして **追加** ページの下部にあります。
<br><br>![アプリケーション][3]<br>

5. [対象たいダイアログの操作を行います] をクリックして **ギャラリーからアプリケーションを追加**します。
<br><br>![シングル サインオンを構成します。 ][4]<br>


6. 検索ボックスに入力 **Docusign**します。
<br><br>![シングル サインオンを構成します。 ][5]<br>

7. 結果ウィンドウで [ **Docusign**, 、クリックして **完了** アプリケーションを追加します。
<br><br>![シングル サインオンを構成します。 ][6]<br>




## シングル サインオンの構成

このセクションでは、ユーザーが SAML プロトコルに基づくフェデレーションを使用して、Azure AD でのユーザーのアカウントで Docusign に対する認証を行うことができるようにする方法を説明します。


### シングル サインオンを構成するには、次の手順に従います。

1. Azure AD ポータルでの **Docusign のアプリケーション統合** ] ページで [ **でのシングル サインオンを構成する** シングル サインオンの構成ダイアログ ボックスを開きます。
<br><br>![シングル サインオンを構成します。 ][7]<br>

2.  **どのようなユーザーが Docusign にサインオン** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、し、[次へ] をクリックします。
<br><br>![シングル サインオンを構成します。 ][8]<br>

3.  **アプリケーション URL の構成** ] ページで、 **Docusign へのサインオン URL** ] ボックスは、Docusign テナントの URL を入力し、をクリックして **次**します。 
URL は次のスキーマが必要: *https://<yourcompanyname>.docusign.net/Member/MemberLogin.aspx?ssoname=<yourSSOInstanceName>*
<br><br>![シングル サインオンを構成します。 ][9]<br>


    > [AZURE.TIP] If you don’t know what your app URL for your tenant is, try contacting Docusign via SSOSetup@Docusign.com to get the SP Initiated SSO URL for your tenant.
 

4.  **Docusign でのシングル サインオンの構成** ] ページで [ **証明書のダウンロード**, 、お使いのコンピューターにローカルで証明書ファイルを保存します。
<br><br>![シングル サインオンを構成します。 ][10]<br>


5. ログインする別の web ブラウザーのウィンドウで、 **Docusign** 会社サイトに管理者として。


6. 上部にあるメニューで、ユーザーのメニューを展開し、をクリックして **の基本設定**, 、次に、左側のナビゲーション ウィンドウで次のように展開します。 **アカウント管理**, 、] をクリックし、 **機能**です。
<br><br>![シングル サインオンを構成します。 ][11]<br>

7. をクリックして **SAML Configuration**, 、順にクリックし、 **SAML Configuration** リンクします。



8.  **SAML 2.0 Configuration** セクションで、次の手順に従います。
<br><br>![シングル サインオンを構成します。 ][13]<br>


    a. In the Azure portal, on the **Configure single sign-on at Docusign** dialogue page, copy the Issuer URL** value, and then paste it into the **Identity Provider Endpoint URL** textbox.

    > [AZURE.IMPORTANT] If this configuration option is unavailable, please contact your Docusign account manager or contact the SSO support team by email ([SSOSetup@docusign.com](mailto:SSOSetup@docusign.com)).
 
    b. Click **Browse** to upload your downloaded certificate.


    c. Select **Enable first name, last name and email address**.


    d. Click **Save**.


9. Azure AD ポータルで、選択、 **シングル サインオンの構成の確認**, 、] をクリックし、 **次**します。
<br><br>![アプリケーション][14]<br>

10.  **シングル サインオンによる確認** ] ページで [ **完了**します。
<br><br>![アプリケーション][15]<br>


 

## アカウント プロビジョニングの構成

このセクションでは、Active Directory のユーザー アカウントのプロビジョニングを DocuSign に対して有効にする方法を説明します。

### ユーザー プロビジョニングを構成するには、次の手順に従います。

1.  **Azure 管理ポータル**, の **DocuSign のアプリケーション統合** ] ページで [ **アカウント プロビジョニングの構成** ユーザー プロビジョニングの構成ダイアログ ボックスを開きます。
<br><br>![アカウント プロビジョニングの構成][30]<br>
 

2.  **設定と管理者資格情報** ] ページで自動ユーザー プロビジョニングを有効にするための十分な権限を持つ DocuSign アカウントの資格情報を提供、順にクリックを **次**します。 
<br><br>![アカウント プロビジョニングの構成][31]<br>

3.  **接続をテストする** ダイアログ ボックスで、] をクリックして **テストの開始**, 、成功したテスト] をクリックし、 **次**します。
<br><br>![アカウント プロビジョニングの構成][32]<br>

3.  **確認** ] ページで [ **完了**します。

<br><br>![アカウント プロビジョニングの構成][33]<br>
 

## ユーザーの割り当て

構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

### ユーザーを Docusign に割り当てるには、次の手順を実行します。

1.  **Azure AD ポータル**, 、テスト アカウントを作成します。

2.  **Docusign のアプリケーション統合** ] ページで [ **ユーザーの割り当て**します。
<br><br>![ユーザーの割り当てください。][40]<br>
 

3. テスト ユーザーを選択して [ **割り当てる**, 、順にクリック **はい** して割り当てを確認します。

<br><br>![ユーザーの割り当て][41]<br>


ここで 10 分間待機し、アカウントが Docusign に同期されたことを確認する必要があります。

最初の検証手順としては、Azure 管理ポータルでの Docusign のアプリケーション統合ページで、d のダッシュ ボードをクリックして、プロビジョニングの状態を確認できます。
<br><br>![ユーザーの割り当て][42]<br>

正常に完了したユーザー プロビジョニング サイクルは、関連する状態で示されます。
<br><br>![ユーザーの割り当て][43]<br>


シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。

アクセス パネルの詳細については、「アクセス パネルの概要」を参照してください。





## その他のリソース

* [SaaS アプリと Azure Active Directory を統合する方法に関するチュートリアルの一覧](active-directory-saas-tutorial-list.md)
* [Azure Active Directory のアプリケーション アクセスとシングル サインオンとは](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[0]: ./media/active-directory-saas-docussign-tutorial/tutorial_docusign_00.png
[1]: ./media/active-directory-saas-docussign-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-docussign-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-docussign-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-docussign-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-docussign-tutorial/tutorial_docusign_01.png
[6]: ./media/active-directory-saas-docussign-tutorial/tutorial_docusign_02.png
[7]: ./media/active-directory-saas-docussign-tutorial/tutorial_docusign_03.png
[8]: ./media/active-directory-saas-docussign-tutorial/tutorial_docusign_04.png
[9]: ./media/active-directory-saas-docussign-tutorial/tutorial_docusign_05.png
[10]: ./media/active-directory-saas-docussign-tutorial/tutorial_docusign_06.png
[11]: ./media/active-directory-saas-docussign-tutorial/tutorial_docusign_07.png

[13]: ./media/active-directory-saas-docussign-tutorial/tutorial_docusign_09.png
[14]: ./media/active-directory-saas-docussign-tutorial/tutorial_docusign_10.png
[15]: ./media/active-directory-saas-docussign-tutorial/tutorial_docusign_11.png

[30]: ./media/active-directory-saas-docussign-tutorial/tutorial_general_400.png
[31]: ./media/active-directory-saas-docussign-tutorial/tutorial_docusign_12.png
[32]: ./media/active-directory-saas-docussign-tutorial/tutorial_docusign_13.png
[33]: ./media/active-directory-saas-docussign-tutorial/tutorial_docusign_14.png



[40]: ./media/active-directory-saas-docussign-tutorial/tutorial_docusign_15.png
[41]: ./media/active-directory-saas-docussign-tutorial/tutorial_docusign_16.png
[42]: ./media/active-directory-saas-docussign-tutorial/tutorial_docusign_17.png
[43]: ./media/active-directory-saas-docussign-tutorial/tutorial_docusign_18.png
